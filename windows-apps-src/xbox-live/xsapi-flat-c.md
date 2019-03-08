---
title: APIs do Xbox Live C
description: Saiba mais sobre o modelo da C API simples que você pode usar para interagir com o serviço Xbox Live.
ms.date: 06/05/2018
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, c, xsapi
ms.localizationpriority: medium
ms.openlocfilehash: a1c73661b561d586f9e28957c7caa6a1b1f9cb03
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597521"
---
# <a name="introduction-to-the-xbox-live-c-apis"></a>Introdução às APIs de C ao vivo do Xbox

Em junho de 2018, uma nova camada de API de C simples foi adicionada à XSAPI. Essa nova camada de API resolve alguns problemas ocorridos com as camadas de C++ e a API do WinRT.

A API de C não abrange todos os recursos XSAPI ainda, mas os recursos adicionais estão sendo trabalhados. Todos os 3 API camadas, C, C++ e WinRT continuarão a ter suporte e adicionou recursos adicionais ao longo do tempo.

> [!NOTE]
> As APIs de C atualmente só funcionam com títulos que usam o Kit de desenvolvedor do Xbox (XDK). Eles não suportam jogos UWP neste momento.

## <a name="features-covered-by-the-c-apis"></a>Recursos abordados pelas APIs do C

A C API atualmente suporta os seguintes recursos e serviços:

- Conquistas
- Presença
- Perfil
- Redes sociais
- Gerenciador de social

## <a name="benefits-of-the-c-api-for-xsapi"></a>Benefícios da C API para XSAPI

- Permite que os títulos controlar as alocações de memória ao chamar XSAPI.
- Permite que os títulos ganhar controle total do thread de tratamento ao chamar XSAPI.
- Usa uma nova biblioteca HTTP, libHttpClient, projetada para desenvolvedores de jogos.

Você pode usar as APIs de C junto com o C++ XSAPI, mas você não obterá os benefícios listados anteriormente com as APIs do C++.

### <a name="managing-memory-allocations"></a>Gerenciando alocações de memória

Com a nova API de C, agora você pode especificar um retorno de chamada de função XSAPI será chamada sempre que ele tenta alocar memória. Se você não especificar os retornos de chamada de função, XSAPI usa rotinas de alocação de memória padrão.

Para especificar manualmente suas rotinas de memória, você pode fazer o seguinte:

- No início do jogo:
  - Chamar `XblMemSetFunctions(memAllocFunc, memFreeFunc)` para especificar os retornos de chamada de alocação para atribuir e liberar memória.
  - Chamar `XblInitialize()` para inicializar a instância de biblioteca.  
- Enquanto o jogo está em execução:
  - Chamar qualquer uma das novas APIs de C em XSAPI que alocar ou liberar memória fará com que a XSAPI chamar retornos de chamada de tratamento da memória especificada.  
- Quando o jogo é encerrado:
  - Chamar `XblCleanup()` recuperar todos os recursos associados com a biblioteca XSAPI.
  - Limpe o Gerenciador de memória personalizado do seu jogo.

### <a name="managing-asynchronous-threads"></a>Gerenciamento de threads assíncronos

A API de C introduz um novo thread assíncrono padrão que permite aos desenvolvedores controle completo sobre o modelo de threading de chamada. Para obter mais informações, consulte [padrão de chamada para XSAPI simples C camada async chamadas](flatc-async-patterns.md).

## <a name="migrating-code-to-use-c-xsapi"></a>Migrando o código para usar XSAPI C

As APIs de C XSAPI pode ser usada junto com as APIs de C++ XSAPI em um projeto, portanto, é recomendável que você migre um recurso por vez.

As APIs de C e APIs de C++ são invólucros finos realmente apenas em torno de um núcleo comum, apenas com pontos de entrada diferentes, para que a funcionalidade deve permanecer inalterada. No entanto, apenas as APIs de C pode tirar proveito da memória personalizada e thread recursos de gerenciamento.

> [!IMPORTANT]
> Você não pode misturar APIs do WinRT XSAPI com as APIs de C.

## <a name="where-to-view-the-c-apis"></a>Onde exibir as APIs de C

- [Arquivos de cabeçalho C API](https://github.com/Microsoft/xbox-live-api/tree/master/Include/xsapi-c)
- [Exemplo de código usando as novas APIs de C](https://github.com/Microsoft/xbox-live-api/tree/master/InProgressSamples/Social/Xbox/C)
- [libHttpClient](https://github.com/Microsoft/libHttpClient)
