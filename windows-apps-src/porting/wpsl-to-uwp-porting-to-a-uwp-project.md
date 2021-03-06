---
description: Você pode começar o processo de portabilidade criando um novo projeto do Windows 10 no Visual Studio e copiar os arquivos nele.
title: Portabilidade de projetos do Windows Phone Silverlight para projetos da UWP
ms.assetid: d86c99c5-eb13-4e37-b000-6a657543d8f4
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c909517a9c48ba5088f52476373a6bb659bd6001
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372431"
---
# <a name="porting-windowsphone-silverlight-projects-to-uwp-projects"></a>Portabilidade de projetos do Windows Phone Silverlight para projetos da UWP


O tópico anterior era [Mapeamentos de namespace e de classe](wpsl-to-uwp-namespace-and-class-mappings.md).

Você pode começar o processo de portabilidade criando um novo projeto do Windows 10 no Visual Studio e copiar os arquivos nele.

## <a name="create-the-project-and-copy-files-to-it"></a>Crie o projeto e copie os arquivos nele

1.  Inicie o Microsoft Visual Studio 2015 e criar um novo projeto de aplicativo em branco (Universal Windows). Para obter mais informações, consulte [Jumpstart seu tempo de execução do Windows 8.x aplicativo usando modelos (C#, C++, Visual Basic)](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10)). Seu novo projeto cria um pacote do aplicativo (um arquivo appx) que será executado em todas as famílias de dispositivos.
2.  Em seu projeto de aplicativo do Windows Phone Silverlight, identifique todos os arquivos de código-fonte e arquivos de ativos visuais que você deseja reutilizar. Usando o Explorador de Arquivos, copie modelos de dados, modelos de exibição, ativos visuais, dicionários de recursos, estrutura de pastas e tudo mais que deseja reutilizar em seu novo projeto. Copie ou crie subpastas no disco conforme necessário.
3.  Copie também os modos de exibição (por exemplo, MainPage.xaml e MainPage.xaml.cs) para o nó do novo projeto. Novamente, crie novas subpastas conforme necessário e remova os modos de exibição existentes do projeto. Porém, antes de substituir ou remover um modo de exibição gerado pelo Visual Studio, mantenha uma cópia, pois ela pode ser útil para referência posterior. A primeira fase de portabilidade de um aplicativo do Windows Phone Silverlight se concentra em fazê-lo parecer BOM e funcionam bem em um dispositivo da família. Mais tarde, você voltará sua atenção para certificar-se de que os modos de exibição adaptam-se bem a todos os fatores forma e, opcionalmente, adicionar qualquer código adaptável para tirar o máximo proveito de uma família de dispositivos específica.
4.  No **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Selecione os arquivos copiados, clique neles com o botão direito do mouse e clique em **Incluir no Projeto**. Isso incluirá automaticamente suas pastas continentes. Em seguida, você pode desativar **Mostrar Todos os Arquivos**, se desejar. Um fluxo de trabalho alternativo, se preferir, é usar o comando **Adicionar Item Existente**, tendo criado qualquer subpasta necessária no **Gerenciador de Soluções** do Visual Studio. Verifique se os ativos visuais têm **Ação de Compilação** definida como **Conteúdo** e **Copiar para Diretório de Saída** definida como **Não Copiar**.
5.  As diferenças nos nomes de namespace e classe gerarão muitos erros de compilação nesse estágio. Por exemplo, se você abrir os modos de exibição gerados pelo Visual Studio, verá que eles são do tipo [**Page**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Page) e não **PhoneApplicationPage**. Existem muitas diferenças de marcação XAML e de código imperativo que os tópicos a seguir abordam em detalhes neste guia de portabilidade. Mas você fará rápido progresso apenas seguindo estas etapas gerais: altere "clr-namespace" para "using" em suas declarações de prefixo de namespace na marcação XAML; use o tópico [Mapeamentos de classes e namespaces](wpsl-to-uwp-namespace-and-class-mappings.md) e o comando **Localizar e Substituir** do Visual Studio para fazer alterações em massa em seu código-fonte (por exemplo, substituir "System.Windows" por "Windows.UI.Xaml"); e no editor de código imperativo no Visual Studio, use os comandos **Resolver** e **Organizar Usos** no menu de contexto para alterações mais direcionadas.

## <a name="extension-sdks"></a>SDKs de extensão

A maioria das APIs da Plataforma Universal do Windows (UWP) que o aplicativo portado chamará está implementada no conjunto de APIs conhecido como família de dispositivos universais. Porém, algumas estão implementadas nos SDKs de extensão, e o Visual Studio só reconhece APIs implementadas pela família de dispositivos de destino do aplicativo ou por quaisquer SDKs de extensão que você tenha consultado.

Em caso de erros de compilação sobre namespaces ou tipos ou membros que não foram encontrados, essa deve ser a provável causa. Abra o tópico da API na documentação de referência da API e navegue até a seção Requisitos; ele informará qual é a implementação da família de dispositivos. Se essa não for a família de dispositivos de destino, para disponibilizar a API para o projeto, você precisará de uma referência para o SDK de extensão dessa família de dispositivos.

Clique em **Project** &gt; **adicionar referência** &gt; **Windows Universal** &gt; **extensões** e Selecione o SDK de extensão apropriado. Por exemplo, se as APIs que você deseja chamar estão disponíveis somente na família de dispositivos móveis e elas foram introduzidas na versão 10.0.x.y, selecione **Extensões do Windows Mobile para UWP**.

Isso adicionará a seguinte referência ao seu arquivo de projeto:

```XML
<ItemGroup>
    <SDKReference Include="WindowsMobile, Version=10.0.x.y">
        <Name>Windows Mobile Extensions for the UWP</Name>
    </SDKReference>
</ItemGroup>
```

O nome e o número da versão correspondem às pastas no local instalado do SDK. Por exemplo, as informações acima correspondem a este nome de pasta:

`\Program Files (x86)\Windows Kits\10\Extension SDKs\WindowsMobile\10.0.x.y`

A menos que o aplicativo seja segmentado para a família de dispositivos que implementa a API, você precisará usar a classe [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) para testar a presença da API antes de chamá-la (isso se chama código adaptável. Essa condição então será avaliada onde quer que seu aplicativo seja executado, mas só será avaliada como "true" em dispositivos em que a API esteja presente e, portanto, disponível para ser chamada. Só use SDKs de extensão e código adaptável depois de verificar primeiramente se existe uma API universal. Alguns exemplos são fornecidos na seção abaixo.

Além disso, consulte [Manifesto do pacote do aplicativo](#the-app-package-manifest).

## <a name="maximizing-markup-and-code-reuse"></a>Maximizando a reutilização de marcação e de código

Você descobrirá que refatorar um pouco, e/ou adicionar código adaptável (que é explicado abaixo), permitirá que você maximize a marcação e o código que funcionam em todas as famílias de dispositivos. Aqui estão mais detalhes.

-   Arquivos que são comuns a todas as famílias de dispositivos não precisam de nenhuma atenção especial. Esses arquivos serão usados pelo aplicativo em todas as famílias de dispositivos em que ele for executado. Isso inclui arquivos de marcação XAML, arquivos de código-fonte imperativo e arquivos de ativos.
-   É possível que seu aplicativo detecte a família de dispositivos em que ele esteja sendo executado e navegue para um modo de exibição que foi projetado especificamente para essa família de dispositivos. Para saber mais, confira [Detectando a plataforma em que seu aplicativo é executado](wpsl-to-uwp-input-and-sensors.md).
-   Uma técnica semelhante que pode ser útil, caso não haja alternativa, é dar um nome especial a um arquivo de marcação ou arquivo **ResourceDictionary** (ou à pasta que contém o arquivo), de maneira que ele seja carregado automaticamente no tempo de execução somente quando o aplicativo é executado em uma família de dispositivos em especial. Essa técnica é ilustrada no estudo de caso [Bookstore1](wpsl-to-uwp-case-study-bookstore1.md).
-   Para usar recursos que não estejam disponíveis em todas as famílias de dispositivos (por exemplo, impressoras, scanners ou o botão da câmera), você pode escrever um código adaptável. Consulte o terceiro exemplo em [Compilação condicional e código adaptável](#conditional-compilation-and-adaptive-code) neste tópico.
-   Se você quiser dar suporte a Windows Phone Silverlight e Windows 10, em seguida, você poderá compartilhar arquivos entre projetos de código-fonte. Veja como: no Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções**, selecione **Adicionar Item Existente**, selecione os arquivos a serem compartilhados e clique em **Adicionar como Link**. Armazene seus arquivos de código-fonte em uma pasta comum no sistema de arquivos onde os projetos vinculados a eles possam vê-los, e não se esqueça de adicioná-los ao controle do código-fonte. Caso possa fatorar o código-fonte imperativo de maneira que a maior parte de um arquivo, se não todo ele, funcione em ambas as plataformas, você não precisa ter duas cópias dele. É possível encapsular qualquer lógica específica da plataforma no arquivo dentro das diretivas de compilação condicional, onde possível, ou das condições de tempo de execução, onde necessário. Consulte a próxima seção abaixo e [Diretivas de pré-processador C#](https://docs.microsoft.com/dotnet/articles/csharp/language-reference/preprocessor-directives/index).
-   Para reutilização no nível binário, em vez de no nível do código fonte, há bibliotecas de classes portáteis que suporte o subconjunto de APIs do .NET que estão disponíveis no Windows Phone Silverlight, bem como o subconjunto para aplicativos do Windows 10 (.NET Core). As assemblies da Biblioteca de Classes Portátil são compatíveis com binários nas plataformas .NET e mais. Use o Visual Studio para criar um projeto direcionado a uma Biblioteca de Classes Portátil. Consulte [Desenvolvimento entre Plataformas com a Biblioteca de Classes Portátil](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilação condicional e código adaptável

Se você quiser dar suporte a Windows Phone Silverlight e Windows 10 em um arquivo de código único, em seguida, você pode fazer isso. Se você olhar no seu projeto do Windows 10 em páginas de propriedades de projeto, você verá que o projeto define WINDOWS\_UAP como um símbolo de compilação condicional. Em geral, você pode usar a lógica a seguir para realizar uma compilação condicional.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // WINDOWS_UAP
```

Se você tiver um código que você tenha sido compartilhando entre um aplicativo do Windows Phone Silverlight e um aplicativo do Windows Runtime 8.x, você já pode ter o código-fonte com lógica como este:

```csharp
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
```

Nesse caso, e se quiser dar suporte a Windows 10 Além disso, em seguida, você pode fazer isso, muito.

```csharp
#if WINDOWS_UAP
    // Code that you want to compile into the Windows 10 app.
#else
#if NETFX_CORE
    // Code that you want to compile into the Windows Runtime 8.x app.
#else
    // Code that you want to compile into the Windows Phone Silverlight app.
#endif // NETFX_CORE
#endif // WINDOWS_UAP
```

Você pode ter usado compilação condicional para limitar a manipulação do botão Voltar do hardware para o Windows Phone. No Windows 10, o evento do botão Voltar é um conceito universal. Os botões Voltar implementados no hardware ou no software acionarão o evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested), logo, este é o evento a ser manipulado.

```csharp
       Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
            this.ViewModelLocator_BackRequested;

...

    private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
    {
        // Handle the event.
    }

```

Você pode ter usado compilação condicional para limitar a manipulação do botão da câmera do hardware para o Windows Phone. No Windows 10, o botão de câmera de hardware é um conceito específico para a família de dispositivos móveis. Como um pacote do aplicativo será executado em todos os dispositivos, mudaremos nossa condição em tempo de compilação para uma condição em tempo de execução usando o que é conhecido como código adaptável. Para isso, usamos a classe [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) para consultar, em tempo de execução, a presença da classe [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons). **HardwareButtons** é definido no SDK de extensão móvel, portanto, precisamos adicionar uma referência a esse SDK ao nosso projeto para que esse código seja compilado. Porém, o manipulador só será executado em um dispositivo que implementa os tipos definidos no SDK de extensão móvel, que é a família de dispositivos móveis. Assim, o código a seguir tem o cuidado em usar apenas recursos que estão presentes, embora consiga isso de maneira diferente da compilação condicional.

```csharp
       // Note: Cache the value instead of querying it more than once.
        bool isHardwareButtonsAPIPresent = Windows.Foundation.Metadata.ApiInformation.IsTypePresent
            ("Windows.Phone.UI.Input.HardwareButtons");

        if (isHardwareButtonsAPIPresent)
        {
            Windows.Phone.UI.Input.HardwareButtons.CameraPressed +=
                this.HardwareButtons_CameraPressed;
        }

    ...

    private void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
    {
        // Handle the event.
    }
```

Consulte também [Detectando a plataforma em que seu aplicativo está sendo executado](wpsl-to-uwp-input-and-sensors.md).

## <a name="the-app-package-manifest"></a>O manifesto do pacote do aplicativo

As configurações no projeto (inclusive todas as referências de SDKs de extensão) determinam a área da superfície da API que o aplicativo pode chamar. Porém, o manifesto do pacote do aplicativo é o que determina o conjunto real de dispositivos em que os clientes podem instalar o aplicativo da Loja. Para saber mais, veja Exemplos em [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily).

Vale a pena saber como editar o manifesto do pacote do aplicativo, pois os tópicos a seguir tratam sobre como usá-lo para diversas declarações, funcionalidades e outras configurações de que alguns recursos precisam. Você pode usar o editor de manifesto de pacote de aplicativo do Visual Studio para editá-lo. Se o **Gerenciador de Soluções** não for mostrado, escolha-o no menu **Exibir**. Clique duas vezes em **Package.appxmanifest**. Isso abre a janela do editor de manifesto. Selecione a guia apropriada para fazer alterações e salve as alterações. Certifique-se de que o elemento **pm:PhoneIdentity** no manifesto do aplicativo portado corresponda ao que está no manifesto do aplicativo que você está portando (para obter detalhes, consulte o tópico [**pm:PhoneIdentity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity)).

Ver [referência de esquema de manifesto de pacote para o Windows 10](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

O próximo tópico é [Solução de problemas](wpsl-to-uwp-troubleshooting.md).

