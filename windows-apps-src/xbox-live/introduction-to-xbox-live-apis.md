---
title: Introdução às APIs do Xbox Live
description: Saiba mais sobre os diversos modelos de API que você pode usar para interagir com o serviço Xbox Live.
ms.assetid: 5918c3a2-6529-4f07-b44d-51f9861f91ec
ms.date: 06/05/2018
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 1f52e379b524952c3361b432a577a7137b02155b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57657691"
---
# <a name="introduction-to-xbox-live-apis"></a>Introdução às APIs do Xbox Live

## <a name="use-xbox-live-services"></a>Usar os serviços do Xbox Live

Há duas maneiras de obter informações dos serviços do Xbox Live:

- Usar uma API do lado cliente chamada API de serviços do Xbox Live (**XSAPI**)
- Chame o **pontos de extremidade do Xbox Live REST** diretamente

As vantagens de usar a API de serviços do Xbox Live (**XSAPI**) incluem:

- Detalhes de autenticação, codificação e HTTP para envio e recebimento são manipulados para você.
- Argumentos para e dados retornados, que a API de wrapper é identificada em dados nativos tipos; Portanto, você não precisa executar a codificação e decodificação de JSON.
- Chamando serviços web diretamente envolve várias etapas assíncronas, que encapsula o wrapper de API; Isso facilita a leitura e gravação do código do título.
- Algumas funcionalidades, como gravar eventos de jogos, só estão disponível em XSAPI.

As vantagens de usar o **pontos de extremidade do Xbox Live REST** diretamente incluem:

- A capacidade de chamar o Xbox Live pontos de extremidade de um serviço web
- A capacidade de chamar pontos de extremidade que não estão incluídos no XSAPI.  XSAPI inclui apenas as APIs que acreditamos que jogos usará, para que se há algo faltando let nos saibam por meio de fóruns.
- Algumas funcionalidades disponíveis por meio de pontos de extremidade REST não podem ter um wrapper XSAPI correspondente.

Seus jogos e aplicativos não estão limitados a usar apenas um dos seguintes métodos. Você pode usar o wrapper XSAPI e ainda chamar os pontos de extremidade REST diretamente se necessário.

## <a name="xbox-live-services-api-overview"></a>Visão geral da API de serviços em tempo real do Xbox ##

A API de serviços do Xbox Live (**XSAPI**) expõe três conjuntos de APIs que oferecem suporte a uma ampla variedade de cenários de cliente do lado do cliente:

- [WinRT XSAPI API](#xsapi-winrt-based-api)
- [API baseada em XSAPI c++11](#xsapi-c11-based-api)
- [API baseada em C XSAPI](#xsapi-c-based-api) (**novo a partir de junho de 2018**)

Comparando as APIs:

### <a name="xsapi-winrt-based-api"></a>API baseada em XSAPI WinRT

- Dá suporte a aplicativos escritos com C + + c++ /CX, C#e JavaScript.
    - C + + c++ /CX é uma extensão do Microsoft C++ para facilitar a programação de WinRT por exemplo, usando ^ como ponteiros de WinRT.
- Dá suporte a aplicativos destinados ao Xbox XDK uma plataforma e Universal Windows Platform (UWP) x86, x64 e arquiteturas ARM.
- Os erros são tratados por meio de exceções em todas as linguagens, incluindo c++ /CLI CX.
- C + + c++ /CLI também há suporte para o WinRT.  Para obter mais informações sobre o C + + c++ /CLI WinRT pode ser encontrados em [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Aqui está um exemplo de chamar a API do WinRT XSAPI usando C + + c++ /CLI WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```

Se você desejar misturar C + + c++ /CLI CX e C + + c++ /CLI WinRT enquanto você estiver migrando o código pode fazer isso muito, mas é um pouco mais complexa.  
Aqui está um exemplo de chamar a API do WinRT XSAPI usando C + + c++ /CLI WinRT dado C + + c++ /CLI CX usuário ^ objeto.

```c++
::Windows::Xbox::System::User^ user1 = ::Windows::Xbox::System::User::Users->GetAt(0);
winrt::Windows::Xbox::System::User cppWinrtUser(nullptr);
winrt::copy_from(cppWinrtUser, reinterpret_cast<winrt::ABI::Windows::Xbox::System::IUser*>(user1));
winrt::Microsoft::Xbox::Services::XboxLiveContext xblContext(cppWinrtUser);
```


### <a name="xsapi-c11-based-api"></a>API baseada em XSAPI c++11

- Usa entre plataformas ISO padrão c++11
- Oferece suporte para aplicativos escritos com C++
- Dá suporte a aplicativos destinados ao Xbox XDK uma plataforma e Universal Windows Platform (UWP) x86, x64 e arquiteturas ARM.
- Erros são tratados por meio de std::error_code.
- O C + + 11 baseado em API é a API recomendada para uso para mecanismos de jogos C++ para melhorar o desempenho e melhor depuração.
- Se você estiver no programa de criadores do Xbox Live, antes de incluir o cabeçalho XSAPI defina XBOX_LIVE_CREATORS_SDK. Isso limita a área de superfície de API para somente aqueles que podem ser usados por desenvolvedores em que o programa de criadores do Xbox Live e altera o método de entrada para trabalhar para títulos no programa de criadores.  Por exemplo:

```c++
#define XBOX_LIVE_CREATORS_SDK
#include "xsapi\services.h"
```

- C + + c++ /CLI também há suporte para o WinRT.  Para obter mais informações sobre o C + + c++ /CLI WinRT pode ser encontrados em [https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/](https://moderncpp.com/2016/10/13/cppwinrt-available-on-github/)

Usar C + + c++ /CLI WinRT com a API do C++ XSAPI, antes de incluir o cabeçalho XSAPI definir XSAPI_CPPWINRT.  Por exemplo:

```c++
#define XSAPI_CPPWINRT
#include "xsapi\services.h"
```

Aqui está um exemplo de chamar a API do C++ XSAPI usando C + + c++ /CLI WinRT:

```c++
winrt::Windows::Xbox::System::User cppWinrtUser = winrt::Windows::Xbox::System::User::Users().GetAt(0);
std::shared_ptr<xbox::services::xbox_live_context> xboxLiveContext = std::make_shared<xbox::services::xbox_live_context>(cppWinrtUser);
```

### <a name="xsapi-c-based-api"></a>API baseada em XSAPI C

- Permite que os títulos controlar as alocações de memória ao chamar XSAPI.
- Permite que os títulos ganhar controle total do thread de tratamento ao chamar XSAPI.
- Usa uma nova biblioteca HTTP, libHttpClient, projetada para desenvolvedores de jogos.

Para obter mais informações, consulte [Introdução às APIs de C do Xbox Live](xsapi-flat-c.md).
