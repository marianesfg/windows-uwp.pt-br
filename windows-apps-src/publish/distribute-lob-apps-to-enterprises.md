---
Description: Você pode publicar aplicativos LOB (linha de negócios) diretamente para empresas para aquisição de grande volume pela Microsoft Store para Empresas ou Microsoft Store para Educação, sem tornar os aplicativos amplamente disponíveis na Loja.
title: Distribuir aplicativos LOB para empresas
ms.assetid: 2050126E-CE49-4DE3-AC2B-A572AC895158
ms.date: 01/16/2020
ms.topic: article
keywords: windows 10, uwp, lob, linha de negócios, aplicativos corporativos, store para empresas, store para educação, empresa
ms.localizationpriority: medium
ms.openlocfilehash: faf750ece274776a147dff9e825f32534eb13014
ms.sourcegitcommit: 7a8aea567b26283c71420e0d305d78f675e1fba7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/16/2020
ms.locfileid: "76125676"
---
# <a name="distribute-lob-apps-to-enterprises"></a>Distribuir aplicativos LOB para empresas

Você tem várias opções para distribuir aplicativos LOB (linha de negócios) para os usuários da sua organização usando [pacotes MSIX](https://docs.microsoft.com/windows/msix/) sem tornar os aplicativos amplamente disponíveis para o público. Você pode usar as ferramentas de gerenciamento de dispositivos, configurar uma implantação baseada no instalador de aplicativos, Sideload os aplicativos diretamente ou publicar os aplicativos no Microsoft Store for Business ou Microsoft Store para educação.

## <a name="microsoft-endpoint-configuration-manager-and-microsoft-intune"></a>Microsoft Endpoint Configuration Manager e Microsoft Intune

Se sua organização usa o Microsoft Endpoint Configuration Manager ou Microsoft Intune para gerenciar dispositivos, você pode implantar aplicativos LOB usando essas ferramentas. Para saber mais, confira estes tópicos:

* [Introdução ao gerenciamento de aplicativos no Configuration Manager](https://docs.microsoft.com/configmgr/apps/understand/introduction-to-application-management)
* [Visão geral do ciclo de vida do aplicativo no Microsoft Intune](https://docs.microsoft.com/intune/apps/app-lifecycle)

## <a name="app-installer"></a>Instalador de Aplicativo

O instalador do aplicativo permite que os aplicativos do Windows 10 sejam instalados clicando duas vezes em um pacote do aplicativo MSIX diretamente ou clicando duas vezes em um arquivo. AppInstaller que instala o pacote do aplicativo de um servidor Web. Isso significa que os usuários não precisam usar o PowerShell ou outras ferramentas de desenvolvedor para instalar aplicativos LOB. O instalador do aplicativo também pode instalar pacotes de aplicativos que incluem pacotes opcionais e conjuntos relacionados.

O Instalador de Aplicativo pode ser baixado para uso offline na empresa por meio do [portal da Web](https://businessstore.microsoft.com/store/details/app-installer/9NBLGGH4NNS1) da Microsoft Store para Empresas. Para obter mais informações sobre o instalador de aplicativos, consulte [instalar aplicativos do Windows 10 com o instalador do aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root).

## <a name="sideloading"></a>Sideload

Outra opção para distribuir aplicativos LOB diretamente aos usuários em sua organização é o Sideload. Essa opção é semelhante à implantação baseada em instalação de aplicativo, pois permite que os usuários instalem pacotes de aplicativos MSIX diretamente. A partir do Windows 10 versão 2004, o Sideload é habilitado por padrão e os usuários podem instalar aplicativos clicando duas vezes em pacotes de aplicativos MSIX assinados. No Windows 10 versão 1909 e anteriores, o Sideload requer algumas configurações adicionais e o uso de um script do PowerShell. Para saber mais, veja [Sideload de aplicativos LOB no Windows 10](https://docs.microsoft.com/windows/application-management/sideload-apps-in-windows-10).

## <a name="microsoft-store-for-business-or-microsoft-store-for-education"></a>Microsoft Store para negócios ou Microsoft Store para educação

Você pode publicar aplicativos LOB (linha de negócios) diretamente para empresas para aquisição de grande volume pela Microsoft Store para Empresas ou Microsoft Store para Educação, sem tornar os aplicativos amplamente disponíveis na Store. Ao usar essa opção, os aplicativos são assinados pela loja e devem estar em conformidade com as políticas de armazenamento padrão.

> [!NOTE]
> No momento, somente aplicativos gratuitos podem ser distribuídos exclusivamente para empresas por meio da Microsoft Store para Empresas ou da Microsoft Store para Educação. Se você enviar um aplicativo pago como LOB, ele não estará disponível para a empresa. 

> [!IMPORTANT]
> Não é possível usar a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para publicar aplicativos LOB diretamente para empresas. Todos os envios de aplicativos LOB devem ser publicados por meio do Partner Center.

### <a name="set-up-the-enterprise-association"></a>Configurar a associação empresarial

A primeira etapa na publicação de aplicativos LOB exclusivamente para uma empresa é estabelecer a associação entre sua conta e o repositório particular da empresa.

> [!IMPORTANT]
> Esse processo de associação deve ser iniciado pela empresa e deve usar o endereço de email associado à conta da Microsoft utilizada para criar a conta de desenvolvedor. Para saber mais, consulte [Trabalhando com aplicativos de linha de negócios](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps).

Quando uma empresa optar por convidá-lo a publicar aplicativos para seu uso exclusivo, você receberá um email que inclui um link para confirmar a associação. Você também pode confirmar essas associações acessando a seção **Associações empresariais** de suas **Configurações de conta** (contanto que você esteja conectado à conta da Microsoft que foi usada para abrir a conta de desenvolvedor).

Para confirmar a associação, clique em **Aceitar**. Sua conta estará, em seguida, apta a publicar aplicativos para uso exclusivo da empresa.

### <a name="submit-lob-apps"></a>Enviar aplicativos LOB

Quando você estiver pronto para publicar um aplicativo para uso exclusivo de uma empresa, o processo será semelhante ao processo de envio do aplicativo. O aplicativo passa pelo mesmo [processo de certificação](the-app-certification-process.md) e deve estar em conformidade com todas as [Políticas da Microsoft Store](store-policies.md). Apenas poucas partes do processo são diferentes.

#### <a name="visibility"></a>Visibilidade

Depois de configurar uma associação empresarial, sempre que enviar um aplicativo, você verá uma caixa de lista suspensa na seção **Visibilidade** da página **Preço e disponibilidade** do envio. Por padrão, essa opção é definida como **Retail distribution**. Para tornar o aplicativo exclusivo de uma empresa, você precisará escolher **Line-of-business (LOB) distribution**.

Depois que **Distribuição de linha de negócios (LOB)** for selecionada, as opções comuns de **Visibilidade** serão substituídas por uma lista das empresas para as quais você pode publicar aplicativos exclusivos. Ninguém fora as empresas selecionadas será capaz de exibir ou baixar o aplicativo.

Você deve selecionar pelo menos uma empresa para publicar um aplicativo como linha de negócios.

<span id="organizational" />

#### <a name="organizational-licensing"></a>Licenciamento para organizações

Por padrão, a caixa para **licenciamento por volume gerenciado pela Store (online)** é marcada quando você envia um aplicativo. Na publicação de aplicativos LOB, essa caixa permanecer marcada, para que a empresa possa adquirir seu aplicativo em volume. Isso não tornará o aplicativo disponível para qualquer pessoa fora das empresas que você selecionou na seção **Distribuição e visibilidade**.

Se você quiser disponibilizar o aplicativo para a empresa por meio de licenciamento desconectado (offline), poderá marcar também a caixa **Licenciamento desconectado (offline)** .

Para obter mais informações, consulte [Opções de licenciamento para organizações](organizational-licensing.md).

#### <a name="age-ratings"></a>Classificações etárias

Para aplicativos LOB, a etapa de [classificação etária](age-ratings.md) do processo de envio funciona da mesma forma para aplicativos de varejo, mas você também tem uma opção adicional que permite que você indique a classificação etária da loja de classificação do seu aplicativo manualmente em vez de concluir o questionário ou importar uma ID de classificação IARC existente. Essa classificação manual pode ser usada somente com a distribuição LOB, portanto, se você mudar a configuração de **Visibilidade** do aplicativo para **Distribuição comercial**, é necessário fazer o questionário de classificação etária antes de publicar o envio.

### <a name="enterprise-deployment-of-lob-apps"></a>Implantação empresarial de aplicativos LOB

Depois de clicar em **Enviar à Store**, o aplicativo passará pelo processo de certificação. Quando estiver pronto, um administrador da empresa deverá adicioná-lo ao seu repositório particular no portal da Microsoft Store para Empresas ou Microsoft Store para Educação. A empresa poderá então implantar o aplicativo para seus usuários.

> [!NOTE]
> Para obter o aplicativo LOB, a organização deve estar localizada em um [mercado com suporte](https://docs.microsoft.com/windows/whats-new/windows-store-for-business-overview#supported-markets), e você não deve ter [excluído esse mercado](define-pricing-and-market-selection.md) ao enviar o aplicativo. 

Para saber mais, veja [Trabalhando com aplicativos de linha de negócios](https://docs.microsoft.com/microsoft-store/working-with-line-of-business-apps) e [Distribuir aplicativos usando seu repositório particular](https://docs.microsoft.com/microsoft-store/distribute-apps-from-your-private-store).

### <a name="update-lob-apps"></a>Atualizar aplicativos LOB

Para publicar atualizações de um aplicativo que você já publicou como LOB, basta criar um novo envio. Você pode carregar novos pacotes ou fazer quaisquer outras alterações e, em seguida, clicar em **Enviar à Store** para disponibilizar a versão atualizada. Certifique-se de manter as mesmas seleções da empresa em **Visibilidade**, a menos que você queira realmente alterá-las, como, por exemplo, selecionar uma empresa adicional para adquirir o aplicativo ou remover uma das empresas às quais você o distribuiu anteriormente.

Se quiser parar de oferecer um aplicativo publicado anteriormente como linha de negócios, e impedir novas aquisições, você deverá criar um novo envio. Primeiro, você precisará alterar sua seleção em **Visibilidade** de **Distribuição de linha de negócios (LOB)** para **Distribuição comercial**. Em seguida,na seção [Detectabilidade](choose-visibility-options.md#discoverability), selecione **Disponibilizar este produto mas não torná-lo detectável na Store** com a opção **Parar aquisição**.

Depois que o envio passar pelo processo de certificação, o aplicativo não estará mais disponível para novas aquisições (embora quem já o tiver possa continuar a usá-lo).

> [!NOTE]
> Ao alterar um aplicativo para **Distribuição comercial**, você deverá completar o [questionário de classificação etária](age-ratings.md), se ainda não tiver preenchido anteriormente (mesmo se o aplicativo não for disponibilizado para compra).