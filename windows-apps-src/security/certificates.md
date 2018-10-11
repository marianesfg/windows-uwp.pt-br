---
title: Introdução a certificados
description: Este artigo discute o uso de certificados em aplicativos da Plataforma Universal do Windows (UWP).
ms.assetid: 4EA2A9DF-BA6B-45FC-AC46-2C8FC085F90D
author: msatranjr
ms.author: misatran
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 1db3af004831f010a3dd4918898ce5f7cf70bb1a
ms.sourcegitcommit: 933e71a31989f8063b020746fdd16e9da94a44c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2018
ms.locfileid: "4532798"
---
# <a name="intro-to-certificates"></a>Introdução a certificados




Este artigo discute o uso de certificados em aplicativos da Plataforma Universal do Windows (UWP). Os certificados digitais são usados na criptografia de chave pública para associar uma chave pública a uma pessoa, um computador ou uma organização. As identidades associadas são usadas com mais frequência para autenticar uma entidade em outra. Por exemplo, os certificados são geralmente usados para autenticar um servidor Web em um usuário e um usuário em um servidor Web. É possível criar solicitações de certificados e instalar ou importar os certificados emitidos. Também é possível inscrever um certificado em uma hierarquia de certificados.

### <a name="shared-certificate-stores"></a>Repositórios de certificados compartilhados

Os aplicativos UWP usam o novo modelo de aplicativo isolationist introduzido no Windows 8. Nesse modelo, os aplicativos são executados em uma construção do sistema operacional de baixo nível, chamada de contêiner de aplicativo, que proíbe o aplicativo de acessar recursos ou arquivos fora de si mesmo, a menos que haja permissão explícita para fazer isso. As seções a seguir descrevem as implicações que isso tem na infraestrutura de chave pública (PKI).

### <a name="certificate-storage-per-app-container"></a>Repositório de certificados por contêiner de aplicativo

Certificados para uso em um contêiner de aplicativo específico são armazenados com base no usuário e nos locais dos contêineres de aplicativo. Um aplicativo executado em um contêiner de aplicativo tem acesso de gravação apenas a seu próprio armazenamento de certificado. Se o aplicativo adiciona certificados a qualquer um dos seus repositórios, esses certificados não podem ser lidos por outros aplicativos. Quando um aplicativo é desinstalado, todos os seus certificados específicos também são removidos. Um aplicativo também tem acesso de leitura a repositórios de certificados do computador local, diferentes do MY e do REQUEST.

### <a name="cache"></a>Cache

Cada contêiner de aplicativo possui um cache isolado no qual ele pode armazenar certificados de emissor necessários para validação, listas de revogação de certificados (CRL) e respostas do protocolo de status de certificado on-line (OCSP).

### <a name="shared-certificates-and-keys"></a>Certificados e chaves compartilhados

Quando um cartão inteligente é inserido no leitor, os certificados e as chaves contidos no cartão são propagados para o MEU repositório do usuário, onde podem ser compartilhados por qualquer aplicativo de confiança total que o usuário esteja executando. Por padrão, contudo, contêineres de aplicativo não têm acesso ao MEU repositório por usuário.

Para corrigir esse problema e permitir que grupos de entidades acessem grupos de recursos, o modelo de isolamento do contêiner de aplicativo dá suporte ao conceito de funcionalidades. Uma funcionalidade permite que um processo de contêiner de aplicativo acesse um recurso específico. A funcionalidade sharedUserCertificates concede, ao contêiner do aplicativo, acesso aos certificados e chaves contidos no repositório MY e no repositório Raízes Confiáveis do Cartão Inteligente do usuário. A funcionalidade não concede acesso de leitura ao repositório de SOLICITAÇÃO do usuário.

Você especifica a funcionalidade sharedUserCertificates no manifesto conforme demonstrado no exemplo a seguir.

```xml
<Capabilities>
    <Capability Name="sharedUserCertificates" />
</Capabilities>
```

## <a name="certificate-fields"></a>Campos do certificado


O padrão de certificado de chave pública X.509 foi revisado ao longo do tempo. Todas as versões sucessivas da estrutura de dados mantiveram os campos que existiam nas versões anteriores e adicionaram mais, conforme mostrado na ilustração a seguir.

![Versões 1, 2 e 3 do certificado x.509](images/x509certificateversions.png)

Alguns destes campos e extensões podem ser especificados diretamente quando você usa a classe [**CertificateRequestProperties**](https://msdn.microsoft.com/library/windows/apps/br212079) para criar uma solicitação de certificado. A maioria não pode. Esses campos podem ser preenchidos pela autoridade de emissão ou podem ser deixados em branco. Para saber mais sobre os campos, consulte as seguintes seções:

### <a name="version-1-fields"></a>Campos da versão 1

| Campo | Descrição |
|-------|-------------|
| Versão | Especifica o número de versão do certificado codificado. Atualmente, os possíveis valores desse campo são 0, 1 ou 2. |
| Número de Série | Contém um inteiro positivo único atribuído pela AC (autoridade de certificação) para o certificado. |
| Algoritmo de Assinatura | Contém um OID (identificador de objeto) que especifica o algoritmo usado pela AC para assinar o certificado. Por exemplo, 1.2.840.113549.1.1.5 especifica um algoritmo de hash SHA-1 combinado com o algoritmo de criptografia RSA da RSA Laboratories. |
| Emissor | Contém o DN (nome diferenciado) X.500 da CA que criou e assinou o certificado. |
| Validade | Especifica o intervalo de tempo durante o qual o certificado é válido. As datas até o final de 2049 usam o formato de Tempo Universal Coordenado (Hora de Greenwich) (yymmddhhmmssz). As datas a partir de 1º de janeiro de 2050 usam o formato de tempo genérico (yyyymmddhhmmssz). |
| Requerente | Contém um nome diferenciado X.500 da entidade associada à chave pública contida no certificado. |
| Chave Pública | Contém a chave pública e as informações de algoritmo associadas. |

### <a name="version-2-fields"></a>Campos da versão 2

Um certificado X.509 versão 2 contém os campos básicos definidos na versão 1 e adiciona os campos a seguir.

| Campo | Descrição |
|-------|-------------|
| Identificador Exclusivo de Emissor | Contém um valor exclusivo que pode ser usado para tornar o nome X.500 da AC não ambíguo quando reutilizado por entidades diferentes com o tempo. |
| Identificador Exclusivo de Entidade | Contém um valor exclusivo que pode ser usado para tornar o nome X.500 da entidade do certificado não ambíguo quando reutilizado por entidades diferentes com o tempo. |

### <a name="version-3-extensions"></a>Extensões da versão 3

Um certificado X.509 versão 3 contém os campos definidos na versão 1 e 2, e adiciona extensões de certificado.

| Campo  | Descrição |
|--------|-------------|
| Identificador da Chave da Autoridade | Identifica a chave pública da autoridade de certificação (AC) que corresponde à chave privada da AC usada para assinar o certificado. |
| Restrições Básicas | Especifica se a entidade pode ser usada como AC e, em caso afirmativo, o número de ACs subordinadas que podem existir abaixo dela na cadeia de certificados. |
| Políticas de Certificado | Especifica as políticas sob as quais o certificado foi emitido e as finalidades para as quais ele pode ser usado. |
| Pontos de Distribuição de CRL | Contém o URI da lista de certificados revogados (CRL) de base. |
| Uso Avançado de Chave | Especifica a maneira na qual a chave pública contida no certificado pode ser usada. |
| Nome Alternativo para o Emissor | Especifica uma ou mais formas de nome alternativas para o emissor da solicitação de certificado. |
| Uso de Chave | Especifica restrições nas operações que podem ser executadas pela chave pública contida no certificado.|
| Restrições de Nome  | Especifica o namespace dentro do qual todos os nomes de entidades em uma hierarquia de certificados devem estar localizados. A extensão é usada somente em um certificado de AC. |
| Restrições de Política | Restringe a validação do caminho, proibindo o mapeamento de política ou exigindo que cada certificado na hierarquia contenha um identificador de política aceitável. A extensão é usada somente em um certificado de AC. |
| Mapeamentos de Política | Especifica as políticas em uma AC subordinada que correspondem às políticas na AC emissora. |
| Período de Uso da Chave Privada | Especifica um período de validade para a chave privada diferente do período para o certificado com o qual a chave privada está associada. |
| Nome Alternativo para a Entidade | Especifica uma ou mais formas de nome alternativas para a entidade da solicitação de certificado. Dentre os exemplos de formas alternativas incluem-se endereços de email, nomes DNS, endereços IP e URIs. |
| Atributos do Diretório de Entidade | Expressa atributos de identificação, como a nacionalidade da entidade do certificado. O valor da extensão é uma sequência de pares de valores OID. |
| Identificador da Chave da Entidade | Diferencia entre várias chaves públicas mantidas pela entidade do certificado. O valor da extensão é geralmente um hash SHA-1 da chave. |

