---
Description: Aprimore seu aplicativo da área de trabalho para usuários do Windows 10 usando as APIs da UWP (Plataforma Universal do Windows).
title: Usar as APIs da UWP em aplicativos de área de trabalho
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 78d9760c5ef21b29d09babaace0f4379b6a51209
ms.sourcegitcommit: cc108c791842789464c38a10e5d596c9bd878871
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/20/2019
ms.locfileid: "75302600"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Chamar as APIs da UWP em aplicativos de área de trabalho

Você pode usar APIs da UWP (Plataforma Universal do Windows) para adicionar experiências modernas aos seus aplicativos de área de trabalho, que se destaquem para os usuários do Windows 10.

Primeiro, configure seu projeto com as referências necessárias. Em seguida, chame as APIs da UWP do seu código para adicionar experiências do Windows 10 ao aplicativo da área de trabalho. Você pode criar separadamente para usuários do Windows 10 ou distribuir os mesmos binários a todos os usuários, independentemente da versão do Windows executada.

Algumas APIs da UWP têm suporte apenas em aplicativos de área de trabalho que possuem [identificador de pacote](modernize-packaged-apps.md). Para obter mais informações, confira [APIs da UWP disponíveis](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurar o projeto

Você precisará fazer algumas alterações em seu projeto para usar as APIs da UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificar um projeto .NET para usar APIs do Windows Runtime

Há duas opções para projetos .NET:

* Caso o seu aplicativo se destine ao Windows 10 versão 1803 ou posterior, você pode instalar um pacote NuGet que forneça todas as referências necessárias.
* Como alternativa, é possível adicionar as referências manualmente.

#### <a name="to-use-the-nuget-option"></a>Para usar a opção do NuGet

1. Verifique se as [referências de pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **Ferramentas -> Gerenciador de Pacotes NuGet -> Configurações do Gerenciador de Pacotes**.
    2. Verifique se **PackageReference** está selecionado para **Formato de gerenciamento de pacotes padrão**.

2. Com o projeto aberto no Visual Studio, clique com o botão direito do mouse no seu projeto no **Gerenciador de Soluções** e escolha **Gerenciar Pacotes NuGet**.

3. Na janela do **Gerenciador de Pacotes NuGet**, selecione a guia **Procurar** e procure por `Microsoft.Windows.SDK.Contracts`.

4. Depois que o pacote `Microsoft.Windows.SDK.Contracts` for encontrado, no painel direito da janela do **Gerenciador de Pacotes NuGet**, selecione a **Versão** do pacote que você quer instalar com base na versão do Windows 10 que você deseja:

    * **10.0.18362.xxxx**: escolha essa opção para o Windows 10, versão 1903.
    * **10.0.17763.xxxx**: escolha essa opção para o Windows 10, versão 1809.
    * **10.0.17134.xxxx**: escolha essa opção para o Windows 10, versão 1803.

5. Clique em **Instalar**.

#### <a name="to-add-the-required-references-manually"></a>Para adicionar as referências necessárias manualmente

1. Abra a caixa de diálogo **Gerenciador de Referências**, escolha o botão **Procurar** e selecione **Todos os Arquivos**.

    ![caixa de diálogo adicionar referência](images/desktop-to-uwp/browse-references.png)

2. Adicione uma referência a esses arquivos.

    |Arquivo|Local|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |windows.winmd|C:\Program Files (x86)\Windows Kits\10\UnionMetadata\\<*sdk version*>\Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.UniversalApiContract\<*version*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Program Files (x86)\Windows Kits\10\References\\<*sdk version*>\Windows.Foundation.FoundationContract\<*version*>|

3. Na janela **Propriedades**, defina o campo **Cópia Local** de cada arquivo *.winmd* como **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modificar um projeto C++ Win32 para usar APIs do Windows Runtime

Use [C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) para consumir APIs do Windows Runtime. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do WinRT (Windows Runtime), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows.

Para configurar o projeto para C++/WinRT:

* Para novos projetos, você pode instalar a [Extensão do Visual Studio Extension (VSIX) para C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) e usar um dos modelos de projeto C++/WinRT incluídos nessa extensão.
* Para projetos existentes, você pode instalar o pacote NuGet [Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) no projeto.

Para obter mais detalhes sobre essas opções, confira [este artigo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Adicionar experiências do Windows 10

Agora você está pronto para adicionar experiências modernas que se destacam quando os usuários executam seu aplicativo no Windows 10. Use este fluxo de design.

:white_check_mark: **Primeiro, decida quais experiências você deseja adicionar**

Há muitas opções. Por exemplo, você pode simplificar o fluxo de ordem de compra usando [APIs de monetização](/windows/uwp/monetize) ou [atenção direta ao seu aplicativo](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) quando você tiver algo interessante para compartilhar, como uma nova foto que outro usuário postou.

![Notificação do sistema](images/desktop-to-uwp/toast.png)

Mesmo que os usuários ignorem a mensagem, eles poderão vê-la novamente na central de ações e clicar na mensagem para abrir seu aplicativo. Isso aumenta o envolvimento com seu aplicativo e tem o bônus agregado de fazer o aplicativo aparecer profundamente integrado com o sistema operacional. Mostraremos o código para essa experiência um pouco mais tarde neste artigo.

Visite a [Documentação da UWP](/windows/uwp/get-started/) para obter mais ideias.

:white_check_mark: **Decida entre aprimorar ou estender**

Frequentemente, você nos ouvirá usar os termos *aprimorar* e *estender* e, portanto, vamos parar um momento para explicar exatamente o que cada um deles significa.

Usamos o termo *aprimorar* para descrever as APIs do Windows Runtime que você pode chamar diretamente do seu aplicativo da área de trabalho (independentemente de ter escolhido ou não empacotar seu aplicativo em um pacote MSIX). Quando você tiver escolhido uma experiência do Windows 10, identifique as APIs de que precisa para criá-la e veja se essa API aparece [nesta lista](desktop-to-uwp-supported-api.md). Essa é uma lista de APIs que você pode chamar diretamente do seu aplicativo de de área de trabalho. Caso sua API não apareça nessa lista, significa que a funcionalidade associada a essa API só pode ser executada em um processo da UWP. Muitas vezes, isso inclui APIs que processam XAML UWP, como um controle de mapa UWP ou um prompt de segurança do Windows Hello.

> [!NOTE]
> Embora as APIs que processam XAML UWP normalmente não possam ser chamadas de forma direta da área de trabalho, você pode usar abordagens alternativas. Se você quiser hospedar controles XAML UWP ou outras experiências visuais personalizadas, poderá usar [Ilhas XAML](xaml-islands.md) (a partir do Windows 10, versão 1903) e a [Camada visual](visual-layer-in-desktop-apps.md) (a partir do Windows 10, versão 1803). Esses recursos podem ser usados em aplicativos da área de trabalho empacotados ou não.

Se você optou por empacotar seu aplicativo da área de trabalho em um pacote MSIX, outra opção é *estender* o aplicativo adicionando um projeto UWP à sua solução. O projeto da área de trabalho ainda é o ponto de entrada do seu aplicativo, mas o projeto UWP oferece acesso a todas as APIs que não aparecem [nesta lista](desktop-to-uwp-supported-api.md). O aplicativo da área de trabalho pode se comunicar com o processo UWP usando um serviço de aplicativo, e temos muitas diretrizes sobre como configurar isso. Se você quiser adicionar uma experiência que exija um projeto UWP, confira [Estender com componentes UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Faça referência a contratos de API**

Se você puder chamar a API diretamente do seu aplicativo da área de trabalho, abra um navegador e pesquise o tópico de referência dessa API.
Abaixo do resumo da API, você encontrará uma tabela que descreve o contrato dessa API. Confira um exemplo dessa tabela:

![Tabela de contrato de API](images/desktop-to-uwp/contract-table.png)

Se você tiver um aplicativo da área de trabalho baseado em .NET, adicione uma referência a esse contrato de API e defina a propriedade **Copiar Local** desse arquivo como **False**. Se você tiver um projeto baseado em C++, adicione aos seus **Diretórios de Inclusão Adicionais**, um caminho para a pasta que contém esse contrato.

:white_check_mark: **Chame as APIs para adicionar sua experiência**

Aqui está o código que você usaria para mostrar a janela de notificação que vimos anteriormente. Essas APIs aparecem nesta [lista](desktop-to-uwp-supported-api.md) e, portanto, você pode adicionar este código ao seu aplicativo da área de trabalho e executá-lo imediatamente.

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

Para saber mais sobre notificações, confira [Notificações do sistema interativas e adaptáveis](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts).

## <a name="support-windows-xp-windows-vista-and-windows-78-install-bases"></a>Dar suporte às bases de instalação do Windows XP, Windows Vista e Windows 7/8

Você pode modernizar seu aplicativo para Windows 10 sem precisar criar um novo branch e manter bases de código separadas.

Se você quiser compilar binários separados para usuários do Windows 10, use a compilação condicional. Se você preferir criar um conjunto de binários implantado para todos os usuários do Windows, use as verificações de runtime.

Vamos dar uma rápida olhada em cada opção.

### <a name="conditional-compilation"></a>Compilação condicional

Você pode manter um código de base e compilar um conjunto de binários apenas para usuários do Windows 10.

Primeiro, adicione uma nova configuração de build ao seu projeto.

![Configuração de build](images/desktop-to-uwp/build-config.png)

Para essa configuração de build, crie uma constante para identificar o código que chama APIs do Windows Runtime.  

Em projetos baseados em .NET, a constante é chamada de **Constante de Compilação Condicional**.

![pré-processador](images/desktop-to-uwp/compilation-constants.png)

Em projetos baseados em C++, a constante é chamada de **Definição de Pré-processador**.

![pré-processador](images/desktop-to-uwp/pre-processor.png)

Adicione essa constante antes de qualquer bloco de código UWP.

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

O compilador só compilará esse código se essa constante for definida em sua configuração de build ativa.

### <a name="runtime-checks"></a>verificações de runtime

Você pode compilar um conjunto de binários para todos os usuários do Windows, independentemente da versão do Windows executada. O aplicativo chamará as APIs do Windows Runtime apenas se o usuário o executar como um aplicativo empacotado no Windows 10.

A maneira mais fácil de adicionar verificações de runtime ao seu código é instalar este pacote NuGet: [Auxiliares da Ponte de Desktop](https://www.nuget.org/packages/DesktopBridge.Helpers/) e use o método ``IsRunningAsUWP()`` para bloquear todo o código que chama as APIs do Windows Runtime. Para obter mais informações, confira esta postagem no blog: [Ponte de Desktop – Identificar o contexto do aplicativo](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Exemplos relacionados

* [Exemplo do Hello World](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Bloco secundário](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemplo de API da Store](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicativo WinForms que implementa uma UpdateTask da UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemplos de ponte de aplicativo da área de trabalho para a UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="find-answers-to-your-questions"></a>Encontrar respostas para suas dúvidas

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
