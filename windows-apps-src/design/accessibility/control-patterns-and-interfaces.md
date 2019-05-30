---
Description: Lista os padrões de controle de Automação da Interface do Usuário da Microsoft, as classes que os clientes usam para acessá-los e as interfaces que os provedores usam para implementá-los.
ms.assetid: 2091883C-5D0C-44ED-936A-709022926A42
title: Padrões e interfaces de controle
label: Control patterns and interfaces
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fc8018a4ccf53c106a1e10410593e214d9dcead7
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66359572"
---
# <a name="control-patterns-and-interfaces"></a>Padrões e interfaces de controle  



Lista os padrões de controle de Automação da Interface do Usuário da Microsoft, as classes que os clientes usam para acessá-los e as interfaces que os provedores usam para implementá-los.

A tabela neste tópico descreve os padrões de controle de Automação da Interface do Usuário Microsoft. Ela também lista as classes usadas pelos clientes de Automação da Interface do Usuário para acessar os padrões de controle e as interfaces usadas pelos provedores de Automação da Interface do Usuário para implementá-las. A coluna **Padrão de controle** mostra o nome do padrão da perspectiva do cliente da Automação da Interface do Usuário, como um valor constante listado em [**Control Pattern Availability Property Identifiers**](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-control-pattern-availability-propids). Da perspectiva do provedor da Automação da Interface do Usuário, cada um desses padrões é um nome de constante [**PatternInterface**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.PatternInterface). A coluna **Class provider interface** mostra o nome da interface que os provedores implementam para fornecer esse padrão para um controle personalizado XAML.

Para saber mais sobre como implementar pares de automação personalizados que exponham padrões de controle e implementem as interfaces, consulte [Pares de automação personalizados](custom-automation-peers.md).

Ao implementar um padrão de controle, você também deverá consultar a documentação do provedor da Automação da Interface do Usuário, que explica algumas das expectativas que os clientes têm de um padrão de controle, independentemente de qual estrutura da IU é usada para implementá-lo. Algumas das informações listadas na documentação geral do provedor da Automação da Interface do Usuário influenciará a forma como você implementa seus pares e dá suporte correto a esse padrão. Consulte [Implementando padrões de controle de Automação da Interface do Usuário](https://docs.microsoft.com/windows/desktop/WinAuto/uiauto-implementinguiautocontrolpatterns), e visualize a página que documenta o padrão que você pretende implementar.

| Padrão de controle | Interface do provedor de classe | Descrição |
|-----------------|--------------------------|-------------|
| **Anotação** | [**IAnnotationProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IAnnotationProvider) | Usado para expor as propriedades de uma anotação em um documento. |
| **Encaixe** | [**IDockProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDockProvider) | Usado para controles que podem ser encaixados em um contêiner. Por exemplo, barras de ferramentas ou paletas de ferramentas. |
| **Arraste** | [**IDragProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDragProvider) | Usado para oferecer suporte aos controles arrastáveis ou controles com itens arrastáveis. |
| **DropTarget** | [**IDropTargetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IDropTargetProvider) | Usado para oferecer suporte aos controles podem ser o destino para uma operação de arrastar e soltar. |
| **ExpandCollapse** | [**IExpandCollapseProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IExpandCollapseProvider) | Usado para oferecer suporte aos controles que se expandem visualmente para exibir mais conteúdo e se recolhem para ocultar o conteúdo. |
| **Grade** | [**IGridProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridProvider) | Usado para controles que dão suporte à funcionalidade de grade, como dimensionamento e transferência para uma célula especificada. Observe que Grade em si não implementa esse padrão porque ele fornece layout, mas não é um controle. |
| **GridItem** | [**IGridItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IGridItemProvider) | Usado para controles que possuem células nas grades. |
| **Invoke** | [**IInvokeProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IInvokeProvider) | Usado para controles que podem ser invocados, como [**Botão**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button). |
| **ItemContainer** | [**IItemContainerProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IItemContainerProvider) | Possibilita que os aplicativos localizem um elemento em um contêiner, como uma lista virtualizada. |
| **MultipleView** | [**IMultipleViewProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IMultipleViewProvider) | Usado para controles que podem alternar entre várias representações do mesmo conjunto de informações, dados ou filhos. |
| **ObjectModel** | [**IObjectModelProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IObjectModelProvider) | Usado para expor um ponteiro para o modelo de objeto subjacente de um documento. |
| **RangeValue** | [**IRangeValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IRangeValueProvider) | Usado para controles que possuem um intervalo de valores que podem ser aplicados ao controle. Por exemplo, um controle giratório contendo anos poderia ter o intervalo de 1900 até ano atual, já outro contendo meses poderia ter o intervalo de 1 a 12. |
| **Rolagem** | [**IScrollProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollProvider) | Usado para controles que podem ser rolados. Por exemplo, um controle que tenha barras de rolagem que ficam ativas quando há mais informações que podem ser exibidas na área de visualização do controle. |
| **ScrollItem** | [**IScrollItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IScrollItemProvider) | Usado para controles que possuem itens individuais em uma lista de rolagem. Por exemplo, um controle de lista que possua itens individuais na lista de rolagem, como um controle de caixa de combinação. |
| **Seleção** | [**ISelectionProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionProvider) | Usado para controles de contêiner de seleção. Por exemplo, [**ListBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ListBox) e [**ComboBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ComboBox). |
| **SelectionItem** | [**ISelectionItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISelectionItemProvider) | Usado para itens individuais nos controles do contêiner de seleção, como caixas de listagem e caixas de combinação. |
| **Spreadsheet** | [**ISpreadsheetProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetProvider) | Usado para expor o conteúdo de uma folha de cálculo ou de outro documento baseado em grade. |
| **SpreadsheetItem** | [**ISpreadsheetItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISpreadsheetItemProvider) | Usado para expor as propriedades de uma célula em uma folha de cálculo ou outro documento baseado em grade. |
| **Estilos** | [**IStylesProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IStylesProvider) | Usado para descrever um elemento de interface do usuário que tem um estilo, cor de preenchimento, padrão de preenchimento ou forma específico. |
| **SynchronizedInput** | [**ISynchronizedInputProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ISynchronizedInputProvider) | Permite que os aplicativos de cliente da Automação da Interface do Usuário redirecionem a entrada do mouse ou do teclado para um elemento da interface do usuário específico. |
| **Tabela** | [**ITableProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableProvider) | Usado para controles que possuem uma grade e informações de cabeçalho. Por exemplo, um controle de calendário tabular. |
| **TableItem** | [**ITableItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITableItemProvider) | Usado para itens em uma tabela. |
| **Texto** | [**ITextProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITextProvider) | Usado para editar controles e documentos que exponham informações textuais. Consulte também [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) e [**ITextProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider2). |
| **TextChild** | [**ITextChildProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextchildprovider) | Usado para acessar o antecessor mais próximo de um elemento que suporta o padrão de controle **Text**. |
| **TextEdit** | Não há classe gerenciada disponível | Fornece acesso a um controle que modifica o texto, por exemplo, um controle que executa correção automática ou permite a composição de entrada através de um IME (Editor de Método de Entrada). |
| **TextRange** | [**ITextRangeProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider) | Fornece acesso a um conjunto de texto contínuo em um contêiner de texto que implementa [**ITextProvider**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextprovider). Consulte também [**ITextRangeProvider2**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.provider.itextrangeprovider2). |
| **Alternar** | [**IToggleProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IToggleProvider) | Usado para controles onde é possível alternar o estado. Por exemplo, [**CheckBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.CheckBox) e itens de menu que podem ser marcados. |
| **Transformação** | [**ITransformProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.ITransformProvider) | Usado para controles que podem ser redimensionados, transferidos e girados. O padrão de controle de transformação costuma ser usado em aplicativos de desenho, design, formulários e editores gráficos. |
| **Valor** | [**IValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) | Possibilita que os clientes obtenham ou definam um valor nos controles que não dão suporte a um intervalo de valores. |
| **VirtualizedItem** | [**IVirtualizedItemProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IVirtualizedItemProvider) | Expõe itens nos contêineres que são virtualizados e precisam estar totalmente acessíveis como elementos de automação da IU. |
| **Window** | [**IWindowProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IWindowProvider) | Expõe as informações específicas do windows, um conceito fundamental para o sistema operacional Microsoft Windows. Exemplos de controles que são janelas filho e caixas de diálogo. |

> [!NOTE]
> Você não encontrará necessariamente implementações de todos esses padrões em controles XAML existentes. Alguns dos padrões têm interfaces exclusivamente para oferecer suporte à paridade com a definição de padrões da estrutura de Automação da IU geral e para oferecer suporte a cenários de mesmo nível de automação que exigirão uma implementação puramente personalizada para oferecer suporte a esse padrão.

> [!NOTE]
> Os aplicativos da Loja do Windows Phone não dão suporte a todos os padrões de controle de Automação da Interface do Usuário listados aqui. **Annotation**, **Dock**, **Drag**, **DropTarget**, **ObjectModel** são alguns dos padrões não suportados.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Pares de automação personalizados](custom-automation-peers.md)
* [Acessibilidade](accessibility.md) 
