---
title: Adicionar uma interface do usuário
description: Saiba como adicionar uma sobreposição de interface do usuário 2D para um jogo UWP do DirectX.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, interface do usuário, directx
ms.localizationpriority: medium
ms.openlocfilehash: ef966901534302c505ddad37bd277d9141b512a1
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66367858"
---
# <a name="add-a-user-interface"></a>Adicionar uma interface do usuário


Agora que nosso jogo tem seus visuais 3D em vigor, é hora de se concentrar na adição de alguns elementos 2D para que o jogo pode fornecer comentários sobre o estado do jogo para o player. Isso pode ser feito pela adição de opções de menu simples e saída do pipeline de componentes de exibição heads-up na parte superior do gráfico 3D.

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Usando o Direct2D, adicione um número de elementos gráficos de interface do usuário e os comportamentos ao nosso incluindo jogos do DirectX UWP:
- Exibir Heads-Up, inclusive [mover a consulta controlador](tutorial--adding-controls.md) retângulos de limite
- Menus de estado do jogo


## <a name="the-user-interface-overlay"></a>A sobreposição da interface do usuário


Embora haja várias maneiras de exibir os elementos da interface do usuário e o texto em um jogo do DirectX, vamos nos concentrar em usando [Direct2D](https://docs.microsoft.com/windows/desktop/Direct2D/direct2d-portal). Também utilizaremos [DirectWrite](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) para os elementos de texto.


Direct2D é que um conjunto de APIs de desenho 2D é usado para desenhar os efeitos e primitivos de pixel. Ao começar a usar o Direct2D, é melhor manter as coisas simples. Layouts e comportamentos de interface complexos exigem tempo e planejamento. Se o seu jogo requer uma interface de usuário complexas, como as encontradas no simulação e jogos de estratégia, considere usar o XAML em vez disso.

> [!NOTE]
> Para obter informações sobre como desenvolver uma interface do usuário com XAML em um jogo UWP DirectX, consulte [estendendo o exemplo do jogo](tutorial-resources.md).

Direct2D não é projetado especificamente para interfaces do usuário ou layouts como HTML e XAML. Ele não fornece componentes de interface do usuário, como botões, caixas ou listas. Ele também não fornece componentes de layout como divs, tabelas ou grades.


Para este exemplo do jogo, temos dois componentes principais da interface do usuário.
1. Uma exibição de alerta para os controles de jogo e pontuação.
2. Uma sobreposição é usado para exibir o texto do estado do jogo e opções, como informações de pausa e opções de inicialização do nível.

### <a name="using-direct2d-for-a-heads-up-display"></a>Usando o Direct2D para um painel transparente

A imagem a seguir mostra a exibição de alerta para o exemplo do jogo. É simples e organizado, permitindo que o jogador para se concentrar em navegando o mundo 3D e destinos de solução. Uma interface boa ou a exibição Heads-Up nunca deve complicar a capacidade do player para processar e reagir aos eventos no jogo.

![uma captura de tela da sobreposição do jogo](images/simple-dx-game-ui-overlay.png)

A sobreposição consiste nos seguintes primitivos básicos.
- [**O DirectWrite** ](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal) texto no canto superior direito que informa o player de 
    - Acertos bem-sucedida
    - Número de capturas que fez o player
    - Tempo restante no nível
    - Número do nível atual 
- Dois segmentos de linha usados para formar uma cruz de interseção
- Dois retângulos nos cantos inferior para o [mover a consulta controlador](tutorial--adding-controls.md) limites. 


O estado do jogo HUD da sobreposição é desenhado na [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) método da [ **GameHud** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) classe. Dentro desse método, a sobreposição do Direct2D que representa a nossa interface do usuário é atualizada para refletir as alterações no número de ocorrências, número de nível e restantes do tempo.

Se o jogo foi inicializado, adicionamos `TotalHits()`, `TotalShots()`, e `TimeRemaining()` para um [ **swprintf_s** ](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) armazenar em buffer e especificar o formato de impressão. Em seguida, podemos pode desenhá-lo usando o [ **DrawText** ](https://docs.microsoft.com/windows/desktop/Direct2D/id2d1rendertarget-drawtext) método. Vamos fazer o mesmo para o indicador de nível atual de desenho vazios números para Mostrar níveis não concluídos como ➀ e preenchidos números como ➊ para mostrar que o nível específico foi concluído.


O trecho de código a seguir explica os **GameHud::Render** processo do método para 
- Criar um Bitmap usando [* * ID2D1RenderTarget::DrawBitmap * *](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawbitmap(id2d1bitmap_constd2d1_rect_f__float_d2d1_bitmap_interpolation_mode_constd2d1_rect_f_))
- Seccionamento desativar áreas da interface do usuário em retângulos usando [ **D2D1::RectF**](https://docs.microsoft.com/windows/desktop/api/dcommon/ns-dcommon-d2d_rect_f)
- Usando o **DrawText** para tornar os elementos de texto

```cpp
void GameHud::Render(_In_ Simple3DGame^ game)
{
    auto d2dContext = m_deviceResources->GetD2DDeviceContext();
    auto windowBounds = m_deviceResources->GetLogicalSize();

    if (m_showTitle)
    {
        d2dContext->DrawBitmap(
            m_logoBitmap.Get(),
            D2D1::RectF(
                GameConstants::Margin,
                GameConstants::Margin,
                m_logoSize.width + GameConstants::Margin,
                m_logoSize.height + GameConstants::Margin
                )
            );
        d2dContext->DrawTextLayout(
            Point2F(m_logoSize.width + 2.0f * GameConstants::Margin, GameConstants::Margin),
            m_titleHeaderLayout.Get(),
            m_textBrush.Get()
            );
        d2dContext->DrawTextLayout(
            Point2F(GameConstants::Margin, m_titleBodyVerticalOffset),
            m_titleBodyLayout.Get(),
            m_textBrush.Get()
            );
    }

    // Draw text for number of hits, total shots, and time remaining
    if (game != nullptr)
    {
        // This section is only used after the game state has been initialized.
        static const int bufferLength = 256;
        static char16 wsbuffer[bufferLength];
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
            m_textFormatBody.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3
                ),
            m_textBrush.Get()
            );

        // Using the unicode characters starting at 0x2780 ( ➀ ) for the consecutive levels of the game.
        // For completed levels start with 0x278A ( ➊ ) (This is 0x2780 + 10).
        uint32 levelCharacter[6];
        for (uint32 i = 0; i < 6; i++)
        {
            levelCharacter[i] = 0x2780 + i + ((static_cast<uint32>(game->LevelCompleted()) == i) ? 10 : 0);
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
            m_textFormatBodySymbol.Get(),
            D2D1::RectF(
                windowBounds.Width - GameConstants::HudRightOffset,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize + GameConstants::Margin) * 3 + GameConstants::Margin,
                windowBounds.Width,
                GameConstants::HudTopOffset + (GameConstants::HudBodyPointSize+ GameConstants::Margin) * 4
                ),
            m_textBrush.Get()
            );

        if (game->IsActivePlay())
        {
            // Draw the move and fire rectangles
            // Draw the crosshairs
        }
    }
}
```

Quebrar o método de busca Além disso, essa informação a [ **GameHud::Render** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) método desenha nossa mudança e acionar retângulos com [ **ID2D1RenderTarget::DrawRectangle** ](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawrectangle(constd2d1_rect_f__id2d1brush_float_id2d1strokestyle))e uma cruz usando duas chamadas para [ **ID2D1RenderTarget::DrawLine**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-drawline).

```cpp
        // Check if game is playing
        if (game->IsActivePlay())
        {
            // Draw a rectangle for the touch input for the move control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    0.0f,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    GameConstants::TouchRectangleSize,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );
            // Draw a rectangle for the touch input of the fire control.
            d2dContext->DrawRectangle(
                D2D1::RectF(
                    windowBounds.Width - GameConstants::TouchRectangleSize,
                    windowBounds.Height - GameConstants::TouchRectangleSize,
                    windowBounds.Width,
                    windowBounds.Height
                    ),
                m_textBrush.Get()
                );

            // Draw two lines to form crosshairs
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f - GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                D2D1::Point2F(windowBounds.Width / 2.0f + GameConstants::CrossHairHalfSize, windowBounds.Height / 2.0f),
                m_textBrush.Get(),
                3.0f
                );
            d2dContext->DrawLine(
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f - GameConstants::CrossHairHalfSize),
                D2D1::Point2F(windowBounds.Width / 2.0f, windowBounds.Height / 2.0f + GameConstants::CrossHairHalfSize),
                m_textBrush.Get(),
                3.0f
                );
        }
```

No **GameHud::Render** método armazenamos o tamanho lógico da janela do jogo no `windowBounds` variável. Isso usa o [ `GetLogicalSize` ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) método da **DeviceResources** classe. 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Obter o tamanho da janela do jogo é essencial para a programação da interface do usuário. O tamanho da janela é fornecido em uma medida chamada DIPs (pixels independentes de dispositivo), onde um DIP é definido como 1/96 de polegada. Direct2D dimensiona as unidades de desenho para pixels reais quando ocorre o desenho, fazer isso usando os Windows configuração pontos por polegada (DPI). Da mesma forma, ao desenhar texto usando [ **DirectWrite**](https://docs.microsoft.com/windows/desktop/DirectWrite/direct-write-portal), especifique quedas em vez de pontos para o tamanho da fonte. Os DIPs são expressos como números de ponto flutuante.

 

### <a name="displaying-game-state-info"></a>Exibindo as informações de estado do jogo

Além da exibição de alerta, o exemplo do jogo tem uma sobreposição que representa os seis estados de jogos. Todos os estados de recursos um primitivo de retângulo preto grandes com o texto para o player ler. Os retângulos de controlador de mover a consulta e uma cruz não é desenhada porque eles não estão ativos nesses estados.

A sobreposição é criada usando o [ **GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) classe, permitindo-nos alternar o texto que é exibido para alinhar com o estado do jogo.

![status e a ação de sobreposição](images/simple-dx-game-ui-finaloverlay.png)

A sobreposição é dividida em duas seções: **Status** e **ação**. O **Status** secton é dividido **título** e **corpo** retângulos. O **ação** seção tem apenas um retângulo. Cada retângulo tem uma finalidade diferente.

-   `titleRectangle` contém o texto do título.
-   `bodyRectangle` contém o texto do corpo.
-   `actionRectangle` contém o texto que informa o player para tomar uma ação específica.

O jogo tem seis estados que podem ser definidos. O estado do jogo concedido usando o **Status** parte da sobreposição. O **Status** retângulos são atualizados usando uma série de métodos correspondentes com os estados a seguir.

- Carregando
- Estatísticas de pontuação alta/início inicial
- Início de nível
- Jogo em pausa
- Fim de jogo
- Jogo ganho


O **ação** parte da sobreposição é atualizado usando o [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) método, permitindo que o texto de ação a ser definido como um dos seguintes.
- "Toque para reproduzir novamente..."
- "Carregamento de nível, aguarde..."
- "Toque para continuar..."
- Nenhuma

> [!NOTE]
> Ambos os métodos serão discutidos mais detalhadamente no [que representa o estado do jogo](#representing-game-state) seção.

Dependendo do que está acontecendo no jogo, o **Status** e **ação** campos de texto de seção são ajustados.
Vejamos como podemos inicializar e desenhar a sobreposição para esses seis estados.

### <a name="initializing-and-drawing-the-overlay"></a>Inicializando e desenhando a sobreposição

Os seis **Status** estados têm algumas coisas em comum, tornando os recursos e métodos eles precisam de muito semelhantes.
    - Todos eles usam um retângulo preto no centro da tela como seu plano de fundo.
    - O texto exibido seja **Title** ou **corpo** texto.
    - O texto usa a fonte Segoe UI e é desenhado na parte superior do retângulo de back. 


O exemplo do jogo tem quatro métodos que entram em jogo ao criar a sobreposição.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
O [ **GameInfoOverlay::GameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) construtor inicializa a sobreposição, mantendo a superfície de bitmap que usaremos para exibir informações para o player no. O construtor obtém uma fábrica do [ **ID2D1Device** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1device) objeto passado a ele, ele usa para criar um [ **ID2D1DeviceContext** ](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nn-d2d1_1-id2d1devicecontext) Se o próprio objeto de sobreposição pode desenhar. [IDWriteFactory::CreateTextFormat](https://docs.microsoft.com/windows/desktop/api/dwrite/nf-dwrite-idwritefactory-createtextformat) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>GameInfoOverlay::CreateDeviceDependentResources
[**GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) é o nosso método para criação de pincéis que será usado para desenhar o nosso texto. Para fazer isso, obtemos uma [ **ID2D1DeviceContext2** ](https://docs.microsoft.com/windows/desktop/api/d2d1_3/nn-d2d1_3-id2d1devicecontext2) object que permite a criação e o desenho de geometria, além de funcionalidades, como reconhecimento de gradiente de renderização de malha. Em seguida, criamos uma série de pincéis coloridos usando [ **ID2D1SolidColorBrush** ](https://docs.microsoft.com/windows/desktop/api/d2d1/nn-d2d1-id2d1solidcolorbrush) para desenhar os elementos de interface do usuário folling.
- Pincel preto para planos de fundo do retângulo
- Pincel de branco para texto de status
- Pincel de laranja para texto de ação

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
O [ **DeviceResources::SetDpi** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) método define os pontos por polegada da janela. Esse método é chamado quando o DPI é alterado e precisa ser reajustados que ocorre quando a janela do jogo é redimensionada. Depois de atualizar o DPI, este método também chama[**DeviceResources::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) para tornar claro que é necessário recursos são recriados sempre que a janela é redimensionada.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
O [ **GameInfoOverlay::CreateWindowsSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) método é onde todos os nossos desenho ocorre. A seguir está uma descrição das etapas do método.
- Três retângulos são criados para a seção desativar o texto da interface do usuário para o **Title**, **corpo**, e **ação** texto.
    ```cpp 
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

- Um Bitmap é criado e denominado `m_levelBitmap`, considerando o DPI atual conta usando **CreateBitmap**.
- `m_levelBitmap` é definido como o nosso destino de renderização 2D usando [ **ID2D1DeviceContext::SetTarget**](https://docs.microsoft.com/windows/desktop/api/d2d1_1/nf-d2d1_1-id2d1devicecontext-settarget).
- O Bitmap é limpo com cada pixel feita preto usando [ **ID2D1RenderTarget::Clear**](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-clear).
- [**ID2D1RenderTarget::BeginDraw** ](https://docs.microsoft.com/windows/desktop/api/d2d1/nf-d2d1-id2d1rendertarget-begindraw) é chamado para iniciar o desenho. 
- **DrawText** é chamado para desenhar o texto armazenado no `m_titleString`, `m_bodyString`, e `m_actionString` no retângulo approperiate usando correspondente **ID2D1SolidColorBrush**.
- [**ID2D1RenderTarget::EndDraw** ](ID2D1RenderTarget::EndDraw) é chamado para interromper todas as operações de desenho no `m_levelBitmap`.
- Outro Bitmap é criado usando **CreateBitmap** denominado `m_tooSmallBitmap` para usar como um fallback, mostrando apenas se a configuração de exibição é muito pequena para o jogo.
- Repita o processo para desenhar na `m_levelBitmap` para `m_tooSmallBitmap`, desta vez, somente a cadeia de caracteres de desenho `Paused` no corpo.




Agora, tudo que precisamos são seis métodos para preencher o texto dos Estados de nossos seis sobreposição!

### <a name="representing-game-state"></a>Que representa o estado do jogo


Cada um dos Estados de seis sobreposição no jogo tem um método correspondente na **GameInfoOverlay** objeto. Esses métodos desenham uma variação da sobreposição para fornecer ao jogador informações explícitas sobre o próprio jogo. Essa comunicação é representada com uma **Title** e **corpo** cadeia de caracteres. Como o exemplo já configurado os recursos e o layout para essas informações quando ele foi inicializado e com o [ **GameInfoOverlay::CreateDeviceDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) método, ele só precisa fornecer as cadeias de caracteres de estado específico de sobreposição.

O **Status** parte da sobreposição é definida com uma chamada para um dos métodos a seguir.

Estado do jogo | Método set do status | Campos de status
:----- | :------- | :---------
Carregando | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Título**</br>Carregamento de recursos </br>**Corpo**</br> Imprime incrementalmente "." significa a atividade de carregamento.
Estatísticas de pontuação alta/início inicial | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Título**</br>Pontuação alta</br> **Corpo**</br> Níveis concluída # </br>Total de pontos de #</br># As tomadas total
Início de nível | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Título**</br>N º de nível</br>**Corpo**</br>Descrição de objetivo de nível.
Jogo em pausa | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Título**</br>Jogo em pausa</br>**Corpo**</br>Nenhuma
Fim de jogo | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Título**</br>Fim de jogo</br> **Corpo**</br> Níveis concluída # </br>Total de pontos de #</br># As tomadas total</br>Níveis concluída #</br>Alta pontuação #
Jogo ganho | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Título**</br>Você GANHOU!</br> **Corpo**</br> Níveis concluída # </br>Total de pontos de #</br># As tomadas total</br>Níveis concluída #</br>Alta pontuação #




Com o [ **GameInfoOverlay::CreateWindowSizeDependentResources** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) método, o exemplo declarado três áreas retangulares que correspondem a regiões específicas da sobreposição.



Tendo essas áreas em mente, examinaremos um dos métodos específicos do estado, **GameInfoOverlay::SetGameStats**, para ver como a sobreposição é desenhada.

```cpp
void GameInfoOverlay::SetGameStats(int maxLevel, int hitCount, int shotCount)
{
    int length;

    auto d2dContext = m_deviceResources->GetD2DDeviceContext();

    d2dContext->SetTarget(m_levelBitmap.Get());
    d2dContext->BeginDraw();
    d2dContext->SetTransform(D2D1::Matrix3x2F::Identity());
    d2dContext->FillRectangle(&m_titleRectangle, m_backgroundBrush.Get());
    d2dContext->FillRectangle(&m_bodyRectangle, m_backgroundBrush.Get());
    m_titleString = "High Score";

    d2dContext->DrawText(
        m_titleString->Data(),
        m_titleString->Length(),
        m_textFormatTitle.Get(),
        m_titleRectangle,
        m_textBrush.Get()
        );
    length = swprintf_s(
        wsbuffer,
        bufferLength,
        L"Levels Completed %d\nTotal Points %d\nTotal Shots %d",
        maxLevel,
        hitCount,
        shotCount
        );
    m_bodyString = ref new Platform::String(wsbuffer, length);
    d2dContext->DrawText(
        m_bodyString->Data(),
        m_bodyString->Length(),
        m_textFormatBody.Get(),
        m_bodyRectangle,
        m_textBrush.Get()
        );

    // We ignore D2DERR_RECREATE_TARGET here. This error indicates that the device
    // is lost. It will be handled during the next call to Present.
    HRESULT hr = d2dContext->EndDraw();
    if (hr != D2DERR_RECREATE_TARGET)
    {
        // The D2DERR_RECREATE_TARGET indicates there has been a problem with the underlying
        // D3D device.  All subsequent rendering will be ignored until the device is recreated.
        // This error will be propagated and the appropriate D3D error will be returned from the
        // swapchain->Present(...) call.   At that point, the sample will recreate the device
        // and all associated resources.  As a result, the D2DERR_RECREATE_TARGET doesn't
        // need to be handled here.
        DX::ThrowIfFailed(hr);
    }
}
```

Usando o contexto de dispositivo Direct2D que o **GameInfoOverlay** objeto inicializado, esse método preenche os retângulos de título e o corpo com preto usando o pincel do plano de fundo. Ele desenha o texto da cadeia de caracteres "High Score" no retângulo de título e uma cadeia de caracteres contendo as informações de estado do jogo atualizadas no retângulo de corpo de texto usando o pincel de texto branco.


O retângulo de ação é atualizado por uma chamada subsequente para [ **GameInfoOverlay::SetAction** ](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) de um método no **GameMain** objeto, que fornece as informações de estado do jogo necessárias por **GameInfoOverlay::SetAction** para determinar a mensagem correta para o jogador, como "Toque para continuar".

A sobreposição para qualquer determinado estado é escolhida na [ **GameMain::SetGameInfoOverlay** ](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) método como este:

```cpp
void GameMain::SetGameInfoOverlay(GameInfoOverlayState state)
{
    m_gameInfoOverlayState = state;
    switch (state)
    {
    case GameInfoOverlayState::Loading:
        m_uiControl->SetGameLoading();
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

Agora o jogo tem uma maneira de comunicar informações de texto para o player com base no estado do jogo, e temos uma maneira de alternar o que é exibido para eles em todo o jogo.

### <a name="next-steps"></a>Próximas etapas

No próximo tópico, [Adicionando controles](tutorial--adding-controls.md), examinaremos como o jogador interage com a amostra de jogo e como a entrada altera o estado do jogo.



 




