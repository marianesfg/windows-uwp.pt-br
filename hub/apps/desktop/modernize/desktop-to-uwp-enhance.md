---
Description: Aprimore seu aplicativo de área de trabalho para usuários do Windows 10 usando APIs Plataforma Universal do Windows (UWP).
title: Usar APIs UWP em aplicativos da área de trabalho
ms.date: 08/20/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: 19H1
ms.openlocfilehash: 44ea01bbc2200c1b028ed41e7c6a2845c7a1568b
ms.sourcegitcommit: 6bb794c6e309ba543de6583d96627fbf1c177bef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/20/2019
ms.locfileid: "69643357"
---
# <a name="call-uwp-apis-in-desktop-apps"></a>Chamar APIs UWP em aplicativos da área de trabalho

Você pode usar APIs Plataforma Universal do Windows (UWP) para adicionar experiências modernas aos aplicativos de área de trabalho que acendem para os usuários do Windows 10.

Primeiro, configure seu projeto com as referências necessárias. Em seguida, chame as APIs UWP do seu código para adicionar experiências do Windows 10 ao seu aplicativo de desktop. Você pode compilar separadamente para usuários do Windows 10 ou distribuir os mesmos binários para todos os usuários, independentemente da versão do Windows que eles executam.

Algumas APIs UWP têm suporte apenas em aplicativos de área de trabalho que são empacotados em um [pacote MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root). Para obter mais informações, consulte [APIs UWP disponíveis](desktop-to-uwp-supported-api.md).

## <a name="set-up-your-project"></a>Configurar seu projeto

Você precisará fazer algumas alterações em seu projeto para usar as APIs UWP.

### <a name="modify-a-net-project-to-use-windows-runtime-apis"></a>Modificar um projeto .NET para usar Windows Runtime APIs

Há duas opções para projetos .NET:

* Se seu aplicativo for destinado ao Windows 10 versão 1803 ou posterior, você poderá instalar um pacote NuGet que fornece todas as referências necessárias.
* Como alternativa, você pode adicionar as referências manualmente.

#### <a name="to-use-the-nuget-option"></a>Para usar a opção NuGet

1. Verifique se as [referências do pacote](https://docs.microsoft.com/nuget/consume-packages/package-references-in-project-files) estão habilitadas:

    1. No Visual Studio, clique em **ferramentas-> Gerenciador de pacotes NuGet-> configurações do Gerenciador de pacotes**.
    2. Verifique se **PackageReference** está selecionado para o **formato de gerenciamento de pacotes padrão**.

2. Com o projeto aberto no Visual Studio, clique com o botão direito do mouse em seu projeto no **Gerenciador de soluções** e escolha **gerenciar pacotes NuGet**.

3. Na janela **Gerenciador de pacotes NuGet** , certifique-se de que **incluir pré-lançamento** está selecionado. Em seguida, selecione a guia **procurar** e procure `Microsoft.Windows.SDK.Contracts`.

4. Depois que `Microsoft.Windows.SDK.Contracts` o pacote for encontrado, no painel direito da janela do **Gerenciador de pacotes NuGet** , selecione a **versão** do pacote que você deseja instalar com base na versão do Windows 10 que você deseja direcionar:

    * **10.0.18362. xxxx-Preview**: Escolha esta para o Windows 10, versão 1903.
    * **10.0.17763. xxxx-Preview**: Escolha esta para o Windows 10, versão 1809.
    * **10.0.17134. xxxx-Preview**: Escolha esta para o Windows 10, versão 1803.

5. Clique em **Instalar**.

#### <a name="to-add-the-required-references-manually"></a>Para adicionar as referências necessárias manualmente

1. Abra a caixa de diálogo **Gerenciador de Referências**, escolha o botão **Procurar** e então selecione **Todos os Arquivos** .

    ![caixa de diálogo adicionar referência](images/desktop-to-uwp/browse-references.png)

2. Adicione uma referência a esses arquivos.

    |Arquivo|Location|
    |--|--|
    |System.Runtime.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.WindowsRuntime.UI.Xaml|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |System.Runtime.InteropServices.WindowsRuntime|C:\Windows\Microsoft.NET\Framework\v4.0.30319|
    |Windows. winmd|C:\Arquivos de programas (x86) \Windows\\Kits\10\UnionMetadata<*SDK versão*> \Facade|
    |Windows.Foundation.UniversalApiContract.winmd|C:\Arquivos de programas (x86) \Windows\\<Kits\10\References*SDK versão*>\<\Windows.Foundation.UniversalApiContract*versão*>|
    |Windows.Foundation.FoundationContract.winmd|C:\Arquivos de programas (x86) \Windows\\<Kits\10\References*SDK versão*>\<\Windows.Foundation.FoundationContract*versão*>|

3. Na janela **Propriedades**, defina o campo **Cópia Local** de cada arquivo *.winmd* como **False**.

    ![copy-local-field](images/desktop-to-uwp/copy-local-field.png)

### <a name="modify-a-c-win32-project-to-use-windows-runtime-apis"></a>Modificar um C++ projeto Win32 para usar Windows Runtime APIs

Use [ C++/WinRT](https://docs.microsoft.com/windows/uwp/cpp-and-winrt-apis/) para consumir APIs de Windows Runtime. C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do WinRT (Windows Runtime), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows.

Para configurar seu projeto para C++/WinRT:

* Para novos projetos, você pode instalar o [ C++VSIX (/WinRT Visual Studio Extension)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) e usar um dos modelos C++de projeto/WinRT incluídos nessa extensão.
* Para projetos existentes, você pode instalar o pacote NuGet [Microsoft. Windows. CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) no projeto.

Para obter mais detalhes sobre essas opções, consulte [Este artigo](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="add-windows-10-experiences"></a>Adicionar experiências do Windows 10

Agora você está pronto para adicionar experiências modernas que acendem quando os usuários executam seu aplicativo no Windows 10. Use este fluxo de design.

:white_check_mark: **Primeiro, decida quais experiências você deseja adicionar**

Há muitas opções. Por exemplo, você pode simplificar o fluxo de ordem de compra usando [APIs monetização](/windows/uwp/monetize)ou [direcionar a atenção para seu aplicativo](/windows/uwp/design/shell/tiles-and-notifications/adaptive-interactive-toasts) quando tiver algo interessante para compartilhar, como uma nova imagem que outro usuário tenha Postado.

![Notificação do sistema](images/desktop-to-uwp/toast.png)

Mesmo que os usuários ignorem a mensagem, eles poderão vê-la novamente na central de ações e então clicar na mensagem para abrir seu app. Isso aumenta o envolvimento com seu aplicativo e tem o bônus adicional de fazer com que seu aplicativo pareça profundamente integrado ao sistema operacional. Mostraremos o código para essa experiência mais adiante neste artigo.

Visite a [documentação do UWP](/windows/uwp/get-started/) para obter mais ideias.

:white_check_mark: **Decida se deseja aprimorar ou estender**

Em geral, você nos ouvirá usar os termos aprimorados e *estender*, portanto, demoraremos um pouco para explicar exatamente o que cada um desses termos significam.

Usamos o termo *aprimorar* para descrever Windows Runtime APIs que você pode chamar diretamente do seu aplicativo de desktop (independentemente de você ter optado por empacotar seu aplicativo em um pacote MSIX). Quando você tiver escolhido uma experiência com o Windows 10, identifique as APIs de que precisa para criá-las e, em seguida, veja se essa API aparece na [lista](desktop-to-uwp-supported-api.md). Esta é uma lista de APIs que você pode chamar diretamente do seu aplicativo de desktop. Se sua API não aparece nessa lista, isso ocorre porque a funcionalidade associada a essa API só pode ser executada em um processo UWP. Muitas vezes, elas incluem APIs que processam o XAML UWP, como um controle de mapa UWP ou um prompt de segurança do Windows Hello.

> [!NOTE]
> Embora as APIs que renderizam o XAML UWP normalmente não possam ser chamadas diretamente da sua área de trabalho, você poderá usar abordagens alternativas. Se você quiser hospedar controles XAML do UWP ou outras experiências visuais personalizadas, poderá usar as [ilhas XAML](xaml-islands.md) (a partir do Windows 10, versão 1903) e a [camada Visual](visual-layer-in-desktop-apps.md) (a partir do windows 10, versão 1803). Esses recursos podem ser usados em aplicativos de área de trabalho empacotados ou desempacotados.

Se você tiver optado por empacotar seu aplicativo de desktop em um pacote MSIX, outra opção é *estender* o aplicativo adicionando um projeto UWP à sua solução. O projeto de desktop ainda é o ponto de entrada do seu aplicativo, mas o projeto UWP fornece acesso a todas as APIs que não aparecem nessa [lista](desktop-to-uwp-supported-api.md). O aplicativo de desktop pode se comunicar com o processo UWP usando um serviço de aplicativo e temos muitas diretrizes sobre como configurá-lo. Se você quiser adicionar uma experiência que exija um projeto UWP, consulte [estender com os componentes UWP](desktop-to-uwp-extend.md).

:white_check_mark: **Fazer referência a contratos de API**

Se você puder chamar a API diretamente do seu aplicativo de área de trabalho, abra um navegador e procure o tópico de referência para essa API.
Abaixo do resumo da API, você encontrará uma tabela que descreve o contrato de API para essa API. Veja um exemplo desta tabela:

![Tabela de contrato de API](images/desktop-to-uwp/contract-table.png)

Se você tiver um aplicativo da área de trabalho baseada em .NET, adicione uma referência a esse contrato de API e então defina a propriedade **Copiar Local** desse arquivo como **False**. Se você tiver um projeto baseado em C++, adicione aos seus **Diretórios Include Adicionais**, um caminho para a pasta que contém este contrato.

:white_check_mark: **Chamar as APIs para adicionar sua experiência**

Aqui está o código que você usaria para mostrar a janela de notificação que vimos anteriormente. Essas APIs aparecem nessa [lista](desktop-to-uwp-supported-api.md) para que você possa adicionar esse código ao seu aplicativo de área de trabalho e executá-lo no momento.

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

Você pode modernizar seu aplicativo para o Windows 10 sem precisar criar uma nova ramificação e manter bases de código separadas.

Se você quiser compilar binários separados para usuários do Windows 10, use a compilação condicional. Se você preferir criar um conjunto de binários implantado para todos os usuários do Windows, use as verificações de tempo de execução.

Vamos dar uma rápida olhada em cada opção.

### <a name="conditional-compilation"></a>Compilação condicional

Você pode manter um código de base e compilar um conjunto de binários apenas para usuários do Windows 10.

Primeiro, adicione uma nova configuração de compilação ao seu projeto.

![Configuração de compilação](images/desktop-to-uwp/build-config.png)

Para essa configuração de compilação, crie uma constante para identificar o código que chama Windows Runtime APIs.  

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

Você pode compilar um conjunto de binários para todos os usuários do Windows, independentemente da versão do Windows executada. Seu aplicativo chama Windows Runtime APIs somente se o usuário executa seu aplicativo como um aplicativo empacotado no Windows 10.

A maneira mais fácil de adicionar verificações de tempo de execução ao seu código é instalar este pacote NuGet: [Auxiliares de ponte de desktop](https://www.nuget.org/packages/DesktopBridge.Helpers/) e, ``IsRunningAsUWP()`` em seguida, usam o método para retirar todo o código que chama Windows Runtime APIs. consulte esta postagem de blog para obter mais detalhes: [Ponte de desktop – identifique o contexto do aplicativo](https://blogs.msdn.microsoft.com/appconsult/2016/11/03/desktop-bridge-identify-the-applications-context/).

## <a name="related-samples"></a>Exemplos relacionados

* [Exemplo de Olá, Mundo](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/HelloWorldSample)
* [Bloco secundário](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SecondaryTileSample)
* [Exemplo de API de armazenamento](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/StoreSample)
* [Aplicativo WinForms que implementa um UpdateTask UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/WinFormsUpdateTaskSample)
* [Exemplo de ponte de aplicativo de desktop para UWP](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)

## <a name="support-and-feedback"></a>Suporte e comentários

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
