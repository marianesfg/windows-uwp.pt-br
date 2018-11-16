---
author: stevewhims
Description: Your tiles and toasts can load strings and images tailored for display language, display scale factor, high contrast, and other runtime contexts.
title: Suporte à notificação de bloco e do sistema para o idioma, a escala e o alto contraste
template: detail.hbs
ms.author: stwhi
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 89a97342139449b6c333055ec66e8939234a9507
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/16/2018
ms.locfileid: "6995067"
---
# <a name="tile-and-toast-notification-support-for-language-scale-and-high-contrast"></a>Suporte a blocos e notificações do sistema para idioma, escala e alto contraste

Seu blocos e notificações do sistema podem carregar cadeias de caracteres e imagens personalizadas para idioma de exibição, [fator de escala de exibição](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md), alto contraste e outros contextos de tempo de execução. Para obter informações sobre como usar os qualificadores nos nomes de seus arquivos de recurso, consulte [Personalizar os recursos para idioma, escala e outros qualificadores](../../../app-resources/tailor-resources-lang-scale-contrast.md) e [ícones e logotipos](/windows/uwp/design/style/app-icons-and-logos).

Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../../globalizing/globalizing-portal.md).

## <a name="refer-to-a-string-resource-from-a-template"></a>Refere-se a um recurso de cadeia de caracteres de um modelo

Em seu modelo de bloco ou notificação do sistema, você pode consultar um recurso de cadeia de caracteres usando o esquema URI (Uniform Resource Identifier) de `ms-resource` seguido por um identificador de recurso de cadeia de caracteres simples. Por exemplo, se você tiver um arquivo Resources.resx que contém uma entrada de recurso cujo nome é "Farewell", você tem um recurso de cadeia de caracteres com o identificador "Farewell". Para obter mais informações sobre identificadores de recurso de cadeia de caracteres e Arquivos de recurso (.resw), consulte [Localizar cadeias de caracteres em sua IU e manifesto do pacote de aplicativo](../../../app-resources/localize-strings-ui-manifest.md).

É assim que uma referência para o identificador de recurso de cadeia de caracteres "Farewell" ficaria no corpo de [texto](/uwp/schemas/tiles/tilesschema/element-text?branch=live) de seu modelo de conteúdo, usando `ms-resource`.

```xml
<text id="1">ms-resource:Farewell</text>
```

Se você omitir o esquema de URI do `ms-resource`, então o corpo de texto é apenas uma cadeia de caracteres literal, e *não* uma referência a um identificador.

```xml
<text id="1">Farewell</text>
```

## <a name="refer-to-an-image-resource-from-a-template"></a>Refere-se a um recurso de imagem de um modelo

Em seu modelo de bloco ou notificação do sistema, você pode consultar um recurso de imagem usando o esquema URI (Uniform Resource Identifier) de `ms-appx` seguido pelo nome do recurso de imagem. É assim também que você fará referência a um recurso de imagem na marcação XAML (para obter mais detalhes, consulte [Fazer referência a uma imagem ou outro ativo no código e na marcação XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)).

Por exemplo, você pode nomear pastas desta forma.

```
\Assets\Images\contrast-standard\welcome.png
\Assets\Images\contrast-high\welcome.png
```

Nesse caso, você tem um recurso de imagem única e seu nome (como um caminho absoluto) é `/Assets/Images/welcome.png`. Veja como usar esse nome em seu modelo.

```xml
<image id="1" src="ms-appx:///Assets/Images/welcome.png"/>
```

Observe como neste exemplo o URI do esquema ("`ms-appx`") é seguido por "`://`" que é seguido por um caminho absoluto (um caminho absoluto começa com "`/`").

## <a name="hosting-and-loading-images-in-the-cloud"></a>Hospedar e carregar imagens na nuvem

Os esquemas URI de `ms-resource` e `ms-appx` realizam a correspondência de qualificador automática para localizar o recurso que é mais apropriado para o contexto atual. Esquemas URI da Web (por exemplo, `http`, `https` e `ftp`) não executam quaisquer correspondências automáticas.

Em vez disso, acrescente no URI da imagem uma cadeia de caracteres de consulta que descreve o valor ou valores do qualificador solicitado.

```xml
<image id="1" src="http://www.contoso.com/Assets/Images/welcome.png?ms-lang=en-US"/>
```

Em seguida, no serviço de aplicativo que fornece suas imagens, implemente um manipulador de HTTP que inspeciona e usa a cadeia de caracteres de consulta para determinar qual imagem devolver.

Você também precisa definir o atributo [**addImageQuery**](/uwp/schemas/tiles/tilesschema/element-visual?branch=live) para `true` na carga XML de notificação do [bloco](/uwp/schemas/tiles/tilesschema/schema-root?branch=live) ou [sistema](/uwp/schemas/tiles/toastschema/schema-root?branch=live). O atributo **addImageQuery** aparece nos elementos `visual`, `binding` e `image` dos esquemas de bloco e notificação do sistema. Definir explicitamente **addImageQuery** em um elemento substituirá o valor definido em um ancestral. Por exemplo, um valor **addImageQuery** de `true` em um elemento de `image` substitui um **addImageQuery** de `false` em seu elemento `binding` pai.

Estas são as cadeias de caracteres de consulta que você pode usar.

| Qualificador | Cadeia de caracteres de consulta | Exemplo |
| --------- | ------------ | ------- |
| Escala | ms-scale | ?ms-scale=400 |
| Idioma | ms-lang | ?ms-lang=en-US |
| Contraste | ms-contrast | ?ms-contrast=high |

Para obter uma tabela de referência de todos os valores de qualificador possíveis que você pode usar em suas cadeias de caracteres de consulte, veja [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues).

## <a name="important-apis"></a>APIs Importantes

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)

## <a name="related-topics"></a>Tópicos relacionados

* [Tamanhos de tela e pontos de interrupção para um design responsivo](../../layout/screen-sizes-and-breakpoints-for-responsive-design.md)
* [Personalizar os recursos para idioma, escala e outros qualificadores](../../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Diretrizes para ativos de bloco e ícone](app-assets.md).
* [Globalização e localização](../../globalizing/globalizing-portal.md)
* [Localizar sequência na interface do usuário e no manifesto do pacote do aplicativo](../../../app-resources/localize-strings-ui-manifest.md)
* [Fazer referência a uma imagem ou outro ativo no código e na marcação XAML](../../../app-resources/images-tailored-for-scale-theme-contrast.md)
* [addImageQuery](/uwp/schemas/tiles/tilesschema/element-visual?branch=live)
* [Esquema de bloco](/uwp/schemas/tiles/tilesschema/schema-root?branch=live)
* [Esquema de notificação do sistema](/uwp/schemas/tiles/toastschema/schema-root?branch=live)
