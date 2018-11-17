---
author: drewbatgit
ms.assetid: 79C284CA-C53A-4C24-807E-6D4CE1A29BFA
description: Esta seção descreve como modificar seu aplicativo de web PlayReady para dar suporte a alterações feitas desde a versão anterior do Windows 8.1 para a versão do Windows 10.
title: Extensão de Mídia Criptografada do PlayReady
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b73464ea10aa835b82df17605e983ebdfb9cd890
ms.sourcegitcommit: 3257416aebb5a7b1515e107866806f8bd57845a8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/17/2018
ms.locfileid: "7149057"
---
# <a name="playready-encrypted-media-extension"></a>Extensão de Mídia Criptografada do PlayReady



Esta seção descreve como modificar seu aplicativo de web PlayReady para dar suporte a alterações feitas desde a versão anterior do Windows 8.1 para a versão do Windows 10.

O uso de elementos de mídia PlayReady no Internet Explorer permite que desenvolvedores criem um aplicativo Web que possa fornecer conteúdo PlayReady ao usuário enquanto impõe regras de acesso definidas pelo provedor de conteúdo. Esta seção descreve como adicionar elementos de mídia PlayReady aos seus aplicativos Web existentes usando somente HTML5 e JavaScript.

## <a name="whats-new-in-playready-encrypted-media-extension"></a>Novidades na extensão de mídia criptografada do PlayReady

Esta seção fornece uma lista das alterações feitas à extensão de mídia criptografada do PlayReady (EME) para habilitar a proteção de conteúdo PlayReady no Windows 10.

A lista a seguir descreve os novos recursos e as alterações feitas à extensão de mídia criptografada do PlayReady para Windows 10:

-   Inclusão do gerenciamento de direitos digitais (DRM) de hardware

    Suporte de proteção de conteúdo baseado em hardware permite a reprodução segura de conteúdo em alta definição (HD) e ultra-alta definição (UHD) em vários dispositivos. O material de chave (incluindo chaves privadas, chaves de conteúdo e qualquer outro material de chave usado para derivar ou desbloquear essas chaves) e amostras de vídeo compactadas e não compactadas descriptografadas são protegidos ao aproveitar a segurança do hardware.

-   Fornece aquisição proativa de licenças não persistentes.
-   Fornece a aquisição de várias licenças em uma mensagem.

    Você pode usar um objeto PlayReady com vários identificadores-chave (KeyIDs) como no Windows 8.1, ou usar [dados de modelo de descriptografia de conteúdo (CDMData)](https://go.microsoft.com/fwlink/p/?LinkID=626819) com várias KeyIDs.

    > [!NOTE]
    > No Windows 10, vários identificadores-chave são suportados em &lt;KeyID&gt; em CDMData.

-   Suporte de vencimento em tempo real ou licença de duração limitada (LDL) adicionada.

    Fornece a capacidade de definir vencimento em tempo real das licenças.

-   Inclusão de suporte à política HDCP Tipo 1 (versão 2.2).
-   O método Miracast agora está implícito como uma saída.
-   Inclusão de parada segura.

    A parada segura fornece os meios para um dispositivo PlayReady afirmar com confiança para um serviço de streaming de mídia que a reprodução de mídia foi interrompida para determinada parte do conteúdo.

-   Inclusão da separação de licença de áudio e vídeo.

    Faixas separadas impedem que o vídeo seja decodificado como áudio; ativar a proteção de conteúdo mais robusta. Padrões emergentes precisam de chaves separadas para faixas de áudio e vídeo.

-   MaxResDecode adicionado.

    Esse recurso foi adicionado para limitar a reprodução de conteúdo a uma resolução máxima mesmo quando houver uma chave de maior capacidade (mas não uma licença). Ele suporta casos em que vários tamanhos de fluxo são codificados com uma única chave.

## <a name="encrypted-media-extension-support-in-playready"></a>Suporte à extensão de mídia criptografada no PlayReady

Esta seção descreve a versão da extensão de mídia criptografada do W3C com suporte pelo PlayReady.

O PlayReady para aplicativos Web está atualmente vinculado à [versão preliminar da extensão de mídia criptografada (EME) do W3C de 10 de maio de 2013](http://www.w3.org/TR/2013/WD-encrypted-media-20130510/). Esse suporte será alterado para a especificação EME atualizada em versões futuras do Windows.

## <a name="use-hardware-drm"></a>Usar DRM de hardware

Esta seção descreve como seu aplicativo Web pode usar o DRM de hardware do PlayReady e como desabilitar o DRM de hardware se o conteúdo protegido não oferecer suporte a ele.

Para usar hardware DRM do PlayReady, o seu aplicativo Web JavaScript deve usar o método EME **isTypeSupported** com um identificador de sistema chave de `com.microsoft.playready.hardware` para consultar por suporte de hardware DRM do PlayReady do navegador.

Ocasionalmente, não há suporte para parte do conteúdo no DRM de hardware. Não há suporte para conteúdo Cocktail no DRM de hardware. Se você deseja reproduzir conteúdo Cocktail, recuse o DRM de hardware. Alguns DRMs de hardware oferecerão suporte a HEVC, enquanto outros não. Se você deseja reproduzir conteúdo HEVC e o DRM de hardware não oferecer suporte a ele, recuse também.

> [!NOTE]
> Para determinar se há suporte para conteúdo HEVC, após instanciar `com.microsoft.playready`, use o método [**PlayReadyStatics.CheckSupportedHardware**](https://msdn.microsoft.com/library/windows/apps/dn986441).

## <a name="add-secure-stop-to-your-web-app"></a>Adicionar parada segura ao seu aplicativo Web

Esta seção descreve como adicionar parada segura ao seu aplicativo Web.

A parada segura fornece os meios para um dispositivo PlayReady afirmar com confiança para um serviço de streaming de mídia que a reprodução de mídia foi interrompida para determinada parte do conteúdo. Esse recurso garante que seus serviços de streaming de mídia forneçam relatórios e imposição precisos de limitações de uso em dispositivos diferentes para uma determinada conta.

Há dois cenários principais para enviar um desafio de parada segura:

-   Quando a apresentação de mídia é interrompida porque foi alcançado o fim do conteúdo ou quando o usuário interrompeu a apresentação de mídia em algum ponto intermediário.
-   Quando a sessão anterior é encerrada inesperadamente (por exemplo, devido a uma falha no sistema ou no aplicativo). O aplicativo precisará consultar, na inicialização ou no desligamento, qualquer sessão de parada segura pendente e enviar desafio(s) separadamente de qualquer outra reprodução de mídia.

Os procedimentos a seguir descrevem como configurar uma parada segura para vários cenários.

Para configurar parada segura para um fim normal de apresentação:

1.  Registre o evento **onEnded** antes de a reprodução iniciar.
2.  O manipulador de evento **onEnded** precisa chamar `removeAttribute(“src”)` do objeto de elemento de vídeo/áudio para definir a fonte para **NULO**, o que disparará o Media Foundation para destruir a topologia e o(s) descriptografador(es) e definir o estado de parada.
3.  Você pode iniciar a sessão CDM da parada segura dentro do manipulador para enviar o desafio de parada segura ao servidor e notificar que a reprodução foi interrompida neste momento, mas isso também pode ser feito posteriormente.

Para configurar a parada segura se o usuário sair da página ou fechar a guia ou o navegador:

-   Nenhuma ação de aplicativo é necessária para registrar o estado de parada; ele será registrado para você.

Para configurar uma parada segura para controles de página personalizados ou ações do usuário (como botões de navegação personalizados ou início de uma nova apresentação antes da conclusão da apresentação atual):

-   Quando uma ação personalizada do usuário ocorre, o aplicativo precisa definir a fonte para **NULO**, o que disparará o Media Foundation para destruir a topologia e o(s) descriptografador(es) e definir o estado de parada.

O exemplo a seguir demonstra como usar a parada segura em seu aplicativo Web:

```JavaScript
// JavaScript source code

var g_prkey = null;
var g_keySession = null;
var g_fUseSpecificSecureStopSessionID = false;
var g_encodedMeteringCert = 'Base64 encoded of your metering cert (aka publisher cert)';

// Note: g_encodedLASessionId is the CDM session ID of the proactive or reactive license acquisition 
//       that we want to initiate the secure stop process.
var g_encodedLASessionId = null;

function main()
{
    ...

    g_prkey = new MSMediaKeys("com.microsoft.playready");

    ...

    // add 'onended' event handler to the video element
    // Assume 'myvideo' is the ID of the video element
    var videoElement = document.getElementById("myvideo");
    videoElement.onended = function (e) { 

        //
        // Calling removeAttribute("src") will set the source to null
        // which will trigger the MF to tear down the topology, destroy the
        // decryptor(s) and set the stop state.  This is required in order
        // to set the stop state.
        //
        videoElement.removeAttribute("src");
        videoElement.load();

        onEndOfStream();
    };
}

function onEndOfStream()
{
    ...

    createSecureStopCDMSession();

    ...    
}

function createSecureStopCDMSession()
{
    try{    
        var targetMediaCodec = "video/mp4";
        var customData = "my custom data";

        var encodedSessionId = g_encodedLASessionId;
        if( !g_fUseSpecificSecureStopSessionID )
        {
            // Use "*" (wildcard) as the session ID to include all secure stop sessions
            // TODO: base64 encode "*" and place encoded result to encodedSessionId
        }

        var int8ArrayCDMdata = formatSecureStopCDMData( encodedSessionId, customData,  g_encodedMeteringCert );
        var emptyArrayofInitData = new Uint8Array();

        g_keySession = g_prkey.createSession(targetMediaCodec, emptyArrayofInitData, int8ArrayCDMdata);

        addPlayreadyKeyEventHandler();

    } catch( e )
    {
        // TODO: Handle exception
    }
}

function addPlayreadyKeyEventHandler()
{
    // add 'keymessage' eventhandler   
    g_keySession.addEventListener('mskeymessage', function (event) {

        // TODO: Get the keyMessage from event.message.buffer which contains the secure stop challenge
        //       The keyMessage format for the secure stop is similar to LA as below:
        //
        //            <PlayReadyKeyMessage type="SecureStop" >
        //              <SecureStop version="1.0" >
        //                <Challenge encoding="base64encoded">
        //                    secure stop challenge
        //                </Challenge>
        //                <HttpHeaders>
        //                    <HttpHeader>
        //                      <name>Content-Type</name>
        //                         <value>"content type data"</value>
        //                    </HttpHeader>
        //                    <HttpHeader>
        //                         <name>SOAPAction</name>
        //                         <value>soap action</value>
        //                     </HttpHeader>
        //                    ....
        //                </HttpHeaders>
        //              </SecureStop>
        //            </PlayReadyKeyMessage>
                
        // TODO: send the secure stop challenge to a server that handles the secure stop challenge

        // TODO: Recevie and response and call event.target.Update() to proecess the response
    });
    
    // add 'keyerror' eventhandler
    g_keySession.addEventListener('mskeyerror', function (event) {
        var session = event.target;
        
        ...

        session.close();
    });
    
    // add 'keyadded' eventhandler
    g_keySession.addEventListener('mskeyadded', function (event) {
        
        var session = event.target;

        ...

        session.close();             
    });
}

/**
* desc@ formatSecureStopCDMData
*   generate playready CDMData
*   CDMData is in xml format:
*   <PlayReadyCDMData type="SecureStop">
*     <SecureStop version="1.0">
*       <SessionID>B64 encoded session ID</SessionID>
*       <CustomData>B64 encoded custom data</CustomData>
*       <ServerCert>B64 encoded server cert</ServerCert>
*     </SecureCert>
* </PlayReadyCDMData>        
*/
function formatSecureStopCDMData(encodedSessionId, customData, encodedPublisherCert) 
{
    var encodedCustomData = null;

    // TODO: base64 encode the custom data and place the encoded result to encodedCustomData

    var CDMDataStr = "<PlayReadyCDMData type=\"SecureStop\">" +
                     "<SecureStop version=\"1.0\" >" +
                     "<SessionID>" + encodedSessionId + "</SessionID>" +
                     "<CustomData>" + encodedCustomData + "</CustomData>" +
                     "<ServerCert>" + encodedPublisherCert + "</ServerCert>" +
                     "</SecureStop></PlayReadyCDMData>";
    
    var int8ArrayCDMdata = null

    // TODO: Convert CDMDataStr to Uint8 byte array and palce the converted result to int8ArrayCDMdata

    return int8ArrayCDMdata;
}
```

> [!NOTE]
> A `<SessionID>B64 encoded session ID</SessionID>` dos dados da parada segura no exemplo acima pode ser um asterisco (\*), que é um curinga para todas as sessões de parada segura registradas. Ou seja, a marcação **SessionID** pode ser uma sessão específica ou um caractere curinga (\*) para selecionar todas as sessões de parada segura.

## <a name="programming-considerations-for-encrypted-media-extension"></a>Considerações sobre a programação de Extensão de mídia criptografada

Esta seção lista as considerações de programação que você deve levar em consideração ao criar seu aplicativo web habilitado para PlayReady para Windows 10.

Os objetos **MSMediaKeys** e **MSMediaKeySession** criados pelo seu aplicativo devem ser mantidos ativos até o seu aplicativo encerrar. Uma maneira de garantir que esses objetos fiquem ativos é atribuí-los como variáveis globais (as variáveis ficariam fora do escopo e sujeitas à coleta de lixo se declaradas como uma variável local dentro de uma função). Por exemplo, a amostra a seguir atribui as variáveis *g\_msMediaKeys* e *g\_mediaKeySession* como globais, que são, então, atribuídas aos objetos **MSMediaKeys** e **MSMediaKeySession** na função.

``` syntax
var g_msMediaKeys;
var g_mediaKeySession;

function foo() {
  ...
  g_msMediaKeys = new MSMediaKeys("com.microsoft.playready");
  ...
  g_mediaKeySession = g_msMediaKeys.createSession("video/mp4", intiData, null);
  g_mediaKeySession.addEventListener(this.KEYMESSAGE_EVENT, function (e) 
  {
    ...
    downloadPlayReadyKey(url, keyMessage, function (data) 
    {
      g_mediaKeySession.update(data);
    });
  });
  g_mediaKeySession.addEventListener(this.KEYADDED_EVENT, function () 
  {
    ...
    g_mediaKeySession.close();
    g_mediaKeySession = null;
  });
}
```

Para obter mais informações, consulte os [aplicativos de exemplo](https://code.msdn.microsoft.com/windowsapps/PlayReady-samples-for-124a3738).

## <a name="see-also"></a>Consulte também
- [DRM do PlayReady](playready-client-sdk.md)




