---
description: Este tópico aborda as estratégias para processar erros ao programar com C++/WinRT.
title: Processamento de erros com C++/WinRT
ms.date: 05/21/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, erro, processamento, exceção
ms.localizationpriority: medium
ms.openlocfilehash: c6f7135e85ab63ddfe92bd0de8c656b58fb1a020
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8343031"
---
# <a name="error-handling-with-cwinrt"></a>Processamento de erros com C++/WinRT

Este tópico aborda as estratégias para processar erros ao programar com [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt). Para obter informações gerais e o histórico, veja [Processamento de erros e exceções (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp).

## <a name="avoid-catching-and-throwing-exceptions"></a>Evitar a captura e a geração de exceções
Recomendamos que você continue a criar [código à prova de exceções](/cpp/cpp/how-to-design-for-exception-safety), mas evite a captura e a geração de exceções sempre que possível. Se não houver nenhum manipulador para uma exceção, o Windows gera automaticamente um relatório de erros (incluindo um minidespejo da pane), que ajudará você a detectar onde está o problema.

Não gere uma exceção que você pretende capturar. Não use exceções para falhas esperadas. Gere uma exceção *somente quando ocorre um erro de tempo de execução inesperado* e manipule todo o resto com códigos de erro/resultado&mdash;diretamente, e feche a origem da falha. Dessa forma, quando uma exceção *é* gerada, você sabe que a causa é um bug no código ou um estado de erro excepcional no sistema.

Considere o cenário de acesso ao Registro do Windows. Se o aplicativo falhar ao ler um valor do Registro, isso é esperado e você deve tratar a situação normalmente. Não gere uma exceção; em vez disso, retorne um valor `bool`ou `enum` que indica se e por que não foi possível ler o valor. Por outro lado, uma falha ao *gravar* um valor no Registro provavelmente indica um problema maior que você pode processar facilmente em seu aplicativo. Em casos assim, não é recomendado que o aplicativo continue, portanto, uma exceção que resulta em um relatório de erros é a maneira mais rápida de impedir que o aplicativo cause danos.

Em outro exemplo, considere recuperar uma imagem em miniatura de uma chamada para [**StorageFile.GetThumbnailAsync**](/uwp/api/windows.storage.storagefile.getthumbnailasync#Windows_Storage_StorageFile_GetThumbnailAsync_Windows_Storage_FileProperties_ThumbnailMode_) e passá-la para [**BitmapSource.SetSourceAsync**](/uwp/api/windows.ui.xaml.media.imaging.bitmapsource.setsourceasync#Windows_UI_Xaml_Media_Imaging_BitmapSource_SetSourceAsync_Windows_Storage_Streams_IRandomAccessStream_). Se essa sequência de chamadas resultar em passar `nullptr` para **SetSourceAsync** (o arquivo não imagem não pode ser lido; talvez a extensão do arquivo faça parece que contém dados de imagem mesmo não contendo), você provocará uma exceção de ponteiro inválido. Se você descobrir um caso semelhante no seu código, em vez de detectar e processar o caso como uma exceção, verifique se `nullptr` foi retornado de **GetThumbnailAsync **.

A geração de exceções tende a ser mais lenta do que usar códigos de erro. Caso gere apenas uma exceção quando um erro fatal ocorrer, se tudo correr bem, você nunca terá um problema de desempenho.

Mas um impacto mais provável no desempenho envolve a sobrecarga do tempo de execução ao garantir que os destruidores apropriados sejam chamados no evento improvável de geração da exceção. O custo dessa garantia é detectado quando uma exceção é gerada ou não. Assim, você deve garantir que o compilador tenha uma boa noção sobre quais funções podem potencialmente gerar exceções. Se o compilador pode provar que não haverá qualquer exceção de funções específicas (a especificação `noexcept`), é possível otimizar o código gerado.

## <a name="catching-exceptions"></a>Capturar exceções
Uma condição de erro que surge na camada [ABI do Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types) é retornada na forma de um valor HRESULT. Mas você não precisa processar HRESULTs em seu código. O código de projeção do C++/WinRT gerado a partir de uma API no lado de consumo detecta um código de erro de HRESULT na camada ABI e converte o código em uma exceção [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error), que você pode capturar e processar.

Por exemplo, se o usuário excluir uma imagem da biblioteca de imagens enquanto o aplicativo está iterando dessa coleção, a projeção gera uma exceção. Nesse caso, é necessário capturar e processar essa exceção. Veja um exemplo de código mostrando esse caso.

```cppwinrt
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
            HRESULT hr = ex.to_abi(); // HRESULT_FROM_WIN32(ERROR_FILE_NOT_FOUND).
            winrt::hstring message = ex.message(); // The system cannot find the file specified.
        }
    }
}
```

Se o mesmo padrão em uma corrotina ao chamar uma função `co_await`-ed. Outro exemplo dessa conversão de HRESULT para exceção é que, quando uma API de componente retorna E_OUTOFMEMORY, isso gera um **std::bad_alloc**.

## <a name="throwing-exceptions"></a>Geração de exceções
Há casos em que você deve decidir que, em caso de falha na chamada para uma função específica, o aplicativo não será capaz de se recuperar (não é possível confiar no funcionamento previsível). O exemplo de código a seguir usa um valor de [**winrt::handle**](/uwp/cpp-ref-for-winrt/handle) como um wrapper identificador retornado por [**CreateEvent**](https://msdn.microsoft.com/library/windows/desktop/ms682396). Em seguida, passa o identificador (criando um valor `bool` dele) para o modelo de função [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool). **winrt::check_bool** funciona com um `bool` ou com qualquer valor convertido para `false` (uma condição de erro) ou `true` (uma condição de sucesso).

```cppwinrt
winrt::handle h{ ::CreateEvent(nullptr, false, false, nullptr) };
winrt::check_bool(bool{ h });
winrt::check_bool(::SetEvent(h.get()));
```

Se o valor que você passa para [**winrt::check_bool**](/uwp/cpp-ref-for-winrt/error-handling/check-bool) é false, a seguinte sequência de ações ocorre.

- **winrt::check_bool** chama a função [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error).
- **WinRT:: throw_last_error** chama [**GetLastError**](https://msdn.microsoft.com/library/windows/desktop/ms679360) para recuperar o valor do último código de erro do thread de chamada e, em seguida, chama a função [**WinRT:: throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult) .
- **winrt::throw_hresult** gera uma exceção usando um objeto [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) (ou um objeto padrão) que representa esse código de erro.

Como as APIs do Windows relatam erros no tempo de execução usando vários tipos de valor de retorno, existem algumas outras funções auxiliares úteis além de **winrt::check_bool** para verificar os valores e gerar exceções.

- [**winrt::check_hresult**](/uwp/cpp-ref-for-winrt/error-handling/check-hresult). Verifica se o código HRESULT representa um erro e, em caso afirmativo, chama **winrt::throw_hresult**.
- [**winrt::check_nt**](/uwp/cpp-ref-for-winrt/error-handling/check-nt). Verifica se um código representa um erro e, em caso afirmativo, chama **winrt::throw_hresult**.
- [**winrt::check_pointer**](/uwp/cpp-ref-for-winrt/error-handling/check-pointer). Verifica se um ponteiro é nulo e, em caso afirmativo, chama **winrt::throw_last_error**.
- [**winrt::check_win32**](/uwp/cpp-ref-for-winrt/error-handling/check-win32). Verifica se um código representa um erro e, em caso afirmativo, chama **winrt::throw_hresult**.

Você pode usar essas funções auxiliares para tipos comuns de código de retorno ou você pode responder a qualquer condição de erro e chamar [**winrt::throw_last_error**](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error) ou [**winrt::throw_hresult**](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult). 

## <a name="throwing-exceptions-when-authoring-an-api"></a>Gerar exceções ao criar uma API
Como é inválido para uma exceção cruzar o limite do [ABI do Windows Runtime](interop-winrt-abi.md#what-is-the-windows-runtime-abi-and-what-are-abi-types), uma condição de erro que ocorre durante uma implementação é retornada na camada ABI na forma de um código de erro HRESULT. Ao criar uma API usando C++/WinRT, o código é gerado para você converter qualquer exceção *gerada* na implementação em um HRESULT. A função [**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) é usada no código gerado em um padrão semelhante a isso.

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

[**winrt::to_hresult**](/uwp/cpp-ref-for-winrt/error-handling/to-hresult) processa as exceções derivadas de **std::exception**, [**winrt::hresult_error**](/uwp/cpp-ref-for-winrt/error-handling/hresult-error) e seus tipos derivados. Na implementação, você deve preferir **winrt::hresult_error** ou um tipo derivado para que os consumidores da API recebam informações de erro avançadas. **std::exception** (mapeada para E_FAIL) é suportada caso surjam exceções devido ao uso da biblioteca de modelo padrão.

## <a name="assertions"></a>Afirmações
Para suposições internas em seu aplicativo existem declarações. Prefira **static_assert** para a validação de tempo de compilação sempre que possível. Para as condições de tempo de execução, use WINRT_ASSERT com uma expressão booliana.

```cppwinrt
WINRT_ASSERT(pos < size());
```

WINRT_ASSERT é compilado nas compilações lançadas; em uma compilação de depuração, ele interrompe o aplicativo no depurador na linha de código onde está a declaração.

Você não deve usar exceções em seu destruidores. Portanto, pelo menos nas compilações de depuração, você pode declarar o resultado da chamada de uma função a partir de um destrutor com WINRT_VERIFY (com uma expressão booliana) e WINRT_VERIFY_ (com um resultado esperado e uma expressão booliana).

```cppwinrt
WINRT_VERIFY(::CloseHandle(value));
WINRT_VERIFY_(TRUE, ::CloseHandle(value));
```

## <a name="important-apis"></a>APIs importantes
* [winrt::check_bool function template](/uwp/cpp-ref-for-winrt/error-handling/check-bool)
* [winrt::check_hresult function](/uwp/cpp-ref-for-winrt/error-handling/check-hresult)
* [winrt::check_nt function template](/uwp/cpp-ref-for-winrt/error-handling/check-nt)
* [winrt::check_pointer function template](/uwp/cpp-ref-for-winrt/error-handling/check-pointer)
* [winrt::check_win32 function template](/uwp/cpp-ref-for-winrt/error-handling/check-win32)
* [winrt::handle struct](/uwp/cpp-ref-for-winrt/handle)
* [winrt::hresult_error struct](/uwp/cpp-ref-for-winrt/error-handling/hresult-error)
* [winrt::throw_hresult function](/uwp/cpp-ref-for-winrt/error-handling/throw-hresult)
* [winrt::throw_last_error function](/uwp/cpp-ref-for-winrt/error-handling/throw-last-error)
* [winrt::to_hresult function](/uwp/cpp-ref-for-winrt/error-handling/to-hresult)

## <a name="related-topics"></a>Tópicos relacionados
* [Processamento de erros e exceções (C++ moderno)](/cpp/cpp/errors-and-exception-handling-modern-cpp)
* [Como: design de proteção para a exceção](/cpp/cpp/how-to-design-for-exception-safety)
