---
author: jnHs
Description: Ao trabalhar com um aplicativo no painel do Centro de Desenvolvimento do Windows, você pode exibir detalhes relacionados à identidade exclusiva atribuída a ele pela Windows Store e obter um link para os detalhes do seu aplicativo na Loja.
title: Exibir detalhes de identidade do aplicativo
ms.assetid: 86F05A79-EFBC-4705-9A71-3A056323AC65
---

# Exibir detalhes de identidade do aplicativo


Ao trabalhar com um aplicativo no painel do Centro de Desenvolvimento do Windows, você pode exibir detalhes relacionados à identidade exclusiva atribuída a ele pela Windows Store e obter um link para os detalhes do seu aplicativo na Loja.

Para encontrar essas informações, navegue para um dos seus aplicativos, depois expanda **Gerenciamento de aplicativo** no menu de navegação esquerdo. Clique em **Identidade do aplicativo** para exibir esses detalhes.

> **Observação** você precisa ter um [nome reservado](create-your-app-by-reserving-a-name.md) para o seu aplicativo para ver a maioria desses detalhes de identidade.

## Valores a serem incluídos no manifesto appx


Os seguintes valores devem ser incluídos em seu manifesto appx. Se você estiver usando o Microsoft Visual Studio para criar seus pacotes, e está conectado com a mesma conta da Microsoft que você associou com sua conta de desenvolvedor, esses detalhes serão incluídos automaticamente. Se você estiver criando o pacote manualmente, precisará adicioná-los.

-   **Pacote/identidade/nome**
-   **Pacote/identidade/editor**

Para saber mais, consulte [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441) na [Referência de esquemas de manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/br211473).

Juntos, esses elementos declaram a identidade do seu aplicativo, estabelecendo a "família do pacote" a qual todos os seus pacotes pertencem. Pacotes individuais terão detalhes adicionais, como arquitetura e versão.

## Valores adicionais de família de pacote


Os valores seguintes são valores adicionais que se referem à família de pacote do seu aplicativo, mas que não estão incluídos em seu manifesto.

-   **Nome da Família de Pacotes (PFN)**: esse valor é usado com determinadas APIs do Windows.
-   **SID de pacote**: você precisará desse valor para enviar notificações do WNS para seu aplicativo. Para saber mais, consulte a [Visão geral do WNS (Serviços de Notificação por Push do Windows)](https://msdn.microsoft.com/library/windows/apps/mt187203).

## Link com os detalhes do seu aplicativo

O link para a página do seu aplicativo pode ser compartilhado para ajudar os clientes a encontrar o aplicativo na Loja. Este link está no formato **`https://www.microsoft.com/store/apps/<your app's Store ID>`**.

> **Observação** Esta URL funcionará para qualquer versão do sistema operacional no qual seu aplicativo esteja disponível. Você também pode ver links adicionais para o Windows 8.1 e versões anteriores e/ou Windows Phone 8.1 e versões anteriores, que funcionarão somente nas versões de sistema operacional especificadas.

Quando um cliente clicar nesse link, ele abrirá a página de listagem baseada na web do seu aplicativo. Se o aplicativo estiver disponível para o dispositivo Windows do cliente, o aplicativo da Loja também será iniciado e exibirá a listagem do aplicativo.

O **ID da Loja** de seu aplicativo também é mostrado nesta seção. Esse ID da Loja pode ser usado para [gerar notificações da Loja](http://go.microsoft.com/fwlink/p/?LinkId=534236) ou identificar seu aplicativo de outra forma.

 

 






<!--HONumber=May16_HO2-->


