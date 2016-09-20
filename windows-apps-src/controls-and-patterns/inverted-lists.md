---
author: Jwmsft
Description: Use uma lista invertida para adicionar novos itens no final dela.
title: Listas invertidas
label: Inverted lists
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: eb6744968a4bf06a3766c45b73b428ad690edc06
ms.openlocfilehash: c70cafe4d1dd3db46d48e9844ba9086dbba9acaa

---
# Listas invertidas

<link rel="stylesheet" href="https://az835927.vo.msecnd.net/sites/uwp/Resources/css/custom.css"> 

Você pode usar uma exibição de lista para apresentar uma conversa de uma experiência de chat com itens que são visualmente distintos para representar o remetente/destinatário.  Usar cores diferentes e o alinhamento horizontal para separar mensagens do remetente/destinatário ajuda o usuário a se orientar rapidamente em uma conversa.
 
Normalmente, você precisa apresentar a lista de modo que ela pareça crescer de baixo para cima, em vez de cima para baixo.  Quando uma nova mensagem chega e é acrescentada ao final, as mensagens anteriores deslizam para cima para liberar espaço chamando a atenção do usuário para a chegada mais recente.  No entanto, se um usuário tiver rolado a tela para cima para ver as respostas anteriores, a chegada de uma nova mensagem não deverá causar uma mudança de visual que possa atrapalhar seu foco.

![aplicativo de chat com a lista invertida](images/listview-inverted.png)

<div class="important-apis" >
<b>APIs importantes</b><br/>
<ul>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.listview.aspx"><strong>Classe ListView</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx"><strong>Classe ItemsStackPanel</strong></a></li>
<li><a href="https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx"><strong>Propriedade ItemsUpdatingScrollMode</strong></a></li>
</ul>

</div>
</div>






## Criar uma lista invertida

Para criar uma lista invertida, use uma exibição de lista com um [**ItemsStackPanel**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.aspx) como seu painel de itens. No ItemsStackPanel, defina o [**ItemsUpdatingScrollMode**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsstackpanel.itemsupdatingscrollmode.aspx) para [**KeepLastItemInView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.itemsupdatingscrollmode.aspx).

> **Importante**&nbsp;&nbsp;O valor de enumeração **KeepLastItemInView** está disponível a partir do Windows 10, versão 1607. Você não pode usar esse valor quando seu aplicativo é executado em versões anteriores do Windows 10.

Este exemplo mostra como alinhar itens da exibição de lista com a parte inferior e indicar que, quando houver uma alteração nos itens, o último item deverá permanecer na exibição.
 
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

## Recomendações

- Alinhe as mensagens do remetente/destinatário em lados opostos para tornar o fluxo da conversa claro para os usuários.
- Deixe as mensagens existentes fora do caminho para exibir a mensagem mais recente se o usuário já estiver no final da conversa aguardando a próxima mensagem.
- Não interrompa o foco dos usuários movendo itens se eles não estiverem lendo o final da conversa.



<!--HONumber=Aug16_HO3-->


