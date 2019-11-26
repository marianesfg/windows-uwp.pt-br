---
Description: Gerencie e exiba detalhes relacionados a cada um dos seus aplicativos no Partner Center e configure serviços, como mapas e testes A/B.
title: Gerenciamento de apps e serviços
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 03/21/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 30610cdacbd9d2be10205958688376371f0387f6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260034"
---
# <a name="app-management-and-services"></a>Gerenciamento de apps e serviços

Você pode gerenciar e exibir detalhes relacionados a cada um dos seus aplicativos no [Partner Center](https://partner.microsoft.com/dashboard)e configurar serviços como notificações, testes A/B e mapas.

Ao trabalhar com um aplicativo no Partner Center, você verá seções no menu de navegação à esquerda para **Serviços** e **Gerenciamento de aplicativos**. Você pode expandir essas seções para acessar a funcionalidade descrita abaixo.

## <a name="services"></a>Serviços

A seção **Serviços** permite que você gerencie vários serviços diferentes para os seus aplicativos.

## <a name="xbox-live"></a>Xbox Live

Se estiver publicando um jogo, você poderá habilitar o [programa do Xbox Live Creators](https://www.xbox.com/developers/creators-program) nesta página. Isso permite que você comece a configurar e testar os recursos do Xbox Live e, eventualmente, publique seu jogo do programa do Xbox Live Creators.

Para obter mais informações, consulte Introdução ao [programa de criadores do Xbox Live](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/get-started-with-xbox-live-creators) e [criar um novo título do programa do Xbox Live Creators e publicar no ambiente de teste](https://docs.microsoft.com/gaming/xbox-live/get-started-with-creators/create-and-test-a-new-creators-title).

## <a name="experimentation"></a>Experimentação

Use a página **Experimentação** para criar e executar experimentos para os seus aplicativos da Plataforma Universal do Windows (UWP) com testes A/B. Em um teste A/B, você mede a eficácia de mudanças (ou variações) de recursos no seu aplicativo com alguns clientes antes de ativar as mudanças para todos os usuários.

Para saber mais, veja [Executar experimentos de aplicativos com testes A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Mapas

Para usar serviços de mapa em aplicativos destinados ao Windows 10 ou Windows 8.x, visite o [Centro de Desenvolvimento do Bing Mapas](https://www.bingmapsportal.com/). Para obter informações sobre como solicitar uma chave de autenticação do Maps do centro de desenvolvedores do Bing Maps e adicioná-la ao seu aplicativo, consulte [solicitar uma chave de autenticação do Maps](../maps-and-location/authentication-key.md) para obter mais informações. 

Use a página **mapas** somente para aplicativos publicados anteriormente para Windows Phone 8,1 e anteriores. Para usar os serviços de mapa nesses aplicativos, você precisará solicitar uma ID de aplicativo de serviço de mapa e um token para incluir no código do aplicativo. Quando você clica em **obter token**, geraremos uma ID do aplicativo do serviço de mapa (**ApplicationId**) e o token de autenticação do serviço de mapa (**AuthenticationToken**) para seu aplicativo. Certifique-se de adicionar esses valores ao seu código antes de empacotar e enviar seu aplicativo. Para saber mais, consulte [Como adicionar um controle de mapa a uma página (Windows Phone 8.1)](https://docs.microsoft.com/previous-versions/windows/apps/jj207033(v=vs.105)?redirectedfrom=MSDN).

## <a name="product-collections-and-purchases"></a>Compras e coleções de produtos

Para usar a API de coleta de Microsoft Store e a API de compra Microsoft Store para acessar informações de propriedade para aplicativos e Complementos, você precisa inserir as IDs de cliente do Azure AD associadas aqui. Observe que essas alterações podem levar até 16 horas para entrar em vigor.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentimento do administrador

Se o seu produto se integrar ao Azure AD e chamar as APIs que solicitam [permissões de aplicativo ou permissões delegadas](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) que exigem o consentimento do administrador, insira sua ID de cliente do Azure ad aqui. Isso permite que os administradores que adquirem o aplicativo para sua organização concedem consentimento para que seu produto atue em nome de todos os usuários no locatário.

Para obter mais informações, consulte [solicitando consentimento para um locatário inteiro](https://docs.microsoft.com/azure/active-directory/develop/v2-permissions-and-consent#requesting-consent-for-an-entire-tenant).

## <a name="app-management"></a>Gerenciamento de aplicativo

A seção **Gerenciamento de aplicativo** permite a você visualizar detalhes de identidade e de pacote, bem como gerenciar os nomes do aplicativo.

## <a name="app-identity"></a>Identidade do aplicativo

Esta página mostra detalhes relacionados à identidade exclusiva do seu aplicativo dentro da loja, incluindo as URLs vinculadas aos detalhes de seu aplicativo.

Para saber mais, consulte [Exibir detalhes da identidade do aplicativo](view-app-identity-details.md).

## <a name="manage-app-names"></a>Gerenciar nomes de aplicativo

É aqui que você pode exibir todos os nomes que reservou para o seu aplicativo. Você pode reservar mais nomes aqui ou excluir nomes que não esteja usando.

Para saber mais, consulte [Gerenciar nomes de aplicativo](manage-app-names.md).

## <a name="current-packages"></a>Pacotes atuais

Essa página permite que você visualize detalhes relacionados a todos os seus pacotes publicados.

> [!NOTE]
> Você não verá as informações aqui até o aplicativo ser publicado.

O nome, a versão e a arquitetura de cada pacote são mostrados. Clique em **Detalhes** para mostrar informações adicionais, idioma com suporte, recursos do aplicativo e tamanhos de arquivo. As informações que você vê de cada pacote podem ser diferentes, dependendo do seu sistema operacional de destino e de outros fatores. 

Os desenvolvedores com permissões de OEM também podem [gerar pacotes de pré-instalação](generate-preinstall-packages-for-oems.md) na página **Pacotes atuais**.

## <a name="wnsmpns"></a>WNS/MPNS

A seção **WNS/MPNS** fornece opções para ajudá-lo a criar e enviar notificações aos clientes do seu aplicativo. 

> [!TIP]
> Para aplicativos UWP, sugerimos o uso do recurso de **notificações** no Partner Center. Esse recurso permite que você envie notificações para todos os clientes do seu aplicativo ou para um subconjunto de destino de seus clientes do Windows 10 que atendem aos critérios que você definiu em um [segmento de cliente](create-customer-segments.md). Para obter mais informações, consulte [Enviar notificações para clientes do seu aplicativo](send-push-notifications-to-your-apps-customers.md).

Dependendo do tipo de pacote do aplicativo e de seus requisitos específicos, você também pode usar uma das seguintes opções: 

-   **Serviços de Notificação por Push do Windows (WNS)** permite que você envie notificações do sistema, blocos, selos e atualizações brutas de seu próprio serviço de nuvem. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Aplicativos Móveis do Microsoft Azure** permite que você envie notificações por push, autentique e gerencie usuários de aplicativo e armazene dados de aplicativo na nuvem. Para saber mais, consulte a [documentação de Aplicativos Móveis](https://docs.microsoft.com/azure/app-service-mobile/).

-   **O serviço de notificações por push da Microsoft (MPNS)** pode ser usado com pacotes. xap publicados anteriormente para Windows Phone. Você pode enviar um número limitado de notificações não autenticadas sem fazer qualquer configuração aqui, embora seja recomendável usar notificações autenticadas para evitar limitações. Se estiver usando o MPNS, você precisará carregar um certificado para o campo fornecido na página **WNS/MPNS** . Para saber mais, veja [Configurando um serviço Web autenticado para enviar notificações por push para Windows Phone 8](https://docs.microsoft.com/previous-versions/windows/apps/ff941099(v=vs.105)?redirectedfrom=MSDN).
 

 
