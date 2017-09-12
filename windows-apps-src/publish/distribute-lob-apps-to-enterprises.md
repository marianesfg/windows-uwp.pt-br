---
author: jnHs
Description: "Você pode publicar aplicativos LOB (linha de negócios) diretamente para empresas para aquisição de grande volume pela Microsoft Store para Empresas ou Microsoft Store para Educação, sem tornar os aplicativos amplamente disponíveis na Loja."
title: Distribuir os aplicativos LOB para empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, lob, linha de negócios, aplicativos corporativos, store para empresas, store para educação"
ms.openlocfilehash: 5dab364d495334d102e73550e5d879d2f530d44e
ms.sourcegitcommit: b42d14c775efbf449a544ddb881abd1c65c1ee86
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/20/2017
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuir os aplicativos LOB para empresas


Você pode publicar aplicativos LOB (linha de negócios) diretamente para empresas para aquisição de grande volume pela Microsoft Store para Empresas ou Microsoft Store para Educação, sem tornar os aplicativos amplamente disponíveis na Loja.

> [!NOTE]
> No momento, somente aplicativos gratuitos podem ser distribuídos exclusivamente para empresas por meio da Microsoft Store para Empresas ou da Microsoft Store para Educação. Se você enviar um aplicativo pago como LOB, ele não estará disponível para a empresa no momento. 


## <a name="set-up-the-enterprise-association"></a>Configurar a associação empresarial

A primeira etapa na publicação de aplicativos LOB exclusivamente para uma empresa é estabelecer a associação entre sua conta e o repositório particular da empresa.

> [!IMPORTANT]
> Esse processo de associação deve ser iniciado pela empresa e deve usar o endereço de email que consta nas **Informações de contato** da conta. Para saber mais, consulte [Trabalhando com aplicativos de linha de negócios](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Quando uma empresa optar por convidá-lo a publicar aplicativos para seu uso exclusivo, você receberá um email que inclui um link para confirmar a associação. Você também pode confirmar essas associações acessando a seção **Associações empresariais** de suas **Configurações de conta**.

Para confirmar a associação, clique em **Aceitar**. Sua conta estará, em seguida, apta a publicar aplicativos para uso exclusivo da empresa.


## <a name="submit-lob-apps"></a>Enviar aplicativos LOB

Quando você estiver pronto para publicar um aplicativo para uso exclusivo de uma empresa, o processo será semelhante ao processo de envio do aplicativo. O aplicativo passa pelo mesmo [processo de certificação](the-app-certification-process.md) e deve estar em conformidade com todas as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944). Apenas poucas partes do processo são diferentes.


### <a name="visibility"></a>Visibilidade

Depois de configurar uma associação empresarial, sempre que enviar um aplicativo, você verá uma caixa de lista suspensa na seção **Visibilidade** da página **Preço e disponibilidade** do envio. Por padrão, essa opção é definida como **Retail distribution**. Para tornar o aplicativo exclusivo de uma empresa, você precisará escolher **Line-of-business (LOB) distribution**.

Depois que **Distribuição de linha de negócios (LOB)** for selecionada, as opções comuns de **Visibilidade** serão substituídas por uma lista das empresas para as quais você pode publicar aplicativos exclusivos. Ninguém fora as empresas selecionadas será capaz de exibir ou baixar o aplicativo.

Você deve selecionar pelo menos uma empresa para publicar um aplicativo como linha de negócios.

<span id="organizational" />
### <a name="organizational-licensing"></a>Licenciamento para organizações

Por padrão, a caixa para **licenciamento por volume gerenciado pela Loja (online)** é marcada quando você envia um aplicativo. Na publicação de aplicativos LOB, essa caixa permanecer marcada, para que a empresa possa adquirir seu aplicativo em volume. Isso não tornará o aplicativo disponível para qualquer pessoa fora das empresas que você selecionou na seção **Distribuição e visibilidade**.

Se você quiser disponibilizar o aplicativo para a empresa por meio de licenciamento desconectado (offline), poderá marcar também a caixa **Licenciamento desconectado (offline)**.

Para saber mais, consulte [Opções de licenciamento para organizações](organizational-licensing.md).


### <a name="age-ratings"></a>Classificações etárias

Para aplicativos LOB, a etapa de [classificação etária](age-ratings.md) do processo de envio funciona da mesma forma para aplicativos de varejo, mas você também tem uma opção adicional que permite que você indique a classificação etária da loja de classificação do seu aplicativo manualmente em vez de concluir o questionário ou importar uma ID de classificação IARC existente. Essa classificação manual pode ser usada somente com a distribuição LOB, portanto, se você mudar a configuração de **Visibilidade** do aplicativo para **Distribuição comercial**, é necessário fazer o questionário de classificação etária antes de publicar o envio.


## <a name="enterprise-deployment-of-lob-apps"></a>Implantação empresarial de aplicativos LOB

Depois de clicar em **Enviar à Loja**, o aplicativo passará pelo processo de certificação. Quando estiver pronto, um administrador da empresa deverá adicioná-lo ao seu repositório particular no portal da Microsoft Store para Empresas ou Microsoft Store para Educação. A empresa poderá então implantar o aplicativo para seus usuários.

> [!NOTE]
> Para obter o aplicativo LOB, a organização deve estar localizada em um [mercado com suporte](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets), e você não deve ter [excluído esse mercado](define-pricing-and-market-selection.md) ao enviar o aplicativo. 

Para saber mais, veja [Trabalhando com aplicativos de linha de negócios](http://go.microsoft.com/fwlink/p/?LinkId=698846) e [Distribuir aplicativos usando seu repositório particular](http://go.microsoft.com/fwlink/p/?LinkId=698847).


## <a name="update-lob-apps"></a>Atualizar aplicativos LOB

Para publicar atualizações de um aplicativo que você já publicou como LOB, basta criar um novo envio. Você pode carregar novos pacotes ou fazer quaisquer outras alterações e, em seguida, clicar em **Enviar à Loja** para disponibilizar a versão atualizada. Certifique-se de manter as mesmas seleções da empresa em **Visibilidade**, a menos que você queira realmente alterá-las, como, por exemplo, selecionar uma empresa adicional para adquirir o aplicativo ou remover uma das empresas às quais você o distribuiu anteriormente.

Se quiser parar de oferecer um aplicativo publicado anteriormente como linha de negócios, e impedir novas aquisições, você deverá criar um novo envio. Primeiro, você precisará alterar sua seleção em **Visibilidade** de **Distribuição de linha de negócios (LOB)** para **Distribuição comercial**. Em seguida,na seção **Visibilidade**, selecione **Disponibilizar este produto mas não torná-lo detectável na Loja** com a opção **Parar aquisição**.

Depois que o envio passar pelo processo de certificação, o aplicativo não estará mais disponível para novas aquisições (embora quem já o tiver possa continuar a usá-lo).

> [!NOTE]
> Ao alterar um aplicativo para **Distribuição comercial**, você deverá completar o [questionário de classificação etária](age-ratings.md), se ainda não tiver preenchido anteriormente (mesmo se o aplicativo não for disponibilizado para compra).


## <a name="distribute-lob-apps-through-sideloading"></a>Distribuir aplicativos LOB por meio de sideload

A disponibilização de aplicativos para uma empresa pela Microsoft Store para Empresas ou Microsoft Store para Educação garante que o aplicativo foi assinado pela Loja e está em conformidade com as Políticas da Loja padrão.

Em alguns casos, as empresas podem não querer que seus aplicativos LOB sejam enviado pelo Centro de Desenvolvimento do Windows (como motivos relacionados a conformidade ou aplicativos que precisam de recursos adicionais). Nesse caso, a empresa pode implantar os aplicativos diretamente nos computadores por meio de sideload, sem usar a Microsoft Store para Empresas ou a Microsoft Store para Educação.

Para saber mais, veja [Sideload de aplicativos LOB no Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 




