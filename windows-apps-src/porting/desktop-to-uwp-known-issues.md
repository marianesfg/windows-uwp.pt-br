---
author: awkoren
Description: "Este artigo contém problemas conhecidos com a Ponte de Desktop para UWP."
Search.Product: eADQiWindows 10XVcnh
title: Problemas conhecidos com a Ponte de Desktop
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: d7f7cfef59c02c3a8930c1bbc2c87cc40675ca22
ms.lasthandoff: 02/08/2017

---
# <a name="known-issues-with-the-desktop-bridge"></a>Problemas conhecidos com a Ponte de Desktop

Este artigo contém problemas conhecidos com a Ponte de Desktop para UWP.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Tela azul com código de erro 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Depois da instalação ou da inicialização de determinados aplicativos da Windows Store, o computador poderá ser reiniciado inesperadamente com o erro: **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**.

Entre os aplicativos afetados conhecidos estão Kodi, JT2Go, Ear Trumpet, Teslagrad e outros.

Uma [atualização do Windows (versão 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) foi lançada em 27/10/16 incluindo correções importantes que resolvem esse problema. Se você enfrentar esse problema, atualize o computador. Se não conseguir atualizar o computador porque ele reinicia antes de você conseguir fazer logon, você deverá usar a restauração do sistema para restaurar o sistema até um ponto anterior ao momento em que instalou um dos aplicativos afetados. Para obter informações sobre como usar a restauração do sistema, consulte [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options). 

Se a atualização não corrigir o problema ou se você não souber como recuperar o computador, entre em contato com o [Suporte da Microsoft](https://support.microsoft.com/contactus/). 

Se for um desenvolvedor, você desejará impedir a instalação dos aplicativos da ponte da área de trabalho em versões do Windows que não incluam essa atualização. Com isso o aplicativo não estará disponível para os usuários que ainda não tenham instalado a atualização. Para limitar a disponibilidade do aplicativo para usuários que tenham instalado essa atualização, modifique o arquivo AppxManifest.xml da seguinte maneira:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Os detalhes a respeito do Windows Update podem ser encontrados em: 
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history
