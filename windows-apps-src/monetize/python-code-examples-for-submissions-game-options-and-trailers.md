---
author: mcleanbyron
description: Use os exemplos de código Python nesta seção para saber mais sobre como enviar opções de jogo e trailers usando a API de envio da Microsoft Store.
title: Exemplo de Python - envio de aplicativo com opções de jogo e trailers
ms.author: mcleans
ms.date: 07/10/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, API de envio da Microsoft Store, exemplos de código, opções de jogos, trailers, listagens avançadas, python
ms.localizationpriority: medium
ms.openlocfilehash: ea7d28cd495a512573f8d0f7c067cf35f696db3b
ms.sourcegitcommit: 0ab8f6fac53a6811f977ddc24de039c46c9db0ad
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/15/2018
ms.locfileid: "1653555"
---
# <a name="python-sample-app-submission-with-game-options-and-trailers"></a>Exemplo de Python: envio de aplicativo com opções e trailers de jogos

Este artigo fornece exemplos de código Python que demonstram como usar a [API de envio da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md) para estas tarefas:

* Obter um token de acesso do Azure AD a ser usado com a API de envio da Microsoft Store.
* Criar um envio de aplicativo
* Configure os dados de listagem da Loja para o envio de aplicativo, incluindo as opções de listagem avançada de [jogos](manage-app-submissions.md#gaming-options-object) e [trailers](manage-app-submissions.md#trailer-object).
* Carregue o arquivo ZIP que contém os pacotes, as imagens de listagem e os arquivos de trailer para o envio de aplicativo.
* Confirme o envio de aplicativo.

<span id="create-app-submission" />

## <a name="create-an-app-submission"></a>Criar um envio de aplicativo

Esse código chama outras classes e funções de exemplo para usar a API de envio da Microsoft Store para criar e confirmar um envio de aplicativo que contém as opções de jogo e um trailer. Para adaptar este código para seu próprio uso:

* Atribua a variável ```tenant``` à ID do locatário do seu app e atribua as variáveis ```client``` e ```secret``` à ID e a chave do cliente para seu app. Para obter mais informações, consulte [Como associar um aplicativo do Azure AD à sua conta do Centro de Desenvolvimento do Windows](create-and-manage-submissions-using-windows-store-services.md#how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account)
* Atribua a variável ```application_id``` à [ID da Loja](in-app-purchases-and-trials.md#store-ids) do app para o qual você deseja criar um envio.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/CreateAndSubmitAppSubmissionExample.py#L1-L74)]

<span id="token" />

## <a name="obtain-an-azure-ad-access-token-and-invoke-the-submission-api"></a>Obter um token de acesso do Azure AD e invocar a API de envio

O exemplo a seguir define estas classes:

* A classe ```DevCenterAccessTokenClient``` define um método auxiliar que usa os seus valores ```tenantId```, ```clientId``` e ```clientSecret``` para criar um token de acesso Azure AD a ser usado com a API de envio da Microsoft Store.
* A classe ```DevCenterClient``` define os métodos auxiliares que invocam uma variedade de métodos na API de envio da Microsoft Store e carregam o arquivo ZIP que contém os pacotes, as imagens de listagem e os arquivos de trailer para o envio de aplicativo.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/devcenterclient.py#L1-L126)]

<span id="token" />

## <a name="get-app-submission-listing-data"></a>Obter dados de listagem de envio de aplicativo

O exemplo a seguir define as funções auxiliares que retornam dados de listagem formatados em JSON para um novo envio de aplicativo de exemplo.

> [!div class="tabbedCodeSnippets"]
[!code[SubmissionApi](./code/StoreServicesExamples_SubmissionAdvancedListings/python/submissiondatasamples.py#L1-L170)]

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)