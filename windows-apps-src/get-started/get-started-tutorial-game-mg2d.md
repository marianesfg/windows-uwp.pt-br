---
title: Criar um jogo UWP em MonoGame 2D
description: Um jogo simples UWP para a Microsoft Store, escrito em c# e MonoGame
author: muhsinking
ms.author: mukin
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: 37d43094ba679ebe5439996373626522590e3fcc
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6974861"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>Criar um jogo UWP em MonoGame 2D

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>Um jogo UWP 2D simples para a Microsoft Store, escrito em C# e MonoGame


![Folha de sprite do Walking Dino](images/JS2D_0.png)

## <a name="introduction"></a>Introdução

O MonoGame é uma estrutura leve de desenvolvimento de jogos. Este tutorial vai lhe ensinar as noções básicas do desenvolvimento de jogos em MonoGame, incluindo como carregar o conteúdo, desenhar sprites, animá-los e manipular as entradas do usuário. Alguns conceitos mais avançados como detecção de colisão e escalonamento para telas de DPI alto também são discutidos. Este tutorial dura de 30 a 60 minutos.

## <a name="prerequisites"></a>Pré-requisitos
+   Windows 10 e Microsoft Visual Studio 2017.  [Clique aqui para saber como fazer a preparação inicial com o Visual Studio](https://docs.microsoft.com/en-us/windows/uwp/get-started/get-set-up).
+ A estrutura de desenvolvimento da área de trabalho do .NET. Se você ainda não tiver instalado, você pode obtê-lo, executando o instalador do Visual Studio novamente e modificar a instalação do Visual Studio 2017.
+   Conhecimento básico de C# ou uma linguagem de programação similar orientada a objetos. [Clique aqui para acessar uma introdução a C#](https://docs.microsoft.com/en-us/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).
+   É útil ter familiaridade com conceitos de ciências básicas de computação, como classes, métodos e variáveis.

## <a name="why-monogame"></a>Por que o MonoGame?
Não faltam opções quando se trata de ambientes de desenvolvimento de jogos. Pode ser difícil saber onde começar quando se tem desde mecanismos repletos de recursos, como Unity, até APIs de multimídia abrangentes e complexas, como DirectX. O MonoGame é um conjunto de ferramentas, com um nível de complexidade que se situa entre um mecanismo de jogo e uma API mais robusta como DirectX. Ele fornece um pipeline de conteúdo fácil de usar, e todas as funcionalidades necessárias para criar jogos leves que são executados em uma ampla variedade de plataformas. Melhor de tudo, apps MonoGame são escritos em c# pura, e você pode distribuí-los rapidamente por meio da Microsoft Store ou outras plataformas de distribuição semelhantes.

## <a name="get-the-code"></a>Obter o código
Se você estiver com vontade de seguir as etapas do tutorial passo a passo e quiser apenas ver o MonoGame em ação, [clique aqui para baixar o app finalizado](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Abra o projeto no Visual Studio 2017 e pressione **F5** para executar o exemplo. A primeira vez que você fizer isso pode demorar um pouco, pois o Visual Studio precisa buscar qualquer pacote NuGet que esteja faltando na sua instalação.

Se você tiver feito isso, pule a próxima seção sobre como configurar o MonoGame para ver um passo a passo do código.

**Observação:** o jogo criado nesta amostra não pretende ser completo (e nem divertido). Sua única finalidade é demonstrar todos os conceitos básicos do desenvolvimento 2D no MonoGame. Fique à vontade para usar este código e fazer algo muito melhor — ou apenas começar do zero após aprender as noções básicas!

## <a name="set-up-monogame-project"></a>Configurar o projeto do MonoGame
1. Instale o **MonoGame 3.6** para Visual Studio de [MonoGame.net](http://www.monogame.net/)

2. Inicie o Visual Studio 2017.

3. Vá para **Arquivo -> Novo -> Projeto**

4. Nos modelos de projeto do Visual C#, selecione **MonoGame** e **projeto Universal do Windows 10 do MonoGame**

5. Nomeie o projeto "MonoGame2D" e selecione OK. Com o projeto criado, provavelmente vai parecer que está cheia de erros — eles devem desaparecer depois que você executar o projeto pela primeira vez, e todos os pacotes NuGet ausentes estiverem instalados.

6. Certifique-se de **x86** e **Máquina Local** estejam definidos como a plataforma de destino e pressione **F5** para compilar e executar o projeto vazio. Se tiver seguido as etapas acima, você deve ver uma janela vazia azul depois que o projeto terminar a compilação.

## <a name="method-overview"></a>Visão geral do método
Agora que você criou o projeto, abra o arquivo **Game1.cs** do **Gerenciador de Soluções**. Aqui é o lugar em que a maior parte da lógica do jogo vai estar. Muitos métodos cruciais são gerados automaticamente aqui quando você cria um novo projeto do MonoGame. Vamos revisá-los rapidamente:

**public Game1()** o construtor. Nós não vamos alterar nada nesse método para este tutorial.

**protected override void Initialize()** Aqui inicializamos todas as variáveis de classe que serão usadas. Esse método é chamado uma vez no início do jogo.

**protected override void LoadContent()** Este método carrega o conteúdo (p. ex. texturas, áudio, fontes) na memória antes de o jogo ser iniciado. Como Inicializar, ele é chamado uma vez quando o app é iniciado.

**protected override void UnloadContent()** Este método é usado para descarregar o conteúdo do gerenciador de não conteúdo. Nós não o usamos.

**protected override void Update (GameTime gameTime)** Esse método é chamado uma vez para cada ciclo do loop do jogo. Aqui, podemos atualizar os estados de qualquer objeto ou variável usados no jogo. Isso inclui itens como posição, a velocidade ou a cor de um objeto. Isso também é que a entrada do usuário é manipulada. Em poucas palavras, esse método manipula cada parte da lógica do jogo, exceto objetos de desenho na tela.
**protected override void Draw(GameTime gameTime)** É aqui que os objetos são desenhados na tela, usando as posições dadas pelo método Update.

## <a name="draw-a-sprite"></a>Desenhar um sprite
Então, você executou seu projeto do MonoGame e encontrou um belo céu azul — vamos adicionar um chão.
No MonoGame, a arte 2D é adicionada ao app sob a forma de "sprites". Um sprite é apenas um gráfico de computador que é manipulado como uma única entidade. Os sprites podem ser movidos, dimensionados, moldados, animados e combinados para criar qualquer coisa que você possa imaginar no espaço 2D.

### <a name="1-download-a-texture"></a>1. Baixe uma textura
Para os nossos objetivos, este primeiro sprite vai ser extremamente tedioso. [Clique aqui para baixar este retângulo verde monótono](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Adicione a textura na pasta de Conteúdo
- Abra o **Gerenciador de Soluções**
- Clique com botão direito em **Content.mgcb** na pasta **Conteúdo** e selecione **Abrir com**. No menu pop-up selecione **Monogame Pipeline**e selecione **OK**.
- Na nova janela, clique com botão direito no item **Conteúdo** e selecione **Adicionar -> Item Existente**.
- Localize e selecione o retângulo verde no navegador de arquivos.
- Nomeie o item "grass.png" e selecione **Adicionar**.

### <a name="3-add-class-variables"></a>3. Adicione variáveis de classe
Para carregar esta imagem como uma textura de sprite, abra **Game1.cs** e adicione as seguintes variáveis de classe.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

A variável SKYRATIO nos informa quanto da cena queremos que seja céu versus grama — nesse caso, dois terços. **screenWidth** e **screenHeight** manterão o controle do tamanho da janela do app, enquanto **grass** é onde vamos armazenar nosso retângulo verde.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. Inicialize as variáveis de classe e defina o tamanho da janela
As variáveis **screenWidth** e **screenHeight** ainda precisam ser inicializadas, então adicione este código ao método **Inicialize**:

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

Além de obtermos a altura e a largura da tela, nós também definimos o modo de janelas do app como **Fullscreen** e deixamos o mouse invisível.

### <a name="5-load-the-texture"></a>5. Carregue a textura
Para carregar a textura para a variável de grama, adicione o seguinte ao método **LoadContent**:

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. Desenhe o sprite
Para desenhar o retângulo, adicione as seguintes linhas ao método **Draw**:

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

Aqui, nós usamos o método **spriteBatch.Draw** para colocar a textura determinada dentro das bordas de um objeto Rectangle. A **Rectangle** é definido por coordenadas x e y de seu canto superior esquerdo e canto inferior direito. Usando as variáveis **screenWidth**, **screenHeight** e **SKYRATIO** que definimos anteriormente, podemos desenhar a textura do retângulo verde no terço inferior da tela. Se executar o programa agora, você deve ver a tela de fundo azul de antes, parcialmente coberta pelo retângulo verde.

![Retângulo verde](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>Dimensione para telas de DPI alto
Se estiver executando o Visual Studio em um monitor de alta densidade de pixels, como as encontradas em um Surface Pro ou Surface Studio, você pode descobrir que o retângulo verde das etapas acima não cobre todo o terço inferior da tela. Ele provavelmente está flutuando sobre o canto esquerdo inferior da tela. Para corrigir isso e unificar a experiência de nosso jogo em todos os dispositivos, precisamos criar um método que dimensione alguns valores relativos à densidade de pixels da tela:

```CSharp
public float ScaleToHighDPI(float f)
{
  DisplayInformation d = DisplayInformation.GetForCurrentView();
  f *= (float)d.RawPixelsPerViewPixel;
  return f;
}
```

Em seguida, substitua as inicializações de **screenHeight** e **screenWidth** no método **Initialize** por isto:

```CSharp
screenHeight = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Height);
screenWidth = ScaleToHighDPI((float)ApplicationView.GetForCurrentView().VisibleBounds.Width);
```

Se estiver usando uma tela DPI alto e executar o aplicativo agora, você deve ver o retângulo verde cobrindo o terço inferior da tela como esperado.

## <a name="build-the-spriteclass"></a>Criar o SpriteClass
Antes de começarmos a animar sprites, vamos fazer uma nova classe chamada "SpriteClass", que nos deixará reduzir a complexidade de nível de superfície de manipulação de sprites.

### <a name="1-create-a-new-class"></a>1. Crie uma nova classe
No **Gerenciador de Soluções**, clique com botão direito do mouse em **MonoGame2D (Universal do Windows)** e selecione **Adicionar -> Classe**. Nomeie a classe "SpriteClass.cs" e selecione **Adicionar**.

### <a name="2-add-class-variables"></a>2. Adicione variáveis de classe
Adicione este código à classe que você acabou de criar:

```CSharp
public Texture2D texture
{
  get;
}

public float x
{
  get;
  set;
}

public float y
{
  get;
  set;
}

public float angle
{
  get;
  set;
}

public float dX
{
  get;
  set;
}

public float dY
{
  get;
  set;
}

public float dA
{
  get;
  set;
}

public float scale
{
  get;
  set;
}
```

Aqui nós definimos as variáveis de classe de que precisamos para desenhar e animar um sprite. As variáveis **x** e **y** representam a posição atual do sprite no plano, enquanto a variável **angle** é o ângulo atual do sprite em graus (0 sendo vertical, 90 sendo inclinado a 90 graus no sentido horário). É importante observar que, para essa classe, **x** e **y** representam as coordenadas do **centro** do sprite, (a origem padrão é o canto superior esquerdo). Isso torna mais fácil girar os sprites, pois eles giram em torno de qualquer origem que receberem, e girar em torno do centro nos dá um movimento de rotação uniforme.

Depois disso, temos **dX**, **dY** e **dA**, que são as taxas de alteração por segundo respectivamente das variáveis **x**, **y** e **angle**.

### <a name="3-create-a-constructor"></a>3. Criar um construtor
Ao criar uma instância de **SpriteClass**, fornecemos o construtor com o dispositivo de elementos gráficos do **Game1.cs**, o caminho para a textura em relação à pasta do projeto e a escala desejada da textura em relação ao seu tamanho original. Vamos definir o restante das variáveis de classe depois de começarmos o jogo, no método update.

```CSharp
public SpriteClass (GraphicsDevice graphicsDevice, string textureName, float scale)
{
  this.scale = scale;
  if (texture == null)
  {
    using (var stream = TitleContainer.OpenStream(textureName))
    {
      texture = Texture2D.FromStream(graphicsDevice, stream);
    }
  }
}
```

### <a name="4-update-and-draw"></a>4. Update e Draw
Ainda existem alguns métodos que precisamos adicionar à declaração de SpriteClass:

```CSharp
public void Update (float elapsedTime)
{
  this.x += this.dX * elapsedTime;
  this.y += this.dY * elapsedTime;
  this.angle += this.dA * elapsedTime;
}

public void Draw (SpriteBatch spriteBatch)
{
  Vector2 spritePosition = new Vector2(this.x, this.y);
  spriteBatch.Draw(texture, spritePosition, null, Color.White, this.angle, new Vector2(texture.Width/2, texture.Height/2), new Vector2(scale, scale), SpriteEffects.None, 0f);
}
```

O método **Update** SpriteClass é chamado no método **Update** de Game1.cs e é usado para atualizar os valores dos sprites **x**, **y**, e **angle** com base em suas respectivas taxas de alteração.

O método **Draw** método é chamado no método **Draw** de Game1.cs e é usado para desenhar o sprite na janela do jogo.

## <a name="user-input-and-animation"></a>Entrada do usuário e animação
Agora temos o SpriteClass compilado e vamos usá-lo para criar dois novos objetos de jogos. O primeiro é um avatar que o jogador pode controlar com as teclas de seta e a barra de espaço. O segundo é um objeto que o jogador deve evitar.

### <a name="1-get-the-textures"></a>1. Obter as texturas
Para o avatar do jogador, vamos usar o gato ninja da própria Microsoft, montado em seu amigo T-rex. [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

Agora, o obstáculo que o jogador precisa evitar. O que gatos ninja e dinossauros carnívoros odeiam mais do que qualquer coisa? Comer verduras! [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

Como já fez antes com o retângulo verde, adicione essas imagens em **Content.mgcb** por meio do **MonoGame Pipeline**, nomeando-as "ninja-cat-dino.png" e "broccoli.png", respectivamente.

### <a name="2-add-class-variables"></a>2. Adicione variáveis de classe
Adicione o seguinte código à lista de variáveis de classe em **Game1.cs**:

```CSharp
SpriteClass dino;
SpriteClass broccoli;

bool spaceDown;
bool gameStarted;

float broccoliSpeedMultiplier;
float gravitySpeed;
float dinoSpeedX;
float dinoJumpY;
float score;

Random random;
```

**dino** e **broccoli** são nossas variáveis SpriteClass. **dino** manterá o avatar do jogador, enquanto **broccoli** manterá o obstáculo brócolis.

**spaceDown** monitora se a barra de espaço está sendo pressionada ou se foi pressionada e liberada.

**gameStarted** nos informa se o usuário iniciou o jogo pela primeira vez.

**broccoliSpeedMultiplier** determina a rapidez com que o obstáculo brócolis se move pela tela.

**gravitySpeed** determina a rapidez com que o avatar do jogador acelera para baixo após um salto.

**dinoSpeedX** e **dinoJumpY** determinam a velocidade com que o avatar do jogador se movimenta e salta.
score rastreia de quantos obstáculos o jogador se esquivou com êxito.

Por fim, **random** será usado para adicionar um pouco de imprevisibilidade ao comportamento do obstáculo brócolis.

### <a name="3-initialize-variables"></a>3. Inicialize as variáveis
Em seguida, precisamos inicializar estas variáveis. Adicione adicione o seguinte código ao método Initialize:

```CSharp
broccoliSpeedMultiplier = 0.5f;
spaceDown = false;
gameStarted = false;
score = 0;
random = new Random();
dinoSpeedX = ScaleToHighDPI(1000f);
dinoJumpY = ScaleToHighDPI(-1200f);
gravitySpeed = ScaleToHighDPI(30f);
```

Observe que as três últimas variáveis precisam ser dimensionadas para dispositivos com alto DPI, porque elas especificam uma taxa de alteração em pixels.

### <a name="4-construct-spriteclasses"></a>4. Construa SpriteClasses
Nós vamos construir objetos SpriteClass no método **LoadContent**. Adicione este código ao que você já tem:

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

A imagem de brócolis é bem maior do que queremos que apareça no jogo, portanto, nós vamos dimensioná-la até 0,2 vezes seu tamanho original.

### <a name="5-program-obstacle-behaviour"></a>5. comportamento de obstáculo programa
Queremos que o brócolis para surja de um lugar fora da tela e vá na direção do avatar do jogador para que ele precise se esquivar. Para fazer isso, adicione este método à classe **Game1.cs** :

```CSharp
public void SpawnBroccoli()
{
  int direction = random.Next(1, 5);
  switch (direction)
  {
    case 1:
      broccoli.x = -100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 2:
      broccoli.y = -100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
    case 3:
      broccoli.x = screenWidth + 100;
      broccoli.y = random.Next(0, (int)screenHeight);
      break;
    case 4:
      broccoli.y = screenHeight + 100;
      broccoli.x = random.Next(0, (int)screenWidth);
      break;
  }

  if (score % 5 == 0) broccoliSpeedMultiplier += 0.2f;

  broccoli.dX = (dino.x - broccoli.x) * broccoliSpeedMultiplier;
  broccoli.dY = (dino.y - broccoli.y) * broccoliSpeedMultiplier;
  broccoli.dA = 7f;
}
```

A primeira parte do do método determina de que ponto fora da tela o objeto brócolis vai surgir, usando dois números aleatórios.

A segunda parte determina a rapidez com que o brócolis se moverá, que é determinada pela pontuação atual. Ele ficará mais rápido a cada cinco brócolis de que o jogador conseguir se esquivar.

A terceira parte define a direção do movimento do sprite brócolis. Ele vai na direção do avatar do jogador (dinossauro) quando o brócolis é gerado. Nós também damos um valor **dA** de 7f, que fará o brócolis girar no ar enquanto persegue o jogador.

### <a name="6-program-game-starting-state"></a>6. Programe o estado inicial do jogo
Antes de podermos à manipulação da entrada do teclado, precisamos de um método que defina o estado inicial do jogo dos dois objetos que criamos. Em vez de o jogo começar assim que o app for executado, queremos que o usuário o inicie manualmente, pressionando a barra de espaços. Adicione o código a seguir, que define o estado inicial dos objetos animados e redefine a pontuação:

```CSharp
public void StartGame()
{
  dino.x = screenWidth / 2;
  dino.y = screenHeight * SKYRATIO;
  broccoliSpeedMultiplier = 0.5f;
  SpawnBroccoli();  
  score = 0;
}
```

### <a name="7-handle-keyboard-input"></a>7. manipular a entrada do teclado
Em seguida, precisamos de um novo método para manipular a entrada do usuário por meio do teclado. Adicione este método para **Game1.cs**:

```CSharp
void KeyboardHandler()
{
  KeyboardState state = Keyboard.GetState();

  // Quit the game if Escape is pressed.
  if (state.IsKeyDown(Keys.Escape))
  {
    Exit();
  }

  // Start the game if Space is pressed.
  if (!gameStarted)
  {
    if (state.IsKeyDown(Keys.Space))
    {
      StartGame();
      gameStarted = true;
      spaceDown = true;
      gameOver = false;
    }
    return;
  }            
  // Jump if Space is pressed
  if (state.IsKeyDown(Keys.Space) || state.IsKeyDown(Keys.Up))
  {
    // Jump if the Space is pressed but not held and the dino is on the floor
    if (!spaceDown && dino.y >= screenHeight * SKYRATIO - 1) dino.dY = dinoJumpY;

    spaceDown = true;
  }
  else spaceDown = false;

  // Handle left and right
  if (state.IsKeyDown(Keys.Left)) dino.dX = dinoSpeedX * -1;

  else if (state.IsKeyDown(Keys.Right)) dino.dX = dinoSpeedX;
  else dino.dX = 0;
}
```

Acima, temos uma série de quatro instruções if:

A primeira encerra o jogo se a tecla **Escape** for pressionada.

A segunda inicia o jogo se a tecla **Space** tecla for pressionada e o jogo não tiver sido iniciado.

A terceira faz o avatar dinossauro pular se **Space** for pressionada, alterando sua propriedade **dY**. Observe que o jogador não pode pular a menos que estejam no "chão" (Dino = screenHeight * SKYRATIO) e também não vai pular se a tecla de espaço está sendo pressionada em vez de ser pressionada uma vez. Isso impede o dinossauro de saltar assim que o jogo é iniciado, utilizando o mesmo pressionamento de tecla que inicia o jogo.

Por fim, a última instrução if/else verifica se as setas direcionais para a esquerda ou direita estão sendo pressionadas e, em caso afirmativo, altera a propriedade **dX** do dinossauro, de acordo.

**Desafio:** você pode fazer o método de manipulação de teclado acima funcionar com o esquema de entrada WASD, além da teclas de seta?

### <a name="8-add-logic-to-the-update-method"></a>8. Adicione lógica ao método Update
Em seguida, precisamos adicionar a lógica para todas essas partes ao método **Update** em **Game1.cs**:

```CSharp
float elapsedTime = (float)gameTime.ElapsedGameTime.TotalSeconds;
KeyboardHandler(); // Handle keyboard input
// Update animated SpriteClass objects based on their current rates of change
dino.Update(elapsedTime);
broccoli.Update(elapsedTime);

// Accelerate the dino downward each frame to simulate gravity.
dino.dY += gravitySpeed;

// Set game floor so the player does not fall through it
if (dino.y > screenHeight * SKYRATIO)
{
  dino.dY = 0;
  dino.y = screenHeight * SKYRATIO;
}

// Set game edges to prevent the player from moving offscreen
if (dino.x > screenWidth - dino.texture.Width/2)
{
  dino.x = screenWidth - dino.texture.Width/2;
  dino.dX = 0;
}
if (dino.x < 0 + dino.texture.Width/2)
{
  dino.x = 0 + dino.texture.Width/2;
  dino.dX = 0;
}

// If the broccoli goes offscreen, spawn a new one and iterate the score
if (broccoli.y > screenHeight+100 || broccoli.y < -100 || broccoli.x > screenWidth+100 || broccoli.x < -100)
{
  SpawnBroccoli();
  score++;
}
```

### <a name="9-draw-spriteclass-objects"></a>9. Draw objetos SpriteClass
Por fim, adicione o seguinte código ao método **Draw** de **Game1.cs**, logo depois da última chamada de **spriteBatch.Draw**:

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

No MonoGame, novas chamadas de **spriteBatch.Draw** vão desenhar sobre todas as chamadas anteriores. Isso significa que os sprites do brócolis e o dinossauro serão desenhados sobre o sprite da grama existente, portanto, eles nunca podem estar oculta atrás dela, independentemente de sua posição.

Tente executar o jogo agora e movimentar o dinossauro com as teclas de seta e a barra de espaços. Se você tiver seguido as etapas acima, você deve ser capaz de fazer seu avatar se movimente dentro da janela do jogo e o brócolis deve gerar a uma velocidade crescente.

![Avatar do jogador e obstáculo](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>Renderize texto com SpriteFont
Usando o código acima, podemos manter o controle de pontuação do jogador nos bastidores, mas não dizemos ao jogador quantos pontos tem. Também temos uma introdução bem contra-intuitiva quando o app é iniciado: o jogador vê uma janela azul e verde, mas não tem como saber que precisa pressionar Space para fazer as coisas começarem.

Para corrigir esses dois problemas, vamos usar um novo tipo de objeto MonoGame chamado **SpriteFonts**.

### <a name="1-create-spritefont-description-files"></a>1. Crie arquivos de descrição SpriteFont
No **Gerenciador de Soluções** encontre a pasta **Conteúdo**. Nesta pasta, clique com botão direito do mouse no arquivo **Content.mgcb** e selecione **Abrir com**. No menu pop-up, selecione **MonoGame Pipeline** e, depois, pressione **OK**. Na nova janela, clique com botão direito do mouse no item **Conteúdo** e selecione **Adicionar -> Novo Item**. Selecione **SpriteFont Description**, chame-o de "Score" e pressione **OK**. Em seguida, adicione outra descrição SpriteFont denominada "GameState" usando o mesmo procedimento.

### <a name="2-edit-descriptions"></a>2. Edite as descrições
Clique com botão direito do mouse na pasta **Conteúdo** no **MonoGame Pipeline** e selecione **Abrir local do arquivo**. Você deve ver uma pasta com os arquivos de descrição SpriteFont que você acabou de criar, bem como as imagens que você adicionou à pasta Conteúdo até agora. Agora você pode fechar e salvar a janela do pipeline do MonoGame. No **Explorador de Arquivos** abra ambos os arquivos de descrição em um editor de texto (Visual Studio, NotePad++, Atom etc).

Cada descrição contém vários valores que descrevem o SpriteFont. Vamos fazer algumas alterações:

Em **Score.spritefont**, altere o valor **<Size>** de 12 para 36.

Em **GameState.spritefont**, altere o valor **<Size>** de 12 para 72 e o valor **<FontName>** de Arial para Agency. Agency é outra fonte que vem por padrão com computadores Windows 10 e adicionará um toque de arte em nossa tela Introdução.

### <a name="3-load-spritefonts"></a>3. Carregue SpriteFonts
De volta ao Visual Studio, primeiro vamos adicionar uma nova textura para a tela inicial de Introdução. [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

Como antes, adicione a textura ao projeto clicando com o botão direito do mouse em Conteúdo e selecionando **Adicionar -> Item existente**. Nomeie o novo item como “start-splash.png”.

Em seguida, adicione as seguintes variáveis de classe a **Game1.cs**:

```CSharp
Texture2D startGameSplash;
SpriteFont scoreFont;
SpriteFont stateFont;
```

Depois, adicione estas linhas ao método **LoadContent**:

```CSharp
startGameSplash = Content.Load<Texture2D>("start-splash");
scoreFont = Content.Load<SpriteFont>("Score");
stateFont = Content.Load<SpriteFont>("GameState");
```

### <a name="4-draw-the-score"></a>4. Desenhe a pontuação
Vá para o método **Draw** de **Game1.cs** e adicione o código a seguir antes de **spriteBatch.End();**

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

O código acima usa a descrição de sprite que criamos (Arial tamanho 36) para informar a pontuação atual do jogador perto do canto superior direito da tela.

### <a name="5-draw-horizontally-centered-text"></a>5. Desenhe um texto centralizado horizontalmente
Ao fazer um jogo, muitas vezes você desejará desenhar texto centralizado, horizontal ou verticalmente. Para centralizar horizontalmente o texto introdutório, adicione este código ao método **Draw** logo antes de **spriteBatch.End();**

```CSharp
if (!gameStarted)
{
  // Fill the screen with black before the game starts
  spriteBatch.Draw(startGameSplash, new Rectangle(0, 0,
  (int)screenWidth, (int)screenHeight), Color.White);

  String title = "VEGGIE JUMP";
  String pressSpace = "Press Space to start";

  // Measure the size of text in the given font
  Vector2 titleSize = stateFont.MeasureString(title);
  Vector2 pressSpaceSize = stateFont.MeasureString(pressSpace);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, title,
  new Vector2(screenWidth / 2 - titleSize.X / 2, screenHeight / 3),
  Color.ForestGreen);
  spriteBatch.DrawString(stateFont, pressSpace,
  new Vector2(screenWidth / 2 - pressSpaceSize.X / 2,
  screenHeight / 2), Color.White);
  }
```

Primeiro, criamos duas cadeias de caracteres, uma para cada linha de texto que queremos desenhar. Em seguida, medimos a largura e altura de cada linha quando impressa, usando o método **SpriteFont.MeasureString(String)**. Isso nos dá o tamanho como um objeto **Vector2**, com a propriedade **X** que contém a largura e a **Y** que contém a altura.

Por fim, desenhamos cada linha. Para centralizar o texto horizontalmente, tornamos o valor **X** de seu vetor de posição igual a **screenWidth / 2 - textSize.X / 2**

**Desafio:** como você alterar o procedimento descrito acima para centralizar o texto verticalmente e também horizontalmente?

Experimente executar o jogo. Você consegue ver a tela inicial de Introdução? A pontuação aumenta a cada vez os brócolis aparecem?

![Tela inicial de Introdução](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>Detecção de colisão
Então, nós temos um brócolis que o segue por toda parte e ainda temos uma pontuação que sobre a cada vez que aparece um novo — mas do jeito em que está não existe realmente nenhuma maneira de perder esse jogo. Precisamos de uma maneira de saber se os sprites do dinossauro e dos brócolis colidem, e quando isso acontece, para declarar fim de jogo.

### <a name="1-rectangular-collision"></a>1. Colisão retangular
Para detectar colisões em um jogo, os objetos geralmente são simplificados para reduzir a complexidade dos cálculos envolvidos. Vamos tratar tanto o avatar do jogador e o obstáculo brócolis como retângulos para fins de detecção de colisão entre eles.

Abra **SpriteClass.cs** e adicione uma nova variável de classe:

```CSharp
const float HITBOXSCALE = .5f;
```

Esse valor representará a medida em que a detecção de colisão é "permissiva" para o jogador. Com um valor de 0,5f, as bordas do retângulo em que o dinossauro pode colidir com o brócolis — muitas vezes chamado de "hitbox" — será metade do tamanho total da textura. Isso resultará em alguns casos em que os cantos das duas texturas colidem, sem que nenhuma parte das imagens realmente pareçam se tocar. Fique à vontade para ajustar esse valor conforme o seu gosto pessoal.

Em seguida, adicione um novo método a **SpriteClass.cs**:

```CSharp
public bool RectangleCollision(SpriteClass otherSprite)
{
  if (this.x + this.texture.Width * this.scale * HITBOXSCALE / 2 < otherSprite.x - otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y + this.texture.Height * this.scale * HITBOXSCALE / 2 < otherSprite.y - otherSprite.texture.Height * otherSprite.scale / 2) return false;
  if (this.x - this.texture.Width * this.scale * HITBOXSCALE / 2 > otherSprite.x + otherSprite.texture.Width * otherSprite.scale / 2) return false;
  if (this.y - this.texture.Height * this.scale * HITBOXSCALE / 2 > otherSprite.y + otherSprite.texture.Height * otherSprite.scale / 2) return false;
  return true;
}
```

Esse método detecta se dois objetos retangulares colidiram. O algoritmo funciona por meio de testes para ver se há uma lacuna entre qualquer uma das laterais dos retângulos. Se houver uma lacuna, não há nenhuma colisão — se não houver nenhuma lacuna, tem de haver uma colisão.

### <a name="2-load-new-textures"></a>2. Carregue novas texturas

Em seguida, abra **Game1.cs** e adicione duas novas variáveis de classe, uma para armazenar a textura do sprite fim de jogo e um boliano para rastrear o estado do jogo:

```CSharp
Texture2D gameOverTexture;
bool gameOver;
```

Em seguida, inicialize **gameOver** no método **Initialize**:

```CSharp
gameOver = false;
```

Por fim, carregue a textura em **gameOverTexture** no método **LoadContent**:

```CSharp
gameOverTexture = Content.Load<Texture2D>("game-over");
```

### <a name="3-implement-game-over-logic"></a>3. Implemente a lógica "fim de jogo"
Adicione este código ao método **Update**, logo após o método **KeyboardHandler** ser chamado:

```CSharp
if (gameOver)
{
  dino.dX = 0;
  dino.dY = 0;
  broccoli.dX = 0;
  broccoli.dY = 0;
  broccoli.dA = 0;
}
```

Isso fará com que todo o movimento pare quando o jogo terminou, congelando os sprites do dinossauro e do brócolis em suas posições atuais.

Em seguida, no final do método **Update**, logo antes de **base. Update(gameTime)**, adicione esta linha:

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

Isso chama o método **RectangleCollision** que criamos na **SpriteClass** e sinaliza o jogo como over se retornar true.

### <a name="4-add-user-input-for-resetting-the-game"></a>4. Adicione a entrada do usuário para redefinir o jogo
Adicione este código ao método **KeyboardHandler** , para permitir que o usuário redefina o jogo se pressionar Enter:

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="5-draw-game-over-splash-and-text"></a>5. Desenhe a tela inicial e o texto fim de jogo
Por fim, adicione este código ao método Draw, logo após a primeira chamada de **spriteBatch.Draw** (essa deve ser a chamada que desenha a textura de grama).

```CSharp
if (gameOver)
{
  // Draw game over texture
  spriteBatch.Draw(gameOverTexture, new Vector2(screenWidth / 2 - gameOverTexture.Width / 2, screenHeight / 4 - gameOverTexture.Width / 2), Color.White);

  String pressEnter = "Press Enter to restart!";

  // Measure the size of text in the given font
  Vector2 pressEnterSize = stateFont.MeasureString(pressEnter);

  // Draw the text horizontally centered
  spriteBatch.DrawString(stateFont, pressEnter, new Vector2(screenWidth / 2 - pressEnterSize.X / 2, screenHeight - 200), Color.White);
}
```

Aqui, usamos o mesmo método de antes para desenhar texto centralizado horizontalmente (reutilizando a fonte que usamos na tela inicial Introdução), bem como a centralização **gameOverTexture** na metade superior da janela.

E pronto! Tente executar o jogo novamente. Se você tiver seguido as etapas acima, o jogo agora deve terminar quando o dinossauro colide com o brócolis, e o jogador deve ser solicitado a reiniciar o jogo pressionando a tecla Enter.

![Fim de jogo](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Publicar na Microsoft Store
Como criamos este jogo como um aplicativo UWP, é possível publicar este projeto na Microsoft Store. Há algumas etapas para o processo.

Você precisa estar [registrado](https://developer.microsoft.com/en-us/store/register) como desenvolvedor no Windows.

Você deve usar a [lista de verificação do envio de aplicativo](https://docs.microsoft.com/en-us/windows/uwp/publish/app-submissions).

O app deve ser enviado para [certificação](https://docs.microsoft.com/en-us/windows/uwp/publish/the-app-certification-process).

Para obter mais detalhes, consulte [Publicando seu aplicativo UWP](https://developer.microsoft.com/en-us/store/publish-apps).
