---
title: Planeje a compatibilização do DirectX
description: Planeje o seu projeto de portabilidade para jogos do DirectX 9 para o DirectX 11 e Plataforma Universal do Windows (UWP) - atualize o código dos elementos gráficos e coloque o seu jogo no ambiente do Windows Runtime.
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx, compatibilização
ms.localizationpriority: medium
ms.openlocfilehash: 247c7cb05027520cb7a39e04ff65579297b66dc9
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66368299"
---
# <a name="plan-your-directx-port"></a>Planeje a compatibilização do DirectX



**Resumo**

-   Planeje a compatibilização do DirectX
-   [Alterações importantes do Direct3D 9 para o Direct3D 11](understand-direct3d-11-1-concepts.md)
-   [Mapeamento de recurso](feature-mapping.md)


Planeje o seu projeto de portabilidade para jogos do DirectX 9 para o DirectX 11 e Plataforma Universal do Windows (UWP): atualize o código dos elementos gráficos e coloque o seu jogo no ambiente do Windows Runtime.

## <a name="plan-to-port-graphics-code"></a>Planejar para a compatibilização do código de elementos gráficos


Antes de você começar a compatibilizar o seu jogo com a UWP, é importante certificar que ele não tenha nenhum remanescente do Direct3D 8. Verifique se o jogo não tem resquícios do pipeline de funções fixas. Veja uma lista completa de recursos preteridos, incluindo funcionalidade de pipeline fixo, em [Recursos preteridos](https://docs.microsoft.com/windows/desktop/direct3d10/d3d10-graphics-programming-guide-api-features-deprecated).

A atualização do Direct3D 9 para o Direct3D 11 é mais do que uma alteração do tipo "localizar e substituir". Você deve saber a diferença entre o dispositivo Direct3D, o contexto de dispositivo e a infraestrutura gráfica. Além disso, deve aprender sobre outras mudanças importantes desde o Direct3D 9. Você pode iniciar esse processo lendo os outros tópicos desta seção.

Você deve substituir as livrarias auxiliares D3DX e DXUT pelas suas ou por ferramentas da comunidade. Veja a seção [Mapeamento de recursos](feature-mapping.md) para saber mais.

> **Observação**    você pode usar o [Kit de ferramentas do DirectX](https://go.microsoft.com/fwlink/p/?LinkID=248929) ou [DirectXTex](https://go.microsoft.com/fwlink/p/?LinkID=248926) para substituir algumas funcionalidades que anteriormente foi fornecida por D3DX e DXUT.

 

Sombreadores escritos na linguagem de assembly devem ser atualizados para HLSL usando o nível de modelo 4 de sombreador 9\_1 ou 9\_3 funcionalidade e sombreadores escritos para a biblioteca de efeitos precisará ser atualizado para uma versão mais recente da sintaxe do HLSL. Veja a seção [Mapeamento de recursos](feature-mapping.md) para saber mais.

Conheça os diferentes [níveis de recursos do Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro). Os níveis de recursos classificam uma ampla gama de hardware de vídeo, definindo conjuntos de funcionalidades conhecidas. Cada conjunto corresponde aproximadamente às versões do Direct3D, de 9.1 a 11.2. Todos os níveis de recursos usam a API do DirectX 11.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Planeje a portabilidade do código da interface do usuário Win32 para CoreWindow


Aplicativos UWP são executados em uma janela criada por um contêiner de aplicativo, chamada [**CoreWindow**](https://docs.microsoft.com/uwp/api/Windows.UI.Core.CoreWindow). Seu jogo controle a janela herdando de [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView), que exige menos detalhes de implementação do que uma janela de área de trabalho. O loop principal do seu jogo estará no método [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run).

O ciclo de vida de um aplicativo UWP é muito diferente de um aplicativo de área de trabalho. Será necessário salvar o jogo frequentemente, porque quando o evento suspenso ocorre, seu aplicativo tem apenas um tempo limitado para parar de executar o código e você precisa garantir que o jogador possa voltar para onde estava assim que o aplicativo for retomado. Os jogos devem ser salvos com frequência suficiente para manter uma experiência de jogabilidade contínua a partir da retomada, mas não com tanta frequência a ponto de o processo influenciar na taxa de quadros ou fazer com que o jogo trave. Seu jogo precisará carregar potencialmente o estado de jogo quando for retomado a partir de um estado encerrado.

[DirectXMath](https://docs.microsoft.com/windows/desktop/dxmath/ovw-xnamath-progguide) pode ser usado como substituto de D3DXMath e XNAMath, e pode ser útil se você precisar de uma biblioteca matemática. DirectXMath possui tipos de dados rápidos e portáteis, além dos tipos alinhados e empacotados com sombreadores.

As bibliotecas nativas, como [API encaixada](https://docs.microsoft.com/windows/desktop/Sync/what-s-new-in-synchronization), foram expandidas para dar suporte a intrínsecos ARM. Se o seu jogo usa APIs entrelaçadas, você pode continuar usando no DirectX 11 e na UWP.

Nossos modelos e exemplos de códigos usam recursos C++ com os quais você pode não estar familiarizado ainda. Por exemplo, métodos assíncronos são usados com [**expressões lambda**](https://docs.microsoft.com/cpp/cpp/lambda-expressions-in-cpp) para carregar recursos do Direct3D sem bloquear o thread de interface do usuário.

Há dois conceitos que você usará com frequência:

-   Referências gerenciadas ([ **^ operator**](https://docs.microsoft.com/cpp/windows/handle-to-object-operator-hat-cpp-component-extensions)) and [**managed classes**](https://docs.microsoft.com/cpp/windows/classes-and-structs-cpp-component-extensions) (ref classes) são partes fundamentais do Windows Runtime. Você precisará usar classes de referência para interagir com componentes do Tempo de Execução do Windows, por exemplo [**IFrameworkView**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Core.IFrameworkView) (você verá mais sobre isso no passo a passo).
-   Ao trabalhar com interfaces do Direct3D 11 COM, use o tipo de modelo [**Microsoft::WRL::ComPtr**](https://docs.microsoft.com/cpp/windows/comptr-class) para deixar ponteiros COM mais fáceis de usar.

 

 




