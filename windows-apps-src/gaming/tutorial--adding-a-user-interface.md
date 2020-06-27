---
title: Adicionar uma interface do usuário
description: Saiba como adicionar uma sobreposição de interface de usuário 2D a um jogo do DirectX UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, interface do usuário, directx
ms.localizationpriority: medium
ms.openlocfilehash: b6b59bd4f42d31e1f29cc1af298199b42cce6781
ms.sourcegitcommit: 20969781aca50738792631f4b68326f9171a3980
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/26/2020
ms.locfileid: "85409535"
---
# <a name="add-a-user-interface"></a>Adicionar uma interface do usuário

> [!NOTE]
> Este tópico faz parte do [jogo criar um simples plataforma universal do Windows (UWP) com a série de tutoriais do DirectX](tutorial--create-your-first-uwp-directx-game.md) . O tópico nesse link define o contexto para a série.

Agora que nosso jogo tem seus visuais 3D em vigor, é hora de se concentrar em adicionar alguns elementos 2D para que o jogo possa fornecer comentários sobre o estado do jogo para o jogador. Isso pode ser feito com a adição de opções de menu simples e componentes de exibição de cabeçotes sobre a saída de pipeline de gráficos 3-D.

>[!Note]
>Se você não tiver baixado o código de jogo mais recente para este exemplo, vá para [jogo de exemplo do Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Usando o Direct2D, adicione vários comportamentos e elementos gráficos da interface do usuário ao nosso jogo UWP DirectX, incluindo:
- Exibição de cabeçotes, incluindo retângulos limitados do [controlador de aparência](tutorial--adding-controls.md)
- Menus de estado do jogo


## <a name="the-user-interface-overlay"></a>A sobreposição da interface do usuário


Embora haja muitas maneiras de exibir texto e elementos de interface do usuário em um jogo do DirectX, vamos nos concentrar em usar o [Direct2D](/windows/desktop/Direct2D/direct2d-portal). Também vamos usar [DirectWrite](/windows/desktop/DirectWrite/direct-write-portal) para os elementos Text.


Direct2D é um conjunto de APIs de desenho 2D usadas para desenhar primitivos e efeitos baseados em pixels. Ao começar com o Direct2D, é melhor manter as coisas simples. Layouts e comportamentos de interface complexos exigem tempo e planejamento. Se seu jogo exigir uma interface de usuário complexa, como as encontradas nos jogos de simulação e estratégia, considere usar XAML em vez disso.

> [!NOTE]
> Para obter informações sobre como desenvolver uma interface do usuário com XAML em um jogo do UWP DirectX, consulte [estendendo o jogo de exemplo](tutorial-resources.md).

O Direct2D não foi projetado especificamente para interfaces do usuário ou layouts como HTML e XAML. Ele não fornece componentes de interface do usuário, como listas, caixas ou botões. Ele também não fornece componentes de layout como divs, tabelas ou grades.


Para este jogo de exemplo, temos dois componentes principais da interface do usuário.
1. Uma exibição de cabeçotes para a pontuação e os controles no jogo.
2. Uma sobreposição usada para exibir as opções e o texto do estado do jogo, como informações de pausa e opções de início de nível.

### <a name="using-direct2d-for-a-heads-up-display"></a>Usando o Direct2D para um painel transparente

A imagem a seguir mostra a exibição de cabeçotes no jogo para o exemplo. É simples e desorganizado, permitindo que o jogador se concentre na navegação no mundo 3D e nos alvos de captura. Uma boa exibição de interface ou de cabeçotes deve nunca complicar a capacidade do jogador de processar e reagir aos eventos do jogo.

![uma captura de tela da sobreposição do jogo](images/simple-dx-game-ui-overlay.png)

A sobreposição consiste nos primitivos básicos a seguir.
- [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal) texto no canto superior direito que informa o jogador de 
    - Acertos bem-sucedidos
    - Número de capturas que o jogador fez
    - Tempo restante no nível
    - Número de nível atual 
- Dois segmentos de linha de interseção usados para formar uma cruz
- Dois retângulos nos cantos inferiores do controlador limites de [movimentação](tutorial--adding-controls.md) . 


O estado de exibição dos cabeçotes no jogo da sobreposição é desenhado no método [**GameHud:: render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) da classe [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) . Dentro desse método, a sobreposição de Direct2D que representa nossa interface do usuário é atualizada para refletir as alterações no número de ocorrências, tempo restante e número de nível.

Se o jogo tiver sido inicializado, adicionaremos `TotalHits()` , `TotalShots()` e `TimeRemaining()` a um buffer de [**swprintf_s**](/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) e especificaremos o formato de impressão. Em seguida, podemos desenhá-lo usando o método [**DrawText**](/windows/desktop/Direct2D/id2d1rendertarget-drawtext) . Fazemos o mesmo para o indicador de nível atual, desenhando números vazios para mostrar níveis não concluídos, como ➀, e números preenchidos como ➊ para mostrar que o nível específico foi concluído.


O trecho de código a seguir percorre o processo do método **GameHud:: render** para 
- Criando um bitmap usando [* * ID2D1RenderTarget::D rawbitmap * *](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- Seção desativando áreas da interface do usuário em retângulos usando [ **d2d1:: RectF**](/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- Usando **DrawText** para criar elementos de texto

```cppwinrt
void GameHud::Render(_In_ std::shared_ptr<Simple3DGame> const& game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.get(),
            D2D1::RectF(
                GameUIConstants::Margin,
                GameUIConstants::Margin,
                m_logoSize.width + GameUIConstants::Margin,
                m_logoSize.height + GameUIConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameUIConstants::Margin, GameUIConstants::Margin),
            m_titleHeaderLayout.get(),
            m_textBrush.get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameUIConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.get(),
            m_textBrush.get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static wchar_t wsbuffer[bufferLength];
        int length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"Hits:\t%10d\nShots:\t%10d\nTime:\t%8.1f",
            game->TotalHits(),
            game->TotalShots(),
            game->TimeRemaining()
            );

        // Draw the upper right portion of the HUD displaying total hits, shots, and time remaining
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBody.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3
                ),
            m_textBrush.get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32_t levelCharacter[6];
        for (uint32_t i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32_t>(game->LevelCompleted()) == i) ? 10 : 0);
        }
        length = swprintf_s(
            wsbuffer,
            bufferLength,
            L"%lc %lc %lc %lc %lc %lc",
            levelCharacter[0],
            levelCharacter[1],
            levelCharacter[2],
            levelCharacter[3],
            levelCharacter[4],
            levelCharacter[5]
            );
        // Create a new rectangle and draw the current level info text inside
        d2dContext->DrawText(
            wsbuffer,
            length,
            m_textFormatBodySymbol.get(),
            D2D1::RectF(
                windowBounds.Width - GameUIConstants::HudRightOffset,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 3 + GameUIConstants::Margin,
                windowBounds.Width,
                GameUIConstants::HudTopOffset + (GameUIConstants::HudBodyPointSize + GameUIConstants::Margin) * 4
                ),
            m_textBrush.get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            ...
            // Draw the crosshairs
            ...
        }
    }
}
```

Dividindo o método mais adiante, essa parte do método [**GameHud:: render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) desenha os retângulos de movimento e incêndio com [**ID2D1RenderTarget::D rawrectangle**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))e Cruz usando duas chamadas para [**ID2D1RenderTarget::D rawline**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline).

```cppwinrt
// Check if game is playing
if (game->IsActivePlay())
{
    // Draw a rectangle for the touch input for the move control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            0.0f,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            GameUIConstants::TouchRectangleSize,
            windowBounds.Height
            ),
        m_textBrush.get()
        );
    // Draw a rectangle for the touch input for the fire control.
    d2dContext->DrawRectangle(
        D2D1::RectF(
            windowBounds.Width - GameUIConstants::TouchRectangleSize,
            windowBounds.Height - GameUIConstants::TouchRectangleSize,
            windowBounds.Width,
            windowBounds.Height
            ),
        m_textBrush.get()
        );

    // Draw the cross hairs
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f - GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        D2D1::Point2F(windowBounds.Width / 2.0f + GameUIConstants::CrossHairHalfSize,
            windowBounds.Height / 2.0f),
        m_textBrush.get(),
        3.0f
        );
    d2dContext->DrawLine(
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f -
            GameUIConstants::CrossHairHalfSize),
        D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f +
            GameUIConstants::CrossHairHalfSize),
        m_textBrush.get(),
        3.0f
        );
}
```

No método **GameHud:: render** , armazenamos o tamanho lógico da janela do jogo na `windowBounds` variável. Isso usa o [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) método da classe **DeviceResources** . 

```cppwinrt
auto windowBounds = m_deviceResources->GetLogicalSize();
```

A obtenção do tamanho da janela do jogo é essencial para a programação da interface do usuário. O tamanho da janela é fornecido em uma medida chamada DIPs (pixels independentes de dispositivo), em que um DIP é definido como 1/96 de uma polegada. Direct2D dimensiona as unidades de desenho para os pixels reais quando o desenho ocorre, fazendo isso usando a configuração pontos por polegada (DPI) do Windows. Da mesma forma, ao desenhar texto usando [**DirectWrite**](/windows/desktop/DirectWrite/direct-write-portal), você especifica DIPs em vez de pontos para o tamanho da fonte. Os DIPs são expressos como números de ponto flutuante. 

### <a name="displaying-game-state-info"></a>Exibindo informações de estado do jogo

Além da exibição dos cabeçotes, o jogo de exemplo tem uma sobreposição que representa seis Estados de jogo. Todos os Estados apresentam um grande primitivo de retângulo preto com texto para o Player ler. Os retângulos do controlador de movimento e de cruz não são desenhados porque não estão ativos nesses Estados.

A sobreposição é criada usando a classe [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) , permitindo que você desative qual texto será exibido para alinhá-lo com o estado do jogo.

![status e ação de sobreposição](images/simple-dx-game-ui-finaloverlay.png)

A sobreposição é dividida em duas seções: **status** e **ação**. A divisão de **status** é dividida em retângulos de **título** e **corpo** . A seção de **ação** tem apenas um retângulo. Cada retângulo tem uma finalidade diferente.

-   `titleRectangle`contém o texto do título.
-   `bodyRectangle`contém o texto do corpo.
-   `actionRectangle`contém o texto que informa ao Player para executar uma ação específica.

O jogo tem seis Estados que podem ser definidos. O estado do jogo transmitido usando a parte de **status** da sobreposição. Os retângulos de **status** são atualizados usando vários métodos correspondentes aos Estados a seguir.

- Carregando
- Estatísticas de início inicial/pontuação alta
- Início do nível
- Jogo pausado
- Fim de jogo
- Jogo ganho


A parte de **ação** da sobreposição é atualizada usando o método [**GameInfoOverlay:: setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) , permitindo que o texto da ação seja definido como um dos seguintes.
- "Toque para reproduzir novamente..."
- "Carregando nível, aguarde..."
- "Toque para continuar..."
- Nenhum

> [!NOTE]
> Esses dois métodos serão discutidos mais detalhadamente na seção [representando o estado do jogo](#representing-game-state) .

Dependendo do que está acontecendo no jogo, os campos de texto da seção **status** e **ação** são ajustados.
Vejamos como inicializamos e desenhamos a sobreposição desses seis Estados.

### <a name="initializing-and-drawing-the-overlay"></a>Inicializando e desenhando a sobreposição

Os seis Estados de **status** têm algumas coisas em comum, tornando os recursos e os métodos de que eles precisam muito semelhantes.
    - Todos eles usam um retângulo preto no centro da tela como seu plano de fundo.
    - O texto exibido é **título** ou **corpo** de texto.
    - O texto usa a fonte Segoe UI e é desenhado na parte superior do retângulo de fundo. 


O jogo de exemplo tem quatro métodos que entram em jogo ao criar a sobreposição.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
O construtor [**GameInfoOverlay:: GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) Inicializa a sobreposição, mantendo a superfície de bitmap que usaremos para exibir informações para o Player em. O Construtor Obtém um alocador do objeto [**ID2D1Device**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) passado para ele, que ele usa para criar um [**ID2D1DeviceContext**](/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) ao qual o objeto de sobreposição pode ser desenhado. [IDWriteFactory:: CreateTextFormat](/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay:: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) é nosso método para criar pincéis que serão usados para desenhar nosso texto. Para fazer isso, obtemos um objeto [**ID2D1DeviceContext2**](/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) que permite a criação e o desenho de geometria, além de funcionalidades como a renderização de tinta e de malha de gradiente. Em seguida, criamos uma série de pincéis coloridos usando [**ID2D1SolidColorBrush**](/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) para desenhar os elementos da interface do usuário do folling.
- Pincel preto para planos de fundo de retângulo
- Pincel branco para texto de status
- Pincel laranja para texto de ação

#### <a name="deviceresourcessetdpi"></a>DeviceResources:: SetDpi

O método [**DeviceResources:: SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) define os pontos por polegada da janela. Esse método é chamado quando o DPI é alterado e precisa ser reajustado, o que acontece quando a janela do jogo é redimensionada. Depois de atualizar o DPI, esse método também chama[**DeviceResources:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) para garantir que os recursos necessários sejam recriados toda vez que a janela for redimensionada.

#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources

O método [**GameInfoOverlay:: CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) é o local em que todo o desenho ocorre. Veja a seguir uma descrição das etapas do método.
- Três retângulos são criados para a seção do texto da interface do usuário para o **título**, **corpo**e texto da **ação** .
    ```cppwinrt 
    m_titleRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        GameInfoOverlayConstant::TopMargin + GameInfoOverlayConstant::TitleHeight
        );
    m_actionRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        overlaySize.height - (GameInfoOverlayConstant::ActionHeight + GameInfoOverlayConstant::BottomMargin),
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        overlaySize.height - GameInfoOverlayConstant::BottomMargin
        );
    m_bodyRectangle = D2D1::RectF(
        GameInfoOverlayConstant::SideMargin,
        m_titleRectangle.bottom + GameInfoOverlayConstant::Separator,
        overlaySize.width - GameInfoOverlayConstant::SideMargin,
        m_actionRectangle.top - GameInfoOverlayConstant::Separator
        );
    ```

- Um bitmap é criado `m_levelBitmap` com o nome, levando o DPI atual em conta usando **CreateBitmap**.
- `m_levelBitmap`é definido como nosso destino de renderização 2D usando [**ID2D1DeviceContext:: SetTarget**](/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget).
- O bitmap é limpo com cada pixel que ficou preto usando [**ID2D1RenderTarget:: Clear**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear).
- [**ID2D1RenderTarget:: BeginDraw**](/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) é chamado para iniciar o desenho. 
- **DrawText** é chamado para desenhar o texto armazenado em `m_titleString` , `m_bodyString` e `m_actionString` no retângulo approperiate usando o **ID2D1SolidColorBrush**correspondente.
- [**ID2D1RenderTarget:: EndDraw**](ID2D1RenderTarget::EndDraw) é chamado para parar todas as operações de desenho em `m_levelBitmap` .
- Outro bitmap é criado usando **CreateBitmap** chamado `m_tooSmallBitmap` para usar como um fallback, mostrando somente se a configuração de exibição for muito pequena para o jogo.
- Repita o processo de desenho `m_levelBitmap` para `m_tooSmallBitmap` , desta vez apenas desenhando a cadeia `Paused` de caracteres no corpo.




Agora, tudo o que precisamos é de seis métodos para preencher o texto dos seis Estados de sobreposição!

### <a name="representing-game-state"></a>Representando o estado do jogo


Cada um dos seis Estados de sobreposição no jogo tem um método correspondente no objeto **GameInfoOverlay** . Esses métodos desenham uma variação da sobreposição para fornecer ao jogador informações explícitas sobre o próprio jogo. Essa comunicação é representada com um **título** e uma cadeia de caracteres de **corpo** . Como o exemplo já configurou os recursos e o layout dessas informações quando ele foi inicializado e com o método [**GameInfoOverlay:: CreateDeviceDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) , ele só precisa fornecer as cadeias de caracteres específicas do estado de sobreposição.

A parte de **status** da sobreposição é definida com uma chamada para um dos métodos a seguir.

Estado do jogo | Método de status definido | Campos de status
:----- | :------- | :---------
Carregando | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Título**</br>Carregando recursos </br>**Corpo**</br> Imprime incrementalmente "." para implicar o carregamento da atividade.
Estatísticas de início inicial/pontuação alta | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Título**</br>Pontuação alta</br> **Corpo**</br> Níveis concluídos # </br>Total de pontos #</br>Total de capturas #
Início do nível | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Título**</br>Geral #</br>**Corpo**</br>Descrição do objetivo de nível.
Jogo pausado | [GameInfoOverlay:: setpause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Título**</br>Jogo pausado</br>**Corpo**</br>Nenhum
Fim de jogo | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Título**</br>Fim de jogo</br> **Corpo**</br> Níveis concluídos # </br>Total de pontos #</br>Total de capturas #</br>Níveis concluídos #</br>Pontuação alta #
Jogo ganho | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Título**</br>Você ganhou!</br> **Corpo**</br> Níveis concluídos # </br>Total de pontos #</br>Total de capturas #</br>Níveis concluídos #</br>Pontuação alta #

Com o método [**GameInfoOverlay:: CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) , o exemplo declarou três áreas retangulares que correspondem a regiões específicas da sobreposição.

Tendo essas áreas em mente, examinaremos um dos métodos específicos do estado, **GameInfoOverlay::SetGameStats**, para ver como a sobreposição é desenhada.

```cppwinrt
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.get());
    m_titleString = L"High Score";

    d2dContext->DrawText(
        m_titleString.c_str(),
        m_titleString.size(),
        m_textFormatTitle.get(),
        m_titleRectangle,
        m_textBrush.get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = std::wstring(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString.c_str(),
        m_bodyString.size(),
        m_textFormatBody.get(),
        m_bodyRectangle,
        m_textBrush.get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device. All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call. At that point, the sample will recreate the device
        // and all associated resources. As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        winrt::check_hresult(hr);
    }
}
```

Usando o contexto do dispositivo Direct2D que o objeto **GameInfoOverlay** inicializou, esse método preenche os retângulos de título e corpo com preto usando o pincel de plano de fundo. Ele desenha o texto da cadeia de caracteres "High Score" no retângulo de título e uma cadeia de caracteres contendo as informações de estado do jogo atualizadas no retângulo de corpo de texto usando o pincel de texto branco.


O retângulo de ação é atualizado por uma chamada subsequente para [**GameInfoOverlay:: setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) a partir de um método no objeto **GameMain** , que fornece as informações de estado do jogo necessárias por **GameInfoOverlay:: setaction** para determinar a mensagem correta para o Player, como "toque para continuar".

A sobreposição de qualquer estado especificado é escolhida no método [**GameMain:: SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) da seguinte maneira:

```cppwinrt
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading(m_loadingCount);
        break;

    case GameInfoOverlayState::GameStats:
        m_uiControl->SetGameStats(
            m_game->HighScore().levelCompleted + 1,
            m_game->HighScore().totalHits,
            m_game->HighScore().totalShots
            );
        break;

    case GameInfoOverlayState::LevelStart:
        m_uiControl->SetLevelStart(
            m_game->LevelCompleted() + 1,
            m_game->CurrentLevel()->Objective(),
            m_game->CurrentLevel()->TimeLimit(),
            m_game->BonusTime()
            );
        break;

    case GameInfoOverlayState::GameOverCompleted:
        m_uiControl->SetGameOver(
            true,
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::GameOverExpired:
        m_uiControl->SetGameOver(
            false,
            m_game->LevelCompleted(),
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->HighScore().totalHits
            );
        break;

    case GameInfoOverlayState::Pause:
        m_uiControl->SetPause(
            m_game->LevelCompleted() + 1,
            m_game->TotalHits(),
            m_game->TotalShots(),
            m_game->TimeRemaining()
            );
        break;
    }
}
```

Agora o jogo tem uma maneira de comunicar informações de texto para o jogador com base no estado do jogo, e temos uma maneira de alternar o que é exibido em todo o jogo.

### <a name="next-steps"></a>Próximas etapas

No próximo tópico, [adicionando controles](tutorial--adding-controls.md), veremos como o jogador interage com o jogo de exemplo e como a entrada muda o estado do jogo.
