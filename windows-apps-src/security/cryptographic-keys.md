---
title: Chaves criptográficas
description: Este artigo mostra como usar funções de derivação de chaves padrão para derivar chaves e como criptografar conteúdo usando chaves simétricas e assimétricas.
ms.assetid: F35BEBDF-28C5-4F91-A94E-F7D862B6ED59
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 9ae9d6e3092acb994c755b01d2ed2487c71011c1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372598"
---
# <a name="cryptographic-keys"></a>Chaves criptográficas




Este artigo mostra como usar funções de derivação de chaves padrão para derivar chaves e como criptografar conteúdo usando chaves simétricas e assimétricas. 

## <a name="symmetric-keys"></a>Chaves simétricas


A criptografia de chave simétrica, também chamada de criptografia de chave secreta, exige o uso da mesma chave para criptografia e descriptografia. Você pode usar uma classe [**SymmetricKeyAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider) para especificar um algoritmo simétrico e criar ou importar uma chave. Você pode usar métodos estáticos na classe [**CryptographicEngine**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.CryptographicEngine) para criptografar e descriptografar dados usando o algoritmo e a chave.

A criptografia de chave simétrica geralmente usa codificações de bloco e modos de codificação de bloco. Uma codificação de bloco é uma função de criptografia simétrica que opera em blocos de tamanho fixo. Se a mensagem que você quer criptografar é mais longa do que o comprimento do bloco, você deve usar um modo de codificação de bloco. Um modo de codificação de bloco é uma função de criptografia simétrica construída usando uma codificação de bloco. Ele criptografa texto sem formatação como uma série de blocos de tamanho fixo. Os seguintes modos são permitidos nos aplicativos:

-   O modo ECB (livro de código eletrônico) criptografa cada bloco da mensagem separadamente. Esse não é considerado um modo de criptografia seguro.
-   O modo CBC (encadeamento de blocos de codificação) usa o bloco de texto cifrado anterior para obstruir o bloco atual. Você deve determinar qual valor usar para o primeiro bloco. Esse valor é chamado de vetor de inicialização (IV).
-   O modo CCM (contador com CBC-MAC) combina o modo de codificação de bloco CBC com um código de autenticação de mensagem (MAC).
-   O modo GCM (modo de contador Galois) combina o modo de criptografia de contador com o modo de autenticação Galois.

Alguns modos, como o CBC, exigem que você use um vetor de inicialização (IV) para o primeiro bloco de texto cifrado. Os seguintes são vetores de inicialização comuns. Você especifica o IV ao chamar [**CryptographicEngine.Encrypt**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.core.cryptographicengine.encrypt). Para a maioria das classes, é importante que o IV nunca seja reutilizado com a mesma chave.

-   Fixo usa o mesmo IV para todas as mensagens a serem criptografadas. Isso permite o vazamento de informações, e seu uso não é recomendado.
-   Contador incrementa o IV para cada bloco.
-   Aleatório cria um IV pseudoaleatório. Você pode usar [**CryptographicBuffer.GenerateRandom**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.cryptographicbuffer.generaterandom) para criar o IV.
-   Gerado por Valor de Uso Único usa um número exclusivo para cada mensagem a ser criptografada. Em geral, o valor de uso único é um identificador de mensagem ou transação modificado. O valor de uso único não precisa ser mantido em segredo, mas nunca deve ser reutilizado sob a mesma chave.

A maioria dos modos exige que o comprimento do texto não criptografado seja um múltiplo exato do tamanho do bloco. Isso geralmente requer que o texto não criptografado seja preenchido para obter o comprimento apropriado.

Enquanto as codificações de bloco são usadas para criptografar blocos de dados, as codificações de fluxo são funções de criptografia simétrica que combinam bits de texto não criptografado com um fluxo de bits pseudoaleatório (chamado de fluxo chave) para gerar o texto cifrado. Alguns modos de codificação de bloco, como o modo de feedback de saída (OTF) e o modo de contador (CTR), efetivamente transformam uma codificação de bloco em uma codificação de fluxo. Entretanto, as codificações de fluxo reais, como RC4, geralmente operam em velocidades mais altas do que as que podem ser atingidas pelos modos de codificação de bloco.

O exemplo a seguir mostra como usar a classe [**SymmetricKeyAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.SymmetricKeyAlgorithmProvider) para criar uma chave simétrica e usá-la para criptografar e descriptografar dados.

## <a name="asymmetric-keys"></a>Chaves assimétricas


Criptografia de chave assimétrica, também chamada de criptografia de chave pública, usa uma chave pública e uma chave privada para executar uma criptografia e descriptografia. As chaves são diferentes, mas matematicamente relacionadas. Normalmente, a chave privada é mantida em segredo e usada para descriptografar dados, enquanto a chave pública é distribuída para as partes interessadas e usada para criptografar dados. A criptografia assimétrica também é útil para assinar dados.

Como a criptografia assimétrica é muito mais lenta do que a simétrica, ela é raramente usada para criptografar grandes quantidades de dados diretamente. Em vez disso, ela é normalmente usada do modo a seguir para criptografar chaves.

-   Alice precisa que Bob envie somente mensagens criptografadas para ela.
-   Alice cria um par de chaves privada/pública, mantém sua chave privada em segredo e publica sua chave pública.
-   Bob quer enviar uma mensagem para Alice.
-   Bob cria uma chave simétrica.
-   Bob usa sua nova chave simétrica para criptografar sua mensagem para Alice.
-   Bob usa a chave pública de Alice para criptografar sua chave simétrica.
-   Bob envia a mensagem criptografada e a chave simétrica criptografada para Alice (em envelope).
-   Alice usa sua chave privada (do par privada/pública) para descriptografar a chave simétrica de Bob.
-   Alice usa a chave simétrica de Bob para descriptografar a mensagem.

Você pode usar um objeto [**AsymmetricKeyAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.AsymmetricKeyAlgorithmProvider) para especificar um algoritmo simétrico ou um algoritmo de assinatura, para criar ou importar um par de chaves efêmero ou para importar a parte de chave pública de um par de chaves.

## <a name="deriving-keys"></a>Derivando chaves


Geralmente é necessário derivar chaves adicionais de um segredo compartilhado. Você pode usar a classe [**KeyDerivationAlgorithmProvider**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.KeyDerivationAlgorithmProvider) e um dos métodos especializados abaixo na classe [**KeyDerivationParameters**](https://docs.microsoft.com/uwp/api/Windows.Security.Cryptography.Core.KeyDerivationParameters) para derivar chaves.

| Object                                                                            | Descrição                                                                                                                                |
|-----------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------------------------------------------------------|
| [**BuildForPbkdf2**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforpbkdf2)    | Cria um objeto KeyDerivationParameters para uso na função de derivação de chaves 2 (PBKDF2) baseada em senha.                                 |
| [**BuildForSP800108**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforsp800108)  | Cria um objeto KeyDerivationParameters para uso em uma função de derivação de chaves (HMAC) com código de autenticação de mensagens em um modo de contador com base em hash. |
| [**BuildForSP80056a**](https://docs.microsoft.com/uwp/api/windows.security.cryptography.core.keyderivationparameters.buildforsp80056a)  | Cria um objeto KeyDerivationParameters para uso na função de derivação de chaves SP800-56A.                                                 |

 
