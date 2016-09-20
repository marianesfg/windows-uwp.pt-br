---
author: drewbatgit
ms.assetid: 
description: "Este artigo mostra como tirar proveito dos recursos especiais da interface do usuário da câmera que estão presentes apenas em dispositivos móveis."
title: "Recursos da interface do usuário da câmera para dispositivos móveis"
translationtype: Human Translation
ms.sourcegitcommit: 77d1709cd42253c229b01df21ae3416e57c1c2ab
ms.openlocfilehash: ec437d7111b1490f52bfc53b3ad2cd06f0c66ef3

---

#Recursos da interface do usuário da câmera para dispositivos móveis

Este artigo mostra como tirar proveito dos recursos especiais da interface do usuário da câmera que estão presentes apenas em dispositivos móveis. 

## Adicionar a extensão móvel ao seu projeto 

Para usar esses recursos, você deve adicionar uma referência ao SDK de extensão do Microsoft Mobile para a plataforma de aplicativos universais ao seu projeto.

**Para adicionar uma referência ao SDK de extensão móvel para suporte ao botão de câmera físico**

1.  No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência**.

2.  Expanda o nó **Universal do Windows** e selecione **Extensões**.

3.  Marque a caixa de seleção **Microsoft Mobile Extension SDK for Universal App Platform**.

## Ocultar a barra de status

Os dispositivos móveis têm um controle [**StatusBar**](https://msdn.microsoft.com/library/windows/apps/dn633864) que fornece ao usuário informações de status sobre o dispositivo. Esse controle ocupa espaço na tela que pode interferir com a interface do usuário de captura de mídia. Você pode ocultar a barra de status chamando [**HideAsync**](https://msdn.microsoft.com/library/windows/apps/dn610339), mas deve fazer essa chamada em um bloco condicional onde o método [**ApiInformation.IsTypePresent**](https://msdn.microsoft.com/library/windows/apps/dn949016) é usado para determinar se a API está disponível. Esse método só retornará true em dispositivos móveis que aceitam a barra de status. Você deve ocultar a barra de status quando o aplicativo for iniciado ou quando você iniciar a visualização da câmera.

[!code-cs[HideStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetHideStatusBar)]

Quando seu aplicativo estiver sendo desligado ou quando o usuário sair da página de captura de mídia do aplicativo, você poderá tornar o controle visível novamente.

[!code-cs[ShowStatusBar](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetShowStatusBar)]

## Usar o botão de câmera do hardware

Alguns dispositivos móveis têm um botão de câmera físico dedicado que alguns usuários preferem em vez de um controle na tela. Para ser notificado quando o botão de câmera físico for pressionado, registre um manipulador para o evento [**HardwareButtons.CameraPressed**](https://msdn.microsoft.com/library/windows/apps/dn653805). Como essa API está disponível somente em dispositivos móveis, você deve usar novamente **IsTypePresent** para certificar-se de que a API é compatível com o dispositivo atual antes de tentar acessá-la.

[!code-cs[PhoneUsing](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetPhoneUsing)]

[!code-cs[RegisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetRegisterCameraButtonHandler)]

No manipulador do evento **CameraPressed**, você pode iniciar uma captura de foto.

[!code-cs[CameraPressed](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetCameraPressed)]

Quando seu aplicativo estiver sendo desligado ou o usuário sair da página de captura de mídia do aplicativo, cancele o registro do manipulador de botão físico.

[!code-cs[UnregisterCameraButtonHandler](./code/BasicMediaCaptureWin10/cs/MainPage.xaml.cs#SnippetUnregisterCameraButtonHandler)]

> [!NOTE]
> Este artigo se destina a desenvolvedores do Windows 10 que escrevem aplicativos da Plataforma Universal do Windows (UWP). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).                                                                                   |

## Tópicos relacionados

* [Câmera](camera.md)
* [Captura básica de fotos, áudio e vídeo com o MediaCapture](basic-photo-video-and-audio-capture-with-MediaCapture.md)








<!--HONumber=Aug16_HO3-->


