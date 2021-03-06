---
description: Você tem duas opções ao começar o processo de portabilidade.
title: Portabilidade de um projeto do Windows Runtime 8.x para um projeto UWP
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 1d398d1cb37bb57e39fc9202af36970aba3d8302
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393621"
---
# <a name="porting-a-windows-runtime-8x-project-to-a-uwp-project"></a>Portabilidade de um projeto do Windows Runtime 8.x para um projeto UWP



Você tem duas opções ao começar o processo de portabilidade. Uma é editar uma cópia dos arquivos do seu projeto existente, inclusive o manifesto do pacote do aplicativo (para essa opção, consulte as informações sobre como atualizar seus arquivos de projeto em [Migrar aplicativos para a UWP (Plataforma Universal do Windows)](https://docs.microsoft.com/visualstudio/misc/migrate-apps-to-the-universal-windows-platform-uwp?view=vs-2015)). A outra opção é criar um novo projeto do Windows 10 no Visual Studio e copiar os arquivos nele. A primeira seção deste tópico descreve a segunda opção, mas o restante do tópico tem informações adicionais aplicáveis às duas opções. Você também pode optar por manter seu novo projeto do Windows 10 na mesma solução que seus projetos existentes e compartilhar arquivos de código-fonte usando um projeto compartilhado. Ou é possível manter o novo projeto em uma solução própria e compartilhar arquivos de código-fonte usando-se o recurso de arquivos vinculados no Visual Studio.

## <a name="create-the-project-and-copy-files-to-it"></a>Crie o projeto e copie os arquivos nele

Estas etapas se concentram na opção de criar um novo projeto do Windows 10 no Visual Studio e copiar seus arquivos nele. Alguns dos detalhes relativos ao número de projetos que você cria e quais arquivos devem ser copiados dependem de fatores e decisões descritos em [Se você tiver um aplicativo Universal 8.1](w8x-to-uwp-root.md) e nas seções que seguem. Estas etapas supõem o caso mais simples.

1.  Inicie o Microsoft Visual Studio 2015 e crie um novo projeto de aplicativo em branco (Windows universal). Para obter mais informações, consulte iniciar [seu aplicativo Windows Runtime 8. x usando modelosC#( C++,, Visual Basic)](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10)). Seu novo projeto cria um pacote do aplicativo (um arquivo appx) que será executado em todas as famílias de dispositivos.
2.  No seu projeto de aplicativo Universal 8.1, identifique todos os arquivos de código-fonte e arquivos de ativo visual que você deseja reutilizar. Usando o Explorador de Arquivos, copie modelos de dados, modelos de exibição, ativos visuais, dicionários de recursos, estrutura de pastas e tudo mais que deseja reutilizar em seu novo projeto. Copie ou crie subpastas no disco conforme necessário.
3.  Copie também os modos de exibição (por exemplo, MainPage.xaml e MainPage.xaml.cs) para o novo projeto. Novamente, crie novas subpastas conforme necessário e remova os modos de exibição existentes do projeto. Porém, antes de substituir ou remover um modo de exibição gerado pelo Visual Studio, mantenha uma cópia, pois ela pode ser útil para referência posterior. A primeira fase da portabilidade de um aplicativo Universal 8.1 concentra-se em fazer com que ele tenha uma aparência boa e funcione bem em uma família de dispositivos. Mais tarde, você voltará sua atenção para certificar-se de que os modos de exibição adaptam-se bem a todos os fatores forma e, opcionalmente, adicionar qualquer código adaptável para tirar o máximo proveito de uma família de dispositivos específica.
4.  No **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Selecione os arquivos copiados, clique neles com o botão direito do mouse e clique em **Incluir no Projeto**. Isso incluirá automaticamente suas pastas continentes. Em seguida, você pode desativar **Mostrar Todos os Arquivos**, se desejar. Um fluxo de trabalho alternativo, se preferir, é usar o comando **Adicionar Item Existente**, tendo criado qualquer subpasta necessária no **Gerenciador de Soluções** do Visual Studio. Verifique se os ativos visuais têm **Ação de Compilação** definida como **Conteúdo** e **Copiar para Diretório de Saída** definida como **Não Copiar**.
5.  É provável que você veja alguns erros de compilação neste estágio. Porém, caso saiba o que precisa mudar, você pode usar o comando **Find and Replace** do Visual Studio para fazer alterações em massa no código-fonte; no editor de código imperativo no Visual Studio, use os comandos **Resolve** e **Organize Usings** no menu de contexto para alterações mais segmentadas.

## <a name="maximizing-markup-and-code-reuse"></a>Maximizando a reutilização de marcação e de código

Você descobrirá que refatorar um pouco, e/ou adicionar código adaptável (que é explicado abaixo), permitirá que você maximize a marcação e o código que funcionam em todas as famílias de dispositivos. Aqui estão mais detalhes.

-   Arquivos que são comuns a todas as famílias de dispositivos não precisam de nenhuma atenção especial. Esses arquivos serão usados pelo aplicativo em todas as famílias de dispositivos em que ele for executado. Isso inclui arquivos de marcação XAML, arquivos de código-fonte imperativo e arquivos de ativos.
-   É possível que seu aplicativo detecte a família de dispositivos em que ele esteja sendo executado e navegue para um modo de exibição que foi projetado especificamente para essa família de dispositivos. Para saber mais, confira [Detectando a plataforma em que seu aplicativo é executado](w8x-to-uwp-input-and-sensors.md).
-   Uma técnica semelhante que pode ser útil, caso não haja alternativa, é dar um nome especial a um arquivo de marcação ou arquivo **ResourceDictionary** (ou à pasta que contém o arquivo), de maneira que ele seja carregado automaticamente no runtime somente quando o aplicativo é executado em uma família de dispositivos em especial. Essa técnica é ilustrada no estudo de caso [Bookstore1](w8x-to-uwp-case-study-bookstore1.md).
-   Você deve ser capaz de remover muitas das diretivas de compilação condicional no código-fonte do seu aplicativo universal 8,1 se precisar apenas oferecer suporte ao Windows 10. Consulte [Compilação condicional e código adaptável](#conditional-compilation-and-adaptive-code) neste tópico.
-   Para usar recursos que não estejam disponíveis em todas as famílias de dispositivos (por exemplo, impressoras, scanners ou o botão da câmera), você pode escrever um código adaptável. Consulte o terceiro exemplo em [Compilação condicional e código adaptável](#conditional-compilation-and-adaptive-code) neste tópico.
-   Se você quiser dar suporte a Windows 8.1, Windows Phone 8,1 e Windows 10, poderá manter três projetos na mesma solução e compartilhar o código com um projeto compartilhado. Como alternativa, você pode compartilhar arquivos de código-fonte entre projetos. Veja como: no Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções**, selecione **Adicionar Item Existente**, selecione os arquivos a serem compartilhados e clique em **Adicionar como Link**. Armazene seus arquivos de código-fonte em uma pasta comum no sistema de arquivos onde os projetos vinculados a eles possam vê-los. E não se esqueça de adicioná-los ao controle do código-fonte.
-   Para reutilização no nível binário, em vez do nível de código-fonte, consulte [criando Windows Runtime C# componentes no e Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/br230301(v=vs.140)). Também há bibliotecas de classes portáteis, que dão suporte ao subconjunto de APIs do .NET que estão disponíveis na .NET Framework para Windows 8.1, Windows Phone 8,1 e aplicativos do Windows 10 (.NET Core) e o .NET Framework completo. Os assemblies da Biblioteca de Classes Portáteis são compatíveis com binários em todas essas plataformas. Use o Visual Studio para criar um projeto direcionado a uma Biblioteca de Classes Portátil. Consulte [Desenvolvimento entre Plataformas com a Biblioteca de Classes Portátil](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library).

## <a name="extension-sdks"></a>SDKs de extensão

A maioria das APIs do Windows Runtime que o aplicativo universal 8.1 já chama está implementada no conjunto de APIs conhecido como a família de dispositivos universais. Porém, algumas estão implementadas nos SDKs de extensão, e o Visual Studio só reconhece APIs implementadas pela família de dispositivos de destino do aplicativo ou por quaisquer SDKs de extensão que você tenha consultado.

Em caso de erros de compilação sobre namespaces ou tipos ou membros que não foram encontrados, essa deve ser a provável causa. Abra o tópico da API na documentação de referência da API e navegue até a seção Requisitos; ele informará qual é a implementação da família de dispositivos. Se essa não for a família de dispositivos de destino, para disponibilizar a API para o projeto, você precisará de uma referência para o SDK de extensão dessa família de dispositivos.

Clique em **projeto** &gt; **Adicionar referência** &gt; **extensões** de &gt; **Universal do Windows** e selecione o SDK de extensão apropriado. Por exemplo, se as APIs que você deseja chamar estão disponíveis somente na família de dispositivos móveis e elas foram introduzidas na versão 10.0.x.y, selecione **Extensões do Windows Mobile para UWP**.

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

Além disso, consulte [Manifesto do pacote do aplicativo](#app-package-manifest).

## <a name="conditional-compilation-and-adaptive-code"></a>Compilação condicional e código adaptável

Se você estiver usando a compilação condicional ( C# com diretivas de pré-processador) para que os arquivos de código funcionem tanto no Windows 8.1 quanto no Windows Phone 8,1, agora você poderá examinar essa compilação condicional em claro o trabalho de convergência feito no Windows 10. Convergência significa que, em seu aplicativo do Windows 10, algumas condições podem ser completamente removidas. Outras mudam para verificações em tempo de execução, conforme demonstrado nos exemplos abaixo.

**Observação**   se você quiser dar suporte a Windows 8.1, Windows Phone 8,1 e Windows 10 em um único arquivo de código, poderá fazer isso também. Se você examinar seu projeto do Windows 10 nas páginas de propriedades do projeto, verá que o projeto define o WINDOWS\_UAP como um símbolo de compilação condicional. Portanto, você pode usá-lo em combinação com o aplicativo WINDOWS\_e o aplicativo WINDOWS\_PHONE\_. Esses exemplos mostram o caso mais simples de remover a compilação condicional de um aplicativo universal 8,1 e substituir o código equivalente para um aplicativo do Windows 10.

Esse primeiro exemplo mostra o padrão de uso para a API **PickSingleFileAsync** (que se aplica somente ao Windows 8.1) e a API **PickSingleFileAndContinue** (que se aplica somente ao Windows Phone 8.1).

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

O Windows 10 convergi na API [**PickSingleFileAsync**](https://docs.microsoft.com/uwp/api/windows.storage.pickers.fileopenpicker.picksinglefileasync) , para que seu código simplifique-se a isso:

```csharp
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
```

Neste exemplo, lidamos com o botão Voltar do hardware, mas somente no Windows Phone.

```csharp
#if WINDOWS_PHONE_APP
        Windows.Phone.UI.Input.HardwareButtons.BackPressed += this.HardwareButtons_BackPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
    void HardwareButtons_BackPressed(object sender, Windows.Phone.UI.Input.BackPressedEventArgs e)
    {
        // Handle the event.
    }
#endif // WINDOWS_PHONE_APP
```

No Windows 10, o evento de botão voltar é um conceito universal. Os botões Voltar implementados no hardware ou no software acionarão o evento [**BackRequested**](https://docs.microsoft.com/uwp/api/windows.ui.core.systemnavigationmanager.backrequested), logo, este é o evento a ser manipulado.

```csharp
    Windows.UI.Core.SystemNavigationManager.GetForCurrentView().BackRequested +=
        this.ViewModelLocator_BackRequested;

...

private void ViewModelLocator_BackRequested(object sender, Windows.UI.Core.BackRequestedEventArgs e)
{
    // Handle the event.
}
```

Este exemplo final é semelhante ao anterior. Aqui, manipulamos o botão da câmera do hardware, mas, novamente, somente no código compilado no pacote do aplicativo do Windows Phone.

```csharp
#if WINDOWS_PHONE_APP
    Windows.Phone.UI.Input.HardwareButtons.CameraPressed += this.HardwareButtons_CameraPressed;
#endif // WINDOWS_PHONE_APP

...

#if WINDOWS_PHONE_APP
void HardwareButtons_CameraPressed(object sender, Windows.Phone.UI.Input.CameraEventArgs e)
{
    // Handle the event.
}
#endif // WINDOWS_PHONE_APP
```

No Windows 10, o botão de câmera de hardware é um conceito específico da família de dispositivos móveis. Como um pacote do aplicativo será executado em todos os dispositivos, mudaremos nossa condição em tempo de compilação para uma condição em tempo de execução usando o que é conhecido como código adaptável. Para isso, usamos a classe [**ApiInformation**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Metadata.ApiInformation) para consultar, em tempo de execução, a presença da classe [**HardwareButtons**](https://docs.microsoft.com/uwp/api/Windows.Phone.UI.Input.HardwareButtons). **HardwareButtons** é definido no SDK de extensão móvel, portanto, precisamos adicionar uma referência a esse SDK ao nosso projeto para que esse código seja compilado. Porém, o manipulador só será executado em um dispositivo que implementa os tipos definidos no SDK de extensão móvel, que é a família de dispositivos móveis. Portanto, esse código é equivalente ao código universal 8.1 no sentido de que é cuidadoso em usar somente recursos que estão presentes, embora consiga isso de uma maneira diferente.

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

Consulte também [Detectando a plataforma em que seu aplicativo está sendo executado](w8x-to-uwp-input-and-sensors.md).

## <a name="app-package-manifest"></a>Manifesto do pacote do aplicativo

O tópico [o que mudou no Windows 10](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/what-s-changed-in-windows-10) lista as alterações na referência do esquema de manifesto do pacote para Windows 10, incluindo elementos que foram adicionados, removidos e alterados. Para obter informações de referência sobre todos os elementos, atributos e tipos do esquema, consulte [Hierarquia de elementos](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/root-elements). Se você estiver portando um aplicativo da loja do Windows Phone, ou se seu aplicativo é uma atualização para um aplicativo da Windows Phone Store, certifique-se de que o elemento **PM: phoneidentity** corresponda ao que está no manifesto do aplicativo do seu aplicativo anterior (use os mesmos GUIDs que foram atribuídos ao aplicativo pela loja). Isso garante que os usuários do seu aplicativo que estiverem atualizando para o Windows 10 receberão seu novo aplicativo como uma atualização, não uma duplicação. Consulte o tópico [**PM: phoneidentity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-pm-phoneidentity) de referência para obter mais detalhes.

As configurações no projeto (inclusive todas as referências de SDKs de extensão) determinam a área da superfície da API que o aplicativo pode chamar. Porém, o manifesto do pacote do aplicativo é o que determina o conjunto real de dispositivos em que os clientes podem instalar o aplicativo da Loja. Para obter mais informações, consulte exemplos em [**TargetDeviceFamily**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-targetdevicefamily).

É possível editar o manifesto do pacote do aplicativo para definir diversas declarações, recursos e outras configurações de que alguns recursos precisam. Você pode usar o editor de manifesto de pacote de aplicativo do Visual Studio para editá-lo. Se o **Gerenciador de Soluções** não for mostrado, escolha-o no menu **Exibir**. Clique duas vezes em **Package.appxmanifest**. Isso abre a janela do editor de manifesto. Selecione a guia apropriada para fazer alterações e salve.

O próximo tópico é [Solução de problemas](w8x-to-uwp-troubleshooting.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Desenvolva aplicativos para o Plataforma Universal do Windows](https://docs.microsoft.com/visualstudio/cross-platform/develop-apps-for-the-universal-windows-platform-uwp?view=vs-2015)
* [Inicie seu aplicativo Windows Runtime 8. x usando modelos (C#, C++, Visual Basic)](https://docs.microsoft.com/previous-versions/windows/apps/hh768232(v=win.10))
* [Criando componentes de Windows Runtime](https://docs.microsoft.com/previous-versions/windows/apps/hh441572(v=vs.140))
* [Desenvolvimento de plataforma cruzada com a biblioteca de classes portátil](https://docs.microsoft.com/dotnet/standard/cross-platform/cross-platform-development-with-the-portable-class-library)

