---
title: Caminho de aprendizagem – construir e configurar um formulário
description: Saiba o que você precisa fazer para criar um formulário robusto em seu aplicativo.
ms.date: 05/07/2018
ms.topic: article
keywords: introdução, uwp, windows 10, caminho de aprendizagem, layout, formulário
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 02cb15d948bf35b1c449bb430c9c31dd33d9eec6
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79543969"
---
# <a name="create-and-customize-a-form"></a>Criar e personalizar um formulário

Se você estiver criando um aplicativo que exige que os usuários forneçam uma quantidade significativa de informações, provavelmente desejará criar um formulário para que eles preencham. Este artigo mostrará o que você precisa saber para criar um formulário útil e robusto.

Este não é um tutorial. Se precisar de um, consulte nosso [tutorial de layout adaptável](../design/basics/xaml-basics-adaptive-layout.md), que fornecerá uma experiência guiada passo a passo.

Vamos discutir quais **controles XAML** são incluídos em um formulário, a melhor forma de organizá-los em sua página e como otimizar seu formulário para alterar os tamanhos de tela. Entretanto, como um formulário aborda a posição relativa dos elementos visuais, primeiro discutiremos o layout da página com XAML.

## <a name="what-do-you-need-to-know"></a>O que você precisa saber?

A UWP não tem um controle de formulário explícito que você pode adicionar ao seu aplicativo e configurar. Em vez disso, você precisará criar um formulário organizando uma coleção de elementos de interface do usuário em uma página.

Para fazer isso, você precisará entender os **painéis de layout**. Eles são contêineres com os elementos da interface do usuário do aplicativo, permitindo organizá-los e agrupá-los. Colocar painéis de layout dentro de outros painéis de layout confere a você bastante controle sobre onde e como seus itens são exibidos um em relação ao outro. Isso também facilita a adaptação de seu aplicativo a tamanhos de tela diferentes.

Leia [esta documentação sobre painéis de layout](../design/layout/layout-panels.md). Como os formulários geralmente são exibidos em uma ou mais colunas verticais, é recomendado agrupar itens semelhantes em um **StackPanel** e organizá-los em um **RelativePanel**, se necessário. Comece reunindo alguns painéis – se precisar de uma referência, veja abaixo uma estrutura de layout básico para um formulário de duas colunas:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.RightOf="Customer">
        <!--Content-->
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Customer">
        <!--Save and Cancel buttons-->
    </StackPanel>
</RelativePanel>
```

## <a name="what-goes-in-a-form"></a>O que é incluído em um formulário?

Você deverá preencher o formulário com vários [Controles XAML](../design/controls-and-patterns/controls-and-events-intro.md). Você provavelmente está familiarizado com eles, mas fique à vontade para ler caso precise se atualizar. Especificamente, você deseja controles específicos que permitam ao usuário inserir texto ou escolher dentre uma lista de valores. Esta é uma lista básica das opções que você pode adicionar. Não é necessário ler tudo sobre elas, apenas o suficiente para compreender o que são e como funcionam.

* [TextBox](../design/controls-and-patterns/text-box.md) permite que um usuário insira texto em seu aplicativo.
* [ToggleSwitch](../design/controls-and-patterns/toggles.md) permite que um usuário escolha entre duas opções.
* [DatePicker](../design/controls-and-patterns/date-picker.md) permite que um usuário selecione um valor de data.
* [TimePicker](../design/controls-and-patterns/time-picker.md) permite que um usuário selecione um valor temporal.
* [ComboBox](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox) expande para exibir uma lista de itens selecionáveis. Você pode aprender mais sobre eles [aqui](../design/controls-and-patterns/combo-box.md)

Você também pode adicionar [botões](../design/controls-and-patterns/buttons.md) para que o usuário possa salvar ou cancelar.

## <a name="format-controls-in-your-layout"></a>Controles de formato no layout

Você sabe como organizar os painéis de layout e tem itens que gostaria de adicionar, mas como eles devem ser formatados? A página [formulários](../design/controls-and-patterns/forms.md) apresenta algumas diretrizes de design específicas. Leia as seções sobre **Tipos de formulários** e **layout** para ver recomendações úteis. Discutiremos a acessibilidade e o layout relativo em breve.

Com essas orientações em mente, você deve começar a adicionar os controles de sua preferência ao layout, certificando-se de que eles recebam rótulos e estejam devidamente espaçados. Por exemplo, esta é a estrutura básica de um formulário de uma página usando as orientações sobre layout, controles e design acima:

```xaml
<RelativePanel>
    <StackPanel x:Name="Customer" Margin="20">
        <TextBox x:Name="CustomerName" Header= "Customer Name" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address" Header="Address" PlaceholderText="Address" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <TextBox x:Name="Address2" Margin="0,24,0,0" PlaceholderText="Address 2" HorizontalAlignment="Left" />
            <RelativePanel>
                <TextBox x:Name="City" PlaceholderText="City" Margin="0,24,0,0" HorizontalAlignment="Left" />
                <ComboBox x:Name="State" PlaceholderText="State" Margin="24,24,0,0" RelativePanel.RightOf="City">
                    <!--List of valid states-->
                </ComboBox>
            </RelativePanel>
    </StackPanel>
    <StackPanel x:Name="Associate" Margin="20" RelativePanel.Below="Customer">
        <TextBox x:Name="AssociateName" Header= "Associate" Margin="0,24,0,0" HorizontalAlignment="Left" />
        <DatePicker x:Name="TargetInstallDate" Header="Target install Date" HorizontalAlignment="Left" Margin="0,24,0,0"></DatePicker>
        <TimePicker x:Name="InstallTime" Header="Install Time" HorizontalAlignment="Left" Margin="0,24,0,0"></TimePicker>
    </StackPanel>
    <StackPanel x:Name="Save" Orientation="Horizontal" RelativePanel.Below="Associate">
        <Button Content="Save" Margin="24" />
        <Button Content="Cancel" Margin="24" />
    </StackPanel>
</RelativePanel>
```

Fique à vontade para personalizar seus controles com mais propriedades para obter uma experiência visual melhor.

## <a name="make-your-layout-responsive"></a>Tornar o layout dinâmico

Os usuários podem exibir a interface do usuário em vários dispositivos com larguras de tela diferentes. Para garantir que eles tenham uma boa experiência independentemente da tela, você deve usar um [design dinâmico](../design/layout/responsive-design.md). Leia essa página para obter boas recomendações sobre as filosofias de design a ter em mente durante o processo.

A página [Layouts dinâmicos com XAML](../design/layout/layouts-with-xaml.md) fornece uma visão geral detalhada de como implementar isso. Por enquanto, vamos nos concentrar em **layouts fluidos** e **estados visuais em XAML**.

A estrutura de formulário básica que criamos já é um **layout fluido**, pois depende principalmente da posição relativa dos controles usando apenas o mínimo de posições e tamanhos de pixel específicos. No entanto, lembre-se dessas orientações para interfaces do usuário que você poderá criar no futuro.

Os **estados visuais** são ainda mais importantes para os layouts dinâmicos. Um estado visual define valores de propriedade aplicados a um elemento específico quando uma condição específica é verdadeira. [Leia sobre como fazer isso em xaml](../design/layout/layouts-with-xaml.md#set-visual-states-in-xaml-markup) e, em seguida, implemente em seu formulário. Veja como é um *muito* básico em nosso exemplo anterior:

```xaml
<Page ...>
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState>
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="640" />
                    </VisualState.StateTriggers>

                    <VisualState.Setters>
                        <Setter Target="Associate.(RelativePanel.RightOf)" Value="Customer"/>
                        <Setter Target="Associate.(RelativePanel.Below)" Value=""/>
                        <Setter Target="Save.(RelativePanel.Below)" Value="Customer"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

        <RelativePanel>
            <!-- Customer StackPanel -->
            <!-- Associate StackPanel -->
            <!-- Save StackPanel -->
        </RelativePanel>
    </Grid>
</Page>
```

> [!IMPORTANT]
> Ao usar StateTriggers, certifique-se sempre de que VisualStateGroups esteja anexado ao primeiro filho da raiz. Aqui, **Grid** é o primeiro filho do elemento **Page** raiz.

Não é prático criar estados visuais para uma ampla gama de tamanhos de tela, pois poucos afetarão significativamente a experiência do usuário de seu aplicativo. É recomendável projetar para alguns pontos de interrupção chave. Você pode [ler mais aqui](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md).

## <a name="add-accessibility-support"></a>Adicionar suporte à acessibilidade

Agora que você tem um layout bem construído que responde às mudanças em tamanhos de tela, uma última coisa que pode fazer para melhorar a experiência do usuário é [disponibilizar o aplicativo](../design/accessibility/accessibility-overview.md). Há muito que pode ser feito, mas em um formulário como esse é mais fácil do que parece. Concentre-se no seguinte:

* Suporte para teclado: certifique-se de que a ordem dos elementos nos painéis da interface do usuário corresponda à forma como são exibidos na tela para que o usuário possa alternar facilmente entre eles.
* Suporte para leitor de tela: certifique-se de que todos os seus controles tenham um nome descritivo.

Ao criar layouts mais complexos com mais elementos visuais, é recomendado consultar a [lista de verificação de acessibilidade](../design/accessibility/accessibility-checklist.md) para obter mais detalhes. Afinal, embora a acessibilidade não seja necessária para um aplicativo, ela ajuda a interagir com um público maior.

## <a name="going-further"></a>Aprofundamento

Embora você tenha criado um formulário aqui, os conceitos de layouts e controles são aplicáveis a todas as interfaces do usuário XAML que você pode construir. Fique à vontade para ler novamente os documentos que vinculamos e para experimentar com seu formulário, adicionando novos recursos da interface do usuário e aperfeiçoando a experiência do usuário. Se quiser obter orientações passo a passo sobre recursos de layout mais detalhados, veja nosso [tutorial de layout adaptável](../design/basics/xaml-basics-adaptive-layout.md)

Os formulários também não precisam ser independentes: você pode ir além e inserir o seu em um [padrão mestre/de detalhes](../design/controls-and-patterns/master-details.md) ou em um [controle de pivô](../design/controls-and-patterns/pivot.md). Ou, se quer começar a trabalhar no code-behind do formulário, é recomendado começar com a [visão geral sobre eventos](../xaml-platform/events-and-routed-events-overview.md).

## <a name="useful-apis-and-docs"></a>APIs e documentos úteis

Veja a seguir um resumo rápido de APIs e outras documentações úteis que ajudarão você a começar a trabalhar com a Associação de Dados.

### <a name="useful-apis"></a>APIs úteis

| API | Descrição |
|------|---------------|
| [Controles úteis para formulários](../design/controls-and-patterns/forms.md#input-controls) | Uma lista de controles de entrada úteis para a criação de formulários e orientação básica de onde usá-los. |
| [Grid](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Grid) | Um painel para organizar elementos em layouts com várias linhas e várias colunas. |
| [RelativePanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RelativePanel) | Um painel para organizar os itens em relação a outros elementos e aos limites do painel. |
| [StackPanel](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.StackPanel) | Um painel para organizar elementos em uma única linha horizontal ou vertical. |
| [VisualState](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.VisualState) | Permite definir a aparência de elementos da interface do usuário quando estão em estados específicos. |

### <a name="useful-docs"></a>Documentos úteis

| Tópico | Descrição |
|-------|----------------|
| [Visão geral de acessibilidade](../design/accessibility/accessibility-overview.md) | Uma visão geral de grande escala das opções de acessibilidade em aplicativos. |
| [Lista de verificação de acessibilidade](../design/accessibility/accessibility-checklist.md) | Uma lista de verificação prática para garantir que seu aplicativo atenda aos padrões de acessibilidade. |
| [Visão geral de eventos](../xaml-platform/events-and-routed-events-overview.md) | Detalhes sobre como adicionar e estruturar eventos para processar ações da interface do usuário. |
| [Formulários](../design/controls-and-patterns/forms.md) | Diretrizes gerais para a criação de formulários. |
| [Painéis de layout](../design/layout/layout-panels.md) | Fornece uma visão geral dos tipos de painéis de layout e de onde usá-los. |
| [Padrão mestre/detalhes](../design/controls-and-patterns/master-details.md) | Um padrão de design que pode ser implementado em um ou vários formulários. |
| [Controle de pivô](../design/controls-and-patterns/pivot.md) | Um controle que pode conter um ou vários formulários. |
| [Design dinâmico](../design/layout/responsive-design.md) | Uma visão geral dos princípios de design dinâmico em grande escala. |
| [Layouts dinâmicos com XAML](../design/layout/layouts-with-xaml.md) | Informações específicas sobre estados visuais e outras implementações de design dinâmico. |
| [Tamanho de tela para design dinâmico](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md) | Orientações na tela sobre quais tamanhos de tela devem estar no escopo de layouts dinâmicos. |

## <a name="useful-code-samples"></a>Exemplos de código úteis

| Exemplo de código | Descrição |
|-----------------|---------------|
| [Tutorial de layout adaptável](../design/basics/xaml-basics-adaptive-layout.md) | Uma experiência guiada passo a passo de layouts adaptáveis e design dinâmico. |
| [Banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Veja o layout e os formulários em ação em uma amostra empresarial de várias páginas. |
| [Galeria de controles XAML](https://github.com/Microsoft/Xaml-Controls-Gallery) | Veja uma seleção de controles XAML e como são implementados. |
| [Exemplos de código adicionais](https://developer.microsoft.com/windows/samples) | Escolha **Controles, layout e texto** na lista suspensa de categoria para ver exemplos de códigos relacionados. |
