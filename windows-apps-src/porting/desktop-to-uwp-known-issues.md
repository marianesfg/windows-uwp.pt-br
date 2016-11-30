---
author: awkoren
Description: "Este artigo contém problemas conhecidos com a ponte da área de trabalho para UWP."
Search.Product: eADQiWindows 10XVcnh
title: "Problemas conhecidos com a ponte da área de trabalho"
translationtype: Human Translation
ms.sourcegitcommit: 537c6a3d4559da4673b68c3ab5bdddb612760849
ms.openlocfilehash: d02921247bd77d59bbb09037a4ced8d3967c33b2

---
# Problemas conhecidos com a ponte da área de trabalho

Este artigo contém problemas conhecidos com a ponte da área de trabalho para UWP.

## Tela azul com código de erro 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Depois da instalação ou da inicialização de determinados aplicativos da Windows Store, o computador poderá ser reiniciado inesperadamente com o erro: **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**.

Entre os aplicativos afetados conhecidos estão Kodi, JT2Go, Ear Trumpet, Teslagrad e outros.

Uma [atualização do Windows (versão 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) foi lançada em 27/10/16 incluindo correções importantes que resolvem esse problema. Se você enfrentar esse problema, atualize o computador. Se não conseguir atualizar o computador porque ele reinicia antes de você conseguir fazer logon, você deverá usar a restauração do sistema para restaurar o sistema até um ponto anterior ao momento em que instalou um dos aplicativos afetados. Para obter informações sobre como usar a restauração do sistema, consulte [Opções de recuperação no Windows 10](https://support.microsoft.com/en-us/help/12415/windows-10-recovery-options). 

Se a atualização não corrigir o problema ou se você não souber como recuperar o computador, entre em contato com o [Suporte da Microsoft](https://support.microsoft.com/contactus/). 

Se for um desenvolvedor, você desejará impedir a instalação dos aplicativos da ponte da área de trabalho em versões do Windows que não incluam essa atualização. Com isso o aplicativo não estará disponível para os usuários que ainda não tenham instalado a atualização. Para limitar a disponibilidade do aplicativo para usuários que tenham instalado essa atualização, modifique o arquivo AppxManifest.xml da seguinte maneira:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Os detalhes a respeito do Windows Update podem ser encontrados em: 
* https://support.microsoft.com/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history


<!--HONumber=Nov16_HO1-->


