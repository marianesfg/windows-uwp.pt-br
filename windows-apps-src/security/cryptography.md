---
title: Criptografia
description: O artigo fornece uma visão geral dos recursos de criptografia disponíveis para aplicativos UWP (Plataforma Universal do Windows). Para obter informações detalhadas sobre tarefas específicas, consulte a tabela no final deste artigo.
ms.assetid: 9C213036-47FD-4AA4-99E0-84006BE63F47
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: aa01cc3d70db7a94667e944d1a1739e911f94b0c
ms.sourcegitcommit: c8f6866100a4b38fdda8394ea185b02d7af66411
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/13/2018
ms.locfileid: "3959458"
---
# <a name="cryptography"></a>Criptografia




O artigo fornece uma visão geral dos recursos de criptografia disponíveis para aplicativos UWP (Plataforma Universal do Windows). Para obter informações detalhadas sobre tarefas específicas, consulte a tabela no final deste artigo.

## <a name="terminology"></a>Terminologia


A terminologia a seguir é usada comumente em criptografia e infraestrutura de chave pública (PKI).

| Termo                        | Descrição                                                                                                                                                                                           |
|-----------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| Criptografia                  | O processo de transformar dados usando um algoritmo criptográfico e uma chave. Os dados transformados podem ser recuperados somente usando o mesmo algoritmo e a mesma chave (pública) simétrica ou relacionada. |
| Descriptografia                  | O processo de voltar os dados criptografados à sua forma original.                                                                                                                                         |
| Texto sem formatação                   | Originalmente, se referia a uma mensagem de texto não criptografada. Atualmente, refere-se a qualquer dado não criptografado.                                                                                                         |
| Texto cifrado                  | Originalmente, se referia a uma mensagem de texto criptografada e, portanto, ilegível. Atualmente, refere-se a qualquer dado criptografado.                                                                                  |
| Hash                     | O processo de converter dados de comprimento variável em um valor de comprimento fixo, geralmente menor. Ao comparar hashes, você pode obter uma garantia razoável de que dois ou mais dados são iguais.            |
| Assinatura                   | Hash criptografado de dados digitais, geralmente usado para autenticar o remetente dos dados ou verificar se os dados não foram violados durante a transmissão.                                               |
| Algoritmo                   | Um procedimento passo a passo para criptografar dados.                                                                                                                                                         |
| Chave                         | Um número aleatório ou pseudoaleatório, usado como entrada para um algoritmo criptográfico para criptografar e descriptografar dados.                                                                                               |
| Criptografia de Chave Simétrica  | Modo no qual a criptografia e a descriptografia usam a mesma chave. Também conhecido como criptografia de chave secreta.                                                                                      |
| Criptografia de Chave Assimétrica | Modo no qual a criptografia e a descriptografia usam uma chave diferente, porém matematicamente relacionada. Também conhecido como criptografia de chave pública.                                                          |
| Codificação                    | O processo de codificar mensagens digitais, incluindo certificados, para transporte por uma rede.                                                                                                     |
| Provedor de Algoritmo          | Uma DLL que implementa um algoritmo criptográfico.                                                                                                                                                      |
| Provedor de Armazenamento de Chaves        | Um contêiner para armazenar material de chaves. Atualmente, as chaves podem ser armazenadas em software, cartões inteligentes ou no módulo de plataforma confiável (TPM).                                                                   |
| Certificado X.509           | Um documento digital, geralmente emitido por uma autoridade de certificação, para verificar a identidade de um indivíduo, sistema ou entidade para outras partes interessadas.                                            |

 
## <a name="namespaces"></a>Namespaces

Os seguintes namespaces estão disponíveis para uso em aplicativos.

### <a name="windowssecuritycryptography"></a>Windows.Security.Cryptography

Contém a classe CryptographicBuffer e métodos estáticos que permitem:

-   Converter dados de/para cadeias de caracteres
-   Converter dados de/para matrizes de bytes
-   Codificar mensagens para transporte de rede
-   Decodificar mensagens depois do transporte

### <a name="windowssecuritycryptographycertificates"></a>Windows.Security.Cryptography.Certificates

Contém classes, interfaces e tipos de enumeração que permitem:

-   Criar uma solicitação de certificado
-   Instalar uma resposta de certificado
-   Importar um certificado em um arquivo PFX
-   Especificar e recuperar propriedades de solicitação de certificado

### <a name="windowssecuritycryptographycore"></a>Windows.Security.Cryptography.Core

Contém classes e tipos de enumeração que permitem:

-   Criptografar e descriptografar dados
-   Fazer hash de dados
-   Assinar dados e verificar assinaturas
-   Criar, importar e exportar chaves
-   Trabalhar com provedores de algoritmo de chave assimétrica
-   Trabalhar com provedores de algoritmo de chave simétrica
-   Trabalhar com provedores de algoritmo de hash
-   Trabalhar com provedores de algoritmo MAC
-   Trabalhar com provedores de algoritmo de derivação de chave

### <a name="windowssecuritycryptographydataprotection"></a>Windows.Security.Cryptography.DataProtection

Contém classes que permitem:

-   Criptografar e descriptografar dados estáticos de modo assíncrono.
-   Criptografar e descriptografar fluxos de dados de modo assíncrono.

## <a name="crypto-and-pki-application-capabilities"></a>Recursos do aplicativo de PKI e criptografia


A interface de programação de aplicativo simplificada disponível para aplicativos permite as seguintes funcionalidades criptográficas e de PKI (infraestrutura de chave pública):

### <a name="cryptography-support"></a>Suporte à criptografia

Você pode executar as seguintes tarefas criptográficas. Para obter mais informações, consulte o namespace [**Windows.Security.Cryptography.Core**](https://msdn.microsoft.com/library/windows/apps/br241547).

-   Criar chaves simétricas
-   Executar criptografia simétrica
-   Criar chaves assimétricas
-   Executar criptografia assimétrica
-   Derivar chaves baseadas em senhas
-   Criar códigos de autenticação de mensagens (MACs)
-   Conteúdo de hash
-   Conteúdo assinado digitalmente

O SDK também fornece uma interface simplificada para proteção de dados baseada em senha. Você pode usá-la para executar estas tarefas. Para obter mais informações, consulte o namespace [**Windows.Security.Cryptography.DataProtection**](https://msdn.microsoft.com/library/windows/apps/br241585).

-   Proteção assíncrona de dados estáticos
-   Proteção assíncrona de um fluxo de dados

### <a name="encoding-support"></a>Suporte à codificação

Um aplicativo pode codificar dados criptográficos para transmissão por uma rede e decodificar dados recebidos de uma fonte de rede. Para obter mais informações, consulte os métodos estáticos disponíveis no namespace [**Windows.Security.Cryptography**](https://msdn.microsoft.com/library/windows/apps/br241404).

### <a name="pki-support"></a>Suporte a PKI

Os aplicativos podem executar as seguintes tarefas PKI. Para obter mais informações, consulte o namespace [**Windows.Security.Cryptography.Certificates**](https://msdn.microsoft.com/library/windows/apps/br241476).

-   Criar um certificado
-   Criar um certificado autoassinado
-   Instalar uma resposta de certificado
-   Importar um certificado em formato PFX
-   Usar chaves e certificados de cartão inteligente (conjunto de recursos sharedUserCertificates)
-   Usar certificados do MEU repositório do usuário (conjunto de recursos sharedUserCertificates)

Além disso, você pode usar o manifesto para realizar as seguintes ações:

-   Especificar certificados raiz confiáveis para cada aplicativo
-   Especificar certificados confiáveis de par para cada aplicativo
-   Explicitamente desabilitar a herança de confiança do sistema
-   Especificar os critérios de seleção de certificado
    -   Somente certificados de hardware
    -   Certificados que são encadeados através de um conjunto especificado de emissores
    -   Selecionar automaticamente um certificado do repositório de aplicativos

## <a name="detailed-articles"></a>Artigos detalhados


Os artigos a seguir fornecem mais detalhes sobre cenários de segurança:

| Tópico                                                                         | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                |
|-------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Certificados](certificates.md)                                               | Este artigo aborda o uso de certificados em aplicativos UWP. Os certificados digitais são usados na criptografia de chave pública para associar uma chave pública a uma pessoa, um computador ou uma organização. As identidades associadas são usadas com mais frequência para autenticar uma entidade em outra. Por exemplo, os certificados são geralmente usados para autenticar um servidor Web em um usuário e um usuário em um servidor Web. É possível criar solicitações de certificados e instalar ou importar os certificados emitidos. Também é possível inscrever um certificado em uma hierarquia de certificados. |
| [Chaves criptográficas](cryptographic-keys.md)                                   | Este artigo mostra como usar funções de derivação de chaves padrão para derivar chaves e como criptografar conteúdo usando chaves simétricas e assimétricas.                                                                                                                                                                                                                                                                                                                                                                             |
| [Proteção de dados](data-protection.md)                                         | Este artigo explica como usar a classe [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) no [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) para criptografar e descriptografar dados digitais em um aplicativo UWP.                                                                                                                                                                                                                  |
| [MACs, hashes e assinaturas](macs-hashes-and-signatures.md)               | Este artigo aborda como códigos de autenticação de mensagem (MACs), hashes e assinaturas podem ser usados em aplicativos UWP para detectar adulteração de mensagem.                                                                                                                                                                                                                                                                                                                                                                                |
| [Restrições de exportação na criptografia](export-restrictions-on-cryptography.md) | Use esta informação para determinar se seu aplicativo usa criptografia de forma que pode impedir que ele seja listado na Microsoft Store.                                                                                                                                                                                                                                                                                                                                                                                            |
| [Tarefas comuns de criptografia](common-cryptography-tasks.md)                     | Estes artigos fornecem código de amostra para tarefas de criptografia comuns da UWP, como criar números aleatórios, comparar buffers, converter entre cadeias de caracteres e dados binários, copiar de e para matrizes de bytes e codificar e decodificar dados.                                                                                                                                                                                                                                                                                    |

 