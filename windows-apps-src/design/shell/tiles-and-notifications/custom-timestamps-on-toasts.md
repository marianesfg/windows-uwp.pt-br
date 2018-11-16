---
author: andrewleader
Description: Learn how to use custom timestamps on your toast notifications.
title: Carimbos de data e hora personalizados em notificações do sistema
label: Custom timestamps on toasts
template: detail.hbs
ms.author: mijacobs
ms.date: 12/15/2017
ms.topic: article
keywords: windows 10, uwp, notificação do sistema, carimbo de data e hora personalizado, carimbo de data e hora, notificação, central de ações
ms.localizationpriority: medium
ms.openlocfilehash: 290825fa079052b79fb2feaec8af928f8da93f95
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6851939"
---
# <a name="custom-timestamps-on-toasts"></a>Carimbos de data e hora personalizados em notificações do sistema

Por padrão, o carimbo de data e hora em notificações do sistema (visíveis na Central de Ações) é definido como a hora de envio da notificação.

<img alt="Toast with custom timestamp" src="images/toast-customtimestamp.jpg" width="396"/>

Como opção, você pode substituir o carimbo de data e hora pela sua data e hora personalizada para que o carimbo representa a hora que a mensagem/informações/conteúdo realmente foi criado, em vez da hora de envio da notificação. Isso também garante que as notificações aparecem na ordem correta na Central de Ações (que são organizadas por hora). Recomendamos que a maioria dos aplicativos especifique um carimbo de data e hora personalizado.

> [!IMPORTANT]
> **Requer a Atualização para Criadores e a biblioteca de Notificações 1.4.0**: você deve estar executando o build 15063 ou posterior os carimbos de data e hora personalizados. Você deve usar a versão 1.4.0 ou posterior da [Biblioteca NuGet de notificações do kit de ferramentas da comunidade UWP](https://www.nuget.org/packages/Microsoft.Toolkit.Uwp.Notifications/) para atribuir o carimbo de data e hora ao conteúdo da notificação do sistema.

Para usar um carimbo de hora personalizado, atribua a propriedade **DisplayTimestamp** ao **ToastContent**.

```csharp
ToastContent toastContent = new ToastContent()
{
    DisplayTimestamp = new DateTime(2017, 04, 15, 19, 45, 00, DateTimeKind.Utc),
    ...
};
```

```xml
<toast displayTimestamp="2017-04-15T19:45:00Z">
  ...
</toast>
```

Se você estiver usando XML, a data deve ser formatada em [ISO 8601](https://en.wikipedia.org/wiki/ISO_8601).

> [!NOTE]
> Você pode usar no máximo três decimais nos segundos (embora não exista nenhum valor em fornecer qualquer valor tão granular). Se você fornecer mais, o conteúdo será inválido e você receberá o aviso de "Nova notificação".


## <a name="usage-guidance"></a>Diretriz de uso

Em geral, recomendamos que a maioria dos aplicativos especifique um carimbo de data e hora personalizado. Isso garante que o carimbo de data e hora da notificação representa com precisão quando a mensagem/informações/conteúdo da mensagem foi gerado, independentemente de atrasos da rede, do modo avião ou do intervalo fixo de tarefas periódica em segundo plano.

Por exemplo, um aplicativo de notícias pode executar uma tarefa em segundo plano a cada 15 minutos para verificar se há novos artigos e exibe as notificações. Antes de carimbos de data e hora personalizado, ele correspondeu a quando a notificação do sistema foi gerada (portanto, sempre em intervalos de 15 minutos). No entanto, agora o aplicativo pode definir o carimbo de data e hora para a hora de publicação do artigo. Da mesma forma, os aplicativos de email e redes sociais podem se beneficiar desse recurso se um padrão semelhante for usado para suas notificações.

Além disso, fornecer um carimbo personalizado garante que o carimbo está correto, mesmo se o usuário foi desconectado da Internet. Por exemplo, quando o usuário ativa seu computador e a tarefa em segundo plano é executada, você finalmente pode garantir que o carimbo de data e hora em suas notificações representa a hora em que as mensagens foram enviadas, em vez da hora em que o usuário ligou o computador.


## <a name="default-timestamp"></a>Carimbo de data e hora padrão

Se você não fornecer um carimbo personalizado, usamos a hora de envio da notificação.

Se você enviou uma notificação por push pelo WNS, usamos a hora quando a notificação é recebida pelo servidor do WNS (portanto, qualquer latência em oferecer a notificação para o dispositivo não afeta o carimbo de data e hora).

Se você enviou uma notificação local, usamos a hora em que a plataforma de notificação recebeu a notificação (que deve ser imediatamente).


## <a name="related-topics"></a>Tópicos relacionados

- [Enviar uma notificação do sistema local](send-local-toast.md)
- [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)