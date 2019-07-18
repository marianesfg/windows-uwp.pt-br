---
description: Use HttpClient e o restante da API do namespace Windows.Web.Http para enviar e receber informações usando os protocolos HTTP 2.0 e HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 06/05/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 877901deeec4da7674c6c8431e5b11f5eae075ed
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714131"
---
# <a name="httpclient"></a>HttpClient

**APIs importantes**

-   [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient)
-   [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http)
-   [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage)

Use [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) e o restante da API do namespace [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) para enviar e receber informações usando os protocolos HTTP 2.0 e HTTP 1.1.

## <a name="overview-of-httpclient-and-the-windowswebhttp-namespace"></a>Visão geral de HttpClient e o namespace Windows.Web.Http

As classes do namespace [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) e os namespaces [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) e [**Windows.Web.Http.Filters**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Filters) relacionados fornecem uma interface de programação para aplicativos UWP (Plataforma Universal do Windows) que atuam como um cliente HTTP para executar solicitações GET básicas ou implementar a funcionalidade HTTP mais avançada listada abaixo.

-   Métodos para verbos comuns (**DELETE**, **GET**, **PUT** e **POST**). Cada uma dessas solicitações é enviada como uma operação assíncrona.

-   Suporte para configurações e padrões de autenticação comuns.

-   Acesso a detalhes do protocolo SSL no transporte.

-   Capacidade de incluir filtros personalizados em aplicativos avançados.

-   Capacidade de obter, definir e excluir cookies.

-   Informações de progresso da Solicitação HTTP disponíveis em métodos assíncronos.

A classe [**Windows.Web.Http.HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) representa uma mensagem de solicitação HTTP enviada por [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient). A classe [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) representa uma mensagem de resposta HTTP recebida de uma solicitação HTTP. As mensagens HTTP são definidas na [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) pela IETF.

O namespace [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) representa conteúdo HTTP como o corpo e os cabeçalhos da entidade HTTP, incluindo cookies. O conteúdo HTTP pode ser associado a uma solicitação HTTP ou a uma resposta HTTP. O namespace **Windows.Web.Http** fornece várias classes diferentes para representar o conteúdo HTTP.

-   [**HttpBufferContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpBufferContent). Conteúdo como um buffer
-   [**HttpFormUrlEncodedContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpFormUrlEncodedContent). Conteúdo como tuplas de nome e valor codificadas com o tipo MIME **application/x-www-form-urlencoded**
-   [**HttpMultipartContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartContent). Conteúdo na forma do tipo MIME **multipart/\*** .
-   [**HttpMultipartFormDataContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpMultipartFormDataContent). Conteúdo que é codificado como o tipo MIME **multipart/form-data**.
-   [**HttpStreamContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStreamContent). Conteúdo como um fluxo (o tipo interno é usado pelo método HTTP GET para receber dados e o método HTTP POST para carregar dados)
-   [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent). Conteúdo como uma cadeia de caracteres.
-   [**IHttpContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.IHttpContent) – Uma interface base para os desenvolvedores criarem seus próprios objetos de conteúdo

O trecho de código na seção "Enviar uma solicitação GET simples por HTTP" usa a classe [**HttpStringContent**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpStringContent) para representar a resposta HTTP de uma solicitação GET HTTP como uma cadeia de caracteres.

O namespace [**Windows.Web.Http.Headers**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.Headers) dá suporte à criação de cabeçalhos e cookies HTTP, que são, então, associados como propriedades a objetos [**HttpRequestMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpRequestMessage) e [**HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage).

## <a name="send-a-simple-get-request-over-http"></a>Enviar uma solicitação GET simples por HTTP

Conforme mencionado anteriormente neste artigo, o namespace [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) permite que os aplicativos UWP enviem solicitações GET. O snippet de código a seguir demonstra como enviar uma solicitação GET para http:\//www.contoso.com usando a classe [**Windows.Web.Http.HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) e a classe [**Windows.Web.Http.HttpResponseMessage**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpResponseMessage) para ler a resposta da solicitação GET.

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

## <a name="post-binary-data-over-http"></a>Dados binários POST via HTTP

O código de exemplo de [C++/WinRT](/windows/uwp/cpp-and-winrt-apis) abaixo ilustra o uso de dados de formulário e de uma solicitação POST para enviar uma pequena quantidade de dados binários como um upload de arquivo para um servidor Web. O código usa a classe [**HttpBufferContent**](/uwp/api/windows.web.http.httpbuffercontent) para representar os dados binários e a classe [**HttpMultipartFormDataContent**](/uwp/api/windows.web.http.httpmultipartformdatacontent) para representar os dados de formulário de várias partes.

> [!NOTE]
> Chamar **get** (como visto no exemplo de código abaixo) não é adequado para um thread de interface do usuário. Para a técnica correta a usar nesse caso, consulte [Simultaneidade e operações assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

```cppwinrt
// pch.h
#pragma once
#include <winrt/Windows.Foundation.h>
#include <winrt/Windows.Security.Cryptography.h>
#include <winrt/Windows.Storage.Streams.h>
#include <winrt/Windows.Web.Http.Headers.h>

// main.cpp : Defines the entry point for the console application.
#include "pch.h"
#include <iostream>
#include <sstream>
using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage::Streams;

int main()
{
    init_apartment();

    auto buffer{
        Windows::Security::Cryptography::CryptographicBuffer::ConvertStringToBinary(
            L"A sentence of text to encode into binary to serve as sample data.",
            Windows::Security::Cryptography::BinaryStringEncoding::Utf8
        )
    };
    Windows::Web::Http::HttpBufferContent binaryContent{ buffer };
    // You can use the 'image/jpeg' content type to represent any binary data;
    // it's not necessarily an image file.
    binaryContent.Headers().Append(L"Content-Type", L"image/jpeg");

    Windows::Web::Http::Headers::HttpContentDispositionHeaderValue disposition{ L"form-data" };
    binaryContent.Headers().ContentDisposition(disposition);
    // The 'name' directive contains the name of the form field representing the data.
    disposition.Name(L"fileForUpload");
    // Here, the 'filename' directive is used to indicate to the server a file name
    // to use to save the uploaded data.
    disposition.FileName(L"file.dat");

    Windows::Web::Http::HttpMultipartFormDataContent postContent;
    postContent.Add(binaryContent); // Add the binary data content as a part of the form data content.

    // Send the POST request asynchronously, and retrieve the response as a string.
    Windows::Web::Http::HttpResponseMessage httpResponseMessage;
    std::wstring httpResponseBody;

    try
    {
        // Send the POST request.
        Uri requestUri{ L"https://www.contoso.com/post" };
        Windows::Web::Http::HttpClient httpClient;
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

Para lançar o conteúdo de um arquivo binário real (em vez dos dados binários explícitos usados acima), você achará mais fácil usar um objeto [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent). Construa um e, como o argumento para seu construtor, passe o valor retornado de uma chamada para [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Esse método retorna um fluxo para os dados dentro de seu arquivo binário.

Além disso, se você está carregando um arquivo grande (maior do que cerca de 10 MB), é recomendável que você use APIs de [transferência em segundo plano](/uwp/api/windows.networking.backgroundtransfer) do Windows Runtime.

## <a name="post-json-data-over-http"></a>Dados JSON de POST por HTTP

O exemplo a seguir posta um JSON para um ponto de extremidade e, em seguida, grava o corpo da resposta.

```cs
using System;
using System.Diagnostics;
using System.Threading.Tasks;
using Windows.Storage.Streams;
using Windows.Web.Http;

private async Task TryPostJsonAsync()
{
    try
    {
        // Construct the HttpClient and Uri. This endpoint is for test purposes only.
        HttpClient httpClient = new HttpClient();
        Uri uri = new Uri("https://www.contoso.com/post");

        // Construct the JSON to post.
        HttpStringContent content = new HttpStringContent(
            "{ \"firstName\": \"Eliot\" }",
            UnicodeEncoding.Utf8,
            "application/json");

        // Post the JSON and wait for a response.
        HttpResponseMessage httpResponseMessage = await httpClient.PostAsync(
            uri,
            content);

        // Make sure the post succeeded, and write out the response.
        httpResponseMessage.EnsureSuccessStatusCode();
        var httpResponseBody = await httpResponseMessage.Content.ReadAsStringAsync();
        Debug.WriteLine(httpResponseBody);
    }
    catch (Exception ex)
    {
        // Write out any exceptions.
        Debug.WriteLine(ex);
    }
}
```

## <a name="exceptions-in-windowswebhttp"></a>Exceções em Windows.Web.Http

Uma exceção é gerada quando uma cadeia de caracteres inválida do URI (Uniform Resource Identifier) é passada para o construtor do objeto [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri).

**.NET:**   O tipo [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri) é exibido como [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) no C# e no VB.

No C# e no Visual Basic, esse erro pode ser evitado usando a classe [**System.Uri**](https://docs.microsoft.com/dotnet/api/system.uri?redirectedfrom=MSDN) no .NET 4.5 e um dos métodos [**System.Uri.TryCreate**](https://docs.microsoft.com/dotnet/api/system.uri.trycreate?redirectedfrom=MSDN#overloads) para testar a cadeia de caracteres recebida de um usuário antes de o URI ser construído.

No C++, não há nenhum método para tentar analisar uma cadeia de caracteres para um URI. Se um aplicativo receber entrada do usuário para o [**Windows.Foundation.Uri**](https://docs.microsoft.com/uwp/api/Windows.Foundation.Uri), o construtor deverá estar em um bloco try/catch. Se uma exceção for lançada, o aplicativo poderá notificar o usuário e solicitar um novo nome de host.

O [**Windows.Web.Http**](https://docs.microsoft.com/uwp/api/Windows.Web.Http) não tem uma função de praticidade. Portanto, o aplicativo que usa [**HttpClient**](https://docs.microsoft.com/uwp/api/Windows.Web.Http.HttpClient) e outras classes nesse namespace precisa usar o valor **HRESULT**.

Nos aplicativos que usam o .NET Framework 4.5 no C#, VB.NET, o [System.Exception](https://docs.microsoft.com/dotnet/api/system.exception?redirectedfrom=MSDN) representa um erro durante a execução do aplicativo quando ocorre uma exceção. A propriedade [System.Exception.HResult](https://docs.microsoft.com/dotnet/api/system.exception.hresult?redirectedfrom=MSDN#System_Exception_HResult) retorna o **HRESULT** atribuído à exceção específica. A propriedade [System.Exception.Message](https://docs.microsoft.com/dotnet/api/system.exception.message?redirectedfrom=MSDN#System_Exception_Message) retorna a mensagem que descreve a exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Nos aplicativos que usam C++ gerenciado, o [Platform::Exception](https://docs.microsoft.com/cpp/cppcx/platform-exception-class) representa um erro durante a execução do aplicativo quando ocorre uma exceção. A propriedade [Platform::Exception::HResult](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#hresult) retorna o **HRESULT** atribuído à exceção específica. A propriedade [Platform::Exception::Message](https://docs.microsoft.com/cpp/cppcx/platform-exception-class#message) retorna a cadeia de caracteres fornecida pelo sistema que está associada ao valor de **HRESULT**. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**. Para algumas chamadas de método ilícitas, o **HRESULT** retornado é **E\_ILLEGAL\_METHOD\_CALL**.

