---
author: mijacobs
description: 
title: Controle de imagem da pessoa
template: detail.hbs
label: Parallax View
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
pm-contact: kevjey
design-contact: kimsea
dev-contact: kefodero
doc-status: Published
ms.openlocfilehash: c71fdf3d19c8c8e43666620df53258825c4a8eb1
ms.sourcegitcommit: 10d6736a0827fe813c3c6e8d26d67b20ff110f6c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/22/2017
---
# <a name="person-picture-control"></a>Controle de imagem da pessoa

> [!IMPORTANT]
> Este artigo descreve uma funcionalidade que ainda não foi lançada e pode ser modificada substancialmente antes de ser lançada comercialmente. A Microsoft não oferece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

O controle de imagem da pessoa exibe a imagem de avatar de uma pessoa, se existir uma disponível; caso contrário, ele exibe as iniciais da pessoa ou um glifo genérico. Você pode usar o controle para exibir um [objeto Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), um objeto que gerencia informações de contato da pessoa ou você pode fornecer informações de contato, como o nome de exibição e a imagem de perfil manualmente.  

> **APIs importantes**: [classe PersonPicture](https://docs.microsoft.com/uwp/api/windows.ui.xaml.controls.personpicture), [classe Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), [classe ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager)

![O controle de imagem da pessoa](images/person-picture/person-picture_hero.png)

## <a name="is-this-the-right-control"></a>Este é o controle correto?

Use a imagem da pessoa quando desejar representar uma pessoa e suas informações de contato. Aqui estão alguns exemplos de quando você pode usar o controle:
* Para exibir o usuário atual
* Para exibir os contatos em um catálogo de endereços
* Para exibir o remetente da mensagem 
* Para exibir um contato de redes sociais

A ilustração mostra o controle de imagem da pessoa em uma lista de contatos: ![o controle de imagem da pessoa](images/person-picture/person-picture-control.png)

## <a name="how-to-use-the-person-picture-control"></a>Como usar o controle de imagem da pessoa

Para criar uma imagem da pessoa, você deve usar a classe PersonPicture. Este exemplo cria um controle PersonPicture e fornece manualmente o nome de exibição da pessoa, a foto de perfil e as iniciais:

```xaml
<Page
    x:Class="App2.ExamplePage"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:App2"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
        <PersonPicture
            DisplayName="Betsy Sherman"
            ProfilePicture="Assets\BetsyShermanProfile.png"
            Initials="BS" />
    </StackPanel>
</Page>
```

## <a name="using-the-person-picture-control-to-display-a-contact-object"></a>Uso do controle de imagem da pessoa para exibir um objeto de Contact

Você pode usar o controle seletor de pessoa para exibir um objeto [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact): 

```xaml
<Page
    x:Class="SampleApp.PersonPictureContactExample"
    xmlns="http://schemas.microsoft.com/winfx/2006/xaml/presentation"
    xmlns:x="http://schemas.microsoft.com/winfx/2006/xaml"
    xmlns:local="using:SampleApp"
    xmlns:d="http://schemas.microsoft.com/expression/blend/2008"
    xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
    mc:Ignorable="d">

    <StackPanel Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">

        <PersonPicture
            Contact="{x:Bind CurrentContact, Mode=OneWay}" />
            
        <Button Click="LoadContactButton_Click">Load contact</Button>
    </StackPanel>
</Page>
```

```csharp
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Runtime.InteropServices.WindowsRuntime;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.ApplicationModel.Contacts;

namespace SampleApp
{
    public sealed partial class PersonPictureContactExample : Page, System.ComponentModel.INotifyPropertyChanged
    {
        public PersonPictureContactExample()
        {
            this.InitializeComponent();
        }

        private Windows.ApplicationModel.Contacts.Contact _currentContact; 
        public Windows.ApplicationModel.Contacts.Contact CurrentContact
        {
            get => _currentContact;
            set
            {
                _currentContact = value;
                PropertyChanged?.Invoke(this,
                    new System.ComponentModel.PropertyChangedEventArgs(nameof(CurrentContact)));
            }

        }
        public event System.ComponentModel.PropertyChangedEventHandler PropertyChanged;

        public static async System.Threading.Tasks.Task<Windows.ApplicationModel.Contacts.Contact> CreateContact()
        {

            var contact = new Windows.ApplicationModel.Contacts.Contact();
            contact.FirstName = "Betsy";
            contact.LastName = "Sherman";

            // Get the app folder where the images are stored.
            var appInstalledFolder = 
                Windows.ApplicationModel.Package.Current.InstalledLocation;
            var assets = await appInstalledFolder.GetFolderAsync("Assets");
            var imageFile = await assets.GetFileAsync("betsy.png");
            contact.SourceDisplayPicture = imageFile;

            return contact;
        }

        private async void LoadContactButton_Click(object sender, RoutedEventArgs e)
        {
            CurrentContact = await CreateContact();
        }
    }
}
```

> [!NOTE]
> Para simplificar o código, este exemplo cria um novo objeto Contact. Em um aplicativo real, você pode permitir que o usuário selecione um contato ou é possível usar [ContactManager](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.ContactManager) para consultar uma lista de contatos. Para obter informações sobre como recuperar e gerenciar contatos, consulte os [artigos sobre Contatos e calendário](../contacts-and-calendar/index.md). 

## <a name="determining-which-info-to-display"></a>Determinar quais informações devem ser exibidas

Ao fornecer um objeto [Contact](https://docs.microsoft.com/en-us/uwp/api/Windows.ApplicationModel.Contacts.Contact), o controle de imagem da pessoa o avalia para determinar quais informações podem ser exibidas. 

Se uma imagem estiver disponível, o controle exibe a primeira imagem encontrada, nesta ordem:

1.    LargeDisplayPicture
2.    SmallDisplayPicture
3.    Thumbnail

Você pode alterar a imagem escolhida ao definir a propriedade PreferSmallImage como true; assim, SmallDisplayPicture tem uma prioridade maior do que LargeDisplayPicture.

Se não houver uma imagem, o controle exibe o nome ou as iniciais do contato; se não houver quaisquer dados de nome, o controle exibe informações de contato, como um endereço de email ou um número de telefone. 

## <a name="related-articles"></a>Artigos relacionados

* [Contatos e calendário](../contacts-and-calendar/index.md)
* [Exemplo de cartões de contato](http://go.microsoft.com/fwlink/p/?LinkId=624040)




