---
author: laurenhughes
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empacotando aplicativos UWP
description: "Para vender seu aplicativo da Plataforma Universal do Windows (UWP) ou distribuí-lo para outros usuários, você precisa criar um pacote appxupload para ele."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 6c699b357a2c1271f6765514331eb2ca0a6ff0b3
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="package-a-uwp-app-with-visual-studio"></a>União de um aplicativo UWP com o Visual Studio

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Para vender seu aplicativo da Plataforma Universal do Windows (UWP) ou distribuí-lo para outros usuários, você precisa criar um pacote appxupload para ele. Quando você criar o appxupload, outro pacote appx será gerado para uso no teste e no sideload. Você pode distribuir o aplicativo diretamente pelo sideload do pacote appx em um dispositivo. Este artigo descreve o processo de configuração, criação e teste de um pacote do aplicativo UWP. Para obter mais informações sobre o sideload, consulte [Fazer o sideload de aplicativos no Windows 10](https://technet.microsoft.com/library/mt269549.aspx).

Para o Windows 10, você gera um pacote (.appxupload) que pode ser carregado para a Windows Store. Assim, o aplicativo fica disponível para ser instalado e executado em qualquer dispositivo com o Windows 10. Aqui estão as etapas para criar um pacote do aplicativo.

1.  [Antes de empacotar seu aplicativo](#before-packaging-your-app). Siga estas etapas para se certificar de que o aplicativo esteja pronto para ser empacotado para envio à loja.
2.  [Configure um pacote do aplicativo](#configure-an-app-package). Use o designer de manifesto para configurar o pacote. Por exemplo, adicione imagens de bloco e escolha as orientações compatíveis com o aplicativo.
3.  [Crie um pacote do aplicativo](#create-an-app-package). Use o assistente no Microsoft Visual Studio para criar um pacote do aplicativo e, em seguida, certificar o pacote com o Kit de Certificação de Aplicativos Windows.
4.  [Fazer o sideload do pacote do aplicativo](#sideload-your-app-package). Depois do sideload do aplicativo para um dispositivo, você poderá testar se ele funciona corretamente.

Depois de concluir as etapas acima, você estará pronto para vender o aplicativo na loja. Se você tem um aplicativo de linha de negócios (LOB) que não pretende vender porque serve apenas para usuários internos, você pode fazer o sideload dele para instalá-lo em qualquer dispositivo Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empacotar o aplicativo

1.  Teste o aplicativo. Antes de empacotar o aplicativo para envio à loja, certifique-se de que ele funcione conforme esperado em todas as famílias de dispositivos a que você pretende dar suporte. Essas famílias de dispositivos podem incluir desktop, celular, Surface Hub, XBOX, dispositivos IoT ou outros.
2.  Otimize o aplicativo. Você pode usar as ferramentas de criação de perfil e depuração para otimizar o desempenho do aplicativo UWP. Por exemplo, a ferramenta de linha do tempo para capacidade de resposta da interface do usuário, a ferramenta de uso da memória, a ferramenta de uso da CPU e muito mais. Para obter mais informações sobre essas ferramentas, consulte [Executar ferramentas de diagnóstico sem depuração](https://msdn.microsoft.com/library/dn957936.aspx).
3.  Verifique a compatibilidade nativa do .NET (para aplicativos VB e C#). Com o UWP, agora existe um novo compilador nativo que melhorará o desempenho do tempo de execução do aplicativo. Com essa alteração, é extremamente recomendável que você teste seu aplicativo nesse ambiente de compilação. Por padrão, a configuração de build **Release** habilita a cadeia de ferramentas nativa .NET, então é importante testar seu aplicativo com essa configuração **Release** e verificar se seu aplicativo se comporta como o esperado. Alguns problemas de depuração comuns que podem acontecer com o .NET nativo estão explicados mais detalhadamente [aqui](http://blogs.msdn.com/b/visualstudioalm/archive/2015/07/29/debugging-net-native-windows-universal-apps.aspx).

## <a name="configure-an-app-package"></a>Configurar um pacote do aplicativo

O arquivo de manifesto do aplicativo (package.appxmanifest) tem as propriedades e as configurações necessárias para criar o pacote do aplicativo. Por exemplo, as propriedades no arquivo de manifesto descrevem a imagem a ser usada como o bloco do aplicativo e as orientações compatíveis com o aplicativo quando um usuário gira o dispositivo.

O Visual Studio tem um designer de manifesto que facilita a atualização do arquivo de manifesto sem editar o XML bruto do arquivo.

O Visual Studio pode associar o pacote à Loja. Quando você faz isso, alguns dos campos na guia Empacotamento do designer de manifesto são atualizados automaticamente.

**Configure um pacote com o designer de manifesto**

1.  Em **Gerenciador de soluções**, expanda o nó do projeto do seu aplicativo UWP.
2.  Clique duas vezes no arquivo **Package.appxmanifest**. Se o arquivo de manifesto já estiver aberto no modo de exibição de código XML, o Visual Studio solicitará que você feche o arquivo.
3.  Agora é possível decidir como configurar o aplicativo. Cada guia contém informações que você pode configurar sobre o aplicativo e links para obter mais informações, se necessário.<br/>
    ![Designer de manifesto do Visual Studio](images/packaging-screen1.jpg)

    Verifique se você tem todas as imagens exigidas para um aplicativo UWP na guia **Ativos visuais**.

    Da guia **Empacotamento**, você pode inserir dados de publicação. É aqui que você pode escolher qual certificado usar para assinar seu aplicativo. Todos os aplicativos UWP devem ser assinados com um certificado. Para fazer o sideload de um pacote do aplicativo, você precisa confiar no pacote. O certificado deve estar instalado nesse dispositivo para confiar no pacote. Para obter mais informações sobre o sideload, consulte [Habilitar seu dispositivo para desenvolvimento](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Salve o arquivo depois de fazer as edições necessárias para o aplicativo.

## <a name="create-an-app-package"></a>Criar um pacote de aplicativo

Para distribuir um aplicativo por meio da Loja, você deve criar um pacote appxupload. Você pode fazer isso usando o assistente **Criar pacotes do aplicativo**. Siga os passos a seguir para criar um pacote adequado para envio de loja com o Microsoft Visual Studio 2015.

**Para criar seu pacote do aplicativo**

1.  Em **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo UWP.
2.  Clique com botão direito no projeto e escolha **Loja**->**Criar pacotes de aplicativo**. Se essa opção estiver desabilitada ou não aparecer, verifique se o projeto é um projeto UWP.<br/>
    ![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)

    O assistente **Criar pacotes de aplicativo** aparecerá.

3.  Selecione Sim na primeira caixa de diálogo perguntando se você deseja criar pacotes para carregar na Windows Store. Depois, clique em Avançar.<br/>
    ![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/packaging-screen3.jpg)

    Se você escolher Não aqui, o Visual Studio não irá gerar o pacote .appxupload de que você precisa para o envio à loja. Caso queira apenas fazer o sideload do aplicativo para executá-lo em dispositivos internos, você pode selecionar essa opção. Para obter mais informações sobre o sideload, consulte [Habilitar seu dispositivo para desenvolvimento](https://msdn.microsoft.com/library/windows/apps/Dn706236).

4.  Entre usando a conta de desenvolvedor no Centro de Desenvolvimento do Windows. (Se ainda não tiver uma conta de desenvolvedor, o assistente ajudará você a criar uma.)
5.  Selecione o nome do aplicativo para o pacote ou reserve um novo caso você ainda não tenha reservado um com o portal do Centro de Desenvolvimento do Windows.<br/>
    ![Janela Criar Pacotes de Aplicativos com a seleção do nome do aplicativo mostrada](images/packaging-screen4.jpg)
6.  Certifique-se de selecionar todas as três configurações de arquitetura (x86, x64 e ARM) na caixa de diálogo **Selecionar e configurar pacotes**. Dessa forma, seu aplicativo poderá ser implantado na grande maioria dos dispositivos. Na caixa de listagem **Gerar lote de aplicativo**, selecione **Sempre**. Isso torna o processo de envio para a Loja muito mais simples, porque você terá apenas um arquivo para carregar (. appxupload). O pacote único conterá todos os pacotes necessários a serem implantados em dispositivos com cada arquitetura de processador.<br/>
    ![Janela Criar Pacotes de Aplicativos com a configuração do pacote mostrada](images/packaging-screen5.jpg)
7.  É uma boa ideia incluir arquivos de símbolo PDB completos tendo em vista a melhor experiência de [análise de falhas](http://blogs.windows.com/buildingapps/2015/07/13/crash-analysis-in-the-unified-dev-center/) no Centro de Desenvolvimento do Windows. Você pode saber mais sobre a depuração com símbolos visitando [Depurando com símbolos](https://msdn.microsoft.com/library/windows/desktop/Ee416588).
8.  Agora é possível configurar os detalhes para criar o pacote. Quando estiver pronto para publicar seu aplicativo, você carregará os pacotes do local de saída.
9.  Clique em **Criar**, para gerar seu pacote appxupload.
10. Agora, você verá esta caixa de diálogo.<br/>
    ![Janela Criação de pacote concluída com opções de validação mostradas](images/packaging-screen6.jpg)

    Valide o aplicativo antes de enviá-lo para à Loja para certificação em uma máquina local ou remota. (Você pode validar apenas compilações de lançamento para o pacote do aplicativo e não compilações de depuração.)

11. Para validar localmente, deixe a opção **Máquina local** selecionada e clique em **Iniciar o Kit de Certificação de Aplicativos Windows**. Para obter mais informações sobre como testar seu aplicativo com o Kit de Certificação de Aplicativos Windows, consulte [Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).

    O Kit de Certificação de Aplicativos Windows realiza testes e mostra os resultados. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

    Se você tem um Windows 10 remoto que gostaria de usar para testar, será preciso instalar manualmente o Kit de Certificação de Aplicativos Windows nesse dispositivo. A próxima seção o guiará pelas etapas. Depois de ter feito isso, você pode selecionar **Máquina remota** e clicar em **Iniciar o Kit de Certificação de Aplicativos Windows** para se conectar ao dispositivo remoto e executar os testes de validação.

12. Após a conclusão de WACK e a aprovação do aplicativo, você estará pronto para carregar na loja. Certifique-se de carregar o arquivo correto. Ele pode ser encontrado na pasta raiz da sua solução \\\[AppName\]\\AppPackages e terminará com a extensão de arquivo .appxupload. O nome terá o formato \[AppName\]\_\[AppVersion\]\_x86\_x64\_arm\_bundle.appxupload.

**Valide seu pacote do aplicativo em um dispositivo Windows 10 remoto.**

1.  Habilite seu dispositivo Windows 10 para desenvolvimento seguindo as instruções de [Habilitar seu dispositivo para desenvolvimento](https://msdn.microsoft.com/library/windows/apps/Dn706236).
    **Importante**  Não é possível validar seu pacote de aplicativo em um dispositivo ARM remoto para Windows 10.
2.  Baixe e instale as ferramentas remotas para o Visual Studio. Essas ferramentas são usadas para executar o Kit de Certificação de Aplicativos Windows remotamente. Você pode obter mais informações sobre essas ferramentas, inclusive onde baixá-las visitando [Executar aplicativos da Windows Store em uma máquina remota](https://msdn.microsoft.com/library/hh441469.aspx#BKMK_Starting_the_Remote_Debugger_Monitor).
3.  Baixe o [Kit de Certificação de Aplicativos Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) exigido e, depois, instale-o em seu dispositivo Windows 10 remoto.
4.  Na página **Criação de pacote concluída** do assistente, escolha o botão de opção **Máquina remota** e, em seguida, escolha o botão de reticências próximo ao botão **Conexão de teste**.
    **Observação**  O botão de opção **Máquina remota** ficará disponível apenas se você selecionar pelo menos uma configuração de solução que tenha suporte para validação. Para obter mais informações sobre como testar o aplicativo com o WACK, consulte [Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/Mt186449).
5.  Especifique uma forma de dispositivo dentro de sua sub-rede, ou forneça o Servidor de Nomes de Domínios DNS ou o endereço IP de um dispositivo que esteja fora de sua sub-rede.
6.  Na lista **Modo de autenticação**, escolha **Nenhum**, se seu dispositivo não exigir que você se registre usando suas credenciais do Windows.
7.  Escolha o botão **Selecionar** e, em seguida, escolha o botão **Iniciar o Kit de Certificação de Aplicativos Windows**. Se as ferramentas remotas estiverem sendo executadas nesse dispositivo, o Visual Studio se conectará a ele e, então, realizará o testes de validação. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186450).

## <a name="sideload-your-app-package"></a>Fazer o sideload do pacote do aplicativo

Com pacotes do aplicativo UWP, não basta instalar um aplicativo no dispositivo, como aplicativos da área de trabalho. Normalmente, você baixa esses aplicativos na Loja e é assim que eles são instalados no dispositivo. Mas você pode fazer o sideload de aplicativos para o dispositivo sem enviá-los à Loja. Isso permite instalar e testá-los usando o pacote do aplicativo (.appx) que você criou. Caso tenha um aplicativo que não queira vender na Loja, como um aplicativo de linha de negócios (LOB), você pode fazer o sideload desse aplicativo de maneira que outros usuários na empresa possam usá-lo.

A lista a seguir apresenta requisitos para o sideload do aplicativo.

-   Você deve [habilitar o dispositivo para desenvolvimento](https://msdn.microsoft.com/library/windows/apps/Dn706236).
-   Para fazer o sideload de seu aplicativo em um dispositivo Windows 10 Mobile, você deve usar a ferramenta [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md).

**Fazer o sideload de um aplicativo para um desktop, laptop ou tablet**

1.  Copie as pastas da versão que você deseja instalar no dispositivo de destino.

    Se você tiver criado um lote de aplicativo, terá uma pasta baseada no número de versão e uma pasta \_test. Por exemplo, estas duas pastas (em que a versão para instalar é 1.0.2):

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test

    Se você não tiver um lote de aplicativo, pode apenas copiar a pasta para a arquitetura correta e a pasta de teste correspondente. Por exemplo, estas duas pastas.

    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64
    -   C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_x64\_Test
2.  No dispositivo de destino, abra a pasta de teste. Por exemplo, C:\\Projects\\MyApp\\MyApp\\AppPackages\\MyApp\_1.0.2.0\_Test
3.  Clique com botão direito no arquivo **Add-AppDevPackage.ps1** e, em seguida, escolha **Executar com PowerShell** e siga os prompts.<br/>
    ![Explorador de arquivos posicionado no script do PowerShell mostrado](images/packaging-screen7.jpg)

    Quando o pacote do aplicativo tiver sido instalado, você verá a seguinte mensagem na janela do PowerShell: Seu aplicativo foi instalado com êxito.

    **Observação**  Para abrir o menu de atalho em um tablet, toque a tela onde você gostaria de clicar com o botão direito, segure até um círculo completo aparecer e, então, levante o dedo. O menu de atalho será exibido depois que você levantar o dedo.
4.  Clique no botão Iniciar e digite o nome do aplicativo para iniciá-lo.
