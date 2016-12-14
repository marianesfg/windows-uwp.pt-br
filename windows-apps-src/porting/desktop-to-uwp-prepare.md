---
author: awkoren
Description: "Este artigo lista as coisas que você precisa saber antes de converter o aplicativo usando a ponte da área de trabalho para UWP. Talvez você não precise fazer muito para preparar seu aplicativo para o processo de conversão."
Search.Product: eADQiWindows 10XVcnh
title: "Preparar o aplicativo para a ponte da área de trabalho para UWP"
translationtype: Human Translation
ms.sourcegitcommit: f7a8b8d586983f42fe108cd8935ef084eb108e35
ms.openlocfilehash: 81a2485d5be22dd392c21aaff281c1c9263883a9

---

# <a name="prepare-an-app-for-conversion-with-the-desktop-bridge"></a>Preparar um app para conversão usando a ponte da área de trabalho

Este artigo lista as coisas que você precisa saber antes de converter o aplicativo usando a ponte da área de trabalho para UWP. Talvez você não precise fazer muito para preparar o aplicativo para o processo de conversão, mas se algum dos itens abaixo se aplicar ao aplicativo, você precisará resolver isso antes da conversão. Lembre-se de que a Windows Store lida com o licenciamento e a atualização automática para você, portanto, você pode remover esses recursos de sua base de código.

+ __Seu aplicativo usa uma versão do .NET anterior à 4.6.1__. Somente o .NET 4.6.1 tem suporte. Você deve redirecionar seu aplicativo para o .NET 4.6.1 antes da conversão. 

+ __Seu aplicativo sempre é executado com privilégios de segurança elevados__. Seu aplicativo precisa funcionar durante a execução como o usuário interativo. Os usuários que instalam seu aplicativo da Windows Store podem não ser administradores de sistema, portanto, exigir que seu aplicativo seja executado com privilégios elevados significa que ele não será executado corretamente para usuários padrão.

+ __Seu aplicativo requer um driver de modo kernel ou um serviço Windows__. A ponte é adequada para um aplicativo, mas não dá suporte a um driver de modo kernel ou um serviço Windows que precisa ser executado em uma conta do sistema. Em vez de um serviço Windows, use uma [tarefa em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Os módulos do seu aplicativo são carregados no processo de processos que não estão em seu pacote de AppX__. Isso não é permitido, o que significa que extensões de processo, como [extensões do shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), não têm suporte. Mas se você tiver dois aplicativos no mesmo pacote, será possível fazer comunicação entre processos entre eles.

+ __Seu aplicativo chama [SetDllDirectory](https://msdn.microsoft.com/library/windows/desktop/ms686203) ou [AddDllDirectory](https://msdn.microsoft.com/library/windows/desktop/hh310513)__. Essas funções não são suportadas para aplicativos convertidos atualmente. Estamos trabalhando em para dar suporte em uma versão futura. Como alternativa, você pode copiar quaisquer .dlls que foram localizadas usando essas funções para a raiz do pacote. 

+ __Seu aplicativo usa uma AUMID (ID do Modelo do Usuário do Aplicativo)__. Se o seu processo chama [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para definir sua própria AUMID, então ele poderá usar apenas a AUMID gerada para ele pelo ambiente de modelo de aplicativo/pacote AppX. Não é possível definir AUMIDs personalizadas.

+ __Seu aplicativo modifica o hive do Registro HKEY_LOCAL_MACHINE (HKLM)__. Qualquer tentativa de seu aplicativo de criar uma chave HKLM, ou abrir uma para modificação, resultará em uma falha de acesso negado. Lembre-se de que seu aplicativo tem sua própria exibição virtualizada particular do registro, portanto, a noção de um hive do registro no âmbito do usuário e do computador (o que o HKLM é) não se aplica. Você precisará encontrar outra maneira de fazer o que o HKLM faz, como, por exemplo, gravar em HKEY_CURRENT_USER (HKCU).

+ __Seu aplicativo usa uma subchave de registro ddeexec como um meio de iniciar outro aplicativo__. Em vez disso, use um dos manipuladores de verbo DelegateExecute conforme configurado pelas várias extensões ativáveis* no [manifesto de pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Seu aplicativo grava na pasta AppData com a intenção de compartilhar dados com outro aplicativo__. Após a conversão, a pasta AppData é redirecionada para o armazenamento de dados de aplicativo local, que é um repositório particular para cada aplicativo UWP. Use uma maneira diferente de compartilhamento de dados entre processos. Para obter mais informações, consulte [Armazene e recupere configurações e outros dados de aplicativo](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Seu aplicativo grava no diretório de instalação do seu aplicativo__. Por exemplo, seu aplicativo grava em um arquivo de log que você coloca no mesmo diretório que o exe. Isso não é aceito, portanto, você precisará encontrar outro local, como o armazenamento de dados de aplicativo local.

+ __A instalação do seu aplicativo requer interação do usuário__. O instalador do seu aplicativo deve ser capaz de ser executado silenciosamente, e ele deve instalar todos os pré-requisitos que não estão por padrão em uma imagem limpa do sistema operacional.

+ __Seu aplicativo usa a pasta de trabalho atual__. No tempo de execução, seu aplicativo convertido não terá o mesmo diretório trabalho especificado anteriormente no atalho .LNK em sua área de trabalho. Você precisa alterar seu CWD no tempo de execução se ter o diretório correto é importante para o seu aplicativo funcionar corretamente.

+ __Seu aplicativo requer UIAccess__. Se o seu aplicativo especifica `UIAccess=true` no elemento `requestedExecutionLevel` do manifesto UAC, a conversão em UWP não será possível no momento. Para obter mais informações, consulte [Visão geral sobre a Automação da Interface do Usuário](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Seu aplicativo expõe objetos COM ou assemblies GAC para uso por outros processos__. Na versão atual, seu aplicativo não pode expor objetos COM nem assemblies GAC para uso por processos originários de executáveis externos ao seu pacote AppX. Os processos de dentro do pacote podem registrar e usar objetos COM e assemblies GAC normalmente, mas eles não estarão visíveis externamente. Isso significa que cenários de interoperabilidade como OLE não funcionarão se forem invocados por processos externos. 

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

+ __Seu app usa uma dependência na pasta System32/SysWOW64__. Para fazer essas DLLs funcionarem, você deve incluí-las na parte do sistema de arquivos virtual do pacote AppX. Isso garante que o app se comporte como se as DLLs tivessem sido instaladas na pasta **System32**/**SysWOW64**. Na raiz do pacote, crie uma pasta chamada **VFS**. Dentro dessa pasta, crie uma pasta **SystemX64** e uma **SystemX86**. Em seguida, coloque a versão de 32 bits de sua DLL na pasta **SystemX86** e a versão de 64 bits na pasta **SystemX64**.

+ __Seu app usa o pacote de estrutura VCLibs do Dev11__. As bibliotecas VCLibs 11 podem ser instaladas diretamente da Windows Store, caso estejam definidas como uma dependência no pacote AppX. Para fazer isso, faça a seguinte alteração no manifesto do pacote do aplicativo: sob o nó `<Dependencies>`, adicione:  
`<PackageDependency Name="Microsoft.VCLibs.110.00.UWPDesktop" MinVersion="11.0.24217.0" Publisher="CN=Microsoft Corporation, O=Microsoft Corporation, L=Redmond, S=Washington, C=US" />`  
Durante a instalação a partir da Windows Store, a versão apropriada (x86 ou x64) da estrutura VCLibs 11 será instalada antes da instalação do app.  
As dependências não serão instaladas se o app for instalado por sideload. Para instalar as dependências manualmente no seu computador, você deve baixar e instalar os [pacotes de estrutura do VC 11.0 para Desktop Bridge](https://www.microsoft.com/download/details.aspx?id=53340&WT.mc_id=DX_MVP4025064). Para obter mais informações sobre esses cenários, consulte [Usando o Visual C++ Runtime no projeto Centennial](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).


<!--HONumber=Dec16_HO1-->


