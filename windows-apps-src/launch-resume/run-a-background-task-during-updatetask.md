---
title: Executar uma tarefa em segundo plano quando o aplicativo UWP é atualizado
description: Saiba como criar uma tarefa em segundo plano que é executada quando seu aplicativo da loja da UWP (Plataforma Universal do Windows) é atualizado.
ms.date: 04/21/2017
ms.topic: article
keywords: Windows 10, uwp, update, tarefa em segundo plano, updatetask, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 8cd7d4494340d1c5e617361f2e3d750b35ebabb9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57603521"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Executar uma tarefa em segundo plano quando o aplicativo UWP é atualizado

Saiba como criar uma tarefa em segundo plano que é executada depois que seu aplicativo da loja da UWP (Plataforma Universal do Windows) é atualizado.

A Tarefa de atualização em segundo plano é chamada pelo sistema operacional depois que o usuário instala uma atualização de um aplicativo instalado no dispositivo. Isso permite ao aplicativo executar tarefas de inicialização, como inicializar um novo canal de notificação por push, atualizar o esquema do banco de dados etc., sem precisar que o usuário inicialize o aplicativo atualizado.

A Tarefa de atualização é diferente de iniciar uma tarefa em segundo plano usando o gatilho [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType), pois nesse caso o aplicativo deve ser executado pelo menos uma vez antes de ser atualizado para registrar a tarefa em segundo plano que será ativada pelo gatilho **ServicingComplete**.  A Tarefa de atualização não é registrada e um aplicativo que nunca foi executado, mas que foi atualizado, ainda terá sua tarefa de atualização disparada.

## <a name="step-1-create-the-background-task-class"></a>Etapa 1: Criar a classe de tarefa em segundo plano

Assim como em outros tipos de tarefas em segundo plano, você implementa a Tarefa de atualização em segundo plano como um componente do Tempo de Execução do Windows. Para criar esse componente, siga as etapas na seção **Criar a classe de Tarefa em segundo plano** de [Criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). As etapas incluem:

- Adicionar um projeto do Componente do Tempo de Execução do Windows para sua solução.
- Criar uma referência do aplicativo para o componente.
- Criação de uma classe público e selada no componente que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementação do método [**Executar**](https://msdn.microsoft.com/library/windows/apps/br224811), que é o ponto de entrada obrigatório chamado quando a Tarefa de atualização é executada. Se você pretende fazer chamadas assíncronas de sua tarefa em segundo plano, [Criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explica como usar um adiamento no método **Executar**.

Você não precisa registrar essa tarefa em segundo plano (a seção "Registrar a tarefa em segundo plano para execução" no tópico **Criar e registrar uma tarefa em segundo plano fora do processo**) para usar a tarefa de atualização. Isso é o principal motivo para usar uma tarefa de atualização, pois você não precisa adicionar nenhum código ao aplicativo para registrar a tarefa e o aplicativo não precisa ser executado pelo menos uma vez antes de ser atualizado a fim de registrar a tarefa em segundo plano.

O seguinte código de exemplo mostra um ponto inicial básico para uma classe de Tarefa de atualização em segundo plano em C#: A classe da tarefa em segundo plano em si, bem como todas as outras classes no projeto da tarefa em segundo plano, precisam ser de classes **públicas** e **seladas**. A classe de tarefa em segundo plano deve derivar de **IBackgroundTask** e ter um método **Executar ()** público com a assinatura mostrada abaixo:

```cs
using Windows.ApplicationModel.Background;

namespace BackgroundTasks
{
    public sealed class UpdateTask : IBackgroundTask
    {
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // your app migration/update code here
        }
    }
}
```

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Etapa 2: Declarar sua tarefa em segundo plano no manifesto do pacote

No Gerenciador de Soluções do Visual Studio, clique com o botão direito no arquivo **Package.appxmanifest** e clique em **Exibir código** para exibir o manifesto do pacote. Adicione o seguinte `<Extensions>` XML para declarar a tarefa de atualização:

```XML
<Package ...>
    ...
  <Applications>  
    <Application ...>  
        ...
      <Extensions>  
        <Extension Category="windows.updateTask"  EntryPoint="BackgroundTasks.UpdateTask">  
        </Extension>  
      </Extensions>

    </Application>  
  </Applications>  
</Package>
```

No XML, certifique-se de que o `EntryPoint` atributo está definido como o nome de namespace.class da classe de tarefa de atualização. O nome diferencia maiúsculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Etapa 3: Depuração e teste sua tarefa de atualização

Certifique-se de que você implantou o aplicativo em seu computador para que exista algo para atualizar.

Defina um ponto de interrupção no método Executar() da tarefa em segundo plano.

![definir ponto de interrupção](images/run-func-breakpoint.png)

Em seguida, no explorador da solução, clique com botão direito no projeto do aplicativo (não no projeto de tarefa em segundo plano) e, em seguida, clique em **Propriedades**. Na janela Propriedades do aplicativo, clique em **Depurar** à esquerda e selecione **Não iniciar, mas depurar meu código quando for iniciado**:

![definir configurações de depuração](images/do-not-launch-but-debug.png)

Em seguida, para garantir que o UpdateTask é acionado, aumente o número de versão do pacote. No Explorador da solução, clique duas vezes no arquivo **Package. appxmanifest** do aplicativo para abrir o designer de pacote e atualize número da **Versão**:

![atualizar a versão](images/bump-version.png)

Agora, no Visual Studio 2017, ao pressionar F5, o aplicativo será atualizado e o sistema ativará o componente UpdateTask em segundo plano. O depurador será automaticamente anexado ao processo em segundo plano. O ponto de interrupção será atingido e você pode percorrer a lógica de atualização de código.

Quando a tarefa em segundo plano é concluída, você pode iniciar o aplicativo em primeiro plano a partir do menu Iniciar do Windows na mesma sessão de depuração. O depurador será anexado automaticamente, desta vez para o processo em primeiro plano, e você pode percorrer a lógica do aplicativo.

> [!NOTE]
> Usuários do Visual Studio 2015: As etapas acima se aplicam ao Visual Studio 2017. Se você estiver usando o Visual Studio 2015, é possível usar as mesmas técnicas para acionar e testar UpdateTask, mas o Visual Studio não será anexado a elas. Um procedimento alternativo no VS 2015 é configurar um [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que define o UpdateTask como o Ponto de entrada e acionar a execução diretamente do aplicativo em primeiro plano.

## <a name="see-also"></a>Consulte também

[Criar e registrar uma tarefa em segundo plano do out-of-process](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
