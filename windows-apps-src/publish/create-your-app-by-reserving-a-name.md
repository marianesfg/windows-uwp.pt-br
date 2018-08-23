---
author: jnHs
Description: The first step in creating a new app in your Windows Dev Center dashboard is reserving an app name. See how to reserve app names and find suggestions for choosing a great name for your app.
title: Criar seu app reservando um nome
keywords: windows 10, uwp, reserva de nome, nome do app, nomes de aplicativo, nomes, nome de produto, nomenclatura, nome reservado, título, nomes, títulos
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.author: wdg-dev-content
ms.date: 8/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 83f2ab8a27810635b569d44961ff532ce3240e28
ms.sourcegitcommit: 9c79fdab9039ff592edf7984732d300a14e81d92
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/23/2018
ms.locfileid: "2815879"
---
# <a name="create-your-app-by-reserving-a-name"></a>Crie seu app reservando um nome

A primeira etapa na criação de um novo aplicativo no [Painel de controle do Windows Dev Center](https://partner.microsoft.com/dashboard) é reservar um nome de aplicativo. Cada nome reservado (às vezes chamado de *título* do seu app) deve ser exclusivo em toda a Microsoft Store.

Você pode reservar um nome para seu app, mesmo se não tiver começado a criar seu app ainda. Recomendamos que você faça isso assim que possível para que ninguém mais possa usar o nome. Observe que você precisará enviar o app dentro de três meses para manter o mesmo nome reservado para seu uso.

Ao [carregar os pacotes do app](upload-app-packages.md), o valor [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) deve corresponder ao nome reservado para o app. Se você usar o Microsoft Visual Studio para criar o pacote de seu aplicativo, esse atributo será preenchido para você.

> [!IMPORTANT]
> Você pode reservar nomes adicionais para um aplicativo, e você pode optar por usar um na versão do seu aplicativo publicada em vez do que reservar ao criar primeiro seu aplicativo no painel. No entanto, lembre-se de que o primeiro nome inserido aqui será usado em algumas das [detalhes de identidade](view-app-identity-details.md), como o **Nome da família de pacote (PFN) do seu aplicativo**. Esses valores podem ser vistos por alguns usuários e não pode ser alterada, portanto certifique-se de que o nome que você reservar é apropriado para este uso.


## <a name="create-your-app-by-reserving-a-new-name"></a>Crie seu aplicativo reservando um novo nome

Reservar um nome é a primeira etapa na criação de um aplicativo no painel. 

1.  Na página **Visão geral**, clique em **Criar um novo aplicativo**.
2.  Na caixa de texto, insira o nome que você deseja usar e, em seguida, selecione **Verificar disponibilidade**. Se o nome estiver disponível, você verá uma marca de seleção verde. (Se o nome que você inseriu já estiver reservado ou sendo usado por outro desenvolvedor, você verá uma mensagem de que o nome não está disponível).
3.  Clique em **Reservar nome do produto**.

O nome agora está reservado, e você pode começar a trabalhar em seu [envio](app-submissions.md) quando estiver pronto. 

> [!NOTE]
> Talvez você descubra que não pode reservar um nome, mesmo que não exista nenhum app com esse nome listado na Microsoft Store. Geralmente isso ocorre porque outro desenvolvedor reservou o nome para o app dele, mas ainda não o enviou. Se você não conseguir reservar um nome que tenha sido registrado por você ou sobre o qual você possui outros direitos legais, ou se encontrar outro app na Microsoft Store com o mesmo nome, [contate a Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=233777).

Depois de reservar um nome, você terá três meses para enviar o app. Se você não enviá-lo em um três meses, a reserva do nome expirará e outro desenvolvedor poderá usar o nome em um app. Você pode encontrar um erro se tentar enviar um aplicativo sob um nome que deixou expirar.

> [!NOTE]
> Se você tiver um aplicativo do Windows Phone criado no painel do Windows Phone mais antigo e nunca reservou um nome para ele, é necessário fazer isso para carregar pacotes .appX para ele ou a fim de [exibir detalhes de identidade de aplicativo](view-app-identity-details.md) específicos para pacotes .AppX. A reserva de um nome exclusivo também impede que outra pessoa reserve o nome para si. No entanto, se você não reservar um nome, ainda é possível gerenciar e enviar o aplicativo para seus clientes do Windows Phone 8.x.


## <a name="choosing-your-apps-name"></a>Escolhendo o nome do aplicativo

Escolher o melhor nome para o aplicativo é uma tarefa importante. Escolha um nome que chamará a atenção dos seus clientes e fazer com que eles queiram saber mais sobre seu aplicativo. Veja algumas dicas para escolher o melhor nome para seu aplicativo.

-   **Mantenha-o curto.** O espaço para exibir o nome do seu aplicativo é limitado em muitos lugares; por isso, sugerimos usar o nome mais curto possível. Embora o nome do aplicativo possa ter até 256 caracteres, nem sempre o final de um nome muito longo pode ficar visível aos clientes.
    > [!NOTE]
    > O número real de caracteres exibidos em vários locais pode variar dependendo do comprimento alocado e dos tipos de caracteres usados no nome do aplicativo. Por exemplo, na fonte Segoe UI que o Windows usa, cerca de 30 caracteres "I" caberão no mesmo espaço que 10 caracteres "W". Em decorrência dessa variação, teste seu aplicativo e verifique como o nome aparece nos blocos (se você optar por sobrepor o nome do aplicativo), nos resultados da pesquisa e dentro do próprio aplicativo antes de enviá-lo. Considere também cada idioma no qual você oferece seu aplicativo. Tenha em mente que caracteres do Leste Asiático tendem a ser mais largos que caracteres latinos, portanto, menos caracteres serão exibidos.
-   **Seja original.** Assegure que o nome do aplicativo seja diferente o suficiente para que não seja confundido com um aplicativo existente.
-   **Não use nomes com marca comercial de outros.** Certifique-se de ter os direitos de uso do nome reservado. Caso alguém tenha registrado o nome, ele poderá relatar uma infração, e você não poderá continuar usando o nome. Se isso ocorrer após a publicação do aplicativo, ele será removido da Loja. Desse modo, você precisará alterar o nome do seu aplicativo e todas as instâncias do nome em todo o aplicativo e seu conteúdo antes de [enviar o aplicativo](app-submissions.md) para certificação novamente.
-   **Evite adicionar informações de diferenciação no fim do nome.** Caso as informações que diferenciam vários aplicativos sejam adicionadas ao final do nome, é possível que os clientes não as vejam, principalmente se o nome for longo; assim, parecerá que todos os aplicativos têm o mesmo nome. Se não for possível evitar essa situação, use logotipos e imagens do aplicativo diferentes para facilitar a diferenciação entre um app e outro.
-   **Não inclua emojis no nome.** Você não poderá reservar um nome que inclua emojis ou outros caracteres não compatíveis.


## <a name="manage-additional-app-names"></a>Gerenciar nomes de aplicativos adicionais

Você pode adicionar e gerenciar mais nomes na página **Gerenciar nomes de aplicativo** na seção **Gerenciamento de aplicativos** para cada um dos seus aplicativos no painel do Centro de Desenvolvimento do Windows.

Em alguns casos, convém reservar vários nomes para usar o mesmo aplicativo, como quando você deseja oferecer seu aplicativo em vários idiomas e quer usar nomes diferentes para cada idioma. Você precisará reservar um nome adicional se desejar alterar o nome do aplicativo completamente.

Nessa página, você também pode excluir todos os nomes que reservou mas não deseja usar.

Para saber mais, consulte [Gerenciar nomes de aplicativo](manage-app-names.md).

 

 




