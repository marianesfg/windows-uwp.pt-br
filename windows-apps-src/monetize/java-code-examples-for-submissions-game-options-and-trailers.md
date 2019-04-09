---
description: Use os exemplos de código Java nesta seção para saber mais sobre como enviar opções de jogo e trailers usando a API de envio da Microsoft Store.
title: Exemplo de Java - envio de aplicativo com opções e trailers de jogos
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, uwp, API de envio da Microsoft Store, exemplos de código, opções de jogo, trailers, listagens avançadas, java
ms.localizationpriority: medium
ms.openlocfilehash: 93c629d509aaf3f92c3010e18077270339c7d332
ms.sourcegitcommit: 6a7dd4da2fc31ced7d1cdc6f7cf79c2e55dc5833
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58334294"
---
# <a name="java-sample-app-submission-with-game-options-and-trailers"></a>Exemplo de Java: envio de aplicativo com opções de jogo e trailers

Este artigo fornece exemplos de código Java que demonstram como usar a [API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tarefas:

* Obter um token de acesso do Azure AD a ser usado com a API de envio da Microsoft Store.
* Criar um envio de aplicativo
* Configure os dados de listagem da Loja para o envio de aplicativo, incluindo as opções de listagem avançada de [jogos](manage-app-submissions.md#gaming-options-object) e [trailers](manage-app-submissions.md#trailer-object).
* Carregue o arquivo ZIP que contém os pacotes, as imagens de listagem e os arquivos de trailer para o envio de aplicativo.
* Confirme o envio de aplicativo.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Criar um envio de aplicativo

A classe `CreateAndSubmitSubmissionExample` implementa um programa `main` que chama outros métodos de exemplo para usar a API de envio da Microsoft Store para criar e confirmar um envio de aplicativo que contém as opções de jogo e um trailer. Para adaptar este código para seu próprio uso:

* Atribua a variável `tenantId` à ID do locatário do seu app e atribua as variáveis `clientId` e `clientSecret` à ID e a chave do cliente para seu app. Para obter mais informações, consulte [como associar um aplicativo do AD do Azure com sua conta no Partner Center](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Atribua a variável `applicationId` à [ID da Loja](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja criar um envio.

> [!div class="tabbedCodeSnippets"]
[!code-java[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/CreateAndSubmitSubmissionExample.java#L1-L313)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obter um token de acesso do Azure AD

A classe `DevCenterAccessTokenClient` define um método auxiliar que usa os seus valores `tenantId`, `clientId` e `clientSecret` para criar um token de acesso Azure AD a ser usado com a API de envio da Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterAccessTokenClient.java#L1-L69)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Métodos auxiliares para invocar a API de envio e carregar os arquivos de envio

A classe `DevCenterClient` define os métodos auxiliares que invocam uma variedade de métodos na API de envio da Microsoft Store e carregam o arquivo ZIP que contém os pacotes, as imagens de listagem e os arquivos de trailer para o envio de aplicativo.

> [!div class="tabbedCodeSnippets"]
[!code-java[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/java/DevCenterClient.java#L1-L224)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
