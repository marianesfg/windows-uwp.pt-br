---
author: Jwmsft
Description: Use uma lista invertida para adicionar novos itens no final dela.
title: Listas invertidas
label: Inverted lists
template: detail.hbs
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 52c1d63d-69c1-48d6-a234-6f39296e4bfd
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 0a33aaf71dbf23e991591f790f7327d812b060ef
ms.lasthandoff: 02/08/2017

---
# <a name="inverted-lists"></a>Listas invertidas

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Você pode usar uma exibição de lista para apresentar uma conversa de uma experiência de chat com itens que são visualmente distintos para representar o remetente/destinatário.  Usar cores diferentes e o alinhamento horizontal para separar mensagens do remetente/destinatário ajuda o usuário a se orientar rapidamente em uma conversa.
 
Normalmente, você precisa apresentar a lista de modo que ela pareça crescer de baixo para cima, em vez de cima para baixo.  Quando uma nova mensagem chega e é acrescentada ao final, as mensagens anteriores deslizam para cima para liberar espaço chamando a atenção do usuário para a chegada mais recente.  No entanto, se um usuário tiver rolado a tela para cima para ver as respostas anteriores, a chegada de uma nova mensagem não deverá causar uma mudança de visual que possa atrapalhar seu foco.

![app de chat com a lista invertida](images/listview-inverted.png)

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li>[**Classe ListView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx)</li>
<li>[**Classe ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx)</li>
<li>[**Propriedade ItemsUpdatingScrollMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx)</li>
</ul>
</div>


## <a name="create-an-inverted-list"></a>Criar uma lista invertida

Para criar uma lista invertida, use uma exibição de lista com um [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) como seu painel de itens. No ItemsStackPanel, defina o [**ItemsUpdatingScrollMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) como [**KeepLastItemInView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx).

> [!IMPORTANT]
> O valor de enumeração **KeepLastItemInView** está disponível desde o Windows 10, versão 1607. Você não pode usar esse valor quando seu app é executado em versões anteriores do Windows 10.

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

