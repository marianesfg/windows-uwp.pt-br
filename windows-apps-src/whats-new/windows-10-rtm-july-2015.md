---
Description: O Windows 10 e as novas ferramentas de desenvolvedor fornecem as ferramentas, os recursos e as experiências da nova UWP (Plataforma Universal do Windows).
title: Novidades para desenvolvedores no Windows 10, RTM - julho de 2015
---

# Novidades para desenvolvedores no Windows 10, RTM: julho de 2015


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O Windows 10 e as novas ferramentas de desenvolvedor fornecem as ferramentas, os recursos e as experiências da nova UWP (Plataforma Universal do Windows). Depois de [instalar as ferramentas e o SDK](https://dev.windows.com/downloads) no Windows 10, você estará pronto para [criar um aplicativo Universal do Windows](https://msdn.microsoft.com/library/windows/apps/bg124288) ou descobrir como pode usar seu [código de aplicativo existente no Windows](https://msdn.microsoft.com/library/windows/apps/mt238321).

Aqui está uma visão recurso por recurso do que há de novo para você no Windows 10, RTM.

## Layouts adaptáveis


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Vários modos de exibição para conteúdo personalizado</td>
<td align="left">O XAML oferece novo suporte para a definição de modos de exibição personalizados (arquivos .xaml) que compartilham o mesmo arquivo de código. Com isso, fica mais fácil para você criar e manter diferentes modos de exibição que são personalizados para uma família de dispositivos ou um cenário específico. Se seu aplicativo tem conteúdo de interface do usuário distinto, layout ou modelos de navegação que são bastante diferentes para diferentes cenários, crie vários modos de exibição. Por exemplo, você pode usar um [<strong>Pivot</strong>] (https://msdn.microsoft.com/library/windows/apps/dn608241) com navegação otimizada para uso com uma mão em seu aplicativo móvel, mas deve usar um [<strong>SplitView</strong>] (https://msdn.microsoft.com/library/windows/apps/dn864360) com um menu de navegação otimizado para mouse em seu aplicativo da área de trabalho.</td>
</tr>
<tr class="even">
<td align="left">StateTriggers</td>
<td align="left">Usando o novo recurso [<strong>VisualState.StateTriggers</strong>] recurso (https://msdn.microsoft.com/library/windows/apps/dn890396), você pode definir propriedades condicionalmente com base na altura/largura da janela ou com base em um gatilho personalizado. Anteriormente, você precisava manipular os eventos [<strong>SizeChanged</strong>] (https://msdn.microsoft.com/library/windows/apps/br209055) de janela em código e chamar [<strong>VisualStateManager. GoToState</strong>] (https://msdn.microsoft.com/library/windows/apps/br209025).</td>
</tr>
<tr class="odd">
<td align="left">Setters</td>
<td align="left"><p>Usando a nova sintaxe [<strong>VisualState.Setters</strong>] (https://msdn.microsoft.com/library/windows/apps/dn890395), você pode usar a marcação simplificada para definir alterações de propriedade no [<strong>VisualStateManager</strong>] (https://msdn.microsoft.com/library/windows/apps/br209021). Anteriormente, você precisava usar um Storyboard e criar animações para aplicar alterações de propriedade, como mudança da orientação de um [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) de Horizontal para Vertical. Em aplicativos universais do Windows, você pode usar esta sintaxe Setter mais simples:</p>
<p><code>&lt;setter target=&quot;stackPanel1.Orientation&quot; value=&quot;Vertical&quot; /&gt;</code></p></td>
</tr>
</tbody>
</table>

 

## Recursos XAML


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Vinculações de dados compilados (x:Bind)</td>
<td align="left"><p>Em aplicativos universais do Windows, você pode usar o novo mecanismo de associação com base em compilador habilitado pela propriedade x:Bind. As vinculações com base em compilador são fortemente tipadas e processadas no momento da compilação, o que é mais rápido e fornece erros de tempo de compilação quando os tipos de vinculação são incompatíveis. E como as associações são convertidas em código de aplicativo compilado, agora você pode depurar as associações percorrendo o código no Visual Studio para diagnosticar problemas específicos de associação. Você também pode usar o x:Bind para associar a um método, assim:</p>
<p><code>&lt;textblock text=&quot;{x:Bind Customer.Address.ToString()}&quot; /&gt;</code></p>
<p>Para cenários de vinculação comuns, você pode usar o x:Bind no lugar da vinculação e obter melhor desempenho e capacidade de manutenção.</p></td>
</tr>
<tr class="even">
<td align="left">Renderização incremental declarativa de listas (x:Phase)</td>
<td align="left"><p>Em aplicativos Universais do Windows, o novo atributo x:Phase permite realizar uma renderização de listas incremental ou em fases usando XAML em vez de código. Ao fazer movimento panorâmico de longas listas com itens complexos, seu aplicativo talvez não consiga renderizar itens de forma rápida o suficiente para acompanhar a velocidade do movimento panorâmico, provocando uma experiência ruim para seus usuários. A renderização em fases permite especificar a prioridade de renderização de elementos individuais em um item de lista, para que somente as partes mais importantes do item de lista são renderizadas em cenários de movimento panorâmico rápido. Isso proporciona uma experiência de movimento panorâmico mais suave para o usuário.</p>
<p>No Windows 8.1, você pode manipular o evento [<strong>ContainerContentChanging</strong>] (https://msdn.microsoft.com/library/windows/apps/dn298914) e escrever um código para renderizar itens de lista em fases. Em aplicativos UWP, você pode fazer a renderização em fases declarativamente usando o atributo x:Phase. Usado em conjunto com vinculações compiladas x:Bind, o x:Phase permite especificar facilmente uma prioridade de renderização para cada elemento associado em um modelo de dados. No movimento panorâmico, o trabalho para renderizar itens tem o tempo dividido com base na fase, que permite a renderização incremental de itens.</p></td>
</tr>
<tr class="odd">
<td align="left">Carregamento adiado de elementos de interface do usuário (x:DeferLoadingStrategy)</td>
<td align="left">Em aplicativos universais do Windows, a nova diretiva x:DeferLoadingStrategy permite especificar as partes da interface do usuário que terão o carregamento adiado, o que melhora o desempenho da inicialização e reduz o uso de memória de seu aplicativo. Por exemplo, se a interface do usuário de seu aplicativo tem um elemento para validação de dados que é mostrado somente quando dados incorretos são inseridos, você pode atrasar o carregamento desse elemento até que ele seja necessário. Assim, os objetos do elemento não são criados quando a página é carregada; em vez disso, eles são criados apenas quando há um erro de dados e não precisam ser adicionados à árvore visual da página.</td>
</tr>
<tr class="even">
<td align="left">SplitView</td>
<td align="left">O novo controle [<strong>SplitView</strong>] controle (https://msdn.microsoft.com/library/windows/apps/dn864360) oferece uma maneira de você mostrar e ocultar conteúdo transitório com facilidade. Ele costuma ser usado em cenários de navegação de nível superior como &quot;menu hambúrguer&quot;, onde o conteúdo da navegação é ocultado e desliza quando necessário como resultado de uma ação do usuário.</td>
</tr>
<tr class="odd">
<td align="left">RelativePanel</td>
<td align="left">[<strong>RelativePanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn879546) é um novo painel de layout que permite posicionar e alinhar objetos filhos em relação uns aos outros ou ao painel pai. Por exemplo, você pode especificar que um texto deve ser sempre posicionado no lado esquerdo do painel, e que um botão sempre deve estar alinhar abaixo do texto. Use <strong>RelativePanel</strong> para criar interfaces do usuário que não tenham um padrão linear claro que exigiria o uso de [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) ou [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704).</td>
</tr>
<tr class="even">
<td align="left">CalendarView</td>
<td align="left">O controle [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052) torna mais fácil exibir e selecionar datas e intervalos de datas com uma exibição personalizável baseada em meses. <strong>CalendarView</strong> oferece suporte a recursos como data mínima, data máxima e datas de blecaute para limitar quais datas podem ser selecionadas. Você também pode definir barras de densidade personalizadas que podem ser usadas para mostrar o &quot;preenchimento&quot; geral da agenda em um dia específico.</td>
</tr>
<tr class="odd">
<td align="left">CalendarDatePicker</td>
<td align="left">[<strong>CalendarDatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn950083) é um controle suspenso que é otimizado para selecionar uma única data em [<strong>CalendarView</strong>](https://msdn.microsoft.com/library/windows/apps/dn890052), onde informações contextuais como o dia da semana ou o preenchimento do calendário são importantes. É similar ao controle [<strong>DatePicker</strong>] controle (https://msdn.microsoft.com/library/windows/apps/dn298584), mas o <strong>DatePicker</strong> é otimizado para selecionar uma data conhecida, como uma data de nascimento.</td>
</tr>
<tr class="even">
<td align="left">MediaTransportControls</td>
<td align="left">A nova classe [<strong>MediaTransportControls</strong>] (https://msdn.microsoft.com/library/windows/apps/dn831962) torna mais fácil personalizar os controles de transporte de um [<strong>MediaElement</strong>] (https://msdn.microsoft.com/library/windows/apps/br242926). No Windows 8.1, você pode ativar os controles de transporte internos do <strong>MediaElement</strong> ou criar seus próprios controles de transporte que se chamam métodos <strong>MediaElement</strong>. Agora você pode usar a funcionalidade interna de <strong>MediaTransportControls</strong>, e ainda personalizar facilmente a aparência de acordo com seu aplicativo.</td>
</tr>
<tr class="odd">
<td align="left">Notificações de mudança de propriedade</td>
<td align="left"><p>Em aplicativos Universais do Windows, você pode ouvir as mudanças de propriedade em [<strong>DependencyObject</strong>] s (https://msdn.microsoft.com/library/windows/apps/br242356), até mesmo para as propriedades que não tenham eventos de alteração correspondentes.</p>
<p>A notificação funciona como um evento, mas na verdade é exposta como retorno de chamada. O retorno de chamada usa um argumento do remetente assim como um manipulador de eventos, mas não usa um argumento de evento. Em vez disso, apenas o identificador da propriedade é transmitido para indicar a propriedade. Com essas informações, seu aplicativo pode definir um único manipulador para várias notificações de propriedade. Para obter mais informações, consulte [<strong>RegisterPropertyChangedCallback</strong>] (https://msdn.microsoft.com/library/windows/apps/dn831902) e [<strong>UnregisterPropertyChangedCallback</strong>] (https://msdn.microsoft.com/library/windows/apps/dn831910).</p></td>
</tr>
<tr class="even">
<td align="left">Mapas</td>
<td align="left"><p>A classe [<strong>MapControl</strong>](https://msdn.microsoft.com/library/windows/apps/dn637004) foi atualizada para fornecer imagens aéreas 3D e exibições em nível de rua. Esses novos recursos e a funcionalidade de mapeamento anterior agora estão disponíveis para Aplicativos do Windows Universais. Adicione mapeamento ao seu aplicativo com as seguintes APIs:</p>
<ul>
<li>Namespace [<strong>Windows.UI.Xaml.Controls.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn610751) - Exiba mapas.</li>
<li>Namespace [<strong>Windows.Services.Maps</strong>](https://msdn.microsoft.com/library/windows/apps/dn636979) Encontre locais e rotas.</li>
</ul>
<p>Para começar a usar essas APIs em um aplicativo Universal do Windows hoje, solicite uma chave do [Bing Maps Developer Center](https://www.bingmapsportal.com/) Centro de Desenvolvimento do Bing Mapas. Para obter mais informações, consulte [How to authenticate a Maps app](https://msdn.microsoft.com/library/windows/apps/xaml/dn741528). Outra novidade no Windows 10 é que os usuários de computador e telefone podem baixar mapas offline do aplicativo Configurações. Quando disponíveis, os mapas offline são usados pelo [<strong>MapControl</strong>] (https://msdn.microsoft.com/library/windows/apps/dn637004) para exibir mapas quando não for possível acessar a Internet.</p></td>
</tr>
<tr class="odd">
<td align="left">Mapeamento de botões de entrada</td>
<td align="left">A classe [<strong>Windows.UI.Xaml.Input.KeyRoutedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/hh943072) tem uma nova propriedade [<strong>OriginalKey</strong>](https://msdn.microsoft.com/library/windows/apps/dn960168) que, juntamente com uma atualização correspondente para [<strong>Windows.System.VirtualKey</strong>](https://msdn.microsoft.com/library/windows/apps/br241812), permite que você obtenha o botão de entrada original, não mapeado, associado ao evento de entrada do teclado.</td>
</tr>
<tr class="even">
<td align="left">Escrita à tinta</td>
<td align="left"><p>Agora é mais simples usar a funcionalidade robusta de escrita à tinta em aplicativos do Windows Runtime em C++, C# ou Visual Basic, graças ao controle [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) e às classes [<strong>InkPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn922011) subjacentes.</p>
<p>O controle [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) define uma área de sobreposição para desenhar e renderizar traços de tinta. A funcionalidade desse controle (entrada, processamento e renderização) é proveniente das classes [<strong>InkPresenter</strong>] (https://msdn.microsoft.com/library/windows/apps/dn922011), [<strong>InkStroke</strong>] (https://msdn.microsoft.com/library/windows/apps/br208485), [<strong>InkRecognizer</strong>] (https://msdn.microsoft.com/library/windows/apps/br208478) e [<strong>InkSynchronizer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903979).</p>
<p></p>
<div class="alert">
<strong>Importante</strong>  Essas classes não são compatíveis com aplicativos do Windows que usam JavaScript.
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

## Recursos XAML atualizados


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Atualizações de CommandBar e AppBar</td>
<td align="left"><p>Os controles [<strong>CommandBar</strong>] (https://msdn.microsoft.com/library/windows/apps/dn279427) e [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) foram atualizados para ter API, comportamento e experiência do usuário consistentes para aplicativos UWP entre famílias de dispositivos.</p>
<p>O controle [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) para aplicativos Universais do Windows foi aprimorado para fornecer um superconjunto da funcionalidade [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) e maior flexibilidade em como você pode usá-lo em seu aplicativo. Você deve usar <strong>CommandBar</strong> para todos os novos aplicativos universais do Windows no Windows 10. Em um <strong>CommandBar</strong> no Windows 8.1, você pode usar apenas os controles que implementaram [<strong>ICommandBarElement</strong>](https://msdn.microsoft.com/library/windows/apps/dn251969), como [<strong>AppBarButton</strong>](https://msdn.microsoft.com/library/windows/apps/dn279244). Em aplicativos universais do Windows, agora você pode colocar conteúdo personalizado no <strong>CommandBar</strong> além de <strong>AppBarButtons</strong>.</p>
<p>O controle [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) foi atualizado para permitir que você mova mais facilmente seus aplicativos do Windows 8.1 que usam <strong>AppBar</strong> para a Plataforma Universal do Windows. O <strong>AppBar</strong> foi projetado para ser usado com aplicativos de tela inteira e ser invocado usando gestos de borda. As atualizações do controle referem-se a problemas como aplicativos de janela e a ausência de gestos de borda no Windows 10.</p>
<p>O [<strong>AppBar.ClosedDisplayMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn633872) oculto, anteriormente somente no Windows Phone, agora é compatível com todas as famílias de dispositivos, permitindo que você escolha entre diferentes níveis de dicas para comandos. O [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) mostra uma dica mínima por padrão para fornecer consistência ao atualizar seus aplicativos do Windows 8.1 para aplicativos universais do Windows, onde você não pode mais contar com o suporte para gesto de borda na plataforma.</p>
<p><strong>Nova API</strong> [<strong>AppBar</strong>](https://msdn.microsoft.com/library/windows/apps/hh701927) <strong></strong>[<strong>Closing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996483), [<strong>OnClosing</strong>](https://msdn.microsoft.com/library/windows/apps/dn996484), [<strong>Opening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996486), [<strong>OnOpening</strong>](https://msdn.microsoft.com/library/windows/apps/dn996485), [<strong>TemplateSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn996487).</p>
<p><strong>Nova API</strong> [<strong>CommandBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn279427) <strong></strong>[<strong>CommandBarOverflowPresenterStyle</strong>](https://msdn.microsoft.com/library/windows/apps/dn975227) e [<strong>CommandBarOverflowPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn975225).</p></td>
</tr>
<tr class="even">
<td align="left">Atualizações do GridView</td>
<td align="left">Antes do Windows 10, a orientação de layout padrão [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) era horizontal no Windows e vertical no Windows Phone. Em aplicativos UWP, <strong>GridView</strong> usa um layout vertical por padrão para todas as famílias de dispositivos para garantir que você tenha uma experiência padrão consistente.</td>
</tr>
<tr class="odd">
<td align="left">Propriedade AreStickyGroupHeadersEnabled</td>
<td align="left">Quando você mostra dados agrupados em [<strong>ListView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242878) ou [<strong>GridView</strong>] (https://msdn.microsoft.com/library/windows/apps/br242705), os cabeçalhos dos grupos agora permanecem visíveis quando a lista é rolada. Isso é importante em grandes conjuntos de dados onde o cabeçalho fornece contexto para os dados que o usuário está visualizando. No entanto, em casos onde há apenas alguns elementos em cada grupo, você pode fazer com que os cabeçalhos rolem para fora da tela com os itens. Você pode definir a propriedade [<strong>AreStickyGroupHeadersEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn932028) em [<strong>ItemsStackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/dn298795) e [<strong>ItemsWrapGrid</strong>](https://msdn.microsoft.com/library/windows/apps/dn298849) para controlar esse comportamento.</td>
</tr>
<tr class="even">
<td align="left">Método GroupHeaderContainerFromItemContainer</td>
<td align="left">Ao mostrar dados agrupados em [<strong>ItemsControl</strong>](https://msdn.microsoft.com/library/windows/apps/br242803), você pode chamar o método [<strong>GroupHeaderContainerFromItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903984) para obter uma referência ao cabeçalho pai do grupo. Por exemplo, se um usuário excluir o último item em um grupo, você poderá obter uma referência para o cabeçalho do grupo e remover o item e o cabeçalho do grupo juntos.</td>
</tr>
<tr class="odd">
<td align="left">Evento ChoosingGroupHeaderContainer</td>
<td align="left">O novo evento [<strong>ChoosingGroupHeaderContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn899082) em [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) permite que você defina o estado em cabeçalhos de grupos em [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) ou [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Por exemplo, você pode manipular esse evento para definir a [<strong>AutomationProperties.NameProperty</strong>](https://msdn.microsoft.com/library/windows/apps/br209099) no cabeçalho do grupo para representar o grupo de tecnologias assistenciais.</td>
</tr>
<tr class="even">
<td align="left">Evento ChoosingItemContainer</td>
<td align="left">O novo evento [<strong>ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) em [<strong>ListViewBase</strong>](https://msdn.microsoft.com/library/windows/apps/br242879) oferece maior controle sobre a virtualização da interface do usuário em [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) ou [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705). Use esse evento em conjunto com o evento [<strong>ContainerContentChanging</strong>](https://msdn.microsoft.com/library/windows/apps/dn298914) para manter sua própria fila de contêineres reciclados dos quais extrair dados conforme necessário. Por exemplo, se a fonte de dados for redefinida devido à filtragem, você poderá relacionar rapidamente um conjunto de elementos visuais (ItemContainers) já criado com seus dados para obter melhor desempenho.</td>
</tr>
<tr class="odd">
<td align="left">Virtualização da rolagem de listas</td>
<td align="left"><p>Os controles XAML [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) e [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) têm um novo evento [<strong>ListViewBase.ChoosingItemContainer</strong>](https://msdn.microsoft.com/library/windows/apps/dn903989) que melhora o desempenho do controle quando ocorre uma alteração na coleta de dados.</p>
<p>Em vez de fazer uma restauração completa da lista, que repete a animação de entrada, agora o sistema mantém os itens atualmente em exibição, juntamente com o estado de seleção e foco; os itens novos e removidos no visor são animados e reduzidos facilmente. Após uma alteração na coleção de dados em que os contêineres não são destruídos, o aplicativo pode combinar itens &quot;antigos&quot; com seu contêiner anterior rapidamente e ignorar o processamento adicional de métodos de substituição do ciclo de vida do contêiner. Somente os &quot;novos&quot; itens são processados e associados a contêineres reciclados ou novos.</p></td>
</tr>
<tr class="even">
<td align="left">Método SelectRange e propriedade SelectedRanges</td>
<td align="left">Em aplicativos Universais do Windows, os controles [<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) e [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) agora permitem que você selecione itens em termos de intervalos de índices de itens, em vez de referências a objetos de itens. Isso é uma maneira mais eficiente de descrever a seleção de itens porque os objetos de item não precisam ser criados para cada item selecionado. Para obter mais informações, consulte [<strong>ListViewBase.SelectedRanges</strong>](https://msdn.microsoft.com/library/windows/apps/dn904244), [<strong>ListViewBase.SelectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904245), e [<strong>ListViewBase.DeselectRange</strong>](https://msdn.microsoft.com/library/windows/apps/dn904241).</td>
</tr>
<tr class="odd">
<td align="left">Novas APIs ListViewItemPresenter</td>
<td align="left">[<strong>ListView</strong>](https://msdn.microsoft.com/library/windows/apps/br242878) e [<strong>GridView</strong>](https://msdn.microsoft.com/library/windows/apps/br242705) usam apresentadores de itens para fornecer os elementos visuais padrão para seleção e foco. Em aplicativos UWP, [<strong>ListViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn298500) e [<strong>GridViewItemPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/dn279298) têm novas propriedades que permitem que você personalize elementos visuais para itens de lista. As novas propriedades são [<strong>CheckBoxBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn913905), [<strong>CheckMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn913923), [<strong>FocusSecondaryBorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn898370), [<strong>PointerOverForeground</strong>](https://msdn.microsoft.com/library/windows/apps/dn898372), [<strong>PressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913931), e [<strong>SelectedPressedBackground</strong>](https://msdn.microsoft.com/library/windows/apps/dn913937).</td>
</tr>
<tr class="even">
<td align="left">Atualizações de SemanticZoom</td>
<td align="left"><p>Agora o controle [<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) tem um comportamento consistente para aplicativos UWP em todas as famílias de dispositivos.</p>
<p>A ação padrão para alternar entre a exibição ampliada e a exibição reduzida é tocar em um cabeçalho de grupo na exibição ampliada. Esse comportamento é o mesmo no Windows Phone 8.1, mas é uma alteração do Windows 8.1, que usava o gesto de pinça para aplicar zoom. Para alterar os modos de exibição usando pinçar para aplicar zoom, defina [<strong>ScrollViewer.ZoomMode</strong>](https://msdn.microsoft.com/library/windows/apps/br209601)=&quot;Enabled&quot; do[<strong>SemanticZoom</strong>](https://msdn.microsoft.com/library/windows/apps/hh702601) no [<strong>ScrollViewer</strong>] interno (https://msdn.microsoft.com/library/windows/apps/br209527).</p>
<p>Para aplicativos Universais do Windows, a exibição reduzida substitui a exibição ampliada e tem o mesmo tamanho da exibição que foi substituída. Esse comportamento é o mesmo no Windows 8.1, mas é uma alteração do Windows Phone 8.1, onde a exibição reduzida ocupava o tamanho total da tela e era renderizada sobre todos os outros tipos de conteúdo.</p></td>
</tr>
<tr class="odd">
<td align="left">Atualizações de DatePicker e TimePicker</td>
<td align="left">Agora os controles [<strong>DatePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn298584) e [<strong>TimePicker</strong>](https://msdn.microsoft.com/library/windows/apps/dn299280) têm uma implementação consistente para aplicativos universais do Windows em todas as famílias de dispositivos. Eles também têm um novo visual para o Windows 10. A parte pop-up do agora usa os controles [<strong>DatePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn625013) e [<strong>TimePickerFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn608313) em todos os dispositivos. Esse comportamento é o mesmo no Windows Phone 8.1, mas é uma alteração do Windows 8.1, que usava controles [<strong>ComboBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209348). O uso dos controles de submenu permite que você crie seletores de data e hora personalizados com mais facilidade.</td>
</tr>
<tr class="even">
<td align="left">Novas APIs ScrollViewer</td>
<td align="left">[<strong>ScrollViewer</strong>](https://msdn.microsoft.com/library/windows/apps/br209527) tem novos eventos [<strong>DirectManipulationStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858544) e [<strong>DirectManipulationCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn858543) para notificar seu aplicativo quando o movimento panorâmico por touch inicia e para. Você pode manipular esses eventos para coordenar sua interface do usuário com essas ações do usuário.</td>
</tr>
<tr class="odd">
<td align="left">Atualizações de MenuFlyout</td>
<td align="left">Em aplicativos Universais do Windows, há novas APIs que permitem que você crie menus de contexto melhores com mais facilidade. O novo método [<strong>MenuFlyout.ShowAt</strong>](https://msdn.microsoft.com/library/windows/apps/dn913174) permite que você especifique onde deseja que o submenu apareça em relação a outro elemento. (E seu [<strong>MenuFlyout</strong>](https://msdn.microsoft.com/library/windows/apps/dn299030) ainda pode sobrepor os limites da janela do aplicativo.) Use a nova classe [<strong>MenuFlyoutSubItem</strong>](https://msdn.microsoft.com/library/windows/apps/dn913170) para criar menus em cascata.</td>
</tr>
<tr class="even">
<td align="left">Novas propriedades de borda para ContentPresenter, Grid e StackPanel</td>
<td align="left">Controles de contêiner comuns têm novas propriedades de borda que lhe permitem desenhar uma borda ao redor delas sem adicionar mais um elemento Border ao seu XAML. [<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378), [<strong>Grid</strong>](https://msdn.microsoft.com/library/windows/apps/br242704), e [<strong>StackPanel</strong>](https://msdn.microsoft.com/library/windows/apps/br209635) têm estas novas propriedades: [<strong>BorderBrush</strong>](https://msdn.microsoft.com/library/windows/apps/dn960179), [<strong>BorderThickness</strong>](https://msdn.microsoft.com/library/windows/apps/dn960181), [<strong>CornerRadius</strong>](https://msdn.microsoft.com/library/windows/apps/dn960183), e [<strong>Padding</strong>](https://msdn.microsoft.com/library/windows/apps/dn960187).</td>
</tr>
<tr class="odd">
<td align="left">Novas APIs de texto em ContentPresenter</td>
<td align="left">[<strong>ContentPresenter</strong>](https://msdn.microsoft.com/library/windows/apps/br209378) tem novas APIs que oferecem a você mais controle sobre a exibição de texto: [<strong>LineHeight</strong>](https://msdn.microsoft.com/library/windows/apps/dn903982), [<strong>LineStackingStrategy</strong>](https://msdn.microsoft.com/library/windows/apps/dn831933), [<strong>MaxLines</strong>](https://msdn.microsoft.com/library/windows/apps/dn831935), e [<strong>TextWrapping</strong>](https://msdn.microsoft.com/library/windows/apps/dn831937).</td>
</tr>
<tr class="even">
<td align="left">Elementos visuais de foco do sistema</td>
<td align="left">Agora os elementos visuais de foco para controles XAML são criados pelo sistema, em vez de ser declarados como elementos XAML no modelo de controle. Os elementos visuais de foco geralmente não são necessários em dispositivos móveis, e permitir que o sistema crie-os e gerencie-os conforme necessário melhora o desempenho do aplicativo. Se você precisar de maior controle sobre elementos visuais de foco, substitua o comportamento do sistema e forneça um modelo de controle personalizado que define os elementos visuais de foco. Consulte [<strong>UseSystemFocusVisuals</strong>](https://msdn.microsoft.com/library/windows/apps/dn899076) e [<strong>IsTemplateFocusTarget</strong>](https://msdn.microsoft.com/library/windows/apps/dn899074) para obter mais informações.</td>
</tr>
<tr class="odd">
<td align="left">PasswordBox.PasswordRevealMode</td>
<td align="left"><p>Em aplicativos Uuniversais do Windows, a propriedade [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) substitui a propriedade [<strong>IsPasswordRevealButtonEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/hh702579) para fornecer um comportamento consistente entre famílias de dispositivos.</p>
<p></p>
<div class="alert">
<strong>Cuidado</strong>  Antes do Windows 10, o botão para revelar a senha não era mostrado por padrão; nos aplicativos Universais do Windows, ele é mostrado por padrão. Se a segurança de seu aplicativo exigir que a senha esteja sempre oculta, defina [<strong>PasswordRevealMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn890867) como Hidden.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">Control.IsTextScaleFactorEnabled</td>
<td align="left">A propriedade [<strong>IsTextScaleFactorEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn624997) que estava disponível no Windows Phone 8.1 agora está disponível para aplicativos Universais do Windows em todas as famílias de dispositivos.</td>
</tr>
<tr class="odd">
<td align="left">AutoSuggestBox</td>
<td align="left">Agora o controle [<strong>AutoSuggestBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn633874) do Windows Phone 8.1 está disponível para aplicativos Universais do Windows em todas as famílias de dispositivos, e você deve usá-lo no lugar de [<strong>SearchBox</strong>](https://msdn.microsoft.com/library/windows/apps/dn252771). <strong>AutoSuggestBox</strong> fornece sugestões conforme o usuário digita e funciona bem com diversos tipos de entrada, como toque, teclado e editores de método de entrada. Ele também tem alguns novos membros para fazê-lo funcionar melhor como uma caixa de pesquisa: a propriedade [<strong>QueryIcon</strong>](https://msdn.microsoft.com/library/windows/apps/dn996494) e o evento [<strong>QuerySubmitted</strong>](https://msdn.microsoft.com/library/windows/apps/dn996497).</td>
</tr>
<tr class="even">
<td align="left">ContentDialog</td>
<td align="left">Agora o controle [<strong>ContentDialog</strong>](https://msdn.microsoft.com/library/windows/apps/dn633972) do Windows Phone 8.1 está disponível para aplicativos Universais do Windows em todas as famílias de dispositivos. <strong>ContentDialog</strong> permite exibir uma caixa de diálogo modal personalizável que funcione bem em toda a gama de dispositivos.</td>
</tr>
<tr class="odd">
<td align="left">Pivô</td>
<td align="left">Agora o controle [<strong>Pivot</strong>](https://msdn.microsoft.com/library/windows/apps/dn608241) do Windows Phone 8.1 está disponível para aplicativos Universais do Windows em todas as famílias de dispositivos. Agora você pode usar o mesmo controle <strong>Pivot</strong> em seu aplicativo para dispositivos móveis e de área de trabalho. <strong>Pivot</strong> proporciona comportamento adaptável com base no tamanho da tela e no tipo de entrada. Você pode definir o estilo de um controle <strong>Pivot</strong> para fornecer o comportamento de tipo guia, com diferentes modos de exibição de informações em cada item de pivô.</td>
</tr>
</tbody>
</table>

 

## Text


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">APIs de texto essenciais do Windows</td>
<td align="left"><p>O novo namespace [<strong>Windows.UI.Text.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958238) tem um sistema cliente-servidor que centraliza o processamento das entradas do teclado em um único servidor.</p>
<p>Você pode usá-lo para manipular o buffer de edição de seu controle de entrada de texto personalizado. O servidor de entrada de texto garante que o conteúdo de seu texto de controle de entrada e o conteúdo do seu próprio buffer de edição esteja sempre em sincronia, por meio de um canal de comunicação assíncrona entre o aplicativo e o servidor.</p></td>
</tr>
<tr class="even">
<td align="left">Ícones de vetor</td>
<td align="left">O elemento [<strong>Glyphs</strong>](https://msdn.microsoft.com/library/windows/apps/br209921) tem as novas propriedades [<strong>IsColorFontEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn900468) e [<strong>ColorFontPalleteIndex</strong>](https://msdn.microsoft.com/library/windows/apps/dn900466) para oferecer suporte a fontes coloridas; agora você pode usar um arquivo de fonte para renderizar ícones baseados em fontes. Quando você usa <strong>ColorFontPalleteIndex</strong> para alternância de paleta de cores, um único ícone pode ser renderizado com conjuntos de cores diferentes; por exemplo, para mostrar uma versão habilitada e desabilitada do ícone.</td>
</tr>
<tr class="odd">
<td align="left">Eventos de janela do Editor de Método de Entrada</td>
<td align="left">Às vezes, os usuários inserem texto por meio de um Editor de Método de Entrada que aparece em uma janela logo abaixo de uma caixa de entrada de texto (geralmente para idiomas do Leste Asiático). Você pode usar o evento [<strong>CandidateWindowBoundsChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn955144) e a propriedde [<strong>DesiredCandidateWindowAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/dn955145) em [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) e [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) para fazer com que a interface do usuário de seu aplicativo funcione melhor com a janela do IME.</td>
</tr>
<tr class="even">
<td align="left">Eventos de composição de texto</td>
<td align="left">[<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683) e [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) têm novos eventos para informar ao seu aplicativo quando o texto é composto com um Editor de Método de Entrada: [<strong>TextCompositionStarted</strong>](https://msdn.microsoft.com/library/windows/apps/dn891458), [<strong>TextCompositionEnded</strong>](https://msdn.microsoft.com/library/windows/apps/dn891457), e [<strong>TextCompositionChanged</strong>](https://msdn.microsoft.com/library/windows/apps/dn891456). Você pode manipular esses eventos para coordenar o código do aplicativo com o processo de composição de texto do IME. Por exemplo, você pode implementar a funcionalidade de preenchimento automático embutida para idiomas do Leste Asiático.</td>
</tr>
<tr class="odd">
<td align="left">Manuseio aprimorado de texto bidirecional</td>
<td align="left"><p>Os controles de texto XAML têm uma nova API para melhorar a manipulação de texto bidirecional, resultando em melhor alinhamento de texto e direcionalidade de parágrafo em uma variedade de idiomas de entrada.</p>
<p>O valor padrão da propriedade [<strong>TextReadingOrder</strong>](https://msdn.microsoft.com/library/windows/apps/dn949133) foi alterado para <strong>DetectFromContent</strong>, a fim de que o suporte para detecção da ordem de leitura seja habilitado por padrão. A propriedade <strong>TextReadingOrder</strong> também foi adicionada a [<strong>PasswordBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227519), [<strong>RichEditBox</strong>](https://msdn.microsoft.com/library/windows/apps/br227548) e [<strong>TextBox</strong>](https://msdn.microsoft.com/library/windows/apps/br209683).</p>
<p>Você pode definir a propriedade [<strong>TextAlignment</strong>](https://msdn.microsoft.com/library/windows/apps/br209703) em controles de texto para o novo valor de <strong>DetectFromContent</strong> para aceitar que o alinhamento seja detectado automaticamente a partir do conteúdo.</p></td>
</tr>
<tr class="even">
<td align="left">Renderização de texto</td>
<td align="left">No Windows 10, o texto em aplicativos XAML agora é renderizado, na maioria das situações, em quase duas vezes a velocidade do Windows 8.1. Na maioria dos casos, os aplicativos se beneficiarão dessa melhoria sem qualquer alteração. Além da renderização mais rápida, essas melhorias também reduzem o consumo de memória típico de aplicativos XAML em 5%.</td>
</tr>
</tbody>
</table>

 

## Modelo de aplicativo


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Cortana</td>
<td align="left"><p>Estenda a funcionalidade básica da <strong>Cortana</strong> com comandos de voz que iniciam e executam uma única ação em um aplicativo externo.</p>
<p>Ao integrar a funcionalidade básica do seu aplicativo e fornecer um ponto de entrada central para o usuário realizar a maioria das tarefas sem abrir o aplicativo diretamente, a <strong>Cortana</strong> pode atuar como uma ligação entre seu aplicativo e o usuário. Em muitos casos, isso pode economizar muito tempo e esforço do usuário.</p>
<p>Saiba como [integrate your app into the Cortana canvas](https://msdn.microsoft.com/library/windows/apps/xaml/dn974230). Se você precisar de ideias, consulte as recomendações de design e as diretrizes de experiência do usuário específicas à <strong>Cortana</strong> em [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics).</p></td>
</tr>
<tr class="even">
<td align="left">Explorador de Arquivos</td>
<td align="left">Os novos métodos [<strong>Windows.System.Launcher.LaunchFolderAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn889616) permitem que você inicie o Explorador de Arquivos e veja o conteúdo da pasta que você especificar.</td>
</tr>
<tr class="odd">
<td align="left">Armazenamento compartilhado</td>
<td align="left">A nova classe [<strong>Windows.ApplicationModel.DataTransfer.SharedStorageAccessManager</strong>](https://msdn.microsoft.com/library/windows/apps/dn889985) e seus métodos permitem que você compartilhe um arquivo com outro aplicativo passando um token de compartilhamento ao iniciar o outro aplicativo usando a ativação de URI. O aplicativo de destino resgata o token para acessar o arquivo compartilhado pelo aplicativo de origem.</td>
</tr>
<tr class="even">
<td align="left">Configurações</td>
<td align="left"><p>Exiba páginas de configurações integradas usando o protocolo ms-settings com o método [<strong>LaunchUriAsync</strong>](https://msdn.microsoft.com/library/windows/apps/hh701476). Por exemplo, o seguinte código exibe a página de configurações de Wi-Fi.</p>
<p><code>bool result = await Launcher.LaunchUriAsync(new Uri(&quot;ms-settings://network/wifi&quot;));</code></p>
<p>Para obter uma lista das páginas de configurações que você pode exibir, consulte [How to display built-in settings pages by using the ms-settings protocol](https://msdn.microsoft.com/library/windows/apps/jj207014.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Comunicação de aplicativo a aplicativo</td>
<td align="left"><p>As novas APIs de [app-to-app communication](https://msdn.microsoft.com/library/windows/apps/xaml/dn997827) do Windows 10 permitem que aplicativos do Windows (bem como Windows Web apps) iniciem uns aos outros e troquem dados e arquivos.</p>
<p>Usando essas novas APIs, tarefas complexas que exigiriam que o usuário usasse vários aplicativos podem agora ser executadas perfeitamente. Por exemplo, seu aplicativo pode iniciar um aplicativo de rede social para selecionar um contato ou iniciar um aplicativo de check-out para concluir um processo de pagamento.</p></td>
</tr>
<tr class="even">
<td align="left">Serviços de aplicativo</td>
<td align="left">Um serviço de aplicativo é uma maneira de um aplicativo fornecer serviços a outros aplicativos no Windows 10. Um serviço de aplicativo assume a forma de tarefa em segundo plano. Os aplicativos em primeiro plano podem chamar um serviço de aplicativo em outro aplicativo para executar tarefas em segundo plano. Para consultar informações sobre a API de serviço do aplicativo, veja [<strong>Windows.ApplicationModel.AppService</strong>] (https://msdn.microsoft.com/library/windows/apps/dn921731).</td>
</tr>
<tr class="odd">
<td align="left">Manifesto do pacote do aplicativo</td>
<td align="left"><p>As atualizações para a referência de [package manifest schema](https://msdn.microsoft.com/library/windows/apps/br211474) (esquema do manifesto do pacote) para o Windows 10 incluem elementos que foram adicionados, removidos e alterados.</p>
<p>Consulte [Element Hierarchy](https://msdn.microsoft.com/library/windows/apps/dn934819) (hierarquia de elementos) para obter informações sobre todos os elementos, atributos e tipos no esquema.</p></td>
</tr>
</tbody>
</table>

 

## Dispositivos


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Microsoft Surface Hub</td>
<td align="left"><p>O Microsoft Surface Hub é um dispositivo de colaboração em equipe eficiente e uma plataforma de tela grande para aplicativos Universais do Windows que são executados nativamente a partir do Surface Hub ou de seu dispositivo conectado.</p>
<p>Crie seus próprios aplicativos, projetados especificamente para sua empresa, que aproveitam as vantagens de tela grande, entrada por toque e à tinta e hardware onboard extensivo, como câmeras e sensores.</p>
<p>Veja as recomendações de design e diretrizes de experiência do usuário específicas ao Surface Hub nas [Design basics for Universal Windows apps](https://dev.windows.com/design/design-basics) (Noções básicas de design para Aplicativos Universais do Windows). Esses documentos explicam técnicas de design responsivo para aplicativos Universais do Windows.</p>
<p>Para obter detalhes sobre suporte a aplicativos públicos, consulte [<strong>SharedModeSettings</strong>](https://msdn.microsoft.com/library/windows/apps/dn949019).</p>
<p>Para saber mais sobre entrada à tinta e obter detalhes sobre suporte para escrita à tinta multiponto no novo controle [<strong>InkCanvas</strong>](https://msdn.microsoft.com/library/windows/apps/dn858535) consulte [<strong>Windows.UI.Input.Inking</strong>](https://msdn.microsoft.com/library/windows/apps/br208524) e [<strong>Windows.UI.Input.Inking.Core</strong>](https://msdn.microsoft.com/library/windows/apps/dn958452).</p>
<p>Para manipular a entrada do sensor, consulte [Integrating devices, printers, and sensors](https://msdn.microsoft.com/library/windows/apps/br229563) (Integrando dispositivos, impressoras e sensores).</p></td>
</tr>
<tr class="even">
<td align="left">Localização</td>
<td align="left"><p>O Windows 10 apresenta um novo método para solicitar ao usuário permissão para acessar sua localização: [<strong>RequestAccessAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn859152).</p>
<p>O usuário define a privacidade de seus dados de localização com as <strong>configurações de privacidade de localização</strong> no aplicativo <strong>Configurações</strong>. Seu aplicativo pode acessar a localização do usuário somente quando:</p>
<ul>
<li><strong>A localização deste dispositivo</strong> estiver ativada (não aplicável ao Windows 10 para telefones).</li>
<li>A configuração de serviços de localização “<strong>Localização</strong>” estiver <strong>ativada</strong>.</li>
<li>Em <strong>Escolher aplicativos que podem usar sua localização</strong>, seu aplicativo está definido como <strong>ativado</strong>.</li>
</ul>
<p>É importante chamar [<strong>RequestAccessAsync</strong>] (https://msdn.microsoft.com/library/windows/apps/dn859152) antes de acessar a localização do usuário. Nesse momento, seu aplicativo deve estar em primeiro plano e <strong>RequestAccessAsync</strong> deve ser chamado do thread da interface do usuário. Até que o usuário conceda permissão para a localização a seu aplicativo, o aplicativo não pode acessar os dados de localização.</p></td>
</tr>
<tr class="odd">
<td align="left">AllJoyn</td>
<td align="left"><p>O namespace [<strong>Windows.Devices.AllJoyn</strong>](https://msdn.microsoft.com/library/windows/apps/dn894971) do Windows Runtime apresenta a implementação da estrutura e serviços de software de código aberto AllJoyn da Microsoft. Essas APIs permitem que seu aplicativo de dispositivo Windows universal participe em cenários da Internet das Coisas (IoT) relacionados ao AllJoyn com outros dispositivos. Para obter mais detalhes sobre as APIs AllJoyn C, baixe a documentação da [The AllSeen Alliance](https://allseenalliance.org/).</p>
<p>Use a ferramenta [AllJoynCodeGen tool](https://msdn.microsoft.com/library/windows/apps/dn913809) fornecida nesta versão para gerar um componente do Windows que você possa usar para permitir cenários com AllJoyn em seu aplicativo de dispositivo.</p>
<div class="alert">
<strong>Observação</strong>  agora o Windows 10 IoT Core está disponível para uma nova classe de dispositivos pequenos, permitindo que você crie dispositivos para a "Internet das Coisas" (IoT) usando o Windows e o Visual Studio. Saiba mais sobre a Windows IoT em [WindowsOnDevices.com](http://www.windowsondevices.com/).
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left">APIs de impressão no celular (XAML)</td>
<td align="left">Há um conjunto unificado de APIs que permite imprimir conteúdo de seus aplicativos UWP baseados em XAML entre famílias de dispositivos, incluindo dispositivos móveis. Agora você pode adicionar impressão ao seu aplicativo móvel usando APIs de impressão conhecidas nos namespaces [<strong>Windows.Graphics.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br226489) e [<strong>Windows.UI.Xaml.Printing</strong>](https://msdn.microsoft.com/library/windows/apps/br243325).</td>
</tr>
<tr class="odd">
<td align="left">Bateria</td>
<td align="left"><p>As APIs de bateria no namespace [<strong>Windows.Devices.Power</strong>](https://msdn.microsoft.com/library/windows/apps/dn895017) permitem que seu aplicativo saiba mais sobre as baterias que estão conectadas ao dispositivo que está executando o aplicativo.</p>
<p>Crie um objeto [<strong>Battery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895004) para representar um controlador de bateria individual ou uma agregação de todos os controladores de bateria (quando criado por [<strong>FromIdAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn895013) ou [<strong>AggregateBattery</strong>](https://msdn.microsoft.com/library/windows/apps/dn895011), respectivamente).</p>
<p>Use o método [<strong>GetReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895015) para retornar um objeto [<strong>BatteryReport</strong>](https://msdn.microsoft.com/library/windows/apps/dn895005) que indica a carga, a capacidade e o status das baterias correspondentes.</p></td>
</tr>
<tr class="even">
<td align="left">Dispositivos MIDI</td>
<td align="left"><p>O novo namespace [<strong>Windows.Devices.Midi</strong>](https://msdn.microsoft.com/library/windows/apps/dn895002) permite que você crie:</p>
<ul>
<li>Aplicativos que podem se comunicar com dispositivos MIDI externos.</li>
<li>Os aplicativos e dispositivos externos que se comunicam diretamente com o sintetizador de software Microsoft GS MIDI.</li>
<li>Cenários onde vários clientes acessam simultaneamente uma única porta MIDI.</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Suporte para sensor personalizado</td>
<td align="left">O namespace [<strong>Windows.Devices.Sensors.Custom</strong>](https://msdn.microsoft.com/library/windows/apps/dn895032) permite que os desenvolvedores de hardware definam novos tipos de sensores personalizados, como um sensor de CO2.</td>
</tr>
<tr class="even">
<td align="left">Emulação de cartão com base em host (HCE)</td>
<td align="left"><p>A emulação de cartão host permite que você implemente serviços de emulação de cartão NFC hospedados no sistema operacional e ainda se comunique com o terminal do leitor externo via rádio NFC.</p>
<p>Implemente uma tarefa em segundo plano para emular um cartão inteligente via NFC. Para disparar a tarefa em segundo plano, use a classe [<strong>SmartCardTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn624853).</p>
<p>O valor de <strong>EmulatorHostApplicationActivated</strong> na enumeração [<strong>SmartCardTriggerType</strong>](https://msdn.microsoft.com/library/windows/apps/dn608017) permite que seu aplicativo saiba que ocorreu um evento HCE.</p></td>
</tr>
</tbody>
</table>

 

## Elementos gráficos


|                      |                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                    |
|----------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| DirectX              | O DirectX 12 no Windows 10 apresenta a próxima versão do Microsoft Direct3D, a API gráfica 3D na base do DirectX. [Os elementos gráficos do Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821) oferecem a eficiência e o desempenho de uma API tipo console de baixo nível. O Direct3D 12 está mais rápido e mais eficiente do que nunca. Ele proporciona cenas mais detalhadas, mais objetos, efeitos mais complexos e o melhor uso de hardware de elementos gráficos modernos.                                                                                                                                                                                                                                                                                     |
| SoftwareBitmapSource | Em aplicativos Universais do Windows, você pode usar o novo tipo [**SoftwareBitmapSource**](https://msdn.microsoft.com/library/windows/apps/dn997854) como fonte de imagem XAML. Isso permite transmitir imagens descodificadas para a estrutura XAML ser exibida imediatamente na tela, ignorando a decodificação da imagem pela estrutura XAML. Você pode conseguir uma renderização de imagem muito mais rápida, como a renderização de fotos de baixa latência diretamente da câmera, usando decodificadores de imagem personalizados, capturando quadros de superfícies do DirectX ou até mesmo criando imagens na memória do zero e renderizando-as diretamente em XAML com baixa latência e sobrecarga de memória baixa.                                                                                                     |
| Câmera de perspectiva   | Em aplicativos universais do Windows, o XAML tem uma nova API Transform3D que permite aplicar transformações de perspectiva a uma árvore (ou cena) XAML, que transforma todos os elementos XAML filho de acordo com essa única transformação de cena (ou câmera). Você podia fazer isso anteriormente usando MatrixTransform e cálculos complexos, mas o Transform3D simplifica esse efeito e também permite que o efeito seja animado. Para saber mais, confira a propriedade [**UIElement.Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn906919), [**Transform3D**](https://msdn.microsoft.com/library/windows/apps/dn914748), [**CompositeTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914714) e [**PerspectiveTransform3D**](https://msdn.microsoft.com/library/windows/apps/dn914740). |

 

## Mídia


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">HTTP Live Streaming</td>
<td align="left">Você pode usar a nova classe [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912) para adicionar recursos de streaming de vídeo adaptáveis aos seus aplicativos. O objeto é inicializado apontando-o para um arquivo de manifesto de streaming. Os formatos de manifesto com suporte incluem HLS (HTTP Live Streaming) e DASH (Dynamic Adaptive Streaming over HTTP). Depois que o objeto é vinculado a um elemento de mídia XAML, a reprodução adaptável é iniciada. As propriedades do fluxo, como as taxas de bits disponíveis, mínimas e máximas, podem ser consultadas e definidas quando apropriado.</td>
</tr>
<tr class="even">
<td align="left">Suporte para XVP (Transcode Video Processor) do Media Foundation para MFTs (Media Foundation Transforms)</td>
<td align="left"><p>Agora os aplicativos do Windows que usam MFTs (Media Foundation Transforms) podem usar o <strong>XVP (Transcode Video Processor) do Media Foundation</strong> para converter, dimensionar e transformar dados brutos de vídeo:</p>
<p>O novo atributo [MF_XVP_CALLER_ALLOCATES_OUTPUT](https://msdn.microsoft.com/library/windows/desktop/dn803919) permite a saída para texturas alocadas pelo chamador mesmo no modo DXVA (Microsoft DirectX Video Acceleration).</p>
<p>A nova interface [<strong>IMFVideoProcessorControl2</strong>](https://msdn.microsoft.com/library/windows/desktop/dn800741) permite que seu aplicativo habilite efeitos de hardware, consulte efeitos de hardware com suporte e substitua a operação de rotação realizada pelo processador de vídeo.</p></td>
</tr>
<tr class="odd">
<td align="left">Transcodificação</td>
<td align="left">A nova API [<strong>MediaProcessingTrigger</strong>](https://msdn.microsoft.com/library/windows/apps/dn806005) permite que seu aplicativo execute a transcodificação de mídia em uma tarefa em segundo plano, para que suas operações de transcodificação possam continuar mesmo quando seu aplicativo em primeiro plano for encerrado.</td>
</tr>
<tr class="even">
<td align="left">Eventos de falha de mídia MediaElement</td>
<td align="left">Em aplicativos Universais do Windows, o [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) reproduzirá conteúdo com vários fluxos mesmo se houver um erro na decodificação de um dos fluxos, desde que o conteúdo da mídia tenha, no mínimo, um fluxo válido. Por exemplo, se o fluxo de vídeo em um conteúdo que tem um fluxo de áudio e vídeo falhar, o <strong>MediaElement</strong> ainda reproduzirá o fluxo de áudio. O evento [<strong>PartialMediaFailureDetected</strong>](https://msdn.microsoft.com/library/windows/apps/dn889635) avisa que um dos fluxos dentro de um fluxo não pôde ser decodificado. Ele também permite que você saiba que tipo de fluxo falhou para poder refletir essas informações na interface do usuário. Se todos os fluxos dentro de um fluxo de mídia falhar, o evento [<strong>MediaFailed</strong>](https://msdn.microsoft.com/library/windows/apps/br227393) será lançado.</td>
</tr>
<tr class="odd">
<td align="left">Suporte para transmissão de vídeo adaptável com MediaElement</td>
<td align="left">[<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) tem o novo método [<strong>SetPlaybackSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn899085) para oferecer suporte à transmissão de vídeo adaptável. Use esse método para definir sua fonte de mídia para um [<strong>AdaptiveMediaSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn946912).</td>
</tr>
<tr class="even">
<td align="left">Transmissão com MediaElement e Image</td>
<td align="left">Os controles [<strong>MediaElement</strong>](https://msdn.microsoft.com/library/windows/apps/br242926) e [<strong>Image</strong>](https://msdn.microsoft.com/library/windows/apps/br242752) têm o novo método [<strong>GetAsCastingSource</strong>](https://msdn.microsoft.com/library/windows/apps/dn920012). Você pode usar esse método para enviar conteúdo de qualquer elemento de mídia ou imagem programaticamente para uma variedade maior de dispositivos remotos, como Miracast, Bluetooth e DLNA.Essa funcionalidade é habilitada automaticamente quando você define [<strong>AreTransportControlsEnabled</strong>](https://msdn.microsoft.com/library/windows/apps/dn298977) como true em um <strong>MediaElement</strong>.</td>
</tr>
<tr class="odd">
<td align="left">Controles de transporte de mídia para aplicativos de área de trabalho</td>
<td align="left">A interface [<strong>ISystemMediaTransportControls</strong>](https://msdn.microsoft.com/library/windows/desktop/dn892299) e as APIs relacionadas permitem que os aplicativos de área de trabalho interajam com os controles de transporte de mídia de sistema internos. Isso inclui responder às interações do usuário com os botões de controle de transporte e atualizar a exibição dos controles de transporte para mostrar metadados sobre reprodução atual de conteúdo de mídia.</td>
</tr>
<tr class="even">
<td align="left">Codificação e decodificação JPEG de acesso aleatório</td>
<td align="left">Os novos métodos WIC [<strong>IWICJpegFrameEncode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903864) e [<strong>IWICJpegFrameDecode</strong>](https://msdn.microsoft.com/library/windows/desktop/dn903834) permitem a codificação e decodificação de imagens JPEG. Você agora também pode habilitar a indexação de dados de imagens, o que fornece acesso aleatório eficiente a imagens grandes graças ao volume de memória maior.</td>
</tr>
<tr class="odd">
<td align="left">Sobreposições para composições de mídia</td>
<td align="left">As novas APIs [<strong>MediaOverlay</strong>](https://msdn.microsoft.com/library/windows/apps/dn764793) e [<strong>MediaOverlayLayer</strong>](https://msdn.microsoft.com/library/windows/apps/dn764795) facilitam adicionar várias camadas de conteúdo de mídia estático ou dinâmico a uma composição de mídia. Opacidade, posição e tempo podem ser ajustados para cada camada, e você ainda pode implementar seu próprio compositor personalizado para camadas de entrada.</td>
</tr>
<tr class="even">
<td align="left">Nova estrutura de efeitos</td>
<td align="left">O namespace [<strong>Windows.Media.Effects</strong>](https://msdn.microsoft.com/library/windows/apps/dn278802) fornece uma estrutura simples e intuitiva para adicionar efeitos a fluxos de áudio e vídeo. A estrutura inclui interfaces básicas que você pode implementar para criar efeitos de vídeo e áudio personalizados e inseri-los no pipeline da mídia.</td>
</tr>
</tbody>
</table>

 

## Rede


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Soquetes</td>
<td align="left"><p>As atualizações de soquete incluem:</p>
<ul>
<li><strong>Agente de soquete.</strong> O agente de soquete pode estabelecer e encerrar conexões de soquete em nome de um aplicativo em qualquer estado do ciclo de vida do aplicativo. Isso torna os aplicativos e os serviços que eles fornecem mais detectáveis. Por exemplo, pelo agente de soquete, um serviço Win32 ainda pode aceitar conexões de soquete de entrada mesmo quando não estando em execução.</li>
<li><strong>Melhorias de produtividade.</strong> A produtividade do soquete foi otimizada para aplicativos que usam o namespace [<strong>Windows.Networking.Sockets</strong>](https://msdn.microsoft.com/library/windows/apps/br226960).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Tarefas pós-processamento de transferência em segundo plano</td>
<td align="left">As novas APIs do namespace [<strong>Windows.Networking.BackgroundTransfer</strong>](https://msdn.microsoft.com/library/windows/apps/br207242) permitem que você registre grupos de tarefas de pós-processamento. Seu aplicativo pode agir de acordo com o sucesso ou a falha das transferências em segundo plano imediatamente, mesmo não estando em primeiro plano, em vez de aguardar até a próxima vez em que o usuário retomar o aplicativo.</td>
</tr>
<tr class="odd">
<td align="left">Suporte a Bluetooth para anúncios</td>
<td align="left">Com o namespace [<strong>Windows.Devices.Bluetooth.Advertisement</strong>](https://msdn.microsoft.com/library/windows/apps/dn894325), seus aplicativos podem enviar, receber e filtrar anúncios Bluetooth LE.</td>
</tr>
<tr class="even">
<td align="left">Atualização da API do Wi-Fi Direct</td>
<td align="left"><p>O agente de dispositivo está atualizado para habilitar o emparelhamento com dispositivos sem sair do aplicativo. As inclusões ao namespace [<strong>Windows.Devices.WiFiDirect</strong>](https://msdn.microsoft.com/library/windows/apps/dn297687) também permitem que um dispositivo torne-se detectável para outros dispositivos e que ele escute as notificações de conexão de entrada.</p>
<div class="alert">
<strong>Observação</strong>  Nesta versão, as melhorias do recurso Wi-Fi Direct não são incorporadas à experiência do usuário e oferecem suporte apenas para o emparelhamento com o pressionar de um botão. Além disso, essa versão oferece suporte apenas a uma conexão ativa.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left">Melhorias do suporte para JSON</td>
<td align="left">Agora o namespace [<strong>Windows.Data.Json</strong>](https://msdn.microsoft.com/library/windows/apps/br240639) oferece melhor suporte a definições padrão existentes e à experiência do desenvolvedor ao converter objetos JSON durante sessões de depuração.</td>
</tr>
</tbody>
</table>

 

## Segurança


|                             |                                                                      |
|-----------------------------|----------------------------------------------------------------------|
| Criptografia ECC              | As novas APIs no namespace [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404) oferecem suporte para a criptografia ECC (Elliptical Curve Cryptography), uma implementação de criptografia de chave pública com base em curvas elípticas sobre campos finitos. A ECC é matematicamente mais complexa que a RSA, fornece tamanhos de chave menores, reduz o consumo de memória e melhora o desempenho. Ela oferece a clientes e serviços Microsoft uma alternativa para chaves RSA e parâmetros de curva NIST aprovados. |
| Microsoft Passport          | Microsoft Passport é um método alternativo de autenticação que substitui as senhas por criptografia assimétrica e um gesto. As classes no namespace [**Credentials**](https://msdn.microsoft.com/library/windows/apps/br227089), como [**KeyCredentialManager**](https://msdn.microsoft.com/library/windows/apps/dn973043), tornam mais fácil para os desenvolvedores criarem aplicativos usando o Microsoft Passport sem a complexidade da criptografia ou biometria.  |
| Microsoft Passport for Work | Microsoft Passport for Work é um método alternativo para entrar no Windows usando sua conta do Active Directory do Azure que não usa senhas, cartões inteligentes físicos e virtuais. Você pode optar por habilitar ou desabilitar essa configuração de política. |
| Token Broker                | O Token Broker é uma nova estrutura de autenticação que torna mais fácil para os aplicativos se conectarem a provedores de identidade online (como o Facebook). Recursos como gerenciamento de nome de usuário e senha da conta e uma interface do usuário simplificada oferecem uma experiência de autenticação aprimorada para os usuários. |

 

## Serviços do sistema


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Energia</td>
<td align="left"><p>Seu aplicativo da área de trabalho do Windows agora pode ser notificado quando a economia de bateria estiver ativada ou desativada. Respondendo às mudanças nas condições de energia, seu aplicativo tem a oportunidade de ajudar a estender a duração da bateria.</p>
<p>[<strong>GUID_POWER_SAVING_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/hh448380): Use esse novo GUID com a função [<strong>PowerSettingRegisterNotification</strong>](https://msdn.microsoft.com/library/windows/desktop/hh769082) para ser notificado quando a economia de bateria está ativada ou desativada.</p>
<p>[<strong>SYSTEM_POWER_STATUS</strong>](https://msdn.microsoft.com/library/windows/desktop/aa373232): essa estrutura foi atualizada para oferecer suporte à economia de bateria. O quarto membro, <strong>SystemStatusFlag</strong> (anteriormente chamado Reserved1), agora indica se a economia de bateria está ativada ou não. Use a função [<strong>GetSystemPowerStatus</strong>](https://msdn.microsoft.com/library/windows/desktop/aa372693) para recuperar um ponteiro para essa estrutura.</p></td>
</tr>
<tr class="even">
<td align="left">Versão</td>
<td align="left"><p>Você pode usar as [Version Helper functions](https://msdn.microsoft.com/library/windows/desktop/dn424972) (funções auxiliares de versão) para determinar a versão do sistema operacional. Para o Windows 10, essas funções auxiliares incluem uma nova função, [<strong>IsWindows10OrGreater</strong>](https://msdn.microsoft.com/library/windows/desktop/dn905474). Você deve usar as funções auxiliares em vez das funções preteridas [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) e [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) quando quiser determinar a versão do sistema. Para mais informações sobre como obter a versão do sistema, consulte [Getting the System Version](https://msdn.microsoft.com/library/windows/desktop/ms724429).</p>
<p>Se você usar a função preterida [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) ou a função [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439) para obter informações sobre a versão em uma estrutura [<strong>OSVERSIONINFOEX</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724833) ou [<strong>OSVERSIONINFO</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724834), lembre-se de que o número da versão que essas estruturas contêm aumenta de 6.3, para o Windows 8.1 e o Windows Server 2012 R2, para 10.0, para o Windows 10. Para obter mais informações sobre números de versão para o sistema operacional, consulte [Operating System Version](https://msdn.microsoft.com/library/windows/desktop/ms724832).</p>
<p>Você também precisa direcionar seu aplicativo especificamente para o Windows 8.1 ou Windows 10 para obter as informações de versão corretas para essas versões com a função [<strong>GetVersionEx</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724451) ou [<strong>GetVersion</strong>](https://msdn.microsoft.com/library/windows/desktop/ms724439). Para obter informações sobre como direcionar seu aplicativo para essas versões do Windows, consulte [Targeting your application for Windows](https://msdn.microsoft.com/library/windows/desktop/dn481241) (Direcionando seu aplicativo para o Windows).</p></td>
</tr>
<tr class="odd">
<td align="left">Informações do usuário</td>
<td align="left">Novas APIs no namespace [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) facilitam o acesso a informações sobre um usuário, como seu nome de usuário e imagem da conta. Também fornecem a capacidade de responder a eventos do usuário, como logon e logoff.</td>
</tr>
<tr class="even">
<td align="left">Gerenciamento de memória e criação de perfil</td>
<td align="left">O suporte à API de criação de perfil de memória [<strong>Windows.System</strong>](https://msdn.microsoft.com/library/windows/apps/br241814) foi estendido para todas as plataformas, e sua funcionalidade geral foi aprimorada com novas classes e funções.</td>
</tr>
</tbody>
</table>

 

## Armazenamento


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">APIs de pesquisa de arquivo disponíveis para o Windows Phone</td>
<td align="left"><p>Como editor de aplicativos, você pode registrar seu aplicativo para compartilhar uma pasta de armazenamento com outros aplicativos que publica, adicionando extensões no manifesto do aplicativo. Em seguida, chame o método [<strong>Windows.Storage.ApplicationData.GetPublisherCacheFolder</strong>](https://msdn.microsoft.com/library/windows/apps/dn889607) para obter o local de armazenamento compartilhado.</p>
<p>O modelo de segurança forte dos aplicativos do Windows Runtime normalmente impede que aplicativos compartilhem dados entre si. Mas pode ser útil para aplicativos do mesmo fornecedor compartilharem arquivos e configurações por usuário.</p></td>
</tr>
</tbody>
</table>

 

## Ferramentas


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Live Visual Tree no Visual Studio</td>
<td align="left">O Visual Studio tem um novo recurso Live Visual Tree. Você pode usá-lo durante a depuração para saber rapidamente o estado da árvore visual de seu aplicativo e descobrir como as propriedades dos elementos foram definidas. Ele também permite alterar os valores de propriedade enquanto seu aplicativo está em execução, para que você possa ajustar e experimentar sem precisar abrir novamente.</td>
</tr>
<tr class="even">
<td align="left">Log de rastreamento</td>
<td align="left"><p>[TraceLogging](https://msdn.microsoft.com/library/windows/desktop/dn904636) é uma nova API de rastreamento de eventos para aplicativos de modo de usuário e drivers de modo kernel; ela é baseada no ETW [Event Tracing for Windows](https://msdn.microsoft.com/library/windows/desktop/bb968803) (Rastreamento de Eventos para Windows). Essa API oferece uma maneira simplificada de instrumentar código e incluir dados estruturados com eventos sem a necessidade de um arquivo XML de manifesto de instrumentação separado.</p>
<p>As APIs WinRT, .NET e C/C++ TraceLogging estão disponíveis para atender a diferentes públicos de desenvolvedores.</p></td>
</tr>
</tbody>
</table>

 

## Experiência do usuário


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Reconhecimento de fala</td>
<td align="left">O reconhecimento de fala contínua para cenários de ditado de formato longo agora é compatível com a Plataforma Universal do Windows. Veja como habilitar o ditado contínuo nos documentos de interação de fala.</td>
</tr>
<tr class="even">
<td align="left">Recursos de arrastar e soltar entre plataformas de aplicativos diferentes</td>
<td align="left"><p>Os novos namespaces [<strong>Windows.ApplicationModel.DataTransfer.DragDrop</strong>](https://msdn.microsoft.com/library/windows/apps/dn894216) levam a funcionalidade de arrastar e soltar para aplicativos Universais do Windows. Anteriormente, os cenários comuns de arrastar e soltar de programas de área de trabalho — como arrastar um documento de uma pasta para uma mensagem de email do Outlook para anexá-lo — não eram possíveis com aplicativos universais do Windows. Usando essas novas APIs, seu aplicativo pode permitir que os usuários movam dados facilmente entre diferentes aplicativos Universais do Windows e a área de trabalho.</p>
<p>Para oferecer suporte para arrastar e soltar entre aplicativos, estas novas APIs foram adicionadas ao XAML:</p>
<ul>
<li>[<strong>ListViewBase.DragItemsCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn831959)</li>
<li>[<strong>UIElement</strong>](https://msdn.microsoft.com/library/windows/apps/br208911): [<strong>CanDrag</strong>](https://msdn.microsoft.com/library/windows/apps/dn903558), [<strong>DragStarting</strong>](https://msdn.microsoft.com/library/windows/apps/dn903560), [<strong>StartDragAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn903562), [<strong>DropCompleted</strong>](https://msdn.microsoft.com/library/windows/apps/dn903561)</li>
<li>[<strong>DragOperationDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831917), [<strong>DragUI</strong>](https://msdn.microsoft.com/library/windows/apps/dn985714), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985715)</li>
<li>[<strong>DragEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/br242372): [<strong>AcceptedOperation</strong>](https://msdn.microsoft.com/library/windows/apps/dn831912), [<strong>DataView</strong>](https://msdn.microsoft.com/library/windows/apps/dn831913), [<strong>DragUIOverride</strong>](https://msdn.microsoft.com/library/windows/apps/dn985710), [<strong>GetDeferral</strong>](https://msdn.microsoft.com/library/windows/apps/dn831914), [<strong>Modifiers</strong>](https://msdn.microsoft.com/library/windows/apps/dn831915)</li>
<li>[<strong>DragItemsCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn831953), [<strong>DropCompletedEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903549), [<strong>DragStartingEventArgs</strong>](https://msdn.microsoft.com/library/windows/apps/dn903540)</li>
</ul></td>
</tr>
<tr class="odd">
<td align="left">Barras de título de janela personalizadas</td>
<td align="left">Para aplicativos UWP para a família de dispositivos de desktop, agora você pode usar a classe [<strong>ApplicationViewTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906115) com a propriedade [<strong>ApplicationView.TitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn906131) e o método [<strong>Window.SetTitleBar</strong>](https://msdn.microsoft.com/library/windows/apps/dn965560) para substituir o conteúdo da barra de título padrão do Windows por seu próprio conteúdo XAML personalizado. O XAML é tratado como &quot;cromo do sistema&quot;, para que o Windows manipule os eventos de entrada em vez de seu aplicativo. Isso significa que o usuário ainda pode arrastar e redimensionar a janela, mesmo ao clicar em seu conteúdo da barra de título personalizada.</td>
</tr>
</tbody>
</table>

 

## Web


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<tbody>
<tr class="odd">
<td align="left">Internet Explorer</td>
<td align="left"><p>O Internet Explorer apresenta o modo de borda: um novo modo de documento &quot;residente&quot; projetado para oferecer máxima interoperabilidade com outros navegadores modernos e conteúdo contemporâneo da Web. Esse modo experimental está sendo distribuído progressivamente para um conjunto de usuários do Windows 10 escolhido aleatoriamente. Você pode habilitar ou desabilitar o modo Edge manualmente usando o novo mecanismo <strong>about:flags</strong> do IE. Para saber mais, veja:</p>
<ul>
<li>[Living on the Edge – our next step in helping the web just work](http://blogs.msdn.com/b/ie/archive/2014/11/11/living-on-the-edge-our-next-step-in-interoperability.aspx) (Na borda — uma nova etapa em fazer com que a Web funcione).</li>
<li>[The Internet Explorer for Windows 10 Developer Guide](https://dev.windows.com/microsoft-edge/) (Guia do Desenvolvedor do Internet Explorer para Windows 10).</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">Navegação no modo WebView Edge</td>
<td align="left">O controle [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) usa o mesmo mecanismo de renderização do novo navegador Edge. Isso fornece o modo de renderização de HTML mais preciso e compatível com padrões.</td>
</tr>
<tr class="odd">
<td align="left">WebView fora do thread</td>
<td align="left">Você pode especificar um [<strong>WebView.ExecutionMode</strong>](https://msdn.microsoft.com/library/windows/apps/dn932034) para habilitar o processamento e a exibição de conteúdo da Web em um thread separado em segundo plano. Isso pode melhorar o desempenho em cenários específicos.</td>
</tr>
<tr class="even">
<td align="left">Evento WebView.UnsupportedUriSchemeIdentified</td>
<td align="left"><p>O novo evento [<strong>WebView.UnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn974400) permite que você decida como seu aplicativo deve lidar com um esquema de URI sem suporte. Você pode manipular esse evento para permitir que seu aplicativo forneça tratamento personalizado a esquemas de URI sem suporte.</p>
<p>Para o controle HTML WebView, veja o evento [<strong>MSWebViewUnsupportedUriSchemeIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn803906.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Evento WebView.NewWindowRequested</td>
<td align="left"><p>O novo evento [<strong>WebView.NewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974397) permite que você responda quando um script em um WebView solicita uma nova janela do navegador.</p>
<p>Para o controle HTML WebView, veja o evento [<strong>MSWebViewNewWindowRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030).</p></td>
</tr>
<tr class="even">
<td align="left">Evento WebView.PermissionRequested</td>
<td align="left"><p>O novo evento [<strong>WebView.PermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn974398) permite que o conteúdo do WebView use as novas APIs HTML5 avançadas que exigem permissão especial do usuário, como localização geográfica.</p>
<p>Para o controle HTML WebView, veja o evento [<strong>MSWebViewPermissionRequested</strong>](https://msdn.microsoft.com/library/windows/apps/dn806030.aspx).</p></td>
</tr>
<tr class="odd">
<td align="left">Evento WebView.UnviewableContentIdentified</td>
<td align="left"><p>O novo evento [<strong>WebView.UnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn299351) permite que você responda quando o [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) é navegado para conteúdo não Web, como um arquivo PDF ou documento do Office.</p>
<p>Para controles HTML WebView, veja o evento [<strong>MSWebViewUnviewableContentIdentified</strong>](https://msdn.microsoft.com/library/windows/apps/dn609716).</p></td>
</tr>
<tr class="even">
<td align="left">Método WebView.AddWebAllowedObject</td>
<td align="left"><p>Você pode chamar o novo método [<strong>WebView.AddWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn903993) para injetar um objeto WinRT em um [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702) XAML e, em seguida, chamar suas funções a partir de um JavaScript hospedado nesse <strong>WebView</strong>. Por exemplo, o conteúdo da Web pode mostrar notificações do sistema solicitando que seu aplicativo pai chame a API WinRT [<strong>ToastNotificationManager</strong>](https://msdn.microsoft.com/library/windows/apps/br208642).</p>
<p>Para o controle HTML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702), veja o método [<strong>addWebAllowedObject</strong>](https://msdn.microsoft.com/library/windows/apps/dn926632).</p></td>
</tr>
<tr class="odd">
<td align="left">Método WebView.ClearTemporaryWebDataAync</td>
<td align="left">Quando um usuário interage com o conteúdo da Web em um XAML [<strong>WebView</strong>](https://msdn.microsoft.com/library/windows/apps/br227702), o controle <strong>WebView</strong> armazena dados em cache com base na sessão do usuário. Você pode chamar o novo método [<strong>ClearTemporaryWebDataAsync</strong>](https://msdn.microsoft.com/library/windows/apps/dn974394) para limpar esse cache. Por exemplo, você pode limpar o cache quando um usuário sai do aplicativo para que outro usuário não possa acessar os dados da sessão anterior.</td>
</tr>
</tbody>
</table>



<!--HONumber=Mar16_HO5-->


