---
title: Alterações importantes do Direct3D 9 para o Direct3D 11
description: Este tópico explica as diferenças de alto nível entre o DirectX 9 e o DirectX 11.
ms.assetid: 35a9e388-b25e-2aac-0534-577b15dae364
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, direct3d 9, direct3d 11, alterações
ms.localizationpriority: medium
ms.openlocfilehash: e3e3ecfaee8a99522623ee6b021d8e3a2d78ab85
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367540"
---
# <a name="important-changes-from-direct3d-9-to-direct3d-11"></a>Alterações importantes do Direct3D 9 para o Direct3D 11



**Resumo**

-   [Planejar sua porta do DirectX](plan-your-directx-port.md)
-   Alterações importantes do Direct3D 9 para o Direct3D 11
-   [Mapeamento de recurso](feature-mapping.md)


Este tópico explica as diferenças de alto nível entre o DirectX 9 e o DirectX 11.

Basicamente, o Direct3D 11 é o mesmo tipo de API que o Direct3D 9: uma interface virtualizada de baixo nível no hardware gráfico. Ele ainda permite realizar operações de desenho de gráficos em diversas implementações de hardware. O layout da API gráfica mudou desde o Direct3D 9; o conceito de um contexto de dispositivo foi expandido, e a API foi adicionada especificamente para a infraestrutura gráfica. Os recursos armazenados no dispositivo Direct3D têm um novo mecanismo de polimorfismo de dados, chamado "exibição de recurso".

## <a name="core-api-functions"></a>Principais funções da API


No Direct3D 9, era necessário criar uma interface para a API do Direct3D antes de começar a usá-lo. Em seu jogo UWP (Plataforma Universal do Windows) Direct3D 11, você chama uma função estática chamada [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) para criar o dispositivo e seu respectivo contexto.

## <a name="devices-and-device-context"></a>Dispositivos e contextos de dispositivo


Um dispositivo Direct3D 11 representa um adaptador gráfico virtualizado. Ele é usado para criar recursos na memória de vídeo, por exemplo: carregar texturas na GPU; criar exibições em recursos de textura e cadeias de permuta; e gerar amostras de texturas. Veja uma lista completa de usos da interface de dispositivos Direct3D 11 em [**ID3D11Device**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11device) e [**ID3D11Device1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11device1).

Um contexto de dispositivo Direct3D 11 é usado para definir o estado do pipeline e gerar comandos de renderização. Por exemplo, uma cadeia de renderização do Direct3D 11 usa um contexto de dispositivo para se configurar e desenhar a cena (consulte abaixo). O contexto de dispositivo é usado para acessar (mapear) a memória de vídeo usada por recursos do dispositivo Direct3D; ele também é usado para atualizar dados de sub-recursos, como dados de buffer constante. Veja uma lista completa de usos da interface de dispositivos Direct3D 11 em [**ID3D11DeviceContext**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicecontext) e [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1). Lembre-se de que a maioria dos exemplos usa um contexto imediato para realizar a renderização diretamente no dispositivo, mas o Direct3D 11 também dá suporte a contextos de dispositivo adiados, que são usados principalmente para multithreading.

No Direct3D 11, o identificador do dispositivo e o identificador de contexto de dispositivo são obtidos chamando [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice). É nesse método também que você pode solicitar um conjunto específico de recursos de hardware e recuperar informações sobre níveis de recursos do Direct3D compatíveis com o adaptador gráfico. Consulte a [introdução a um dispositivo em Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-intro) para saber mais sobre dispositivos, contextos de dispositivo e threading.

## <a name="device-infrastructure-frame-buffers-and-render-target-views"></a>Infraestrutura do dispositivo, buffers de quadros e exibições de destinos de renderização


No Direct3D 11, o adaptador do dispositivo e a configuração de hardware são definidos com a API DXGI (DirectX Graphics Infrastructure), usando as interfaces COM [**IDXGIAdapter**](https://docs.microsoft.com/windows/desktop/api/dxgi/nn-dxgi-idxgiadapter) e [**IDXGIDevice1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgidevice2). Os buffers e outros recursos de janela (visíveis ou fora da tela) são criados e configurados por interfaces DXGI específicas; a implementação de padrões de fábrica [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) obtém recursos da DXGI, como o buffer de quadros. Como a cadeia de troca pertence à DXGI, uma interface DXGI é usada para apresentar os quadros na tela (veja [**IDXGISwapChain1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgiswapchain1)).

Use [**IDXGIFactory2**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nn-dxgi1_2-idxgifactory2) para criar uma cadeia de troca compatível com seu jogo. Você deve criar uma cadeia de troca para uma janela principal ou para composição (interoperabilidade XAML), em vez de criar uma cadeia de troca para HWND.

## <a name="device-resources-and-resource-views"></a>Recursos/exibições de recursos do dispositivo


O Direct3D 11 oferece suporte a um nível adicional de polimorfismo sobre recursos de memória de vídeo; ele é conhecido como "exibição". Basicamente, onde havia um único objeto Direct3D 9 para uma textura, agora há dois objetos: o recurso da textura, que contém os dados, e a exibição de recurso, que indica como a exibição é usada para renderização. Uma exibição baseada em um recurso permite que este seja usado para um fim específico. Por exemplo, um recurso de textura 2D é criado como [**ID3D11Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d); em seguida, uma exibição de recurso de sombreador ([**ID3D11ShaderResourceView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11shaderresourceview)) é criada nele para que possa ser usada como textura em um sombreador. Uma exibição de destino de renderização ([**ID3D11RenderTargetView**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview)) também pode ser criada no mesmo recurso de textura 2D para utilização como superfície de desenho. Em outro exemplo, os mesmos dados de pixel são representados em dois formatos diferentes, usando duas exibições separadas em apenas um recurso de textura.

O recurso subjacente deve ser criado com propriedades compatíveis com o tipo das exibições criadas a partir dele. Por exemplo, se um [ **ID3D11RenderTargetView** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11rendertargetview) é aplicado a uma superfície, que são exibidos é criado com o [ **D3D11\_ASSOCIAR\_RENDERIZAÇÃO\_ DESTINO** ](https://docs.microsoft.com/windows/desktop/api/d3d11/ne-d3d11-d3d11_bind_flag) sinalizador. A superfície também deve ter um formato de superfície DXGI compatível com o processamento (consulte [ **DXGI\_formato**](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format)).

A maioria dos recursos usados para renderização é herdada da interface [**ID3D11Resource**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11resource), que, por sua vez, é herdada de [**ID3D11DeviceChild**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild). Os buffers de vértices/índice/constantes e os sombreadores são recursos do Direct3D 11. Os layouts de entrada e os estados de exemplo são herdados diretamente de [**ID3D11DeviceChild**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11devicechild).

Modos de exibição de recursos usam um DXGI\_valor de enumeração de formato para indicar o formato de pixel. Nem toda D3DFMT tem suporte como um DXGI\_formato. Por exemplo, há nenhum formato RGB 24bpp em DXGI é equivalente a D3DFMT\_R8G8B8. Também não há equivalentes BGR para todos os formatos de RGB (DXGI\_formato\_R10G10B10A2\_UNORM é equivalente a D3DFMT\_A2B10G10R10, mas não há nenhum equivalente direto ao D3DFMT\_A2R10G10B10). Você deve planejar a conversão todo conteúdo nesses formatos herdados em formatos compatíveis no momento da compilação. Para uma lista completa de DXGI formatos Consulte os [ **DXGI\_formato** ](https://docs.microsoft.com/windows/desktop/api/dxgiformat/ne-dxgiformat-dxgi_format) enumeração.

Os recursos de dispositivos Direct3D (e respectivas exibições) são criados antes da renderização da cena. Os contextos de dispositivo são usados para configurar a cadeia de renderização (consulte a explicação a seguir).

## <a name="device-context-and-the-rendering-chain"></a>Contexto de dispositivo e cadeia de renderização


No Direct3D 9 e no Direct3D 10.x, havia um único objeto de dispositivo Direct3D que gerenciava a criação, o estado e o desenho de recursos. No Direct3D 11, a interface do dispositivo Direct3D continua a gerenciar a criação de recursos, mas todas as operações relativas a desenho e ao estado são executadas usando um contexto de dispositivo Direct3D. Veja um exemplo de como o contexto de dispositivo (interface [**ID3D11DeviceContext1**](https://docs.microsoft.com/windows/desktop/api/d3d11_1/nn-d3d11_1-id3d11devicecontext1)) é usado para configurar a cadeia de renderização:

-   Definir e limpar as exibições de destinos de renderização (e a exibição do estêncil de profundidade)
-   Define os buffers de vértice/índice e o layout de entrada para o estágio do assembler de entrada (estágio IA).
-   Vincule os sombreadores de vértice e pixel ao pipeline
-   Vincule os buffers constantes aos sombreadores
-   Vincular as amostras e exibições de texturas ao sombreador de pixel
-   Desenhar a cena

Quando um dos métodos [**ID3D11DeviceContext::Draw**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11devicecontext-draw) é chamado, a cena é desenhada na exibição do destino de renderização. Quando você conclui o desenho, o adaptador DXGI é usado para apresentar o quadro concluído, chamando [**IDXGISwapChain1::Present1**](https://docs.microsoft.com/windows/desktop/api/dxgi1_2/nf-dxgi1_2-idxgiswapchain1-present1).

## <a name="state-management"></a>Gerenciamento de estado


O Direct3D 9 gerenciava configurações de estado com um grande conjunto de alternâncias individuais, definidas com os métodos SetRenderState, SetSamplerState e SetTextureStageState. Como o Direct3D 11 não oferece suporte ao pipeline de funções fixas herdadas, o método SetTextureStageState é substituído pela gravação de sombreadores de pixels (PS). Não há equivalente a um bloco de estado do Direct3D 9. Em vez disso, o Direct3D 11 gerencia o estado usando quatro tipos de objetos, que permitem agrupar o estado de renderização de modo mais simples.

Por exemplo, em vez de usar SetRenderState com D3DRS\_ZENABLE, você cria um objeto de DepthStencilState com este e outros relacionados configurações de estado e usá-lo para alterar o estado durante a renderização.

Ao realizar a portabilidade de aplicativos do Direct3D 9 para objetos de estado, lembre-se de que as diversas combinações de estado são representadas como objetos imutáveis. Eles devem ser criados uma vez e reutilizados enquanto forem válidos.

## <a name="direct3d-feature-levels"></a>Níveis de recursos do Direct3D


O Direct3D tem um novo mecanismo chamado "níveis de recursos", que determina o suporte a hardware. Os níveis de recursos simplificam a tarefa de descobrir o que o adaptador gráfico pode fazer, permitindo que você solicite um conjunto bem definido de funcionalidades de GPU. Por exemplo, o 9\_implementa de nível 1 recurso a funcionalidade fornecida por adaptadores de gráficos do Direct3D 9, incluindo o sombreador de modelo 2. x. Desde 9\_1 é o menor nível de recurso, você pode esperar que todos os dispositivos para dar suporte a sombreador de vértice e um sombreador de pixel, que eram os estágios mesmos compatíveis com o modelo de sombreador programável do Direct3D 9.

Seu jogo usará [**D3D11CreateDevice**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) para criar o dispositivo Direct3D e o respectivo contexto. Ao chamar essa função, você fornece uma lista de níveis de recursos compatíveis com o jogo. Ela obterá na lista o nível de recurso mais alto com suporte. Por exemplo, se o seu jogo pode usar texturas BC4/BC5 (um recurso de hardware do DirectX 10), inclua pelo menos 9\_1 e 10\_0 na lista de níveis de recursos com suporte. Se o jogo está em execução no hardware do DirectX 9 e texturas BC4/BC5 não podem ser usadas, [ **D3D11CreateDevice** ](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-d3d11createdevice) retornará 9\_1. Seu jogo poderá retornar para um formato de textura diferente (e texturas menores).

Caso decida expandir o jogo em Direct3D 9 de modo a oferecer níveis de recursos Direct3D maiores, termine de realizar a portabilidade do código gráfico do Direct3D 9 primeiro. Depois de fazer o jogo funcionar no Direct3D 11, será mais fácil adicionar mais caminhos de renderização com gráficos aprimorados.

Consulte uma explicação detalhada do suporte a níveis de recursos no tópico sobre [níveis de recursos do Direct3D](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-devices-downlevel-intro). Consulte [Recursos do Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-features) e [Recursos do Direct3D 11.1](https://docs.microsoft.com/windows/desktop/direct3d11/direct3d-11-1-features) para obter uma lista completa de recursos do Direct3D 11.

## <a name="feature-levels-and-the-programmable-pipeline"></a>Níveis de recursos e pipeline programável


O hardware continua a evoluir desde o Direct3D 9, e muitos novos estágios opcionais foram adicionados ao pipeline de elementos gráficos programável. O conjunto de opções disponível para o pipeline gráfico varia com o nível de recursos do Direct3D. O nível de recursos 10.0 inclui o estágio do sombreador de geometria, com transmissão opcional para fora, para renderização com passagem múltipla na GPU. 11 de nível de recurso\_0 incluem o sombreador hull e a sombreador de domínio para uso com o mosaico de hardware. 11 de nível de recurso\_0 também inclui suporte completo para sombreadores do DirectCompute, embora o suportam a níveis de recurso 10.x incluem apenas para uma forma limitada de DirectCompute.

Todos os sombreadores são escritos em HLSL usando um perfil que corresponde a um nível de recursos do Direct3D. Perfis de sombreador são para cima compatíveis, portanto, um sombreador HLSL que compila usando o vs\_4\_0\_nível\_9\_1 ou ps\_4\_0\_nível\_9\_1 funcionará em todos os dispositivos. Perfis do sombreador não são compatível, de nível inferior, portanto, um sombreador compilado usando o vs\_4\_1 funcionará apenas no nível de recurso 10\_1, 11\_0 ou 11\_1 dispositivos.

O Direct3D 9 gerenciava as restrições de sombreadores usando uma matriz compartilhada com SetVertexShaderConstant e SetPixelShaderConstant. O Direct3D 11 usa buffers constantes, que são recursos como um buffer de vértice ou índice. Os buffers constantes são projetados para atualização eficiente. Em vez de as constantes do sombreador serem organizadas em uma matriz global, você deve organizá-las em agrupamentos lógicos e gerenciá-las por meio de um ou mais buffers constantes. Ao fazer a portabilidade de um jogo do Direct3D 9 para Direct3D 11, planeje a organização dos buffers constantes de modo a atualizá-los corretamente. Por exemplo, agrupe constantes do sombreador que não sejam atualizadas a cada quadro e um buffer constante separado, para que você não precise carregar constantemente esses dados no adaptador gráfico juntamente com constantes mais dinâmicas do sombreador.

> **Observação**    aplicativos mais Direct3D 9 faz uso extensivo de sombreadores, mas ocasionalmente misto no uso do comportamento herdado da função fixa. O Direct3D 11 usa apenas um modelo de sombreamento programável. Os recursos de funções fixas herdadas do Direct3D 9 foram preteridos.

 

 

 




