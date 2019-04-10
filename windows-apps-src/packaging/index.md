---
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empacotando apps
description: Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP).
ms.date: 03/18/2019
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: 8eb0fa1eef5b859de561407a91215d5b75624030
ms.sourcegitcommit: e63fbd7a63a7e8c03c52f4219f34513f4b2bb411
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/19/2019
ms.locfileid: "58173002"
---
# <a name="packaging-apps"></a>Empacotando apps


## <a name="purpose"></a>Finalidade

Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP).

| Tópico | Descrição |
|-------|-------------|
| [União de um aplicativo UWP com o Visual Studio](packaging-uwp-apps.md) | Para distribuir ou vender seu aplicativo UWP (Plataforma Universal do Windows), será necessário criar um pacote do aplicativo para ele. |
| [Empacotamento manual de aplicativo](manual-packaging-root.md) | Se você quiser criar e assinar um pacote do aplicativo sem ter usado o Visual Studio para desenvolver o aplicativo, precisará usar as ferramentas de empacotamento manual do aplicativo. |
| [Arquiteturas de pacote do aplicativo](device-architecture.md) | Saiba mais sobre quais arquiteturas de processador você deve usar ao criar seu pacote do aplicativo UWP. |
| [Instalação de streaming de aplicativo UWP](streaming-install.md) | A Instalação de Streaming do Aplicativo da UWP (Plataforma Universal do Windows) permite que você especifique as partes do seu aplicativo que você gostaria que a Microsoft Store baixasse primeiro. Quando os arquivos essenciais do aplicativo são baixados primeiro, o usuário pode iniciar e interagir com o aplicativo enquanto o resto termina de fazer o download em segundo plano. |
| [Criação de pacotes opcionais e conjunto relacionado](optional-packages.md) | Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o DLC (conteúdo para download), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original. |
| [Pacotes opcionais com código executável](optional-packages-with-executable-code.md) | Saiba como usar o Visual Studio para criar um pacote opcional com código executável. |
| [Instalar aplicativos do Windows 10 com o Instalador de Aplicativo](https://docs.microsoft.com/windows/msix/app-installer/app-installer-root) | O Instalador de Aplicativo permite que os aplicativos do Windows 10 sejam instalados clicando duas vezes no pacote do aplicativo. |
| [Instalar aplicativos usando a ferramenta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo UWP de um computador Windows 10 em qualquer dispositivo Windows 10 Mobile. É possível usar essa ferramenta para implantar um pacote do aplicativo quando o dispositivo Windows 10 Mobile está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse aplicativo. Este artigo descreve como instalar aplicativos UWP usando essa ferramenta. |
| [Configurar builds automáticos para seu aplicativo UWP](auto-build-package-uwp-apps.md) | Se você quiser empacotar seu aplicativo como parte de um processo de compilação automática, este tópico mostra como usar o Visual Studio Team Services (VSTS) para fazer isso. |
| [Declarações de funcionalidades do aplicativo](app-capability-declarations.md) | As funcionalidades devem ser declaradas no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211474) do aplicativo UWP para acessar determinada API ou recursos, como imagens, músicas ou dispositivos, como câmera ou microfone. |
| [Baixar e instalar atualizações de pacote da Store](self-install-package-updates.md) | Seu aplicativo UWP pode verificar programaticamente se há atualizações de pacote e instalar as atualizações. Seu aplicativo também pode consultar os pacotes que foram marcados como obrigatórios no Partner Center e desabilitar a funcionalidade até a atualização obrigatória ser instalada.  |
