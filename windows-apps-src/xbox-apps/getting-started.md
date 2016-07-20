---
author: Mtoepke
title: "Introdução ao desenvolvimento de aplicativos UWP no Xbox One"
description: Como configurar o seu computador e Xbox One para o desenvolvimento UWP.
area: Xbox
translationtype: Human Translation
ms.sourcegitcommit: 5f050eee9430dc7aaa2738a4610a2c4f5083839d
ms.openlocfilehash: 92a9cc54c6257c35b1e7ae19838b01c8452c4b36

---

#Introdução ao desenvolvimento de aplicativos UWP no Xbox One


              Siga estas etapas **com cuidado** para configurar com êxito o seu computador e Xbox One para desenvolvimento UWP. Depois que você tiver configurado tudo, saiba mais sobre o Modo de Desenvolvedor do Xbox One e sobre como desenvolver aplicativos UWP na página [UWP for Xbox One](index.md). 

## Antes de começar
Antes de começar, você precisará fazer o seguinte:
-   Criar uma conta do [Centro de Desenvolvimento do Windows](https://dev.windows.com).
-   Inscrever-se no [Programa Windows Insider](https://insider.windows.com/). Você precisará disso para obter a visualização do SDK do Windows.
-   Configurar uma computador com Windows 10 (qualquer versão serve, incluindo a versão de pré-lançamento do Windows 10 do Programa Windows Insider). Para esta visualização, nossas ferramentas de desenvolvimento precisam que você esteja executando o Windows 10. 
-   Conecte seu console do Xbox One a uma rede. Para obter melhor desempenho, use uma conexão com fio.
- Ter pelo menos 5 GB de espaço livre no seu console Xbox One.

## Configurando o seu computador de desenvolvimento
1.  Instale o Visual Studio 2015 Update 2. Lembre-se de selecionar a instalação **Personalizada** e marcar a caixa de seleção **Ferramentas de Desenvolvimento de Aplicativo Universal do Windows**. Isso não é parte da instalação padrão. Consulte [Development environment setup](development-environment-setup.md) para obter mais informações (além disso, se você for um desenvolvedor de C++, lembre-se de selecionar Instalação personalizada e C++ também).

2.  Instale a versão prévia mais recente do SDK do Windows 10. Você pode obtê-la no [Programa Windows Insider](http://go.microsoft.com/fwlink/p/?LinkId=780552).
  
  > 
              **Importante**&nbsp;&nbsp;A instalação dessa visualização do SDK em seu computador impedirá que você envie aplicativos para a loja criada nesse computador. Por isso, não instale em seu computador de desenvolvimento de produção. 

## Configurando seu console Xbox One
1.  Ative o Modo de Desenvolvedor no seu Xbox One. Baixe o aplicativo, obtenha o código de ativação e insira-o na página xboxactivate em sua conta do Centro de Desenvolvimento. Consulte [Enabling developer mode on Xbox One](devkit-activation.md) para obter mais informações. 

2.  Aguarde até que seu Xbox One receba uma atualização do sistema e passe a executar a Developer Preview. Isso pode levar até quatro horas. Se você não quiser esperar, force a reinicialização do seu console pressionando o botão de energia e mantendo-o pressionado por 10 segundos. Religue-o em seguida. Isso irá disparar a atualização.  

3.  Vá para o aplicativo de Ativação do Modo de Desenvolvedor e selecione **Alternar e reiniciar**. Parabéns! Agora você tem um Xbox One no Modo de Desenvolvedor!
  
  > 
              **Observação**&nbsp;&nbsp;Seus aplicativos e jogos de varejo não serão executados no Modo de Desenvolvedor, apenas os aplicativos ou jogos que você criar. Volte para o Modo de Varejo para executar seus jogos e aplicativos favoritos.
  
  > 
              **Observação**&nbsp;&nbsp;Para poder implantar um aplicativo ao Xbox One no Modo de Desenvolvedor, você deve ter um usuário conectado no console. Você pode usar sua conta do Xbox Live existente ou criar uma nova conta para seu console em modo de desenvolvimento. 

## Criando seu primeiro projeto no Visual Studio 2015

Consulte [Development environment setup](development-environment-setup.md) para obter informações mais detalhadas.

1.  Para C#: crie um novo projeto Universal do Windows, vá para as propriedades do projeto e selecione a guia **Depurar**. Altere **Dispositivo de Destino** para **Computador Remoto**, digite o endereço IP ou o nome do host do seu Xbox One no campo **Computador remoto** e selecione **Universal (Protocolo Não Criptografado)** na lista suspensa **Modo de Autenticação**.   

    Você pode encontrar o endereço IP do seu Xbox One iniciando a Dev Home no seu console (o grande bloco do lado direito de Início), no canto superior esquerdo. Consulte [Introduction to Xbox One tools](introduction-to-xbox-tools.md) para obter mais informações sobre a Dev Home.  

2.  Para projetos em C++ e HTML/Javascript: siga um caminho semelhante mas, nas propriedades do projeto, vá para a guia **Depuração**, selecione **Computador Remoto** no Depurador para abrir a lista suspensa. Digite o endereço IP ou o nome do host do seu console no campo **Nome do Computador** e selecione **Universal (Protocolo Não Criptografado** no campo **Modo de Autenticação**.
   
3.  Quando você pressionar F5, seu aplicativo será compilado e começará a ser implantado em seu Xbox One.
  
4.  Na primeira vez que você fizer isso, o Visual Studio solicitará um PIN para seu Xbox One. Você pode obter um PIN a partir da Dev Home do Xbox One, pressionando o botão **Pair with Visual Studio**.
  
5.  Depois que você tiver feito o emparelhamento, o aplicativo começará a implantação. A primeira vez que você fizer isso pode ser um pouco lenta (precisamos copiar todas as ferramentas para o seu Xbox), mas se o processo demorar mais de alguns minutos, provavelmente há algo de errado. Certifique-se de que você seguiu todas as etapas acima (em especial, você configurou o **Modo de Autenticação** como **Universal**?) e que você está usando uma conexão de rede com fio em seu Xbox One.  

6. Relaxe. Aproveite seu primeiro aplicativo em execução no console!  

## Pronto!

![Hello World](images/getting-started-hello-world.png)

## Consulte também  
- [Perguntas frequentes](frequently-asked-questions.md)  
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Jul16_HO2-->


