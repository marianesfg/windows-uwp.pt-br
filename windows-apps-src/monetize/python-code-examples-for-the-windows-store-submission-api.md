---
author: Xansky
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: Use os exemplos de código Python nesta seção para saber mais sobre como usar a API de envio da Microsoft Store.
title: Exemplo de Python - envios para apps, complementos e versões de pré-lançamento
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, exemplos de código, python
ms.localizationpriority: medium
ms.openlocfilehash: 34d686b8e20d384da4a3db1ea3805ad082d5f8a8
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6996869"
---
# <a name="python-sample-submissions-for-apps-add-ons-and-flights"></a>Exemplo de Python: envios para apps, complementos e versões de pré-lançamento

Este artigo fornece exemplos de código Python que demonstram como usar a [API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tarefas:

* [Obter um token de acesso do Azure AD](#token)
* [Criar um complemento](#create-add-on)
* [Criar um pacote de pré-lançamento](#create-package-flight)
* [Criar um envio de aplicativo](#create-app-submission)
* [Criar um envio de complemento](#create-add-on-submission)
* [Criar um envio de pacote de pré-lançamento](#create-flight-submission)

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obter um token de acesso do Azure AD

O exemplo a seguir demonstra como [obter um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que é possível usar para chamar métodos na API de envio da Microsoft Store. Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Criar um complemento

O exemplo a seguir demonstra como [criar](create-an-add-on.md) e depois [excluir](delete-an-add-on.md) um complemento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Criar um pacote de pré-lançamento

O exemplo a seguir demonstra como [criar](create-a-flight.md) e depois [excluir](delete-a-flight.md) um pacote de pré-lançamento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Criar um envio de aplicativo

O exemplo a seguir mostra como usar diversos métodos na API de envio da Microsoft Store para criar um envio de aplicativo. Para fazer isso, o código cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Partner Center. Especificamente, o exemplo realiza estas tarefas:

1. Para começar, o exemplo [obtém dados do aplicativo especificado](get-an-app.md).
2. Em seguida, ele [exclui o envio pendente para o aplicativo](delete-an-app-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o aplicativo](create-an-app-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele muda alguns detalhes para o novo envio e carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualizações](update-an-app-submission.md) e, em seguida, [confirma](commit-an-app-submission.md) o novo envio para o Partner Center.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-app-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Criar um envio de complemento

O exemplo a seguir mostra como usar diversos métodos na API de envio da Microsoft Store para criar um envio de complemento. Para fazer isso, o código cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Partner Center. Especificamente, o exemplo realiza estas tarefas:

1. Para começar, o exemplo [obtém dados do complemento especificado](get-an-add-on.md).
2. Em seguida, ele [exclui o envio pendente para o complemento](delete-an-add-on-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um arquivo ZIP que contém ícones para o envio no armazenamento do Blob do Azure. Para obter mais informações, consulte as instruções relevantes sobre como carregar um arquivo ZIP para o armazenamento do Blob do Azure em [Criar um envio de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
5. Em seguida, ele [atualizações](update-an-add-on-submission.md) e, em seguida, [confirma](commit-an-add-on-submission.md) o novo envio para o Partner Center.
6. Por fim, ele periodicamente [verifica o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Criar um envio de pacote de pré-lançamento

O exemplo a seguir mostra como usar diversos métodos na API de envio da Microsoft Store para criar um envio de pacote de pré-lançamento. Para fazer isso, o código cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Partner Center. Especificamente, o exemplo realiza estas tarefas:

1. Para começar, o exemplo [obtém dados do pacote de pré-lançamento especificado](get-a-flight.md).
2. Em seguida, ele [exclui o envio pendente para o pacote de pré-lançamento](delete-a-flight-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o pacote de pré-lançamento](create-a-flight-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um novo pacote para o envio no armazenamento do Blob do Azure. Para obter mais informações, consulte as instruções relevantes sobre como carregar um arquivo ZIP para o armazenamento do Blob do Azure em [Criar um envio de pacote de pré-lançamento](manage-flight-submissions.md#create-a-package-flight-submission).
5. Em seguida, ele [atualizações](update-a-flight-submission.md) e, em seguida, [confirma](commit-a-flight-submission.md) o novo envio para o Partner Center.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-a-flight-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
