---
title: Proteção de dados
description: Este artigo explica como usar a classe DataProtectionProvider no namespace Windows.Security.Cryptography.DataProtection para criptografar e descriptografar dados digitais em um aplicativo UWP.
ms.assetid: 9EE3CC45-5C44-4196-BD8B-1D64EFC5C509
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 15feb86677cd70774a99fe4046d817c71fc71f90
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372587"
---
# <a name="data-protection"></a>Proteção de dados



Este artigo explica como usar a classe [**DataProtectionProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider) no namespace [**Windows.Security.Cryptography.DataProtection**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection) para criptografar e descriptografar dados digitais em um aplicativo UWP.

Você pode usar as APIs de proteção de dados de várias formas:

-   Para proteger dados de uma entidade de segurança do Active Directory (AD), como um grupo do AD. Qualquer membro do grupo pode descriptografar os dados.
-   Para proteger dados na chave pública contida em um certificado X.509. O proprietário da chave privada pode descriptografar os dados.
-   Para proteger dados usando uma chave simétrica. Isso funciona, por exemplo, para proteger dados em uma entidade não AD como o Live ID.
-   Para proteger dados nas credenciais (senha) usadas durante o logon em um site.

Para proteger dados, ao criar um objeto [**DataProtectionProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.DataProtection.DataProtectionProvider), você deve especificar um descritor de proteção antes de chamar [**ProtectAsync**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectasync) ou [**ProtectStreamAsync**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectstreamasync). O exemplo a seguir mostra possíveis descritores de proteção de amostra.

## <a name="protecting-static-data"></a>Protegendo dados estáticos


O exemplo seguinte mostra como usar os métodos [**ProtectAsync**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectasync) e [**UnprotectAsync**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.unprotectasync) para proteger dados estáticos assíncronos para o SID do usuário atual.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.DataProtection;
using Windows.Storage.Streams;
using System.Threading.Tasks;

namespace SampleProtectAsync
{
    sealed partial class StaticDataProtectionApp : Application
    {
        public StaticDataProtectionApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Protect data asynchronously.
            this.Protect();
        }

        public async void Protect()
        {
            // Initialize function arguments.
            String strMsg = "This is a message to be protected.";
            String strDescriptor = "LOCAL=user";
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;

            // Protect a message to the local user.
            IBuffer buffProtected = await this.SampleProtectAsync(
                strMsg,
                strDescriptor,
                encoding);

            // Decrypt the previously protected message.
            String strDecrypted = await this.SampleUnprotectData(
                buffProtected,
                encoding);
        }

        public async Task<IBuffer> SampleProtectAsync(
            String strMsg,
            String strDescriptor,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object for the specified descriptor.
            DataProtectionProvider Provider = new DataProtectionProvider(strDescriptor);

            // Encode the plaintext input message to a buffer.
            encoding = BinaryStringEncoding.Utf8;
            IBuffer buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Encrypt the message.
            IBuffer buffProtected = await Provider.ProtectAsync(buffMsg);

            // Execution of the SampleProtectAsync function resumes here
            // after the awaited task (Provider.ProtectAsync) completes.
            return buffProtected;
        }

        public async Task<String> SampleUnprotectData(
            IBuffer buffProtected,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object.
            DataProtectionProvider Provider = new DataProtectionProvider();

            // Decrypt the protected message specified on input.
            IBuffer buffUnprotected = await Provider.UnprotectAsync(buffProtected);

            // Execution of the SampleUnprotectData method resumes here
            // after the awaited task (Provider.UnprotectAsync) completes
            // Convert the unprotected message from an IBuffer object to a string.
            String strClearText = CryptographicBuffer.ConvertBinaryToString(encoding, buffUnprotected);

            // Return the plaintext string.
            return strClearText;
        }
    }
}
```

## <a name="protecting-stream-data"></a>Protegendo dados de fluxo


O exemplo a seguir mostra como usar os métodos [**ProtectStreamAsync**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.protectstreamasync) e [**UnprotectStreamAsync**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.dataprotection.dataprotectionprovider.unprotectstreamasync) para proteger de forma assíncrona os dados de fluxo para o SID do usuário atual.

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.DataProtection;
using Windows.Storage.Streams;
using System.Threading.Tasks;

namespace SampleProtectStreamAsync
{

    sealed partial class StreamDataProtectionApp : Application
    {
        public StreamDataProtectionApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Protect a stream synchronously
            this.ProtectData();
         }

        public async void ProtectData()
        {
            // Initialize function arguments.
            String strDescriptor = "LOCAL=user";
            String strLoremIpsum = "Lorem ipsum dolor sit amet, consectetur adipiscing elit. Suspendisse elementum "
                + "ullamcorper eros, vitae gravida nunc consequat sollicitudin. Vivamus lacinia, "
                + "diam a molestie porttitor, sapien neque volutpat est, non suscipit leo dolor "
                + "sit amet nisl. Praesent tincidunt tincidunt quam ut pharetra. Sed tincidunt "
                + "sit amet nisl. Praesent tincidunt tincidunt quam ut pharetra. Sed tincidunt "
                + "porttitor massa, at convallis dolor dictum suscipit. Nullam vitae lectus in "
                + "lorem scelerisque convallis sed scelerisque orci. Praesent sed ligula vel erat "
                + "eleifend tempus. Nullam dignissim aliquet mauris a aliquet. Nulla augue justo, "
                + "posuere a consectetur ut, suscipit et sem. Proin eu libero ut felis tincidunt "
                + "interdum. Curabitur vulputate eros nec sapien elementum ut dapibus eros "
                + "dapibus. Suspendisse quis dui dolor, non imperdiet leo. In consequat, odio nec "
                + "aliquam tincidunt, magna enim ultrices massa, ac pharetra est urna at arcu. "
                + "Nunc suscipit, velit non interdum suscipit, lectus lectus auctor tortor, quis "
                + "ultrices orci felis in dolor. Etiam congue pretium libero eu vestibulum. "
                + "Mauris bibendum erat eleifend nibh consequat eu pharetra metus convallis. "
                + "Morbi sem eros, venenatis vel vestibulum consequat, hendrerit rhoncus purus.";
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf16BE;

            // Encrypt the data as a stream.
            IBuffer buffProtected = await this.SampleDataProtectionStream(
                strDescriptor,
                strLoremIpsum,
                encoding);

            // Decrypt a data stream.
            String strUnprotected = await this.SampleDataUnprotectStream(
                buffProtected,
                encoding);
        }

        public async Task<IBuffer> SampleDataProtectionStream(
            String descriptor,
            String strMsg,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object for the specified descriptor.
            DataProtectionProvider Provider = new DataProtectionProvider(descriptor);

            // Convert the input string to a buffer.
            IBuffer buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a random access stream to contain the plaintext message.
            InMemoryRandomAccessStream inputData = new InMemoryRandomAccessStream();

            // Create a random access stream to contain the encrypted message.
            InMemoryRandomAccessStream protectedData = new InMemoryRandomAccessStream();

            // Retrieve an IOutputStream object and fill it with the input (plaintext) data.
            IOutputStream outputStream = inputData.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBuffer(buffMsg);
            await writer.StoreAsync();
            await outputStream.FlushAsync();

            // Retrieve an IInputStream object from which you can read the input data.
            IInputStream source = inputData.GetInputStreamAt(0);

            // Retrieve an IOutputStream object and fill it with encrypted data.
            IOutputStream dest = protectedData.GetOutputStreamAt(0);
            await Provider.ProtectStreamAsync(source, dest);
            await dest.FlushAsync();

            //Verify that the protected data does not match the original
            DataReader reader1 = new DataReader(inputData.GetInputStreamAt(0));
            DataReader reader2 = new DataReader(protectedData.GetInputStreamAt(0));
            await reader1.LoadAsync((uint)inputData.Size);
            await reader2.LoadAsync((uint)protectedData.Size);
            IBuffer buffOriginalData = reader1.ReadBuffer((uint)inputData.Size);
            IBuffer buffProtectedData = reader2.ReadBuffer((uint)protectedData.Size);

            if (CryptographicBuffer.Compare(buffOriginalData, buffProtectedData))
            {
                throw new Exception("ProtectStreamAsync returned unprotected data");
            }

            // Return the encrypted data.
            return buffProtectedData;
        }

        public async Task<String> SampleDataUnprotectStream(
            IBuffer buffProtected,
            BinaryStringEncoding encoding)
        {
            // Create a DataProtectionProvider object.
            DataProtectionProvider Provider = new DataProtectionProvider();

            // Create a random access stream to contain the encrypted message.
            InMemoryRandomAccessStream inputData = new InMemoryRandomAccessStream();

            // Create a random access stream to contain the decrypted data.
            InMemoryRandomAccessStream unprotectedData = new InMemoryRandomAccessStream();

            // Retrieve an IOutputStream object and fill it with the input (encrypted) data.
            IOutputStream outputStream = inputData.GetOutputStreamAt(0);
            DataWriter writer = new DataWriter(outputStream);
            writer.WriteBuffer(buffProtected);
            await writer.StoreAsync();
            await outputStream.FlushAsync();

            // Retrieve an IInputStream object from which you can read the input (encrypted) data.
            IInputStream source = inputData.GetInputStreamAt(0);

            // Retrieve an IOutputStream object and fill it with decrypted data.
            IOutputStream dest = unprotectedData.GetOutputStreamAt(0);
            await Provider.UnprotectStreamAsync(source, dest);
            await dest.FlushAsync();

            // Write the decrypted data to an IBuffer object.
            DataReader reader2 = new DataReader(unprotectedData.GetInputStreamAt(0));
            await reader2.LoadAsync((uint)unprotectedData.Size);
            IBuffer buffUnprotectedData = reader2.ReadBuffer((uint)unprotectedData.Size);

            // Convert the IBuffer object to a string using the same encoding that was
            // used previously to conver the plaintext string (before encryption) to an
            // IBuffer object.
            String strUnprotected = CryptographicBuffer.ConvertBinaryToString(encoding, buffUnprotectedData);

            // Return the decrypted data.
            return strUnprotected;
        }
    }
}
```