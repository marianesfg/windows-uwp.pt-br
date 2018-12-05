---
Description: Describes the requirements for declaring your Universal Windows Platform (UWP) app as accessible in the Microsoft Store.
ms.assetid: 59FA3B87-75A6-4B30-BA7C-A0E769D68050
title: Acessibilidade na Store
label: Accessibility in the Store
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 6a9991cd4a0a3fce630b1c7be64650c79daf74e6
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8739728"
---
# <a name="accessibility-in-the-store"></a>Acessibilidade na Store  



Descreve os requisitos para declarar o seu aplicativo da Plataforma Universal do Windows (UWP) como acessível na Microsoft Store.

Ao enviar o seu aplicativo para a Microsoft Store para certificação, você pode declará-lo como acessível. Declarar o seu aplicativo como acessível facilita descobrir os usuários que estão interessados em aplicativos acessíveis, como os usuários com deficiências visuais. Os usuários descobrem aplicativos acessíveis usando o filtro **Acessível** ao pesquisar a Microsoft Store. Declarar o seu aplicativo como acessível também adiciona a marca **Acessível** à descrição do aplicativo.

Ao declarar seu aplicativo como acessível, você confirma que ele tem as [informações básicas de acessibilidade](basic-accessibility-information.md) de que os usuários precisam para os principais cenários usando um ou mais dos seguintes itens:

* O teclado.
* Um tema de alto contraste.
* Uma definição variável de pontos por polegada (dpi)
* Tecnologia adaptativa comum, como os recursos de acessibilidade do Windows, entre eles o Narrador, a Lupa e o Teclado Virtual.

Você deverá declarar seu aplicativo como acessível se o tiver compilado e testado para acessibilidade. Isso significa que você deve ter feito o seguinte:

* Definiu todas as informações de acessibilidade relevantes para elementos da interface do usuário, incluindo o nome, a função, o valor, etc.
* Implementado a acessibilidade total do teclado, permitindo que o usuário:
    * Realize os principais cenários do aplicativo usando apenas o teclado.
    * Percorra os elementos da interface do usuário seguindo uma ordem lógica.
    * Navegue entre os elementos da interface do usuário dentro de um controle usando as teclas direcionais.
    * Use atalhos do teclado para acessar a funcionalidade principal do aplicativo.
    * Use os gestos de toque do Narrador para equivalência de guia e seta para dispositivos sem teclado.
* Garanta que a interface do usuário do seu aplicativo é acessível visualmente: tem um índice de contraste de texto mínimo de 4.5:1, não depende de cores para passar informações e assim por diante.
* Uso de ferramentas de teste de acessibilidade como [**Inspect**](https://msdn.microsoft.com/library/windows/desktop/Dd318521) e [**UIAVerify**](https://msdn.microsoft.com/library/windows/desktop/Hh920986) para verificar a sua implementação de acessibilidade e resolução de todos os erros de prioridade 1 reportados por elas.
* Verificou os cenários principais do seu aplicativo, de ponta a ponta, usando o Narrador, a Lupa, o Teclado Virtual, um tema de alto contraste e configurações de dpi ajustadas.

Consulte [Accessibility checklist](accessibility-checklist.md) para ver uma análise desses procedimentos e links para recursos que ajudarão você a cumpri-los.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados    
* [Acessibilidade](accessibility.md) 
