---
title: Compartilhamento de Minhas Pessoas
description: Explica como adicionar suporte para o compartilhamento de Minhas Pessoas
ms.date: 06/28/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f0549aa1e20d8ed787eed550f4a7e7171a812831
ms.sourcegitcommit: 51d884c3646ba3595c016e95bbfedb7ecd668a88
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/11/2019
ms.locfileid: "67820186"
---
# <a name="my-people-sharing"></a>Compartilhamento de Minhas Pessoas

O recurso Minhas Pessoas permite que os usuários fixem contatos na barra de tarefas, permitindo que eles fiquem em contato facilmente de qualquer lugar do Windows, não importa por qual aplicativo eles estejam conectados. Agora os usuários podem compartilhar conteúdo com seus contatos fixados arrastando arquivos do Explorador de Arquivos para o bloco Minhas Pessoas fixado. Eles também podem compartilhar com quaisquer contatos do repositório do Windows por meio do botão compartilhar padrão. Continue lendo para saber como habilitar seu aplicativo como um destino de compartilhamento de Minhas Pessoas.

![Painel de compartilhamento de Minhas Pessoas](images/my-people-sharing.png)

## <a name="requirements"></a>Requisitos

+ Windows 10 e Microsoft Visual Studio 2019. Para obter detalhes da instalação, consulte [Prepare-se para começar o Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ Conhecimento básico de C# ou uma linguagem de programação similar orientada a objetos. Para começar a usar C#, consulte [Criar um app "Hello, world"](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).

## <a name="overview"></a>Visão geral

Há três etapas que você deve seguir para habilitar seu aplicativo como destino de compartilhamento de Minhas Pessoas:

1. [Declare suporte para o contrato de ativação shareTarget no manifesto do aplicativo.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#declaring-support-for-the-share-contract)
2. [Anote os contatos que os usuários podem compartilhar a usar seu aplicativo.](https://docs.microsoft.com/en-us/windows/uwp/contacts-and-calendar/my-people-sharing#annotating-contacts)
3. Dê suporte a várias instâncias do aplicativo em execução ao mesmo tempo.  Os usuários devem poder interagir com a versão completa de seu aplicativo enquanto também o usam para compartilhar com outras pessoas. Eles podem usá-lo em várias janelas de compartilhamento ao mesmo tempo. Para dar suporte a isso, seu aplicativo precisa ser capaz de executar várias exibições simultaneamente. Para saber como fazer isso, consulte o artigo ["Mostrar vários modos de exibição para um aplicativo"](https://docs.microsoft.com/windows/uwp/design/layout/show-multiple-views).

Feito isso, seu aplicativo será exibido como destino de compartilhamento na janela de compartilhamento de Minhas Pessoas, que pode ser iniciada de duas maneiras:
1. Um contato é escolhido pelo botão compartilhar.
2. Arquivos são arrastados e soltos em um contato fixado na barra de tarefas.

## <a name="declaring-support-for-the-share-contract"></a>Declarando suporte para o contrato de compartilhamento

Para declarar suporte para seu aplicativo como destino de compartilhamento, abra seu aplicativo no Visual Studio. No **Gerenciador de Soluções**, clique com o botão direito do mouse no arquivo **Package.appxmanifest** e selecione **Abrir com**. No menu, selecione **Editor de XML (texto)** e clique em **OK**. Em seguida, faça as seguintes alterações no manifesto:


**Antes**
```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
    </Application>
</Applications>
```

**Depois**

```xml
<Applications>
    <Application Id="MyApp"
      Executable="$targetnametoken$.exe"
      EntryPoint="My.App">
        <Extensions>
            <uap:Extension Category="windows.shareTarget">
                <uap:ShareTarget Description="Share with MyApp">
                    <uap:SupportedFileTypes>
                        <uap:SupportsAnyFileType/>
                    </uap:SupportedFileTypes>
                    <uap:DataFormat>Text</uap:DataFormat>
                    <uap:DataFormat>Bitmap</uap:DataFormat>
                    <uap:DataFormat>Html</uap:DataFormat>
                    <uap:DataFormat>StorageItems</uap:DataFormat>
                    <uap:DataFormat>URI</uap:DataFormat>
                </uap:ShareTarget>
            </uap:Extension>
         </Extensions>
    </Application>
</Applications>
```

Este código adiciona suporte para todos os formatos de dados e arquivos, mas você pode optar por especificar quais tipos de arquivos e formatos de dados são compatíveis (consulte a [documentação da classe ShareTarget](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget) para obter mais detalhes).

## <a name="annotating-contacts"></a>Anotando contatos

Para permitir que a janela de compartilhamento de Minhas Pessoas mostre seu aplicativo como destino de compartilhamento para seus contatos, você precisa gravá-los no repositório de contatos do Windows. Para saber como gravar contatos, consulte o [exemplo de integração de cartão de contato](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration). 

Para seu aplicativo ser exibido como destino de compartilhamento de Minhas Pessoas ao compartilhar com um contato, ele deve gravar uma anotação para esse contato. Anotações são dados do seu aplicativo que são associados a um contato. A anotação deve conter a classe ativável correspondente ao seu modo de exibição desejado no membro **ProviderProperties** e declarar suporte para a operação **Share**.

Você pode anotar contatos a qualquer momento enquanto seu aplicativo estiver em execução, mas geralmente é necessário anotar os contatos assim que eles são adicionados ao repositório de contatos do Windows.

```Csharp
if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
{
    // Create a new contact annotation
    ContactAnnotation annotation = new ContactAnnotation();
    annotation.ContactId = myContact.Id;

    // Add appId and Share support to the annotation
    String appId = "MyApp_vqvv5s4y3scbg!App";
    annotation.ProviderProperties.Add("ContactShareAppID", appId);
    annotation.SupportedOperations = ContactAnnotationOperations::Share;

    // Save annotation to contact annotation list
    // Windows.ApplicationModel.Contacts.ContactAnnotationList 
    await contactAnnotationList.TrySaveAnnotationAsync(annotation);
}
```

O "appId" é o Nome da Família de Pacotes, seguido por '!' e a ID da Classe Ativável. Para localizar o Nome da Família de Pacotes, abra **Package.appxmanifest** usando o editor padrão e procure na guia "Packaging". Aqui, o "App" é a Classe Ativável correspondente para o modo de exibição de destino de compartilhamento.

## <a name="running-as-a-my-people-share-target"></a>Execução como destino de compartilhamento de Minhas Pessoas

Por fim, para executar o app, substitua o método [OnShareTargetActivated](https://docs.microsoft.com/en-us/uwp/api/Windows.UI.Xaml.Application#Windows_UI_Xaml_Application_OnShareTargetActivated_Windows_ApplicationModel_Activation_ShareTargetActivatedEventArgs_) na classe principal do app para processar a ativação do destino de compartilhamento. A propriedade [ShareTargetActivatedEventArgs.ShareOperation.Contacts](https://docs.microsoft.com/en-us/uwp/api/windows.applicationmodel.datatransfer.sharetarget.shareoperation#Properties) conterá os contatos com os quais o conteúdo está sendo compartilhado ou estará vazia se for uma operação de compartilhamento padrão (e não um compartilhamento de Minhas Pessoas).

```Csharp
protected override void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    bool isPeopleShare = false;
    if (ApiInformation.IsApiContractPresent("Windows.Foundation.UniversalApiContract", 5))
    {
        // Make sure the current OS version includes the My People feature before
        // accessing the ShareOperation.Contacts property
        isPeopleShare = (args.ShareOperation.Contacts.Count > 0);
    }

    if (isPeopleShare)
    {
        // Show share UI for MyPeople contact(s)
    }
    else
    {
        // Show standard share UI for unpinned contacts
    }
}
```

## <a name="see-also"></a>Consulte também
+ [Adicionar pessoas Meu suporte](my-people-support.md)
+ [Classe ShareTarget](https://docs.microsoft.com/en-us/uwp/schemas/appxpackage/appxmanifestschema/element-sharetarget)
+ [Exemplo de integração de cartão de contato](https://github.com/Microsoft/Windows-universal-samples/tree/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/ContactCardIntegration)
