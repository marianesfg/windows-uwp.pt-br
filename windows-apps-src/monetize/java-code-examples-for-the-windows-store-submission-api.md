---
author: Xansky
ms.assetid: 4920D262-B810-409E-BA3A-F68AADF1B1BC
description: Use os exemplos de código Java nesta seção para saber mais sobre como usar a API de envio da Microsoft Store.
title: Exemplo de Java - envios de apps, complementos e versões de pré-lançamento
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, exemplos de código, java
ms.localizationpriority: medium
ms.openlocfilehash: 4a0df9fe873ab7d7330e06a18bb1816df3157d7a
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7304868"
---
# <a name="java-sample-submissions-for-apps-add-ons-and-flights"></a>Exemplo de Java: envios de apps, complementos e versões de pré-lançamento

Este artigo fornece exemplos de código Java que demonstram como usar a [API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tarefas:

* [Obter um token de acesso do Azure AD](#token)
* [Criar um complemento](#create-add-on)
* [Criar um pacote de pré-lançamento](#create-package-flight)
* [Criar um envio de aplicativo](#create-app-submission)
* [Criar um envio de complemento](#create-add-on-submission)
* [Criar um envio de pacote de pré-lançamento](#create-flight-submission)

É possível examinar cada exemplo para saber mais sobre a tarefa que ele demonstra ou compilar todos os exemplos de código neste artigo em um aplicativo de console. Para a listagem de código completo, consulte a seção [listagem de códigos](java-code-examples-for-the-windows-store-submission-api.md#code-listing) no final deste artigo.

## <a name="prerequisites"></a>Pré-requisitos

Estes exemplos usam as seguintes bibliotecas:

* [Apache Commons Logging 1.2](http://commons.apache.org/proper/commons-logging)  (commons-logging-1.2.jar).
* [Apache HttpComponents Core 4.4.5 e Apache HttpComponents Client 4.5.2](https://hc.apache.org/) (httpcore-4.4.5.jar e httpclient-4.5.2.jar).
* [JSR 353 JSON Processing API 1.0](https://mvnrepository.com/artifact/javax.json/javax.json-api/1.0) e [JSR 353 JSON Processing Default Provider API 1.0.4](https://mvnrepository.com/artifact/org.glassfish/javax.json/1.0.4) (javax.json-api-1.0.jar e javax.json-1.0.4.jar).

## <a name="main-program-and-imports"></a>Programa principal e importações

O exemplo a seguir mostra as instruções de importação usadas por todos os exemplos de código e implementa um programa de linha de comando que chama os outros métodos de exemplo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/MainExample.java#L1-L64)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obtenha um token de acesso do Azure AD

O exemplo a seguir demonstra como [obter um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que é possível usar para chamar métodos na API de envio da Microsoft Store. Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L65-L95)]

<span id="create-add-on" />

## <a name="create-an-add-on"></a>Criar um complemento

O exemplo a seguir demonstra como [criar](create-an-add-on.md) e depois [excluir](delete-an-add-on.md) um complemento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L310-L345)]

<span id="create-package-flight" />

## <a name="create-a-package-flight"></a>Criar um pacote de pré-lançamento

O exemplo a seguir demonstra como [criar](create-a-flight.md) e depois [excluir](delete-a-flight.md) um pacote de pré-lançamento.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L185-L221)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Criar um envio de aplicativo

O exemplo a seguir mostra como usar diversos métodos na API de envio da Microsoft Store para criar um envio de aplicativo. Para fazer isso, o ```SubmitNewApplicationSubmission``` método cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Partner Center. Especificamente, o método ```SubmitNewApplicationSubmission``` realiza estas tarefas:

1. Para começar, o método [obtém dados do aplicativo especificado](get-an-app.md).
2. Em seguida, ele [exclui o envio pendente para o app](delete-an-app-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o aplicativo](create-an-app-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele muda alguns detalhes para o novo envio e carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualizações](update-an-app-submission.md) e, em seguida, [confirma](commit-an-app-submission.md) o novo envio para o Partner Center.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-app-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L97-L183)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Criar um envio de complemento

O exemplo a seguir mostra como usar diversos métodos na API de envio da Microsoft Store para criar um envio de complemento. Para fazer isso, o ```SubmitNewInAppProductSubmission``` método cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Partner Center. Especificamente, o método ```SubmitNewInAppProductSubmission``` realiza estas tarefas:

1. Para começar, o método [obtém dados do complemento especificado](get-an-add-on.md).
2. Em seguida, ele [exclui o envio pendente para o complemento](delete-an-add-on-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um arquivo ZIP que contém ícones para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualizações](update-an-add-on-submission.md) e, em seguida, [confirma](commit-an-add-on-submission.md) o novo envio para o Partner Center.
6. Por fim, ele periodicamente [verifica o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L347-L431)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Criar um envio de pacote de pré-lançamento

O exemplo a seguir mostra como usar diversos métodos na API de envio da Microsoft Store para criar um envio de pacote de pré-lançamento. Para fazer isso, o ```SubmitNewFlightSubmission``` método cria um novo envio como um clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Partner Center. Especificamente, o método ```SubmitNewFlightSubmission``` realiza estas tarefas:

1. Para começar, o método [obtém dados do pacote de pré-lançamento especificado](get-a-flight.md).
2. Em seguida, ele [exclui o envio pendente para o pacote de pré-lançamento](delete-a-flight-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o pacote de pré-lançamento](create-a-flight-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualizações](update-a-flight-submission.md) e, em seguida, [confirma](commit-a-flight-submission.md) o novo envio para PartnerCenter.
6. Por fim, ele periodicamente [verifica o status do novo envio](get-status-for-a-flight-submission.md) até que o envio seja confirmado com êxito.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L223-L308)]

<span id="utilities" />

## <a name="utility-methods-to-upload-submission-files-and-handle-request-responses"></a>Métodos de utilitário para carregar arquivos de envio e manipular respostas à solicitação

Os seguintes métodos de utilitário demonstram estas tarefas:

* Como carregar um arquivo ZIP contendo novos ativos para um envio de aplicativo ou complemento no armazenamento do Blob do Azure. Para obter mais informações sobre como carregar um arquivo ZIP no armazenamento do Blob do Azure para envios de aplicativo e complemento, consulte as instruções relevantes em [Criar um envio de aplicativo](manage-app-submissions.md#create-an-app-submission), [Criar um envio de complemento](manage-add-on-submissions.md#create-an-add-on-submission) e [Criar um envio de pacote de pré-lançamento](manage-flight-submissions.md#create-a-package-flight-submission).
* Como manipular respostas à solicitação

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L433-L490)]

<span id="code-listing" />

## <a name="complete-code-listing"></a>Listagem de código completo

A listagem de código a seguir contém todos os exemplos anteriores organizados em um único arquivo de origem.

[!code[SubmissionApi](./code/StoreServicesExamples_Submission/java/CompleteExample.java#L1-L491)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
