---
author: abbycar
title: Adicionar uma interface do usuário
description: Saiba como adicionar uma sobreposição de interface do usuário 2D para um jogo DirectX UWP.
ms.assetid: fa40173e-6cde-b71b-e307-db90f0388485
ms.author: abigailc
ms.date: 10/24/2017
ms.topic: article
keywords: windows 10, uwp, jogos, interface do usuário, directx
ms.localizationpriority: medium
ms.openlocfilehash: 9962cc9043bd650390721715ca73b2e85a219c25
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035090"
---
# <a name="add-a-user-interface"></a>Adicionar uma interface do usuário


Agora que nosso jogo tem seus elementos visuais 3D no lugar, ele é tempo para se concentrar sobre como adicionar alguns elementos 2D para que o jogo possa fornecer comentários sobre o estado do jogo ao jogador. Isso pode ser feito com a adição de opções de menu simples e componentes do painel transparente sobre os elementos gráficos 3D pipeline de saída.

>[!Note]
>Se você ainda não tiver baixado o código de jogo mais recente para este exemplo, acesse [Jogo de exemplo em Direct3D](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Simple3DGameDX). Este exemplo faz parte de uma grande coleção de exemplos de recursos UWP. Para obter instruções sobre como baixar o exemplo, consulte [Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub](https://docs.microsoft.com/windows/uwp/get-started/get-uwp-app-samples).

## <a name="objective"></a>Objetivo

Usando Direct2D, adicione um número de elementos gráficos de interface do usuário e comportamentos para nossos incluindo jogos do DirectX UWP:
- Transparente, incluindo os retângulos do [controlador move-look](tutorial--adding-controls.md) limite
- Menus de estado do jogo


## <a name="the-user-interface-overlay"></a>A sobreposição da interface do usuário


Embora existam muitas maneiras de exibir elementos de interface do usuário e texto em um jogo do DirectX, vamos nos concentrar sobre como usar [Direct2D](https://msdn.microsoft.com/library/windows/apps/dd370990.aspx). Também usaremos [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) para os elementos de texto.


Direct2D é que um conjunto de APIs de desenho 2D usadas para desenhar e efeitos em primitivas com base em pixel. Quando começando com Direct2D, é melhor manter as coisas simples. Layouts e comportamentos de interface complexos exigem tempo e planejamento. Se seu jogo exigir uma interface do usuário complexas, como as encontradas em jogos de simulação e estratégia, considere usar XAML.

> [!NOTE]
> Para obter informações sobre o desenvolvimento de uma interface do usuário com XAML em um jogo DirectX UWP, consulte [Estendendo o exemplo de jogo](tutorial-resources.md).

Direct2D não é projetado especificamente para interfaces do usuário ou layouts HTML e XAML. Ele não fornece componentes da interface do usuário como listas, caixas ou botões. Ele também não fornece componentes de layout como divs, tabelas ou grades.


Neste jogo de exemplo, temos dois componentes principais da interface do usuário.
1. Um painel transparente para a pontuação e os controles do jogo.
2. Uma sobreposição usada para exibir texto de estado do jogo e opções, como informações de pausa e opções de inicialização de nível.

### <a name="using-direct2d-for-a-heads-up-display"></a>Usando o Direct2D para um painel transparente

A imagem a seguir mostra o transparente no jogo para a amostra. É simples e despojado, permitindo que o jogador se concentre em navegar pelo mundo 3D e atirar nos alvos. Uma boa interface ou painel transparente nunca deve complicar a capacidade do jogador de processar e reagir aos eventos do jogo.

![uma captura de tela da sobreposição do jogo](images/simple-dx-game-ui-overlay.png)

A sobreposição consiste em primitivas básicas seguintes.
- Texto [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038) no canto superior direito que informa ao jogador 
    - Ocorrências bem-sucedida
    - Número de tiros o player
    - Tempo restante no nível
    - Número do nível atual 
- Dois interseção segmentos de linha usados para formar uma mira
- Dois retângulos nos cantos inferior para os limites implícitos [controlador move-look](tutorial--adding-controls.md) . 


O estado da sobreposição do painel transparente no jogo é desenhado no método [**gamehud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L234-L358) da classe [**GameHud**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.h) . Neste método, a sobreposição de Direct2D que representa nossa interface do usuário é atualizada para refletir as alterações no número de ocorrências, hora número do nível e restantes.

Se o jogo foi inicializado, podemos adicionar `TotalHits()`, `TotalShots()`, e `TimeRemaining()` para um [**swprintf_s**](https://docs.microsoft.com/cpp/c-runtime-library/reference/sprintf-s-sprintf-s-l-swprintf-s-swprintf-s-l) buffer e especificar o formato de impressão. Em seguida, podemos pode desenhá-lo usando o método [**DrawText**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd742848) . Vamos fazer o mesmo para o indicador de nível atual, desenho números vazios para Mostrar níveis não concluídos como ➀ e números preenchidos como ➊ para mostrar que o nível específico foi concluído.


O trecho de código a seguir orienta pelas etapas de processo do método **gamehud:: Render** para 
- Criar um Bitmap usando [* * ID2D1RenderTarget::DrawBitmap * *](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371880)
- Seccionamento desativar áreas de interface do usuário em retângulos usando [ **D2D1::RectF**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368184)
- Usando **DrawText** para criar elementos de texto

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

Quebrando o método para baixo para além disso, esta parte da [**gamehud:: Render**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameHud.cpp#L320-L358) método desenha nosso movimento e tiro retângulos com [**ID2D1RenderTarget::DrawRectangle**](https://msdn.microsoft.com/library/windows/desktop/dd371902)e cruz usando duas chamadas para [**ID2D1RenderTarget::DrawLine**](https://msdn.microsoft.com/library/windows/desktop/dd371895).

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

No método **gamehud:: Render** armazenamos o tamanho lógico da janela do jogo no `windowBounds` variável. Usa o [`GetLogicalSize`](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.h#L41) método da classe **DeviceResources** . 
```cpp
auto windowBounds = m_deviceResources->GetLogicalSize();
```

 Obter o tamanho da janela do jogo é essencial para programação de interface do usuário. O tamanho da janela é fornecido uma medida chamada DIPs (pixels independentes de dispositivo), onde um DIP é definido como 1/96 de uma polegada. Direct2D redimensiona as unidades do desenho para pixels reais quando o desenho ocorre, ao fazer isso usando os Windows configuração pontos por polegada (DPI). Da mesma forma, ao desenhar texto usando [**DirectWrite**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368038), você especifica DIPs em vez de pontos para o tamanho da fonte. Os DIPs são expressos como números de ponto flutuante.

 

### <a name="displaying-game-state-info"></a>Exibir informações de estado do jogo

Além transparente, o exemplo de jogo possui uma sobreposição que representa a seis estados do jogo. Todos os estados de recursos uma grande primitiva retangular preta com texto para ser lido pelo jogador. Os retângulos do controlador move-look e cruz não é desenhados porque não estão ativos nesses estados.

A sobreposição é criada usando a classe [**GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.h) , possibilitando alternar o texto que é exibido para alinhar com o estado do jogo.

![status e a ação de sobreposição](images/simple-dx-game-ui-finaloverlay.png)

A sobreposição é dividida em duas seções: **Status** e a **ação**. O **Status** secton ainda mais é dividido em retângulos do **título** e **corpo** . A seção de **ação** tem apenas um retângulo. Cada retângulo tem uma finalidade diferente.

-   `titleRectangle` contém o texto do título.
-   `bodyRectangle` contém o texto do corpo.
-   `actionRectangle` contém o texto que informa o jogador para realizar uma ação específica.

O jogo tem seis estados que podem ser definidos. O estado do jogo transmitido usando a parte de **Status** da sobreposição. Os retângulos de **Status** são atualizados usando um número de métodos correspondentes com os seguintes estados.

- Carregando
- Estatísticas de pontuação inicial iniciar/alto
- Início de nível
- Jogo em pausa
- Fim de jogo
- Jogo ganho


A parte de **ação** da sobreposição é atualizada usando o método [**gameinfooverlay:: Setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) , permitindo que o texto da ação a ser definido como um dos seguintes.
- "Toque para jogar novamente..."
- "Nível de carregamento, aguarde..."
- "Tocar para continuar..."
- Nenhum(a)

> [!NOTE]
> Esses dois métodos serão discutidos mais detalhes na seção [que representa o estado do jogo](#representing-game-state) .

Dependendo do que está acontecendo no jogo, o **Status** e a seção de **ação** , campos de texto são ajustados.
Vamos examinar como inicializar e desenhar a sobreposição para esses seis estados.

### <a name="initializing-and-drawing-the-overlay"></a>Inicializando e desenhando a sobreposição

Os seis estados de **Status** têm algumas características em comum, tornando os recursos e métodos que precisam muito semelhante.
    - Todos eles usarem um retângulo preto no centro da tela como a tela de fundo.
    - O texto exibido é texto de **título** ou **corpo** .
    - O texto usa a fonte Segoe UI e é desenhado sobre o retângulo preto. 


O exemplo de jogo tem quatro métodos que entram em cena ao criar a sobreposição.
 

#### <a name="gameinfooverlaygameinfooverlay"></a>GameInfoOverlay::GameInfoOverlay
O construtor [**GameInfoOverlay::GameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L30-L78) inicializa a sobreposição, mantendo a superfície de bitmap que usaremos para exibir informações ao jogador em. O construtor obtém um alocador do objeto [**ID2D1Device**](https://msdn.microsoft.com/library/windows/desktop/hh404478) passado para ele, que ele usa para criar um [**ID2D1DeviceContext**](https://msdn.microsoft.com/library/windows/desktop/hh404479) que o próprio objeto de sobreposição pode desenhar. [IDWriteFactory::CreateTextFormat](https://msdn.microsoft.com/en-us/library/windows/desktop/dd368203) 


#### <a name="gameinfooverlaycreatedevicedependentresources"></a>Gameinfooverlay:: Createdevicedependentresources
[**Gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) é nosso método para criar pincéis que serão usados para desenhar o texto. Para fazer isso, podemos obter um objeto [**ID2D1DeviceContext2**](https://msdn.microsoft.com/en-us/library/windows/desktop/dn890789) que permite a criação e o desenho de geometria, além de funcionalidade como tinta e gradiente renderização de malha. Em seguida, criamos uma série de pincéis coloridos usando [**ID2D1SolidColorBrush**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd372207) para desenhar os elementos de interface do usuário folling.
- Pincel preto para planos de fundo do retângulo
- Pincel branco para texto de status
- Pincel laranja para texto de ação

#### <a name="deviceresourcessetdpi"></a>DeviceResources::SetDpi
O método [**DeviceResources::SetDpi**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L514-L527) define os pontos por polegada da janela. Esse método é chamado quando o DPI é alterado e precisa ser reajustado à medida que ocorre quando a janela do jogo é redimensionada. Depois de atualizar o DPI, esse método também chama[**deviceresources:: Createwindowsizedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/Common/DeviceResources.cpp#L214-L487) para garantir que os recursos necessários serão recriados sempre que a janela for redimensionada.


#### <a name="gameinfooverlaycreatewindowssizedependentresources"></a>GameInfoOverlay::CreateWindowsSizeDependentResources
O método [**GameInfoOverlay::CreateWindowsSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L108-L225) é onde todos os nossa desenho ocorre. A seguir está um esboço das etapas do método.
- Três retângulos são criados à seção de desativar o texto da interface do usuário para o texto de **título**, **corpo**e **ação** .
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

- Um Bitmap é criado nomeado `m_levelBitmap`, levando o DPI atual em conta usando **CreateBitmap**.
- `m_levelBitmap` é definido como usando [**ID2D1DeviceContext::SetTarget**](https://msdn.microsoft.com/en-us/library/windows/desktop/hh404533)de destino de renderização de nosso 2D.
- O Bitmap é limpos com cada pixel feita preto usando [**ID2D1RenderTarget::Clear**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371772).
- [**ID2D1RenderTarget::BeginDraw**](https://msdn.microsoft.com/en-us/library/windows/desktop/dd371768) é chamado para iniciar o desenho. 
- **DrawText** é chamado para desenhar o texto armazenado no `m_titleString`, `m_bodyString`, e `m_actionString` no retângulo apropriado usando o correspondente **ID2D1SolidColorBrush**.
- [**ID2D1RenderTarget::EndDraw**](ID2D1RenderTarget::EndDraw) é chamado para interromper todas as operações de desenho em `m_levelBitmap`.
- Outro Bitmap é criado usando **CreateBitmap** chamado `m_tooSmallBitmap` para usar como um fallback, mostrando apenas se a configuração de exibição é muito pequena para o jogo.
- Repita o processo para desenhar em `m_levelBitmap` para `m_tooSmallBitmap`, desta vez apenas desenhar a cadeia de caracteres `Paused` no corpo.




Agora, todos que precisamos são seis métodos para preencher o texto dos nossos estados de seis sobreposição!

### <a name="representing-game-state"></a>Que representa o estado do jogo


Cada um dos Estados de seis sobreposição no jogo tem um método correspondente no objeto **GameInfoOverlay** . Esses métodos desenham uma variação da sobreposição para fornecer ao jogador informações explícitas sobre o próprio jogo. Essa comunicação é representada com uma cadeia de caracteres do **título** e **corpo** . Como o exemplo já configurado recursos e o layout para essas informações quando ele foi inicializado e com o método [**gameinfooverlay:: Createdevicedependentresources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L82-L104) , ele só precisa fornecer a sobreposição cadeias de caracteres de estado específico.

A parte de **Status** da sobreposição é definida com uma chamada para um dos seguintes métodos.

Estado do jogo | Status definido método | Campos de status
:----- | :------- | :---------
Carregando | [GameInfoOverlay::SetGameLoading](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L254-L306) |**Title**</br>Carregamento de recursos </br>**Body**</br> Imprime incrementalmente "." para implica a atividade de carregamento.
Estatísticas de pontuação inicial iniciar/alto | [GameInfoOverlay::SetGameStats](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L310-L354) |**Title**</br>Pontuação</br> **Body**</br> Níveis concluído # </br>Total de pontos #</br>Capturas de total #
Início de nível | [GameInfoOverlay::SetLevelStart](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L413-L471) |**Title**</br>Nível #</br>**Body**</br>Descrição do nível de objetivo.
Jogo em pausa | [GameInfoOverlay::SetPause](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L475-L502) |**Title**</br>Jogo em pausa</br>**Body**</br>Nenhum(a)
Fim de jogo | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>Fim de jogo</br> **Body**</br> Níveis concluído # </br>Total de pontos #</br>Capturas de total #</br>Níveis concluído #</br>Alta classificação #
Jogo ganho | [GameInfoOverlay::SetGameOver](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L358-L409) |**Title**</br>Você GANHOU!</br> **Body**</br> Níveis concluído # </br>Total de pontos #</br>Capturas de total #</br>Níveis concluído #</br>Alta classificação #




Com o método [**GameInfoOverlay::CreateWindowSizeDependentResources**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L117-L134) , o exemplo declarou três áreas retangulares que correspondem a regiões específicas da sobreposição.



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

Usando o contexto de dispositivo Direct2D que o objeto **GameInfoOverlay** inicializado, esse método preenche os retângulos do título e corpo com preto usando o pincel de plano de fundo. Ele desenha o texto da cadeia de caracteres "High Score" no retângulo de título e uma cadeia de caracteres contendo as informações de estado do jogo atualizadas no retângulo de corpo de texto usando o pincel de texto branco.


O retângulo de ação é atualizado por uma chamada subsequente para [**gameinfooverlay:: Setaction**](https://github.com/Microsoft/Windows-universal-samples/blob/5f0d0912214afc1c2a7c7470203933ddb46f7c89/Samples/Simple3DGameDX/cpp/GameInfoOverlay.cpp#L522-L564) de um método no objeto **GameMain** , que fornece as informações de estado do jogo necessárias **gameinfooverlay:: Setaction** para determinar a mensagem certa para o Player, como "Tocar para continuar".

A sobreposição para qualquer estado específico é escolhida no método [**GameMain::SetGameInfoOverlay**](https://github.com/Microsoft/Windows-universal-samples/blob/6370138b150ca8a34ff86de376ab6408c5587f5d/Samples/Simple3DGameXaml/cpp/GameMain.cpp#L606-L661) desta forma:

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

Agora o jogo tem uma maneira de comunicar informações de texto ao jogador com base no estado do jogo, e temos uma maneira de alternar o que é exibido para eles em todo o jogo.

### <a name="next-steps"></a>Próximas etapas

No próximo tópico, [Adicionando controles](tutorial--adding-controls.md), examinaremos como o jogador interage com a amostra de jogo e como a entrada altera o estado do jogo.



 




