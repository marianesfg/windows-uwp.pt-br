---
description: Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso que vem da implementação personalizada da pesquisa de recursos. A pesquisa de recursos é executada pela implementação da classe CustomXamlResourceLoader.
title: Extensão de marcação CustomResource
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d59a8662d430c527aa2f83aea945ee5bcf48642e
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366432"
---
# <a name="customresource-markup-extension"></a>Extensão de marcação {CustomResource}


Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso que vem da implementação personalizada da pesquisa de recursos. A pesquisa de recursos é executada pela implementação da classe [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader).

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| key | A chave para o recurso solicitado. A forma como a chave é inicialmente atribuída é específica da implementação da classe [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) registrada atualmente para uso. |

## <a name="remarks"></a>Comentários

**CustomResource** é uma técnica para obtenção de valores definidos em outro lugar em um repositório de recursos personalizado. Esta técnica é relativamente avançada e não é usada pela maioria dos cenários de aplicativos Tempo de Execução do Windows.

A forma como um **CustomResource** é resolvido para um dicionário de recursos não é descrita neste tópico, pois isso pode variar muito dependendo de como [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) é implementado.

O método [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) da implementação de [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) é chamado pelo analisador XAML do Windows Runtime sempre que ele encontra um uso de `{CustomResource}` na marcação. O *resourceId* que é passado para **GetResource** vem do argumento *key*, e os outros parâmetros de entrada vêm do contexto, como a propriedade à qual o uso se aplica.

O uso de um `{CustomResource}` não funciona por padrão (a implementação básica do [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource) está incompleta). Para fazer uma referência de `{CustomResource}` válida, você deve executar estas etapas:

1.  Derive uma classe personalizada de [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader) e substitua o método [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource). Não chame a base na implementação.
2.  Defina [**CustomXamlResourceLoader.Current**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.current) para referência de sua classe na lógica de inicialização. Isso deve acontecer antes de ser carregado qualquer XAML de nível de página, que inclui o uso da extensão `{CustomResource}`. Um local para definir **CustomXamlResourceLoader.Current** é o construtor da subclasse [**Application**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Application) gerado para você nos modelos code-behind de App.xaml.
3.  Agora você pode usar as extensões `{CustomResource}` no XAML que seu aplicativo carrega como páginas, ou de dentro de dicionários de recursos XAML.

**CustomResource** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação no uso XAML a "\{"e"\}" caracteres na sintaxe de atributo, que é a convenção pela qual um processador XAML reconhece que uma extensão de marcação precisa processar o atributo.

## <a name="related-topics"></a>Tópicos relacionados

* [Referências de recurso de ResourceDictionary e XAML](https://docs.microsoft.com/windows/uwp/controls-and-patterns/resourcedictionary-and-xaml-resource-references)
* [**CustomXamlResourceLoader**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Resources.CustomXamlResourceLoader)
* [**GetResource**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.resources.customxamlresourceloader.getresource)

