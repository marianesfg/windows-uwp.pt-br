---
title: Autorização de EDS
assetID: bd0bdc8e-084a-7140-98da-6d3721bda112
permalink: en-us/docs/xboxlive/rest/edsauthorization.html
description: " Autorização de EDS"
ms.date: 10/12/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 3e5c5ef3bf3c864215544391bc291a26f6c05d0f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57607601"
---
# <a name="eds-authorization"></a>Autorização de EDS
 
  * [Introdução](#ID4EN)
  * [Processo de autorização](#ID4EFB)
  * [3.0 tokens de: Multiuser vs. Usuário único](#ID4EEC)
  * [EDS dá suporte a vários usuários?](#ID4EYC)
 
<a id="ID4EN"></a>

 
## <a name="introduction"></a>Introdução
 
Serviços de descoberta de entretenimento (EDS) 3.1 não dará suporte a tráfego anônimo. Autenticação é necessária para todas as solicitações para EDS. EDS exigirá um XToken de chamadores autenticar corretamente os clientes. Esses tokens são produzidos por XSTS e podem ser obtidos por meio de serviços de autenticação de Xbox vários (XAS). Há serviços de autenticação separado para o dispositivo, os usuários e títulos de todos os definirão a identidade do token.
 
XSTS é o gatekeeper para Xbox LIVE. É a primeira linha de defesa para determinar se um dispositivo ou usuário está autorizado a se conectar a qualquer um dos serviços do Xbox LIVE. Depois de autenticar o usuário, o XSTS gera um XToken que eles podem usar para se identificar para qualquer componente no serviço. Este XToken é seu Passaport útil.
 
As pessoas e as coisas, para usar nossos serviços. E queremos que a maioria dessas coisas e pessoas para poder usar nossos serviços. Mas como podemos ter certeza de que as coisas não são fingindo ser pessoas e as pessoas realmente são quem dizem ser? Fornecemos-los com os tokens que eles podem usar para se identificar aos outros.
 
Esses tokens são produzidos pelo XSTS e são geralmente denominados XTokens. XToken é um termo abrangente que é usado para cobrir os tokens que contêm uma variedade de coisas diferentes e podem vir de várias formas diferentes, mas eles são todos criados, assinados e opcionalmente criptografados pelo servidor XSTS. Internamente, XSTS usa MXAN para criar e formatar os tokens. MXAN é o único componente que nunca extrai informações de um XToken. Os serviços consumindo os tokens passam os cabeçalhos de solicitação para MXAN para ser decifradas. Os tokens são processados e validados e as declarações que expressa no token são retornadas para o serviço. O serviço pode em seguida, use que esses valores para identificar o usuário ou dispositivo de chamada e executar ações com base nessas informações de declaração.
 
Os tokens de identidade básico - para o usuário, dispositivo e título – são fornecidos por serviços de autenticação de Xbox (XAS). Cada XAS é responsável por gerar um token de identidade que especifica valores para várias declarações que eles são responsáveis.
 
   * XASD (XAS para dispositivos): cria um DToken que fornece uma identidade de dispositivo
   * XASU (XAS para usuários): cria um UToken que fornece uma identidade de usuário
   * XAST (XAS para títulos): cria um TToken que fornece uma identidade de título
   
<a id="ID4EFB"></a>

 
## <a name="authorization-process"></a>Processo de autorização
 
   * Obter um ou mais tokens de identidade. Você pode solicitar um XToken com no máximo um de tokens de T, U e D. Você deve fornecer pelo menos um dos D ou U. 
     * Solicitar um DToken de XASD, fornecendo os detalhes de autenticação do dispositivo
     * Solicite um UToken de XASU com alguma forma de autenticação de usuário. Provavelmente, a autenticação de usuário é fornecida na forma de um token do MSA (RPS).
     * Solicite um TToken de XAST. Os títulos que estão disponíveis dependem da plataforma atualmente em execução para que você deve fornecer um DToken para XAST também.
  
   * Crie uma solicitação XSTS.
 
     * Defina a parte confiável que você está solicitando um token para.
     * Preencha as propriedades de solicitação com os tokens de D, U e/ou T.
    * Executar a solicitação de XSTS e armazenar em cache o XToken resultante. O XToken retornado (no máximo) contém todas as informações de identidade do dispositivo, o usuário e o título de tokens de identidade e declarações adicionais (o status atual da assinatura, grupos de usuários, etc.).
   
<a id="ID4EEC"></a>

 
## <a name="30-tokens-multiuser-vs-single-user"></a>3.0 tokens de: Multiuser vs. Usuário único
 
Esse é o formato do token 3.0: `XBL3.0 x=&lt;hash>;&lt;token>`
 
Dependendo do &lt;hash >, o token é tratado de forma diferente:
 
   * Se &lt;hash > é igual a * (asterisco), em seguida, nenhum usuário específico está realizando a solicitação e todos os usuários no token estão presentes no principal desserializado. Essa é a forma de multiusuário true.
   * Se &lt;hash > é igual a - (hífen), em seguida, nenhum usuário está executando a solicitação. Todos os usuários a entidade de segurança desserializado são eliminados.
   * Se &lt;hash > não é igual a * ou -, em seguida, ele é um identificador que indica qual usuário no token está fazendo a solicitação. Somente o usuário indicado estará presente a entidade de segurança desserializado. Todos os outros usuários são eliminados. Esse é o token de usuário único 3.0.
   
<a id="ID4EYC"></a>

 
## <a name="does-eds-support-multi-users"></a>EDS dá suporte a vários usuários?
 * A resposta é nenhum. No caso em que foi descrito, o console será sempre enviar tokens de usuário único. Mesmo se houver vários usuários conectados ao, deve haver um indicado "chamador", onde todas as outras identidades são descartadas.
  
<a id="ID4E6C"></a>

 
## <a name="see-also"></a>Consulte também
 
<a id="ID4EBD"></a>

 
##### <a name="parent"></a>Parent  

[Referência adicional](atoc-xboxlivews-reference-additional.md)

  
<a id="ID4END"></a>

 
##### <a name="further-information"></a>Informações adicionais 

[URIs de Marketplace](../uri/marketplace/atoc-reference-marketplace.md)

   