---
author: Mtoepke
title: Introdução ao desenvolvimento de aplicativos UWP no Xbox One
description: Como configurar o seu computador e Xbox One para o desenvolvimento UWP.
ms.author: scotmi
ms.date: 10/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: da260b4f9f5f50d97d39af883217dfbae91a566e
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4693250"
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

    Se estiver instalando o Visual Studio 2015 atualização 3, certifique-se de que você escolha a instalação **personalizada** e marque a caixa de seleção de **Ferramentas de desenvolvimento de aplicativos universais do Windows** , ela não é parte da instalação padrão. Se você for um desenvolvedor de C++, escolha **Instalação personalizada** e selecione **C++**.

    Se estiver instalando o Visual Studio 2017, escolha a carga de trabalho **Desenvolvimento da Plataforma Universal do Windows**. Se você for um desenvolvedor de C++, no painel de **Resumo** à direita, em **desenvolvimento da plataforma Universal do Windows**, certifique-se de que você selecione a caixa de seleção de **Ferramentas da plataforma Universal do Windows C++** . Não é parte da instalação padrão.

    Para obter mais informações, consulte [Configurar UWP no ambiente de desenvolvimento do Xbox](development-environment-setup.md).

2.  Instale o [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)mais recente.

3.  Habilitar o modo de desenvolvedor para seu computador de desenvolvimento (**configurações / atualização e segurança / para desenvolvedores / usar recursos de desenvolvedor / modo de desenvolvedor**).

Agora que seu computador de desenvolvimento está pronto, você pode assistir a este vídeo ou continuar lendo para saber como configurar o seu Xbox One para desenvolvimento e criar e implantar um aplicativo UWP para ele.
</br>
</br>
<iframe src="https://channel9.msdn.com/Events/Xbox/App-Dev-on-Xbox/Get-started-with-App-Dev-on-Xbox/player#time=51s:paused" width="600" height="338"  allowFullScreen frameBorder="0"></iframe>

## <a name="setting-up-your-xbox-one-console"></a>Configurando seu console Xbox One

1.  Ative o Modo de Desenvolvedor no seu Xbox One. Baixar o aplicativo, obtenha o código de ativação e, em seguida, insira-o na página [consoles gerenciar Xbox One](https://partner.microsoft.com/xboxactivate) em sua conta do Centro de desenvolvimento. Para saber mais, consulte [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md). 

2.  Abra o aplicativo de **Ativação do modo de desenvolvedor** e selecione **Alternar e reiniciar**. Parabéns! Agora você tem um Xbox One no Modo de Desenvolvedor!
  
  > [!NOTE]
  > Seus aplicativos e jogos de varejo não serão executados no Modo de Desenvolvedor, apenas os aplicativos ou jogos que você criar. Volte para o Modo de Varejo para executar seus jogos e aplicativos favoritos.
    
  > [!NOTE]
  > Para poder implantar um aplicativo ao Xbox One no Modo de Desenvolvedor, você deve ter um usuário conectado no console. Você pode usar sua conta do Xbox Live existente ou criar uma nova conta para seu console em modo de desenvolvimento. 

## <a name="creating-your-first-project-in-visual-studio"></a>Criando seu primeiro projeto no Visual Studio

Para obter informações mais detalhadas, consulte [Configurar UWP no ambiente de desenvolvimento do Xbox](development-environment-setup.md).

1.  **Para c#**: criar um novo projeto Universal do Windows e no **Gerenciador de soluções**, clique com botão direito no projeto e selecione **Propriedades**. Selecione a guia **Depurar** , altere o **dispositivo de destino** para **Computador remoto**, digite o endereço IP ou nome do host do seu console Xbox One no campo **máquina remota** e selecione **Universal (protocolo não criptografado)** na ** Modo de autenticação** lista suspensa.   

    Você pode encontrar o endereço IP do seu Xbox One iniciando a Dev Home no seu console (o grande bloco do lado direito de Início), no canto superior esquerdo. Para obter mais informações sobre a Dev Home, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).  

2.  **Para C++ e HTML/Javascript projetos**: siga um caminho semelhante para projetos c#, mas nas propriedades do projeto vá para a guia **depuração** , selecione **Máquina remota** no depurador para abrir a lista suspensa, digite o endereço IP ou nome do host da console no campo de **Nome do computador** e selecione **Universal (protocolo não criptografado)** no campo de **Tipo de autenticação** .

3. Selecione **x64** na lista suspensa à esquerda do botão verde reproduzir na barra de menu principal.
   
4.  Quando você pressionar F5, seu aplicativo será compilado e começará a ser implantado em seu Xbox One.
  
5.  Na primeira vez que você fizer isso, o Visual Studio solicitará um PIN para seu Xbox One. Você pode obter um PIN a partir do início do desenvolvimento no seu Xbox One e selecionando o botão de **pin de mostrar o Visual Studio** .
  
6.  Depois que você tiver feito o emparelhamento, o aplicativo começará a implantação. A primeira vez que você fizer isso pode ser um pouco lenta (precisamos copiar todas as ferramentas para o seu Xbox), mas se o processo demorar mais de alguns minutos, provavelmente há algo de errado. Certifique-se de que você seguiu todas as etapas acima (em especial, você configurou o **Modo de Autenticação** como **Universal**?) e que você está usando uma conexão de rede com fio em seu Xbox One.  

7. Relaxe. Aproveite seu primeiro aplicativo em execução no console!  

## <a name="thats-it"></a>Pronto!

![Hello World](images/getting-started-hello-world.png)

## <a name="see-also"></a>Consulte também  
- [Perguntas frequentes](frequently-asked-questions.md)  
- [Problemas conhecidos com o Programa de desenvolvedor UWP no Xbox](known-issues.md)
- [UWP no Xbox One](index.md) 
