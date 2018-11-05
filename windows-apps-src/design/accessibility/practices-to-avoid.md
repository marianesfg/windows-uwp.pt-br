---
author: Xansky
Description: Lists the practices to avoid if you want to create an accessible Universal Windows Platform (UWP) app.
ms.assetid: 024A9B70-9821-45BB-93F1-61C0B2ECF53E
title: Práticas de acessibilidade a evitar
label: Accessibility practices to avoid
template: detail.hbs
ms.author: mhopkins
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9505dfc666042c22e6f77ed02ffca7c5973d4fba
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6038493"
---
# <a name="accessibility-practices-to-avoid"></a>Práticas de acessibilidade a evitar

Se você quiser criar um aplicativo acessível da Plataforma Universal do Windows (UWP), consulte esta lista de práticas a serem evitadas: 

* **Evite a criação de elementos de interface do usuário personalizados se for possível usar os controles padrão do Windows** ou os controles que já implementaram o suporte à Automação de Interface do Usuário da Microsoft. Controles padrão do Windows são acessíveis por padrão e costumam precisar da adição de apenas alguns atributos de acessibilidade específicas ao aplicativo. De forma contrária, implementar o suporte [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) para um controle personalizado de verdade está, de certa maneira, mais evolvido (consulte [Pares de automação personalizados](custom-automation-peers.md)).
* **Não coloque texto estático ou outros elementos não interativos na ordem de guia** (por exemplo, definir a propriedade [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461) para um elemento que não é interativo). Se há elementos não interativos na ordem de guia, isso vai contra as diretrizes de acessibilidade do teclado porque diminui a eficiente da navegação do teclado para usuários. Muitas tecnologias assistenciais usam a ordem de tabulação e a capacidade de focar um elemento como parte de sua lógica de como apresentar uma interface do aplicativo para o usuário de tecnologia assistencial. Os elementos somente texto podem confundir os usuários que esperam apenas elementos interativos na ordem de tabulação (botões, caixas de seleção, campos de entrada, caixas de combinação, listas, etc.).
* **Evite usar posicionamento absoluto de elementos de IU** (como em um elemento [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267)) porque a ordem de apresentação costuma ser diferente da ordem de declaração de elemento filho (que, de fato, é a ordem lógica). Sempre que possível, disponha os elementos da interface do usuário em documento ou em ordem lógica para assegurar que os leitores de tela possam ler esses elementos na ordem correta. Se a ordem visível dos elementos de IU podem divergir da ordem do documento ou lógica, use valores de índice de guia explícitos (defina [**TabIndex**](https://msdn.microsoft.com/library/windows/apps/BR209461)) para definir a ordem de leitura correta.
* **Não use cores como a única forma de transmitir informações.** Os usuários que não distinguem cores não podem receber informações transportadas somente através de cores, como em um indicador de status por cor. Inclua outros indicadores visuais, preferencialmente de texto, para assegurar que as informações sejam acessíveis.
* **Não atualize automaticamente uma tela de aplicativo inteira**, a menos que seja realmente necessário para a funcionalidade do aplicativo. Se precisar atualizar o conteúdo da página automaticamente, atualize somente algumas áreas da página. Tecnologias adaptativas costumam supor que uma tela de aplicativo atualizada é uma estrutura completamente nova, mesmo que as alterações efetivas sejam mínimas. O custo disso para o usuário de tecnologia assistencial é que qualquer modo de exibição ou descrição de documento do aplicativo atualizado agora deve ser recriado e apresentado para o usuário novamente.
  
  Uma navegação de página deliberada iniciada pelo usuário é um motivo legítimo para atualizar a estrutura do aplicativo. Mas verifique se o item de interface do usuário que inicia a navegação está identificado ou nomeado corretamente para fornecer alguma indicação que chamá-lo resultará em uma mudança de contexto e no recarregamento da página.

  > [!NOTE]
  > Se você atualizar o conteúdo dentro de uma região, considere definir a propriedade de acessibilidade [**AccessibilityProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516) nesse elemento para uma das configurações não padrão **Polite** or **Assertive**. Algumas tecnologias adaptativas podem mapear essa configuração para o conceito de Accessible Rich Internet Applications (ARIA) de regiões dinâmicas e, portanto, informar ao usuário que uma região de conteúdo foi alterada.

* **Não use elementos da interface do usuário que piscam mais de três vezes por segundo.** Os elementos piscantes podem fazer com que algumas pessoas tenham ataques epilépticos. É melhor evitar o uso de elementos da interface do usuário que piscam.
* **Não mude o contexto do usuário nem ative a funcionalidade automaticamente.** As mudanças de contexto ou de ativação devem ocorrer somente quando o usuário faz uma ação direta sobre um elemento da interface do usuário que tem foco. As mudanças no contexto do usuário incluem mudança de foco, exibição de conteúdo novo e navegação para uma página diferente. Fazer mudanças de contexto sem envolver o usuário pode ser desorientador para os usuários que possuem deficiências. As exceções a essa exigência incluem exibição de submenus, validação de formulários, exibição de texto auxiliar em outro controle e mudança de contexto em resposta a um evento assíncrono.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Acessibilidade na Store](accessibility-in-the-store.md)
* [Lista de verificação de acessibilidade](accessibility-checklist.md)
