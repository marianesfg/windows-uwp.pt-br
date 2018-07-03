---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: Saiba como habilitar compras no aplicativo e avaliações em aplicativos UWP.
title: Compras no aplicativo e avaliações
ms.author: mcleans
ms.date: 05/09/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, compras no aplicativo, IAPs, complementos, avaliações, consumível, durável, assinatura
ms.localizationpriority: medium
ms.openlocfilehash: 7b5b889dfd1dae69cbe4234bf0606127c1190522
ms.sourcegitcommit: dc3389ef2e2c94b324872a086877314d6f963358
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/11/2018
ms.locfileid: "1874294"
---
# <a name="in-app-purchases-and-trials"></a>Compras e avaliações no aplicativo

O SDK do Windows fornece APIs que você pode usar para implementar os seguintes recursos para ganhar mais dinheiro com seu aplicativo da Plataforma Universal do Windows (UWP):

* **Compras no aplicativo**&nbsp;&nbsp;Seja seu aplicativo gratuito ou não, você pode vender conteúdo ou uma nova funcionalidade do aplicativo (como o desbloqueio do próximo nível de um jogo) no próprio aplicativo.

* **Funcionalidade de avaliação**&nbsp;&nbsp;Se você configurar seu aplicativo como [avaliação gratuita no painel do Centro de Desenvolvimento do Windows](../publish/set-app-pricing-and-availability.md#free-trial), incentive seus clientes a comprar a versão completa do aplicativo, excluindo ou limitando alguns recursos durante o período de avaliação. Você também pode habilitar recursos, como faixas ou marcas-d'água, que são mostrados apenas durante a avaliação, antes de o cliente comprar o aplicativo.

Este artigo fornece uma visão geral de como as compras no aplicativo e as avaliações funcionam em aplicativos UWP.

<span id="choose-namespace" />

## <a name="choose-which-namespace-to-use"></a>Escolha qual namespace usar

Há dois namespaces diferentes que você pode usar para adicionar compras no aplicativo e funcionalidade de avaliação aos seus aplicativos UWP, dependendo da versão do Windows 10 à qual seus aplicativos se destinam. Embora as APIs nesses namespaces tenha os mesmos objetivos, elas foram criadas de forma bem diferente, e o código não é compatível entre as duas APIs.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;A partir do Windows 10, versão 1607, aplicativos podem usar a API nesse namespace para implementar compras no aplicativo e avaliações. Recomendamos que você use os membros nesse namespace se o projeto do aplicativo for voltado para o **Windows 10 Anniversary Edition (10.0, Compilação 14393)** ou uma versão posterior no Visual Studio. Esse namespace dá suporte aos tipos de complemento mais recentes, como complementos consumíveis gerenciados pela Store, e foi projetado para ser compatível com tipos de produtos e recursos futuros compatíveis com o Centro de Desenvolvimento do Windows e a Store. Para obter mais informações sobre esse namespace, consulte a seção [Compras no aplicativo e avaliações que usam o namespace Windows.Services.Store](#api_intro) neste artigo.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;Todas as versões do Windows 10 também oferecem suporte a uma API mais antiga para compras no aplicativo e avaliações nesse namespace. Para obter informações sobre o namespace **Windows.ApplicationModel.Store**, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

> [!IMPORTANT]
> O namespace **Windows.ApplicationModel.Store** não está sendo atualizado com os novos recursos e recomendamos que você use o namespace **Windows.Services.Store** em vez disso, se possível para seu app. O namespace **Windows.ApplicationModel.Store** não tem suporte em aplicativos da área de trabalho do Windows que usam a [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop) ou em apps ou jogos que usam uma área restrita de desenvolvimento no Centro de Desenvolvimento (por exemplo, esse é o caso para qualquer jogo que se integre ao Xbox Live).

<span id="concepts" />

## <a name="basic-concepts"></a>Conceitos básicos

Cada item que é oferecido na Store é geralmente chamado de *produto*. A maioria dos desenvolvedores só trabalha com os seguintes tipos de produtos: *apps* e *complementos*.

Um complemento é um produto ou recurso que você disponibiliza para os clientes no contexto do aplicativo: por exemplo, a moeda a ser usada em um aplicativo ou jogo, novos mapas ou armas para um jogo, a possibilidade de usar o aplicativo sem anúncios ou conteúdo digital, como músicas ou vídeos, para aplicativos que tenham a possibilidade de oferecer esse tipo de conteúdo. Todos os aplicativos e complementos têm uma licença associada que indica se o usuário tem direito de usar o aplicativo ou complemento. Se o usuário tiver direito de usar o aplicativo ou complemento como avaliação, a licença também fornece informações adicionais sobre a avaliação.

Para oferecer um complemento para clientes em seu aplicativo, você deve [definir o complemento para o aplicativo no painel do Centro de Desenvolvimento](../publish/add-on-submissions.md) para que a Store saiba disso. Em seguida, seu aplicativo pode usar APIs no namespace **Windows.Services.Store** ou **Windows.ApplicationModel.Store** para oferecer o complemento para venda ao usuário como uma compra no aplicativo.

Aplicativos UWP podem oferecer os seguintes tipos de complementos.

| Tipo de complemento |  Descrição  |
|---------|-------------------|
| Durável  |  Um complemento que dura o tempo de vida útil que você especificar no [painel do Centro de Desenvolvimento do Windows](../publish/enter-iap-properties.md). <p/><p/>Por padrão, os complementos duráveis nunca expiram, podendo ser adquiridos somente uma vez. Se você especificar uma duração específica para o complemento, o usuário poderá comprar novamente o complemento depois que ele expirar. |
| Consumível gerenciado pelo desenvolvedor  |  Um complemento que pode ser comprado, usado e comprado novamente após o consumo. Você é responsável por controlar o saldo de itens do usuário que o complemento representa.<p/><p/>Quando o usuário consome qualquer item associado ao complemento, você é responsável pela manutenção do saldo do usuário e por relatar a compra do complemento como providenciada para a Store depois que o usuário consome todos os itens. O usuário não pode comprar o complemento novamente até que seu aplicativo tenha informado a compra anterior do complemento como providenciada. <p/><p/>Por exemplo, se o seu complemento representar 100 moedas em um jogo e o usuário consumir 10 moedas, seu aplicativo ou o serviço deverá manter o novo saldo restante de 90 moedas para o usuário. Depois que o usuário tiver consumido todas as 100 moedas, seu aplicativo deverá declarar o complemento como providenciado e, em seguida, o usuário poderá comprar o complemento de 100 moedas novamente.    |
| Consumível gerenciado pela Store  |  Um complemento que pode ser comprado, usado e comprado a qualquer momento. A Store mantém o controle do saldo de itens do usuário que o complemento representa.<p/><p/>Quando o usuário consome todos os itens associados ao complemento, você é responsável por relatar esses itens como providenciados para a Store, e esta atualiza o saldo do usuário. O usuário pode adquirir o complemento quantas vezes desejar (não é necessário consumir os itens primeiro). Seu aplicativo pode consultar o saldo atual para o usuário a qualquer momento. <p/><p/> Por exemplo, se o complemento representar uma quantidade inicial de 100 moedas em um jogo e o usuário consumir 50 moedas, o aplicativo relatará para a Store que 50 unidades do complemento foram providenciadas, e a Store atualizará o saldo restante. Se o usuário comprar o complemento novamente para adquirir mais 100 moedas, ele agora terá 150 moedas no total. <p/><p/>**Observação**&nbsp;&nbsp;Para usar consumíveis gerenciados pela Microsoft Store, o aplicativo deve ser direcionado ao **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio, e deve usar o namespace **Windows.Services.Store** em vez do namespace **Windows.ApplicationModel.Store**.  |
| Assinatura | Um complemento durável em que o cliente continua a ser cobrado em intervalos recorrentes para continuar usando o complemento. O cliente pode cancelar a assinatura a qualquer momento para evitar cobranças futuras. <p/><p/>**Observação**&nbsp;&nbsp;Para usar complementos de assinatura, o aplicativo deve ser direcionado ao **Windows 10 Anniversary Edition (10.0; Build 14393)** ou uma versão posterior no Visual Studio, e deve usar o namespace **Windows.Services.Store** em vez do namespace **Windows.ApplicationModel.Store**.  |

<span />

> [!NOTE]
> Outros tipos de complementos, como complementos duráveis com pacotes (também conhecidos como conteúdo baixável ou DLC), estão disponíveis apenas para um conjunto restrito de desenvolvedores e não são abordados nesta documentação.

<span id="api_intro" />

## <a name="in-app-purchases-and-trials-using-the-windowsservicesstore-namespace"></a>Compras no aplicativo e avaliações usando o namespace Windows.Services.Store

Esta seção fornece uma visão geral das tarefas e dos conceitos importantes para o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Esse namespace está disponível somente para aplicativos destinados ao **Windows 10 Anniversary Edition (10.0; Compilação 14393)** ou uma versão posterior no Visual Studio (isso corresponde ao Windows 10, versão 1607). Recomendamos que os aplicativos usem o namespace **Windows.Services.Store** em vez do namespace [Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx), se possível. Para obter informações sobre o namespace **Windows.ApplicationModel.Store**, consulte [este artigo](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

**Nesta seção**

* [Vídeo](#video)
* [Introdução à classe StoreContext](#get-started-storecontext)
* [Implementar compras no aplicativo](#implement-iap)
* [Implementar a funcionalidade de avaliação](#implement-trial)
* [Testar a compra realizada em aplicativo ou a implementação de avaliação](#testing)
* [Recibos para compras no aplicativo](#receipts)
* [Como usar a classe StoreContext com o Desktop Bridge](#desktop)
* [Produtos, SKUs e disponibilidades](#products-skus)
* [IDs da Store](#store-ids)

<span id="video" />

### <a name="video"></a>Vídeo

Assista ao vídeo a seguir para obter uma visão geral de como implementar compras no aplicativo em seu app usando o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).
<br/>
<br/>
> [!VIDEO https://channel9.msdn.com/Blogs/One-Dev-Minute/Adding-In-App-Purchases-to-Your-UWP-App/player]

<span id="get-started-storecontext" />

### <a name="get-started-with-the-storecontext-class"></a>Introdução à classe StoreContext

O ponto de entrada principal para o namespace **Windows.Services.Store** é a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Essa classe fornece métodos que você pode usar para obter informações do aplicativo atual e seus complementos disponíveis, obter informações de licença do aplicativo atual ou seus complementos, comprar um aplicativo ou um complemento para o usuário atual e realizar outras tarefas. Para obter um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), siga um destes procedimentos:

* Em um app de usuário único (ou seja, um app executado apenas no contexto do usuário que iniciou o app), use o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) estático para obter um objeto **StoreContext** que seja possível usar para acessar dados relacionados à Microsoft Store para o usuário. A maioria dos aplicativos da Plataforma Universal do Windows (UWP) é um aplicativo de usuário único.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Em um [app multiusuário](../xbox-apps/multi-user-applications.md), use o método [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) estático para obter um objeto **StoreContext** que seja possível usar para acessar dados relacionados à Microsoft Store para um usuário específico que tenha feito logon usando a conta da Microsoft durante o uso do app. O exemplo a seguir obtém um objeto **StoreContext** para o primeiro usuário disponível.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

> [!NOTE]
> Os aplicativos da área de trabalho do Windows que usam o [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop) devem realizar etapas adicionais para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para poder usar esse objeto. Para obter mais informações, consulte [esta seção](#desktop).

Depois que tiver um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), você poderá começar a chamar métodos desse objeto para obter informações da Store sobre o aplicativo atual e os complementos, recuperar informações de licença para o aplicativo atual e os complementos, comprar um aplicativo ou um complemento para o usuário atual e realizar outras tarefas. Para obter mais informações sobre tarefas comuns que você pode realizar usando esse objeto, consulte os seguintes artigos:

* [Obter informações do produto para aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de assinatura para o app](enable-subscription-add-ons-for-your-app.md)
* [Implementar uma versão de avaliação do app](implement-a-trial-version-of-your-app.md)

Para obter um aplicativo de exemplo que demonstre como usar **StoreContext** e outros tipos no namespace **Windows.Services.Store**, consulte o [exemplo da Store](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />

### <a name="implement-in-app-purchases"></a>Implementar compras no aplicativo

Para oferecer uma compra no aplicativo aos clientes em seu aplicativo usando o namespace **Windows.Services.Store**:

1. Se seu aplicativo oferece complementos que os clientes podem adquirir, [crie envios de complemento para o seu aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Escreva um código em seu aplicativo para [recuperar informações de produto para o aplicativo ou um complemento oferecido pelo aplicativo](get-product-info-for-apps-and-add-ons.md) e [determinar se a licença está ativa](get-license-info-for-apps-and-add-ons.md) (ou seja, se o usuário tiver uma licença para usar o aplicativo ou um complemento). Se a licença não estiver ativa, exiba uma interface do usuário que ofereça o aplicativo ou complemento para venda ao usuário como uma compra no aplicativo.

3. Se o usuário optar por comprar o aplicativo ou o complemento, use o método indicado para comprar o produto:

    * Se o usuário estiver comprando o aplicativo ou um complemento durável, siga as instruções em [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md).
    * Se o usuário estiver comprando um complemento consumível, siga as instruções em [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md).
    * Se o usuário estiver comprando um complemento de assinatura, siga as instruções em [Habilitar complementos de assinatura para seu app](enable-subscription-add-ons-for-your-app.md).

4. Teste sua implementação seguindo as [diretrizes de teste](#testing) neste artigo.

<span id="implement-trial" />

### <a name="implement-trial-functionality"></a>Implementar a funcionalidade de avaliação

Para excluir ou limitar recursos em uma versão de avaliação do seu aplicativo usando o namespace **Windows.Services.Store**:

1. [Configure seu aplicativo como uma avaliação gratuita no painel do Centro de Desenvolvimento do Windows](../publish/set-app-pricing-and-availability.md#free-trial).

2. Escreva um código em seu aplicativo para [recuperar informações de produto para o aplicativo ou um complemento oferecido pelo aplicativo](get-product-info-for-apps-and-add-ons.md) e [determine se a licença associada ao aplicativo é uma licença de avaliação](get-license-info-for-apps-and-add-ons.md).

3. Exclua ou limite determinados recursos no aplicativo caso ela seja uma versão de avaliação e habilite os recursos quando o usuário comprar uma licença completa. Para obter mais informações e um exemplo de código, consulte [Implementar uma versão de avaliação do aplicativo](implement-a-trial-version-of-your-app.md).

4. Teste a implementação seguindo as [diretrizes de teste](#testing) neste artigo.

<span id="testing" />

### <a name="test-your-in-app-purchase-or-trial-implementation"></a>Testar a compra realizada em aplicativo ou a implementação de avaliação

Se seu app usa APIs no namespace **Windows.Services.Store** para implementar compras no aplicativo ou a funcionalidade de avaliação, você deve publicar seu aplicativo na Microsoft Store e baixá-lo em seu dispositivo de desenvolvimento para usar sua licença de teste. Siga este processo para testar seu código:

1. Se seu aplicativo ainda não estiver publicado e disponibilizado na Microsoft Store, verifique se ele atende aos requisitos mínimos do [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit), [envie seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) para o painel do Centro de Desenvolvimento do Windows e verifique se o aplicativo passou no processo de certificação. Você pode [configurar seu app para que ele não possa ser descoberto na Store](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) durante os testes.

2. Em seguida, certifique-se de que você tenha concluído o seguinte:

    * Escreva um código no aplicativo que use a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) e outros tipos relacionados no namespace **Windows.Services.Store** para implementar [compras no aplicativo](#implement-iap) ou [a funcionalidade de avaliação](#implement-trial).
    * Se o aplicativo oferece um complemento que os clientes possam comprar, [crie um envio de complemento para o aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).
    * Se você quiser excluir ou limitar alguns recursos em uma versão de avaliação do aplicativo, [configure o aplicativo como uma avaliação gratuita no painel do Centro de Desenvolvimento do Windows](../publish/set-app-pricing-and-availability.md#free-trial).

3. Com seu projeto aberto no Visual Studio, clique no **menu Projeto**, aponte para **Store** e clique em **Associar Aplicativo à Store**. Siga as instruções do assistente para associar o projeto de aplicativo ao aplicativo em sua conta do Centro de Desenvolvimento do Windows que você deseja usar para teste.
    > [!NOTE]
    > Se você não associar seu projeto a um aplicativo na Store, os métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) definirão a propriedade **ExtendedError** dos valores de retorno como o valor de código de erro 0x803F6107. Esse valor indica que a Store desconhece o aplicativo.
4. Se você não tiver feito isso ainda, instale o aplicativo da Store que você especificou na etapa anterior, execute-o uma vez e, em seguida, feche-o. Isso garante que uma licença válida para o aplicativo seja instalada em seu dispositivo de desenvolvimento.

5. No Visual Studio, comece a executar ou depurar seu projeto. Seu código deverá recuperar dados do aplicativo e do complemento do aplicativo da Store que você associou ao seu projeto local. Se for solicitada a reinstalação do aplicativo, siga as instruções e execute ou depure o projeto.
    > [!NOTE]
    > Depois de concluir essas etapas, você poderá continuar atualizando o código do aplicativo e, em seguida, depurar o projeto atualizado no computador de desenvolvimento sem enviar novos pacotes de aplicativos para a Store. Você só precisa baixar a versão da Store do aplicativo no computador de desenvolvimento uma vez para obter a licença local que será usada para teste. Você só precisará enviar novos pacotes de aplicativos para a Store depois de concluir o teste e se quiser disponibilizar os recursos relacionados à avaliação ou à compra no aplicativo no aplicativo para os clientes.

Se o seu app usa o namespace **Windows.ApplicationModel.Store**, você pode usar a classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) em seu app para simular as informações de licença durante o teste antes de enviar seu app para a Store. Para obter mais informações, consulte [Introdução às classes CurrentApp e CurrentAppSimulator] (in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md#get-started-with-the-currentapp-and-currentappsimulator-classes).  

> [!NOTE]
> O namespace **Windows.Services.Store** não fornece uma classe que seja possível usar para simular informações de licença durante o teste. Se você usar o namespace **Windows.Services.Store** para implementar compras ou avaliações no aplicativo, deverá publicar seu app na Store e baixá-lo em seu dispositivo de desenvolvimento para usar sua licença de teste como descrito acima.

<span id="receipts" />

### <a name="receipts-for-in-app-purchases"></a>Recibos para compras no aplicativo

O namespace **Windows.Services.Store** não fornece uma API que seja possível usar para obter um recibo de transação para compras bem-sucedidas no código do aplicativo. Essa é uma experiência diferente de aplicativos que usam o namespace **Windows.ApplicationModel.Store**, que pode [usar uma API do lado do cliente para recuperar um recibo de transação](use-receipts-to-verify-product-purchases.md).

Se você implementar compras no aplicativo usando o namespace **Windows.Services.Store** e você desejar validar se um determinado cliente comprou um app ou um complemento, você pode usar a [consulta para o método de produtos](query-for-products.md) na [API REST da coleção da Microsoft Store](view-and-grant-products-from-a-service.md). Os dados de retorno para esse método confirmam se o cliente especificado tem um direito para um determinado produto e fornecem dados para a transação na qual o usuário adquiriu o produto. A API de coleção da Microsoft Store usa a autenticação do Azure AD para recuperar essas informações.

<span id="desktop" />

### <a name="using-the-storecontext-class-with-the-desktop-bridge"></a>Como usar a classe StoreContext com o Desktop Bridge

Os aplicativos da área de trabalho que usam o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) podem utilizar a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para implementar compras no aplicativo e avaliações. No entanto, se você tiver um aplicativo da área de trabalho Win32 ou um aplicativo da área de trabalho que tenha um identificador de janela (HWND) associado à estrutura de renderização (por exemplo, um aplicativo WPF), seu aplicativo deve configurar o objeto **StoreContext** para especificar qual janela do aplicativo é a janela do proprietário para caixas de diálogo modais que são mostradas pelo objeto.

Muitos membros do **StoreContext** (e membros de outros tipos relacionados que são acessados por meio do objeto **StoreContext**) exibem uma caixa de diálogo modal para o usuário para operações relacionadas à Store, como comprar um produto. Se um aplicativo de desktop não configura o objeto **StoreContext** para especificar a janela do proprietário para caixas de diálogo modais, esse objeto retornará dados imprecisos ou erros.

Para configurar um objeto **StoreContext** em um aplicativo da área de trabalho que use o Desktop Bridge, siga estas etapas.

1. Siga um destes procedimentos para permitir que o aplicativo acesse a interface [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx):

    * Se o aplicativo estiver escrito em uma linguagem gerenciada, como C# ou Visual Basic, declare a interface **IInitializeWithWindow** no código do aplicativo com o atributo [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx) conforme mostrado no exemplo em C# a seguir. Este exemplo pressupõe que o arquivo de código tenha uma declaração **using** para o namespace **InteropServices**.

        ```csharp
        [ComImport]
        [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
        [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
        public interface IInitializeWithWindow
        {
            void Initialize(IntPtr hwnd);
        }
        ```

    * Se o aplicativo estiver escrito em C++, adicione uma referência ao arquivo de cabeçalho shobjidl.h no código. Este arquivo de cabeçalho contém a declaração da interface **IInitializeWithWindow**.

2. Obtenha um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) usando o método [GetDefault](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getdefault) (ou [GetForUser](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getforuser) caso o aplicativo seja um [aplicativo multiusuário](../xbox-apps/multi-user-applications.md)) conforme descrito anteriormente neste artigo, e converta esse objeto em um objeto [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Em seguida, chame o método [IInitializeWithWindow.Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) e passe o identificador da janela da qual você deseja ser o proprietário para eventuais caixas de diálogo modais mostradas por métodos **StoreContext**. O exemplo em C# a seguir mostra como passar o identificador da janela principal do aplicativo para o método.
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />

### <a name="products-skus-and-availabilities"></a>Produtos, SKUs e disponibilidades

Cada produto na Store tem pelo menos uma *SKU*, e cada SKU tem pelo menos uma *disponibilidade*. Esses conceitos são abstraídos da maioria dos desenvolvedores no painel do Centro de Desenvolvimento do Windows, e a maioria dos desenvolvedores nunca definirão SKUs ou disponibilidades para seus aplicativos ou complementos. No entanto, como o objeto de modelo para produtos da Store no namespace **Windows.Services.Store** inclui disponibilidades e SKUs, um entendimento básico sobre esses conceitos pode ser útil para alguns cenários.

| Objeto |  Descrição  |
|---------|-------------------|
| Produto  |  Um *produto* refere-se a qualquer tipo de produto que esteja disponível na Store, incluindo um app ou um complemento. <p/><p/> Cada produto na Store tem um objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) correspondente. Essa classe fornece propriedades que você pode usar para acessar dados, como a ID da Store do produto, as imagens e os vídeos para a listagem da Store e informações de preços. Também fornece métodos que você pode usar para comprar o produto. |
| SKU |  A *SKU* é uma versão específica de um produto com sua própria descrição, preço e outros detalhes exclusivos do produto. Cada aplicativo ou complemento tem uma SKU padrão. O único momento em que a maioria dos desenvolvedores terá várias SKUs para um aplicativo é se publicarem uma versão completa do aplicativo e uma versão de avaliação (no catálogo da Store, cada uma dessas versões é uma SKU diferente do mesmo aplicativo). <p/><p/> Alguns fornecedores podem definir suas próprias SKUs. Por exemplo, um grande fornecedor de jogos pode lançar um jogo com uma SKU que mostre sangue verde em mercados que não permitem sangue vermelho e outra SKU que mostre sangue vermelho nos demais mercados. Como alternativa, um fornecedor que vende conteúdo em vídeo digital pode publicar duas SKUs para um vídeo, uma SKU para a versão em alta definição e outra SKU para a versão em definição padrão. <p/><p/> Cada SKU na Store tem um objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) correspondente. Cada [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) tem uma propriedade [Skus](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.skus) que você pode usar para acessar as SKUs para o produto. |
| Disponibilidade  |  Uma *disponibilidade* é uma versão específica de uma SKU com suas próprias informações de preço. Cada SKU tem uma disponibilidade padrão. Alguns fornecedores podem definir suas próprias disponibilidade para apresentar opções de preço diferentes para determinada SKU. <p/><p/> Cada disponibilidade na Store tem um objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) correspondente. Cada [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) tem uma propriedade [Availabilities](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.availabilities) que você pode usar para acessar as disponibilidades para a SKU. Para a maioria dos desenvolvedores, cada SKU tem uma única disponibilidade padrão.  |

<span id="store_ids" />

### <a name="store-ids"></a>IDs da Store

Cada app, complemento ou outro produto na Store tenha uma **ID da Store** associada (que às vezes também é chamada de *ID da Store do produto*). Muitas APIs exigem a ID da LOja para executar uma operação em um app ou complemento.

A ID da Store de qualquer produto na Store é uma cadeia de 12 caracteres alfanuméricos, como ```9NBLGGH4R315```. Há várias maneiras diferentes de se obter a ID da Store para um produto na Store:

* Para um app, você pode obter a ID da Store na [página Identidade do app](../publish/view-app-identity-details.md) do painel do Centro de Desenvolvimento.
* Para um complemento, você pode obter a ID da Store na página de visão geral do complemento no painel.
* Para qualquer produto, você também pode obter a ID da Store programaticamente usando a propriedade [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeproduct.storeid) do objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) que representa o produto.

Para produtos com SKUs e disponibilidades, as SKUs e as disponibilidades também têm suas próprias IDs da Store com formatos diferentes.

| Objeto |  Formato da ID da Store  |
|---------|-------------------|
| SKU |  A ID da Store para uma SKU tem o formato ```<product Store ID>/xxxx```, onde ```xxxx``` é uma cadeia de 4 caracteres alfanuméricos que identifica uma SKU do produto. Por exemplo, ```9NBLGGH4R315/000N```. Essa ID é retornada pela propriedade [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storesku.storeid) de um objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) e é chamada, às vezes, de *ID da Store para a SKU*. |
| Disponibilidade  |  A ID da Store para uma disponibilidade tem o formato ```<product Store ID>/xxxx/yyyyyyyyyyyy```, onde ```xxxx``` é uma cadeia de 4 caracteres alfanuméricos que identifica uma SKU do produto e ```yyyyyyyyyyyy``` é uma cadeia de 12 caracteres alfanuméricos que identifica uma disponibilidade para a SKU. Por exemplo, ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Essa ID é retornada pela propriedade [StoreId](https://docs.microsoft.com/uwp/api/windows.services.store.storeavailability.storeid) de um objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) e é chamada, às vezes, de *ID da Store para a disponibilidade*.  |

<span id="product-ids" />

## <a name="how-to-use-product-ids-for-add-ons-in-your-code"></a>Como usar IDs do produto (product IDs) para complementos no seu código

Se você quiser disponibilizar um complemento para seus clientes no contexto de seu app, [insira uma ID do produto exclusiva](../publish/set-your-add-on-product-id.md#product-id) para o complemento quando você [criar seu envio de complemento](../publish/add-on-submissions.md) no painel do Centro de Desenvolvimento. Você pode usar essa ID do produto (product ID) para fazer referência ao complemento em seu código, embora os cenários específicos nos quais você poderá usar a ID do produto dependerão do namespace usado para compras no aplicativo do seu app.

> [!NOTE]
> A ID do produto (product ID) inserida no painel do Centro de Desenvolvimento para um complemento é diferente da [ID da Store ](#store-ids) do complemento. A ID da Store é gerada pelo Centro de Desenvolvimento.

### <a name="apps-that-use-the-windowsservicesstore-namespace"></a>Apps que usam o namespace Windows.Services.Store

Se seu app usar o namespace **Windows.Services.Store**, você poderá usar a ID do produto (product ID) para identificar com facilidade o [StoreProduct](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct) que representa o complemento ou a [StoreLicense](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense) que representa a licença do complemento. A ID do produto (product ID) é exposta pelas propriedades [StoreProduct.InAppOfferToken](https://docs.microsoft.com/uwp/api/Windows.Services.Store.StoreProduct.InAppOfferToken) e [StoreLicense.InAppOfferToken](https://docs.microsoft.com/uwp/api/windows.services.store.storelicense.InAppOfferToken).

> [!NOTE]
> Embora a ID do produto (product ID) seja uma maneira útil de identificar um complemento em seu código, a maioria das operações no namespace **Windows.Services.Store** usa a [ID da Store](#store-ids) de um complemento em vez da ID do produto. Por exemplo, para recuperar programaticamente um ou mais complementos conhecidos para um app, passe as IDs da Store (em vez das IDs do produto (product IDs) dos complementos para o método [GetStoreProductsAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.getstoreproductsasync). Da mesma forma, para reportar um complemento consumível como providenciado, passe a ID da Store do complemento (em vez da ID do produto (productID)) para o método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.services.store.storecontext.reportconsumablefulfillmentasync).

### <a name="apps-that-use-the-windowsapplicationmodelstore-namespace"></a>Apps que usam o namespace Windows.ApplicationModel.Store

Se seu app usar o namespace**Windows.ApplicationModel.Store**, você precisará usar a ID do produto (product ID)atribuída a um complemento no painel do Centro de Desenvolvimento para a maioria das operações. Por exemplo:

* Use a ID do produto (product ID) para identificar a [ProductListing](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting) que representa o complemento ou a [ProductLicense](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense) que representa licença do seu complemento. A ID do produto (product ID) é exposta pelas propriedades [ProductListing.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlisting.ProductId) e [ProductLicense.ProductId](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.productlicense.ProductId).

* Especifique a ID de produto (product ID) quando comprar o complemento para o usuário usando o método [RequestProductPurchaseAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.requestproductpurchaseasync). Para obter mais informações, consulte [Habilitar compras de produto no aplicativo](enable-in-app-product-purchases.md).

* Especifique a ID de produto (product ID) quando reportar o complemento consumível como providenciado usando o método [ReportConsumableFulfillmentAsync](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store.currentapp.reportconsumablefulfillmentasync). Para obter mais informações, consulte [Habilitar compras de produto consumível no aplicativo](enable-consumable-in-app-product-purchases.md).

## <a name="related-topics"></a>Tópicos relacionados

* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de assinatura para o app](enable-subscription-add-ons-for-your-app.md)
* [Implementar uma versão de avaliação do app](implement-a-trial-version-of-your-app.md)
* [Códigos de erro para operações da Microsoft Store](error-codes-for-store-operations.md)
* [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)
