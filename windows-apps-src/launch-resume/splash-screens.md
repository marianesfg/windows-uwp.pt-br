---
author: PatrickFarley
title: Telas iniciais
description: "Esta seção descreve como definir e configurar a tela inicial do aplicativo."
ms.assetid: 6b954bb3-e5b0-46d1-8afc-fb805536cf6d
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 51add4631b807940e051a8f4cbaca92063e41d4a
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
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