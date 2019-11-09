---
Description: Use a codificação de caracteres UTF-8 para compatibilidade ideal entre aplicativos Web e outras plataformas baseadas em \*Nix (Unix, Linux e variantes), minimize os bugs de localização e reduza a sobrecarga de testes.
title: Use a página de código do Windows UTF-8
template: detail.hbs
ms.date: 06/12/2019
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 4b4050dfea1589fbe79db08061bcc56e392173f1
ms.sourcegitcommit: 13ce25364201223e21e2e5e89f99bc7aa4d93f56
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/08/2019
ms.locfileid: "73847594"
---
# <a name="use-the-utf-8-code-page"></a>Use a página de código UTF-8

Use a codificação de caracteres [UTF-8](http://www.utf-8.com/) para compatibilidade ideal entre aplicativos Web e outras plataformas baseadas em \*Nix (Unix, Linux e variantes), minimize os bugs de localização e reduza a sobrecarga de testes.

UTF-8 é a página de código universal para internacionalização e é capaz de codificar todo o conjunto de caracteres Unicode. Ele é usado de acordo com a Web e é o padrão para plataformas baseadas no * Nix.

> [!NOTE]
> Um caractere codificado leva entre 1 e 4 bytes. A codificação UTF-8 dá suporte a sequências de byte mais longas, até 6 bytes, mas o maior ponto de código do Unicode 6,0 (U + 10FFFF) leva apenas 4 bytes.

## <a name="-a-vs--w-apis"></a>-As APIs vs.-W
  
As APIs do Win32 geralmente dão suporte a variantes-A e-W.

-As variantes reconhecem a página de código ANSI configurada no sistema e dão suporte a `char*`, enquanto as variantes-W operam em UTF-16 e dão suporte a `WCHAR`.

Até recentemente, o Windows enfatiza as variantes de "Unicode"-W em relação a uma API. No entanto, versões recentes usaram a página de código ANSI e-uma APIs como um meio de introduzir o suporte a UTF-8 para aplicativos. Se a página de código ANSI estiver configurada para UTF-8, as APIs funcionarão em UTF-8. Esse modelo tem o benefício de dar suporte a código existente criado com APIs-A sem nenhuma alteração de código.

## <a name="set-a-process-code-page-to-utf-8"></a>Definir uma página de código de processo para UTF-8

A partir da versão 1903 do Windows (maio de 2019 atualização), você pode usar a propriedade ActiveCodePage no appxmanifest para aplicativos empacotados ou o manifesto do Fusion para aplicativos não empacotados, para forçar um processo a usar UTF-8 como a página de código do processo.

Você pode declarar essa propriedade e o destino/executar em compilações anteriores do Windows, mas você deve manipular a detecção de página de código herdada e a conversão como de costume. Com uma versão de destino mínima do Windows versão 1903, a página de código do processo sempre será UTF-8 para que a detecção e a conversão de página de código herdada possam ser evitadas.

## <a name="examples"></a>Exemplos

**Manifesto do Appx para um aplicativo empacotado:**

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

**Manifesto de fusão para um aplicativo Win32 não empacotado:**

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
> Adicionar um manifesto a um executável existente da linha de comando com `mt.exe -manifest <MANIFEST> -outputresource:<EXE>;#1`

## <a name="code-page-conversion"></a>Conversão de página de código

Como o Windows Opera nativamente no UTF-16 (`WCHAR`), talvez seja necessário converter dados UTF-8 em UTF-16 (ou vice-versa) para interoperar com APIs do Windows.

[MultiByteToWideChar](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-multibytetowidechar) e [WideCharToMultiByte](https://docs.microsoft.com/windows/desktop/api/stringapiset/nf-stringapiset-widechartomultibyte) permitem que você converta entre UTF-8 e UTF-16 (`WCHAR`) (e outras páginas de código). Isso é particularmente útil quando uma API do Win32 herdada só pode entender `WCHAR`. Essas funções permitem que você converta a entrada UTF-8 em `WCHAR` para passar para uma API-W e, em seguida, converter os resultados de volta, se necessário.
Ao usar essas funções com `CodePage` definido como `CP_UTF8`, use `dwFlags` de `0` ou `MB_ERR_INVALID_CHARS`, caso contrário, ocorrerá uma `ERROR_INVALID_FLAGS`.

> [!NOTE]
> `CP_ACP` é igual a `CP_UTF8` somente se estiver executando no Windows versão 1903 (maio de 2019 atualização) ou acima e a propriedade ActiveCodePage descrita acima estiver definida como UTF-8. Caso contrário, ela honra a página de código do sistema herdada. É recomendável usar `CP_UTF8` explicitamente.

## <a name="related-topics"></a>Tópicos relacionados

- [Páginas de código](https://docs.microsoft.com/windows/desktop/Intl/code-pages)
- [Identificadores de página de código](https://docs.microsoft.com/windows/desktop/Intl/code-page-identifiers)
