---
Description: Há vários esquemas de URI (Uniform Resource Identifier) que você pode usar para fazer referência aos arquivos que vêm no conjunto de aplicativo, nas pastas de dados do seu aplicativo ou na nuvem. Você também pode usar um esquema de URI para fazer referência a cadeias de caracteres carregadas dos arquivos de recursos (.resw) do app.
title: Esquemas de URI
template: detail.hbs
ms.date: 10/16/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 5d66d717d77d2797e8a483871b8d3369befb5b6b
ms.sourcegitcommit: 46890e7f3c1287648631c5e318795f377764dbd9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2019
ms.locfileid: "58320579"
---
# <a name="uri-schemes"></a>Esquemas de URI

Há vários esquemas de URI (Uniform Resource Identifier) que você pode usar para fazer referência aos arquivos que vêm no conjunto de aplicativo, nas pastas de dados do seu aplicativo ou na nuvem. Você também pode usar um esquema de URI para fazer referência a cadeias de caracteres carregadas dos arquivos de recursos (.resw) do app. Você pode usar esses esquemas de URI em seu código, em sua marcação XAML, no manifesto do conjunto de aplicativo ou em seu bloco e modelos de notificação do sistema.

## <a name="common-features-of-the-uri-schemes"></a>Recursos comuns dos esquemas de URI

Todos os esquemas descritos neste tópico seguem as regras típicas de esquema URI para normalização e recursos de recuperação. Consulte [RFC 3986](https://go.microsoft.com/fwlink/p/?LinkId=263444) para obter a sintaxe genérica de uma URI.

Todos os esquemas de URI definem a parte hierárquica por [RFC 3986](https://go.microsoft.com/fwlink/p/?LinkId=263444) como os componentes de autoridade e de caminho do URI.

```syntax
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Isso significa que há essencialmente três componentes para um URI. Imediatamente após as duas barras "/" do *esquema* URI, está um componente (que pode estar vazio) chamado de *autoridade*. E imediatamente após isto, está o *caminho*. Levando o URI `http://www.contoso.com/welcome.png` como exemplo, o esquema é "`http://`", a autoridade é "`www.contoso.com`", e o caminho é "`/welcome.png`". Outro exemplo é o `ms-appx:///logo.png` de URI, onde os componentes de autoridade estão vazios e têm um valor padrão.

O componente de fragmento é ignorado pelo processamento específico do esquema de URIs mencionado neste tópico. Durante a recuperação e a comparação de recursos, o componente de fragmento não tem relevância. No entanto, as camadas acima da implementação específica podem interpretar o fragmento para recuperar um recurso secundário.

A comparação ocorre byte por byte após a normalização de todos os componentes de IRI.

## <a name="case-insensitivity-and-normalization"></a>Sem diferenciação de maiúsculas e minúsculas e normalização

Todos os esquemas de URI descritos neste tópico seguem as regras típicas de URI (RFC 3986) para normalização e recursos de recuperação de esquemas. A forma normalizada desses URIs mantém a capitalização e faz a decodificação percentual dos caracteres não reservados RFC 3986.

Para todos os esquemas URI descritos neste tópico, *esquema*, *autoridade* e *caminho* diferenciam maiúsculas de minúsculas por padrão, ou são processados pelo sistema de uma forma que diferencie maiúsculas de minúsculas. **Observação** A única exceção a essa regra é a *autoridade* de `ms-resource`, que diferencia maiúsculas de minúsculas.

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx e ms-appx-web

Use o esquema de URI `ms-appx` ou `ms-appx-web` para fazer referência a um arquivo que vem do conjunto de aplicativo (veja [Empacotando aplicativos](../packaging/index.md)). Arquivos em seu pacote de aplicativo geralmente são imagens estáticas, dados, código e arquivos de layout. O esquema `ms-appx-web` acessa os mesmos arquivos que `ms-appx`, mas no compartimento da Web. Para obter exemplos e mais informações, consulte [Referenciar uma imagem ou outro caminho da marcação e código de XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Nome de esquema (ms-appx e ms-appx-web)

O nome do esquema de URI é a cadeia de caracteres "ms-appx" ou "ms-appx-web".

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autoridade (ms-appx e ms-appx-web)

A autoridade é o nome de identidade de pacote que é definido no manifesto do pacote. Portanto, ela está limitada no formato URI e IRI (Internationalized resource identifier) ao conjunto de caracteres permitido em um nome de identificador de pacote. O nome do pacote deve ser o nome de um dos pacotes no gráfico de dependência de conjunto de aplicativo em execução atualmente.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Se algum outro caractere aparecer na autoridade, então a recuperação e a comparação falharão. O valor padrão para a autoridade é o conjunto de aplicativo em execução no momento.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Informações do usuário e porta (ms-appx e ms-appx-web)

O esquema `ms-appx`, ao contrário de outros esquemas populares, não define um componente de informações do usuário ou de porta. Como "@" and ":" não são permitidos como valores de autoridade válidos, a pesquisa falhará se eles forem incluídos. Todos os itens a seguir falham.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Caminho (ms-appx e ms-appx-web)

O componente de caminho corresponde à sintaxe RFC 3986 genérica e aceita caracteres não ASCII em IRIs. O componente de caminho define o caminho de arquivo físico ou lógico de um arquivo. Esse arquivo está em uma pasta associada ao local instalado do conjunto de aplicativo especificado pela autoridade.

Se o caminho fizer referência a um nome de arquivo, então o caminho físico do arquivo será recuperado. Mas se esse arquivo físico não for encontrado, então o recurso real retornado durante a recuperação é determinado através da negociação do conteúdo em tempo de execução. Essa determinação baseia-se no aplicativo, sistema operacional e configurações do usuário, como idioma, exibir fator de escala, tema, alto contraste e outros contextos de tempo de execução. Por exemplo, uma combinação dos idiomas do aplicativo, as configurações de exibição do sistema e as configurações de alto contraste do usuário podem ser levadas em consideração ao determinar o valor de recurso real a ser recuperado.

```xml
ms-appx:///images/logo.png
```

O URI acima é realmente capaz de recuperar um arquivo no conjunto de aplicativo atual com o seguinte nome de arquivo físico.

<blockquote>
<pre>
\Images\fr-FR\logo.scale-100_contrast-white.png
</blockquote>
</pre>

É claro que também é possível recuperar esse mesmo arquivo físico fazendo referência a ele diretamente pelo seu nome completo.

```xaml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

O componente de caminho de `ms-appx(-web)` diferencia maiúsculas de minúsculas, como os URIs genéricos. No entanto, quando o sistema de arquivos subjacente pelo qual o recurso é acessado não faz diferenciação de maiúsculas e minúsculas, como para NTFS, a recuperação do recurso é feita sem diferenciar maiúsculas de minúsculas.

O formato normalizado do URI mantém a capitalização e faz a decodificação (um símbolo de "%" seguido pela representação hexadecimal de dois dígitos) RFC 3986 dos caracteres. Os caracteres "?", "#", "/", "*" e '”' (o caractere de aspas duplas) devem ter codificação por percentual em um caminho para representar dados como um arquivo ou nomes de pasta. Todos os caracteres codificados percentuais são decodificados antes da recuperação. Dessa forma, para recuperar um arquivo denominado Hello#World.html, use essa URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Consulta (ms-appx e ms-appx-web)

Parâmetros de consulta são ignorados durante a recuperação de recursos. O formato normalizado de parâmetros de consulta mantém a capitalização. Os parâmetros de consulta não são ignorados durante a comparação.

## <a name="ms-appdata"></a>ms-appdata

Use o esquema de URI `ms-appdata` para se referir a arquivos que são provenientes das pastas de dados temporários, móveis e locais do aplicativo. Para obter mais informações sobre essas pastas de dados de aplicativo, consulte [Armazenar e recuperar configurações e outros dados de aplicativo](../design/app-settings/store-and-retrieve-app-data.md).

O esquema `ms-appdata` de URI não realiza a negociação de conteúdo de tempo de execução que [ms-appx e ms-appx-web](#ms-appx-and-ms-appx-web) realiza. Mas você pode responder ao conteúdo do [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) e carregar os ativos adequados de dados do aplicativo usando o nome de arquivo físico completo no URI.

### <a name="scheme-name-ms-appdata"></a>Nome de esquema (ms-appdata)

O nome do esquema de URI é a cadeia de caracteres "ms-appdata".

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autoridade (ms-appdata)

A autoridade é o nome de identidade de pacote que é definido no manifesto do pacote. Portanto, ela está limitada no formato URI e IRI (Internationalized resource identifier) ao conjunto de caracteres permitido em um nome de identificador de pacote. O nome do pacote deve ser o nome do conjunto de aplicativo em execução atual.

```xml
ms-appdata://Contoso.MyApp/
```

Se algum outro caractere aparecer na autoridade, então a recuperação e a comparação falharão. O valor padrão para a autoridade é o conjunto de aplicativo em execução no momento.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Informações do usuário e a porta (ms-appdata)

O esquema `ms-appdata`, ao contrário de outros esquemas populares, não define um componente de informações do usuário ou de porta. Como "@" and ":" não são permitidos como valores de autoridade válidos, a pesquisa falhará se eles forem incluídos. Todos os itens a seguir falham.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Caminho (ms-appdata)

O componente de caminho corresponde à sintaxe RFC 3986 genérica e aceita caracteres não ASCII em IRIs. Dentro da localização [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live) estão três pastas reservadas para armazenamento de estado local, roaming e temporário. O esquema `ms-appdata` permite acesso a arquivos e pastas nesses locais. O primeiro segmento do componente de caminho deve indicar a pasta específica no local a seguir. Dessa forma, o formato "path-empty" de "hier-part" não é permitido.

Pasta local.

```xml
ms-appdata:///local/
```

Pasta temporária.

```xml
ms-appdata:///temp/
```

Pasta móvel.

```xml
ms-appdata:///roaming/
```

O componente de caminho de `ms-appdata` diferencia maiúsculas de minúsculas, como os URIs genéricos. No entanto, quando o sistema de arquivos subjacente pelo qual o recurso é acessado não faz diferenciação de maiúsculas e minúsculas, como para NTFS, a recuperação do recurso é feita sem diferenciar maiúsculas de minúsculas.

O formato normalizado do URI mantém a capitalização e faz a decodificação (um símbolo de "%" seguido pela representação hexadecimal de dois dígitos) RFC 3986 dos caracteres. Os caracteres "?", "#", "/", "*" e '”' (o caractere de aspas duplas) devem ter codificação por percentual em um caminho para representar dados como um arquivo ou nomes de pasta. Todos os caracteres codificados percentuais são decodificados antes da recuperação. Dessa forma, para recuperar um arquivo local denominado Hello#World.html, use essa URI.

```xml
ms-appdata://local/Hello%23World.html
```

A recuperação do recurso e a identificação do segmento de caminho de nível superior são controladas após a normalização de pontos (".././b/c"). Portanto, os URIs não podem colocar o ponto neles mesmos fora de uma das pastas reservadas. Sendo assim, o seguinte URI não é permitido.

```xml
ms-appdata:///local/../hello/logo.png
```

Mas este URI é permitido (embora redundante).

```xml
ms-appdata:///local/../roaming/logo.png
```

### <a name="query-ms-appdata"></a>Consulta (ms-appdata)

Parâmetros de consulta são ignorados durante a recuperação de recursos. O formato normalizado de parâmetros de consulta mantém a capitalização. Os parâmetros de consulta não são ignorados durante a comparação.

## <a name="ms-resource"></a>ms-resource

Use o esquema URI `ms-resource` para fazer referência a cadeias de caracteres carregadas dos arquivos de recursos (.resw) do app. Para obter exemplos e mais informações sobre os arquivos de recursos, consulte [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote de aplicativos](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Nome de esquema (ms-resource)

O nome do esquema de URI é a cadeia de caracteres "ms-resource".

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autoridade (ms-resource)

A autoridade é o mapa de recursos de nível superior definido no Package Resource Index (PRI), que geralmente corresponde ao nome do identificador de pacote definido no manifesto do pacote. Consulte [Empacotando aplicativos](../packaging/index.md)). Portanto, ela está limitada no formato URI e IRI (Internationalized resource identifier) ao conjunto de caracteres permitido em um nome de identificador de pacote. O nome do pacote deve ser o nome de um dos pacotes no gráfico de dependência de conjunto de aplicativo em execução atualmente.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Se algum outro caractere aparecer na autoridade, então a recuperação e a comparação falharão. O valor padrão para a autoridade é o nome do pacote que difere maiúsculas de minúsculas em execução no momento.

```xml
ms-resource:///
```

A autoridade faz diferenciação de maiúsculas de minúsculas, e o formato normalizado mantém sua capitalização. No entanto, a pesquisa de um recurso faz isso sem diferenciar maiúsculas de minúsculas.

### <a name="user-info-and-port-ms-resource"></a>Informações do usuário e a porta (ms-resource)

O esquema `ms-resource`, ao contrário de outros esquemas populares, não define um componente de informações do usuário ou de porta. Como "@" and ":" não são permitidos como valores de autoridade válidos, a pesquisa falhará se eles forem incluídos. Todos os itens a seguir falham.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Caminho (ms-resource)

O caminho identifica a localização hierárquica da subárvore [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) (veja [Sistema de Gerenciamento de Recursos](https://msdn.microsoft.com/library/windows/apps/jj552947)) e o [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResource?branch=live) presente nela. Geralmente, isso corresponde ao nome do arquivo (excluindo a extensão) de Arquivos de Recursos (.resw) e o identificador de um recurso de cadeia de caracteres dentro dele.

Para obter exemplos e mais informações, consulte [Localizar cadeias de caracteres em sua IU e manifesto do pacote de aplicativo](localize-strings-ui-manifest.md) e [Suporte à notificação de bloco e notificações do sistema para o idioma, escala e alto contraste](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).

O componente de caminho de `ms-resource` diferencia maiúsculas de minúsculas, como os URIs genéricos. No entanto, a recuperação subjacente faz um [CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628) com *ignoreCase* definido como `true`.

O formato normalizado do URI mantém a capitalização e faz a decodificação (um símbolo de "%" seguido pela representação hexadecimal de dois dígitos) RFC 3986 dos caracteres. Os caracteres "?", "#", "/", "*" e '”' (o caractere de aspas duplas) devem ter codificação por percentual em um caminho para representar dados como um arquivo ou nomes de pasta. Todos os caracteres codificados percentuais são decodificados antes da recuperação. Portanto, para recuperar um recurso de cadeia de caracteres de um arquivo de recursos chamado `Hello#World.resw`, usar este URI.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Consulta (ms-resource)

Parâmetros de consulta são ignorados durante a recuperação de recursos. O formato normalizado de parâmetros de consulta mantém a capitalização. Os parâmetros de consulta não são ignorados durante a comparação. Eles são comparados quanto ao uso de maiúsculas e minúsculas.

Os desenvolvedores de componentes específicos dispostos em camadas acima desta análise de URI podem optar por usar os parâmetros de consulta conforme considerarem adequado.

## <a name="related-topics"></a>Tópicos relacionados

* [Uniform Resource Identifier (URI): Sintaxe genérica](https://go.microsoft.com/fwlink/p/?LinkId=263444)
* [Empacotando aplicativos](../packaging/index.md)
* [Fazer referência a uma imagem ou outros ativos de código e marcação XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Armazene e recupere configurações e outros dados de aplicativo](../design/app-settings/store-and-retrieve-app-data.md)
* [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md)
* [Sistema de Gerenciamento de Recursos](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [Suporte à notificação de bloco e notificação do sistema para o idioma, escala e alto contraste](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md)