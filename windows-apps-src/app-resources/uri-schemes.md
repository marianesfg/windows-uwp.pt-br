---
author: stevewhims
Description: "Há vários esquemas URI (Uniform Resource Identifier) que você pode usar para fazer referência aos arquivos provenientes do pacote do app, das pastas de dados do app ou da nuvem. Você também pode usar um esquema URI para fazer referência a cadeias de caracteres carregadas a partir dos arquivos de recursos (.resw)."
title: Esquemas URI
template: detail.hbs
ms.author: stwhi
ms.date: 10/16/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
localizationpriority: medium
ms.openlocfilehash: 7cbd4a6fea3ca7d179eae9857c6e98010d11439e
ms.sourcegitcommit: d0c93d734639bd31f264424ae5b6fead903a951d
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/03/2017
---
<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css">

# <a name="uri-schemes"></a>Esquemas URI

Há vários esquemas URI (Uniform Resource Identifier) que você pode usar para fazer referência aos arquivos provenientes do pacote do app, das pastas de dados do app ou da nuvem. Você também pode usar um esquema URI para fazer referência a cadeias de caracteres carregadas a partir dos arquivos de recursos (.resw). Você pode usar esses esquemas URI no código, na marcação XAML, no manifesto do pacote do aplicativo ou nos modelos de bloco e de notificação do sistema.

## <a name="common-features-of-the-uri-schemes"></a>Recursos comuns dos esquemas URI

Todos os esquemas descritos neste tópico seguem as regras típicas de esquema URI para normalização e recuperação de recursos. Consulte [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444) para obter a sintaxe genérica de um URI.

Todos os esquemas URI definem a parte hierárquica de acordo com a [RFC 3986](http://go.microsoft.com/fwlink/p/?LinkId=263444) como componentes de autoridade e de caminho do URI.

```
URI         = scheme ":" hier-part [ "?" query ] [ "#" fragment ]
hier-part   = "//" authority path-abempty
            / path-absolute
            / path-rootless
            / path-empty
```

Isso significa que há essencialmente três componentes em um URI. Imediatamente após as duas barras "/" do *esquema* URI, está um componente (que pode estar vazio) chamado *autoridade*. E imediatamente após isso, está o *caminho*. Tomando o URI `http://www.contoso.com/welcome.png` como exemplo, o esquema é "`http://`", a autoridade é "`www.contoso.com`" e o caminho é "`/welcome.png`". Outro exemplo é o URI `ms-appx:///logo.png`, em que os componentes de autoridade estão vazios e têm um valor padrão.

O componente de fragmento é ignorado pelo processamento específico de esquema dos URIs mencionados neste tópico. Durante a recuperação e a comparação dos recursos, o componente de fragmento não tem relevância. No entanto, as camadas acima da implementação específica podem interpretar o fragmento para recuperar um recurso secundário.

A comparação ocorre byte por byte após a normalização de todos os componentes de IRI.

## <a name="case-insensitivity-and-normalization"></a>Sem diferenciação de maiúsculas e minúsculas e normalização

Todos os esquemas de URI descritos neste tópico seguem as regras típicas de URI (RFC 3986) quanto a normalização e recuperação recursos nos esquemas. A forma normalizada desses URIs mantém a capitalização e faz a decodificação percentual dos caracteres não reservados da RFC 3986.

Em todos os esquemas URI descritos neste tópico, *esquema*, *autoridade* e *caminho* não diferenciam maiúsculas de minúsculas por padrão, ou são processados pelo sistema de uma forma que não diferencia maiúsculas de minúsculas. **Observação** A única exceção a essa regra é a *autoridade* de `ms-resource`, que diferencia maiúsculas de minúsculas.

## <a name="ms-appx-and-ms-appx-web"></a>ms-appx e ms-appx-web

Use o esquema URI `ms-appx` ou `ms-appx-web` para fazer referência a um arquivo que vem do pacote do aplicativo (consulte [Empacotando apps](../packaging/index.md)). Os arquivos do pacote de aplicativo geralmente são imagens estáticas, dados, código e arquivos de layout. O esquema `ms-appx-web` acessa os mesmos arquivos que `ms-appx`, mas no compartimento da Web. Para obter exemplos e mais informaões, consulte [Fazer referência a uma imagem ou outro ativo no código e na marcação XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

### <a name="scheme-name-ms-appx-and-ms-appx-web"></a>Nome do esquema (ms-appx e ms-appx-web)

O nome do esquema URI é a cadeia de caracteres "ms-appx" ou "ms-appx-web".

```xml
ms-appx://
```

```xml
ms-appx-web://
```

### <a name="authority-ms-appx-and-ms-appx-web"></a>Autoridade (ms-appx e ms-appx-web)

A autoridade é o nome de identidade de pacote definido no manifesto do pacote. Portanto, ela está limitada no formato URI e IRI (Internationalized resource identifier) ao conjunto de caracteres permitido em um nome de identidade de pacote. O nome do pacote deve ser o nome de um dos pacotes no gráfico de dependência de pacote do app em execução.

```xml
ms-appx://Contoso.MyApp/
ms-appx-web://Contoso.MyApp/
```

Se algum outro caractere aparecer na autoridade, é sinal de que houve falha na recuperação e na comparação. O valor padrão da autoridade é o pacote do app em execução.

```xml
ms-appx:///
ms-appx-web:///
```

### <a name="user-info-and-port-ms-appx-and-ms-appx-web"></a>Informações do usuário e porta (ms-appx e ms-appx-web)

O esquema `ms-appx`, diferente de outros esquemas populares, não define um componente de informações do usuário ou de porta. Como "@" and ":" não são permitidos como valores de autoridade válidos, a pesquisa falhará se eles forem incluídos. Todos os itens a seguir apresentam falha.

```xml
ms-appx://john@contoso.myapp/default.html
ms-appx://john:password@contoso.myapp/default.html
ms-appx://contoso.myapp:8080/default.html
ms-appx://john:password@contoso.myapp:8080/default.html
```

### <a name="path-ms-appx-and-ms-appx-web"></a>Caminho (ms-appx e ms-appx-web)

O componente de caminho corresponde à sintaxe RFC 3986 genérica e aceita caracteres não ASCII em IRIs. O componente de caminho define o caminho de arquivo físico ou lógico de um arquivo. Esse arquivo está em uma pasta associada ao local instalado do pacote do aplicativo, para o app especificado pela autoridade.

Se o caminho fizer referência a um nome de arquivo e caminho físico, o ativo do arquivo físico será recuperado. Mas, se esse arquivo físico não for encontrado, o recurso real retornado durante a recuperação será determinado através da negociação do conteúdo em tempo de execução. Essa determinação baseia-se no app, no sistema operacional e nas configurações do usuário, como idioma, fator de escala de exibição, tema, alto contraste e outros contextos de tempo de execução. Por exemplo, uma combinação de idiomas do app, configurações de exibição do sistema e configurações de alto contraste do usuário podem ser levadas em consideração ao determinar o valor de recurso real a ser recuperado.

```xml
ms-appx:///images/logo.png
```

O URI acima é realmente capaz de recuperar um arquivo no pacote do app atual com o seguinte nome de arquivo físico.

```
\Images\fr-FR\logo.scale-100_contrast-white.png
```

Claro que também é possível recuperar esse mesmo arquivo físico fazendo referência a ele diretamente pelo seu nome completo.

**XAML**
```xml
<Image Source="ms-appx:///images/fr-FR/logo.scale-100_contrast-white.png"/>
```

O componente de caminho de `ms-appx(-web)` diferencia maiúsculas de minúsculas, como os URIs genéricos. No entanto, quando o sistema de arquivos subjacente pelo qual o recurso é acessado não faz diferenciação de maiúsculas e minúsculas, como no NTFS, a recuperação do recurso é feita sem diferenciar maiúsculas de minúsculas.

O formato normalizado do URI mantém a capitalização e faz a decodificação percentual (um símbolo de "%" seguido pela representação hexadecimal de dois dígitos) dos caracteres não reservados da RFC 3986. Os caracteres "?", "#", "/", "*" e '”' (o caractere de aspas duplas) devem ter codificação percentual em um caminho para representar dados como nomes de arquivo ou de pasta. Todos os caracteres codificados por percentual são decodificados antes da recuperação. Dessa forma, para recuperar um arquivo denominado Hello#World.html, use esse URI.

```xml
ms-appx:///Hello%23World.html
```

### <a name="query-ms-appx-and-ms-appx-web"></a>Consulta (ms-appx e ms-appx-web)

Os parâmetros de consulta são ignorados durante a recuperação de recursos. O formato normalizado de parâmetros de consulta mantém a capitalização. Os parâmetros de consulta não são ignorados durante a comparação.

## <a name="ms-appdata"></a>ms-appdata

Use o esquema URI `ms-appdata` para fazer referência a arquivos que são provenientes das pastas de dados locais, móveis e temporários do app. Para obter mais informações sobre essas pastas de dados de app, consulte [Armazenar e recuperar configurações e outros dados de app](../app-settings/store-and-retrieve-app-data.md).

O esquema URI `ms-appdata` não faz a negociação de conteúdo de tempo de execução que [ms-appx e ms-appx-web](#ms-appx-and-ms-appx-web) faz. Mas você pode responder ao conteúdo de [Qualifiervalues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live#Windows_ApplicationModel_Resources_Core_ResourceContext_QualifierValues) e carregar os ativos adequados dos dados do app usando o nome de arquivo físico completo no URI.

### <a name="scheme-name-ms-appdata"></a>Nome do esquema (ms-appdata)

O nome do esquema URI é a cadeia de caracteres "ms-appdata".

```xml
ms-appdata://
```

### <a name="authority-ms-appdata"></a>Autoridade (ms-appdata)

A autoridade é o nome de identidade de pacote que é definido no manifesto do pacote. Portanto, ela está limitada no formato URI e IRI (Internationalized resource identifier) ao conjunto de caracteres permitido em um nome de identificador de pacote. O nome do pacote deve ser o nome do pacote do app em execução.

```xml
ms-appdata://Contoso.MyApp/
```

Se algum outro caractere aparecer na autoridade, é sinal de que houve falha na recuperação e na comparação. O valor padrão da autoridade é o pacote do app em execução.

```xml
ms-appdata:///
```

### <a name="user-info-and-port-ms-appdata"></a>Informações do usuário e porta (ms-appdata)

O esquema `ms-appdata`, diferente de outros esquemas populares, não define um componente de informações do usuário ou de porta. Como "@" and ":" não são permitidos como valores de autoridade válidos, a pesquisa falhará se eles forem incluídos. Todos os itens a seguir apresentam falha.

```xml
ms-appdata://john@contoso.myapp/local/data.xml
ms-appdata://john:password@contoso.myapp/local/data.xml
ms-appdata://contoso.myapp:8080/local/data.xml
ms-appdata://john:password@contoso.myapp:8080/local/data.xml
```

### <a name="path-ms-appdata"></a>Caminho (ms-appdata)

O componente de caminho corresponde à sintaxe genérica da RFC 3986 e oferece suporte a caracteres não ASCII em IRIs. No local [Windows.Storage.ApplicationData](/uwp/api/Windows.Storage.ApplicationData?branch=live), há três pastas reservadas para armazenamento de estado local, móvel e temporário. O esquema `ms-appdata` permite acesso a arquivos e pastas nesses locais. O primeiro segmento do componente de caminho deve indicar a pasta específica da seguinte maneira: Dessa forma, o formato "path-empty" de "hier-part" não é permitido.

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

O componente de caminho de `ms-appdata` diferencia maiúsculas de minúsculas, como os URIs genéricos. No entanto, quando o sistema de arquivos subjacente pelo qual o recurso é acessado não faz diferenciação de maiúsculas e minúsculas, como no NTFS, a recuperação do recurso é feita sem diferenciar maiúsculas de minúsculas.

O formato normalizado do URI mantém a capitalização e faz a decodificação percentual (um símbolo de "%" seguido pela representação hexadecimal de dois dígitos) dos caracteres não reservados da RFC 3986. Os caracteres "?", "#", "/", "*" e '”' (o caractere de aspas duplas) devem ter codificação percentual em um caminho para representar dados como nomes de arquivo ou de pasta. Todos os caracteres codificados por percentual são decodificados antes da recuperação. Dessa forma, para recuperar um arquivo local denominado Hello#World.html, use esse URI.

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

Os parâmetros de consulta são ignorados durante a recuperação de recursos. O formato normalizado de parâmetros de consulta mantém a capitalização. Os parâmetros de consulta não são ignorados durante a comparação.

## <a name="ms-resource"></a>ms-resource

Use o esquema URI `ms-resource` para fazer referência a cadeias de caracteres carregadas dos arquivos de recursos (.resw) do app. Para obter exemplos e mais informações sobre os arquivos de recursos, consulte [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md).

### <a name="scheme-name-ms-resource"></a>Nome do esquema (ms-resource)

O nome do esquema URI é a cadeia de caracteres "ms-resource".

```xml
ms-resource://
```

### <a name="authority-ms-resource"></a>Autoridade (ms-resource)

A autoridade é o mapa de recursos de nível superior definido no PRI (Índice de Recursos do Pacote), que geralmente corresponde ao nome de identidade de pacote definido no manifesto do pacote. Consulte [Empacotando apps](../packaging/index.md)). Portanto, ela está limitada no formato URI e IRI (Internationalized resource identifier) ao conjunto de caracteres permitido em um nome de identificador de pacote. O nome do pacote deve ser o nome de um dos pacotes no gráfico de dependência de pacote do app em execução.

```xml
ms-resource://Contoso.MyApp/
ms-resource://Microsoft.WinJS.1.0/
```

Se algum outro caractere aparecer na autoridade, é sinal de que houve falha na recuperação e na comparação. O valor padrão da autoridade é o nome de pacote com diferenciação de maiúsculas e minúsculas do app em execução.

```xml
ms-resource:///
```

A autoridade faz diferenciação de maiúsculas e minúsculas, e o formato normalizado mantém sua capitalização. No entanto, a pesquisa de um recurso faz isso sem diferenciar maiúsculas de minúsculas.

### <a name="user-info-and-port-ms-resource"></a>Informações do usuário e porta (ms-resource)

O esquema `ms-resource`, diferente de outros esquemas populares, não define um componente de informações do usuário ou de porta. Como "@" and ":" não são permitidos como valores de autoridade válidos, a pesquisa falhará se eles forem incluídos. Todos os itens a seguir apresentam falha.

```xml
ms-resource://john@contoso.myapp/Resources/String1
ms-resource://john:password@contoso.myapp/Resources/String1
ms-resource://contoso.myapp:8080/Resources/String1
ms-resource://john:password@contoso.myapp:8080/Resources/String1
```

### <a name="path-ms-resource"></a>Caminho (ms-resource)

O caminho identifica a localização hierárquica da subárvore [ResourceMap](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceMap?branch=live) (consulte [Sistema de Gerenciamento de Recursos](https://msdn.microsoft.com/library/windows/apps/jj552947)) e o [NamedResource](/uwp/api/Windows.ApplicationModel.Resources.Core.NamedResourcebranch=live) presente nela. Geralmente, isso corresponde ao nome de um arquivo de recursos (.resw), excluindo a extensão, e ao identificador de um recurso de cadeia de caracteres nele contido.

Para obter exemplos e mais informações, consulte [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md) e [Suporte a blocos e notificações do sistema para idioma, escala e alto contraste](tile-toast-language-scale-contrast.md).

O componente de caminho de `ms-resource` diferencia maiúsculas de minúsculas, como os URIs genéricos. No entanto, quando a recuperação subjacente realiza um [CompareStringOrdinal](https://msdn.microsoft.com/library/windows/apps/br224628) com *ignoreCase* definido como `true`.

O formato normalizado do URI mantém a capitalização e faz a decodificação (um símbolo de "%" seguido pela representação hexadecimal de dois dígitos) dos caracteres não reservados da RFC 3986. Os caracteres "?", "#", "/", "*" e '”' (o caractere de aspas duplas) devem ter codificação percentual em um caminho para representar dados como nomes de arquivo ou de pasta. Todos os caracteres codificados por percentual são decodificados antes da recuperação. Assim, para recuperar um recurso de cadeia de caracteres de um arquivo de recursos chamado Hello#World.resw, use esse URI.

```xml
ms-resource:///Hello%23World/String1
```

### <a name="query-ms-resource"></a>Consulta (ms-resource)

Os parâmetros de consulta são ignorados durante a recuperação de recursos. O formato normalizado de parâmetros de consulta mantém a capitalização. Os parâmetros de consulta não são ignorados durante a comparação. Eles são comparados quanto ao uso de maiúsculas e minúsculas.

Os desenvolvedores de componentes específicos dispostos em camadas acima desta análise de URI podem optar por usar os parâmetros de consulta conforme considerarem adequado.

## <a name="related-topics"></a>Tópicos relacionados

* [URI (Uniform Resource Identifier): sintaxe genérica](http://go.microsoft.com/fwlink/p/?LinkId=263444)
* [Empacotando apps](../packaging/index.md)
* [Fazer referência a uma imagem ou outro ativo no código e na marcação XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code)
* [Armazenar e recuperar configurações e outros dados de app](../app-settings/store-and-retrieve-app-data.md)
* [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md)
* [Sistema de Gerenciamento de Recursos](https://msdn.microsoft.com/library/windows/apps/jj552947)
* [Suporte a blocos e notificações do sistema para idioma, escala e alto contraste](tile-toast-language-scale-contrast.md)