---
Description: Descubra as diferentes opções que os aplicativos Win32 da área de trabalho têm para enviar notificações do sistema
title: Notificações do sistema a partir de aplicativos da área de trabalho
label: Toast notifications from desktop apps
template: detail.hbs
ms.date: 05/01/2018
ms.topic: article
keywords: Windows 10, UWP, Win32, área de trabalho, notificações do sistema, ponte de área de trabalho, msix, pacote esparso, opções para enviar notificações de notificação, servidor com, com, com, falso com, sem com, sem com, enviar notificação
ms.localizationpriority: medium
ms.openlocfilehash: b84120a592a1c2f5f18c6b6121568cbf126a582e
ms.sourcegitcommit: 87fd0ec1e706a460832b67f936a3014f0877a88c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/12/2020
ms.locfileid: "83234547"
---
# <a name="toast-notifications-from-desktop-apps"></a>Notificações do sistema a partir de aplicativos da área de trabalho

Os aplicativos da área de trabalho (incluindo aplicativos [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) empacotados, aplicativos que usam [pacotes esparsos](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps) para obter a identidade do pacote e aplicativos Win32 não empacotados clássicos) podem enviar notificações de sistema interativas, assim como os aplicativos do Windows. No entanto, há algumas opções diferentes para aplicativos da área de trabalho por causa de esquemas de ativação diferentes.

Neste artigo, listamos as opções que você tem para enviar uma notificação do sistema no Windows 10. Cada opção oferece suporte completo para...

* Persistir na Central de Ações
* Ativar pelo pop-up e na Central de Ações
* Ativar enquanto o EXE não estiver em execução

## <a name="all-options"></a>Todas as opções

A tabela a seguir ilustra as opções para oferecer suporte a notificações do sistema no aplicativo da área de trabalho e os recursos com suporte correspondentes. Você pode usar a tabela para selecionar a melhor opção para seu cenário.<br/><br/>

| Opção | Visuais | Ações | Entradas | Ativas no processo |
| -- | -- | -- | -- | -- |
| [Ativador COM](#preferred-option---com-activator) | ✔️ | ✔️ | ✔️ | ✔️ |
| [Nenhum CLSID COM/Stub](#alternative-option---no-com--stub-clsid) | ✔️ | ✔️ | ❌ | ❌ |


## <a name="preferred-option---com-activator"></a>Opção preferida - ativador COM

Essa é a opção preferida que funciona para aplicativos de desktop e dá suporte a todos os recursos de notificação. Não tenha medo do "ativador COM;" temos uma biblioteca [para C#](send-local-toast-desktop.md) e [aplicativos C++](send-local-toast-desktop-cpp-wrl.md) que torna isso muito simples, mesmo se você nunca tiver criado um servidor COM antes.<br/><br/>

| Visuais | Ações | Entradas | Ativas no processo |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ✔️ | ✔️ |

Com a opção de ativador COM, você pode usar os seguintes modelos de notificação e os tipos de ativação em seu aplicativo.<br/><br/>

| Tipo de modelo e a ativação | Pacote MSIX/esparso | Win32 clássico |
| -- | -- | -- |
| ToastGeneric em primeiro plano | ✔️ | ✔️ |
| ToastGeneric em segundo plano | ✔️ | ✔️ |
| Protocolo ToastGeneric | ✔️ | ✔️ |
| Modelos herdados | ✔️ | ❌ |

> [!NOTE]
> Se você adicionar o COM Activator ao seu aplicativo de pacote MSIX/esparso existente, as ativações de primeiro plano/segundo plano e de notificação herdadas agora ativarão o seu ativador em vez de sua linha de comando.

Para saber como usar essa opção, consulte [Enviar uma notificação do sistema local de aplicativos C# da área de trabalho](send-local-toast-desktop.md) ou [Enviar uma notificação do sistema local de aplicativos de WRL do Desktop C++](send-local-toast-desktop-cpp-wrl.md).


## <a name="alternative-option---no-com--stub-clsid"></a>Opção alternativa - sem CLSID COM/Stub

Esta é uma opção alternativa, se você não puder implementar um ativador COM. No entanto, você sacrifica alguns recursos, como suporte de entrada (caixas de texto em notificações do sistema) e ativa no processo.<br/><br/>

| Visuais | Ações | Entradas | Ativas no processo |
| -- | -- | -- | -- |
| ✔️ | ✔️ | ❌ | ❌ |

Com essa opção, se houver suporte para Win32 clássico, você é muito mais limitado nos modelos de notificação e tipos de ativação que você pode usar, conforme visto abaixo.<br/><br/>

| Tipo de modelo e a ativação | Pacote MSIX/esparso | Win32 clássico |
| -- | -- | -- |
| ToastGeneric em primeiro plano | ✔️ | ❌ |
| ToastGeneric em segundo plano | ✔️ | ❌ |
| Protocolo ToastGeneric | ✔️ | ✔️ |
| Modelos herdados | ✔️ | ❌ |

Para aplicativos e aplicativos [MSIX](https://docs.microsoft.com/windows/msix/desktop/source-code-overview) empacotados que usam [pacotes esparsos](https://docs.microsoft.com/windows/apps/desktop/modernize/grant-identity-to-nonpackaged-apps), basta enviar notificações do sistema como um aplicativo UWP. Quando o usuário clica em sua notificação do sistema, o aplicativo será iniciado pela linha de comando com os argumentos de inicialização especificados na notificação do sistema.

Para aplicativos clássicos do Win32, configure a AUMID para que você pode enviar notificações do sistema e, em seguida, especifique uma CLSID no seu atalho. Pode ser qualquer GUID aleatória. Não adicione o servidor/ativador COM. Você estiver adicionando um "stub" COM CLSID, que fará com que a Central de ações mantenha a notificação. Observe que você pode usar somente notificações de ativação de protocolo, pois o CLSID stub interrompe a ativação de outras ativações de notificação do sistema. Portanto, você precisa atualizar seu aplicativo para oferecer suporte à ativação de protocolo e fazer com que o protocolo de notificações do sistema ative seu próprio aplicativo.


## <a name="resources"></a>Recursos

* [Enviar uma notificação do sistema local a partir de aplicativos C# da área de trabalho](send-local-toast-desktop.md)
* [Enviar uma notificação do sistema local a partir de aplicativos C++ WRL da área de trabalho](send-local-toast-desktop-cpp-wrl.md)
* [Conteúdo e documentação sobre notificações do sistema](adaptive-interactive-toasts.md)
