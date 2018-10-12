---
author: jwmsft
description: Saiba como otimizar seu aplicativo para fornecer a melhor experiência na Interface do usuário de conjuntos.
title: Projetando para conjuntos
template: detail.hbs
ms.author: jimwalk
ms.date: 05/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, barra de título
doc-status: Draft
ms.localizationpriority: medium
ms.openlocfilehash: 7c3e0e6ec7331e860c9153e2a2e29a51fb5848bd
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4536317"
---
# <a name="designing-for-sets"></a>Projetando para conjuntos

> [!IMPORTANT]
> Este artigo descreve uma funcionalidade que ainda não foi lançada e pode ser modificada substancialmente antes de ser lançada comercialmente. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

A partir do Windows Insider Preview, o recurso de Conjuntos está disponível para os usuários do aplicativo. Com os Conjuntos, seu aplicativo é desenhado em uma janela que pode ser compartilhada com outros aplicativos, e cada um obtém sua própria guia na barra de título.

Neste artigo, vamos examinar as áreas onde você pode querer otimizar o aplicativo para fornecer a melhor experiência na interface do usuário dos conjuntos.

> [!TIP]
> Para obter mais informações sobre Conjuntos, veja a postagem de blog [Apresentação dos conjuntos](https://insider.windows.com/en-us/articles/introducing-sets/) e a conversa [Desenvolvimento para conjuntos](https://developer.microsoft.com/events/build/content/developing-for-sets-on-windows-10) do Microsoft Build 2018.

## <a name="customizing-tab-visuals"></a>Personalizar elementos visuais de guia

Por padrão, o sistema tenta selecionar o texto e as cores de ícone adequadas para a guia do seu aplicativo quando ele estiver ativo. Isso garante que a guia do seu aplicativo terá um bom visual com mínimo esforço de sua parte ou mesmo se não fizer qualquer otimização para Conjuntos.

No entanto, pode haver casos em que é melhor personalizar a cor da guia para seu aplicativo. Nesta seção, explicamos os comportamentos de guia padrão e discutimos quando você deve ou não deve modificá-los para seu aplicativo.

### <a name="tab-states"></a>Estados de guia

Quando seu aplicativo está em um Conjunto, a guia pode estar em um dos três estados: selecionada e ativa, selecionada e inativa ou não selecionada e inativa.

- **Selecionada-Ativa**: uma guia que está selecionada em um conjunto de janelas agrupadas e está na janela ativa em primeiro plano.

    (neste documento, qualquer discussão sobre modificar a guia _ativa_ significa que ela é Selecionada-Ativa.)
- **Selecionada-Inativa**: uma guia que está selecionada em um conjunto de janelas agrupadas, mas não está na janela ativa em primeiro plano.
- **Não selecionada-Inativa**: uma guia que não está selecionada em um conjunto de janelas agrupadas.

A cor de qualquer guia inativa é atualizada e mantida pelo sistema com base no tema do sistema. Não há nenhuma maneira de influenciá-lo pelo aplicativo.

Por padrão, a guia selecionada e ativa respeita a cor tema do sistema especificada pelo usuário nas configurações do Windows. Você pode personalizar a cor da guia para o seu aplicativo apenas quando a guia está ativa.

![Estados de guia em Conjuntos](images/sets-tab-states.jpg)

### <a name="coloring-of-active-tabs"></a>Coloração de guias ativas

A cor da guia ativa é determinada por valores definidos no aplicativo ou nas configurações do sistema. A cor da guia usada quando seu aplicativo está ativo é determinada da seguinte maneira:

- Se você especificar uma cor da guia no aplicativo, ela tem prioridade mais alta. A cor da guia especificada no aplicativo é usada quando ele está ativo, independentemente das configurações do sistema.
- Caso contrário, se o usuário seleciona a opção nas configurações do Windows para mostrar a cor de destaque nas barras de título, a cor de destaque do sistema é usada.
  - Essa configuração é encontrada no aplicativo de configurações do Windows em Personalização > Cores > Mostrar cor de destaque nas seguintes superfícies: barras de título.
- Por fim, se nenhuma configuração de aplicativo ou usuário forem aplicadas, a guia usa a cor de tema do sistema atual.

### <a name="considerations-when-you-modify-tab-colors"></a>Considerações sobre quando você modifica as cores de guia

Aqui, vamos discutir situações em que você talvez deseje modificar a cor da guia do aplicativo e o que deve ser considerado nesses casos. Também abordamos algumas situações em que recomendamos não modificar a cor da guia, mas deixar que o sistema gerencie isso.

#### <a name="match-your-brand-colors"></a>Combinar às cores da marca

Normalmente, o fator de substituição que determina se você modifica a cor da guia é o desejo de combinar com a cor da marca. Ao modificar a guia do aplicativo para corresponder a cor da marca, você deve testar a aparência mediante as outras considerações descritas nesta seção, como o layout do aplicativo ou as cores de tema diferentes do sistema.

#### <a name="horizontal-layout"></a>Layout horizontal

Se o layout do aplicativo inclui uma faixa de cores sólidas (não acrílica) na horizontal da parte superior, ele geralmente funciona bem para conectar seu aplicativo à guia usando uma cor correspondente. Escolha uma cor sólida para a guia, que pareça conectada ao seu aplicativo. O ideal é fornecer algum tipo de continuidade à cor usada na parte superior do aplicativo.

#### <a name="horizontal-layout-with-acrylic"></a>Layout horizontal com acrílico

Caso seu aplicativo use uma faixa de material Acrílico na horizontal da parte superior, recomendamos que você permita ao sistema determinar a cor da guia.

Também recomendamos usar Acrílico no aplicativo aqui em vez de usar o fundo Acrílico para permitir que o fluxo do aplicativo flua para essa área em vez de criar um efeito de faixa mostrado pelo aplicativo ou pela área de trabalho por trás.

Observe que o usuário é capaz de definir essas guias para usar a cor de destaque nesse caso para que possam aparecer como um tema ou cor de destaque claro/escuro.

#### <a name="vertical-layout"></a>Layout vertical

Se o layout do aplicativo inclui um painel vertical de cor sólida na vertical, recomendamos que você não personalize as cores da guia. A posição da guia acima do aplicativo pode ser alterada a qualquer momento pelo usuário, portanto, você não pode depender da continuidade de cor entre a parte superior do aplicativo e a guia. O sistema usa outras indicações visuais, como sombras, para conectar-se à guia com o aplicativo.

### <a name="how-to-modify-tab-colors"></a>Como modificar cores de guia

A cor de guia ativa usa as APIs de personalização atuais da barra de título. Se você já tiver personalizado a cor da barra de título do aplicativo, a alteração também se aplica à guia de aplicativo quando ele está em um Conjunto.

Para modificar as cores de guia, defina as propriedades de [ApplicationViewTitleBar](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.applicationviewtitlebar) para especificar:

- Uma cor de fundo sólida para sua guia.
- Uma cor de fundo sólida para o texto da guia.

Este exemplo mostra como obter uma instância de ApplicationViewTitleBar e definir suas propriedades de cor.

```csharp
// using Windows.UI.ViewManagement;
var titleBar = ApplicationView.GetForCurrentView().TitleBar;

// Set active window colors
titleBar.ForegroundColor = Windows.UI.Colors.White;
titleBar.BackgroundColor = Windows.UI.Colors.Green;
```

Para obter mais informações, consulte a seção _Personalização de cores simples_ do artigo [Personalização da barra de título](title-bar.md#simple-color-customization) e o [Exemplo de personalização na barra de título](http://go.microsoft.com/fwlink/p/?LinkId=620613).

### <a name="considerations-for-full-title-bar-customization"></a>Considerações para a personalização completa da barra de título

Você também pode personalizar a barra de título do seu aplicativo, como descrito na seção _Personalização completa_ do artigo [Personalização da barra de título](title-bar.md#full-customization). Normalmente, você faz isso para [estender Acrílico na barra de título](../style/acrylic.md#extend-acrylic-into-the-title-bar) ou para colocar conteúdo personalizado na barra de título. Se você fizer isso, não se esqueça de seguir as diretrizes para o modo de tela inteira e tablet, e mostre somente o conteúdo da barra de título personalizada quando [CoreApplicationViewTitleBar.IsVisible](/uwp/api/windows.applicationmodel.core.coreapplicationviewtitlebar.isvisible) for **true**.

Quando o aplicativo for executado em um Conjunto, CoreApplicationViewTitleBar.IsVisible é **false**, e o conteúdo da barra de título não deve ser mostrado. Entretanto, se você não seguir essas orientações para ocultar o conteúdo da barra de título personalizada, isso será mostrado abaixo da guia do aplicativo e não na área da barra de título.

Se você colocou conteúdo ou funcionalidade na interface do usuário da barra de título personalizada, considere torná-lo disponível em outra superfície da interface do usuário no aplicativo.

### <a name="how-to-modify-the-tab-icon"></a>Como modificar o ícone da guia

Para garantir que o ícone do aplicativo tenha a melhor aparência em um Conjunto, você deve fornecer um ícone alternativo, sem fundo para o aplicativo. (o ícone do aplicativo usado na guia é o mesmo ícone usado na barra de tarefas). A finalidade do ícone alternativo é a aparência em relação a qualquer cor de fundo. O ícone alternativo será usado, se disponível.

No manifesto do aplicativo, especifique um ícone sem fundo de forma alternativa além de ícone normal. Para obter mais informações, consulte [logotipos e ícones de aplicativos](/windows/uwp/design/style/app-icons-and-logos). O ícone para especificar está documentado como "ativos da lista de tamanho desejado sem fundo" na seção [mais informações sobre os ativos de ícone de aplicativo](/windows/uwp/design/style/app-icons-and-logos#more-about-app-icon-assets) do artigo.

Caso não especifique um ícone alternativo no manifesto do aplicativo, o sistema usará novamente a corda da guia no ícone do bloco.

![Ícones usados em Conjuntos](images/sets-icons.png)

> O mesmo ícone é usado na barra de tarefas e na guia do aplicativo.

## <a name="restore-previous-sets-with-user-activities"></a>Restaurar Conjuntos anteriores com atividades do usuário

Uma vantagem dos Conjuntos é que ele permite aos usuários restaurar guias abertas anteriormente para aplicativos e conteúdo da Web ao iniciar um aplicativo ou abrir um documento. (Veja o vídeo na postagem de blog [Apresentar conjuntos](https://insider.windows.com/en-us/articles/introducing-sets/) para obter mais informações.) Isso é possibilitado por _atividades do usuário_.

Por padrão, o sistema cria atividades do usuário em nome de seu aplicativo, que permite ao aplicativo ser restaurado em uma guia quando um usuário inicia um aplicativo ou abre um documento. Entretanto, as atividades do usuário padrão criadas pelo sistema podem apenas iniciar seu aplicativo no estado padrão. Não é possível restaurar o aplicativo para o estado em que estava anteriormente como parte do conjunto.

Você pode otimizar o aplicativo para Conjuntos fornecendo as atividades do usuário personalizadas. A atividade do usuário que você fornece vincula-se ao seu aplicativo a fim de restaurá-lo para o estado em que estava como parte do Conjunto restaurado.

Para fornecer uma atividade do usuário personalizada:

- Responder a um sistema operacional inicializado [UserActivityRequest](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequest) com uma [UserActivity](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivity) apropriada.
- A UserActivity contém um URI de link direto de ativação que o sistema usa para iniciar o aplicativo com um contexto específico.

Para obter mais informações, consulte [UserActivityRequestManager.UserActivityRequested Event](https://docs.microsoft.com/uwp/api/windows.applicationmodel.useractivities.useractivityrequestmanager.useractivityrequested), [Processar ativação de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation) e [Continuar a atividade do usuário, mesmo em outros dispositivos](https://docs.microsoft.com/windows/uwp/launch-resume/useractivities).

## <a name="enable-multi-instance-for-uwp-apps"></a>Permitir várias instâncias de aplicativos UWP

A partir do Windows 10, versão 1803, os aplicativos UWP oferecem suporte a várias instâncias. A UWP ainda é de instância única por padrão, e você precisa aceitar explicitamente em cada aplicativo que desejar ativar as múltiplas instâncias.

Se você ativar várias instâncias no aplicativo, ele fornece a vantagem de permitir que os usuários executem o aplicativo em mais de um Conjunto por vez. Um aplicativo de instância única pode ser executado somente em um Conjunto por vez.

Para obter mais informações sobre como habilitar várias instâncias no seu aplicativo UWP, veja [Criar um Aplicativo Universal do Windows de várias instâncias](https://docs.microsoft.com/windows/uwp/launch-resume/multi-instance-uwp).

## <a name="use-an-in-app-back-button"></a>Usar um botão de voltar no aplicativo

Para implementar a navegação regressiva no aplicativo, é recomendado colocar um botão de voltar na interface do usuário de acordo com as [orientações de botão de voltar](../basics/navigation-history-and-backwards-navigation.md). Se o aplicativo usa o controle NavigationView, você deve usar o botão Voltar integrado no NavigationView .

Se o aplicativo usa o botão Voltar do sistema, você deve substitui-lo por um botão Voltar no aplicativo em vez disso. Isso garante uma experiência de botão Voltar consistente para os usuários, independentemente do aplicativo ser executado em um conjunto, e também garante que os elementos visuais do botão de voltar permanecem consistentes em todos os aplicativos.

Para obter orientações detalhadas sobre como integrar um botão Voltar no aplicativo, consulte [Histórico de navegação e navegação regressiva](../basics/navigation-history-and-backwards-navigation.md).

### <a name="support-for-the-system-back-button-in-sets"></a>Suporte para o botão Voltar do sistema em Conjuntos

Se o aplicativo usa o botão Voltar do sistema em vez de um botão no aplicativo, a interface do usuário do sistema ainda renderiza o botão do sistema para garantir a compatibilidade com versões anteriores

- Caso o aplicativo não esteja em um conjunto, o botão Voltar é renderizado na barra de título. A experiência visual e as interações do usuário para o botão Voltar ficam inalteradas.
- Caso o aplicativo não esteja em um conjunto, o botão Voltar é renderizado na barra de voltar do sistema.

A barra de voltar do sistema é uma "faixa" inserida entre a faixa de guia e a área de conteúdo do aplicativo. A faixa passa pela largura do aplicativo, com o botão Voltar na borda esquerda. O faixa tem uma altura vertical grande o suficiente para garantir o tamanho do alvo de toque adequado para o botão Voltar.

![O barra de voltar do sistema em Conjuntos](images/sets-system-back-bar.png)

> A barra de voltar do sistema mostrada em um aplicativo.

A barra de voltar do sistema é exibida dinamicamente com base na visibilidade do botão Voltar. Quando o botão Voltar estiver visível, a barra de voltar do sistema é inserida, mudando o conteúdo do aplicativo abaixo da faixa da guia. Quando o botão Voltar é oculto, a barra de voltar do sistema é removida dinamicamente, mudando o conteúdo do aplicativo até a faixa da guia.

Para evitar que a interface do usuário do aplicativo mova-se para cima ou para baixo, é recomendado usar um botão Voltar no aplicativo em vez do botão Voltar do sistema.

Personalizações da barra de título são transferidas para a guia do aplicativo e a barra de voltar do sistema. Se você usar APIs de ApplicationViewTitleBar para especificar as propriedades de cor de primeiro e segundo plano, as cores são aplicadas à guia e à barra de voltar do sistema.

## <a name="related-articles"></a>Artigos relacionados

- [Personalização da barra de título](title-bar.md)
- [Histórico de navegação e navegação regressiva](../basics/navigation-history-and-backwards-navigation.md)
- [Cor](../style/color.md)
