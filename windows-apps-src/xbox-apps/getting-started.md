---
title: Introdução ao desenvolvimento de aplicativos UWP no Xbox One
description: Como configurar o seu computador e Xbox One para o desenvolvimento UWP.
ms.date: 10/12/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c94d27e87853b570268e3a39fe941c817b3eda6a
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590971"
---
# <a name="getting-started-with-uwp-app-development-on-xbox-one"></a>Introdução ao desenvolvimento de aplicativos UWP no Xbox One

Siga estas etapas **com cuidado** para configurar com êxito o seu computador e Xbox One para desenvolvimento da Plataforma Universal do Windows. Depois que você tiver configurado tudo, saiba mais sobre o Modo de Desenvolvedor do Xbox One e sobre como desenvolver aplicativos UWP na página [UWP for Xbox One](index.md). 

## <a name="before-you-start"></a>Antes de começar

Antes de começar, você precisará fazer o seguinte:
-   Configure um computador com a versão mais recente do Windows 10.
<!-- -  Install Microsoft Visual Studio 2015 Update 3 or Microsoft Visual Studio 2017.

    > [!NOTE]
    > Visual Studio 2017 is required if you are using the Windows 10, build 15063 SDK. -->

- Ter pelo menos 5 GB de espaço livre no seu console Xbox One.

## <a name="setting-up-your-development-pc"></a>Configurando o seu computador de desenvolvimento

1.  Instale o Visual Studio 2015 atualização 3 ou o Visual Studio 2017.

    Se você estiver instalando o Visual Studio 2015 atualização 3, certifique-se de que você escolhe **personalizado** instalar e selecionar a **ferramentas de desenvolvimento de aplicativo Universal do Windows** caixa de seleção – não é parte do padrão de instalar. Se você for um desenvolvedor de C++, escolha **Instalação personalizada** e selecione **C++**.

    Se estiver instalando o Visual Studio 2017, escolha a carga de trabalho **Desenvolvimento da Plataforma Universal do Windows**. Se você for um desenvolvedor de C++, nos **resumo** painel à direita, sob **desenvolvimento da plataforma Universal do Windows**, certifique-se de que você selecione o **ferramentas C++ da plataforma Windows Universal** caixa de seleção. Não é parte da instalação padrão.

    Para obter mais informações, consulte [configurar seu UWP no ambiente de desenvolvimento do Xbox](development-environment-setup.md).

2.  Instale o versão mais recente [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

3.  Habilitar o modo de desenvolvedor para seu computador de desenvolvimento (**configurações / atualização e segurança / para os desenvolvedores / usar recursos de desenvolvedor / modo de desenvolvedor**).

Agora que seu computador de desenvolvimento está pronto, você pode assistir a este vídeo ou continuar lendo para saber como configurar o seu Xbox One para desenvolvimento e criar e implantar um aplicativo UWP para ele.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configurando seu console Xbox One

1.  Ative o Modo de Desenvolvedor no seu Xbox One. Baixe o aplicativo, obtenha o código de ativação e, em seguida, insira-o na **consoles gerenciar Xbox One** página em sua conta de desenvolvedor de aplicativo do Partner Center. Para saber mais, consulte [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md). 

2.  Abra o **ativação do modo de desenvolvimento** aplicativo e selecione **Switch e reinicialização**. Parabéns! Agora você tem um Xbox One no Modo de Desenvolvedor!
  
  > [!NOTE]
  > Seus aplicativos e jogos de varejo não serão executados no Modo de Desenvolvedor, apenas os aplicativos ou jogos que você criar. Volte para o Modo de Varejo para executar seus jogos e aplicativos favoritos.
    
  > [!NOTE]
  > Para poder implantar um aplicativo ao Xbox One no Modo de Desenvolvedor, você deve ter um usuário conectado no console. Você pode usar sua conta do Xbox Live existente ou criar uma nova conta para seu console em modo de desenvolvimento. 

## <a name="creating-your-first-project-in-visual-studio"></a>Criando seu primeiro projeto no Visual Studio

Para obter mais informações, consulte [configurar seu UWP no ambiente de desenvolvimento do Xbox](development-environment-setup.md).

1.  **Para C#** : Criar um novo projeto do Universal Windows e, na **Gerenciador de soluções**, clique com botão direito no projeto e selecione **propriedades**. Selecione o **Debug** , altere **dispositivo de destino** para **máquina remota**, digite o endereço IP ou nome do host do seu console Xbox One no **máquina remota**  campo e selecione **Universal (protocolo não criptografado)** no **modo de autenticação** lista suspensa.   

    Você pode encontrar o endereço IP do seu Xbox One iniciando a Dev Home no seu console (o grande bloco do lado direito de Início), no canto superior esquerdo. Para obter mais informações sobre a Dev Home, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).  

2.  **Para projetos C++ e HTML/Javascript**: Você seguir um caminho semelhante a C# projetos, mas, nas propriedades do projeto ir para o **Debugging** guia, selecione **máquina remota** no depurador para abrir a lista suspensa, digite o endereço IP ou nome do host do o console para o **nome da máquina** campo e selecione **Universal (protocolo não criptografado)** no **tipo de autenticação** campo.

3. Selecione **x64** na lista suspensa à esquerda do botão verde para executar na barra de menus superior.
   
4.  Quando você pressionar F5, seu aplicativo será compilado e começará a ser implantado em seu Xbox One.
  
5.  Na primeira vez que você fizer isso, o Visual Studio solicitará um PIN para seu Xbox One. Você pode obter um PIN, iniciando o desenvolvimento inicial em seu Xbox One e selecionando o **pin de mostrar o Visual Studio** botão.
  
6.  Depois que você tiver feito o emparelhamento, o aplicativo começará a implantação. A primeira vez que você fizer isso pode ser um pouco lenta (precisamos copiar todas as ferramentas para o seu Xbox), mas se o processo demorar mais de alguns minutos, provavelmente há algo de errado. Certifique-se de que você seguiu todas as etapas acima (em especial, você configurou o **Modo de Autenticação** como **Universal**?) e que você está usando uma conexão de rede com fio em seu Xbox One.  

7. Relaxe. Aproveite seu primeiro aplicativo em execução no console!  

## <a name="thats-it"></a>Pronto!

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Consulte também  
- [Perguntas frequentes](frequently-asked-questions.md)  
- [Problemas conhecidos com UWP no programa de desenvolvedores do Xbox](known-issues.md)
- [UWP no Xbox One](index.md) 
