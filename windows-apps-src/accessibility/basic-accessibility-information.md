---
Description: As informações básicas de acessibilidade costumam ser categorizadas por nome, função e valor. Este tópico descreve códigos para ajudar seu aplicativo a expor as informações básicas de que tecnologias adaptativas precisam.
title: Expor informações básicas de acessibilidade
ms.assetid: 9641C926-68C9-4842-8B55-C38C39A9E5C5
label: Expose basic accessibility information
template: detail.hbs
---

Expor informações básicas de acessibilidade
==========================================================================================================

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


As informações básicas de acessibilidade costumam ser categorizadas por nome, função e valor. Este tópico descreve códigos para ajudar seu aplicativo a expor as informações básicas de que tecnologias adaptativas precisam.

<span id="accessible_name"></span><span id="ACCESSIBLE_NAME"></span>Nome acessível
-----------------------------------------------------------------------------------

Um nome acessível é uma cadeia de caracteres de texto curta e descritiva que o leitor de tela usa para anunciar um elemento de interface do usuário. Defina o nome acessível dos elementos de interface do usuário para que ele tenha um significado importante para a compreensão do conteúdo ou da interação com a interface do usuário. Esses elementos costumam incluir imagens, campos de entrada, botões, controles e regiões.

Esta tabela descreve como definir ou obter um nome acessível para vários tipos de elementos em uma IU XAML.

| Tipo de elemento      | Descrição                                                                                                                                                                                                                                                                                                                                                            |
|-------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Texto estático       | Um nome acessível é determinado automaticamente a partir do texto visível (interno) para elementos [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) e [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565). Todo o texto nesse elemento é usado como o nome. Consulte [Nome a partir do texto interno](#name_from_inner_text).                                                                    |
| Imagens            | O elemento [**Image**](https://msdn.microsoft.com/library/windows/apps/BR242752) XAML não tem um análogo direto ao atributo **alt** HTML de **img** e elementos similares. Use [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) ou a técnica de legendagem para fornecer um nome. Consulte [Nomes acessíveis para imagens](#images).                                   |
| Elementos de formulário     | O nome acessível para um elemento de formulário deve ser o mesmo que o rótulo exibido para esse elemento. Consulte [Rótulos e LabeledBy](#labels).                                                                                                                                                                                                                        |
| Botões e links | Por padrão, o nome acessível de um botão ou link se baseia no texto visível, usando as mesmas regras descritas em [Nome do texto interno](#name_from_inner_text). Nos casos em que um botão contém apenas uma imagem, use [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) para fornecer um equivalente apenas em texto da ação pretendida do botão. |

 

A maioria dos elementos de contêiner (como painéis) não promove o seu conteúdo como nome acessível. Isso é porque é o conteúdo do item que deve informar um nome e a função correspondente, não seu contêiner. O elemento contêiner pode relatar que é um elemento que tem filhos em uma representação de Automação de Interface do Usuário da Microsoft, de forma que a lógica de tecnologia adaptativa possa atravessá-lo. Mas os usuários de tecnologias adaptativas geralmente não precisam saber sobre os contêineres e, assim, a maioria dos contêineres não é nomeada.

<span id="role_value"></span><span id="ROLE_VALUE"></span>Função e valor
------------------------------------------------------------------------

Os controles e outros elementos de interface do usuário que fazem parte do vocabulário XAML implementam o suporte à Automação de Interface do Usuário para relatar a função e o valor como parte de suas definições. Você pode usar ferramentas de Automação da Interface do Usuário para examinar as informações de função e valor para os controles, ou pode ler a documentação para as implementações de [**AutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR209185) para cada controle. As funções disponíveis em uma estrutura de Automação da IU são definidas na enumeração [**AutomationControlType**](https://msdn.microsoft.com/library/windows/apps/BR209182). Os clientes de Automação da Interface do Usuário, como tecnologias adaptativas, podem obter informações de função chamando métodos que a estrutura de Automação da IU expõe usando o **AutomationPeer** do controle.

Nem todos os controles têm um valor. Os controles que não têm um valor reportam essa informação à Automação da Interface do Usuário por meio de pares e padrões para os quais esse controle oferece suporte. Por exemplo, um elemento de formulário [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) tem um valor. Um tecnologia adaptativa pode ser um cliente de Automação da Interface do Usuário e pode descobrir que um valor existe e qual é esse valor. Nesse caso específico, o **TextBox** tem suporte para o padrão [**IValueProvider**](https://msdn.microsoft.com/library/windows/apps/BR242663) por meio das definições [**TextBoxAutomationPeer**](https://msdn.microsoft.com/library/windows/apps/BR242550).

**Nota**  Para casos em que você usa [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) ou outras técnicas para fornecer o nome acessível explicitamente, não inclua o mesmo texto como é usado pela função de controle ou a informação de tipo no nome acessível. Por exemplo, não inclua cadeias de caracteres como "botão" ou "lista" no nome. As informações de função e tipo vêm de uma propriedade de Automação da Interface do Usuário diferente (**LocalizedControlType**) que é fornecida pelo suporte de controle padrão para Automação da Interface do Usuário. Muitas tecnologias adaptativas anexam o **LocalizedControlType** ao nome acessível. Por isso, duplicar a função no nome acessível pode resultar em palavras repetidas desnecessariamente. Por exemplo, se você der a um controle [**Button**](https://msdn.microsoft.com/library/windows/apps/BR209265) o nome acessível "botão" ou incluir "botão" como a última parte do nome, isso poderá ser lido pelos leitores de tela como "botão botão". Você deve testar esse aspecto de suas informações de acessibilidade usando o Narrador.

 

<span id="Influencing_the_UI_Automation_tree_views"></span><span id="influencing_the_ui_automation_tree_views"></span><span id="INFLUENCING_THE_UI_AUTOMATION_TREE_VIEWS"></span>Influenciando os modos de exibição de árvore de automação da IU
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

A estrutura de Automação da IU tem um conceito de exibições em árvore, onde os clientes da Automação da Interface do Usuário podem recuperar as relações entre os elementos de uma interface do usuário usando três visualizações possíveis: bruto, de controle e de conteúdo. A visualização de controle é a visualização usada frequentemente por clientes da Automação da Interface do Usuário, pois fornece uma boa representação e organização dos elementos em uma interface do usuário que são interativos. As ferramentas de teste costumam permitir que você escolha qual modo de exibição de árvore usar quando a ferramenta apresenta a organização dos elementos.

Por padrão, qualquer classe derivada de [**Control**](https://msdn.microsoft.com/library/windows/apps/BR209390) e alguns outros elementos aparecerão na visualização de controle quando a estrutura de Automação da IU representar a IU para um aplicativo da Plataforma Universal do Windows (UWP). Mas às vezes você não quer que um elemento apareça na exibição do controle devido à composição da interface do usuário, onde esse elemento está duplicando ou apresentando informações que não são importantes para os cenários de acessibilidade. Use a propriedade anexada [**AutomationProperties.AccessibilityView**](https://msdn.microsoft.com/library/windows/apps/Dn251788) para alterar como os elementos são expostos para os modos de exibição de árvore. Se você colocar um elemento na árvore **Raw**, a maioria das tecnologias adaptativas não reportará esse elemento como parte de seus modos de exibição. Para ver alguns exemplos de como isso funciona em controles existentes, abra o arquivo XAML de referência de design generic.xaml em um editor de texto e pesquise por **AutomationProperties.AccessibilityView** nos modelos.

<span id="name_from_inner_text"></span><span id="NAME_FROM_INNER_TEXT"></span>Nome a partir do texto interno
--------------------------------------------------------------------------------------------------

Para facilitar o uso de cadeias de caracteres que já existem na IU visível para valores de nome acessível, muitos dos controles e outros elementos de IU oferecem suporte para determinar automaticamente um nome acessível padrão com base no texto interno dentro do elemento ou a partir de valores de cadeias de caracteres com propriedades de conteúdo.

-   [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652), [**RichTextBlock**](https://msdn.microsoft.com/library/windows/apps/BR227565), [**TextBox**](https://msdn.microsoft.com/library/windows/apps/BR209683) e **RichTextBlock** promovem o valor da propriedade **Text** como o nome acessível padrão.
-   Qualquer subclasse [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) usa uma técnica "ToString" iterativa para encontrar cadeias de caracteres em seu valor [**Content**](https://msdn.microsoft.com/library/windows/apps/BR209365_content) e promove essas cadeias de caracteres como o nome acessível padrão.

**Nota**   Como imposto pela Automação da Interface do Usuário, o comprimento do nome acessível não pode ter mais do que 2.048 caracteres. Se uma cadeia de caracteres usada para determinação automática de nome acessível exceder esse limite, o nome acessível será truncado nesse momento.

 

<span id="images"></span><span id="IMAGES"></span>Nomes acessíveis para imagens
-----------------------------------------------------------------------------

Para oferecer suporte a leitores de tela e fornecer as informações básicas de identificação para cada elemento da interface do usuário, você pode ter que fornecer alternativas de texto para informações não textuais, como imagens e gráficos (excluindo todos os elementos puramente decorativos ou estruturais). Estes elementos não têm texto interno, assim, o nome acessível não terá um valor calculado. Você pode definir o nome acessível diretamente configurando a propriedade anexada [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) como mostrado neste exemplo.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Image Source=&quot;product.png&quot;
  AutomationProperties.Name=&quot;An image of a customer using the product.&quot;/&gt;</code></pre></td>
</tr>
</tbody>
</table>

Como alternativa, leve em consideração a inclusão de uma legenda de texto que apareça na IU visível e que também sirva como a informação de acessibilidade associada ao rótulo para o conteúdo de imagem. Aqui está um exemplo:

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Image HorizontalAlignment=&quot;Left&quot; Width=&quot;480&quot; x:Name=&quot;img_MyPix&quot;
  Source=&quot;snoqualmie-NF.jpg&quot;
  AutomationProperties.LabeledBy=&quot;{Binding ElementName=caption_MyPix}&quot;/&gt;
&lt;TextBlock x:Name=&quot;caption_MyPix&quot;&gt;
Mount Snoqualmie Skiing
&lt;/TextBlock&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span id="labels"></span><span id="LABELS"></span>Rótulos e LabeledBy
----------------------------------------------------------------------

A melhor maneira de associar um rótulo a um elemento de formulário é usar um [**TextBlock**](https://msdn.microsoft.com/library/windows/apps/BR209652) com um **x:Name** para texto de rótulo e, então, configurar a propriedade anexada [**AutomationProperties.LabeledBy**](https://msdn.microsoft.com/library/windows/apps/Hh759769) no elemento de formulário para fazer referência ao **TextBlock** rotulador pelo seu nome XAML. Se você usar esse padrão, quando o usuário clicar no rótulo, o foco se moverá para o controle associado e as tecnologias adaptativas poderão usar o texto de rótulo como o nome acessível para o campo de formulário. Aqui está um exemplo que mostra essa técnica.

<span codelanguage="XAML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XAML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code> &lt;StackPanel x:Name=&quot;LayoutRoot&quot; Background=&quot;White&quot;&gt;
   &lt;StackPanel Orientation=&quot;Horizontal&quot;&gt;
     &lt;TextBlock Name=&quot;lbl_FirstName&quot;&gt;First name&lt;/TextBlock&gt;
     &lt;TextBox
      AutomationProperties.LabeledBy=&quot;{Binding ElementName=lbl_FirstName}&quot;
      Name=&quot;tbFirstName&quot; Width=&quot;100&quot;/&gt;
   &lt;/StackPanel&gt;
   &lt;StackPanel Orientation=&quot;Horizontal&quot;&gt;
     &lt;TextBlock Name=&quot;lbl_LastName&quot;&gt;Last name&lt;/TextBlock&gt;
     &lt;TextBox
      AutomationProperties.LabeledBy=&quot;{Binding ElementName=lbl_LastName}&quot;
      Name=&quot;tbLastName&quot; Width=&quot;100&quot;/&gt;
   &lt;/StackPanel&gt;
 &lt;/StackPanel&gt;</code></pre></td>
</tr>
</tbody>
</table>

<span id="accessible_description"></span><span id="ACCESSIBLE_DESCRIPTION"></span>Descrição acessível (opcional)
-------------------------------------------------------------------------------------------------------------------

Uma descrição acessível oferece informações de acessibilidade adicionais sobre um elemento de IU específico. Normalmente, você fornece uma descrição acessível quando o nome acessível sozinho não informa adequadamente a finalidade do elemento.

O leitor de tela Narrador só lê a descrição acessível do elemento quando o usuário pede mais informações sobre o elemento, pressionando Caps Lock + F.

O nome acessível se destina a identificar o controle, em vez de fazê-lo para documentar completamente seu comportamento. Se uma descrição breve não for suficiente para explicar o controle, você pode configurar a propriedade anexada [**AutomationProperties.HelpText**](https://msdn.microsoft.com/library/windows/apps/Hh759765), além de [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770).

<span id="Testing_accessibility_early_and_often"></span><span id="testing_accessibility_early_and_often"></span><span id="TESTING_ACCESSIBILITY_EARLY_AND_OFTEN"></span>Testando a acessibilidade cedo e com frequência
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Definitivamente, a melhor abordagem para suportar leitores de tela é testar o seu aplicativo usando, você mesmo, um leitor de tela. Isso lhe mostrará como se comporta o leitor de tela e as informações básicas de acessibilidade podem estar ausentes do aplicativo. Em seguida, você pode ajustar os valores da interface do usuário ou da propriedade de Automação da Interface do Usuário adequadamente. Para obter mais informações, consulte [Testes de acessibilidade](accessibility-testing.md).

Uma das ferramentas que você pode usar para testar a acessibilidade chama-se **AccScope**. A ferramenta **AccScope** é especialmente útil porque você pode ver representações visuais de sua IU que demostram como as tecnologias assistidas poderiam ver seu aplicativo como uma árvore de automação. Em particular, há um modo Narrador que dá uma visão de como o Narrador obtém o texto de seu aplicativo e como ele organiza os elementos na IU. A AccScope foi criada de modo que possa ser usada e útil ao longo do ciclo de desenvolvimento de um aplicativo, mesmo durante a fase preliminar de design. Para obter mais informações, consulte [AccScope](https://msdn.microsoft.com/library/windows/desktop/Dn433239).

<span id="Accessible_names_from_dynamic_data"></span><span id="accessible_names_from_dynamic_data"></span><span id="ACCESSIBLE_NAMES_FROM_DYNAMIC_DATA"></span>Nomes acessíveis a partir de dados dinâmicos
-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

O Windows oferece suporte a muitos controles que podem ser usados para exibir valores provenientes de uma fonte de dados associada, por meio de um recurso conhecido como *vinculação de dados*. Quando você preenche as listas com itens de dados, talvez precise usar uma técnica que define os nomes acessíveis para itens de lista vinculada a dados depois que a lista inicial é preenchida. Para obter mais informações, consulte o "Cenário 4" na [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570).

<span id="Accessible_names_and_localization"></span><span id="accessible_names_and_localization"></span><span id="ACCESSIBLE_NAMES_AND_LOCALIZATION"></span>Nomes acessíveis e localização
---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

Para verificar se o nome acessível também é um elemento que é localizado, você deve usar técnicas corretas para armazenar cadeias de caracteres localizáveis como recursos e fazer referência às conexões de recursos com valores da [diretiva x:Uid](https://msdn.microsoft.com/library/windows/apps/Mt204791). Se o nome acessível vier de um uso [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) configurado explicitamente, certifique-se de que a cadeia de caracteres também possa ser localizada.

Observe que as propriedades anexadas, como as propriedades [**AutomationProperties**](https://msdn.microsoft.com/library/windows/apps/BR209081), usam uma sintaxe qualificadora especial para o nome de recurso, para que o recurso faça referência à propriedade anexada conforme se aplica a um elemento específico. Por exemplo, o nome de recurso para [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770) conforme se aplica a elementos de IU chamados `MediumButton` é: `MediumButton.[using:Windows.UI.Xaml.Automation]AutomationProperties.Name`.

<span id="related_topics"></span>Tópicos relacionados
-----------------------------------------------

* [Acessibilidade](accessibility.md)
* [**AutomationProperties.Name**](https://msdn.microsoft.com/library/windows/apps/Hh759770)
* [Amostra de acessibilidade XAML](http://go.microsoft.com/fwlink/p/?linkid=238570)
* [Testes de acessibilidade](accessibility-testing.md)
 

 





<!--HONumber=Mar16_HO3-->


