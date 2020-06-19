---
Description: Caso tenha fornecido pacotes direcionados a diferentes sistemas operacionais, você tem a opção de personalizar partes da listagem da Loja para os diferentes sistemas operacionais de destino.
title: Criar listagens específicas de plataforma da Store
ms.assetid: 5BE66BE2-669C-49E0-8915-60F1027EF94A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, personalizar, listagem, descrição, anteriormente
ms.localizationpriority: medium
ms.openlocfilehash: 475526662737eaa5b95267e36016e342c6652a89
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945952"
---
# <a name="create-platform-specific-store-listings"></a>Criar listagens específicas de plataforma da Store


Se o aplicativo publicado anteriormente tiver pacotes direcionados a diferentes sistemas operacionais, você terá a opção de personalizar partes da sua listagem de armazenamento para clientes em versões anteriores do sistema operacional (Windows 8. x ou anterior e/ou Windows Phone 8. x ou anterior). 

Os clientes no Windows 10 (incluindo o Xbox) sempre verão a [listagem de armazenamento](create-app-store-listings.md)padrão. Você não verá a opção de criar listagens de armazenamento específicas da plataforma, a menos que você já tenha publicado seu aplicativo com pacotes que dão suporte a uma ou mais versões anteriores do sistema operacional. 

> [!IMPORTANT]
> Você não pode mais carregar novos pacotes XAP criados usando os SDKs do Windows Phone 8. x. Os aplicativos que já estão em armazenamento com pacotes XAP continuarão a funcionar em dispositivos Windows 10 Mobile. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

As listagens de armazenamento específicas da plataforma podem ser úteis se você quiser mencionar OS recursos que aparecem apenas em uma versão do sistema operacional ou deseja fornecer capturas de tela específicas para um sistema operacional específico (independente do tipo de dispositivo).

> [!NOTE]
> A criação de uma listagem da Microsoft Store específica de plataforma em um idioma não cria uma listagem da Microsoft Store específica da plataforma em outros idiomas aos quais o aplicativo oferece suporte. Você precisará criar a listagem da Loja específica da plataforma separadamente para cada idioma. Observe também que você não pode [importar e exportar dados de listagem de repositório](import-and-export-store-listings.md) para listagens específicas de plataforma.


## <a name="creating-a-platform-specific-store-listing"></a>Criando uma listagem específica da Loja da plataforma

Próximo à parte superior da sua página de **listagem de armazenamento** , se o aplicativo publicado anteriormente incluir pacotes que dão suporte a versões anteriores do sistema operacional ((Windows 8. x ou anterior e/ou Windows Phone 8. x ou anterior), você poderá selecionar **criar uma listagem de repositório de aplicativos específica da plataforma**. Depois de selecionar essa opção, você deverá escolher entre as versões do sistema operacional direcionadas e compatíveis com o envio. Depois de criar listagens de armazenamento específicas da plataforma para todas as versões anteriores do sistema operacional que seu aplicativo se destina, você não poderá fazer outra seleção.

Você pode usar a listagem de armazenamento padrão (Windows 10) como um ponto de partida, o que trará o texto e as imagens aplicáveis que você inseriu para sua listagem de armazenamento padrão; em seguida, você poderá fazer as alterações desejadas antes de salvar. Você também pode iniciar uma listagem da Loja completamente nova se preferir.

Depois que você clicar em **Continuar**, a página **Listagem da Microsoft Store** agora incluirá uma seção para a listagem da Microsoft Store específica de plataforma que você acabou de criar. Esta seção incluirá seu próprio conjunto de campos para **Descrição** (necessário), **Novidades desta versão**, **Capturas de tela**, **Ícone do bloco do aplicativo**, **Recursos do aplicativo** e **Requisitos adicionais do sistema**. Certifique-se de inserir informações em cada campo em que você deseja exibir informações na listagem da Loja personalizada, mesmo que sejam as mesmas informações da listagem da Loja padrão. Se você deixar um desses campos em branco, nenhuma informação será exibida para tal campo na listagem da Loja personalizada.

> [!IMPORTANT]
> Os campos da seção [Informações adicionais](create-app-store-listings.md#additional-information) da listagem da Store não podem ser personalizados para diferentes versões do sistema operacional.
> 
> Além disso, como alguns dos campos na página de [listagem de repositório](create-app-store-listings.md) padrão se aplicam somente aos clientes no Windows 10, você não verá todas as mesmas opções ao criar uma listagem de repositório específica da plataforma. Por exemplo, você não pode adicionar trailers a uma listagem da Microsoft Store específica da plataforma, pois os trailers são exibidos somente para clientes no Windows 10, versão 1607 ou posterior. 

Você pode continuar a editar listagens específicas da plataforma conforme necessário para fazer alterações para os clientes em uma determinada versão do sistema operacional.


## <a name="removing-a-platform-specific-store-listing"></a>Removendo uma listagem da Microsoft Store específica de plataforma

Se você criar uma listagem da Microsoft Store específica de plataforma e depois decidir que quer mostrar a listagem padrão para os clientes desse sistema operacional, selecione o link **Excluir** ao lado da listagem.

Depois de confirmar que você deseja mostrar a listagem da Microsoft Store padrão aos clientes, selecione **OK**. A listagem da Microsoft Store específica da plataforma será removida (para todos os idiomas em que existia) e os clientes com essa versão do sistema operacional agora exigem sua listagem padrão da Microsoft Store. Se mudar de ideia, você pode criar outra listagem da Microsoft Store específica da plataforma para o sistema operacional seguindo as etapas listadas acima.
 

 




