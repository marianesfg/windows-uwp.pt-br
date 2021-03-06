---
ms.assetid: c43d4af3-9a1a-4eae-a137-1267c293c1b5
description: Este artigo mostra como tirar proveito dos recursos especiais da interface do usuário da câmera que estão presentes apenas em dispositivos móveis.
title: Recursos da interface do usuário da câmera para dispositivos móveis
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a6e72aea74c4aed092cab450c05dc0982e838f09
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66358965"
---
# <a name="camera-ui-features-for-mobile-devices"></a>Recursos da interface do usuário da câmera para dispositivos móveis

Este artigo mostra como tirar proveito dos recursos especiais da interface do usuário da câmera que estão presentes apenas em dispositivos móveis. 

## <a name="add-the-mobile-extension-to-your-project"></a>Adicionar a extensão móvel ao seu projeto 

Para usar esses recursos, você deve adicionar uma referência ao SDK de extensão do Microsoft Mobile para a plataforma de aplicativos universais ao seu projeto.

**Para adicionar uma referência para o SDK de extensão móvel para o suporte do botão de câmera de hardware**

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência**.

2.  Expanda o nó **Universal do Windows** e selecione **Extensões**.

3.  Marque a caixa de seleção **Microsoft Mobile Extension SDK for Universal App Platform**.

## <a name="hide-the-status-bar"></a>Ocultar a barra de status

Os dispositivos móveis têm um controle [**StatusBar**](https://docs.microsoft.com/uwp/api/Windows.UI.ViewManagement.StatusBar) que fornece ao usuário informações de status sobre o dispositivo. Esse controle ocupa espaço na tela que pode interferir com a interface do usuário de captura de mídia. Você pode ocultar a barra de status chamando [**HideAsync**](https://docs.microsoft.com/uwp/api/windows.ui.viewmanagement.statusbar.hideasync), mas deve fazer essa chamada em um bloco condicional onde o método [**ApiInformation.IsTypePresent**](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation.istypepresent) é usado para determinar se a API está disponível. Esse método só retornará true em dispositivos móveis que aceitam a barra de status. Você deve ocultar a barra de status quando o aplicativo for iniciado ou quando você iniciar a visualização da câmera.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Quando seu aplicativo estiver sendo desligado ou quando o usuário sair da página de captura de mídia do aplicativo, você poderá tornar o controle visível novamente.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## <a name="use-the-hardware-camera-button"></a>Usar o botão de câmera do hardware

Alguns dispositivos móveis têm um botão de câmera físico dedicado que alguns usuários preferem em vez de um controle na tela. Para ser notificado quando o botão de câmera físico for pressionado, registre um manipulador para o evento [**HardwareButtons.CameraPressed**](https://docs.microsoft.com/uwp/api/windows.phone.ui.input.hardwarebuttons.camerapressed). Como essa API está disponível somente em dispositivos móveis, você deve usar novamente **IsTypePresent** para certificar-se de que a API é compatível com o dispositivo atual antes de tentar acessá-la.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

No manipulador do evento **CameraPressed**, você pode iniciar uma captura de foto.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Quando seu aplicativo estiver sendo desligado ou o usuário sair da página de captura de mídia do aplicativo, cancele o registro do manipulador de botão físico.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

## <a name="related-topics"></a>Tópicos relacionados

* [Câmera](camera.md)
* [Básica de fotos, vídeo e áudio capturar com MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)





