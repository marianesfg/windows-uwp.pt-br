---
title: Restrições de exportação na criptografia
description: Use esta informação para determinar se seu aplicativo usa criptografia de forma que pode impedir que ele seja listado na Microsoft Store.
ms.assetid: 204C7D1D-6F08-4AEE-A333-434D715E7617
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: c647d91213ddf1fd8a3dafd80c6888a026cda576
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74258932"
---
# <a name="export-restrictions-on-cryptography"></a>Restrições de exportação na criptografia



Use esta informação para determinar se seu aplicativo usa criptografia de forma que pode impedir que ele seja listado na Microsoft Store.

O Bureau of Industry and Security do Departamento de Comércio dos Estados Unidos regula a exportação de tecnologia que usa determinados tipos de criptografia. Todos os apps listados na Microsoft Store devem estar em conformidade com estas leis e regulamentações porque os arquivos do aplicativo podem ser armazenados nos Estados Unidos. Mesmo os aplicativos carregados por desenvolvedores de aplicativos de outros países/regiões para distribuição fora dos Estados Unidos devem estar em conformidade com essas regulamentações. Sendo assim, ao enviar um aplicativo para a Microsoft Store, todos os desenvolvedores devem garantir que seus apps não contenham tecnologia proibida por essas regulamentações.

> **Observe**  as informações fornecidas aqui fornecem algumas diretrizes, mas é sua responsabilidade como desenvolvedor de aplicativos que está publicando aplicativos no Microsoft Store para garantir que seu aplicativo esteja em conformidade com todas as leis e regulamentos aplicáveis.

 

Para saber mais sobre o Departamento de Comércio dos EUA e o Bureau of Industry and Security, consulte [Bureau of Industry and Security](https://www.bis.doc.gov/about/index.htm).

Para saber mais sobre os regulamentos EAR (Controle de exportações dos EUA) que regem a exportação de tecnologia que inclui criptografia, consulte [Controles de EAR para itens que usam criptografia](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="governed-uses"></a>Usos controlados

Primeiro, determine se seu aplicativo usa um tipo de criptografia controlado pelos regulamentos do Controle de exportação dos EUA. A questão inclui os exemplos mostrados nesta lista; mas lembre-se de que esta lista não inclui todos os aplicativos possíveis de criptografia.

> **Importante**  considere não apenas o código que você escreveu para seu aplicativo, mas também todas as bibliotecas de software, utilitários e componentes do sistema operacional aos quais seu aplicativo inclui ou links.

-   Qualquer uso de uma assinatura digital, como autenticação ou verificação de integridade
-   Criptografia de dados ou arquivos que seu aplicativo usa ou acessa
-   Gerenciamento de chaves, gerenciamento de certificados ou qualquer coisa que interage com uma infraestrutura de chave pública
-   Usando um canal de comunicação seguro como NTLM, Kerberos, SSL ou TLS
-   Criptografando senhas ou outras formas de segurança da informação
-   Proteção contra cópia ou DRM (gerenciamento de direitos digitais)
-   Proteção antivírus

Para obter a lista completa e atualizada de aplicativos de criptografia, consulte [Controles de EAR para itens que usam criptografia](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

## <a name="non-restricted-uses"></a>Usos não restritos

Observe que alguns dos aplicativos de criptografia não são restritos. Estas são as tarefas sem restrições:

-   Criptografia de senha
-   Proteção contra cópia
-   Autenticação
-   Gerenciamento de direitos digitais
-   Usando assinaturas digitais

Para obter a lista completa e atualizada de aplicativos de criptografia, consulte [Controles de EAR para itens que usam criptografia](https://www.bis.doc.gov/index.php/policy-guidance/encryption).

Se seu aplicativo chamar, der suporte, contiver ou usar criptografia para qualquer tarefa que não esteja nessa lista, ele precisará de um ECCN (Número de classificação de mercadoria de exportação).

Se você não tiver um ECCN, consulte [Perguntas e respostas sobre ECCN](https://www.bis.doc.gov/licensing/do_i_needaneccn.html).
