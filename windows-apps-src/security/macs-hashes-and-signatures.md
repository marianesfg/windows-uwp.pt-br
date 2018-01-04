---
title: MACs, hashes e assinaturas
description: "Esse artigo discute como códigos de autenticação de mensagem (MACs), hashes e assinaturas podem ser usados em aplicativos da Plataforma Universal do Windows (UWP) para detectar violação de mensagem."
ms.assetid: E674312F-6678-44C5-91D9-B489F49C4D3C
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 4de3b93cb6f86f409d6f915386b9d763056e89fa
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: HT
ms.contentlocale: pt-BR
---
# <a name="macs-hashes-and-signatures"></a>MACs, hashes e assinaturas


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Esse artigo discute como códigos de autenticação de mensagem (MACs), hashes e assinaturas podem ser usados em aplicativos da Plataforma Universal do Windows (UWP) para detectar violação de mensagem.

## <a name="message-authentication-codes-macs"></a>Códigos de autenticação de mensagem (MACs)


A criptografia ajuda a impedir que pessoas não autorizadas leiam uma mensagem, mas não as impede de adulterá-la. A alteração de uma mensagem (ainda que resulte apenas em tolices) pode resultar em prejuízos. O MAC (Message Authentication Code) ajuda a impedir a adulteração de mensagens. Por exemplo, imagine a seguinte situação:

-   Bob e Alice compartilham uma chave secreta e concordaram em usar uma função MAC.
-   Bob cria uma mensagem e a insere, juntamente com a chave secreta, em uma função MAC para recuperar um valor MAC.
-   Bob envia para Alice, por uma rede, a mensagem \[não criptografada\] e o valor MAC.
-   Alice usa a chave secreta e a mensagem como entrada para a função MAC. Ela compara o valor MAC gerado com o valor MAC enviado por Bob. Se forem idênticos, significa que a mensagem não foi alterada em trânsito.

Eva, que está secretamente atenta à conversa entre Bob e Alice, não pode manipular efetivamente a mensagem. Ela não tem acesso à chave privada e, portanto, não pode criar um valor MAC que faria a mensagem adulterada parecer legítima para Alice.

Criar um MAC ajuda a garantir apenas que a mensagem original não foi alterada. Juntamente com o uso de uma chave secreta compartilhada, garante também que o hash de mensagem foi assinado por alguém com acesso a essa chave privada.

Use o [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) para enumerar os algoritmos MAC disponíveis e gerar uma chave simétrica. Você pode usar métodos estáticos na classe [**CryptographicEngine**](https://msdn.microsoft.com/library/windows/apps/br241490) para executar a criptografia necessária à criação do valor MAC.

As assinaturas digitais são as chaves públicas equivalentes aos códigos de autenticação de mensagens de chaves (MACs). Os MACs usam chaves privadas (para que o destinatário da mensagem possa verificar se ela não foi alterada durante a transmissão), ao passo que as assinaturas usam um par de chaves privada/pública.

Esse código de exemplo mostra como usar a classe [**MacAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241530) para criar um HMAC (código de autenticação de mensagens com hash).

```cs
using Windows.Security.Cryptography;
using Windows.Security.Cryptography.Core;
using Windows.Storage.Streams;

namespace SampleMacAlgorithmProvider
{
    sealed partial class MacAlgProviderApp : Application
    {
        public MacAlgProviderApp()
        {
            // Initialize the application.
            this.InitializeComponent();

            // Initialize the hashing process.
            String strMsg = "This is a message to be authenticated";
            String strAlgName = MacAlgorithmNames.HmacSha384;
            IBuffer buffMsg;
            CryptographicKey hmacKey;
            IBuffer buffHMAC;

            // Create a hashed message authentication code (HMAC)
            this.CreateHMAC(
                strMsg,
                strAlgName,
                out buffMsg,
                out hmacKey,
                out buffHMAC);

            // Verify the HMAC.
            this.VerifyHMAC(
                buffMsg,
                hmacKey,
                buffHMAC);
        }

        void CreateHMAC(
            String strMsg,
            String strAlgName,
            out IBuffer buffMsg,
            out CryptographicKey hmacKey,
            out IBuffer buffHMAC)
        {
            // Create a MacAlgorithmProvider object for the specified algorithm.
            MacAlgorithmProvider objMacProv = MacAlgorithmProvider.OpenAlgorithm(strAlgName);

            // Demonstrate how to retrieve the name of the algorithm used.
            String strNameUsed = objMacProv.AlgorithmName;

            // Create a buffer that contains the message to be signed.
            BinaryStringEncoding encoding = BinaryStringEncoding.Utf8;
            buffMsg = CryptographicBuffer.ConvertStringToBinary(strMsg, encoding);

            // Create a key to be signed with the message.
            IBuffer buffKeyMaterial = CryptographicBuffer.GenerateRandom(objMacProv.MacLength);
            hmacKey = objMacProv.CreateKey(buffKeyMaterial);

            // Sign the key and message together.
            buffHMAC = CryptographicEngine.Sign(hmacKey, buffMsg);

            // Verify that the HMAC length is correct for the selected algorithm
            if (buffHMAC.Length != objMacProv.MacLength)
            {
                throw new Exception("Error computing digest");
            }
         }

        public void VerifyHMAC(
            IBuffer buffMsg,
            CryptographicKey hmacKey,
            IBuffer buffHMAC)
        {
            // The input key must be securely shared between the sender of the HMAC and 
            // the recipient. The recipient uses the CryptographicEngine.VerifySignature() 
            // method as follows to verify that the message has not been altered in transit.
            Boolean IsAuthenticated = CryptographicEngine.VerifySignature(hmacKey, buffMsg, buffHMAC);
            if (!IsAuthenticated)
            {
                throw new Exception("The message cannot be verified.");
            }
        }
    }
}
```

## <a name="hashes"></a>Hashes


Uma função hash criptográfica assume um bloco de dados arbitrariamente longo e devolve uma sequência de caracteres de bit de tamanho fixo. As funções hash são usadas normalmente quando da assinatura de dados. Como a maioria das operações de assinaturas de chaves públicas, geralmente é mais eficiente assinar (criptografar) um hash de mensagem do que assinar a mensagem original. O seguinte procedimento representa um cenário comum e simplificado:

-   Bob e Alice compartilham uma chave secreta e concordaram em usar uma função MAC.
-   Bob cria uma mensagem e a insere, juntamente com a chave secreta, em uma função MAC para recuperar um valor MAC.
-   Bob envia para Alice, por uma rede, a mensagem \[não criptografada\] e o valor MAC.
-   Alice usa a chave secreta e a mensagem como entrada para a função MAC. Ela compara o valor MAC gerado com o valor MAC enviado por Bob. Se forem idênticos, significa que a mensagem não foi alterada em trânsito.

Observe que Paula enviou uma mensagem não criptografada. Somente o hash foi criptografado. O procedimento garante somente que a mensagem original não foi alterada e, com o uso da chave pública de Alice, garante que o hash da mensagem foi assinado por alguém com acesso à chave particular de Alice, presumivelmente Alice.

Você pode usar a classe [**HashAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241511) para enumerar os algoritmos de hash disponíveis e criar um valor [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498).

As assinaturas digitais são as chaves públicas equivalentes aos códigos de autenticação de mensagens de chaves (MACs). Enquanto MACs utilizam chaves particulares para habilitar um destinatário de mensagem a verificar se uma mensagem não foi alterada durante a transmissão, as assinaturas usam um par de chaves privada/pública.

O objeto [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) pode ser usado para fazer hash repetidamente de diversos dados sem necessidade de recriar o objeto para cada uso. O método [**Append**](https://msdn.microsoft.com/library/windows/apps/br241499) adiciona dados novos a um buffer para hash. O método [**GetValueAndReset**](https://msdn.microsoft.com/library/windows/apps/hh701376) faz hash dos dados e reinicializa o objeto para outro uso. Isso é demonstrado no exemplo a seguir.

```cs
public void SampleReusableHash()
{
    // Create a string that contains the name of the hashing algorithm to use.
    String strAlgName = HashAlgorithmNames.Sha512;

    // Create a HashAlgorithmProvider object.
    HashAlgorithmProvider objAlgProv = HashAlgorithmProvider.OpenAlgorithm(strAlgName);

    // Create a CryptographicHash object. This object can be reused to continually
    // hash new messages.
    CryptographicHash objHash = objAlgProv.CreateHash();

    // Hash message 1.
    String strMsg1 = "This is message 1.";
    IBuffer buffMsg1 = CryptographicBuffer.ConvertStringToBinary(strMsg1, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg1);
    IBuffer buffHash1 = objHash.GetValueAndReset();

    // Hash message 2.
    String strMsg2 = "This is message 2.";
    IBuffer buffMsg2 = CryptographicBuffer.ConvertStringToBinary(strMsg2, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg2);
    IBuffer buffHash2 = objHash.GetValueAndReset();

    // Hash message 3.
    String strMsg3 = "This is message 3.";
    IBuffer buffMsg3 = CryptographicBuffer.ConvertStringToBinary(strMsg3, BinaryStringEncoding.Utf16BE);
    objHash.Append(buffMsg3);
    IBuffer buffHash3 = objHash.GetValueAndReset();

    // Convert the hashes to string values (for display);
    String strHash1 = CryptographicBuffer.EncodeToBase64String(buffHash1);
    String strHash2 = CryptographicBuffer.EncodeToBase64String(buffHash2);
    String strHash3 = CryptographicBuffer.EncodeToBase64String(buffHash3);
}

```

## <a name="digital-signatures"></a>Assinaturas digitais


As assinaturas digitais são as chaves públicas equivalentes aos códigos de autenticação de mensagens de chaves (MACs). Enquanto MACs utilizam chaves particulares para habilitar um destinatário de mensagem a verificar se uma mensagem não foi alterada durante a transmissão, as assinaturas usam um par de chaves particulares/públicas.

Como a maioria das operações de assinaturas de chaves públicas, geralmente é mais eficiente assinar (criptografar) um hash de mensagem do que assinar a mensagem original. O remetente cria um hash de mensagem, assina e envia a assinatura e a mensagem (não criptografada). O destinatário calcula um hash sobre a mensagem, descriptografa a assinatura e compara a assinatura descriptografada com o valor hash. Se eles combinarem, o destinatário pode estar quase certo de que a mensagem está correta, de fato, veio do remetente e não foi alterada durante a transmissão.

A assinatura garante somente que a mensagem original não foi alterada e, ao usar a chave pública do remetente, assegura também que o hash da mensagem foi assinado por alguém com acesso à chave particular.

Você pode usar um objeto [**AsymmetricKeyAlgorithmProvider**](https://msdn.microsoft.com/library/windows/apps/br241478) para enumerar os algoritmos de assinaturas disponíveis e gerar ou importar um par de chaves. É possível usar métodos estáticos na classe [**CryptographicHash**](https://msdn.microsoft.com/library/windows/apps/br241498) para assinar uma mensagem ou verificar uma assinatura.