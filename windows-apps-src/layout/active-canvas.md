---
author: Jwmsft
Description: "Um padrão com uma área de conteúdo e uma área de comando para aplicativos com exibição única ou experiências modais, como visualizadores/editores de fotos, visualizadores de documentos, mapas, aplicativos de pintura ou outros aplicativos que usam uma exibição com rolagem livre."
title: "Padrão de layout de tela ativa"
ms.assetid: 4D768472-64D6-406C-9E87-F750F6B981A0
label: TBD
template: detail.hbs
op-migration-status: ready
ms.author: jimwalk
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 1782540069254f30b241022b687f493110d3b280
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="active-canvas-layout-pattern"></a>Padrão de layout de tela ativa

Uma tela ativa é um padrão com uma área de conteúdo e uma área de comando. É para aplicativos de modo de exibição único ou experiências modais, como visualizadores/editores de fotos, visualizadores de documentos, mapas, pinturas ou outros aplicativos que fazem uso de um modo de exibição de rolagem livre. Para executar ações, uma tela ativa pode ser combinada com uma barra de comandos ou botões apenas, dependendo do número e dos tipos de ações necessárias.

## <a name="examples"></a>Exemplos

Esse design de um aplicativo de edição de fotos apresenta um padrão de tela ativo, com um exemplo móvel à esquerda e um exemplo da área de trabalho à direita. A superfície de edição de imagens é uma tela, e a barra de comandos na parte inferior contém todas as ações contextuais para o aplicativo.

![Exemplo de um editor de fotos usando o padrão de tela ativa](images/uap-photo-pc-phone-700.png)

Esse design de um aplicativo de mapa do metrô usa uma tela ativa com uma faixa de interface do usuário simples na parte superior com apenas duas ações e uma caixa de pesquisa. Ações contextuais são mostradas no menu de contexto, como visto na imagem à direita.

![Exemplo de um aplicativo de mapas usando o padrão de tela ativa](images/uap-subway-pc-phone-700.png)


## <a name="implementing-this-pattern"></a>Implementando este padrão

O padrão de tela ativa consiste em uma área de conteúdo e uma área de comando.

**Área de conteúdo**  A área de conteúdo costuma ser uma tela de rolagem livre. Várias áreas de conteúdo podem existir dentro de um aplicativo.

**Área de comando.**  Se você estiver posicionando muitos comandos, a barra de comandos, que responde de acordo com o tamanho da tela, deve ser a solução a se seguir. Se você não está posicionando muitos comandos e não se preocupa tanto com uma interface de usuário ágil na resposta, botões que economizam espaço funcionam bem.



## <a name="related-articles"></a>Artigos relacionados

-   [**Barra de aplicativos e barra de comandos**](../controls-and-patterns/app-bars.md)
