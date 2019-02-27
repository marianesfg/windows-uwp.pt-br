---
Description: A button gives the user a way to trigger an immediate action.
title: Cartão de visita
ms.date: 03/07/2018
ms.topic: article
keywords: windows 10, uwp
pm-contact: kele
design-contact: tbd
dev-contact: tbd
doc-status: not-published
ms.localizationpriority: medium
ms.openlocfilehash: 71a3108e21455086e2742987db1d7125c733f6e2
ms.sourcegitcommit: ff131135248c85a8a2542fc55437099d549cfaa5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9117596"
---
# <a name="contact-card"></a>Cartão de visita

O cartão de visita exibe informações de contato, como o nome, número de telefone e endereço, para um [Contato](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) (o mecanismo que UWP usa para representar pessoas e empresas).  O cartão de visita também permite que o usuário edite informações de contato. Você pode optar por exibir um cartão de visita compacto ou completo que contém informações adicionais.

> **APIs importantes**: [método ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard), [método ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_), [método IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported), [classe Contact](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact)  

Há duas maneiras de exibir o cartão de visita:  
* Como um cartão de contato padrão que é exibido em um submenu é light-rejeitável relativa às ações – o cartão de contato dissapears quando o usuário clica fora dele. 
* Como um cartão de contato completas que ocupa mais espaço e não é light-rejeitável relativa às ações — o usuário deve clicar **fechar** para fechá-lo. 


<figure>
    <img src="images/contact-card/contact-card-standard.png" alt="The full contact card">
    <figcaption>O cartão de contato padrão</figcaption>
</figure>

<figure>
    <img src="images/contact-card/contact-card-full.png" alt="The full contact card">
    <figcaption>O cartão de contato completo</figcaption>
</figure>


## <a name="is-this-the-right-control"></a>Esse é o controle correto?

Use o cartão de contato quando você deseja exibir informações de contato de um contato. Se você quiser exibir o nome do contato e a imagem, use o [controle de imagem da pessoa](person-picture.md). 


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

## <a name="show-a-standard-contact-card"></a>Exibir um cartão de contato padrão

1. Normalmente, você mostrar um cartão de contato, como o usuário clicou algo: um botão ou talvez o [controle de imagem da pessoa](person-picture.md). Não queremos ocultar o elemento. Para evitar ocultá-la, precisamos criar um [Rect](/uwp/api/windows.foundation.rect) que descreve a localização e o tamanho do elemento. 

    Vamos criar uma função de utilitário que faz que para nós – vamos usá-lo posteriormente.
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

2. Determinar se você pode exibir o cartão de contato chamando o método [ContactManager.IsShowContactCardSupported](/uwp/api/windows.applicationmodel.contacts.contactmanager.IsShowContactCardSupported). Se não houver suporte, exiba uma mensagem de erro. (Este exemplo pressupõe que você vai mostrando o cartão de contato em resposta a um evento click.)
    ```csharp
    // Contact and Contact Managers are existing classes 
    private void OnUserClickShowContactCard(object sender, RoutedEventArgs e) 
    { 
        if (ContactManager.IsShowContactCardSupported()) 
        { 

    ```

3. Use a função de utilitário que você criou na etapa 1 para obter os limites do controle que disparou o evento (para que não tenha sido abordado-com o cartão de contato).

    ```csharp
            Rect selectionRect = GetElementRect((FrameworkElement)sender); 
    ```

4. Obtenha o objeto [contato](//docs.microsoft.com/uwp/api/Windows.ApplicationModel.Contacts.Contact) que você deseja exibir. Este exemplo cria apenas um contato simple, mas seu código deve recuperar um contato real. 

    ```csharp
                // Retrieve the contact to display
                var contact = new Contact(); 
                var email = new ContactEmail(); 
                email.Address = "jsmith@contoso.com"; 
                contact.Emails.Add(email); 
    ```
5. Mostrar cartão de contato chamando o método [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard). 

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

## <a name="show-a-full-contact-card"></a>Exibir um cartão de contato completo

Para mostrar o cartão de contato completas, chame o método [ShowFullContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager#Windows_ApplicationModel_Contacts_ContactManager_ShowContactCard_Windows_ApplicationModel_Contacts_Contact_Windows_ApplicationModel_Contacts_FullContactCardOptions_) em vez de [ShowContactCard](/uwp/api/windows.applicationmodel.contacts.contactmanager.showcontactcard).

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

Os exemplos neste artigo criam um contato simples. Em um aplicativo real, você provavelmente deseja recuperar um contato existente. Para obter instruções, consulte o artigo [contatos e calendário](/windows/uwp/contacts-and-calendar/).




## <a name="related-articles"></a>Artigos relacionados
- [Contatos e calendário](/windows/uwp/contacts-and-calendar/)
- [Exemplo de cartões de contato](https://go.microsoft.com/fwlink/p/?LinkId=624040)
- [Controle de imagem de pessoas](/windows/uwp/controls-and-patterns/person-picture/)
