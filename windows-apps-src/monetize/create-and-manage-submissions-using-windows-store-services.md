---
author: mcleanbyron
ms.assetid: 7CC11888-8DC6-4FEE-ACED-9FA476B2125E
description: "Use a API de envio da Windows Store para criar e gerenciar de forma programática os envios de apps que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: "Criar e gerenciar envios usando serviços da Windows Store"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Windows Store
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f73470c456bf59544bc702b137da64f57c6a6943
ms.lasthandoff: 02/07/2017

---

# <a name="create-and-manage-submissions-using-windows-store-services"></a>Criar e gerenciar envios usando serviços da Windows Store


Use a *API de envio da Windows Store* para consultar e criar programaticamente os envios de apps, complementos (também conhecidos como produtos no app ou IAPs) e pacotes de pré-lançamento para a sua conta e a para a conta do Centro de Desenvolvimento do Windows da sua organização. Essa API será útil se sua conta gerenciar muitos apps ou complementos e você quiser automatizar e otimizar o processo de envio para esses ativos. Essa API usa o Active Directory do Azure (Azure AD) para autenticar as chamadas do seu app ou serviço.

As etapas a seguir descrevem o processo completo de usar a API de envio da Windows Store:

1.  Certifique-se de que você tenha concluído todos os [pré-requisitos](#prerequisites).
3.  Antes de chamar um método na API de envio da Windows Store [obtenha um token de acesso do Azure AD](#obtain-an-azure-ad-access-token). Depois de obter um token, você tem 60 minutos para usá-lo em chamadas para a API de envio da Windows Store antes que ele expire. Depois que o token expirar, você poderá gerar um novo.
4.  [Chame a API de envio da Windows Store](#call-the-windows-store-submission-api).


<span id="not_supported" />
>**Observações importantes**

> * Essa API pode ser usada somente para contas do Centro de Desenvolvimento do Windows que têm permissão para usar a API. Essa permissão está sendo habilitada para contas de desenvolvedor em estágios, e nem todas as contas têm essa permissão habilitado no momento. Para solicitar acesso anterior, fazer logon no painel do Centro de Desenvolvimento, clique em **Comentários** na parte inferior do painel, selecione **API de envio** para a área de comentários e envie sua solicitação. Você receberá um email quando essa permissão for habilitada em sua conta.
<br/><br/>
>* Se você usar essa API para criar um envio para um app, um pacote de pré-lançamento ou um complemento, certifique-se de fazer outras alterações no envio somente usando a API, em vez do painel do Centro de desenvolvimento. Se você usar o painel para alterar um envio que criou originalmente usando a API, você não poderá alterar ou confirmar esse envio usando a API. Em alguns casos, o envio pode ficar em um estado de erro em que ele não pode continuar no processo de envio. Se isso ocorrer, você deve excluir o envio e criar um novo envio.
<br/><br/>
> * Essa API não pode ser usada com apps ou complementos que usam determinados recursos que foram introduzidos no painel do Centro de Desenvolvimento em agosto de 2016, incluindo (mas não se limitando a) atualizações obrigatórias de apps e complementos de consumíveis gerenciados pela Loja. Se você usar a API de envio da Windows Store com um app ou um complemento que usa um desses recursos, a API retornará um código de erro 409. Nesse caso, você deve usar o painel para gerenciar os envios para o app ou um complemento.


<span id="prerequisites" />
## <a name="step-1-complete-prerequisites-for-using-the-windows-store-submission-api"></a>Etapa 1: complete os pré-requisitos para usar a API de envio da Windows Store

Antes de começar a escrever o código para chamar a API de envio da Windows Store, certifique-se de que você concluiu os pré-requisitos a seguir.

* Você (ou sua organização) deve ter um diretório do Azure AD, e você deve ter permissão de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para o diretório. Se você já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem o diretório Azure AD. Caso contrário, você pode [criar um novo Azure AD no Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users) sem nenhum custo adicional.

* Você deve [associar um app Azure AD à sua conta do Centro de Desenvolvimento do Windows](#associate-an-azure-ad-application-with-your-windows-dev-center-account) e obter a ID de locatário, a ID do cliente e a chave. Você precisa desses valores para obter um token de acesso do Azure AD, que você usará em chamadas para a API de envio da Windows Store.

* Prepare seu app para uso com a API de envio da Windows Store:

  * Se seu app ainda não existir no Centro de Desenvolvimento, [crie seu app no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name). Você não pode usar a API de envio da Windows Store para criar um app no Centro de Desenvolvimento; você deve criar seu app usando o painel e, em seguida, usar a API para acessar o app e criar programaticamente envios para ele. No entanto, você pode usar a API para criar complementos e pacotes de pré-lançamento programaticamente antes de criar envios para eles.

  * Antes de criar um envio para um determinado app usando essa API, você deve primeiro [criar um envio para o app no painel do Centro de Desenvolvimento](https://msdn.microsoft.com/windows/uwp/publish/app-submissions), inclusive respondendo as perguntas sobre [classificações etárias](https://msdn.microsoft.com/windows/uwp/publish/age-ratings). Depois que você fizer isso, você poderá criar novos envios para este app usando a API programaticamente. Você não precisa criar um envio de complemento nem o envio de versão de pré-lançamento do pacote antes de usar a API para esses tipos de envios.

  * Se você estiver criando ou atualizando um envio de app e você precisar incluir um pacote do app, [prepare o pacote do app](https://msdn.microsoft.com/windows/uwp/publish/app-package-requirements).

  * Se você estiver criando ou atualizando um envio de app e você precisar incluir capturas de tela ou imagens para a listagem da Loja, [prepare as imagens e capturas de tela do app](https://msdn.microsoft.com/windows/uwp/publish/app-screenshots-and-images).

  * Se você estiver criando ou atualizando um envio de complemento e você precisar incluir um ícone, [prepare o ícone](https://msdn.microsoft.com/windows/uwp/publish/create-iap-descriptions#icon).

<span id="associate-an-azure-ad-application-with-your-windows-dev-center-account" />
### <a name="how-to-associate-an-azure-ad-application-with-your-windows-dev-center-account"></a>Como associar um app do Azure AD à sua conta do Centro de Desenvolvimento do Windows

Antes de usar a API de envio da Windows Store, você deverá associar um app Azure AD à sua conta do Centro de Desenvolvimento, recuperar a ID do locatário e a ID do cliente para o app e gerar uma chave. O app do Azure AD representa o app ou serviço do qual você quer chamar a API de envio da Windows Store. Você precisa da ID do locatário, da ID do cliente e da chave para obter um token de acesso do Azure AD que você passa para a API.

>**Observação**&nbsp;&nbsp;Você só precisa executar essa tarefa uma vez. Depois que você tiver a ID do locatário, a ID do cliente e a chave, poderá reutilizá-las sempre que precisa criar um novo token de acesso do Azure AD.

1.  No Centro de Desenvolvimento, acesse suas **Configurações de conta**, clique em **Gerenciar usuários** e associe a sua conta do Centro de Desenvolvimento ao diretório do Azure AD da sua organização. Para obter instruções detalhadas, consulte [Gerenciar usuários de conta](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users).

2.  Na página **Gerenciar usuários**, clique em **Adicionar apps do Azure AD**, adicione o app do Azure AD que representa o app ou o serviço que você usará para acessar envios de sua conta do Centro de Desenvolvimento e atribua-o a função **Gerente**. Se esse app já existe no diretório do Azure AD, selecione-o na página **Adicionar apps do Azure AD** para adicioná-lo à sua conta do Centro de Desenvolvimento. Do contrário, você pode criar um novo app do Azure AD na página **Add Azure AD applications**. Para obter mais informações, consulte [Adicionar e gerenciar apps Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

3.  Volte para a página **Gerenciar usuários**, clique no nome do seu app Azure AD para ir para as configurações do app e copie os valores da **ID do locatário** e da **ID do cliente**.

4. Clique em **Adicionar nova chave**. Na tela seguinte, copie o valor da **Chave**. Você não poderá acessar essas informações novamente depois que você sair desta página. Para obter mais informações, consulte as informações sobre o gerenciamento de chaves em [Adicionar e gerenciar apps Azure AD](https://msdn.microsoft.com/windows/uwp/publish/manage-account-users#add-and-manage-azure-ad-applications).

<span id="obtain-an-azure-ad-access-token" />
## <a name="step-2-obtain-an-azure-ad-access-token"></a>Etapa 2: Obter um token de acesso do Azure AD

Antes de chamar qualquer um dos métodos na API de envio da Windows Store, primeiro você deve obter um token de acesso do Azure AD que você passa para o cabeçalho **Autorização** de cada método na API. Depois de obter um token de acesso, você terá 60 minutos para usá-lo antes que ele expire. Depois que o token expirar, você poderá atualizar o token para que você possa continuar a usá-lo em outras chamadas à API.

Para obter o token de acesso, siga as instruções em [Chamadas de serviço a serviço usando credenciais do cliente](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-service-to-service/) para enviar um HTTP POST para o ponto de extremidade ```https://login.microsoftonline.com/<tenant_id>/oauth2/token```. Aqui está um exemplo de solicitação.

```
POST https://login.microsoftonline.com/<tenant_id>/oauth2/token HTTP/1.1
Host: login.microsoftonline.com
Content-Type: application/x-www-form-urlencoded; charset=utf-8

grant_type=client_credentials
&client_id=<your_client_id>
&client_secret=<your_client_secret>
&resource=https://manage.devcenter.microsoft.com
```

Para os valor de *tenant\_id* no POST URI e os parâmetros *client\_id* e *client\_secret*, especifique a ID do locatário, a ID do cliente e a chave para o app que você recuperou do Centro de Desenvolvimento na seção anterior. Para o parâmetro *resource*, especifique ```https://manage.devcenter.microsoft.com```.

Depois que seu token de acesso expirar, você poderá atualizá-lo seguindo as instruções descritas [aqui](https://azure.microsoft.com/documentation/articles/active-directory-protocols-oauth-code/#refreshing-the-access-tokens).

Para obter exemplos que demonstram como obter um token de acesso usado código C#, Java ou Python, consulte os [exemplos de código](#code-examples) da API de envio da Windows Store.

<span id="call-the-windows-store-submission-api">
## <a name="step-3-use-the-windows-store-submission-api"></a>Etapa 3: usar a API de envio da Windows Store

Depois que tiver um token de acesso do Azure AD, você poderá chamar métodos na API de envio da Windows Store. A API inclui muitos métodos que são agrupados em cenários de apps, complementos e pacotes de pré-lançamento. Para criar ou atualizar os envios, você normalmente chama vários métodos na API de envio da Windows Store em uma ordem específica. Para obter informações sobre cada cenário e a sintaxe de cada método, consulte os artigos na tabela a seguir.

>**Observação**&nbsp;&nbsp;Depois de obter um token de acesso, você tem 60 minutos para chamar métodos na API de envio da Windows Store antes que ele expire.

| Cenário       | Descrição                                                                 |
|---------------|----------------------------------------------------------------------|
| Aplicativos |  Recupere dados de todos os apps que estão registrados em sua conta do Centro de Desenvolvimento do Windows e crie os envios de apps. Para obter mais informações sobre esses métodos, consulte os seguintes artigos: <ul><li>[Obter dados de app](get-app-data.md)</li><li>[Gerenciar envios de app](manage-app-submissions.md)</li></ul> |
| Complementos | Obtenha, crie ou exclua complementos para seus apps e, em seguida, obtenha, crie ou exclua os envios dos complementos. Para obter mais informações sobre esses métodos, consulte os seguintes artigos: <ul><li>[Gerenciar complementos](manage-add-ons.md)</li><li>[Gerenciar envios de complemento](manage-add-on-submissions.md)</li></ul> |
| Pacotes de pré-lançamento | Obtenha, crie ou exclua pacotes de pré-lançamento para seus apps e, em seguida, obtenha, crie ou exclua os envios de pacotes de pré-lançamento. Para obter mais informações sobre esses métodos, consulte os seguintes artigos: <ul><li>[Gerenciar pacotes de pré-lançamento](manage-flights.md)</li><li>[Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)</li></ul> |

<span id="code-samples"/>
## <a name="code-examples"></a>Exemplos de código

Os artigos a seguir fornecem exemplos detalhados de código que demonstram como usar a API de envio da Windows Store em várias linguagens de programação diferentes:

* [Exemplos de código em C#](csharp-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código em Java](java-code-examples-for-the-windows-store-submission-api.md)
* [Exemplos de código em Python](python-code-examples-for-the-windows-store-submission-api.md)

>**Observação**&nbsp;&nbsp;além dos exemplos de código acima, também fornecemos um módulo de PowerShell de código-fonte aberto que implementa uma interface de linha de comando sobre o envio da Windows Store API. Esse módulo é chamado [StoreBroker](https://aka.ms/storebroker). Você pode usar esse módulo para gerenciar seu app, versão de pré-lançamento e envios de complemento na linha de comando em vez de chamar diretamente o envio da Windows Store API, ou você pode simplesmente procurar a fonte para ver mais exemplos de como chamar essa API. O módulo StoreBroker ativamente é usado dentro da Microsoft como a principal forma de muitos apps de terceiros serem enviados para a Loja. Para obter mais informações, consulte nosso [StoreBroker página no GitHub](https://aka.ms/storebroker).

## <a name="troubleshooting"></a>Solução de problemas

| Problema      | Resolução                                          |
|---------------|---------------------------------------------|
| Depois de chamar a API de envio da Windows Store do PowerShell, os dados de resposta da API ficarão corrompidos se você convertê-la do formato JSON para um objeto PowerShell usando o cmdlet [ConvertFrom-Json](https://technet.microsoft.com/library/hh849898.aspx) e depois voltar para o formato JSON usando o cmdlet [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx). |  Por padrão, o parâmetro *-Depth* para o cmdlet [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx) é definido como 2 níveis de objetos, que é muito superficial para a maioria dos objetos JSON que são retornados pela API de envio da Windows Store. Quando você chamar o cmdlet [ConvertTo Json](https://technet.microsoft.com/library/hh849922.aspx), defina o parâmetro *-Depth* como um número maior, como 20. |

## <a name="additional-help"></a>Ajuda adicional

Se você tiver dúvidas sobre a API de envio da Windows Store ou precisar de ajuda para gerenciar seus envios com essa API, use os seguintes recursos:

* Pergunte em nossos [fóruns](https://social.msdn.microsoft.com/Forums/windowsapps/home?forum=wpsubmit).
* Visite nossa [página de suporte](https://developer.microsoft.com/windows/support) e solicite uma das opções de suporte assistido para o painel do Centro de Desenvolvimento. Se você for solicitado a escolher um tipo de problema e categoria, escolha **Certificação e envio de app** e **Enviando um app**, respectivamente.  

## <a name="related-topics"></a>Tópicos relacionados

* [Obter dados de app](get-app-data.md)
* [Gerenciar envios de app](manage-app-submissions.md)
* [Gerenciar complementos](manage-add-ons.md)
* [Gerenciar envios de complemento](manage-add-on-submissions.md)
* [Gerenciar pacotes de pré-lançamento](manage-flights.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)
 

