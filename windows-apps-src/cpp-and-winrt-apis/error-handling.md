---
description: Este tópico aborda as estratégias para processar erros ao programar com C++/WinRT.
title: Processamento de erros com C++/WinRT
ms.date: 04/23/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, erro, processamento, exceção
ms.localizationpriority: medium
ms.openlocfilehash: 1b72bb3cb2527585c114d386981e02d4730614a2
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "66721644"
---
# <a name="error-handling-with-cwinrt"></a>Processamento de erros com C++/WinRT

Este tópico aborda as estratégias para processar erros ao programar com [C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Para obter informações gerais e o histórico, veja [Processamento de erros e exceções (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Evitar a captura e a geração de exceções
É recomendável continuar escrevendo [código à prova de exceções](/cpp/cpp/how-to-design-for-exception-safety), mas evite a captura e a geração de exceções sempre que possível. Se não houver nenhum manipulador para uma exceção, o Windows vai gerar automaticamente um relatório de erros (incluindo um minidespejo da falha), que ajudará você a detectar onde está o problema.

Não gere uma exceção que você pretende capturar. E não use exceções para falhas esperadas. Gere uma exceção *somente quando ocorrer um erro de tempo de execução inesperado* e manipule todo o restante com códigos de erro/resultado&mdash;diretamente, e feche a origem da falha. Dessa forma, quando uma exceção *for* gerada, você saberá que a causa é um bug no código ou um estado de erro excepcional no sistema.

Considere o cenário de acesso ao Registro do Windows. Se o aplicativo falhar ao ler um valor no Registro, isso era esperado, e você deve tratar a situação normalmente. Não gere uma exceção; em vez disso, retorne um valor `bool` ou `enum`, indicando que, e talvez por que, o valor não foi lido. Por outro lado, a falha ao *gravar* um valor no Registro, provavelmente indica que há um problema maior que você pode processar de maneira perceptível em seu aplicativo. Em casos assim, não é recomendado que o aplicativo continue. Portanto, uma exceção que resulta em um relatório de erros é a maneira mais rápida de impedir que o aplicativo cause danos.

Em outro exemplo, considere recuperar uma imagem em miniatura de uma chamada a [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) e passá-la para [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_). Se essa sequência de chamadas resultar em passar `nullptr` para **SetSourceAsync** (o arquivo de imagem não pode ser lido; talvez sua extensão faça parecer que ele contém dados de imagem, mesmo não contendo), você provocará a geração de uma exceção de ponteiro inválido. Se você descobrir um caso semelhante no seu código, em vez de capturar e processar o caso como uma exceção, verifique se `nullptr` foi retornado de **GetThumbnailAsync**.

A geração de exceções tende a ser mais lenta do que usar códigos de erro. Caso uma exceção seja gerada somente quando um erro fatal ocorrer, e se tudo correr bem, você nunca terá um problema de desempenho.

Mas um impacto mais provável no desempenho envolve a sobrecarga do tempo de execução ao garantir que os destruidores apropriados sejam chamados no evento improvável de geração da exceção. O custo dessa garantia é percebido não importando se uma exceção é de fato gerada ou não. Assim, você deve garantir que o compilador tenha uma boa noção sobre quais funções podem potencialmente gerar exceções. Se o compilador puder provar que não haverá qualquer exceção nas funções específicas (a especificação `noexcept`), é possível otimizar o código gerado.

## <a name="catching-exceptions"></a>Capturando exceções
Uma condição de erro que surge na camada [ABI do Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) é retornada na forma de um valor HRESULT. Mas você não precisa processar HRESULTs em seu código. O código de projeção do C++/WinRT gerado para uma API no lado de consumo detecta um código de erro de HRESULT na camada ABI e converte o código em uma exceção [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error), que você pode capturar e processar. Se você *realmente* desejar processar HRESULTS, use o tipo **winrt::hresult**.

Por exemplo, se o usuário excluir uma imagem da biblioteca de imagens enquanto o aplicativo estiver iterando nessa coleção, a projeção vai gerar uma exceção. Nesse caso, é necessário capturar e processar essa exceção. Veja a seguir um exemplo de código mostrando esse caso.

```cppwinrt
#include <winrt/Windows.Foundation.Collections.h>
#include <winrt/Windows.Storage.h>
#include <winrt/Windows.UI.Xaml.Media.Imaging.h>

using namespace winrt;
using namespace Windows::Foundation;
using namespace Windows::Storage;
using namespace Windows::UI::Xaml::Media::Imaging;

IAsyncAction MakeThumbnailsAsync()
{
    auto imageFiles{ co_await KnownFolders::PicturesLibrary().GetFilesAsync() };

    for (StorageFile const& imageFile : imageFiles)
    {
        BitmapImage bitmapImage;
        try
        {
            auto thumbnail{ co_await imageFile.GetThumbnailAsync(FileProperties::ThumbnailMode::PicturesView) };
            if (thumbnail) bitmapImage.SetSource(thumbnail);
        }
        catch (winrt::hresult_error const& ex)
        {
            winrt::hresult hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Use esse mesmo padrão em uma corrotina ao chamar uma função `co_await`-ed. Outro exemplo dessa conversão de HRESULT em exceção é que, quando uma API de componente retorna E_OUTOFMEMORY, isso gera um **std::bad_alloc**.

## <a name="throwing-exceptions"></a>Acionamento de exceções
Há casos em que você deve decidir que, em caso de falha na chamada para uma função específica, o aplicativo não será capaz de se recuperar (não é possível confiar no funcionamento previsível). O exemplo de código abaixo usa um valor de [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) como um wrapper em torno do IDENTIFICADOR retornado de [**CreateEvent**](https://docs.microsoft.com/windows/desktop/api/synchapi/nf-synchapi-createeventa). Em seguida, passa o identificador (criando um valor `bool` a partir dele) para o modelo de função [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool). **winrt::check_bool** funciona com um `bool` ou com qualquer valor que possa ser convertido em `false` (uma condição de erro) ou `true` (uma condição de sucesso).

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Se o valor que você passar para [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) for false, ocorrerá a sequência de ações a seguir.

- **winrt::check_bool** chama a função [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error).
- **winrt::throw_last_error** chama [**GetLastError**](https://docs.microsoft.com/windows/desktop/api/errhandlingapi/nf-errhandlingapi-getlasterror) para recuperar o valor do último código de erro do thread e, em seguida, chama a função [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult).
- **winrt::throw_hresult** gera uma exceção usando um objeto [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) (ou um objeto padrão) que representa esse código de erro.

Como as APIs do Windows relatam erros em tempo de execução usando vários tipos de valor de retorno, existem algumas outras funções auxiliares úteis além de **winrt::check_bool** para verificar os valores e gerar exceções.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Verifica se o código HRESULT representa um erro e, em caso afirmativo, chama **winrt::throw_hresult**.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Verifica se um código representa um erro e, em caso afirmativo, chama **winrt::throw_hresult**.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Verifica se um ponteiro é nulo e, em caso afirmativo, chama **winrt::throw_last_error**.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Verifica se um código representa um erro e, em caso afirmativo, chama **winrt::throw_hresult**.

Você pode usar essas funções auxiliares para tipos comuns de código de retorno ou pode responder a qualquer condição de erro e chamar [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) ou [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult). 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Gerando exceções ao criar uma API
Como é inválido para uma exceção cruzar o limite do [ABI do Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types), uma condição de erro que ocorre durante uma implementação é retornada na camada ABI na forma de um código de erro HRESULT. Ao criar uma API usando C++/WinRT, o código é gerado para você converter qualquer exceção *gerada* na implementação em um HRESULT. A função [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) é usada no código gerado em um padrão semelhante a este.

```cppwinrt
HRESULT DoWork() noexcept
{
    try
    {
        // Shim through to your C++/WinRT implementation.
        return S_OK;
    }
    catch (...)
    {
        return winrt::to_hresult(); // Convert any exception to an HRESULT.
    }
}
```

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) processa as exceções derivadas de **std::exception**, [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) e seus tipos derivados. Na implementação, você deve preferir **winrt::hresult_error**, ou um tipo derivado, para que os consumidores da API recebam informações de erro detalhadas. **std::exception** (mapeada para E_FAIL) será aceita caso surjam exceções devido ao uso da Biblioteca de Modelos Padrão.

## <a name="assertions"></a>Asserções
Para suposições internas em seu aplicativo, existem asserções. Prefira **static_assert** para a validação de tempo de compilação sempre que possível. Para as condições de tempo de execução, use WINRT_ASSERT com uma expressão booliana.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT é compilado nas compilações lançadas; em uma compilação de depuração, ele interrompe o aplicativo no depurador na linha de código onde está a asserção.

Você não deve usar exceções em seu destruidores. Portanto, pelo menos nas compilações de depuração, você pode declarar o resultado da chamada de uma função a partir de um destrutor com WINRT_VERIFY (com uma expressão booliana) e WINRT_VERIFY_ (com um resultado esperado e uma expressão booliana).

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>APIs Importantes
* [Modelo de função winrt::check_bool](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [Função winrt::check_hresult](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [Modelo de função winrt::check_nt](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [Modelo de função winrt::check_pointer](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [Modelo de função winrt::check_win32](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [Struct winrt::handle](/uwp/cpp-ref-for-winrt/handle)
* [Struct winrt::hresult_error](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [Função winrt::throw_hresult](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [Função winrt::throw_last_error](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [Função winrt::to_hresult](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Tópicos relacionados
* [Processamento de erros e exceções (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Como projetar para segurança de exceções](/cpp/cpp/how-to-design-for-exception-safety)
