---
title: Guia passo a passo para os criadores do Xbox Live
description: Fornece uma orientação de etapas para integrar o Xbox Live para o programa de criadores.
ms.assetid: 7f9d093e-479a-4ad4-9965-a4ea6f0b2ac3
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, para criadores
ms.localizationpriority: medium
ms.openlocfilehash: ae02fe412e6300ac74b3f1106cdf6a5bb304d36b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57628301"
---
# <a name="step-by-step-guide-to-integrate-xbox-live-creators-program"></a>Guia passo a passo para integrar o Programa de Criadores do Xbox Live

Nesta seção ajudarão você a obter o título em funcionamento com o Xbox Live:

## <a name="1-ensure-you-have-a-title-created-in-partner-center"></a>1. Verifique se que você tem um título criado no Partner Center
Cada título Xbox Live deve ser definido em [Partner Center](https://partner.microsoft.com/dashboard) antes de poder entrar e fazer chamadas de serviço do Xbox Live.  [Criar um título Criadores](create-and-test-a-new-creators-title.md) mostrará como fazê-lo.

## <a name="2-follow-the-appropriate-guide-to-setup-your-ide-or-game-engine"></a>2. Siga o guia apropriado para configurar seu IDE ou mecanismo de jogos
Você pode siga o guia de Introdução apropriado para sua plataforma e o mecanismo e conheça os fundamentos do Xbox Live que você avança.

* [Desenvolver um título para criadores com o Visual Studio](develop-creators-title-with-visual-studio.md) mostrará como vincular o projeto do Visual Studio com a configuração do Xbox Live no Partner Center.

* [Desenvolver um título para criadores com o Unity](develop-creators-title-with-unity.md) mostrará a você como criar um novo serviço Xbox Live habilitado título do Unity, adicionar recursos como placares de líderes ao seu título e gerar um projeto nativo do Visual Studio.

## <a name="3-xbox-live-concepts"></a>3. Conceitos de Xbox Live
Após a criação de um título, você deve aprender sobre os conceitos do Xbox Live que afetarão sua experiência desenvolvendo títulos.

- [Ambiente de teste do Xbox Live](../xbox-live-sandboxes.md)
- [Autorizar contas do Xbox Live](authorize-xbox-live-accounts.md)

## <a name="4-add-xbox-live-features"></a>4. Adicionar recursos ao vivo do Xbox

Adicione recursos do Xbox Live para seu jogo:

- [Plataforma Social - perfil, amigos, a presença do Xbox Live](../social-platform/social-platform.md)
- [Placar de líderes de plataforma – estatísticas, de dados, conquistas do Xbox Live](../data-platform/data-platform.md)
- [Armazenamento de título da plataforma - armazenamento conectado, de armazenamento ao vivo Xbox](../storage-platform/storage-platform.md)

## <a name="5-release-your-title"></a>5. O título da versão

Se você estiver usando o programa de criadores do Xbox Live, o processo é não é diferente de liberação de quaisquer outro UWP.  Para obter detalhes sobre o processo, você pode ver [aplicativos do Windows publicar](https://developer.microsoft.com/en-us/store/publish-apps).
