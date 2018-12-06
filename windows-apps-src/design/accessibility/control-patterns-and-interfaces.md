---
Description: Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Padrões e interfaces de controle
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 87afe086ca28e27a39f5508a2bea5ea9fcb1c6a5
ms.sourcegitcommit: d7613c791107f74b6a3dc12a372d9de916c0454b
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8757573"
---
# <a name="control-patterns-and-interfaces"></a>Padrões e interfaces de controle  



Lista os padrões de controle de Automação da Interface do Usuário da Microsoft, as classes que os clientes usam para acessá-los e as interfaces que os provedores usam para implementá-los.

A tabela neste tópico descreve os padrões de controle de Automação da Interface do Usuário Microsoft. Ela também lista as classes usadas pelos clientes de Automação da Interface do Usuário para acessar os padrões de controle e as interfaces usadas pelos provedores de Automação da Interface do Usuário para implementá-las. A coluna **Padrão de controle** mostra o nome do padrão da perspectiva do cliente da Automação da Interface do Usuário, como um valor constante listado em [**Control Pattern Availability Property Identifiers**](https://msdn.microsoft.com/library/windows/desktop/Ee671199). Da perspectiva do provedor da Automação da Interface do Usuário, cada um desses padrões é um nome de constante [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496). A coluna **Class provider interface** mostra o nome da interface que os provedores implementam para fornecer esse padrão para um controle personalizado XAML.

Para saber mais sobre como implementar pares de automação personalizados que exponham padrões de controle e implementem as interfaces, consulte [Pares de automação personalizados](custom-automation-peers.md).

Ao implementar um padrão de controle, você também deverá consultar a documentação do provedor da Automação da Interface do Usuário, que explica algumas das expectativas que os clientes têm de um padrão de controle, independentemente de qual estrutura da IU é usada para implementá-lo. Algumas das informações listadas na documentação geral do provedor da Automação da Interface do Usuário influenciará a forma como você implementa seus pares e dá suporte correto a esse padrão. Consulte [Implementando padrões de controle de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671292), e visualize a página que documenta o padrão que você pretende implementar.

| Padrão de controle | Interface do provedor de classe | Descrição |
|-----------------|--------------------------|-------------|
| **Anotações** | [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) | Usado para expor as propriedades de uma anotação em um documento. |
| **Doca** | [**IDockProvider**](https://msdn.microsoft.com/library/windows/apps/BR242565) | Usado para controles que podem ser encaixados em um contêiner. Por exemplo, barras de ferramentas ou paletas de ferramentas. |
| **Arrastar** | [**IDragProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750322) | Usado para oferecer suporte aos controles arrastáveis ou controles com itens arrastáveis. |
| **DropTarget** | [**IDropTargetProvider**](https://msdn.microsoft.com/library/windows/apps/Hh750327) | Usado para oferecer suporte aos controles podem ser o destino para uma operação de arrastar e soltar. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/BR242568) | Usado para oferecer suporte aos controles que se expandem visualmente para exibir mais conteúdo e se recolhem para ocultar o conteúdo. |
| **Grade** | [**IGridProvider**](https://msdn.microsoft.com/library/windows/apps/BR242578) | Usado para controles que dão suporte à funcionalidade de grade, como dimensionamento e transferência para uma célula especificada. Observe que Grade em si não implementa esse padrão porque ele fornece layout, mas não é um controle. |
| **GridItem** | [**IGridItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242572) | Usado para controles que possuem células nas grades. |
| **Invocar** | [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582) | Usado para controles que podem ser invocados, como [**Botão**](https://msdn.microsoft.com/library/windows/apps/BR209265). |
| **ItemContainer** | [**IItemContainerProvider**](https://msdn.microsoft.com/library/windows/apps/BR242583) | Possibilita que os aplicativos localizem um elemento em um contêiner, como uma lista virtualizada. |
| **MultipleView** | [**IMultipleViewProvider**](https://msdn.microsoft.com/library/windows/apps/BR242585) | Usado para controles que podem alternar entre várias representações do mesmo conjunto de informações, dados ou filhos. |
| **ObjectModel** | [**IObjectModelProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251815) | Usado para expor um ponteiro para o modelo de objeto subjacente de um documento. |
| **RangeValue** | [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) | Usado para controles que possuem um intervalo de valores que podem ser aplicados ao controle. Por exemplo, um controle giratório contendo anos poderia ter o intervalo de 1900 até ano atual, já outro contendo meses poderia ter o intervalo de 1 a 12. |
| **Rolagem** | [**IScrollProvider**](https://msdn.microsoft.com/library/windows/apps/BR242601) | Usado para controles que podem ser rolados. Por exemplo, um controle que tenha barras de rolagem que ficam ativas quando há mais informações que podem ser exibidas na área de visualização do controle. |
| **ScrollItem** | [**IScrollItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242599) | Usado para controles que possuem itens individuais em uma lista de rolagem. Por exemplo, um controle de lista que possua itens individuais na lista de rolagem, como um controle de caixa de combinação. |
| **Seleção** | [**ISelectionProvider**](https://msdn.microsoft.com/library/windows/apps/BR242616) | Usado para controles de contêiner de seleção. Por exemplo, [**ListBox**](https://msdn.microsoft.com/library/windows/apps/BR242868) e [**ComboBox**](https://msdn.microsoft.com/library/windows/apps/BR209348). |
| **SelectionItem** | [**ISelectionItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242610) | Usado para itens individuais nos controles do contêiner de seleção, como caixas de listagem e caixas de combinação. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251821) | Usado para expor o conteúdo de uma folha de cálculo ou de outro documento baseado em grade. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251817) | Usado para expor as propriedades de uma célula em uma folha de cálculo ou outro documento baseado em grade. |
| **Estilos** | [**IStylesProvider**](https://msdn.microsoft.com/library/windows/apps/Dn251823) | Usado para descrever um elemento de interface do usuário que tem um estilo, cor de preenchimento, padrão de preenchimento ou forma específico. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://msdn.microsoft.com/library/windows/apps/Dn279198) | Permite que os aplicativos de cliente da Automação da Interface do Usuário redirecionem a entrada do mouse ou do teclado para um elemento da interface do usuário específico. |
| **Tabela** | [**ITableProvider**](https://msdn.microsoft.com/library/windows/apps/BR242623) | Usado para controles que possuem uma grade e informações de cabeçalho. Por exemplo, um controle de calendário tabular. |
| **TableItem** | [**ITableItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242620) | Usado para itens em uma tabela. |
| **Text** | [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) | Usado para editar controles e documentos que exponham informações textuais. Consulte também [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) e [**ITextProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextchildprovider) | Usado para acessar o antecessor mais próximo de um elemento que suporta o padrão de controle **Text**. |
| **TextEdit** | Não há classe gerenciada disponível | Fornece acesso a um controle que modifica o texto, por exemplo, um controle que executa correção automática ou permite a composição de entrada através de um IME (Editor de Método de Entrada). |
| **TextRange** | [**ITextRangeProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider) | Fornece acesso a um conjunto de texto contínuo em um contêiner de texto que implementa [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextprovider). Consulte também [**ITextRangeProvider2**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Toggle** | [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653) | Usado para controles onde é possível alternar o estado. Por exemplo, [**CheckBox**](https://msdn.microsoft.com/library/windows/apps/BR209316) e itens de menu que podem ser marcados. |
| **Transform** | [**ITransformProvider**](https://msdn.microsoft.com/library/windows/apps/BR242656) | Usado para controles que podem ser redimensionados, transferidos e girados. O padrão de controle de transformação costuma ser usado em aplicativos de desenho, design, formulários e editores gráficos. |
| **Value** | [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) | Possibilita que os clientes obtenham ou definam um valor nos controles que não dão suporte a um intervalo de valores. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://msdn.microsoft.com/library/windows/apps/BR242668) | Expõe itens nos contêineres que são virtualizados e precisam estar totalmente acessíveis como elementos de automação da IU. |
| **Window** | [**IWindowProvider**](https://msdn.microsoft.com/library/windows/apps/BR242670) | Expõe informações específicas de janelas, um conceito fundamental para o sistema operacional Microsoft Windows. Exemplos de controles que são janelas filho e caixas de diálogo. |

> [!NOTE]
> Você não encontrará necessariamente implementações de todos esses padrões em controles XAML existentes. Alguns dos padrões têm interfaces exclusivamente para oferecer suporte à paridade com a definição de padrões da estrutura de Automação da IU geral e para oferecer suporte a cenários de mesmo nível de automação que exigirão uma implementação puramente personalizada para oferecer suporte a esse padrão.

> [!NOTE]
> Os aplicativos da Store do Windows Phone não dão suporte a todos os padrões de controle de Automação da Interface do Usuário listados aqui. **Annotation**, **Dock**, **Drag**, **DropTarget**, **ObjectModel** são alguns dos padrões sem suporte.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Pares de automação personalizados](custom-automation-peers.md)
* [Acessibilidade](accessibility.md) 
