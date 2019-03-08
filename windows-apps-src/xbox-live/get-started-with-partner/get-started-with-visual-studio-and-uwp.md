---
title: Introdução ao Visual Studio para jogos da UWP
description: Saiba como configurar um projeto do Visual Studio para habilitar o Xbox Live para um jogo UWP
ms.assetid: b53bc91f-79db-4d8f-8919-b9144e2d609b
ms.date: 11/28/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 314d5e937bb8680dc26b7dfdfa15ff90f8f26c81
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57651401"
---
# <a name="get-started-using-visual-studio-for-uwp-games"></a>Introdução ao uso do Visual Studio para jogos da UWP

## <a name="requirements"></a>Requisitos

1. Registro na  **[programa de desenvolvedores do Partner Center](https://developer.microsoft.com/store/register)**.
2. **[Windows 10](https://microsoft.com/windows)**.
3. **[Visual Studio](https://www.visualstudio.com/)**  com o **ferramentas de desenvolvimento de aplicativos do Windows Universal**. A versão mínima necessária para aplicativos UWP é Visual Studio 2015 atualização 3. É recomendável que você use a versão mais recente do Visual Studio para desenvolvedores e atualizações de segurança. 
4. **[SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk) v10.0.10586.0** ou posterior.

> [!IMPORTANT]
> Visual Studio 2017 será necessária se usando o SDK do Windows 10 versão 10.0.15063.0 (também conhecida como Creators Update) ou posterior.

## <a name="create-a-new-product-in-partner-center"></a>Criar um novo produto no Partner Center

Cada título Xbox Live deve ter um produto criado na [Partner Center](https://partner.microsoft.com/dashboard) antes de poder entrar e fazer chamadas de serviço do Xbox Live. Ver [criar um título em UDC](create-a-new-title.md) para obter mais informações.

## <a name="configuring-your-development-device"></a>Configurar seu dispositivo de desenvolvimento

As seguintes etapas de configuração preliminar são necessárias em seu dispositivo, para que você possa com êxito o logon com o Xbox Live e chame os vários serviços do Xbox Live.

### <a name="set-your-sandbox"></a>Definir sua área restrita

As áreas restritas oferecem uma maneira de manter sua [configuração do serviço Xbox Live](../xbox-live-service-configuration.md) isolado de varejo até estar pronto para liberar seu título. Alguns dados que você acumular são específicos para uma área restrita. Por exemplo digamos que seu título define um stat chamado *Headshots*, e acumular alguns números de Headshots em uma conta de usuário ao testar seu título. Esse valor seria específico para a área restrita de desenvolvimento do seu título e, se você mudou para execução na versão comercial do seu título, os headshots não seriam transferidas.

Consulte a [áreas de segurança do Xbox Live](../xbox-live-sandboxes.md) artigo para saber mais e ver como definir sua área restrita.

### <a name="sign-in-with-a-test-account"></a>Entrar com uma conta de teste

Para fazer logon em sua área restrita para desenvolvimento, você deve criar uma conta de teste ou provisionar um regular conta MSA (Microsoft) para acesso à sua área restrita. Isso fornece segurança aprimorada para os títulos no desenvolvimento, bem como alguns outros benefícios.

Para saber mais sobre contas de teste e como criá-lo, consulte [contas de teste do Xbox Live](../xbox-live-test-accounts.md)

## <a name="visual-studio-project-setup"></a>Configuração de projeto do Visual Studio

### <a name="1-open-a-uwp-project"></a>1. Abra um projeto UWP
Se você ainda não tiver um projeto existente do UWP, você pode criar uma fazendo o seguinte:

1. No Visual Studio, **arquivo** > **nova** > **projeto**.
2. No **novo projeto** caixa de diálogo, selecione o **Visual C#**   >  **Windows** > **Universal** nó no painel esquerdo e clique em **aplicativo em branco (Universal Windows)** no painel à direita.
3. Na parte inferior da caixa de diálogo, dê um nome ao projeto e especifique o local do projeto.
4. Especifique a versão de destino e a versão mínima do Windows 10 SDK. Ver [escolher uma versão UWP](https://docs.microsoft.com/windows/uwp/updates-and-versions/choose-a-uwp-version) para obter mais informações.

![criar o projeto no VS](../images/getting_started/vs-create-project.gif)

> [!NOTE]
> Xbox Live API (XSAPI) requer uma versão mínima 10.0.10586.0 ou superior.

### <a name="2-add-references-to-the-xbox-live-api-xsapi-in-your-project"></a>2. Adicione referências para o Xbox Live API (XSAPI) em seu projeto

A API de serviços do Xbox possui tipos para UWP e XDK, em C++ e o WinRT e ter seu namespace estruturado como **Microsoft.Xbox.Live.SDK.*. UWP** e **Microsoft.Xbox.Live.SDK.*. XboxOneXDK**.

1. **UWP** é para desenvolvedores que estão criando um jogo UWP, que pode ser executados no PC, o Xbox One ou Windows Phone.
2. **XboxOneXDK** destina-se ID@Xbox e desenvolvedores que estão usando um Xbox XDK gerenciados.
3. O SDK do C++ pode ser usado para os mecanismos de jogos C++, enquanto que o SDK do WinRT é para mecanismos de jogos escritos com C++, C#, ou JavaScript.
4. Ao usar o WinRT com um mecanismo de C++, você deve usar C + + c++ /CX que usa chapéus (^). C++ é a API recomendada para uso para mecanismos de jogos do C++.  

> [!TIP]
> Você pode ler mais sobre a execução de UWP no Xbox One em [Introdução ao desenvolvimento de aplicativos UWP no Xbox One](https://docs.microsoft.com/windows/uwp/xbox-apps/getting-started).

Para usar a API do Xbox Live, do seu projeto, você pode adicionar referências para os binários usando pacotes do NuGet ou adicionando a origem de API. A adição de pacotes NuGet torna a compilação mais rápida enquanto a adição da fonte torna a depuração mais rápida. Este artigo irá percorrer usando pacotes NuGet. Se você quiser usar o código-fonte, em seguida, consulte [Compilando o Xbox Live APIs fonte no seu projeto UWP](add-xbox-live-apis-source-to-a-uwp-project.md). Você pode adicionar o pacote NuGet do Xbox Live SDK por:

1. No Visual Studio, vá para **ferramentas** > **Gerenciador de pacotes NuGet** > **gerenciar pacotes NuGet para solução...** .
2. No Gerenciador de pacotes do NuGet, clique em **navegue** e insira **Xbox.Live.SDK** na caixa de pesquisa.
3. Selecione a versão do Xbox Live SDK que você deseja usar na lista à esquerda. Nesse caso, usaremos o pacote Microsoft.Xbox.Live.SDK.WinRT.UWP.
3. No lado direito da janela, marque a caixa ao lado do seu projeto e clique em **instalar**.

![Adicionar XBL por meio do NuGet](../images/getting_started/vs-add-nuget-xbl.gif)


> [!IMPORTANT]
> Para `Microsoft.Xbox.Live.SDK.Cpp.*` projetos baseados em, certifique-se de incluir o cabeçalho `#include <xsapi\services.h>` na fonte do seu projeto.

### <a name="3-optional-using-connected-storage-andor-secure-sockets"></a>3. (Opcional) Usando armazenamento conectado e/ou Secure Sockets
Dependendo da versão do SDK do Windows que você está usando, talvez você precise instalar conteúdo adicional ou adicionar manualmente referências ao seu projeto para usar o Xbox Live [armazenamento conectado](../storage-platform/connected-storage/connected-storage-technical-overview.md) ou SSL. Se você quiser usar o recurso de armazenamento conectado, você precisará acessar o `Windows.Gaming.XboxLive.Storage` namespace. Se você quiser usar SSL, você precisará acessar `Windows.Networking.XboxLive`.

#### <a name="windows-10-sdk-version-10016299-or-higher"></a>SDK do Windows 10 versão 10.0.16299 ou superior
Se o destino do SDK do Windows 10 10.0.16299 ou superior, em seguida, você será capaz de acessar o namespace de armazenamento conectado sem fazer qualquer trabalho adicional. Para acessar o Secure Sockets, você precisará adicionar uma referência a **extensões de área de trabalho do Windows para UWP**. Você pode fazer isso:

1. No **Gerenciador de soluções**, clique com botão direito no **referências** nó e escolha **adicionar referência...**
2. No lado esquerdo do **Gerenciador de referências** caixa de diálogo, selecione **Windows Universal** > **extensões**.
3. Na lista que aparece, pesquise **extensões de área de trabalho do Windows para UWP** e marque a caixa de seleção ao lado da versão que corresponde ao seu SDK do Windows 10.
4. Clique em **OK**.

![Adicionar nova referência no VS](../images/getting_started/get-started-vs-add-ref.png)

#### <a name="windows-10-sdk-version-10015063-or-lower"></a>Versão do SDK do Windows 10 10.0.15063 ou inferior
Se você quiser usar o armazenamento conectado ou Secure Sockets, você precisará instalar o SDK de extensões do Xbox Live plataformas antes de adicionar referências ao seu projeto. Você pode fazer isso:

1. Baixe e extraia o [Xbox Live extensões do SDK da plataforma](https://aka.ms/xblextsdk).
2. Após a extração, execute o arquivo MSI incluído que corresponde à versão do SDK do Windows 10 que você está usando.

Depois de ter instalado o Xbox Live plataforma extensões do SDK, você precisará adicionar uma referência a ele no Visual Studio. Você pode fazer isso:

1. No **Gerenciador de soluções**, clique com botão direito no **referências** nó e escolha **adicionar referência...**
2. No lado esquerdo do **Gerenciador de referências** caixa de diálogo, selecione **Windows Universal** > **extensões**.
3. Na lista que aparece, pesquise **extensões de área de trabalho do Windows para UWP** e marque a caixa de seleção ao lado da versão que corresponde ao seu SDK do Windows 10.
4. Clique em **OK**.

### <a name="4-associate-your-visual-studio-project-with-your-uwp-app"></a>4. Associar seu projeto do Visual Studio com seu aplicativo UWP

Para o seu jogo poder entrar, ele deve estar associado com o produto que você criou no Partner Center. Você pode associar seu jogo no Visual Studio usando o Assistente de associação de Store. No Visual Studio, faça o seguinte:

1.  Com o botão direito do mouse no projeto primário (o projeto de inicialização), clique em **Store** > **associar aplicativo com o Store...**
2.  Entrar com o **conta de desenvolvedor do Windows** usado para criar o aplicativo, se solicitado e siga os prompts.

> [!TIP]
> Ver [Empacotando aplicativos](https://docs.microsoft.com/windows/uwp/packaging/) para obter mais informações sobre como preparar seu jogo para Windows Store.

### <a name="5-add-internet-capabilities-to-your-visual-studio-project"></a>5. Adicionar recursos de Internet ao seu projeto do Visual Studio
Seu projeto de UWP será necessário especificar os recursos de internet para se comunicar com o Xbox Live. Você pode definir essas propriedades:

1. Clique duas vezes no **Package. appxmanifest** arquivo no Visual Studio para abrir o **Designer de manifesto**.
2. Clique no **capacidades** guia e verifique **Internet (cliente)**.

![Adicionar nova referência no VS](../images/getting_started/get-started-vs-add-capability.png)

### <a name="6-associate-your-visual-studio-project-with-your-xbox-live-enabled-title"></a>6. Associar o seu projeto do Visual Studio com o título do Xbox Live habilitado

Para se comunicar com o serviço Xbox Live, você precisará adicionar um arquivo de configuração de serviço ao seu projeto. Isso pode ser feito facilmente:

1. Em seu projeto de inicialização, clique com botão direito e selecione **Add** > **Novo Item**.
2. Selecione o **arquivo de texto** de tipo e nomeie-o **xboxservices.config**.
3. Clique com botão direito no arquivo, selecione **propriedades** e certifique-se de que:
    1. **Ação de Build** é definido como **conteúdo**, e  
    2. **Copiar para diretório de saída** é definido como **copiar sempre**.
5.  Edite o arquivo de configuração com o modelo a seguir, substituindo o **TitleId** e **PrimaryServiceConfigId** com os valores aplicáveis ao seu título. Você pode obter os valores corretos da página de raiz Xbox Live no Partner Center. O **PrimaryServiceConfigId** aparece no Centro de parceiros como **SCID**.

```json
    {
       "TitleId" : "your title ID (JSON number in decimal)",
       "PrimaryServiceConfigId" : "your primary service config ID"
    }
```

Por exemplo:

```json
    {
        "TitleId" : 1563044810,
        "PrimaryServiceConfigId" : "12200100-88da-4d8b-af88-e38f5d2a2bca"
    }
```

> [!TIP]
> Todos os valores dentro de xboxservices.config diferenciam maiusculas de minúsculas. Ver [configuração do serviço](../xbox-live-service-configuration.md) para obter mais informações sobre como obter o TitleID e PrimaryServiceConfigId.

### <a name="7-optional-add-multiplayer-capabilities"></a>7. (Opcional) Adicionar recursos para vários participantes

Se você planeja adicionar Multiplayer dão suporte ao seu título e deseja implementar a capacidade de jogadores e convidar outros usuários para um jogo com vários participantes, você precisará adicionar outro campo para o arquivo AppXManifest. Ver [configurar o AppXManifest para Multiplayer](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md) para obter mais informações.

## <a name="learn-more"></a>Saiba Mais

O [exemplos do SDK do Xbox Live](https://github.com/Microsoft/xbox-live-samples) são uma boa maneira de ver como as APIs do Xbox Live são usadas e apresentar as APIs disponíveis para desenvolvedores no ID@Xbox programa. Para usar os exemplos, você precisará alterar sua área restrita para XDKS.1.
