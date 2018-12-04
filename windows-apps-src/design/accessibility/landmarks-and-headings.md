---
Description: Describes the landmarks and headings features of accessibility.
ms.assetid: 019CC63D-D915-4EBD-9442-DE899AB973C9
title: Marcos e títulos
label: Landmarks and Headings
template: detail.hbs
ms.date: 01/24/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d81957c379bd948a50d08b980ff20debc6c223c5
ms.sourcegitcommit: b4c502d69a13340f6e3c887aa3c26ef2aeee9cee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2018
ms.locfileid: "8469599"
---
# <a name="landmarks-and-headings"></a>Marcos e títulos

Uma interface do usuário geralmente é organizada de maneira visualmente eficiente, permitindo que um usuário que possa ver que veja rapidamente o que interessa para ele sem precisar ir devagar para ler *todo* o conteúdo. Um usuário leitor de tela deve ter essa mesma capacidade de passar os olhos rapidamente. Os marcos e títulos definem seções de uma interface do usuário que auxiliam em uma navegação mais eficiente para os usuários de tecnologia adaptativa (AT). A marcação de conteúdo em marcos e títulos fornece a um usuário de leitor de tela a opção de leitura rápida do conteúdo semelhante à maneira que um usuário com visão.

Os conceitos de [marcos ARIA](https://www.w3.org/WAI/GL/wiki/Using_ARIA_landmarks_to_identify_regions_of_a_page), [títulos ARIA](https://www.w3.org/TR/WCAG20-TECHS/ARIA12.html), e [títulos HTML](https://www.w3.org/TR/2016/NOTE-WCAG20-TECHS-20161007/H42.html) têm sido usados no conteúdo da web há anos para permitir a navegação mais rápida pelos usuários de leitor de tela. Páginas da Web utilizam marcos e títulos para tornar o conteúdo mais utilizável, permitindo que o usuário AT acesse rapidamente a parte grande (ponto de referência) e a parte menor (título). Especificamente, os leitores de tela têm comandos que permitem que os usuários pulem entre pontos de referência e entre títulos (próximo/anterior ou nível de título específico). É importante considerar pontos de referência e títulos em seus aplicativos pelos mesmos motivos.

Pontos de referência permitem que o conteúdo seja agrupado em várias categorias, como pesquisa, navegação, conteúdo principal e assim por diante. Depois de agrupados, o usuário AT pode navegar rapidamente entre os grupos. Essa navegação rápida permite que o usuário ignore quantidades de conteúdo potencialmente grandes que anteriormente teriam de ser navegadas item a item, o que tornava a experiência insatisfatória. 

Por exemplo, ao usar um painel de guia, considere isso um ponto de referência de navegação. Quando usar uma caixa de edição de pesquisa, considere isso um ponto de referência de pesquisa e considere configurar o conteúdo principal como um ponto de referência de conteúdo principal. Dentro de um ponto de referência ou até mesmo fora de um ponto de referência, considere configurar sub elementos como títulos com níveis de título lógico. 

Considere a página **Facilidade de Acesso** no aplicativo Configurações do Windows. 

![Página Facilidade de Acesso no aplicativo configurações do Windows](images/EaseOfAccessSettings.png)  

Há uma caixa de edição de pesquisa que é resumida em um ponto de referência de pesquisa. Os elementos de navegação à esquerda são resumidos dentro de um ponto de referência de navegação e o conteúdo principal à direita é resumido em um ponto de referência de conteúdo principal. Detalhando, no ponto de referência de navegação há um título de grupo principal chamado **Facilidade de Acesso**, que é um título de nível 1. Lá estão as subopções **Visão**, **Audição**, e assim por diante. Eles têm um nível de título 2. A definição de títulos continua dentro do conteúdo principal novamente, definindo o assunto principal, **Tela**, como título nível 1, e subgrupos como **Deixar tudo maior** como o nível de título 2. 

O aplicativo Configurações seria acessível sem pontos de referência e títulos, mas ele se torna mais útil com eles. Um usuário de leitor de tela pode chegar de maneira rápida e fácil ao grupo (ponto de referência) que precisam e, em seguida, chegar rapidamente ao subgrupo (título) também. 

Use [AutomationProperties.LandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LandmarkTypeProperty) para configurar o elemento de interface do usuário como o [tipo de ponto de referência](https://msdn.microsoft.com/library/windows/desktop/mt759299) que desejar. Esse elemento de interface do usuário do ponto de referência encapsularia todos os outros elementos da interface de usuário que fizessem sentido para esse ponto de referência. 

Use [AutomationProperties.LocalizedLandmarkTypeProperty](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.LocalizedLandmarkTypeProperty) para nomear especificamente o ponto de referência. Se você selecionar um tipo de ponto de referência pré-definido, como principal ou navegação, esses nomes serão usados para o nome do ponto de referência. No entanto, se você definir o tipo de ponto de referência a ser personalizado, você precisa nomear especificamente o ponto de referência por meio dessa propriedade. Você também pode usar essa propriedade para substituir os nomes padrão dos tipos de ponto de referência não personalizados. 

Use [AutomationProperties.HeadingLevel](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.headinglevelproperty) para definir o elemento da interface de usuário como título de um nível específico do *Nível1* até o *Nível9*.

