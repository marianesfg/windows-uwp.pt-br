---
ms.assetid: FABA802F-9CB2-4894-9848-9BB040F9851F
description: Use os exemplos de código C# nesta seção para saber mais sobre como usar a API de envio da Microsoft Store.
title: Exemplo de C# - envios de apps, complementos e versões de pré-lançamento
ms.date: 08/03/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, exemplos de código, C#
ms.localizationpriority: medium
ms.openlocfilehash: b3073e2a5ffa445a39bdf6d54dd288be97c88207
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334964"
---
# <a name="c-sample-submissions-for-apps-add-ons-and-flights"></a>C\# exemplo: envios de voos, aplicativos e complementos

Este artigo fornece exemplos de código C# que demonstram como usar a [API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tarefas:

* [Criar um envio de aplicativo](#create-app-submission)
* [Criar um envio de complemento](#create-add-on-submission)
* [Atualizar uma apresentação de complemento](#update-add-on-submission)
* [Criar um envio de voo do pacote](#create-flight-submission)

É possível examinar cada exemplo para saber mais sobre a tarefa que ele demonstra ou compilar todos os exemplos de código neste artigo em um aplicativo de console. Para compilar os exemplos, crie um aplicativo de console C# chamado **DeveloperApiCSharpSample** no Visual Studio, copie cada exemplo em um arquivo de código separado no projeto e compile o projeto.

## <a name="prerequisites"></a>Pré-requisitos

Estes exemplos usam as seguintes bibliotecas:

* Microsoft.WindowsAzure.Storage.dll. Essa biblioteca está disponível no [SDK do Azure para .NET](https://azure.microsoft.com/downloads/) ou você pode obtê-la instalando o [pacote NuGet do WindowsAzure.Storage](https://www.nuget.org/packages/WindowsAzure.Storage).
* [Newtonsoft.Json](https://www.newtonsoft.com/json) pacote NuGet do Newtonsoft.

## <a name="main-program"></a>Programa principal

O exemplo a seguir implementa um programa de linha de comando que chama os outros métodos de exemplo neste artigo para demonstrar as diversas maneiras de usar a API do envio da Microsoft Store. Para adaptar este programa para seu próprio uso:

* Atribua as propriedades ```ApplicationId```, ```InAppProductId``` e ```FlightId``` à ID do app, ao complemento e ao pacote de pré-lançamento que você deseja gerenciar.
* Atribua as propriedades ```ClientId``` e ```ClientSecret``` para a ID e a chave do cliente para seu app e substitua a cadeia de caracteres *tenantid* na URL de ```TokenEndpoint``` pela ID de locatário de seu app. Para obter mais informações, consulte [como associar um aplicativo do AD do Azure com sua conta no Partner Center](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/Program.cs#Main)]

<span id="clientconfiguration" />

## <a name="clientconfiguration-helper-class"></a>Classe auxiliar ClientConfiguration

O app de exemplo usa a classe auxiliar ```ClientConfiguration``` para transmitir dados do Azure Active Directory e dados do app para cada um dos métodos de exemplo que usam a API de envio da Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/ClientConfiguration.cs#ClientConfiguration)]

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Criar um envio de aplicativo

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Microsoft Store para atualizar um envio de aplicativo. O ```RunAppSubmissionUpdateSample``` método na classe cria um novo envio como um clone do último envio publicado e, em seguida, ele atualiza e confirma o envio clonado Partner Center. Especificamente, o método ```RunAppSubmissionUpdateSample``` realiza estas tarefas:

1. Para começar, o método [obtém dados do aplicativo especificado](get-an-app.md).
2. Em seguida, ele [exclui o envio pendente para o aplicativo](delete-an-app-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o aplicativo](create-an-app-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele muda alguns detalhes para o novo envio e carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualizações](update-an-app-submission.md) e, em seguida [confirma](commit-an-app-submission.md) o envio de novo ao Partner Center.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-app-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/AppSubmissionUpdateSample.cs#AppSubmissionUpdateSample)]

<span id="create-add-on-submission" />

## <a name="create-an-add-on-submission"></a>Criar um envio de complemento

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Microsoft Store para criar um novo envio de complemento. O método ```RunInAppProductSubmissionCreateSample``` na classe executa estas tarefas:

1. Para começar, o método [cria um novo complemento](create-an-add-on.md).
2. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md).
3. Ele carrega um arquivo ZIP que contém ícones para o envio no armazenamento do Blob do Azure.
4. Em seguida, ele [confirma o novo envio Partner Center](commit-an-add-on-submission.md).
5. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionCreateSample.cs#InAppProductSubmissionCreateSample)]

<span id="update-add-on-submission" />

## <a name="update-an-add-on-submission"></a>Atualizar um envio de complemento

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Microsoft Store para atualizar um envio de complemento existente. O ```RunInAppProductSubmissionUpdateSample``` método na classe cria um novo envio como um clone do último envio publicado e, em seguida, ele atualiza e confirma o envio clonado Partner Center. Especificamente, o método ```RunInAppProductSubmissionUpdateSample``` realiza estas tarefas:

1. Para começar, o método [obtém dados do complemento especificado](get-an-add-on.md).
2. Em seguida, ele [exclui o envio pendente para o complemento](delete-an-add-on-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o complemento](create-an-add-on-submission.md) (o novo envio é uma cópia do último envio publicado).
5. Em seguida, ele [atualizações](update-an-add-on-submission.md) e, em seguida [confirma](commit-an-add-on-submission.md) o envio de novo ao Partner Center.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-an-add-on-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/InAppProductSubmissionUpdateSample.cs#InAppProductSubmissionUpdateSample)]

<span id="create-flight-submission" />

## <a name="create-a-package-flight-submission"></a>Criar um envio de pacote de pré-lançamento

O exemplo a seguir implementa uma classe que usa vários métodos na API de envio da Microsoft Store para atualizar um envio de pacote de pré-lançamento. O ```RunFlightSubmissionUpdateSample``` método na classe cria um novo envio como um clone do último envio publicado e, em seguida, ele atualiza e confirma o envio clonado Partner Center. Especificamente, o método ```RunFlightSubmissionUpdateSample``` realiza estas tarefas:

1. Para começar, o método [obtém dados do pacote de pré-lançamento especificado](get-a-flight.md).
2. Em seguida, ele [exclui o envio pendente para o pacote de pré-lançamento](delete-a-flight-submission.md), caso haja um.
3. Em seguida, ele [cria um novo envio para o pacote de pré-lançamento](create-a-flight-submission.md) (o novo envio é uma cópia do último envio publicado).
4. Ele carrega um novo pacote para o envio no armazenamento do Blob do Azure.
5. Em seguida, ele [atualizações](update-a-flight-submission.md) e, em seguida [confirma](commit-a-flight-submission.md) o envio de novo ao Partner Center.
6. Por fim, ele [verifica periodicamente o status do novo envio](get-status-for-a-flight-submission.md) até que o envio seja confirmado com êxito.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/FlightSubmissionUpdateSample.cs#FlightSubmissionUpdateSample)]

<span id="ingestionclient" />

## <a name="ingestionclient-helper-class"></a>Classe auxiliar IngestionClient

A classe ```IngestionClient``` fornece métodos auxiliares que são usados por outros métodos no app de exemplo para executar as seguintes tarefas:

* [Obter um token de acesso do Azure AD](create-and-manage-submissions-using-windows-store-services.md#obtain-an-azure-ad-access-token) que pode ser usado para chamar métodos na API de envio da Microsoft Store. Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.
* Carregar um arquivo ZIP contendo novos ativos para um envio de aplicativo ou complemento no armazenamento do Blob do Azure. Para obter mais informações sobre como carregar um arquivo ZIP no armazenamento do Blob do Azure para envios de aplicativos e complementos, consulte as instruções relevantes em [Criar um envio de aplicativo](manage-app-submissions.md#create-an-app-submission) e [Criar um envio de complemento](manage-add-on-submissions.md#create-an-add-on-submission).
* Processar as solicitações HTTP para a API de envio da Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-csharp[SubmissionApi](./code/StoreServicesExamples_Submission/cs/IngestionClient.cs#IngestionClient)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
