---
author: mcleanbyron
ms.assetid: 32572890-26E3-4FBB-985B-47D61FF7F387
description: "Saiba como habilitar compras no aplicativo e avaliações em aplicativos UWP destinados a versões antes do Windows 10, versão 1607."
title: "Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store"
translationtype: Human Translation
ms.sourcegitcommit: 812fa1789c5c86657b8e73e45a851c7a58a1c84e
ms.openlocfilehash: 5a4f943357660a22217351f04d735c14cab828ff

---

# Compras no aplicativo e avaliações usando o namespace Windows.ApplicationModel.Store

O SDK do Windows fornece membros no namespace [ApplicationModel](https://msdn.microsoft.com/library/windows/apps/windows.applicationmodel.store.aspx) que você pode usar para adicionar compras no aplicativo e a funcionalidade de avaliação ao seu aplicativo da Plataforma Universal do Windows (UWP) para ajudar a monetizar seu aplicativo e adicionar nova funcionalidade. Essas APIs também fornecem acesso às informações de licença de seu aplicativo.

Os artigos nesta seção fornecem orientações detalhadas e exemplos de código para usar os membros em membros no namespace **Windows.ApplicationModel.Store** para vários cenários comuns. Para uma visão geral dos conceitos relacionados a compras no aplicativo em aplicativos UWP, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).

Para obter um exemplo completo que demonstra como implementar avaliações e compras no aplicativo usando o namespace **Windows.ApplicationModel.Store**, consulte o [Exemplo da Loja](https://github.com/Microsoft/Windows-universal-samples/tree/win10-1507/Samples/Store).

>**Observações**&nbsp;&nbsp;
>
> * Se seu aplicativo for destinado ao Windows 10, versão 1607 ou posterior, recomendamos que você use membros do namespace [Windows.Services.Store](https://msdn.microsoft.com/library/windows/apps/windows.services.store.aspx), em vez do namespace **Windows.ApplicationModel.Store**. O namespace **Windows.Services.Store** dá suporte aos tipos de complemento mais recentes, como complementos consumíveis gerenciados pela Loja, e foi projetado para ser compatível com tipos de produtos e recursos futuros com suporte do Centro de Desenvolvimento do Windows e da Loja. O namespace **Windows.Services.Store** também foi projetado para ter um desempenho melhor. Para obter mais informações, consulte [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md).
<br/><br/>
> * O namespace **ApplicationModel** não tem suporte em aplicativos da área de trabalho do Windows que usam o [Desktop Bridge](https://developer.microsoft.com/windows/bridges/desktop). Esses aplicativos devem usar o namespace **Windows.Services.Store** para implementar compras no aplicativo e avaliações.

## Nesta seção


| Tópico                                                                                                       | Descrição                 |
|-------------------------------------------------------------------------------------------------------------|-----------------------------|
| [Habilitar compras de produtos no aplicativo](enable-in-app-product-purchases.md)      |  Seja seu aplicativo gratuito ou não, você pode vender conteúdo, outros aplicativos ou uma nova funcionalidade do aplicativo (como o desbloqueio do próximo nível de um jogo) no próprio aplicativo. Veja a seguir como habilitar esses produtos no seu aplicativo.  |
| [Habilitar compras de produtos consumíveis no aplicativo](enable-consumable-in-app-product-purchases.md)      | Ofereça produtos consumíveis no aplicativo — itens que podem ser comprados, usados e comprados novamente — por meio da plataforma de comércio da Loja para proporcionar aos seus clientes uma experiência de compra robusta e confiável. Isso é especialmente útil para coisas como moedas em jogos (ouro, moedas, etc.) que podem ser compradas e então usadas para comprar power-ups específicos. |
| [Excluir ou limitar recursos em uma versão de avaliação](exclude-or-limit-features-in-a-trial-version-of-your-app.md) | Se você permitir que os clientes usem seu aplicativo gratuitamente durante um período de avaliação, incentive-os a atualizar para a versão completa do aplicativo, excluindo ou limitando alguns recursos durante o período de avaliação. |
| [Gerenciar um catálogo abrangente de produtos no aplicativo](manage-a-large-catalog-of-in-app-products.md)      |   Se o seu aplicativo oferecer um catálogo abrangente de produtos no aplicativo, você também poderá seguir o processo descrito neste tópico para ajudar a gerenciar seu catálogo.    |
| [Usar recibos para verificar compras de produtos](use-receipts-to-verify-product-purchases.md)      |   Cada transação da Windows Store que resulta em uma compra do produto bem-sucedida pode retornar um recibo de transação que fornece informações sobre o produto listado e o custo monetário ao cliente. Ter acesso a essas informações dá suporte a cenários nos quais seu aplicativo precisa confirmar que um usuário adquiriu seu aplicativo ou fez compras de produto no aplicativo da Windows Store. |



<!--HONumber=Nov16_HO1-->


