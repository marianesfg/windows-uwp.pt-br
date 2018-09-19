---
author: jnHs
Description: The Windows Dev Center dashboard gives you the option to make your app available only to specified people so that you can have testers try it out before you offer it to the public.
title: Teste beta e distribuição direcionada
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 05/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, teste beta, distribuição limitada, beta, betas, teste, testadores
ms.localizationpriority: medium
ms.openlocfilehash: e453be22d752ed78263cb34011cdf9a333057e03
ms.sourcegitcommit: 68fcac3288d5698a13dbcbd57f51b30592f24860
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/19/2018
ms.locfileid: "4058170"
---
# <a name="beta-testing-and-targeted-distribution"></a>Teste beta e distribuição direcionada

Não importa se você testou seu aplicativo com cuidado extremo, não há nada como a experiência concreta de outras pessoas o testarem. Seus testadores podem descobrir problemas que você ignorou, como erros ortográficos, fluxo confuso do aplicativo ou erros que poderiam causar falhas no aplicativo. Você terá a chance de corrigir esses problemas antes de fazer o envio ao público, o que resultará em um produto final apurado. 

O painel do Centro de Desenvolvimento do Windows oferece diversas opções para disponibilizar um aplicativo apenas para pessoas específicas; assim, os testadores podem testá-lo antes que você o ofereça ao público. 

Seja qual for o método escolhido, aqui estão algumas coisas para ter em mente enquanto você realiza um teste beta em seu app.

- Você não poderá revogar o acesso ao app depois que os testadores o baixarem. Depois que eles baixarem o aplicativo, poderão continuar a usá-lo, e eles obterão as atualizações que você publicar posteriormente.
- Você precisa determinar como deseja receber feedback dos testadores. Considere fornecer um link no app beta para permitir que os testadores forneçam comentários por email ou por meio do [Hub de Feedback](../monetize/launch-feedback-hub-from-your-app.md), se confidencialidade não for um problema). 
- Você pode analisar os [relatórios analíticos](analytics.md) do aplicativo, incluindo relatórios de uso e integridade, além de classificações ou análises deixadas pelos testadores.
- Você pode incluir complementos quando você distribuir seu aplicativo aos testadores. Já que você provavelmente não deseja cobrá-los para um complemento, você poderá [gerar códigos promocionais](generate-promotional-codes.md) e distribuí-los aos seus testadores para permitir que eles obtenham o complemento gratuitamente, ou pode definir o preço do complemento como **Gratuito** durante os testes (então, antes de disponibilizar o app para outros clientes, crie um novo envio para o complemento para alterar o preço). Observe que cada complemento só pode ser comprado uma vez por cada conta da Microsoft, portanto o mesmo testador não poderá testar o processo de aquisição de complemento mais de uma vez. 
- Você pode dar aos testadores uma versão atualizada do seu app a qualquer momento. Basta criar um novo envio com novos pacotes. Os testadores receberão a atualização depois que ela passar pelo processo de certificação, da mesma forma como receberam o pacote original, mas ninguém mais poderá obtê-lo (a menos que você faça alterações adicionais, como mover um app da **Audiência particular** para a **Audiência pública** ou alterar a associação dos grupos que poderão obtê-lo).

## <a name="private-audience"></a>Audiência particular

Se você quiser permitir que os testadores usem seu aplicativo antes que ele esteja disponível para outras pessoas e certificar-se que ninguém mais possa ver sua listagem, use a opção **Audiência particular** em [Visibilidade](choose-visibility-options.md) (na página **Preço e disponibilidade** do seu envio). Esse é o único método que permite que você distribua seu app para os testadores enquanto impede totalmente que qualquer outra pessoa veja a listagem da Store do app, mesmo se eles forem capazes de digitar o link direto. 

A opção de **audiência** pode pode apenas ser usada quando você não tiver publicado já seu aplicativo para uma audiência pública. Você pode usar essa opção com aplicativos destinados a qualquer versão do sistema operacional, mas seus testadores devem estar executando o Windows 10, versão 1607 ou posterior (incluindo o Xbox One) e devem estar conectados com a conta da Microsoft associada ao endereço de email que você fornecer.

Para saber mais, consulte a [Audiência particular](choose-visibility-options.md#audience).


## <a name="package-flights"></a>Pacotes de pré-lançamento

Se você já publicou o aplicativo, é possível criar pacotes de pré-lançamento para distribuir um conjunto diferente de pacotes a pessoas específicas. Você pode até criar vários pacotes de pré-lançamento para o mesmo aplicativo a serem usados com diferentes grupos de pessoas. Esta é uma ótima maneira de experimentar diferentes pacotes simultaneamente e você pode extrair pacotes de uma versão de pré-lançamento para seu envio de versão completa, caso decida que os pacotes estão prontos para distribuição a todos os usuários.

Os pacotes de pré-lançamento podem ser usados com os apps destinados a qualquer versão do sistema operacional, mas seus testadores só poderão obter o app se eles estiverem executando o Windows.Desktop build 10586 ou posterior; o Windows.Mobile build 10586.63 ou posterior ou Xbox One.

Para saber mais, veja [Pacotes de pré-lançamento](package-flights.md).


<span id="hide" />

## <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Como ocultar o app na Store e usar códigos promocionais

Essa opção oferece outra maneira de limitar a distribuição de um aplicativo para apenas um determinado grupo de testadores, enquanto impede que outra pessoa descobrir seu aplicativo na loja (ou adquiri-lo sem um código promocional). No entanto, ao contrário da opção de audiência privada, seria possível para qualquer um ver a listagem do seu app se tivesse o link direto. Se confidencialidade for essencial para seu envio, é recomendável publicar para uma audiência particular em vez disso.

Ocultar o app e usar os códigos promocionais podem ser usados com os apps direcionados a qualquer versão do sistema operacional, mas seus testadores só poderão obter o app se estiverem executando o Windows 10.

Para usar esta opção:

- Na seção **Visibilidade** da página **Preço e disponibilidade**, em [Detectabilidade](choose-visibility-options.md#discoverability), selecione **Disponibilizar este produto mas não torná-lo detectável na Store**. Escolha a opção de **Parar a aquisição: os clientes com um link direto poderão ver a listagem da Loja do produto, mas só poderão baixá-lo se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10**. 
- Depois que o aplicativo for aprovado na certificação, [gere códigos promocionais](generate-promotional-codes.md) para o aplicativo e distribua-os aos seus testadores. Você pode gerar códigos que permitem até 1600 resgates para um único aplicativo em um período de seis meses. Esses códigos fornecem aos testadores um link direto para os detalhes do aplicativo e permitem que eles o baixem gratuitamente, mesmo se você tiver definido um preço para ele quando criou seu envio.
- Quando você estiver pronto para disponibilizar o aplicativo para o público, crie um novo envio e altere a opção **Visibilidade** para **Disponibilizar este produto e torná-lo detectável na Store** (juntamente com quaisquer outras alterações que você queira fazer).


## <a name="targeted-distribution-with-a-link-to-your-apps-listing"></a>Distribuição direcionada com um link para a listagem do app

Ao contrário das opções descritas acima, essa opção funciona para clientes no Windows Phone 8.1, bem como no Windows 10 (mas não no Windows 8. x). Nenhum cliente conseguirá encontrar o aplicativo pesquisando ou navegando na Store, mas qualquer pessoa com o link direto para a listagem da Store poderá baixá-lo em um dispositivo com o Windows Phone 8.1 ou anteriores ou o Windows 10. Tenha em mente que, para que os testadores possam baixar o aplicativo gratuitamente, você terá de definir o preço como **Gratuito**.

Para usar esta opção:
- Na seção **Visibilidade** da página **Preço e disponibilidade**, em [Detectabilidade](choose-visibility-options.md#discoverability), selecione **Disponibilizar este produto mas não torná-lo detectável na Store**. Escolha a opção **Somente link direto: qualquer cliente com um link direto para a listagem do produto pode baixá-lo, exceto no Windows 8.x.**.
- Depois que seu produto tiver sido publicado, distribua o link (a **URL** na [Página de identidade do app](view-app-identity-details.md)) para os testadores para que eles possam experimentá-lo.
- Quando você estiver pronto para disponibilizar o aplicativo para o público, crie um novo envio e altere a opção **Visibilidade** para **Disponibilizar este produto e torná-lo detectável na Store** (juntamente com quaisquer outras alterações que você queira fazer).


## <a name="targeted-distribution-to-windows-phone-customers-with-specified-email-addresses"></a>Distribuição direcionada a clientes do Windows Phone com endereços de email especificados

> [!IMPORTANT]
> Essa opção não está disponível para novos envios. Se anteriormente você tiver selecionado essa opção para um app destinado ao Windows Phone 8.1 ou versões anteriores, você poderá continuar a usá-la para o app. Você pode fazer alterações na lista de testadores (até 10.000) ao criar um novo envio. 

Com essa opção, as pessoas com os endereços de email que você especificou podem baixar seu app (somente em um dispositivo com Windows Phone 8.1 ou versões anteriores) usando o link direto para a listagem. Outro clientes não poderão baixar o app, mesmo que tenham o link, e eles não poderão encontrar o app na Store pesquisando ou navegando. Para que os testadores baixem o aplicativo, você precisará dar a eles o link (a **URL** na [Página de identidade do app](view-app-identity-details.md)), e eles devem estar conectados com uma conta da Microsoft associada a um endereço de email fornecido. Você também pode tornar o app disponível para os testadores em dispositivos Windows 10 por meio da [geração de códigos promocionais](generate-promotional-codes.md); qualquer pessoa com um dos códigos promocionais do seu app poderá baixá-lo em um dispositivo Windows 10, mesmo que você não tenha inserido o email deles aqui.
