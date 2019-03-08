---
title: Tratamento de erros da API C++
description: Saiba como tratar erros ao fazer uma chamada de serviço Xbox Live com as APIs do C++.
ms.assetid: 10b47e68-8b1f-4023-96a4-404f3f6a9850
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, tratamento de erros
ms.localizationpriority: medium
ms.openlocfilehash: 90fd816f8d44b27c1df0ded9bee6473f642478a9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57636521"
---
# <a name="c-api-error-handling"></a>Tratamento de erros da API C++

A API do C++, em vez de lançar exceções, a maioria das chamadas retornará xbox_live_result < payload_type > conforme apropriado.

## <a name="xboxliveresult-structure"></a>estrutura xbox_live_result
xbox_live_result tem 3 itens:
1. O erro retornado pela operação,
2. Mensagem de erro específica usada para fins de depuração e
3. A carga do resultado (pode estar vazia se ocorreu um erro)"

Você pode obter mais informações sobre xbox_live_result como bem como qual o erro códigos estão na documentação do Xbox Live.

A estrutura é a seguinte:

```cpp
template<typename T>
class xbox_live_result
{
    const std::error_code& err();
    const std::string& err_message();
    T& payload();
};
```

**Err** -retorna o erro.  Será uma referência nula sem erro.  Esse comportamento é como o C++ STL erro faz em que você pode obter o valor primitivo chamando Value ().  Message () de chamada receberá uma representação de cadeia de caracteres.  Portanto, se o código de erro significa que "Argumento inválido", em seguida, ```err().message()``` seria o texto "Argumento inválido".

**err_message** -elabora o erro.  Por exemplo se **err** é "Argumento inválido", em seguida, **err_message** seria elaborar na qual o argumento é inválido.

**carga** -retornar o item de interesse.  Por exemplo considere ```xbox_live_result<achievement>``` que pode ser obtido chamando get_achievement.  Neste exemplo, a carga seria feito em si (se nenhum erro estiver presente).

## <a name="example"></a>Exemplo

```cpp
// Function which returns an xbox_live_result
xbox_live_result<std::shared_ptr<title_presence_change_subscription>> presenceChangeSubscriptionResult =
xbox::services::presence::subscribe_to_title_presence_change(
    xboxUserId,
    titleId
    );

printf("Error value %d, string %s", achievementResult.err().value(), achievementResult.err().message());

// Would output:
// "0 Success" if successful
// "401 Unauthorized" if auth issue

if (!achievementResult.err())
{
  // Do things on success.  Payload will be populated if applicable.
  std::shared_ptr<title_presence_change_subscription> presenceChangeSubscription = presenceChangeSubscriptionResult->payload();

  // ...
}
else if (achievement.err() == xboxlive_error_code::http_status_403_forbidden)
{
  // Special handling for 403 errors
}
else if (achievementResult.err() == xbox_live_error_condition::auth)
{
  // Handle broad auth failures.  See below section for more info on xbox_live_error_condition
}

```

## <a name="using-xboxliveerrorcondition-to-test-against-broad-error-categories"></a>Usando xbox_live_error_condition para testar com base em categorias amplas de erro
No exemplo acima, podemos testar o código de erro em relação a 403 erros, bem como algo chamado ```xbox_live_error_condition::auth```.

 Ao usar a função de erro () xbox_live_result, um pode testar em relação a códigos de erro individualmente.  Por exemplo, para 400 erros de classe, você poderia ter testes individuais e controle de fluxo para:

* xbox_live_error_code::http_status_400_bad_request
* xbox_live_error_code::http_status_401_unauthorized
* xbox_live_error_code::http_status_403_forbidden
* etc

Mas normalmente isso é não o que você deseja fazer, e você deseja testar em relação a uma classe de erros como um.  Portanto, você pode testar em relação a uma classe de erros usando as enums disponíveis no ```xbox_live_error_condition``` classe.  Implementamos uma sobrecarga para o operador de igualdade que irá automatizar testes em relação a muitos códigos de erro.  Além ```auth```, há categorias, como ```rta``` e ```http```.  A lista completa pode ser encontrada em *errors.h* ou na *xblsdk_cpp.chm*.

Para obter um vídeo que aborda isso e alguns outros recursos da API do serviço C++ Xbox, faça check-out de nossa conversa XFest na [Xfest 2015 vídeos](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2015.aspx) sob *XSAPI: C++, nenhuma exceção!*
