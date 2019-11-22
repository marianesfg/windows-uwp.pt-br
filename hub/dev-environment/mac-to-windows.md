---
title: Help moving from Mac (Unix) to Windows
description: A guide to help you transition from a Mac (Unix) to a Windows development environment, including shortcut key mapping and a brief overview of concepts that differ between Mac and Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
ms.technology: windows-nodejs
keywords: Mac to Windows, shortcut key mapping, move from Unix to Windows, transition from Mac to Windows, help moving from MacBook to Surface, how to use Windows for a Macintosh user, switching from Macintosh to Windows, help changing dev environments, Mac OS X to Windows, help moving from Mac to PC
ms.localizationpriority: medium
ms.date: 09/19/2019
ms.openlocfilehash: 563a8ad659cfff1396049aae78342642d1db3e72
ms.sourcegitcommit: 4cb3ee28baa8020ec925b0bdd896ab197a1ddadb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2019
ms.locfileid: "74309155"
---
# <a name="guide-for-changing-your-dev-environment-from-mac-to-windows"></a>Guide for changing your dev environment from Mac to Windows

The following tips and control equivalents should help you in your transition between a Mac and Windows (or WSL/Linux) development environment.

For app development, the nearest equivalent to Xcode would be [Visual Studio](https://visualstudio.microsoft.com). There is also a version of [Visual Studio for Mac](https://visualstudio.microsoft.com/vs/mac/), if you ever feel the need to go back. For cross-platform source code editing (and a huge number of plug-ins) [Visual Studio Code](https://code.visualstudio.com/?wt.mc_id=DX_841432) is the most popular choice.

## <a name="keyboard-shortcuts"></a>Atalhos de teclado

| **Operation** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Copiar | Command+C | Ctr+C |
| Recortar | Command+X | Ctr+X |
| Colar | Command+V | Ctr+V |
| Desfazer | Command+Z | Ctrl+Z |
| salve | Command+S | Ctrl+S |
| Abra o | Command+O | Ctrl+O |
| Lock computer | Command+Control+Q | WindowKey+L |
| Show desktop | Command+F3 | WindowKey+D |
| Minimize windows | COMMAND +M | Windows key+M |
| Pesquisar | Command+Space | tecla do Windows |
| Close active window | Command+W | Control+W |
| Switch current task | Command+Tab | Alt+Tab |
| Save screen (Screenshot) | Command+Shift+3 | Windows+Shift+S |
| Save window | Command+Shift+4 | Windows+Shift+S |
| View item information or properties | Command+I | Alt+Enter |
 | Select all items | Command+A | Ctrl+A |
| Select more than one item in a list (noncontiguous) | Command, then click each item | Control, then click each item |
| Type special characters | Option+ character key | Alt+ character key|

## <a name="trackpad-shortcuts"></a>Atalhos do trackpad

Note: Some of these shortcuts require a “Precision Trackpad”, such as the trackpad on Surface devices and some other third party laptops.

 **Operation** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Rolagem | Two finger vertical swipe | Two finger vertical swipe |
| Zoom | Two finger pinch in and out | Two finger pinch in and out |
| Swipe back and forward between views | Two finger sideways swipe | Two finger sideways swipe |
| Switch virtual workspaces | Four fingers sideways swipe | Four fingers sideways swipe |
| Display currently open apps | Four fingers upward swipe | Three fingers upward swipe |
| Switch between apps | N/D | Slow three finger sideways swipe |
| Go to desktop | Spread out four fingers | Three finger swipe downwards |
| Open Cortana / Action center | Two finger slide from right | Three finger tap |
| Open extra information | Three finger tap | N/D |
|Show launchpad / start an app | Pinch with four fingers | Tap with four fingers |

Note: Trackpad options are configurable on both platforms.

## <a name="terminal-and-shell"></a>Terminal e Shell

Windows provides several alternatives to the Mac's terminal emulator.

1. The Windows Command Line

The Windows command line will accept DOS commands, and is the most commonly used command line tool on Windows. To open it: Press **Windows+R** to open the **Run** box, then type **cmd** and then click **OK**. To open an administrator command line, type **cmd** and then press **Ctrl+Shift+Enter**. 

2. PowerShell

[PowerShell](https://docs.microsoft.com/powershell/scripting/overview?view=powershell-6) is a "PowerShell is a task-based command-line shell and scripting language built on .NET. PowerShell helps system administrators and power-users rapidly automate tasks that manage operating systems". In other words, it's a very powerful command line, and is especially loved by system admins.

Incidentally, PowerShell is [also available for Mac](https://docs.microsoft.com/powershell/scripting/install/installing-powershell-core-on-macos?view=powershell-6).

3. WSL (Subsistema do Windows para Linux)

WSL allows you to run a Linux shell within Windows. This means you can run *bash** or other shell, depending on choice and the specific Linux distro installed. Using WSL will provide the kind of environment most familiar to Mac users. For example, you will **ls** to list the files in a current directory, not **dir** as you would with the Windows command line. To learn about instaling and using WSL, see the [Windows Subsystem for Linux Installation Guide for Windows 10](https://docs.microsoft.com/en-us/windows/wsl/install-win10).

## <a name="apps-and-utilities"></a>Aplicativos e utilitários

 **App** | **Mac** | **Windows** |
|---------------|--------------------|---------------------|
| Settings and Preferences | System Preferences | Configurações |
| Gerenciador de tarefas | Activity Monitor | Gerenciador de Tarefas |
| Disk formatting | Disk Utility | Gerenciamento de disco |
| Text editing | TextEdit | Bloco de notas |
| Event viewing | Console | Visualizador de Eventos |
| Find files/apps | Command+Space | tecla do Windows |
