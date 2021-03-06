---
Description: O som ajuda a completar a experiência do usuário do aplicativo e dá a eles aquele toque extra de áudio para combinar com a personalidade do Windows em todas as plataformas.
label: Sound
title: Som
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
pm-contact: kisai
design-contact: mattben
dev-contact: joyate
doc-status: Published
ms.localizationpriority: medium
ms.openlocfilehash: 8ed1344b5ee49244a6c1afcbb873b54fcc28624f
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75684880"
---
# <a name="sound"></a>Som

![imagem Hero](images/header-sound.svg)

Há muitas maneiras de usar o som para aprimorar o aplicativo. Você pode usar som para complementar outros elementos de interface do usuário permitindo que os usuários reconheçam eventos de maneira audível. O som pode ser um elemento de interface do usuário efetivo para pessoas com deficiências visuais. É possível usar som para criar uma atmosfera que envolva o usuário; por exemplo, convém executar uma trilha sonora extravagante em segundo plano do quebra-cabeça ou usar efeitos sonoros de suspense para um jogo de terror/sobrevivência.

## <a name="examples"></a>Exemplos

<table>
<th align="left">XAML Controls Gallery<th>
<tr>
<td><img src="images/xaml-controls-gallery-sm.png" alt="XAML controls gallery"></img></td>
<td>
    <p>Se você tiver um aplicativo <strong style="font-weight: semi-bold">XAML Controls Gallery</strong> instalado, clique aqui para <a href="xamlcontrolsgallery:/item/Sound">abri-lo e ver o Som em ação</a>.</p>
    <ul>
    <li><a href="https://www.microsoft.com/store/productId/9MSVH128X2ZT">Obtenha o aplicativo XAML Controls Gallery (Microsoft Store)</a></li>
    <li><a href="https://github.com/Microsoft/Xaml-Controls-Gallery">Obtenha o código-fonte (GitHub)</a></li>
    </ul>
</td>
</tr>
</table>

## <a name="sound-global-api"></a>API de Som Global

O UWP fornece um sistema de som facilmente acessível que permite simplesmente "usar um interruptor" e obter uma intensa experiência de áudio em seu aplicativo inteiro.

O [**ElementSoundPlayer**](https://docs.microsoft.com/uwp/api/windows.ui.xaml.elementsoundplayer) é um sistema de som integrado no XAML e, quando ativado em todos os controles padrão, reproduzirá sons automaticamente.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
O **ElementSoundPlayer** tem três estados diferentes: **Ativado** **Desativado** e **Automático**.

Se definido como **Desativado**, não importa onde seu aplicativo seja executado, o som nunca será reproduzido. Se definido como **Ativado** os sons de seu aplicativo serão executados em todas as plataformas.

Ao habilitar o ElementSoundPlayer, o áudio espacial (som 3D) também é habilitado automaticamente. Para desabilitar o som 3D (enquanto mantém o som), desabilite o **SpatialAudioMode** do ElementSoundPlayer: 

```C#
ElementSoundPlayer.SpatialAudioMode = ElementSpatialAudioMode.Off
```

A propriedade **SpatialAudioMode** pode usar esses valores: 
- **Automático**: o áudio espacial é ligado quando som está ativado. 
- **Desativado**: o áudio espacial sempre está desativado, mesmo quando o som está ativado.
- **Ativado**: o áudio espacial sempre será reproduzido.

Para saber mais sobre o áudio espacial e como o XAML processa isso, veja [AudioGraph – Áudio espacial](/windows/uwp/audio-video-camera/audio-graphs#spatial-audio).

### <a name="sound-for-tv-and-xbox"></a>Som de TV e Xbox

O som é uma parte essencial da experiência dos 3 metros e, por padrão, o estado do **ElementSoundPlayer** é **Auto**, o que significa que você só obterá som quando seu aplicativo estiver em execução no Xbox.
Para obter mais informações sobre como projetar para Xbox e TV, consulte o artigo [Projetando para Xbox e TV](https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv?redirectedfrom=MSDN).

## <a name="sound-volume-override"></a>Substituição de volume do som

Todos os sons no aplicativo podem ser esmaecidos com o controle **Volume**. No entanto, os sons no aplicativo não podem ficar *mais altos do que o volume do sistema*.

Para definir o nível de volume do aplicativo, chame:
```C#
ElementSoundPlayer.Volume = 0.5;
```
Quando volume máximo (em relação ao volume do sistema) for 1,0 e o mínimo for 0,0 (essencialmente silencioso).

## <a name="control-level-state"></a>Estado de nível de controle

Se não desejar o som do padrão de um controle, ele poderá ser desabilitado. Isso é feito por meio do **ElementSoundMode** no controle.

O **ElementSoundMode** tem dois estados: **Desativado** e **Padrão**. Quando não definido, é **Padrão**. Se definido como **Desativado**, cada som que for reproduzido pelo controle será silenciado, *exceto o foco*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## <a name="is-this-the-right-sound"></a>Este é o som correto?

Ao criar um controle personalizado ou alterar o som de um controle existente, é importante entender os usos de todos os sons que o sistema fornece.

Cada som está relacionado a uma determinada interação básica do usuário e, embora os sons possam ser personalizados para reprodução em qualquer interação, esta seção serve para ilustrar os cenários nos quais os sons devem ser usados para manter uma experiência consistente em todos os aplicativos UWP.

### <a name="invoking-an-element"></a>Invocar um elemento

O som disparado por controle mais comum em nosso sistema hoje é o **Invoke**. Esse som é reproduzido quando o usuário invoca um controle por meio de um toque/clique/enter/espaço ou pressionamento do botão "A" em um gamepad.

Normalmente, esse som só é executado quando um usuário direciona explicitamente um controle simples ou parte de um controle por meio de um [dispositivo de entrada](../input/index.md).


Para reproduzir esse som de qualquer evento de controle, basta chamar o método Play de **ElementSoundPlayer** e transmitir **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### <a name="showing--hiding-content"></a>Mostrar e ocultar conteúdo

Há muitos submenus, caixas de diálogo e interfaces do usuário rejeitadas em XAML, e qualquer ação que dispare uma dessas sobreposições deve chamar o som **Mostrar** ou **Ocultar**.

Quando uma janela de conteúdo de sobreposição é exibida, o som **Mostrar** deve ser chamado:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Por outro lado, quando uma janela de conteúdo de sobreposição é fechada (ou ignorada), o som **Ocultar** deve ser chamado:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### <a name="navigation-within-a-page"></a>Navegação dentro de uma página

Quando se navega entre os painéis ou modos de exibição na página do aplicativo (confira [Guias e pivôs](../controls-and-patterns/pivot.md)), normalmente há movimento bidirecional. Ou seja, você pode passar para o modo de exibição/painel próximo ou o anterior, sem sair da página atual do aplicativo.

A experiência de áudio em torno esse conceito de navegação é englobada pelos sons **MovePrevious** e **MoveNext**.

Ao mudar para um modo de exibição/painel que é considerado o *próximo item* em uma lista, chame:

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
E ao mudar para um modo de exibição/painel anterior em uma lista considerado o *item anterior*, chame:

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### <a name="back-navigation"></a>Navegação regressiva

Ao navegar da página atual para a página anterior dentro de um aplicativo, o som **GoBack** deve ser chamado:

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### <a name="focusing-on-an-element"></a>Focar em um elemento

O som **Focus** é o único som implícito em nosso sistema. Ou seja, um usuário não está interagindo diretamente com nada, mas ainda ouve um som.

O foco acontece quando um usuário navega por meio de um aplicativo, que pode ser com o teclado/gamepad/remoto ou kinect. Normalmente, o som **Foco***não é reproduzido em eventos PointerEntered nem do mouse*.

Para configurar um controle para reproduzir o som **Focus** quando o controle recebe foco, chame:

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### <a name="cycling-focus-sounds"></a>Exibição cíclica de sons de foco

Como um recurso adicionado à chamada de **ElementSound.Focus**, o sistema de som, por padrão, percorrerá por quatro sons diferentes em cada disparador de navegação. Isso significa que dois sons de foco exatos não serão reproduzidos um após o outro.

O objetivo por trás desse recurso de ciclo é evitar que os sons de foco se tornem monótonos e sejam perceptíveis pelo usuário; os sons de foco serão reproduzidos com mais frequência e, portanto, devem ser mais sutis.

## <a name="get-the-sample-code"></a>Obter o código de exemplo

- [Exemplo do XAML Controls Gallery](https://github.com/Microsoft/Xaml-Controls-Gallery): veja todos os controles XAML em um formato interativo.

## <a name="related-articles"></a>Artigos relacionados

* [Projetando para TV e Xbox](/windows/uwp/design/devices/designing-for-tv)
* [Documentação da classe ElementSoundPlayer](/uwp/api/windows.ui.xaml.elementsoundplayer)
