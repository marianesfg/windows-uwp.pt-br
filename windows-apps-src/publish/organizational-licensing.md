---
Description: É possível indicar se e como seu aplicativo pode ser oferecido para compras em grande volume por meio da Microsoft Store para Empresas e da Microsoft Store para Educação na seção Licenciamento organizacional de um envio de aplicativo.
title: Opções de licenciamento para organizações
ms.assetid: 1EB139B0-67E7-4F66-AAEF-491B1E52E96F
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, store para empresas, store para educação, organizacional, licenciamento por volume, empresa, store educacional, store empresarial, compra em volume, massa
localizationpriority: high
ms.openlocfilehash: 8cfa4d4a18112ef1cad793048399d04835ff4633
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67320579"
---
# <a name="organizational-licensing-options"></a>Opções de licenciamento para organizações


É possível indicar se e como seu aplicativo pode ser oferecido para compras em grande volume por meio da Microsoft Store para Empresas e da Microsoft Store para Educação na seção **Licenciamento organizacional** da página [Preço e disponibilidade](set-app-pricing-and-availability.md#organizational-licensing) de um envio de aplicativo.

Por meio dessas configurações, você pode optar por permitir que seu aplicativo fique disponível para organizações (empresarial e educacional) que adquirem e implantam várias licenças para os usuários delas, oferecendo uma oportunidade de aumentar seu alcance para organizações entre os tipos de dispositivos do Windows 10, incluindo PCs, tablets e celulares.

Também será necessário permitir licenciamento organizacional para quaisquer [aplicativos de LOB (linha de negócios)](distribute-lob-apps-to-enterprises.md) que você publicar diretamente para as empresas.

> [!NOTE]
> As seleções de cada um dos seus aplicativos são configuradas independentemente umas das outras. Você pode alterar as preferências de um aplicativo a qualquer momento, criando um novo envio, e suas alterações terão efeito após o envio do [processo de certificação](the-app-certification-process.md).

> [!IMPORTANT]
> Os envios que usam a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) não ficarão disponíveis na Microsoft Store para Empresas e na Microsoft Store para Educação. Para disponibilizar seu aplicativo para compras em volume por organizações, crie e envie-o pelo Partner Center.


## <a name="allowing-your-app-to-be-offered-to-organizations"></a>Permitindo que seu aplicativo seja oferecido para organizações

Por padrão, a caixa chamada **Make my app available to organizations with Store-managed (online) licensing and distribution** é marcada. Isso significa que você deseja que seu aplicativo se torne disponível para inclusão em catálogos de aplicativos que serão disponibilizados para organizações para aquisição em grande volume, com licenças de aplicativo gerenciadas por meio do sistema de licenciamento online da Loja.

> [!NOTE]
> Isso não garante que seu aplicativo estará disponível para todas as organizações.

Se você preferir não nos permitem oferecer seu aplicativo para organizações de aquisição por volume, desmarque esta caixa. Observe que essa alteração só ocorrerá depois que o aplicativo concluir o processo de certificação. Se quaisquer organizações adquiriram anteriormente licenças para o seu aplicativo, essas licenças ainda serão válidas e as pessoas que já têm o aplicativo podem continuar a usá-lo.

> [!TIP]
> Para publicar aplicativos de LOB (linha de negócios) exclusivamente para uma organização específica, você pode definir uma associação empresarial e permitir que a organização adicione os aplicativos diretamente ao repositório particular delas. Para saber mais, consulte [Distribuir aplicativos LOB para empresas](distribute-lob-apps-to-enterprises.md).


## <a name="allowing-disconnected-offline-licensing"></a>Permitindo licenciamento desconectado (offline)

Muitas organizações precisam de aplicativos habilitados para licenciamento offline. Por exemplo, algumas organizações precisam implantar aplicativos em dispositivos que raramente ou nunca se conectam à internet. Se quiser permitir que seu aplicativo seja disponibilizado para esses clientes, marque a caixa rotulada **Allow organization-managed (offline) licensing and distribution for organizations**.

Observe que esta caixa está **desmarcada** por padrão. Marque-a para permitir que seu aplicativo fique disponível para organizações verificadas que o instalarão usando licenciamento gerenciado pela organização (offline). As organizações devem passar por validação adicional para instalar aplicativos pagos para seus usuários finais dessa maneira.

O licenciamento offline permite que as organizações adquiram seu aplicativo por volume e depois instalem o aplicativo sem precisar que cada dispositivo contate com o sistema de licenciamento da Loja. A organização é capaz de baixar o pacote do aplicativo com uma licença que lhes permite instalá-lo para dispositivos (por meio de suas próprias ferramentas de gerenciamento ou da pré-carregamento de aplicativos em imagens do sistema operacional) sem notificar à Loja quando uma licença específica foi usada. Habilitar esse cenário aumenta bastante a flexibilidade de implantação e pode aumentar significativamente a atratividade de seu aplicativo para esses clientes.

> [!IMPORTANT]
> Não há suporte para o licenciamento offline de pacotes .xap.

 
## <a name="paid-app-support"></a>Suporte a aplicativo pago

Atualmente, as contas de desenvolvedor localizadas em determinados mercados podem oferecer aplicativos pagos para aquisição de volume por meio da Microsoft Store para Empresas. 

> [!NOTE]
> Em alguns mercados, o preço exibido para um aplicativo na Microsoft Store para Empresas ou na Microsoft Store para Educação pode ser diferente do preço mostrado para clientes comerciais na Microsoft Store na mesma faixa de preço. O pagamento de receitas das compras organizacionais funciona da mesma maneira que para compras de consumidor do seu aplicativo. Para obter mais informações, consulte [Recebendo pagamento](getting-paid-apps.md) e o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement). Para ver uma lista dos mercados em que a Microsoft Store para Empresas e a Microsoft Store para Educação estão disponíveis, confira [Visão geral da Microsoft Store para Empresas e da Microsoft Store para Educação](https://docs.microsoft.com/windows/manage/windows-store-for-business-overview#supported-markets).

Caso seu país ou região não esteja listado abaixo, seus aplicativos pagos não serão oferecidos na Microsoft Store para Empresas e na Microsoft Store para Educação. Se esse for o caso, as seleções de licenciamento organizacionais feitas para seus aplicativos pagos poderão ser aplicadas futuramente, pois poderemos adicionar suporte para envios de outros mercados de conta de desenvolvedor no futuro.

No momento, os desenvolvedores localizados nos seguintes países e regiões podem distribuir aplicativos pagos para clientes organizacionais pela Microsoft Store para Empresas e a Microsoft Store para Educação:

- Áustria
- Bélgica
- Bulgária
- Canadá
- Croácia
- Chipre
- República Tcheca
- Dinamarca
- Estônia
- Finlândia
- França
- Alemanha
- Grécia
- Hungria
- Irlanda
- Ilha de Man
- Itália
- Letônia
- Liechtenstein
- Lituânia
- Luxemburgo
- Malta
- Mônaco
- Países Baixos
- Noruega
- Polônia
- Portugal
- Romênia
- Eslováquia
- Eslovênia
- Espanha
- Suécia
- Suíça
- Reino Unido
- Estados Unidos
