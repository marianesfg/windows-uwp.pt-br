---
Description: Dialogs and flyouts display transient UI elements that appear when the user requests them or when something happens that requires notification or approval.
title: Caixas de diálogo e submenus
template: detail.hbs
ms.date: 07/06/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: ad6affd9-a3c0-481f-a237-9a1ecd561be8
pm-contact: yulikl
design-contact: kimsea
dev-contact: niallm
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 2d5635a41bec716487c08dd57e6ba2ac360649ad
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8203831"
---
# <a name="dialogs-and-flyouts"></a>Caixas de diálogo e submenus



Caixas de diálogo e submenus são elementos transitórios da interface do usuário que aparecem quando acontece algo que requer notificação, aprovação ou informações adicionais do usuário.

> **APIs importantes**: [classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [classe de submenu](/uwp/api/Windows.UI.Xaml.Controls.Flyout)


:::row:::
    :::column:::
        **Dialogs**
        
        ![Example of a dialog](../images/dialogs/dialog_RS2_delete_file.png)

        Dialogs are modal UI overlays that provide contextual app information. Dialogs block interactions with the app window until being explicitly dismissed. They often request some kind of action from the user.
    :::column-end:::
    :::column::: 
        **Flyouts**

        ![Example of a flyout](../images/flyout-example2.png)

        A flyout is a lightweight contextual popup that displays UI related to what the user is doing. It includes placement and sizing logic, and can be used to reveal a secondary control or show more detail about an item.

        Unlike a dialog, a flyout can be quickly dismissed by tapping or clicking somewhere outside the flyout, pressing the Escape key or Back button, resizing the app window, or changing the device's orientation.
    :::column-end:::
:::row-end:::


## <a name="is-this-the-right-control"></a>Este é o controle correto?

Caixas de diálogo e submenus certificam-se de que os usuários estão cientes da informações importantes, mas eles também interrompem a experiência do usuário. Como as caixas de diálogo são modais (bloqueando), elas interrompem os usuários, impedindo que façam algo até que interajam com a caixa. Submenus fornecem uma experiência menos desagradável, mas exibir muitos submenus pode ser uma decisão traiçoeira.

Depois de determinar que você deseja usar uma caixa de diálogo ou menu suspenso, você precisa escolher qual usar.

Considerando que as caixas de diálogo bloqueiam interações e os submenus não, as caixas de diálogo devem ser reservadas para situações em que você deseja que o usuário solte tudo para focar em uma informação específica ou responder a uma pergunta. Submenus, por outro lado, podem ser usados quando você deseja chamar a atenção para algo, mas terá problemas se o usuário desejar ignorá-lo.

:::row:::
    :::column:::
   <p><b>Use uma caixa de diálogo para...</b> <br/>
<ul>
<li>Expressar informações importantes que o usuário <b>deve</b> ler e confirmar antes de prosseguir. Os exemplos incluem:
<ul>
  <li>Quando a segurança do usuário pode ser comprometida</li>
  <li>Quando o usuário está prestes a alterar um ativo valioso de forma permanente</li>
  <li>Quando o usuário está prestes a excluir um ativo valioso</li>
  <li>Para confirmar uma compra no aplicativo</li>
</ul>

</li>
<li>Mensagens de erro que se aplicam ao contexto geral do aplicativo, como um erro de conectividade.</li>
<li>Perguntas, quando o aplicativo precisar fazer uma pergunta de bloqueio ao usuário, por exemplo, quando o aplicativo não puder escolher em nome do usuário. Uma pergunta de bloqueio não pode ser ignorada ou adiada e deve oferecer ao usuário opções bem-definidas.</li>
</ul>
</p>
    :::column-end:::
    :::column:::
   <p><b>Use um submenu para...</b> <br/>
<ul>
<li>Coletar informações adicionais necessárias para que uma ação possa ser concluída.</li>
<li>Exibir informações que são relevantes apenas algumas vezes. Por exemplo, em um aplicativo da galeria de fotos, quando o usuário clica em uma miniatura da imagem, você pode usar um submenu para exibir uma versão grande da imagem.</li>
<li>Exibindo mais informações, como detalhes ou descrições mais longas de um item na página.</li>
</ul></p>
    :::column-end:::
:::row-end:::


## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>Formas de evitar o uso de caixas de diálogo e submenus

Considere a importância das informações que você deseja compartilhar: é importante o suficiente para interromper o usuário? Considere também a frequência com que as informações precisam ser exibidas. Se você estiver mostrando uma caixa de diálogo ou notificação a cada poucos minutos, convém alocar espaço para essas informações na interface do usuário principal, em vez disso. Por exemplo, em um cliente de chat, em vez de mostrar um submenu sempre que um amigo entra, você pode exibir uma lista de amigos que estão online no momento e realçar amigos conforme eles entrarem.

As caixas de diálogo são usadas para confirmar uma ação (como excluir um arquivo) antes de executá-la. Se você espera que o usuário execute uma ação específica com frequência, considere fornecer uma maneira para que o usuário desfaça a ação se for um engano, em vez de forçar os usuários a confirmarem a ação toda vez.

## <a name="how-to-create-a-dialog"></a>Como criar uma caixa de diálogo

Consulte o [artigo de caixas de diálogo](dialogs.md). 

## <a name="how-to-create-a-flyout"></a>Como criar um submenu

Consulte o [artigo de submenu](flyouts.md). 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlUIBasics">Obter o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

