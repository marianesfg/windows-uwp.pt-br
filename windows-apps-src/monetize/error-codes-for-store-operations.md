---
author: Xansky
description: Este artigo descreve os códigos de erro comuns para operações de loja para aplicativos e complementos, incluindo atualizações de aplicativo instalar sozinho, licenciamento e compra no aplicativo.
title: Códigos de erro para operações da Microsoft Store
ms.author: mhopkins
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, compras no aplicativo, IAPs, complementos, códigos de erro
ms.localizationpriority: medium
ms.openlocfilehash: bc2d3a4562be403172520f8377afb16c782a49c0
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5512941"
---
# <a name="error-codes-for-store-operations"></a>Códigos de erro para operações da Microsoft Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Este artigo descreve os códigos de erro comuns que você pode encontrar enquanto você estiver desenvolvendo ou teste operações relacionadas à loja no seu aplicativo.

## <a name="in-app-purchase-error-codes"></a>Códigos de erro de compra no aplicativo

Os seguintes códigos de erro são relacionados às operações de compra no aplicativo.

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F6100   | A compra no aplicativo não pôde ser concluída porque da criança está ativa. Para concluir a compra, entre no dispositivo com sua conta da Microsoft e executar o aplicativo novamente.               |
| 0x803F6101   | O aplicativo especificado não foi encontrado. O aplicativo pode não estar disponível na loja, ou você pode ter sido a ID da loja errado para o aplicativo.     |
| 0x803F6102   | O complemento especificado não foi encontrado. O complemento não pode estar disponível na loja ou seu pode ter sido a ID da loja errado para o complemento.                                               |
| 0x803F6103   | O produto especificado não pôde ser encontrado. O produto não pode estar disponível na loja, ou você pode ter sido a ID da loja errado para o produto.                                          |
| 0x803F6104   | A compra no aplicativo não pôde ser concluída porque você está executando uma versão de avaliação do aplicativo. Para concluir compras no aplicativo, instale a versão completa do aplicativo.               |
| 0x803F6105   | A compra no aplicativo não pôde ser concluída porque você não estiver conectado com sua conta da Microsoft.                                              |
| 0x803F6107   | Algo inesperado ocorreu durante o processamento a operação atual.                                             |
| 0x803F6108   | A compra no aplicativo não pôde ser concluída porque a licença do aplicativo não tem informações. Esse erro pode ocorrer quando você sideload seu aplicativo. Para resolver esse problema, desinstale o aplicativo e reinstale da loja para atualizar a licença do aplicativo.                                          |
| 0x803F6109   | O atendimento de complementos consumíveis não pôde ser concluído porque a quantidade especificada é mais do que o saldo restante.        |
| 0x803F610A   | Não há suporte para o tipo de provedor especificado para a conta de usuário de armazenamento.                                            |
| 0x803F610B   | Não há suporte para a operação de Store especificada.                                             |
| 0x803F610C   | O aplicativo não oferece suporte para o contrato de tarefa em segundo plano especificado.                                             |
| 0x80040001   | A lista fornecida de produto do complemento IDs é inválido.                        |
| 0x80040002   | Lista de palavras-chave fornecida é inválida.                   |
| 0x80040003   | O destino de atendimento é inválido.                       |

## <a name="licensing-error-codes"></a>Códigos de erro de licenciamento

Os seguintes códigos de erro estão relacionados para o licenciamento operações para aplicativos ou complementos.

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F700C   | O dispositivo está offline no momento. Para usar este aplicativo enquanto o dispositivo está offline, abra as configurações de armazenamento e alterne a configuração de **Permissões off-line** .            |
| 0x803F8001   | Você não tem um direito para o produto. Você pode estar usando uma conta da Microsoft diferente daquele que foi usada para comprar o produto.           |
| 0x803F8002   | Sua qualificação para o produto expirou.           |
| 0x803F8003   | É sua qualificação para o produto em um estado inválido que impede que uma licença que está sendo criada.   |
| 0x803F8009<br/>0x803F800A   | O período de avaliação para o aplicativo expirou.   |
| 0x803F8190   |  A licença não permite que o produto a ser usado no atual país ou região do dispositivo.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Você atingiu o número máximo de dispositivos que podem ser usados com jogos e aplicativos da Store. Para usar este jogo ou o aplicativo no dispositivo atual, primeiro remova outro dispositivo de sua conta.  |
| 0x803F9000<br/>0x803F9001    |  A licença está expirado ou corrompido. Para ajudar a resolver esse erro, tente executar a [solução de problemas para aplicativos do Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) para redefinir o cache de armazenamento.     |
| 0x803F9006    |  A operação não pôde ser concluída porque o usuário que está qualificado para este produto não é conectado ao dispositivo com a conta da Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  O dispositivo está offline. O dispositivo precisa estar online para usar este produto.            |
| 0x803F900A    |  A assinatura expirou.            |


## <a name="self-install-update-error-codes"></a>Instalar sozinho códigos de erro de atualização

Os seguintes códigos de erro estão relacionados ao [instalar automaticamente atualizações de pacote](../packaging/self-install-package-updates.md).

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F6200   | Consentimento do usuário é necessário para baixar a atualização do pacote.               |
| 0x803F6201   | Consentimento do usuário é necessário para baixar e instalar a atualização do pacote.                                                  |
| 0x803F6203   | Consentimento do usuário é necessário para instalar a atualização do pacote.                                         |
| 0x803F6204   | Consentimento do usuário é necessário para baixar a atualização do pacote porque o download ocorrerão em uma conexão de rede limitada.                                             |
| 0x803F6206   | Consentimento do usuário é necessário para baixar e instalar a atualização do pacote porque o download ocorrerão em uma conexão de rede limitada.     |


## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de assinatura para o app](enable-subscription-add-ons-for-your-app.md)
* [Implementar uma versão de avaliação do app](implement-a-trial-version-of-your-app.md)
