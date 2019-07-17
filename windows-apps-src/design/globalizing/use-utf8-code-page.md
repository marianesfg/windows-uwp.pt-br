---
Description: Use o caractere UTF-8 codificação ideal compatibilidade entre aplicativos web e outro * nix plataformas (Unix, Linux e variantes), minimizar os bugs de localização e reduzir a sobrecarga de teste.
title: Use a página de código UTF-8 do Windows
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: a9386b31d16796c68d41a27ab48a5b2c9a9a342b
ms.sourcegitcommit: 734aa941dc675157c07bdeba5059cb76a5626b39
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/15/2019
ms.locfileid: "68141812"
---
# <a name="use-the-utf-8-code-page"></a>Use a página de código UTF-8

Use [UTF-8](http://www.utf-8.com/) caractere de codificação para compatibilidade ideal entre aplicativos web e outra * nix plataformas (Unix, Linux e variantes), minimizar os bugs de localização e reduzir a sobrecarga de teste.

UTF-8 é a página de código universal para internacionalização e dá suporte a todos os pontos de código Unicode usando a codificação de largura variável de 1 a 4 bytes. Ele é usado cômoda na web e é o padrão para * nix plataformas.

## <a name="-a-vs--w-apis"></a>-A x -W APIs
  
As APIs do Win32 com frequência dão suporte a variantes - um e -W.

-A variantes reconhecem a página de código ANSI configurada no sistema e do suporte `char*`, enquanto as variantes -W operam em UTF-16 e suporte `WCHAR`.

Até recentemente, o Windows tem enfatizado variantes de "Unicode" -W em - um APIs. No entanto, versões recentes usou a página de código ANSI e - A APIs como um meio para apresentar a UTF-8 dão suporte a aplicativos. Se a página de código ANSI é configurada para UTF-8, - A APIs operam em UTF-8. Esse modelo tem a vantagem de código existente criado com - A APIs sem qualquer alteração de código de suporte.

## <a name="set-a-process-code-page-to-utf-8"></a>Definir uma página de código do processo para UTF-8

A partir da versão do Windows 1903 (atualização de 2019 maio), você pode usar a propriedade ActiveCodePage o appxmanifest para aplicativos empacotados ou o manifesto de fusão para aplicativos descompactados, para forçar um processo para usar o UTF-8 como a página de código do processo.

Você pode declarar esta propriedade e compilações de destino/execução no Windows anteriores, mas você deve lidar com detecção de página de código herdado e conversão como de costume. Com uma versão de destino mínima do Windows versão 1903, a página de código do processo sempre será UTF-8 para que a conversão e a detecção de página de código herdado podem ser evitados.

## <a name="examples"></a>Exemplos

**Manifesto AppX para um aplicativo empacotado:**

```xaml
<?xml version="1.0" encoding="utf-8"?>
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         ...
         xmlns:uap7="http://schemas.microsoft.com/appx/manifest/uap/windows10/7"
         xmlns:uap8="http://schemas.microsoft.com/appx/manifest/uap/windows10/8"
         ...
         IgnorableNamespaces="... uap7 uap8 ...">

  <Applications>
    <Application ...>
      <uap7:Properties>
        <uap8:ActiveCodePage>UTF-8</uap8:ActiveCodePage>
      </uap7:Properties>
    </Application>
  </Applications>
</Package>
```

**Manifesto de fusão para um aplicativo Win32 descompactado:**

``` xaml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<assembly manifestVersion="1.0" xmlns="urn:schemas-microsoft-com:asm.v1">
  <assemblyIdentity type="win32" name="..." version="6.0.0.0"/>
  <application>
    <windowsSettings>
      <activeCodePage xmlns="http://schemas.microsoft.com/SMI/2019/WindowsSettings">UTF-8</activeCodePage>
    </windowsSettings>
  </application>
</assembly>
```

> [!NOTE]
> Adicionar um manifesto de um executável existente da linha de comando com `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Conversão de página de código

Como o Windows funciona nativamente no UTF-16 (`WCHAR`), talvez seja necessário converter os dados UTF-8 em UTF-16 (ou vice-versa) para interoperar com APIs do Windows.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) e [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) permitem que você converter entre UTF-8 e UTF-16 (`WCHAR`) (e outras páginas de código). Isso é particularmente útil quando herdado Win32 API só pode compreender `WCHAR`. Essas funções permitem que você converter a entrada de UTF-8 para `WCHAR` para passar para um -W API e, em seguida, converter todos os resultados novamente se necessário.
Ao usar essas funções com `CodePage` definido como `CP_UTF8`, use `dwFlags` deles `0` ou `MB_ERR_INVALID_CHARS`; caso contrário, um `ERROR_INVALID_FLAGS` ocorre.

Observação: `CP_ACP` é igual a `CP_UTF8` somente se executando o Windows versão 1903 (atualização de 2019 maio) ou superior e a propriedade ActiveCodePage descrita acima é definido como UTF-8. Caso contrário, ele respeita a página de código do sistema herdado. É recomendável usar `CP_UTF8` explicitamente.

## <a name="related-topics"></a>Tópicos relacionados

- [Páginas de código](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Identificadores de páginas de código](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
