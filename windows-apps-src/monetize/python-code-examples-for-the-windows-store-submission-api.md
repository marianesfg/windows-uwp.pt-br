---
author: mcleanbyron
ms.assetid: 8AC56AAF-8D8C-4193-A6B3-BB5D0669D994
description: "Use os exemplos de código Python nesta seção para saber mais sobre como usar a API de envio da Windows Store."
title: "Exemplos de código Python para a API de envio da Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envio da Windows Store, exemplos de código"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f787fd042982e4e5a965c9bb77ef72a8dabb6ae9
ms.lasthandoff: 02/07/2017

---

# <a name="python-code-examples-for-the-windows-store-submission-api"></a>Exemplos de código Python para a API de envio da Windows Store

Este artigo fornece exemplos de código Python para usar a *API de envio da Windows Store*. Para saber mais sobre essa API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

Estes exemplos de códigos demonstram as seguintes tarefas:

* [Obter um token de acesso do Azure AD](#token)
* [Criar um complemento](#create-add-on)
* [Criar um pacote de pré-lançamento](#create-package-flight)
* [Criar um envio de app](#create-app-submission)
* [Criar um envio de complemento](#create-add-on-submission)
* [Criar um envio de pacote de pré-lançamento](#create-flight-submission)

<span id="token" />
## <a name="obtain-an-azure-ad-access-token"></a>Obter um token de acesso do Azure AD

O exemplo a seguir demonstra como [obter um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que é possível usar para chamar métodos na API de envio da Windows Store. Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Windows Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L1-L20)]

<span id="create-add-on" />
## <a name="create-an-add-on"></a>Criar um complemento

O exemplo a seguir demonstra como [criar](create-an-add-on.md) e depois [excluir](delete-an-add-on.md) um complemento (os complementos também são conhecidos como produtos no app ou IAPs).

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L26-L52)]

<span id="create-package-flight" />
## <a name="create-a-package-flight"></a>Criar um pacote de pré-lançamento

O exemplo a seguir demonstra como [criar](create-a-flight.md) e depois [excluir](delete-a-flight.md) um pacote de pré-lançamento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L58-L87)]

<span id="create-app-submission" />
## <a name="create-an-app-submission"></a>Criar um envio de app

O exemplo a seguir mostra como usar diversos métodos na API de envio da Windows Store para criar um envio de app. Para isso, o código cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Centro de Desenvolvimento do Windows. Especificamente, o exemplo realiza estas tarefas:

1. Para começar, o exemplo [obtém dados do app especificado](get-an-app.md).
2. Em seguida, ele [exclui o envio pendente para o app](delete-an-app-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o app](create-an-app-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele muda alguns detalhes para o novo envio e carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualiza](update-an-app-submission.md) e [confirma](commit-an-app-submission.md) o novo envio para o Centro de Desenvolvimento do Windows.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-app-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L93-L166)]

<span id="create-add-on-submission" />
## <a name="create-an-add-on-submission"></a>Criar um envio de complemento

O exemplo a seguir mostra como usar diversos métodos na API de envio da Windows Store para criar um envio de complemento. Para isso, o código cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Centro de Desenvolvimento do Windows. Especificamente, o exemplo realiza estas tarefas:

1. Para começar, o exemplo [obtém dados do complemento especificado](get-an-add-on.md).
2. Em seguida, ele [exclui o envio pendente para o complemento](delete-an-add-on-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um arquivo ZIP que contém ícones para o envio no armazenamento do Blob do Azure. Para obter mais informações, consulte as instruções relevantes sobre como carregar um arquivo ZIP para o armazenamento do Blob do Azure em [Criar um envio de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
5. Em seguida, ele [atualiza](update-an-add-on-submission.md) e [confirma](commit-an-add-on-submission.md) o novo envio para o Centro de Desenvolvimento do Windows.
6. Por fim, ele periodicamente [verifica o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L172-L245)]

<span id="create-flight-submission" />
## <a name="create-a-package-flight-submission"></a>Criar um envio de pacote de pré-lançamento

O exemplo a seguir mostra como usar diversos métodos na API de envio da Windows Store para criar um envio de pacote de pré-lançamento. Para isso, o código cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Centro de Desenvolvimento do Windows. Especificamente, o exemplo realiza estas tarefas:

1. Para começar, o exemplo [obtém dados do pacote de pré-lançamento especificado](get-a-flight.md).
2. Em seguida, ele [exclui o envio pendente para o pacote de pré-lançamento](delete-a-flight-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o pacote de pré-lançamento](create-a-flight-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um novo pacote para o envio no armazenamento do Blob do Azure. Para obter mais informações, consulte as instruções relevantes sobre como carregar um arquivo ZIP para o armazenamento do Blob do Azure em [Criar um envio de pacote de pré-lançamento](manage-flight-submissions.md#create-a-package-flight-submission).
5. Em seguida, ele [atualiza](update-a-flight-submission.md) e [confirma](commit-a-flight-submission.md) o novo envio para o Centro de Desenvolvimento do Windows.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-a-flight-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/python/Examples.py#L251-L325)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)

