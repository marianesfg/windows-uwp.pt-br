---
Description: As informações básicas de acessibilidade costumam ser categorizadas por nome, função e valor. Este tópico descreve códigos para ajudar seu aplicativo a expor as informações básicas de que tecnologias adaptativas precisam.
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
title: Expor informações básicas de acessibilidade
label: Expose basic accessibility information
template: detail.hbs
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ff19eea5a2fa57d4e5b2728ddbd87e5d99ff539a
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234138"
---
# <a name="expose-basic-accessibility-information"></a>Expor informações básicas de acessibilidade  



As informações básicas de acessibilidade costumam ser categorizadas por nome, função e valor. Este tópico descreve códigos para ajudar seu aplicativo a expor as informações básicas de que tecnologias adaptativas precisam.

<span id="accessible_name"/>
<span id="ACCESSIBLE_NAME"/>

## <a name="accessible-name"></a>Nome acessível  
Um nome acessível é uma cadeia de caracteres de texto curta e descritiva que o leitor de tela usa para anunciar um elemento de interface do usuário. Defina o nome acessível dos elementos de interface do usuário para que ele tenha um significado importante para a compreensão do conteúdo ou da interação com a interface do usuário. Esses elementos costumam incluir imagens, campos de entrada, botões, controles e regiões.

Esta tabela descreve como definir ou obter um nome acessível para vários tipos de elementos em uma IU XAML.

| Tipo de elemento | Descrição |
|--------------|-------------|
| Texto estático | Um nome acessível é determinado automaticamente a partir do texto visível (interno) para elementos [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) e [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock). Todo o texto nesse elemento é usado como o nome. Consulte [Nome a partir do texto interno](#name_from_inner_text). |
| Imagens | O elemento [**Image**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Image) XAML não tem um análogo direto ao atributo **alt** HTML de **img** e elementos similares. Use [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) ou a técnica de legendagem para fornecer um nome. Consulte [Nomes acessíveis para imagens](#images). |
| Elementos de formulário | O nome acessível para um elemento de formulário deve ser o mesmo que o rótulo exibido para esse elemento. Consulte [Rótulos e LabeledBy](#labels). |
| Botões e links | Por padrão, o nome acessível de um botão ou link se baseia no texto visível, usando as mesmas regras descritas em [Nome do texto interno](#name_from_inner_text). Nos casos em que um botão contém apenas uma imagem, use [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) para fornecer um equivalente apenas em texto da ação pretendida do botão. |

A maioria dos elementos de contêiner (como painéis) não promove o seu conteúdo como nome acessível. Isso é porque é o conteúdo do item que deve informar um nome e a função correspondente, não seu contêiner. O elemento contêiner pode relatar que é um elemento que tem filhos em uma representação de Automação de Interface do Usuário da Microsoft, de forma que a lógica de tecnologia adaptativa possa atravessá-lo. Mas os usuários de tecnologias adaptativas geralmente não precisam saber sobre os contêineres e, assim, a maioria dos contêineres não é nomeada.

<span id="role_value"/>
<span id="ROLE_VALUE"/>

## <a name="role-and-value"></a>Função e valor  
Os controles e outros elementos de interface do usuário que fazem parte do vocabulário XAML implementam o suporte à Automação de Interface do Usuário para relatar a função e o valor como parte de suas definições. Você pode usar ferramentas de Automação da Interface do Usuário para examinar as informações de função e valor para os controles, ou pode ler a documentação para as implementações de [**AutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationPeer) para cada controle. As funções disponíveis em uma estrutura de Automação da IU são definidas na enumeração [**AutomationControlType**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.AutomationControlType). Os clientes de Automação da Interface do Usuário, como tecnologias adaptativas, podem obter informações de função chamando métodos que a estrutura de Automação da IU expõe usando o **AutomationPeer** do controle.

Nem todos os controles têm um valor. Os controles que não têm um valor reportam essa informação à Automação da Interface do Usuário por meio de pares e padrões para os quais esse controle oferece suporte. Por exemplo, um elemento de formulário [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) tem um valor. Um tecnologia adaptativa pode ser um cliente de Automação da Interface do Usuário e pode descobrir que um valor existe e qual é esse valor. Nesse caso específico, o **TextBox** tem suporte para o padrão [**IValueProvider**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Provider.IValueProvider) por meio das definições [**TextBoxAutomationPeer**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.Peers.TextBoxAutomationPeer).

> [!NOTE]
> Nos casos em que você usa [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) ou outras técnicas para fornecer o nome acessível explicitamente, não inclua o mesmo texto como é usado pelas informações de função ou tipo de controle no nome acessível. Por exemplo, não inclua cadeias de caracteres como "botão" ou "lista" no nome. As informações de função e tipo vêm de uma propriedade de Automação da Interface do Usuário diferente (**LocalizedControlType**) que é fornecida pelo suporte de controle padrão para Automação da Interface do Usuário. Muitas tecnologias adaptativas anexam o **LocalizedControlType** ao nome acessível. Por isso, duplicar a função no nome acessível pode resultar em palavras repetidas desnecessariamente. Por exemplo, se você der a um controle [**Button**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Button) o nome acessível "botão" ou incluir "botão" como a última parte do nome, isso poderá ser lido pelos leitores de tela como "botão botão". Você deve testar esse aspecto de suas informações de acessibilidade usando o Narrador.

<span id="Influencing_the_UI_Automation_tree_views"/>
<span id="influencing_the_ui_automation_tree_views"/>
<span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"/>

## <a name="influencing-the-ui-automation-tree-views"></a>Influenciando as exibições de árvore de automação da IU  
A estrutura de Automação da IU tem um conceito de exibições em árvore, onde os clientes da Automação da Interface do Usuário podem recuperar as relações entre os elementos de uma interface do usuário usando três visualizações possíveis: bruto, de controle e de conteúdo. A visualização de controle é a visualização usada frequentemente por clientes da Automação da Interface do Usuário, pois fornece uma boa representação e organização dos elementos em uma interface do usuário que são interativos. As ferramentas de teste costumam permitir que você escolha qual modo de exibição de árvore usar quando a ferramenta apresenta a organização dos elementos.

Por padrão, qualquer classe derivada de [**controle**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.Control) e alguns outros elementos aparecerão no modo de exibição de controle quando a estrutura de automação da interface do usuário representar a interface do usuário para um aplicativo do Windows. Mas às vezes você não quer que um elemento apareça na exibição do controle devido à composição da interface do usuário, onde esse elemento está duplicando ou apresentando informações que não são importantes para os cenários de acessibilidade. Use a propriedade anexada [**AutomationProperties.AccessibilityView**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.automation.automationproperties.accessibilityviewproperty) para alterar como os elementos são expostos para os modos de exibição de árvore. Se você colocar um elemento na árvore **Raw**, a maioria das tecnologias adaptativas não reportará esse elemento como parte de seus modos de exibição. Para ver alguns exemplos de como isso funciona em controles existentes, abra o arquivo XAML de referência de design generic.xaml em um editor de texto e pesquise por **AutomationProperties.AccessibilityView** nos modelos.

<span id="name_from_inner_text"/>
<span id="NAME_FROM_INNER_TEXT"/>

## <a name="name-from-inner-text"></a>Nome do texto interno  
Para facilitar o uso de cadeias de caracteres que já existem na IU visível para valores de nome acessível, muitos dos controles e outros elementos de IU oferecem suporte para determinar automaticamente um nome acessível padrão com base no texto interno dentro do elemento ou a partir de valores de cadeias de caracteres com propriedades de conteúdo.

* [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock), [**RichTextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.RichTextBlock), [**TextBox**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBox) e **RichTextBlock** promovem o valor da propriedade **Text** como o nome acessível padrão.
* Qualquer subclasse [**ContentControl**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content) usa uma técnica "ToString" iterativa para encontrar cadeias de caracteres em seu valor [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content) e promove essas cadeias de caracteres como o nome acessível padrão.

> [!NOTE]
> Como imposto pela Automação da Interface do Usuário, o tamanho do nome acessível não pode exceder 2048 caracteres. Se uma cadeia de caracteres usada para determinação automática de nome acessível exceder esse limite, o nome acessível será truncado nesse momento.

<span id="images"/>
<span id="IMAGES"/>

## <a name="accessible-names-for-images"></a>Nomes acessíveis para imagens
Para oferecer suporte a leitores de tela e fornecer as informações básicas de identificação para cada elemento da interface do usuário, você pode ter que fornecer alternativas de texto para informações não textuais, como imagens e gráficos (excluindo todos os elementos puramente decorativos ou estruturais). Estes elementos não têm texto interno, assim, o nome acessível não terá um valor calculado. Você pode definir o nome acessível diretamente configurando a propriedade anexada [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) como mostrado neste exemplo.

XAML
```xml
<!-- Comment -->
<Image Source="product.png"
  AutomationProperties.Name="An image of a customer using the product."/>
```

Como alternativa, leve em consideração a inclusão de uma legenda de texto que apareça na IU visível e que também sirva como a informação de acessibilidade associada ao rótulo para o conteúdo de imagem. Aqui está um exemplo:

XAML
```xml
<Image HorizontalAlignment="Left" Width="480" x:Name="img_MyPix"
  Source="snoqualmie-NF.jpg"
  AutomationProperties.LabeledBy="{Binding ElementName=caption_MyPix}"/>
<TextBlock x:Name="caption_MyPix">Mount Snoqualmie Skiing</TextBlock>
```

<span id="labels"/>
<span id="LABELS"/>

## <a name="labels-and-labeledby"></a>Rótulos e LabeledBy  
A melhor maneira de associar um rótulo a um elemento de formulário é usar um [**TextBlock**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.TextBlock) com um **x:Name** para texto de rótulo e, então, configurar a propriedade anexada [**AutomationProperties.LabeledBy**](https://docs.microsoft.com/previous-versions/windows/silverlight/dotnet-windows-silverlight/ms591292(v=vs.95)) no elemento de formulário para fazer referência ao **TextBlock** rotulador pelo seu nome XAML. Se você usar esse padrão, quando o usuário clicar no rótulo, o foco se moverá para o controle associado e as tecnologias adaptativas poderão usar o texto de rótulo como o nome acessível para o campo de formulário. Aqui está um exemplo que mostra essa técnica.

XAML
```xml
<StackPanel x:Name="LayoutRoot" Background="White">
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_FirstName">First name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_FirstName}"
      Name="tbFirstName" Width="100"/>
   </StackPanel>
   <StackPanel Orientation="Horizontal">
     <TextBlock Name="lbl_LastName">Last name</TextBlock>
     <TextBox
      AutomationProperties.LabeledBy="{Binding ElementName=lbl_LastName}"
      Name="tbLastName" Width="100"/>
   </StackPanel>
 </StackPanel>
```

<span id="accessible_description"/>
<span id="ACCESSIBLE_DESCRIPTION"/>

## <a name="accessible-description-optional"></a>Descrição acessível (opcional)  
Uma descrição acessível oferece informações de acessibilidade adicionais sobre um elemento de IU específico. Normalmente, você fornece uma descrição acessível quando o nome acessível sozinho não informa adequadamente a finalidade do elemento.

O leitor de tela Narrador só lê a descrição acessível do elemento quando o usuário pede mais informações sobre o elemento, pressionando CapsLock + F.

O nome acessível se destina a identificar o controle, em vez de fazê-lo para documentar completamente seu comportamento. Se uma descrição breve não for suficiente para explicar o controle, você pode configurar a propriedade anexada [**AutomationProperties.HelpText**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.helptext), além de [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name).

<span id="Testing_accessibility_early_and_often"/>
<span id="testing_accessibility_early_and_often"/>
<span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"/>

## <a name="testing-accessibility-early-and-often"></a>Testando a acessibilidade de forma precoce e frequente  
Definitivamente, a melhor abordagem para suportar leitores de tela é testar o seu aplicativo usando, você mesmo, um leitor de tela. Isso lhe mostrará como se comporta o leitor de tela e as informações básicas de acessibilidade podem estar ausentes do aplicativo. Em seguida, você pode ajustar os valores da interface do usuário ou da propriedade de Automação da Interface do Usuário adequadamente. Para obter mais informações, consulte [Testes de acessibilidade](accessibility-testing.md).

Uma das ferramentas que você pode usar para testar a acessibilidade chama-se **AccScope**. A ferramenta **AccScope** é especialmente útil porque você pode ver representações visuais de sua IU que demostram como as tecnologias assistidas poderiam ver seu aplicativo como uma árvore de automação. Em particular, há um modo Narrador que dá uma visão de como o Narrador obtém o texto de seu aplicativo e como ele organiza os elementos na IU. A AccScope foi criada de modo que possa ser usada e útil ao longo do ciclo de desenvolvimento de um aplicativo, mesmo durante a fase preliminar de design. Para obter mais informações, consulte [AccScope](https://docs.microsoft.com/windows/desktop/WinAuto/accscope).

<span id="Accessible_names_from_dynamic_data"/>
<span id="accessible_names_from_dynamic_data"/>
<span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"/>

## <a name="accessible-names-from-dynamic-data"></a>Nomes acessíveis de dados dinâmicos  
O Windows oferece suporte a muitos controles que podem ser usados para exibir valores provenientes de uma fonte de dados associada, por meio de um recurso conhecido como *vinculação de dados*. Quando você preenche as listas com itens de dados, talvez precise usar uma técnica que define os nomes acessíveis para itens de lista vinculada a dados depois que a lista inicial é preenchida. Para obter mais informações, consulte o "Cenário 4" na [Amostra de acessibilidade XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample).

<span id="Accessible_names_and_localization"/>
<span id="accessible_names_and_localization"/>
<span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"/>

## <a name="accessible-names-and-localization"></a>Nomes acessíveis e localização  
Para verificar se o nome acessível também é um elemento que é localizado, você deve usar técnicas corretas para armazenar cadeias de caracteres localizáveis como recursos e fazer referência às conexões de recursos com valores da [diretiva x:Uid](https://docs.microsoft.com/windows/uwp/xaml-platform/x-uid-directive). Se o nome acessível vier de um uso [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) configurado explicitamente, certifique-se de que a cadeia de caracteres também possa ser localizada.

Observe que as propriedades anexadas, como as propriedades [**AutomationProperties**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Automation.AutomationProperties), usam uma sintaxe qualificadora especial para o nome de recurso, para que o recurso faça referência à propriedade anexada conforme se aplica a um elemento específico. Por exemplo, o nome de recurso para [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name) conforme se aplica a elementos de IU chamados `MediumButton` é: `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`.

<span id="related_topics"/>

## <a name="related-topics"></a>Tópicos relacionados  
* [Acessibilidade](accessibility.md)
* [**AutomationProperties.Name**](https://docs.microsoft.com/dotnet/api/system.windows.automation.automationproperties.name)
* [Amostra de acessibilidade XAML](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/411c271e537727d737a53fa2cbe99eaecac00cc0/Official%20Windows%20Platform%20Sample/XAML%20accessibility%20sample)
* [Testes de acessibilidade](accessibility-testing.md)
