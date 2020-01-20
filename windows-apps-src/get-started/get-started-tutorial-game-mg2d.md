---
title: Criar um jogo UWP em MonoGame 2D
description: Um jogo UWP simples para a Microsoft Store, escrito em C# e MonoGame
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 5d5f7af2-41a9-4749-ad16-4503c64bb80c
ms.localizationpriority: medium
ms.openlocfilehash: 4300bdc0224a18874a7ff9153195f81f8bb8d101
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685055"
---
# <a name="create-a-uwp-game-in-monogame-2d"></a>Criar um jogo UWP em MonoGame 2D

## <a name="a-simple-2d-uwp-game-for-the-microsoft-store-written-in-c-and-monogame"></a>Um jogo UWP simples em 2D para a Microsoft Store, escrito em C# e MonoGame


![Folha de sprite do Walking Dino](images/JS2D_0.png)

## <a name="introduction"></a>Introdução

O MonoGame é uma estrutura leve de desenvolvimento de jogos. Este tutorial ensinará as noções básicas do desenvolvimento de jogos em MonoGame, incluindo como carregar o conteúdo, desenhar sprites, animá-los e manipular as entradas do usuário. Alguns conceitos mais avançados, como detecção de colisão e aumento da escala para telas de DPI alto, também são discutidos. Este tutorial dura de 30 a 60 minutos.

## <a name="prerequisites"></a>Pré-requisitos
+   Windows 10 e Microsoft Visual Studio 2019.  [Clique aqui para saber como fazer a preparação inicial com o Visual Studio](https://docs.microsoft.com/windows/uwp/get-started/get-set-up).
+ A estrutura de desenvolvimento de desktop .NET. Se ainda não a tiver instalada, você poderá obtê-la executando novamente o instalador do Visual Studio e modificando a instalação do Visual Studio 2019.
+   Conhecimento básico de C# ou de uma linguagem de programação similar orientada a objeto. [Clique aqui para saber como começar a usar o C#](https://docs.microsoft.com/windows/uwp/get-started/create-a-hello-world-app-xaml-universal).
+   É útil ter familiaridade com conceitos básicos da ciência da computação, como classes, métodos e variáveis.

## <a name="why-monogame"></a>Por que o MonoGame?
Não faltam opções quando se trata de ambientes de desenvolvimento de jogos. Pode ser difícil saber por onde começar quando se tem desde mecanismos repletos de recursos, como Unity, até APIs de multimídia abrangentes e complexas, como DirectX. O MonoGame é um conjunto de ferramentas, com um nível de complexidade que se situa entre um mecanismo de jogos e uma API mais robusta, como DirectX. Ele fornece um pipeline de conteúdo fácil de usar e todas as funcionalidades necessárias para criar jogos leves que são executados em uma ampla variedade de plataformas. Ainda melhor, os aplicativos MonoGame são escritos em C# puro, e é possível distribuí-los rapidamente por meio da Microsoft Store ou de outras plataformas de distribuição semelhantes.

## <a name="get-the-code"></a>Obter o código
Se não quiser seguir as etapas do tutorial passo a passo e quiser apenas ver o MonoGame em ação, [clique aqui para baixar o aplicativo finalizado](https://github.com/Microsoft/Windows-appsample-get-started-mg2d).

Abra o projeto no Visual Studio 2019 e pressione **F5** para executar o exemplo. A primeira vez que você fizer isso poderá demorar um pouco, pois o Visual Studio precisará buscar eventuais pacotes NuGet que estiverem faltando em sua instalação.

Se você tiver feito isso, passe a próxima seção sobre como configurar o MonoGame para ver um passo a passo do código.

**Observação**: o jogo criado nesta amostra não pretende ser completo (e nem divertido). Sua única finalidade é demonstrar todos os conceitos básicos do desenvolvimento 2D no MonoGame. Fique à vontade para usar o código e fazer algo muito melhor – ou apenas comece do zero após aprender as noções básicas!

## <a name="set-up-monogame-project"></a>Configurar o projeto do MonoGame
1. Instale o **MonoGame 3.6** para Visual Studio de [MonoGame.net](https://www.monogame.net/)

2. Inicie o Visual Studio 2019.

3. Vá para **Arquivo -> Novo -> Projeto**

4. Nos modelos de projeto do Visual C#, selecione **MonoGame** e **Projeto Universal do Windows 10 do MonoGame**

5. Nomeie o projeto "MonoGame2D" e selecione OK. Com o projeto criado, provavelmente vai parecer que está cheio de erros – eles deverão desaparecer depois que você executar o projeto pela primeira vez e que pacotes NuGet ausentes forem instalados.

6. Certifique-se de que **x86** e **Computador Local** estejam definidos como a plataforma de destino e pressione **F5** para criar e executar o projeto vazio. Se tiver seguido as etapas acima, você deverá ver uma janela azul vazia depois que o projeto terminar de ser criado.

## <a name="method-overview"></a>Visão geral do método
Agora que você criou o projeto, abra o arquivo **Game1.cs** do **Gerenciador de Soluções**. Este é o lugar onde a maior parte da lógica do jogo estará. Muitos métodos cruciais são gerados automaticamente aqui quando você cria um projeto do MonoGame. Vamos revisá-los rapidamente:

**public Game1()** O construtor. Nós não vamos alterar nada neste método para o tutorial.

**protected override void Initialize()** Aqui, inicializamos todas as variáveis de classe que serão usadas. Este método é chamado uma vez no início do jogo.

**protected override void LoadContent()** Este método carrega o conteúdo (p. ex. texturas, áudio, fontes) na memória antes de o jogo ser iniciado. Assim como Initialize, ele é chamado quando o aplicativo é iniciado.

**protected override void UnloadContent()** Este método é usado para descarregar conteúdo não pertencente ao gerenciador de conteúdo. Nós não o usamos.

**protected override void Update(GameTime gameTime)** Este método é chamado uma vez a cada ciclo do loop do jogo. Aqui, atualizamos os estados de qualquer objeto ou variável usada no jogo. Isso inclui itens como a posição, a velocidade ou a cor de um objeto. É aqui também que a entrada do usuário é manipulada. Em poucas palavras, esse método manipula cada parte da lógica do jogo, exceto pelo desenho de objetos na tela.

**protected override void Draw(GameTime gameTime)** É aqui que os objetos são desenhados na tela, usando as posições dadas pelo método Update.

## <a name="draw-a-sprite"></a>Desenhar um sprite
Então você executou seu novo projeto do MonoGame e encontrou um belo céu azul – vamos adicionar um chão.
No MonoGame, a arte 2D é adicionada ao aplicativo na forma de "sprites". Um sprite é apenas um gráfico de computador manipulado como uma única entidade. Os sprites podem ser movidos, dimensionados, moldados, animados e combinados para criar qualquer coisa que você possa imaginar no espaço 2D.

### <a name="1-download-a-texture"></a>1. Baixar uma textura
Para nossos objetivos, este primeiro sprite será extremamente tedioso. [Clique aqui para baixar este retângulo verde sem nada de especial](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/grass.png).

### <a name="2-add-the-texture-to-the-content-folder"></a>2. Adicionar a textura à pasta Conteúdo
- Abra o **Gerenciador de Soluções**
- Clique com o botão direito do mouse em **Content.mgcb** na pasta **Conteúdo** e selecione **Abrir com**. No menu pop-up, selecione **Pipeline do Monogame** e selecione **OK**.
- Na nova janela, clique com o botão direito do mouse no item **Conteúdo** e selecione **Adicionar -> Item Existente**.
- Localize e selecione o retângulo verde no navegador de arquivos.
- Nomeie o item "grass.png" e selecione **Adicionar**.

### <a name="3-add-class-variables"></a>3. Adicionar variáveis de classe
Para carregar esta imagem como uma textura de sprite, abra **Game1.cs** e adicione as seguintes variáveis de classe.

```CSharp
const float SKYRATIO = 2f/3f;
float screenWidth;
float screenHeight;
Texture2D grass;
```

A variável SKYRATIO nos informa quanto da cena queremos que seja céu em relação à grama – nesse caso, dois terços. **screenWidth** e **screenHeight** manterão o controle do tamanho da janela do aplicativo, enquanto **grass** é onde armazenaremos nosso retângulo verde.

### <a name="4-initialize-class-variables-and-set-window-size"></a>4. Inicializar as variáveis de classe e definir o tamanho da janela
As variáveis **screenWidth** e **screenHeight** ainda precisam ser inicializadas, sendo assim, adicione este código ao método **Initialize**:

```CSharp
ApplicationView.PreferredLaunchWindowingMode = ApplicationViewWindowingMode.FullScreen;

screenHeight = (float)ApplicationView.GetForCurrentView().VisibleBounds.Height;
screenWidth = (float)ApplicationView.GetForCurrentView().VisibleBounds.Width;

this.IsMouseVisible = false;
```

Além de obtermos a altura e a largura da tela, nós também definimos o modo de janelas do aplicativo como **Fullscreen** e deixamos o mouse invisível.

### <a name="5-load-the-texture"></a>5. Carregar a textura
Para carregar a textura na variável de grama, adicione o seguinte ao método **LoadContent**:

```CSharp
grass = Content.Load<Texture2D>("grass");
```

### <a name="6-draw-the-sprite"></a>6. Desenhar o sprite
Para desenhar o retângulo, adicione as seguintes linhas ao método **Draw**:

```CSharp
GraphicsDevice.Clear(Color.CornflowerBlue);
spriteBatch.Begin();
spriteBatch.Draw(grass, new Rectangle(0, (int)(screenHeight * SKYRATIO),
  (int)screenWidth, (int)screenHeight), Color.White);
spriteBatch.End();
```

Aqui, usamos o método **spriteBatch.Draw** para colocar a determinada textura dentro das bordas de um objeto Rectangle. A **Rectangle** é definido pelas coordenadas x e y de seu canto superior esquerdo e canto inferior direito. Usando as variáveis **screenWidth**, **screenHeight** e **SKYRATIO** que definimos anteriormente, desenhamos a textura do retângulo verde no terço inferior da tela. Se executar o programa agora, você deverá ver a tela de fundo azul de antes, parcialmente coberta pelo retângulo verde.

![Retângulo verde](images/monogame-tutorial-1.png)

## <a name="scale-to-high-dpi-screens"></a>Dimensionar para telas de DPI alto
Se estiver executando o Visual Studio em um monitor de alta densidade de pixels, como as encontradas em um Surface Pro ou Surface Studio, talvez você descubra que o retângulo verde das etapas acima não cobre todo o terço inferior da tela. Ele provavelmente está flutuando acima do canto inferior esquerdo da tela. Para corrigir isso e unificar a experiência de nosso jogo em todos os dispositivos, precisamos criar um método que dimensione alguns valores relativos à densidade de pixels da tela:

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

Se estiver usando uma tela de DPI alto e tentar executar o aplicativo agora, você deverá ver o retângulo verde cobrindo o terço inferior da tela como esperado.

## <a name="build-the-spriteclass"></a>Criar SpriteClass
Antes de começarmos a animar sprites, vamos fazer uma nova classe chamada "SpriteClass", que nos permitirá reduzir a complexidade do nível de superfície da manipulação de sprites.

### <a name="1-create-a-new-class"></a>1. Criar uma classe
No **Gerenciador de Soluções**, clique com o botão direito do mouse em **MonoGame2D (Universal do Windows)** e selecione **Adicionar -> Classe**. Dê à classe o nome "SpriteClass.cs" e selecione **Adicionar**.

### <a name="2-add-class-variables"></a>2. Adicionar variáveis de classe
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

Aqui, nós definimos as variáveis de classe de que precisamos para desenhar e animar um sprite. As variáveis **x** e **y** representam a posição atual do sprite no plano, enquanto a variável **angle** é o ângulo atual do sprite em graus (sendo 0 vertical, 90 inclinado em 90 graus no sentido horário). É importante observar que, para essa classe, **x** e **y** representam as coordenadas do **centro** do sprite, (a origem padrão é o canto superior esquerdo). Isso torna mais fácil girar os sprites, pois eles giram em torno de qualquer origem que receberem, e girar em torno do centro nos dá um movimento de rotação uniforme.

Depois disso, temos **dX**, **dY** e **dA**, que são as taxas de alteração por segundo, respectivamente, das variáveis **x**, **y** e **angle**.

### <a name="3-create-a-constructor"></a>3. Criar um construtor
Ao criar uma instância de **SpriteClass**, fornecemos ao construtor o dispositivo de elementos gráficos de **Game1.cs**, o caminho para a textura em relação à pasta do projeto e a escala desejada da textura em relação ao seu tamanho original. Vamos definir o restante das variáveis de classe depois de começarmos o jogo, no método Update.

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
Ainda existem há alguns métodos que precisamos adicionar à declaração de SpriteClass:

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

O método **Update** de SpriteClass é chamado no método **Update** de Game1.cs e é usado para atualizar os valores dos sprites **x**, **y** e **angle**, com base em suas respectivas taxas de alteração.

O método **Draw** é chamado no método **Draw** de Game1.cs e é usado para desenhar o sprite na janela do jogo.

## <a name="user-input-and-animation"></a>Entrada do usuário e animação
Agora, temos SpriteClass compilado e vamos usá-lo para criar dois novos objetos de jogo. O primeiro é um avatar que o jogador pode controlar com as teclas de seta e a barra de espaço. O segundo é um objeto que o jogador deve evitar.

### <a name="1-get-the-textures"></a>1. Obter as texturas
Para o avatar do jogador, vamos usar o gato ninja da própria Microsoft, montado em seu fiel amigo T-rex. [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/ninja-cat-dino.png).

Agora, o obstáculo que o jogador precisa evitar. O que gatos ninja e dinossauros carnívoros odeiam mais do que qualquer coisa? Comer verduras! [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/broccoli.png).

Como já fez antes com o retângulo verde, adicione essas imagens a **Content.mgcb** por meio do **Pipeline do MonoGame**, nomeando-as "ninja-cat-dino.png" e "broccoli.png", respectivamente.

### <a name="2-add-class-variables"></a>2. Adicionar variáveis de classe
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

**dino** e **broccoli** são nossas variáveis SpriteClass. **dino** conterá o avatar do jogador, enquanto **broccoli** conterá o obstáculo brócolis.

**spaceDown** monitora se a barra de espaço está sendo pressionada ou se foi pressionada e liberada.

**gameStarted** nos informa se o usuário iniciou o jogo pela primeira vez.

**broccoliSpeedMultiplier** determina a rapidez com que o obstáculo brócolis se move pela tela.

**gravitySpeed** determina a rapidez com que o avatar do jogador acelera para baixo após um salto.

**dinoSpeedX** e **dinoJumpY** determinam a velocidade com que o avatar do jogador se movimenta e salta.
score rastreia de quantos obstáculos o jogador se esquivou com êxito.

Por fim, **random** será usado para adicionar um pouco de imprevisibilidade ao comportamento do obstáculo brócolis.

### <a name="3-initialize-variables"></a>3. Inicializar as variáveis
Em seguida, precisamos inicializar essas variáveis. Adicione o seguinte código ao método Initialize:

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

Observe que as três últimas variáveis precisam ser dimensionadas para dispositivos com alto DPI, porque especificam uma taxa de alteração em pixels.

### <a name="4-construct-spriteclasses"></a>4. Construir SpriteClasses
Nós vamos construir objetos SpriteClass no método **LoadContent**. Adicione este código ao que você já tem:

```CSharp
dino = new SpriteClass(GraphicsDevice, "Content/ninja-cat-dino.png", ScaleToHighDPI(1f));
broccoli = new SpriteClass(GraphicsDevice, "Content/broccoli.png", ScaleToHighDPI(0.2f));
```

A imagem de brócolis é bem maior do que queremos que apareça no jogo, portanto, vamos reduzi-la para 0,2 vezes seu tamanho original.

### <a name="5-program-obstacle-behaviour"></a>5. Programar o comportamento do obstáculo
Queremos que os brócolis surjam de um lugar fora da tela e vão na direção do avatar do jogador para que ele precise se esquivar. Para fazer isso, adicione este método à classe **Game1.cs**:

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

A primeira parte do método determina de que ponto fora da tela o objeto brócolis vai surgir, usando dois números aleatórios.

A segunda parte determina a rapidez com que os brócolis se moverão, que é determinada pela pontuação atual. Ele ficará mais rápido a cada cinco brócolis de que o jogador conseguir se esquivar.

A terceira parte define a direção do movimento do sprite brócolis. Ele vai na direção do avatar do jogador (dinossauro) quando os brócolis são gerados. Nós também damos a ele um valor de **dA** de 7f, que fará os brócolis girarem no ar enquanto perseguem o jogador.

### <a name="6-program-game-starting-state"></a>6. Programar o estado inicial do jogo
Antes que possamos passar para a manipulação da entrada do teclado, precisamos de um método que defina o estado inicial do jogo dos dois objetos que criamos. Em vez de o jogo começar assim que o aplicativo for executado, queremos que o usuário o inicie manualmente, pressionando a barra de espaços. Adicione o código a seguir, que define o estado inicial dos objetos animados e redefine a pontuação:

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

### <a name="7-handle-keyboard-input"></a>7. Manipular a entrada do teclado
Depois, precisamos de um novo método para manipular a entrada do usuário por meio do teclado. Adicione o método a **Game1.cs**:

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

A primeira encerra o jogo quando a tecla **Escape** é pressionada.

A segunda inicia o jogo quando a tecla **Space** é pressionada e o jogo não foi iniciado.

A terceira faz o avatar de dinossauro pular quando **Space** é pressionado, alterando sua propriedade **dY**. Observe que o jogador não pode pular a menos que esteja no "chão" (dino.y = screenHeight * SKYRATIO) e também não vai pular se a tecla de espaço for mantida pressionada em vez de ser pressionada uma vez. Isso impede o dinossauro de saltar assim que o jogo é iniciado, utilizando o mesmo pressionamento de tecla que inicia o jogo.

Por fim, a última instrução if/else verifica se as setas direcionais para a esquerda ou a direita estão sendo pressionadas e, em caso afirmativo, altera a propriedade **dX** do dinossauro de acordo com o comando.

**Desafio:** você consegue fazer o método de manipulação de teclado acima funcionar com o esquema de entrada WASD, bem como as teclas de seta?

### <a name="8-add-logic-to-the-update-method"></a>8. Adicionar lógica ao método Update
Em seguida, precisamos adicionar lógica para todas essas partes ao método **Update** em **Game1.cs**:

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

### <a name="9-draw-spriteclass-objects"></a>9. Desenhar objetos SpriteClass
Por fim, adicione o seguinte código ao método **Draw** de **Game1.cs**, logo depois da última chamada de **spriteBatch.Draw**:

```CSharp
broccoli.Draw(spriteBatch);
dino.Draw(spriteBatch);
```

No MonoGame, novas chamadas de **spriteBatch.Draw** vão desenhar sobre todas as chamadas anteriores. Isso significa que os sprites de brócolis e de dinossauro serão desenhados sobre o sprite de grama existente. Portanto, eles nunca poderão se ocultar atrás dela, independentemente de sua posição.

Tente executar o jogo e movimentar o dinossauro usando as teclas de seta e a barra de espaços. Se tiver seguido as etapas acima, você deverá ser capaz de fazer com que o avatar se movimente dentro da janela do jogo e os brócolis deverão surgir com velocidade sempre crescente.

![Avatar do jogador e obstáculo](images/monogame-tutorial-2.png)

## <a name="render-text-with-spritefont"></a>Renderizar texto com SpriteFont
Usando o código acima, podemos manter o controle da pontuação do jogador nos bastidores, mas não dizemos ao jogador quantos pontos ele tem. Também temos uma introdução bem contraintuitiva quando o aplicativo é iniciado: o jogador vê uma janela azul e verde, mas não tem como saber que precisa pressionar Espaço para que o jogo comece.

Para corrigir esses dois problemas, vamos usar um novo tipo de objeto de MonoGame chamado **SpriteFonts**.

### <a name="1-create-spritefont-description-files"></a>1. Criar arquivos de descrição SpriteFont
No **Gerenciador de Soluções**, encontre a pasta **Conteúdo**. Nesta pasta, clique com o botão direito do mouse no arquivo **Content.mgcb** e selecione **Abrir com**. No menu pop-up, selecione **Pipeline do MonoGame** e, em seguida, pressione **OK**. Na nova janela, clique com o botão direito do mouse no item **Conteúdo** e selecione **Adicionar -> Novo Item**. Selecione **SpriteFont Description**, dê ao objeto o nome "Score" e pressione **OK**. Em seguida, adicione outra descrição de SpriteFont denominada "GameState" usando o mesmo procedimento.

### <a name="2-edit-descriptions"></a>2. Editar descrições
Clique com o botão direito do mouse na pasta **Conteúdo** no **Pipeline do MonoGame** e selecione **Abrir local do arquivo**. Você deverá ver uma pasta com os arquivos de descrição SpriteFont que acabou de criar, bem como as imagens que adicionou à pasta Conteúdo até agora. Agora, você pode fechar e salvar a janela do Pipeline do MonoGame. No **Explorador de Arquivos**, abra ambos os arquivos de descrição em um editor de texto (Visual Studio, NotePad++, Atom etc.).

Cada descrição contém vários valores que descrevem o SpriteFont. Vamos fazer algumas alterações:

Em **Score.spritefont**, altere o valor **<Size>** de 12 para 36.

Em **GameState.spritefont**, altere o valor **<Size>** de 12 para 72 e o valor **<FontName>** de Arial para Agency. Agency é outra fonte que vem por padrão com computadores Windows 10 e dará um toque de arte a nossa tela Introdução.

### <a name="3-load-spritefonts"></a>3. Carregar SpriteFonts
De volta ao Visual Studio, primeiro vamos adicionar uma nova textura à tela inicial de introdução. [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/start-splash.png).

Como antes, adicione a textura ao projeto clicando com o botão direito do mouse em Conteúdo e selecionando **Adicionar -> Item existente**. Dê ao novo item o nome “start-splash.png”.

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

### <a name="4-draw-the-score"></a>4. Desenhar a pontuação
Vá para o método **Draw** de **Game1.cs** e adicione o código a seguir logo antes de **spriteBatch.End();**

```CSharp
spriteBatch.DrawString(scoreFont, score.ToString(),
new Vector2(screenWidth - 100, 50), Color.Black);
```

O código acima usa a descrição de sprite que criamos (Arial tamanho 36) para informar a pontuação atual do jogador perto do canto superior direito da tela.

### <a name="5-draw-horizontally-centered-text"></a>5. Desenhar um texto centralizado horizontalmente
Ao fazer um jogo, muitas vezes você desejará desenhar texto centralizado, seja horizontal ou verticalmente. Para centralizar horizontalmente o texto introdutório, adicione este código ao método **Draw** logo antes de **spriteBatch.End();**

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

Primeiro, criamos duas cadeias de caracteres, uma para cada linha de texto que queremos desenhar. Em seguida, medimos a largura e a altura de cada linha quando impressa, usando o método **SpriteFont.MeasureString(String)** . Isso nos dá o tamanho como um objeto **Vector2**, com a propriedade **X** que contém a largura e **Y** que contém a altura.

Por fim, desenhamos cada linha. Para centralizar o texto horizontalmente, tornamos o valor **X** de seu vetor de posição igual a **screenWidth / 2 – textSize.X / 2**.

**Desafio:** como você alteraria o procedimento descrito acima para centralizar o texto verticalmente e também horizontalmente?

Experimente executar o jogo. Você vê a tela inicial de introdução? A pontuação aumenta a cada vez que os brócolis aparecem?

![Tela inicial de introdução](images/monogame-tutorial-3.png)

## <a name="collision-detection"></a>Detecção de colisão
Então, nós temos brócolis que o segue por toda parte e ainda temos uma pontuação que sobe a cada vez que aparece um novo – mas do jeito que está, não existe de fato uma maneira de perder esse jogo. Precisamos de uma maneira de saber se os sprites do dinossauro e dos brócolis colidem para que, caso isso aconteça, possamos declarar fim de jogo.

### <a name="1-get-the-textures"></a>1. Obter as texturas
A última imagem de que precisamos é uma para "game over". [Clique aqui para baixar a imagem](https://github.com/Microsoft/Windows-appsample-get-started-mg2d/blob/master/MonoGame2D/Content/game-over.png).

Como já fez com as imagens de retângulo verde, gato ninja e brócolis, adicione essa imagem a **Content.mgcb** por meio do **Pipeline do MonoGame**, dando a ela o nome "game-over.png".

### <a name="2-rectangular-collision"></a>2. Colisão retangular
Para detectar colisões em um jogo, os objetos geralmente são simplificados para reduzir a complexidade dos cálculos envolvidos. Vamos tratar tanto o avatar do jogador e o obstáculo brócolis como retângulos para fins de detecção de colisão entre eles.

Abra **SpriteClass.cs** e adicione uma nova variável de classe:

```CSharp
const float HITBOXSCALE = .5f;
```

Esse valor representará o quanto a detecção de colisão é "permissiva" para o jogador. Com um valor de 0,5 f, as bordas do retângulo em que o dinossauro pode colidir com os brócolis – muitas vezes chamado de "hitbox" – será metade do tamanho total da textura. Isso resultará em alguns casos em que os cantos das duas texturas colidem, sem que nenhuma parte das imagens realmente pareçam se tocar. Fique à vontade para ajustar esse valor conforme seu gosto pessoal.

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

Esse método detecta se dois objetos retangulares colidiram. O algoritmo faz testes para ver se há uma lacuna entre qualquer uma das laterais dos retângulos. Se houver uma lacuna, não haverá nenhuma colisão – se não houver uma lacuna, terá de haver uma colisão.

### <a name="3-load-new-textures"></a>3. Carregar novas texturas

Em seguida, abra **Game1.cs** e adicione duas novas variáveis de classe, uma para armazenar a textura do sprite fim de jogo e um booliano para rastrear o estado do jogo:

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

### <a name="4-implement-game-over-logic"></a>4. Implementar a lógica de "fim de jogo"
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

Isso fará com que todo o movimento pare quando o jogo terminar, congelando os sprites do dinossauro e dos brócolis em suas posições atuais.

Em seguida, no final do método **Update**, logo antes de **base. Update(gameTime)** , adicione esta linha:

```CSharp
if (dino.RectangleCollision(broccoli)) gameOver = true;
```

Isso chama o método **RectangleCollision** que criamos na **SpriteClass** e sinaliza o jogo como encerrado caso retorne true.

### <a name="5-add-user-input-for-resetting-the-game"></a>5. Adicionar a entrada do usuário para redefinir o jogo
Adicione este código ao método **KeyboardHandler** para permitir que o usuário redefina o jogo se pressionar Enter:

```CSharp
if (gameOver && state.IsKeyDown(Keys.Enter))
{
  StartGame();
  gameOver = false;
}
```

### <a name="6-draw-game-over-splash-and-text"></a>6. Desenhar a tela inicial e o texto de fim de jogo
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

E pronto! Tente executar o jogo novamente. Se você tiver seguido as etapas acima, o jogo agora deverá terminar quando o dinossauro colidir com os brócolis, e o jogador deverá ser solicitado a reiniciar o jogo pressionando a tecla Enter.

![Fim de jogo](images/monogame-tutorial-4.png)

## <a name="publish-to-the-microsoft-store"></a>Publicar na Microsoft Store
Como criamos este jogo como um aplicativo UWP, é possível publicar o projeto na Microsoft Store. Há algumas etapas no processo.

Você precisa estar [registrado](https://developer.microsoft.com/store/register) como Desenvolvedor do Windows.

Você deve usar a [lista de verificação de envio de aplicativo](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

O aplicativo deve ser enviado para [certificação](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process).

Para obter mais detalhes, confira [Publicar seu aplicativo UWP](https://docs.microsoft.com/windows/uwp/publish/).
