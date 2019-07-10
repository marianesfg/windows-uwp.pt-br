---
Description: Um botão dá ao usuário uma forma de acionar uma ação imediata.
title: Cartão de visita
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: 4c227629ace1f3fdbb2af8582401f9273cf11c2e
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "63799649"
---
# <a name="contact-card"></a>Cartão de visita

O cartão de visita exibe informações de contato, como nome, número de telefone e endereço, para um [Contato](/uwp/api/Windows.ApplicationModel.Contacts.Contact) (o mecanismo que a UWP usa para representar pessoas e empresas).  O cartão de visita também permite que o usuário edite informações de contato. Você pode optar por exibir um cartão de visita compacto ou completo que contém informações adicionais.

> **APIs importantes**: [método ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard), [método ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard), [método IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [classe Contact](/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

Há duas maneiras de exibir o cartão de visita:  
* Como um cartão de visita padrão exibido em um submenu light dismiss – o cartão de visita desaparece quando o usuário clica fora dele. 
* Como um cartão de visita completo que ocupa mais espaço e não é light dismiss – o usuário deve clicar em **Fechar** para fechá-lo. 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>O cartão de visita padrão</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>O cartão de visita completo</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use o cartão de visita quando quiser exibir informações de contato para um contato. Se você quiser exibir somente o nome e a imagem do contato, use o [controle de imagem da pessoa](person-picture.md). 


<!-- TODO: Add examples back when the contact card has been added. -->

<!-- ## Examples

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>If you have the <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> app installed, click here to <a href="xamlcontrolsgallery:/item/Button">open the app and see the Button in action</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Get the XAML Controls Gallery app (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Get the source code (GitHub)</a></li>
    </ul>
</td>
</tr>
</table> -->

## <a name="show-a-standard-contact-card"></a>Exibir um cartão de visita padrão

1. Normalmente, você mostra um cartão de visita porque o usuário clicou em algo: um botão ou talvez o [controle de imagem da pessoa](person-picture.md). Não queremos ocultar o elemento. Para evitar isso, precisamos criar um [Rect](/uwp/api/windows.foundation.rect) que descreve a localização e o tamanho do elemento. 

    Vamos criar uma função de utilitário que faz isso para nós – vamos usá-la posteriormente.
    ```csharp
    // Gets the rectangle of the element 
    public static Rect GetElementRectHelper(FrameworkElement element) 
    { 
        // Passing "null" means set to root element. 
        GeneralTransform elementTransform = element.TransformToVisual(null); 
        Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
        return rect; 
    } 

    ```

2. Determine se você pode exibir o cartão de visita chamando o método [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported). Se não for compatível, exiba uma mensagem de erro. Este exemplo pressupõe que você mostrará o cartão de visita em resposta a um evento de clique.
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. Use a função de utilitário criada na etapa 1 para obter os limites do controle que disparou o evento (para que não seja coberto pelo cartão de visita).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. Obtenha o objeto [Contato](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) que você deseja exibir. Este exemplo apenas cria um contato simples, mas o código precisa recuperar um contato real. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. Mostre o cartão de visita chamando o método [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard). 

    ```csharp
            ContactManager.ShowFullContactCard(
                contact, selectionRect, Placement.Default); 
        } 
    } 
    ```

Este é o exemplo de código completo:

```csharp
// Gets the rectangle of the element 
public static Rect GetElementRect(FrameworkElement element) 
{ 
    // Passing "null" means set to root element. 
    GeneralTransform elementTransform = element.TransformToVisual(null); 
    Rect rect = elementTransform.TransformBounds(new Rect(0, 0, element.ActualWidth, element.ActualHeight)); 
    return rect; 
} 
 
// Display a contact in response to an event
private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
{ 
    if (ContactManager.IsShowContactCardSupported()) 
    { 
        Rect selectionRect = GetElementRect((FrameworkElement)sender);

        // Retrieve the contact to display
        var contact = new Contact(); 
        var email = new ContactEmail(); 
        email.Address = "jsmith@contoso.com"; 
        contact.Emails.Add(email); 
    
        ContactManager.ShowContactCard(
            contact, selectionRect, Placement.Default); 
    } 
} 

```

## <a name="show-a-full-contact-card"></a>Exibir um cartão de visita completo

Para mostrar o cartão de visita completo, chame o método [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showfullcontactcard) em vez de [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard).

```csharp
private void onUserClickShowContactCard() 
{ 
   
    Contact contact = new Contact(); 
    ContactEmail email = new ContactEmail(); 
    email.Address = "jsmith@hotmail.com"; 
    contact.Emails.Add(email); 
 
 
    // Setting up contact options.     
    FullContactCardOptions fullContactCardOptions = new FullContactCardOptions(); 
 
    // Display full contact card on mouse click.   
    // Launch the People’s App with full contact card  
    fullContactCardOptions.DesiredRemainingView = ViewSizePreference.UseLess; 
     
 
    // Shows the full contact card by launching the People App. 
    ContactManager.ShowFullContactCard(contact, fullContactCardOptions); 
} 

```

## <a name="retrieving-real-contacts"></a>Recuperar contatos "reais"

Os exemplos neste artigo criam um contato simples. Em um aplicativo real, você provavelmente deseja recuperar um contato existente. Para obter instruções, confira o artigo [Contatos e calendário](/windows/uwp/contacts-and-calendar/).




## <a name="related-articles"></a>Artigos relacionados
- [Contatos e calendário](/windows/uwp/contacts-and-calendar/)
- [Amostra de cartões de visita](https://go.microsoft.com/fwlink/p/?LinkId=624040)
- [Controle de imagem de pessoas](/windows/uwp/controls-and-patterns/person-picture/)
