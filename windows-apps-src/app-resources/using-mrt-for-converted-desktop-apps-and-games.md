---
author: ptorr-msft
title: Usando o MRT para jogos e aplicativos de área de trabalho convertidos
description: Empacotando o app ou jogo .NET ou Win32 como um pacote AppX, você pode aproveitar o Sistema de Gerenciamento de Recursos para carregar recursos de app personalizados para o contexto de tempo de execução. Este tópico detalhado descreve as técnicas.
ms.author: ptorr
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp, mrt, pri. recursos, jogos, centennial, desktop app converter, mui, assembly satélite
ms.localizationpriority: medium
ms.openlocfilehash: 927e0c5438ea11b751fba40cb76210d0bce112d4
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5943016"
---
# <a name="use-the-windows-10-resource-management-system-in-a-legacy-app-or-game"></a>Use o Sistema de Gerenciamento de Recursos do Windows 10 em um app ou jogo herdado

## <a name="overview"></a>Visão geral

Os apps e jogos .NET e Win32 geralmente são localizados em diferentes idiomas para expandir totalmente o mercado ao qual se destinam. Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md). Empacotando o app ou jogo .NET ou Win32 como um pacote AppX, você pode aproveitar o Sistema de Gerenciamento de Recursos para carregar recursos de app personalizados para o contexto de tempo de execução. Este tópico detalhado descreve as técnicas.

Há muitas maneiras de localizar um aplicativo Win32 tradicionais, mas o Windows 8 introduziu um [novo sistema de gerenciamento de recursos](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx) que funciona entre linguagens de programação, entre tipos de aplicativos e fornece uma funcionalidade que transcende a simples localização. Esse sistema será chamado de "MRT" neste tópico. Antigamente, a sigla significava "Modern Resource Technology", mas o termo "Modern" deixou de ser utilizado. O gerenciador de recursos também pode ser conhecido como MRM (Modern Resource Manager) ou PRI ( Índice de Recurso do Pacote).

Combinado com a implantação baseada em AppX (por exemplo, na Microsoft Store), o MRT pode fornecer automaticamente os recursos mais aplicáveis a um determinado usuário/dispositivo, minimizando o tamanho do download e da instalação do aplicativo. Essa redução de tamanho pode ser significativa para aplicativos com uma grande quantidade de conteúdo localizado, talvez na ordem de vários *gigabytes* para jogos AAA. Os benefícios adicionais do MRT incluem listagens localizadas no Shell do Windows e na Microsoft Store, lógica de fallback automática quando o idioma preferencial de um usuário não corresponde aos recursos disponíveis.

Este documento descreve a arquitetura de alto nível do MRT e fornece um guia de portabilidade para ajudar a migrar aplicativos Win32 herdados para MRT com o mínimo de alterações no código. Depois que a migração para o MRT é feita, benefícios adicionais (por exemplo, a capacidade de segmentar recursos por fator de escala ou tema do sistema) são disponibilizados para o desenvolvedor. Observe que a localização baseada em MRT funciona para aplicativos UWP e Win32 processados pela Ponte de Desktop (também conhecida como "Centennial").

Em muitas situações, você pode continuar usando o código-fonte e os formatos de localização existentes, integrando-se, ao mesmo tempo, ao MRT para resolver recursos em tempo de execução e minimizar os tamanhos dos downloads; não é uma abordagem "tudo ou nada". A tabela a seguir resume o trabalho e o custo estimado/benefício de cada estágio. Esta tabela não inclui tarefas de não localização, como fornecimento de ícones de aplicativos de alta resolução ou de alto contraste. Para obter mais informações sobre como fornecer vários ativos para blocos, ícones etc., consulte [Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md).

<table>
<tr>
<th>Trabalho</th>
<th>Benefício</th>
<th>Custo estimado</th>
</tr>
<tr>
<td>Localizar o manifesto AppX</td>
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

Se o usuário fala um idioma diferente do inglês ou do alemão ou tem um fator de escala de exibição diferente de 100% ou 300%, o MRT seleciona o candidato correspondente "mais próximo" com base em um conjunto de regras de fallback (consulte [o tópico **Sistema de Gerenciamento de Recursos** no MSDN](https://msdn.microsoft.com/en-us/library/windows/apps/jj552947.aspx) para obter mais informações). 

Observe que o MRT dá suporte a recursos que são personalizados para mais de um qualificador; por exemplo, se a imagem do logotipo continha texto inserido que também precisava ser localizado, o logotipo teria quatro candidatos: EN/Scale-100, DE/Scale-100, EN/Scale-300 e DE/Scale-300.

### <a name="sections-in-this-document"></a>Seções deste documento

As seções a seguir descrevem as tarefas de alto nível necessárias para integrar o MRT ao aplicativo.

**Fase 0: Criar um pacote de aplicativos**

Esta seção descreve como obter o aplicativo de área de trabalho existente criando como um pacote de aplicativos. Nenhum recurso MRT é usado neste estágio.

**Fase 1: Localizar o manifesto do aplicativo**

Esta seção descreve como localizar o manifesto do aplicativo (para que ele seja exibido corretamente no Shell do Windows), usando ainda o formato de recurso herdado e a API para empacotar e localizar recursos. 

**Fase 2: Usar o MRT para identificar e localizar recursos**

Esta seção descreve como modificar o código do aplicativo (e possivelmente o layout do recurso) para localizar recursos usando o MRT, usando ainda os formatos de recurso e APIs existentes para carregar e consumir os recursos. 

**Fase 3: Criar pacotes de recursos**

Esta seção descreve as alterações finais necessárias para separar os recursos em *pacotes de recursos* distintos, minimizando o tamanho do download e (da instalação) do aplicativo.

### <a name="not-covered-in-this-document"></a>Não abordado neste documento

Após concluir as fases 0 a 3 acima, você terá um "pacote" de aplicativos que poderá ser enviado à Microsoft Store e minimizará o tamanho do download e da instalação para os usuários, omitindo os recursos não necessários (por exemplo, os idiomas que eles não falam). É possível fazer melhorias adicionais no tamanho e na funcionalidade do aplicativo através da execução de uma etapa final. 

**Etapa 4: Migrar para formatos de recurso MRT e APIs**

Esta fase está além do escopo deste documento. Ela envolve a migração dos recursos (principalmente as cadeias de caracteres) de formatos herdados, como DLLs MUI ou assemblies de recurso .NET, para arquivos PRI. Ela pode resultar em economia de espaço adicional para tamanhos de download e instalação. Ele também permite o uso de outros recursos MRT, como minimizar o download e a instalação de arquivos de imagem com base no fator de escala, nas configurações de acessibilidade etc.

- - -

## <a name="phase-0-build-an-application-package"></a>Fase 0: Criar um pacote de aplicativos

Antes de fazer qualquer alteração nos recursos do aplicativo, primeiro você deve substituir a tecnologia de empacotamento e instalação atual pela tecnologia de empacotamento e implantação de UWP padrão. Há três maneiras de fazer isso:

0. Se você tiver um aplicativo de área de trabalho grande com um instalador complexo ou utilizar muitos pontos de extensibilidade de SO, poderá usar a ferramenta Desktop App Converter para gerar o layout de arquivo UWP e as informações de manifesto a partir do instalador de aplicativo existente (por exemplo, um MSI)
0. Se você tiver um aplicativo de área de trabalho menor com poucos arquivos ou um instalador simples e nenhum gancho de extensibilidade, poderá criar o layout do arquivo e as informações do manifesto manualmente
0. Se você estiver recriando a partir da fonte e quiser atualizar o aplicativo para ser um aplicativo UWP "puro", poderá criar um novo projeto no Visual Studio e contar com o IDE para fazer grande parte do trabalho para você

Se você quiser usar o [Desktop App Converter](https://aka.ms/converter), consulte [o tópico **Ponte de Desktop para UWP: Desktop App Converter** no MSDN](https://aka.ms/converterdocs) para obter mais informações sobre o processo de conversão. Um conjunto completo de exemplos do Desktop Converter pode ser encontrado no [repositório **Exemplos de Ponte de Desktop para UWP** do GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples).

Se você quiser criar o pacote manualmente, precisará criar uma estrutura de diretórios que inclua todos os arquivos do aplicativo (executáveis e conteúdo, mas sem código-fonte) e um arquivo `AppXManifest.xml`. Um exemplo pode ser encontrado na [amostra **Olá, mundo** do GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/blob/master/Samples/HelloWorldSample/CentennialPackage/AppxManifest.xml), mas um arquivo `AppXManifest.xml` básico que executa o arquivo executável de área de trabalho chamado `ContosoDemo.exe` tem a seguinte aparência, na qual o <span style="background-color: yellow">texto realçado</span> será substituído pelos seus próprios valores:

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8" ?&gt;
&lt;Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         IgnorableNamespaces="uap mp rescap"&gt;
    &lt;Identity Name="<span style="background-color: yellow">Contoso.Demo</span>"
              Publisher="<span style="background-color: yellow">CN=Contoso.Demo</span>"
              Version="<span style="background-color: yellow">1.0.0.0</span>" /&gt;
    &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: yellow">Contoso App</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: yellow">Contoso, Inc</span>&lt;/PublisherDisplayName&gt;
    &lt;Logo&gt;Assets\StoreLogo.png&lt;/Logo&gt;
  &lt;/Properties&gt;
    &lt;Dependencies&gt;
    &lt;TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.0" 
                        MaxVersionTested="10.0.14393.0" /&gt;
  &lt;/Dependencies&gt;
    &lt;Resources&gt;
    &lt;Resource Language="<span style="background-color: yellow">en-US</span>" /&gt;
  &lt;/Resources&gt;
    &lt;Applications&gt;
    &lt;Application Id="<span style="background-color: yellow">ContosoDemo</span>" Executable="<span style="background-color: yellow">ContosoDemo.exe</span>" 
                 EntryPoint="Windows.FullTrustApplication"&gt;
    &lt;uap:VisualElements DisplayName="<span style="background-color: yellow">Contoso Demo</span>" BackgroundColor="#777777" 
                        Square150x150Logo="Assets\Square150x150Logo.png" 
                        Square44x44Logo="Assets\Square44x44Logo.png" 
        Description="<span style="background-color: yellow">Contoso Demo</span>"&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
    &lt;Capabilities&gt;
    &lt;rescap:Capability Name="runFullTrust" /&gt;
  &lt;/Capabilities&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Para obter mais informações sobre o arquivo `AppXManifest.xml` e o layout do pacote, consulte [o tópico **Manifesto do pacote de aplicativos** no MSDN](https://msdn.microsoft.com/en-us/library/windows/apps/br211474.aspx).

Por fim, se você estiver usando o Visual Studio para criar um novo projeto e migrar o código existente, consulte [a documentação do MSDN sobre a criação de um novo projeto UWP](https://msdn.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal). Você pode incluir o código existente no novo projeto, mas você provavelmente precisará fazer alterações significativas no código (especialmente na interface do usuário) para que ele seja executado como uma UWP "pura". Essas alterações estão fora do escopo deste documento.

***

## <a name="phase-1-localize-the-application-manifest"></a>Fase 1: Localizar o manifesto do aplicativo

### <a name="step-11-update-strings--assets-in-the-appxmanifest"></a>Etapa 1.1: Atualizar cadeias de caracteres e ativos no AppXManifest

Na Fase 0, você criou um arquivo `AppXManifest.xml` básico para o aplicativo (com base nos valores fornecidos ao conversor, extraídos do MSI ou inseridos manualmente no manifesto), mas ele não conterá informações localizadas nem oferecerá suporte a recursos adicionais, como ativos de alta resolução do bloco da tela inicial etc. 

Para garantir que o nome e a descrição do aplicativo serão localizados corretamente, defina alguns recursos em um conjunto de arquivos de recurso e atualize o manifesto AppX para fazer referência a eles.

**Criando um arquivo de recurso padrão**

A primeira etapa é criar um arquivo de recurso padrão no seu idioma padrão (por exemplo, Português (Brasil)). Você pode fazer isso manualmente com um editor de texto ou por meio do Designer de Recursos no Visual Studio.

Se você quiser criar os recursos manualmente:

0. Crie um arquivo XML denominado `resources.resw` e coloque-o na subpasta `Strings\en-us` do projeto. 
 * Use o código BCP-47 apropriado se seu idioma padrão não for Português (Brasil) 
0. No arquivo XML, adicione o conteúdo a seguir, no qual o <span style="background-color: yellow">texto realçado</span> é substituído pelo texto apropriado do app, no idioma padrão.

**Observação** Há restrições sobre os tamanhos de algumas dessas cadeias de caracteres. Para obter mais informações, consulte [VisualElements](/uwp/schemas/appxpackage/appxmanifestschema/element-visualelements?branch=live).

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (English)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, USA</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (EN)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

Se você quiser usar o designer no Visual Studio:

0. Crie a pasta `Strings\en-us` (ou outro idioma conforme apropriado) no projeto e adicione um **Novo Item** à pasta raiz do projeto, usando o nome padrão `resources.resw`
 * Escolha **Arquivo de Recursos (.resw)**, e não **Dicionário de Recursos** - um dicionário de recursos é um arquivo usado por aplicativos XAML
0. Usando o designer, insira as seguintes cadeias de caracteres (use os mesmos `Names`, mas substitua os `Values` pelo texto apropriado para o aplicativo):

<img src="images\editing-resources-resw.png"/>

Observação: se você começar com o designer do Visual Studio, sempre poderá editar o XML diretamente pressionando `F7`. Mas, se você começar com um arquivo XML mínimo, *o designer não reconhecerá o arquivo* por não ter vários metadados adicionais; você pode corrigir isso copiando as informações XSD clichês de um arquivo gerado pelo designer para o arquivo XML editado manualmente. 

**Atualizar o manifesto para fazer referência aos recursos**

Depois que os valores forem definidos no arquivo `.resw`, a próxima etapa será atualizar o manifesto para fazer referência às cadeias de caracteres de recurso. Mais uma vez, você pode editar um arquivo XML diretamente ou contar com o Designer de Manifesto do Visual Studio.

Se você estiver editando o XML diretamente, abra o arquivo `AppxManifest.xml` e faça as seguintes alterações nos <span style="background-color: lightgreen">valores realçados</span> - use *exatamente* este texto, e não o texto específico do seu aplicativo. Não há nenhuma exigência para o uso desses nomes de recurso exatos; você pode escolher o nome, desde que ele corresponda exatamente ao que estiver no arquivo `.resw`. Esses nomes devem corresponder aos `Names` criados no arquivo `.resw`, prefixados com o esquema `ms-resource:` e o namespace `Resources/`. 

*Observação: vários elementos do manifesto foram omitidos nesse trecho; não exclua nada!*

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;Package&gt;
  &lt;Properties&gt;
    &lt;DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PackageDisplayName</span>&lt;/DisplayName&gt;
    &lt;PublisherDisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/PublisherDisplayName</span>&lt;/PublisherDisplayName&gt;
  &lt;/Properties&gt;
  &lt;Applications&gt;
    &lt;Application&gt;
      &lt;uap:VisualElements DisplayName="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDisplayName</span>"
        Description="<span style="background-color: lightgreen">ms-resource:Resources/ApplicationDescription</span>"&gt;
        &lt;uap:DefaultTile ShortName="<span style="background-color: lightgreen">ms-resource:Resources/TileShortName</span>"&gt;
          &lt;uap:ShowNameOnTiles&gt;
            &lt;uap:ShowOn Tile="square150x150Logo" /&gt;
          &lt;/uap:ShowNameOnTiles&gt;
        &lt;/uap:DefaultTile&gt;
      &lt;/uap:VisualElements&gt;
    &lt;/Application&gt;
  &lt;/Applications&gt;
&lt;/Package&gt;
</pre>
</blockquote>

Se você estiver usando o designer de manifesto do Visual Studio, abra o arquivo `Package.appxmanifest` e altere os <span style="background-color: lightgreen">valores realçados</span> nas guias `Application` e `Packaging`:

<img src="images\editing-application-info.png"/>
<img src="images\editing-packaging-info.png"/>

### <a name="step-12-build-pri-file-make-an-appx-package-and-verify-its-working"></a>Etapa 1.2: Criar o arquivo PRI, criar um pacote AppX e verificar se ele está funcionando

Agora você poderá criar o arquivo `.pri` e implantar o aplicativo para verificar se as informações corretas (em seu idioma padrão) estão aparecendo no menu Iniciar. 

Se você estiver criando no Visual Studio, basta pressionar `Ctrl+Shift+B` para compilar o projeto e, em seguida, clicar com o botão direito do mouse no projeto e escolher `Deploy` no menu de contexto. 

Se você estiver criando manualmente, siga estas etapas para criar um arquivo de configuração para a ferramenta `MakePRI` e gerar o arquivo `.pri` (mais informações podem ser encontradas no [tópico **Empacotamento manual de aplicativos** no MSDN](https://docs.microsoft.com/en-us/windows/uwp/packaging/manual-packaging-root)):

0. Abra um prompt de comando de desenvolvedor na pasta `Visual Studio 2015` no menu Iniciar
0. Alterne para o diretório raiz do projeto (aquele que contém o arquivo `AppxManifest.xml` e a pasta `Strings`)
0. Digite o comando a seguir, substituindo "contoso_demo.xml" por um nome adequado para seu projeto e "en-US" pelo idioma padrão do seu aplicativo (ou mantenha en-US se aplicável). Observe que o arquivo xml é criado no diretório pai (e **não** no diretório do projeto), pois ele não é parte do aplicativo (você pode escolher qualquer outro diretório, mas não se esqueça de substituir isso nos comandos futuros).

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o
```

0. Você pode digitar `makepri createconfig /?` para ver o que cada parâmetro faz, mas, em resumo:
 * `/cf` define o nome de arquivo de configuração (a saída deste comando)
 * `/dq` define os qualificadores padrão; neste caso, o idioma `en-US`
 * `/pv` define a versão da plataforma; neste caso, Windows 10
 * `/o` define-o para substituir o arquivo de saída, se houver
0. Agora você tem um arquivo de configuração, execute `MakePRI` novamente para procurar recursos no disco e empacotá-los em um arquivo PRI. Substitua "contoso_demop.xml" pelo nome de arquivo XML usado na etapa anterior e especifique o diretório pai para entrada e saída: 

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. Você pode digitar `makepri new /?` para ver o que cada parâmetro faz, mas, em resumo:
 * `/pr` define a raiz do projeto (neste caso, o diretório atual)
 * `/cf` define o nome de arquivo de configuração, criado na etapa anterior
 * `/of` define o arquivo de saída 
 * `/mf` cria um arquivo de mapeamento (portanto, podemos excluir arquivos no pacote em uma etapa posterior)
 * `/o` define-o para substituir o arquivo de saída, se houver
0. Agora você tem um arquivo `.pri` com os recursos de idioma padrão (por exemplo, en-US). Para verificar se ele funcionou corretamente, execute o comando a seguir:

    `makepri dump /if ..\resources.pri /of ..\resources /o`
0. Você pode digitar `makepri dump /?` para ver o que cada parâmetro faz, mas, em resumo:
 * `/if` define o nome de arquivo de entrada 
 * `/of` define o nome de arquivo de saída (`.xml` será acrescentado automaticamente)
 * `/o` define-o para substituir o arquivo de saída, se houver
0. Por fim, você pode abrir `..\resources.xml`em um editor de texto e verificar se ele lista os valores `<NamedResource>` (como `ApplicationDescription` e `PublisherDisplayName`) juntamente com os valores `<Candidate>` do idioma padrão escolhido (haverá outro conteúdo no início do arquivo; ignore isso por enquanto).

Se você quiser, poderá abrir o arquivo de mapeamento `..\resources.map.txt` para verificar se ele contém os arquivos necessários ao projeto (incluindo o arquivo PRI, que não faz parte do diretório do projeto). Importante: o arquivo de mapeamento *não* incluirá uma referência ao arquivo `resources.resw` porque o conteúdo desse arquivo já foi inserido no arquivo PRI. No entanto, ele conterá outros recursos, como os nomes de arquivo das imagens.

**Criando e assinando o pacote**

Agora que o arquivo PRI já foi criado, você pode criar e assinar o pacote:

0. Para criar o pacote de aplicativos, execute o comando a seguir substituindo `contoso_demo.appx` pelo nome do arquivo AppX que você deseja criar e escolhendo outro diretório para o arquivo `.AppX` (este exemplo usa o diretório pai; ele pode estar em qualquer lugar, mas **não** deve ser o diretório do projeto):

    `makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o`
0. Você pode digitar `makeappx pack /?` para ver o que cada parâmetro faz, mas, em resumo:
 * `/m` define o arquivo de manifesto a ser usado
 * `/f` define o arquivo de mapeamento a ser usado (criado na etapa anterior) 
 * `/p` define o nome do pacote de saída
 * `/o` define-o para substituir o arquivo de saída, se houver
0. Depois que o pacote é criado, ele deve ser assinado. A maneira mais fácil de obter um certificado de assinatura é criando um projeto Windows Universal vazio no Visual Studio e copiando o `.pfx` arquivo, ele cria, mas você pode criar um manualmente usando o `MakeCert` e `Pvk2Pfx` utilitários conforme descrito em [o **como criar um certificado de assinatura de pacote de aplicativo** tópico no MSDN] (https://msdn.microsoft.com/en-us/library/windows/desktop/jj835832(v=vs.85).aspx). 
 * **Importante:** se você criar manualmente um certificado de assinatura, verifique se colocou os arquivos em um diretório que não seja o diretório de origem do pacote ou o diretório do projeto; caso contrário, ele poderá fazer parte do pacote, incluindo a chave privada!
0. Para assinar o pacote, use o comando a seguir. Observe que o `Publisher` especificado no elemento `Identity` do `AppxManifest.xml` deve coincidir com o `Subject` do certificado (esse **não** é o elemento `<PublisherDisplayName>`, que é o nome de exibição localizado que aparecerá para os usuários). Como sempre, substitua os nomes de arquivo `contoso_demo...` pelos nomes apropriados para o projeto e (**muito importante**) assegure que o arquivo `.pfx` não está no diretório atual (caso contrário, ele teria sido criado como parte do pacote, incluindo a chave privada de assinatura):

    `signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appx`
0. Você pode digitar `signtool sign /?` para ver o que cada parâmetro faz, mas, em resumo:
 * `/fd` define o algoritmo de síntese de arquivo (SHA256 é o padrão para AppX)
 * `/a` selecionará automaticamente o melhor certificado
 * `/f` especifica o arquivo de entrada que contém o certificado de assinatura

Por fim, você pode clicar duas vezes no arquivo `.appx` para instalá-lo ou, se preferir a linha de comando, você pode abrir um prompt do PowerShell, ir para o diretório que contém o pacote e digitar o seguinte (substituindo `contoso_demo.appx` pelo nome do pacote):

```CMD
    add-appxpackage contoso_demo.appx
```

Se você receber erros informando que o certificado não é confiável, verifique se ele é adicionado ao repositório da máquina (e **não** ao repositório do usuário). Para adicionar o certificado ao repositório do computador, use a linha de comando ou o Windows Explorer.

Para usar a linha de comando:

0. Execute um prompt de comando do Visual Studio 2015 como administrador.
0. Vá para o diretório que contém o arquivo `.cer` (Lembre-se de verificar se esse arquivo está fora dos diretórios de origem ou de projeto!)
0. Digite o comando a seguir, substituindo `contoso_demo.cer` pelo nome de arquivo:

    `certutil -addstore TrustedPeople contoso_demo.cer`
0. Você pode executar `certutil -addstore /?` para ver o que cada parâmetro faz, mas, em resumo:
 * `-addstore` adiciona um certificado a um repositório de certificados
 * `TrustedPeople` indica o repositório em que o certificado é colocado

Para usar o Windows Explorer:

0. Navegue até a pasta que contém o arquivo `.pfx`
0. Clique duas vezes no arquivo `.pfx`; o **Assistente para Importação de Certificados** aparecerá
0. Escolha `Local Machine` e clique em `Next`
0. Aceite a solicitação de elevação a administração do Controle de Conta de Usuário, caso ele apareça, e clique em `Next`
0. Insira a senha da chave privada, se houver, e clique em `Next`
0. Selecione `Place all certificates in the following store`
0. Clique em `Browse` e escolha a pasta `Trusted People` (e **não** "Trusted Publishers")
0. Clique em `Next` e, em seguida `Finish`

Após adicionar o certificado ao repositório `Trusted People`, tente instalar o pacote novamente.

Você verá seu aplicativo na lista "Todos os Aplicativos" do menu Iniciar, com as informações corretas do arquivo `.resw` / `.pri`. Se aparecer uma cadeia de caracteres em branco ou a cadeia de caracteres `ms-resource:...`, algo deu errado. Verifique novamente as edições para saber se estão corretas. Se você clicar com o botão direito do mouse no seu aplicativo no menu Iniciar, poderá fixá-lo como um bloco e verificar se as informações corretas são exibidas nesse local também.

### <a name="step-13-add-more-supported-languages"></a>Etapa 1.3: Adicionar mais idiomas com suporte

Depois que as alterações forem feitas no manifesto AppX e o arquivo inicial `resources.resw` for criado, adicionar outros idiomas será fácil.

**Criar recursos localizados adicionais**

Primeiro, crie os valores de recurso localizados adicionais. 

Na pasta `Strings`, crie pastas adicionais para cada idioma com suporte usando o código BCP-47 apropriado (por exemplo, `Strings\de-DE`). Em cada uma dessas pastas, crie um arquivo `resources.resw` (usando um editor XML ou o designer do Visual Studio) que inclua os valores de recurso traduzidos. Parte-se do pressuposto de que as cadeias de caracteres localizadas já existem em algum lugar, e você só precisa copiá-las para o arquivo `.resw`; este documento não abrange a etapa de tradução. 

Por exemplo, o arquivo `Strings\de-DE\resources.resw` teria esta aparência, com o <span style="background-color: yellow">texto realçado</span> alterado de `en-US`:

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="utf-8"?&gt;
&lt;root&gt;
  &lt;data name="ApplicationDescription"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo app with localized resources (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="ApplicationDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Sample (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PackageDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Demo Package (German)</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="PublisherDisplayName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso Samples, DE</span>&lt;/value&gt;
  &lt;/data&gt;
  &lt;data name="TileShortName"&gt;
    &lt;value&gt;<span style="background-color: yellow">Contoso (DE)</span>&lt;/value&gt;
  &lt;/data&gt;
&lt;/root&gt;
</pre>
</blockquote>

As etapas a seguir pressupõem que você adicionou recursos para `de-DE` e `fr-FR`, mas o mesmo padrão pode ser seguido para qualquer idioma.

**Atualizar o manifesto AppX para listar idiomas com suporte**

O manifesto AppX deve ser atualizado para listar os idiomas compatíveis com o aplicativo. O Desktop App Converter adiciona o idioma padrão, mas os outros devem ser adicionados explicitamente. Se você estiver editando o arquivo `AppxManifest.xml` diretamente, atualize o nó `Resources` da seguinte maneira, adicionando quantos elementos forem necessárias, substituindo os <span style="background-color: yellow">idiomas apropriados com suporte</span> e assegurando que a primeira entrada na lista é o idioma padrão (fallback). Neste exemplo, o padrão é Inglês (EUA) com suporte adicional para Alemão (Alemanha) e Francês (França):

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">EN-US</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">DE-DE</span>" /&gt;
  &lt;Resource Language="<span style="background-color: yellow">FR-FR</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

Se você estiver usando o Visual Studio, não precisará fazer nada; se você analisar o `Package.appxmanifest`, verá o valor especial <span style="background-color: yellow">x-generate</span>, que faz com que o processo de compilação insira os idiomas encontrados no projeto (com base nas pastas nomeadas com códigos BCP-47). Observe que isso não é um valor válido para um manifesto Appx real; ele só funciona para projetos do Visual Studio:

<blockquote>
<pre>
&lt;Resources&gt;
  &lt;Resource Language="<span style="background-color: yellow">x-generate</span>" /&gt;
&lt;/Resources&gt;
</pre>
</blockquote>

**Crie novamente com os valores localizados**

Agora você pode compilar e implantar o aplicativo novamente e, se você alterar sua preferência de idioma no Windows, verá os valores recém-localizados no menu Iniciar (veja a seguir instruções sobre como alterar o idioma).

No Visual Studio, mais uma vez você pode simplesmente usar `Ctrl+Shift+B` para criar e clicar com o botão direito do mouse no projeto para `Deploy`.

Se você estiver criando manualmente o projeto, siga as mesmas etapas acima, mas adicione outros idiomas, separados por sublinhados, à lista de qualificadores padrão (`/dq`) ao criar o arquivo de configuração. Por exemplo, para oferecer suporte aos recursos em inglês, alemão e francês adicionados na etapa anterior:

```CMD
    makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_fr-FR /pv 10.0 /o
```

Isso criará um arquivo PRI que contém todos os idiomas especificados que você pode usar facilmente no teste. Se o tamanho total dos recursos for pequeno ou se você oferecer suporte apenas a um pequeno número de idiomas, isso pode ser aceitável no envio do aplicativo, mas apenas se você quiser os benefícios da minimização dos tamanhos de instalação/download para os recursos nos quais precisa fazer o trabalho adicional de criação de pacotes de idiomas separados.

**Testar com os valores localizados**

Para testar as novas alterações localizadas, basta adicionar um novo idioma preferencial de interface do usuário ao Windows. Não é necessário baixar pacotes de idiomas, reinicializar o sistema ou exibir a interface do usuário do Windows em um idioma estrangeiro. 

0. Execute o aplicativo `Settings` (`Windows + I`)
0. Vá para `Time & language`
0. Vá para `Region & language`
0. Clique em `Add a language`
0. Digite (ou selecione) o idioma desejado (por exemplo, `Deutsch` ou `German`)
 * Se houver idiomas secundários, escolha o desejado (por exemplo, `Deutsch / Deutschland`)
0. Selecione o novo idioma na lista de idiomas
0. Clique em `Set as default`

Agora, abra o menu Iniciar e procure o aplicativo; você verá os valores localizados do idioma selecionado (pode ser que outros aplicativos também apareçam localizados). Se o nome localizado não aparecer imediatamente, aguarde alguns minutos até que o cache do menu Iniciar seja atualizado. Para retornar ao idioma nativo, basta torná-lo o idioma padrão na lista de idiomas. 

### <a name="step-14-localizing-more-parts-of-the-appx-manifest-optional"></a>Etapa 1.4: Localizando mais partes do manifesto AppX (opcional)

Outras seções do manifesto AppX podem ser localizadas. Por exemplo, se o aplicativo manipular extensões de arquivo, ele deverá ter uma extensão `windows.fileTypeAssociation` no manifesto, usando o <span style="background-color: lightgreen">texto realçado em verde</span> exatamente conforme mostrado (desde que ele faça referência aos recursos) e substituindo o <span style="background-color: yellow">texto realçado em amarelo</span> com informações específicas do aplicativo:

<blockquote>
<pre>
&lt;Extensions&gt;
  &lt;uap:Extension Category="windows.fileTypeAssociation"&gt;
    &lt;uap:FileTypeAssociation Name="default"&gt;
      &lt;uap:DisplayName&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeDisplayName</span>&lt;/uap:DisplayName&gt;
      &lt;uap:Logo&gt;<span style="background-color: yellow">Assets\StoreLogo.png</span>&lt;/uap:Logo&gt;
      &lt;uap:InfoTip&gt;<span style="background-color: lightgreen">ms-resource:Resources/FileTypeInfoTip</span>&lt;/uap:InfoTip&gt;
      &lt;uap:SupportedFileTypes&gt;
        &lt;uap:FileType ContentType="<span style="background-color: yellow">application/x-contoso</span>"&gt;<span style="background-color: yellow">.contoso</span>&lt;/uap:FileType&gt;
      &lt;/uap:SupportedFileTypes&gt;
    &lt;/uap:FileTypeAssociation&gt;
  &lt;/uap:Extension&gt;
&lt;/Extensions&gt;
</pre>
</blockquote>

Você também pode adicionar essas informações por meio do Designer de Manifesto do Visual Studio, usando a guia `Declarations`, anotando os <span style="background-color: lightgreen">valores realçados</span>:

<p><img src="images\editing-declarations-info.png"/></p>

Agora, adicione os nomes de recurso correspondentes a cada um dos arquivos `.resw`, substituindo o <span style="background-color: yellow">texto realçado</span> pelo texto apropriado para seu aplicativo (lembre-se de fazer isso para *cada idioma com suporte!*):

<blockquote>
<pre>
... existing content...

&lt;data name="FileTypeDisplayName"&gt;
  &lt;value&gt;<span style="background-color: yellow">Contoso Demo File</span>&lt;/value&gt;
&lt;/data&gt;
&lt;data name="FileTypeInfoTip"&gt;
  &lt;value&gt;<span style="background-color: yellow">Files used by Contoso Demo App</span>&lt;/value&gt;
&lt;/data&gt;
</pre>
</blockquote>

Isso aparecerá em partes do shell do Windows, como o Explorador de Arquivos:

<p><img src="images\file-type-tool-tip.png"/></p>

Compile e teste o pacote como antes, praticando quaisquer novos cenários que mostrarão as novas cadeias de caracteres da interface do usuário.

- - -

## <a name="phase-2-use-mrt-to-identify-and-locate-resources"></a>Fase 2: Usar o MRT para identificar e localizar recursos

A seção anterior mostrou como usar o MRT para localizar o arquivo de manifesto do aplicativo para que o Shell do Windows possa exibir corretamente o nome do aplicativo e outros metadados. Nenhuma alteração de código foi necessária para isso; foi necessário apenas o uso dos arquivos `.resw` e de algumas ferramentas adicionais. Esta seção mostrará como usar o MRT para localizar recursos nos formatos de recurso existentes e usando o código de manipulação de recursos existente com alterações mínimas.

### <a name="assumptions-about-existing-file-layout--application-code"></a>Suposições sobre o layout de arquivo e o código de aplicativo existentes

Como existem muitas maneiras de localizar aplicativos de área de trabalho Win32, este documento fará algumas pressuposições simples sobre a estrutura do aplicativo existente que você precisa para mapear para seu ambiente específico. Talvez seja necessário fazer algumas alterações na base de código ou no layout de recurso existente em conformidade com os requisitos do MRT, mas isso está fora do escopo deste documento.

**Layout do arquivo de recurso**

Este white paper presume que os recursos localizados tenham os mesmos nomes de arquivo (por exemplo, `contoso_demo.exe.mui`, `contoso_strings.dll` ou `contoso.strings.xml`), mas que sejam colocados em diferentes pastas com nomes BCP-47 (`en-US`, `de-DE` etc.). Não importa quantos arquivos de recurso você tenha, quais são seus nomes, quais são seus formatos de arquivo/APIs associadas etc. O que importa é que cada recurso *lógico* tenha o mesmo nome de arquivo (mas seja colocado em um diretório *físico* diferente). 

Por outro lado, se o aplicativo usar uma estrutura de arquivo simples com um único diretório `Resources` contendo os arquivos `english_strings.dll` e `french_strings.dll`, ele não fará um mapeamento satisfatório para o MRT. Uma estrutura melhor seria o diretório `Resources` com subdiretórios e arquivos `en\strings.dll` e `fr\strings.dll`. Também é possível usar o mesmo nome de arquivo base, mas com qualificadores inseridos, como `strings.lang-en.dll` e `strings.lang-fr.dll`; no entanto, o uso de diretórios com os códigos de idioma é conceitualmente mais simples, então, vamos nos concentrar nisso.

**Observação** Ainda é possível usar o MRT e os benefícios do empacotamento AppX, mesmo se você não puder seguir essa convenção de nomenclatura de arquivo; isso apenas requer mais trabalho.

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

**Código de carregamento de recursos**

Este white paper supõe que, em algum ponto do código, você precisará localizar o arquivo que contém um recurso localizado, carregá-lo e usá-lo. As APIs usadas para carregar os recursos, as APIs usadas para extrair os recursos etc. não são importantes. Em pseudocódigo, há basicamente três etapas:

```
set userLanguage = GetUsersPreferredLanguage()
set resourceFile = FindResourceFileForLanguage(MY_RESOURCE_NAME, userLanguage)
set resource = LoadResource(resourceFile) 
    
// now use 'resource' however you want
```

O MRT requer apenas a alteração das duas primeiras etapas deste processo: como determinar os melhores recursos candidatos e como localizá-los. Ele não exige que você mude a forma de carregamento ou uso dos recursos (embora forneça recursos para fazer isso, caso você queira aproveitá-los).
 
Por exemplo, o aplicativo pode usar a API Win32 `GetUserPreferredUILanguages`, a função CRT `sprintf` e a API Win32 `CreateFile` para substituir as três funções de pseudocódigo acima e, em seguida, analisar manualmente o arquivo de texto procurando os pares `name=value`. (Os detalhes não são importantes; o exemplo serve apenas para ilustrar que o MRT não tem impacto sobre as técnicas usadas para manipular recursos depois que eles são localizados).

### <a name="step-21-code-changes-to-use-mrt-to-locate-files"></a>Etapa 2.1: Alterações de código para usar o MRT na localização de arquivos

Alternar o código para usar o MRT na localização de recursos não é difícil. Ele requer o uso de alguns tipos de WinRT e algumas linhas de código. Os principais tipos que você usará são:

* [ResourceContext](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext), que encapsula o conjunto atualmente ativo de valores de qualificador (idioma, fator de escala etc.)
* [ResourceManager](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemanager) (a versão WinRT, e não a versão .NET), que permite o acesso a todos os recursos no arquivo PRI
* [ResourceMap](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcemap), que representa um subconjunto específico de recursos no arquivo PRI (neste exemplo, os recursos baseados em arquivo versus os recursos de cadeia de caracteres)
* [NamedResource](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource), que representa um recurso lógico e todos os seus candidatos possíveis
* [ResourceCandidate](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.resources.core.resourcecandidate), que representa um único recurso candidato concreto 

No pseudocódigo, esta é a maneira como resolveria um determinado nome de arquivo de recurso (como `UICommands\ui.txt` no exemplo acima):

```
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
```

Observe que o código **não** solicita uma pasta de idioma específica, por exemplo, `UICommands\en-US\ui.txt`, mesmo que essa seja a forma como os arquivos existam no disco. Em vez disso, ele solicita o nome de arquivo *lógico* `UICommands\ui.txt` e conta com o MRT para localizar o arquivo em disco apropriado em um dos diretórios de idioma.

A partir daqui, o aplicativo de exemplo pode continuar usando `CreateFile` para carregar `absoluteFileName` e analisar os pares `name=value` exatamente como antes; nada dessa lógica precisar mudar no aplicativo. Se você estiver escrevendo em C# ou C++/CX, o código real não é muito mais complicado do que esse (e, na verdade, muitas das variáveis intermediárias podem ser omitidas); consulte a seção **Carregando recursos .NET** a seguir. Os aplicativos baseados em C++/WRL serão mais complexos devido às APIs de baixo nível baseadas em COM usadas para ativar e chamar as APIs do WinRT, mas as etapas fundamentais necessárias são as mesmas; consulte a seção **Carregando recursos MUI Win32** a seguir.

**Carregando recursos .NET**

Como o .NET tem um mecanismo interno para localizar e carregar recursos (conhecidos como "Assemblies satélites"), não há nenhum código explícito a ser substituído no exemplo resumido anterior; no .NET, basta que as DLLs de recurso estejam nos diretórios apropriados que elas serão localizadas automaticamente para você. Quando um aplicativo é empacotado como um AppX por meio dos pacotes de recursos, a estrutura de diretórios é um pouco diferente; em vez de os diretórios de recursos serem subdiretórios do diretório de aplicativo principal, eles são pares dele (ou nem aparecerão se o usuário não tiver o idioma listado em suas preferências). 

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

```C#
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

```C#
void EnableMrtResourceLookup()
{
  AppDomain.CurrentDomain.AssemblyResolve += PriResourceResolver.ResolveResourceDll;
}
```

O tempo de execução do .NET acionará o evento `AssemblyResolve` sempre que não conseguir encontrar as DLLs de recurso; nesse momento, o manipulador de eventos fornecido localizará o arquivo desejado via MRT e retornará o assembly.

**Observação** Se o app já tiver um manipulador `AssemblyResolve` para outros fins, será necessário integrar o código de resolução de recurso ao código existente.

**Carregando recursos MUI Win32**

O carregamento de recursos MUI Win32 é basicamente igual ao carregamento de assemblies satélites .NET, só que usando código C++/CX ou C++/WRL. O uso do C++/CX oferece um código muito mais simples que corresponde estreitamente ao código C# acima, mas ele usa extensões de linguagem C++, opções de compilador e sobrecarga de tempo de execução adicional, que você possivelmente deseja evitar. Se esse for o caso, o uso do C++/WRL oferece uma solução de impacto muito menor, porém, com um código mais detalhado. No entanto, se você estiver familiarizado com a programação ATL (ou COM, em geral), o WRL lhe parecerá familiar. 

A função de exemplo a seguir mostra como usar C++/WRL para carregar uma DLL de recurso específica e retornar um `HINSTANCE` que pode ser usado para carregar ainda mais recursos através das APIs de recurso Win32 usuais. Observe que, diferente do exemplo C#, que inicializa explicitamente o `ResourceContext` com o idioma solicitado pelo tempo de execução do .NET, esse código conta com o idioma atual do usuário.

```CPP
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

## <a name="phase-3-building-resource-packs"></a>Fase 3: Criando pacotes de recursos

Agora que você tem um "pacote gordo" com todos os recursos, há dois caminhos para criar um pacote principal separado e pacotes de recursos, a fim de minimizar os tamanhos dos downloads e das instalações:

0. Execute um pacote gordo por meio da [ferramenta de geração de pacotes](https://aka.ms/bundlegen) para criar pacotes de recursos automaticamente. Essa será a abordagem preferencial se você tiver um sistema de compilação que já produz um pacote gordo e quiser processá-lo posteriormente para gerar os pacotes de recursos.
0. Produza diretamente os pacotes de recursos individuais e compile-os em um único pacote. Essa será a abordagem preferencial se você tiver mais controle sobre o sistema de compilação e puder criar os pacotes diretamente.

### <a name="step-31-creating-the-bundle"></a>Etapa 3.1: Criando o pacote

**Usando a ferramenta de geração de pacotes**

Para usar a ferramenta de geração de pacotes, o arquivo de configuração PRI criado para o pacote precisa ser atualizado manualmente para remover a seção `<packaging>`.

Se você estiver usando o Visual Studio, consulte [o tópico **Garantir que os recursos sejam instalados...** no MSDN](https://msdn.microsoft.com/en-us/library/dn482043.aspx) para obter informações sobre como criar todos os idiomas no pacote principal, criando os arquivos `priconfig.packaging.xml` e `priconfig.default.xml`.

Se você estiver editando manualmente os arquivos, siga estas etapas: 

0. Crie o arquivo de configuração da mesma maneira que antes, substituindo pelo caminho, nome de arquivo e idiomas corretos:

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US_de-DE_es-MX /pv 10.0 /o`
0. Abra manualmente o arquivo `.xml` criado e exclua toda a seção `&lt;packaging&rt;` (mas mantenha todo o restante intacto):

<blockquote>
<pre>
&lt;?xml version="1.0" encoding="UTF-8" standalone="yes" ?&gt; 
&lt;resources targetOsVersion="10.0.0" majorVersion="1"&gt;
  &lt;!-- Packaging section has been deleted... --&gt;
  &lt;index root="\" startIndexAt="\"&gt;
    &lt;default&gt;
    ...
    ...
</pre>
</blockquote>

0. Compile o arquivo `.pri` e o pacote `.appx` como antes, usando o arquivo de configuração atualizado e o diretório e os nomes de arquivo apropriados (consulte acima para obter mais informações sobre esses comandos):

```CMD
makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o
makeappx pack /m AppXManifest.xml /f ..\resources.map.txt /p ..\contoso_demo.appx /o
```

0. Depois que o pacote for criado, use o comando a seguir para criar o pacote, usando o diretório e os nomes de arquivo apropriados:

    `BundleGenerator.exe -Package ..\contoso_demo.appx -Destination ..\bundle -BundleName contoso_demo`

Agora você pode passar para a etapa final, que é a assinatura (veja abaixo).

**Criando manualmente pacotes de recursos**

A criação manual de pacotes de recursos requer a execução de um conjunto de comandos um pouco diferente para criar arquivos `.pri` e `.appx` separados; como esses comandos são semelhantes aos usados acima para criar pacotes gordos, não é necessária muita explicação. Observação: todos os comandos pressupõem que o diretório atual é o diretório que contém o arquivo `AppXManifest.xml`, mas todos os arquivos são colocados no diretório pai (você pode usar um diretório diferente, se necessário, mas não deve poluir o diretório do projeto com nenhum desses arquivos). Como sempre, substitua os nomes de arquivo "Contoso" pelos seus próprios nomes de arquivo.

0. Use o comando a seguir para criar um arquivo de configuração que nomeie **somente** o idioma padrão como qualificador padrão; neste caso, `en-US`:

    `makepri createconfig /cf ..\contoso_demo.xml /dq en-US /pv 10.0 /o`
0. Crie um arquivo padrão `.pri` e `.map.txt` para o pacote principal, além de um conjunto adicional de arquivos para cada idioma encontrado no projeto, com o seguinte comando:

    `makepri new /pr . /cf ..\contoso_demo.xml /of ..\resources.pri /mf AppX /o`
0. Use o comando a seguir para criar o pacote principal (que contém o código executável e os recursos de idioma padrão). Como sempre, altere o nome conforme achar apropriado, embora você deva colocar o pacote em um diretório separado para facilitar a criação do pacote posteriormente (este exemplo usa o diretório `..\bundle`):

    `makeappx pack /m .\AppXManifest.xml /f ..\resources.map.txt /p ..\bundle\contoso_demo.main.appx /o`
0. Depois que o pacote principal for criado, use o comando a seguir uma vez para cada idioma adicional (ou seja, repita esse comando para cada arquivo de mapa de idioma gerado na etapa anterior). Mais uma vez, a saída será gerada em um diretório separado (o mesmo do pacote principal). Observe que o idioma é especificado nas **duas** opções: `/f` e `/p`; use o novo argumento `/r` (o que indica que é necessário um pacote de recursos):

    `makeappx pack /r /m .\AppXManifest.xml /f ..\resources.language-de.map.txt /p ..\bundle\contoso_demo.de.appx /o`
0. Combine todos os pacotes do diretório de pacotes em um único arquivo `.appxbundle`. A nova opção `/d` especifica o diretório a ser usado para todos os arquivos do pacote (isso acontece porque os arquivos `.appx` são colocados em um diretório separado na etapa anterior):

    `makeappx bundle /d ..\bundle /p ..\contoso_demo.appxbundle /o`

A etapa final de compilação do pacote é a assinatura.

### <a name="step-32-signing-the-bundle"></a>Etapa 3.2: Assinando o pacote

Depois que você criar o arquivo `.appxbundle` (seja manualmente ou por meio da ferramenta de geração de pacotes), terá um único arquivo com o pacote principal, além de todos os pacotes de recursos. A etapa final é assinar o arquivo para que o Windows o instale:

    signtool sign /fd SHA256 /a /f ..\contoso_demo_key.pfx ..\contoso_demo.appxbundle

Isso produzirá um arquivo `.appxbundle` assinado que contém o pacote principal, além de todos os pacotes de recursos específicos do idioma. Para instalar o aplicativo e quaisquer idiomas apropriados com base nas preferências de idioma do Windows do usuário, clique duas vezes no arquivo assinado, como você faria em um arquivo de pacote.

## <a name="related-topics"></a>Tópicos relacionados

* [Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md)