---
author: mcleblanc
description: "Você tem duas opções ao começar o processo de portabilidade."
title: Portabilidade de um projeto do Windows Runtime 8.x para um projeto UWP
ms.assetid: 2dee149f-d81e-45e0-99a4-209a178d415a
ms.sourcegitcommit: 07058b48a527414b76d55b153359712905aa9786
ms.openlocfilehash: 60f59c58aedc56da58350a050acd184a376b8067

---

# Portabilidade de um projeto do Windows Runtime 8.x para um projeto UWP

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Você tem duas opções ao começar o processo de portabilidade. Uma é editar uma cópia dos arquivos do seu projeto existente, inclusive o manifesto do pacote do aplicativo (para essa opção, consulte as informações sobre como atualizar seus arquivos de projeto em [Migrar aplicativos para a UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/library/mt148501.aspx)). A outra opção é criar um novo projeto do Windows 10 no Visual Studio e copiar seus arquivos para ele. A primeira seção deste tópico descreve a segunda opção, mas o restante do tópico tem informações adicionais aplicáveis às duas opções. Também é possível optar por manter o novo projeto do Windows 10 na mesma solução que os projetos existentes e compartilhar arquivos de código-fonte usando-se um projeto compartilhado. Ou é possível manter o novo projeto em uma solução própria e compartilhar arquivos de código-fonte usando-se o recurso de arquivos vinculados no Visual Studio.

## Crie o projeto e copie os arquivos nele

Essas etapas focam na opção por criar um novo projeto do Windows 10 no Visual Studio e copiar seus arquivos para ele. Alguns dos detalhes relativos ao número de projetos que você cria e quais arquivos devem ser copiados dependem de fatores e decisões descritos em [Se você tiver um aplicativo Universal 8.1](w8x-to-uwp-root.md#if-you-have-an-81-universal-windows-app) e nas seções que seguem. Estas etapas supõem o caso mais simples.

1.  Inicie o Microsoft Visual Studio 2015 e crie um novo projeto Aplicativo em Branco (Windows Universal). Para obter mais informações, consulte [Iniciar rapidamente seu aplicativo da Windows Store usando modelos (C#, C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232). Seu novo projeto cria um pacote do aplicativo (um arquivo appx) que será executado em todas as famílias de dispositivos.
2.  No seu projeto de aplicativo Universal 8.1, identifique todos os arquivos de código-fonte e arquivos de ativo visual que você deseja reutilizar. Usando o Explorador de Arquivos, copie modelos de dados, modelos de exibição, ativos visuais, dicionários de recursos, estrutura de pastas e tudo mais que deseja reutilizar em seu novo projeto. Copie ou crie subpastas no disco conforme necessário.
3.  Copie também os modos de exibição (por exemplo, MainPage.xaml e MainPage.xaml.cs) para o novo projeto. Novamente, crie novas subpastas conforme necessário e remova os modos de exibição existentes do projeto. Porém, antes de substituir ou remover um modo de exibição gerado pelo Visual Studio, mantenha uma cópia, pois ela pode ser útil para referência posterior. A primeira fase da portabilidade de um aplicativo Universal 8.1 concentra-se em fazer com que ele tenha uma aparência boa e funcione bem em uma família de dispositivos. Mais tarde, você voltará sua atenção para certificar-se de que os modos de exibição adaptam-se bem a todos os fatores forma e, opcionalmente, adicionar qualquer código adaptável para tirar o máximo proveito de uma família de dispositivos específica.
4.  No **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Selecione os arquivos copiados, clique neles com o botão direito do mouse e clique em **Incluir no Projeto**. Isso incluirá automaticamente suas pastas continentes. Em seguida, você pode desativar **Mostrar Todos os Arquivos**, se desejar. Um fluxo de trabalho alternativo, se preferir, é usar o comando **Adicionar Item Existente**, tendo criado qualquer subpasta necessária no **Gerenciador de Soluções** do Visual Studio. Verifique se os ativos visuais têm **Ação de Compilação** definida como **Conteúdo** e **Copiar para Diretório de Saída** definida como **Não Copiar**.
5.  É provável que você veja alguns erros de compilação neste estágio. Porém, caso saiba o que precisa mudar, você pode usar o comando **Find and Replace** do Visual Studio para fazer alterações em massa no código-fonte; no editor de código imperativo no Visual Studio, use os comandos **Resolve** e **Organize Usings** no menu de contexto para alterações mais segmentadas.

## Maximizando a reutilização de marcação e de código

Você descobrirá que refatorar um pouco, e/ou adicionar código adaptável (que é explicado abaixo), permitirá que você maximize a marcação e o código que funcionam em todas as famílias de dispositivos. Aqui estão mais detalhes.

-   Arquivos que são comuns a todas as famílias de dispositivos não precisam de nenhuma atenção especial. Esses arquivos serão usados pelo aplicativo em todas as famílias de dispositivos em que ele for executado. Isso inclui arquivos de marcação XAML, arquivos de código-fonte imperativo e arquivos de ativos.
-   É possível que seu aplicativo detecte a família de dispositivos em que ele esteja sendo executado e navegue para um modo de exibição que foi projetado especificamente para essa família de dispositivos. Para saber mais, confira [Detectando a plataforma em que seu aplicativo é executado](w8x-to-uwp-input-and-sensors.md#detecting-the-platform).
-   Uma técnica semelhante que pode ser útil, caso não haja alternativa, é dar um nome especial a um arquivo de marcação ou arquivo **ResourceDictionary** (ou à pasta que contém o arquivo), de maneira que ele seja carregado automaticamente no tempo de execução somente quando o aplicativo é executado em uma família de dispositivos em especial. Essa técnica é ilustrada no estudo de caso [Bookstore1](w8x-to-uwp-case-study-bookstore1.md#an-optional-adjustment).
-   Você poderá remover muitas diretivas de compilação condicional no código-fonte do seu aplicativo Universal 8.1 caso precise somente de suporte para o Windows 10. Consulte [Compilação condicional e código adaptável](#reviewing-conditional-compilation) neste tópico.
-   Para usar recursos que não estejam disponíveis em todas as famílias de dispositivos (por exemplo, impressoras, scanners ou o botão da câmera), você pode escrever um código adaptável. Consulte o terceiro exemplo em [Compilação condicional e código adaptável](#reviewing-conditional-compilation) neste tópico.
-   Caso queira dar suporte ao Windows 8.1, ao Windows Phone 8.1 e ao Windows 10, você pode manter três projetos na mesma solução e compartilhar o código com um projeto compartilhado. Como alternativa, você pode compartilhar arquivos de código-fonte entre projetos. Veja como: no Visual Studio, clique com o botão direito do mouse no projeto no **Gerenciador de Soluções**, selecione **Adicionar Item Existente**, selecione os arquivos a serem compartilhados e clique em **Adicionar como Link**. Armazene seus arquivos de código-fonte em uma pasta comum no sistema de arquivos onde os projetos vinculados a eles possam vê-los. E não se esqueça de adicioná-los ao controle do código-fonte.
-   Para reutilização no nível binário, em vez do nível do código-fonte, consulte [Criando componentes do Tempo de Execução do Windows em C# e Visual Basic](http://msdn.microsoft.com/library/windows/apps/xaml/br230301.aspx). Também existem Bibliotecas de Classes Portáteis, que dão suporte ao subconjunto de APIs .NET que estão disponíveis no .NET Framework para aplicativos do Windows 8.1, do Windows Phone 8.1 e do Windows 10 (.NET Core) e no .NET Framework completo. Os assemblies da Biblioteca de Classes Portáteis são compatíveis com binários em todas essas plataformas. Use o Visual Studio para criar um projeto direcionado a uma Biblioteca de Classes Portátil. Confira [Desenvolvimento entre Plataformas com a Biblioteca de Classes Portátil](http://msdn.microsoft.com/library/gg597391.aspx).

## SDKs de Extensão

A maioria das APIs do Windows Runtime que o aplicativo universal 8.1 já chama está implementada no conjunto de APIs conhecido como a família de dispositivos universais. Porém, algumas estão implementadas nos SDKs de extensão, e o Visual Studio só reconhece APIs implementadas pela família de dispositivos de destino do aplicativo ou por quaisquer SDKs de extensão que você tenha consultado.

Em caso de erros de compilação sobre namespaces ou tipos ou membros que não foram encontrados, essa deve ser a provável causa. Abra o tópico da API na documentação de referência da API e navegue até a seção Requisitos; ele informará qual é a implementação da família de dispositivos. Se essa não for a família de dispositivos de destino, para disponibilizar a API para o projeto, você precisará de uma referência para o SDK de extensão dessa família de dispositivos.

Clique em **Projeto**&gt;**Adicionar referência**&gt;**Windows Universal**&gt;**Extensões** e selecione o SDK de extensão apropriado. Por exemplo, se as APIs que você deseja chamar estão disponíveis somente na família de dispositivos móveis e elas foram introduzidas na versão 10.0.x.y, selecione **Extensões do Windows Mobile para UWP**.

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

A menos que o aplicativo seja segmentado para a família de dispositivos que implementa a API, você precisará usar a classe [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para testar a presença da API antes de chamá-la (isso se chama código adaptável. Essa condição então será avaliada onde quer que seu aplicativo seja executado, mas só será avaliada como "true" em dispositivos em que a API esteja presente e, portanto, disponível para ser chamada. Só use SDKs de extensão e código adaptável depois de verificar primeiramente se existe uma API universal. Alguns exemplos são fornecidos na seção abaixo.

Além disso, consulte [Manifesto do pacote do aplicativo](#appxpackage).

## Compilação condicional e código adaptável

Caso esteja usando uma compilação condicional (com diretivas de pré-processador C#) de maneira que os arquivos de código funcionem no Windows 8.1 e no Windows Phone 8.1, você agora pode examinar essa compilação condicional diante do trabalho de convergência feito no Windows 10. Convergência significa que, em seu aplicativo do Windows 10, algumas condições podem ser removidas completamente. Outras mudam para verificações em tempo de execução, conforme demonstrado nos exemplos abaixo.


            **Observação**   Caso queira dar suporte ao Windows 8.1, ao Windows Phone 8.1 e ao Windows 10 em um único arquivo de código, você pode fazer isso também. Se examinar o projeto do Windows 10 nas páginas de propriedades do projeto, você verá que o projeto define WINDOWS\_UAP como um símbolo de compilação condicional. Assim, é possível usar isso em combinação com WINDOWS\_APP e WINDOWS\_PHONE\_APP. Estes exemplos mostram o caso mais simples de remoção da compilação condicional de um aplicativo Universal 8.1 e a substituição do código equivalente para um aplicativo do Windows 10.

Esse primeiro exemplo mostra o padrão de uso para a API **PickSingleFileAsync** (que se aplica somente ao Windows 8.1) e a API **PickSingleFileAndContinue** (que se aplica somente ao Windows Phone 8.1).

```csharp
#if WINDOWS_APP
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAsync
#else
    // Use Windows.Storage.Pickers.FileOpenPicker.PickSingleFileAndContinue
#endif // WINDOWS_APP
```

O Windows 10 converge na API [**PickSingleFileAsync**](https://msdn.microsoft.com/library/windows/apps/jj635275), de modo que seu código é simplificado para:

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

No Windows 10, o evento do botão Voltar é um conceito universal. Os botões Voltar implementados no hardware ou no software acionarão o evento [**BackRequested**](https://msdn.microsoft.com/library/windows/apps/dn893596), logo, este é o evento a ser manipulado.

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

No Windows 10, o botão da câmera do hardware é um conceito específico à família de dispositivos móveis. Como um pacote do aplicativo será executado em todos os dispositivos, mudaremos nossa condição em tempo de compilação para uma condição em tempo de execução usando o que é conhecido como código adaptável. Para isso, usamos a classe [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para consultar, em tempo de execução, a presença da classe [**HardwareButtons**](https://msdn.microsoft.com/library/windows/apps/jj207557). 
            **HardwareButtons** é definido no SDK de extensão móvel, portanto, precisamos adicionar uma referência a esse SDK ao nosso projeto para que esse código seja compilado. Porém, o manipulador só será executado em um dispositivo que implementa os tipos definidos no SDK de extensão móvel, que é a família de dispositivos móveis. Portanto, esse código é equivalente ao código universal 8.1 no sentido de que é cuidadoso em usar somente recursos que estão presentes, embora consiga isso de uma maneira diferente.

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

Consulte também [Detectando a plataforma em que seu aplicativo está sendo executado](w8x-to-uwp-input-and-sensors.md#detecting-the-platform).

## Manifesto do pacote do aplicativo

O tópico [O que mudou no Windows 10](https://msdn.microsoft.com/library/windows/apps/dn705793) relaciona as mudanças de referência do esquema do manifesto do pacote do Windows 10, inclusive os elementos que foram adicionados, removidos e alterados. Para obter informações de referência sobre todos os elementos, atributos e tipos do esquema, consulte [Hierarquia de elementos](https://msdn.microsoft.com/library/windows/apps/dn934819). Se você estiver portando um aplicativo da Loja do Windows Phone, assegure-se de que o elemento **pm:PhoneIdentity** no manifesto do aplicativo portado corresponda ao que está no manifesto do aplicativo que você está portando (consulte o tópico [**pm:PhoneIdentity**](https://msdn.microsoft.com/library/windows/apps/dn934763) para obter detalhes completos).

As configurações no projeto (inclusive todas as referências de SDKs de extensão) determinam a área da superfície da API que o aplicativo pode chamar. Porém, o manifesto do pacote do aplicativo é o que determina o conjunto real de dispositivos em que os clientes podem instalar o aplicativo da Loja. Para obter mais informações, consulte exemplos em [**TargetDeviceFamily**](https://msdn.microsoft.com/library/windows/apps/dn986903).

É possível editar o manifesto do pacote do aplicativo para definir diversas declarações, recursos e outras configurações de que alguns recursos precisam. Você pode usar o editor de manifesto de pacote de aplicativo do Visual Studio para editá-lo. Se o **Gerenciador de Soluções** não for mostrado, escolha-o no menu **Exibir**. Clique duas vezes em **Package.appxmanifest**. Isso abre a janela do editor de manifesto. Selecione a guia apropriada para fazer alterações e salve.

O próximo tópico é [Solução de problemas](w8x-to-uwp-troubleshooting.md).

## Tópicos relacionados

* [Desenvolver aplicativos para a Plataforma Universal do Windows](http://msdn.microsoft.com/library/dn975273.aspx)
* [Iniciar rapidamente seu aplicativo da Windows Store usando modelos (C#, C++, Visual Basic)](https://msdn.microsoft.com/library/windows/apps/hh768232)
* [Criando componentes do Tempo de Execução do Windows](https://msdn.microsoft.com/library/windows/apps/xaml/hh441572.aspx)
* [Desenvolvimento entre Plataformas com a Biblioteca de Classes Portátil](http://msdn.microsoft.com/library/gg597391.aspx)




<!--HONumber=Jun16_HO4-->


