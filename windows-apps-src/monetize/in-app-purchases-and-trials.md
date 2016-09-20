---
author: mcleanbyron
ms.assetid: F45E6F35-BC18-45C8-A8A5-193D528E2A4E
description: "Saiba como habilitar compras no aplicativo e avaliações em aplicativos UWP."
title: "Compras no aplicativo e avaliações"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: 99143d48a5f2155b0a47008574d0a78243dea925

---

# Compras no aplicativo e avaliações

O SDK do Windows fornece APIs que você pode usar para adicionar compras no aplicativo e a funcionalidade de avaliação ao seu aplicativo da Plataforma Universal do Windows (UWP) para ajudar a monetizar seu aplicativo e adicionar nova funcionalidade. Essas APIs também fornecem acesso às informações de licença de seu aplicativo.

Para esses cenários, o Windows 10 oferece duas APIs diferentes:

* Todas as versões do Windows 10 dão suporte a uma API para compras no aplicativo e informações de licença do namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx).

* A partir do Windows 10, versão 1607, há uma API alternativa para compras no aplicativo e as informações de licença no namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx).  

Embora as APIs nesses namespaces tenha os mesmos objetivos, elas foram criadas de forma bem diferente, e o código não é compatível entre as duas APIs. Se seu aplicativo for voltado para o Windows 10, versão 1607 ou posterior, recomendamos que você use o namespace **Windows.Services.Store**. Esse namespace dá suporte aos tipos de complemento mais recentes, como complementos consumíveis gerenciados pela Loja, e foi projetado para ser compatível com tipos de produtos e recursos futuros com suporte do Centro de Desenvolvimento do Windows e da Loja. O namespace **Windows.Services.Store** também tem um desempenho melhor.

Este artigo apresenta o recurso de compras no aplicativo para aplicativos UWP e fornece uma visão geral do namespace **Windows.Services.Store** que está disponível a partir do Windows 10, versão 1607. Para obter informações sobre como usar os membros no namespace **Windows.ApplicationModel.Store**, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).


## Visão geral de compras no aplicativo em aplicativos UWP

Esta seção descreve os conceitos básicos sobre como compras no aplicativo e avaliações funcionam com aplicativos UWP na Loja. A maioria desses conceitos se aplicam aos namespaces **Windows.Services.Store** e **Windows.ApplicationModel.Store**.

Cada item que é oferecido na Loja é geralmente chamado de *produto*. A maioria dos desenvolvedores trabalha com os seguintes tipos de produtos: *aplicativos* e *complementos* (também conhecidos como produtos no aplicativo ou IAPs). Um complemento se refere a um produto ou recurso que você disponibiliza para os clientes no contexto de seu aplicativo. Um complemento pode representar qualquer funcionalidade que seu aplicativo oferece aos clientes: por exemplo, a moeda a ser usada em um aplicativo ou jogos, novos mapas ou armas para um jogo, a capacidade de usar seu aplicativo sem anúncios ou conteúdo digital, como músicas ou vídeos, para aplicativos que podem oferecer esse tipo de conteúdo.

Todos os aplicativos e complementos têm uma licença associada que indica se o usuário tem direito de usar o aplicativo ou complemento. Se o usuário tiver direito de usar o aplicativo ou complemento como avaliação, a licença também fornece informações adicionais sobre a avaliação.

Para oferecer um complemento para clientes em seu aplicativo, comece [definindo os complementos para seu aplicativo no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). Em seguida, escreva o código em seu aplicativo para determinar se o usuário tem uma licença para usar o recurso que é representado pelo complemento e ofereça o complemento para venda ao usuário como uma compra no aplicativo caso ele ainda não tenha uma licença para ele. Para obter exemplos que demonstram tarefas relacionadas que usam o namespace **Windows.Services.Store** em aplicativos destinados ao Windows 10, versão 1607 ou posterior, consulte os artigos a seguir:

* [Obter informações do produto para aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)

Para obter exemplos que demonstram tarefas relacionadas que usam o namespace **ApplicationModel**, consulte [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md).

Todos os desenvolvedores podem criar os tipos de complementos a seguir.

| Tipo de complemento |  Descrição  |
|---------|-------------------|
| Durável  |  Um complemento que dura o tempo de vida útil que você especificar no [painel do Centro de Desenvolvimento do Windows](https://msdn.microsoft.com/windows/uwp/publish/enter-iap-properties). <p/><p/>Por padrão, os complementos duráveis nunca expiram, podendo ser adquiridos somente uma vez. Se você especificar uma duração específica para o complemento, o usuário poderá comprar novamente o complemento depois que ele expirar.  |
| Consumível gerenciado pelo desenvolvedor  |  Um complemento que pode ser comprado, usado e comprado novamente. Esse tipo de complemento normalmente é usado para a moeda no aplicativo. <p/><p/>Para esse tipo de produto consumível, você é responsável por controlar o saldo do usuário de itens do usuário que o complemento representa e por relatar a compra do complemento como providenciada para a Loja depois que o usuário consome todos os itens. O usuário não pode comprar o complemento novamente até que seu aplicativo tenha informado a compra anterior do complemento como providenciada. <p/><p/>Por exemplo, se o seu complemento representar 100 moedas em um jogo e o usuário consumir 10 moedas, seu aplicativo ou o serviço deverá manter o novo saldo restante de 90 moedas para o usuário. Depois que o usuário tiver consumido todas as 100 moedas, seu aplicativo deverá declarar o complemento como providenciado e, em seguida, o usuário poderá comprar o complemento de 100 moedas novamente.    |
| Consumível gerenciado pela Loja  |  Um complemento que pode ser comprado, usado e comprado novamente. Esse tipo de complemento normalmente é usado para a moeda no aplicativo.<p/><p/>Para esse tipo de produto consumível, a Loja mantém o controle do saldo de itens do usuário que o complemento representa. Quando o usuário consome todos os itens, você é responsável por relatar esses itens como providenciados para a Loja, e a Loja atualiza o saldo do usuário. Seu aplicativo pode consultar o saldo atual para o usuário a qualquer momento. Depois que o usuário consome todos os itens, ele pode comprar o complemento novamente.  <p/><p/> Por exemplo, se o complemento representar uma quantidade inicial de 100 moedas em um jogo e o usuário consumir 10 moedas, o aplicativo relatará para a Loja que 10 unidades do complemento foram providenciadas, e a Loja atualizará o saldo restante. Depois que o usuário tiver consumido todas as 100 moedas, o usuário poderá comprar o complemento de 100 moedas novamente. <p/><p/> **Os consumíveis gerenciados pela Loja estão disponíveis a partir do Windows 10, versão 1607. A capacidade de criar um produto consumível gerenciado pela Loja no painel do Centro de Desenvolvimento do Windows estará disponível em breve.**  |

<span />

>**Observação**&nbsp;&nbsp;Outros tipos de complementos, como complementos duráveis com pacotes (também conhecidos como conteúdo para download ou DLC), estão disponíveis apenas para um conjunto restrito de desenvolvedores e não são abordados nesta documentação.

<span id="api_intro" />
## Introdução ao namespace Windows.Services.Store

O ponto de entrada principal para o namespace **Windows.Services.Store** é a classe [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx). Essa classe fornece métodos que você pode usar para obter informações do aplicativo atual e seus complementos disponíveis, comprar um aplicativo ou um complemento para o usuário atual, obter informações de licença do aplicativo atual ou suas complementos e outras tarefas. Para obter um objeto [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), siga um destes procedimentos:

* Em um aplicativo de usuário único (ou seja, um aplicativo que é executado somente no contexto do usuário que iniciou o aplicativo), use o método [GetDefault](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getdefault.aspx) para obter um objeto **StoreContext** que você possa usar para acessar e gerenciar os dados relacionados à Windows Store para o usuário. Os aplicativos da Plataforma Universal do Windows (UWP), em sua maioria, são aplicativos de usuário único.

  ```csharp
  Windows.Services.Store.StoreContext context = StoreContext.GetDefault();
  ```

* Em um aplicativo multiusuário, use o método [GetForUser](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.getforuser.aspx) para obter um objeto **StoreContext** que você possa usar para acessar e gerenciar os dados relacionados à Windows Store para um usuário específico que está conectado com sua conta da Microsoft enquanto estiver usando o aplicativo. Para obter mais informações sobre os aplicativos multiusuários, consulte [Introdução aos aplicativos multiusuários](https://msdn.microsoft.com/windows/uwp/xbox-apps/multi-user-applications). O exemplo a seguir obtém um objeto **StoreContext** para o primeiro usuário disponível.

  ```csharp
  var users = await Windows.System.User.FindAllAsync();
  Windows.Services.Store.StoreContext context = StoreContext.GetForUser(users[0]);
  ```

Depois que você tiver um [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx), você pode começar a chamar métodos para comprar um aplicativo ou um complemento para o usuário atual e outras tarefas. Para obter mais informações, consulte os seguintes artigos:

* [Obter informações do produto para aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)

Para obter um exemplo completo que demonstra como implementar avaliações e compras no aplicativo usando o namespace **Windows.Services.Store**, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Store).

<span />
### Modelo de objeto para produtos, SKUs e disponibilidades

Cada produto na Loja tem pelo menos uma *SKU*, e cada SKU tem pelo menos uma *disponibilidade*. Esses conceitos são abstraídos da maioria dos desenvolvedores no painel do Centro de Desenvolvimento do Windows, e a maioria dos desenvolvedores nunca definirão SKUs ou disponibilidades para seus aplicativos ou complementos. No entanto, como o objeto de modelo para produtos da Loja no namespace **Windows.Services.Store** inclui disponibilidades e SKUs, um entendimento básico sobre esses conceitos pode ser útil.

| Tipo de objeto |  Descrição  |
|---------|-------------------|
| Produto  |  A classe [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx) representa qualquer tipo de produto que esteja disponível na Loja, incluindo um aplicativo ou um complemento. Essa classe fornece propriedades que você pode usar para acessar dados, como a ID da Loja do produto, as imagens e os vídeos para a listagem da Loja e informações de preços. Também fornece métodos que você pode usar para comprar o produto. |
| SKU |  A classe [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) representa uma *SKU* de um produto. SKU é uma versão específica de um produto com sua própria descrição, preço e outros detalhes exclusivos do produto. Cada aplicativo ou complemento tem uma SKU padrão. O único momento em que a maioria dos desenvolvedores terá várias SKUs para um aplicativo é se publicarem uma versão completa do aplicativo e uma versão de avaliação (no catálogo da Loja, cada uma dessas versões é uma SKU diferente do mesmo aplicativo). <p/><p/> Alguns fornecedores podem definir suas próprias SKUs. Por exemplo, um grande fornecedor de jogos pode lançar um jogo com uma SKU que mostre sangue verde em mercados que não permitem sangue vermelho e outra SKU que mostre sangue vermelho nos demais mercados. Como alternativa, um fornecedor que vende conteúdo em vídeo digital pode publicar duas SKUs para um vídeo, uma SKU para a versão em alta definição e outra SKU para a versão em definição padrão. <p/><p/> Cada produto tem uma propriedade [Skus](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.skus.aspx) que você pode usar para acessar as SKUs. |
| Disponibilidade  |  A classe [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) representa uma *disponibilidade* para uma SKU. Disponibilidade é uma versão específica de uma SKU com suas próprias informações de preço. Cada SKU tem uma disponibilidade padrão. Alguns fornecedores podem definir suas próprias disponibilidade para apresentar opções de preço diferentes para determinada SKU. <p/><p/> Cada SKU tem uma propriedade [Availabilities](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.availabilities.aspx) que você pode usar para acessar as disponibilidades. Para a maioria dos desenvolvedores, cada SKU tem uma única disponibilidade padrão.  |

<span id="store_ids" />
### IDs da Loja

Todos os aplicativos e complementos na Loja têm uma **ID da Loja** associada. Muitas das APIs do namespace **Windows.Services.Store** exigem a ID da Loja para executar uma operação em um aplicativo ou complemento. Os produtos, SKUs e disponibilidades têm diversos formatos de ID da Loja.

| Tipo de objeto |  Formato da ID da Loja  |
|---------|-------------------|
| Produto  |  A ID da Loja de qualquer produto na Loja é uma cadeia de 12 caracteres alfanuméricos, como ```9NBLGGH4R315```. Essa ID da Loja está disponível na página do painel do Centro de Desenvolvimento do Windows para o aplicativo ou complemento, e ela é retornada pela propriedade [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.storeid.aspx) do objeto [StoreProduct](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeproduct.aspx). Essa ID é chamada, às vezes, de *ID da Loja do produto*. |
| SKU |  Para uma SKU, a ID da Loja tem o formato ```<product Store ID>/xxxx```, onde ```xxxx``` é uma cadeia de 4 caracteres alfanuméricos que identifica uma SKU do produto. Por exemplo, ```9NBLGGH4R315/000N```. Essa ID é retornada pela propriedade [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.storeid.aspx) de um objeto [StoreSku](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storesku.aspx) e é chamada, às vezes, de *ID da Loja para a SKU*. |
| Disponibilidade  |  Para obter uma disponibilidade, a ID da Loja tem o formato ```<product Store ID>/xxxx/yyyyyyyyyyyy```, onde ```xxxx``` é uma cadeia de 4 caracteres alfanuméricos que identifica uma SKU do produto e ```yyyyyyyyyyyy``` é uma cadeia de 12 caracteres alfanuméricos que identifica uma disponibilidade para a SKU. Por exemplo, ```9NBLGGH4R315/000N/4KW6QZD2VN6X```. Essa ID é retornada pela propriedade [StoreId](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.storeid.aspx) de um objeto [StoreAvailability](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storeavailability.aspx) e é chamada, às vezes, de *ID da Loja para a disponibilidade*.  |

<span id="testing" />
### Testando aplicativos que usam o namespace Windows.Services.Store

O namespace **Windows.Services.Store** não fornece uma classe que você pode usar para simular as informações de licença durante o teste. Em vez disso, você deve publicar um aplicativo na Loja e baixá-lo para seu dispositivo de desenvolvimento para usar sua licença para teste. Essa é uma experiência diferente de aplicativos que usam o namespace **Windows.ApplicationModel.Store**, pois esses aplicativos podem usar a classe [CurrentAppSimulator](https://msdn.microsoft.com/library/windows/apps/hh779766) para simular as informações de licença durante o teste

Se seu aplicativo usa APIs no namespace **Windows.Services.Store** para acessar informações de seu aplicativo e seus complementos, siga este processo para testar seu código:

1. Se seu aplicativo já estiver publicado e disponível na Loja e você quiser atualizá-lo para usar APIs no namespace **Windows.Services.Store**, estará pronto para começar. Se você quiser oferecer complementos para o aplicativo, certifique-se de [definir os complementos para seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions) no painel do Centro de Desenvolvimento.

  Se você ainda não tem um aplicativo publicado e disponível na Loja, crie um aplicativo básico que atenda aos requisitos mínimos do [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) e [envie esse aplicativo](https://msdn.microsoft.com/windows/uwp/publish/app-submissions) para o painel do Centro de Desenvolvimento do Windows. Se você quiser oferecer complementos para o aplicativo, certifique-se de [definir os complementos para seu aplicativo](https://msdn.microsoft.com/windows/uwp/publish/iap-submissions). Opcionalmente, você pode [ocultar o aplicativo da Loja](https://msdn.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability) durante o teste.

2. Escreva um código em seu aplicativo que use um dos métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) no namespace **Windows.Services.Store** para executar tarefas como [obter os complementos disponíveis para o aplicativo atual](get-product-info-for-apps-and-add-ons.md), [comprar um aplicativo ou um complemento](enable-in-app-purchases-of-apps-and-add-ons.md) ou [obter informações de licença para seu aplicativo](get-license-info-for-apps-and-add-ons.md). Consulte a seção de tópicos relacionados abaixo para obter mais exemplos.

3. No Visual Studio, clique no **menu Projeto**, aponte para **Loja** e clique em **Associar Aplicativo à Loja**. Siga as instruções do assistente para associar o projeto de aplicativo ao aplicativo em sua conta do Centro de Desenvolvimento do Windows que você deseja usar para teste.

  >**Observação**&nbsp;&nbsp;Se você não associar seu projeto a um aplicativo na Loja, os métodos [StoreContext](https://msdn.microsoft.com/library/windows/apps/windows.services.store.storecontext.aspx) definirão a propriedade **ExtendedError** dos valores de retorno como o valor de código de erro 0x803F6107. Esse valor indica que a Loja desconhece o aplicativo.

4. Se você não tiver feito isso ainda, instale o aplicativo da Loja que você especificou na etapa anterior, execute-o uma vez e, em seguida, feche-o. Isso garante que uma licença válida para o aplicativo seja instalada em seu dispositivo de desenvolvimento.

5. No Visual Studio, comece a executar ou depurar seu projeto. Seu código deverá recuperar dados do aplicativo e do complemento do aplicativo da Loja que você associou ao seu projeto local. Se você for solicitado a reinstalar o aplicativo, siga as instruções e execute ou depure seu projeto.

## Tópicos relacionados

* [Obter informações do produto para aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras de aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
* [Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store](in-app-purchases-and-trials-using-the-windows-applicationmodel-store-namespace.md)



<!--HONumber=Aug16_HO5-->


