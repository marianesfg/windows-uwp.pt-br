---
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Desempenho
description: Os usuários esperam que seus aplicativos mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c13fb0f1b3d982d372896547bf2e47d978532ea4
ms.sourcegitcommit: d38e2f31c47434cd6dbbf8fe8d01c20b98fabf02
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2019
ms.locfileid: "70393440"
---
# <a name="performance"></a>Desempenho


Os usuários esperam que seus aplicativos mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria. Tecnicamente, o desempenho é um requisito não funcional, mas tratar o desempenho como um recurso ajudará você atender às expectativas dos usuários. Especificar metas e mensurar são fatores importantes. Determine quais são os cenários críticos de desempenho; defina o que significa bom desempenho. Em seguida, faça medições o quanto antes e com frequência suficiente ao longo do ciclo de vida do projeto para cumprir suas metas. Esta seção mostra como organizar seu fluxo de trabalho de desempenho, corrigir problemas de taxa de quadros e falhas de animação e ajustar seu tempo de inicialização, tempo de navegação de página e uso de memória.

Se você ainda não fez isso, uma etapa que vimos resultar em melhorias significativas de desempenho é fazer a portabilidade do seu aplicativo para Windows 10. Várias otimizações XAML (por exemplo, [{x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) só estão disponíveis em aplicativos do Windows 10. Confira [Portando aplicativos para o Windows 10](https://docs.microsoft.com/windows/uwp/porting/index) e a sessão //build/ [Migrando para a Plataforma Universal do Windows](https://channel9.msdn.com/Events/Build/2015/3-741).

| Tópico | Descrição |
|-------|-------------|
| [Planejando para o desempenho](planning-and-measuring-performance.md) | Os usuários esperam que seus aplicativos mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria. Tecnicamente, o desempenho é um requisito não funcional, mas tratar o desempenho como um recurso ajudará você atender às expectativas dos usuários. Especificar metas e mensurar são fatores importantes. Determine quais são os cenários críticos de desempenho; defina o que significa bom desempenho. Em seguida, faça medições o quanto antes e com frequência suficiente ao longo do ciclo de vida do projeto para cumprir suas metas. |
| [Otimizar a atividade em segundo plano](optimize-background-activity.md) | Crie aplicativos UWP que funcionem com o sistema para usar tarefas em segundo plano economizando a bateria. |
| [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md) | Melhore o desempenho de [<strong>GridView</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) e de tempo de inicialização por meio de virtualização da interface do usuário, redução de elemento e atualização progressiva de itens. |
| [Virtualização de dados de ListView e GridView](listview-and-gridview-data-optimization.md) | Melhore o desempenho [<strong>GridView</strong>](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.GridView) e de tempo de inicialização por meio de virtualização da interface do usuário. |
| [Melhore o desempenho de coleta de lixo](improve-garbage-collection-performance.md) | Os aplicativos da Plataforma Universal do Windows (UWP) em C# e Visual Basic fazem o gerenciamento de memória automático a partir do coletor de lixo do .NET. Esta seção resume as melhores práticas de comportamento e desempenho para o coletor de lixo .NET em aplicativos UWP. |
| [Mantenha o thread de interface do usuário responsivo](keep-the-ui-thread-responsive.md) | Os usuários esperam que um aplicativo continue respondendo enquanto executa cálculos, independentemente do tipo de computador. Isso significa coisas diferentes para aplicativos diferentes. Para alguns, isso significa oferecer física mais realista, carregar dados do disco ou da Web mais rapidamente, apresentar cenas complexas e navegar entre páginas, encontrar referências de local ou processar dados com mais agilidade. Independentemente do tipo de cálculo, os usuários querem que o aplicativo aja com sua entrada, e instâncias nas quais ele parece não responder enquanto &quot;pensa&quot; sejam eliminadas. |
| [Otimizar sua marcação XAML](optimize-xaml-loading.md) | Analisar a marcação XAML para construir objetos na memória é demorado para uma interface do usuário complexa. Aqui está o que você pode fazer para melhorar a análise de marcação XAML, o tempo de carregamento e a eficiência de memória para seu aplicativo. | 
| [Otimizar o layout XAML](optimize-your-xaml-layout.md) | O layout pode ser uma parte cara de um aplicativo XAML, tanto no uso de CPU quanto na sobrecarga de memória. Aqui estão algumas etapas simples que você pode seguir para melhorar o desempenho de layout do seu aplicativo XAML. | 
| [Dicas de MVVM e desempenho de linguagem](mvvm-performance-tips.md) | Este tópico aborda algumas considerações de desempenho relacionadas à sua escolha de padrões de design de software e linguagem de programação. |
| [Práticas recomendadas para o desempenho inicial de seu aplicativo](best-practices-for-your-app-s-startup-performance.md) | Crie aplicativos UWP com tempos de inicialização ótimos melhorando a maneira como você manipula a inicialização e a ativação. |
| [Otimizar animações, mídia e imagens](optimize-animations-and-media.md) | Crie aplicativos da Plataforma Universal do Windows (UWP) com animações suaves, taxa de quadros elevada e captura e reprodução de mídia de alto desempenho. |
| [Otimizar suspensão/retomada](optimize-suspend-resume.md) | Crie aplicativos UWP que simplificam o uso do sistema de tempo de vida do processo para continuar de forma eficiente após suspensão ou encerramento. |
| [Otimizar o acesso a arquivos](optimize-file-access.md) | Crie aplicativos UWP que acessam o sistema de arquivos com eficiência, evitando problemas de desempenho devido a ciclos de memória/CPU e de latência de disco. |
| [Componentes do Windows Runtime e otimização de interoperabilidade](windows-runtime-components-and-optimizing-interop.md) | Crie aplicativos UWP que usam componentes UWP e interoperabilidade entre tipos nativos e gerenciados, evitando problemas de desempenho de interoperabilidade. |
| [Ferramentas de criação de perfil e de desempenho](tools-for-profiling-and-performance.md) | A Microsoft fornece várias ferramentas para ajudá-lo a melhorar o desempenho do seu aplicativo UWP.|

