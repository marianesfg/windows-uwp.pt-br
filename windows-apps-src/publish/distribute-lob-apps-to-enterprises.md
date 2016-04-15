---
Description: Você pode publicar aplicativos de linha de negócios (LOB) diretamente para empresas, para aquisição de volume, usando a Windows Store para Empresas sem tornar os aplicativos amplamente disponíveis na Loja.
title: Distribuir aplicativos LOB para empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
---

# Distribuir aplicativos LOB para empresas


Você pode publicar aplicativos de linha de negócios (LOB) diretamente para empresas, para aquisição de volume, usando a Windows Store para Empresas sem tornar os aplicativos amplamente disponíveis na Loja.

> **Importante**  No momento, somente aplicativos gratuitos podem ser distribuídos exclusivamente a empresas por meio da Loja para Empresas. Se você enviar um aplicativo pago como LOB, ele ficará disponível para a empresa em uma data posterior, uma vez que seja adicionado suporte para a aquisição de aplicativos pagos em volume. 

## Configurando a associação empresarial


A primeira etapa na publicação de aplicativos LOB exclusivamente para uma empresa é estabelecer a associação entre sua conta e o repositório particular da empresa.

> **Importante**  Esse processo de associação deve ser iniciado pela empresa. Para saber mais, consulte [Trabalhando com aplicativos de linha de negócios](http://go.microsoft.com/fwlink/p/?LinkId=698846).

Quando uma empresa optar por convidá-lo a publicar aplicativos para seu uso exclusivo, você receberá um email que inclui um link para confirmar a associação. Você também pode confirmar essas associações acessando a seção **Associações empresariais** de suas **Configurações de conta**.

Para confirmar a associação, clique em **Aceitar**. Sua conta estará, em seguida, apta a publicar aplicativos para uso exclusivo da empresa.

## Enviando um aplicativo LOB


Quando você estiver pronto para publicar um aplicativo para uso exclusivo de uma empresa, o processo será semelhante ao processo de envio do aplicativo. O aplicativo passa pelo mesmo processo de certificação, e deve estar em conformidade com todas as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944). Apenas duas partes do processo são diferentes.

### Distribuição e visibilidade

Depois de configurar uma associação empresarial, sempre que enviar um aplicativo, você verá uma caixa de lista suspensa na seção **Distribuição e visibilidade** da página **Preço e disponibilidade** do envio. Por padrão, essa opção é definida como **Retail distribution**. Para tornar o aplicativo exclusivo de uma empresa, você precisará escolher **Line-of-business (LOB) distribution**.

Uma vez que **Line-of-business (LOB) distribution** seja selecionado, as opções comuns de **Distribuição e a visibilidade** serão substituídas por uma lista das empresas para as quais você pode publicar aplicativos exclusivos.

Selecione as empresas que poderão obter o aplicativo. Nenhuma outra poderá acessá-lo.

> **Observação**  Pelo menos uma empresa deve ser selecionada para publicar um aplicativo como linha de negócios.

### Licenciamento para organizações

Por padrão, a caixa para **licenciamento por volume gerenciado pela Loja (online)** é marcada quando você envia um aplicativo. Na publicação de aplicativos LOB, essa caixa deve ser marcada, para que a empresa possa adquirir seu aplicativo em volume. Isso não tornará o aplicativo disponível para qualquer pessoa fora das empresas que você selecionou na seção **Distribuição e visibilidade**.

Se você quiser disponibilizar o aplicativo para a empresa por meio de licenciamento desconectado (offline), poderá marcar também a caixa **Licenciamento desconectado (offline)**.

Para saber mais, veja [Opções de licenciamento para organizações](organizational-licensing.md).

### Implantação empresarial de aplicativos LOB

Depois de clicar em **Enviar à Loja**, o aplicativo passará pelo processo de certificação. Quando estiver pronto, um administrador da empresa deverá adicioná-lo ao seu repositório particular no portal da Windows Store para Empresas. A empresa poderá então implantar o aplicativo para seus usuários.

Para saber mais, veja [Trabalhando com aplicativos de linha de negócios](http://go.microsoft.com/fwlink/p/?LinkId=698846) e [Distribuir aplicativos usando seu repositório particular](http://go.microsoft.com/fwlink/p/?LinkId=698847).

### Atualizando aplicativos LOB

Para publicar atualizações de um aplicativo que você já publicou como LOB, basta criar um novo envio. Você pode carregar novos pacotes ou fazer quaisquer outras alterações e, em seguida, clicar em **Enviar à Loja** para disponibilizar a versão atualizada. Certifique-se de manter as mesmas seleções da empresa em **Distribuição e visibilidade** (a menos que você queira realmente alterá-las, como, por exemplo, selecionar uma empresa adicional para adquirir o aplicativo ou remover uma das empresas às quais você o distribuiu anteriormente).

Se quiser parar de oferecer um aplicativo publicado anteriormente como linha de negócios, e impedir novas aquisições, você deverá criar um novo envio. Primeiro, você precisará alterar sua seleção em **Distribuição e visibilidade** de **Line-of-business (LOB) distribution** para **Retail distribution**. Em seguida, nas opções de **Distribuição e visibilidade**, escolha **Hide this app and prevent acquisition**. Depois que o envio passar pelo processo de certificação, o aplicativo não estará mais disponível para novas aquisições (embora quem já o tiver possa continuar a usá-lo).

### Distribuir aplicativos LOB por meio de sideload

A disponibilização de aplicativos por meio da Loja para Negócios garante que o aplicativo foi assinado pela Loja e está em conformidade com as Políticas da Loja padrão.

Em alguns casos, as empresas podem não querer que seus aplicativos LOB sejam enviado pelo Centro de Desenvolvimento do Windows por vários motivos (como motivos relacionados a conformidade ou aplicativos que precisam de recursos adicionais). Nesse caso, as empresas podem implantar os aplicativos diretamente nos computadores por meio de sideload, sem usar a Windows Store para Empresas.

Para saber mais, veja [Sideload de aplicativos LOB no Windows 10](http://go.microsoft.com/fwlink/p/?LinkId=623433).

 

 






<!--HONumber=Mar16_HO1-->


