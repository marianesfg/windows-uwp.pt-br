---
description: Permite que os usuários facilmente visualizem e definam classificações que refletem o grau de satisfação com o conteúdo e serviços.
title: Controle de classificação
template: detail.hbs
ms.date: 10/25/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: abarlow
design-contact: kimsea
dev-contact: mitra
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: c1da476f55d1b57048eae8a0027a91c4e6ab8b57
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928674"
---
# <a name="rating-control"></a>Controle de classificação

O controle de classificação permite que os usuários facilmente visualizem e definam classificações que refletem o grau de satisfação com o conteúdo e serviços. Os usuários podem interagir com o controle de classificação com toque, caneta, mouse, gamepad e teclado. Diretrizes a seguir mostra como usar os recursos do controle de classificação para oferecem flexibilidade e personalização.

> **APIs importantes**: [classe RatingControl](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.ratingcontrol)

![Exemplo de controle de classificações](images/rating_rs2_doc_ratings_intro.png)

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tem um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RatingControl">abrir o aplicativo e ver o RatingControl em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>Classificação editável com o valor de espaço reservado

Talvez a maneira mais comum de usar o controle de classificações é exibir uma classificação média e ainda permitir que o usuário insira seu próprio valor de classificação. Nesse cenário, o controle de classificações é definido inicialmente para refletir a classificação média de satisfação de todos os usuários de determinado serviço ou tipo de conteúdo (como músicas, vídeos, livros etc.). Ele permanece nesse estado até que um usuário interaja com o controle para classificar um item individualmente. Essa interação altera o estado do controle de classificações para refletir a classificação do usuário em relação à satisfação pessoal.

#### <a name="initial-average-rating-state"></a>Estado inicial de classificação média
![Estado inicial de classificação média](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>Representação da classificação do usuário depois de definida

![Representação da Classificação do usuário depois de definida](images/rating_rs2_doc_movie_user.png)

```XAML
<RatingControl x:Name="MyRating" ValueChanged="RatingChanged"/>
```

```csharp
private void RatingChanged(RatingControl sender, object args)
{
    if (sender.Value == null)
    {
        MyRating.Caption = "(" + SomeWebService.HowManyPreviousRatings() + ")";
    }

    else
    {
        MyRating.Caption = "Your rating";
    }
}
```

### <a name="read-only-rating-mode"></a>Modo de classificação somente leitura

Às vezes, você precisa mostrar classificações de conteúdo secundário, como o exibido no conteúdo recomendado ou ao exibir uma lista de comentários e suas classificações correspondentes. Nesse caso, o usuário não deve ser capaz de editar a classificação, então você pode deixar o controle como somente leitura.
O modo somente leitura também é a maneira recomendada de usar o controle de classificações quando ele é usado em listas virtualizadas muito grandes de conteúdo, por motivos de desempenho e design da interface do usuário.

![Lista longa somente leitura](images/rating_rs2_doc_reviews.png)

Para fazer isso, proceda da seguinte maneira:

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>Funcionalidade adicional

O controle de classificações tem muitos recursos adicionais que podem ser usados. Detalhes para usar esses recursos podem ser encontrados em nossa documentação de referência do MSDN.
Esta é uma lista não abrangente de funcionalidade adicional:
-   Ótimo desempenho da longa lista
-   Dimensionamento compacto para cenários de interface do usuário firmes
-   Preenchimento e classificação de valor contínuo
-   Personalização de espaçamento
-   Desabilitar animações de crescimento
-   Personalização do número de estrelas

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics) - veja todos os controles XAML em um formato interativo.