---
author: Xansky
Description: Describes the concept of automation peers for Microsoft UI Automation, and how you can provide automation support for your own custom UI class.
ms.assetid: AA8DA53B-FE6E-40AC-9F0A-CB09637C87B4
title: Pares de automação personalizados
label: Custom automation peers
template: detail.hbs
ms.author: mhopkins
ms.date: 07/13/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a2f9caf8519aa76ef9487e5318a238a6e1d53fe2
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2918877"
---
# <a name="custom-automation-peers"></a>Pares de automação personalizados  

Descreve o conceito de pares de automação para Automação da Interface do Usuário Microsoft e como você pode dar suporte à automação para sua própria classe de interface do usuário personalizada.

A Automação da Interface do Usuário fornece uma estrutura que os clientes de automação podem usar para examinar ou operar as interfaces do usuário de diversas plataformas e estruturas de interface do usuário. Se você estiver gravando um aplicativo da Plataforma Universal do Windows (UWP), as classes que você usa em sua interface do usuário já fornecem suporte para Automação da Interface do Usuário. Você pode derivar de classes existentes não lacradas para definir um novo tipo de classe de controle ou suporte da interface do usuário. Nesse processo, sua classe pode adicionar o comportamento que deve ter suporte à acessibilidade, mas que o suporte à Automação da Interface do Usuário padrão não abrange. Nesse caso, você deve ampliar o suporte existente à Automação da Interface do Usuário da classe [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) usada pela implementação base, adicionando todo o suporte necessário à implementação do par e informando a infraestrutura de controle da UWP (Plataforma Universal do Windows) em que ela deve criar o novo par.

A Automação da Interface do Usuário habilita aplicativos de acessibilidade e tecnologias assistenciais, como leitores de tela, além de código de controle de qualidade (teste). Em qualquer cenário, os clientes de Automação da Interface do Usuário podem examinar elementos da interface do usuário e simular a interação do usuário com seu aplicativo a partir de outro código externo ao seu aplicativo. Para saber mais sobre a Automação da Interface do Usuário em todas as plataformas e seu significado mais amplo, consulte [Visão geral da Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee684076).

Existem dois públicos diferentes que usam a estrutura de Automação da IU.

* Os ***clientes* de Automação da Interface do Usuário** chamam APIs de Automação da Interface do Usuário para conhecer tudo sobre a interface do usuário atualmente mostrada para o usuário. Por exemplo, uma tecnologia assistencial, como um leitor de tela, age como um cliente de Automação da Interface do Usuário. A interface do usuário é apresentada como uma árvore de elementos de automação relacionados. O cliente de Automação da Interface do Usuário pode estar interessado em apenas um aplicativo por vez ou na árvore inteira. O cliente de Automação da Interface do Usuário pode usar APIs de Automação da Interface do Usuário para navegar na árvore e ler ou mudar as informações nos elementos de automação.
* Os ***provedores* de Automação da Interface do Usuário** contribuem com informações para a árvore de Automação da IU, implementando APIs que expõem os elementos da interface do usuário que introduziram como parte de seu aplicativo. Quando cria um novo controle, você deve agir como participante do cenário de provedor de Automação da Interface do Usuário. Como provedor, você deve garantir que todos os clientes de Automação da Interface do Usuário possam usar a estrutura de Automação da IU de modo a interagir com o controle para fins de acessibilidade e testes.

Normalmente, existem APIs paralelas na estrutura de Automação da IU: uma API para clientes de Automação da Interface do Usuário e outra API com um nome parecido para os provedores de Automação da Interface do Usuário. Para a maior parte, este tópico aborda as APIs para o provedor da Automação da Interface do Usuário, e, especificamente, as classes e interfaces que permitem a extensibilidade de provedor nessa estrutura da IU. Ocasionalmente, nós mencionamos APIs de Automação da Interface do Usuário usadas por clientes de Automação da Interface do Usuário para dar uma perspectiva ou fornecemos uma tabela de pesquisa que relaciona as APIs de cliente e provedor. Para saber mais sobre a perspectiva do cliente, consulte [Guia do programador do cliente de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee684021).

> [!NOTE]
> Os clientes da Automação da Interface do Usuário geralmente não usam código gerenciado e normalmente não são implementados como um aplicativo UWP (eles geralmente são aplicativos de área de trabalho). A Automação da Interface do Usuário tem como base um padrão, e não uma implementação ou estrutura específica. Muitos clientes de Automação da Interface do Usuário, incluindo produtos de tecnologia adaptativa (como leitores de tela), usam interfaces COM (Component Object Model) para interagir com a Automação da Interface do Usuário, o sistema e os aplicativos executados em janelas filho. Para saber mais sobre interfaces COM e sobre como escrever um cliente de Automação da Interface do Usuário usando COM, consulte [Conceitos básicos da Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="Determining_the_existing_state_of_UI_Automation_support_for_your_custom_UI_class"/>
<span id="determining_the_existing_state_of_ui_automation_support_for_your_custom_ui_class"/>
<span id="DETERMINING_THE_EXISTING_STATE_OF_UI_AUTOMATION_SUPPORT_FOR_YOUR_CUSTOM_UI_CLASS"/>

## <a name="determining-the-existing-state-of-ui-automation-support-for-your-custom-ui-class"></a>Determinando o estado existente de suporte à Automação de Interface do Usuário para sua classe de interface do usuário personalizada  
Antes de tentar implementar um par de automação para um controle personalizado, você deve testar se a classe base e seu par de automação já fornecem o suporte à acessibilidade ou automação de que você precisa. Em muitos casos, a combinação das implementações de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472), pares específicos e os padrões que eles implementam pode fornecer uma experiência de acessibilidade básica, mas satisfatória. Isso será verdade dependendo de quantas alterações foram feitas na exposição do modelo de objeto para seu controle em relação à sua classe base. Além disso, depende da correlação entre suas adições à funcionalidade da classe base e os novos elementos da interface do usuário no contrato do modelo ou na aparência visual do controle. Em alguns casos, suas mudanças podem introduzir novos aspectos da experiência do usuário que requerem suporte adicional à acessibilidade.

Mesmo que o uso da classe de par base existente forneça o suporte básico à acessibilidade, é melhor definir um par para que você possa relatar informações precisas de **ClassName** para a Automação da Interface do Usuário em cenários de teste automatizados. Essa consideração é especialmente importante quando você escreve um controle destinado ao consumo de terceiros.

<span id="Automation_peer_classes__"/>
<span id="automation_peer_classes__"/>
<span id="AUTOMATION_PEER_CLASSES__"/>

## <a name="automation-peer-classes"></a>Classes de pares de automação  
O UWP se baseia nas técnicas e convenções de Automação da Interface do Usuário existentes usadas pelas estruturas de IU anteriores de código gerenciado, como Windows Forms, Windows Presentation Foundation (WPF) e Microsoft Silverlight. Muitas das classes de controle e suas funções e finalidades também têm origem em uma estrutura da IU anterior.

Por convenção, os nomes de classe de par começam com o nome de classe de controle e terminam com "AutomationPeer". Por exemplo, [**ButtonAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242458) é a classe de par da classe de controle [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265).

> [!NOTE]
> Neste tópico, nós consideramos as propriedades relacionadas à acessibilidade como mais importantes quando você implementa um par de controles. Porém, para um conceito mais geral do suporte à Automação da Interface do Usuário, você deve implementar um par de acordo com as recomendações documentadas pelo [Guia do programador do provedor de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671596) e [Conceitos básicos de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee684007). Esses tópicos não incluem as APIs de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) específicas que você usaria para fornecer as informações da estrutura de UWP para a Automação da Interface do Usuário, mas descrevem as propriedades que identificam sua classe ou fornecem outras informações ou interações.

<span id="Peers__patterns_and_control_types"/>
<span id="peers__patterns_and_control_types"/>
<span id="PEERS__PATTERNS_AND_CONTROL_TYPES"/>

## <a name="peers-patterns-and-control-types"></a>Pares, padrões e tipos de controle  
Um *padrão de controle* é uma implementação de interface que expõe um aspecto específico de uma funcionalidade do controle para um cliente de Automação da Interface do Usuário. Os clientes de Automação da Interface do Usuário usam as propriedades e os métodos expostos por meio de um padrão de controle para recuperar informações sobre funcionalidades do controle ou para tratar o comportamento do controle em tempo de execução.

Os padrões de controle permitem categorizar e expor a funcionalidade de um controle independente do tipo ou da aparência do controle. Por exemplo, um controle que apresenta uma interface tabular usa o padrão de controle **Grid** para expor o número de linhas e colunas na tabela e para permitir que um cliente de Automação da Interface do Usuário recupere os itens da tabela. Como outros exemplos, o cliente de Automação da Interface do Usuário pode usar o padrão de controle **Invoke** para controles que podem ser invocados, como botões, e o padrão de controle **Scroll** para controles que têm barras de rolagem, como caixas de listagem, modos de exibição em lista ou caixas de combinação. Cada padrão de controle representa uma tipo de funcionalidade separado, e os padrões de controle podem ser combinados para descrever todo o conjunto de funcionalidades com suporte por um controle específico.

Os padrões de controle se relacionam à interface do usuário da mesma maneira que as interfaces se relacionam com objetos COM. No COM, você pode consultar um objeto para saber a quais interfaces ele dá suporte e usar essas interfaces para acessar a funcionalidade. Na Automação da Interface do Usuário, os clientes de Automação da Interface do Usuário podem perguntar a um elemento de Automação da IU os padrões de controle aos quais ele dá suporte e então interagir com o elemento e seu controle par por meio das propriedades, métodos, eventos e estruturas expostos pelos padrões de controle com suporte.

Uma das principais finalidades de um par de automação é relatar para um cliente de Automação da Interface do Usuário a quais padrões de controle o elemento de interface do usuário pode dar suporte por meio de seu par. Para isso, os provedores de Automação da Interface do Usuário implementam novos pares que alteram o comportamento do método [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) substituindo o método [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore). Os clientes de Automação da Interface do Usuário fazem chamadas que o provedor de Automação da Interface do Usuário mapeia para chamar **GetPattern**. Os clientes de Automação da Interface do Usuário consultam cada padrão específico com o qual desejam interagir. Se o par dá suporte ao padrão, ele retorna uma referência do objeto a si próprio. Caso contrário, ele retorna **null**. Se **null** não for retornado, o cliente de Automação da Interface do Usuário vai esperar que ele chame as APIs da interface padrão como um cliente para interagir com esse padrão de controle.

Um *tipo de controle* é uma forma de definir amplamente a funcionalidade de um controle representado pelo par. Esse conceito é diferente do padrão de controle, pois o padrão informa à Automação da Interface do Usuário quais informações ela pode obter ou quais ações ela pode executar por meio de determinada interface; o tipo de controle fica um nível acima disso. Cada tipo de controle possui diretrizes sobre esses aspectos da Automação da Interface do Usuário:

* Padrões de controle da Automação da Interface do Usuário: um tipo de controle permite mais de um padrão, cada um representando uma classificação diferente de informações ou interação. Cada tipo de controle possui um conjunto de padrões que o controle tem que permitir, um conjunto opcional e um conjunto que o controle não deve permitir.
* Valores de propriedades da Automação da Interface do Usuário: cada tipo de controle possui um conjunto de propriedades que o controle tem que permitir. Estas são as propriedades gerais, como descrito na [Visão geral das propriedades da Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671594), e não são específicas ao padrão.
* Eventos de Automação da Interface do Usuário: cada tipo de controle possui um conjunto de eventos que o controle tem que permitir. Eles também são gerais, e não específicos ao padrão, conforme descrito na [Visão geral dos eventos de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671221).
* Estrutura da árvore de automação da IU: cada tipo de controle define como o controle deve aparecer na estrutura da árvore de automação da IU.

Independentemente de como os pares de automação da estrutura estão implementados, a funcionalidade de cliente de Automação da Interface do Usuário não está vinculada a UWP, e, na realidade, é provável que os clientes de Automação da Interface do Usuário existentes, como as tecnologias adaptativas, usem outros modelos de programação, como COM. No COM, os clientes podem usar **QueryInterface** para a interface de padrão de controle do COM que implementa o padrão solicitado ou para a estrutura geral de Automação da IU para examinar as propriedades, os eventos ou a árvore. Para os padrões, a estrutura de Automação da IU realiza marshaling do código da interface no código do UWP que é executado no provedor da Automação da Interface do Usuário do aplicativo e no par relevante.

Quando você implementa padrões de controle de uma estrutura de código gerenciado, como um aplicativo UWP em C\# ou Microsoft Visual Basic, pode usar as interfaces do .NET Framework para representar esses padrões, em vez de usar a representação da interface COM. Por exemplo, a interface do padrão de Automação da Interface do Usuário de uma implementação do provedor Microsoft .NET do padrão **Invoke** é [**IInvokeProvider**](https://msdn.microsoft.com/library/windows/apps/BR242582).

Para ver uma lista de padrões de controle, interfaces de provedor e suas finalidades, consulte [Padrões de controle e interfaces](control-patterns-and-interfaces.md). Para ver a lista de tipos de controle, confira [Visão geral dos tipos de controle de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671197).

<span id="Guidance_for_how_to_implement_control_patterns"/>
<span id="guidance_for_how_to_implement_control_patterns"/>
<span id="GUIDANCE_FOR_HOW_TO_IMPLEMENT_CONTROL_PATTERNS"/>

### <a name="guidance-for-how-to-implement-control-patterns"></a>Diretrizes de como implementar padrões de controle  
Os padrões de controle e suas finalidades fazem parte de uma definição maior da estrutura de Automação da IU e não se aplicam apenas ao suporte para acessibilidade de um aplicativo UWP. Ao implementar um padrão de controle, sempre verifique se você está seguindo as diretrizes documentadas no MSDN e também na especificação da Automação da Interface do Usuário. Você encontra as diretrizes fazendo uma busca geral nos tópicos do MSDN, sem precisar mencionar a especificação. As diretrizes de cada padrão estão documentadas aqui: [Implementando padrões de controle de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Observe que cada tópico dessa área tem uma seção de diretrizes e convenções de implementação e outra de membros necessários. As diretrizes geralmente referem-se a APIs específicas da interface de padrão de controle na referência [Interfaces padrão de controle para provedores](https://msdn.microsoft.com/library/windows/desktop/Ee671201). Essas interfaces são do tipo nativo/COM (e suas APIs usam sintaxe de estilo COM). Mas tudo o que aparece lá tem seu equivalente no namespace [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225).

Se você está usando os pares de automação padrão e expandindo seu comportamento, esses pares serão gravados de acordo com as diretrizes da Automação da Interface do Usuário. Se eles permitirem padrões de controle, você poderá confiar no suporte aos padrões em conformidade com as diretrizes em [Implementando padrões de controle da Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671292). Se um par de controle relatar que é representante de um tipo de controle definido pela Automação da Interface do Usuário, ele terá seguido as diretrizes documentadas em [Permitindo tipos de controle de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671633).

Mas talvez você precise de diretrizes adicionais para os padrões ou tipos de controle para seguir as recomendações da Automação da Interface do Usuário na implementação do par. Isso poderá ocorrer principalmente se você estiver implementando suporte a tipo ou padrão de controle que ainda não exista como uma implementação padrão em um controle de UWP. Por exemplo, o padrão para anotações não está implementado em nenhum dos controles XAML padrão. Mas você pode ter um aplicativo que use bastante as anotações e, portanto, deseja expor essa funcionalidade para que fique acessível. Nesse cenário, seu par deve implementar [**IAnnotationProvider**](https://msdn.microsoft.com/library/windows/apps/Hh738493) e, provavelmente, relatar a si mesmo como o tipo de controle **Document** com as propriedades adequadas para indicar que seus documentos permitem anotações.

Nós recomendamos que você use as diretrizes referentes aos padrões em [Implementando padrões de controle de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671292) ou aos tipos de controle em [Permitindo tipos de controle de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee671633) de acordo com a orientação e as diretrizes gerais. Você pode até mesmo tentar seguir alguns dos links de API para descrições e comentários sobre a finalidade das APIs. Mas, para obter as especificações de sintaxe que são necessárias para a programação de aplicativo UWP, encontre a API equivalente no namespace [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225) e use suas páginas de referência para saber mais.

<span id="Built-in_automation_peer_classes"/>
<span id="built-in_automation_peer_classes"/>
<span id="BUILT-IN_AUTOMATION_PEER_CLASSES"/>

## <a name="built-in-automation-peer-classes"></a>Classes de pares de automação internas  
Em geral, os elementos implementam uma classe de par de automação quando aceitam a atividade da interface do usuário ou quando contêm informações necessárias para usuários de tecnologias adaptativas que representam a interface do usuário interativa ou significativa dos aplicativos. Nem todos os elementos visuais do UWP têm pares de automação. Os exemplos de classes que implementam pares de automação são [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) e [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683). Os exemplos de classes que não implementam pares de automação são [**Border**](https://msdn.microsoft.com/library/windows/apps/BR209250) e as classes baseadas em [**Panel**](https://msdn.microsoft.com/library/windows/apps/BR227511), como [**Grid**](https://msdn.microsoft.com/library/windows/apps/BR242704) e [**Canvas**](https://msdn.microsoft.com/library/windows/apps/BR209267). Um **Panel** não tem par porque fornece um comportamento de layout apenas visual. O usuário não pode interagir de jeito nenhum de acessibilidade relevante com o **Panel**. Todos os elementos filhos que um **Panel** contém são relatados a árvores de Automação da Interface do Usuário como elementos filho do próximo pai disponível na árvore que tenha um par ou representação de elemento.

<span id="UI_Automation_and_UWP_process_boundaries"/>
<span id="ui_automation_and_uwp_process_boundaries"/>
<span id="UI_AUTOMATION_AND_UWP_PROCESS_BOUNDARIES"/>

## <a name="ui-automation-and-uwp-process-boundaries"></a>Limites de processo de Automação de Interface do Usuário e de UWP  
Normalmente, o código de cliente da Automação da Interface do Usuário que acessa um aplicativo UWP é executado fora do processo. A infraestrutura da estrutura de Automação da IU permite que as informações passem o limite do processo. Esse conceito é explicado melhor em [Conceitos básicos de Automação da Interface do Usuário](https://msdn.microsoft.com/library/windows/desktop/Ee684007).

<span id="OnCreateAutomationPeer"/>
<span id="oncreateautomationpeer"/>
<span id="ONCREATEAUTOMATIONPEER"/>

## <a name="oncreateautomationpeer"></a>OnCreateAutomationPeer  
Todas as classes derivadas de [**UIElement**](https://msdn.microsoft.com/library/windows/apps/BR208911) contêm o método virtual protegido [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer). A sequência de inicialização do objeto para pares de automação chama **OnCreateAutomationPeer** para obter o objeto de par de automação para cada controle e assim construir uma árvore de Automação da IU para uso em tempo de execução. O código de Automação da Interface do Usuário pode usar o par para obter informações sobre recursos e características de um controle e para simular o uso interativo por meio de seus padrões de controle. Um controle personalizado que dá suporte à automação deve substituir **OnCreateAutomationPeer** e retornar uma instância de uma classe derivada de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185). Por exemplo, se um controle personalizado é derivado da classe [**ButtonBase**](https://msdn.microsoft.com/library/windows/apps/BR227736), o objeto retornado por **OnCreateAutomationPeer** deve derivar de [**ButtonBaseAutomationPeer**](https://docs.microsoft.com/en-us/uwp/api/windows.ui.xaml.automation.peers.buttonbaseautomationpeer).

Se você estiver escrevendo uma classe de controle personalizada e pretende também fornecer um novo par de automação, você deverá substituir o método [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) do controle personalizado para que ele retorne uma nova instância do par. A classe de par precisa ser derivada direta ou indiretamente de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185).

Por exemplo, o código a seguir declara que o controle personalizado `NumericUpDown` deve usar o par `NumericUpDownPeer` para fins de Automação da Interface do Usuário.

```csharp
using Windows.UI.Xaml.Automation.Peers;
...
public class NumericUpDown : RangeBase {
    public NumericUpDown() {
    // other initialization; DefaultStyleKey etc.
    }
    ...
    protected override AutomationPeer OnCreateAutomationPeer()
    {
        return new NumericUpDownAutomationPeer(this);
    }
}
```

```vb
Public Class NumericUpDown
    Inherits RangeBase
    ' other initialization; DefaultStyleKey etc.
       Public Sub New()
       End Sub
       Protected Overrides Function OnCreateAutomationPeer() As AutomationPeer
              Return New NumericUpDownAutomationPeer(Me)
       End Function
End Class
```

```cppwinrt
// NumericUpDown.idl
namespace MyNamespace
{
    runtimeclass NumericUpDown : Windows.UI.Xaml.Controls.Primitives.RangeBase
    {
        NumericUpDown();
        Int32 MyProperty;
    }
}

// NumericUpDown.h
...
struct NumericUpDown : NumericUpDownT<NumericUpDown>
{
    ...
    Windows::UI::Xaml::Automation::Peers::AutomationPeer OnCreateAutomationPeer()
    {
        return winrt::make<MyNamespace::implementation::NumericUpDownAutomationPeer>(*this);
    }
};
```

```cpp
//.h
public ref class NumericUpDown sealed : Windows::UI::Xaml::Controls::Primitives::RangeBase
{
// other initialization not shown
protected:
    virtual AutomationPeer^ OnCreateAutomationPeer() override
    {
         return ref new NumericUpDownAutomationPeer(this);
    }
};
```

> [!NOTE]
> A implementação [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) não deve fazer nada além de inicializar uma nova instância do par de automação personalizado (passando o controle da chamada como proprietário) e retornar a instância. Não tente lógicas adicionais nesse método. Especificamente, qualquer lógica que possa levar à destruição do [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) na mesma chamada pode levar a um comportamento de tempo de execução inesperado.

Em implementações típicas de [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer), o *owner* é especificado como **this** ou **Me**, pois a substituição do método está no mesmo escopo que o resto da definição de classe do controle.

A definição de classe de par real pode ser feita no mesmo arquivo de código do controle ou em um arquivo de código separado. As definições de pares existem no namespace [**Windows.UI.Xaml.Automation.Peers**](https://msdn.microsoft.com/library/windows/apps/BR242563), separado dos controles para os quais os pares são fornecidos. Você também pode optar por declarar os pares em um namespace separado, desde que faça referência aos namespaces necessários para a chamada do método [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer).

<span id="Choosing_the_correct_peer_base_class"/>
<span id="choosing_the_correct_peer_base_class"/>
<span id="CHOOSING_THE_CORRECT_PEER_BASE_CLASS"/>

### <a name="choosing-the-correct-peer-base-class"></a>Escolhendo a classe base do par correto  
Verifique se o seu [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) é derivado de uma classe base que dá a melhor correspondência para a lógica de par existente da classe de controle da qual você está derivando. No caso do exemplo anterior, como `NumericUpDown` deriva de [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863), há uma classe [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) disponível que você deve basear seu ponto. Usando a classe de par mais próxima correspondente em paralelo a como você deriva o próprio controle, é possível evitar substituir pelo menos algumas das funcionalidades de [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) porque a classe de par base já as implementa.

A classe [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) base não tem uma classe de par correspondente. Se você precisa de uma classe de par para corresponder a um controle personalizado que deriva de **Control**, derive a classe de par personalizada de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472).

Se você derivar diretamente de [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), essa classe não terá um comportamento de par de automação padrão, porque não há uma implementação de [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer) que referencie uma classe de par. Assim, implemente **OnCreateAutomationPeer** para usar seu próprio par ou use [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) como par, se esse nível de suporte à acessibilidade é adequado para seu controle.

> [!NOTE]
> Você geralmente não deriva de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) em vez de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472). Se derivar diretamente de **AutomationPeer**, você precisará duplicar muito suporte básico de acessibilidade que poderiam vir de **FrameworkElementAutomationPeer**.

<span id="Initialization_of_a_custom_peer_class"/>
<span id="initialization_of_a_custom_peer_class"/>
<span id="INITIALIZATION_OF_A_CUSTOM_PEER_CLASS"/>

## <a name="initialization-of-a-custom-peer-class"></a>Inicialização de uma classe de par personalizada  
O par de automação deve definir um construtor de tipo seguro que usa uma instância do controle do proprietário para a inicialização base. No próximo exemplo, a implementação passa o valor de *owner* para a base de [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506) e, em última instância, é o [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) que realmente usa *owner* para definir [**FrameworkElementAutomationPeer.Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner).

```csharp
public NumericUpDownAutomationPeer(NumericUpDown owner): base(owner)
{}
```

```vb
Public Sub New(owner As NumericUpDown)
    MyBase.New(owner)
End Sub
```

```cppwinrt
// NumericUpDownAutomationPeer.idl
import "NumericUpDown.idl";
namespace MyNamespace
{
    runtimeclass NumericUpDownAutomationPeer : Windows.UI.Xaml.Automation.Peers.AutomationPeer
    {
        NumericUpDownAutomationPeer(NumericUpDown owner);
        Int32 MyProperty;
    }
}

// NumericUpDownAutomationPeer.h
...
struct NumericUpDownAutomationPeer : NumericUpDownAutomationPeerT<NumericUpDownAutomationPeer>
{
    ...
    NumericUpDownAutomationPeer(MyNamespace::NumericUpDown const& owner);
};
```

```cpp
//.h
public ref class NumericUpDownAutomationPeer sealed :  Windows::UI::Xaml::Automation::Peers::RangeBaseAutomationPeer
//.cpp
public:    NumericUpDownAutomationPeer(NumericUpDown^ owner);
```

<span id="Core_methods_of_AutomationPeer"/>
<span id="core_methods_of_automationpeer"/>
<span id="CORE_METHODS_OF_AUTOMATIONPEER"/>

## <a name="core-methods-of-automationpeer"></a>Métodos Core de AutomationPeer  
Devido à infraestrutura de UWP, os métodos substituíveis de um par de automação fazem parte de um par de métodos: o método de acesso público que o provedor de Automação da Interface do Usuário usa como ponto de encaminhamento dos clientes de Automação da Interface do Usuário e o método de personalização "Core" protegido que uma classe UWP pode substituir para afetar o comportamento. O par de métodos é conectado por padrão de forma que a chamada do método de acesso sempre invoca o método "Core" paralelo que tem a implementação do provedor ou, como um fallback, invoca a implementação padrão das classes base.

Ao implementar um par para um controle personalizado, substitua todos os métodos "Core" da classe de par de automação base onde você quer expor um comportamento exclusivo para seu controle personalizado. O código de Automação da Interface do Usuário obtém informações sobre o seu controle chamando métodos públicos da classe de par. Para fornecer informações sobre seu controle, substitua cada método por um nome terminado com "Core" quando a implementação e o design do controle cria cenários de acessibilidade ou outros cenários de Automação da Interface do Usuário diferentes dos que têm suporte na classe de par de automação base.

Sempre que você define uma nova classe de par, implemente pelo menos o método [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), como mostra o próximo exemplo.

```csharp
protected override string GetClassNameCore()
{
    return "NumericUpDown";
}
```

> [!NOTE]
> Convém armazenar as cadeias como constantes em vez de fazer isso diretamente no corpo do método, mas é você quem decide. Para [**GetClassNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclassnamecore), você não precisa localizar essa cadeia de caracteres. A propriedade **LocalizedControlType** é usada sempre que um cliente de Automação da Interface do Usuário (não **ClassName**) necessita de uma cadeia localizada.

### <span id="GetAutomationControlType"/>
<span id="getautomationcontroltype"/>
<span id="GETAUTOMATIONCONTROLTYPE"/>GetAutomationControlType

Algumas tecnologias assistenciais usam o valor [**GetAutomationControlType**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltype) diretamente ao relatar característica dos itens em uma árvore de Automação da IU, como informações adicionais além do **Name** de Automação da Interface do Usuário. Se seu controle for bastante diferente do controle do qual está fazendo a derivação e quiser relatar um tipo de controle diferente daquele relatado pela classe do par base usada pelo controle, você deverá implementar um par e substituir [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) na implementação do par. Isso é especialmente importante quando você deriva de uma classe base generalizada, como [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) ou [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365), em que o par base não fornece informações precisas sobre o tipo de controle.

Sua implementação de [**GetAutomationControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getautomationcontroltypecore) descreve seu controle retornando um valor de [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182). Você pode retornar **AutomationControlType.Custom**, mas deve retornar um dos tipos de controle mais específicos se ele descrever precisamente os cenários principais de seu controle. Veja um exemplo.

```csharp
protected override AutomationControlType GetAutomationControlTypeCore()
{
    return AutomationControlType.Spinner;
}
```

> [!NOTE]
> A menos que você especifique [**AutomationControlType.Custom**](https://msdn.microsoft.com/library/windows/apps/BR209182), não é necessário implementar [**GetLocalizedControlTypeCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlocalizedcontroltypecore) para fornecer um valor da propriedade **LocalizedControlType** aos clientes. A infraestrutura comum da Automação da Interface do Usuário fornece cadeias de caracteres traduzidas para cada valor de **AutomationControlType** possível diferente de **AutomationControlType.Custom**.

<span id="GetPattern_and_GetPatternCore"/>
<span id="getpattern_and_getpatterncore"/>
<span id="GETPATTERN_AND_GETPATTERNCORE"/>

### <a name="getpattern-and-getpatterncore"></a>GetPattern e GetPatternCore  
A implementação do par de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) retorna o objeto que dá suporte ao padrão solicitado no parâmetro de entrada. Especificamente, um cliente de Automação da Interface do Usuário chama um método que é encaminhado ao método [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) do provedor e especifica um valor de enumeração [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) que nomeia o padrão solicitado. Sua substituição do **GetPatternCore** deve retornar o objeto que implementa o padrão especificado. Esse objeto é o próprio par, pois o par deve implementar a interface do padrão correspondente sempre que relata o suporte a um padrão. Se o seu par não tem uma implementação personalizada de um padrão, mas você sabe que a base do par não implementa o padrão, pode chamar a implementação de **GetPatternCore** do tipo base de **GetPatternCore**. O **GetPatternCore** de um par deve retornar **null** se o par não dá suporte ao padrão. Porém, em vez de retornar **null** diretamente de sua implementação, normalmente você dependeria da chamada da implementação base para retornar **null** a qualquer padrão sem suporte.

Quando um padrão tem suporte, a implementação de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) pode retornar **this** ou **Me**. Espera-se que o cliente de Automação da Interface do Usuário converta o valor de retorno de [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) para a interface padrão solicitada sempre que não for **null**.

Se uma classe de par é herdada de outro par e todos os relatórios de padrão e suporte já são tratados pela classe base, a implementação de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) não é necessária. Por exemplo, se você está implementando um controle de intervalo que deriva de [**RangeBase**](https://msdn.microsoft.com/library/windows/apps/BR227863) e seu par derivar de [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506), esse par se retorna para o [**PatternInterface.RangeValue**](https://msdn.microsoft.com/library/windows/apps/BR242496) e tem implementações ativas da interface [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) que dão suporte ao padrão.

Embora este não seja o código literal, este exemplo é próximo da implementação de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) já presente no [**RangeBaseAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242506).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    return base.GetPattern(patternInterface);
}
```

Se você estiver implementando um par em que não tem todo o suporte necessário de uma classe de pares base ou se desejar alterar ou adicionar ao conjunto de padrões herdados de base aos quais seu par pode dar suporte, substitua o [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) para permitir que os clientes de Automação de Interface do Usuário usem os padrões.

Para obter uma lista dos padrões de provedor disponíveis na implementação do UWP do suporte à Automação de Interface do Usuário, consulte [**Windows.UI.Xaml.Automation.Provider**](https://msdn.microsoft.com/library/windows/apps/BR209225). Cada tal padrão tem um valor correspondente a enumeração de [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496), que é como os clientes de Automação da Interface do Usuário solicitam o padrão em uma chamada do [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern).

Um par pode relatar que dá suporte a mais de um padrão. Nesse caso, a substituição deve incluir a lógica do caminho de retorno para cada valor de [**PatternInterface**](https://msdn.microsoft.com/library/windows/apps/BR242496) com suporte e retornar o par a cada correspondência. Espera-se que o chamador solicite apenas uma interface por vez e o chamador decide se deve fazer a conversão para a interface esperada.

Consulte um exemplo da uma substituição de [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) para um par personalizado. Ele relata o suporte a dois padrões, [**IRangeValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242590) e [**IToggleProvider**](https://msdn.microsoft.com/library/windows/apps/BR242653). Este é um controle de exibição de mídia que pode ser mostrado em tela inteira (o modo de alternância) e tem uma barra de progresso na qual os usuários podem escolher uma posição (o controle de intervalo). Esse código vem do [Exemplo de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570).


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.RangeValue)
    {
        return this;
    }
    else if (patternInterface == PatternInterface.Toggle)
    {
        return this;
    }
    return null;
}
```

<span id="Forwarding_patterns_from_sub-elements"/>
<span id="forwarding_patterns_from_sub-elements"/>
<span id="FORWARDING_PATTERNS_FROM_sub-elementS"/>

### <a name="forwarding-patterns-from-sub-elements"></a>Encaminhando padrões de subelementos  
Uma implementação do método [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) também pode especificar um subelemento ou uma parte como um provedor de padrões para seu host. Este exemplo imita a maneira como [**ItemsControl**](https://msdn.microsoft.com/library/windows/apps/BR242803) transfere o tratamento do padrão de rolagem para o par de seu controle [**ScrollViewer**](https://msdn.microsoft.com/library/windows/apps/BR209527) interno. Para especificar um subelemento para o tratamento de padrão, esse código obtém o objeto de subelemento, cria um par para esse subelemento usando o método [**FrameworkElementAutomationPeer.CreatePeerForElement**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.createpeerforelement) e retorna o novo par.


```csharp
protected override object GetPatternCore(PatternInterface patternInterface)
{
    if (patternInterface == PatternInterface.Scroll)
    {
        ItemsControl owner = (ItemsControl) base.Owner;
        UIElement itemsHost = owner.ItemsHost;
        ScrollViewer element = null;
        while (itemsHost != owner)
        {
            itemsHost = VisualTreeHelper.GetParent(itemsHost) as UIElement;
            element = itemsHost as ScrollViewer;
            if (element != null)
            {
                break;
            }
        }
        if (element != null)
        {
            AutomationPeer peer = FrameworkElementAutomationPeer.CreatePeerForElement(element);
            if ((peer != null) && (peer is IScrollProvider))
            {
                return (IScrollProvider) peer;
            }
        }
    }
    return base.GetPatternCore(patternInterface);
}
```

<span id="Other_Core_methods"/>
<span id="other_core_methods"/>
<span id="OTHER_CORE_METHODS"/>

### <a name="other-core-methods"></a>Outros métodos Core  
Talvez seu controle precise dar suporte a equivalentes de teclado para os cenários principais. Para saber porque isso pode ser necessário, consulte [Acessibilidade pelo teclado](keyboard-accessibility.md). A implementação do suporte a chaves deve fazer parte do código do controle e não do código do par, porque ela faz parte de uma lógica de controle, mas a classe de par deve substituir os métodos [**GetAcceleratorKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getacceleratorkeycore) e [**GetAccessKeyCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getaccesskeycore) para relatar para clientes de Automação da Interface do Usuário cujas teclas são usadas. Considere que talvez as cadeias de caracteres que relatam informações de teclas precisem ser localizadas e, portanto, devem ser provenientes de recursos e não de cadeias de caracteres embutidas em código.

Se você estiver fornecendo um par para uma classe suporta uma coleção, será melhor derivar de classes funcionais e classes de par que já têm esse tipo de suporte à coleção. Se você não puder fazê-lo, os pares de controles que mantêm as coleções de filhos podem ter que substituir o método de par relacionado à coleção do [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore) para relatar corretamente as relações pai-filho para a árvore de Automação de Interface do Usuário.

Implemente os métodos [**IsContentElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontentelementcore) e [**IsControlElementCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iscontrolelementcore) para indicar se o controle contém dados ou tem uma função interativa na interface do usuário (ou ambos). Por padrão, os dois métodos retornam **true**. Essas configurações melhoram a usabilidade de tecnologias assistenciais, como leitores de tela, que podem usar esses métodos para filtrar a árvore de automação. Se o seu método [**GetPatternCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpatterncore) transfere o tratamento de padrões para um par de subelemento, o método **IsControlElementCore** do par de subelemento pode retornar **false** para ocultar o par de subelemento na árvore de automação.

Alguns controles podem dar suporte a cenários de rotulamento, em que uma parte do rótulo de texto fornece informações para uma parte que não é de texto, ou um controle se destina a uma relação de rotulamento conhecida com outro controle da interface do usuário. Quando é possível fornecer um comportamento baseado em classe útil, você pode substituir [**GetLabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) para fornecer esse comportamento.

[**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore) e [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore) são usados principalmente para cenários de testes automatizados. Se você quiser dar suporte a testes automatizados para seu controle, convém substituir esses métodos. Isso pode ser útil em controles de tipo de intervalo, em que você não pode sugerir um único ponto porque o local em que o usuário clica no espaço de coordenadas tem um efeito diferente em um intervalo. Por exemplo, o par de automação [**ScrollBar**](https://msdn.microsoft.com/library/windows/apps/BR209745) padrão substitui **GetClickablePointCore** para retornar um valor [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) "não é um número".

[**GetLiveSettingCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlivesettingcore) influencia o padrão de controle do valor de **LiveSetting** de Automação da Interface do Usuário. Convém substitui-lo quando você quer que o controle retorne um valor diferente de [**AutomationLiveSetting.Off**](https://msdn.microsoft.com/library/windows/apps/JJ191519). Para saber mais sobre o que **LiveSetting** representa, consulte [**AutomationProperties.LiveSetting**](https://msdn.microsoft.com/library/windows/apps/JJ191516).

Você pode substituir [**GetOrientationCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getorientationcore) quando o controle tem uma propriedade de orientação configurável que pode mapear para [**AutomationOrientation**](https://msdn.microsoft.com/library/windows/apps/BR209184). As classes [**ScrollBarAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242522) e [**SliderAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242546) são capazes disso.

<span id="Base_implementation_in_FrameworkElementAutomationPeer"/>
<span id="base_implementation_in_frameworkelementautomationpeer"/>
<span id="BASE_IMPLEMENTATION_IN_FRAMEWORKELEMENTAUTOMATIONPEER"/>

### <a name="base-implementation-in-frameworkelementautomationpeer"></a>Implementação base em FrameworkElementAutomationPeer  
A implementação base de [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) fornece algumas informações de Automação da Interface do Usuário que podem ser interpretadas por meio de várias propriedades de layout e comportamento definidas em nível de estrutura.

* [**GetBoundingRectangleCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getboundingrectanglecore): retorna uma estrutura [**Rect**](https://msdn.microsoft.com/library/windows/apps/BR225994) baseada nas características de layout conhecidas. Retorna um valor de 0 **Rect** se [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen) é **true**.
* [**GetClickablePointCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getclickablepointcore): retorna uma estrutura [**Point**](https://msdn.microsoft.com/library/windows/apps/BR225870) baseada nas características de layout conhecidas, desde que haja um **BoundingRectangle** diferente de zero
* [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore): um comportamento mais amplo do que pode ser resumido aqui; consulte [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore). Basicamente, é tentada uma conversão de cadeia de caracteres em qualquer conteúdo conhecido de um [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) ou de classes relacionadas que têm conteúdo. Além disso, quando há um valor de [**LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769), o valor de **Name** desse item é usado como **Name**.
* [**HasKeyboardFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.haskeyboardfocuscore): avaliado com base nas propriedades [**FocusState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.focusstate) e [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) do proprietário. Os elementos que não são controles sempre retornam **false**.
* [**IsEnabledCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isenabledcore): avaliado com base na propriedade [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled) do proprietário, se tem um [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390). Os elementos que não são controles sempre retornam **true**. Isso não significa que o proprietário está habilitado do ponto de vista da interação convencional. Significa que o par está habilitado, mesmo que o proprietário não tenha uma propriedade **IsEnabled**.
* [**IsKeyboardFocusableCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.iskeyboardfocusablecore): retorna **true** se proprietário é um [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390); caso contrário, é **false**.
* [**IsOffscreenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreencore): uma [**Visibility**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.uielement.visibility) de [**Collapsed**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.visibility) no elemento do proprietário ou qualquer de seus pais se iguala a um valor **true** para [**IsOffscreen**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.isoffscreen). Exceção: um objeto [**Popup**](https://msdn.microsoft.com/library/windows/apps/BR227842) pode estar visível, mesmo que os pais de seu proprietário não estejam.
* [**SetFocusCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.setfocuscore): chama [**Focus**](https://msdn.microsoft.com/library/windows/apps/hh702161).
* [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent): chama [**FrameworkElement.Parent**](https://msdn.microsoft.com/library/windows/apps/BR208739) do proprietário e pesquisa o par apropriado. Este não é um par de substituição com um método "Core". Por isso, você não pode alterar esse comportamento.

> [!NOTE]
> Os pares do UWP padrão implementam um comportamento usando o código nativo interno que implementa o UWP, não necessariamente usando o código de UWP padrão. Você não poderá ver o código ou a lógica da implementação por meio da reflexão do CLR (common language runtime) ou de outras técnicas. Você também não vê páginas de referência distintas na referência para substituições específicas de subclasse do comportamento do par base. Por exemplo, pode haver um comportamento adicional para [**GetNameCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getnamecore) de um [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550) que não é descrito na página de referência de **AutomationPeer.GetNameCore**, e não existe uma página de referência de **TextBoxAutomationPeer.GetNameCore**. Não existe nem mesmo uma página de referência de **TextBoxAutomationPeer.GetNameCore**. Em vez disso, leia o tópico de referência para a classe dos pares mais imediatos e consulte as notas de implementação na seção Comentários.

<span id="Peers_and_AutomationProperties"/>
<span id="peers_and_automationproperties"/>
<span id="PEERS_AND_AUTOMATIONPROPERTIES"/>

## <a name="peers-and-automationproperties"></a>Pares e AutomationProperties  
O ponto de automação deve fornecer os valores padrão apropriados para obter informações relacionadas à acessibilidade de seu controle. Observe que qualquer código de aplicativo que use o controle pode substituir alguns desses comportamentos incluindo valores de propriedades anexadas de [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) em instâncias de controles. Os chamadores podem fazer isso para os controles padrão ou para controles personalizados. Por exemplo, o código XAML a seguir cria um botão com duas propriedades personalizadas de Automação da Interface do Usuário:  `<Button AutomationProperties.Name="Special"      AutomationProperties.HelpText="This is a special button."/>`

Para saber mais sobre propriedades anexadas de [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081), consulte [Informações básicas de acessibilidade](basic-accessibility-information.md).

Alguns dos métodos [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) existem devido ao contrato geral de como se espera que os provedores de Automação da Interface do Usuário relatem informações, mas geralmente esses métodos não são implementados em pares de controle. Isso ocorre porque espera-se que essas informações sejam fornecidas por valores de [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081) aplicados ao código do aplicativo que usa os controles em uma interface do usuário específica. Por exemplo, a maioria dos aplicativos definiria a relação de rotulamento entre dois controles diferentes na interface do usuário aplicando um valor [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769). Porém, [**LabeledByCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getlabeledbycore) é implementado em determinados pares que representam relações de dados ou itens em um controle, como ao usar uma parte do cabeçalho para rotular uma parte do campo de dados, rotular itens com seus contêineres ou cenários semelhantes.

<span id="Implementing_patterns"/>
<span id="implementing_patterns"/>
<span id="IMPLEMENTING_PATTERNS"/>

## <a name="implementing-patterns"></a>ImpIementando padrões  
Consultemos como escrever um par para um controle que implementa um comportamento de expandir-recolher implementando a interface do padrão do controle de expandir-recolher. O par deve permitir a acessibilidade para o comportamento de expandir-recolher retornando a si mesmo sempre que o [**GetPattern**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getpattern) é chamado com um valor de [**PatternInterface.ExpandCollapse**](https://msdn.microsoft.com/library/windows/apps/BR242496). O par deve então herdar a interface do provedor para esse padrão ([**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider)) e fornecer implementações para cada um dos membros dessa interface do provedor. Nesse caso, a interface tem três membros para serem substituídos: [**Expand**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expand), [**Collapse**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.collapse), [**ExpandCollapseState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.iexpandcollapseprovider.expandcollapsestate).

É útil planejar com antecedência a acessibilidade no design de API da própria classe. Sempre que você tem um comportamento que pode ser solicitado por interações típicas com um usuário que trabalha na interface do usuário ou por meio de um padrão de provedor de automação, forneça um método único que a resposta da interface do usuário ou o padrão de automação possa chamar. Por exemplo, se o seu controle tem partes de botões com manipuladores de eventos conectados que podem expandir ou recolher o controle, e tem equivalentes de teclado para essas ações, esses manipuladores de eventos devem chamar o mesmo método que você chama no corpo das implementações de [**Expand**](https://msdn.microsoft.com/library/windows/apps/BR242570) ou [**Collapse**](https://msdn.microsoft.com/library/windows/apps/BR242569) para [**IExpandCollapseProvider**](https://msdn.microsoft.com/library/windows/desktop/Ee671242) no par. Usar um método de lógica comum também pode ser uma maneira útil para verificar se os estados visuais do seu controle são atualizados para mostrar o estado lógico de maneira uniforme, independentemente de como o comportamento foi invocado.

Em uma implementação típica, as APIs do provedor primeiro chamam [**Owner**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.frameworkelementautomationpeer.owner) para acessar a instância do controle em tempo de execução. Depois, os métodos de comportamento necessários podem ser chamados nesse objeto.


```csharp
public class IndexCardAutomationPeer : FrameworkElementAutomationPeer, IExpandCollapseProvider {
    private IndexCard ownerIndexCard;
    public IndexCardAutomationPeer(IndexCard owner) : base(owner)
    {
         ownerIndexCard = owner;
    }
}
```

Como alternativa de implementação, o próprio controle pode fazer referência a seu par. Esse é um padrão comum em caso de acionamento de eventos de automação do controle, pois [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent) é um método de par.

<span id="UI_Automation_events"/>
<span id="ui_automation_events"/>
<span id="UI_AUTOMATION_EVENTS"/>

## <a name="ui-automation-events"></a>Eventos de Automação da Interface do Usuário  

Os eventos de Automação da Interface do Usuário estão nas seguintes categorias.

| Evento | Descrição |
|-------|-------------|
| Alteração da propriedade | É disparado quando uma propriedade em um padrão de controle ou elemento de Automação da IU muda. Por exemplo, se um cliente precisa monitorar o controle de uma caixa de seleção do aplicativo, ele pode registrar-se para escutar um evento de alteração de propriedade na propriedade [**ToggleState**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.provider.itoggleprovider.togglestate). Quando o controle da caixa de seleção está marcado ou desmarcado, o provedor dispara o evento e o cliente pode agir conforme o necessário. |
| Ação de elemento | É disparado quando uma alteração na interface do usuário resulta de atividade do usuário ou programática. Por exemplo, quando um botão é clicado ou invocado por meio do padrão **Invoke**. |
| Alteração de estrutura | É disparado quando a estrutura da árvore de Automação da IU muda. A estrutura muda quando novos itens da interface do usuário se tornam visíveis, ocultos ou são removidos da área de trabalho. |
| Alteração global | É disparado quando ocorrem ações de interesse global do cliente, como quando o foco muda de um elemento para outro ou quando uma janela filha é fechada. Alguns eventos não indicam necessariamente que o estado da interface do usuário mudou. Por exemplo, se o usuário vai para um campo de entrada de texto e clica em um botão para atualizar o campo, um evento [**TextChanged**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.textbox.textchanged) é disparado, mesmo que o usuário não tenha realmente mudado o texto. Ao processar um evento, pode ser necessário que o aplicativo cliente verifique se algo realmente mudou antes de executar uma ação. |

<span id="AutomationEvents_identifiers"/>
<span id="automationevents_identifiers"/>
<span id="AUTOMATIONEVENTS_IDENTIFIERS"/>

### <a name="automationevents-identifiers"></a>Identificadores AutomationEvents  
Os eventos de Automação da Interface do Usuário são identificados por valores de [**AutomationEvents**](https://msdn.microsoft.com/library/windows/apps/BR209183). Os valores da enumeração identificam o tipo de evento de forma exclusiva.

<span id="Raising_events"/>
<span id="raising_events"/>
<span id="RAISING_EVENTS"/>

### <a name="raising-events"></a>Acionando eventos  
Os clientes de Automação de Interface do Usuário podem se inscrever nos eventos de automação. No modelo de par de automação, os pares de controles personalizados devem informar as alterações de estado do controle que são relevantes para a acessibilidade, chamando o método [**RaiseAutomationEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raiseautomationevent). Da mesma forma, quando um valor de propriedade de Automação de Interface do Usuário de chave é alterado, os pares de controle personalizados devem chamar o método [**RaisePropertyChangedEvent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.raisepropertychangedevent).

O próximo exemplo de código mostra como obter o objeto de par do código de definição do controle e chamar um método para disparar um evento desse par. Como uma otimização, o código determina se há algum ouvinte para esse tipo de evento. Disparar o evento e criar o objeto par somente quando há ouvintes evita sobrecarga desnecessária e ajuda o controle a continuar respondendo.


```csharp
if (AutomationPeer.ListenerExists(AutomationEvents.PropertyChanged))
{
    NumericUpDownAutomationPeer peer =
        FrameworkElementAutomationPeer.FromElement(nudCtrl) as NumericUpDownAutomationPeer;
    if (peer != null)
    {
        peer.RaisePropertyChangedEvent(
            RangeValuePatternIdentifiers.ValueProperty,
            (double)oldValue,
            (double)newValue);
    }
}
```

<span id="Peer_navigation"/>
<span id="peer_navigation"/>
<span id="PEER_NAVIGATION"/>

## <a name="peer-navigation"></a>Navegação de par  
Depois de localizar um par de automação, um cliente de Automação de Interface do Usuário pode navegar pela estrutura de par de um aplicativo chamando os métodos [**GetChildren**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildren) e [**GetParent**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getparent) do objeto de par. A navegação entre elementos de interface do usuário dentro de um controle é suportada pela implementação de par do método [**GetChildrenCore**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.peers.automationpeer.getchildrencore). O sistema de Automação da Interface do Usuário chama esse método para construir uma árvore de subelementos contida em um controle; por exemplo, itens de lista em uma caixa de lista. O método **GetChildrenCore** padrão na [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472) percorre a árvore visual de elementos para construir a árvore de pares de automação. Os controles personalizados podem substituir esse método para expor uma representação diferente dos elementos filho para os clientes de automação, retornando os pares de automação dos elementos que transmitem informações ou permitem a interação do usuário.

<span id="Native_automation_support_for_text_patterns"/>
<span id="native_automation_support_for_text_patterns"/>
<span id="NATIVE_AUTOMATION_SUPPORT_FOR_TEXT_PATTERNS"/>

## <a name="native-automation-support-for-text-patterns"></a>Suporte à automação nativo para padrões de texto  
Alguns dos pares de automação de aplicativo UWP padrão dão suporte ao padrão de controle para o padrão de texto ([**PatternInterface.Text**](https://msdn.microsoft.com/library/windows/apps/BR242496)). O suporte ocorre por meio de métodos nativos e os pares envolvidos não notam a interface de [**ITextProvider**](https://msdn.microsoft.com/library/windows/apps/BR242627) na herança (gerenciada). No entanto, se um cliente de Automação da Interface do Usuário gerenciado ou não gerenciado consulta os padrões do par, ele relata suporte ao padrão de texto e fornece comportamento para partes do padrão quando as APIs do cliente são chamadas.

Se você pretende derivar de um dos controles de texto de aplicativo UWP e também criar um par personalizado que deriva de um dos pares relacionados a texto, verifique as seções Comentários do par para saber mais sobre qualquer suporte a padrões de nível nativo. É possível acessar o comportamento de base nativo no par personalizado se você chamar a implementação base a partir da implementações da interface do provedor gerenciado, mas é difícil modificar o que a implementação base faz porque as interfaces nativas no par e no controle do proprietário não estão expostas. Normalmente você deve usar as implementações base como foram fornecidas (somente com base em chamada) ou substituir por completo a funcionalidade com seu próprio código gerenciado e não chamar a implementação de base. Esse último é um cenário avançado no qual você precisa de um bom conhecimento da estrutura de serviços de texto que é usada pelo controle para permitir os requisitos de acessibilidade durante o uso dessa estrutura.

<span id="AutomationProperties.AccessibilityView"/>
<span id="automationproperties.accessibilityview"/>
<span id="AUTOMATIONPROPERTIES.ACCESSIBILITYVIEW"/>

## <a name="automationpropertiesaccessibilityview"></a>AutomationProperties.AccessibilityView  
Além de proporcionar um par personalizado, você também pode ajustar a representação da exibição de grade para qualquer instância de controle, através da criação [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.automation.automationproperties.accessibilityview) em XAML. Isso não é implementado como parte de uma classe de pares, mas vamos mencioná-lo aqui porque é pertinente para suporte geral de acessibilidade para controles personalizados ou para modelos que você personalizar.

O principal cenário para uso de **AutomationProperties.AccessibilityView** é para omitir deliberadamente determinados controles em um modelo das exibições da Automação da Interface do Usuário, pois eles não contribuem significativamente para a visualização de acessibilidade do controle inteiro. Para evitar isso, defina **AutomationProperties.AccessibilityView** como "Raw".

<span id="Throwing_exceptions_from_automation_peers"/>
<span id="throwing_exceptions_from_automation_peers"/>
<span id="THROWING_EXCEPTIONS_FROM_AUTOMATION_PEERS"/>

## <a name="throwing-exceptions-from-automation-peers"></a>Gerando exceções de pares de automação  
As APIs que você implementa para seu suporte a par de automação são permitidas para gerar exceções. Espera-se que qualquer cliente da Automação da Interface do Usuário que esteja escutando seja robusto o suficiente para continuar mesmo após a geração da maioria das exceções. Muito provavelmente, o ouvinte busca uma árvore de automação totalmente ativa que inclua aplicativos diferentes dos seus, e é inaceitável para o design do cliente desativar o cliente inteiro apenas porque uma área da árvore gerou uma exceção de mesmo nível quando o cliente chamou suas APIs.

Para os parâmetros que são transmitidos para o seu par, é aceitável validar a entrada e, por exemplo, lançar [**ArgumentNullException**](https://msdn.microsoft.com/library/windows/apps/system.argumentnullexception) caso ele tenha passado **null** e isso não é um valor válido para sua implementação. Mas se houver operações subsequentes para serem executadas por seu par, lembre-se de que as interações do par com o controle de hospedagem têm alguma característica assíncrona em relação a elas. Tudo o que um par faz não necessariamente bloqueia o thread de interface de usuário no controle (e é muito provável que não faça isso). Portanto, pode haver situações em que um objeto estava disponível ou tinha determinadas propriedades quando o par foi criado ou um método de par de automação foi chamado pela primeira vez mas, no meio tempo, o estado do controle foi modificado. Nesses casos, há duas exceções dedicadas que o provedor pode gerar:

* Gerar [**ElementNotAvailableException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotavailableexception) caso você não consiga contatar o proprietário do par nem um elemento do par relacionado com base nas informações originais passadas por sua API. Por exemplo, você pode ter um par que esteja tentando executar seus métodos, mas o proprietário já foi removido da interface do usuário, como uma caixa de diálogo modal que tenha sido fechada. Para um cliente não .NET, isso é mapeado para [**UIA\_E\_ELEMENTNOTAVAILABLE**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).
* Gerar [**ElementNotEnabledException**](https://msdn.microsoft.com/library/system.windows.automation.elementnotenabledexception) caso ainda exista um proprietário, mas ele esteja em um modo, como [**IsEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.controls.control.isenabled)`=`**false**, que bloqueia algumas das mudanças programáticas específicas que seu par esteja tentando fazer. Para um cliente não .NET, isso é mapeado para [**UIA\_E\_ELEMENTNOTENABLED**](https://msdn.microsoft.com/library/windows/desktop/Ee671218).

Além disso, os pares devem ser relativamente conservadores no que diz respeito às exceções que eles geram usando seu suporte a par. A maioria dos clientes não pode tratar exceções de pares nem torná-los opções acionáveis para os usuários ao interagirem com o cliente. Portanto, as exceções inoperantes e de captura, sem serem geradas novamente em suas implementações de par, às vezes são uma estratégia melhor do que gerar exceções toda vez que o par tentar fazer alguma coisa que não funcione. Leve em consideração também que a maioria dos clientes da Automação da Interface do Usuário não é gravada em código gerenciado. Grande parte deles é gravada no COM e apenas verifica **S\_OK** em **HRESULT** sempre que chamam um método do cliente da Automação da Interface do Usuário que acaba acessando seu par.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [**FrameworkElementAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242472)
* [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185)
* [**OnCreateAutomationPeer**](https://msdn.microsoft.com/ibrary/windows/apps/windows.ui.xaml.uielement.oncreateautomationpeer)
* [Padrões e interfaces de controle](control-patterns-and-interfaces.md)
