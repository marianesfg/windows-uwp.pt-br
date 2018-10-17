---
author: laurenhughes
title: Instale um aplicativo UWP a partir de um servidor IIS
description: Este tutorial demonstra como configurar um servidor IIS, verifique se seu aplicativo web pode hospedar pacotes de aplicativos e invocar e usar o instalador de aplicativo com eficiência.
ms.author: cdon
ms.date: 05/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, relacionados pacotes opcionais, definidos, servidor IIS
ms.localizationpriority: medium
ms.openlocfilehash: 214ddd2b55bca1acecbab0a841cf2048335e7b3a
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4750993"
---
# <a name="install-a-uwp-app-from-an-iis-server"></a>Instale um aplicativo UWP a partir de um servidor IIS

Este tutorial demonstra como configurar um servidor IIS, verifique se seu aplicativo web pode hospedar pacotes de aplicativos e invocar e usar o instalador de aplicativo com eficiência.

O aplicativo do Instalador de aplicativo permite que desenvolvedores e profissionais do setor de TI distribuam aplicativos do Windows 10 hospedando-os em sua própria Rede de disponibilização de conteúdo (CDN. Isso é útil para empresas que não desejam ou precisam publicar seus aplicativos na Microsoft Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10. 

## <a name="setup"></a>Configuração

Para passar com êxito com este tutorial, você precisará do seguinte:

1. Visual Studio 2017  
2. Ferramentas de desenvolvimento da Web e do IIS 
3. Pacote do aplicativo UWP: o pacote do aplicativo que você distribuirá

Opcional: [Projeto inicial](https://github.com/AppInstaller/MySampleWebApp) no GitHub. Isso é útil se você não tiver pacotes de aplicativos para trabalhar com, mas ainda quer aprender a usar esse recurso.

## <a name="step-1---install-iis-and-aspnet"></a>Etapa 1: instalar o IIS e ASP.NET 

[Internet Information Services](https://www.iis.net/) é um recurso do Windows que pode ser instalado por meio do menu Iniciar. No **menu Iniciar** , procure **Ativar recursos do Windows ativado ou desativado**.

Localizar e selecionar os **Serviços de informações da Internet** para instalar o IIS.

> [!NOTE]
> Você não precisa selecionar todas as caixas de seleção em serviços de informações da Internet. Apenas aqueles selecionado quando você verificar **Internet Information Services** são suficientes.

Você também precisará instalar o ASP.NET 4.5 ou maior. Para instalá-lo, localize **Internet Information Services -> World Wide Web Services -> recursos de desenvolvimento de aplicativo**. Selecione uma versão do ASP.NET que seja maior ou igual a ASP.NET 4.5.

![Instalar o ASP.NET](images/install-asp.png)

## <a name="step-2---install-visual-studio-2017-and-web-development-tools"></a>Etapa 2: instalar o Visual Studio 2017 e ferramentas de desenvolvimento da Web 

[Instalar o Visual Studio 2017](https://docs.microsoft.com/visualstudio/install/install-visual-studio) se você ainda não tiver instalado-lo. Se você já tiver o Visual Studio 2017, certifique-se de que as cargas de trabalho a seguir estão instaladas. Se as cargas de trabalho não estão presentes em sua instalação, acompanhar usando o instalador do Visual Studio (encontrada no menu Iniciar).  

Durante a instalação, selecione o **desenvolvimento da Web e ASP.NET** e outras cargas de trabalho que você está interessado. 

Depois que a instalação for concluída, inicie o Visual Studio e crie um novo projeto (**arquivo** -> **Novo projeto**).

## <a name="step-3---build-a-web-app"></a>Etapa 3: criar um aplicativo Web

Inicie o Visual Studio 2017 como **administrador** e crie um novo projeto de **Aplicativo de Web do Visual c#** com um modelo de projeto **vazio** . 

![Novo projeto](images/sample-web-app.png)

## <a name="step-4---configure-iis-with-our-web-app"></a>Etapa 4: configurar o IIS com nosso aplicativo Web 

No Gerenciador de soluções, clique com o botão direito do mouse no projeto raiz e selecione **Propriedades**.

Nas propriedades do aplicativo da web, selecione a guia **Web** . Na seção **servidores** , escolha o **IIS Local** no menu suspenso e clique em **Criar diretório Virtual**. 

![guia Web](images/web-tab.png)

## <a name="step-5---add-an-app-package-to-a-web-application"></a>Etapa 5: adicionar um pacote de aplicativo para um aplicativo web 

Adicione o pacote do aplicativo que você pretende distribuir no aplicativo da web. Você pode usar o pacote do aplicativo que faz parte dos [pacotes do projeto starter](https://github.com/AppInstaller/MySampleWebApp/tree/master/MySampleWebApp/packages) fornecido no GitHub se você não tiver um pacote do aplicativo disponível. O certificado (MySampleApp.cer) que o pacote usou também faz parte da amostra no GitHub. Você deve ter o certificado instalado em seu dispositivo antes de instalar o aplicativo (etapa 9).

No aplicativo da web de projeto inicial, uma nova pasta foi adicionada ao aplicativo web chamado `packages` que contém os pacotes de aplicativo para ser distribuído. Para criar a pasta no Visual Studio, clique com o botão direito do mouse na raiz do Gerenciador de soluções, selecione **Add** -> **Nova pasta** e nomeie-o `packages`. Para adicionar pacotes de aplicativos para a pasta, clique com botão direito do `packages` pasta e selecione **Add** -> local do pacote de**Item existente...** e navegue até o aplicativo. 

![Adicionar pacote](images/add-package.png)

## <a name="step-6---create-a-web-page"></a>Etapa 6: criar uma página da Web

Este aplicativo web de exemplo usa HTML simple. Você é livre para criar seu aplicativo web conforme exigido por suas necessidades. 

Clique com o botão direito do mouse no projeto raiz do Gerenciador de soluções, selecione **Add** -> **Novo Item**, e adicione uma nova **Página HTML** da seção **Web** .

Depois que a página HTML é criada, clique com o botão direito do mouse na página HTML no Gerenciador de soluções e selecione **Definir como página inicial**.  

Clique duas vezes no arquivo HTML para abri-lo na janela do editor de código. Neste tutorial, somente os elementos necessários na página da web para invocar o instalador de aplicativo com êxito para instalar um aplicativo do Windows 10 serão usados. 

Inclua o seguinte código HTML em sua página da web. A chave para invocar com êxito o instalador de aplicativo é usar o esquema personalizado que o instalador de aplicativo registra com o sistema operacional: `ms-appinstaller:?source=`. Consulte o exemplo de código abaixo para obter mais detalhes.

> [!NOTE]
> Certifique-se de que o caminho da URL especificado após o esquema personalizado corresponde à Url de projeto na guia web da sua solução do VS.
 
```HTML
<html>
<head>
    <meta charset="utf-8" />
    <title> Install Page </title>
</head>
<body>
    <a href="ms-appinstaller:?source=http://localhost/SampleWebApp/packages/MySampleApp.appxbundle"> Install My Sample App</a>
</body>
</html>
```

## <a name="step-7---configure-the-web-app-for-app-package-mime-types"></a>Etapa 7 - configurar o aplicativo web para tipos MIME do pacote de aplicativo

Abra o arquivo **Web. config** do Gerenciador de soluções e adicione as seguintes linhas dentro do `<configuration>` elemento. 

```xml
<system.webServer>
    <!--This is to allow the web server to serve resources with the appropriate file extension-->
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/appx" />
      <mimeMap fileExtension=".msix" mimeType="application/msix" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/appxbundle" />
      <mimeMap fileExtension=".msixbundle" mimeType="application/msixbundle" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/appinstaller" />
    </staticContent>
</system.webServer>
```

## <a name="step-8---add-loopback-exemption-for-app-installer"></a>Etapa 8 - adicionar isenção de loopback para o instalador de aplicativo

Por causa de isolamento de rede, aplicativos UWP como o instalador de aplicativo são restritos a usar os endereços IP de loopback como http://localhost/. Ao usar o servidor IIS local, o instalador de aplicativo deve ser adicionado à lista de isenção de loopback. 

Para fazer isso, abra o **Prompt de comando** como **administrador** e digite o seguinte: ' ' linha de comando CheckNetIsolation.exe LoopbackExempt - um-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

To verify that the app is added to the exempt list, use the following command to display the apps in the loopback exempt list: 
```Command Line
CheckNetIsolation.exe LoopbackExempt -s
```

Você deve encontrar `microsoft.desktopappinstaller_8wekyb3d8bbwe` na lista.

Quando a validação de local de instalação de aplicativo por meio do instalador de aplicativo for concluída, você pode remover a isenção de loopback que você adicionou nesta etapa por:

' ' Linha de comando CheckNetIsolation.exe LoopbackExempt -d-n=microsoft.desktopappinstaller_8wekyb3d8bbwe
```

## Step 9 - Run the Web App 

Build and run the web application by clicking on the run button on the VS Ribbon as shown in the image below:

![run](images/run.png)

A web page will open in your browser:

![web page](images/web-page.png)

Click on the link in the web page to launch the App Installer app and install your Windows 10 app package.


## Troubleshooting issues

### Not sufficient privilege 

If running the web app in Visual Studio displays an error such as "You do not have sufficient privilege to access IIS web sites on your machine", you will need to run Visual Studio as an administrator. Close the current instance of Visual Studio and reopen it as an admin.

### Set start page 

If running the web app causes the browser to load with an HTTP 403.14 - Forbidden error, it's because the web app doesn't have a defined start page. Refer to Step 6 in this tutorial to learn how to define a start page.
