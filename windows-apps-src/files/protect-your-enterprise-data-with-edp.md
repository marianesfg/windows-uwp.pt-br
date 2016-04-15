---
Description: 'Este tópico mostra exemplos das tarefas de codificação necessárias para obter algumas das situações de Proteção de dados empresariais (EDP) relacionados a arquivos mais comuns.'
MS-HAID: 'dev\_files.protect\_your\_enterprise\_data\_with\_edp'
MSHAttr: 'PreferredLib:/library/windows/apps'
Search.Product: eADQiWindows 10XVcnh
title: 'Usar a Proteção de dados empresariais (EDP) para proteger arquivos'
---

# Usar a Proteção de dados empresariais (EDP) para proteger arquivos

__Observação__ A Proteção de dados empresariais (EDP) não pode ser aplicada ao Windows 10, Versão 1511 (compilação 10586) ou anterior.

Este tópico mostra exemplos das tarefas de codificação necessárias para obter algumas das situações de Proteção de dados empresariais (EDP) relacionados a arquivos mais comuns. Para a noção de desenvolvedor completa de como EDP está relacionada a arquivos, streams, área de transferência, rede, tarefas em segundo plano e proteção de dados sob bloqueio, consulte [Proteção de dados empresariais (EDP)](../enterprise/edp-hub.md).

**Observação**  A [amostra de Proteção de dados empresariais (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) trata de muitos das situações demonstradas neste tópico.

## Pré-requisitos

-   **Preparar-se para a EDP**

    Consulte [Configurar seu computador para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Comprometer-se a criar um aplicativo habilitado para empresas**

    Um aplicativo que garante de forma autônoma que os dados empresariais permaneçam sob o controle administrativo da empresa é conhecido como aplicativo habilitado para empresas. Um aplicativo habilitado é mais avançado, inteligente, flexível e confiável que um não habilitado. Seu aplicativo avisa ao sistema que é habilitado declarando a funcionalidade restrita **enterpriseDataPolicy**. Entretanto, ser habilitado significa mais do que definir uma funcionalidade. Para obter mais informações, consulte [Aplicativos habilitados para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Entender programação assíncrona para aplicativos da Plataforma Universal do Windows (UWP)**

    Para saber mais sobre como escrever aplicativos assíncronos em in C\# ou Visual Basic, consulte [Chamar APIs assíncronas no Visual Basic ou C#](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber mais sobre como escrever aplicativos assíncronos em in C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## O caminho da pasta local e a visualização de arquivos protegidos no Explorador de Arquivos


Você pode criar um aplicativo para hospedar os exemplos de código neste tópico para experimentá-los. Os exemplos de código gravam arquivos na pasta local do aplicativo e a localização exata da pasta no disco é exclusiva ao seu aplicativo. Para determinar a localização da pasta local de seu aplicativo, você pode adicionar o código a seguir.

```CSharp
// Put a breakpoint on the line after the line of code below. Use the value of localFolderPath
// in File Explorer to find the output file that the later code examples create.
string localFolderPath = ApplicationData.Current.LocalFolder.Path;
```

Quando você tiver o caminho, poderá usar o Explorador de Arquivos para localizar facilmente os arquivos que o seu aplicativo cria. Dessa forma, você poderá confirmar que são protegidos e que são protegidos para a identidade correta.

No Explorador de Arquivos, **Alterar opções de pasta e pesquisa** e na guia **Exibir**, marque **Mostrar arquivos criptografados em cor**. Também use os comandos **Exibir** &gt; **Adicionar colunas** do Explorador de Arquivos para adicionar a coluna **Criptografado a** para que você possa ver a identidade da empresa para quem você protege seus arquivos.

## Proteger dados empresariais em um novo arquivo (para um aplicativo interativo)


Os dados empresariais podem ser inseridos em seu aplicativo de muitas formas, inclusive por meio de determinados pontos de extremidade da rede, de arquivos, da área de transferência ou do contrato de compartilhamento. Seu aplicativo pode criar novos dados empresariais também. Seja qual for o meio pelo qual seu aplicativo habilitado receba dados empresariais, o aplicativo precisa ter cuidado para proteger os dados para a identidade empresarial gerenciada quando mantiver os dados para um novo arquivo.

As etapas básicas são para usar uma API de armazenamento regular para criar o arquivo, usar uma API de EDP para proteger o arquivo à identidade da empresa e depois (novamente, usando APIs de armazenamento regular) para gravar o arquivo. Tenha o cuidado de proteger o arquivo antes de gravá-lo (como mostrado no exemplo abaixo). Use o método [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) para proteger o arquivo. E, como sempre, só faz sentido proteger a uma identidade se tal identidade for gerenciada. Para saber mais sobre o porquê disso e sobre como o seu aplicativo pode determinar a identidade da empresa em que está sendo executado, consulte [Confirmando se uma identidade é gerenciada](../enterprise/edp-hub.md#confirming_an_identity_is_managed).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFile storageFile = storageFolder.CreateFileAsync("sample.txt",
        CreationCollisionOption.ReplaceExisting);

    // It's important to protect a file *before* writing enterprise data to it.
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(storageFile, identity);

    // If the file is now protected, to the intended identity, then we can write to it.
    if (fileProtectionInfo.Identity == identity &&
        fileProtectionInfo.Status == FileProtectionStatus.Protected)
        await Windows.Storage.FileIO.WriteTextAsync(storageFile, enterpriseData);
}
```

## Proteger dados empresariais em um novo arquivo (para uma tarefa em segundo plano)


A API [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) que usamos na seção anterior só é adequada a aplicativos interativos. Para uma tarefa em segundo plano, seu código poderá ser executado enquanto estiver na tela de bloqueio. E a organização pode estar administrando uma política de "proteção de dados sob bloqueio" (DPL) segura, em que as chaves de criptografia necessárias para acessar recursos protegidos são temporariamente removidas da memória do dispositivo quando o dispositivo está bloqueado. Isso impede vazamentos de dados se o dispositivo for perdido. O recurso também remove chaves associadas com arquivos protegidos quando seus identificadores são fechados. No entanto, é possível criar novos arquivos protegidos durante a janela de bloqueio (o tempo entre o dispositivo ser bloqueado e desbloqueado) e acessá-los, mantendo o identificador de arquivo aberto. **StorageFolder.CreateFileAsync** fecha o identificador quando o arquivo é criado para que esse algoritmo não possa ser usado.

1.  Criar um novo arquivo usando **StorageFolder.CreateFileAsync**.
2.  Criptografar usando **FileProtectionManager.ProtectAsync**.
3.  Abra o arquivo e grave-o.

Já que a Etapa 1 envolve encerrar o identificador de arquivo (e mesmo que a Etapa 1 não o encerrasse, a Etapa 2 o faria), a Etapa 3 não é possível porque as chaves de criptografia para acessar o arquivo não estão mais disponíveis.

Para tratar dessa situação, você pode usar [**FileProtectionManager.CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153) para criar um arquivo protegido e devolver um **IRandomAccessStream** a ele. Isso permite que os aplicativos façam várias gravações em um arquivo, mantendo o identificador de arquivo aberto.

O código de exemplo demonstra um aplicativo de email simples gravando em um novo arquivo quando um novo email é recebido. Aplicativos de email devem sincronizar o email quando o dispositivo está bloqueado. Quando esse aplicativo recebe um novo email, ele cria um novo arquivo usando [**CreateProtectedAndOpenAsync**](https://msdn.microsoft.com/library/windows/apps/dn705153), recupera um stream de saída e grava o corpo do email nele.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

private async void SaveEnterpriseDataToFile(string enterpriseData, string identity)
{
    // You should only protect to an identity that is managed.
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;

    ProtectedFileCreateResult protectedFileCreateResult =
        await FileProtectionManager.CreateProtectedAndOpenAsync(storageFolder,
            "sample.txt", identity, CreationCollisionOption.ReplaceExisting);

    // It&#39;s important to successfully protect a file *before* writing enterprise data to it.
    if (protectedFileCreateResult.ProtectionInfo.Identity == identity &&
        protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.Protected)
    {
        using (IOutputStream outputStream =
            protectedFileCreateResult.Stream.GetOutputStreamAt(0))
        {
            using (DataWriter writer = new DataWriter(outputStream))
            {
                writer.WriteString(enterpriseData);
                await writer.StoreAsync();
                await writer.FlushAsync();
            }
        }
    }
    else if (protectedFileCreateResult.ProtectionInfo.Status == FileProtectionStatus.AccessSuspended)
    {
        // Perform any special processing for the access suspended case.
    }
}
```

## Proteger uma pasta cuja finalidade é conter dados empresariais


Se você deseja que cada item dentro de uma pasta seja protegido, também pode fazer isso. Você pode usar o método [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) para proteger uma pasta vazia. Em seguida, qualquer arquivo ou pasta criada em seguida dentro da pasta será também protegido. Você pode proteger uma pasta existente ou criar uma nova pasta para proteger (o exemplo a abaixo cria uma nova pasta). Mas, de qualquer maneira, para a proteção obter êxito, a pasta deve estar vazia no momento. Se não estiver, então [**FileProtectionInfo.Status**](https://msdn.microsoft.com/library/windows/apps/dn705150) conterá um valor de [**FileProtectionStatus.NotProtectable**](https://msdn.microsoft.com/library/windows/apps/dn279147).

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CreateANewFolderAndProtectItAsync(string folderName, string identity)
{
    if (!ProtectionPolicyManager.IsIdentityManaged(identity)) return false;

    StorageFolder storageFolder = ApplicationData.Current.LocalFolder;
    StorageFolder newStorageFolder =
        await storageFolder.CreateFolderAsync(folderName);

    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.ProtectAsync(newStorageFolder, identity);

    if (fileProtectionInfo.Identity != identity ||
        fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // Protection failed.
    }
}
```

## Proteção contra cópias de um arquivo para outro


Neste caso, já existe um arquivo que você sabe que está protegido à identidade de empresa adequada. Você pode replicar essa proteção em outro arquivo de forma muito conveniente. Você sequer precisa saber qual é a identidade: só precisa saber que é a certa.

Para fazer uma cópia simples de um arquivo protegido, você pode chamar [**StorageFile.CopyAsync**](https://msdn.microsoft.com/library/windows/apps/br227190). A cópia do arquivo resultante terá a mesma proteção que o original.

Para proteger um arquivo existente desprotegido antes de gravar dados empresariais nele, uma alternativa para chamar [**FileProtectionManager.ProtectAsync**](https://msdn.microsoft.com/library/windows/apps/dn705157) (que vimos em uma situação anterior e para o qual você precisa passar uma identidade gerenciada) é chamar [**FileProtectionManager.CopyProtectionAsync**](https://msdn.microsoft.com/library/windows/apps/dn705152) conforme mostrado no exemplo de código.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void CopyProtectionFromOneFileToAnother
    (StorageFile sourceStorageFile, StorageFile targetStorageFile)
{
    bool copyResult = await
        FileProtectionManager.CopyProtectionAsync(sourceStorageFile, targetStorageFile);

    if (!copyResult)
    {
        // Copying failed. To diagnose, you could check the file's status.
        // (call FileProtectionManager.GetProtectionInfoAsync and
        // check FileProtectionInfo.Status).
    }
}
```

## Manipular o acesso negado a um arquivo que você protegeu


Neste caso, o seu aplicativo tentar acessar um arquivo—que ele mesmo protegeu anteriormente—e tem o acesso negado. Você precisará verificar o status do arquivo para ver o que deu errado. Nesse exemplo de código, o aplicativo chama a API [**FileProtectionManager.GetProtectionInfoAsync**](https://msdn.microsoft.com/library/windows/apps/dn705154) para consultar o status e determinar se o motivo é que o acesso ao arquivo foi revogado agora como resultado de gerenciamento remoto.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async System.Threading.Tasks.Task<bool> IsFileProtectionStatusRevoked
    (StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo =
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status == FileProtectionStatus.Revoked)
    {
        // Inform the user that their data has been revoked.
    }
    return fileProtectionInfo.Status == FileProtectionStatus.Revoked;
}
```

## Habilitar imposição de política de interface do usuário com base na identidade protegida de um arquivo


Quando seu aplicativo está prestes a exibir o conteúdo de um arquivo protegido (como um PDF) em sua interface do usuário, ele deve habilitar a imposição de política de interface do usuário com base na identidade protegida do arquivo. Você deve consultar as informações de proteção do arquivo e habilitar imposição de política de IU do sistema de sua identidade recuperada.

```CSharp
using Windows.Security.EnterpriseData;
using Windows.Storage;

...

private async void EnableUIPolicyFromFile(StorageFile storageFile)
{
    FileProtectionInfo fileProtectionInfo = 
        await FileProtectionManager.GetProtectionInfoAsync(storageFile);

    if (fileProtectionInfo.Status != FileProtectionStatus.Protected)
    {
        // No policy enforcement required, unless the file is inaccessible
        // (Revoked, ProtectedToOtherIdentity) in which case that should
        // be handled in an app-specific way.
        return;
    }

    ProtectionPolicyManager.TryApplyProcessUIPolicy(fileProtectionInfo.Identity);
}
```

**Observação**  Este artigo se destina a desenvolvedores do Windows 10 que escrevem da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados


[amostra de proteção de dados corporativos (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 





<!--HONumber=Mar16_HO5-->


