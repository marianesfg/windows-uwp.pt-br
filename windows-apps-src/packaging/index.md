---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empacotando apps
description: "Esta seção contém links ou se vincula a artigos sobre empacotamento de apps da Plataforma Universal do Windows (UWP)."
ms.author: lahugh
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 0a9689738fac363012fb9af197f52ac8813b47c9
ms.lasthandoff: 02/07/2017

---
# <a name="packaging-apps"></a>Empacotando apps

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="purpose"></a>Finalidade

Esta seção contém links ou se vincula a artigos sobre empacotamento de apps da Plataforma Universal do Windows (UWP).

| Tópico | Descrição |
|-------|-------------|
| [Empacotando apps UWP](packaging-uwp-apps.md) | Para vender seu app UWP ou distribuí-lo para outros usuários, você precisa criar um pacote appxupload para ele. Quando você criar o appxupload, outro pacote appx será gerado para uso no teste e no sideload. Você pode distribuir o app diretamente pelo sideload do pacote appx em um dispositivo. Este artigo descreve o processo de configuração, criação e teste de um pacote do app UWP. Para obter mais informações sobre o sideload, consulte [Fazer o sideload de apps com o DISM](http://go.microsoft.com/fwlink/?LinkID=231020). |
| [Criar um pacote do app com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | A MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de apps. |
| [Criar um certificado para assinatura de pacote](create-certificate-package-signing.md) | Crie e exporte um certificado para assinatura de pacote de app com as ferramentas do PowerShell. |
| [Assinar um pacote do app usando a SignTool](sign-app-package-using-signtool.md) | Use SignTool para assinar manualmente um pacote do app com um certificado. |
| [Instalar apps usando a ferramenta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um app UWP de um computador Windows 10 em qualquer dispositivo Windows 10 Mobile. É possível usar essa ferramenta para implantar um pacote .appx quando o dispositivo com o Windows 10 Mobile está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse app. Este artigo descreve como instalar apps UWP usando essa ferramenta. |
| [Configurar compilações automáticas para seu app UWP](auto-build-package-uwp-apps.md) | Se você quiser empacotar seu app como parte de um processo de compilação automática, este tópico mostra como usar o Visual Studio Team Services (VSTS) para fazer isso. |
| [Declarações de funcionalidades do app](app-capability-declarations.md) | As funcionalidades devem ser declaradas no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211474) do app UWP para acessar determinada API ou recursos, como imagens, músicas ou dispositivos, como câmera ou microfone. |
| [Baixar e instalar atualizações de pacote para seu app](self-install-package-updates.md) | Seu app UWP pode verificar programaticamente se há atualizações de pacote e instalar as atualizações. Seu app também pode consultar os pacotes que foram marcados como obrigatórios no painel do Centro de Desenvolvimento do Windows e desabilitar a funcionalidade até que a atualização obrigatória seja instalada. Este artigo descreve como realizar essas tarefas. |
 

