---
author: TylerMSFT
Description: 'Este tópico mostra exemplos das tarefas de codificação necessárias para obter alguns dos cenários mais comuns de EDP (proteção de dados empresariais) relacionados a fluxos e buffers.'
MS-HAID: 'dev\_files.use\_edp\_to\_protect\_streams\_and\_buffers'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Usar EDP (proteção de dados empresariais) para proteger fluxos e buffers'
---

# Usar EDP (proteção de dados empresariais) para proteger fluxos e buffers

__Observação__ A EDP (Proteção de Dados Empresariais) não pode ser aplicada ao Windows 10, Versão 1511 (compilação 10586) ou anterior.

Este tópico mostra exemplos das tarefas de codificação necessárias para obter alguns dos cenários mais comuns de EDP (proteção de dados empresariais) relacionados a fluxos e buffers. Para obter o panorama completo para desenvolvedores de como a EDP está relacionada a arquivos, fluxos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio, veja [Proteção de dados empresariais](../enterprise/edp-hub.md).

**Observação**  O [exemplo de EDP (proteção de dados empresariais)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) trata de muitas das situações demonstradas neste tópico.

## Pré-requisitos


-   **Preparar-se para a EDP**

    Consulte [Configurar seu computador para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Comprometer-se a criar um aplicativo habilitado para empresas**

    Um aplicativo que garante de forma autônoma que os dados empresariais permaneçam sob o controle administrativo da empresa é conhecido como aplicativo habilitado para empresas. Um aplicativo habilitado é mais avançado, inteligente, flexível e confiável que um não habilitado. Seu aplicativo avisa ao sistema que é habilitado declarando a funcionalidade restrita **enterpriseDataPolicy**. Entretanto, ser habilitado significa mais do que definir uma funcionalidade. Para obter mais informações, consulte [Aplicativos habilitados para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Entender programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Para saber mais sobre como escrever aplicativos assíncronos em in C\# ou Visual Basic, consulte [Chamar APIs assíncronas no Visual Basic ou C#](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, veja [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Proteger um fluxo de dados para uma identidade empresarial


**Observação** Sempre que você proteger um fluxo ou um buffer, é altamente recomendável que você assine o evento [**ProtectionPolicyManager.PolicyChanged**](https://msdn.microsoft.com/library/windows/apps/mt608411) para que seu aplicativo esteja ciente de que a EDP fica desativada no dispositivo. Quando isso acontece, você deve desproteger streams e buffers. Qualquer fluxo ou buffer que você deixar protegido estará qualificado para revogação se o usuário cancelar o registro do dispositivo no MDM (gerenciamento de dispositivo móvel). E se a EDP foi desabilitada quando o recurso foi criado, essa revogação será inadequada. Desproteger os recursos quando a EDP é desabilitada impede esse processo.



Nesse cenário, seu aplicativo tem acesso a um stream desprotegido que contém dados corporativos. Para garantir que esse stream seja protegido quando transferi-lo de e para o dispositivo, seu aplicativo protege os dados da identidade corporativa a qual ele pertence. Isso permite que a empresa apague os dados quando necessário. Para posteriormente determinar o método "unprotect" deve ou não ser chamado em um stream, o aplicativo deve manter o estado que indica se o stream foi protegido, o que é o motivo de a função retornar esse estado no exemplo de código. Se a identidade passada não for gerenciada ou se o aplicativo não tiver permissão da identidade, o fluxo não será protegido e um status 'Unprotected' será retornado da chamada a [**DataProtectionManager.ProtectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706021).

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async System.Threading.Tasks.Task<bool> ProtectAStream
    (InMemoryRandomAccessStream unprotectedInMemoryRandomAccessStream, string identity,
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    IInputStream unprotectedStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0);
    IOutputStream protectedStream = protectedInMemoryRandomAccessStream.GetOutputStreamAt(0);

    // Protect "inputStream".
    DataProtectionInfo info = 
        await DataProtectionManager.ProtectStreamAsync(unprotectedStream, identity, protectedStream);

    // Indicate to the caller whether the stream was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. Similar to buffers, this status
    // must be stored by the app. UnprotectStreamAsync must only be called if the stream was protected
    // successfully earlier.

    return (info.Status == DataProtectionStatus.Protected);
}
```

Para mostrar como você pode usar um método como no exemplo de código acima, aqui está um método auxiliar que o utiliza para converter uma cadeia de caracteres em um fluxo desprotegido e, em seguida, protegê-lo.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<bool> ProtectStringAsStreamAsync
    (string unprotectedEnterpriseData, string identity, 
    InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        using (var dataWriter = new DataWriter(unprotectedInMemoryRandomAccessStream))
        {
            dataWriter.WriteString(unprotectedEnterpriseData);
            await dataWriter.StoreAsync();
            await dataWriter.FlushAsync();
            return await this.ProtectAStream(unprotectedInMemoryRandomAccessStream,
                identity, protectedInMemoryRandomAccessStream);
        }
    }
}
```

## Recuperar o status de um stream


Nesse cenário, seu aplicativo protegeu anteriormente um stream ao qual você deve evitar o acesso não autorizado. Para recuperar o conteúdo do fluxo novamente quando necessário, seu aplicativo pode verificar o status do fluxo.

Observe que o status de um fluxo também é retornado de [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023). Essa API nunca retornará o status 'Unprotected', já que ele requer que o recurso de entrada esteja protegido (não é possível verificar de forma confiável se um recurso está desprotegido). Lembre-se de que se você tiver o código que compara o resultado com 'Unprotected', isso indica a presença de uma falha de design. É uma indicação de que seu código não é mais capaz de identificar se o fluxo está protegido.

```CSharp
using Windows.Storage.Streams;
using Windows.Security.EnterpriseData;

private async void CheckProtectedStreamStatus(IInputStream protectedStream)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetStreamProtectionInfoAsync(protectedStream);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation. Perhaps, show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Desproteger um stream de dados


Nesse cenário, o aplicativo pretende desproteger um stream que foi protegido anteriormente. Este exemplo de código usa um fluxo protegido (observe que o fluxo deve ser protegido para que este processo funcione) e desprotege-o com uma chamada de [**DataProtectionManager.UnprotectStreamAsync**](https://msdn.microsoft.com/library/windows/apps/dn706023). Em seguida, ele lê uma cadeia de caracteres do fluxo e retorna-o.

```CSharp
using Windows.Storage.Streams;

private async System.Threading.Tasks.Task<string> UnprotectStreamIntoStringAsync
    (InMemoryRandomAccessStream protectedInMemoryRandomAccessStream)
{
    using (var unprotectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream())
    {
        DataProtectionInfo dataProtectionInfo = 
            await DataProtectionManager.UnprotectStreamAsync(protectedInMemoryRandomAccessStream, 
            unprotectedInMemoryRandomAccessStream);

        using (var inputStream = unprotectedInMemoryRandomAccessStream.GetInputStreamAt(0))
        {
            using (var dataReader = new DataReader(inputStream))
            {
                await dataReader.LoadAsync((uint)unprotectedInMemoryRandomAccessStream.Size);
                return dataReader.ReadString((uint)unprotectedInMemoryRandomAccessStream.Size);
            }
        }
    }
}
```

Para mostrar como você pode usar os métodos auxiliares apresentados até agora, aqui está um manipulador de eventos que obtém uma cadeia de caracteres de uma caixa de texto, grava a cadeia de caracteres em um fluxo, protege o fluxo, desprotege o fluxo (caso ele tenha sido protegido com êxito) e finalmente lê a cadeia de caracteres do fluxo desprotegido e exibe-o na interface do usuário.

```CSharp
using Windows.Storage.Streams;

private async void ProtectAndThenUnprotectStream_Click(object sender, RoutedEventArgs e)
{
    var protectedInMemoryRandomAccessStream = new InMemoryRandomAccessStream();
    bool isStreamProtected = await this.ProtectStringAsStreamAsync
        (this.enterpriseDataTextBox.Text, MainPage.IDENTITY, protectedInMemoryRandomAccessStream);

    // Only unprotect the stream if we're sure that the stream actually was protected.
    if (isStreamProtected)
    {
        this.resultDataTextBlock.Text = 
            await this.UnprotectStreamIntoStringAsync(protectedInMemoryRandomAccessStream);
    }
}
```

## Recuperar o status de um buffer de dados estáticos


Nesse cenário, seu aplicativo protegeu anteriormente um buffer ao qual você deve evitar o acesso não autorizado. Para recuperar o conteúdo do buffer novamente quando necessário, seu aplicativo pode verificar o status do buffer.

Observe que o status de um buffer também é retornado de [**DataProtectionManager.UnprotectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706022). Essa API nunca retornará o status de 'Unprotected', já que ela requer que o recurso de entrada esteja protegido.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

private async void CheckProtectedBufferStatus(IBuffer protectedData)
{
    DataProtectionInfo dataProtectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(protectedData);

    if (dataProtectionInfo.Status == DataProtectionStatus.Revoked)
    {
        // Code goes here to handle this situation, perhaps show UI
        // saying that the user's data has been revoked.
    }
    else if (dataProtectionInfo.Status != DataProtectionStatus.Protected)
    {
        // Code goes here to handle the unexpected protection status.
    }
}
```

## Proteger dados estáticos recuperados de um recurso corporativo


Esse cenário aborda o mesmo nível apresentado pelos exemplos de código de stream, exceto pelo fato de que ele funciona com um buffer de dados. Novamente, você precisa manter o estado que indica se o buffer foi protegido, conforme mostrado. Se a identidade passada não for gerenciada ou se o aplicativo não tiver permissão da identidade, o buffer não será protegido e um status 'Unprotected' será retornado da chamada a [**DataProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn706020).

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private IBuffer data = null;
private bool isProtected = false;

void StoreBuffer(IBuffer data, bool isProtected)
{
    this.data = data;
    this.isProtected = isProtected;
}

IBuffer GetStoredBuffer(out bool isProtected)
{
    isProtected = this.isProtected;
    return this.data;
}

private string identity = "contoso.com";

private async void ProtectAndThenUnprotectBuffer_Click(object sender, RoutedEventArgs e)
{
    BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
    IBuffer inputData = CryptographicBuffer.ConvertStringToBinary
        (this.enterpriseDataTextBox.Text, encoding);
    BufferProtectUnprotectResult result =
        await DataProtectionManager.ProtectAsync(inputData, this.identity);

    // Record whether the buffer was protected successfully. It will only be protected if
    // the identity is managed AND this app is allowed for this identity. This status
    // must be stored by the app. UnprotectAsync must only be called if the buffer was
    // protected successfully earlier.
    bool isBufferProtected = 
        (result.ProtectionInfo.Status == DataProtectionStatus.Protected);
    IBuffer outputData = result.Buffer;

    // Store the data away...
    this.StoreBuffer(outputData, isBufferProtected);

    // ...and then later retrieve it.
    outputData = this.GetStoredBuffer(out isBufferProtected);

    string storedString = string.Empty;

    if (isBufferProtected)
    {
        result = await DataProtectionManager.UnprotectAsync(outputData);

        if (result.ProtectionInfo.Status != DataProtectionStatus.Unprotected)
        {
            // Code goes here to handle the situation where the buffer
            // is no longer accessible.
            return;
        }
        outputData = result.Buffer;
    }
    this.resultDataTextBlock.Text = CryptographicBuffer.ConvertBinaryToString(encoding, outputData);
}
```

## Habilitar imposição da política de interface do usuário com base em uma identidade protegida de fluxo ou buffer


Quando seu aplicativo estiver prestes a exibir o conteúdo de um fluxo ou buffer protegido na interface do usuário, ele deve habilitar a imposição da política de interface do usuário com base na identidade para a qual o recurso foi protegido. Você deve consultar as informações de proteção do recurso e habilitar a imposição da política de interface do usuário do sistema na identidade recuperada.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;

...

private async void EnableUIPolicyFromProtectedBuffer(IBuffer buffer)
{
    DataProtectionInfo protectionInfo = 
        await DataProtectionManager.GetProtectionInfoAsync(buffer);

    if (protectionInfo.Status != DataProtectionStatus.Protected)
    {
        // In this case, the app has lost access to the buffer
        // (ProtectedToOtherIdentity, Revoked). This must be handled.
        // 'Unprotected' is never returned for GetProtectionInfoAsync().
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(protectionInfo.Identity);
}

```

**Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados


[exemplo de EDP (proteção de dados empresariais)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=May16_HO2-->


