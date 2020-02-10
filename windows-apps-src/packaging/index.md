---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empacotando apps
description: Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP).
ms.date: 07/22/2019
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: bb772007cd5c4391634f9df6ba0b6d2037a7b5de
ms.sourcegitcommit: 3e7a4f7605dfb4e87bac2d10b6d64f8b35229546
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/08/2020
ms.locfileid: "77089472"
---
# <a name="packaging-apps"></a>Empacotando apps

Esta seção contém artigos (ou links para eles) sobre o empacotamento de aplicativos UWP (Plataforma Universal do Windows) em pacotes de aplicativos MSIX e .appx para implantação e instalação. Alguns desses links acessam artigos relevantes na [documentação do MSIX](https://docs.microsoft.com/windows/msix/).

> [!NOTE]
> O formato original de empacotamento de aplicativos UWP no Windows 10 era .appx. Da versão 1809 do Windows 10 em diante, esse formato de empacotamento foi renomeado para .msix e estendido para dar suporte a todos os tipos de aplicativos da área de trabalho do Windows, incluindo aplicativos .NET e C++/Win32. O suporte para MSIX também está sendo estendido para as versões anteriores do Windows. Para obter mais informações, confira a [documentação do MSIX](https://docs.microsoft.com/windows/msix/).

| Tópico | Descrição |
|-------|-------------|
| [União de um aplicativo UWP com o Visual Studio](/windows/msix/package/packaging-uwp-apps) | Para distribuir ou vender seu aplicativo UWP (Plataforma Universal do Windows), será necessário criar um pacote do aplicativo para ele. |
| [Empacotamento manual de aplicativo](/windows/msix/package/manual-packaging-root) | Se você quiser criar e assinar um pacote do aplicativo sem ter usado o Visual Studio para desenvolver o aplicativo, precisará usar as ferramentas de empacotamento manual do aplicativo. |
| [Arquiteturas de pacote do aplicativo](/windows/msix/package/device-architecture) | Saiba mais sobre quais arquiteturas de processador você deve usar ao criar um pacote do aplicativo. |
| [Instalação de streaming de aplicativo UWP](/windows/msix/package/streaming-install) | A instalação de streaming de aplicativo permite que você especifique quais partes do aplicativo deseja que a Microsoft Store baixe primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano. |
| [Criação de pacotes opcionais e conjunto relacionado](/windows/msix/package/optional-packages) | Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o DLC (conteúdo para download), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original. |
| [Pacotes opcionais com código executável](/windows/msix/package/optional-packages-with-executable-code) | Saiba como usar o Visual Studio para criar um pacote opcional com código executável. |
| [Instalar aplicativos do Windows 10 com o Instalador de Aplicativo](/windows/msix/app-installer/app-installer-root) | O Instalador de Aplicativo permite que os aplicativos do Windows 10 sejam instalados clicando duas vezes no pacote do aplicativo. |
| [Instalar aplicativos usando a ferramenta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo UWP de um computador Windows 10 em qualquer dispositivo Windows 10 Mobile. É possível usar essa ferramenta para implantar um pacote do aplicativo quando o dispositivo Windows 10 Mobile está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse aplicativo. Este artigo descreve como instalar aplicativos UWP usando essa ferramenta. |
| [Configurar builds automáticos para seu aplicativo UWP](auto-build-package-uwp-apps.md) | Se você quiser empacotar seu aplicativo como parte de um processo de compilação automática, este tópico mostra como usar o Visual Studio Team Services (VSTS) para fazer isso. |
| [Declarações de funcionalidades do aplicativo](app-capability-declarations.md) | As funcionalidades precisam ser declaradas no [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/appx-package-manifest) do aplicativo para acessar determinadas APIs ou recursos (como imagens, músicas) ou dispositivos (como câmera, microfone). |
| [Baixar e instalar atualizações de pacote da Store](self-install-package-updates.md) | Seu aplicativo UWP pode verificar programaticamente se há atualizações de pacote e instalar as atualizações. Seu aplicativo também pode consultar os pacotes que foram marcados como obrigatórios no Partner Center e desabilitar a funcionalidade até a atualização obrigatória ser instalada.  |
