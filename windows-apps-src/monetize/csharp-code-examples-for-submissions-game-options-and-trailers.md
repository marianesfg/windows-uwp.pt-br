---
author: Xansky
description: Use os exemplos de código C# nesta seção para saber mais sobre como enviar opções de jogo e trailers usando a API de envio da Microsoft Store.
title: Exemplo de C# - envio de aplicativo com opções de jogo e trailers
ms.author: mhopkins
ms.date: 07/10/2017
ms.topic: article
keywords: Windows 10, uwp, API de envio da Microsoft Store, exemplos de código, opções de jogos, trailers, listagens avançadas, C#
ms.localizationpriority: medium
ms.openlocfilehash: 61d2a919d6bebcb1807d8084dda39a2e6d660ca5
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/10/2018
ms.locfileid: "6278222"
---
# <a name="c-sample-app-submission-with-game-options-and-trailers"></a>Exemplo de C#: envio de aplicativo com opções de jogo e trailers

Este artigo fornece exemplos de código C# que demonstram como usar a [API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tarefas:

* Obter um token de acesso do Azure AD a ser usado com a API de envio da Microsoft Store.
* Criar um envio de aplicativo
* Configure os dados de listagem da Loja para o envio de aplicativo, incluindo as opções de listagem avançada de [jogos](manage-app-submissions.md#gaming-options-object) e [trailers](manage-app-submissions.md#trailer-object).
* Carregue o arquivo ZIP que contém os pacotes, as imagens de listagem e os arquivos de trailer para o envio de aplicativo.
* Confirme o envio de aplicativo.

Você pode examinar cada exemplo para saber mais sobre a tarefa que ele demonstra ou compilar todos os exemplos de código neste artigo em um aplicativo de console. Para compilar os exemplos, crie um aplicativo de console C# chamado **DevCenterApiSample** no Visual Studio, copie cada exemplo em um arquivo de código separado no projeto e compile o projeto.

## <a name="prerequisites"></a>Pré-requisitos

Estes exemplos têm os seguintes requisitos:

* Adicione a referência ao assembly System.Web em seu projeto.
* Instale o pacote NuGet [Newtonsoft.Json](http://www.newtonsoft.com/json) da Newtonsoft para seu projeto.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Criar um envio de aplicativo

A classe ```CreateAndSubmitSubmissionExample``` define um método ```Execute``` público que chama outros métodos de exemplo para usar a API de envio da Microsoft Store para criar e confirmar um envio de aplicativo que contém as opções de jogo e um trailer. Para adaptar este código para seu próprio uso:

* Atribua a variável ```tenantId``` à ID do locatário do seu app e atribua as variáveis ```clientId``` e ```clientSecret``` à ID e a chave do cliente para seu app. Para obter mais informações, consulte [como associar um aplicativo do Azure AD com sua conta do Partner Center](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-partner-center-account)
* Atribua a variável ```applicationId``` à [ID da Loja](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja criar um envio.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/CreateAndSubmitSubmissionExample.cs#CreateAndSubmitSubmissionExample)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token"></a>Obter um token de acesso do Azure AD

A classe ```DevCenterAccessTokenClient``` define um método auxiliar que usa os seus valores ```tenantId```, ```clientId``` e ```clientSecret``` para criar um token de acesso Azure AD a ser usado com a API de envio da Microsoft Store.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterAccessTokenClient.cs#DevCenterAccessTokenClient)]

<span id="utilities" />

## <a name="helper-methods-to-invoke-the-submission-api-and-upload-submission-files"></a>Métodos auxiliares para invocar a API de envio e carregar os arquivos de envio

A classe ```DevCenterClient``` define os métodos auxiliares que invocam uma variedade de métodos na API de envio da Microsoft Store e carregam o arquivo ZIP que contém os pacotes, as imagens de listagem e os arquivos de trailer para o envio de aplicativo.

> [!div class="tabbedCodeSnippets"]
[!code-cs[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/cs/DevCenterClient.cs#DevCenterClient)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
