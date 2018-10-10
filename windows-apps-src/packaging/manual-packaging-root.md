---
author: laurenhughes
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Empacotamento manual de app
description: Esta seção contém links ou se vincula a artigos sobre o empacotamento manual de apps da Plataforma Universal do Windows (UWP).
ms.author: lahugh
ms.date: 04/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: fcd6d937c7261b5cfa8af954eb5d2ec2869d8afd
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4505178"
---
# <a name="manual-app-packaging"></a>Empacotamento manual de app

Se você quiser criar e assinar um pacote de app mas se não tiver usado o Visual Studio para desenvolver o app, precisará usar as ferramentas de empacotamento manual do app.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu aplicativo, é recomendável que você use o Assistente do Visual Studio para criar e assinar seu pacote de aplicativo. Para obter mais informações, consulte [Empacotar um aplicativo UWP com Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Finalidade

Esta seção contém links ou se vincula a artigos sobre o empacotamento manual de apps da Plataforma Universal do Windows (UWP).

| Tópico | Descrição |
|-------|-------------|
| [Criar um pacote do app com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de apps. |
| [Criar um certificado para assinatura de pacote](create-certificate-package-signing.md) | Crie e exporte um certificado para assinatura de pacote de app com as ferramentas do PowerShell. |
| [Assinar um pacote do app usando a SignTool](sign-app-package-using-signtool.md) | Use SignTool para assinar manualmente um pacote do app com um certificado. |

### <a name="advanced-topics"></a>Tópicos avançados

Esta seção contém tópicos mais avançados para inserir componentes em um aplicativo grande e/ou complexo para um empacotamento e a instalação mais eficientes. 

> [!IMPORTANT]
> Se você pretende enviar seu aplicativo para a Store, você precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) e receber aprovação para usar qualquer um dos recursos avançados nesta seção.


| Tópico | Descrição |
|-------|-------------|
| [Introdução aos pacotes de ativo](asset-packages.md) | Pacotes de ativo são um tipo de pacote que atuam como um local centralizado para arquivos comuns de um aplicativo, eliminando efetivamente a necessidade de arquivos duplicados através de seus pacotes de arquitetura. |
| [Desenvolvendo com os pacotes de ativo e dobramento de pacote](package-folding.md) | Saiba como organizar de forma eficiente seu aplicativo com pacotes de ativo e dobramento de pacote. |
| [Pacotes do lote simples de aplicativo](flat-bundles.md) | Descreve como criar um lote simples para os arquivos do pacote do aplicativo. |
| [Criação do pacote com o layout de empacotamento](packaging-layout.md) | O layout de empacotamento é um documento único que descreve a estrutura de empacotamento do aplicativo. Ele especifica os pacotes de um aplicativo (principal e opcional), os pacotes no lote e os arquivos nos pacotes. |
