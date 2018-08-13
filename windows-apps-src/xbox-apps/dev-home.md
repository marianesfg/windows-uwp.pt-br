---
author: v-angraf
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Página inicial do desenvolvedor no Console (Dev Home)
description: Fornece informações sobre o aplicativo Dev Home para um Xbox.
ms.author: v-angraf@microsoft.com
ms.date: 08/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 3b802b9b53811e03e11ee3afd78f69db4bfd9986
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "1015355"
---
# <a name="developer-home-on-the-console-dev-home"></a>Página inicial do desenvolvedor no Console (Dev Home)
   
  
Home dev é uma experiência de ferramentas no Xbox um kit de desenvolvimento projetada para auxiliar a produtividade do desenvolvedor. Home dev oferece funcionalidade para gerenciar e configurar o kit de desenvolvimento, gerenciar usuários, início títulos instalados e realizar captura e rastreia. Em futuras versões que podemos continuará a expandir a funcionalidade para habilitar recursos adicionais, com base nos seus comentários e também para permitir a adição de suas próprias ferramentas e extensibilidade.   
   
  
Estamos muito interessados em seus comentários sobre Dev Home e os cenários em que você está interessado mais vê-lo a oferecer suporte. Forneça comentários por meio dos métodos descritos em **Enviar comentários** no menu principal do aplicativo ou por meio de seu desenvolvedor conta Manager (mãe).   
   
  
Para iniciar o Dev inicial sobre o de 2015 novembro ou recuperação posterior:  
 
   1. Abra o guia movendo à esquerda em casa ou double clicando no botão de ligação  
   1. Mover para baixo para **configurações** (ícone de engrenagem)   
   1. Selecione **todas as configurações**  
   1. Na página de **desenvolvedor** do padrão, selecione **Home do desenvolvedor** (o ícone residencial)   

 ![](images/dev_home_icons.png)   
  
Na recuperação anteriores selecione os blocos de Dev Home no lado direito da tela inicial em **Destaque conteúdo** ou exibir a lista de aplicativos no Gerenciador de um Xbox e início **Dev Home**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface do usuário  
   
  
O cabeçalho da interface do usuário Dev Home contém o seguinte importante "em um relance" informações sobre o console de desenvolvimento:   
 
   *  **IP de console:** O endereço IP atual do console.   
   *  **Nome console:** O nome de host atual do console.  
   *  **Área restrita:** O nome da área de segurança que o console é in.  
   *  **Versão do sistema operacional:** A versão de recuperação atual que está executando no console.
   *  Hora atual do sistema.   

   
  
O restante do Dev Home interface do usuário é dividido em páginas a seguir. Para obter mais informações sobre as ferramentas essas páginas, consulte seus tópicos individuais.   
 
   *  [Início](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Settings](devhome-settings.md)  
   *  [Captura de mídia](devhome-capture.md)  
   *  [Rede](devhome-networking.md)  
   *  [Desempenho](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu principal  
   
  
Pressionando o botão **menu** no seu controlador, você pode acessar o menu principal que permite a configuração de espaço de trabalho do aplicativo, a capacidade de gerenciar credenciais para acessar os locais de rede e obter informações sobre como fornecer comentários sobre o aplicativo.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Ajustar o modo eu  
   
  
Várias ferramentas existentes e futuras no Dev casa, como a rede e com vários participantes, foram projetadas para ser usado aderido até o lado enquanto você estiver executando o seu título, para que você possa ter acesso fácil a ferramentas enquanto você estiver testando.   
   
  
Para acessar o modo de Snap, realce o título da ferramenta adequada, pressione o botão **Exibir** no seu controlador e selecione **Ajustar** no menu de contexto:  
 ![](images/dev_home_4.png)   
  
A Dev Home será ajustada à direita. Você pode alternar o contexto tocando duas vezes no botão Nexus como de costume.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizando a Dev Home  
   
  
A Dev Home foi projetada para ser personalizável e pessoal. Você pode configurar o aplicativo de acordo com seu fluxo de trabalho e então salve que como um espaço de trabalho. Este espaço de trabalho pode ser exportado e importadas, permitindo que você copie o layout para outros consoles como necessário. Essas opções estão localizadas no menu principal em um **espaço de trabalho**. O arquivo exportado estará localizado na unidade do sistema scratch no `Dev Home\Workspaces` directory.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Redimensionamento e reordenar ferramentas  
   
  
Para alterar o tamanho ou posição de uma ferramenta, use o botão de menu de contexto (botão Exibir no seu controlador) enquanto o título tem o foco. No menu de contexto, selecione **Mover** ou **Redimensionar**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Alterando a cor do tema e a imagem do plano de fundo  
   
  
No Menu principal, você pode selecionar o **espaço de trabalho** e, em seguida, **alterar a cor de tema**. Selecione uma nova cor e selecione **Salvar** para atualizar a cor do tema usada para realçar foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configurando o aplicativo padrão para um pacote  
   
  
Se um pacote contém vários aplicativos, Dev Home permitirá que você defina o aplicativo padrão a ser iniciado. Realce o pacote no iniciador e pressione o botão **uma** para abrir a lista de aplicativos disponíveis. Realce aquele que deseja definir como padrão e pressione o botão de **modo de exibição** e clique em **Definir como padrão** , no menu de contexto.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Usando Dev Home para registrar e iniciar os títulos de um compartilhamento de rede  
   
  
Do iniciador, na parte inferior da lista de jogos, de aplicativos instalados e você pode selecionar a opção **registrar um jogo de um compartilhamento de rede** para executar uma versão do arquivo afastado do título remotamente.   
 ![](images/dev_home_8.png)   
  
Você pode, em seguida, digite o caminho de rede para o arquivo appxmanifest.xml para o título que você deseja registrar. Home dev tentará registrar o título usando as credenciais existentes para esse compartilhamento e se necessário vai solicitar novas credenciais de rede. Se você precisar acessar compartilhamentos adicionais (por exemplo, para recursos de acesso simbolicamente vinculado em um servidor separado), em seguida, você precisará adicioná-los por meio da opção abaixo.   
   
  
Você pode gerenciar essas credenciais armazenadas (e adicionar outros adicionais) no console via opção de **Gerenciar credenciais de rede** do menu principal.   
 ![](images/dev_home_9.png)   
  
Você pode exibir as credenciais atualmente no console, editar credenciais selecionando o caminho da credencial e clicando em **um** botão e remover uma credencial selecionando o link remover e clicando em **um** botão.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Nesta seção  
  
[Página inicial (Home Dev)](devhome-home.md)  


&nbsp;&nbsp;Fornece acesso rápido às tarefas rotineiramente executadas em um console de desenvolvimento. 
  
  
[Page (Dev Home) do Xbox Live](devhome-live.md)  


&nbsp;&nbsp;Captura informações com vários participantes e exibe o status atual do serviço Xbox Live. 
  
  
[Página de configurações (Dev Home)](devhome-settings.md)  


&nbsp;&nbsp;Fornece acesso a várias configurações para o console de desenvolvimento. 
  
  
[Captura de mídia de página (Dev Home)](devhome-capture.md)  


&nbsp;&nbsp;Página de **captura de mídia** inicial Dev captura vídeo do que está sendo executada no console do título. 
  
  
[Página de rede (Dev Home)](devhome-networking.md)  


&nbsp;&nbsp;Simula várias condições de redes para fins de solução de problemas. 
  
  
[Página de desempenho (Dev Home)](devhome-performance.md)  


&nbsp;&nbsp;Simula várias atividade de disco e condições de uso de CPU para fins de solução de problemas. 
 