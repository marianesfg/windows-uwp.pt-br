---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Página inicial do desenvolvedor no Console (Home Page de Desenvolvimento)
description: Fornece informações sobre o aplicativo Home Page de Desenvolvimento para Xbox One.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620841"
---
# <a name="developer-home-on-the-console-dev-home"></a>Página inicial do desenvolvedor no Console (Home Page de Desenvolvimento)
   
  
A Home Page de Desenvolvimento é uma experiência de ferramentas no kit de desenvolvimento do Xbox One criada para auxiliar a produtividade do desenvolvedor. A dev Home oferece funcionalidades para gerenciar e configurar o kit de desenvolvimento, gerenciar usuários, iniciar títulos instalados e executar capturas e rastros. Nas próximas versões, continuaremos a expandir as funcionalidades para habilitar recursos adicionais com base nos seus comentários, e também para habilitar extensibilidade e permitir a adição de suas próprias ferramentas.   
   
  
Estamos muito interessados nos seus comentários sobre a Home Page de Desenvolvimento e sobre os cenários que está mais interessado em vê-la oferecer suporte. Por favor, forneça comentários por meio dos métodos descritos em **Enviar Feedback** no menu principal do aplicativo ou por meio do seu Gerenciador de Conta de Desenvolvedor (DAM).   
   
  
Para iniciar a Home Page de Desenvolvimento na recuperação de novembro de 2015 ou posterior:  
 
   1. Abra o guia movendo para a esquerda na Página inicial, ou clicando duas vezes no botão Nexus.  
   1. Desça até **Configurações** (o ícone de engrenagem)   
   1. Selecione **Todas as configurações**  
   1. Na página padrão **Desenvolvedor**, selecione **Página inicial do Desenvolvedor** (o ícone de casa)   

 ![](images/dev_home_icons.png)   
  
Em recuperações anteriores, selecione o bloco Home Page de Desenvolvimento no lado direito da tela inicial em **conteúdo em destaque** ou exiba a lista de aplicativos no Gerenciador do Xbox One e inicie a **Home Page de Desenvolvimento**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface de Usuário  
   
  
O cabeçalho da interface de usuário da Home Page de Desenvolvimento contém as seguintes informações "num relance" importantes sobre o console de desenvolvimento:   
 
   *  **IP do console:** O endereço IP atual do console.   
   *  **Nome do console:** O nome de host atual do console.  
   *  **Área restrita:** O nome da área de segurança que o console é in.  
   *  **Versão do sistema operacional:** A versão de recuperação atual que está sendo executado no console do.
   *  Hora atual do sistema.   

   
  
O restante da interface de usuário da Home Page de Desenvolvimento está dividida nas seguintes páginas. Para obter mais informações sobre as ferramentas nessas páginas, veja seus tópicos individuais.   
 
   *  [Casa](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Configurações](devhome-settings.md)  
   *  [Captura de mídia](devhome-capture.md)  
   *  [Rede](devhome-networking.md)  
   *  [Desempenho](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu Principal  
   
  
Ao pressionar o botão **menu** no seu controle, é possível acessar o menu principal que permite a configuração do espaço de trabalho do aplicativo, a capacidade de gerenciar credenciais de acesso a redes locais e informações sobre o envio de comentários no aplicativo.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Modo de Ajuste UX  
   
  
Várias ferramentas existentes e futuras na Home Page de Desenvolvimento, como Redes e Multiplayer, são projetadas para o uso ajustado ao lado enquanto seu título está sendo executado, para que você tenha fácil acesso a ferramentas enquanto realiza os testes.   
   
  
Para acessar o modo Ajuste, selecione o título da ferramenta apropriada, pressione o botão **exibir** no seu controle, e selecione **ajustar** no menu de contexto:  
 ![](images/dev_home_4.png)   
  
A Dev Home será ajustada à direita. Você pode alternar o contexto tocando duas vezes no botão Nexus, como de costume.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizando a Dev Home  
   
  
A Dev Home foi projetada para ser personalizável e pessoal. Você pode configurar o aplicativo de acordo com o seu fluxo de trabalho e, em seguida, salvar como um espaço de trabalho. Esse espaço de trabalho pode ser exportado e importado, permitindo que você copie o layout para outros consoles conforme necessário. Essas opções estão localizadas no menu principal em **espaço de trabalho**. O arquivo exportado estará localizado na unidade temporária do sistema, no diretório `Dev Home\Workspaces`.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Redimensionando e Reordenando Ferramentas  
   
  
Para alterar o tamanho ou a posição de uma ferramenta, use o botão do menu de contexto (botão Exibir no seu controle) enquanto o foco está no título. No menu de contexto, selecione **Mover** ou **Redimensionar**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Alterando a cor do tema e a imagem do plano de fundo  
   
  
No Menu principal, você pode selecionar **Espaço de trabalho** e, em seguida, **Alterar a cor do tema**. Selecione uma nova cor e selecione **Salvar** para atualizar a cor do tema usado para o destaque em foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configurando o aplicativo padrão para um pacote  
   
  
Se um pacote contém vários aplicativos, a Home Page de Desenvolvimento permitirá que você defina o aplicativo padrão para ser iniciado. Escolha o pacote no iniciador e pressione o botão **A** para abrir a lista de aplicativos disponíveis. Escolha aquele que deseja definir como padrão e pressione o botão **exibir** e, em seguida, selecione **Definir como Padrão** no menu de contexto.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Usando a Home Page de Desenvolvimento para registrar e iniciar títulos de um compartilhamento de rede  
   
  
No iniciador, na parte inferior da lista de jogos e aplicativos instalados, você pode selecionar a opção **Registrar um jogo de um compartilhamento de rede** para executar uma versão de arquivo flexível de um título remotamente.   
 ![](images/dev_home_8.png)   
  
Em seguida, você pode inserir o caminho de rede no arquivo appxmanifest.xml para o título que deseja registrar. A Home Page de Desenvolvimento irá tentar registrar o título usando as credenciais existentes para esse compartilhamento e, se necessário, solicitará novas credenciais de rede. Se você precisar acessar compartilhamentos adicionais (por exemplo, para acessar recursos vinculados de maneira simbólica em um servidor separado) será necessário adicioná-los através da opção abaixo.   
   
  
Você pode gerenciar essas credenciais armazenadas (e adicionar outras) no console através da opção **Gerenciar credenciais de rede** no menu principal.   
 ![](images/dev_home_9.png)   
  
Você pode visualizar as credenciais que estão no console no momento, editar credenciais selecionando o caminho da credencial e clicando no botão **A**, e remover uma credencial selecionando o link de remoção e clicando no botão **A**.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Nesta seção  
  
[Home Page (página inicial do desenvolvimento)](devhome-home.md)  


&nbsp;&nbsp;Fornece acesso rápido a tarefas que são executadas rotineiramente em um console de desenvolvimento. 
  
  
[Xbox Live página (página inicial do desenvolvimento)](devhome-live.md)  


&nbsp;&nbsp;Captura informações com vários participantes e exibe o status atual do serviço Xbox Live. 
  
  
[Página de configurações (desenvolvimento Home)](devhome-settings.md)  


&nbsp;&nbsp;Fornece acesso a várias configurações para o console de desenvolvimento. 
  
  
[Captura de mídia de página (página inicial do desenvolvimento)](devhome-capture.md)  


&nbsp;&nbsp;O **captura de mídia** página da página inicial do desenvolvimento captura de vídeo do título que está sendo executado no console. 
  
  
[Página de rede (desenvolvimento Home)](devhome-networking.md)  


&nbsp;&nbsp;Simula várias condições de rede para fins de solução de problemas. 
  
  
[Página de desempenho (página inicial do desenvolvimento)](devhome-performance.md)  


&nbsp;&nbsp;Simula vários a atividade de disco e condições de uso de CPU para fins de solução de problemas. 
 