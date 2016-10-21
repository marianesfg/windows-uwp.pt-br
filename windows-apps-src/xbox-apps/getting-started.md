---
author: Mtoepke
title: "Introdução ao desenvolvimento de aplicativos UWP no Xbox One"
description: Como configurar o seu computador e Xbox One para o desenvolvimento UWP.
translationtype: Human Translation
ms.sourcegitcommit: d4ef0da606c98c5eb024f349720fe9641e4a541e
ms.openlocfilehash: 33b8369be9cc0fd54ee044ec2aa6ea38b352c908

---

#Introdução ao desenvolvimento de aplicativos UWP no Xbox One

Siga estas etapas **com cuidado** para configurar com êxito o seu computador e Xbox One para desenvolvimento da Plataforma Universal do Windows. Depois que você tiver configurado tudo, saiba mais sobre o Modo de Desenvolvedor do Xbox One e sobre como desenvolver aplicativos UWP na página [UWP for Xbox One](index.md). 

## Antes de começar
Antes de começar, você precisará fazer o seguinte:
-   Configurar um computador com o Windows 10.
-   Instalar o Microsoft Visual Studio 2015 Atualização 3.
- Ter pelo menos 5 GB de espaço livre no seu console Xbox One.

## Configurando o seu computador de desenvolvimento
1.  Instale a Atualização do Visual Studio 2015. Lembre-se de selecionar a instalação **Personalizada** e marcar a caixa de seleção **Ferramentas de Desenvolvimento de Aplicativo Universal do Windows**. Isso não é parte da instalação padrão. Se você for um desenvolvedor de C++, escolha **Instalação personalizada** e selecione **C++**. Para obter mais detalhes, veja [Configuração do ambiente de desenvolvimento](development-environment-setup.md). 

2.  Instale o SDK mais recente do Windows 10. Acesse [https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk](https://developer.microsoft.com/en-US/windows/downloads/windows-10-sdk) para baixá-lo.

3.  Habilite o Modo de Desenvolvedor para seu computador de desenvolvimento (Configurações/Atualização e segurança/Para desenvolvedores/Modo de desenvolvedor).

## Configurando seu console Xbox One
1.  Ative o Modo de Desenvolvedor no seu Xbox One. Baixe o aplicativo, obtenha o código de ativação e insira-o na página xboxactivate em sua conta do Centro de Desenvolvimento. Para saber mais, consulte [Habilitando o Modo de Desenvolvedor no Xbox One](devkit-activation.md). 

2.  Vá para o aplicativo de Ativação do Modo de Desenvolvedor e selecione **Alternar e reiniciar**. Parabéns! Agora você tem um Xbox One no Modo de Desenvolvedor!
  
  > [!NOTE]
  > Seus aplicativos e jogos de varejo não serão executados no Modo de Desenvolvedor, apenas os aplicativos ou jogos que você criar. Volte para o Modo de Varejo para executar seus jogos e aplicativos favoritos.
    
  > [!NOTE]
  > Para poder implantar um aplicativo ao Xbox One no Modo de Desenvolvedor, você deve ter um usuário conectado no console. Você pode usar sua conta do Xbox Live existente ou criar uma nova conta para seu console em modo de desenvolvimento. 

## Criando seu primeiro projeto no Visual Studio 2015

Para obter informações mais detalhadas, veja [Configuração do ambiente de desenvolvimento](development-environment-setup.md).

1.  **Para C#**: crie um novo projeto Universal do Windows, vá para as propriedades do projeto e selecione a guia **Depurar**. Altere **Dispositivo de Destino** para **Computador Remoto**, digite o endereço IP ou o nome do host do seu console do Xbox One no campo **Computador remoto** e selecione **Universal (Protocolo Não Criptografado)** na lista suspensa **Modo de Autenticação**.   

    Você pode encontrar o endereço IP do seu Xbox One iniciando a Dev Home no seu console (o grande bloco do lado direito de Início), no canto superior esquerdo. Para obter mais informações sobre a Dev Home, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).  

2.  **Para projetos em C++ e HTML/Javascript**: siga um caminho semelhante mas, nas propriedades do projeto, vá para a guia **Depuração**, selecione **Computador Remoto** no Depurador para abrir a lista suspensa. Digite o endereço IP ou o nome do host do seu console no campo **Nome do Computador** e selecione **Universal (Protocolo Não Criptografado** no campo **Modo de Autenticação**.
   
3.  Quando você pressionar F5, seu aplicativo será compilado e começará a ser implantado em seu Xbox One.
  
4.  Na primeira vez que você fizer isso, o Visual Studio solicitará um PIN para seu Xbox One. Você pode obter um PIN a partir da Dev Home do Xbox One, selecionando o botão **Pair with Visual Studio**.
  
5.  Depois que você tiver feito o emparelhamento, o aplicativo começará a implantação. A primeira vez que você fizer isso pode ser um pouco lenta (precisamos copiar todas as ferramentas para o seu Xbox), mas se o processo demorar mais de alguns minutos, provavelmente há algo de errado. Certifique-se de que você seguiu todas as etapas acima (em especial, você configurou o **Modo de Autenticação** como **Universal**?) e que você está usando uma conexão de rede com fio em seu Xbox One.  

6. Relaxe. Aproveite seu primeiro aplicativo em execução no console!  

## Pronto!

![Hello World](images/getting-started-hello-world.png)

## Consulte também  
- [Perguntas frequentes](frequently-asked-questions.md)  
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md) 



<!--HONumber=Aug16_HO4-->


