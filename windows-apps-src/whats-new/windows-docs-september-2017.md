---
author: QuinnRadich
title: Novidades do Windows Docs em setembro de 2017 - Desenvolver aplicativos UWP
description: Novos recursos, vídeos e diretrizes para desenvolvedores foram adicionados à documentação do desenvolvedor do Windows 10 referente a setembro de 2017.
keywords: novidades, atualização, recursos, diretrizes para desenvolvedores, Windows 10, 1709
ms.author: quradic
ms.date: 09/06/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 33091903ebf1a7ff1150dcaa9bd83a3e76417926
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4316514"
---
# <a name="whats-new-in-the-windows-developer-docs-in-september-2017"></a>Novidades dos documentos de desenvolvedor do Windows em setembro de 2017

A documentação do desenvolvedor do Windows está sendo constantemente atualizada com informações sobre os novos recursos disponíveis para desenvolvedores em toda a plataforma Windows. Disponibilizaram-se as visões gerais de recurso, as diretrizes para desenvolvedores e amostrar a seguir, contendo informações novas e atualizadas para desenvolvedores do Windows.

A Atualização de criadores do segundo semestre também está chegando, portanto, fique atento para uma grande variedade de documentações no próximo mês.

[Instale as ferramentas e o SDK](http://go.microsoft.com/fwlink/?LinkId=821431) no Windows 10 e você estará pronto para [criar um aplicativo Universal do Windows](../get-started/your-first-app.md) ou descobrir como pode usar seu [código de aplicativo existente no Windows](../porting/index.md).

## <a name="features"></a>Recursos

### <a name="xbox-live-creators-program"></a>Programa de Criadores do Xbox Live

O programa de criadores do Xbox Live agora é dinâmico, permitindo que você crie e publique facilmente jogos UWP que podem ser executados em computadores com Windows 10 e consoles Xbox One. Para obter mais informações, consulte [Introdução ao Programa de Criadores do Xbox Live](../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md).

## <a name="developer-guidance"></a>Diretrizes para desenvolvedor

### <a name="xaml-basics-tutorials"></a>Tutoriais sobre noções básicas de XAML

Escrevemos quatro [tutoriais de noções básicas de XAML](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-intro) para acompanhar a nova [amostra do PhotoLab](https://github.com/Microsoft/Windows-appsample-photo-lab), abordando quatro aspectos essenciais da programação em XAML: interfaces do usuário, vinculação de dados, estilos personalizados e layouts adaptáveis. Cada tutorial inicia com uma versão parcialmente completa da Amostra do PhotoLab e aproveita um componente faltando do passo a passo do aplicativo final. 

![Captura de tela da amostra do PhotoLab mostrando a página da galeria de fotos](images/PhotoLab-gallery-page.png)  

Visão geral rápida dos novos artigos:

+ [**Criar interfaces de usuário**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-ui) mostra como criar a interface básica da galeria de fotos.
+ [**Criar associações de dados**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-data-binding) mostra como adicionar associações de dados à galeria de fotos, preenchendo-a com dados da imagem real.
+ [**Criar estilos personalizados**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-style) mostra como adicionar estilos personalizados sofisticados aa menu de edição de fotos.
+ [**Criar layouts adaptáveis**](https://docs.microsoft.com/en-us/windows/uwp/get-started/xaml-basics-adaptive-layout) mostra como tornar o layout de galeria adaptável para que tenha uma boa aparência em todos os tamanhos de dispositivo e tela.

### <a name="get-started-tutorials"></a>Tutoriais de introdução

A seção Introdução dos documentos UWP foi atualizada com uma [nova página de aterrissagem da seção de tutoriais](https://docs.microsoft.com/windows/uwp/get-started/create-uwp-apps). Esta seção fornece uma estrutura nova e aprimorada para a experiência de Introdução, ajudando os usuários a encontrar e usar facilmente os tutoriais adequados para eles, incluindo os tutoriais de noções básicas de XAML mencionados acima.

### <a name="voice-and-tone"></a>Voz e tom

Adicionamos novas [orientações de voz em tom em aplicativos UWP](https://docs.microsoft.com/windows/uwp/in-app-help/voice-and-tone) a fim de fornecer recomendações para a criação de textos em seu aplicativo. Independentemente do que estiver criando, é importante que a linguagem usada seja acessível, amigável e informativa.

## <a name="samples"></a>Exemplos

### <a name="photolab-sample"></a>Amostra do PhotoLab

A [amostra do PhotoLab](https://github.com/Microsoft/windows-appsample-photo-lab) fornece uma galeria de fotos básica e uma experiência de edição de fotos.

![Captura de tela da amostra do PhotoLab mostrando a página de edição de fotos](images/PhotoLab-editing-page.png)  

### <a name="customer-orders"></a>Pedidos de clientes

A amostra do [Banco de dados de pedidos do cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) foi atualizada para usar o novo .NET Core 2.0 e o Entity Framework.