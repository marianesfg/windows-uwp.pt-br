---
author: mcleblanc
ms.assetid: 64F7FC51-E8AC-4098-9C5F-0172E4724B5C
title: Desempenho
description: "Os usuários esperam que seus apps mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria."
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 38a78b8af1555bdb4409c967bd27e5967b5c40aa
ms.lasthandoff: 02/07/2017

---
# <a name="performance"></a>Desempenho

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Os usuários esperam que seus apps mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria. Tecnicamente, o desempenho é um requisito não funcional, mas tratar o desempenho como um recurso ajudará você atender às expectativas dos usuários. Especificar metas e mensurar são fatores importantes. Determine quais são os cenários críticos de desempenho; defina o que significa bom desempenho. Em seguida, faça medições o quanto antes e com frequência suficiente ao longo do ciclo de vida do projeto para cumprir suas metas. Esta seção mostra como organizar seu fluxo de trabalho de desempenho, corrigir problemas de taxa de quadros e falhas de animação e ajustar seu tempo de inicialização, tempo de navegação de página e uso de memória.

Se você ainda não fez isso, uma etapa que vimos resultar em melhorias significativas de desempenho é fazer a portabilidade do seu app para Windows 10. Várias otimizações XAML (por exemplo, [{x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783)) só estão disponíveis em apps do Windows 10. Consulte [Portando apps para o Windows 10](https://msdn.microsoft.com/library/windows/apps/Mt238321) e a sessão //build/ [Migrando para a Plataforma Universal do Windows](http://channel9.msdn.com/Events/Build/2015/3-741).

| Tópico | Descrição |
|-------|-------------|
| [Planejando para o desempenho](planning-and-measuring-performance.md) | Os usuários esperam que seus apps mantenham a capacidade de resposta e naturalidade no uso e não consumam muita bateria. Tecnicamente, o desempenho é um requisito não funcional, mas tratar o desempenho como um recurso ajudará você atender às expectativas dos usuários. Especificar metas e mensurar são fatores importantes. Determine quais são os cenários críticos de desempenho; defina o que significa bom desempenho. Em seguida, faça medições o quanto antes e com frequência suficiente ao longo do ciclo de vida do projeto para cumprir suas metas. |
| [Otimizar a atividade em segundo plano](optimize-background-activity.md) | Crie apps UWP que funcionem com o sistema para usar tarefas em segundo plano economizando a bateria. |
| [Otimização das interfaces do usuário ListView e GridView](optimize-gridview-and-listview.md) | Melhore o desempenho de [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) e de tempo de inicialização por meio de virtualização da interface do usuário, redução de elemento e atualização progressiva de itens. |
| [Virtualização de dados de ListView e GridView](listview-and-gridview-data-optimization.md) | Melhore o desempenho [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/BR242705) e de tempo de inicialização por meio de virtualização da interface do usuário. |
| [Melhorar desempenho de coleta de lixo](improve-garbage-collection-performance.md) | Aplicativos da Plataforma Universal do Windows (UWP) escritos em C# e Visual Basic obtém gerenciamento de memória automático do coletor de lixo .NET. Esta seção resume as melhores práticas de comportamento e desempenho para o coletor de lixo .NET em apps UWP. |
| [Manter o thread de interface do usuário responsivo](keep-the-ui-thread-responsive.md) | Os usuários esperam que um app continue respondendo enquanto executa cálculos, independentemente do tipo de computador. Isso significa coisas diferentes para apps diferentes. Para alguns, isso significa oferecer física mais realista, carregar dados do disco ou da Web mais rapidamente, apresentar cenas complexas e navegar entre páginas, encontrar referências de local ou processar dados com mais agilidade. Independentemente do tipo de cálculo, os usuários querem que o app aja com sua entrada, e instâncias nas quais ele parece não responder enquanto &quot;pensa&quot; sejam eliminadas. |
| [Otimizar sua marcação XAML](optimize-xaml-loading.md) | Analisar a marcação XAML para construir objetos na memória é demorado para uma interface do usuário complexa. Aqui está o que você pode fazer para melhorar a análise de marcação XAML, o tempo de carregamento e a eficiência de memória para seu app. | 
| [Otimizar o layout XAML](optimize-your-xaml-layout.md) | O layout pode ser uma parte cara de um app XAML, tanto no uso de CPU quanto na sobrecarga de memória. Aqui estão algumas etapas simples que você pode seguir para melhorar o desempenho de layout do seu app XAML. | 
| [MVVM e dicas de desempenho de linguagem](mvvm-performance-tips.md) | Este tópico discute algumas considerações de desempenho relacionadas à sua escolha de padrões de design de software e linguagem de programação. |
| [Práticas recomendadas para o desempenho inicial de seu app](best-practices-for-your-app-s-startup-performance.md) | Crie apps UWP com tempos de inicialização ótimos melhorando a maneira como você manipula a inicialização e a ativação. |
| [Otimizar animações, mídia e imagens](optimize-animations-and-media.md) | Crie apps da Plataforma Universal do Windows (UWP) com animações suaves, taxa de quadros elevada e captura e reprodução de mídia de alto desempenho. |
| [Otimizar suspensão/retomada](optimize-suspend-resume.md) | Crie apps UWP que simplificam o uso do sistema de tempo de vida do processo para continuar de forma eficiente após suspensão ou encerramento. |
| [Otimizar o acesso a arquivos](optimize-file-access.md) | Crie apps UWP que acessam o sistema de arquivos com eficiência, evitando problemas de desempenho devido a ciclos de memória/CPU e de latência de disco. |
| [Componentes do Tempo de Execução do Windows e otimização de interoperabilidade](windows-runtime-components-and-optimizing-interop.md) | Crie apps UWP que usam componentes UWP e interoperabilidade entre tipos nativos e gerenciados, evitando problemas de desempenho de interoperabilidade. |
| [Ferramentas para criação de perfil e desempenho](tools-for-profiling-and-performance.md) | A Microsoft fornece várias ferramentas para ajudá-lo a melhorar o desempenho do seu app UWP.|


