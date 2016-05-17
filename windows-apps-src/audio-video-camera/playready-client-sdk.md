---
author: eliotcowley
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: Este tópico descreve como adicionar conteúdos de mídia protegida do PlayReady ao seu aplicativo da Plataforma Universal do Windows (UWP).
title: DRM do PlayReady
---

# DRM do PlayReady

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este tópico descreve como adicionar conteúdos de mídia protegida do PlayReady ao seu aplicativo da Plataforma Universal do Windows (UWP).

O DRM do PlayReady permite que desenvolvedores criem aplicativos UWP que possam fornecer conteúdo PlayReady ao usuário enquanto impõe regras de acesso definidas pelo provedor de conteúdo. Esta seção descreve as alterações feitas ao DRM do Microsoft PlayReady para Windows 10 e como modificar o seu aplicativo UWP PlayReady para dar suporte às alterações feitas desde a versão anterior do Windows 8.1 até a versão do Windows 10.
 
| Tópico                                                                     | Descrição                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DRM de hardware](hardware-drm.md)                                           | Este tópico fornece uma visão geral de como adicionar hardware do PlayReady com base em gerenciamento de direitos digitais (DRM) ao seu aplicativo UWP.                                                                                                                                                                 |
| [Streaming adaptável com PlayReady](adaptive-streaming-with-playready.md) | Este artigo descreve como adicionar conteúdo de multimídia de streaming adaptável com a proteção de conteúdo da Microsoft PlayReady a um aplicativo da Plataforma Universal do Windows (UWP). Esse recurso atualmente oferece suporte para reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP). |

## Quais são as novidades no DRM do PlayReady

A lista a seguir descreve os novos recursos e alterações feitas ao DRM do PlayReady para Windows 10.

-   Inclusão do gerenciamento de direitos digitais (DRM) de hardware.

    Suporte de proteção de conteúdo baseado em hardware permite a reprodução segura de conteúdo em alta definição (HD) ultra-alta definição (UHD) em vários dispositivos. O material de chave (incluindo chaves privadas, chaves de conteúdo e qualquer outro material de chave usado para derivar ou desbloquear essas chaves) e amostras de vídeo compactadas e não compactadas descriptografadas são protegidos ao aproveitar a segurança do hardware. Quando o DRM de Hardware está sendo usado, nenhum ativador desconhecido (reproduzir para desconhecido / reproduzir para desconhecido com downres) tem significado, pois o pipeline HWDRM sempre conhece a saída que está sendo usada. Para obter mais informações, consulte [DRM de hardware](hardware-drm.md)

-   O PlayReady não é mais um componente de estrutura appX, mas um componente nativo do sistema operacional. O namespace foi alterado de **Microsoft.Media.PlayReadyClient** para [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454)
-   Os seguintes cabeçalhos definindo os códigos de erro do PlayReady agora fazem parte do Software Development Kit do Windows (SDK do Windows): Windows.Media.Protection.PlayReadyErrors.h e Windows.Media.Protection.PlayReadyResults.h.
-   Fornece aquisição proativa de licenças não persistentes.

    Versões anteriores ao DRM do PlayReady não davam suporte à aquisição proativa de licenças não persistentes. Esse recurso foi adicionado nesta versão. Isso pode reduzir o tempo para o primeiro quadro. Para obter mais informações, consulte [Adquirir uma licença não persistente de forma proativa antes da reprodução](#proactively_acquire_a_non_persistent_license_before_playback)

-   Fornece a aquisição de várias licenças em uma mensagem.

    Permite que o aplicativo cliente adquira várias licenças não persistentes em uma mensagem de aquisição de licença. Isso pode reduzir o tempo para o primeiro quadro pela aquisição de licenças para várias partes de conteúdo, enquanto o usuário ainda estiver navegando em sua biblioteca de conteúdo; isso impede um atraso na aquisição de licença quando o usuário seleciona o conteúdo a ser reproduzido. Além disso, ele permite a criptografia dos fluxos de áudio e vídeo para chaves separadas, por meio da habilitação de um cabeçalho de conteúdo que inclui vários identificadores de chave (KIDs); isso permite que uma aquisição de licença simples para adquirir todas as licenças para todos os fluxos dentro de um arquivo de conteúdo em vez de usar uma lógica personalizada e várias solicitações de aquisição de licença para obter o mesmo resultado.

-   Suporte de vencimento em tempo real ou licença de duração limitada (LDL) adicionada.

    Fornece a capacidade de definir a expiração em tempo real em licenças e a transição uniforme de uma licença que esteja vencendo para outra licença (válida) no meio de reprodução. Quando combinado com aquisição de várias licenças em uma mensagem, isso permite que um aplicativo adquira várias LDLs de forma assíncrona enquanto o usuário ainda estiver navegando na biblioteca de conteúdo e adquira uma licença de duração mais longa depois que o usuário tiver selecionado conteúdo para a reprodução. A reprodução será iniciada de forma mais rápida (porque uma licença já está disponível) e, desde que o aplicativo tenha adquirido uma licença de duração mais longa no momento em que a LDL expirar, continuará a ser reproduzida uniformemente até o fim do conteúdo sem interrupção.

-   Inclusão de cadeias de licença não persistentes.
-   Inclusão de suporte para restrições baseadas em tempo (incluindo expiração, expiração depois da primeira opção e expiração em tempo real) de licenças não persistentes.
-   Inclusão de suporte à política HDCP Tipo 1 (versão 2.2).

    Consulte [Coisas a serem consideradas](#things_to_consider) para obter mais informações.

-   O método Miracast agora está implícito como uma saída.
-   Inclusão de parada segura.

    A parada segura fornece os meios para um dispositivo PlayReady afirmar com confiança para um serviço de streaming de mídia que a reprodução de mídia foi interrompida para determinada parte do conteúdo. Esse recurso garante que seus serviços de streaming de mídia forneçam relatórios e imposição precisos de limitações de uso em dispositivos diferentes para uma determinada conta.

-   Inclusão da separação de licença de áudio e vídeo.

    Faixas separadas impedem que o vídeo seja decodificado como áudio; ativar a proteção de conteúdo mais robusta. Padrões emergentes precisam de chaves separadas para faixas de áudio e vídeo.

-   MaxResDecode adicionado.

    Esse recurso foi adicionado para limitar a reprodução de conteúdo a uma resolução máxima mesmo quando houver uma chave de maior capacidade (mas não uma licença). Ele suporta casos em que vários tamanhos de fluxo são codificados com uma única chave.

As novas interfaces, classes e enumerações a seguir foram adicionadas ao DRM do PlayReady:

-   [
              Interface **IPlayReadyLicenseAcquisitionServiceRequest**
            ](https://msdn.microsoft.com/library/windows/apps/dn986077)
-   [
              Interface **IPlayReadyLicenseSession**
            ](https://msdn.microsoft.com/library/windows/apps/dn986080)
-   [
              Interface **IPlayReadySecureStopServiceRequest**
            ](https://msdn.microsoft.com/library/windows/apps/dn986090)
-   [
              Classe **PlayReadyLicenseSession**
            ](https://msdn.microsoft.com/library/windows/apps/dn986309)
-   [
              Classe **PlayReadySecureStopIterable**
            ](https://msdn.microsoft.com/library/windows/apps/dn986371)
-   [
              Classe **PlayReadySecureStopIterator**
            ](https://msdn.microsoft.com/library/windows/apps/dn986375)
-   [
              Enumerador **PlayReadyHardwareDRMFeatures**
            ](https://msdn.microsoft.com/library/windows/apps/dn986265)

Uma nova amostra foi criada para demonstrar como usar os novos recursos DRM do PlayReady. A amostra pode ser baixada em [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)

## Coisas a serem consideradas

-   O DRM do PlayReady agora dá suporte a HDCP Tipo 1 (a partir da versão 2.2). O PlayReady carrega a política na licença para que o dispositivo aplique. Esse recurso pode ser habilitado na sua licença do servidor v3.0 SDK do PlayReady (o servidor controla essa política na licença usando o ativador play **GUID**). Para obter mais informações, consulte [PlayReady Compliance and Robustness Rules](http://www.microsoft.com/playready/licensing/compliance/)
-   Não há suporte para o Windows Media Video (também conhecido como VC-1) no DRM de hardware (consulte [Substituir DRM de hardware](hardware-drm.md#override-hardware-drm))
-   O DRM do PlayReady agora dá suporte ao padrão de compactação de vídeo de codificação de vídeo de alta eficiência (HEVC /H.265). Para dar suporte ao HEVC, seu aplicativo deve usar conteúdo de versão 2 do esquema de criptografia comum (CENC) que inclui deixar cabeçalhos de fração de conteúdo não criptografados. Para mais informações, consulte ISO/IEC 23001-7 Information technology -- MPEG systems technologies -- Part 7: Common encryption in ISO base media file format files (Versão de especificação ISO/IEC 23001-7:2015 ou superior necessária). A Microsoft também recomenda a utilização do CENC versão 2 para todo o conteúdo HWDRM. Além disso, alguns itens do DRM de hardware darão suporte ao HEVC, e outros não (consulte [Substituir DRM de hardware](hardware-drm.md#override-hardware-drm))
-   Para usufruir determinados recursos novos do PlayReady 3.0 (incluindo, mas não se limitando a, SL3000 para clientes baseados em hardware, a aquisição de várias licenças não persistentes em uma mensagem de aquisição de licença e restrições baseadas em tempo em licenças não persistentes), é necessário que o servidor PlayReady seja o Microsoft PlayReady Server Software Development Kit v3.0.2769 ou posterior.
-   Dependendo da política de proteção de saída especificada na licença do conteúdo, pode ocorrer falha na reprodução de mídia para os usuários finais se sua saída conectada não oferecer suporte a esses requisitos. A tabela a seguir lista o conjunto de erros comuns que ocorrem como resultado. Para obter mais informações, consulte [PlayReady Compliance and Robustness Rules](http://www.microsoft.com/playready/licensing/compliance/)

| Erro                                                   | Valor      | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | A Política de proteção de saída da licença exige que o monitor ative HDCP, mas HDCP não pôde ser ativada.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | A Política de proteção de saída da licença exige que o monitor ative HDCP Tipo 1, mas HDCP Tipo 1 não pôde ser ativada.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | Esse código de erro ocorre somente quando executado em DRM de hardware. A política de proteção de saída da licença requer que o monitor envolva a HDCP ou reduza a resolução efetiva do conteúdo, mas a HDCP não pôde ser envolvida e a resolução efetiva do conteúdo não pôde ser reduzida porque o DRM de hardware não dá suporte à redução de resolução do conteúdo. Em DRM de software, o conteúdo é reproduzido. Consulte [Considerações sobre como usar o DRM de hardware](hardware-drm.md#considerations-for-using-hardware-drm) |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | O driver de elemento gráfico não oferece suporte à proteção de saída. Por exemplo, o monitor está conectado por meio de VGA ou um driver de gráficos apropriado para saída digital não está instalado. No último caso, o driver típico instalado é o adaptador de vídeo básico da Microsoft. A instalação de um driver de elementos gráficos apropriado resolverá o problema.                                                                                                                                                  |

## Pré-requisitos

Antes de começar a criar seu aplicativo UWP protegido por PlayReady, o seguinte software deve ser instalado no sistema:

-   Windows 10.
-   Se você estiver compilando alguma das amostras para o DRM do PlayReady para aplicativos UWP, você deve usar Microsoft Visual Studio 2015 ou superior para compilar as amostras. Você ainda pode usar o Microsoft Visual Studio 2013 para compilar qualquer uma das amostras do DRM do PlayReady DRM para aplicativos da Loja do Windows 8.1.

Se você pretende reproduzir conteúdo MPEG-2/H.262 em seu aplicativo, também deverá baixar e instalar o [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876)

## Guia de migração de aplicativos PlayReady da Windows Store

Esta seção inclui informações sobre como migrar os seus aplicativos existentes PlayReady da Loja do Windows 8.x para o Windows 10.

O namespace para aplicativos UWP do PlayReady no Windows 10 foi alterado de **Microsoft.Media.PlayReadyClient** para [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454). Isso significa que você precisará pesquisar e substituir o namespace antigo pelo novo em seu código. Você ainda fará referência a um arquivo winmd. Ele é parte do windows.media.winmd nos sistema operacional do Windows 10. Ele está em windows.winmd como parte do SDK do Windows do TH. Para UWP, ele está referenciado em windows.foundation.univeralappcontract.winmd.

Para reproduzir conteúdo HD (alta definição) (1080p) e UHD (ultra-alta definição) protegido por PlayReady, será necessário implementar o DRM de hardware do PlayReady. Para obter mais informações sobre como implementar o DRM de hardware do PlayReady, consulte [DRM de hardware](hardware-drm.md)

Não há suporte para parte do conteúdo no DRM de hardware. Para informações sobre como desabilitar o DRM de hardware e habilitar o DRM de software, consulte [Substituir DRM de hardware](hardware-drm.md#override-hardware-drm)

Sobre o gerenciador de proteção de mídia, verifique se o seu código tem as seguintes configurações caso ainda não as tenha:

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## Adquirir uma licença não persistente de forma proativa antes da reprodução

Esta seção descreve como adquirir licenças não persistentes de forma proativa antes do início da reprodução.

Em versões anteriores do PlayReady DRM, as licenças não persistentes eram adquiridas somente de forma reativa durante a reprodução. Nesta versão, você pode adquirir licenças não persistentes de forma proativa antes do início da reprodução.

1.  Crie de forma proativa uma sessão de reprodução em que a licença não persistente possa ser armazenada. Por exemplo:

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Vincule a sessão de reprodução à classe de aquisição de licença. Por exemplo:

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Criar uma solicitação de serviço de licença. Por exemplo:

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Executar a aquisição de licença usando a solicitação de serviço criada na etapa 3. A licença será armazenada na sessão de reprodução.
5.  Vincule a sessão de reprodução à origem de mídia para reprodução. Por exemplo:

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## Adicionar parada segura

Esta seção descreve como adicionar uma parada segura ao seu aplicativo UWP.

A parada segura fornece os meios para um dispositivo PlayReady afirmar com confiança para um serviço de streaming de mídia que a reprodução de mídia foi interrompida para determinada parte do conteúdo. Esse recurso garante que seus serviços de streaming de mídia forneçam relatórios e imposição precisos de limitações de uso em dispositivos diferentes para uma determinada conta.

Há dois cenários principais para enviar um desafio de parada segura:

-   Quando a apresentação de mídia é interrompida porque foi alcançado o fim do conteúdo ou quando o usuário interrompeu a apresentação de mídia em algum ponto intermediário.
-   Quando a sessão anterior é encerrada inesperadamente (por exemplo, devido a uma falha no sistema ou no aplicativo). O aplicativo precisará consultar, na inicialização ou no desligamento, qualquer sessão de parada segura pendente e enviar desafio(s) separadamente de qualquer outra reprodução de mídia.

Para obter uma implementação de exemplo de parada segura, consulte o arquivo securestop.cs no exemplo de PlayReady localizado em [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670)

 

 






<!--HONumber=May16_HO2-->


