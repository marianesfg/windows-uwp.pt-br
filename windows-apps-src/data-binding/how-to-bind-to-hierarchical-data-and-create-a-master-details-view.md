---
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Associar dados hierárquicos e criar um modo de exibição mestre/detalhes
description: Você pode criar um modo de exibição mestre/detalhes de vários níveis (também conhecido como lista/detalhes) de dados hierárquicos associando controles de itens a instâncias CollectionViewSource que são associadas em uma cadeia.
---
# Associar dados hierárquicos e criar um modo de exibição mestre/detalhes

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


> **Observação**  Consulte também o [Amostra de mestre/detalhes](http://go.microsoft.com/fwlink/p/?linkid=619991).

Você pode criar um modo de exibição mestre/detalhes de vários níveis (também conhecido como lista/detalhes) de dados hierárquicos, associando controles de itens a instâncias [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) que são associadas em uma cadeia. Neste tópico, usamos a [extensão de marcação {x:Bind}](https://msdn.microsoft.com/library/windows/apps/Mt204783) onde possível, e a [extensão de marcação {Binding}](https://msdn.microsoft.com/library/windows/apps/Mt204782) mais flexível (mas menos eficiente) onde necessário.

Uma estrutura comum dos aplicativos da Plataforma Universal do Windows (UWP) é navegar em diferentes páginas de detalhes quando um usuário faz uma seleção em uma lista mestra. Isso é útil quando você quiser providenciar uma representação visual detalhada de cada item em cada nível da hierarquia. Outra opção é exibir vários níveis de dados em uma única página. Isso é útil quando você deseja exibir algumas listas simples que permitem que o usuário explore rapidamente um item de interesse. Este tópico descreve como implementar essa interação. As instâncias de [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) controlam a seleção atual em cada nível hierárquico.

Criaremos um modo de exibição de hierarquia de esportes em equipe que é organizado em listas de ligas, divisões e times e inclui um modo de exibição detalhado dos times. Quando você seleciona um item de qualquer lista, os modos de exibição subsequentes são automaticamente atualizados.

![modo de exibição mestre/detalhado de uma hierarquia de esportes](images/xaml-masterdetails.png)

## Pré-requisitos

Este tópico pressupõe que você saiba como criar um aplicativo UWP básico. Para obter instruções para criar seu primeiro aplicativo UWP, consulte [Criar seu primeiro aplicativo UWP em C# ou Visual Basic](https://msdn.microsoft.com/library/windows/apps/Hh974581).

## Criar o projeto

Crie um novo projeto **Aplicativo em branco (universal do Windows)**. Chame-o de "MasterDetailsBinding".

## Criar o modelo de dados

Adicione uma nova classe a seu projeto, chame-a de ViewModel.cs e adicione este código a ela. Esta será sua classe de origem de associação.

```cs
using System;
using System.Collections.Generic;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

namespace MasterDetailsBinding
{
    public class Team
    {
        public string Name { get; set; }
        public int Wins { get; set; }
        public int Losses { get; set; }
    }

    public class Division
    {
        public string Name { get; set; }
        public IEnumerable&lt;Team&gt; Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable&lt;Division&gt; Divisions { get; set; }
    }

    public class LeagueList : List&lt;League&gt;
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable&lt;League&gt; GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = &quot;League &quot; + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable&lt;Division&gt; GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format(&quot;Division {0}-{1}&quot;, x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable&lt;Team&gt; GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format(&quot;Team {0}-{1}-{2}&quot;, x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## Criar o modo de exibição

Em seguida, exponha a classe de origem de associação na classe que representa a página de marcação. Fazemos isso adicionando uma propriedade do tipo **LeagueList** a **MainPage**.

```cs
namespace MasterDetailsBinding
{
    /// &lt;summary&gt;
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// &lt;/summary&gt;
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
            this.ViewModel = new LeagueList();
        }
        public LeagueList ViewModel { get; set; }
    }
}
```

Por fim, substitua o conteúdo do arquivo MainPage.xaml pela marcação a seguir, que declara três instâncias de [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833) e as associa em uma cadeia. Os controles subsequentes podem então ser associados ao **CollectionViewSource** apropriado, dependendo do seu nível na hierarquia.

```xaml
<Page
    x:Class=&quot;MasterDetailsBinding.MainPage&quot;
    xmlns=&quot;http://schemas.microsoft.com/winfx/2006/xaml/presentation&quot;
    xmlns:x=&quot;http://schemas.microsoft.com/winfx/2006/xaml&quot;
    xmlns:local=&quot;using:MasterDetailsBinding&quot;
    xmlns:d=&quot;http://schemas.microsoft.com/expression/blend/2008&quot;
    xmlns:mc=&quot;http://schemas.openxmlformats.org/markup-compatibility/2006&quot;
    mc:Ignorable=&quot;d&quot;>

    <Page.Resources>
        <CollectionViewSource x:Name=&quot;Leagues&quot;
            Source=&quot;{x:Bind ViewModel}&quot;/>
        <CollectionViewSource x:Name=&quot;Divisions&quot;
            Source=&quot;{Binding Divisions, Source={StaticResource Leagues}}&quot;/>
        <CollectionViewSource x:Name=&quot;Teams&quot;
            Source=&quot;{Binding Teams, Source={StaticResource Divisions}}&quot;/>

        <Style TargetType=&quot;TextBlock&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
            <Setter Property=&quot;FontWeight&quot; Value=&quot;Bold&quot;/>
        </Style>

        <Style TargetType=&quot;ListBox&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

        <Style TargetType=&quot;ContentControl&quot;>
            <Setter Property=&quot;FontSize&quot; Value=&quot;15&quot;/>
        </Style>

    </Page.Resources>

    <Grid Background=&quot;{ThemeResource ApplicationPageBackgroundThemeBrush}&quot;>

        <StackPanel Orientation=&quot;Horizontal&quot;>

            <!-- All Leagues view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;All Leagues&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Leagues}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Leagues}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Divisions}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin=&quot;5&quot;>
                <TextBlock Text=&quot;{Binding Name, Source={StaticResource Divisions}}&quot;/>
                <ListBox ItemsSource=&quot;{Binding Source={StaticResource Teams}}&quot; 
                    DisplayMemberPath=&quot;Name&quot;/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content=&quot;{Binding Source={StaticResource Teams}}&quot;>
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin=&quot;5&quot;>
                            <TextBlock Text=&quot;{Binding Name}&quot; 
                                FontSize=&quot;15&quot; FontWeight=&quot;Bold&quot;/>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,10&quot;>
                                <TextBlock Text=&quot;Wins:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Wins}&quot;/>
                            </StackPanel>
                            <StackPanel Orientation=&quot;Horizontal&quot; Margin=&quot;10,0&quot;>
                                <TextBlock Text=&quot;Losses:&quot; Margin=&quot;0,0,5,0&quot;/>
                                <TextBlock Text=&quot;{Binding Losses}&quot;/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Observe que ao associar diretamente o [**CollectionViewSource**](https://msdn.microsoft.com/library/windows/apps/BR209833), você está indicando que deseja associar ao item atual em associações onde o caminho não pode ser encontrado na própria coleção. Não é necessário especificar a propriedade **CurrentItem** como o caminho para a associação, embora você possa fazer isso se houver ambiguidade. Por exemplo, o [**ContentControl**](https://msdn.microsoft.com/library/windows/apps/BR209365) que representa o modo de exibição do time tem sua propriedade [**Content**](https://msdn.microsoft.com/library/windows/apps/BR209365-content) associada ao `Teams`**CollectionViewSource**. No entanto, os controles no [**DataTemplate**](https://msdn.microsoft.com/library/windows/apps/BR242348) associam a propriedades da classe `Team` porque o **CollectionViewSource** fornece automaticamente o time selecionado no momento na lista de times quando necessário.

 

 



<!--HONumber=Mar16_HO1-->


