---
author: awkoren
Description: "Este tópico mostra exemplos das tarefas de codificação necessárias para obter alguns dos cenários mais comuns de EDP (proteção de dados empresariais) relacionados à transferência de arquivos."
MS-HAID: dev\_app\_to\_app.use\_edp\_to\_protect\_enterprise\_data\_transferred\_between\_apps
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: Usar EDP para proteger dados empresariais transferidos entre aplicativos
translationtype: Human Translation
ms.sourcegitcommit: 36bc5dcbefa6b288bf39aea3df42f1031f0b43df
ms.openlocfilehash: 77533d4aca3cc84e0a021a0faac57f5afbbefdd7

---

# Usar EDP para proteger dados empresariais transferidos entre aplicativos

__Observação__ A EDP (Proteção de Dados Empresariais) não pode ser aplicada ao Windows 10, Versão 1511 (compilação 10586) ou anterior.

Este tópico mostra exemplos das tarefas de codificação necessárias para obter alguns dos cenários mais comuns de EDP (proteção de dados empresariais) relacionados à transferência de arquivos. Para obter o panorama completo para desenvolvedores de como a EDP está relacionada a arquivos, fluxos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio, veja [Proteção de dados empresariais](../enterprise/edp-hub.md).

**Observação**  O [exemplo de EDP (proteção de dados empresariais)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) trata de muitas das situações demonstradas neste tópico.

## Pré-requisitos


-   **Preparar-se para a EDP**

    Consulte [Configurar seu computador para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Comprometer-se a criar um aplicativo habilitado para empresas**

    Um aplicativo que garante de forma autônoma que os dados empresariais permaneçam sob o controle administrativo da empresa é conhecido como aplicativo habilitado para empresas. Um aplicativo habilitado é mais avançado, inteligente, flexível e confiável que um não habilitado. Seu aplicativo avisa ao sistema que é habilitado declarando a funcionalidade restrita **enterpriseDataPolicy**. Entretanto, ser habilitado significa mais do que definir uma funcionalidade. Para obter mais informações, consulte [Aplicativos habilitados para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Entender programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Para saber mais sobre como escrever aplicativos assíncronos em in C\# ou Visual Basic, consulte [Chamar APIs assíncronas no Visual Basic ou C#](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, veja [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Origem da área de transferência simples


Neste cenário, seu aplicativo é um tipo de bloco de notas que manipula arquivos pessoais e da empresa. Aqui, o aplicativo não precisa alterar sua lógica copiar-colar; ele só precisa chamar [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) sempre que o usuário abrir e exibir o conteúdo de um documento empresarial. Depois que o conteúdo é exibido na interface do usuário do seu aplicativo, o usuário copia-o para colar em um aplicativo diferente. Por isso, é importante definir a política de interface do usuário. Dessa forma, o sistema operacional pode aplicar a política atualmente definida à operação de colar envolvendo dados protegidos. Da mesma forma, é importante limpar a política de interface do usuário assim que ela não for mais necessária, para que o usuário fique mais uma vez livre para copiar e colar dados pessoais. **TryApplyProcessUIPolicy** retornará false se o argumento de identidade não estiver sendo gerenciado por uma política empresarial.

```CSharp
using Windows.Security.EnterpriseData;

...

private void OnFileLoaded(FileProtectionInfo fileProtectionInfo, string contentsOfFile)
{
    if (fileProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
            (fileProtectionInfo.Identity);
        if (isIdentityManaged)
        {
            // UI policy is now in effect for the file's identity.
        }
        else
        {
            // Enterprise policy is not in effect, because the file's identity
            // is not managed. In this case, we have a file protected to an
            // unmanaged identity, which is not a valid situation.
            // We still have to call ClearProcessUIPolicy if we want to clear the policy.
            ProtectionPolicyManager.ClearProcessUIPolicy();
        }
    }
    else
    {
        // In case we applied the policy for the previous file, now we need to clear it.
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
    // Code goes here to display "contentsOfFile" in your UI. Ready for copy-paste...
}
```

## Destino da área de transferência simples


Neste cenário, seu aplicativo é um aplicativo de email que manipula contas pessoais e corporativas. O usuário tenta colar dados em uma resposta de email usando sua conta pessoal. Nesse caso, antes de o aplicativo recuperar o conteúdo, ele precisa apenas fazer uma chamada a [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636). Se já tivermos acesso, esse método será retornado imediatamente. Porém, se não tivermos acesso, o método gera uma caixa de diálogo que solicita o consentimento do usuário e "desbloqueia" o pacote de dados caso o consentimento seja concedido. Isso é para oferecer ao usuário a chance de cancelar uma operação de colagem feita por engano.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteSimple()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // In case we don't already have acccess, request consent from the user
        // for us to access the clipboard data.
        await dataPackageView.RequestAccessAsync();

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## O destino da área de transferência é um documento neutro vazio


Neste cenário, seu aplicativo é um processador de texto. Depois de criar um novo documento, desde que o documento permaneça vazio, seu aplicativo trata o documento como neutro, nem empresarial nem pessoal. O usuário cola os dados empresarias nesse documento de contexto neutro. Como o documento está em um contexto neutro, podemos apenas alterná-lo para um contexto empresarial e evitar chamar [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636) completamente (já que não é necessário mostrar a caixa de diálogo nesse caso). Portanto, se os dados estiverem protegidos, basta alternarmos para um contexto empresarial e colar os dados.

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private async void OnPasteWithApplyPolicy()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(dataPackageView.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult policyResult =
                await dataPackageView.RequestAccessAsync(dataPackageView.Properties.EnterpriseId);
            if (this.isNewEmptyDocument &&
                policyResult == ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy(dataPackageView.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it's not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                await dataPackageView.RequestAccessAsync();
            }
        }

        try
        {
            string contentsOfClipboard = await dataPackageView.GetTextAsync();
            // Code goes here to insert the text into the email...
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the clipboard.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

## Origem da área de transferência com identidade empresarial explícita


Neste cenário, seu aplicativo é um processador de texto. Ele usa um thread em segundo plano para confirmar as operações de cópia do usuário. O usuário copia alguns dados de um arquivo empresarial e, imediatamente, alterna para um arquivo pessoal. Nesse momento, o contexto global do aplicativo se torna pessoal. Neste ponto, já que o estado global foi alterado e não deve ser usado, o thread do aplicativo em segundo plano deve informar explicitamente à área de transferência que os dados de entrada são empresariais. Isso é feito com a definição da propriedade [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861).

```CSharp
using Windows.ApplicationModel.DataTransfer;

...

private void CopyToClipboard(string stringToCopy, string identity)
{
    // Copy the string to the clipboard.
    var dataPackage = new DataPackage();
    dataPackage.SetText(stringToCopy);
    dataPackage.Properties.EnterpriseId = identity;
    Clipboard.SetContent(dataPackage);
}
```

## Marcar uma janela específica com identidade empresarial


Neste cenário, seu aplicativo é um processador de texto que manipula vários documentos em janelas diferentes, algumas das quais são empresariais e outras, pessoais. O aplicativo quer garantir que quaisquer dados sendo colados na janela do documento pessoal sejam corretamente examinados (isto é, recusados ou aceitos, caso sejam dados empresariais) e que quaisquer dados de saída da janela do documento da empresa sejam marcados corretamente. Você faz isso definido a propriedade [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785).

```CSharp
using Windows.Security.EnterpriseData;

...

private void TagCurrentViewWithEnterpriseId(string identity)
{
    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
}
```

## Marcar objeto de arraste de saída com a identidade empresarial


Seu aplicativo tem uma janela pessoal aberta com algum conteúdo empresarial arrastável. O usuário começa a arrastar uma parte desse conteúdo, e seu aplicativo precisa garantir que o conteúdo seja marcado como empresarial. Este cenário não envolve nenhuma API nova. Para este cenário, seu aplicativo definirá a propriedade [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) (consulte o cenário [Origem da área de transferência com identidade empresarial explícita](#clipboard_source_explicit_id) acima).

## Consulta da identidade empresarial do objeto de arraste recebido


Seu aplicativo tem um documento novo e vazio aberto, que é considerado neutro desde que esteja vazio, e o usuário arrasta e solta algum conteúdo no documento. O aplicativo deve agora determinar a identidade empresarial do objeto para alterar o estado do documento de acordo com essa informação. Para este cenário, seu aplicativo receberá a propriedade **EnterpriseId** do pacote de dados lendo [**DataPackagePropertySet.EnterpriseId**](https://msdn.microsoft.com/library/windows/apps/dn913861) (consulte o cenário [Origem da área de transferência com identidade empresarial explícita](#clipboard_source_explicit_id) acima).

## Seu aplicativo como uma origem do contrato de Compartilhamento


Quando você dá suporte ao contrato de Compartilhamento em seu aplicativo, para configurar uma origem de compartilhamento, defina o contexto de identidade empresarial no [**DataPackage**](https://msdn.microsoft.com/library/windows/apps/br205873) conforme mostra este exemplo de código.

**Observação** Este exemplo de código depende de você já ter definido a identidade no objeto gerenciador de política de proteção como seu modo de exibição atual (consulte [Marcar uma janela específica com identidade empresarial](#tag_window_with_id)); caso contrário, a propriedade [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) conterá a cadeia de caracteres vazia.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private void OnShareSourceOperation(object sender, RoutedEventArgs e)
{
    // Register the current page as a share source (or you could do this earlier in your app).
    DataTransferManager.GetForCurrentView().DataRequested += OnDataRequested;
    DataTransferManager.ShowShareUI();
}

private void OnDataRequested(DataTransferManager sender, DataRequestedEventArgs args)
{
    if (!string.IsNullOrEmpty(this.shareSourceContent))
    {
        var protectionPolicyManager = ProtectionPolicyManager.GetForCurrentView();
        DataPackage requestData = args.Request.Data;
        requestData.Properties.Title = this.shareSourceTitle;
        requestData.Properties.EnterpriseId = protectionPolicyManager.Identity;
        requestData.SetText(this.shareSourceContent);
    }
}
```

## Seu aplicativo como um destino do contrato de Compartilhamento


Este exemplo de código mantém nossa política desde que o arquivo com o qual estamos trabalhando esteja vazio. Em seguida, estamos livres para alternar contextos conforme apropriado para dados de entrada e evitar a abertura da interface do usuário de consentimento onde for possível. Portanto, quando seu aplicativo receber dados como um destino do contrato de Compartilhamento, ele deverá chamar [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) se não houver nenhum conteúdo existente; caso contrário, deverá chamar [**DataPackage.RequestAccessAsync**](https://msdn.microsoft.com/library/windows/apps/dn706636). O exemplo de código mostra como.

```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.ApplicationModel.DataTransfer.ShareTarget;

...

string identity = "microsoft.com";

protected override async void OnShareTargetActivated(ShareTargetActivatedEventArgs args)
{
    ShareOperation shareOperation = args.ShareOperation;
    if (shareOperation.Data.Contains(StandardDataFormats.Text))
    {
        // If the data has no enterprise identity, then we already have access.
        if (!string.IsNullOrEmpty(shareOperation.Data.Properties.EnterpriseId))
        {
            ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
                await ProtectionPolicyManager.RequestAccessAsync
                    (shareOperation.Data.Properties.EnterpriseId, identity);
            if (this.isNewEmptyDocument && protectionPolicyEvaluationResult ==
                ProtectionPolicyEvaluationResult.Allowed)
            {
                // If this is a new and empty document, and we're allowed to access
                // the data, then we can avoid popping the consent dialog.
                bool isIdentityManaged = ProtectionPolicyManager.TryApplyProcessUIPolicy
                    (shareOperation.Data.Properties.EnterpriseId);
                // You can use "isIdentityManaged", but it';s not necessary.
            }
            else
            {
                // In this case, we can't optimize the workflow, so we just
                // request consent from the user in this case.
                protectionPolicyEvaluationResult = await shareOperation.Data.RequestAccessAsync();
            }
        }

        try
        {
            // Get the text from the share operation...
            App.shareTargetContent = await shareOperation.Data.GetTextAsync();
        }
        catch (Exception)
        {
            // Code goes here to handle the exception retrieving text from the share operation.
        }
    }
    else
    {
        // The value in the share operation is not in the text format.
    }
}
```

## Consultar a política de copiar-colar passivamente


Neste cenário, seu aplicativo habilita a interface do usuário de colar somente quando há dados na área de transferência. Para esse recurso, você pode usar o método [**ProtectionPolicyManager.CheckAccess**](https://msdn.microsoft.com/library/windows/apps/dn705783), que permite uma verificação passiva da política.

**Observação** Este exemplo de código depende de você já ter definido a identidade no objeto gerenciador de política de proteção como seu modo de exibição atual (consulte [Marcar uma janela específica com identidade empresarial](#tag_window_with_id)); caso contrário, a propriedade [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) conterá a cadeia de caracteres vazia.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private bool IsClipboardPeekAllowedAsync()
{
    ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult = ProtectionPolicyEvaluationResult.Blocked;
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);
    }

    // Since this is a convenience feature to allow a peek of clipboard content,
    // if state is Blocked or ConsentRequired, do not show peek if this helper
    // method returns false.
    return (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed);
}
```

## Solicitar acesso para a operação copiar-colar


Este cenário mostra como verificar o acesso para uma operação de colar.

**Observação** Este exemplo de código depende de você já ter definido a identidade no objeto gerenciador de política de proteção como seu modo de exibição atual (consulte [Marcar uma janela específica com identidade empresarial](#tag_window_with_id)); caso contrário, a propriedade [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) conterá a cadeia de caracteres vazia.



```CSharp
using Windows.ApplicationModel.DataTransfer;
using Windows.Security.EnterpriseData;

...

private async void OnPasteWithRequestAccess()
{
    DataPackageView dataPackageView = Clipboard.GetContent();
    if (dataPackageView.Contains(StandardDataFormats.Text))
    {
        ProtectionPolicyEvaluationResult protectionPolicyEvaluationResult =
            ProtectionPolicyManager.CheckAccess(dataPackageView.Properties.EnterpriseId,
                ProtectionPolicyManager.GetForCurrentView().Identity);

        if (protectionPolicyEvaluationResult == ProtectionPolicyEvaluationResult.Allowed)
        {
            try
            {
                string contentsOfClipboard = await dataPackageView.GetTextAsync();
                // Code goes here to insert the text into the app...
                this.fileContentsTextBox.Text = contentsOfClipboard;
            }
            catch (Exception)
            {
                // Code goes here to handle the exception retrieving text from the clipboard.
            }
        }
        else
        {
            // Paste from the enterprise context is not allowed by policy.
        }
    }
    else
    {
        // The value on the clipboard is not in the text format.
    }
}
```

**Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Tópicos relacionados


[exemplo de EDP (proteção de dados empresariais)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)








<!--HONumber=Jun16_HO4-->


