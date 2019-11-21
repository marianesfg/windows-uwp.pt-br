---
description: Use seu computador Mac atual para desenvolver aplicativos para Windows.
title: Configurando o seu Mac com o Windows 10
ms.assetid: 6D520610-5DE0-476E-A792-AA57E002D309
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 55165e0369c6bda64c19dc384c5c2addf224b8ba
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74259113"
---
# <a name="setting-up-your-mac-with-windows-10"></a>Configurando o seu Mac com o Windows 10


Use seu computador Mac atual para desenvolver aplicativos para Windows.

## <a name="run-windows-on-your-mac-and-use-visual-studio"></a>Execute o Windows em seu Mac e use o Visual Studio

Você está pronto para começar a desenvolver aplicativos Universais do Windows, mas não tem um computador disponível? Tudo bem. Você pode usar o seu Mac! Com soluções de terceiros populares, como Apple Boot Camp, Oracle VirtualBox, VMware Fusion e paralelos desktop, você pode instalar o Windows 10 e o Microsoft Visual Studio em seu computador da Apple.

**Observação**  você precisará de uma imagem inicializável do Windows 10 em disco ou unidade flash USB. Se você for um assinante do MSDN, poderá baixar a imagem da instalação do Centro de Desenvolvimento do MSDN Subscriber. Se você não for um assinante, o instalador poderá ser adquirido no [Microsoft Store](https://www.microsoft.com/store/apps). Você também pode baixá-lo [deste local](https://www.microsoft.com/software-download/windows10), o que é útil se você já estiver executando o Windows e deseja atualizar.

Depois que o Windows estiver em execução, você poderá instalar a versão mais recente do Visual Studio a partir dos [downloads para desenvolvedores do Windows 10](https://developer.microsoft.com/en-us/windows/downloads) e começar a escrever aplicativos!

**Observação**  se você planeja usar os emuladores de dispositivo do Visual Studio, você **deve** instalar uma versão de 64 bits (x64) do Windows 10 pro ou superior. Infelizmente, alguns Macs mais antigos não podem executar Windows de 64 bits. Verifique junto à Apple se seu hardware é compatível nesta[página de suporte da Apple](https://support.apple.com/kb/HT5634).

## <a name="apple-boot-camp"></a>Apple Boot Camp

O aplicativo Boot Camp Assistant é pré-instalado em todos os Mac recentes e iniciá-lo guiará você pelo processo de instalação do Windows 10. Tudo o que você precisa é de uma cópia do Windows (das fontes listadas acima) e pelo menos 30 Gb de espaço livre em disco. Depois de instalado, você pode optar por inicializar no Mac OSX ou no Windows 10. Para obter mais informações, consulte a [página de instruções de Boot Camp](https://support.apple.com/HT201468) da Apple.

## <a name="parallels-desktop"></a>Parallels Desktop

Usando o Parallels Desktop 11, você pode executar aplicativos do Windows lado a lado com os aplicativos Mac existentes, incluindo o Visual Studio e o Cortana. Está disponível uma versão pro que inclui recursos extras para desenvolvedores, incluindo depuração aprimorada e suporte para Docker e Jenkins. Para obter mais informações e uma versão de avaliação gratuita, consulte [Parallels Desktop](https://www.parallels.com/download/desktop/).

## <a name="vmware-fusion"></a>VMWare Fusion

O Fusion 8 da VMWare permitirá que você execute o Visual Studio diretamente em sua área de trabalho do Mac. Uma versão pro está disponível para oferecer aos desenvolvedores alguns recursos mais avançados como o suporte a vSphere. Para obter mais informações e uma versão de avaliação gratuita, consulte [VMware Fusion](http://www.vmware.com/products/fusion/).

## <a name="oracle-virtualbox"></a>Oracle VirtualBox

O VirtualBox é um aplicativo gratuito para máquinas virtuais em execução no computador, e ele dá suporte à execução do Windows no Mac. É uma opção sem ornamentos, mas o preço é atraente. Para obter mais informações, consulte [VirtualBox](https://www.virtualbox.org/wiki/Downloads).

