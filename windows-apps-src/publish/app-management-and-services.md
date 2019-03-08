---
Description: Exibir detalhes relacionados a cada um dos seus aplicativos no Partner Center e configurar os serviços como A / B, teste e é mapeado.
title: Gerenciamento de aplicativos e serviços
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d6261a7cce86c82b4865d7ca1d68c082cba9ccca
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57610001"
---
# <a name="app-management-and-services"></a>Gerenciamento de aplicativos e serviços

Você pode gerenciar e exibir os detalhes relacionados a cada um dos seus aplicativos no [Partner Center](https://partner.microsoft.com/dashboard/)e configurar os serviços, como notificações, A / B, teste e é mapeado.

Ao trabalhar com um aplicativo no Partner Center, você verá seções no menu de navegação à esquerda **Services** e **gerenciamento de aplicativo**. Você pode expandir essas seções para acessar a funcionalidade descrita abaixo.

## <a name="services"></a>Serviços

A seção **Serviços** permite que você gerencie vários serviços diferentes para os seus aplicativos.

## <a name="xbox-live"></a>Xbox Live

Se você estiver publicando um jogo, você pode habilitar o [programa de criadores do Xbox Live](https://xbox.com/developers/creators-program) nesta página. Isso permite começar a configurar e testar recursos do Xbox Live e eventualmente publicar seu jogo do programa de criadores do Xbox Live.

Para obter mais informações, consulte [começar com o programa de criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md) e [criar um novo título do programa de criadores do Xbox Live e publicar no ambiente de teste](../xbox-live/get-started-with-creators/create-and-test-a-new-creators-title.md).

## <a name="experimentation"></a>Experimentação

Use a página **Experimentação** para criar e executar experimentos para os seus aplicativos da Plataforma Universal do Windows (UWP) com testes A/B. Em um teste A/B, você mede a eficácia de mudanças (ou variações) de recursos no seu aplicativo com alguns clientes antes de ativar as mudanças para todos os usuários.

Para saber mais, veja [Executar experimentos de aplicativos com testes A/B](../monetize/run-app-experiments-with-a-b-testing.md).

## <a name="maps"></a>Mapas

Para usar serviços de mapa em aplicativos destinados ao Windows 10 ou Windows 8.x, visite o [Centro de Desenvolvimento do Bing Mapas](https://go.microsoft.com/fwlink/p/?LinkId=614880). Para obter informações sobre como solicitar uma chave de autenticação de mapas do Bing Maps Developer Center e adicioná-lo ao seu aplicativo, consulte [solicitar uma chave de autenticação de mapas](../maps-and-location/authentication-key.md) para obter mais informações. 

Use o **mapas** página apenas para aplicativos publicados anteriormente para Windows Phone 8.1 e versões anteriores. Para usar os serviços de mapa nesses aplicativos, você precisará solicitar uma ID de aplicativo do serviço de mapa e um token para incluir no código do seu aplicativo. Quando você clica em **obter um token**, vamos gerar um ID do aplicativo de serviço do mapa (**ApplicationID**) e o Token de autenticação do serviço de mapa (**AuthenticationToken**) para seu aplicativo. Certifique-se de adicionar esses valores ao seu código antes de você empacota e enviar seu aplicativo. Para saber mais, consulte [Como adicionar um controle de mapa a uma página (Windows Phone 8.1)](https://go.microsoft.com/fwlink/p/?LinkId=614882).

## <a name="product-collections-and-purchases"></a>Compras e coleções de produtos

Para usar o Microsoft Store API de coleta e a API de compra do Microsoft Store para acessar as informações de propriedade de aplicativos e complementos, você precisa inserir associado do Azure AD IDs de cliente aqui. Observe que essas alterações podem levar até 16 horas para entrar em vigor.

Para obter mais informações, consulte [Gerenciar direitos a produtos de um serviço](../monetize/view-and-grant-products-from-a-service.md).

## <a name="administrator-consent"></a>Consentimento do administrador

Se o seu produto se integra ao AD do Azure e chama as APIs que solicitam uma [permissões de aplicativo ou permissões delegadas](https://developer.microsoft.com/graph/docs/concepts/permissions_reference) que exigem o consentimento do administrador, digite sua ID de cliente do Azure AD aqui. Isso permite que os administradores que adquirem o aplicativo para a organização conceder consentimento para o seu produto atuar em nome de todos os usuários no locatário.

Para obter mais informações, consulte [solicitar o consentimento para um locatário inteiro](https://docs.microsoft.com/en-us/azure/active-directory/develop/active-directory-v2-scopes#requesting-consent-for-an-entire-tenant).

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

O **WNS/MPNS** seção fornece opções para ajudá-lo a criar e enviar notificações aos clientes do seu aplicativo. 

> [!TIP]
> Para aplicativos UWP, é recomendável usar o **notificações** recurso no Partner Center. Esse recurso permite que você envie notificações para todos os clientes do seu aplicativo, ou a um subconjunto de destino de seus clientes do Windows 10 que atendem aos critérios que você definiu em uma [segmento de clientes](create-customer-segments.md). Para obter mais informações, consulte [Enviar notificações para clientes do seu aplicativo](send-push-notifications-to-your-apps-customers.md).

Dependendo do tipo de pacote do aplicativo e seus requisitos específicos, você também pode usar uma das seguintes opções: 

-   **Serviço de Notificação por Push do Windows (WNS)** permite que você envie notificações do sistema, blocos, selos e atualizações brutas de seu próprio serviço de nuvem. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](../design/shell/tiles-and-notifications/windows-push-notification-services--wns--overview.md).

-   **Aplicativos Móveis do Microsoft Azure** permite que você envie notificações por push, autentique e gerencie usuários de aplicativo e armazene dados de aplicativo na nuvem. Para saber mais, consulte a [documentação de Aplicativos Móveis](https://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Serviços de notificações por Push da Microsoft (MPNS)** pode ser usado com pacotes XAP publicada anteriormente para Windows Phone. Você pode enviar um número limitado de notificações não autenticadas sem fazer qualquer configuração aqui, embora seja recomendável usar notificações autenticadas para evitar limitações. Se você estiver usando o MPNS, você precisará carregar um certificado para o campo fornecido na **WNS/MPNS** página. Para saber mais, veja [Configurando um serviço Web autenticado para enviar notificações por push para Windows Phone 8](https://go.microsoft.com/fwlink/p/?LinkId=528736).
 

 
