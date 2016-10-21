---
author: jnHs
Description: "Você pode gerenciar e visualizar detalhes relacionados a cada um de seus aplicativos no painel do Centro de Desenvolvimento do Windows e configurar serviços como notificações por push e mapas."
title: "Gerenciamento de aplicativos e serviços"
ms.assetid: 99DA2BC1-9B5D-4746-8BC0-EC723D516EEF
translationtype: Human Translation
ms.sourcegitcommit: 3afdf00864e023d913b635beef0c506735881b23
ms.openlocfilehash: 9787ef724622a95d291b4631196b3e13bcb1298a

---

# Gerenciamento de aplicativos e serviços

Você pode gerenciar e visualizar detalhes relacionados a cada um de seus aplicativos no painel do Centro de Desenvolvimento do Windows e configurar serviços como notificações por push e mapas.

Ao trabalhar com um aplicativo em seu painel, você verá seções no menu de navegação esquerdo para gerenciamento de aplicativos e serviços. Você pode expandir essas seções para acessar a funcionalidade descrita abaixo.

## Serviços

A seção **Serviços** permite que você gerencie vários serviços diferentes para os seus aplicativos.

### Notificações por push

Dependendo do tipo de pacote do aplicativo e seus requisitos específicos, você pode usar uma das opções a seguir para notificações por push:

-   **Serviço de Notificação por Push do Windows (WNS)** permite que você envie notificações do sistema, blocos, selos e atualizações brutas de seu próprio serviço de nuvem. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](https://msdn.microsoft.com/library/windows/apps/mt187203).

-   **Aplicativos Móveis do Microsoft Azure** permite que você envie notificações por push, autentique e gerencie usuários de aplicativo e armazene dados de aplicativo na nuvem. Para saber mais, consulte a [documentação de Aplicativos Móveis](http://go.microsoft.com/fwlink/p/?LinkId=221116).

-   **Serviço de Notificação por Push da Microsoft (MPNS)** pode ser usado com seus pacotes .xap para Windows Phone. Você pode enviar um número limitado de notificações não autenticadas sem fazer qualquer configuração aqui, embora seja recomendável usar notificações autenticadas para evitar limitações. Se você estiver usando o MPNS, será necessário carregar um certificado para o campo fornecido na página **Notificações por push**. Para saber mais, veja [Configurando um serviço Web autenticado para enviar notificações por push para Windows Phone 8](http://go.microsoft.com/fwlink/p/?LinkId=528736).

### Experimentação

Use a página **Experimentação** para criar e executar experimentos para os seus aplicativos da Plataforma Universal do Windows (UWP) com testes A/B. Em um teste A/B, você mede a eficácia de mudanças (ou variações) de recursos no seu aplicativo com alguns clientes antes de ativar as mudanças para todos os usuários.

Para saber mais, veja [Executar experimentos de aplicativos com testes A/B](../monetize/run-app-experiments-with-a-b-testing.md).

### Mapas

Para usar os serviços de mapa em aplicativos para Windows Phone 8.1 e versões anteriores, você precisa de uma ID de aplicativo de serviço de mapa e um token para incluir no código do seu aplicativo. Você pode obter esse token na página **Mapas** na seção **Serviços**.

> **Observação**  Para usar serviços de mapa em aplicativos destinados a outros sistemas operacionais, visite o [Centro de Desenvolvimento do Bing Mapas](http://go.microsoft.com/fwlink/p/?LinkId=614880). Consulte [Solicitar uma chave de autenticação de mapas](https://msdn.microsoft.com/library/windows/apps/mt219694) para obter mais informações.

Para saber mais, consulte [Usar serviços de mapa](use-map-services.md).

### Compras e coleções de produtos

Para usar a API de coleção da Windows Store e a API de compra da Windows Store para acessar informações de propriedade para aplicativos e complementos, você precisa inserir as IDs de cliente do Azure AD associado aqui. Observe que essas alterações podem levar até 16 horas para entrar em vigor.

Para saber mais, consulte [Exibir e conceder produtos de um serviço](https://msdn.microsoft.com/library/windows/apps/mt609002).

## Gerenciamento de aplicativo

A seção **Gerenciamento de aplicativo** permite que você exiba detalhes de identidade e de pacote e gerencie seus nomes de aplicativo.

### Identidade do aplicativo

Esta página mostra detalhes relacionados à identidade exclusiva do seu aplicativo dentro da loja, incluindo as URLs vinculadas aos detalhes de seu aplicativo.

Para saber mais, consulte [Exibir detalhes da identidade do aplicativo](view-app-identity-details.md).

### Gerenciar nomes de aplicativo

É aqui que você pode exibir todos os nomes que reservou para o seu aplicativo. Você pode reservar mais nomes aqui ou excluir nomes que não esteja usando.

Para saber mais, consulte [Gerenciar nomes de aplicativo](manage-app-names.md).

### Pacotes atuais

Essa página permite que você visualize detalhes relacionados a todos os seus pacotes publicados.

> **Nota**  Você não verá nenhuma informação aqui até seu aplicativo ter sido publicado.

O nome, a versão e a arquitetura de cada pacote são mostrados. Clique em **Detalhes** para mostrar informações adicionais, idioma com suporte, recursos do aplicativo e tamanhos de arquivo.

As informações exatas que você vê de cada pacote podem ser diferentes, dependendo do seu sistema operacional de destino e de outros fatores. Por exemplo, caso tenha adicionado [Mediação de anúncios do Windows](https://msdn.microsoft.com/library/windows/apps/mt219691) em seu pacote, você encontrará um link para configurar a mediação desse pacote aqui.

Os desenvolvedores com permissões de OEM também podem [gerar pacotes de pré-instalação](generate-preinstall-packages-for-oems.md) na página **Pacotes atuais**.

 

 



<!--HONumber=Aug16_HO3-->


