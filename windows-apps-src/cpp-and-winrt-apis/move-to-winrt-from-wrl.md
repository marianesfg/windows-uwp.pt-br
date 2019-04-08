---
description: Este tópico mostra como fazer a portabilidade do código WRL para seu equivalente no C++/WinRT.
title: Mudar de WRL para C++/WinRT
ms.date: 05/30/2018
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, porta, migrar, WRL
ms.localizationpriority: medium
ms.openlocfilehash: e81f82fe823ee0fdf81741c89576adf268940d91
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57630741"
---
# <a name="move-to-cwinrt-from-wrl"></a>Mudar de WRL para C++/WinRT
Este tópico mostra como portar [biblioteca de modelos em C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) código para seu equivalente em [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt).

A primeira etapa na portabilidade para C + + c++ /CLI WinRT é adicionar manualmente C + + c++ /CLI WinRT suporte ao seu projeto (consulte [suporte do Visual Studio para C + + c++ /CLI WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package)). Para fazer isso, instale o [pacote do Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) em seu projeto. Abra o projeto no Visual Studio, clique em **Project** \> **gerenciar pacotes NuGet...** \> **Navegue**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **instalar** para instalar o pacote para o projeto. Um efeito dessa alteração é que o suporte para [C + + c++ /CX](/cpp/cppcx/visual-c-language-reference-c-cx) está desativado no projeto. Se estiver usando o C++/CX no projeto, você pode deixar o suporte desativado e atualizar o código do C++/CX para C++/WinRT (consulte [Mudar do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)). Ou você pode ativar o suporte novamente (nas propriedades do projeto **C/C++** \> **geral** \> **consumir extensão de tempo de execução do Windows** \> **Sim (/ZW)**) e o foco primeiro como portar seu código WRL. C + + c++ /CLI CX e C + + c++ /CLI código WinRT pode coexistir no mesmo projeto, com exceção do suporte a compilador de XAML e componentes de tempo de execução do Windows (consulte [mover para C + + c++ /CLI WinRT da C + + c++ /CLI CX](move-to-winrt-from-cx.md)).

Defina a propriedade do projeto **gerais** \> **versão da plataforma de destino** para 10.0.17134.0 (Windows 10, versão 1803) ou maior.

No arquivo de cabeçalho pré-compilado (em geral, `pch.h`), inclua `winrt/base.h`.

```cppwinrt
#include <winrt/base.h>
```

Se você incluir qualquer cabeçalho de API C++/WinRT projetado do Windows (por exemplo, `winrt/Windows.Foundation.h`), não é necessário incluir explicitamente `winrt/base.h` dessa forma, pois será incluído automaticamente para você.

## <a name="porting-wrl-com-smart-pointers-microsoftwrlcomptrcppwindowscomptr-class"></a>Fazer a portabilidade de ponteiros inteligentes COM do WRL ([Microsoft::WRL::ComPtr](/cpp/windows/comptr-class))
Qualquer código que usa a porta **Microsoft::WRL::ComPtr\<T\>**  usar [ **winrt::com_ptr\<T\>**](/uwp/cpp-ref-for-winrt/com-ptr). Veja um exemplo anterior e posterior do código. Na versão *posterior*, a função de membro [**com_ptr::put**](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) recupera o ponteiro bruto subjacente para poder defini-lo.

```cpp
ComPtr<IDXGIAdapter1> previousDefaultAdapter;
DX::ThrowIfFailed(m_dxgiFactory->EnumAdapters1(0, &previousDefaultAdapter));
```

```cppwinrt
winrt::com_ptr<IDXGIAdapter1> previousDefaultAdapter;
winrt::check_hresult(m_dxgiFactory->EnumAdapters1(0, previousDefaultAdapter.put()));
```

> [!IMPORTANT]
> Se você tiver um [ **winrt::com_ptr** ](/uwp/cpp-ref-for-winrt/com-ptr) que já está encaixado (seu ponteiro bruto interno já tem um destino) e você deseja encaixá-la para apontar para um objeto diferente de novamente, em seguida, você primeiro precisará atribuir `nullptr` a ele&mdash;conforme mostrado no exemplo de código abaixo. Se você não fizer isso, em seguida, um já fixada **com_ptr** desenhará o problema para a sua atenção (quando você chama [ **com_ptr::put** ](/uwp/cpp-ref-for-winrt/com-ptr#comptrput-function) ou [ **com_ptr:: put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function)), confirmando que seu ponteiro interno não é nulo.

```cppwinrt
winrt::com_ptr<IDXGISwapChain1> m_pDXGISwapChain1;
...
// We execute the code below each time the window size changes.
m_pDXGISwapChain1 = nullptr; // Important because we're about to re-seat 
winrt::check_hresult(
    m_pDxgiFactory->CreateSwapChainForHwnd(
        m_pCommandQueue.get(), // For Direct3D 12, this is a pointer to a direct command queue, and not to the device.
        m_hWnd,
        &swapChainDesc,
        nullptr,
        nullptr,
        m_pDXGISwapChain1.put())
);
```

No exemplo a seguir, (na versão *posterior*), a função de membro [**com_ptr::put_void**](/uwp/cpp-ref-for-winrt/com-ptr#comptrputvoid-function) recupera o ponteiro bruto subjacente como um ponteiro para anulação.

```cpp
ComPtr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(IID_PPV_ARGS(&debugController))))
{
    debugController->EnableDebugLayer();
}
```

```cppwinrt
winrt::com_ptr<ID3D12Debug> debugController;
if (SUCCEEDED(D3D12GetDebugInterface(__uuidof(debugController), debugController.put_void())))
{
    debugController->EnableDebugLayer();
}
```

Substitua **ComPtr::Get** por [**com_ptr::get**](/uwp/cpp-ref-for-winrt/com-ptr#comptrget-function).

```cpp
m_d3dDevice->CreateDepthStencilView(m_depthStencil.Get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

```cppwinrt
m_d3dDevice->CreateDepthStencilView(m_depthStencil.get(), &dsvDesc, m_dsvHeap->GetCPUDescriptorHandleForHeapStart());
```

Quando você quer passar o ponteiro bruto subjacente para uma função que espera um ponteiro para **IUnknown**, use o [ **winrt::get_unknown** ](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown#getunknown-function) livre de função, como mostra o próximo passo exemplo.

```cpp
ComPtr<IDXGISwapChain1> swapChain;
DX::ThrowIfFailed(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.Get(),
        reinterpret_cast<IUnknown*>(m_window.Get()),
        &swapChainDesc,
        nullptr,
        &swapChain
    )
);
```

```cppwinrt
winrt::agile_ref<winrt::Windows::UI::Core::CoreWindow> m_window; 
winrt::com_ptr<IDXGISwapChain1> swapChain;
winrt::check_hresult(
    m_dxgiFactory->CreateSwapChainForCoreWindow(
        m_commandQueue.get(),
        winrt::get_unknown(m_window.get()),
        &swapChainDesc,
        nullptr,
        swapChain.put()
    )
);
```

## <a name="porting-a-wrl-module-microsoftwrlmodule"></a>Portabilidade de um módulo WRL (Microsoft::WRL::Module)
Você pode adicionar gradualmente o código do C++/WinRT a um projeto existente que usa o WRL para implementar um componente e as classes WRL existente continuam a ser suportadas. Esta seção mostra como.

Se você criar um novo tipo de projeto do **Componente do Windows Runtime (C++/WinRT)** no Visual Studio e compilar, o arquivo `Generated Files\module.g.cpp` é gerado para você. Esse arquivo contém as definições de duas funções úteis de C++/WinRT (listadas abaixo), que você pode copiar e adicionar ao projeto. Essas funções são **WINRT_CanUnloadNow** e **WINRT_GetActivationFactory**. Como você pode ver, elas chama WRL condicionalmente para oferecer suporte à etapa de portabilidade atual.

```cppwinrt
HRESULT WINRT_CALL WINRT_CanUnloadNow()
{
#ifdef _WRL_MODULE_H_
    if (!::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().Terminate())
    {
        return S_FALSE;
    }
#endif

    if (winrt::get_module_lock())
    {
        return S_FALSE;
    }

    winrt::clear_factory_cache();
    return S_OK;
}

HRESULT WINRT_CALL WINRT_GetActivationFactory(HSTRING classId, void** factory)
{
    try
    {
        *factory = nullptr;
        wchar_t const* const name = WINRT_WindowsGetStringRawBuffer(classId, nullptr);

        if (0 == wcscmp(name, L"MoveFromWRLTest.Class"))
        {
            *factory = winrt::detach_abi(winrt::make<winrt::MoveFromWRLTest::factory_implementation::Class>());
            return S_OK;
        }

#ifdef _WRL_MODULE_H_
        return ::Microsoft::WRL::Module<::Microsoft::WRL::InProc>::GetModule().GetActivationFactory(classId, reinterpret_cast<::IActivationFactory**>(factory));
#else
        return winrt::hresult_class_not_available().to_abi();
#endif
    }
    catch (...) { return winrt::to_hresult(); }
}
```

Quando essas funções estiverem no projeto, em vez de chamar [**Module::GetActivationFactory**](/cpp/windows/module-getactivationfactory-method) diretamente, chame **WINRT_GetActivationFactory** (que chama a função WRL internamente). Veja um exemplo anterior e posterior do código.

```cpp
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    auto & module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    return module.GetActivationFactory(activatableClassId, factory);
}
```

```cppwinrt
HRESULT __stdcall WINRT_GetActivationFactory(HSTRING activatableClassId, void** factory);
HRESULT WINAPI DllGetActivationFactory(_In_ HSTRING activatableClassId, _Out_ ::IActivationFactory **factory)
{
    return WINRT_GetActivationFactory(activatableClassId, reinterpret_cast<void**>(factory));
}
```

Em vez de chamar [**Module::Terminate**](/cpp/windows/module-terminate-method) diretamente, chame **WINRT_CanUnloadNow** (que chama a função WRL internamente). Veja um exemplo anterior e posterior do código.

```cpp
HRESULT __stdcall DllCanUnloadNow(void)
{
    auto &module = Microsoft::WRL::Module<Microsoft::WRL::InProc>::GetModule();
    HRESULT hr = (module.Terminate() ? S_OK : S_FALSE);
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

```cppwinrt
HRESULT __stdcall WINRT_CanUnloadNow();
HRESULT __stdcall DllCanUnloadNow(void)
{
    HRESULT hr = WINRT_CanUnloadNow();
    if (hr == S_OK)
    {
        hr = ...
    }
    return hr;
}
```

## <a name="important-apis"></a>APIs Importantes
* [WinRT::com_ptr struct modelo](/uwp/cpp-ref-for-winrt/com-ptr)
* [struct WinRT::Windows::Foundation::IUnknown](/uwp/cpp-ref-for-winrt/windows-foundation-iunknown)

## <a name="related-topics"></a>Tópicos relacionados
* [Introdução ao C++/WinRT](intro-to-using-cpp-with-winrt.md)
* [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md)
* [WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
