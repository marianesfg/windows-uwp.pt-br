---
Description: Caixas de diálogo e submenus exibem elementos transitórios da interface do usuário que aparecem quando o usuário os solicita ou quando acontece algo que requer notificação ou aprovação.
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
ms.openlocfilehash: c61d1478c38df315a3fe3c20151de8c2bfbca4e2
ms.sourcegitcommit: 23c5d8dfaeb6edbca780637ffd26fe892db27519
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/11/2020
ms.locfileid: "81123585"
---
# <a name="dialogs-and-flyouts"></a>Caixas de diálogo e submenus

Caixas de diálogo e submenus são elementos transitórios da interface do usuário que aparecem quando acontece algo que requer notificação, aprovação ou informações adicionais do usuário.

> **APIs de plataforma:** [Classe ContentDialog](/uwp/api/Windows.UI.Xaml.Controls.ContentDialog), [classe Flyout](/uwp/api/Windows.UI.Xaml.Controls.Flyout)

**Caixas de diálogo**

![Exemplo de uma caixa de diálogo](../images/dialogs/dialog_RS2_delete_file.png)

Caixas de diálogo são sobreposições de interface do usuário modais que fornecem informações contextuais do aplicativo. As caixas de diálogo bloqueiam interações com a janela do aplicativo até que sejam explicitamente ignoradas. Elas muitas vezes solicitam algum tipo de ação do usuário.

**Submenus**

![Exemplo de um submenu](../images/flyout-example2.png)

Um submenu é um pop-up contextual leve que exibe a interface do usuário relacionada ao que o usuário está fazendo. Inclui a lógica de dimensionamento e posicionamento e, além disso, pode ser usado para exibir um controle secundário ou mostrar mais detalhes sobre um item.

Ao contrário de uma caixa de diálogo, um submenu pode ser ignorado rapidamente tocando ou clicando em algum lugar fora do submenu, pressionando a tecla Escape ou botão Back, redimensionando a janela do aplicativo ou mudando a orientação do dispositivo.

## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Caixas de diálogo e submenus certificam-se de que os usuários estão cientes da informações importantes, mas eles também interrompem a experiência do usuário. Como caixas de diálogo são modais (bloqueando), elas interrompem os usuários, impedindo que façam algo até que interajam com a caixa. Submenus fornecem uma experiência menos desagradável, mas exibir muitos submenus pode ser uma decisão traiçoeira.

Depois de determinar que você deseja usar uma caixa de diálogo ou menu suspenso, você precisa escolher qual usar.

Considerando que as caixas de diálogo bloqueiam interações e os submenus não, as caixas de diálogo devem ser reservadas para situações em que você deseja que o usuário solte tudo para focar em uma informação específica ou responder a uma pergunta. Submenus, por outro lado, podem ser usados quando você deseja chamar a atenção para algo, mas terá problemas se o usuário desejar ignorá-lo.

   <p><b>Use uma caixa de diálogo para...</b> <br/>
<ul>
<li>Expressar informações importantes que o usuário <b>deve</b> ler e confirmar antes de prosseguir. Alguns exemplos:
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


   <p><b>Use um submenu para...</b> <br/>
<ul>
<li>Coletar informações adicionais necessárias para que uma ação possa ser concluída.</li>
<li>Exibir informações que são relevantes apenas algumas vezes. Por exemplo, em um aplicativo da galeria de fotos, quando o usuário clica em uma miniatura da imagem, você pode usar um submenu para exibir uma versão grande da imagem.</li>
<li>Exibindo mais informações, como detalhes ou descrições mais longas de um item na página.</li>
</ul></p>

## <a name="ways-to-avoid-using-dialogs-and-flyouts"></a>Maneiras de evitar o uso de caixas de diálogo e submenus

Considere a importância das informações que você deseja compartilhar: é importante o suficiente para interromper o usuário? Considere também a frequência com que as informações precisam ser exibidas. Se você estiver mostrando uma caixa de diálogo ou notificação a cada poucos minutos, convém alocar espaço para essas informações na interface do usuário principal, em vez disso. Por exemplo, em um cliente de chat, em vez de mostrar um submenu sempre que um amigo entra, você pode exibir uma lista de amigos que estão online no momento e realçar amigos conforme eles entrarem.

As caixas de diálogo são usadas para confirmar uma ação (como excluir um arquivo) antes de executá-la. Se você espera que o usuário execute uma ação específica com frequência, considere fornecer uma maneira para que o usuário desfaça a ação se for um engano, em vez de forçar os usuários a confirmarem a ação toda vez.

## <a name="how-to-create-a-dialog"></a>Como criar uma caixa de diálogo

Consulte o [Artigo de caixas de diálogo](dialogs.md). 

## <a name="how-to-create-a-flyout"></a>Como criar um submenu

Consulte o [Artigo de submenu](flyouts.md). 

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="../images/xaml-controls-gallery-app-icon-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver o aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para abrir o aplicativo para ver o <a href="xamlcontrolsgallery:/item/ContentDialog">ContentDialog</a> ou <a href="xamlcontrolsgallery:/item/Flyout">Flyout</a> em ação.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

