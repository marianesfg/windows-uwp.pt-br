---
author: normesta
Description: "Aprimore seu aplicativo da área de trabalho para usuários do Windows 10 usando as APIs da UWP (Plataforma Universal do Windows)."
Search.Product: eADQiWindows 10XVcnh
title: "Aprimorar seu aplicativo da área de trabalho para Windows 10"
ms.author: normesta
ms.date: 07/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 9f1522527825d6580ddf4fc36599352a9ca8a243
ms.sourcegitcommit: f93e887fbab6c1f824a8f762ba848f64c7f77c49
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2017
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Aprimorar seu aplicativo da área de trabalho para Windows 10

Você pode usar APIs UWP para adicionar experiências modernas que atraiam os usuários do Windows 10.

Primeiro, configure seu projeto. Em seguida, adicione as experiências do Windows 10. Você pode criar separadamente para usuários do Windows 10 ou distribuir os mesmos binários exatos para todos os usuários, independentemente da versão do Windows executada.

## <a name="first-set-up-your-project"></a>Primeiro, configure seu projeto

Você precisará fazer algumas alterações em seu projeto para usar as APIs UWP.

### <a name="modify-a-net-project-to-use-uwp-apis"></a>Modificar um projeto .NET para usar APIs UWP

Abra a caixa de diálogo **Gerenciador de Referências**, escolha o botão **Procurar** e então selecione **Todos os Arquivos **.

![caixa de diálogo adicionar referência](images/desktop-to-uwp/browse-references.png)

Em seguida, adicione uma referência a esses arquivos.

|Arquivo|Local|
|--|--|
|System.Runtime.WindowsRuntime|C:\Arquivos de Programas (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Arquivos de Programas (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|System.Runtime.InteropServices.WindowsRuntime|C:\Arquivos de Programas (x86)\Reference Assemblies\Microsoft\Framework\\.NETCore\v4.5|
|Windows.winmd|C:\Arquivos de Programas (x86)\Windows Kits\10\UnionMetadata\Facade|
|Windows.Foundation.UniversalApiContract.winmd|C:\Arquivos de Programas (x86)\Windows Kits\10\References\<*versão do sdk*>\Windows.Foundation.UniversalApiContract\<*versão*>|
|Windows.Foundation.FoundationContract.winmd|C:\Arquivos de Programas (x86)\Windows Kits\10\References\<*versão do sdk*>\Windows.Foundation.FoundationContract\<*versão*>|

Na janela **Propriedades**, defina o campo **Copiar Local** dos arquivos **Windows.winmd** e **Windows.Foundation.FoundationContract.winmd** como **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-uwp-apis"></a>Modificar um projeto C++ para usar APIs UWP

Abra as páginas de propriedades do projeto.

Nas configurações **Geral** do grupo de configurações **C/C++**, defina o campo **Consume Windows Runtime Extension** como **Sim(/ZW)**.

   ![Consumir a Extensão do Windows Runtime](images/desktop-to-uwp/enable-winrt-objects.png)

Abra a caixa de diálogo **Diretórios #using adicionais** e adicione esses diretórios.

* C:\Arquivos de Programas (x86)\Microsoft Visual Studio 14.0\VC\vcpackages
* C:\Arquivos de Programas (x86)\Windows Kits\10\UnionMetadata
* C:\Arquivos de Programas (x86)\Windows Kits\10\References\Windows.Foundation.UniversalApiContract\<*última versão*>
* C:\Arquivos de Programas (x86)\Windows Kits\10\References\Windows.Foundation.FoundationContract\<*latest version*>

![Diretórios using adicionais](images/desktop-to-uwp/additional-using.png)

Abra a caixa de diálogo **Diretórios Include Adicionais** e adicione este diretório: C:\Arquivos de Programas (x86)\Windows Kits\10\Include\<*última versão*>\um

![Diretórios include adicionais](images/desktop-to-uwp/additional-include.png)

Nas configurações **Geração de Código** do grupo de configurações **C/C++**, defina a configuração**Habilitar Recompilação Mínima** como **Não(/GM-)**.

![Habilitar Recompilação Mínima](images/desktop-to-uwp/disable-min-build.png)


## <a name="add-windows-10-experiences"></a>Adicionar experiências do Windows 10

Agora você está pronto para adicionar experiências modernas que acendem quando os usuários executam seu aplicativo no Windows 10. Use este fluxo de design.

:white_check_mark: **Primeiro, decida quais experiências você deseja adicionar**

Há muitas opções. Por exemplo, você pode simplificar o fluxo de ordem de compra usando APIs de monetização ou atenção direta ao seu app quando você tiver algo interessante para compartilhar, como uma nova foto que outro usuário postou.

![Notificação do sistema](images/desktop-to-uwp/toast.png)

Mesmo que os usuários ignorem a mensagem, eles poderão vê-la novamente na central de ações e então clicar na mensagem para abrir seu app. Isso aumenta o envolvimento com seu app e tem o bônus agregado de fazer seu app aparecer profundamente integrado com o sistema operacional. Mostraremos o código para essa experiência um pouco mais tarde.

Visite a nossa [central de desenvolvedores](https://developer.microsoft.com/windows) para ter ideias.

:white_check_mark: **Decida se irá aprimorar ou estender**

Frequentemente, você nos ouvirá usar os termos "aprimorar" e "estender" e, portanto, vamos parar um momento para explicar exatamente o que cada um desses termos significa.

Usamos o termo "aprimorar" para descrever as APIs UWP que você pode chamar diretamente do seu aplicativo da área de trabalho. Quando você tiver escolhido uma experiência do Windows 10, identifique as APIs de que você precisa para criá-la e então veja se essa API aparece nesta [lista](desktop-to-uwp-supported-api.md). Essa é uma lista de APIs que você pode chamar diretamente do seu aplicativo da área de trabalho. Se sua API não aparece nessa lista, isso ocorre porque a funcionalidade associada a essa API só pode ser executada em um processo UWP. Muitas vezes, isso inclui APIs que mostram interfaces do usuário modernas, como um controle de mapa UWP ou um prompt de segurança do Windows Hello.

Dito isso, se você quiser incluir essas experiências em seu aplicativo, basta "estender" o aplicativo com a adição de um projeto UWP à sua solução. O projeto da área de trabalho ainda é o ponto de entrada do seu aplicativo, mas o projeto UWP oferece acesso a todas as APIs que não aparecem nesta [lista](desktop-to-uwp-supported-api.md). O aplicativo da área de trabalho pode se comunicar com o processo UWP usando um serviço de app e temos muitas das diretrizes sobre como configurar isso. Se você quiser adicionar uma experiência que exija um projeto UWP, consulte [Estender com UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Fazer referência a contratos de API**

Se você pode chamar a API diretamente do seu aplicativo da área de trabalho, abra um navegador e procure o tópico de referência para essa API.
Abaixo do resumo da API, você encontrará uma tabela que descreve o contrato de API para essa API. Veja um exemplo desta tabela:

![Tabela de contrato de API](images/desktop-to-uwp/contract-table.png)

Se você tiver um aplicativo da área de trabalho baseada em .NET, adicione uma referência a esse contrato de API e então defina a propriedade **Copiar Local** desse arquivo como **False**. Se você tiver um projeto baseado em C++, adicione aos seus **Diretórios Include Adicionais**, um caminho para a pasta que contém este contrato.

:white_check_mark: **Chamar as APIs para adicionar sua experiência**

Aqui está o código que você usaria para mostrar a janela de notificação que vimos anteriormente. Essas APIs aparecem nesta [lista](desktop-to-uwp-supported-api.md) e, portanto, você pode adicionar este código ao seu aplicativo da área de trabalho e execute-o imediatamente.

```csharp
using Windows.Foundation;
using Windows.System;
using Windows.UI.Notifications;
using Windows.Data.Xml.Dom;
...

private void ShowToast()
{
    string title = "featured picture of the day";
    string content = "beautiful scenery";
    string image = "https://unsplash.it/360/180?image=104";
    string logo = "https://unsplash.it/64?image=883";

    string xmlString =
    $@"<toast><visual>
       <binding template='ToastGeneric'>
       <text>{title}</text>
       <text>{content}</text>
       <image src='{image}'/>
       <image src='{logo}' placement='appLogoOverride' hint-crop='circle'/>
       </binding>
      </visual></toast>";

    XmlDocument toastXml = new XmlDocument();
    toastXml.LoadXml(xmlString);

    ToastNotification toast = new ToastNotification(toastXml);

    ToastNotificationManager.CreateToastNotifier().Show(toast);
}
```

```C++
using namespace Windows::Foundation;
using namespace Windows::System;
using namespace Windows::UI::Notifications;
using namespace Windows::Data::Xml::Dom;

void UWP::ShowToast()
{
    Platform::String ^title = "featured picture of the day";
    Platform::String ^content = "beautiful scenery";
    Platform::String ^image = "https://unsplash.it/360/180?image=104";
    Platform::String ^logo = "https://unsplash.it/64?image=883";

    Platform::String ^xmlString =
        L"<toast><visual><binding template='ToastGeneric'>" +
        L"<text>" + title + "</text>" +
        L"<text>"+ content + "</text>" +
        L"<image src='" + image + "'/>" +
        L"<image src='" + logo + "'" +
        L" placement='appLogoOverride' hint-crop='circle'/>" +
        L"</binding></visual></toast>";

    XmlDocument ^toastXml = ref new XmlDocument();

    toastXml->LoadXml(xmlString);

    ToastNotificationManager::CreateToastNotifier()->Show(ref new ToastNotification(toastXml));
}
```
Para saber mais sobre notificações, consulte [Notificações do sistema interativas e adaptáveis](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Dar suporte às bases de instalação do Windows XP, do Windows Vista e do Windows 7/8

Você pode modernizar seu app para Windows 10 sem precisar criar um novo branch e manter bases de código separadas.

Se você quiser compilar binários separados para usuários do Windows 10, use a compilação condicional. Se você preferir criar um conjunto de binários implantado para todos os usuários do Windows, use as verificações de tempo de execução.

Vamos dar uma rápida olhada em cada opção.

### <a name="conditional-compilation"></a>Compilação condicional

Você pode manter um código de base e compilar um conjunto de binários apenas para usuários do Windows 10.

Primeiro, adicione uma nova configuração de compilação ao seu projeto.

![Configuração de compilação](images/desktop-to-uwp/build-config.png)

Para essa configuração de compilação, crie uma constante para identificar o código que chama as APIs UWP.  

Para projetos baseados em .NET, a constante é chamada uma **Constante de Compilação Condicional**.

![pré-processador](images/desktop-to-uwp/compilation-constants.png)

Para projetos baseados em C++, a constante é chamada de uma **Definição de Pré-processador**.

![pré-processador](images/desktop-to-uwp/pre-processor.png)

Adicione esse constante antes de qualquer bloco de código UWP.

```csharp

[System.Diagnostics.Conditional("_UWP")]
private void ShowToast()
{
 ...
}

```

```C++

#if _UWP
void UWP::ShowToast()
{
 ...
}
#endif

```

O compilador só cria esse código se essa constante for definida em sua configuração de compilação ativa.

### <a name="runtime-checks"></a>Verificações de tempo de execução

Você pode compilar um conjunto de binários para todos os usuários do Windows, independentemente da versão do Windows executada. Seu app só chama APIs UWP se o usuário executa seu app como um app empacotado no Windows 10.

A maneira mais fácil de adicionar verificações de tempo de execução ao seu código é instalar esse pacote Nuget: [Auxiliares de Ponte de Desktop](https://www.nuget.org/packages/DesktopBridge.Helpers/) e então use o método ``IsRunningAsUWP()`` como uma ponte de todo o código UWP. consulte esta postagem de blog para obter mais detalhes: [Ponte de Desktop - identificar o contexto do aplicativo](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Vídeo relacionado

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Exemplos relacionados

* [Exemplo do Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Bloco Secundário](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemplo de API da Loja](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [App WinForms que implementa uma UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemplos de ponte de aplicativo de desktop para UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para dúvidas específicas**

Nossa equipe monitora estas [marcas do StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial)

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.