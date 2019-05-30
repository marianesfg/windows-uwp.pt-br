---
title: Usando o MRT para jogos e aplicativos de área de trabalho convertidos
description: Empacotando o app ou jogo .NET ou Win32 como um pacote AppX, você pode aproveitar o Sistema de Gerenciamento de Recursos para carregar recursos de app personalizados para o contexto de tempo de execução. Este tópico detalhado descreve as técnicas.
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. recursos, jogos, centennial, desktop app converter, mui, assembly satélite
ms.localizationpriority: medium
ms.openlocfilehash: 82050c92311ce8bb7457637a486943a5fed3e334
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359325"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Use o Sistema de Gerenciamento de Recursos do Windows 10 em um app ou jogo herdado

Os apps e jogos .NET e Win32 geralmente são localizados em diferentes idiomas para expandir totalmente o mercado ao qual se destinam. Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md). Ao empacotar seu aplicativo .NET ou Win32 ou jogo como um pacote de AppX MSIX, você pode aproveitar o sistema de gerenciamento de recursos para carregar os recursos do aplicativo sob medidos para o contexto de tempo de execução. Este tópico detalhado descreve as técnicas.

Há muitas maneiras de localizar um aplicativo Win32 tradicionais, mas o Windows 8 introduziu um [novo sistema de gerenciamento de recursos](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) que funciona entre linguagens de programação, entre tipos de aplicativos e fornece uma funcionalidade que transcende a simples localização. Esse sistema será chamado de "MRT" neste tópico. Antigamente, a sigla significava "Modern Resource Technology", mas o termo "Modern" deixou de ser utilizado. O gerenciador de recursos também pode ser conhecido como MRM (Modern Resource Manager) ou PRI ( Índice de Recurso do Pacote).

Combinado com a implantação com base em AppX ou MSIX (por exemplo, de a Microsoft Store), MRT pode fornecer automaticamente os recursos mais aplicáveis para um determinado usuário / dispositivo, o que minimiza o download e instala o tamanho do seu aplicativo. Essa redução de tamanho pode ser significativa para aplicativos com uma grande quantidade de conteúdo localizado, talvez na ordem de vários *gigabytes* para jogos AAA. Os benefícios adicionais do MRT incluem listagens localizadas no Shell do Windows e na Microsoft Store, lógica de fallback automática quando o idioma preferencial de um usuário não corresponde aos recursos disponíveis.

Este documento descreve a arquitetura de alto nível do MRT e fornece um guia de portabilidade para ajudar a migrar aplicativos Win32 herdados para MRT com o mínimo de alterações no código. Depois que a migração para o MRT é feita, benefícios adicionais (por exemplo, a capacidade de segmentar recursos por fator de escala ou tema do sistema) são disponibilizados para o desenvolvedor. Observe que a localização baseada em MRT funciona para aplicativos UWP e Win32 processados pela Ponte de Desktop (também conhecida como "Centennial").

Em muitas situações, você pode continuar usando o código-fonte e os formatos de localização existentes, integrando-se, ao mesmo tempo, ao MRT para resolver recursos em tempo de execução e minimizar os tamanhos dos downloads; não é uma abordagem "tudo ou nada". A tabela a seguir resume o trabalho e o custo estimado/benefício de cada estágio. Esta tabela não inclui tarefas de não localização, como fornecimento de ícones de aplicativos de alta resolução ou de alto contraste. Para obter mais informações sobre como fornecer vários ativos para blocos, ícones etc., consulte [Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Trabalho</th>
<th>Benefício</th>
<th>Custo estimado</th>
</tr>
<tr>
<td>Localizar o manifesto do pacote</td>
<td>Descobrir o mínimo de trabalho necessário para que o conteúdo localizado apareça no Shell do Windows e na Microsoft Store</td>
<td>Pequeno</td>
</tr>
<tr>
<td>Usar o MRT para identificar e localizar recursos</td>
<td>Pré-requisito para minimizar os tamanhos dos downloads e das instalações; fallback de idioma automático</td>
<td>Médio</td>
</tr>
<tr>
<td>Criar pacotes de recursos</td>
<td>Etapa final para minimizar os tamanhos dos downloads e das instalações</td>
<td>Pequeno</td>
</tr>
<tr>
<td>Migrar para formatos de recurso MRT e APIs</td>
<td>Tamanhos de arquivo significativamente menores (dependendo da tecnologia de recurso existente)</td>
<td>Grande</td>
</tr>
</table>

## <a name="introduction"></a>Introdução

A maioria dos aplicativos não triviais contém elementos de interface do usuário conhecidos como *recursos*, que são dissociados do código do aplicativo (em contraposição aos *valores embutidos em código* criados no próprio código-fonte). Há vários motivos para preferirmos os recursos, em vez dos valores embutidos em código: a facilidade de edição por não desenvolvedores é um deles, por exemplo; mas um dos principais motivos é permitir que o aplicativo selecione representações diferentes do mesmo recurso lógico em tempo de execução. Por exemplo, o texto a ser exibido em um botão (ou a imagem a ser exibida em um ícone) pode ser diferente dependendo dos idiomas que o usuário compreende, as características do dispositivo de exibição ou se o usuário tem alguma tecnologia adaptativa habilitada.

Desse modo, o objetivo principal de qualquer tecnologia de gerenciamento de recursos é converter, em tempo de execução, uma solicitação de *nome de recurso* lógico ou simbólico (como `SAVE_BUTTON_LABEL`) de um conjunto de *candidatos* possíveis (por exemplo, "Salvar", "Speichern" ou "저장") no melhor *valor* real possível (por exemplo, "Salvar"). O MRT fornece essa função e permite que os aplicativos identifiquem os candidatos a recursos usando uma ampla variedade de atributos, chamados *qualificadores*, como o idioma do usuário, o fator de escala da exibição, o tema selecionado pelo usuário e outros fatores ambientais. O MRT oferece até mesmo suporte a qualificadores personalizados em aplicativos que precisam dele (por exemplo, um aplicativo pode fornecer ativos de gráfico diferentes para os usuários que fizeram logon usando uma conta x usuários convidados, sem adicionar explicitamente essa verificação em cada parte do aplicativo). O MRT funciona com recursos de cadeia de caracteres e recursos baseados em arquivo; estes últimos são implementados como referências aos dados externos (os arquivos propriamente ditos).

### <a name="example"></a>Exemplo

Este é um exemplo simples de um aplicativo que tem rótulos de texto em dois botões (`openButton` e `saveButton`) e um arquivo PNG usado para um logotipo (`logoImage`). Os rótulos de texto são localizados em inglês e alemão, e o logotipo é otimizado para desktops normais (fator de escala 100%) e telefones de alta resolução (fator de escala 300%). Observe que este diagrama apresenta uma visão geral conceitual do modelo; ele não é mapeado exatamente para a implementação.

<p><img src="images\conceptual-resource-model.png"/></p>

No gráfico, o código do aplicativo faz referência aos três nomes de recurso lógico. Em tempo de execução, a pseudofunção `GetResource` usa o MRT para analisar esses nomes de recurso na tabela de recursos (conhecida como arquivo PRI) e localizar o candidato mais apropriado com base nas condições ambientais (o idioma do usuário e o fator de escala do visor). No caso dos rótulos, as cadeias de caracteres são usadas diretamente. No caso da imagem do logotipo, as cadeias de caracteres são interpretadas como nomes de arquivo, e os arquivos são lidos no disco. 

Se o usuário está ligado a um idioma diferente do inglês ou em alemão ou tem um fator de escala exibição que não seja 100% ou 300%, MRT escolhe a correspondência "mais próximo" release candidate com base em um conjunto de regras de fallback (consulte [sistema de gerenciamento de recursos](https://docs.microsoft.com/previous-versions/windows/apps/jj552947(v=win.10)) para obter mais informações em segundo plano).

Observe que MRT dá suporte a recursos que são personalizados para mais de um qualificador - por exemplo, se a imagem do logotipo continha texto incorporado que também precisava ser localizado, o logotipo teria quatro candidatos: Escala/EN-100, escala/DE-100, escala/EN-300 e escala/DE-300.

### <a name="sections-in-this-document"></a>Seções deste documento

As seções a seguir descrevem as tarefas de alto nível necessárias para integrar o MRT ao aplicativo.

#### <a name="phase-0-build-an-application-package"></a>Fase 0: Criar um pacote de aplicativo

Esta seção descreve como obter o aplicativo de área de trabalho existente criando como um pacote de aplicativos. Nenhum recurso MRT é usado neste estágio.

#### <a name="phase-1-localize-the-application-manifest"></a>Fase 1: Localizar o manifesto do aplicativo

Esta seção descreve como localizar o manifesto do aplicativo (para que ele seja exibido corretamente no Shell do Windows), usando ainda o formato de recurso herdado e a API para empacotar e localizar recursos. 

#### <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: Usar o MRT para identificar e localizar recursos

Esta seção descreve como modificar o código do aplicativo (e possivelmente o layout do recurso) para localizar recursos usando o MRT, usando ainda os formatos de recurso e APIs existentes para carregar e consumir os recursos. 

#### <a name="phase-3-build-resource-packs"></a>Fase 3: Criar pacotes de recursos

Esta seção descreve as alterações finais necessárias para separar os recursos em *pacotes de recursos* distintos, minimizando o tamanho do download e (da instalação) do aplicativo.

### <a name="not-covered-in-this-document"></a>Não abordado neste documento

Depois de concluir as fases de 0 a 3 acima, você terá um aplicativo "pacote" que podem ser enviados para a Microsoft Store e o que minimizará o download e instalar o tamanho para os usuários, omitindo os recursos que eles não precisam (por exemplo, linguagens que eles não falam). É possível fazer melhorias adicionais no tamanho e na funcionalidade do aplicativo através da execução de uma etapa final.

#### <a name="phase-4-migrate-to-mrt-resource-formats-and-apis"></a>Fase 4: Migrar para formatos de recurso MRT e APIs

Esta fase está além do escopo deste documento. Ela envolve a migração dos recursos (principalmente as cadeias de caracteres) de formatos herdados, como DLLs MUI ou assemblies de recurso .NET, para arquivos PRI. Ela pode resultar em economia de espaço adicional para tamanhos de download e instalação. Ele também permite o uso de outros recursos MRT, como minimizar o download e a instalação de arquivos de imagem com base no fator de escala, nas configurações de acessibilidade etc.

## <a name="phase-0-build-an-application-package"></a>Fase 0: Criar um pacote de aplicativo

Antes de fazer qualquer alteração nos recursos do aplicativo, primeiro você deve substituir a tecnologia de empacotamento e instalação atual pela tecnologia de empacotamento e implantação de UWP padrão. Há três maneiras de fazer isso:

* Se você tiver um grande aplicativo da área de trabalho com um instalador complexo ou você utilizar muitos pontos de extensibilidade do sistema operacional, você pode usar a ferramenta de Desktop App Converter para gerar o layout de arquivo do UWP e informações do manifesto do seu instalador de aplicativo existente (por exemplo, um MSI).
* Se você tiver um aplicativo da área de trabalho menor com relativamente poucos arquivos ou um instalador simple e nenhuma ganchos de extensibilidade, você pode criar o layout de arquivo e manualmente informações do manifesto.
* Se você estiver recriando da fonte e quiser atualizar seu aplicativo para ser um aplicativo UWP puro, você pode criar um novo projeto no Visual Studio e contar com o IDE para fazer grande parte do trabalho para você.

Se você quiser usar o [Desktop App Converter](https://aka.ms/converter), consulte [empacotar um aplicativo da área de trabalho usando o Desktop App Converter](https://aka.ms/converterdocs) para obter mais informações sobre o processo de conversão. Um conjunto completo de exemplos de conversor de área de trabalho pode ser encontrado em [repositório GitHub de exemplos da ponte de Desktop para a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Se você quiser criar manualmente o pacote, você precisará criar uma estrutura de diretório que inclui todos os arquivos do seu aplicativo (executáveis e conteúdo, mas não o código-fonte) e um arquivo de manifesto de pacote (. appxmanifest). Um exemplo pode ser encontrado na [o exemplo de Hello, World GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), mas um arquivo de manifesto de pacote básico que executa a área de trabalho executável denominada `ContosoDemo.exe` é o seguinte, onde o <span style="background-color: yellow">texto realçado</span> seria substituído por seus próprios valores.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap">
    <Identity Name="Contoso.Demo"
              Publisher="CN=Contoso.Demo"
              Version="1.0.0.0" />
    <Properties>
    <DisplayName>Contoso App</DisplayName>
    <PublisherDisplayName>Contoso, Inc</PublisherDisplayName>
    <Logo>Assets\StoreLogo.png</Logo>
  </Properties>
    <Dependencies>
    <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" />
  </Dependencies>
    <Resources>
    <Resource Language="en-US" />
  </Resources>
    <Applications>
    <Application Id="ContosoDemo" Executable="ContosoDemo.exe" 
                 EntryPoint="Windows.FullTrustApplication">
    <uap:VisualElements DisplayName="Contoso Demo" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="Contoso Demo">
      </uap:VisualElements>
    </Application>
  </Applications>
    <Capabilities>
    <rescap:Capability Name="runFullTrust" />
  </Capabilities>
</Package>
```

Para obter mais informações sobre o arquivo de manifesto de pacote e o layout do pacote, consulte [manifesto do pacote de aplicativo](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appx-package-manifest).

Por fim, se você estiver usando o Visual Studio para criar um novo projeto e migrar seu código existente entre, consulte [criar um "Olá, mundo" aplicativo](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). Você pode incluir o código existente para o novo projeto, mas você provavelmente terá que fazer alterações significativas do código (especialmente na interface do usuário) para executar como um aplicativo UWP puro. Essas alterações estão fora do escopo deste documento.

## <a name="phase-1-localize-the-manifest"></a>Fase 1: Localizar o manifesto

### <a name="step-11-update-strings--assets-in-the-manifest"></a>Etapa 1.1: Atualizar cadeias de caracteres & ativos no manifesto

Na fase de 0, você criou um arquivo de manifesto (. appxmanifest) de pacote básico para o seu aplicativo (com base nos valores fornecidos para o conversor, extraído do MSI ou inseridos manualmente no manifesto do), mas não conterá informações localizadas, nem oferecerá suporte recursos adicionais como início de alta resolução lado a lado ativos, etc.

Para garantir que o nome e uma descrição do seu aplicativo estão localizados corretamente, você deve definir alguns recursos em um conjunto de arquivos de recurso e atualiza o manifesto de pacote para fazer referência a eles.

#### <a name="creating-a-default-resource-file"></a>Criando um arquivo de recurso padrão

A primeira etapa é criar um arquivo de recurso padrão no seu idioma padrão (por exemplo, Português (Brasil)). Você pode fazer isso manualmente com um editor de texto ou por meio do Designer de Recursos no Visual Studio.

Se você quiser criar os recursos manualmente:

1. Crie um arquivo XML denominado `resources.resw` e coloque-o na subpasta `Strings\en-us` do projeto. Use o código de BCP-47 apropriado se o idioma padrão não for inglês (EUA).
2. No arquivo XML, adicione o conteúdo a seguir, no qual o <span style="background-color: yellow">texto realçado</span> é substituído pelo texto apropriado do app, no idioma padrão.

> [!NOTE]
> Há restrições em comprimentos de algumas dessas cadeias de caracteres. Para obter mais informações, consulte [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements).

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (English)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (English)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (English)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, USA</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (EN)</value>
  </data>
</root>
```

Se você quiser usar o designer no Visual Studio:

1. Criar o `Strings\en-us` pasta (ou outra linguagem conforme apropriado) no seu projeto e adicione uma **Novo Item** para a pasta raiz do seu projeto, usando o nome padrão do `resources.resw`. Verifique se você escolheu **arquivo de recursos (. resw)** e não **dicionário de recursos** -um dicionário de recursos é um arquivo usado por aplicativos XAML.
2. Usando o designer, insira as seguintes cadeias de caracteres (use os mesmos `Names`, mas substitua os `Values` pelo texto apropriado para o aplicativo):

<img src="images\editing-resources-resw.png"/>

> [!NOTE]
> Se você iniciar com o designer do Visual Studio, você sempre poderá editar o XML diretamente, pressionando `F7`. Mas, se você começar com um arquivo XML mínimo, *o designer não reconhecerá o arquivo* por não ter vários metadados adicionais; você pode corrigir isso copiando as informações XSD clichês de um arquivo gerado pelo designer para o arquivo XML editado manualmente.

#### <a name="update-the-manifest-to-reference-the-resources"></a>Atualizar o manifesto para fazer referência aos recursos

Depois de ter os valores definidos no `.resw` arquivo, a próxima etapa é atualizar o manifesto para referenciar as cadeias de caracteres de recurso. Mais uma vez, você pode editar um arquivo XML diretamente ou contar com o Designer de Manifesto do Visual Studio.

Se você estiver editando o XML diretamente, abra o arquivo `AppxManifest.xml` e faça as seguintes alterações nos <span style="background-color: lightgreen">valores realçados</span> - use *exatamente* este texto, e não o texto específico do seu aplicativo. Não há nenhuma exigência para o uso desses nomes de recurso exatos; você pode escolher o nome, desde que ele corresponda exatamente ao que estiver no arquivo `.resw`. Esses nomes devem corresponder aos `Names` criados no arquivo `.resw`, prefixados com o esquema `ms-resource:` e o namespace `Resources/`. 

> [!NOTE]
> Muitos elementos do manifesto foram omitidos neste trecho de código – não excluir nada!

```xml
<?xml version="1.0" encoding="utf-8"?>
<Package>
  <Properties>
    <DisplayName>ms-resource:Resources/PackageDisplayName</DisplayName>
    <PublisherDisplayName>ms-resource:Resources/PublisherDisplayName</PublisherDisplayName>
  </Properties>
  <Applications>
    <Application>
      <uap:VisualElements DisplayName="ms-resource:Resources/ApplicationDisplayName"
        Description="ms-resource:Resources/ApplicationDescription">
        <uap:DefaultTile ShortName="ms-resource:Resources/TileShortName">
          <uap:ShowNameOnTiles>
            <uap:ShowOn Tile="square150x150Logo" />
          </uap:ShowNameOnTiles>
        </uap:DefaultTile>
      </uap:VisualElements>
    </Application>
  </Applications>
</Package>
```

Se você estiver usando o designer de manifesto do Visual Studio, abra o arquivo. AppX e altere o <span style="background-color: lightgreen">realçado valores</span> os valores no **Application* guia e o *empacotamento*guia:

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-msix-package-and-verify-its-working"></a>Etapa 1.2: Compilar arquivo PRI, faça um pacote MSIX e verificar se ele está funcionando

Agora você poderá criar o arquivo `.pri` e implantar o aplicativo para verificar se as informações corretas (em seu idioma padrão) estão aparecendo no menu Iniciar.

Se você estiver criando no Visual Studio, basta pressionar `Ctrl+Shift+B` para compilar o projeto e, em seguida, clicar com o botão direito do mouse no projeto e escolher `Deploy` no menu de contexto.

Se você estiver criando manualmente, siga estas etapas para criar um arquivo de configuração para `MakePRI` ferramenta de e para gerar o `.pri` próprio arquivo (mais informações podem ser encontradas no [empacotamento Manual de aplicativo](https://docs.microsoft.com/en-us/windows/uwp/packaging/manual-packaging-root)):

1. Abra um prompt de comando do desenvolvedor do **Visual Studio 2017** ou **2019 do Visual Studio** pasta no menu Iniciar.
2. Alternar para o diretório raiz do projeto (aquele que contém o arquivo. AppX e o **cadeias de caracteres** pasta).
3. Digite o comando a seguir, substituindo "contoso_demo.xml" por um nome adequado para seu projeto e "en-US" pelo idioma padrão do seu aplicativo (ou mantenha en-US se aplicável). Observe que o arquivo XML é criado no diretório pai (**não** no diretório do projeto), pois ele não é parte do aplicativo (você pode escolher qualquer outro diretório desejado, mas não se esqueça de substituir que comandos no futuro).

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

    Você pode digitar `makepri createconfig /?` para ver o que cada parâmetro faz, mas, em resumo:
      * `/cf` Define o nome do arquivo de configuração (a saída desse comando)
      * `/dq` Define os qualificadores padrão, neste caso, o idioma `en-US`
      * `/pv` Define a versão da plataforma, neste caso, o Windows 10
      * `/o` Define-o para substituir o arquivo de saída, se existir

4. Agora você tem um arquivo de configuração, execute `MakePRI` novamente para procurar recursos no disco e empacotá-los em um arquivo PRI. Substitua "contoso_demop.xml" pelo nome de arquivo XML usado na etapa anterior e especifique o diretório pai para entrada e saída: 

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

    Você pode digitar `makepri new /?` para ver o que cada parâmetro faz, mas, em resumo:
      * `/pr` Define a raiz do projeto (nesse caso, o diretório atual)
      * `/cf` Define o nome de arquivo de configuração criado na etapa anterior
      * `/of` Define o arquivo de saída 
      * `/mf` cria um arquivo de mapeamento (portanto, é possível excluir os arquivos no pacote em uma etapa posterior)
      * `/o` Define-o para substituir o arquivo de saída, se existir

5. Agora você tem um arquivo `.pri` com os recursos de idioma padrão (por exemplo, en-US). Para verificar se ele funcionou corretamente, execute o comando a seguir:

    ```CMD
    makepri dump /if ..\resources.pri /of ..\resources /o
    ```

    Você pode digitar `makepri dump /?` para ver o que cada parâmetro faz, mas, em resumo:
      * `/if` Define o nome do arquivo de entrada 
      * `/of` Define o nome do arquivo de saída (`.xml` será acrescentado automaticamente)
      * `/o` Define-o para substituir o arquivo de saída, se existir

6. Por fim, você pode abrir `..\resources.xml`em um editor de texto e verificar se ele lista os valores `<NamedResource>` (como `ApplicationDescription` e `PublisherDisplayName`) juntamente com os valores `<Candidate>` do idioma padrão escolhido (haverá outro conteúdo no início do arquivo; ignore isso por enquanto).

Você pode abrir o arquivo de mapeamento `..\resources.map.txt` para verificar se ele contém os arquivos necessários para seu projeto (incluindo o arquivo PRI, que não faz parte do diretório do projeto). Importante: o arquivo de mapeamento *não* incluirá uma referência ao arquivo `resources.resw` porque o conteúdo desse arquivo já foi inserido no arquivo PRI. No entanto, ele conterá outros recursos, como os nomes de arquivo das imagens.

#### <a name="building-and-signing-the-package"></a>Criando e assinando o pacote 

Agora que o arquivo PRI já foi criado, você pode criar e assinar o pacote:

1. Para criar o pacote do aplicativo, execute o comando a seguir substituindo `contoso_demo.appx` com o nome do AppX do MSIX/arquivo você deseja criar e certificando-se de escolher um diretório diferente para o arquivo (Este exemplo usa o diretório pai; ele pode estar em qualquer lugar, mas deve **não** ser o diretório do projeto).

    ```CMD
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

    Você pode digitar `makeappx pack /?` para ver o que cada parâmetro faz, mas, em resumo:
      * `/m` Define o arquivo de manifesto para uso
      * `/f` Define o mapeamento de arquivo a ser usado (criado na etapa anterior) 
      * `/p` Define a saída do nome do pacote
      * `/o` Define-o para substituir o arquivo de saída, se existir

2. Depois que o pacote é criado, ele deve ser assinado. A maneira mais fácil de obter um certificado de autenticação é criando um projeto vazio do Windows Universal no Visual Studio e copiando o `.pfx` arquivo que ele cria, mas você pode criar um manualmente usando o `MakeCert` e `Pvk2Pfx` utilitários conforme descrito em [ Como criar um certificado de assinatura de pacote do aplicativo](https://docs.microsoft.com/windows/desktop/appxpkg/how-to-create-a-package-signing-certificate).

    > [!IMPORTANT]
    > Se você criar manualmente um certificado de autenticação, certifique-se de que colocar os arquivos em um diretório diferente que seu projeto de código-fonte ou a origem do pacote, caso contrário, que ele pode obter incluído como parte do pacote, incluindo a chave privada!

3. Para assinar o pacote, use o comando a seguir. Observe que o `Publisher` especificado no elemento `Identity` do `AppxManifest.xml` deve coincidir com o `Subject` do certificado (esse **não** é o elemento `<PublisherDisplayName>`, que é o nome de exibição localizado que aparecerá para os usuários). Como sempre, substitua os nomes de arquivo `contoso_demo...` pelos nomes apropriados para o projeto e (**muito importante**) assegure que o arquivo `.pfx` não está no diretório atual (caso contrário, ele teria sido criado como parte do pacote, incluindo a chave privada de assinatura):

    ```CMD
    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx
    ```

    Você pode digitar `signtool sign /?` para ver o que cada parâmetro faz, mas, em resumo:
      * `/fd` Define o algoritmo de resumo do arquivo (SHA256 é o padrão para AppX)
      * `/a` selecionará automaticamente o melhor certificado
      * `/f` Especifica o arquivo de entrada que contém o certificado de autenticação

Por fim, você pode clicar duas vezes no arquivo `.appx` para instalá-lo ou, se preferir a linha de comando, você pode abrir um prompt do PowerShell, ir para o diretório que contém o pacote e digitar o seguinte (substituindo `contoso_demo.appx` pelo nome do pacote):

```CMD
add-appxpackage contoso_demo.appx
```

Se você receber erros informando que o certificado não é confiável, verifique se ele é adicionado ao repositório da máquina (e **não** ao repositório do usuário). Para adicionar o certificado ao repositório do computador, use a linha de comando ou o Windows Explorer.

Para usar a linha de comando:

1. Execute um prompt de comando do Visual Studio 2017 ou 2019 do Visual Studio como administrador.
2. Vá para o diretório que contém o arquivo `.cer` (Lembre-se de verificar se esse arquivo está fora dos diretórios de origem ou de projeto!)
3. Digite o comando a seguir, substituindo `contoso_demo.cer` pelo nome de arquivo:
    ```CMD
    certutil -addstore TrustedPeople contoso_demo.cer
    ```
    
    Você pode executar `certutil -addstore /?` para ver o que cada parâmetro faz, mas, em resumo:
      * `-addstore` Adiciona um certificado a um repositório de certificados
      * `TrustedPeople` indica o repositório no qual o certificado é colocado

Para usar o Windows Explorer:

1. Navegue até a pasta que contém o arquivo `.pfx`
2. Clique duas vezes no arquivo `.pfx`; o **Assistente para Importação de Certificados** aparecerá
3. Escolha `Local Machine` e clique em `Next`
4. Aceitar a solicitação de elevação do controle de conta de usuário administrador, se ela é exibida e clique em `Next`
5. Insira a senha para a chave privada, se houver um e clique em `Next`
6. Selecione `Place all certificates in the following store`
7. Clique em `Browse` e escolha a pasta `Trusted People` (e **não** "Trusted Publishers")
8. Clique em `Next` e, em seguida, `Finish`

Após adicionar o certificado ao repositório `Trusted People`, tente instalar o pacote novamente.

Você verá seu aplicativo na lista "Todos os Aplicativos" do menu Iniciar, com as informações corretas do arquivo `.resw` / `.pri`. Se aparecer uma cadeia de caracteres em branco ou a cadeia de caracteres `ms-resource:...`, algo deu errado. Verifique novamente as edições para saber se estão corretas. Se você clicar com o botão direito do mouse no seu aplicativo no menu Iniciar, poderá fixá-lo como um bloco e verificar se as informações corretas são exibidas nesse local também.

### <a name="step-13-add-more-supported-languages"></a>Etapa 1.3: Adicionar mais idiomas com suporte

Depois que as alterações foram feitas para o manifesto do pacote e a inicial `resources.resw` arquivo tiver sido criado, a adição de outros idiomas é fácil.

#### <a name="create-additional-localized-resources"></a>Criar recursos localizados adicionais

Primeiro, crie os valores de recurso localizados adicionais. 

Na pasta `Strings`, crie pastas adicionais para cada idioma com suporte usando o código BCP-47 apropriado (por exemplo, `Strings\de-DE`). Em cada uma dessas pastas, crie um arquivo `resources.resw` (usando um editor XML ou o designer do Visual Studio) que inclua os valores de recurso traduzidos. Parte-se do pressuposto de que as cadeias de caracteres localizadas já existem em algum lugar, e você só precisa copiá-las para o arquivo `.resw`; este documento não abrange a etapa de tradução. 

Por exemplo, o arquivo `Strings\de-DE\resources.resw` teria esta aparência, com o <span style="background-color: yellow">texto realçado</span> alterado de `en-US`:

```xml
<?xml version="1.0" encoding="utf-8"?>
<root>
  <data name="ApplicationDescription">
    <value>Contoso Demo app with localized resources (German)</value>
  </data>
  <data name="ApplicationDisplayName">
    <value>Contoso Demo Sample (German)</value>
  </data>
  <data name="PackageDisplayName">
    <value>Contoso Demo Package (German)</value>
  </data>
  <data name="PublisherDisplayName">
    <value>Contoso Samples, DE</value>
  </data>
  <data name="TileShortName">
    <value>Contoso (DE)</value>
  </data>
</root>
```

As etapas a seguir pressupõem que você adicionou recursos para `de-DE` e `fr-FR`, mas o mesmo padrão pode ser seguido para qualquer idioma.

#### <a name="update-the-package-manifest-to-list-supported-languages"></a>Atualiza o manifesto de pacote para os idiomas com suporte de lista

O manifesto do pacote deve ser atualizado à lista os idiomas suportados pelo aplicativo. O Desktop App Converter adiciona o idioma padrão, mas os outros devem ser adicionados explicitamente. Se você estiver editando o arquivo `AppxManifest.xml` diretamente, atualize o nó `Resources` da seguinte maneira, adicionando quantos elementos forem necessárias, substituindo os <span style="background-color: yellow">idiomas apropriados com suporte</span> e assegurando que a primeira entrada na lista é o idioma padrão (fallback). Neste exemplo, o padrão é Inglês (EUA) com suporte adicional para Alemão (Alemanha) e Francês (França):

```xml
<Resources>
  <Resource Language="EN-US" />
  <Resource Language="DE-DE" />
  <Resource Language="FR-FR" />
</Resources>
```

Se você estiver usando o Visual Studio, não precisará fazer nada; se você analisar o `Package.appxmanifest`, verá o valor especial <span style="background-color: yellow">x-generate</span>, que faz com que o processo de compilação insira os idiomas encontrados no projeto (com base nas pastas nomeadas com códigos BCP-47). Observe que isso não é um valor válido para um manifesto de pacote real; ele funciona somente para projetos do Visual Studio:

```xml
<Resources>
  <Resource Language="x-generate" />
</Resources>
```

#### <a name="re-build-with-the-localized-values"></a>Crie novamente com os valores localizados

Agora você pode compilar e implantar o aplicativo novamente e, se você alterar sua preferência de idioma no Windows, verá os valores recém-localizados no menu Iniciar (veja a seguir instruções sobre como alterar o idioma).

No Visual Studio, mais uma vez você pode simplesmente usar `Ctrl+Shift+B` para criar e clicar com o botão direito do mouse no projeto para `Deploy`.

Se você estiver criando manualmente o projeto, siga as mesmas etapas acima, mas adicione outros idiomas, separados por sublinhados, à lista de qualificadores padrão (`/dq`) ao criar o arquivo de configuração. Por exemplo, para oferecer suporte aos recursos em inglês, alemão e francês adicionados na etapa anterior:

```CMD
makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Isso criará um arquivo PRI que contém todos os idiomas especificados que você pode usar facilmente no teste. Se o tamanho total dos recursos for pequeno ou se você oferecer suporte apenas a um pequeno número de idiomas, isso pode ser aceitável no envio do aplicativo, mas apenas se você quiser os benefícios da minimização dos tamanhos de instalação/download para os recursos nos quais precisa fazer o trabalho adicional de criação de pacotes de idiomas separados.

#### <a name="test-with-the-localized-values"></a>Testar com os valores localizados

Para testar as novas alterações localizadas, basta adicionar um novo idioma preferencial de interface do usuário ao Windows. Não é necessário baixar pacotes de idiomas, reinicializar o sistema ou exibir a interface do usuário do Windows em um idioma estrangeiro. 

1. Execute o aplicativo `Settings` (`Windows + I`)
2. Ir para `Time & language`
3. Ir para `Region & language`
4. Clique em `Add a language`
5. Digite (ou selecione) o idioma desejado (por exemplo, `Deutsch` ou `German`)
 * Se houver idiomas secundários, escolha o desejado (por exemplo, `Deutsch / Deutschland`)
6. Selecione o novo idioma na lista de idiomas
7. Clique em `Set as default`

Agora, abra o menu Iniciar e procure o aplicativo; você verá os valores localizados do idioma selecionado (pode ser que outros aplicativos também apareçam localizados). Se o nome localizado não aparecer imediatamente, aguarde alguns minutos até que o cache do menu Iniciar seja atualizado. Para retornar ao idioma nativo, basta torná-lo o idioma padrão na lista de idiomas. 

### <a name="step-14-localizing-more-parts-of-the-package-manifest-optional"></a>Etapa 1.4: Localizando mais partes de manifesto do pacote (opcional)

Outras seções do manifesto do pacote podem ser localizadas. Por exemplo, se o aplicativo manipular extensões de arquivo, ele deverá ter uma extensão `windows.fileTypeAssociation` no manifesto, usando o <span style="background-color: lightgreen">texto realçado em verde</span> exatamente conforme mostrado (desde que ele faça referência aos recursos) e substituindo o <span style="background-color: yellow">texto realçado em amarelo</span> com informações específicas do aplicativo:

```xml
<Extensions>
  <uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="default">
      <uap:DisplayName>ms-resource:Resources/FileTypeDisplayName</uap:DisplayName>
      <uap:Logo>Assets\StoreLogo.png</uap:Logo>
      <uap:InfoTip>ms-resource:Resources/FileTypeInfoTip</uap:InfoTip>
      <uap:SupportedFileTypes>
        <uap:FileType ContentType="application/x-contoso">.contoso</uap:FileType>
      </uap:SupportedFileTypes>
    </uap:FileTypeAssociation>
  </uap:Extension>
</Extensions>
```

Você também pode adicionar essas informações por meio do Designer de Manifesto do Visual Studio, usando a guia `Declarations`, anotando os <span style="background-color: lightgreen">valores realçados</span>:

<p><img src="images\editing-declarations-info.png"/></p>

Agora, adicione os nomes de recurso correspondentes a cada um dos arquivos `.resw`, substituindo o <span style="background-color: yellow">texto realçado</span> pelo texto apropriado para seu aplicativo (lembre-se de fazer isso para *cada idioma com suporte!* ):

```xml
... existing content...
<data name="FileTypeDisplayName">
  <value>Contoso Demo File</value>
</data>
<data name="FileTypeInfoTip">
  <value>Files used by Contoso Demo App</value>
</data>
```

Isso aparecerá em partes do shell do Windows, como o Explorador de Arquivos:

<p><img src="images\file-type-tool-tip.png"/></p>

Compile e teste o pacote como antes, praticando quaisquer novos cenários que mostrarão as novas cadeias de caracteres da interface do usuário.

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: Usar o MRT para identificar e localizar recursos

A seção anterior mostrou como usar o MRT para localizar o arquivo de manifesto do aplicativo para que o Shell do Windows possa exibir corretamente o nome do aplicativo e outros metadados. Nenhuma alteração de código foi necessária para isso; foi necessário apenas o uso dos arquivos `.resw` e de algumas ferramentas adicionais. Esta seção mostrará como usar o MRT para localizar recursos nos formatos de recurso existentes e usando o código de manipulação de recursos existente com alterações mínimas.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Suposições sobre o layout de arquivo e o código de aplicativo existentes

Como existem muitas maneiras de localizar aplicativos de área de trabalho Win32, este documento fará algumas pressuposições simples sobre a estrutura do aplicativo existente que você precisa para mapear para seu ambiente específico. Talvez seja necessário fazer algumas alterações na base de código ou no layout de recurso existente em conformidade com os requisitos do MRT, mas isso está fora do escopo deste documento.

#### <a name="resource-file-layout"></a>Layout do arquivo de recurso

Este artigo pressupõe que os recursos localizados para todos os têm os mesmos nomes de arquivo (por exemplo, `contoso_demo.exe.mui` ou `contoso_strings.dll` ou `contoso.strings.xml`), mas que eles são colocados em pastas diferentes com nomes de BCP-47 (`en-US`, `de-DE`, etc.). Ela não importa quantos arquivos de recurso, você tem, quais são seus nomes, quais seus formatos de arquivo / associados APIs são, etc. A única coisa que importa é que cada *lógicas* recurso tem o mesmo nome de arquivo (mas posicionados em um local diferente *físico* directory). 

Por outro lado, se o aplicativo usar uma estrutura de arquivo simples com um único diretório `Resources` contendo os arquivos `english_strings.dll` e `french_strings.dll`, ele não fará um mapeamento satisfatório para o MRT. Uma estrutura melhor seria o diretório `Resources` com subdiretórios e arquivos `en\strings.dll` e `fr\strings.dll`. Também é possível usar o mesmo nome de arquivo base, mas com qualificadores inseridos, como `strings.lang-en.dll` e `strings.lang-fr.dll`; no entanto, o uso de diretórios com os códigos de idioma é conceitualmente mais simples, então, vamos nos concentrar nisso.

>[!NOTE]
> Ainda é possível usar MRT e os benefícios do empacotamento, mesmo se você não pode seguir esta convenção; de nomenclatura de arquivo Assim, ele requer mais trabalho.

Por exemplo, o aplicativo pode ter um conjunto de comandos de interface do usuário personalizados (usado nos rótulos de botão etc.) em um arquivo de texto simples denominado <span style="background-color: yellow">ui.txt</span>, disposto abaixo da pasta <span style="background-color: yellow">UICommands</span>:

<blockquote>
<pre>
+ ProjectRoot
|--+ Strings
|  |--+ en-US
|  |  \--- resources.resw
|  \--+ de-DE
|     \--- resources.resw
|--+ <span style="background-color: yellow">UICommands</span>
|  |--+ en-US
|  |  \--- <span style="background-color: yellow">ui.txt</span>
|  \--+ de-DE
|     \--- <span style="background-color: yellow">ui.txt</span>
|--- AppxManifest.xml
|--- ...rest of project...
</pre>
</blockquote>

#### <a name="resource-loading-code"></a>Código de carregamento de recursos

Este artigo pressupõe que em algum momento no seu código que você deseja localizar o arquivo que contém um recurso localizado, carregá-lo e, em seguida, usá-lo. As APIs usadas para carregar os recursos, as APIs usadas para extrair os recursos etc. não são importantes. Em pseudocódigo, há basicamente três etapas:

<blockquote>
<pre>
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
</pre>
</blockquote>

O MRT requer apenas a alteração das duas primeiras etapas deste processo: como determinar os melhores recursos candidatos e como localizá-los. Ele não exige que você mude a forma de carregamento ou uso dos recursos (embora forneça recursos para fazer isso, caso você queira aproveitá-los).

Por exemplo, o aplicativo pode usar a API Win32 `GetUserPreferredUILanguages`, a função CRT `sprintf` e a API Win32 `CreateFile` para substituir as três funções de pseudocódigo acima e, em seguida, analisar manualmente o arquivo de texto procurando os pares `name=value`. (Os detalhes não são importantes; o exemplo serve apenas para ilustrar que o MRT não tem impacto sobre as técnicas usadas para manipular recursos depois que eles são localizados).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Etapa 2.1: Alterações de código para usar MRT para localizar arquivos

Alternar o código para usar o MRT na localização de recursos não é difícil. Ele requer o uso de alguns tipos de WinRT e algumas linhas de código. Os principais tipos que você usará são:

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), que encapsula o conjunto atualmente ativo de valores de qualificador (idioma, fator de escala etc.)
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (a versão WinRT, e não a versão .NET), que permite o acesso a todos os recursos no arquivo PRI
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), que representa um subconjunto específico de recursos no arquivo PRI (neste exemplo, os recursos baseados em arquivo versus os recursos de cadeia de caracteres)
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), que representa um recurso lógico e todos os seus candidatos possíveis
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), que representa um único recurso candidato concreto 

No pseudocódigo, esta é a maneira como resolveria um determinado nome de arquivo de recurso (como `UICommands\ui.txt` no exemplo acima):

<blockquote>
<pre>
// Get the ResourceContext that applies to this app
set resourceContext = ResourceContext.GetForViewIndependentUse()
    
// Get the current ResourceManager (there's one per app)
set resourceManager = ResourceManager.Current
    
// Get the "Files" ResourceMap from the ResourceManager
set fileResources = resourceManager.MainResourceMap.GetSubtree("Files")
    
// Find the NamedResource with the logical filename we're looking for,
// by indexing into the ResourceMap
set desiredResource = fileResources["UICommands\ui.txt"]
    
// Get the ResourceCandidate that best matches our ResourceContext
set bestCandidate = desiredResource.Resolve(resourceContext)
   
// Get the string value (the filename) from the ResourceCandidate
set absoluteFileName = bestCandidate.ValueAsString
</blockquote>
</pre>

Observe que o código **não** solicita uma pasta de idioma específica, por exemplo, `UICommands\en-US\ui.txt`, mesmo que essa seja a forma como os arquivos existam no disco. Em vez disso, ele solicita o nome de arquivo *lógico*`UICommands\ui.txt` e conta com o MRT para localizar o arquivo em disco apropriado em um dos diretórios de idioma.

A partir daqui, o aplicativo de exemplo pode continuar usando `CreateFile` para carregar `absoluteFileName` e analisar os pares `name=value` exatamente como antes; nada dessa lógica precisar mudar no aplicativo. Se você estiver escrevendo em C# ou C++/CX, o código real não é muito mais complicado do que esse (e, na verdade, muitas das variáveis intermediárias podem ser omitidas); consulte a seção **Carregando recursos .NET** a seguir. Os aplicativos baseados em C++/WRL serão mais complexos devido às APIs de baixo nível baseadas em COM usadas para ativar e chamar as APIs do WinRT, mas as etapas fundamentais necessárias são as mesmas; consulte a seção **Carregando recursos MUI Win32** a seguir.

#### <a name="loading-net-resources"></a>Carregando recursos .NET

Como o .NET tem um mecanismo interno para localizar e carregar recursos (conhecidos como "Assemblies satélites"), não há nenhum código explícito a ser substituído no exemplo resumido anterior; no .NET, basta que as DLLs de recurso estejam nos diretórios apropriados que elas serão localizadas automaticamente para você. Quando um aplicativo é empacotado como um MSIX ou AppX usando pacotes de recursos, a estrutura do diretório é um pouco diferentes - em vez de ter os diretórios de recursos ser subdiretórios do diretório principal do aplicativo, eles são pares dele (ou não está presente em todos os se o usuário não tenha o idioma listado nas suas preferências). 

Por exemplo, imagine um aplicativo .NET com o layout a seguir, em que todos os arquivos estão abaixo da pasta `MainApp`:

<blockquote>
<pre>
+ MainApp
|--+ en-us
|  \--- MainApp.resources.dll
|--+ de-de
|  \--- MainApp.resources.dll
|--+ fr-fr
|  \--- MainApp.resources.dll
\--- MainApp.exe
</pre>
</blockquote>

Após a conversão em AppX, o layout terá uma aparência semelhante a esta, supondo que `en-US` foi o idioma padrão e os idiomas alemão e francês estão relacionados na lista de idiomas do usuário:

<blockquote>
<pre>
+ WindowsAppsRoot
|--+ MainApp_neutral
|  |--+ en-us
|  |  \--- <span style="background-color: yellow">MainApp.resources.dll</span>
|  \--- MainApp.exe
|--+ MainApp_neutral_resources.language_de
|  \--+ de-de
|     \--- <span style="background-color: yellow">MainApp.resources.dll</span>
\--+ MainApp_neutral_resources.language_fr
   \--+ fr-fr
      \--- <span style="background-color: yellow">MainApp.resources.dll</span>
</pre>
</blockquote>

Como os recursos localizados não existem mais nos subdiretórios abaixo do local de instalação do executável principal, a resolução de recurso .NET interna apresenta falha. Felizmente, o .NET tem um mecanismo bem definido para manipulação de tentativas de carregamento de assembly com falha: o evento `AssemblyResolve`. Um aplicativo .NET que usa o MRT deve se registrar nesse evento e fornecer o assembly ausente do subsistema do recurso .NET. 

Um exemplo sucinto de como usar as APIs do WinRT para localizar os assemblies satélites usados pelo .NET é apresentado a seguir; o código mostrado é intencionalmente compactado para mostrar uma implementação mínima, embora você possa vê-lo estreitamente mapeado para o pseudocódigo acima, com o `ResolveEventArgs` transmitido fornecendo o assembly que precisamos localizar. Uma versão executável desse código (com comentários detalhados e tratamento de erros) pode ser encontrada no arquivo `PriResourceRsolver.cs`, no [exemplo **Resolvedor de assembly .NET** do GitHub ](https://aka.ms/fvgqt4).

```csharp
static class PriResourceResolver
{
  internal static Assembly ResolveResourceDll(object sender, ResolveEventArgs args)
  {
    var fullAssemblyName = new AssemblyName(args.Name);
    var fileName = string.Format(@"{0}.dll", fullAssemblyName.Name);

    var resourceContext = ResourceContext.GetForViewIndependentUse();
    resourceContext.Languages = new[] { fullAssemblyName.CultureName };

    var resource = ResourceManager.Current.MainResourceMap.GetSubtree("Files")[fileName];

    // Note use of 'UnsafeLoadFrom' - this is required for apps installed with AppX, but
    // in general is discouraged. The full sample provides a safer wrapper of this method
    return Assembly.UnsafeLoadFrom(resource.Resolve(resourceContext).ValueAsString);
  }
}
```

Dada a classe acima, você adicionaria o seguinte em algum lugar no início do código de inicialização do aplicativo (antes que qualquer recurso localizado precise ser carregado):

```csharp
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

O tempo de execução do .NET acionará o evento `AssemblyResolve` sempre que não conseguir encontrar as DLLs de recurso; nesse momento, o manipulador de eventos fornecido localizará o arquivo desejado via MRT e retornará o assembly.

> [!NOTE]
> Se seu aplicativo já tem um `AssemblyResolve` manipulador para outras finalidades, você precisará integrar o código de resolução de recursos com o seu código existente.

#### <a name="loading-win32-mui-resources"></a>Carregando recursos MUI Win32

O carregamento de recursos MUI Win32 é basicamente igual ao carregamento de assemblies satélites .NET, só que usando código C++/CX ou C++/WRL. O uso do C++/CX oferece um código muito mais simples que corresponde estreitamente ao código C# acima, mas ele usa extensões de linguagem C++, opções de compilador e sobrecarga de tempo de execução adicional, que você possivelmente deseja evitar. Se esse for o caso, o uso do C++/WRL oferece uma solução de impacto muito menor, porém, com um código mais detalhado. No entanto, se você estiver familiarizado com a programação ATL (ou COM, em geral), o WRL lhe parecerá familiar. 

A função de exemplo a seguir mostra como usar C++/WRL para carregar uma DLL de recurso específica e retornar um `HINSTANCE` que pode ser usado para carregar ainda mais recursos através das APIs de recurso Win32 usuais. Observe que, diferente do exemplo C#, que inicializa explicitamente o `ResourceContext` com o idioma solicitado pelo tempo de execução do .NET, esse código conta com o idioma atual do usuário.

```cpp
#include <roapi.h>
#include <wrl\client.h>
#include <wrl\wrappers\corewrappers.h>
#include <Windows.ApplicationModel.resources.core.h>
#include <Windows.Foundation.h>
   
#define IF_FAIL_RETURN(hr) if (FAILED((hr))) return hr;
    
HRESULT GetMrtResourceHandle(LPCWSTR resourceFilePath,  HINSTANCE* resourceHandle)
{
  using namespace Microsoft::WRL;
  using namespace Microsoft::WRL::Wrappers;
  using namespace ABI::Windows::ApplicationModel::Resources::Core;
  using namespace ABI::Windows::Foundation;
    
  *resourceHandle = nullptr;
  HRESULT hr{ S_OK };
  RoInitializeWrapper roInit{ RO_INIT_SINGLETHREADED };
  IF_FAIL_RETURN(roInit);
    
  // Get Windows.ApplicationModel.Resources.Core.ResourceManager statics
  ComPtr<IResourceManagerStatics> resourceManagerStatics;
  IF_FAIL_RETURN(GetActivationFactory(
    HStringReference(
    RuntimeClass_Windows_ApplicationModel_Resources_Core_ResourceManager).Get(),
    &resourceManagerStatics));
    
  // Get .Current property
  ComPtr<IResourceManager> resourceManager;
  IF_FAIL_RETURN(resourceManagerStatics->get_Current(&resourceManager));
    
  // get .MainResourceMap property
  ComPtr<IResourceMap> resourceMap;
  IF_FAIL_RETURN(resourceManager->get_MainResourceMap(&resourceMap));
    
  // Call .GetValue with supplied filename
  ComPtr<IResourceCandidate> resourceCandidate;
  IF_FAIL_RETURN(resourceMap->GetValue(HStringReference(resourceFilePath).Get(),
    &resourceCandidate));
    
  // Get .ValueAsString property
  HString resolvedResourceFilePath;
  IF_FAIL_RETURN(resourceCandidate->get_ValueAsString(
    resolvedResourceFilePath.GetAddressOf()));
    
  // Finally, load the DLL and return the hInst.
  *resourceHandle = LoadLibraryEx(resolvedResourceFilePath.GetRawBuffer(nullptr),
    nullptr, LOAD_LIBRARY_AS_DATAFILE | LOAD_LIBRARY_AS_IMAGE_RESOURCE);
    
  return S_OK;
}
```

## <a name="phase-3-building-resource-packs"></a>Fase 3: Pacotes de recursos de construção

Agora que você tem um "pacote gordo" com todos os recursos, há dois caminhos para criar um pacote principal separado e pacotes de recursos, a fim de minimizar os tamanhos dos downloads e das instalações:

* Execute um pacote gordo por meio da [ferramenta de geração de pacotes](https://aka.ms/bundlegen) para criar pacotes de recursos automaticamente. Essa será a abordagem preferencial se você tiver um sistema de compilação que já produz um pacote gordo e quiser processá-lo posteriormente para gerar os pacotes de recursos.
* Produza diretamente os pacotes de recursos individuais e compile-os em um único pacote. Essa será a abordagem preferencial se você tiver mais controle sobre o sistema de compilação e puder criar os pacotes diretamente.

### <a name="step-31-creating-the-bundle"></a>Etapa 3.1: Criação do pacote

#### <a name="using-the-bundle-generator-tool"></a>Usando a ferramenta de geração de pacotes

Para usar a ferramenta de geração de pacotes, o arquivo de configuração PRI criado para o pacote precisa ser atualizado manualmente para remover a seção `<packaging>`.

Se você estiver usando o Visual Studio, consulte [Certifique-se de que os recursos são instalados em um dispositivo, independentemente se um dispositivo exige que eles](https://docs.microsoft.com/en-us/previous-versions/dn482043(v=vs.140)) para obter informações sobre como compilar todos os idiomas no pacote principal com a criação de arquivos `priconfig.packaging.xml`e `priconfig.default.xml`.

Se você estiver editando manualmente os arquivos, siga estas etapas: 

1. Crie o arquivo de configuração da mesma maneira que antes, substituindo pelo caminho, nome de arquivo e idiomas corretos:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o
    ```

2. Abra manualmente o arquivo `.xml` criado e exclua toda a seção `&lt;packaging&rt;` (mas mantenha todo o restante intacto):

    ```xml
    <?xml version="1.0" encoding="UTF-8" standalone="yes" ?> 
    <resources targetOsVersion="10.0.0" majorVersion="1">
      <!-- Packaging section has been deleted... -->
      <index root="\" startIndexAt="\">
        <default>
        ...
        ...
    ```

3. Compile o arquivo `.pri` e o pacote `.appx` como antes, usando o arquivo de configuração atualizado e o diretório e os nomes de arquivo apropriados (consulte acima para obter mais informações sobre esses comandos):

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
    ```

4. Depois que o pacote tiver sido criado, use o seguinte comando para criar o pacote, usando os nomes de arquivo e diretório apropriados:

    ```CMD
    BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo
    ```

Agora você pode mover a etapa final, assinatura (veja abaixo).

#### <a name="manually-creating-resource-packages"></a>Criando manualmente pacotes de recursos

A criação manual de pacotes de recursos requer a execução de um conjunto de comandos um pouco diferente para criar arquivos `.pri` e `.appx` separados; como esses comandos são semelhantes aos usados acima para criar pacotes gordos, não é necessária muita explicação. Observação: Todos os comandos pressupõem que o diretório atual é o diretório que contém o `AppXManifest.xml` arquivo, mas todos os arquivos são colocados no diretório pai (você pode usar um diretório diferente, se necessário, mas você não deve poluam o diretório do projeto com qualquer um dos Esses arquivos). Como sempre, substitua os nomes de arquivo "Contoso" pelos seus próprios nomes de arquivo.

1. Use o comando a seguir para criar um arquivo de configuração que nomeie **somente** o idioma padrão como qualificador padrão; neste caso, `en-US`:

    ```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
    ```

2. Crie um arquivo padrão `.pri` e `.map.txt` para o pacote principal, além de um conjunto adicional de arquivos para cada idioma encontrado no projeto, com o seguinte comando:

    ```CMD
    makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
    ```

3. Use o comando a seguir para criar o pacote principal (que contém o código executável e os recursos de idioma padrão). Como sempre, altere o nome conforme achar apropriado, embora você deva colocar o pacote em um diretório separado para facilitar a criação do pacote posteriormente (este exemplo usa o diretório `..\bundle`):

    ```CMD
    makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o
    ```

4. Depois que o pacote principal for criado, use o comando a seguir uma vez para cada idioma adicional (ou seja, repita esse comando para cada arquivo de mapa de idioma gerado na etapa anterior). Mais uma vez, a saída será gerada em um diretório separado (o mesmo do pacote principal). Observe que o idioma é especificado nas **duas** opções: `/f` e `/p`; use o novo argumento `/r` (o que indica que é necessário um pacote de recursos):

    ```CMD
    makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o
    ```

5. Combine todos os pacotes do diretório de pacotes em um único arquivo `.appxbundle`. A nova opção `/d` especifica o diretório a ser usado para todos os arquivos do pacote (isso acontece porque os arquivos `.appx` são colocados em um diretório separado na etapa anterior):

    ```CMD
    makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o
    ```

A etapa final para criar o pacote está se conectando.

### <a name="step-32-signing-the-bundle"></a>Etapa 3.2: Assinar o pacote

Depois que você criar o arquivo `.appxbundle` (seja manualmente ou por meio da ferramenta de geração de pacotes), terá um único arquivo com o pacote principal, além de todos os pacotes de recursos. A etapa final é assinar o arquivo para que o Windows o instale:

```CMD
signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle
```

Isso produzirá um arquivo `.appxbundle` assinado que contém o pacote principal, além de todos os pacotes de recursos específicos do idioma. Para instalar o aplicativo e quaisquer idiomas apropriados com base nas preferências de idioma do Windows do usuário, clique duas vezes no arquivo assinado, como você faria em um arquivo de pacote.

## <a name="related-topics"></a>Tópicos relacionados

* [Personalizar os recursos de idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md)