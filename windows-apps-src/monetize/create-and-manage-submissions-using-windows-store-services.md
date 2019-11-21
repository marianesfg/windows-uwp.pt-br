---
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: Use a API de envio de Microsoft Store para criar e gerenciar programaticamente os envios de aplicativos que estão registrados em sua conta do Partner Center.
title: Criar e gerenciar envios
ms.date: 06/04/2018
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store
ms.localizationpriority: medium
ms.openlocfilehash: 8a41c0784302310be6f65a71b68233161a1c627d
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260312"
---
# <a name="create-and-manage-submissions"></a>Criar e gerenciar envios

Use a *API de envio de Microsoft Store* para consultar e criar programaticamente os envios de aplicativos, Complementos e vôos de pacote para sua conta do centro de parceiros da sua organização. Essa API será útil se sua conta gerenciar muitos aplicativos ou complementos e você quiser automatizar e otimizar o processo de envio para esses ativos. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu aplicativo ou serviço.

As etapas a seguir descrevem o processo completo de usar a API de envio da Microsoft Store:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
3.  Antes de chamar um método na API de envio da Microsoft Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Microsoft Store antes que ele expire. Depois que o token expirar, será possível gerar um novo.
4.  [Chame a API de envio da Microsoft Store](#call-the-windows-store-submission-api).

<span id="not_supported" />

> [!IMPORTANT]
> Se você usar essa API para criar um envio para um aplicativo, um vôo de pacote ou um complemento, certifique-se de fazer outras alterações no envio somente usando a API, em vez de no Partner Center. Se você usar o Partner Center para alterar um envio originalmente criado usando a API, não será mais possível alterar ou confirmar esse envio usando a API. Em alguns casos, o envio pode ficar em um estado de erro em que ele não pode continuar no processo de envio. Se isso ocorrer, você deve excluir o envio e criar um novo.

> [!IMPORTANT]
> Você não pode usar essa API para publicar os envios de [compras de volume por meio da Microsoft Store para Empresas e da Microsoft Store para Educação](../publish/organizational-licensing.md) ou publicar os envios de [aplicativos LOB](../publish/distribute-lob-apps-to-enterprises.md) diretamente para empresas. Para ambos os cenários, você deve usar publicar o envio no Partner Center.

> [!NOTE]
> Essa API não pode ser usada com apps ou complementos que usem atualizações de app obrigatórias e complementos consumíveis gerenciados pela Store. Se você usar a API de envio da Microsoft Store com um aplicativo ou um complemento que usa um desses recursos, a API retornará um código de erro 409. Nesse caso, você deve usar o Partner Center para gerenciar os envios para o aplicativo ou complemento.

<span id="prerequisites" />

## <a name="step-1-complete-prerequisites-for-using-the-microsoft-store-submission-api"></a>Etapa 1: Concluir os pré-requisitos para usar a API de envio da Microsoft Store

Antes de começar a escrever o código para chamar a API de envio da Microsoft Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Partner Center](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) sem custo adicional.

* Você deve [associar um aplicativo do Azure ad à sua conta do Partner Center](#associate-an-azure-ad-application-with-your-windows-partner-center-account) e obter a ID do locatário, a ID do cliente e a chave. Você precisa desses valores para obter um token de acesso do Azure AD, que você usará em chamadas para a API de envio da Microsoft Store.

* Prepare seu aplicativo para uso com a API de envio da Microsoft Store:

  * Se seu aplicativo ainda não existir no Partner Center, você deverá [criar seu aplicativo reservando seu nome no Partner Center](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Você não pode usar a API de envio de Microsoft Store para criar um aplicativo no Partner Center; Você deve trabalhar no Partner Center para criá-lo e, depois disso, pode usar a API para acessar o aplicativo e criar programaticamente envios para ele. No entanto, você pode usar a API para criar complementos e pacotes de pré-lançamento programaticamente antes de criar envios para eles.

  * Antes de criar um envio para um determinado aplicativo usando essa API, você deve primeiro [criar um envio para o aplicativo no Partner Center](https://docs.microsoft.com/windows/uwp/publish/app-submissions), incluindo responder ao questionário de [classificações etárias](https://docs.microsoft.com/windows/uwp/publish/age-ratings) . Depois que você fizer isso, você poderá criar novos envios para este aplicativo usando a API programaticamente. Você não precisa criar um envio de complemento nem o envio de versão de pré-lançamento do pacote antes de usar a API para esses tipos de envios.

  * Se você estiver criando ou atualizando um envio de aplicativo e você precisar incluir um pacote do aplicativo, [prepare o pacote do aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Se você estiver criando ou atualizando um envio de aplicativo e você precisar incluir capturas de tela ou imagens para a listagem da Loja, [prepare as imagens e capturas de tela do aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Se você estiver criando ou atualizando um envio de complemento e você precisar incluir um ícone, [prepare o ícone](https://docs.microsoft.com/windows/uwp/publish/create-iap-descriptions).

<span id="associate-an-azure-ad-application-with-your-windows-partner-center-account" />

### <a name="how-to-associate-an-azure-ad-application-with-your-partner-center-account"></a>Como associar um aplicativo do Azure AD à sua conta do Partner Center

Antes de usar a API de envio do Microsoft Store, você deve associar um aplicativo do Azure AD à sua conta do Partner Center, recuperar a ID do locatário e a ID do cliente para o aplicativo e gerar uma chave. O aplicativo do Azure AD representa o aplicativo ou serviço do qual você quer chamar a API de envio da Microsoft Store. Você precisa da ID do locatário, da ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.

> [!NOTE]
> Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

1.  No Partner Center, [associe a conta do Partner Center da sua organização ao diretório do Azure ad da sua organização](../publish/associate-azure-ad-with-partner-center.md).

2.  Em seguida, na página **usuários** na seção **configurações de conta** do Partner Center, [adicione o aplicativo do Azure ad](../publish/add-users-groups-and-azure-ad-applications.md#add-azure-ad-applications-to-your-partner-center-account) que representa o aplicativo ou serviço que você usará para acessar os envios para sua conta do Partner Center. Certifique-se de atribuir esse aplicativo à **Manager**. Se o aplicativo ainda não existir no diretório do Azure AD, você poderá [criar um novo aplicativo do Azure AD no Partner Center](../publish/add-users-groups-and-azure-ad-applications.md#create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account).  

3.  Volte para a página **Usuários**, clique no nome do seu aplicativo Azure AD para ir para as configurações do aplicativo e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte [Gerenciar chaves para um aplicativo do Azure AD](../publish/add-users-groups-and-azure-ad-applications.md#manage-keys).

<span id="obtain-an-azure-ad-access-token" />

## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de envio da Microsoft Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Authorization** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar um HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```json
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para o valor da *ID de\_do locatário* no URI de postagem e os parâmetros de id de *\_* do cliente e\_de *segredo do cliente* , especifique a ID do locatário, a ID do cliente e a chave do aplicativo que você recuperou do Partner Center na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Para obter exemplos que demonstram como obter um token de acesso usado código C#, Java ou Python, consulte os [exemplos de código](#code-examples) da API de envio da Microsoft Store.

<span id="call-the-windows-store-submission-api">

## <a name="step-3-use-the-microsoft-store-submission-api"></a>Etapa 3: Usar a API de envio da Microsoft Store

Depois que tiver um token de acesso do Azure AD, você poderá chamar métodos na API de envio da Microsoft Store. A API inclui muitos métodos que são agrupados em cenários de aplicativos, complementos e pacotes de pré-lançamento. Para criar ou atualizar os envios, você normalmente chama vários métodos na API de envio da Microsoft Store em uma ordem específica. Para obter informações sobre cada cenário e a sintaxe de cada método, consulte os artigos na tabela a seguir.

> [!NOTE]
> Depois de obter um token de acesso, você tem 60 minutos para chamar métodos na API de envio da Microsoft Store antes que o token expire.

| Cenário       | Descrição                                                                 |
|---------------|----------------------------------------------------------------------|
| Apps |  Recupere dados para todos os aplicativos que estão registrados em sua conta do Partner Center e crie envios para aplicativos. Para obter mais informações sobre esses métodos, consulte os seguintes artigos: <ul><li>[Obter dados do aplicativo](get-app-data.md)</li><li>[Gerenciar envios de aplicativo](manage-app-submissions.md)</li></ul> |
| Complementos | Obtenha, crie ou exclua complementos para seus aplicativos e, em seguida, obtenha, crie ou exclua os envios dos complementos. Para obter mais informações sobre esses métodos, consulte os seguintes artigos: <ul><li>[Gerenciar Complementos](manage-add-ons.md)</li><li>[Gerenciar envios de complemento](manage-add-on-submissions.md)</li></ul> |
| Pacotes de pré-lançamento | Obtenha, crie ou exclua pacotes de pré-lançamento para seus aplicativos e, em seguida, obtenha, crie ou exclua os envios de pacotes de pré-lançamento. Para obter mais informações sobre esses métodos, consulte os seguintes artigos: <ul><li>[Gerenciar vôos de pacote](manage-flights.md)</li><li>[Gerenciar envios de vôo de pacote](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>

## <a name="code-examples"></a>Exemplos de código

Os artigos a seguir fornecem exemplos detalhados de código que demonstram como usar a API de envio da Microsoft Store em várias linguagens de programação diferentes:

* [C#exemplo: envios de aplicativos, Complementos e vôos](csharp-code-examples-for-the-windows-store-submission-api.md)
* [C#exemplo: envio de aplicativo com opções de jogo e trailers](csharp-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemplo de Java: envios de aplicativos, Complementos e vôos](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de Java: envio de aplicativo com opções de jogo e trailers](java-code-examples-for-submissions-game-options-and-trailers.md)
* [Exemplo de Python: envios de aplicativos, Complementos e vôos](python-code-examples-for-the-windows-store-submission-api.md)
* [Exemplo de Python: envio de aplicativo com opções de jogo e trailers](python-code-examples-for-submissions-game-options-and-trailers.md)

## <a name="storebroker-powershell-module"></a>Módulo StoreBroker do PowerShell

Como uma alternativa à chamada direta à API de envio da Microsoft Store, nós também fornecemos um módulo do PowerShell de software livre que implementa uma interface de linha de comando sobre API. Esse módulo é chamado [StoreBroker](https://github.com/Microsoft/StoreBroker). Você pode usar esse módulo para gerenciar seu app, versão de pré-lançamento e envios de complemento na linha de comando em vez de chamar diretamente a API de envio da Microsoft Store, ou você pode simplesmente procurar a fonte para ver mais exemplos de como chamar essa API. O módulo StoreBroker ativamente é usado dentro da Microsoft como a principal forma de muitos apps de terceiros serem enviados para a Store.

Para obter mais informações, consulte nossa [Página do StoreBroker no GitHub](https://github.com/Microsoft/StoreBroker).

## <a name="troubleshooting"></a>Solução de problemas

| Problema      | Resolução                                          |
|---------------|---------------------------------------------|
| Depois de chamar a API de envio da Microsoft Store do PowerShell, os dados de resposta da API ficarão corrompidos se você convertê-la do formato JSON para um objeto do PowerShell usando o cmdlet [ConvertFrom-Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertFrom-Json) e depois voltar para o formato JSON usando o cmdlet [ConvertTo Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json). |  Por padrão, o parâmetro *-Depth* para o cmdlet [ConvertTo Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json) é definido como dois níveis de objetos, que é muito superficial para a maioria dos objetos JSON que são retornados pela API de envio da Microsoft Store. Quando você chamar o cmdlet [ConvertTo Json](https://docs.microsoft.com/powershell/module/5.1/microsoft.powershell.utility/ConvertTo-Json), defina o parâmetro *-Depth* como um número maior, como 20. |

## <a name="additional-help"></a>Ajuda adicional

Se você tiver dúvidas sobre a API de envio da Microsoft Store ou precisar de ajuda para gerenciar seus envios com essa API, use os seguintes recursos:

* Pergunte em nossos [fóruns](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visite nossa [página de suporte](https://developer.microsoft.com/windows/support) e solicite uma das opções de suporte assistido para o Partner Center. Se você for solicitado a escolher um tipo de problema e categoria, escolha **Certificação e envio de aplicativo** e **Enviando um aplicativo**, respectivamente.  

## <a name="related-topics"></a>Tópicos relacionados

* [Obter dados do aplicativo](get-app-data.md)
* [Gerenciar envios de aplicativo](manage-app-submissions.md)
* [Gerenciar Complementos](manage-add-ons.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Gerenciar vôos de pacote](manage-flights.md)
* [Gerenciar envios de vôo de pacote](manage-flight-submissions.md)
