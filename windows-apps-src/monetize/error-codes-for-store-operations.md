---
description: Este artigo descreve os códigos de erro comuns para operações da Microsoft Store para aplicativos e complementos, incluindo compras no aplicativo, licenciamento e atualizações de aplicativos com instalação automática.
title: Códigos de erro para operações da Microsoft Store
ms.date: 08/24/2017
ms.topic: article
keywords: windows 10, uwp, compras no aplicativo, IAPs, complementos, códigos de erro
ms.localizationpriority: medium
ms.openlocfilehash: ba505b30076c356a39ae195e1d187cbc49d8a66a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57662871"
---
# <a name="error-codes-for-store-operations"></a>Códigos de erro para operações da Microsoft Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Este artigo descreve os códigos de erro comuns que podem ocorrer durante o desenvolvimento ou os testes das operações relacionadas à Microsoft Store no aplicativo.

## <a name="in-app-purchase-error-codes"></a>Códigos de erro de compra no aplicativo

Os códigos de erro a seguir estão relacionados às operações de compra no aplicativo.

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F6100   | A compra no aplicativo não pôde ser concluída, pois o Espaço da criança está ativado. Para concluir a compra, conecte-se ao dispositivo com sua conta da Microsoft e execute o aplicativo novamente.               |
| 0x803F6101   | O aplicativo especificado não foi encontrado. O aplicativo pode não estar disponível na Microsoft Store ou talvez você tenha fornecido a ID errada para o aplicativo.     |
| 0x803F6102   | O complemento especificado não foi encontrado. O complemento pode não estar disponível na Microsoft Store ou talvez você tenha fornecido a ID errada para o complemento.                                               |
| 0x803F6103   | O produto especificado não foi encontrado. O produto pode não estar disponível na Microsoft Store ou talvez você tenha fornecido a ID errada para o produto.                                          |
| 0x803F6104   | Não foi possível concluir a compra realizada no aplicativo, pois você está executando uma versão de avaliação do aplicativo. Para concluir as compras no aplicativo, instale a versão completa dele.               |
| 0x803F6105   | Não foi possível concluir a compra realizada em aplicativo porque você não está conectado com sua conta da Microsoft.                                              |
| 0x803F6107   | Algo inesperado aconteceu ao processar a operação atual.                                             |
| 0x803F6108   | A compra realizada no aplicativo não pôde ser concluída devido à falta informações de licença do aplicativo. Esse erro pode ocorrer ao fazer o sideload do aplicativo. Para resolver esse problema, desinstale o aplicativo e reinstale-o na Microsoft Store para atualizar a licença do aplicativo.                                          |
| 0x803F6109   | A compra do complemento para consumo não pôde ser concluída porque a quantidade especificada é superior ao saldo restante.        |
| 0x803F610A   | Não há suporte para o tipo de provedor especificado para a conta de usuário da Microsoft Store.                                            |
| 0x803F610B   | Não há suporte para a operação especificada da Microsoft Store.                                             |
| 0x803F610C   | O aplicativo não oferece suporte ao contrato de tarefa em segundo plano especificado.                                             |
| 0x80040001   | A lista de IDs de produto de complemento fornecida é inválida.                        |
| 0x80040002   | A lista fornecida de palavras-chave é inválida.                   |
| 0x80040003   | O destino de atendimento é inválido.                       |

## <a name="licensing-error-codes"></a>Códigos de erro de licenciamento

Os códigos de erro a seguir estão relacionados às operações de licenciamento para aplicativos ou complementos.

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F700C   | O dispositivo está offline no momento. Para usar esse aplicativo enquanto o dispositivo está offline, abra as configurações da Microsoft Store e ative a configuração de **Permissões offline**.            |
| 0x803F8001   | Você não tem direito ao produto. Você pode estar usando uma conta da Microsoft diferente da usada para comprar o produto.           |
| 0x803F8002   | Seus direitos para o produto expiraram.           |
| 0x803F8003   | Seus direitos para o produto têm estado inválido e impedem a criação de uma licença.   |
| 0x803F8009<br/>0x803F800A   | O período de avaliação do aplicativo expirou.   |
| 0x803F8190   |  A licença não permite que o produto seja usado no país ou na região atual do seu dispositivo.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Você atingiu o número máximo de dispositivos que podem ser usados com jogos e aplicativos da Microsoft Store. Para usar esse jogo ou o aplicativo no dispositivo atual, primeiro remova outro dispositivo de sua conta.  |
| 0x803F9000<br/>0x803F9001    |  A licença está expirada ou corrompida. Para ajudar a resolver esse erro, tente executar o [solução de problemas para aplicativos do Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) para redefinir o cache de Store.     |
| 0x803F9006    |  A operação não pôde ser concluída, pois o usuário que está qualificado para este produto não está registrado no dispositivo com sua conta da Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  O dispositivo está offline. O dispositivo deve estar online para usar esse produto.            |
| 0x803F900A    |  A assinatura expirou.            |


## <a name="self-install-update-error-codes"></a>Códigos de erro de atualização com instalação automática

Os códigos de erro a seguir estão relacionados às [atualizações do pacote de instalação automática](../packaging/self-install-package-updates.md).

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F6200   | O consentimento do usuário é necessário para baixar a atualização de pacote.               |
| 0x803F6201   | O consentimento do usuário é necessário para baixar e instalar a atualização do pacote.                                                  |
| 0x803F6203   | O consentimento do usuário é necessário para instalar a atualização de pacote.                                         |
| 0x803F6204   | O consentimento do usuário é necessário para baixar a atualização de pacote, pois o download ocorrerá em uma conexão de rede limitada.                                             |
| 0x803F6206   | O consentimento do usuário é necessário para baixar e instalar a atualização de pacote, pois o download ocorrerá em uma conexão de rede limitada.     |


## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliação](in-app-purchases-and-trials.md)
* [Obter informações sobre produtos para os aplicativos e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para aplicativos e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras no aplicativo de aplicativos e complementos](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de assinaturas para seu aplicativo](enable-subscription-add-ons-for-your-app.md)
* [Implementar uma versão de avaliação do seu aplicativo](implement-a-trial-version-of-your-app.md)
