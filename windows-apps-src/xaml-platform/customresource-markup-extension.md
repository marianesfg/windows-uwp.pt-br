---
author: jwmsft
description: "Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso que vem da implementação personalizada da pesquisa de recursos. A pesquisa de recursos é executada pela implementação da classe CustomXamlResourceLoader."
title: "Extensão de marcação CustomResource"
ms.assetid: 3A59A8DE-E805-4F04-B9D9-A91E053F3642
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 4758f67c7bcbc58fda47faf1e872998302086c10

---

# Extensão de marcação {CustomResource}

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Fornece um valor para qualquer atributo XAML avaliando uma referência a um recurso que vem da implementação personalizada da pesquisa de recursos. A pesquisa de recursos é executada pela implementação da classe [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327).

## Uso do atributo XAML

``` syntax
<object property="{CustomResource key}" .../>
```

## Valores XAML

| Termo | Descrição |
|------|-------------|
| key | A chave para o recurso solicitado. A forma como a chave é inicialmente atribuída é específica da implementação da classe [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) registrada atualmente para uso. |

## Comentários


            **CustomResource** é uma técnica para obtenção de valores definidos em outro lugar em um repositório de recursos personalizado. Esta técnica é relativamente avançada e não é usada pela maioria dos cenários de aplicativos Tempo de Execução do Windows.

A forma como um **CustomResource** é resolvido para um dicionário de recursos não é descrita neste tópico, pois isso pode variar muito dependendo de como [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) é implementado.

O método [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) da implementação de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) é chamado pelo analisador XAML do Windows Runtime sempre que ele encontra um uso de `{CustomResource}` na marcação. O *resourceId* que é passado para **GetResource** vem do argumento *key*, e os outros parâmetros de entrada vêm do contexto, como a propriedade à qual o uso se aplica.

O uso de um `{CustomResource}` não funciona por padrão (a implementação básica do [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340) está incompleta). Para fazer uma referência de `{CustomResource}` válida, você deve executar estas etapas:

1.  Derive uma classe personalizada de [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327) e substitua o método [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340). Não chame a base na implementação.
2.  Defina [**CustomXamlResourceLoader.Current**](https://msdn.microsoft.com/library/windows/apps/br243328) para referência de sua classe na lógica de inicialização. Isso deve acontecer antes de ser carregado qualquer XAML de nível de página, que inclui o uso da extensão `{CustomResource}`. Um local para definir **CustomXamlResourceLoader.Current** é o construtor da subclasse [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) gerado para você nos modelos code-behind de App.xaml.
3.  Agora você pode usar as extensões `{CustomResource}` no XAML que seu aplicativo carrega como páginas, ou de dentro de dicionários de recursos XAML.


            **CustomResource** é uma extensão de marcação. As extensões de marcação geralmente são implementadas quando é necessário efetuar um escape de valores de atributo para que sejam diferentes de valores literais ou nomes de manipulador e o requisito é mais global do que simplesmente colocar conversores de tipo em certos tipos ou propriedades. Todas as extensões de marcação em XAML usam os caracteres "\{" e "\}" na sintaxe de atributo, sendo esta a convenção pela qual um processador XAML reconhece que uma extensão de marcação deve processar o atributo.

## Tópicos relacionados

* [Referências de recursos de ResourceDictionary e XAML](https://msdn.microsoft.com/library/windows/apps/mt187273)
* [**CustomXamlResourceLoader**](https://msdn.microsoft.com/library/windows/apps/br243327)
* [**GetResource**](https://msdn.microsoft.com/library/windows/apps/br243340)




<!--HONumber=Jun16_HO4-->


