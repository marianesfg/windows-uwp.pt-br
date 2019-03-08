---
title: Instalar aplicativos UWP com o Instalador de Aplicativo
description: Esta seção contém ou é ligada a artigos sobre o Instalador de Aplicativo e como usar seus recursos.
ms.date: 06/05/2018
ms.topic: article
keywords: windows 10, uwp, instalador de aplicativo, AppInstaller, sideload, conjunto relacionado, pacotes opcionais
ms.localizationpriority: medium
ms.openlocfilehash: ca72f9570c5ecef4a93b03f297ecb1d5064c5bef
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57623321"
---
# <a name="install-uwp-apps-with-app-installer"></a>Instalar aplicativos UWP com o Instalador de Aplicativo

## <a name="purpose"></a>Finalidade
Esta seção contém ou é ligada a artigos sobre o Instalador de Aplicativo e como usar seus recursos. 

Instalador de Aplicativo permite que os aplicativos UWP sejam instalados clicando duas vezes no conjunto de aplicativo. Isso significa que os usuários não precisam usar o PowerShell ou outras ferramentas de desenvolvedor para implantar aplicativos UWP. O Instalador de Aplicativo também pode instalar um aplicativo da Web, pacotes opcionais e conjuntos relacionados. Para saber como usar o Instalador de Aplicativo para instalar seu aplicativo, consulte os tópicos na tabela.

| Tópico | Descrição |
|-------|-------------|
| [Criar o arquivo do instalador de aplicativo com o Visual Studio](create-appinstallerfile-vs.md)| Saiba como usar o Visual Studio para permitir atualizações automáticas usando o arquivo .appinstaller. |
| [Instalar aplicativos da UWP a partir de uma página da web](installing-UWP-apps-web.md) | Nesta seção, analisaremos as etapas que você precisa executar para permitir que os usuários instalem seus aplicativos diretamente a partir da página da Web. |
| [Instalar um conjunto relacionado usando um arquivo do instalador do aplicativo](install-related-set.md) | Nesta seção, saiba como permitir a instalação de um conjunto relacionado por meio do Instalador de Aplicativo. Também explicaremos as etapas para construir um arquivo do Instalador de Aplicativo que definirão o conjunto relacionado. |
| [Solucionar problemas de instalação com o arquivo do instalador do aplicativo](troubleshoot-appinstaller-issues.md) | Problemas e soluções comuns ao fazer sideload de aplicativos com o arquivo do Instalador de Aplicativo. |
| [Referência de arquivo (appinstaller) de instalador de aplicativo](https://docs.microsoft.com/uwp/schemas/appinstallerschema/app-installer-file) | Exibir o esquema XML completo para o arquivo do Instalador de Aplicativo. |

## <a name="tutorials"></a>Tutoriais 

Siga estes tutoriais e saiba como hospedar e instalar um aplicativo UWP de diversas plataformas de distribuição. Estes tutoriais são úteis para empresas e desenvolvedores que não desejam ou precisam publicar seus aplicativos na Store, mas ainda querem aproveitar a plataforma de empacotamento e implantação do Windows 10.

| Tutorial | Descrição |
|----------|-------------|
| [Instalar um aplicativo UWP a partir de um aplicativo Web do Azure](web-install-azure.md) | Crie um aplicativo Web do Azure e use-o para hospedar e distribuir seu pacote de aplicativo UWP. |
| [Instalar um aplicativo UWP a partir de um servidor IIS](web-install-IIS.md) | Configure um servidor IIS, verifique se o aplicativo Web pode hospedar pacotes de aplicativos e use o Instalador de aplicativo de maneira eficaz. |
| [Instalam UWP de hospedagem de pacotes de aplicativos no AWS para web](web-install-aws.md) | Saiba como configurar o serviço Amazon Simple Storage para hospedar seu pacote de aplicativo UWP de um site. |

