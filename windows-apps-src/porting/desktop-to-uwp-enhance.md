---
Description: Aprimore seu aplicativo da área de trabalho para usuários do Windows 10 usando o Universal Windows Platform (UWP) APIs.
Search.Product: eADQiWindows 10XVcnh
title: Aprimorar seu aplicativo da área de trabalho para Windows 10
ms.date: 10/15/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0e06de7d33604981e0055d9d57f774d280753db2
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628031"
---
# <a name="enhance-your-desktop-application-for-windows-10"></a>Aprimorar seu aplicativo da área de trabalho para Windows 10

Você pode usar APIs do Windows Runtime para adicionar experiências modernas que são aprimorados para usuários do Windows 10.

Primeiro, configure seu projeto. Em seguida, adicione as experiências do Windows 10. Você pode criar separadamente para usuários do Windows 10 ou distribuir os mesmos binários exatos para todos os usuários, independentemente da versão do Windows executada.

## <a name="first-set-up-your-project"></a>Primeiro, configure seu projeto

Você precisará fazer algumas alterações em seu projeto para usar as APIs UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificar um projeto do .NET para usar APIs do Windows Runtime

Abra a caixa de diálogo **Gerenciador de Referências**, escolha o botão **Procurar** e então selecione **Todos os Arquivos** .

![caixa de diálogo adicionar referência](images/desktop-to-uwp/browse-references.png)

Em seguida, adicione uma referência a esses arquivos.

|Arquivo|Location|
|--|--|
|System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
|Windows.Foundation.UniversalApiContract.winmd|C:\Program arquivos (x86) \Windows Kits\10\References\<*versão do sdk*> \Windows.Foundation.UniversalApiContract\<*versão*>|
|Windows.Foundation.FoundationContract.winmd|C:\Program arquivos (x86) \Windows Kits\10\References\<*versão do sdk*> \Windows.Foundation.FoundationContract\<*versão*>|

Na janela **Propriedades**, defina o campo **Cópia Local** de cada arquivo *.winmd* como **False**.

![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-project-to-use-windows-runtime-apis"></a>Modificar um projeto do C++ para usar APIs do Windows Runtime

Use [C + + c++ /CLI WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) consumir APIs do Windows Runtime. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows.

Para configurar seu projeto para C + + c++ /CLI WinRT, consulte [modificar um projeto de aplicativo de área de trabalho do Windows para adicionar o C + + c++ /CLI WinRT suporte](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/get-started#modify-a-windows-desktop-application-project-to-add-cwinrt-support).

## <a name="add-windows-10-experiences"></a>Adicionar experiências do Windows 10

Agora você está pronto para adicionar experiências modernas que acendem quando os usuários executam seu aplicativo no Windows 10. Use este fluxo de design.

:white_check_mark: **Primeiro, decida quais experiências que você deseja adicionar**

Há muitas opções. Por exemplo, você pode simplificar seu fluxo de ordem de compra por meio de monetização de APIs ou atenção direta para seu aplicativo quando você tem algo interessante para compartilhar, como uma nova imagem que outro usuário tiver sido lançada.

![Notificação do sistema](images/desktop-to-uwp/toast.png)

Mesmo que os usuários ignorem a mensagem, eles poderão vê-la novamente na central de ações e então clicar na mensagem para abrir seu app. Isso aumenta o envolvimento com seu aplicativo e tem o benefício adicional de tornar seu aplicativo aparecem está profundamente integrado com o sistema operacional. Mostraremos o código para essa experiência um pouco mais tarde.

Visite a nossa [central de desenvolvedores](https://developer.microsoft.com/windows) para ter ideias.

:white_check_mark: **Decida se deseja aprimorar ou estender**

Frequentemente, você nos ouvirá usar os termos "aprimorar" e "estender" e, portanto, vamos parar um momento para explicar exatamente o que cada um desses termos significa.

Usamos o termo "aprimorar" para descrever APIs de tempo de execução do Windows que você pode chamar diretamente do seu aplicativo da área de trabalho. Quando você tiver escolhido uma experiência do Windows 10, identifique as APIs de que você precisa para criá-la e então veja se essa API aparece nesta [lista](desktop-to-uwp-supported-api.md). Essa é uma lista de APIs que você pode chamar diretamente do seu aplicativo da área de trabalho. Se sua API não aparece nessa lista, isso ocorre porque a funcionalidade associada a essa API só pode ser executada em um processo UWP. Muitas vezes, isso inclui APIs que mostram interfaces do usuário modernas, como um controle de mapa UWP ou um prompt de segurança do Windows Hello.

Dito isso, se você quiser incluir essas experiências em seu aplicativo, basta "estender" o aplicativo com a adição de um projeto UWP à sua solução. O projeto da área de trabalho ainda é o ponto de entrada do seu aplicativo, mas o projeto UWP oferece acesso a todas as APIs que não aparecem nesta [lista](desktop-to-uwp-supported-api.md). O aplicativo da área de trabalho pode se comunicar com o processo UWP usando um serviço de app e temos muitas das diretrizes sobre como configurar isso. Se você quiser adicionar uma experiência que exija um projeto UWP, consulte [Estender com UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Contratos de API de referência**

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
    string image = "https://picsum.photos/360/180?image=104";
    string logo = "https://picsum.photos/64?image=883";

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
    Platform::String ^image = "https://picsum.photos/360/180?image=104";
    Platform::String ^logo = "https://picsum.photos/64?image=883";

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
Para saber mais sobre notificações, consulte [Notificações do sistema interativas e adaptáveis](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Dar suporte às bases de instalação do Windows XP, do Windows Vista e do Windows 7/8

Você pode modernizar seu aplicativo para Windows 10 sem precisar criar um novo branch e manter as bases de código separado.

Se você quiser compilar binários separados para usuários do Windows 10, use a compilação condicional. Se você preferir criar um conjunto de binários implantado para todos os usuários do Windows, use as verificações de tempo de execução.

Vamos dar uma rápida olhada em cada opção.

### <a name="conditional-compilation"></a>Compilação condicional

Você pode manter um código de base e compilar um conjunto de binários apenas para usuários do Windows 10.

Primeiro, adicione uma nova configuração de compilação ao seu projeto.

![Configuração de compilação](images/desktop-to-uwp/build-config.png)

Para essa configuração de compilação, crie uma constante para identificar o código que chama as APIs do Windows Runtime.  

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

Você pode compilar um conjunto de binários para todos os usuários do Windows, independentemente da versão do Windows executada. O aplicativo chamar APIs do Windows Runtime somente se o usuário for executar seu aplicativo como um aplicativo empacotado no Windows 10.

A maneira mais fácil adicionar verificações de tempo de execução ao seu código é instalar este pacote do Nuget: [Ponte de desktop auxiliares](https://www.nuget.org/packages/DesktopBridge.Helpers/) e, em seguida, use o ``IsRunningAsUWP()`` método para o portão de todo o código que chama as APIs do Windows Runtime. Consulte este blog para obter mais detalhes: [Ponte de desktop - identificar o contexto do aplicativo](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-video"></a>Vídeo relacionado

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Use-UWP-APIs-in-Your-Code-3d78c6WhD_9506218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="related-samples"></a>Exemplos relacionados

* [Exemplo Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Bloco secundário](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemplo de Store de API](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicativo de WinForms que implementa um UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Ponte de aplicativo da área de trabalho e amostras da UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
