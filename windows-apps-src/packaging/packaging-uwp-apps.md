---
ms.assetid: 96361CAF-C347-4671-9721-8208CE118CA4
title: Empacotando aplicativos UWP
description: Para distribuir ou vender seu aplicativo UWP (Plataforma Universal do Windows), será necessário criar um pacote de apps para ele.
ms.date: 06/10/2019
ms.topic: article
keywords: windows 10, uwp
f1_keywords:
- vs.packagewizard
- vs.storeassociationwizard
ms.localizationpriority: medium
ms.openlocfilehash: 265e034b264cf82bacfa5a32141eb5d999d57108
ms.sourcegitcommit: aa5a055e3ff9ee9defc73ed9567196d59f59542a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/10/2019
ms.locfileid: "66825030"
---
# <a name="package-a-uwp-app-with-visual-studio"></a>União de um aplicativo UWP com o Visual Studio

Para vender seu aplicativo UWP (Plataforma Universal do Windows) ou distribuí-lo para outros usuários, você precisa empacotá-lo. Se você não quiser distribuir seu app por meio da Microsoft Store, poderá fazer o sideload do pacote do app diretamente para um dispositivo ou distribuí-lo através de [Instalação Web](installing-UWP-apps-web.md). Este artigo descreve o processo de configuração, criação e teste de um pacote do aplicativo UWP usando o Visual Studio. Para obter mais informações sobre como gerenciar e implantar aplicativos da linha de negócios (LOB), consulte [Gerenciamento de aplicativos corporativos](https://docs.microsoft.com/windows/client-management/mdm/enterprise-app-management).

No Windows 10, você pode enviar um pacote do aplicativo, pacote de aplicativo ou um arquivo de upload do pacote de aplicativo completo para [Partner Center](https://partner.microsoft.com/dashboard). Dessas opções, enviar um arquivo de upload do pacote de aplicativo fornece a melhor experiência.

## <a name="types-of-app-packages"></a>Tipos de pacotes de aplicativo

- **Pacote do aplicativo (. AppX ou .msix)**  
    Um arquivo que contém seu aplicativo em um formato que pode ser adicionado por sideload em um dispositivo. Qualquer arquivo do pacote de aplicativo único criado pelo Visual Studio está **não** se destina a ser enviado ao Partner Center e deve ser usado para o sideload e apenas para fins de teste. Se você quiser enviar seu aplicativo para o Centro de parceiros, use o arquivo de carregamento do pacote de aplicativo.  

- **Pacote de aplicativo (. appxbundle ou .msixbundle)**  
    Um lote de aplicativo é um tipo de pacote que pode conter vários pacotes de aplicativos, cada um deles é criado para dar suporte a uma arquitetura de dispositivo específico. Por exemplo, um lote de aplicativo pode conter três pacotes de aplicativo separado para configurações x86, x64 e ARM. Lotes de aplicativo devem ser gerados sempre que possível, pois eles permitem que seu aplicativo esteja disponível na maior variedade possível de dispositivos.  

- **Arquivo de carregamento do pacote do aplicativo (. appxupload ou .msixupload)**  
    Um único arquivo que pode conter vários pacotes de aplicativos ou um lote de aplicativo para dar suporte a várias arquiteturas de processador. O arquivo de carregamento do pacote de aplicativo também contém um arquivo de símbolo para [analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) depois que seu aplicativo foi publicado na Microsoft Store. Esse arquivo será criado automaticamente para você se você estiver criando seu aplicativo com o Visual Studio com a intenção de enviá-lo ao Partner Center para publicação.

Aqui está uma visão geral das etapas de preparação e de criação de um pacote do app:

1.  [Antes de empacotar seu aplicativo](#before-packaging-your-app). Siga estas etapas para garantir que seu aplicativo está pronto para ser empacotado para envio do Partner Center.
2.  [Configure um pacote do aplicativo](#configure-an-app-package). Use o designer de manifesto do Visual Studio para configurar o pacote. Por exemplo, adicione imagens de bloco e escolha as orientações compatíveis com o aplicativo.
3.  [Crie um arquivo de upload de pacote do aplicativo](#create-an-app-package-upload-file). Use o assistente de pacote de aplicativo do Visual Studio para criar um pacote do aplicativo e, em seguida, certificar o pacote com o Kit de Certificação de Aplicativos Windows.
4.  [Fazer o sideload do pacote do aplicativo](#sideload-your-app-package). Depois do sideload do aplicativo para um dispositivo, você poderá testar se ele funciona como esperado.

Depois de concluir as etapas acima, você estará pronto para distribuir seu aplicativo. Se você tiver um aplicativo (LOB) de linha de negócios que você não planeja vender porque é apenas para usuários internos, você pode efetuar sideload desse aplicativo para instalá-lo em qualquer dispositivo Windows 10.

## <a name="before-packaging-your-app"></a>Antes de empacotar o aplicativo

1.  **Teste seu aplicativo.** Antes de empacotar seu aplicativo para envio do Partner Center, verifique se que ele funciona conforme o esperado em todas as famílias de dispositivos que você planeja oferecer suporte. Essas famílias de dispositivos podem incluir desktop, celular, Surface Hub, Xbox, dispositivos IoT ou outros. Para obter mais informações sobre como implantar e testar seu aplicativo usando o Visual Studio, consulte [implantar e depurar aplicativos UWP](../debug-test-perf/deploying-and-debugging-uwp-apps.md).
2.  **Otimize seu aplicativo.** Você pode usar as ferramentas de criação de perfil e depuração do Visual Studio para otimizar o desempenho do aplicativo UWP. Por exemplo, a ferramenta de linha do tempo para capacidade de resposta da interface do usuário, a ferramenta de uso da memória, a ferramenta de uso da CPU e muito mais. Para obter mais informações sobre essas ferramentas, consulte o tópico [Tour sobre recurso de perfil](https://docs.microsoft.com/visualstudio/profiling/profiling-feature-tour).
3.  **Verificar a compatibilidade do .NET Native (para o VB e C# aplicativos).** Na Plataforma Universal do Windows, existe um compilador nativo que melhorará o desempenho do tempo de execução do app. Com essa alteração, você deverá testar seu aplicativo nesse ambiente de compilação. Por padrão, a configuração de build **Release** habilita a cadeia de ferramentas .NET Native, então é importante testar seu aplicativo com essa configuração **Release** e verificar se seu aplicativo se comporta como o esperado. Alguns problemas de depuração comuns que podem acontecer com o .NET Native serão explicados mais detalhadamente em [Depuração de aplicativos universais do Windows do .NET Native](https://blogs.msdn.microsoft.com/devops/2015/07/29/debugging-net-native-windows-universal-apps/).

## <a name="configure-an-app-package"></a>Configurar um pacote do aplicativo

O arquivo de manifesto do app (Package.appxmanifest.xml) é um arquivo XML que contém as propriedades e as configurações necessárias para criar o pacote do app. Por exemplo, as propriedades no arquivo de manifesto do aplicativo descrevem a imagem a ser usada como o bloco do aplicativo e as orientações compatíveis com o aplicativo quando um usuário gira o dispositivo.

O designer de manifesto do Visual Studio permite a atualização do arquivo de manifesto sem a edição do XML bruto do arquivo.

### <a name="configure-a-package-with-the-manifest-designer"></a>Configure um pacote com o designer de manifesto

1.  Em **Gerenciador de soluções**, expanda o nó do projeto do seu aplicativo UWP.
2.  Clique duas vezes no arquivo **Package.appxmanifest**. Se o arquivo de manifesto já estiver aberto no modo de exibição de código XML, o Visual Studio solicitará que você feche o arquivo.
3.  Agora é possível decidir como configurar o aplicativo. Cada guia contém informações que você pode configurar sobre o aplicativo e links para obter mais informações, se necessário.  
    ![Designer de manifesto do Visual Studio](images/packaging-screen1.jpg)

    Verifique se você tem todas as imagens exigidas para um aplicativo UWP na guia **Ativos visuais**.

    Da guia **Empacotamento**, você pode inserir dados de publicação. É aqui que você pode escolher qual certificado usar para assinar seu aplicativo. Todos os aplicativos UWP devem ser assinados com um certificado.

    >[!IMPORTANT]
    >Se você estiver publicando seu aplicativo na Microsoft Store, seu aplicativo será assinado com um certificado confiável para você. Isso permite que o usuário instale e execute seu aplicativo sem precisar instalar o certificado de autenticação do aplicativo associado.

    Se você não estiver publicando seu aplicativo e deseja simplesmente carregar um pacote do aplicativo, você precisa confiar no pacote. Para confiar o pacote, o certificado deve estar instalado no dispositivo do usuário. Para obter mais informações sobre o sideload, consulte [Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).

4.  Salve seu arquivo **Package.appxmanifest** depois de fazer as edições necessárias para o aplicativo.

Se você estiver distribuindo seu aplicativo por meio do Microsoft Store, o Visual Studio pode associar seu pacote com o Store. Para fazer isso, o nome do projeto no Gerenciador de soluções com o botão direito e escolha **Store**->**associar aplicativo com o Store**. Você também pode fazer isso **criar pacotes de aplicativos** assistente, o que é descrito na seção a seguir. Quando você associa seu aplicativo, alguns dos campos na guia Empacotamento do designer de manifesto são atualizados automaticamente.

## <a name="create-an-app-package-upload-file"></a>Crie um arquivo de upload de pacote do aplicativo

Para distribuir um aplicativo por meio da Microsoft Store, você deve criar um pacote do aplicativo (. AppX ou .msix), pacote de aplicativo (. appxbundle ou .msixbundle) ou um arquivo de carregamento de pacote de aplicativo (. appxupload ou .msixupload) e [submeter o aplicativo empacotado Partner Center](https://docs.microsoft.com/windows/uwp/publish/app-submissions). Embora seja possível enviar um pacote de aplicativo ou pacote de aplicativos Partner Center sozinho, recomendamos que você envie um arquivo de carregamento de pacote do aplicativo. Você pode criar um arquivo de upload do pacote de aplicativo usando o **criar pacotes de aplicativos** assistente no Visual Studio, ou você pode criar um manualmente a partir de pacotes de aplicativo existentes ou pacotes de aplicativos.

>[!NOTE]
> Se você quiser criar um pacote de aplicativo (. AppX ou .msix) ou o pacote de aplicativo (. appxbundle ou .msixbundle) manualmente, consulte [criar um pacote do aplicativo com a ferramenta MakeAppx.exe](https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool).

### <a name="create-your-app-package-upload-file-using-visual-studio"></a>Criar seu arquivo de upload do pacote de aplicativo usando o Visual Studio

1.  Em **Gerenciador de soluções**, abra a solução para seu projeto de aplicativo UWP.
2.  Clique com botão direito no projeto e escolha **Loja**->**Criar pacotes de aplicativo**. Se essa opção estiver desabilitada ou não aparecer, verifique se o projeto é um projeto Universal do Windows.  
    ![Menu de contexto com a navegação para criar pacotes de aplicativos](images/packaging-screen2.jpg)

    O assistente **Criar pacotes de aplicativo** aparecerá.

3.  Selecione **quero criar pacotes para carregar o Microsoft Store usando um novo nome de aplicativo** na primeira caixa de diálogo e, em seguida, clique **próxima**.  
    ![Criar janela de caixa de diálogo de seus pacotes mostrada](images/packaging-screen3.jpg)

    Se você já tiver associado a seu projeto com um aplicativo na Store, você também tem uma opção para criar pacotes para o aplicativo Store associado. Se você escolher **quero criar pacotes para sideload**, Visual Studio não irá gerar o aplicativo upload (.msixupload ou. appxupload) arquivo de pacote para envios do Partner Center. Caso queira apenas fazer o sideload do aplicativo para executá-lo em dispositivos internos ou para testes, você pode selecionar essa opção. Para obter mais informações sobre o sideload, consulte [Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
4.  Na página seguinte, entre com sua conta de desenvolvedor Partner Center. Se ainda não tiver uma conta de desenvolvedor, o assistente ajudará você a criar uma.
    ![Criar janela de pacotes de aplicativos com a seleção do nome do aplicativo mostrada](images/packaging-screen4.jpg)
5.  Selecione o nome do aplicativo para o pacote da lista de aplicativos registrados atualmente à sua conta ou reservar um novo se ainda não tiver reservado um no Partner Center.  
6.  Certifique-se de que selecionou todas as três configurações de arquitetura (x86, x64 e ARM) na caixa de diálogo **Selecionar e Configurar Pacotes** para garantir que seu aplicativo possa ser implantado para a mais ampla variedade de dispositivos. Na caixa de listagem **Gerar lote de aplicativo**, selecione **Sempre**. Um pacote de aplicativos (. appxbundle ou .msixbundle) é preferível um arquivo de pacote de aplicativo único porque ela contém uma coleção de pacotes de aplicativos configurados para cada tipo de arquitetura do processador. Quando você escolhe gerar o pacote de aplicativo, pacote de aplicativo será incluído no aplicativo final arquivo do pacote upload (. appxupload ou .msixupload) juntamente com informações de depuração e análise de falha. Se você não tiver certeza de quais arquiteturas escolher ou deseja saber mais sobre quais arquiteturas são usadas por vários dispositivos, consulte [Arquiteturas de pacote do aplicativo](https://docs.microsoft.com/windows/uwp/packaging/device-architecture).  
    ![Criar janela de pacotes de aplicativos com a configuração de pacote mostrada](images/packaging-screen5.jpg)
7.  Incluir arquivos de símbolo PDB completos para [analisar o desempenho do aplicativo](https://docs.microsoft.com/windows/uwp/publish/analytics) do Partner Center depois que seu aplicativo foi publicado. Configurar todos os detalhes adicionais, como a numeração de versão ou o local de saída do pacote.
8.  Clique em **Criar** para gerar o pacote do aplicativo. Se você tiver selecionado um dos **quero criar pacotes para carregar o Microsoft Store** opções na etapa 3 e estiver criando um pacote para envio do Partner Center, o assistente criará um arquivo de pacote upload (. appxupload ou .msixupload). Se você selecionou **quero criar pacotes para sideload** na etapa 3, o assistente criará um pacote de aplicativo único ou um pacote de aplicativo com base nas suas seleções na etapa 6.
9. Quando seu aplicativo foi empacotado com êxito, você verá essa caixa de diálogo e você pode recuperar o arquivo de upload do pacote de aplicativo do local de saída especificado. Neste ponto, você pode [validar seu pacote do aplicativo no computador local ou um computador remoto](#validate-your-app-package) e [automatizar envios de repositório](#automate-store-submissions).
    ![Criação de pacote concluída janela com opções de validação exibidas](images/packaging-screen6.jpg)

### <a name="create-your-app-package-upload-file-manually"></a>Criar manualmente o seu arquivo de upload do pacote de aplicativo

1. Coloque os arquivos a seguir em uma pasta:
    - Um ou mais pacotes de aplicativo (.msix ou. AppX) ou um pacote de aplicativos (.msixbundle ou. appxbundle).
    - Um arquivo. appxsym. Este é um arquivo. PDB compactado que contém símbolos públicos do seu aplicativo usado para [análise de falhas](../publish/health-report.md) no Partner Center. Você pode omitir esse arquivo, mas se você fizer isso, nenhuma informação de depuração ou analítico falha estarão disponível para seu aplicativo.
2. Compacte a pasta.
3. Altere o nome da pasta zipada extensão de. zip para .msixupload ou. appxupload.

## <a name="validate-your-app-package"></a>Validar seu pacote do aplicativo

Valide seu aplicativo antes de enviá-lo ao Partner Center para certificação em um computador local ou remoto. Você pode validar apenas compilações de lançamento para o pacote do app e não compilações de depuração. Para obter mais informações sobre como enviar seu aplicativo Partner Center, consulte [envios de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

### <a name="validate-your-app-package-locally"></a>Validar seu pacote do aplicativo localmente

1. No final **criação de pacote concluída** página do **criar pacotes de aplicativos** assistente, deixe o **Máquina Local** opção selecionada e clique em **iniciar Kit de certificação de aplicativos do Windows**. Para obter mais informações sobre como testar seu aplicativo com o Kit de Certificação de Aplicativos Windows, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).

    O Kit de certificação de aplicativo do Windows (WACK) executa vários testes e retorna os resultados. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests) para obter informações mais específicas.

    Se você tiver um dispositivo remoto do Windows 10 que você deseja usar para teste, você precisará instalar o Kit de certificação de aplicativos do Windows manualmente nesse dispositivo. A próxima seção o guiará pelas etapas. Depois de ter feito isso, você pode selecionar **Máquina remota** e clicar em **Iniciar o Kit de Certificação de Aplicativos Windows** para se conectar ao dispositivo remoto e executar os testes de validação.

2. Depois que o WACK e seu aplicativo tiver sido aprovado a certificação, você estará pronto para enviar seu aplicativo para o Partner Center. Certifique-se de carregar o arquivo correto. O local padrão do arquivo pode ser encontrado na pasta raiz de sua solução `\[AppName]\AppPackages` e terminará com a extensão de arquivo. appxupload ou .msixupload. O nome será do formulário `[AppName]_[AppVersion]_x86_x64_arm_bundle.appxupload` ou `[AppName]_[AppVersion]_x86_x64_arm_bundle.msixupload` se você tiver optado por um pacote de aplicativos com todos da arquitetura do pacote selecionada.

### <a name="validate-your-app-package-on-a-remote-windows10-device"></a>Validar seu pacote do aplicativo em um dispositivo remoto do Windows 10

1. Habilitar seu dispositivo Windows 10 para desenvolvimento, seguindo a [habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) instruções.
    >[!IMPORTANT]
    > Você não pode validar seu pacote do aplicativo em um dispositivo ARM remoto para Windows 10.
2. Baixe e instale as ferramentas remotas para o Visual Studio. Essas ferramentas são usadas para executar o Kit de Certificação de Aplicativos Windows remotamente. Você pode obter mais informações sobre essas ferramentas, inclusive onde baixá-las visitando [Executar aplicativos UWP em uma máquina remota](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-on-a-remote-machine?view=vs-2015).
3. Baixe o necessária [Kit de certificação de aplicativos do Windows](https://go.microsoft.com/fwlink/p/?LinkID=309666) e, em seguida, instale-o em seu dispositivo Windows 10 remoto.
4. Na página **Criação de pacote concluída** do assistente, escolha o botão de opção **Máquina remota** e, em seguida, escolha o botão de reticências próximo ao botão **Conexão de teste**.
    >[!NOTE]
    > O **máquina remota** botão de opção está disponível somente se você selecionou pelo menos uma configuração de solução compatível com a validação. Para obter mais informações sobre como testar o aplicativo com o WACK, consulte [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit).
5. Especifique uma forma de dispositivo dentro de sua sub-rede, ou forneça o Servidor de Nomes de Domínios DNS ou o endereço IP de um dispositivo que esteja fora de sua sub-rede.
6. Na lista **Modo de autenticação**, escolha **Nenhum**, se seu dispositivo não exigir que você se registre usando suas credenciais do Windows.
7. Escolha o botão **Selecionar** e, em seguida, escolha o botão **Iniciar o Kit de Certificação de Aplicativos Windows**. Se as ferramentas remotas estiverem sendo executadas nesse dispositivo, o Visual Studio se conectará ao aplicativo e, então, realizará o testes de validação. Consulte [Testes do Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit-tests).

## <a name="automate-store-submissions"></a>Automatizar os envios de Store

A partir de 2019 do Visual Studio, você pode enviar o arquivo. appxupload gerado para a Microsoft Store diretamente do IDE, selecionando o **envie automaticamente para a Microsoft Store após a validação do Kit de certificação de aplicativos do Windows** opção no final de [assistente criar pacotes de aplicativos](#create-your-app-package-upload-file-using-visual-studio). Este recurso aproveita o Azure Active Directory para acessar as informações necessárias para publicar seu aplicativo da conta do Partner Center. Para usar esse recurso, você precisará associar o Active Directory do Azure com sua conta no Partner Center e recuperar várias credenciais necessárias para envios.

### <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associar o Active Directory do Azure com sua conta no Partner Center

Antes de você pode recuperar as credenciais que são necessárias para envios de Store automáticos, você primeiro deve seguir estas etapas na [painel do Partner Center](https://partner.microsoft.com/dashboard) se você ainda não fez isso.

1. [Associar sua conta no Partner Center do Azure Active Directory da sua organização](https://docs.microsoft.com/windows/uwp/publish/associate-azure-ad-with-partner-center). Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo locatário do AD do Azure de dentro do Partner Center sem nenhum custo adicional.

2. [Adicionar um aplicativo do Azure AD ao seu parceiro de conta do Centro de](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#add-azure-ad-applications-to-your-partner-center-account). Este aplicativo do Azure AD representa o aplicativo ou serviço que você usará para acessar os envios para sua conta do Centro de desenvolvimento. Você deve atribuir esse aplicativo para o **Manager** função. Se esse aplicativo já existe no diretório do Azure AD, selecione-o na página **Adicionar aplicativos do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo aplicativo do Azure AD na página **Add Azure AD applications**.

### <a name="retrieve-the-credentials-required-for-submissions"></a>Recuperar as credenciais necessárias para envios

Em seguida, você pode recuperar as credenciais do Partner Center necessárias para envios: o **ID do locatário do Azure**, o **ID do cliente** e o **chave do cliente**.

1. Vá para o [painel do Partner Center](https://partner.microsoft.com/dashboard) e entre com suas credenciais do AD do Azure.

2. No painel do Partner Center, selecione o ícone de engrenagem (perto do canto superior direito do painel de controle) e, em seguida, selecione **configurações do desenvolvedor**.

3. No **as configurações** menu no painel esquerdo, clique em **usuários**.

4. Clique no nome do seu aplicativo do Azure AD para ir para as configurações do aplicativo. Nessa página, copie o **ID do locatário** e **ID do cliente** valores.

5. No **teclas** seção, clique em **adicionar nova chave**. Na próxima tela, copie o **chave** valor, que corresponde ao segredo do cliente. Você não poderá acessar essa informação novamente depois que você sair desta página, portanto, certifique-se de não perdê-lo. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](https://docs.microsoft.com/windows/uwp/publish/add-users-groups-and-azure-ad-applications#manage-keys-for-an-azure-ad-application).

### <a name="configure-automatic-store-submissions-in-visual-studio"></a>Configurar o envio automático de Store no Visual Studio

Depois de concluir as etapas anteriores, você pode configurar o envio automático de Store no Visual Studio de 2019.

1. No final o [assistente criar pacotes de aplicativos](#create-your-app-package-upload-file-using-visual-studio), selecione **enviar automaticamente para a Microsoft Store após a validação do Kit de certificação de aplicativos do Windows** e clique em **reconfigurar**.

2. No **as configurações de envio de Microsoft Store configurar** caixa de diálogo, insira a chave de ID, ID do cliente e cliente do locatário do Azure.

    ![Configurar as configurações de envio do Microsoft Store](images/packaging-screen8.jpg)

    > [!Important]
    > Suas credenciais podem ser salvos em seu perfil para ser usado em envios futuros

3. Clique em **OK**.

O envio será iniciada depois que o teste WACK tiverem terminado. Você pode acompanhar o progresso de envio na **verificar e publicar** janela.

![Verifique e publique o progresso](images/packaging-screen9.jpg)

## <a name="sideload-your-app-package"></a>Fazer o sideload do pacote do aplicativo

Com pacotes de aplicativos UWP, aplicativos não são instalados em um dispositivo como estão com aplicativos da área de trabalho. Normalmente, você baixa os aplicativos UWP da Microsoft Store, que também instala o app em seu dispositivo para você. Os apps podem ser instalados sem ser publicados à Store (sideload). Isso permite que você instalar e testar aplicativos usando o pacote de aplicativo do arquivo que você criou. Caso tenha um aplicativo que não queira vender na Loja, como um aplicativo de linha de negócios (LOB), você pode fazer o sideload desse aplicativo de maneira que outros usuários na empresa possam usá-lo.

Antes de poder fazer sideload de seu aplicativo em um dispositivo de destino, você deve [habilitar seu dispositivo para desenvolvimento](../get-started/enable-your-device-for-development.md).

Para fazer sideload de seu aplicativo em um dispositivo Windows 10 Mobile, use o [WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) ferramenta. Para desktops, laptops e tablets, siga as instruções abaixo.

### <a name="sideload-your-app-package-on-windows-10-anniversary-update-or-later"></a>Fazer sideload de seu aplicativo do pacote na atualização de aniversário do Windows 10 ou posterior

Introduzido no Windows 10 Anniversary Update (Windows 10, versão 1607), pacotes de aplicativos podem ser instalados simplesmente clicando duas vezes o arquivo de pacote do aplicativo. Para usar isso, navegue até seu pacote do aplicativo ou arquivo de pacote do aplicativo e clique duas vezes nele. [O instalador de aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) inicia e fornece as informações de aplicativo básico, bem como um botão instalar, barra de progresso da instalação e quaisquer mensagens de erro relevantes.

![O Instalador de App é exibido para a instalação de um app de exemplo chamado Contoso](images/appinstaller-screen.png)

> [!NOTE]
> O instalador de aplicativo pressupõe que o aplicativo é confiável pelo dispositivo. Se você estiver fazendo sideload de um app de desenvolvedor ou corporativo, será necessário instalar o certificado de autenticação no repositório Autoridades Certificação de Fornecedores ou Pessoas Confiáveis no dispositivo. Se você não tiver certeza de como fazer isso, consulte [Instalação de certificados de teste](https://docs.microsoft.com/windows-hardware/drivers/install/installing-test-certificates).

### <a name="sideload-your-app-package-on-previous-versions-of-windows"></a>Fazer sideload de seu aplicativo de pacote em versões anteriores do Windows

1.  Copie as pastas da versão do app a ser instalado no dispositivo de destino.

    Se você tiver criado um lote de aplicativo, terá uma pasta baseada no número de versão e uma pasta `*_Test`. Por exemplo, estas duas pastas (em que a versão para instalar é 1.0.2.0):

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_Test`

    Se você não tiver um lote de aplicativo, copie a pasta para a arquitetura correta e sua pasta `*_Test` correspondente. Estas duas pastas são um exemplo de um pacote do app com a arquitetura x64 e sua pasta `*_Test`:

    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64`
    -   `C:\Projects\MyApp\MyApp\AppPackages\MyApp_1.0.2.0_x64_Test`

2.  No dispositivo de destino, abra a pasta `*_Test`.
3.  Clique com botão direito do mouse no arquivo **Add-AppDevPackage.ps1**. Escolha **Executar com PowerShell** e siga os prompts.  
    ![Explorador de arquivos, navegada até o script do PowerShell mostrado](images/packaging-screen7.jpg)

    Quando o pacote do aplicativo tiver sido instalado, a janela do PowerShell exibe esta mensagem: **O aplicativo foi instalado com êxito.**
    >[!TIP]
    > Para abrir o menu de atalho em um tablet, toque na tela onde você deseja com o botão direito, espere até aparece um círculo completo e, em seguida, levante o dedo. O menu de atalho será exibido depois que você levantar o dedo.

4.  Clique no botão Iniciar para procurar o app pelo nome e o inicie.
