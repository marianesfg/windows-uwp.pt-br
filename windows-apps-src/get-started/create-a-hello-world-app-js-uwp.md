---
ms.assetid: 3a17e682-40be-41b4-8bd3-fbf0b15259d6
title: Criar um aplicativo Hello, world (JS)
description: Este tutorial ensina a usar JavaScript e HTML para criar um aplicativo &\#0034;Olá, Mundo&\#0034; simples orientado para a UWP (Plataforma Universal do Windows) no Windows 10.
ms.date: 03/06/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 682a69726b20be52a3217f49bed4947b8b621230
ms.sourcegitcommit: aaa4b898da5869c064097739cf3dc74c29474691
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2019
ms.locfileid: "64564513"
---
# <a name="create-a-hello-world-app-js"></a>Criar um aplicativo Hello, world (JS)

Este tutorial ensina a usar JavaScript e HTML para criar um aplicativo "Olá, Mundo" simples orientado para a UWP (Plataforma Universal do Windows) no Windows 10. Com um único projeto no Microsoft Visual Studio, você pode criar um aplicativo que seja executado em qualquer dispositivo do Windows 10.

> [!NOTE]
> Este tutorial usa o Visual Studio Community 2017. Se você estiver usando uma versão diferente do Visual Studio, talvez ele seja um pouco diferente.


Aqui, você aprenderá a:

-   Criar um novo projeto do **Visual Studio** direcionado ao **Windows 10** e à **UWP**.
-   Adicionar conteúdo HTML e JavaScript
-   Execute o projeto na área de trabalho local no Visual Studio

## <a name="before-you-start"></a>Antes de começar...

-   [O que é um aplicativo UWP?](universal-application-platform-guide.md).
-   Para concluir este tutorial, você precisa do Windows 10 e do Visual Studio. [Prepare-se para começar](get-set-up.md).
-   Também pressupomos que você esteja usando o layout de janela padrão no Visual Studio. Se você alterar o layout padrão, poderá redefini-lo no menu **Janela** usando o comando **Redefinir Layout da Janela**.

## <a name="step-1-create-a-new-project-in-visual-studio"></a>Etapa 1: Criar um novo projeto no Visual Studio.

1.  Inicie o Visual Studio.

2.  No menu **Arquivo**, selecione **Novo > Projeto...** para abrir a caixa de diálogo *Novo Projeto*.

3.  Na lista de modelos no lado esquerdo, abra **Instalado > Modelos > JavaScript** e, em seguida, escolha **Universal do Windows** para ver a lista de modelos do projeto de UWP.

    (Se você não vir modelos de Universal, talvez não tenha os componentes para a criação de aplicativos UWP. Repita o processo de instalação e adicione suporte a UWP clicando em **Abrir instalador do Visual Studio** na caixa de diálogo *Novo Projeto*. Confira [Preparar-se](get-set-up.md)

4.  Escolha o modelo **Aplicativo em branco (Universal Windows)** e insira "HelloWorld" como **nome**. Clique em **OK**.

    ![A janela Novo projeto](images/win10-js-01.png)

> [!NOTE]
> Se esta for a primeira vez que usa o Visual Studio, talvez veja uma caixa de diálogo Configurações solicitando a habilitação do **Modo de desenvolvedor**. O Modo de desenvolvedor é uma configuração especial que habilita determinados recursos, como a permissão para executar aplicativos diretamente, em vez de apenas na Store. Para saber mais, leia [Habilitar seu dispositivo para desenvolvimento](enable-your-device-for-development.md). Para continuar com este guia, selecione **Modo de desenvolvedor**, clique em **Sim** e feche a caixa de diálogo.

 ![Ativar a caixa de diálogo Modo de desenvolvedor](images/win10-cs-00.png)

5.  A caixa de diálogo de versão pretendida/versão mínima é exibida. As configurações padrão são adequadas para este tutorial, então selecione **OK** para criar o projeto.

    ![Janela Gerenciador de soluções](images/win10-cs-02.png)

6.  Quando o seu novo projeto é aberto, seus arquivos são exibidos no painel do **Gerenciador de soluções** à direita. Talvez seja necessário escolher a guia **Gerenciador de soluções** em vez da guia **Propriedades** para ver seus arquivos.

    ![Janela Gerenciador de soluções](images/win10-js-02.png)

Apesar de ser um modelo básico, **Aplicativo em Branco (Universal do Windows)** contém vários arquivos. Esses arquivos são essenciais para todos os aplicativos UWP em JavaScript. Eles fazem parte de todos os projetos criados no Visual Studio.


### <a name="whats-in-the-files"></a>O que os arquivos incluem?

Para exibir e editar um arquivo no projeto, clique duas vezes no arquivo no **Gerenciador de Soluções**.

*default.css*

-  A folha de estilos padrão usada pelo aplicativo.

*main.js*

- O arquivo JavaScript padrão. Ele é referenciado no arquivo index.html.

*index.html*

- A página da Web do aplicativo, carregada e exibida quando o aplicativo é iniciado.

*Um conjunto de imagens de logotipo*
-   Assets/Square150x150Logo.scale-200.png representa seu aplicativo no menu Iniciar.
-   Assets/StoreLogo.png representa seu aplicativo na Microsoft Store.
-   Assets/SplashScreen.scale-200.png é a tela inicial que será exibida quando o aplicativo iniciar.

## <a name="step-2-adding-a-button"></a>Etapa 2: Adicionar um botão

Clique em *index.html* para selecioná-lo no editor e altere o HTML que contém para ler:

```html
<!DOCTYPE html>
<html>

<head>
    <meta charset="utf-8" />
    <title>Hello World</title>
    <script src="js/main.js"></script>
    <link href="css/default.css" rel="stylesheet" />
</head>

<body>
    <p>Click the button..</p>
    <button id="button">Hello world!</button>
</body>

</html>
```

Ele deve ser assim:

 ![O HTML do projeto](images/win10-js-03.png)

Este HTML referencia o *main.js* que conterá o JavaScript e, em seguida, adiciona uma única linha de texto e um único botão ao corpo da página da Web. O botão recebe uma *ID* para que o JavaScript possa fazer referência a ele.


## <a name="step-3-adding-some-javascript"></a>Etapa 3: Adicionar algum JavaScript

Agora, vamos adicionar o JavaScript. Clique em *main.js* para selecioná-lo e adicione o seguinte:

```javascript
// Your code here!

window.onload = function () {
    document.getElementById("button").onclick = function (evt) {
        sayHello()
    }
}


function sayHello() {
    var messageDialog = new Windows.UI.Popups.MessageDialog("Hello, world!", "Alert");
    messageDialog.showAsync();
}

```

Ele deve ser assim:

 ![O JavaScript do projeto](images/win10-js-04.png)

Esse JavaScript declara duas funções. A função *window.onload* é chamada automaticamente quando *index.html* é exibido. Ela localiza o botão (usando a ID que declaramos) e adiciona um manipulador onclick: o método que será chamado quando o botão é clicado.

A segunda função, *sayHello()* , cria e exibe uma caixa de diálogo. Isso é muito parecido com a função *Alert()* que você talvez conheça do desenvolvimento de JavaScript anterior.


## <a name="step-4-run-the-app"></a>Etapa 4: Executar o aplicativo.

Agora, você pode executar o aplicativo pressionando F5. O aplicativo será carregado e a página da Web será exibida. Clique no botão e a caixa de diálogo será de mensagem exibida.

 ![Executando o projeto](images/win10-js-05.png)



## <a name="summary"></a>Resumo


Parabéns, você criou um aplicativo JavaScript para o Windows 10 e a UWP! Este é um exemplo incrivelmente simples, porém agora você pode começar a adicionar suas bibliotecas e estruturas JavaScript favoritas para criar seu próprio aplicativo. E como é um aplicativo UWP, você poderá publicá-lo na Store. Para um exemplo de como estruturas de terceiros podem ser adicionadas, consulte estes projetos:

* [Um jogo simples 2D UWP para a Microsoft Store, escrito em JavaScript e CreateJS](get-started-tutorial-game-js2d.md)
* [Um jogo 3D UWP para a Microsoft Store, escrito em JavaScript e threeJS](get-started-tutorial-game-js3d.md)
