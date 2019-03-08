---
title: Trabalhando com buffers de armazenamento conectada
description: Saiba mais sobre como trabalhar com buffers de armazenamento conectados.
ms.assetid: 1d9d1b52-4bfe-4cd9-8b80-50ca6c0e9ae1
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox, armazenamento conectado
ms.localizationpriority: medium
ms.openlocfilehash: 3df95e4807e8d3457143e67eebfb62011bf365cc
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57595021"
---
# <a name="working-with-connected-storage-buffers"></a>Trabalhando com buffers de armazenamento conectada

Usa a API de armazenamento conectados **Windows::Storage::Streams::Buffer** instâncias para passar dados para e de um aplicativo. Porque os tipos de WinRT não podem expor ponteiros brutos, o acesso aos dados de uma instância de Buffer ocorre por meio **DataReader** e **DataWriter** classes. No entanto, **Buffer** também implementa a interface COM **ibufferbyteaccess da**, que torna possível obter um ponteiro diretamente para os dados de buffer.

### <a name="to-get-a-pointer-to-a-buffer-instances-data"></a>Para obter um ponteiro para dados de uma instância Buffer

1.  Use **reinterpretada\_cast** para converter a instância de Buffer como **IUnknown**.

```cpp
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
```

2.  Consulta de **IUnknown** interface para o **ibufferbyteaccess da** interface COM.

```cpp
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
```

3.  Use **IBufferByteAccess::Buffer** para obter um ponteiro diretamente aos dados do buffer.

```cpp
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
```

Por exemplo, o exemplo de código a seguir mostra como criar um buffer que contém a hora atual do sistema. Como buffers tem um valor de capacidade e o comprimento separado é necessário definir explicitamente a capacidade e o comprimento. Por padrão, o tamanho é 0.

```cpp
    inline byte* GetBufferData(Windows::Storage::Streams::IBuffer^ buffer)
    {
        using namespace Windows::Storage::Streams;
        IUnknown* unknown = reinterpret_cast<IUnknown*>(buffer);
        Microsoft::WRL::ComPtr<IBufferByteAccess> bufferByteAccess;
        HRESULT hr = unknown->QueryInterface(_uuidof(IBufferByteAccess), &bufferByteAccess);
        if (FAILED(hr))
        return nullptr;
        byte* bytes = nullptr;
        bufferByteAccess->Buffer(&bytes);
        return bytes;
    }

    IBuffer^ WrapRawBuffer( void* ptr, size_t size )
    {
        using namespace Windows::Storage::Streams;

        //uint32 size = sizeof(FILETIME);
        Buffer^ buffer = ref new Buffer(size);
        buffer->Length = size;

        memcpy(GetBufferData(buffer),ptr,size);


        return buffer;

    };
```
