---
Description: A primeira etapa na criação de um novo aplicativo no Partner Center é reservar um nome de aplicativo. Veja como reservar nomes e encontrar sugestões para escolher um nome excelente para o seu aplicativo.
title: Crie seu app reservando um nome
keywords: windows 10, uwp, reserva de nome, nome do app, nomes de aplicativo, nomes, nome de produto, nomenclatura, nome reservado, título, nomes, títulos
ms.assetid: 6DC58A9A-DF47-4652-8D13-0AC9289F5950
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 79831e1e14ddf8c525f1697074e6a435513f5ea1
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259022"
---
# <a name="create-your-app-by-reserving-a-name"></a>Crie seu app reservando um nome

A primeira etapa na criação de um novo aplicativo no [Partner Center](https://partner.microsoft.com/dashboard) é reservar um nome de aplicativo. Cada nome reservado (às vezes chamado de *título* do seu app) deve ser exclusivo em toda a Microsoft Store.

Você pode reservar um nome para seu app, mesmo se não tiver começado a criar seu app ainda. É recomendável fazer isso assim que possível, para que ninguém possa usar o nome. Observe que você precisará enviar o app dentro de três meses para manter o mesmo nome reservado para seu uso.

Ao [carregar os pacotes do app](upload-app-packages.md), o valor [**Package/Properties/DisplayName**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-displayname) deve corresponder ao nome reservado para o app. Se você usar o Microsoft Visual Studio para criar o pacote de seu aplicativo, esse atributo será preenchido para você.

> [!IMPORTANT]
> Você pode reservar nomes adicionais para um aplicativo e pode optar por usar um deles na versão publicada do seu aplicativo, em vez do que você reservar ao criar o aplicativo pela primeira vez no Partner Center. No entanto, lembre-se de que o nome inserido aqui será usado em alguns [detalhes de identidade](view-app-identity-details.md)do aplicativo, como o **nome da família de pacotes (PFN)** . Esses valores podem ser visíveis para alguns usuários e não podem ser alterados, portanto certifique-se de que o nome reservado é apropriado para esse uso.


## <a name="create-your-app-by-reserving-a-new-name"></a>Crie seu aplicativo reservando um novo nome

Reservar um nome é a primeira etapa na criação de um aplicativo no Partner Center. 

1.  Na página **Visão geral**, clique em **Criar um novo aplicativo**.
2.  Na caixa de texto, insira o nome que você deseja usar e, em seguida, selecione **Verificar disponibilidade**. Se o nome estiver disponível, você verá uma marca de seleção verde. (Se o nome que você inseriu já estiver reservado ou sendo usado por outro desenvolvedor, você verá uma mensagem de que o nome não está disponível).
3.  Clique em **Reservar nome do produto**.

O nome agora está reservado, e você pode começar a trabalhar em seu [envio](app-submissions.md) quando estiver pronto. 

> [!NOTE]
> Talvez você descubra que não pode reservar um nome, mesmo que não exista nenhum app com esse nome listado na Microsoft Store. Geralmente isso ocorre porque outro desenvolvedor reservou o nome para o aplicativo dele, mas ainda não enviou o aplicativo. Se você não conseguir reservar um nome que tenha sido registrado por você ou sobre o qual você possui outros direitos legais, ou se encontrar outro app na Microsoft Store com o mesmo nome, [contate a Microsoft](https://www.microsoft.com/info/cpyrtInfrg.html).

Depois de reservar um nome, você terá três meses para enviar o app. Se você não enviá-lo em um três meses, a reserva do nome expirará e outro desenvolvedor poderá usar o nome em um app. Você pode encontrar um erro se tentar enviar um aplicativo sob um nome que deixou expirar.


## <a name="choosing-your-apps-name"></a>Escolhendo o nome do aplicativo

Escolher o melhor nome para o aplicativo é uma tarefa importante. Escolha um nome que chamará a atenção dos seus clientes e fazer com que eles queiram saber mais sobre seu aplicativo. Veja algumas dicas para escolher o melhor nome para seu aplicativo.

-   **Mantenha-o curto.** O espaço para exibir o nome do seu aplicativo é limitado em muitos lugares; por isso, sugerimos usar o nome mais curto possível. Embora o nome do aplicativo possa ter até 256 caracteres, nem sempre o final de um nome muito longo pode ficar visível aos clientes.
    > [!NOTE]
    > O número real de caracteres exibidos em vários locais pode variar dependendo do comprimento alocado e dos tipos de caracteres usados no nome do aplicativo. Por exemplo, na fonte Segoe UI que o Windows usa, cerca de 30 caracteres "I" caberão no mesmo espaço que 10 caracteres "W". Devido a essa variação, certifique-se de testar seu aplicativo e verificar como seu nome aparece em seus blocos (se você optar por sobrepor o nome do aplicativo), nos resultados da pesquisa e no próprio aplicativo. Considere também cada idioma no qual você oferece seu aplicativo. Tenha em mente que caracteres do Leste Asiático tendem a ser mais largos que caracteres latinos, portanto, menos caracteres serão exibidos.
-   **Ser original.** Assegure que o nome do aplicativo seja diferente o suficiente para que não seja confundido com um aplicativo existente.
-   **Não use os nomes com marcas registradas por outras pessoas.** Certifique-se de ter os direitos de uso do nome reservado. Caso alguém tenha registrado o nome, ele poderá relatar uma infração, e você não poderá continuar usando o nome. Se isso ocorrer após a publicação do aplicativo, ele será removido da Loja. Desse modo, você precisará alterar o nome do seu aplicativo e todas as instâncias do nome em todo o aplicativo e seu conteúdo antes de [enviar o aplicativo](app-submissions.md) para certificação novamente.
-   **Evite adicionar informações de diferenciação no final do nome.** Caso as informações que diferenciam vários aplicativos sejam adicionadas ao final do nome, é possível que os clientes não as vejam, principalmente se o nome for longo; assim, parecerá que todos os aplicativos têm o mesmo nome. Se isso for inevitável, use diferentes logotipos e imagens de aplicativo para facilitar a diferenciação de um aplicativo de outro.
-   **Não inclua emojis em seu nome.** Você não poderá reservar um nome que inclua emojis ou outros caracteres não compatíveis.


## <a name="manage-additional-app-names"></a>Gerenciar nomes de aplicativos adicionais

Você pode adicionar e gerenciar nomes adicionais na página **gerenciar nomes de aplicativos** na seção **Gerenciamento de aplicativos** para cada um de seus aplicativos no Partner Center.

Em alguns casos, convém reservar vários nomes para usar o mesmo aplicativo, como quando você deseja oferecer seu aplicativo em vários idiomas e quer usar nomes diferentes para cada idioma. Você precisará reservar um nome adicional se desejar alterar o nome do aplicativo completamente.

Nessa página, você também pode excluir todos os nomes que reservou mas não deseja usar.

Para saber mais, consulte [Gerenciar nomes de aplicativo](manage-app-names.md).

 

 




