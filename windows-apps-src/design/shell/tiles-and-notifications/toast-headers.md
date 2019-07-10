---
Description: Saiba como usar cabeçalhos para agrupar visualmente as notificações do sistema na Central de ações.
title: Cabeçalhos de notificação do sistema
label: Toast headers
template: detail.hbs
ms.date: 12/07/2017
ms.topic: article
keywords: windows 10, uwp, notificação do sistema, cabeçalho, cabeçalhos de notificação do sistema, notificação, notificações de grupo, central de ações
ms.localizationpriority: medium
ms.openlocfilehash: c7d1e3ce0a012d36bea671f87efb8df3a5d49b5f
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67714082"
---
# <a name="toast-headers"></a>Cabeçalhos de notificação do sistema

Você pode agrupar visualmente um conjunto de notificações relacionadas na Central de ações por meio de um cabeçalho de notificação do sistema em suas notificações.

> [!IMPORTANT]
> **Requer a atualização para criadores de área de trabalho e 1.4.0 da biblioteca de notificações**: Você deve estar executando os cabeçalhos da área de trabalho de build 15063 ou superior para ver a notificação do sistema. Você deve usar a versão 1.4.0 ou posterior da [Biblioteca NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para inserir o cabeçalho no conteúdo da notificação do sistema. Há suporte para os cabeçalhos somente na área de trabalho.

Conforme visto abaixo, essa conversa de grupo é unificada em um único cabeçalho, "Acampamentos!!". Cada mensagem na conversa é uma notificação do sistema separada compartilhando o mesmo cabeçalho de notificação do sistema.

<img alt="Toasts with header" src="images/toast-headers-action-center.png" width="396"/>

Você também pode optar por agrupar visualmente as notificações por categoria, como lembretes de voo, rastreamento de pacote e muito mais.

## <a name="add-a-header-to-a-toast"></a>Adicionar um cabeçalho a uma notificação do sistema

Veja como adicionar um cabeçalho a uma notificação do sistema.

> [!NOTE]
> Há suporte para os cabeçalhos somente na área de trabalho. Os dispositivos que não aceitam cabeçalhos simplesmente os ignoram.

```csharp
ToastContent toastContent = new ToastContent()
{
    Header = new ToastHeader()
    {
        Id = "6289",
        Title = "Camping!!",
        Arguments = "action=openConversation&id=6289",
    },

    Visual = new ToastVisual() { ... }
};
```

```xml
<toast>

    <header
        id="6289"
        title="Camping!!"
        arguments="action=openConversation&amp;id=6289"/>

    <visual>
        ...
    </visual>

</toast>
```

Resumindo...

1. Adicione **Header** ao **ToastContent**
2. Atribua as propriedades **Id**, **Title** e **Arguments** necessárias
3. Enviar a notificação ([saiba mais](send-local-toast.md))
4. Em outra notificação, use a mesma **Id** do cabeçalho para unificá-las sob o cabeçalho. A **Id** é a única propriedade usada para determinar se as notificações devem ser agrupadas, ou seja, **Title** e **Arguments** podem ser diferentes. São usadas as propriedades **Title** e **Arguments** da notificação mais recente em um grupo. Se a notificação for removida, **Title** e **Arguments** utilizam a próxima notificação mais recente.


## <a name="handle-activation-from-a-header"></a>Manipular a ativação de um cabeçalho

Os cabeçalhos podem ser clicados pelos usuários para que, ao clicar, o usuário descubra mais sobre o aplicativo.

Portanto, os aplicativos podem oferecer **Arguments** no cabeçalho, de forma semelhante aos argumentos de inicialização na própria notificação.

A ativação é manipulada de modo idêntico à [ativação de notificação do sistema normal](send-local-toast.md#handling-activation-1), ou seja, você pode recuperar esses argumentos no método **OnActivated** de `App.xaml.cs` de forma semelhante a quando o usuário clica no corpo da sua notificação do sistema ou em um botão dela.

```csharp
protected override void OnActivated(IActivatedEventArgs e)
{
    // Handle toast activation
    if (e is ToastNotificationActivatedEventArgs)
    {
        // Arguments specified from the header
        string arguments = (e as ToastNotificationActivatedEventArgs).Argument;
    }
}
```


## <a name="additional-info"></a>Informações adicionais

O cabeçalho separa e agrupa visualmente as notificações. Ele não altera qualquer outra logística sobre a quantidade máxima de notificações que um aplicativo pode ter (20) e o comportamento de primeiro a entrar, primeiro a sair da lista de notificações.

A ordem das notificações em cabeçalhos são da seguinte maneira... Para um determinado aplicativo, a notificação mais recente do aplicativo (e o grupo de todo o cabeçalho se parte de um cabeçalho) aparecerão primeira.

O **Id** pode ser qualquer sequência que você escolher. Não há nenhuma restrição de tamanho ou caracteres em qualquer uma das propriedades em **ToastHeader**. A única restrição é que o conteúdo de notificação do sistema XML inteiro não pode ser maior que 5 KB.

A criação de cabeçalhos não altera o número de notificações mostradas na Central de ações antes que o botão "Veja mais" apareça (esse número é três por padrão e pode ser configurado pelo usuário para cada aplicativo nas Configurações do sistema para notificações).

Clicar no cabeçalho, assim como clicar no nome do aplicativo, não limpa todas as notificações desse cabeçalho (o aplicativo deve usar as APIs para apagar as notificações relevantes).


## <a name="related-topics"></a>Tópicos relacionados

- [Enviar uma ativação de notificação do sistema e o identificador de local](send-local-toast.md)
- [Documentação de conteúdo de notificação do sistema](adaptive-interactive-toasts.md)
