---
ms.assetid: 0C69521B-47E0-421F-857B-851B0E9605F2
title: Associar dados hierárquicos e criar um modo de exibição mestre/detalhes
description: Você pode criar um modo de exibição mestre/detalhes de vários níveis (também conhecido como lista/detalhes) de dados hierárquicos, associando controles de itens a instâncias CollectionViewSource que são associadas em uma cadeia.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 14b6ab96ac5423d1811618c6a3c91ccf56645664
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74255121"
---
# <a name="bind-hierarchical-data-and-create-a-masterdetails-view"></a>Associar dados hierárquicos e criar um modo de exibição mestre/detalhes



> **Observe**  também consulte o [exemplo mestre/detalhes](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/XamlMasterDetail).

Você pode criar um modo de exibição mestre/detalhes de vários níveis (também conhecido como lista/detalhes) de dados hierárquicos, associando controles de itens a instâncias [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) que são associadas em uma cadeia. Neste tópico, usamos a [extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) onde possível, e a [extensão de marcação {Binding}](https://docs.microsoft.com/windows/uwp/xaml-platform/binding-markup-extension) mais flexível (mas menos eficiente) onde necessário.

Uma estrutura comum dos aplicativos da Plataforma Universal do Windows (UWP) é navegar em diferentes páginas de detalhes quando um usuário faz uma seleção em uma lista mestra. Isso é útil quando você quiser providenciar uma representação visual detalhada de cada item em cada nível da hierarquia. Outra opção é exibir vários níveis de dados em uma única página. Isso é útil quando você deseja exibir algumas listas simples que permitem que o usuário explore rapidamente um item de interesse. Este tópico descreve como implementar essa interação. As instâncias de [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) controlam a seleção atual em cada nível hierárquico.

Criaremos um modo de exibição de hierarquia de esportes em equipe que é organizado em listas de ligas, divisões e times e inclui um modo de exibição detalhado dos times. Quando você seleciona um item de qualquer lista, os modos de exibição subsequentes são automaticamente atualizados.

![modo de exibição mestre/detalhado de uma hierarquia de esportes](images/xaml-masterdetails.png)

## <a name="prerequisites"></a>Pré-requisitos

Este tópico pressupõe que você saiba como criar um aplicativo UWP básico. Para obter instruções para criar seu primeiro aplicativo UWP, consulte [Criar seu primeiro aplicativo UWP em C# ou Visual Basic](https://docs.microsoft.com/previous-versions/windows/apps/hh974581(v=win.10)).

## <a name="create-the-project"></a>Criar o projeto

Crie um novo projeto **Aplicativo em branco (universal do Windows)** . Chame-o de "MasterDetailsBinding".

## <a name="create-the-data-model"></a>Criar o modelo de dados

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
        public IEnumerable<Team> Teams { get; set; }
    }

    public class League
    {
        public string Name { get; set; }
        public IEnumerable<Division> Divisions { get; set; }
    }

    public class LeagueList : List<League>
    {
        public LeagueList()
        {
            this.AddRange(GetLeague().ToList());
        }

        public IEnumerable<League> GetLeague()
        {
            return from x in Enumerable.Range(1, 2)
                   select new League
                   {
                       Name = "League " + x,
                       Divisions = GetDivisions(x).ToList()
                   };
        }

        public IEnumerable<Division> GetDivisions(int x)
        {
            return from y in Enumerable.Range(1, 3)
                   select new Division
                   {
                       Name = String.Format("Division {0}-{1}", x, y),
                       Teams = GetTeams(x, y).ToList()
                   };
        }

        public IEnumerable<Team> GetTeams(int x, int y)
        {
            return from z in Enumerable.Range(1, 4)
                   select new Team
                   {
                       Name = String.Format("Team {0}-{1}-{2}", x, y, z),
                       Wins = 25 - (x * y * z),
                       Losses = x * y * z
                   };
        }
    }
}
```

## <a name="create-the-view"></a>Criar o modo de exibição

Em seguida, exponha a classe de origem de associação na classe que representa a página de marcação. Fazemos isso adicionando uma propriedade do tipo **LeagueList** a **MainPage**.

```cs
namespace MasterDetailsBinding
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
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

Por fim, substitua o conteúdo do arquivo MainPage.xaml pela marcação a seguir, que declara três instâncias de [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource) e as associa em uma cadeia. Os controles subsequentes podem então ser associados ao **CollectionViewSource** apropriado, dependendo do seu nível na hierarquia.

```xml
<Page
    x:Class="MasterDetailsBinding.MainPage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:MasterDetailsBinding"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <Page.Resources>
        <CollectionViewSource x:Name="Leagues"
            Source="{x:Bind ViewModel}"/>
        <CollectionViewSource x:Name="Divisions"
            Source="{Binding Divisions, Source={StaticResource Leagues}}"/>
        <CollectionViewSource x:Name="Teams"
            Source="{Binding Teams, Source={StaticResource Divisions}}"/>

        <Style TargetType="TextBlock">
            <Setter Property="FontSize" Value="15"/>
            <Setter Property="FontWeight" Value="Bold"/>
        </Style>

        <Style TargetType="ListBox">
            <Setter Property="FontSize" Value="15"/>
        </Style>

        <Style TargetType="ContentControl">
            <Setter Property="FontSize" Value="15"/>
        </Style>

    </Page.Resources>

    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <StackPanel Orientation="Horizontal">

            <!-- All Leagues view -->

            <StackPanel Margin="5">
                <TextBlock Text="All Leagues"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Leagues}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- League/Divisions view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Leagues}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Divisions}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Division/Teams view -->

            <StackPanel Margin="5">
                <TextBlock Text="{Binding Name, Source={StaticResource Divisions}}"/>
                <ListBox ItemsSource="{Binding Source={StaticResource Teams}}" 
                    DisplayMemberPath="Name"/>
            </StackPanel>

            <!-- Team view -->

            <ContentControl Content="{Binding Source={StaticResource Teams}}">
                <ContentControl.ContentTemplate>
                    <DataTemplate>
                        <StackPanel Margin="5">
                            <TextBlock Text="{Binding Name}" 
                                FontSize="15" FontWeight="Bold"/>
                            <StackPanel Orientation="Horizontal" Margin="10,10">
                                <TextBlock Text="Wins:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Wins}"/>
                            </StackPanel>
                            <StackPanel Orientation="Horizontal" Margin="10,0">
                                <TextBlock Text="Losses:" Margin="0,0,5,0"/>
                                <TextBlock Text="{Binding Losses}"/>
                            </StackPanel>
                        </StackPanel>
                    </DataTemplate>
                </ContentControl.ContentTemplate>
            </ContentControl>

        </StackPanel>

    </Grid>
</Page>
```

Observe que ao associar diretamente o [**CollectionViewSource**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Data.CollectionViewSource), você está indicando que deseja associar ao item atual em associações onde o caminho não pode ser encontrado na própria coleção. Não é necessário especificar a propriedade **CurrentItem** como o caminho para a associação, embora você possa fazer isso se houver ambiguidade. Por exemplo, o [**ContentControl**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.Controls.ContentControl) que representa o modo de exibição do time tem sua propriedade [**Content**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.contentcontrol.content) associada ao `Teams`**CollectionViewSource**. No entanto, os controles no [**DataTemplate**](https://docs.microsoft.com/uwp/api/Windows.UI.Xaml.DataTemplate) associam a propriedades da classe `Team` porque o **CollectionViewSource** fornece automaticamente o time selecionado no momento na lista de times quando necessário.

 

 

