---
title: Telas iniciais
description: Esta seção descreve como definir e configurar a tela inicial do aplicativo.
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df3fc8f54a4174006fd28f319d7cab09142a81fd
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7847951"
---
# <a name="splash-screens"></a>Telas iniciais

Todos os aplicativos UWP devem ter uma tela inicial composta de uma imagem e uma cor da tela de fundo, e ambas podem ser personalizadas.

Sua tela inicial é exibida imediatamente quando o usuário inicia seu aplicativo. Isso proporciona uma resposta imediata aos usuários enquanto os recursos do aplicativo são inicializados. Assim que o aplicativo estiver pronto para interação, a tela inicial será ignorada.

Uma tela inicial bem projetada pode tornar seu aplicativo mais atraente. Aqui está uma tela inicial simples e sutil:

![uma captura de tela em escala de 75% da tela inicial da amostra de tela inicial.](images/regularsplashscreen.png)

Essa tela inicial foi criada combinando uma cor da tela de fundo verde com uma imagem PNG de fundo transparente.

Uma imagem simples com uma cor de fundo parece boa independentemente do dispositivo em que seu aplicativo esteja sendo executado. Somente o tamanho do plano de fundo muda para compensar uma variedade de tamanhos de tela. A imagem permanece sempre intacta.

Além disso, você pode usar a classe [**SplashScreen**](https://msdn.microsoft.com/library/windows/apps/br224763) para personalizar a experiência de inicialização do seu aplicativo. Você pode posicionar uma tela inicial estendida, criada por você, para dar ao aplicativo mais tempo para concluir tarefas adicionais, como a preparação da interface do usuário do aplicativo ou a conclusão de operações de rede. Você também pode usar a classe **SplashScreen** como notificação para quando a tela inicial for ignorada, para que você possa dar início às animações de entrada.

| Tópico | Descrição |
|-------|-------------|
| [Adicionar uma tela inicial](add-a-splash-screen.md) | Defina a imagem e a cor de fundo da tela inicial do seu app. |
| [Exibir uma tela inicial por mais tempo](create-a-customized-splash-screen.md) | Exiba uma tela inicial por mais tempo criando uma tela inicial estendida para o seu aplicativo. Essa tela estendida imita a tela inicial exibida quando o aplicativo é iniciado e pode ser personalizada. |