---
title: Solução de problemas de compatibilidade de programas no ARM
author: msatranjr
description: Diretrizes para ajustar as configurações de compatibilidade se seu aplicativo não estiver funcionando corretamente no ARM
ms.author: misatran
ms.date: 02/15/2018
ms.topic: article
keywords: windows 10 s, sempre conectado, solução de problemas de compatibilidade, windows no ARM
ms.localizationpriority: medium
ms.openlocfilehash: 4765ad324e90167c7279c9245bccd840bce1163d
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7161775"
---
# <a name="program-compatibility-troubleshooter-on-arm"></a>Solução de problemas de compatibilidade de programas no ARM
Emulação para dar suporte a aplicativos x86 é um novo recurso criado para Windows 10 no ARM64. Às vezes, a emulação executa otimizações que não resultam na melhor experiência. Você pode usar a Solução de problemas de compatibilidade de programas para ativar/desativar as configurações de emulação para seu aplicativo x86, reduzindo as otimizações de padrão e possivelmente aumentando a compatibilidade.

## <a name="start-the-program-compatibility-troubleshooter"></a>Iniciar a Solução de problemas de compatibilidade de programas
Inicie a [Solução de problemas de compatibilidade de programas](https://support.microsoft.com/en-us/help/15078/windows-make-older-programs-compatible) manualmente da mesma maneira em qualquer computador Windows 10: clique com botão direito em um arquivo executável (.exe) e selecione **Solucionar problemas de compatibilidade**. Essa tela aparece.

![Captura de tela da opção Solucionar problemas de compatibilidade](images/arm/Capture4.png)

Se você clicar em **Solucionar problemas de programas**, verá as opções a seguir.

![Captura de tela da opção Solucionar problemas de compatibilidade](images/arm/Capture5.png)

Todas as opções ativam as configurações que são aplicáveis e aplicadas em todos os computadores Windows 10. Além disso, a primeira, segunda e quarta opções aplicam as configurações de emulação [Desativar cache do aplicativo](#disable-app-cache) e [Desativar modo de execução híbrido](#disable-hybrid-exec-mode).

## <a name="toggling-emulation-settings"></a>Alternar configurações de emulação
> [!WARNING]
> Alterar configurações de emulação pode resultar em seu aplicativo inesperadamente falhar ou não iniciar.

Você pode alternar configurações de emulação clicando com o botão direito do mouse no executável e selecionando **Propriedades**.

No ARM, uma seção intitulada **Windows 10 no ARM** estará disponível na guia **Compatibilidade**. Clique em **Alterar configurações de emulação** para iniciar uma segunda janela como esta.

![Captura de tela de Alterar configurações de emulação](images/arm/Capture.png)

Essa janela fornece duas maneiras de modificar as configurações de emulação. Você pode selecionar um grupo de configurações de emulação predefinido ou pode clicar na opção **Usar configurações avançadas** para permitir a escolha de configurações individuais.

As configurações de emulação agrupadas reduzem as otimizações de desempenho em favor de qualidade. Veja a seguir algumas configurações agrupadas que você pode selecionar.

![Captura de tela2 de Alterar configurações de emulação](images/arm/Capture2.png)

Selecione **Usar configurações avançadas** para escolher configurações individuais, conforme descrito nesta tabela.

| Configuração de emulação | Resultado |
| ----------------- | ----------- |
| <p id="disable-app-cache">Desativar cache do aplicativo</p> | O sistema operacional armazenará em cache blocos de código compilados para reduzir a sobrecarga de emulação em execuções subsequentes. Essa configuração requer que o emulador recompile todo o código de aplicativo em tempo de execução. |
| <p id="disable-hybrid-exec-mode">Desativar modo de execução híbrido</p> | Executável Portátil Híbrido Compilado (CHPE), binários são binários x86 compatíveis que incluem código ARM64 nativo para melhorar o desempenho, mas que podem não ser compatíveis com alguns aplicativos. Essa configuração força o uso dos binários somente x86. |
| Suporte estrito de código com modificação automática | Ative para garantir que qualquer código com modificação automática tenha suporte correto em emulação. Os cenários mais comuns de código com modificação automática são cobertos pelo comportamento padrão do emulador. Habilitar essa opção reduz significativamente o desempenho do código com modificação automática durante a execução. |
| Desativar otimização do desempenho de página RWX | Essa otimização melhora o desempenho do código em páginas legíveis, graváveis e executáveis (RWX), mas pode ser incompatível com alguns aplicativos. |

Você também pode selecionar as configurações de vários núcleos, conforme mostrado aqui.

![Captura de tela de configurações de vários núcleos](images/arm/Capture3.png)

Essas configurações alteram o número de barreiras de memória usadas para sincronizar os acessos de memória entre núcleos em aplicativos durante a emulação. **Rápido** é o modo padrão, mas as opções **estrito** e **muito estrito** aumentarão o número de barreiras. O aplicativo fica mais lento, mas reduz o risco de erros de aplicativo. A opção **single-core** remove todas as barreiras, mas força todos os threads de aplicativo a serem executados em um único núcleo.

Se o problema for resolvido ao alterar uma configuração específica, envie um email para *woafeedback@microsoft.com* com detalhes, para que possamos incorporar seus comentários.