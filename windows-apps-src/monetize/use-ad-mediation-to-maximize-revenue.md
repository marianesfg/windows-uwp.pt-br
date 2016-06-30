---
author: mcleanbyron
ms.assetid: 772DEBF2-1578-4330-9C14-70BCC6F55005
description: "A Microsoft dá suporte ao controle de anúncios, o que permite otimizar a receita de publicidade no aplicativo com o controle de solicitações de anúncios em faixa de várias redes de publicidade."
title: "Usar controle de anúncios para maximizar a receita"
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: c0669e35b285ee7dfeda0c039d8455a4237960f5

---

#  Usar controle de anúncios para maximizar a receita


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

A Microsoft dá suporte ao controle de anúncios, o que permite otimizar a receita de publicidade no aplicativo com o controle de solicitações de anúncios em faixa de várias redes de publicidade. Redes de publicidade diferentes podem ter seus pontos fortes, com algumas tendo um custo mais alto por milhares de visualizações (eCPM) ou uma taxa de preenchimento mais alta (porcentagem de anúncios veiculados quando seu aplicativo faz uma solicitação) em certos mercados do que outras. Com uma única rede de publicidade, você pode acabar com solicitações de anúncios não processadas, o que lhe causará perda de receita em potencial. Com o controle de anúncios, você maximiza a monetização de anúncios ao garantir a exibição constante de um anúncio ativo.

O suporte ao controle de anúncios está disponível por meio do [SDK do Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk). Depois de instalar o SDK e adicionar o controle de anúncios ao seu aplicativo, você poderá especificar a frequência com que cada rede é usada atualizando sua configuração de controle no Centro de Desenvolvimento. Você pode otimizar isso por mercado, para poder usar as redes de anúncios mais eficazes nas regiões apropriadas. E você pode mudar a forma como cada rede de publicidade é usada sem precisar republicar seu aplicativo.

## Introdução ao controle de anúncios


Siga estas etapas para instalar e configurar o controle de anúncios em seu aplicativo:

1.  Examine a lista de redes de publicidade e tipos de projetos compatíveis com o controle de anúncios, configure contas com as redes de publicidade que você deseja usar e siga as instruções de cada rede de publicidade para carregar um aplicativo. Para obter mais informações, consulte [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md).

2.  Instale o [SDK do Microsoft Store Engagement and Monetization](http://aka.ms/store-em-sdk) com o Visual Studio 2015 ou o Visual Studio 2013.

3.  No Visual Studio, abra o projeto ou crie um novo projeto. Abra a página onde deseja hospedar anúncios e arraste um **AdMediatorControl** para a página. Se estiver usando o Microsoft Advertising, ajuste a altura e a largura do controle para adaptá-lo aos tamanhos de anúncios com suporte. Para obter mais informações, consulte [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md).

4.  Execute **Serviços Conectados** para escolher as redes de publicidade às quais você se dirige e configurar os parâmetros necessários para cada rede. Para obter mais informações, consulte [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md).

5.  Teste a implementação de controle de anúncios no aplicativo. Para obter mais informações, consulte [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md).

6.  Envie seu aplicativo ao painel do Centro de Desenvolvimento do Windows e defina as configurações de controle de anúncios no painel. Para obter mais informações, consulte [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md).

7.  Analise seus relatórios de controle de anúncios no painel do Centro de Desenvolvimento. Para saber mais, consulte [Relatório de controle de anúncios](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Usando o Microsoft Advertising sem o controle de anúncios


Se você não quiser usar o controle de anúncios ou se o tipo de projeto atualmente não for compatível com o controle de anúncios, você ainda poderá veicular anúncios do Microsoft Advertising sem usar o controle de anúncios. Para saber mais, consulte [AdControl em XAML e .NET](https://msdn.microsoft.com/library/mt313186.aspx) e [AdControl em HTML 5 e JavaScript](https://msdn.microsoft.com/library/mt313130.aspx).

## Tópicos relacionados

* [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md)
* [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md)
* [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md)
* [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de controle de anúncios](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


