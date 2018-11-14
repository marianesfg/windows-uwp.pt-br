---
author: PatrickFarley
description: Ferramentas e técnicas para depurar e testar como o aplicativo funciona com o Gerenciamento do Tempo de Vida do Processo.
title: Testando e depurando ferramentas para PLM (Gerenciamento do Tempo de Vida do Processo)
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8ac6d127-3475-4512-896d-80d1e1d66ccd
ms.localizationpriority: medium
ms.openlocfilehash: 92d03ce30443f6efe8b19f4938b35d4040d7ea70
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6666764"
---
# <a name="testing-and-debugging-tools-for-process-lifetime-management-plm"></a>Testando e depurando ferramentas para PLM (Gerenciamento do Tempo de Vida do Processo)

Uma das principais diferenças entre aplicativos UWP e aplicativos da área de trabalho tradicionais é que títulos UWP residem em um contêiner de aplicativo sujeito ao PLM (Gerenciamento do Tempo de Vida do Processo). Os aplicativos UWP podem ser suspensos, retomados ou encerrados em todas as plataformas pelo serviço Agente do Tempo de Execução, e há ferramentas dedicadas a serem usadas para forçar essas transições quando você testa ou depura o código que as manipula.

## <a name="features-in-visual-studio-2015"></a>Recursos no Visual Studio 2015

O depurador interno do Visual Studio 2015 pode ajudar a investigar problemas em potencial durante o uso de recursos UWP exclusivos. Você pode forçar o aplicativo em diferentes estados PLM usando a barra de ferramentas **Eventos de Ciclo de Vida**, que se torna visível quando você executa e depura o título.

![Barra de ferramentas Eventos de Ciclo de Vida](images/gs-debug-uwp-apps-001.png)

## <a name="the-plmdebug-tool"></a>A ferramenta PLMDebug

PLMDebug.exe é uma ferramenta de linha de comando que permite controlar o estado PLM de um pacote de aplicativos e é fornecida como parte do SDK do Windows. Depois de instalada, a ferramenta reside em *C:\Program Files (x86) \Windows Kits\10\Debuggers\x64* por padrão. 

PLMDebug também permite desabilitar o PLM de qualquer pacote de aplicativos instalado, algo necessário para alguns depuradores. A desabilitação do PLM impede que o serviço Agente do Tempo de Execução encerre o aplicativo antes que você tenha uma chance de depurar. Para desabilitar PLM, use a opção **/enableDebug**, seguida do *nome completo do pacote* do aplicativo UWP (o nome curto, o nome da família de pacotes, ou o AUMID de um pacote não funcionará):

```
plmdebug /enableDebug [PackageFullName]
```

Depois de implantar o aplicativo UWP do Visual Studio, o nome completo do pacote é exibido na janela de saída. Você também pode recuperar o nome completo do pacote executando **Get-AppxPackage** em um console do PowerShell.

![Executando Get-AppxPackage](images/gs-debug-uwp-apps-003.png)

Também é possível especificar um caminho absoluto para um depurador que será iniciado automaticamente quando o pacote do aplicativos for ativado. Se quiser fazer isso usando o Visual Studio, você precisará especificar VSJITDebugger.exe como o depurador. No entanto, VSJITDebugger.exe requer que você especifique a opção "-p", além do ID do processo (PID) do aplicativo UWP. Como não é possível saber o PID do aplicativo UWP com antecedência, não é possível usar esse cenário prontamente.

Você pode contornar essa limitação escrevendo um script ou uma ferramenta que identifique o processo do jogo e, em seguida, o shell executa VSJITDebugger.exe, passando o PID do aplicativo UWP. A amostra de código C# a seguir ilustra uma abordagem simples para fazer isso.

```
using System.Diagnostics;

namespace VSJITLauncher
{
    class Program
    {
        static void Main(string[] args)
        {
            // Name of UWP process, which can be retrieved via Task Manager.
            Process[] processes = Process.GetProcessesByName(args[0]);

            // Get PID of most recent instance
            // Note the highest PID is arbitrary. Windows may recycle or wrap the PID at any time.
            int highestId = 0;
            foreach (Process detectedProcess in processes)
            {
                if (detectedProcess.Id > highestId)
                    highestId = detectedProcess.Id;
            }

            // Launch VSJITDebugger.exe, which resides in C:\Windows\System32
            ProcessStartInfo startInfo = new ProcessStartInfo("vsjitdebugger.exe", "-p " + highestId);
            startInfo.UseShellExecute = true;

            Process process = new Process();
            process.StartInfo = startInfo;
            process.Start();
        }
    }
}
```

Uso de exemplo disso com PLMDebug:

```
plmdebug /enableDebug 279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg "\"C:\VSJITLauncher.exe\" Game"
```
em que `Game` é o nome do processo e `279f7062-ce35-40e8-a69f-cc22c08e0bb8_1.0.0.0_x86__c6sq6kwgxxfcg` é o nome completo do pacote do aplicativo UWP de exemplo.

Cada chamada para **/enableDebug** deverá ser acoplada posteriormente a outra chamada PLMDebug com a opção **/disableDebug**. Além disso, o caminho para um depurador deve ser absoluto (caminhos relativos não são compatíveis).

## <a name="related-topics"></a>Tópicos relacionados
- [Implantando e depurando aplicativos UWP](deploying-and-debugging-uwp-apps.md)
- [Depuração, teste e desempenho](index.md)
