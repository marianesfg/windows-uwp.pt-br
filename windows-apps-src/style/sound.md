---
author: mijacobs
Description: "O som ajuda a completar a experiência do usuário do aplicativo e dá a eles aquele toque extra de áudio para combinar com a personalidade do Windows em todas as plataformas."
label: Sound
title: Som
template: detail.hbs
ms.assetid: 9fa77494-2525-4491-8f26-dc733b6a18f6
translationtype: Human Translation
ms.sourcegitcommit: 7bb23094d569bb29c7227ccd628abd0989b575a4
ms.openlocfilehash: e6dab48935cd5345ee734e6fda7e6fd4d333bb90

---
[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.] *Este artigo fornece uma visualização de recursos que ainda não estão disponíveis.*

# Som

Há muitas maneiras de usar o som para aprimorar o aplicativo. Você pode usar som para complementar outros elementos de interface do usuário permitindo que os usuários reconheçam eventos de maneira audível. O som pode ser um elemento de interface do usuário efetivo para pessoas com deficiências visuais. É possível usar som para criar uma atmosfera que envolva o usuário; por exemplo, convém executar uma trilha sonora extravagante em segundo plano do quebra-cabeça ou usar efeitos sonoros de suspense para um jogo de terror/sobrevivência.

## API de som Global

O UWP fornece um sistema de som facilmente acessível que permite simplesmente "usar um interruptor" e obter uma intensa experiência de áudio em seu aplicativo inteiro.

O **ElementSoundPlayer** é um sistema de som integrado no XAML e, quando ativado em todos os controles padrão, reproduz sons automaticamente.
```C#
ElementSoundPlayer.State = ElementSoundPlayerState.On;
```
O **ElementSoundPlayer** tem três estados diferentes: **Ativado** **Desativado** e **Auto**.

Se definido como **Desativado**, não importa onde seu aplicativo seja executado, o som nunca será reproduzido. Se definido como **Ativado** os sons de seu aplicativo serão executados em todas as plataformas.

### Som de TV e Xbox

O som é uma parte essencial da experiência de 3 metros e, por padrão, o estado do **ElementSoundPlayer** é **Auto**, o que significa que você só obterá som quando seu aplicativo estiver em execução no Xbox.
Para saber mais sobre como projetar para Xbox e TV, consulte o artigo [Projetando para Xbox e TV](http://go.microsoft.com/fwlink/?LinkId=760736).

## Substituição de volume do som

Todos os sons no aplicativo podem ser reduzidos com o controle **Volume**. No entanto, os sons no aplicativo não podem ficar *mais altos do que o volume do sistema*.

Para definir o nível de volume do aplicativo, chame:
```C#
ElementSoundPlayer.Volume = 0.5f;
```
Onde volume máximo (em relação ao volume do sistema) é 1,0 e o mínimo é 0,0 (essencialmente silencioso).

## Estado de nível de controle

Se não desejar o som do padrão de um controle, ele poderá ser desativado. Isso é feito por meio do **ElementSoundMode** no controle.

O **ElementSoundMode** tem dois estados: **Desativado** e **Padrão**. Quando não definido, é **Padrão**. Se definido como **Desativado**, cada som que for reproduzido pelo controle será silenciado, *exceto o foco*.

```XAML
<Button Name="ButtonName" Content="More Info" ElementSoundMode="Off"/>
```

```C#
ButtonName.ElementSoundState = ElementSoundMode.Off;
```

## Este é o som correto?

Ao criar um controle personalizado ou alterar o som de um controle existente, é importante entender os usos de todos os sons que o sistema fornece.

Cada som está relacionado a uma determinada interação básica do usuário e, embora os sons possam ser personalizados para reprodução em qualquer interação, esta seção serve para ilustrar os cenários nos quais os sons devem ser usados para manter uma experiência consistente em todos os aplicativos UWP.

### Invocando um elemento

O som disparado por controle mais comum em nosso sistema hoje é o **Invoke**. Esse som é reproduzido quando o usuário invoca um controle por meio de um toque/clique/enter/espaço ou pressionamento do botão "A" em um gamepad.

Normalmente, esse som só é executado quando um usuário direciona explicitamente um controle simples ou parte de um controle por meio de um [dispositivo de entrada](../input-and-devices/input-primer.md).

<clipe de som SelectButtonClick.mp3 aqui>

Para reproduzir esse som a partir de qualquer evento de controle, basta chamar o método Play de **ElementSoundPlayer** e transmitir **ElementSound.Invoke**:
```C#
ElementSoundPlayer.Play(ElementSoundKind.Invoke);
```

### Mostrando e ocultando conteúdo

Há muitos submenus, caixas de diálogo e interfaces do usuário rejeitadas em XAML, e qualquer ação que dispare uma dessas sobreposições deve chamar o som **Mostrar** ou **Ocultar**.

Quando uma janela de conteúdo de sobreposição é exibida, o som **Mostrar** deve ser chamado:

<clipe de som OverlayIn.mp3 aqui>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Show);
```
Por outro lado, quando uma janela de conteúdo de sobreposição é fechada (ou light dismissed), o som **Ocultar** deve ser chamado:

<clipe de som OverlayOut.mp3 aqui>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Hide);
```
### Navegação dentro de uma página

Quando se navega entre os painéis ou modos de exibição na página do aplicativo (consulte [Hub](../controls-and-patterns/hub.md) ou [Guias e pivôs](../controls-and-patterns/tabs-pivot.md)), normalmente há movimento bidirecional. Ou seja, você pode passar para o modo de exibição/painel próximo ou o anterior, sem sair da página atual do aplicativo.

A experiência de áudio em torno esse conceito de navegação é englobada pelos sons **MovePrevious** e **MoveNext**.

Ao mudar para um modo de exibição/painel que é considerado o *próximo item* em uma lista, chame:

<clipe de som PageTransitionRight.mp3 aqui>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MoveNext);
```
E ao mudar para um modo de exibição/painel anterior em uma lista considerado o *item anterior*, chame:

<clipe de som PageTransitionLeft.mp3 aqui>

```C#
ElementSoundPlayer.Play(ElementSoundKind.MovePrevious);
```
### Navegação regressiva

Ao navegar da página atual para a página anterior dentro de um aplicativo, o som **GoBack** deve ser chamado:

<clipe de som BackButtonClick.mp3 aqui>

```C#
ElementSoundPlayer.Play(ElementSoundKind.GoBack);
```
### Focando em um elemento

O som **Foco** é o único som implícito em nosso sistema. Ou seja, um usuário não está interagindo diretamente com nada, mas ainda ouve um som.

O foco acontece quando um usuário navega por meio de um aplicativo, que pode ser com o teclado/gamepad/remoto ou kinect. Normalmente, o som **Foco** *não é reproduzido em eventos PointerEntered nem do mouse*.

Para configurar um controle para reproduzir o som **Foco** quando o controle recebe foco, chame:

<clipe de som ElementFocus1.mp3 aqui>

```C#
ElementSoundPlayer.Play(ElementSoundKind.Focus);
```
### Exibição cíclica de sons de foco

Como um recurso adicionado à chamada de **ElementSound.Focus**, o sistema de som, por padrão, percorrerá por quatro sons diferentes em cada disparador de navegação. Isso significa que dois sons de foco exatos não serão reproduzidos um após o outro.

O objetivo por trás desse recurso de ciclo é evitar que os sons de foco se tornem monótonos e sejam perceptíveis pelo usuário; os sons de foco serão reproduzidos com mais frequência e, portanto, devem ser mais sutis.

## Artigos relacionados

* [Projetando para TV e Xbox](http://go.microsoft.com/fwlink/?LinkId=760736)



<!--HONumber=Jul16_HO1-->


