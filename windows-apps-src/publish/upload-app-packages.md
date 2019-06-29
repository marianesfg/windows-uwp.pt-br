---
Description: A página de pacotes é onde você carrega todos os arquivos de pacote (. appxupload,. AppX,. appxbundle e/ou. xap) para o aplicativo que você está enviando.
title: Carregue os pacotes do aplicativo
ms.assetid: B1BB810D-3EAA-4FB5-B03C-1F01AFB2DE36
ms.date: 10/02/2018
ms.topic: article
keywords: Windows 10, uwp, pacotes, carregamento, upload do pacote
ms.localizationpriority: medium
ms.openlocfilehash: 97735a8e860f7c941cc35d77a21496696683640f
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468876"
---
# <a name="upload-app-packages"></a>Carregue os pacotes do aplicativo

O **pacotes** página é onde você carrega todos os arquivos de pacote (.msix, .msixupload, .msixbundle,. AppX,. appxupload e/ou. appxbundle) para o aplicativo que você está enviando. Você pode carregar todos os seus pacotes para o mesmo aplicativo nesta página, e quando um cliente baixa o seu aplicativo, a Store fornecerá automaticamente cada cliente com o pacote que funciona melhor para seu dispositivo. Depois de carregar os pacotes, você verá uma tabela indicando [quais pacotes serão oferecidos para famílias de dispositivos Windows 10 específicas](#device-family-availability) (e versões anteriores do sistema operacional, se aplicável) na ordem de classificação.

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, produtos recém-criado não podem incluir os pacotes direcionados a 8.x/Windows do Windows Phone 8.x ou anterior. Para obter mais informações, consulte este [postagem de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para obter detalhes sobre o que um pacote inclui e como ele deve ser estruturado, veja [Requisitos do pacote do aplicativo](app-package-requirements.md). Você também vai querer saber mais sobre [como números de versão impacto quais pacotes são entregues aos clientes específicos](package-version-numbering.md) e [como gerenciar pacotes para vários cenários](guidance-for-app-package-management.md).


## <a name="uploading-packages-to-your-submission"></a>Carregando pacotes para seu envio

Para carregar os pacotes, arraste-os para o campo de carregamento ou clique em para procurar os arquivos. O **pacotes** página permitirá que você carregue arquivos .msix, .msixupload, .msixbundle,. AppX,. appxupload e/ou. appxbundle.

> [!IMPORTANT]
> Para Windows 10, é recomendável carregar o arquivo. appxupload ou de .msixupload aqui em vez de .msix,. AppX, .msixbundle ou. appxbundle.  Para obter mais informações sobre como empacotar aplicativos UWP para a Loja, consulte [Empacotar um aplicativo UWP com o Visual Studio](../packaging/packaging-uwp-apps.md).

Caso tenha criado [pacotes de pré-lançamento](package-flights.md) para seu aplicativo, você verá uma lista suspensa com a opção para copiar pacotes de um dos pacotes de pré-lançamento. Selecione o pacote de pré-lançamento que tiver os pacotes que você deseja puxar. Em seguida, você pode selecionar qualquer um ou todos os seus pacotes para incluir nesse envio.

Se detectarmos erros com um pacote ao validá-lo, vamos exibir uma mensagem para que você saiba o que está errado. Você precisará remover o pacote, corrija o problema e, em seguida, tente carregá-lo novamente. Você também pode ver avisos para que saiba quais questões podem causar problemas, mas não impedem que você continue o seu envio.


## <a name="device-family-availability"></a>Disponibilidade da família de dispositivos

Depois que os pacotes tiverem sido carregados com êxito, a seção **Disponibilidade da família de dispositivos** exibirá uma tabela que indica quais pacotes serão oferecidos a famílias de dispositivos Windows 10 específicas (e versões anteriores do sistema operacional, se aplicável), em ordem de classificação. Esta seção também permite escolher se você deseja oferecer ou não o envio para clientes em famílias de dispositivos Windows 10 específicas.

Para obter mais informações, consulte [Disponibilidade da família de dispositivos](device-family-availability.md).


## <a name="package-details"></a>Detalhes do pacote

Os pacotes carregados são listados aqui, agrupados por sistema operacional de destino. O nome, a versão e a arquitetura do pacote serão exibidos. Para obter mais informações, como os idiomas compatíveis, os recursos do aplicativo e o tamanho do arquivo de cada pacote, clique em **Mostrar detalhes**.

Se você precisar remover um pacote do envio, clique no link **Remover** na parte inferior da seção **Detalhes** de cada pacote.


## <a name="removing-redundant-packages"></a>Removendo pacotes redundantes

Se detectarmos que um ou mais dos seus pacotes são redundantes, exibiremos um aviso sugerindo que você remova os pacotes redundantes deste envio. Geralmente, isso ocorre quando você já tiver carregado pacotes e agora está fornecendo pacotes com versões superiores que dão suporte ao mesmo conjunto de clientes. Neste caso, nenhum cliente jamais obteria o pacote redundante porque agora há um pacote melhor para dar suporte a esses clientes (com maior número de versão).

Quando detectarmos que você tem pacotes redundantes, forneceremos uma opção para remover todos os pacotes redundantes deste envio automaticamente. Você também pode remover pacotes individualmente deste envio, se preferir.


## <a name="gradual-package-rollout"></a>Distribuição de pacote gradual

Se o envio for uma atualização para um aplicativo publicado anteriormente, você verá uma caixa de seleção que diz **Roll out update gradually after this submission is published (to Windows 10 customers only)** . Isso permite escolher um percentual de clientes que receberão os pacotes do envio, de maneira que você possa monitorar comentários e dados analíticos para se certificar da atualização antes de implantá-la mais amplamente. Você pode aumentar a porcentagem (ou parar a atualização) a qualquer tempo sem precisar criar um novo envio. 

Para obter mais informações, consulte [Distribuição gradual de pacote](gradual-package-rollout.md).


## <a name="mandatory-update"></a>Atualização obrigatória

Se o envio for uma atualização para um aplicativo publicado anteriormente, você verá uma caixa de seleção que diz **Make this update mandatory**. Isso permite definir a data e a hora para uma atualização obrigatória, pressupondo que você tenha usado as APIs Windows.Services.Store para permitir que o aplicativo verifique programaticamente se há atualizações de pacote, baixe e instale os pacotes atualizados. O aplicativo deve segmentar o Windows 10, versão 1607, ou posterior para usar essa opção.

Para obter mais informações, consulte [Baixar e instalar atualizações de pacote para seu aplicativo](../packaging/self-install-package-updates.md).

 




