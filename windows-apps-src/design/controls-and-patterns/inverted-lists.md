---
author: muhsinking
Description: Use an inverted list to add new items at the bottom.
title: Listas invertidas
label: Inverted lists
template: detail.hbs
ms.author: mukin
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
pm-contact: predavid
design-contact: kimsea
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: d19d18d55c973644f5c3f99ae3d442e6dbf3a724
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7435283"
---
# <a name="inverted-lists"></a>Listas invertidas

 

Você pode usar uma exibição de lista para apresentar uma conversa de uma experiência de chat com itens que são visualmente distintos para representar o remetente/destinatário.  Usar cores diferentes e o alinhamento horizontal para separar mensagens do remetente/destinatário ajuda o usuário a se orientar rapidamente em uma conversa.

> **APIs importantes**: [classe ListView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx), [classe ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx), [propriedade ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)
 
Normalmente, você precisa apresentar a lista de modo que ela pareça crescer de baixo para cima, em vez de cima para baixo.  Quando uma nova mensagem chega e é acrescentada ao final, as mensagens anteriores deslizam para cima para liberar espaço chamando a atenção do usuário para a chegada mais recente.  No entanto, se um usuário tiver rolado a tela para cima para ver as respostas anteriores, a chegada de uma nova mensagem não deverá causar uma mudança de visual que possa atrapalhar seu foco.

![Aplicativo de chat com a lista invertida](images/listview-inverted.png)

## <a name="create-an-inverted-list"></a>Criar uma lista invertida

Para criar uma lista invertida, use uma exibição de lista com um [ItemsStackPanel](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) como o painel de itens. Em ItemsStackPanel, defina [ItemsUpdatingScrollMode](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) como [KeepLastItemInView](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx).

> [!IMPORTANT]
> O valor de enumeração **KeepLastItemInView** está disponível desde o Windows 10, versão 1607. Você não pode usar esse valor quando seu aplicativo é executado em versões anteriores do Windows 10.

Este exemplo mostra como alinhar itens da exibição de lista com a parte inferior e indicar que, quando houver uma alteração nos itens, o último item deve permanecer na exibição.
 
 **XAML**
 ```xaml
<ListView>
    <ListView.ItemsPanel>
        <ItemsPanelTemplate>
            <ItemsStackPanel VerticalAlignment="Bottom"
                             ItemsUpdatingScrollMode="KeepLastItemInView"/>
        </ItemsPanelTemplate>
    </ListView.ItemsPanel>
</ListView>
```

## <a name="dos-and-donts"></a>O que fazer e o que não fazer

- Alinhe mensagens do remetente/destinatário em lados opostos para esclarecer o fluxo da conversa para os usuários.
- Deixe as mensagens existentes fora do caminho para exibir a mensagem mais recente se o usuário já estiver no final da conversa aguardando a próxima mensagem.
- Não interrompa o foco dos usuários movendo itens se eles não estiverem lendo o final da conversa.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo de lista de baixo para cima XAML](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlBottomUpList)