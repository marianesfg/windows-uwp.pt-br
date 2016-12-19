---
author: laurenhughes
ms.assetid: 1abcbb13-80f0-4bf1-a812-649ee8bd1915
title: Empacotando apps
description: "Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP)."
translationtype: Human Translation
ms.sourcegitcommit: 28cd2b2a922a20e0b9ffc4d1ca65f6a55e92aa8f
ms.openlocfilehash: 8aa4bfc8004b4d0739fe09e6e49e66de372569c4

---
# <a name="packaging-apps"></a>Empacotando apps

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

## <a name="purpose"></a>Finalidade

Esta seção contém links ou se vincula a artigos sobre empacotamento de aplicativos da Plataforma Universal do Windows (UWP).

| Tópico | Descrição |
|-------|-------------|
| [Empacotando aplicativos UWP](packaging-uwp-apps.md) | Para vender seu aplicativo UWP ou distribuí-lo para outros usuários, você precisa criar um pacote appxupload para ele. Quando você criar o appxupload, outro pacote appx será gerado para uso no teste e no sideload. Você pode distribuir o aplicativo diretamente pelo sideload do pacote appx em um dispositivo. Este artigo descreve o processo de configuração, criação e teste de um pacote do aplicativo UWP. Para obter mais informações sobre o sideload, consulte [Fazer o sideload de apps com o DISM](http://go.microsoft.com/fwlink/?LinkID=231020). |
| [Criar um pacote do aplicativo com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | A MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de aplicativos. |
| [Instalar apps usando a ferramenta WinAppDeployCmd.exe](install-universal-windows-apps-with-the-winappdeploycmd-tool.md) | O Windows Application Deployment (WinAppDeployCmd.exe) é uma ferramenta de linha de comando que pode ser usada para implantar um aplicativo UWP de um computador Windows 10 em qualquer dispositivo Windows 10 Mobile. É possível usar essa ferramenta para implantar um pacote .appx quando o dispositivo com o Windows 10 Mobile está conectado por USB ou disponível na mesma sub-rede sem a necessidade do Microsoft Visual Studio ou a solução desse aplicativo. Este artigo descreve como instalar aplicativos UWP usando essa ferramenta. |
| [Configurar compilações automáticas para seu aplicativo UWP](auto-build-package-uwp-apps.md) | Se você quiser empacotar seu aplicativo como parte de um processo de compilação automática, este tópico mostra como usar o Visual Studio Team Services (VSTS) para fazer isso. |
| [Declarações de funcionalidades do app](app-capability-declarations.md) | As funcionalidades devem ser declaradas no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211474) do aplicativo UWP para acessar determinada API ou recursos, como imagens, músicas ou dispositivos, como câmera ou microfone. |
| [Baixar e instalar atualizações de pacote para seu app](self-install-package-updates.md) | Seu aplicativo UWP pode verificar programaticamente se há atualizações de pacote e instalar as atualizações. Seu aplicativo também pode consultar os pacotes que foram marcados como obrigatórios no painel do Centro de Desenvolvimento do Windows e desabilitar a funcionalidade até que a atualização obrigatória seja instalada. Este artigo descreve como realizar essas tarefas. |
 



<!--HONumber=Dec16_HO1-->


