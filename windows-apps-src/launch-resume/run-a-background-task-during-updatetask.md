---
author: TylerMSFT
title: Executar uma tarefa em segundo plano quando o aplicativo UWP é atualizado
description: Saiba como criar uma tarefa em segundo plano que é executada quando seu aplicativo da loja da UWP (Plataforma Universal do Windows) é atualizado.
ms.author: twhitney
ms.date: 04/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, atualização, tarefa em segundo plano, updatetask, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: fcba2cb736f86cebc6d2664e2ec3b557d47c86d7
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3379545"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Executar uma tarefa em segundo plano quando o aplicativo UWP é atualizado

Saiba como escrever uma tarefa em segundo plano que é executada depois que o aplicativo de loja Universal Windows Platform (UWP) é atualizado.

A tarefa em segundo plano de tarefa de atualização é invocada pelo sistema operacional depois que o usuário instala uma atualização de um aplicativo que está instalado no dispositivo. Isso permite que seu aplicativo executar tarefas de inicialização como inicializar um novo canal de notificação por push, atualizar o esquema do banco de dados e assim por diante, antes do usuário inicia o aplicativo atualizado.

A tarefa de atualização difere iniciar uma tarefa em segundo plano usando o gatilho [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) porque nesse caso, seu aplicativo deve executar pelo menos uma vez antes de ser atualizado para registrar a tarefa em segundo plano que será ativada pelo ** ServicingComplete** gatilho.  A tarefa de atualização não é registrada e então um aplicativo que nunca foi executado, mas que é atualizado, ainda terão sua tarefa de atualização disparada.

## <a name="step-1-create-the-background-task-class"></a>Etapa 1: Criar a classe de tarefa em segundo plano

Como com outros tipos de tarefas em segundo plano, você implementa a tarefa em segundo plano de tarefa de atualização como um componente do tempo de execução do Windows. Para criar esse componente, siga as etapas na seção **criar a classe de tarefa em segundo plano** de [criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). As etapas incluem:

- Adicionar um projeto de componente de tempo de execução do Windows à sua solução.
- Criar uma referência de seu aplicativo para o componente.
- Criar uma classe pública selada no componente que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementar o método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , que é o ponto de entrada obrigatório que é chamado quando a tarefa de atualização é executada. Se você pretende fazer chamadas assíncronas de sua tarefa em segundo plano, [criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explica como usar um adiamento em seu método **Run** .

Você não precisará registrar essa tarefa em segundo plano (a "Registrar a tarefa em segundo plano seja executada" seção no tópico **criar e registrar uma tarefa em segundo plano fora do processo** ) para usar a tarefa de atualização. Isso é o principal motivo para usar uma tarefa de atualização porque você não precisa adicionar nenhum código ao seu aplicativo para registrar a tarefa e o aplicativo não precisa executar pelo menos uma vez antes de ser atualizado para registrar a tarefa em segundo plano.

O exemplo de código a seguir mostra um ponto de partida básico para uma classe de tarefa de plano de fundo de tarefa de atualização em c#. A classe de tarefa em segundo plano em si e todas as outras classes no projeto de tarefa em segundo plano precisam ser **públicas** e **seladas**. Sua classe de tarefa em segundo plano deve derivar de **IBackgroundTask** e ter um método **Run ()** público com a assinatura mostrada abaixo:

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Etapa 2: Declare sua tarefa em segundo plano no manifesto do pacote

No Visual Studio Solution Explorer, clique com botão direito **Package. appxmanifest** e clique em **Modo de exibição de código** para exibir o manifesto do pacote. Adicione o seguinte `<Extensions>` XML declarar sua tarefa de atualização:

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

O XML acima, garante que o `EntryPoint` atributo é definido como o nome de namespace.class de sua classe de tarefa de atualização. O nome diferencia maiusculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Etapa 3: Depuração/teste sua tarefa de atualização

Certifique-se de que você implantou o aplicativo à sua máquina para que haja algo para atualizar.

Defina um ponto de interrupção no método Run () de sua tarefa em segundo plano.

![conjunto de ponto de interrupção](images/run-func-breakpoint.png)

Em seguida, no Gerenciador de soluções, clique com botão direito projeto do seu aplicativo (não o projeto de tarefa em segundo plano) e, em seguida, clique em **Propriedades**. Na janela de propriedades do aplicativo, clique em **Depurar** no lado esquerdo, selecione **não iniciar, mas sim depurar meu código quando iniciar**:

![Definir configurações de depuração](images/do-not-launch-but-debug.png)

Em seguida, para garantir que o UpdateTask é acionado, aumente o número de versão do pacote. No Gerenciador de soluções, clique duas vezes no arquivo **Package. appxmanifest** de seu aplicativo para abrir o designer de pacote e, em seguida, atualize o número de **compilação** :

![atualizar a versão](images/bump-version.png)

Agora, no Visual Studio 2017 quando você pressionar F5, seu aplicativo será atualizado e o sistema ativará o componente UpdateTask em segundo plano. O depurador será automaticamente anexado ao processo em segundo plano. O ponto de interrupção será atingido e você pode percorrer a lógica de código de atualização.

Quando a tarefa em segundo plano for concluída, você pode iniciar o aplicativo em primeiro plano do menu Iniciar Windows dentro da mesma sessão de depuração. O depurador será automaticamente anexado, desta vez para seu processo em primeiro plano, e você pode percorrer a lógica do seu aplicativo.

> [!NOTE]
> Usuários do Visual Studio 2015: as etapas acima se aplicam ao Visual Studio 2017. Se você estiver usando o Visual Studio 2015, você pode usar as mesmas técnicas gatilho e teste o UpdateTask, exceto o Visual Studio não será anexado a ele. Um procedimento alternativo no VS 2015 é um [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que define o UpdateTask como seu ponto de entrada de instalação e acionar a execução diretamente do aplicativo em primeiro plano.

## <a name="see-also"></a>Veja também

[Criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
