---
ms.assetid: BF877F23-1238-4586-9C16-246F3F25AE35
description: Este artigo descreve como adicionar streaming adaptável de conteúdo multimídia com proteção de conteúdo do Microsoft PlayReady a um aplicativo UWP (Plataforma Universal do Windows).
title: Streaming adaptável com PlayReady
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 36cd006b4608d82999281ebd407fd32e168ae38b
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8933256"
---
# <a name="adaptive-streaming-with-playready"></a>Streaming adaptável com PlayReady


Este artigo descreve como adicionar streaming adaptável de conteúdo multimídia com proteção de conteúdo do Microsoft PlayReady a um aplicativo UWP (Plataforma Universal do Windows). 

Esse recurso atualmente oferece suporte à reprodução de conteúdo Dynamic Streaming over HTTP (DASH).

Não há suporte para HLS (HTTP Live Streaming da Apple) com PlayReady.

No momento, também não há suporte nativo a Smooth Streaming; no entanto, o PlayReady é extensível e usando um código adicional ou bibliotecas, o Smooth Streaming protegido com PlayReady pode ter suporte, aproveitando o DRM (gerenciamento de direitos digitais) do software ou inclusive do hardware.

Este artigo lida apenas com os aspectos de streaming adaptável específicos para o PlayReady. Para obter informações sobre a implementação de streaming adaptável em geral, consulte [Streaming adaptável](adaptive-streaming.md).

Este artigo usa código do [Exemplo de streaming adaptável](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AdaptiveStreaming) no repositório **Windows-universal-samples** da Microsoft no GitHub. O Cenário 4 lida com o uso de streaming adaptável com PlayReady. Você pode baixar o repositório em um arquivo ZIP, navegando até o nível raiz do repositório e selecionando o botão **Baixar o ZIP**.

Você precisará das seguintes instruções **using**:

```csharp
using LicenseRequest;
using System;
using System.Net.Http;
using System.Net.Http.Headers;
using System.Runtime.InteropServices;
using System.Threading.Tasks;
using Windows.Foundation.Collections;
using Windows.Media.Protection;
using Windows.Media.Protection.PlayReady;
using Windows.Media.Streaming.Adaptive;
using Windows.UI.Xaml.Controls;
```

O namespace **LicenseRequest** é de **CommonLicenseRequest.cs**, um arquivo do PlayReady fornecido pela Microsoft para licenciados.

Você precisará declarar algumas variáveis globais:

```csharp
private AdaptiveMediaSource ams = null;
private MediaProtectionManager protectionManager = null;
private string playReadyLicenseUrl = "";
private string playReadyChallengeCustomData = "";
```

Também convém declarar a constante a seguir:

```csharp
private const uint MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED = 0x8004B895;
```

## <a name="setting-up-the-mediaprotectionmanager"></a>Configurando o MediaProtectionManager

Para adicionar proteção de conteúdo PlayReady ao seu aplicativo UWP, você precisará configurar um objeto [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040). Você fará isso ao inicializar seu objeto [**AdaptiveMediaSource**](https://msdn.microsoft.com/library/windows/apps/dn946912).

O código a seguir configura um [MediaProtectionManager](https://msdn.microsoft.com/library/windows/apps/br207040):

```csharp
private void SetUpProtectionManager(ref MediaElement mediaElement)
{
    protectionManager = new MediaProtectionManager();

    protectionManager.ComponentLoadFailed += 
        new ComponentLoadFailedEventHandler(ProtectionManager_ComponentLoadFailed);

    protectionManager.ServiceRequested += 
        new ServiceRequestedEventHandler(ProtectionManager_ServiceRequested);

    PropertySet cpSystems = new PropertySet();

    cpSystems.Add(
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}", 
        "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput");

    protectionManager.Properties.Add("Windows.Media.Protection.MediaProtectionSystemIdMapping", cpSystems);

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionSystemId", 
        "{F4637010-03C3-42CD-B932-B48ADF3A6A54}");

    protectionManager.Properties.Add(
        "Windows.Media.Protection.MediaProtectionContainerGuid", 
        "{9A04F079-9840-4286-AB92-E65BE0885F95}");

    mediaElement.ProtectionManager = protectionManager;
}
```

Este código pode simplesmente ser copiado para o seu aplicativo, já que ele é obrigatório para configurar a proteção de conteúdo.

O evento [ComponentLoadFailed](https://msdn.microsoft.com/library/windows/apps/br207041) é acionado quando a carga de dados binários falha. Precisamos adicionar um manipulador de eventos para lidar com isso, indicando que a carga não foi concluída:

```csharp
private void ProtectionManager_ComponentLoadFailed(
    MediaProtectionManager sender, 
    ComponentLoadFailedEventArgs e)
{
    e.Completion.Complete(false);
}
```

Da mesma forma, precisamos adicionar um manipulador de eventos para o evento [ServiceRequested](https://msdn.microsoft.com/library/windows/apps/br207045), que é acionado quando um serviço é solicitado. Este código verifica qual é o tipo de solicitação e responde adequadamente:

```csharp
private async void ProtectionManager_ServiceRequested(
    MediaProtectionManager sender, 
    ServiceRequestedEventArgs e)
{
    if (e.Request is PlayReadyIndividualizationServiceRequest)
    {
        PlayReadyIndividualizationServiceRequest IndivRequest = 
            e.Request as PlayReadyIndividualizationServiceRequest;

        bool bResultIndiv = await ReactiveIndivRequest(IndivRequest, e.Completion);
    }
    else if (e.Request is PlayReadyLicenseAcquisitionServiceRequest)
    {
        PlayReadyLicenseAcquisitionServiceRequest licenseRequest = 
            e.Request as PlayReadyLicenseAcquisitionServiceRequest;

        LicenseAcquisitionRequest(
            licenseRequest, 
            e.Completion, 
            playReadyLicenseUrl, 
            playReadyChallengeCustomData);
    }
}
```

## <a name="individualization-service-requests"></a>Solicitações de serviço de individualização

O código a seguir cria reativamente uma solicitação de serviço de individualização do PlayReady. Passamos a solicitação como um parâmetro para a função. Colocamos a chamada em um bloco try/catch e se não houver exceções, dizemos que a solicitação foi concluída com êxito:

```csharp
async Task<bool> ReactiveIndivRequest(
    PlayReadyIndividualizationServiceRequest IndivRequest, 
    MediaProtectionServiceCompletion CompletionNotifier)
{
    bool bResult = false;
    Exception exception = null;

    try
    {
        await IndivRequest.BeginServiceRequest();
    }
    catch (Exception ex)
    {
        exception = ex;
    }
    finally
    {
        if (exception == null)
        {
            bResult = true;
        }
        else
        {
            COMException comException = exception as COMException;
            if (comException != null && comException.HResult == MSPR_E_CONTENT_ENABLING_ACTION_REQUIRED)
            {
                IndivRequest.NextServiceRequest();
            }
        }
    }

    if (CompletionNotifier != null) CompletionNotifier.Complete(bResult);
    return bResult;
}
```

Como alternativa, podemos fazer proativamente uma solicitação de serviço de individualização, neste caso, chamamos a função a seguir em vez de chamar o código `ReactiveIndivRequest` de `ProtectionManager_ServiceRequested`:

```csharp
async void ProActiveIndivRequest()
{
    PlayReadyIndividualizationServiceRequest indivRequest = new PlayReadyIndividualizationServiceRequest();
    bool bResultIndiv = await ReactiveIndivRequest(indivRequest, null);
}
```

## <a name="license-acquisition-service-requests"></a>Solicitações de serviço de aquisição de licença

Se em vez disso a solicitação era [PlayReadyLicenseAcquisitionServiceRequest](https://msdn.microsoft.com/library/windows/apps/dn986285), chamamos a função a seguir para solicitar e obter a licença do PlayReady. Informamos ao objeto **MediaProtectionServiceCompletion** que passamos se a solicitação foi bem-sucedida ou não, e concluímos a solicitação:

```csharp
async void LicenseAcquisitionRequest(
    PlayReadyLicenseAcquisitionServiceRequest licenseRequest, 
    MediaProtectionServiceCompletion CompletionNotifier, 
    string Url, 
    string ChallengeCustomData)
{
    bool bResult = false;
    string ExceptionMessage = string.Empty;

    try
    {
        if (!string.IsNullOrEmpty(Url))
        {
            if (!string.IsNullOrEmpty(ChallengeCustomData))
            {
                System.Text.UTF8Encoding encoding = new System.Text.UTF8Encoding();
                byte[] b = encoding.GetBytes(ChallengeCustomData);
                licenseRequest.ChallengeCustomData = Convert.ToBase64String(b, 0, b.Length);
            }

            PlayReadySoapMessage soapMessage = licenseRequest.GenerateManualEnablingChallenge();

            byte[] messageBytes = soapMessage.GetMessageBody();
            HttpContent httpContent = new ByteArrayContent(messageBytes);

            IPropertySet propertySetHeaders = soapMessage.MessageHeaders;

            foreach (string strHeaderName in propertySetHeaders.Keys)
            {
                string strHeaderValue = propertySetHeaders[strHeaderName].ToString();

                if (strHeaderName.Equals("Content-Type", StringComparison.OrdinalIgnoreCase))
                {
                    httpContent.Headers.ContentType = MediaTypeHeaderValue.Parse(strHeaderValue);
                }
                else
                {
                    httpContent.Headers.Add(strHeaderName.ToString(), strHeaderValue);
                }
            }

            CommonLicenseRequest licenseAcquision = new CommonLicenseRequest();

            HttpContent responseHttpContent = 
                await licenseAcquision.AcquireLicense(new Uri(Url), httpContent);

            if (responseHttpContent != null)
            {
                Exception exResult = licenseRequest.ProcessManualEnablingResponse(
                                         await responseHttpContent.ReadAsByteArrayAsync());

                if (exResult != null)
                {
                    throw exResult;
                }
                bResult = true;
            }
            else
            {
                ExceptionMessage = licenseAcquision.GetLastErrorMessage();
            }
        }
        else
        {
            await licenseRequest.BeginServiceRequest();
            bResult = true;
        }
    }
    catch (Exception e)
    {
        ExceptionMessage = e.Message;
    }

    CompletionNotifier.Complete(bResult);
}
```

## <a name="initializing-the-adaptivemediasource"></a>Inicializando o AdaptiveMediaSource

Por fim, você precisará de uma função para inicializar o [AdaptiveMediaSource](https://msdn.microsoft.com/library/windows/apps/dn946912), criada de um determinado [Uri](https://msdn.microsoft.com/library/windows/apps/xaml/system.uri.aspx) e [MediaElement](https://msdn.microsoft.com/library/windows/apps/br242926). O **Uri** deve ser o link para o arquivo de mídia (HLS ou DASH); o **MediaElement** deve ser definido em seu XAML.

```csharp
async private void InitializeAdaptiveMediaSource(System.Uri uri, MediaElement m)
{
    AdaptiveMediaSourceCreationResult result = await AdaptiveMediaSource.CreateFromUriAsync(uri);
    if (result.Status == AdaptiveMediaSourceCreationStatus.Success)
    {
        ams = result.MediaSource;
        SetUpProtectionManager(ref m);
        m.SetMediaStreamSource(ams);
    }
    else
    {
        // Error handling
    }
}
```

Você pode chamar essa função em qualquer evento que manipular o início do streaming adaptável; por exemplo, em um evento de clique de um botão.

## <a name="see-also"></a>Consulte também
- [DRM do PlayReady](playready-client-sdk.md)




