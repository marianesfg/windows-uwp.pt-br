---
description: Permite que os usuários facilmente exibam e definam classificações que refletem o grau de satisfação com o conteúdo e os serviços.
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
ms.openlocfilehash: d242107789f7c7575b5796c30d371dbe8bb0a412
ms.sourcegitcommit: 0dee502484df798a0595ac1fe7fb7d0f5a982821
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/08/2020
ms.locfileid: "82970791"
---
# <a name="rating-control"></a>Controle de classificação

O controle de classificação permite que os usuários facilmente exibam e definam classificações que refletem o grau de satisfação com o conteúdo e os serviços. Os usuários podem interagir com o controle de classificação com toque, caneta, mouse, gamepad e teclado. As diretrizes a seguir mostram como usar os recursos do controle de classificação para oferecer flexibilidade e personalização.

![Exemplo de controle de classificação](images/rating_rs2_doc_ratings_intro.png)

**Obter a biblioteca de interface do usuário do Windows**

|  |  |
| - | - |
| ![Logotipo do WinUI](images/winui-logo-64x64.png) | O controle **RatingControl** está incluído como parte da Biblioteca de interface do usuário do Windows, um pacote NuGet que contém novos controles e recursos de interface do usuário para aplicativos do Windows. Para obter mais informações, incluindo instruções de instalação, confira [Biblioteca de interface do usuário do Windows](https://docs.microsoft.com/uwp/toolkits/winui/). |

> **APIs da Biblioteca de interface do usuário do Windows:** [Classe RatingControl](/uwp/api/microsoft.ui.xaml.controls.ratingcontrol)
>
> **APIs de plataforma:** [Classe RatingControl](/uwp/api/windows.ui.xaml.controls.ratingcontrol)

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/RatingControl">abrir o aplicativo e ver o RatingControl em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

### <a name="editable-rating-with-placeholder-value"></a>Classificação editável com o valor de espaço reservado

Talvez a maneira mais comum de usar o controle de classificação seja exibir uma classificação média e ainda permitir que o usuário insira seu próprio valor de classificação. Nesse cenário, o controle de classificação é definido inicialmente para refletir a classificação média de satisfação de todos os usuários de determinado serviço ou tipo de conteúdo (como músicas, vídeos, livros etc.). Ele permanece nesse estado até que um usuário interaja com o controle para classificar um item individualmente. Essa interação altera o estado do controle de classificações para refletir a classificação do usuário em relação à satisfação pessoal.

#### <a name="initial-average-rating-state"></a>Estado inicial de classificação média
![Estado Inicial de Classificação Média](images/rating_rs2_doc_movie_aggregate.png)

#### <a name="representation-of-user-rating-once-set"></a>Representação da classificação do usuário depois de definida

![Representação da Classificação do Usuário Depois de Definida](images/rating_rs2_doc_movie_user.png)

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
O modo somente leitura também é a maneira recomendada de usar o controle de classificações quando ele é usado em listas de conteúdo virtualizadas muito grandes, por motivos de desempenho e design da interface do usuário.

![Lista longa somente leitura](images/rating_rs2_doc_reviews.png)

Para fazer isso, proceda da seguinte maneira:

```XAML
<RatingControl IsReadOnly="True"/>
```

## <a name="additional-functionality"></a>Funcionalidade adicional

O controle de classificações tem muitos recursos adicionais que podem ser usados. Detalhes para usar esses recursos podem ser encontrados em nossa documentação de referência.
Esta é uma lista não abrangente de funcionalidade adicional:
-   Ótimo desempenho da longa lista
-   Dimensionamento compacto para cenários de interface do usuário restritos
-   Preenchimento e classificação de valor contínuo
-   Personalização de espaçamento
-   Desabilitar animações de crescimento
-   Personalização do número de estrelas

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery) - veja todos os controles XAML em um formato interativo.
