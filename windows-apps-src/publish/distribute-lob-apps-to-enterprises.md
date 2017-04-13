---
author: jnHs
Description: "Você pode publicar aplicativos de linha de negócios (LOB) diretamente para empresas, para aquisição de volume, usando a Windows Store para Empresas sem tornar os aplicativos amplamente disponíveis na Loja."
title: Distribuir os aplicativos LOB para empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: e29809f34facafc442b9b26580b91e17ed0a364e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuir os aplicativos LOB para empresas


Você pode publicar aplicativos de linha de negócios (LOB) diretamente para empresas, para aquisição de volume, usando a Windows Store para Empresas sem tornar os aplicativos amplamente disponíveis na Loja.

> **Importante**  No momento, somente aplicativos gratuitos podem ser distribuídos exclusivamente a empresas por meio da Windows Store para Empresas. Se você enviar um aplicativo pago como LOB, ele não estará disponível para a empresa no momento. 

## <a name="setting-up-the-enterprise-association"></a>Configurando a associação empresarial


A primeira etapa na publicação de aplicativos LOB exclusivamente para uma empresa é estabelecer a associação entre sua conta e o repositório particular da empresa.

> **Importante**  Esse processo de associação deve ser iniciado pela empresa, e deve ser enviado para o endereço de email que consta das **Informações de contato** da sua conta. Para saber mais, consulte [Trabalhando com aplicativos de linha de negócios](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Quando uma empresa optar por convidá-lo a publicar aplicativos para seu uso exclusivo, você receberá um email que inclui um link para confirmar a associação. Você também pode confirmar essas associações acessando a seção **Associações empresariais** de suas **Configurações de conta**.

Para confirmar a associação, clique em **Aceitar**. Sua conta estará, em seguida, apta a publicar aplicativos para uso exclusivo da empresa.

## <a name="submitting-an-lob-app"></a>Enviando um aplicativo LOB


Quando você estiver pronto para publicar um aplicativo para uso exclusivo de uma empresa, o processo será semelhante ao processo de envio do aplicativo. O aplicativo passa pelo mesmo processo de certificação, e deve estar em conformidade com todas as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944). Apenas poucas partes do processo são diferentes.

### <a name="distribution-and-visibility"></a>Distribuição e visibilidade

Depois de configurar uma associação empresarial, sempre que enviar um aplicativo, você verá uma caixa de lista suspensa na seção **Distribuição e visibilidade** da página **Preço e disponibilidade** do envio. Por padrão, essa opção é definida como **Retail distribution**. Para tornar o aplicativo exclusivo de uma empresa, você precisará escolher **Line-of-business (LOB) distribution**.

Uma vez que **Line-of-business (LOB) distribution** seja selecionado, as opções comuns de **Distribuição e a visibilidade** serão substituídas por uma lista das empresas para as quais você pode publicar aplicativos exclusivos.

Selecione as empresas que poderão obter o aplicativo. Nenhuma outra poderá acessá-lo.

> **Observação**  Pelo menos uma empresa deve ser selecionada para publicar um aplicativo como linha de negócios.

### <a name="organizational-licensing"></a>Licenciamento para organizações

Por padrão, a caixa para **licenciamento por volume gerenciado pela Loja (online)** é marcada quando você envia um aplicativo. Na publicação de aplicativos LOB, essa caixa deve ser marcada, para que a empresa possa adquirir seu aplicativo em volume. Isso não tornará o aplicativo disponível para qualquer pessoa fora das empresas que você selecionou na seção **Distribuição e visibilidade**.

Se você quiser disponibilizar o aplicativo para a empresa por meio de licenciamento desconectado (offline), poderá marcar também a caixa **Licenciamento desconectado (offline)**.

Para saber mais, consulte [Opções de licenciamento para organizações](organizational-licensing.md).

### <a name="age-ratings"></a>Classificações etárias
Para aplicativos LOB, a etapa de [classificação etária](age-ratings.md) do processo de envio funciona da mesma forma para aplicativos de varejo, mas você também tem uma opção adicional que permite que você indique a classificação etária da loja de classificação do seu aplicativo manualmente em vez de concluir o questionário ou importar uma ID de classificação IARC existente. Essa classificação manual só pode ser usada com a distribuição LOB, portanto, se você mudar a configuração de **Distribuição e visibilidade** do aplicativo para **Distribuição comercial**, você precisará fazer o questionário de classificação etária antes de publicar o envio.

### <a name="enterprise-deployment-of-lob-apps"></a>Implantação empresarial de aplicativos LOB

Depois de clicar em **Enviar à Loja**, o aplicativo passará pelo processo de certificação. Quando estiver pronto, um administrador da empresa deverá adicioná-lo ao seu repositório particular no portal da Windows Store para Empresas. A empresa poderá então implantar o aplicativo para seus usuários.

> **Observação** Para obter seu aplicativo LOB, a organização deve estar localizada em um [mercado com suporte](https://technet.microsoft.com/itpro/windows/whats-new/windows-store-for-business-overview#supported-markets), e você deve não ter excluído esse mercado ao enviar seu aplicativo. 

Para saber mais, veja [Trabalhando com aplicativos de linha de negócios](http://go.microsoft.com/fwlink/p/?LinkId=698846) e [Distribuir aplicativos usando seu repositório particular](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### <a name="updating-lob-apps"></a>Atualizando aplicativos LOB

Para publicar atualizações de um aplicativo que você já publicou como LOB, basta criar um novo envio. Você pode carregar novos pacotes ou fazer quaisquer outras alterações e, em seguida, clicar em **Enviar à Loja** para disponibilizar a versão atualizada. Certifique-se de manter as mesmas seleções da empresa em **Distribuição e visibilidade** (a menos que você queira realmente alterá-las, como, por exemplo, selecionar uma empresa adicional para adquirir o aplicativo ou remover uma das empresas às quais você o distribuiu anteriormente).

Se quiser parar de oferecer um aplicativo publicado anteriormente como linha de negócios, e impedir novas aquisições, você deverá criar um novo envio. Primeiro, você precisará alterar sua seleção em **Distribuição e visibilidade** de **Line-of-business (LOB) distribution** para **Retail distribution**. Em seguida, nas opções de **Distribuição e visibilidade**, escolha **Hide this app and prevent acquisition**. Depois que o envio passar pelo processo de certificação, o aplicativo não estará mais disponível para novas aquisições (embora quem já o tiver possa continuar a usá-lo).

> **Observação** Ao alterar um aplicativo para **Distribuição comercial**, você precisará completar o [questionário de classificação etária](age-ratings.md), se ainda não tiver preenchido anteriormente (mesmo se o app não for disponibilizado para compra).

### <a name="distributing-lob-apps-through-sideloading"></a>Distribuir aplicativos LOB por meio de sideload

A disponibilização de aplicativos por meio da Loja para Negócios garante que o aplicativo foi assinado pela Loja e está em conformidade com as Políticas da Loja padrão.

Em alguns casos, as empresas podem não querer que seus aplicativos LOB sejam enviado pelo Centro de Desenvolvimento do Windows por vários motivos (como motivos relacionados a conformidade ou aplicativos que precisam de recursos adicionais). Nesse caso, as empresas podem implantar os aplicativos diretamente nos computadores por meio de sideload, sem usar a Windows Store para Empresas.

Para saber mais, veja [Sideload de aplicativos LOB no Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 




