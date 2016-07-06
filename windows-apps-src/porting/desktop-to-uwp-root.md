---
author: awkoren
Description: "Prepare seu aplicativo da área de trabalho do Windows (Win32, WPF e Windows Forms) para a conversão em um aplicativo UWP (Plataforma Universal do Windows) usando as extensões de conversão de área de trabalho."
Search.Product: eADQiWindows 10XVcnh
title: "Converter o seu aplicativo da área de trabalho em um aplicativo UWP (Plataforma Universal do Windows)"
ms.sourcegitcommit: 37d4c8ef6178fe78c7333c47848177b5185c6d61
ms.openlocfilehash: 16d75ebd2499e56af742c1c9994fd92cc320c22b

---

# Converter o seu aplicativo da área de trabalho em um aplicativo UWP (Plataforma Universal do Windows)

\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

Prepare seu aplicativo da área de trabalho do Windows (Win32, WPF e Windows Forms) para a conversão em um aplicativo UWP (Plataforma Universal do Windows) usando as extensões de conversão de área de trabalho.

## Benefícios da conversão de seu aplicativo em UWP

UWP usando extensões de conversão de área de trabalho é uma ponte que permite que você converta seu aplicativo de área de trabalho clássico (Win32, Windows Forms e WPF) ou jogo em um aplicativo ou jogo UWP (Plataforma Universal do Windows. Para obter mais informações, consulte [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631.aspx). Após a conversão, seu aplicativo de área de trabalho clássico é empacotado, reparado e implantado na forma de um pacote de aplicativo UWP (.appx ou .appxbundle) na área de trabalho do Windows 10.

Há duas partes na tecnologia que permite que os aplicativos da área de trabalho sejam convertidos em pacotes UWP. A primeira é o conversor de aplicativos da área de trabalho, que usa seus binários existentes e os empacota novamente como um pacote UWP. O código ainda é o mesmo, apenas é empacotado de forma diferente. A segunda parte compreende tecnologias de tempo de execução na atualização de aniversário do Windows que permitem que um pacote UWP tenha arquivos executáveis que são executados com confiança total, em vez de em um contêiner de aplicativo. Essa tecnologia também dá um identificador de pacote a um aplicativo convertido, que é necessário para usar algumas APIs de UWP.

Aqui estão alguns dos benefícios da conversão de seu aplicativo de área de trabalho clássico.

* A experiência de instalação do seu aplicativo é muito mais tranquila para seus clientes. Você pode implantá-lo em computadores que usam sideload (consulte [Sideload de aplicativos LOB no Windows 10](https://technet.microsoft.com/library/mt269549.aspx)), e ele não deixa rastros depois de ser desinstalado. A longo prazo, você também poderá publicar seu aplicativo na Windows Store.

* Como seu aplicativo convertido tem um identificador de pacote, você pode chamar mais APIs de UWP, até mesmo da partição de confiança total, do que poderia antes.

* Em seu próprio ritmo, você pode adicionar recursos UWP ao pacote do seu aplicativo, como uma interface de usuário XAML, atualizações de bloco dinâmico, tarefas de UWP em segundo plano, serviços de aplicativo, e muito mais. Todas as funcionalidades disponíveis para qualquer outro aplicativo UWP estão disponíveis para seu aplicativo.

* Se você optar por mover toda a funcionalidade do seu aplicativo para fora da partição de confiança total do aplicativo e para dentro da partição do contêiner do aplicativo, seu aplicativo poderá ser executado em qualquer dispositivo Windows 10.

* Como um aplicativo UWP, seu aplicativo é capaz de fazer o que ele poderia como um aplicativo de área de trabalho clássico. Ele interage com um modo de exibição virtualizado do registro e do sistema de arquivos que é idêntico ao registro e ao sistema de arquivos reais.

* Seu aplicativo pode participar dos recursos internos de atualização automática e de licenciamento da Windows Store. A atualização automática é um mecanismo altamente confiável e eficiente, pois somente as partes alteradas dos arquivos são baixadas.

## Preparando seu aplicativo da área de trabalho para a conversão em UWP
Talvez você não precise fazer muito para preparar seu aplicativo para o processo de conversão. Lembre-se de que a Windows Store lida com o licenciamento e a atualização automática para você, portanto, você pode remover esses recursos de sua base de código. Se qualquer uma destas situações se aplica ao seu aplicativo, você precisará resolver o problema antes da conversão.

+ __Seu aplicativo usa uma versão do .NET anterior à 4.6.1__. Somente o .NET 4.6.1 tem suporte. Você deve redirecionar seu aplicativo para o .NET 4.6.1 antes da conversão. 

+ __Seu aplicativo sempre é executado com privilégios de segurança elevados__. Seu aplicativo precisa funcionar durante a execução como o usuário interativo. Os usuários que instalam seu aplicativo da Windows Store podem não ser administradores de sistema, portanto, exigir que seu aplicativo seja executado com privilégios elevados significa que ele não será executado corretamente para usuários padrão.

+ __Seu aplicativo requer um driver de modo kernel ou um serviço Windows__. A ponte é adequada para um aplicativo, mas não dá suporte a um driver de modo kernel ou um serviço Windows que precisa ser executado em uma conta do sistema. Em vez de um serviço Windows, use uma [tarefa em segundo plano](https://msdn.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task).

+ __Os módulos do seu aplicativo são carregados no processo de processos que não estão em seu pacote de AppX__. Isso não é permitido, o que significa que extensões de processo, como [extensões do shell](https://msdn.microsoft.com/library/windows/desktop/dd758089.aspx), não têm suporte. Mas se você tiver dois aplicativos no mesmo pacote, será possível fazer comunicação entre processos entre eles.

+ __Seu aplicativo usa uma AUMID (ID do Modelo do Usuário do Aplicativo)__. Se o seu processo chama [SetCurrentProcessExplicitAppUserModelID](https://msdn.microsoft.com/library/windows/desktop/dd378422.aspx) para definir sua própria AUMID, então ele poderá usar apenas a AUMID gerada para ele pelo ambiente de modelo de aplicativo/pacote AppX. Não é possível definir AUMIDs personalizadas.

+ __Seu aplicativo modifica o hive do Registro HKEY_LOCAL_MACHINE (HKLM)__. Qualquer tentativa de seu aplicativo de criar uma chave HKLM, ou abrir uma para modificação, resultará em uma falha de acesso negado. Lembre-se de que seu aplicativo tem sua própria exibição virtualizada particular do registro, portanto, a noção de um hive do registro no âmbito do usuário e do computador (o que o HKLM é) não se aplica. Você precisará encontrar outra maneira de fazer o que o HKLM faz, como, por exemplo, gravar em HKEY_CURRENT_USER (HKCU).

+ __Seu aplicativo usa uma subchave de registro ddeexec como um meio de iniciar outro aplicativo__. Em vez disso, use um dos manipuladores de verbo DelegateExecute conforme configurado pelas várias extensões ativáveis* no [manifesto de pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/br211474.aspx).

+ __Seu aplicativo grava na pasta AppData com a intenção de compartilhar dados com outro aplicativo__. Após a conversão, a pasta AppData é redirecionada para o armazenamento de dados de aplicativo local, que é um repositório particular para cada aplicativo UWP. Use uma maneira diferente de compartilhamento de dados entre processos. Para obter mais informações, consulte [Armazene e recupere configurações e outros dados de aplicativo](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data).

+ __Seu aplicativo grava no diretório de instalação do seu aplicativo__. Por exemplo, seu aplicativo grava em um arquivo de log que você coloca no mesmo diretório que o exe. Isso não é aceito, portanto, você precisará encontrar outro local, como o armazenamento de dados de aplicativo local.

+ __A instalação do seu aplicativo requer interação do usuário__. O instalador do seu aplicativo deve ser capaz de ser executado silenciosamente, e ele deve instalar todos os pré-requisitos que não estão por padrão em uma imagem limpa do sistema operacional.

+ __Seu aplicativo usa a pasta de trabalho atual__. No tempo de execução, seu aplicativo convertido não terá o mesmo diretório trabalho especificado anteriormente no atalho .LNK em sua área de trabalho. Você precisa alterar seu CWD no tempo de execução se ter o diretório correto é importante para o seu aplicativo funcionar corretamente.

+ __Seu aplicativo requer UIAccess__. Se o seu aplicativo especifica `UIAccess=true` no elemento `requestedExecutionLevel` do manifesto UAC, a conversão em UWP não será possível no momento. Para obter mais informações, consulte [Visão geral sobre a Automação da Interface do Usuário](https://msdn.microsoft.com/library/ms742884.aspx).

+ __Seu aplicativo expõe objetos COM ou assemblies GAC para uso por outros processos__. Na versão atual, seu aplicativo não pode expor objetos COM nem assemblies GAC para uso por processos originários de executáveis externos ao seu pacote AppX. Os processos de dentro do pacote podem registrar e usar objetos COM e assemblies GAC normalmente, mas eles não estarão visíveis externamente. Isso significa que cenários de interoperabilidade como OLE não funcionarão se forem invocados por processos externos. 

+ __Seu aplicativo está vinculando bibliotecas de tempo de execução C de maneira incompatível__. A biblioteca de tempo de execução C/C++ da Microsoft oferece rotinas de programação para o sistema operacional Microsoft Windows. Essas rotinas automatizam muitas tarefas comuns de programação que não são fornecidas pelas linguagens C e C++. Se seu aplicativo utiliza a biblioteca de tempo de execução C/C++, você precisa garantir que ela seja vinculada de maneira compatível. 
    
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

+ __Seu aplicativo instala e carrega os assemblies da pasta lado a lado do Windows__. Por exemplo, seu aplicativo usa bibliotecas de tempo de execução C VC8 ou VC9 e está vinculando-as dinamicamente da pasta lado a lado do Windows, ou seja, seu código está usando os arquivos DLL comuns de uma pasta compartilhada. Isso não tem suporte. Você precisará vinculá-las estaticamente por meio de links aos arquivos de biblioteca redistribuível diretamente no seu código.


## Nesta seção

| Tópico | Descrição |
|-------|-------------|
| [Visualização Conversor de Aplicativos da Área de Trabalho (Projeto Centennial)](desktop-to-uwp-run-desktop-app-converter.md) | Mostra como executar o conversor de aplicativos da área de trabalho. Você provavelmente não precisará fazer muito, ou nada, para preparar seu aplicativo para o processo de conversão. |
| [Converter manualmente o seu aplicativo da área de trabalho do Windows em um aplicativo UWP (Plataforma Universal do Windows)](desktop-to-uwp-manual-conversion.md) | Saiba como criar um pacote de aplicativo e manifestá-lo manualmente. |
| [Implantar e depurar seu aplicativo UWP convertido](desktop-to-uwp-deploy-and-debug.md) | Contém informações para ajudá-lo a implantar e depurar seu aplicativo com êxito após convertê-lo. Além disso, se você estiver curioso sobre alguns dos elementos internos das extensões de conversão de área de trabalho, então este tópico é para você. |
| [Exemplos de código de ponte de aplicativo de área de trabalho para UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples) | Exemplos de código em GitHub demonstrando recursos dos aplicativos convertidos. |



<!--HONumber=Jun16_HO5-->


