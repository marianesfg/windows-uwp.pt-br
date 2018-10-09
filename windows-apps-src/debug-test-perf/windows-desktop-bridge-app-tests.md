---
author: PatrickFarley
ms.assetid: 2f76c520-84a3-4066-8eb3-ecc0ecd198a7
title: Testes do app de Ponte de Desktop do Windows
description: Use testes internos da ponte de Desktop para garantir que seu aplicativo da área de trabalho é otimizado para a sua conversão em um aplicativo UWP.
ms.author: pafarley
ms.date: 12/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, certificação de aplicativos
ms.localizationpriority: medium
ms.openlocfilehash: 96087d2a41eb443374d8cd9bda5608d6156f9173
ms.sourcegitcommit: 49aab071aa2bd88f1c165438ee7e5c854b3e4f61
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/09/2018
ms.locfileid: "4461523"
---
# <a name="windows-desktop-bridge-app-tests"></a>Testes de aplicativo da Ponte de Desktop Windows

[Aplicativos de ponte de desktop](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-root) são convertidos em aplicativos Universal Windows Platform (UWP) usando a [Ponte da área de trabalho](https://developer.microsoft.com/en-us/windows/bridges/desktop)de aplicativos da área de trabalho do Windows. Após a conversão, o aplicativo da área de trabalho do Windows é empacotado, reparado e implantado na forma de um pacote de aplicativo UWP (.appx ou .appxbundle) na área de trabalho do Windows 10.

## <a name="required-versus-optional-tests"></a>Testes obrigatórios versus opcionais
Testes opcionais para aplicativos de ponte de Desktop do Windows são apenas informativas e não serão usados para avaliar seu aplicativo durante o carregamento da Microsoft Store. É recomendável investigando esses resultados para produzir apps da melhor qualidade de teste. Os critérios gerais de aprovação/reprovação para a a integração à loja são determinados pelos testes obrigatórios e não por esses testes opcionais.

## <a name="current-optional-tests"></a>Testes opcionais atuais

### <a name="1-digitally-signed-file-test"></a>1. Teste de arquivo assinado digitalmente 
**Contexto**  
Este teste verifica que todos os arquivos executáveis portáteis (PE) contêm uma assinatura válida. A presença de arquivos assinados digitalmente permite que os usuários saibam que o software é original.

**Detalhes do teste**  
O teste verifica todos os arquivos executáveis portáteis no pacote e verifica seus cabeçalhos em busca de uma assinatura. É recomendável assinar digitalmente todos os arquivos PE. Um aviso será gerado se qualquer um dos arquivos PE não estiver assinado.
 
**Ações corretivas**  
Sempre é recomendável ter arquivos assinados digitalmente. Para saber mais, veja [Introdução à assinatura de código](https://msdn.microsoft.com/en-us/library/ms537361(v=vs.85).aspx).

### <a name="2-file-association-verbs"></a>2. Verbos de associação de arquivo 
**Contexto**  
Esse teste examina o registro do pacote para verificar se qualquer verbo de associação de arquivo está registrado. 

**Detalhes do teste**  
Os apps da área de trabalho convertidos podem ser aprimorados com uma ampla gama de APIs da Plataforma Universal do Windows. Este teste verifica se os binários da UWP no app não chamam APIs que não sejam da UWP. Os binários UWP têm o sinalizador **AppContainer** definido.

**Ações corretivas**  
Veja [Ponte de Desktop para UWP: extensões de apps](https://docs.microsoft.com/en-us/windows/uwp/porting/desktop-to-uwp-extensions) para obter uma explicação sobre essas extensões e como usá-las corretamente. 

### <a name="3-debug-configuration-test"></a>3. Teste de configuração de depuração
Esse teste verifica se o appx não é um build de depuração.
 
**Contexto**  
Para serem certificados para a Microsoft Store, aplicativos não devem ser compilados para depuração e não devem referenciar versões de depuração de um arquivo executável. Além disso, você deve criar seu código como otimizado para que seu aplicativo passe nesse teste.
 
**Detalhes do teste**  
Teste o aplicativo para garantir que ele não é uma compilação de depuração e que não está vinculado a estruturas de depuração.
 
**Ações corretivas**  
* Crie o aplicativo como uma compilação de versão antes de enviá-lo na Microsoft Store.
* Verifique se a versão correta do .NET framework está instalada.
* Certifique-se de que o aplicativo não está vinculando versões de depuração de uma estrutura e se a versão é de liberação. Se o aplicativo contiver componentes .NET, verifique se você instalou a versão correta do .NET framework.

### <a name="4-package-sanity-test"></a>4. Testes de integridade do pacote
#### <a name="41-archive-files-usage"></a>4.1 Uso de arquivos mortos

**Contexto**  
Esse teste ajuda a criar os melhores aplicativos da Ponte de Desktop para execução nos computadores com [Windows 10 S](https://www.microsoft.com/windows/windows-10-s).

**Detalhes do teste**  
Este teste verifica todos os arquivos executáveis dentro dos arquivos mortos ou do conteúdo autoextraível. Como os arquivos executáveis contidos nesse tipo de conteúdo não são sinalizados durante a integração com a Windows Store, pode ser que o arquivo não seja executado como esperado nos sistemas com Windows 10 S.
 
**Ações corretivas**
* Considere a possibilidade de avaliar os arquivos sinalizados pelo teste para determinar se há impacto para seu aplicativo sendo executado em um ambiente com Windows 10 S.
* Se houver a possibilidade de que seu aplicativo seja afetado, remova os arquivos executáveis dos arquivos mortos e não use arquivos autoextraíveis para colocar arquivos executáveis no disco. Isso deve evitar a perda de funcionalidade do aplicativo.

#### <a name="42-blocked-executables"></a>4.2 Executáveis bloqueados

**Contexto**  
Esse teste ajuda a criar os melhores aplicativos da Ponte de Desktop para execução nos computadores com [Windows 10 S](https://www.microsoft.com/windows/windows-10-s). 

**Detalhes do teste**  
Este teste verifica se o aplicativo está tentando iniciar arquivos executáveis, o que está restrito nos sistemas do Windows 10 S. Aplicativos que dependem dessa funcionalidade podem não funcionar como esperado em sistemas com Windows 10 S. 

**Ações corretivas**  
* Identifique qual das entradas sinalizadas do teste representa uma chamada para iniciar um arquivo executável que não faz parte do seu aplicativo e remova essas chamadas. 
* Se os arquivos sinalizados fizerem parte do seu aplicativo, você pode ignorar o aviso.


## <a name="current-required-tests"></a>Testes necessários atuais

### <a name="1-app-capabilities-test-special-use-capabilities"></a>1. Teste de funcionalidades de aplicativo (funcionalidades de uso especial)

**Contexto**  
As funcionalidades de uso especial destinam-se a cenários bastante específicos. Somente contas empresariais podem usar esses recursos. 

**Detalhes do teste**  
Valide se o app está declarando qualquer uma das funcionalidades abaixo: 
* EnterpriseAuthentication
* SharedUserCertificates
* DocumentsLibrary

Se qualquer uma dessas capacidades for declarada, o teste exibirá um aviso para o usuário. 

**Ações corretivas**  
Considere a remoção da funcionalidade de uso especial caso ela não seja necessária ao seu app. Além disso, o uso dessas funcionalidades está sujeito à análise da política do serviço.

### <a name="2-app-manifest-resources-tests"></a>2, Testes de recursos do manifesto do app 
#### <a name="21-app-resources-validation"></a>2.1 Validação de recursos do app
O app pode não ser instalado adequadamente se as cadeias de caracteres ou imagens declaradas no manifesto do seu app estiverem incorretas. Se o app não for instalado com esses erros, o logotipo do app ou outras imagens poderão não ser exibidas corretamente.    

**Detalhes do teste**  
Inspeciona os recursos definidos no manifesto do app para garantir que estão presentes e são válidos.

**Ação corretiva**  
Use a tabela a seguir como guia.

Mensagem de erro | Comentários
--------------|---------
A imagem {image name} define os qualificadores Scale e TargetSize; você pode definir somente um qualificador por vez. | Você pode personalizar imagens para resoluções diferentes. Na mensagem real, {image name} contém o nome da imagem com o erro. Verifique se cada imagem define Scale ou TargetSize como o qualificador. 
Houve falha nas restrições de tamanho da imagem {image name}.  | Verifique se todas as imagens do aplicativo atendem às restrições adequadas de tamanho. Na mensagem real, {image name} contém o nome da imagem com o erro. 
A imagem {image name} está ausente no pacote.  | Uma imagem necessária está ausente. Na mensagem real, {image name} contém o nome da imagem que está ausente. 
A imagem {image name} não é um arquivo de imagem válido.  | Verifique se todas as imagens do aplicativo atendem às restrições adequadas de tipo de formato de arquivo. Na mensagem real, {image name} contém o nome da imagem que não é válida. 
A imagem "BadgeLogo" tem um valor ABGR {value} na posição (x, y) que não é válido. O pixel deve ser branco (##FFFFFF) ou transparente (00######)  | O logotipo de selo é uma imagem mostrada ao lado da notificação de selo para identificar o aplicativo na tela de bloqueio. Essa imagem deve ser monocromática (pode conter somente pixels brancos ou transparentes). Na mensagem real, {value} contém o valor da cor na imagem que não é válido. 
A imagem "BadgeLogo" tem um valor ABGR {value} na posição (x, y) que não é válido para a imagem branca de alto contraste. O pixel deve ser (##2A2A2A) ou mais escuro, ou transparente (00######).  | O logotipo de selo é uma imagem mostrada ao lado da notificação de selo para identificar o aplicativo na tela de bloqueio. Como o logotipo do selo é mostrado em uma tela de fundo branca quando está em branco de alto contraste, ele deve ser uma versão mais escura do logotipo padrão do selo. No banco de alto contraste, o logotipo do selo pode conter somente pixels que são mais escuros que (##2A2A2A) ou transparentes. Na mensagem real, {value} contém o valor da cor na imagem que não é válido. 
A imagem deve definir pelo menos uma variante, sem um qualificador TargetSize. Ela deve definir um qualificador Scale ou deixar Scale e TargetSize não especificados, que tem o padrão Scale-100.  | Para saber mais, veja os guias sobre [design responsivo](https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx) e [recursos do app](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data). 
O pacote tem um arquivo "resources.pri" ausente.  | Se você tiver conteúdo localizável no manifesto do aplicativo, verifique se o pacote do aplicativo inclui um arquivo resources.pri válido. 
O arquivo "resources.pri" deve conter um mapa de recursos com um nome que corresponda ao nome do pacote {package full name}  | Você pode obter esse erro quando o manifesto é alterado e o nome do mapa de recursos no resources.pri não corresponde mais ao nome do pacote no manifesto. Na mensagem real, {package full name} contém o nome do pacote que resources.pri deve conter. Para corrigir isso, você precisa recompilar o resources.pri e a maneira mais fácil de fazer isso é recompilando o pacote do app. 
O arquivo "resources.pri" não deve ter o AutoMerge habilitado.  | O MakePRI.exe oferece suporte a uma opção denominada AutoMerge. O valor padrão de AutoMerge é desativar. Quando está habilitado, o AutoMerge mescla os recursos de pacote de idiomas do app em um único resources.pri no tempo de execução. Não recomendamos isso para aplicativos que você pretende distribuir por meio da Microsoft Store. O Resources. PRI de um aplicativo que é distribuído pela Microsoft Store deve estar na raiz do pacote do aplicativo e conter todas as referências de idiomas compatíveis com o aplicativo. 
A cadeia de caracteres {string} falhou na restrição de comprimento máximo de {number} caracteres.  | Consulte os [Requisitos do pacote do aplicativo](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). Na mensagem real, {string} é substituído pela cadeia de caracteres com o erro e {number} contém o comprimento máximo. 
A cadeia de caracteres {string} não pode ser espaço inicial/final.  | O esquema dos elementos no manifesto do aplicativo não permite caracteres de espaço iniciais ou finais. Na mensagem real, {string} é substituído pela cadeia de caracteres com o erro. Verifique se algum dos valores localizados nos campos do manifesto no resources.pri contém caracteres de espaço iniciais ou finais. 
A cadeia de caracteres não pode estar vazia (o comprimento deve ser maior que zero)  | Para obter mais informações, consulte [Requisitos do pacote do aplicativo](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). 
Não há recurso padrão especificado no arquivo "resources.pri".  | Para saber mais, veja o guia sobre [recursos do app](https://docs.microsoft.com/en-us/windows/uwp/app-settings/store-and-retrieve-app-data). Na configuração de compilação padrão, o Visual Studio inclui apenas os recursos de imagem de escala 200 no pacote do app quando gera pacotes, colocando outros recursos no pacote de recursos. Inclua recursos de imagem de escala 200 ou configure seu projeto para incluir os recursos que você tem. 
Não há valor de recurso especificado no arquivo "resources.pri".  | Verifique se o manifesto do aplicativo tem recursos válidos definidos no resources.pri. 
O arquivo de imagem {filename} deve ter menos de 204.800 bytes.  | Reduza o tamanho das imagens indicadas. 
O arquivo {filename} não deve conter uma seção de mapa reverso.  | Apesar do mapa reverso ser gerado durante a "depuração F5" do Visual Studio durante a chamada no makepri.exe, ele pode ser removido. Basta executar makepri.exe sem o parâmetro /m ao gerar um arquivo pri. 



#### <a name="22-branding-validation"></a>2.2 Validação de identidade visual
**Contexto**  
Espera-se que os Apps de Ponte de Desktop estejam completos e totalmente funcionais. Os aplicativos que usam as imagens padrão (de modelos ou exemplos de SDK) apresentam uma experiência do usuário ruim e não podem ser identificados facilmente no catálogo da loja.

**Detalhes do teste**  
O teste valida se as imagens usadas pelo app não são imagens padrão de exemplos do SDK ou do Visual Studio. 

**Ações corretivas**  
Substitua as imagens padrão por algo mais distinto e que represente seu app.

### <a name="3-package-compliance-tests"></a>3. Testes de conformidade do pacote
#### <a name="31-app-manifest"></a>3.1 Manifesto do App
Testa o conteúdo do manifesto do app para garantir que seu conteúdo esteja correto.

**Histórico**  
Os aplicativos devem ter um manifesto corretamente formatado.

**Detalhes do teste**  
Analisa o manifesto do app para verificar se o conteúdo está correto, conforme descrito em [Requisitos do pacote do app](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements). As seguintes verificações estão concluídas neste teste:
* **Protocolos e extensões de arquivo**  
Seu app pode declarar os tipos de arquivo aos quais ele pode ser associado. Uma declaração de um grande número de tipos de arquivo incomuns prejudica a experiência de usuário. Este teste limita o número de extensões de arquivo aos quais um app pode se associar.
* **Regra de dependência de estrutura**  
Esse teste enfatiza o requisito de que os app declarem as devidas dependências na UWP. Se existir uma dependência inadequada, o teste falhará. Se houver incompatibilidade entre a versão do sistema operacional ao qual o aplicativo se destina e as dependências de estrutura que foram feitas, o teste falhará. O teste também falha quando o aplicativo se refere a versões de "visualização" das dlls de estrutura.
* **Verificação da comunicação entre processos (IPC)**  
Esse teste impõe o requisito de que os apps de Ponte de Desktop não se comunicam fora do contêiner do app para componentes de desktop. A comunicação entre processos é destinada apenas a apps de sideload. Os aplicativos que especificarem o [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) com nome igual a `DesktopApplicationPath` falharão nesse teste.  

**Ação corretiva**  
Analise o manifesto do app em relação aos requisitos descritos em [Requisitos do pacote do app](https://docs.microsoft.com/en-us/windows/uwp/publish/app-package-requirements).


#### <a name="32-application-count"></a>3.2 Contagem de aplicativos
Esse teste verifica se um pacote do app (.appx,, lote de aplicativo) contém um aplicativo. 

**Contexto**  
Esse teste é implementado de acordo com a política da Loja. 

**Detalhes do teste**  
Este teste verifica se o número total de pacotes .appx no lote é inferior a 512 e se há apenas um pacote "principal" no lote. Ele também verifica se o número de revisão da versão do lote está definido como 0. 

**Ações corretivas**  
Garantir que o pacote e o lote do app atendam aos requisitos declarados em **Detalhes do teste**.


#### <a name="33-registry-checks"></a>3.3 Verificações de registro
**Contexto**  
Este teste verifica se o aplicativo instala ou atualiza todos os novos serviços ou drivers.

**Detalhes do teste**  
O teste busca no arquivo registry.dat atualizações em locais específicos do registro que indiquem que um novo serviço ou driver está sendo registrado. Se o app estiver tentando instalar um driver ou serviço, o teste falhará.  

**Ações corretivas**  
Examine as falhas e remova os serviços ou drivers em questão se eles forem desnecessários. Se o app depender deles, será necessário revisá-lo se você quiser integrar-se à Loja.


### <a name="4-platform-appropriate-files-test"></a>4. Teste de arquivos apropriados para a plataforma
Os apps que instalam binários mistos podem falhar ou não funcionar corretamente dependendo da arquitetura do processador do usuário. 

**Contexto**  
Este teste examina os binários em um pacote de aplicativo em busca de conflitos de arquitetura. Um pacote de aplicativo não deve incluir os binários que não podem ser utilizados na arquitetura do processador especificado no manifesto. Incluir binários sem suporte pode levar o app à falhas ou um aumento desnecessário no tamanho do pacote do app. 

**Detalhes do teste**  
Valida se o "número de bit" de cada arquivo no cabeçalho executável portável é apropriado em caso de referência cruzada com a declaração de arquitetura do processador do pacote do aplicativo 

**Ações corretivas**  
Siga estas diretrizes para garantir que seu pacote de apps contenha apenas arquivos com suporte da arquitetura especificada no manifesto do app: 
* Se a Arquitetura do Processador Alvo para o app for do Tipo de Processador Neutro, o pacote de aplicativo não poderá conter um binário x86, x64 ou ARM ou arquivos do tipo imagem.
* Se a Arquitetura do processador alvo para o aplicativo for tipo de processador x86, o pacote de aplicativo deve conter apenas binário x86 ou arquivos do tipo imagem. Se o pacote contiver binário x64 ou ARM ou tipos de imagem, ele irá falhar no teste.
* Se a Arquitetura do processador alvo para o aplicativo for tipo de processador x64, o pacote de aplicativo deve conter binário x64 ou arquivos do tipo imagem. Observe que, neste caso, o pacote também poderá incluir arquivos x86, mas a experiência do app primário deve utilizar o binário x64. Se o pacote contiver binário ARM ou arquivos do tipo imagem, ou se contiver *apenas* binários x86 ou arquivos de tipo de imagem, ele irá falhar no teste.
* Se a Arquitetura do processador alvo para o aplicativo for tipo de processador ARM, o pacote de aplicativo deve conter apenas binário ARM ou arquivos do tipo imagem. Se o pacote contiver binário x64 ou x86 ou arquivos de tipos de imagem, ele falhará no teste. 

### <a name="5-supported-api-test"></a>5. Teste de API com suporte
Verifica o app em relação ao uso de quaisquer APIs não compatíveis. 

**Contexto**  
Os apps de Ponte de Desktop podem aproveitar algumas APIs Win32 herdadas juntamente com APIs modernas (componentes da UWP). Esse teste identifica binários gerenciados que usam APIs sem suporte.
 
**Detalhes do teste**  
Esse teste verifica todos os componentes da UWP no app:
* Verifica se cada binário gerenciado no pacote do aplicativo não têm uma dependência em uma API do Win32 que não há suporte para desenvolvimento de aplicativos UWP, verificando a tabela de endereço de importação do binário.
* Verifica se cada binário gerenciado no pacote do app não depende de uma função fora do perfil aprovado. 

**Ações corretivas**  
Isso pode ser corrigido, garantindo que o app foi compilado como um build de versão e não como um build de depuração. 

> [!NOTE]
> A compilação de depuração de um aplicativo falhará nesse teste mesmo se o aplicativo usa [APIs para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx). Consulte as mensagens de erro para identificar a API presente que não é uma API permitida para aplicativos UWP. 

> [!NOTE]
> Aplicativos C++ integrados em uma configuração de depuração falhará neste teste mesmo se a configuração usar somente APIs do SDK do Windows para aplicativos UWP. Consulte [alternativas às APIs do Windows em aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/hh464945.aspx) para obter mais informações.

### <a name="6-user-account-control-uac-test"></a>6. Teste de UAC (controle de conta de usuário).  

**Contexto**  
Garante que o app não esteja solicitando o controle de conta de usuário em tempo de execução.

**Detalhes do teste**  
Um aplicativo não pode solicitar a elevação de administrador ou UIAccess por política da Microsoft Store. As permissões de segurança elevadas não têm suporte. 

**Ações corretivas**  
Os apps devem ser executados como um usuário interativo. Veja [Visão geral sobre a Automação da Interface do Usuário](https://go.microsoft.com/fwlink/?linkid=839440) para obter detalhes.

 
### <a name="7-windows-runtime-metadata-validation"></a>7. Validação dos metadados do Windows Runtime
**Contexto**  
Verifica se os componentes que vêm com o app são compatíveis com o sistema do tipo UWP.

**Detalhes do teste**  
Esse teste gera alguns sinalizadores relacionados ao uso apropriado de tipos.

**Ações corretivas**  
* **Atributo ExclusiveTo**  
Garantir que as classes UWP não implementem interfaces marcadas como outra classe ExclusiveTo
* **Exatidão dos Metadados Gerais**  
Garanta que o compilador que você está usando para gerar seus tipos esteja atualizado de acordo com as especificações da UWP.
* **Propriedades**  
Garanta que todas as propriedades em uma classe UWP tenham um método `get` (os métodos `set` são opcionais). Para todas as propriedades, verifique se o tipo retornado pelo método `get`corresponde ao tipo do parâmetro de entrada do método `set`.
* **Local do tipo**  
Verifique se os metadados de todos os tipos UWP estão localizados no arquivo .winmd que tem o nome correspondente ao namespace mais longo no pacote do app.
* **Diferenciação de maiúsculas e minúsculas de nomes de tipo**  
Verifique se todos os tipos UWP têm nomes exclusivos que não diferenciam maiúsculas de minúsculas no pacote do app. Verifique também se nenhum nome de tipo UWP é usado como nome de namespace no pacote do app.
* **Exatidão do nome de tipo**  
Verifique se não há tipos UWP no namespace global nem no namespace de nível superior do Windows.
 

### <a name="8-windows-security-features-tests"></a>8. Testes dos recursos de segurança do Windows
Alterar as proteções de segurança padrão do Windows pode colocar os clientes em risco elevado. 

#### <a name="81-banned-file-analyzer"></a>8.1 Analisador de arquivos banidos
**Contexto**  
Determinados arquivos foram atualizados com melhorias importantes de segurança, confiabilidade ou outras. Os apps de Ponte de Desktop do Windows devem conter as versões mais recentes desses arquivos, já que as versões desatualizadas representam um risco. O Kit de Certificação de Aplicativos Windows bloqueia esses arquivos para garantir que todos os apps usem a versão atual.

**Detalhes do teste**  
A verificação de arquivo banido no Kit de Certificação de Aplicativos Windows atualmente verifica os seguintes arquivos:
* *Bing.Maps.JavaScript\js\veapicore.js*  
Essa verificação geralmente falha quando um app está usando uma versão "Release Preview" do arquivo em vez da versão mais recente oficial. 

**Ações corretivas**  
Para corrigir esse problema, use a versão mais recente do [SDK do Bing mapas](http://go.microsoft.com/fwlink/p/?linkid=614880) para aplicativos UWP.

#### <a name="82-private-code-signing"></a>8.2 Assinatura de códigos privados
Testes para a existência de binários de assinatura de código privado no pacote de app. 

**Histórico**  
Os arquivos de assinatura de código privado devem ser mantidos privados, já que eles podem ser utilizados para fins maliciosos no caso de serem comprometidos. 

**Detalhes do teste**  
Verifica se há arquivos no pacote de aplicativos com uma extensão .pfx ou .snk, que indica que as chaves de assinatura privadas foram incluídas. 

**Ações corretivas**  
Remova todas as chaves de assinatura de código privado (como arquivos .snk e .pfx) do pacote.


## <a name="related-topics"></a>Tópicos relacionados

* [Políticas da Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Dn764944)
