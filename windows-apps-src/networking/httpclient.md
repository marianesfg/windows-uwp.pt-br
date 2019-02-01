---
description: Use HttpClient e o restante da API do namespace Windows.Web.Http para enviar e receber informações usando os protocolos HTTP 2.0 e HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b753b9e93a0cd2abae089f9d53915e0c109f6b63
ms.sourcegitcommit: 2d2483819957619b6de21b678caf887f3b1342af
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/01/2019
ms.locfileid: "9042418"
---
# <a name="httpclient"></a>HttpClient

**APIs importantes**

-   [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
-   [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
-   [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631)

Use [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) e o restante da API do namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) para enviar e receber informações usando os protocolos HTTP 2.0 e HTTP 1.1.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Visão geral de HttpClient e o namespace Windows.Web.Http

As classes do namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) e os namespaces [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) e [**Windows.Web.Http.Filters**](https://msdn.microsoft.com/library/windows/apps/dn298623) relacionados fornecem uma interface de programação para aplicativos UWP (Plataforma Universal do Windows) que atuam como um cliente HTTP para executar solicitações GET básicas ou implementar a funcionalidade HTTP mais avançada listada abaixo.

-   Métodos para verbos comuns (**DELETE**, **GET**, **PUT** e **POST**). Cada uma dessas solicitações é enviada como uma operação assíncrona.

-   Suporte para configurações e padrões de autenticação comuns.

-   Acesso a detalhes do protocolo SSL no transporte.

-   Capacidade de incluir filtros personalizados em aplicativos avançados.

-   Capacidade de obter, definir e excluir cookies.

-   Informações de progresso da Solicitação HTTP disponíveis em métodos assíncronos.

A classe [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) representa uma mensagem de solicitação HTTP enviada por [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639). A classe [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) representa uma mensagem de resposta HTTP recebida de uma solicitação HTTP. As mensagens HTTP são definidas na [RFC 2616](http://go.microsoft.com/fwlink/p/?linkid=241642) pela IETF.

O namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) representa conteúdo HTTP como o corpo e os cabeçalhos da entidade HTTP, incluindo cookies. O conteúdo HTTP pode ser associado a uma solicitação HTTP ou a uma resposta HTTP. O namespace **Windows.Web.Http** fornece várias classes diferentes para representar o conteúdo HTTP.

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). Conteúdo como um buffer
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). Conteúdo como tuplas de nome e valor codificadas com o tipo MIME **application/x-www-form-urlencoded**
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). Conteúdo na forma do tipo MIME **multipart/\***.
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). Conteúdo que é codificado como o tipo MIME **multipart/form-data**.
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). Conteúdo como um fluxo (o tipo interno é usado pelo método HTTP GET para receber dados e o método HTTP POST para carregar dados)
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). Conteúdo como uma cadeia de caracteres.
-   [**IHttpContent**](https://msdn.microsoft.com/library/windows/apps/dn279684) – Uma interface base para os desenvolvedores criarem seus próprios objetos de conteúdo

O trecho de código na seção "Enviar uma solicitação GET simples por HTTP" usa a classe [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) para representar a resposta HTTP de uma solicitação GET HTTP como uma cadeia de caracteres.

O namespace [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) dá suporte à criação de cabeçalhos e cookies HTTP, que são, então, associados como propriedades a objetos [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) e [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631).

## <a name="send-a-simple-get-request-over-http"></a>Enviar uma solicitação GET simples por HTTP

Conforme mencionado anteriormente neste artigo, o namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) permite que os aplicativos UWP enviem solicitações GET. O trecho de código a seguir demonstra como enviar uma solicitação GET para http://www.contoso.com usando a classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) e a classe [**httpresponsemessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) para ler a resposta da solicitação GET.

```csharp
//Create an HTTP client object
Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();

//Add a user-agent header to the GET request. 
var headers = httpClient.DefaultRequestHeaders;

//The safe way to add a header value is to use the TryParseAdd method and verify the return value is true,
//especially if the header value is coming from user input.
string header = "ie";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

header = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
if (!headers.UserAgent.TryParseAdd(header))
{
    throw new Exception("Invalid header value: " + header);
}

Uri requestUri = new Uri("http://www.contoso.com");

//Send the GET request asynchronously and retrieve the response as a string.
Windows.Web.Http.HttpResponseMessage httpResponse = new Windows.Web.Http.HttpResponseMessage();
string httpResponseBody = "";

try
{
    //Send the GET request
    httpResponse = await httpClient.GetAsync(requestUri);
    httpResponse.EnsureSuccessStatusCode();
    httpResponseBody = await httpResponse.Content.ReadAsStringAsync();
}
catch (Exception ex)
{
    httpResponseBody = "Error: " + ex.HResult.ToString("X") + " Message: " + ex.Message;
}
```

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
using namespace winrt;
using namespace Windows::Foundation;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    // Add a user-agent header to the GET request.
    auto headers{ httpClient.DefaultRequestHeaders() };

    // The safe way to add a header value is to use the TryParseAdd method, and verify the return value is true.
    // This is especially important if the header value is coming from user input.
    std::wstring header{ L"ie" };
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    header = L"Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";
    if (!headers.UserAgent().TryParseAdd(header))
    {
        throw L"Invalid header value: " + header;
    }

    Uri requestUri{ L"http://www.contoso.com" };

    // Send the GET request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the GET request.
        httpResponseMessage = httpClient.GetAsync(requestUri).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

## <a name="post-binary-data-over-http"></a>Dados binários POST por HTTP

O [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis) código de exemplo abaixo ilustra enviando uma pequena quantidade de dados binários com uma solicitação POST, usando a classe [HttpBufferContent](/uwp/api/windows.web.http.httpbuffercontent) . Chamar **obter** (conforme visto no exemplo de código abaixo) não é adequado para um thread de interface do usuário. Para a técnica correta para usar nesse caso, consulte [simultaneidade e operações assíncronas com C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
#include <winrt/Windows.Security.Cryptography.h>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    // Create an HttpClient object.
    Windows::Web::Http::HttpClient httpClient;

    Uri requestUri{ L"http://www.contoso.com/post" };

    auto buffer{
    Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
        L"A sentence of text by way of sample data",
        Windows::Security::Cryptography::BinaryStringEncoding::Utf8)
    };
    Windows::Web::Http::HttpBufferContent postContent{ buffer };
    postContent.Headers().Append(L"Content-Type", L"image/jpeg");

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        httpResponseMessage = httpClient.PostAsync(requestUri, postContent).get();
        httpResponseMessage.EnsureSuccessStatusCode();
        httpResponseBody = httpResponseMessage.Content().ReadAsStringAsync().get();
    }
    catch (winrt::hresult_error const& ex)
    {
        httpResponseBody = ex.message();
    }
    std::wcout << httpResponseBody;
}
```

Para postar o conteúdo de um arquivo binário, você encontrará mais fácil de usar um objeto [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) . Construir um e, como o argumento para o construtor, passe o valor retornado de uma chamada para [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Esse método retorna um fluxo de dados dentro de seu arquivo binário.

Além disso, se você estiver carregando um arquivos grandes (mais de cerca de 10MB), em seguida, é recomendável que você use as APIs de [Transferência em segundo plano](/uwp/api/windows.networking.backgroundtransfer) de execução do Windows.

## <a name="exceptions-in-windowswebhttp"></a>Exceções em Windows.Web.Http

Uma exceção é gerada quando uma cadeia de caracteres inválida do URI (Uniform Resource Identifier) é passada para o construtor do objeto [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**.NET:** o tipo de [**Windows. Foundation**](https://msdn.microsoft.com/library/windows/apps/br225998) aparece como [**System. URI**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) em c# e VB.

No C# e no Visual Basic, esse erro pode ser evitado usando a classe [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) no .NET 4.5 e um dos métodos [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) para testar a cadeia de caracteres recebida de um usuário antes de o URI ser construído.

No C++, não há nenhum método para tentar analisar uma cadeia de caracteres para um URI. Se um aplicativo receber entrada do usuário para o [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), o construtor deverá estar em um bloco try/catch. Se uma exceção for lançada, o aplicativo poderá notificar o usuário e solicitar um novo nome de host.

O [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) não tem uma função de praticidade. Portanto, o aplicativo que usa [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) e outras classes nesse namespace precisa usar o valor **HRESULT**.

Em aplicativos usar o Framework4.5 .NET em c#, VB.NET, o [System. Exception](http://msdn.microsoft.com/library/system.exception.aspx) representa um erro durante a execução do aplicativo quando ocorre uma exceção. A propriedade [System.Exception.HResult](http://msdn.microsoft.com/library/system.exception.hresult.aspx) retorna o **HRESULT** atribuído à exceção específica. A propriedade [System.Exception.Message](http://msdn.microsoft.com/library/system.exception.message.aspx) retorna a mensagem que descreve a exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror.h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Nos aplicativos que usam C++ gerenciado, o [Platform::Exception](http://msdn.microsoft.com/library/windows/apps/hh755825.aspx) representa um erro durante a execução do aplicativo quando ocorre uma exceção. A propriedade [Platform::Exception::HResult](http://msdn.microsoft.com/library/windows/apps/hh763371.aspx) retorna o **HRESULT** atribuído à exceção específica. A propriedade [Platform::Exception::Message](http://msdn.microsoft.com/library/windows/apps/hh763375.aspx) retorna a cadeia de caracteres fornecida pelo sistema que está associada ao valor de **HRESULT**. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror.h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**. Para algumas chamadas de método ilícitas, o **HRESULT** retornado é **E\_ILLEGAL\_METHOD\_CALL**.

