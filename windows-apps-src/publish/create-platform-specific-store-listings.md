---
author: jnHs
Description: If you've provided packages targeting different operating systems, you have the option to customize parts of your Store listing for different targeted operating systems.
title: Criar listagens específicas de plataforma da Store
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.author: wdg-dev-content
ms.date: 3/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, personalizar, listagem, descrição, anteriormente
ms.localizationpriority: medium
ms.openlocfilehash: 6f30a825cc7aec1b6f7dbf5cff0ea1c17c43ffd7
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4742396"
---
# <a name="create-platform-specific-store-listings"></a>Criar listagens específicas de plataforma da Store


Se o aplicativo tiver pacotes direcionados a diferentes sistemas operacionais, você tem a opção de personalizar partes da listagem da Store para clientes em versões anteriores do sistema operacional (Windows 8. x ou versões anteriores e/ou Windows Phone 8. x ou anterior). 

> [!IMPORTANT]
> Os clientes no Windows 10 sempre verão a [listagem padrão da Microsoft Store](create-app-store-listings.md). Você não verá a opção de criar listagens da Microsoft Store específicas da plataforma, exceto se você já tiver carregado pacotes que oferecem suporte a uma ou mais versões anteriores do sistema operacional. 

Listagens da Loja específicas da plataforma podem ser úteis se você quer mencionar recursos que aparecem apenas em uma versão do sistema operacional ou se quer fornecer capturas de tela que sejam específicas a um determinado sistema operacional (independentemente do tipo de dispositivo), em vez de todos os clientes verem a mesma listagem da Loja.

> [!NOTE]
> A criação de uma listagem da Microsoft Store específica de plataforma em um idioma não cria uma listagem da Microsoft Store específica da plataforma em outros idiomas aos quais o aplicativo oferece suporte. Você precisará criar a listagem da Loja específica da plataforma separadamente para cada idioma. Observe também que você não pode importar e exportar dados de listagem da Microsoft Store para listagens específicas da plataforma.


## <a name="creating-a-platform-specific-store-listing"></a>Criando uma listagem específica da Loja da plataforma

Próximo da parte superior da página de **Listagem da Microsoft Store**, se o aplicativo oferecer suporte para versões anteriores do sistema operacional (Windows 8.x ou versões anteriores e/ou Windows Phone 8.x ou versões anteriores), você pode selecionar **criar uma listagem da App Store específica da plataforma**. 

> [!TIP]
> Você só verá a opção de criação de listagens da Microsoft Store específica de plataforma após ter carregado os pacotes.

Depois de selecionar essa opção, você deverá escolher entre as versões do sistema operacional direcionadas e compatíveis com o envio. Depois que você tiver criado listagens da Microsoft Store específicas de plataforma para todas as versões de sistema operacional às quais seu aplicativo se destina, não será possível fazer outra seleção. (O Windows 10 não está incluído na lista de opções, pois os clientes no Windows 10 sempre verão a listagem da Microsoft Store padrão do aplicativo.)

Você pode usar a listagem da Microsoft Store padrão como ponto de partida, o que mostrará o texto e as imagens aplicáveis inseridos na listagem da Microsoft Store padrão. Em seguida, você poderá fazer as alterações que quiser antes de salvar. Você também pode iniciar uma listagem da Loja completamente nova se preferir.

Depois que você clicar em **Continuar**, a página **Listagem da Microsoft Store** agora incluirá uma seção para a listagem da Microsoft Store específica de plataforma que você acabou de criar. Esta seção incluirá seu próprio conjunto de campos para **Descrição** (necessário), **Novidades desta versão**, **Capturas de tela**, **Ícone do bloco do aplicativo**, **Recursos do aplicativo** e **Requisitos adicionais do sistema**. Certifique-se de inserir informações em cada campo em que você deseja exibir informações na listagem da Loja personalizada, mesmo que sejam as mesmas informações da listagem da Loja padrão. Se você deixar um desses campos em branco, nenhuma informação será exibida para tal campo na listagem da Loja personalizada.


> [!IMPORTANT]
> Os campos da seção [Informações adicionais](create-app-store-listings.md#additional-information) da listagem da Store não podem ser personalizados para diferentes versões do sistema operacional.
> 
> Além disso, como alguns campos na página padrão [listagem da Store](create-app-store-listings.md) se aplicam apenas aos clientes no Windows 10, você não verá as mesmas opções ao criar uma listagem da Store específica da plataforma. Por exemplo, você não pode adicionar trailers a uma listagem da Microsoft Store específica da plataforma, pois os trailers são exibidos somente para clientes no Windows 10, versão 1607 ou posterior. 


## <a name="removing-a-platform-specific-store-listing"></a>Removendo uma listagem da Microsoft Store específica de plataforma

Se você criar uma listagem da Microsoft Store específica de plataforma e depois decidir que quer mostrar a listagem padrão para os clientes desse sistema operacional, selecione o link **Excluir** ao lado da listagem.

Depois de confirmar que você deseja mostrar a listagem da Microsoft Store padrão aos clientes, selecione **OK**. A listagem da Microsoft Store específica da plataforma será removida (para todos os idiomas em que existia) e os clientes com essa versão do sistema operacional agora exigem sua listagem padrão da Microsoft Store. Se mudar de ideia, você pode criar outra listagem da Microsoft Store específica da plataforma para o sistema operacional seguindo as etapas listadas acima.

 

 




