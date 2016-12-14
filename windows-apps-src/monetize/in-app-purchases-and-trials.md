---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "Saiba como habilitar compras no aplicativo e avaliações em aplicativos UWP."
title: "Compras no aplicativo e avaliações"
translationtype: Human Translation
ms.sourcegitcommit: ffda100344b1264c18b93f096d8061570dd8edee
ms.openlocfilehash: 7783b6017a314ddb24509c55db8134a4c214430f

---

# <a name="in-app-purchases-and-trials"></a>Compras no aplicativo e avaliações

O SDK do Windows fornece APIs que você pode usar para implementar os seguintes recursos para ganhar mais dinheiro com seu aplicativo da Plataforma Universal do Windows (UWP):

* **Compras no aplicativo**&nbsp;&nbsp;Seja seu aplicativo gratuito ou não, você pode vender conteúdo ou uma nova funcionalidade do aplicativo (como o desbloqueio do próximo nível de um jogo) no próprio aplicativo.

* **Funcionalidade de avaliação**&nbsp;&nbsp;Se você configurar seu aplicativo como [avaliação gratuita no painel do Centro de Desenvolvimento do Windows](../publish/set-app-pricing-and-availability.md#free-trial), incentive seus clientes a comprar a versão completa do aplicativo, excluindo ou limitando alguns recursos durante o período de avaliação. Você também pode habilitar recursos, como faixas ou marcas-d'água, que são mostrados apenas durante a avaliação, antes de o cliente comprar o aplicativo.

Este artigo fornece uma visão geral de como as compras no aplicativo e as avaliações funcionam em aplicativos UWP.

<span id="choose-namespace" />
## <a name="choose-which-namespace-to-use"></a>Escolha qual namespace usar

Há dois namespaces diferentes que você pode usar para adicionar compras no aplicativo e funcionalidade de avaliação aos seus aplicativos UWP, dependendo da versão do Windows 10 à qual seus aplicativos se destinam. Embora as APIs nesses namespaces tenha os mesmos objetivos, elas foram criadas de forma bem diferente, e o código não é compatível entre as duas APIs.

* **[Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx)**&nbsp;&nbsp;A partir do Windows 10, versão 1607, aplicativos podem usar a API nesse namespace para implementar compras no aplicativo e avaliações. Recomendamos que você use os membros nesse namespace se seu aplicativo for voltado para o Windows 10, versão 1607, ou uma versão posterior. Esse namespace dá suporte aos tipos de complemento mais recentes, como complementos consumíveis gerenciados pela Loja, e foi projetado para ser compatível com tipos de produtos e recursos futuros com suporte do Centro de Desenvolvimento do Windows e da Loja. Para saber mais sobre esse namespace, consulte a seção [usando o namespace Windows.Services.Store](#api_intro) neste artigo.

* **[Windows.ApplicationModel.Store](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx)**&nbsp;&nbsp;Todas as versões do Windows 10 também oferecem suporte a uma API mais antiga para compras no aplicativo e avaliações nesse namespace. Embora qualquer aplicativo UWP para o Windows 10 possa usar esse namespace, esse namespace não pode ser atualizado para dar suporte a novos tipos de produtos e recursos no Centro de Desenvolvimento e na Loja no futuro. Para obter informações sobre esse namespace, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

<span id="concepts" />
## <a name="basic-concepts"></a>Conceitos básicos

Esta seção apresenta conceitos básicos para compras no aplicativo e avaliações em aplicativos UWP. A maioria desses conceitos se aplicam aos namespaces **Windows.Services.Store** e **Windows.ApplicationModel.Store**, exceto quando houver alguma observação.

Cada item que é oferecido na Loja é geralmente chamado de *produto*. A maioria dos desenvolvedores trabalha com os seguintes tipos de produtos: *aplicativos* e *complementos* (também conhecidos como produtos no aplicativo ou IAPs).

Um complemento se refere a um produto ou recurso que você disponibiliza para os clientes no contexto de seu aplicativo. Um complemento pode representar qualquer funcionalidade que seu aplicativo oferece aos clientes: por exemplo, a moeda a ser usada em um aplicativo ou jogos, novos mapas ou armas para um jogo, a capacidade de usar seu aplicativo sem anúncios ou conteúdo digital, como músicas ou vídeos, para aplicativos que podem oferecer esse tipo de conteúdo. Todos os aplicativos e complementos têm uma licença associada que indica se o usuário tem direito de usar o aplicativo ou complemento. Se o usuário tiver direito de usar o aplicativo ou complemento como avaliação, a licença também fornece informações adicionais sobre a avaliação.

Para oferecer um complemento para clientes em seu aplicativo, você deve [definir o complemento para o aplicativo no painel do Centro de Desenvolvimento](../publish/iap-submissions.md) para que a Loja saiba disso. Em seguida, seu aplicativo pode usar APIs no namespace **Windows.Services.Store** ou **Windows.ApplicationModel.Store** para oferecer o complemento para venda ao usuário como uma compra no aplicativo.

Aplicativos UWP podem oferecer os seguintes tipos de complementos.

| Tipo de complemento |  Descrição  |
|---------|-------------------|
| Durável  |  Um complemento que dura o tempo de vida útil que você especificar no [painel do Centro de Desenvolvimento do Windows](../publish/enter-iap-properties.md). <p/><p/>Por padrão, os complementos duráveis nunca expiram, podendo ser adquiridos somente uma vez. Se você especificar uma duração específica para o complemento, o usuário poderá comprar novamente o complemento depois que ele expirar. |
| Consumível gerenciado pelo desenvolvedor  |  Um complemento que pode ser comprado, usado e comprado novamente. Esse tipo de complemento frequentemente é usado para a moeda no aplicativo. <p/><p/>Para esse tipo de produto consumível, você é responsável por controlar o saldo do usuário de itens do usuário que o complemento representa e por relatar a compra do complemento como providenciada para a Loja depois que o usuário consome todos os itens. O usuário não pode comprar o complemento novamente até que seu aplicativo tenha informado a compra anterior do complemento como providenciada. <p/><p/>Por exemplo, se o seu complemento representar 100 moedas em um jogo e o usuário consumir 10 moedas, seu aplicativo ou o serviço deverá manter o novo saldo restante de 90 moedas para o usuário. Depois que o usuário tiver consumido todas as 100 moedas, seu aplicativo deverá declarar o complemento como providenciado e, em seguida, o usuário poderá comprar o complemento de 100 moedas novamente.    |
| Consumível gerenciado pela Loja  |  Um complemento que pode ser comprado, usado e comprado novamente. Esse tipo de complemento frequentemente é usado para a moeda no aplicativo.<p/><p/>Para esse tipo de produto consumível, a Loja mantém o controle do saldo de itens do usuário que o complemento representa. Quando o usuário consome todos os itens, você é responsável por relatar esses itens como providenciados para a Loja, e a Loja atualiza o saldo do usuário. Seu aplicativo pode consultar o saldo atual para o usuário a qualquer momento. Depois que o usuário consome todos os itens, ele pode comprar o complemento novamente.  <p/><p/> Por exemplo, se o complemento representar uma quantidade inicial de 100 moedas em um jogo e o usuário consumir 10 moedas, o aplicativo relatará para a Loja que 10 unidades do complemento foram providenciadas, e a Loja atualizará o saldo restante. Depois que o usuário tiver consumido todas as 100 moedas, o usuário poderá comprar o complemento de 100 moedas novamente. <p/><p/>**Observação**&nbsp;&nbsp;Os consumíveis gerenciados pela Loja estão disponíveis a partir do Windows 10, versão 1607. Para usar consumíveis gerenciados pela Loja, o aplicativo deve segmentar o Windows 10, versão 1607 ou posterior, e deve usar a API no namespace **Windows.Services.Store**, em vez do namespace **ApplicationModel**.  |

<span />

>**Observação**&nbsp;&nbsp;Outros tipos de complementos, como complementos duráveis com pacotes (também conhecidos como conteúdo para download ou DLC), estão disponíveis apenas para um conjunto restrito de desenvolvedores e não são abordados nesta documentação.

<span id="api_intro" />
## <a name="using-the-windowsservicesstore-namespace"></a>Usando o namespace Windows.Services.Store

O restante deste artigo descreve como implementar compras no aplicativo e avaliações usando o namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx). Esse namespace está disponível somente para aplicativos destinados ao Windows 10, versão 1607, ou posterior, e recomendamos que os aplicativos usem esse namespace em vez do namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx), se possível.

Se você procura informações sobre o namespace **Windows.ApplicationModel.Store**, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

### <a name="get-started-with-the-storecontext-class"></a>Introdução à classe StoreContext

O ponto de entrada principal para o namespace **Windows.Services.Store** é a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Essa classe fornece métodos que você pode usar para obter informações do aplicativo atual e seus complementos disponíveis, obter informações de licença do aplicativo atual ou seus complementos, comprar um aplicativo ou um complemento para o usuário atual e realizar outras tarefas. Para obter um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), siga um destes procedimentos:

* Em um aplicativo de usuário único (ou seja, um aplicativo que é executado somente no contexto do usuário que iniciou o aplicativo), use o método [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) para obter um objeto **StoreContext** que você possa usar para acessar e gerenciar os dados relacionados à Windows Store para o usuário. Os aplicativos da Plataforma Universal do Windows (UWP), em sua maioria, são aplicativos de usuário único.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Em um [aplicativo multiusuário](../xbox-apps/multi-user-applications.md), use o método [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) para obter um objeto **StoreContext** que você possa usar para acessar e gerenciar os dados relacionados à Windows Store para um usuário específico que está conectado com sua conta da Microsoft enquanto estiver usando o aplicativo. O exemplo a seguir obtém um objeto **StoreContext** para o primeiro usuário disponível.

  > [!div class="tabbedCodeSnippets"]
  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

>**Observação**&nbsp;&nbsp;Aplicativos da área de trabalho do Windows que usam o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) devem executar etapas adicionais para configurar o objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) antes que eles possam usar esse objeto. Para obter mais informações, consulte [Usando a classe StoreContext em um aplicativo da área de trabalho que usa o Desktop Bridge](#desktop).

Depois que você tiver um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), você pode começar a chamar métodos para obter informações da Loja sobre o aplicativo atual e seus complementos, recuperar informações de licença para o aplicativo atual e seus complementos, comprar um aplicativo ou um complemento para o usuário atual e realizar outras tarefas. Para obter mais informações sobre tarefas comuns que você pode realizar usando esse namespace, consulte os artigos a seguir:

* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)

Para conferir um exemplo de aplicativo que demonstre como usar o namespace **Windows.Services.Store**, consulte o [exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span id="implement-iap" />
### <a name="implement-in-app-purchases"></a>Implementar compras no aplicativo

Para oferecer uma compra no aplicativo aos clientes em seu aplicativo usando o namespace **Windows.Services.Store**:

1. Se seu aplicativo oferece complementos que os clientes podem adquirir, [crie envios de complemento para o seu aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

2. Escreva um código em seu aplicativo para [recuperar informações de produto para o aplicativo ou um complemento oferecido pelo aplicativo](get-product-info-for-apps-and-add-ons.md) e [determinar se a licença está ativa](get-license-info-for-apps-and-add-ons.md) (ou seja, se o usuário tiver uma licença para usar o aplicativo ou um complemento). Se a licença não estiver ativa, exiba uma interface do usuário que ofereça o aplicativo ou complemento para venda ao usuário como uma compra no aplicativo.

3. Se o usuário optar por comprar seu aplicativo ou complemento, use o método apropriado para comprar o produto. Se o usuário estiver adquirindo seu aplicativo ou um complemento durável, siga o processo em [Habilitar compras de aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md). Se o usuário estiver adquirindo um complemento consumível, siga as instruções em [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md).

4. Teste sua implementação seguindo as [diretrizes para teste](#testing) neste artigo.

<span id="implement-trial" />
### <a name="implement-trial-functionality"></a>Implementar a funcionalidade de avaliação

Para excluir ou limitar recursos em uma versão de avaliação do seu aplicativo usando o namespace **Windows.Services.Store**:

1. [Configure seu aplicativo como uma avaliação gratuita no painel do Centro de Desenvolvimento do Windows](../publish/set-app-pricing-and-availability.md#free-trial).

2. Escreva um código em seu aplicativo para [recuperar informações de produto para o aplicativo ou um complemento oferecido pelo aplicativo](get-product-info-for-apps-and-add-ons.md) e [determine se a licença associada ao aplicativo é uma licença de avaliação](get-license-info-for-apps-and-add-ons.md).

3. Exclua ou limite alguns recursos em seu aplicativo se for uma versão de avaliação e habilite os recursos quando o usuário comprar uma licença completa. Para obter mais informações, consulte [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md).

4. Teste sua implementação seguindo as [diretrizes para teste](#testing) neste artigo.

<span id="testing" />
### <a name="test-your-implementation"></a>Teste sua implementação

O namespace **Windows.Services.Store** não fornece uma classe que você pode usar para simular as informações de licença durante o teste. Em vez disso, você deve publicar um aplicativo na Loja e baixá-lo para seu dispositivo de desenvolvimento para usar sua licença para teste. Essa é uma experiência diferente de aplicativos que usam o namespace **Windows.ApplicationModel.Store**, os quais podem usar a classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para simular as informações de licença durante o teste

Se seu aplicativo usa APIs no namespace **Windows.Services.Store** para acessar informações de seu aplicativo e seus complementos, siga este processo para testar seu código:

1. Se seu aplicativo ainda não estiver publicado e disponibilizado na Loja, verifique se ele atende aos requisitos mínimos do [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit), [envie seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) para o painel do Centro de Desenvolvimento do Windows e verifique se ele foi aprovado no processo de certificação para que seja listado na Loja. Opcionalmente, você pode [ocultar seu aplicativo da Loja](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) para que ele não esteja disponível para clientes enquanto você o testa.

2. Em seguida, certifique-se de que você tenha concluído o seguinte:

  * Escrever um código em seu aplicativo que use a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) e outros tipos relacionados no namespace **Windows.Services.Store** para implementar [compras no aplicativo](#implement-iap) ou [a funcionalidade de avaliação](#implement-trial).

  * Se seu aplicativo oferece complementos que os clientes podem adquirir, [crie envios de complemento para o seu aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/add-on-submissions).

  * Se você quiser excluir ou limitar alguns recursos na versão de avaliação do aplicativo, [configure seu aplicativo como uma avaliação gratuita no painel do Centro de Desenvolvimento do Windows](../publish/set-app-pricing-and-availability.md#free-trial).

3. Com seu projeto aberto no Visual Studio, clique no **menu Projeto**, aponte para **Loja** e clique em **Associar Aplicativo à Loja**. Siga as instruções do assistente para associar o projeto de aplicativo ao aplicativo em sua conta do Centro de Desenvolvimento do Windows que você deseja usar para teste.

  >**Observação**&nbsp;&nbsp;Se você não associar seu projeto a um aplicativo na Loja, os métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) definirão a propriedade **ExtendedError** dos valores de retorno como o valor de código de erro 0x803F6107. Esse valor indica que a Loja desconhece o aplicativo.

4. Se você não tiver feito isso ainda, instale o aplicativo da Loja que você especificou na etapa anterior, execute-o uma vez e, em seguida, feche-o. Isso garante que uma licença válida para o aplicativo seja instalada em seu dispositivo de desenvolvimento.

5. No Visual Studio, comece a executar ou depurar seu projeto. Seu código deverá recuperar dados do aplicativo e do complemento do aplicativo da Loja que você associou ao seu projeto local. Se você for solicitado a reinstalar o aplicativo, siga as instruções e execute ou depure seu projeto.

<span id="receipts" />
### <a name="receipts-for-in-app-purchases"></a>Recibos para compras no aplicativo

O namespace **Windows.Services.Store** não fornece uma API que você possa usar para obter um recibo de transação para compras bem-sucedidas no código do aplicativo. Essa é uma experiência diferente de aplicativos que usam o namespace **Windows.ApplicationModel.Store**, que pode [usar uma API do lado do cliente para recuperar um recibo de transação](use-receipts-to-verify-product-purchases.md).

Se você implementar compras no aplicativo usando o namespace **Windows.Services.Store** e você desejar validar se um determinado cliente comprou um aplicativo ou um complemento, você pode usar a [consulta para o método de produtos](query-for-products.md) na [API REST de coleção da Windows Store](view-and-grant-products-from-a-service.md). Os dados de retorno para esse método confirmam se o cliente especificado tem um direito para um determinado produto e fornecem dados para a transação na qual o usuário adquiriu o produto. A API de coleção da Windows Store usa a autenticação do Azure AD para recuperar essas informações.

<span id="desktop" />
### <a name="using-the-storecontext-class-in-an-app-that-uses-the-desktop-bridge"></a>Usando a classe StoreContext em um aplicativo que usa o Desktop Bridge

Aplicativos da área de trabalho que usam o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop) podem utilizar a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) para implementar compras no aplicativo e avaliações. No entanto, se você tiver um aplicativo da área de trabalho Win32 ou um aplicativo da área de trabalho que tenha um identificador de janela (HWND) associado à estrutura de renderização (por exemplo, um aplicativo WPF), seu aplicativo deve configurar o objeto **StoreContext** para especificar qual janela do aplicativo é a janela do proprietário para caixas de diálogo modais que são mostradas pelo objeto.

Muitos membros do **StoreContext** (e membros de outros tipos relacionados que são acessados por meio do objeto **StoreContext**) exibem uma caixa de diálogo modal para o usuário para operações relacionadas à Loja, como comprar um produto. Se um aplicativo de desktop não configura o objeto **StoreContext** para especificar a janela do proprietário para caixas de diálogo modais, esse objeto retornará dados imprecisos ou erros.

Para configurar um objeto **StoreContext** em um aplicativo da área de trabalho que usa o Desktop Bridge, siga estas etapas.

  1. Se seu aplicativo foi escrito em um idioma gerenciado, como C# ou Visual Basic, declare a interface [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx) no código do seu aplicativo com o atributo [ComImport](https://msdn.microsoft.com/library/system.runtime.interopservices.comimportattribute.aspx), conforme mostrado no exemplo a seguir. Este exemplo pressupõe que seu arquivo de código tenha uma declaração **using** para o namespace **InteropServices**.

    > [!div class="tabbedCodeSnippets"]
    ```csharp
    [ComImport]
    [Guid("3E68D4BD-7135-4D10-8018-9FB6D9F33FA1")]
    [InterfaceType(ComInterfaceType.InterfaceIsIUnknown)]
    public interface IInitializeWithWindow
    {
        void Initialize(IntPtr hwnd);
    }
    ```

    Se seu aplicativo for escrito em código nativo, como C++, você não precisa importar a interface **IInitializeWithWindow**. Basta adicionar uma referência ao arquivo de cabeçalho shobjidl.h em seu código.

  2. Use o método [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) (ou [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx)) para obter um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), conforme descrito anteriormente neste artigo, e converta esse objeto em um objeto [IInitializeWithWindow](https://msdn.microsoft.com/library/windows/desktop/hh706981.aspx). Em seguida, chame o método [Initialize](https://msdn.microsoft.com/library/windows/desktop/hh706982.aspx) e passe o identificador da janela da qual você deseja ser o proprietário para quaisquer caixas de diálogo modais mostradas por métodos **StoreContext**. O exemplo a seguir mostra como passar o identificador da janela principal do seu aplicativo para o método.

    > [!div class="tabbedCodeSnippets"]
    ```csharp
    StoreContext context = StoreContext.GetDefault();
    IInitializeWithWindow initWindow = (IInitializeWithWindow)(object)context;
    initWindow.Initialize(System.Diagnostics.Process.GetCurrentProcess().MainWindowHandle);
    ```

<span id="products-skus" />
### <a name="products-skus-and-availabilities"></a>Produtos, SKUs e disponibilidades

Cada produto na Loja tem pelo menos uma *SKU*, e cada SKU tem pelo menos uma *disponibilidade*. Esses conceitos são abstraídos da maioria dos desenvolvedores no painel do Centro de Desenvolvimento do Windows, e a maioria dos desenvolvedores nunca definirão SKUs ou disponibilidades para seus aplicativos ou complementos. No entanto, como o objeto de modelo para produtos da Loja no namespace **Windows.Services.Store** inclui disponibilidades e SKUs, um entendimento básico sobre esses conceitos pode ser útil.

| Tipo de objeto |  Descrição  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  Essa classe representa qualquer tipo de produto que esteja disponível na Loja, incluindo um aplicativo ou um complemento. Essa classe fornece propriedades que você pode usar para acessar dados, como a ID da Loja do produto, as imagens e os vídeos para a listagem da Loja e informações de preços. Também fornece métodos que você pode usar para comprar o produto. |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  Essa classe representa uma *SKU* para um produto. SKU é uma versão específica de um produto com sua própria descrição, preço e outros detalhes exclusivos do produto. Cada aplicativo ou complemento tem uma SKU padrão. O único momento em que a maioria dos desenvolvedores terá várias SKUs para um aplicativo é se publicarem uma versão completa do aplicativo e uma versão de avaliação (no catálogo da Loja, cada uma dessas versões é uma SKU diferente do mesmo aplicativo). <p/><p/> Alguns fornecedores podem definir suas próprias SKUs. Por exemplo, um grande fornecedor de jogos pode lançar um jogo com uma SKU que mostre sangue verde em mercados que não permitem sangue vermelho e outra SKU que mostre sangue vermelho nos demais mercados. Como alternativa, um fornecedor que vende conteúdo em vídeo digital pode publicar duas SKUs para um vídeo, uma SKU para a versão em alta definição e outra SKU para a versão em definição padrão. <p/><p/> Cada produto tem uma propriedade [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) que você pode usar para acessar as SKUs. |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  Essa classe representa uma *disponibilidade* para uma SKU. Disponibilidade é uma versão específica de uma SKU com suas próprias informações de preço. Cada SKU tem uma disponibilidade padrão. Alguns fornecedores podem definir suas próprias disponibilidade para apresentar opções de preço diferentes para determinada SKU. <p/><p/> Cada SKU tem uma propriedade [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) que você pode usar para acessar as disponibilidades. Para a maioria dos desenvolvedores, cada SKU tem uma única disponibilidade padrão.  |

<span id="store_ids" />
### <a name="store-ids"></a>IDs da Loja

Todos os aplicativos e complementos na Loja têm uma **ID da Loja** associada. Muitas das APIs do namespace **Windows.Services.Store** exigem a ID da Loja para executar uma operação em um aplicativo ou complemento. Os produtos, SKUs e disponibilidades têm diversos formatos de ID da Loja.

| Tipo de objeto |  Formato da ID da Loja  |
|---------|-------------------|
| [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx)  |  A ID da Loja de qualquer produto na Loja é uma cadeia de 12 caracteres alfanuméricos, como ```9NBLGGH4R315```. Essa ID da Loja está disponível na página do painel do Centro de Desenvolvimento do Windows para o aplicativo ou complemento, e ela é retornada pela propriedade [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) do objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). Essa ID é chamada, às vezes, de *ID da Loja do produto*. |
| [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) |  Para uma SKU, a ID da Loja tem o formato ```<product Store ID>/xxxx```, onde ```xxxx``` é uma cadeia de 4 caracteres alfanuméricos que identifica uma SKU do produto. Por exemplo, ```9NBLGGH4R315/000N```. Essa ID é retornada pela propriedade [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) de um objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) e é chamada, às vezes, de *ID da Loja para a SKU*. |
| [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx)  |  Para obter uma disponibilidade, a ID da Loja tem o formato ```<product Store ID>/xxxx/yyyyyyyyyyyy```, onde ```xxxx``` é uma cadeia de 4 caracteres alfanuméricos que identifica uma SKU do produto e ```yyyyyyyyyyyy``` é uma cadeia de 12 caracteres alfanuméricos que identifica uma disponibilidade para a SKU. Por exemplo, ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Essa ID é retornada pela propriedade [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) de um objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) e é chamada, às vezes, de *ID da Loja para a disponibilidade*.  |

## <a name="related-topics"></a>Tópicos relacionados

* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Dec16_HO1-->


