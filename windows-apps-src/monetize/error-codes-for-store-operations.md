---
author: mcleanbyron
description: Este artigo descreve os códigos de erro comuns para operações de repositório para aplicativos e complementos, incluindo app autoinstalar atualizações, licenciamento e compra-app.
title: Códigos de erro para operações da Microsoft Store
ms.author: mcleans
ms.date: 08/24/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, no aplicativo compras, IAPs, complementos, códigos de erro
ms.localizationpriority: medium
ms.openlocfilehash: 0931397e24eaba44cdf04092f5f367c43a91dd82
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "959371"
---
# <a name="error-codes-for-store-operations"></a>Códigos de erro para operações da Microsoft Store

<!-- confirm whether symbolic names are defined for app developers, or do they just handle direct error code values -->

Este artigo descreve os códigos de erro comuns que você pode encontrar enquanto você estiver desenvolvendo ou testar operações relacionadas ao repositório em seu aplicativo.

## <a name="in-app-purchase-error-codes"></a>Códigos de erro no aplicativo de compra

Os códigos de erro a seguir estão relacionados às operações de compra-app.

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F6100   | A compra-app não pôde ser concluída porque cantinho está ativo. Para concluir a compra, inscreva-se para o dispositivo com sua conta da Microsoft e executar o aplicativo novamente.               |
| 0x803F6101   | O aplicativo especificado não foi encontrado. O aplicativo pode não estar mais disponível no repositório ou talvez você tenha fornecido a ID da loja errado para o aplicativo.     |
| 0x803F6102   | O complemento especificado não foi encontrado. O complemento pode não estar mais disponível no repositório de ou seu pode ter fornecido a ID da loja errado para o complemento.                                               |
| 0x803F6103   | O produto especificado não pôde ser encontrado. O produto pode não estar mais disponível no repositório ou talvez você tenha fornecido a ID da loja errado para o produto.                                          |
| 0x803F6104   | A compra-app não pôde ser concluída porque você está executando uma versão de avaliação do aplicativo. Para concluir compras-app, instale a versão completa do aplicativo.               |
| 0x803F6105   | A compra-app não pôde ser concluída porque você não tiver entrado com sua conta da Microsoft.                                              |
| 0x803F6107   | Algo inesperado aconteceu ao processar a operação atual.                                             |
| 0x803F6108   | A compra-app não pôde ser concluída porque a licença de aplicativo estiver faltando informações. Esse erro pode ocorrer quando você lado-carga seu aplicativo. Para resolver esse problema, desinstale o aplicativo e reinstalá-lo usando o repositório para atualizar a licença de aplicativo.                                          |
| 0x803F6109   | O cumprimento de complemento consumo não pôde ser concluído porque a quantidade especificada é maior do que o equilíbrio restante.        |
| 0x803F610A   | Não há suporte para o tipo de provedor especificado para a conta de usuário do repositório.                                            |
| 0x803F610B   | Não há suporte para a operação do repositório especificada.                                             |
| 0x803F610C   | O aplicativo não oferece suporte para o contrato de tarefa do plano de fundo especificada.                                             |
| 0x80040001   | IDs de lista fornecida de produto de complemento é inválido.                        |
| 0x80040002   | Lista de palavras-chave fornecida é inválida.                   |
| 0x80040003   | O destino de adequação é inválido.                       |

## <a name="licensing-error-codes"></a>Códigos de erro de licenciamento

Os códigos de erro a seguir estão relacionados às operações para aplicativos ou complementos de licenciamento.

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F700C   | O dispositivo está offline no momento. Para usar este aplicativo enquanto o dispositivo estiver offline, abra suas configurações de armazenamento e alternar a configuração **Permissões Offline** .            |
| 0x803F8001   | Você não tem um direito para o produto. Você pode estar usando uma conta da Microsoft diferente daquela que foi utilizado para adquirir o produto.           |
| 0x803F8002   | Seu direito para o produto expirou.           |
| 0x803F8003   | Seu direito para o produto está em um estado inválido que impede que uma licença que está sendo criada.   |
| 0x803F8009<br/>0x803F800A   | O período de avaliação para o aplicativo expirou.   |
| 0x803F8190   |  A licença não permite que o produto a ser usado no atual país ou região do seu dispositivo.  |
| 0x803F81F5<br/>0x803F81F6<br/>0x803F81F7<br/>0x803F81F8<br/>0x803F81F9   |  Você atingiu o número máximo de dispositivos que podem ser usados com jogos e aplicativos do repositório. Para usar este jogo ou o aplicativo no dispositivo atual, primeiro remova outro dispositivo da sua conta.  |
| 0x803F9000<br/>0x803F9001    |  A licença é expirados ou está corrompido. Para ajudar a resolver esse erro, tente executar a [solução de problemas para aplicativos do Windows](https://support.microsoft.com/help/4027498/windows-run-the-troubleshooter-for-windows-apps) para redefinir o cache do repositório.     |
| 0x803F9006    |  A operação não pôde ser concluída porque o usuário que tem o direito desse produto não tiver entrado no dispositivo com sua conta da Microsoft.            |
| 0x803F9008<br/>0x803F9009    |  Seu dispositivo está offline. O dispositivo deve estar online para usar este produto.            |
| 0x803F900A    |  A assinatura expirou.            |


## <a name="self-install-update-error-codes"></a>Instalar sozinho códigos de erro de atualização

Os códigos de erro a seguir estão relacionados às [atualizações do pacote de instalação automática](../packaging/self-install-package-updates.md).

|  Código de erro  |  Descrição  |
|--------------|---------------|
| 0x803F6200   | Consentimento do usuário é necessária para baixar o pacote de atualização.               |
| 0x803F6201   | Consentimento do usuário é necessária para baixar e instalar o pacote de atualização.                                                  |
| 0x803F6203   | Consentimento do usuário é necessário para instalar o pacote de atualização.                                         |
| 0x803F6204   | Consentimento do usuário é necessária para baixar a atualização do pacote, pois o download ocorrerá em uma conexão de rede monitorados.                                             |
| 0x803F6206   | Consentimento do usuário é necessária para baixar e instalar a atualização do pacote, pois o download ocorrerá em uma conexão de rede monitorados.     |


## <a name="related-topics"></a>Tópicos relacionados

* [Compras no aplicativo e avaliações](in-app-purchases-and-trials.md)
* [Obter informações do produto para apps e complementos](get-product-info-for-apps-and-add-ons.md)
* [Obter informações de licença para apps e complementos](get-license-info-for-apps-and-add-ons.md)
* [Habilitar compras nos aplicativos e complementos no aplicativo](enable-in-app-purchases-of-apps-and-add-ons.md)
* [Habilitar compras de complementos consumíveis](enable-consumable-add-on-purchases.md)
* [Habilitar complementos de assinatura para o app](enable-subscription-add-ons-for-your-app.md)
* [Implementar uma versão de avaliação do app](implement-a-trial-version-of-your-app.md)
