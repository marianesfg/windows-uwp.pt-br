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
ms.sourcegitcommit: f2f4820dd2026f1b47a2b1bf2bc89d7220a79c1a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/22/2018
ms.locfileid: "2787058"
---
# <a name="run-a-background-task-when-your-uwp-app-is-updated"></a>Executar uma tarefa em segundo plano quando o aplicativo UWP é atualizado

Saiba como escrever uma tarefa de plano de fundo executada depois que o seu aplicativo do repositório de plataforma de Windows Universal (UWP) é atualizado.

A tarefa de plano de fundo de atualização de tarefa é invocada pelo sistema operacional depois que o usuário instale uma atualização para um aplicativo que está instalado no dispositivo. Isso permite que o seu aplicativo executar tarefas de inicialização como inicializar um novo canal de notificação de push, atualizando o esquema de banco de dados e assim por diante, antes do usuário inicia o seu aplicativo atualizado.

A tarefa de atualização difere lançar uma tarefa de plano de fundo usando o gatilho [ServicingComplete](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Background.SystemTriggerType) porque neste caso seu aplicativo deve ser executado pelo menos uma vez antes de serem atualizado para registrar a tarefa de plano de fundo que será ativada pelo ** ServicingComplete** gatilho.  A tarefa de atualização não é registrada e assim um aplicativo que nunca tiver sido executado, mas que é atualizado, ainda terá sua tarefa de atualização disparada.

## <a name="step-1-create-the-background-task-class"></a>Etapa 1: Criar a classe de tarefa do plano de fundo

Como com outros tipos de tarefas do plano de fundo, você implementar a tarefa de plano de fundo de tarefa de atualização como um componente de tempo de execução do Windows. Para criar este componente, siga as etapas na seção de **criar a classe de tarefa do plano de fundo** de [criar e registrar uma tarefa de plano de fundo de fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task). As etapas incluem:

- Adicionando um projeto de componente de tempo de execução do Windows à sua solução.
- Criando uma referência a partir de seu aplicativo para o componente.
- Criando uma classe pública, fechada no componente que implementa [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794).
- Implementando o método [**Run**](https://msdn.microsoft.com/library/windows/apps/br224811) , que é o ponto de entrada necessários que é chamado quando a tarefa de atualização é executada. Se você pretende fazer chamadas assíncronas de sua tarefa de plano de fundo, [criar e registrar uma tarefa fora do processo de plano de fundo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task) explica como usar um adiamento em seu método **execute** .

Você não precisa registrar essa tarefa de plano de fundo (seção "Registro a tarefa de plano de fundo para executar" no tópico **Create and registrar uma tarefa fora do processo de plano de fundo** ) para usar a tarefa de atualização. Esse é o principal motivo para usar uma tarefa de atualização porque você não precisa adicionar qualquer código ao seu aplicativo para registrar a tarefa e o aplicativo não precisa ser executado pelo menos uma vez antes que está sendo atualizado para registrar a tarefa de plano de fundo.

O código de exemplo a seguir mostra um ponto de partida básico para uma classe de tarefa do plano de fundo de tarefa de atualização em c#. A classe de tarefa do plano de fundo propriamente dita - e todas as outras classes do projeto de tarefa do plano de fundo - precisam ser **públicos** e **fechada**. Sua classe de tarefa do plano de fundo deve derivar de **IBackgroundTask** e ter um método **Run** público com a assinatura mostrada abaixo:

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

## <a name="step-2-declare-your-background-task-in-the-package-manifest"></a>Etapa 2: Declarar sua tarefa de plano de fundo no manifesto do pacote

No Visual Studio Solution Explorer, clique com o botão **Package.appxmanifest** e clique em **Exibir código** para exibir o manifesto do pacote. Adicione a seguinte `<Extensions>` XML para declarar sua tarefa de atualização:

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

No XML acima, verifique se o `EntryPoint` atributo estiver definido como o nome de namespace.class da sua classe de tarefa de atualização. O nome diferencia maiusculas de minúsculas.

## <a name="step-3-debugtest-your-update-task"></a>Etapa 3: Depuração e teste sua tarefa de atualização

Certifique-se de que você implantou o seu aplicativo em sua máquina, para que haja algo a ser atualizado.

Defina um ponto de interrupção no método Run de sua tarefa de plano de fundo.

![ponto de interrupção do conjunto](images/run-func-breakpoint.png)

Em seguida, no solution explorer, clique com o botão projeto do seu aplicativo (não o projeto de tarefa de plano de fundo) e, em seguida, clique em **Propriedades**. Na janela de propriedades do aplicativo, clique em **Depurar** à esquerda e selecione o **início não, mas depurar meu código quando ele é iniciado**:

![Definir configurações de depuração](images/do-not-launch-but-debug.png)

Em seguida, para garantir que o UpdateTask é acionado, aumente o número de versão do pacote. No Solution Explorer, clique duas vezes no arquivo de **Package.appxmanifest** do seu aplicativo para abrir o designer de pacote e, em seguida, atualize o número de **compilação** :

![atualizar a versão](images/bump-version.png)

Agora, em 2017 do Visual Studio quando você pressiona F5, seu aplicativo será atualizado e o sistema ativará seu componente UpdateTask em segundo plano. O depurador será automaticamente anexado ao processo de plano de fundo. O ponto de interrupção será atingido e você pode percorrer a lógica de código de atualização.

Quando a tarefa de plano de fundo é concluída, você pode iniciar o aplicativo de primeiro plano, no menu Iniciar do Windows dentro da mesma sessão de depuração. O depurador novamente automaticamente será anexado, desta vez para seu processo de primeiro plano, e você pode percorrer a lógica do seu aplicativo.

> [!NOTE]
> Os usuários do Visual Studio 2015: as etapas acima se aplicam ao 2017 do Visual Studio. Se você estiver usando 2015 do Visual Studio, você pode usar as mesmas técnicas para disparar e teste UpdateTask, exceto o Visual Studio não será anexado a ela. Um procedimento alternativo no VS 2015 é um [ApplicationTrigger](https://docs.microsoft.com/windows/uwp/launch-resume/trigger-background-task-from-app) que define o UpdateTask como seu ponto de partida da instalação e disparar a execução diretamente a partir do aplicativo de primeiro plano.

## <a name="see-also"></a>Veja também

[Criar e registrar uma tarefa em segundo plano fora do processo](https://docs.microsoft.com/windows/uwp/launch-resume/create-and-register-a-background-task)
