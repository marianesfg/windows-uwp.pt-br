---
ms.assetid: ee51eae3-ed55-419e-ad74-6adf1e1fb8b9
title: Empacotamento manual de app
description: Esta seção contém links ou se vincula a artigos sobre o empacotamento manual de apps da Plataforma Universal do Windows (UWP).
ms.date: 04/30/2018
ms.topic: article
keywords: windows 10, uwp, empacotamento
ms.localizationpriority: medium
ms.openlocfilehash: d90307560c315741751a5ab58ccdf0eaf35d97fc
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66372788"
---
# <a name="manual-app-packaging"></a>Empacotamento manual de app

Se você quiser criar e assinar um pacote de app mas se não tiver usado o Visual Studio para desenvolver o app, precisará usar as ferramentas de empacotamento manual do app.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu aplicativo, é recomendável que você use o Assistente do Visual Studio para criar e assinar seu pacote de aplicativo. Para obter mais informações, consulte [Empacotar um aplicativo UWP com Visual Studio](https://docs.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

## <a name="purpose"></a>Finalidade

Esta seção contém links ou se vincula a artigos sobre o empacotamento manual de apps da Plataforma Universal do Windows (UWP).

| Tópico | Descrição |
|-------|-------------|
| [Criar um pacote do aplicativo com a ferramenta MakeAppx.exe](create-app-package-with-makeappx-tool.md) | MakeAppx.exe cria, criptografa, descriptografa e extrai arquivos de pacotes e lotes de apps. |
| [Criar um certificado de assinatura do pacote](create-certificate-package-signing.md) | Criar e exportar um certificado para a assinatura de pacote de apps com as ferramentas do PowerShell. |
| [Assinar um pacote de aplicativo usando o SignTool](sign-app-package-using-signtool.md) | Use SignTool para assinar um pacote de aplicativos com um certificado manualmente. |

### <a name="advanced-topics"></a>Tópicos avançados

Esta seção contém tópicos mais avançados para inserir componentes em um aplicativo grande e/ou complexo para um empacotamento e a instalação mais eficientes. 

> [!IMPORTANT]
> Se você pretende enviar seu aplicativo para a Store, você precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) e receber aprovação para usar qualquer um dos recursos avançados nesta seção.


| Tópico | Descrição |
|-------|-------------|
| [Introdução aos pacotes de ativos](asset-packages.md) | Pacotes de ativo são um tipo de pacote que atuam como um local centralizado para arquivos comuns de um aplicativo, eliminando efetivamente a necessidade de arquivos duplicados através de seus pacotes de arquitetura. |
| [Desenvolvimento com pacotes de ativos e dobra de pacote](package-folding.md) | Saiba como organizar de forma eficiente seu aplicativo com pacotes de ativo e dobramento de pacote. |
| [Pacotes de aplicativos simples de pacote](flat-bundles.md) | Descreve como criar um pacote simples para arquivos de pacote do aplicativo. |
| [Criação de pacote com o layout de empacotamento](packaging-layout.md) | O layout de empacotamento é um documento único que descreve a estrutura de empacotamento do aplicativo. Ele especifica os pacotes de um aplicativo (principal e opcional), os pacotes no lote e os arquivos nos pacotes. |
