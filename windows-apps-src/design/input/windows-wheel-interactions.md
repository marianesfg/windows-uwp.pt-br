---
author: Karl-Bridge-Microsoft
Description: Incorporate speech into your apps using Cortana voice commands, speech recognition, and speech synthesis.
title: Interações com o Surface Dial
label: Surface Dial interactions
template: detail.hbs
keywords: Surface Dial, roda do Windows, RadialController, controle Radial, interação do usuário, entrada
ms.author: kbridge
ms.date: 02/08/2017
ms.topic: article
ms.assetid: e7deb1d6-feeb-471e-9a83-26386d1aaf37
ms.localizationpriority: medium
ms.openlocfilehash: 0a360bf936843450f5646c0e4e03ad9c3bac34d2
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/14/2018
ms.locfileid: "6670135"
---
# <a name="surface-dial-interactions"></a>Interações com o Surface Dial

![Imagem do Surface Dial com Surface Studio](images/windows-wheel/dial-pen-studio-600px.png)  
*Surface Dial com Surface Studio e caneta* (disponível para compra na [Microsoft Store](https://aka.ms/purchasesurfacedial)).

## <a name="overview"></a>Visão geral

Os dispositivos de rolagem do Windows, como o Surface Dial, são uma nova categoria de dispositivo de entrada que proporcionam uma série de experiências de interação do usuário atraentes e exclusivas para Windows e aplicativos do Windows. 

> [!IMPORTANT]
> Neste tópico, nos referimos especificamente a interações com o Surface Dial, mas as informações são aplicáveis a todos os dispositivos de rolagem do Windows. 

| Vídeos |   |
| --- | --- |
| <iframe src="https://www.youtube-nocookie.com/embed/WMklcdzcNcU" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> | <iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Programming-the-Microsoft-Surface-Dial/player" width="300" height="200" allowFullScreen="true" frameBorder="0"></iframe> |
| *Parceiros de aplicativo do Surface Dial* | *Surface Dial para desenvolvedores* |

Com um fator forma com base em uma ação (ou gesto) *girar*, o Surface Dial destina-se como um dispositivo de entrada secundário para vários tipos de mídia que complementa a entrada de um dispositivo principal. Na maioria dos casos, o dispositivo é manipulado pela mão não dominante de um usuário durante a execução de uma tarefa com a mão dominante (por exemplo, escrita à tinta com uma caneta). Ele não foi projetado para entrada de ponteiro de precisão (por exemplo, toque, caneta ou mouse). 

O Surface Dial também dá suporte às ações *pressionar e segurar* e *clicar*. Pressionar e segurar tem uma única função: exibir um menu de comandos. Se o menu estiver ativo, a entrada por girar e clicar será processada pelo menu. Caso contrário, a entrada será transmitida ao seu aplicativo para processamento. 

**Assim como acontece com todos os dispositivos de entrada do Windows, você pode personalizar e adaptar a experiência de interação com o Surface Dial de acordo com a funcionalidade em seus aplicativos.**

> [!TIP]
> Usados juntos, o Surface Dial e o novo Surface Studio podem fornecer uma experiência de usuário ainda mais distinta.  
>
>Além da experiência do menu padrão de pressionar e segurar descrita, o Surface Dial também pode ser colocado diretamente na tela do Surface Studio. Isso ativa um menu "na tela" especial. 
>
>Detectando o local do contato e os limites do Surface Dial, o sistema usa essas informações para lidar com oclusão pelo dispositivo e exibir uma versão maior do menu que encapsula a parte de fora do Dial. Essas mesmas informações também podem ser usadas pelo seu aplicativo para adaptar a interface do usuário para a presença do dispositivo e seu uso previsto, como o posicionamento da mão e do braço do usuário.

| Menu fora da tela do Surface Dial | | Menu na tela do Surface Dial |
| --- | --- | --- |
| ![Menu fora da tela do Surface Dial](images/windows-wheel/surface-dial-menu-offscreen.png) | | ![Menu na tela do Surface Dial](images/windows-wheel/surface-dial-menu-onscreen.png) |

## <a name="system-integration"></a>Integração do sistema

O Surface Dial está totalmente integrado ao Windows e dá suporte a um conjunto de ferramentas internas no menu: volume do sistema, rolar, aumentar/diminuir zoom e desfazer/refazer.

Essa coleção de ferramentas internas se adapta ao contexto atual do sistema para incluir:
- Uma ferramenta de brilho do sistema quando o usuário está na área de trabalho do Windows
- Uma ferramenta de faixa anterior/próxima durante a reprodução de mídia

Além desse suporte de plataforma geral, o Surface Dial também integra-se aos controles da plataforma Windows Ink ([**InkCanvas**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkCanvas) e [**InkToolbar**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.InkToolbar)).

![Surface Dial com caneta Surface](images/windows-wheel/dial-and-pen-400px.png)  
*Surface Dial com caneta Surface*

Quando usados com o Surface Dial, esses controles permitem funcionalidade adicional para modificar os atributos de tinta e controlar o estêncil da régua da barra de ferramentas de tinta.

Quando você abre o Menu do Surface Dial em um aplicativo de escrita à tinta que usa a barra de ferramentas de tinta, o menu agora inclui ferramentas para controlar a espessura do pincel e o tipo de caneta. Quando a régua está habilitada, uma ferramenta correspondente é adicionada ao menu que permite que o dispositivo controle a posição e o ângulo da régua.

![Menu do Surface Dial com a ferramenta de seleção de caneta para a barra de ferramentas do Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-pen.png)  
*Menu do Surface Dial com a ferramenta de seleção de caneta para a barra de ferramentas do Windows Ink*

![Menu do Surface Dial com a ferramenta de tamanho de traço para a barra de ferramentas do Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-strokesize.png)  
*Menu do Surface Dial com a ferramenta de tamanho de traço para a barra de ferramentas do Windows Ink*

![Menu do Surface Dial com a ferramenta de régua da barra de ferramentas do Windows Ink](images/windows-wheel/surface-dial-menu-inktoolbar-ruler.png)  
*Menu do Surface Dial com a ferramenta de régua da barra de ferramentas do Windows Ink*

## <a name="user-customization"></a>Personalização do usuário

Os usuários podem personalizar alguns aspectos da experiência do Surface Dial por meio da página **Configurações do Windows -> Dispositivos -> Roda**, incluindo ferramentas padrão, vibração (ou comentários hápticos) e mão de escrita (ou dominante). 

Ao personalizar a experiência do usuário do Surface Dial, você deve sempre garantir que uma função ou comportamento específico esteja disponível e habilitado pelo usuário.

## <a name="custom-tools"></a>Ferramentas personalizadas

Aqui vamos discutir diretrizes de experiência do usuário e desenvolvedor para personalizar as ferramentas expostas no menu do Surface Dial.

### <a name="ux-guidance"></a>Diretrizes de experiência do usuário

**Verifique se suas ferramentas correspondem ao contexto atual** Quando você deixa claro e intuitivo o que uma ferramenta faz e como funciona a interação do Surface Dial, você ajuda os usuários a aprender rapidamente e manter o foco na tarefa.

**Minimizar o número de ferramentas de aplicativo o máximo possível**  
O menu do Surface Dial tem espaço para sete itens. Se houver oito ou mais itens, o usuário precisará ativar o Dial para ver quais ferramentas estão disponíveis em um submenu de estouro, dificultando a navegação do menu e a detecção e seleção de ferramentas.

É recomendável fornecer uma única ferramenta personalizada para seu aplicativo ou contexto do aplicativo. Fazer isso permite que você defina essa ferramenta com base no que o usuário está fazendo sem exigir que ele ative o menu do Surface Dial e selecione uma ferramenta. 

**Atualizar dinamicamente a coleção de ferramentas**  
Como os itens de menu do Surface Dial não oferecem suporte a um estado desativado, você deve adicionar e remover dinamicamente ferramentas (incluindo ferramentas padrão internas) com base no contexto do usuário (modo de exibição atual ou janela focada). Se uma ferramenta não for relevante à atividade atual ou for redundante, remova-a.

> [!IMPORTANT]
> Quando você adicionar um item ao menu, verifique se o item ainda não existe.

**Não remova a ferramenta interna de configuração de volume do sistema**  
O controle de volume normalmente sempre é exigido pelo usuário. Ele pode ouvir música enquanto estiver usando seu aplicativo e, assim, as ferramentas de volume e próxima faixa sempre devem estar acessíveis no menu do Surface Dial. (A ferramenta da próxima faixa é automaticamente adicionada ao menu durante a reprodução de mídia.)

**Seja consistente com a organização do menu**  
Isso ajuda os usuários a descobrir e saber quais ferramentas estão disponíveis ao usar seu aplicativo e ajuda a melhorar sua eficiência ao alternar ferramentas.

**Fornecer ícones de alta qualidade consistentes com os ícones incorporados**  
Os ícones podem transmitir profissionalismo e excelência, além de inspirar confiança nos usuários.
- Fornecer uma imagem PNG de 64 x 64 pixels de alta qualidade (44 x 44 é o menor com suporte)
- Verifique se a tela de fundo é transparente
- O ícone deve preencher a maior parte da imagem
- Um ícone branco deve ter um contorno preto que fique visível no modo de alto contraste

|   |   |   |
| --- | --- | --- |
| ![Ícone com segundo plano em alfa](images/windows-wheel/surface-dial-menu-icon1.png) | ![Ícone exibido no menu de roda de rolagem com o ícone de tema padrão](images/windows-wheel/surface-dial-menu-icon2.png) | ![Menu na tela do Surface Dial](images/windows-wheel/surface-dial-menu-icon3.png) |
| *Ícone com segundo plano em alfa* | *Ícone exibido no menu de roda de rolagem com o tema padrão* | *Ícone exibido no menu de roda de rolagem com o tema branco em alto contraste* |

**Usar nomes descritivos e concisos**  
O nome da ferramenta é exibido no menu Ferramentas, juntamente com o ícone da ferramenta, e também é usado pelos leitores de tela. 
- Os nomes devem ser curtos para caber dentro do círculo central do menu de roda de rolagem
- Os nomes devem identificar claramente a ação principal (uma ação complementar pode ser imposta):
  - Rolagem indica o efeito de ambas as direções de rotação
  - Desfazer especifica uma ação principal, mas Refazer (a ação complementar) pode ser inferido e descoberto facilmente pelo usuário

### <a name="developer-guidance"></a>Diretrizes do desenvolvedor

Você pode personalizar a experiência do Surface Dial para complementar a funcionalidade em seus aplicativos por meio de um conjunto abrangente de [APIs do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController). 

Conforme mencionado anteriormente, o menu do Surface Dial padrão é previamente populado com um conjunto de ferramentas internas que abrangem uma ampla gama de recursos básicos do sistema (volume do sistema, brilho do sistema, rolagem, zoom, desfazer e controle de mídia quando o sistema detecta áudio em andamento ou reprodução de vídeo). No entanto, essas ferramentas padrão não podem fornecer a funcionalidade necessária pelo seu aplicativo. 

Nas seções a seguir, descrevemos como adicionar uma ferramenta personalizada ao menu do Surface Dial e especificar quais ferramentas internas são expostas.

**Adicionar uma ferramenta personalizada**

Neste exemplo, vamos adicionar uma ferramenta personalizada básica que passa os dados de entrada de eventos de rotação e clique para alguns controles da interface XAML.

1. Primeiro, nós declaramos nossa interface do usuário (apenas um controle deslizante e switch de alternância) em XAML.

   ![Imagem da interface do aplicativo de exemplo](images/windows-wheel/surface-dial-snippet-customtool1.png)  
   *A interface do usuário do aplicativo de exemplo*

    ```Xaml
    <Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
      <Grid.RowDefinitions>
        <RowDefinition Height="Auto"/>
        <RowDefinition Height="*"/>
      </Grid.RowDefinitions>
      <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
          <TextBlock x:Name="Header"
            Text="RadialController customization sample"
            VerticalAlignment="Center"
            Style="{ThemeResource HeaderTextBlockStyle}"
            Margin="10,0,0,0" />
      </StackPanel>
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center"
        Grid.Row="1">
          <!-- Slider for rotation input -->
          <Slider x:Name="RotationSlider"
            Width="300"
            HorizontalAlignment="Left"/>
          <!-- Switch for click input -->
          <ToggleSwitch x:Name="ButtonToggle"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    ```

2. Em seguida, no code-behind, adicionamos uma ferramenta personalizada ao menu do Surface Dial e declaramos os manipuladores de entrada [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController). 

   Podemos obter uma referência ao objeto [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) para o Surface Dial (myController) chamando [**CreateForCurrentView**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.CreateForCurrentView).

   Em seguida, criamos uma instância de um [**RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) (myItem) chamando [**RadialControllerMenuItem.CreateFromIcon**](https://msdn.microsoft.com/library/windows/apps/mt759255). 

   Em seguida, acrescentamos esse item à coleção de itens de menu.

   Declaramos os manipuladores de eventos de entrada ([**ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) e [**RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged)) para o objeto [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController).

   Por fim, definimos os manipuladores de eventos.

    ```csharp
    public sealed partial class MainPage : Page
    {
        RadialController myController;

        public MainPage()
        {
            this.InitializeComponent();
            // Create a reference to the RadialController.
            myController = RadialController.CreateForCurrentView();

            // Create an icon for the custom tool.
            RandomAccessStreamReference icon =
              RandomAccessStreamReference.CreateFromUri(
                new Uri("ms-appx:///Assets/StoreLogo.png"));

            // Create a menu item for the custom tool.
            RadialControllerMenuItem myItem =
              RadialControllerMenuItem.CreateFromIcon("Sample", icon);

            // Add the custom tool to the RadialController menu.
            myController.Menu.Items.Add(myItem);

            // Declare input handlers for the RadialController.
            myController.ButtonClicked += MyController_ButtonClicked;
            myController.RotationChanged += MyController_RotationChanged;
        }

        // Handler for rotation input from the RadialController.
        private void MyController_RotationChanged(RadialController sender,
          RadialControllerRotationChangedEventArgs args)
        {
            if (RotationSlider.Value + args.RotationDeltaInDegrees > 100)
            {
                RotationSlider.Value = 100;
                return;
            }
            else if (RotationSlider.Value + args.RotationDeltaInDegrees < 0)
            {
                RotationSlider.Value = 0;
                return;
            }
            RotationSlider.Value += args.RotationDeltaInDegrees;
        }

        // Handler for click input from the RadialController.
        private void MyController_ButtonClicked(RadialController sender,
          RadialControllerButtonClickedEventArgs args)
        {
            ButtonToggle.IsOn = !ButtonToggle.IsOn;
        }
    }
    ```

Ao executar o aplicativo, usamos o Surface Dial para interagir com ele. Primeiro, pressionamos e seguramos para abrir o menu e selecionamos nossa ferramenta personalizada. Depois que a ferramenta personalizada é ativada, o controle deslizante pode ser ajustado girando o Dial e o switch pode ser alternado clicando no Dial.

![Imagem da interface do usuário do aplicativo de exemplo ativado usando a ferramenta personalizada do Surface Dial](images/windows-wheel/surface-dial-snippet-customtool2.png)  
*A interface do usuário do aplicativo de exemplo ativado usando a ferramenta personalizada do Surface Dial*

**Especificar as ferramentas internas**

Você pode usar a classe [**RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) para personalizar a coleção de itens de menu internos para o seu aplicativo.

Por exemplo, se seu aplicativo não tem regiões de rolagem ou zoom e não exige funcionalidade de desfazer/refazer, essas ferramentas podem ser removidas do menu. Isso abre espaço no menu para adicionar ferramentas personalizadas ao seu aplicativo. 

> [!IMPORTANT] 
> O menu do Surface Dial deve ter pelo menos um item de menu. Se todas as ferramentas padrão forem removidas antes de adicionar uma das suas ferramentas personalizadas, as ferramentas padrão serão restauradas e sua ferramenta será acrescentada à coleção padrão.

Segundo as diretrizes de design, não recomendamos remover as ferramentas de controle de mídia (volume e faixa anterior/próxima), pois os usuários costumam ouvir música em segundo enquanto executam outras tarefas.

Aqui, mostramos como configurar o menu do Surface Dial para incluir somente controles de mídia de volume e faixa anterior/seguinte.

```csharp
public MainPage()
{
  ...
  //Remove a subset of the default system tools
  RadialControllerConfiguration myConfiguration = 
  RadialControllerConfiguration.GetForCurrentView();
  myConfiguration.SetDefaultMenuItems(new[] 
  {
    RadialControllerSystemMenuItemKind.Volume,
      RadialControllerSystemMenuItemKind.NextPreviousTrack
  });
}
```

## <a name="custom-interactions"></a>Interações personalizadas

Conforme mencionado, o Surface Dial dá suporte a três gestos (pressionar e segurar, girar, clicar) com interações padrão correspondentes. 

Verifique se as interações personalizadas com base nesses gestos fazem sentido para a ação ou a ferramenta selecionada. 

> [!NOTE]
> A experiência de interação depende do estado do menu do Surface Dial. Se o menu estiver ativo, processará a entrada. Caso contrário, seu aplicativo fará isso.

### <a name="press-and-hold"></a>Pressionar e segurar

Esse gesto ativa e mostra o menu do Surface Dial; não há nenhuma funcionalidade de aplicativo associada a esse gesto. 

Por padrão, o menu é exibido no centro da tela do usuário. No entanto, o usuário pode capturá-lo e movê-lo para qualquer lugar.

> [!NOTE]
> Quando o Surface Dial é colocado na tela do Surface Studio, o menu é centralizado na tela do Surface Dial.

### <a name="rotate"></a>Girar

O Surface Dial foi projetado principalmente para dar suporte à rotação para interações que envolvem ajustes suaves e incrementais em valores ou controles analógicos.

O dispositivo pode ser girado no sentido horário e no sentido anti-horário, e também pode fornecer comentários hápticos para indicar distâncias discretas.

> [!NOTE]
> Os comentários hápticos podem ser desabilitados pelo usuário na página **Configurações do Windows -> Dispositivos -> Roda**.

#### <a name="ux-guidance"></a>Diretrizes de experiência do usuário

**Ferramentas com sensibilidade rotacional contínua ou alta devem desabilitar comentários hápticos**

Os comentários hápticos correspondem à sensibilidade rotacional da ferramenta ativa. Recomendamos desabilitar os comentários hápticos para ferramentas com sensibilidade rotacional contínua ou alta, pois a experiência do usuário pode ser prejudicada. 

**A mão dominante não deve afetar interações baseadas em rotação**

O Surface Dial não consegue detectar qual mão está sendo usada, mas o usuário pode definir a mão de escrita (ou dominante) em **Configurações do Windows -> Dispositivo -> Caneta e Windows Ink**.

**A localidade deve ser considerada para todas as interações de rotação**

Maximize a satisfação do cliente acomodado e adaptando suas interações a localidade e layouts da direita para a esquerda.

As ferramentas internas e os comandos no menu do Surface Dial seguem estas diretrizes para interações baseadas em rotação:

|   |   |   |
| --- | --- | --- |
| Esquerda<br/>Para cima<br/>Fora | ![Imagem do Surface Dial](images/windows-wheel/surface-dial-rotate.png) | Direita<br/>Para baixo<br/>Dentro |
|   |   |   |

| Direção conceitual | Mapeamento para o Surface Dial | Rotação no sentido horário | Rotação no sentido anti-horário |
| --- | --- | --- | --- |
| Horizontal | Mapeamento esquerdo e direito com base na parte superior do Surface Dial | Direita | Esquerda |
| Vertical | Mapeamento para cima e para baixo com base no lado esquerdo do Surface Dial | Para baixo | Para cima |
| Eixo Z | Dentro (ou mais próximo) mapeado para cima/direita<br/>Fora (ou mais) mapeado para baixo/esquerda | Dentro | Fora |

#### <a name="developer-guidance"></a>Diretrizes do desenvolvedor

Conforme o usuário gira o dispositivo, os eventos [**RadialController.RotationChanged**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationChanged) são disparados com base em um delta ([**RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs.RotationDeltaInDegrees)) em relação à direção de rotação. A sensibilidade (ou resolução) dos dados pode ser definida com a propriedade [**RadialController.RotationResolutionInDegrees**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.RotationResolutionInDegrees).

> [!NOTE]
> Por padrão, um evento de entrada rotacional é fornecido para um objeto [**RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController) somente quando o dispositivo é girado no mínimo 10 graus. Cada evento de entrada faz com que o dispositivo vibre.

Em geral, é recomendável desabilitar comentários hápticos quando a resolução de rotação é definida como menos de 5 graus. Isso proporciona uma experiência mais uniforme para interações contínuas. 

Você pode habilitar e desabilitar comentários hápticos para ferramentas personalizadas definindo a propriedade [**RadialController.UseAutomaticHapticFeedback**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.UseAutomaticHapticFeedback).

> [!NOTE]
> Você não pode substituir o comportamento háptico para ferramentas do sistema como o controle de volume. Para essas ferramentas, os comentários hápticos podem ser desabilitados somente pelo usuário na página de configurações de roda de rolagem.

Veja um exemplo de como personalizar a resolução dos dados de rotação e habilitar ou desabilitar comentários hápticos.

```csharp
private void MyController_ButtonClicked(RadialController sender, 
  RadialControllerButtonClickedEventArgs args)
{
  ButtonToggle.IsOn = !ButtonToggle.IsOn;

  if(ButtonToggle.IsOn)
  {
    //high resolution mode
    RotationSlider.LargeChange = 1;
    myController.UseAutomaticHapticFeedback = false;
    myController.RotationResolutionInDegrees = 1;
  }
  else
  {
    //low resolution mode
    RotationSlider.LargeChange = 10;
    myController.UseAutomaticHapticFeedback = true;
    myController.RotationResolutionInDegrees = 10;
  }
}
```

### <a name="click"></a>Clicar

Clicar no Surface Dial é semelhante a clicar no botão esquerdo do mouse (o estado de rotação do dispositivo não tem efeito sobre essa ação).

#### <a name="ux-guidance"></a>Diretrizes de experiência do usuário

**Não mapeie uma ação ou comando para esse gesto se o usuário não puder recuperar facilmente do resultado**

Qualquer ação executada pelo seu aplicativo com base no usuário que clica no Surface Dial deve ser reversível. Sempre permita que o usuário volte facilmente no aplicativo e restaure um estado anterior do aplicativo.

Operações binárias como ativar/desativar notificação ou mostrar/ocultar fornecem boas experiências de usuário com o gesto de clique.

**Ferramentas modais não devem ser ativadas ou desabilitadas clicando no Surface Dial**

Alguns modos de aplicativo/ferramenta podem entrar em conflito com ou desabilitar interações que dependem de rotação. Ferramentas como a régua na barra de ferramentas do Windows Ink devem ser ativadas ou desativadas por meio de outras funcionalidades de interface do usuário (a barra de ferramentas do Windows Ink fornece um controle [**ToggleButton**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Xaml.Controls.Primitives.ToggleButton) interno).

Para ferramentas modais, mapeie o item de menu ativo do Surface Dial para a ferramenta de destino ou para o item de menu selecionado anteriormente.

#### <a name="developer-guidance"></a>Diretrizes do desenvolvedor

Quando o Surface Dial é clicado, um evento [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) é acionado. Os [**RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs) incluem uma propriedade [**Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact) que contém o local e a área delimitadora do contato do Surface Dial na tela do Surface Studio. Se o Surface Dial não estiver em contato com a tela, essa propriedade será nula. 

### <a name="on-screen"></a>Na tela

Conforme descrito anteriormente, o Surface Dial pode ser usado em conjunto com o Surface Studio para exibir o menu do Surface Dial em um modo na tela especial. 

Nesse modo, você pode integrar e personalizar suas experiências de interação do Dial com seus aplicativos ainda mais. Exemplos de experiências únicas só possíveis com o Surface Dial e o Surface Studio incluem:
- Exibir ferramentas contextuais (por exemplo, uma paleta de cores) com base na posição do Surface Dial, o que facilita localização e uso
- Definir a ferramenta ativa com base na interface do usuário em que o Surface Dial é colocado
- Ampliar uma área da tela com base no local do Surface Dial
- Interações exclusivas de jogos com base no local da tela

#### <a name="ux-guidance"></a>Diretrizes de experiência do usuário

**Os aplicativos devem responder quando o Surface Dial é detectado na tela**

O feedback visual ajuda a indicar aos usuários que seu aplicativo detectou o dispositivo na tela do Surface Studio.

**Ajustar a interface do usuário relacionada ao Surface Dial com base na localização do dispositivo**

O dispositivo (e o corpo do usuário) pode ocultar a interface do usuário essencial dependendo de onde o usuário o coloca.

**Ajustar a interface do usuário relacionada ao Surface Dial com base na interação do usuário**

Além de oclusão de hardware, a mão e o braço de um usuário podem ocultar parte da tela ao usar o dispositivo. 

A área obstruída depende de qual mão está sendo usada com o dispositivo. Como o dispositivo foi projetado para ser usado principalmente com a mão não dominante, a interface do usuário relacionada ao Surface Dial deve ser ajustada para a mão oposta especificada pelo usuário (**Configurações do Windows > Dispositivos > Caneta e Windows Ink > Escolher com qual mão você escreve**).

**As interações devem responder à posição do Surface Dial em vez de movimento**

O pé do dispositivo foi projetado para aderir à tela em vez de deslizar, pois não é um dispositivo apontador de precisão. Portanto, esperamos que seja mais comum que os usuários levantem e coloquem o Surface Dial em vez de arrastá-lo pela tela.

**Usar a posição da tela para determinar a intenção do usuário**

Definir a ferramenta ativa com base no contexto da interface do usuário, como proximidade com um controle, tela ou janela, pode melhorar a experiência do usuário, reduzindo as etapas necessárias para executar uma tarefa.

#### <a name="developer-guidance"></a>Diretrizes do desenvolvedor

Quando o Surface Dial é colocado na superfície de digitalizador do Surface Studio, um evento [**RadialController.ScreenContactStarted**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ScreenContactStarted) é acionado e as informações de contato ([**RadialControllerScreenContactStartedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs.Contact)) são fornecidas para seu aplicativo.

Da mesma forma, se o Surface Dial é clicado quando em contato com a superfície do digitalizador do Surface Studio, um evento [**RadialController.ButtonClicked**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController.ButtonClicked) é acionado e as informações de contato ([**RadialControllerButtonClickedEventArgs.Contact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs.Contact)) são fornecidas para seu aplicativo. 

As informações de contato ([**RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact)) incluem as coordenadas X/Y do centro do Surface Dial no espaço de coordenadas do aplicativo ([**RadialControllerScreenContact.Position**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Position)), bem como o retângulo delimitador ([**RadialControllerScreenContact.Bounds**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact.Bounds)) nos pixels independentes de dispositivo (DIPs). Essas informações são muito úteis para fornecer contexto para a ferramenta ativa e fornecer feedback visual relacionado ao dispositivo para o usuário.

No exemplo a seguir, criamos um aplicativo básico com quatro seções distintas, cada uma delas inclui um controle deslizante e um switch de alternância. Em seguida, usamos a posição na tela do Surface Dial para ditar qual conjunto de controles deslizantes e switches de alternância é controlado pelo Surface Dial.

1. Primeiro, nós declaramos nossa interface do usuário (quatro seções, cada uma com um switch de alternância e um controle deslizante) em XAML.

   ![Imagem da interface do aplicativo de exemplo](images/windows-wheel/surface-dial-snippet-customtool3.png)  
   *A interface do usuário do aplicativo de exemplo*

   ```xaml 
<Grid Background="{ThemeResource ApplicationPageBackgroundThemeBrush}">
  <Grid.RowDefinitions>
    <RowDefinition Height="Auto"/>
    <RowDefinition Height="*"/>
  </Grid.RowDefinitions>
  <StackPanel x:Name="HeaderPanel" 
        Orientation="Horizontal" 
        Grid.Row="0">
    <TextBlock x:Name="Header"
      Text="RadialController customization sample"
      VerticalAlignment="Center"
      Style="{ThemeResource HeaderTextBlockStyle}"
      Margin="10,0,0,0" />
  </StackPanel>
  <Grid Grid.Row="1" x:Name="RootGrid">
    <Grid.RowDefinitions>
      <RowDefinition Height="*"/>
      <RowDefinition Height="*"/>
    </Grid.RowDefinitions>
    <Grid.ColumnDefinitions>
      <ColumnDefinition Width="*"/>
      <ColumnDefinition Width="*"/>
    </Grid.ColumnDefinitions>
    <Grid x:Name="Grid0"
      Grid.Row="0"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider0"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle0"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid1"
      Grid.Row="0"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider1"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle1"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid2"
      Grid.Row="1"
      Grid.Column="0">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider2"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle2"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
    <Grid x:Name="Grid3"
      Grid.Row="1"
      Grid.Column="1">
      <StackPanel Orientation="Vertical" 
        VerticalAlignment="Center" 
        HorizontalAlignment="Center">
        <!-- Slider for rotational input -->
        <Slider x:Name="RotationSlider3"
          Width="300"
          HorizontalAlignment="Left"/>
        <!-- Switch for button input -->
        <ToggleSwitch x:Name="ButtonToggle3"
            HorizontalAlignment="Left"/>
      </StackPanel>
    </Grid>
  </Grid>
</Grid>
   ```

2. Aqui está o code-behind com manipuladores definidos para a posição da tela do Surface Dial.

```csharp
Slider ActiveSlider;
ToggleSwitch ActiveSwitch;
Grid ActiveGrid;

public MainPage()
{
  ...

  myController.ScreenContactStarted += 
    MyController_ScreenContactStarted;
  myController.ScreenContactContinued += 
    MyController_ScreenContactContinued;
  myController.ScreenContactEnded += 
    MyController_ScreenContactEnded;
  myController.ControlLost += MyController_ControlLost;

  //Set initial grid for Surface Dial input.
  ActiveGrid = Grid0;
  ActiveSlider = RotationSlider0;
  ActiveSwitch = ButtonToggle0;
}

private void MyController_ScreenContactStarted(RadialController sender, 
  RadialControllerScreenContactStartedEventArgs args)
{
  //find grid at contact location, update visuals, selection
  ActivateGridAtLocation(args.Contact.Position);
}

private void MyController_ScreenContactContinued(RadialController sender, 
  RadialControllerScreenContactContinuedEventArgs args)
{
  //if a new grid is under contact location, update visuals, selection
  if (!VisualTreeHelper.FindElementsInHostCoordinates(
    args.Contact.Position, RootGrid).Contains(ActiveGrid))
  {
    ActiveGrid.Background = new 
      SolidColorBrush(Windows.UI.Colors.White);
    ActivateGridAtLocation(args.Contact.Position);
  }
}

private void MyController_ScreenContactEnded(RadialController sender, object args)
{
  //return grid color to normal when contact leaves screen
  ActiveGrid.Background = new 
  SolidColorBrush(Windows.UI.Colors.White);
}

private void MyController_ControlLost(RadialController sender, object args)
{
  //return grid color to normal when focus lost
  ActiveGrid.Background = new 
    SolidColorBrush(Windows.UI.Colors.White);
}

private void ActivateGridAtLocation(Point Location)
{
  var elementsAtContactLocation = 
    VisualTreeHelper.FindElementsInHostCoordinates(Location, 
      RootGrid);

  foreach (UIElement element in elementsAtContactLocation)
  {
    if (element as Grid == Grid0)
    {
      ActiveSlider = RotationSlider0;
      ActiveSwitch = ButtonToggle0;
      ActiveGrid = Grid0;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid1)
    {
      ActiveSlider = RotationSlider1;
      ActiveSwitch = ButtonToggle1;
      ActiveGrid = Grid1;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid2)
    {
      ActiveSlider = RotationSlider2;
      ActiveSwitch = ButtonToggle2;
      ActiveGrid = Grid2;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
    else if (element as Grid == Grid3)
    {
      ActiveSlider = RotationSlider3;
      ActiveSwitch = ButtonToggle3;
      ActiveGrid = Grid3;
      ActiveGrid.Background = new SolidColorBrush( 
        Windows.UI.Colors.LightGoldenrodYellow);
      return;
    }
  }
}
```

Ao executar o aplicativo, usamos o Surface Dial para interagir com ele. Primeiro, colocamos o dispositivo na tela do Surface Studio, que o aplicativo detecta e associa à seção inferior direita (veja a imagem). Em seguida, pressione e segure o Surface Dial para abrir o menu e selecione nossa ferramenta personalizada. Depois que a ferramenta personalizada é ativada, o controle deslizante pode ser ajustado girando o Surface Dial e o switch pode ser alternado clicando no Surface Dial.

![Imagem da interface do usuário do aplicativo de exemplo ativado usando a ferramenta personalizada do Surface Dial](images/windows-wheel/surface-dial-snippet-customtool4.png)  
*A interface do usuário do aplicativo de exemplo ativado usando a ferramenta personalizada do Surface Dial*

## <a name="summary"></a>Resumo

Este tópico fornece uma visão geral do dispositivo de entrada Surface Dial com as diretrizes de experiência do usuário e desenvolvedor sobre como personalizar a experiência do usuário para cenários fora da tela, bem como cenários na tela quando usado com o Surface Studio.

## <a name="feedback"></a>Comentários

Envie suas perguntas, sugestões e comentários para [radialcontroller@microsoft.com](mailto:radialcontroller@microsoft.com).

## <a name="related-articles"></a>Artigos relacionados

### <a name="api-reference"></a>Referência da API

- [Classe **RadialController**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialController)
- [Classe **RadialControllerButtonClickedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerButtonClickedEventArgs)
- [Classe **RadialControllerConfiguration**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerConfiguration) 
- [Classe **RadialControllerControlAcquiredEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerControlAcquiredEventArgs) 
- [Classe **RadialControllerMenu**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenu) 
- [Classe **RadialControllerMenuItem**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuItem) 
- [Classe **RadialControllerRotationChangedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerRotationChangedEventArgs) 
- [Classe **RadialControllerScreenContact**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContact) 
- [Classe **RadialControllerScreenContactContinuedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactContinuedEventArgs) 
- [Classe **RadialControllerScreenContactStartedEventArgs**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerScreenContactStartedEventArgs)
- [Enumeração **RadialControllerMenuKnownIcon**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerMenuKnownIcon) 
- [Enumeração **RadialControllerSystemMenuItemKind**](https://msdn.microsoft.com/library/windows/apps/Windows.UI.Input.RadialControllerSystemMenuItemKind) 

### <a name="samples"></a>Exemplos

[Exemplos da Plataforma Universal do Windows (C# e C++)](https://go.microsoft.com/fwlink/?linkid=832713)

[Exemplo da área de trabalho clássica do Windows](https://aka.ms/radialcontrollerclassicsample)