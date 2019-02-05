---
ms.assetid: 333f67f5-f012-4981-917f-c6fd271267c6
description: Este estudo de caso, que se baseia nas informações fornecidas no Bookstore1, começa com um aplicativo Universal 8.1 que exibe dados agrupados em um controle SemanticZoom.
title: Estudo de caso do Windows Runtime 8.x para UWP Bookstore2
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a81980bc03a272cb2be0e66772591f4e395d7722
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9051009"
---
# <a name="windows-runtime-8x-to-uwp-case-study-bookstore2"></a>Estudo de caso do Windows Runtime 8.x para UWP: Bookstore2


Este estudo de caso, que se baseia nas informações fornecidas no [Bookstore1](w8x-to-uwp-case-study-bookstore1.md), começa com um aplicativo Universal 8.1 que exibe dados agrupados em um controle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601). No modelo de exibição, cada instância da classe **Author** representa o grupo dos livros escritos por esse autor e, no **SemanticZoom**, podemos exibir a lista de livros agrupados por autor ou reduzir o zoom para ver uma lista de atalhos de autores. A lista de atalhos proporciona uma navegação mais rápida do que rolar pela lista de livros. Veremos as etapas de portabilidade do aplicativo para um aplicativo da plataforma Windows10Universal do Windows (UWP).

**Observação**  ao abrir Bookstore2Universal\_10 no Visual Studio, caso você veja a mensagem "Atualização do Visual Studio necessária", siga as etapas em [TargetPlatformVersion](w8x-to-uwp-troubleshooting.md).

## <a name="downloads"></a>Downloads

[Baixe o aplicativo Universal 8.1 Bookstore2\_81](https://go.microsoft.com/fwlink/?linkid=532951).

[Baixe o Bookstore2Universal\_10 aplicativo do Windows 10](https://go.microsoft.com/fwlink/?linkid=532952).

## <a name="the-universal-81-app"></a>O aplicativo Universal 8.1

É assim que o Bookstore2\_81, o aplicativo que vamos portar, se parece. Trata-se de um [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) com rolagem horizontal (rolagem vertical no Windows Phone) que mostra os livros agrupados por autor. É possível reduzir o zoom para a lista de atalhos e, a partir daí, navegar de volta para qualquer grupo. Há duas partes principais para esse aplicativo: o modelo de exibição, que fornece a fonte de dados agrupados, e a interface do usuário, que se associa a esse modelo de exibição. Como veremos, ambas essas partes portam facilmente da tecnologia do WinRT 8.1 para Windows 10.

![bookstore2\-81 no windows, exibição ampliada](images/w8x-to-uwp-case-studies/c02-01-win81-zi-how-the-app-looks.png)

Bookstore2\_81 no Windows, exibição ampliada
 

![bookstore2\-81 no windows, exibição reduzida](images/w8x-to-uwp-case-studies/c02-02-win81-zo-how-the-app-looks.png)

Bookstore2\_81 no Windows, exibição reduzida

![bookstore2\-81 no windows phone, exibição ampliada](images/w8x-to-uwp-case-studies/c02-03-wp81-zi-how-the-app-looks.png)

Bookstore2\_81 no Windows Phone, exibição ampliada

![bookstore2\-81 no windows phone, exibição reduzida](images/w8x-to-uwp-case-studies/c02-04-wp81-zo-how-the-app-looks.png)

Bookstore2\_81 no Windows Phone, exibição reduzida

##  <a name="porting-to-a-windows10-project"></a>Portando para um projeto do Windows 10

A solução Bookstore2\_81 é um projeto de Aplicativo Universal 8.1. O projeto bookstore2\_81.Windows compila o pacote do aplicativo para Windows 8.1, e o projeto Bookstore2\_81.WindowsPhone compila o pacote do aplicativo para Windows Phone 8.1. Bookstore2\_81.Shared é o projeto que contém o código-fonte, os arquivos de marcação e outros ativos e recursos usados pelos outros dois projetos.

Assim como no estudo de caso anterior, a opção que vamos dar (of os descritos na [se você tiver um aplicativo Universal 8.1](w8x-to-uwp-root.md)) é a porta o conteúdo da Shared projeto para um Windows 10 destinado à família de dispositivos Universal.

Comece criando um novo projeto de Aplicativo em Branco (Windows Universal). Dê a ele o nome de Bookstore2Universal\_10. Estes são os arquivos que devem ser copiados de Bookstore2\_81 para Bookstore2Universal\_10.

**No projeto compartilhado**

-   Copie a pasta que contém os arquivos PNG de imagem da capa do livro (a pasta é \\Assets\\CoverImages). Depois de copiar a pasta, no **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativada. Clique com o botão direito do mouse na pasta que você copiou e clique em **Incluir no Projeto**. Esse comando é o que chamamos de "incluir" arquivos ou pastas em um projeto. Cada vez que você copiar um arquivo ou pasta, cada cópia, clique em **Atualizar** no **Gerenciador de Soluções** e, em seguida, inclua o arquivo ou pasta no projeto. Não é preciso fazer isso para os arquivos que você está substituindo no destino.
-   Copie a pasta que contém o arquivo de origem do modelo de exibição (a pasta é \\ViewModel).
-   Copie MainPage.xaml e substitua o arquivo no destino.

**No projeto do Windows**

-   Copie BookstoreStyles.xaml. Vamos usar essa uma um bom ponto de partida porque todas as chaves de recurso neste arquivo serão resolvidas em um aplicativo do Windows 10; Alguns no arquivo equivalente do WindowsPhone serão não.
-   Copie SeZoUC.xaml e SeZoUC.xaml.cs. Vamos começar com a versão dessa exibição no Windows, adequada para o janelas largas. Depois, faremos ela se adaptar a janelas menores e, consequentemente, a dispositivos menores.

Edite o código-fonte e os arquivos de marcação que você acabou de copiar e altere as referências ao namespace Bookstore2\_81 para Bookstore2Universal\_10. Uma maneira rápida de fazer isso é usar o recurso **Substituir nos Arquivos**. Não são necessárias alterações no código no modelo de exibição, nem em qualquer outro código imperativo. Porém, apenas para facilitar a identificação da versão do aplicativo em execução, altere o valor retornado pela propriedade **Bookstore2Universal\_10.BookstoreViewModel.AppName** de "Bookstore2\_81" para "BOOKSTORE2UNIVERSAL\_10".

Agora, é possível compilar e executar. Aqui está a aparência do nosso novo aplicativo UWP ter tido nenhum trabalho ainda para portá-lo para o Windows 10.

![o aplicativo do windows 10 com as mudanças de código-fonte iniciais em execução em um dispositivo de desktop, exibição ampliada](images/w8x-to-uwp-case-studies/c02-05-desk10-zi-initial-source-code-changes.png)

O aplicativo do Windows 10 com o código-fonte iniciais as mudanças em execução em um dispositivo Desktop, exibição ampliada

![O aplicativo do windows 10 com as mudanças de código-fonte iniciais em execução em um dispositivo de desktop, exibição reduzida](images/w8x-to-uwp-case-studies/c02-06-desk10-zo-initial-source-code-changes.png)

O aplicativo do Windows 10 com o código-fonte iniciais as mudanças em execução em um dispositivo Desktop, exibição reduzida

O modelo de exibição e as exibições ampliada e reduzida funcionam juntos corretamente, apesar de haver problemas que tornam a visualização um pouco difícil. Um problema é que o [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) não rola. Isso ocorre porque, no Windows 10, o estilo padrão de um [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) faz com que ela seja disposta na vertical (e as diretrizes de design do Windows 10 recomendam que podemos usá-lo dessa maneira em novos aplicativos e portados). Porém, o modelo de painel de itens personalizados do projeto Bookstore2\_81 as configurações de rolagem horizontal (que foi projetado para o 8.1 aplicativo) estão em conflito com as configurações de rolagem verticais no estilo padrão Windows 10 que está sendo aplicada como resultado nós precisar ser portado para um aplicativo do Windows 10. Em segundo lugar, o aplicativo ainda não adaptou sua interface do usuário para oferecer a melhor experiência em janelas de tamanhos diferentes e em dispositivos pequenos. E, em terceiro lugar, os estilos e pincéis corretos ainda não estão sendo usados. Por isso, uma grande parte do texto está invisível (inclusive os cabeçalhos dos grupos nos quais é possível clicar para reduzir o zoom). Então, nas próximas três seções ([Alterações de design de SemanticZoom e GridView](#semanticzoom-and-gridview-design-changes), [Interface do usuário adaptável](#adaptive-ui) e [Estilo universal](#universal-styling)), corrigiremos esses problemas.

## <a name="semanticzoom-and-gridview-design-changes"></a>Alterações de design de SemanticZoom e GridView

As alterações de design no Windows 10 para o controle [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) são descritas na seção [alterações em SemanticZoom](w8x-to-uwp-porting-xaml-and-ui.md). Nesta seção, não precisamos fazer nada em relação a essas alterações.

As alterações do [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) estão descritas na seção [Alterações de GridView/ListView](w8x-to-uwp-porting-xaml-and-ui.md). Devemos fazer alguns ajustes mínimos para nos adaptar a essas alterações, conforme descrito a seguir.

-   Em SeZoUC.xaml, em `ZoomedInItemsPanelTemplate`, defina `Orientation="Horizontal"` e `GroupPadding="0,0,0,20"`.
-   Em SeZoUC.xaml, exclua `ZoomedOutItemsPanelTemplate` e remova o atributo `ItemsPanel` da exibição reduzida.

Pronto!

## <a name="adaptive-ui"></a>Interface do usuário adaptável

Após essa alteração, o layout da interface do usuário fornecido por SeZoUC.xaml é ótimo para a execução do aplicativo em uma janela larga (que só é possível em dispositivos com tela grande). Porém, quando a janela do aplicativo é estreita (o que acontece em dispositivos pequenos e também pode acontecer em dispositivos grandes), a interface do usuário que tínhamos no aplicativo da Loja do Windows Phone Store certamente é mais apropriada.

Para isso, podemos usar o recurso adaptável do Gerenciador de Estado Visual. Definiremos as propriedades em elementos visuais de maneira que, por padrão, a interface do usuário seja disposta no estado estreito usando os modelos menores que usávamos no aplicativo da Loja do Windows Phone. Depois, vamos detectar quando a janela do aplicativo for mais larga ou igual a um tamanho específico (medido em unidades de [pixels efetivos](w8x-to-uwp-porting-xaml-and-ui.md)), e alterar as propriedades dos elementos visuais de acordo para obter um layout mais largo e maior. Colocaremos essas alterações de propriedade em um estado visual e usaremos um acionador adaptável para monitorar e determinar continuamente se esse estado visual deve ou não ser aplicado, dependendo da largura da janela em pixels efetivos. Nesse caso, vamos acionar na largura da janela, mas também é possível acionar na altura da janela.

A largura mínima de janela de 548 epx é apropriada para este caso porque esse é o tamanho do menor dispositivo no qual queremos mostrar o layout largo. Normalmente, os telefones têm menos de 548 epx. Por isso, em dispositivos pequenos assim, preferimos continuar com o layout estreito padrão. Em um computador, por padrão, a janela será aberta com largura suficiente para disparar a troca para o estado largo. De lá, você poderá arrastar a janela até que fique estreita o suficiente para exibir duas colunas dos itens de 250x250. Se ela for um pouco mais estreita, o gatilho será desativado, o estado visual largo será removido e o layout estreito padrão entrará em vigor.

Assim, quais propriedades precisamos definir, e alterar, para obter esses dois layouts diferentes? Há duas alternativas e cada uma envolve uma abordagem diferente.

1.  Podemos colocar dois controles [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) em nossa marcação. Um deles seria uma cópia da marcação que estávamos usando no aplicativo do Windows Runtime 8. x (usando controles [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705) dentro dela) e recolhida por padrão. O outro seria uma cópia da marcação que estávamos usando no aplicativo da Loja do Windows Phone (com controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878) dentro dela), visível por padrão. O estado visual alternaria as propriedades de visibilidade dos dois controles **SemanticZoom**. Isso exigiria pouquíssimo trabalho mas, em geral, não é uma técnica de alto desempenho. Portanto, caso opte por usá-la, você deve criar o perfil do aplicativo e verificar se ele ainda atende às metas de desempenho.
2.  Podemos usar um único [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) contendo controles [**ListView**](https://msdn.microsoft.com/library/windows/apps/br242878). Para conseguir os dois layouts, no estado visual largo, alteraríamos as propriedades dos controles **ListView**, inclusive os modelos aplicados a eles para que sejam dispostos da mesma forma que um [**GridView**](https://msdn.microsoft.com/library/windows/apps/br242705). Isso pode ter um desempenho melhor, mas também há muitas pequenas diferenças entre os diversos estilos e modelos de **GridView** e **ListView**, e entre seus vários tipos de item. Por isso, essa pode ser uma solução mais difícil. Essa solução também está fortemente vinculada à forma como os estilos e modelos padrão são projetados no momento. Por isso, temos uma solução frágil e sensível a quaisquer alterações futuras nos padrões.

Neste estudo de caso, optaremos pela primeira alternativa. Porém, se quiser, você poderá tentar a segunda opção e ver se ela funciona melhor. Estas são as etapas a serem seguidas para implementar a primeira alternativa.

-   No [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601), na marcação do seu novo projeto, defina `x:Name="wideSeZo"` e `Visibility="Collapsed"`.
-   Volte para o projeto Bookstore2\_81.WindowsPhone e abra SeZoUC.xaml. Copie a marcação do elemento [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) desse arquivo e cole-a imediatamente depois de `wideSeZo` no novo projeto. Defina `x:Name="narrowSeZo"` no elemento que você acabou de colar.
-   Mas `narrowSeZo` precisa de alguns estilos que ainda não copiamos. Em Bookstore2\_81.WindowsPhone novamente, copie os dois estilos (`AuthorGroupHeaderContainerStyle` e `ZoomedOutAuthorItemContainerStyle`) de SeZoUC.xaml e cole-os em BookstoreStyles.xaml no seu novo projeto.
-   Agora, você tem dois elementos [**SemanticZoom**](https://msdn.microsoft.com/library/windows/apps/hh702601) no novo SeZoUC.xaml. Encapsule esses dois elementos em uma **Grid**.
-   Em BookstoreStyles.xaml no novo projeto, acrescente a palavra `Wide` a essas três chaves de recurso (e a suas referências em SeZoUC.xaml, mas somente para as referências dentro de `wideSeZo`): `AuthorGroupHeaderTemplate`, `ZoomedOutAuthorTemplate` e `BookTemplate`.
-   No projeto Bookstore2\_81.WindowsPhone, abra BookstoreStyles.xaml. Nesse arquivo, copie os mesmos três recursos (mencionados acima) e os dois conversores de itens da lista de atalhos, além da declaração de prefixo de namespace Windows\_UI\_Xaml\_Controls\_Primitives, e cole tudo no BookstoreStyles.xaml de seu novo projeto.
-   Por fim, em SeZoUC.xaml no novo projeto, adicione a marcação apropriada do Gerenciador de Estado Visual à **Grid** que você adicionou acima.

```xml
    <Grid>
        <VisualStateManager.VisualStateGroups>
            <VisualStateGroup>
                <VisualState x:Name="WideState">
                    <VisualState.StateTriggers>
                        <AdaptiveTrigger MinWindowWidth="548"/>
                    </VisualState.StateTriggers>
                    <VisualState.Setters>
                        <Setter Target="wideSeZo.Visibility" Value="Visible"/>
                        <Setter Target="narrowSeZo.Visibility" Value="Collapsed"/>
                    </VisualState.Setters>
                </VisualState>
            </VisualStateGroup>
        </VisualStateManager.VisualStateGroups>

    ...

    </Grid>
```

## <a name="universal-styling"></a>Estilo universal

Agora corrijamos alguns problemas de estilo, inclusive o que introduzimos acima durante a cópia do projeto anterior.

-   Em MainPage.xaml, altere Background de `LayoutRoot` para `"{ThemeResource ApplicationPageBackgroundThemeBrush}"`.
-   Em BookstoreStyles.xaml, defina o valor do recurso `TitlePanelMargin` como `0` (ou qualquer valor que pareça bom para você).
-   Em SeZoUC.xaml, defina a Margem de `wideSeZo` como `0` (ou qualquer valor que pareça bom para você).
-   Em BookstoreStyles.xaml, remova o atributo Margin de `AuthorGroupHeaderTemplateWide`.
-   Remova o atributo FontFamily de `AuthorGroupHeaderTemplate` e de `ZoomedOutAuthorTemplate`.
-   Em Bookstore2\_81, as chaves de recurso `BookTemplateTitleTextBlockStyle`, `BookTemplateAuthorTextBlockStyle` e `PageTitleTextBlockStyle` foram usadas como uma indireção para que uma única chave tivesse implementações diferentes nos dois aplicativos. Não precisamos mais dessa indireção; podemos apenas referenciar diretamente os estilos do sistema. Assim, substitua essas referências em todo o aplicativo por `TitleTextBlockStyle`, `CaptionTextBlockStyle` e `HeaderTextBlockStyle`, respectivamente. Você pode usar o recurso **Substituir nos Arquivos** do Visual Studio para fazer isso com rapidez e precisão. Em seguida, pode excluir os três recursos não utilizados.
-   Em `AuthorGroupHeaderTemplate`, substitua `PhoneAccentBrush` por `SystemControlBackgroundAccentBrush` e defina `Foreground="White"` no **TextBlock** para que ele pareça correto ao ser executado na família de dispositivos móveis.
-   Em `BookTemplateWide`, copie o atributo Foreground do segundo **TextBlock** para o primeiro.
-   Em `ZoomedOutAuthorTemplateWide`, altere a referência a `SubheaderTextBlockStyle` (que agora está muito grande) para uma referência a `SubtitleTextBlockStyle`.
-   A exibição reduzida (a lista de atalhos) não se sobrepõe mais à exibição ampliada na nova plataforma. Assim, podemos remover o atributo `Background` da exibição reduzida de `narrowSeZo`.
-   Para que todos os estilos e modelos fiquem em um arquivo, mova `ZoomedInItemsPanelTemplate` de SeZoUC.xaml para BookstoreStyles.xaml.

Essa última sequência de operações de estilo deixa o aplicativo com esta aparência.

![o aplicativo do windows 10 portado em execução em um dispositivo de desktop, exibição ampliada, dois tamanhos de janela](images/w8x-to-uwp-case-studies/c02-07-desk10-zi-ported.png)

O aplicativo Windows 10 portado em execução em um dispositivo de Desktop, exibição ampliada, dois tamanhos de janela

![o aplicativo do windows 10 portado em execução em um dispositivo de desktop, exibição reduzida, dois tamanhos de janela](images/w8x-to-uwp-case-studies/c02-08-desk10-zo-ported.png)

O aplicativo Windows 10 portado em execução em um dispositivo de Desktop, exibição reduzida, dois tamanhos de janela

![o aplicativo do windows 10 portado em execução em um dispositivo móvel, exibição ampliada](images/w8x-to-uwp-case-studies/c02-09-mob10-zi-ported.png)

O aplicativo Windows 10 portado em execução em um dispositivo móvel, exibição ampliada

![o aplicativo do windows 10 portado em execução em um dispositivo móvel, exibição reduzida](images/w8x-to-uwp-case-studies/c02-10-mob10-zo-ported.png)

O aplicativo Windows 10 portado em execução em um dispositivo móvel, exibição reduzida

## <a name="conclusion"></a>Conclusão

Este estudo de caso envolveu uma interface do usuário mais ambiciosa do que a anterior. Assim como no estudo de caso anterior, este modelo de exibição específico não exigiu qualquer trabalho, e nossos esforços foram principalmente para refatorar a interface do usuário. Algumas mudanças foram necessárias como resultado da combinação de dois projetos em um, com suporte a muitos fatores forma (na verdade, muito mais do que antes). Algumas mudanças estavam relacionadas com as alterações feitas na plataforma.

O próximo estudo de caso é o [QuizGame](w8x-to-uwp-case-study-quizgame.md), no qual analisaremos o acesso e a exibição de dados agrupados.
