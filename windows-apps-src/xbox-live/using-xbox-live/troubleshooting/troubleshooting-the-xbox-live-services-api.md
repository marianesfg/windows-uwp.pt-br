---
title: A API de serviços em tempo real do Xbox de solução de problemas
description: Saiba como registrar informações de erro extra durante a solução de problemas com as APIs do Xbox Live.
ms.assetid: 3827bba1-902f-4f2d-ad51-af09bd9354c4
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, solução de problemas, erro, log
ms.localizationpriority: medium
ms.openlocfilehash: 67735d83e5ff301ee4434a6917fce814cabe8309
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57621411"
---
# <a name="troubleshooting-the-xbox-live-apis"></a>As APIs do Xbox Live de solução de problemas

## <a name="code"></a>Código

É difícil diagnosticar uma falha usando apenas o erro da camada de API de serviços do Xbox Live. Informações de erro extra — como registro em log de todas as chamadas RESTful — poderia estar disponível para o servidor. Para escutar a esses dados, conectar o agente de resposta e habilitar o rastreamento de depuração. Registro em log de resposta permite que você veja o tráfego e o web service códigos de resposta HTTP, que geralmente é tão útil quando um rastreamento do Fiddler.

### <a name="c"></a>C++

O exemplo de código a seguir habilita o log de resposta e define o nível de erro de depuração para detalhado (você também pode definir o nível de erro de depuração para o erro para mostrar somente o rastreamento de chamadas com falha ou Off para desabilitar o rastreamento). A saída de depuração resultante é enviada para o painel de saída ao executar seu projeto no Visual Studio.  

```cpp

        // Set up debug tracing to the Output window in Visual Studio.
            xbox::services::system::xbox_live_services_settings::get_singleton_instance()->set_diagnostics_trace_level(
                xbox_services_diagnostics_trace_level::verbose
                );
```

Você também pode optar por redirecionar a saída de depuração para seu próprio arquivo de log da seguinte forma:

```cpp

        // Set up debug tracing of the Xbox Live Services API traffic to the game UI.
        m_xboxLiveContext->Settings->EnableServiceCallRoutedEvents = true;
        m_xboxLiveContext->Settings->ServiceCallRouted += ref new
        Windows::Foundation::EventHandler<Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^>(
            [=] ( Platform::Object^, Microsoft::Xbox::Services::XboxServiceCallRoutedEventArgs^ args )
            {
                gameUI->Log(L"[URL]: " + args->HttpMethod + " " + args->Url->AbsoluteUri);
                gameUI->Log(L"");
                gameUI->Log(L"[Response]: " + args->HttpStatus.ToString() + " " + args->ResponseBody);
            });

```
