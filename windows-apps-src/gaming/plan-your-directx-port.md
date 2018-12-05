---
title: Planejar a portabilidade do DirectX
description: Planeje o seu projeto de portabilidade para jogos do DirectX 9 para o DirectX 11 e Plataforma Universal do Windows (UWP) - atualize o código dos elementos gráficos e coloque o seu jogo no ambiente do Windows Runtime.
ms.assetid: 3c0c33ca-5d15-ae12-33f8-9b5d8da08155
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, directx, compatibilização
ms.localizationpriority: medium
ms.openlocfilehash: abbcd688df01b779a1cb3ab9e30bd13709926be4
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8696042"
---
# <a name="plan-your-directx-port"></a>Planejar a portabilidade do DirectX



**Resumo**

-   Planejar a portabilidade do DirectX
-   [Alterações importantes do Direct3D 9 para o Direct3D 11](understand-direct3d-11-1-concepts.md)
-   [Mapeamento de recursos](feature-mapping.md)


Planeje o seu projeto de portabilidade para jogos do DirectX 9 para o DirectX 11 e Plataforma Universal do Windows (UWP): atualize o código dos elementos gráficos e coloque o seu jogo no ambiente do Windows Runtime.

## <a name="plan-to-port-graphics-code"></a>Planejar para a compatibilização do código de elementos gráficos


Antes de você começar a compatibilizar o seu jogo com a UWP, é importante certificar que ele não tenha nenhum remanescente do Direct3D 8. Verifique se o jogo não tem resquícios do pipeline de funções fixas. Veja uma lista completa de recursos preteridos, incluindo funcionalidade de pipeline fixo, em [Recursos preteridos](https://msdn.microsoft.com/library/windows/desktop/cc308047).

A atualização do Direct3D 9 para o Direct3D 11 é mais do que uma alteração do tipo "localizar e substituir". Você deve saber a diferença entre o dispositivo Direct3D, o contexto de dispositivo e a infraestrutura gráfica. Além disso, deve aprender sobre outras mudanças importantes desde o Direct3D 9. Você pode iniciar esse processo lendo os outros tópicos desta seção.

Você deve substituir as livrarias auxiliares D3DX e DXUT pelas suas ou por ferramentas da comunidade. Veja a seção [Mapeamento de recursos](feature-mapping.md) para saber mais.

> **Observação**  você pode usar o [Kit de ferramentas do DirectX](http://go.microsoft.com/fwlink/p/?LinkID=248929) ou [DirectXTex](http://go.microsoft.com/fwlink/p/?LinkID=248926) para substituir uma funcionalidade que antes era fornecida pelo D3DX e DXUT.

 

Os sombreadores escritos em linguagem de assembly devem ser atualizados para HLSL usando a funcionalidade nível 9\_1 ou 9\_3 do modelo 4 de sombreador e os sombreadores escritos para a biblioteca de efeitos terão que ser atualizados para uma versão mais recente de sintaxe HLSL. Veja a seção [Mapeamento de recursos](feature-mapping.md) para saber mais.

Conheça os diferentes [níveis de recursos do Direct3D](https://msdn.microsoft.com/library/windows/desktop/ff476876). Os níveis de recursos classificam uma ampla gama de hardware de vídeo, definindo conjuntos de funcionalidades conhecidas. Cada conjunto corresponde aproximadamente às versões do Direct3D, de 9.1 a 11.2. Todos os níveis de recursos usam a API do DirectX 11.

## <a name="plan-to-port-win32-ui-code-to-corewindow"></a>Planeje a portabilidade do código da interface do usuário Win32 para CoreWindow


Aplicativos UWP são executados em uma janela criada por um contêiner de aplicativo, chamada [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). Seu jogo controle a janela herdando de [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478), que exige menos detalhes de implementação do que uma janela de área de trabalho. O loop principal do seu jogo estará no método [**IFrameworkView::Run**](https://msdn.microsoft.com/library/windows/apps/hh700505).

O ciclo de vida de um aplicativo UWP é muito diferente de um aplicativo de área de trabalho. Será necessário salvar o jogo frequentemente, porque quando o evento suspenso ocorre, seu aplicativo tem apenas um tempo limitado para parar de executar o código e você precisa garantir que o jogador possa voltar para onde estava assim que o aplicativo for retomado. Os jogos devem ser salvos com frequência suficiente para manter uma experiência de jogabilidade contínua a partir da retomada, mas não com tanta frequência a ponto de o processo influenciar na taxa de quadros ou fazer com que o jogo trave. Seu jogo precisará carregar potencialmente o estado de jogo quando for retomado a partir de um estado encerrado.

[DirectXMath](https://msdn.microsoft.com/library/windows/desktop/ee415571) pode ser usado como substituto de D3DXMath e XNAMath, e pode ser útil se você precisar de uma biblioteca matemática. DirectXMath possui tipos de dados rápidos e portáteis, além dos tipos alinhados e empacotados com sombreadores.

As bibliotecas nativas, como [API encaixada](https://msdn.microsoft.com/library/windows/desktop/dd405529), foram expandidas para dar suporte a intrínsecos ARM. Se o seu jogo usa APIs entrelaçadas, você pode continuar usando no DirectX 11 e na UWP.

Nossos modelos e exemplos de códigos usam recursos C++ com os quais você pode não estar familiarizado ainda. Por exemplo, métodos assíncronos são usados com [**expressões lambda**](https://msdn.microsoft.com/library/windows/apps/dd293608.aspx) para carregar recursos do Direct3D sem bloquear o thread de interface do usuário.

Há dois conceitos que você usará com frequência:

-   Referências gerenciadas ([**^ operator**](https://msdn.microsoft.com/library/windows/apps/yk97tc08.aspx)) and [**managed classes**](https://msdn.microsoft.com/library/windows/apps/6w96b5h7.aspx) (ref classes) são partes fundamentais do Windows Runtime. Você precisará usar classes de referência para interagir com componentes do Tempo de Execução do Windows, por exemplo [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478) (você verá mais sobre isso no passo a passo).
-   Ao trabalhar com interfaces do Direct3D 11 COM, use o tipo de modelo [**Microsoft::WRL::ComPtr**](https://msdn.microsoft.com/library/windows/apps/br244983.aspx) para deixar ponteiros COM mais fáceis de usar.

 

 




