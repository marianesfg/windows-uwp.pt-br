---
author: jwmsft
description: Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso que vem da implementação personalizada da pesquisa de recursos. A pesquisa de recursos é executada pela implementação da classe CustomXamlResourceLoader.
title: Extensão de marcação CustomResource
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6fb88fc3a764a89dfe7ac8a9c399149b6b98eca5
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6282978"
---
# <a name="customresource-markup-extension"></a>Extensão de marcação {CustomResource}


Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso que vem da implementação personalizada da pesquisa de recursos. A pesquisa de recursos é executada pela implementação da classe [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327).

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| chave | A chave para o recurso solicitado. A forma como a chave é inicialmente atribuída é específica da implementação da classe [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) registrada atualmente para uso. |

## <a name="remarks"></a>Comentários

**CustomResource** é uma técnica para obtenção de valores definidos em outro lugar em um repositório de recursos personalizado. Esta técnica é relativamente avançada e não é usada pela maioria dos cenários de aplicativos Tempo de Execução do Windows.

A forma como um **CustomResource** é resolvido para um dicionário de recursos não é descrita neste tópico, pois isso pode variar muito dependendo de como [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) é implementado.

O método [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) da implementação de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) é chamado pelo analisador XAML do Windows Runtime sempre que ele encontra um uso de `{CustomResource}` na marcação. O *resourceId* que é passado para **GetResource** vem do argumento *key*, e os outros parâmetros de entrada vêm do contexto, como a propriedade à qual o uso se aplica.

O uso de um `{CustomResource}` não funciona por padrão (a implementação básica do [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) está incompleta). Para fazer uma referência de `{CustomResource}` válida, você deve executar estas etapas:

1.  Derive uma classe personalizada de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) e substitua o método [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340). Não chame a base na implementação.
2.  Defina [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328) para referência de sua classe na lógica de inicialização. Isso deve acontecer antes de ser carregado qualquer XAML de nível de página, que inclui o uso da extensão `{CustomResource}`. Um local para definir **CustomXamlResourceLoader.Current** é o construtor da subclasse [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) gerado para você nos modelos code-behind de App.xaml.
3.  Agora você pode usar as extensões `{CustomResource}` no XAML que seu aplicativo carrega como páginas, ou de dentro de dicionários de recursos XAML.

**CustomResource** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres "\{" e "\}" na sintaxe de atributo, sendo esta a convenção pela qual um processador XAML reconhece que uma extensão de marcação deve processar o atributo.

## <a name="related-topics"></a>Tópicos relacionados

* [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)

