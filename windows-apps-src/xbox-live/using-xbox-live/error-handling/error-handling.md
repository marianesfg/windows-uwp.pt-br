---
title: Tratamento de erros
description: Saiba como tratar erros ao fazer um serviço Xbox Live a chamada.
ms.assetid: e433dfbd-488b-44ff-8333-1dcf0329cd60
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, erro, chamada de serviço
ms.localizationpriority: medium
ms.openlocfilehash: 637b9da84ef3ae50ea6235ca86e6318ae62acd11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57637001"
---
# <a name="error-handling"></a>Tratamento de erros

Os desenvolvedores devem tomar cuidado especial ao fazer uma chamada de serviço. O título deve sempre tratar erros ao fazer chamadas de rede. Mesmo para chamadas de serviço que tem trabalhado consistentemente durante o desenvolvimento, chamadas de serviço podem falhar em ambientes do mundo real por vários motivos diferentes. Por exemplo, sua chamada poderá falhar devido a:

* A rede não está disponível.
* O serviço está muito ocupado, retornando um código de status HTTP 503.
* A solicitação não é válida, retornando um código de status HTTP 400.
* O usuário não tem as permissões corretas, retornando um código de status HTTP 403.
* O usuário foi bloqueado, retornando um código de status HTTP 401.
IXMLHTTPRequest2, que é chamado pelo serviço APIs do WinRT, é possível enviar a solicitação (ERROR_TIMEOUT, RPC_S_CALL_FAILED_DNE, etc.)
* A lista acima não é exaustiva. A maioria desses problemas resultará em uma exceção sendo lançada da camada de API de serviços. O título deve capturar essas exceções e tratá-las adequadamente.

Existe um dois estilos diferentes de na API de serviços de Xbox (XSAPI) dependendo da API de tratamento de erro você está usando:

Ver [tratamento de erro da API do C++](error-handling-cpp.md).

Ver [tratamento de erros de API do WinRT](error-handling-winrt.md).
