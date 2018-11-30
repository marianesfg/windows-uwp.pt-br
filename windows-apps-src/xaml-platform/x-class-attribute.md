---
description: Configura a compilação XAML para associar classes parciais entre marcação e code-behind. A classe parcial de código é definida em um arquivo de código separado. Já a classe parcial de marcação é criada pela geração do código durante a compilação XAML.
title: Atributo xClass
ms.assetid: 40A7C036-133A-44DF-9D11-0D39232C948F
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8eb1238499355cf37b3f5113dbb10c456de55961
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8206391"
---
# <a name="xclass-attribute"></a>Atributo x:Class


Configura a compilação XAML para associar classes parciais entre marcação e code-behind. A classe parcial de código é definida em um arquivo de código separado. Já a classe parcial de marcação é criada pela geração do código durante a compilação XAML.

## <a name="xaml-attribute-usage"></a>Uso do atributo XAML


``` syntax
<object x:Class="namespace.classname"...>
  ...
</object>
```

## <a name="xaml-values"></a>Valores XAML

| Termo | Descrição |
|------|-------------|
| namespace | Opcional. Especifica um namespace que contém a classe parcial identificada por _classname_. Se _namespace_ for especificado, um ponto (.) separa _namespace_ e _classname_. Se _namespace_ é omitido, considera-se que _classname_ não tem namespace. |
| classname | Obrigatório. Especifica o nome da classe parcial que conecta o XAML carregado e o code-behind desse XAML. | 

## <a name="remarks"></a>Comentários

**x:Class** pode ser declarado como um atributo para qualquer elemento que seja raiz de um arquivo/árvore de objetos XAML e esteja sendo compilado por ações de compilação, ou para a raiz [**Application**](https://msdn.microsoft.com/library/windows/apps/br242324) na definição de um aplicativo compilado. A declaração **x:Class** em qualquer elemento que não seja um nó raiz e, sob qualquer circunstância, para um arquivo XAML não compilado com a ação de compilação **Page** gerará um erro de tempo de compilação.

A classe usada como **x:Class** não pode ser aninhada.

O valor do atributo **x:Class** deve ser uma cadeia de caracteres que especifica o nome totalmente qualificado de uma classe. Você poderá omitir informações de namespace contanto que o code-behind também seja estruturado desse modo (sua definição de classe começa no nível de classe). O arquivo code-behind de uma definição de página ou de aplicativo deve estar em um arquivo de código incluído como parte do projeto. A classe code-behind deve ser pública. A classe de code-behind deve ser parcial.

## <a name="clr-language-rules"></a>Regras da linguagem CLR

Embora seu arquivo code-behind possa ser um arquivo em C++, há certas convenções que ainda seguem a forma de linguagem CLR, para que não haja qualquer diferença na sintaxe XAML. Em particular, o separador entre os componentes namespace e classname de um valor **x:Class** é sempre um ponto ("."), mesmo que o separador entre namespace e classname do arquivo de código C++ associado ao XAML seja "::". Caso você declare namespaces aninhados em C++, o separador entre as cadeias de caracteres de namespaces aninhados sucessivos também deve ser "." em vez de "::" quando especifica a parte *namespace* do valor **x:Class**.

