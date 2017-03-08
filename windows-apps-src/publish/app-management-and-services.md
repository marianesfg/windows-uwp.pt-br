---
author: jnHs
Description: "Gerencie e visualize detalhes relacionados a cada um de seus apps no painel do Centro de Desenvolvimento do Windows e configure serviços como notificações por push, teste A/B e mapas."
title: "Gerenciamento de apps e serviços"
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 1aaab8d122968265ff17b352a53b69081e84f01e
ms.lasthandoff: 02/07/2017

---

# <a name="app-management-and-services"></a>Gerenciamento de apps e serviços

Você pode gerenciar e visualizar detalhes relacionados a cada um de seus apps no painel do Centro de Desenvolvimento do Windows e configurar serviços como notificações por push, teste A/B e mapas.

Ao trabalhar com um app em seu painel, você verá seções no menu de navegação esquerdo para **gerenciamento de apps** e **serviços**. Você pode expandir essas seções para acessar a funcionalidade descrita abaixo.

## <a name="services"></a>Serviços

A seção **Serviços** permite que você gerencie vários serviços diferentes para os seus apps.

### <a name="push-notifications"></a>Notificações por push

A seção **Notificações por push** permite criar e enviar notificações por push direcionadas para clientes do seu app. Você pode enviá-las para todos os clientes do seu app ou para um subconjunto de clientes do Windows 10 que atendem aos critérios definidos em um [segmento do cliente](create-customer-segments.md). Para obter mais informações, consulte [Enviar notificações por push direcionadas para clientes do seu app](send-push-notifications-to-your-apps-customers.md).

Dependendo do tipo de pacote do app e de seus requisitos específicos, você também pode usar uma das opções a seguir para notificações por push clicando na página **WNS/MPNS** no menu de navegação esquerdo: 

-   **Serviços de Notificação por Push do Windows (WNS)** permite que você envie notificações do sistema, blocos, selos e atualizações brutas de seu próprio serviço de nuvem. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](https://msdn.microsoft.com/library/windows/apps/mt187203).

-   **Aplicativos Móveis do Microsoft Azure** permite que você envie notificações por push, autentique e gerencie usuários de app e armazene dados de app na nuvem. Para saber mais, consulte a [documentação de Aplicativos Móveis](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Serviço de Notificação por Push da Microsoft (MPNS)** pode ser usado com seus pacotes .xap para Windows Phone. Você pode enviar um número limitado de notificações não autenticadas sem fazer qualquer configuração aqui, embora seja recomendável usar notificações autenticadas para evitar limitações. Se você estiver usando o MPNS, será necessário carregar um certificado para o campo fornecido na página **Notificações por push**. Para saber mais, veja [Configurando um serviço Web autenticado para enviar notificações por push para Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).

### <a name="experimentation"></a>Experimentação

Use a página **Experimentação** para criar e executar experimentos para os seus apps da Plataforma Universal do Windows (UWP) com testes A/B. Em um teste A/B, você mede a eficácia de mudanças (ou variações) de recursos no seu app com alguns clientes antes de ativar as mudanças para todos os usuários.

Para saber mais, veja [Executar experimentos de apps com testes A/B](../monetize/run-app-experiments-with-a-b-testing.md).

### <a name="maps"></a>Mapas

Para usar os serviços de mapa em apps para Windows Phone 8.1 e versões anteriores, você precisa de uma ID de app de serviço de mapa e um token para incluir no código do seu app. Você pode obter esse token na página **Mapas** na seção **Serviços**.

> **Observação**  Para usar serviços de mapa em apps destinados ao Windows 10 ou Windows 8.x, visite o [Centro de Desenvolvimento do Bing Mapas](http://go.microsoft.com/fwlink/p/?LinkId=614880). Consulte [Solicitar uma chave de autenticação de mapas](https://msdn.microsoft.com/library/windows/apps/mt219694) para obter mais informações.

Para saber mais, consulte [Usar serviços de mapa](use-map-services.md).

### <a name="product-collections-and-purchases"></a>Compras e coleções de produtos

Para usar a API de coleção da Windows Store e a API de compra da Windows Store para acessar informações de propriedade para apps e complementos, você precisa inserir as IDs de cliente do Azure AD associado aqui. Observe que essas alterações podem levar até 16 horas para entrar em vigor.

Para saber mais, consulte [Exibir e conceder produtos de um serviço](https://msdn.microsoft.com/library/windows/apps/mt609002).

## <a name="app-management"></a>Gerenciamento de app

A seção **Gerenciamento de app** permite que você exiba detalhes de identidade e de pacote e gerencie seus nomes de app.

### <a name="app-identity"></a>Identidade do app

Esta página mostra detalhes relacionados à identidade exclusiva do seu app dentro da loja, incluindo as URLs vinculadas aos detalhes de seu app.

Para saber mais, consulte [Exibir detalhes da identidade do app](view-app-identity-details.md).

### <a name="manage-app-names"></a>Gerenciar nomes de app

É aqui que você pode exibir todos os nomes que reservou para o seu app. Você pode reservar mais nomes aqui ou excluir nomes que não esteja usando.

Para saber mais, consulte [Gerenciar nomes de app](manage-app-names.md).

### <a name="current-packages"></a>Pacotes atuais

Essa página permite que você visualize detalhes relacionados a todos os seus pacotes publicados.

> **Nota**  Você não verá nenhuma informação aqui até seu app ter sido publicado.

O nome, a versão e a arquitetura de cada pacote são mostrados. Clique em **Detalhes** para mostrar informações adicionais, idioma com suporte, recursos do app e tamanhos de arquivo. As informações que você vê de cada pacote podem ser diferentes, dependendo do seu sistema operacional de destino e de outros fatores. 

Os desenvolvedores com permissões de OEM também podem [gerar pacotes de pré-instalação](generate-preinstall-packages-for-oems.md) na página **Pacotes atuais**.

 

 

