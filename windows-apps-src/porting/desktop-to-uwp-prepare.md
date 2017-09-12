---
author: normesta
Description: "Este artigo lista as coisas que você precisa saber antes de empacotar seu app com a Ponte de Desktop. Talvez você não precise fazer muito para preparar seu app para o processo de empacotamento."
Search.Product: eADQiWindows 10XVcnh
title: Preparar para empacotar um aplicativo (Ponte de Desktop)
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71a57ca2-ca00-471d-8ad9-52f285f3022e
ms.openlocfilehash: f7337ee7bf78730e2300a11d7d606f328c7c418b
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="prepare-to-package-an-app-desktop-bridge"></a>Preparar para empacotar um aplicativo (Ponte de Desktop)

Este artigo lista as coisas que você precisa saber antes de empacotar seu aplicativo da área de trabalho. Talvez você não precise fazer muito para preparar o aplicativo para o processo de empacotamento, mas se qualquer um dos itens abaixo se aplicar ao seu aplicativo, você precisará resolvê-lo antes do empacotamento. Lembre-se de que a Windows Store lida com o licenciamento e a atualização automática para você, portanto, você pode remover qualquer recurso relacionado a essas tarefas de seu código base.

+ __Seu aplicativo usa uma versão do .NET anterior à 4.6.1__. Somente o .NET 4.6.1 tem suporte. Você terá que redirecionar seu aplicativo para o .NET 4.6.1 antes de empacotá-lo.

+ __Seu aplicativo sempre é executado com privilégios de segurança elevados__. Seu aplicativo precisa funcionar durante a execução como o usuário interativo. Os usuários que instalam seu aplicativo da Windows Store podem não ser administradores de sistema, portanto, exigir que seu aplicativo seja executado com privilégios elevados significa que ele não será executado corretamente para usuários padrão.

+ __Seu aplicativo requer um driver de modo kernel ou um serviço Windows__. A ponte é adequada para um aplicativo, mas não dá suporte a um driver de modo kernel ou um serviço Windows que precisa ser executado em uma conta do sistema. Em vez de um serviço Windows, use uma [tarefa em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Os módulos do seu aplicativo são carregados em processo, para processos que não estão no seu pacote do aplicativo do Windows__. Isso não é permitido, o que significa que extensões de processo, como [extensões do shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), não têm suporte. Mas se você tiver dois aplicativos no mesmo pacote, será possível fazer comunicação entre processos entre eles.

+ __Seu aplicativo usa uma ID do Modelo do Usuário do Aplicativo (AUMID)__. Se o seu processo chama [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para definir sua própria AUMID, então ele poderá usar apenas a AUMID gerada para ele pelo ambiente de modelo de aplicativo/pacote de aplicativo do Windows. Não é possível definir AUMIDs personalizadas.

+ __Seu aplicativo modifica o hive do Registro HKEY_LOCAL_MACHINE (HKLM)__. Qualquer tentativa de seu aplicativo de criar uma chave HKLM, ou abrir uma para modificação, resultará em uma falha de acesso negado. Lembre-se de que seu aplicativo tem sua própria exibição virtualizada particular do registro, portanto, a noção de um hive do registro no âmbito do usuário e do computador (o que o HKLM é) não se aplica. Você precisará encontrar outra maneira de fazer o que o HKLM faz, como, por exemplo, gravar em HKEY_CURRENT_USER (HKCU).

+ __Seu aplicativo usa uma subchave de registro ddeexec como um meio de iniciar outro aplicativo__. Em vez disso, use um dos manipuladores de verbo DelegateExecute conforme configurado pelas várias extensões ativáveis* no [manifesto de pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Seu app grava na pasta AppData ou no Registro com a intenção de compartilhar dados com outro app__. Após a conversão, a pasta AppData é redirecionada para o armazenamento de dados de aplicativo local, que é um repositório particular para cada aplicativo UWP.

  Todas as entradas que seu app grava no hive do Registro HKEY_LOCAL_MACHINE são redirecionadas para um arquivo binário isolado e quaisquer entradas que seu app grave no hive do registro HKEY_CURRENT_USER são colocadas em um local particular por usuário, por aplicativo. Para obter mais detalhes sobre o redirecionamento de arquivos e do Registro, consulte [Bastidores da Ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).  

  Use uma maneira diferente de compartilhamento de dados entre processos. Para obter mais informações, consulte [Armazene e recupere configurações e outros dados de aplicativo](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Seu aplicativo grava no diretório de instalação do seu aplicativo__. Por exemplo, seu aplicativo grava em um arquivo de log que você coloca no mesmo diretório que o exe. Isso não é aceito, portanto, você precisará encontrar outro local, como o armazenamento de dados de aplicativo local.

+ __A instalação do seu aplicativo requer interação do usuário__. O instalador do seu aplicativo deve ser capaz de ser executado silenciosamente, e ele deve instalar todos os pré-requisitos que não estão por padrão em uma imagem limpa do sistema operacional.

+ __Seu aplicativo usa a pasta de trabalho atual__. No tempo de execução, seu aplicativo da área de trabalho empacotado não terá o mesmo diretório trabalho especificado anteriormente no atalho .LNK em sua área de trabalho. Você precisa alterar seu CWD no tempo de execução se ter o diretório correto é importante para o seu aplicativo funcionar corretamente.

+ __Seu aplicativo requer UIAccess__. Se o seu aplicativo especifica `UIAccess=true` no elemento `requestedExecutionLevel` do manifesto UAC, a conversão em UWP não será possível no momento. Para obter mais informações, consulte [Visão geral sobre a Segurança da Automação da Interface de Usuário](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Seu aplicativo expõe objetos COM__. Processos e extensões de dentro do pacote podem registrar e usar servidores COM & OLE, tanto dentro do processo como fora do processo (OOP).  A Atualização dos Criadores adiciona suporte para COM integrados, o que fornece a habilidade de registrar servidores OOP COM & OLE que estão agora visíveis fora do pacote.  Consulte [Suporte ao Servido COM e ao Documento OLE para Ponte de Desktop](https://blogs.windows.com/buildingapps/2017/04/13/com-server-ole-document-support-desktop-bridge/#bjPyETFgtpZBGrS1.97).

   O suporte ao COM Integrado funciona para APIs COM existentes, mas não funcionará para extensões de aplicativo que dependem da leitura direta do registro, uma vez que a localização do COM Integrado é privada.

+ __Seu aplicativo expõe conjuntos GAC para uso em outros processos__. Na versão atual, seu aplicativo não pode expor conjuntos GAC para o uso de processos originários de executáveis externos ao seu pacote do aplicativo do Windows. Os processos de dentro do pacote podem registrar e usar conjuntos GAC normalmente, mas eles não estarão visíveis externamente. Isso significa que cenários de interoperabilidade como OLE não funcionarão se forem invocados por processos externos.

+ __O aplicativo está vinculando bibliotecas de tempo de execução C (CRT) de maneira incompatível__. A biblioteca de tempo de execução C/C++ da Microsoft oferece rotinas de programação para o sistema operacional Microsoft Windows. Essas rotinas automatizam muitas tarefas comuns de programação que não são fornecidas pelas linguagens C e C++. Se seu aplicativo utiliza a biblioteca de tempo de execução C/C++, você precisa garantir que ela seja vinculada de maneira compatível.

    O Visual Studio 2015 dá suporte à vinculação dinâmica, para permitir que seu código use arquivos DLL comuns, ou vinculação estática, para vincular a biblioteca diretamente no seu código à versão atual da CRT. Se possível, recomendamos que o seu aplicativo use vinculação dinâmica com VS 2015.

    O suporte nas versões anteriores do Visual Studio varia. Para obter detalhes, consulte a tabela a seguir:

    <table>
    <th>Versão do Visual Studio</td><th>Vinculação dinâmica</th><th>Vinculação estática</th></th>
    <tr><td>2005 (VC 8)</td><td>Sem suporte</td><td>Com suporte</td>
    <tr><td>2008 (VC 9)</td><td>Sem suporte</td><td>Com suporte</td>
    <tr><td>2010 (VC 10)</td><td>Com suporte</td><td>Com suporte</td>
    <tr><td>2012 (VC 11)</td><td>Com suporte</td><td>Sem suporte</td>
    <tr><td>2013 (VC 12)</td><td>Com suporte</td><td>Sem suporte</td>
    <tr><td>2015 (VC 14)</td><td>Com suporte</td><td>Com suporte</td>
    </table>

    Observação: em todos os casos, você deve se vincular à última CRT publicamente disponível.

+ __Seu aplicativo instala e carrega os assemblies da pasta lado a lado do Windows__. Por exemplo, seu aplicativo usa bibliotecas de tempo de execução C VC8 ou VC9 e está vinculando-as dinamicamente da pasta lado a lado do Windows, ou seja, seu código está usando os arquivos DLL comuns de uma pasta compartilhada. Isso não tem suporte. Você precisará vinculá-las de forma estática vinculando-as aos arquivos da biblioteca redistribuível diretamente no código.

+ __Seu app usa uma dependência na pasta System32/SysWOW64__. Para fazer essas DLLs funcionarem, você deve incluí-las na parte do sistema de arquivos virtual do pacote de aplicativo do Windows. Isso garante que o app se comporte como se as DLLs tivessem sido instaladas na pasta **System32**/**SysWOW64**. Na raiz do pacote, crie uma pasta chamada **VFS**. Dentro dessa pasta, crie uma pasta **SystemX64** e uma **SystemX86**. Em seguida, coloque a versão de 32 bits de sua DLL na pasta **SystemX86** e a versão de 64 bits na pasta **SystemX64**.

+ __Seu app usa o pacote de estrutura VCLibs do Dev11__. As bibliotecas VCLibs 11 podem ser instaladas diretamente da Windows Store, caso estejam definidas como uma dependência no pacote de aplicativo do Windows. Para fazer isso, faça a seguinte alteração no manifesto do pacote do app: sob o nó `<Dependencies>`, adicione:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Durante a instalação a partir da Windows Store, a versão apropriada (x86 ou x64) da estrutura VCLibs 11 será instalada antes da instalação do app.  
As dependências não serão instaladas se o app for instalado por sideload. Para instalar as dependências manualmente no seu computador, você deve baixar e instalar os [pacotes de estrutura do VC 11.0 para Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). Para obter mais informações sobre esses cenários, consulte [Usando o Visual C++ Runtime no projeto Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

+ __Seu aplicativo contém uma lista de atalhos personalizada__. Há vários problemas e limitações a que é preciso estar atento ao usar listas de atalhos.

    - __A arquitetura do seu aplicativo não corresponde ao sistema operacional.__  As listas de atalhos atualmente não funcionam corretamente se as arquiteturas do aplicativo e do sistema operacional não corresponderem (por exemplo, um aplicativo x86 em execução em no Windows x64). Neste momento, não há uma solução alternativa que não seja recompilar o aplicativo para a arquitetura correspondente.

    - __Seu aplicativo cria entradas da lista de atalhos e chama [ICustomDestinationList::SetAppID](https://msdn.microsoft.com/library/windows/desktop/dd378403(v=vs.85).aspx) ou [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422(v=vs.85).aspx)__. Não defina AppID de forma programática no código. Isso fará com que as entradas da lista de atalhos não sejam exibidas. Se seu app precisa de uma ID personalizada, especifique-a usando o arquivo de manifesto. Consulte [Empacotar manualmente um app (Ponte de Desktop)](desktop-to-uwp-manual-conversion.md) para obter instruções. A AppID de seu aplicativo é especificada na seção *YOUR_PRAID_HERE*.

    - __Seu app adiciona um link de shell de lista de atalhos que faz referência a um executável em seu pacote__. Você não pode iniciar executáveis diretamente em seu pacote a partir de uma lista de atalhos (com a exceção do caminho absoluto do .exe do próprio aplicativo). Registre um alias de execução do app (que permita que seu aplicativo da área de trabalho empacotado seja iniciado por meio de uma palavra-chave como se estivesse no PATH) e defina o caminho de destino do link como o alias. Para obter detalhes sobre como usar a extensão appExecutionAlias, consulte [Integrar seu aplicativo com o Windows 10 (Ponte de Desktop)](desktop-to-uwp-extensions.md). Observe que, se você precisar que os ativos do link na lista de atalhos correspondam ao .exe original, defina os ativos, como o ícone, usando [**SetIconLocation**](https://msdn.microsoft.com/library/windows/desktop/bb761047(v=vs.85).aspx) e o nome de exibição com PKEY_Title como você faria para outras entradas personalizadas.

    - __Seu app adiciona uma lista de atalhos com entradas que referenciam ativos no pacote do app por caminhos absolutos__. O caminho de instalação de um app pode mudar quando seus pacotes são atualizados, alterando o local dos ativos (como ícones, documentos, executáveis e assim por diante). Se as entradas da lista de atalhos referenciam esses ativos por caminhos absolutos, o app deve atualizar sua lista de atalhos periodicamente (por exemplo, na inicialização do app) para garantir que os caminhos se resolvam corretamente. Como alternativa, use as APIs UWP [**Windows.UI.StartScreen.JumpList**](https://msdn.microsoft.com/library/windows/apps/windows.ui.startscreen.jumplist.aspx), que permitem referenciar ativos de cadeias de caracteres e imagens usando o esquema de URI package-relative ms-resource (que também reconhece linguagem, DPI e alto contraste).

+ __Seu aplicativo inicia um utilitário para executar tarefas__. Evite iniciar utilitários de comando, como o PowerShell e o Cmd.exe. Na verdade, se os usuários instalarem seu aplicativo em um sistema que executa o Windows 10 S, o seu aplicativo não será capaz de iniciá-los de maneira alguma. Isso pode bloquear o envio do seu app para a Windows Store porque todos os apps enviados para a Windows Store devem ser compatíveis com o Windows 10 S.

Iniciar um utilitário pode geralmente fornecer uma maneira conveniente para obter informações do sistema operacional, acessar o registro ou acessar as funcionalidades do sistema. No entanto, você pode usar APIs UWP para realizar esses tipos de tarefas. Esses APIs são mais eficientes, porque eles não precisam de um executável separado para ser executado, mas o mais importante é que eles impedem que o aplicativo saia do pacote. O design do aplicativo permanece consistente com o isolamento, a confiança e a segurança que acompanham o aplicativo de ponte de desktop, e seu aplicativo irá se comportar como esperado em sistemas que executam o Windows 10 S.

+ __Seu aplicativo hospeda suplementos, plug-ins ou extensões__.   Em muitos casos, as extensões do estilo COM provavelmente continuarão a funcionar desde que a extensão não tenha sido empacotada e seja instalada com confiança total. Isso ocorre porque esses instaladores podem usar suas funcionalidades de confiança total para modificar o registro e colocar arquivos de extensão em qualquer lugar que o seu aplicativo espere encontrá-los.

   No entanto, se essas extensões forem empacotadas e então instaladas como um pacote do aplicativo do Windows, elas não funcionarão porque cada pacote (o app host e a extensão) estarão isolados um do outro. Para saber mais sobre como a ponte de desktop isola os aplicativos do sistema, consulte [Nos bastidores da Ponte de Desktop](desktop-to-uwp-behind-the-scenes.md).

 Todos os aplicativos e extensões que os usuários instalam em um sistema que executa o Windows 10 S devem ser instalados como pacotes do aplicativo do Windows. Portanto, se você pretende empacotar suas extensões ou planeja incentivar seus colaboradores a empacotá-las, considere como você pode facilitar a comunicação entre o pacote do aplicativo e qualquer outro pacote de extensão. Uma maneira que você pode ser capaz de fazer isso é usando um [serviço de aplicativo](../launch-resume/app-services.md).

+ __Seu aplicativo gera código__. Seu aplicativo pode gerar o código que ele consome na memória, mas gravar código gerado em disco pois o processo de Certificação de Aplicativos Windows não consegue validar o código antes do envio do aplicativo. Além disso, os apps que gravam código em disco não serão executados adequadamente em sistemas com o Windows 10 S. Isso pode bloquear o envio do seu app para a Windows Store porque todos os apps enviados para a Windows Store devem ser compatíveis com o Windows 10 S.

+ __Seu aplicativo usa o API MAPI__. No momento, a [API MAPI do Outlook](https://msdn.microsoft.com/library/office/cc765775.aspx(d=robot)) não tem suporte em apps de ponte de desktop.

## <a name="next-steps"></a>Próximas etapas

**Criar um pacote de aplicativo do Windows para o seu aplicativo da área de trabalho**

Consulte [Criar um pacote de aplicativo do Windows](desktop-to-uwp-root.md#convert)

**Encontre respostas para dúvidas específicas**

Nossa equipe monitora estas [marcas do StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.
