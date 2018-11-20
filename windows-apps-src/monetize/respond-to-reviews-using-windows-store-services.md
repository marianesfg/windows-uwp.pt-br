---
author: Xansky
ms.assetid: c92c0ea8-f742-4fc1-a3d7-e90aac11953e
description: Use a API de análises da Microsoft Store para enviar respostas programaticamente às críticas do seu app na Store.
title: Responder às críticas usando serviços da Store
ms.author: mhopkins
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, API de análises da Microsoft Store, responder às análises
ms.localizationpriority: medium
ms.openlocfilehash: 063c228a9a2fcfde9350af4872aabba44f9bb8a5
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7431801"
---
# <a name="respond-to-reviews-using-store-services"></a>Responder às críticas usando serviços da Store

Use a *API de análises da Microsoft Store* para enviar respostas às críticas ao seu aplicativo de forma programada na Store. Essa API é especialmente útil para desenvolvedores que desejam em massa responder às diversas análises sem usar o Partner Center. Essa API usa o Azure Active Directory (Azure AD) para autenticar as chamadas do seu app ou serviço.

As etapas a seguir descrevem o processo completo:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
2.  Antes de chamar um método na API de análises da Microsoft Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de análises da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.
3.  [Chame a API de análises da Microsoft Store](#call-the-windows-store-reviews-api).

> [!NOTE]
> Além de usar a Microsoft Store API de análises para responder programaticamente às críticas, você também pode responder às análises [usando Partner Center](../publish/respond-to-customer-reviews.md).

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-reviews-api"></a>Etapa 1: complete os pré-requisitos para usar a API de análises da Microsoft Store

Antes de começar a escrever o código para chamar a API de análises da Microsoft Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Partner Center](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sem nenhum custo adicional.

* Você deve associar um aplicativo Azure AD à sua conta do Partner Center, recuperar a ID do locatário e ID do cliente para o aplicativo e gerar uma chave. O app do Azure AD representa o app ou serviço do qual você quer chamar a API de análises da Microsoft Store. Você precisa da ID do locatário, da ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.
    > [!NOTE]
    > Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

Para associar um aplicativo Azure AD à sua conta do Partner Center e recuperar os valores necessários:

1.  No Partner Center, [associe a conta do Partner Center da sua organização com o diretório do Azure AD da sua organização](../publish/associate-azure-ad-with-dev-center.md).

2.  Em seguida, na página de **usuários** na seção **configurações da conta** do Partner Center, [Adicione o aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa o aplicativo ou serviço que você usará para responder às análises. Certifique-se de atribuir esse aplicativo à **Manager**. Se o aplicativo ainda não existe no diretório do Azure AD, você pode [criar um novo aplicativo Azure AD no Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account). 

3.  Volte para a página **Usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de análises da Microsoft Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar um HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```syntax
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para o valor de *tenant\_id* no POST URI e os parâmetros *client\_id* e *client\_secret* , especifique a ID de locatário, ID de cliente e a chave para seu aplicativo que você recuperou do Partner Center na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

<span id="call-the-windows-store-reviews-api" />

## <a name="step-3-call-the-microsoft-store-reviews-api"></a>Etapa 3: Chamar a API de análises da Microsoft Store

Depois que tiver um token de acesso do Azure AD, você estará pronto para chamar a API de análises da Microsoft Store. Você deve passar o token de acesso no cabeçalho **Autorização** de cada método.

A API de análises da Microsoft Store contém vários métodos que você pode usar para determinar se tem permissão para responder a uma determinada análise e enviar respostas a uma ou mais críticas. Siga este processo para usar essa API:

1. Obtenha as IDs das análises que deseja responder. As IDs de revisão estão disponíveis nos dados de resposta do método [obter avaliações de app](get-app-reviews.md) na API de análise da Microsoft Store e no [download offline](../publish/download-analytic-reports.md) do [Relatório de avaliações](../publish/reviews-report.md).
2. Chame o método [obter as informações de resposta para avaliações de app](get-response-info-for-app-reviews.md) para determinar se você tem permissão para responder as análises. Quando um cliente envia uma análise, pode optar por não receber respostas para ela. Você não poderá responder análises enviadas por clientes que optaram por não receber respostas a essas análises.
3. Chame o método [enviar respostas às análises de app](submit-responses-to-app-reviews.md) para responder programaticamente as análises.


## <a name="related-topics"></a>Tópicos relacionados

* [Obter avaliações de app](get-app-reviews.md)
* [Obter informações de resposta para avaliações de app](get-response-info-for-app-reviews.md)
* [Enviar respostas às avaliações do app](submit-responses-to-app-reviews.md)

 
