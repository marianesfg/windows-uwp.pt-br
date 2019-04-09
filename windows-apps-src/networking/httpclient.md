---
description: Use HttpClient e o restante da API do namespace Windows.Web.Http para enviar e receber informações usando os protocolos HTTP 2.0 e HTTP 1.1.
title: HttpClient
ms.assetid: EC9820D3-3A46-474F-8A01-AE1C27442750
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 304c023251a15995ce15f5b3d846c662797661cd
ms.sourcegitcommit: bad7ed6def79acbb4569de5a92c0717364e771d9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59244322"
---
# <a name="httpclient"></a>HttpClient

**APIs Importantes**

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

A classe [**Windows.Web.Http.HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) representa uma mensagem de solicitação HTTP enviada por [**Windows.Web.Http.HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639). A classe [**Windows.Web.Http.HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631) representa uma mensagem de resposta HTTP recebida de uma solicitação HTTP. As mensagens HTTP são definidas na [RFC 2616](https://go.microsoft.com/fwlink/p/?linkid=241642) pela IETF.

O namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) representa conteúdo HTTP como o corpo e os cabeçalhos da entidade HTTP, incluindo cookies. O conteúdo HTTP pode ser associado a uma solicitação HTTP ou a uma resposta HTTP. O namespace **Windows.Web.Http** fornece várias classes diferentes para representar o conteúdo HTTP.

-   [**HttpBufferContent**](https://msdn.microsoft.com/library/windows/apps/dn298625). Conteúdo como um buffer
-   [**HttpFormUrlEncodedContent**](https://msdn.microsoft.com/library/windows/apps/dn298685). Conteúdo como tuplas de nome e valor codificadas com o tipo MIME **application/x-www-form-urlencoded**
-   [**HttpMultipartContent**](https://msdn.microsoft.com/library/windows/apps/dn298708). Conteúdo na forma do **com várias partes /\***  tipo MIME.
-   [**HttpMultipartFormDataContent**](https://msdn.microsoft.com/library/windows/apps/dn279596). Conteúdo que é codificado como o tipo MIME **multipart/form-data**.
-   [**HttpStreamContent**](https://msdn.microsoft.com/library/windows/apps/dn279649). Conteúdo como um fluxo (o tipo interno é usado pelo método HTTP GET para receber dados e o método HTTP POST para carregar dados)
-   [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661). Conteúdo como uma cadeia de caracteres.
-   [**IHttpContent** ](https://msdn.microsoft.com/library/windows/apps/dn279684) -uma interface base para os desenvolvedores criarem seus próprios objetos de conteúdo

O trecho de código na seção "Enviar uma solicitação GET simples por HTTP" usa a classe [**HttpStringContent**](https://msdn.microsoft.com/library/windows/apps/dn279661) para representar a resposta HTTP de uma solicitação GET HTTP como uma cadeia de caracteres.

O namespace [**Windows.Web.Http.Headers**](https://msdn.microsoft.com/library/windows/apps/dn252713) dá suporte à criação de cabeçalhos e cookies HTTP, que são, então, associados como propriedades a objetos [**HttpRequestMessage**](https://msdn.microsoft.com/library/windows/apps/dn279617) e [**HttpResponseMessage**](https://msdn.microsoft.com/library/windows/apps/dn279631).

## <a name="send-a-simple-get-request-over-http"></a>Enviar uma solicitação GET simples por HTTP

Conforme mencionado anteriormente neste artigo, o namespace [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) permite que os aplicativos UWP enviem solicitações GET. O trecho de código a seguir demonstra como enviar uma solicitação GET para http://www.contoso.com usando o [ **HttpClient** ](https://msdn.microsoft.com/library/windows/apps/dn298639) classe e o [  **Windows.Web.Http.HttpResponseMessage** ](https://msdn.microsoft.com/library/windows/apps/dn279631) classe para ler a resposta da solicitação GET.

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

## <a name="post-binary-data-over-http"></a>Dados de POSTAGEM binários via HTTP

O [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis) código de exemplo abaixo ilustra o uso de dados de formulário e uma solicitação POST para enviar uma pequena quantidade de dados binários como um upload de arquivo para um servidor web. O código usa o [ **HttpBufferContent** ](/uwp/api/windows.web.http.httpbuffercontent) classe para representar os dados binários e o [ **HttpMultipartFormDataContent** ](/uwp/api/windows.web.http.httpmultipartformdatacontent) classe a representam os dados de formulário de várias partes.

> [!NOTE]
> Chamando **obter** (como visto no exemplo de código abaixo) não é adequada para um thread de interface do usuário. Para a técnica correta usar nesse caso, consulte [simultaneidade e operações assíncronas com C++/WinRT](/windows/uwp/cpp-and-winrt-apis/concurrency).

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

Para lançar o conteúdo de um arquivo binário real (em vez dos dados binários explícitos usados acima), você achará mais fácil de usar um [HttpStreamContent](/uwp/api/windows.web.http.httpstreamcontent) objeto. Construir um e, como o argumento para seu construtor, passe o valor retornado de uma chamada para [StorageFile.OpenReadAsync](/uwp/api/windows.storage.storagefile.openreadasync). Esse método retorna um fluxo para os dados dentro de seu arquivo binário.

Além disso, se você estiver carregando um arquivo grande (maior do que cerca de 10MB), em seguida, é recomendável que você use o tempo de execução do Windows [transferência em segundo plano](/uwp/api/windows.networking.backgroundtransfer) APIs.

## <a name="exceptions-in-windowswebhttp"></a>Exceções em Windows.Web.Http

Uma exceção é gerada quando uma cadeia de caracteres inválida do URI (Uniform Resource Identifier) é passada para o construtor do objeto [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998).

**.NET:**  as [ **Windows.Foundation.Uri** ](https://msdn.microsoft.com/library/windows/apps/br225998) tipo aparece como [ **System. URI** ](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) em C# e VB.

No C# e no Visual Basic, esse erro pode ser evitado usando a classe [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) no .NET 4.5 e um dos métodos [**System.Uri.TryCreate**](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.trycreate.aspx) para testar a cadeia de caracteres recebida de um usuário antes de o URI ser construído.

No C++, não há nenhum método para tentar analisar uma cadeia de caracteres para um URI. Se um aplicativo receber entrada do usuário para o [**Windows.Foundation.Uri**](https://msdn.microsoft.com/library/windows/apps/br225998), o construtor deverá estar em um bloco try/catch. Se uma exceção for lançada, o aplicativo poderá notificar o usuário e solicitar um novo nome de host.

O [**Windows.Web.Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) não tem uma função de praticidade. Portanto, o aplicativo que usa [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) e outras classes nesse namespace precisa usar o valor **HRESULT**.

Em aplicativos usando o .NET Framework 4.5 no C#, VB.NET, o [System. Exception](https://msdn.microsoft.com/library/system.exception.aspx) representa um erro durante a execução do aplicativo quando ocorre uma exceção. A propriedade [System.Exception.HResult](https://msdn.microsoft.com/library/system.exception.hresult.aspx) retorna o **HRESULT** atribuído à exceção específica. A propriedade [System.Exception.Message](https://msdn.microsoft.com/library/system.exception.message.aspx) retorna a mensagem que descreve a exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Nos aplicativos que usam C++ gerenciado, o [Platform::Exception](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) representa um erro durante a execução do aplicativo quando ocorre uma exceção. A propriedade [Platform::Exception::HResult](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) retorna o **HRESULT** atribuído à exceção específica. A propriedade [Platform::Exception::Message](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) retorna a cadeia de caracteres fornecida pelo sistema que está associada ao valor de **HRESULT**. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **eletrônico\_INVALIDARG**. Para algumas chamadas de método ilegais, o **HRESULT** retornado é **eletrônico\_ilegal\_método\_chamar**.

