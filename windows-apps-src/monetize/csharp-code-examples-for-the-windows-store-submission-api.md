---
author: mcleanbyron
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: "Use os exemplos de código C# nesta seção para saber mais sobre como usar a API de envio da Windows Store."
title: "Exemplos de código C# para a API de envio da Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envio da Windows Store, exemplos de código"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: c71555eb854e5dcf6cbccf89e9b0b8ffe35ab0e4
ms.lasthandoff: 02/07/2017

---

# <a name="c-code-examples-for-the-windows-store-submission-api"></a>Exemplos de código C\# para a API de envio da Windows Store

Este artigo fornece exemplos de código C# para usar a *API de envio da Windows Store*. Para saber mais sobre essa API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

Estes exemplos de códigos demonstram as seguintes tarefas:

* [Atualizar um envio de app](#update-app-submission)
* [Criar um novo envio de complemento](#create-add-on-submission)
* [Atualizar um envio de complemento](#update-add-on-submission)
* [Atualizar um envio de pacote de pré-lançamento](#update-flight-submission)

Você pode examinar cada exemplo para saber mais sobre a tarefa que ele demonstra ou compilar todos os exemplos de código neste artigo em um app de console. Para compilar os exemplos, crie um app de console C# chamado **DeveloperApiCSharpSample** no Visual Studio, copie cada exemplo em um arquivo de código separado no projeto e compile o projeto.

## <a name="prerequisites"></a>Pré-requisitos

Estes exemplos usam as seguintes bibliotecas:

* Microsoft.WindowsAzure.Storage.dll. Essa biblioteca está disponível no [SDK do Azure para .NET](https://azure.microsoft.com/downloads/) ou você pode obtê-la instalando o [pacote NuGet do WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage).
* [Json.NET](http://www.newtonsoft.com/json) da Newtonsoft.

## <a name="main-program"></a>Programa principal

O exemplo a seguir implementa um programa de linha de comando que chama os outros métodos de exemplo neste artigo para demonstrar as diversas maneiras de usar a API do envio da Windows Store. Para adaptar este programa para seu próprio uso:

* Atribua as propriedades ```ApplicationId```, ```InAppProductId``` e ```FlightId``` para a ID do app, o complemento (os complementos também são conhecidos como produtos no app ou IAPs) e o pacote de pré-lançamento que você deseja gerenciar. Essas IDs estão disponíveis no painel do Centro de Desenvolvimento.
* Atribua as propriedades ```ClientId``` e ```ClientSecret``` para a ID e a chave do cliente para seu app e substitua a cadeia de caracteres *tenantid* na URL de ```TokenEndpoint``` pela ID de locatário de seu app. Para obter mais informações, consulte [Como associar um app do Azure AD à sua conta do Centro de Desenvolvimento do Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />
## <a name="clientconfiguration-helper-class"></a>Classe auxiliar ClientConfiguration

O app de exemplo usa a classe auxiliar ```ClientConfiguration``` para transmitir dados do Azure Active Directory e dados do app para cada um dos métodos de exemplo que usam a API de envio da Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="update-app-submission" />
## <a name="update-an-app-submission"></a>Atualizar um envio de app

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Windows Store para atualizar um envio de app. O método ```RunAppSubmissionUpdateSample``` na classe cria um novo envio como clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Centro de Desenvolvimento do Windows. Especificamente, o método ```RunAppSubmissionUpdateSample``` executa estas tarefas:

1. Para começar, o método [obtém dados do app especificado](get-an-app.md).
2. Em seguida, ele [exclui o envio pendente para o app](delete-an-app-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o app](create-an-app-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele muda alguns detalhes para o novo envio e carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualiza](update-an-app-submission.md) e [confirma](commit-an-app-submission.md) o novo envio para o Centro de Desenvolvimento do Windows.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-app-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />
## <a name="create-a-new-add-on-submission"></a>Criar um novo envio de complemento

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Windows Store para criar um novo envio de complemento. O método ```RunInAppProductSubmissionCreateSample``` na classe executa estas tarefas:

1. Para começar, o método [cria um novo complemento](create-an-add-on.md).
2. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md).
3. Ele carrega um arquivo ZIP que contém ícones para o envio no armazenamento do Blob do Azure.
4. Em seguida, ele [confirma o novo envio para o Centro de Desenvolvimento do Windows](commit-an-add-on-submission.md).
5. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />
## <a name="update-an-add-on-submission"></a>Atualizar um envio de complemento

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Windows Store para atualizar um envio de complemento existente. O método ```RunInAppProductSubmissionUpdateSample``` na classe cria um novo envio como clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Centro de Desenvolvimento do Windows. Especificamente, o método ```RunInAppProductSubmissionUpdateSample``` executa estas tarefas:

1. Para começar, o método [obtém dados do complemento especificado](get-an-add-on.md).
2. Em seguida, ele [exclui o envio pendente para o complemento](delete-an-add-on-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md) (o novo envio é uma cópia do último envio publicado).
5. Em seguida, ele [atualiza](update-an-add-on-submission.md) e [confirma](commit-an-add-on-submission.md) o novo envio para o Centro de Desenvolvimento do Windows.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="update-flight-submission" />
## <a name="update-a-package-flight-submission"></a>Atualizar um envio de pacote de pré-lançamento

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Windows Store para atualizar um envio de pacote de pré-lançamento. O método ```RunFlightSubmissionUpdateSample``` na classe cria um novo envio como clone do último envio publicado e, em seguida, atualiza e confirma o envio clonado para o Centro de Desenvolvimento do Windows. Especificamente, o método ```RunFlightSubmissionUpdateSample``` executa estas tarefas:

1. Para começar, o método [obtém dados do pacote de pré-lançamento especificado](get-a-flight.md).
2. Em seguida, ele [exclui o envio pendente para o pacote de pré-lançamento](delete-a-flight-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o pacote de pré-lançamento](create-a-flight-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualiza](update-a-flight-submission.md) e [confirma](commit-a-flight-submission.md) o novo envio para o Centro de Desenvolvimento do Windows.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-a-flight-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />
## <a name="ingestionclient-helper-class"></a>Classe auxiliar IngestionClient

A classe ```IngestionClient``` fornece métodos auxiliares que são usados por outros métodos no app de exemplo para executar as seguintes tarefas:

* [Obter um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que pode ser usado para chamar métodos na API de envio da Windows Store. Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Windows Store antes que ele expire. Depois que o token expirar, você poderá gerar um novo.
* Carregar um arquivo ZIP contendo novos ativos para um envio de app ou complemento no armazenamento do Blob do Azure. Para obter mais informações sobre como carregar um arquivo ZIP no armazenamento do Blob do Azure para envios de apps e complementos, consulte as instruções relevantes em [Criar um envio de app](manage-app-submissions.md#create-an-app-submission) e [Criar um envio de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
* Processar as solicitações HTTP para a API de envio da Windows Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)

