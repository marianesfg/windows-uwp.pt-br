---
title: APIs experimentais
description: Noções básicas sobre APIs experimentais
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, experimental, api
ms.localizationpriority: medium
ms.openlocfilehash: 0e564d0bf7dc2acb3ffa05eaaf70def63a0908cd
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321980"
---
# <a name="experimental-apis"></a>APIs experimentais

As APIs experimentais estão nos estágios iniciais de design e têm probabilidade de mudar à medida que os proprietários incorporam os comentários e adicionam suporte a cenários adicionais.

Essas APIs são liberadas externamente usando [SDKs do Windows Insider](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK) de forma que os desenvolvedores possam experimentá-las e fazer comentários antes que se tornem parte da plataforma oficial. Enquanto eles estiverem na versão de pré-lançamento, não haverá nenhuma promessa de estabilidade ou de compromisso.

## <a name="consuming-experimental-apis"></a>Como consumir APIs experimentais
O IntelliSense avisará você se uma API for experimental. Você também receberá um aviso do compilador quando usar uma API experimental como "... é para fins de avaliação e está sujeita a alterações ou a remoção em atualizações futuras".

Esses avisos ajudam a proteger você da criação de dependências de APIs experimentais em projetos de produção. Para projetos experimentais, você pode ignorar ou desabilitar esses avisos.

Por padrão, essas APIs são desabilitadas em tempo de execução e chamá-las resultará em uma exceção de tempo de execução. Essa é outra proteção para ajudar a evitar dependências inadvertidas e a ampla distribuição de aplicativos que consumam APIs experimentais.

Para habilitar essas APIs para experimentação, use o plug-in Recursos do [WDP (Portal de Dispositivos do Windows)](https://docs.microsoft.com/en-us/windows/uwp/debug-test-perf/device-portal) no dispositivo de destino para habilitar o recurso correspondente para a API que você deseja chamar.

A documentação para uma determinada API experimental fica a critério da equipe proprietária.

## <a name="providing-feedback"></a>Fornecendo comentários

Se você tiver testado uma API experimental e quiser fazer comentários, use a categoria **Plataforma do Desenvolvedor** no [Hub do Windows Feedback](https://support.microsoft.com/help/4021566/windows-10-send-feedback-to-microsoft-with-feedback-hub).