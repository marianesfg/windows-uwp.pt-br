---
ms.assetid: a56156e4-7adb-bf37-527b-fc3243e04b46
title: Home de desenvolvedor no Console (Dev Home)
description: Fornece informações sobre o aplicativo Dev Home para o Xbox One.
ms.date: 08/09/2017
ms.topic: article
keywords: windows 10, uwp
permalink: en-us/docs/xdk/dev-home.html
ms.localizationpriority: medium
ms.openlocfilehash: 4113df37446d93883cf395e7c1e86b1de6c1b328
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8194732"
---
# <a name="developer-home-on-the-console-dev-home"></a>Home de desenvolvedor no Console (Dev Home)
   
  
A dev Home é uma experiência de ferramentas no Xbox One development kit criada para auxiliar a produtividade do desenvolvedor. A dev Home oferece funcionalidade para gerenciar e configurar o kit de desenvolvimento, gerenciar usuários, iniciar títulos instalados e realizar captura e rastreia. Em futuras versões que continuaremos a expandir a funcionalidade para habilitar recursos adicionais com base nos seus comentários e também para permitir a extensibilidade e a adição de suas próprias ferramentas.   
   
  
Estamos muito interessados em seus comentários sobre a Dev Home e os cenários que você está mais interessado em ver o suporte. Forneça comentários por meio dos métodos descritos em **Enviar comentários** no menu principal do aplicativo ou por meio de seu Gerenciador de conta de desenvolvedor (mãe).   
   
  
Para iniciar a Dev Home em novembro de 2015 ou posterior recuperação:  
 
   1. Abra o guia movendo esquerdo em Home ou clicar no botão Nexus double  
   1. Mover para baixo para **configurações** (o ícone de engrenagem)   
   1. Selecione **todas as configurações**  
   1. Na página do **desenvolvedor** padrão, selecione **Home de desenvolvedor** (o ícone inicial)   

 ![](images/dev_home_icons.png)   
  
Em recuperações anteriores selecione o bloco Dev Home no lado direito da tela inicial no **conteúdo de destaque** ou exibir a lista de aplicativos no Gerenciador do Xbox One e iniciar **Dev Home**.   
 ![](images/dev_home_1.png) 
<a id="ID4EBC"></a>

   

## <a name="user-interface"></a>Interface do usuário  
   
  
O cabeçalho da interface do usuário Dev Home contém as seguintes importantes "em um relance" informações sobre o console de desenvolvimento:   
 
   *  **IP do console:** O endereço IP atual do console.   
   *  **Nome do console:** O nome do host atual do console.  
   *  **Área restrita:** O nome da área restrita que o console está no.  
   *  **Versão do sistema operacional:** A versão de recuperação atual que está em execução no console.
   *  Hora atual do sistema.   

   
  
O restante da Dev Home interface do usuário é dividido em páginas a seguir. Para obter mais informações sobre as ferramentas sobre essas páginas, consulte seus tópicos individuais.   
 
   *  [Início](devhome-home.md)  
   *  [Xbox Live](devhome-live.md)  
   *  [Settings](devhome-settings.md)  
   *  [Captura de mídia](devhome-capture.md)  
   *  [Rede](devhome-networking.md)  
   *  [Desempenho](devhome-performance.md)  

  
<a id="ID4EKE"></a>

   

## <a name="main-menu"></a>Menu principal  
   
  
Pressionando o botão de **menu** em seu controlador, você pode acessar o menu principal que permite a configuração de espaço de trabalho do aplicativo, a capacidade de gerenciar credenciais para acessar locais de rede e informações sobre como fornecer comentários sobre o aplicativo.   
  
<a id="ID4EUE"></a>

   

## <a name="snap-mode-ux"></a>Experiência do usuário do modo de ajuste  
   
  
Várias ferramentas existentes e futuras na Dev Home, como rede e vários jogadores, são projetadas para ser usado ajustado ao lado enquanto você estiver executando seu título, para que você possa ter acesso fácil às ferramentas enquanto você está testando.   
   
  
Para acessar o modo de ajuste, realçar o título da ferramenta apropriada, pressione o botão **modo de exibição** em seu controlador e selecione o **ajuste** no menu de contexto:  
 ![](images/dev_home_4.png)   
  
A Dev Home será ajustada à direita. Você pode alternar o contexto tocando duas vezes no botão Nexus como de costume.  
 ![](images/dev_home_5.png)  
<a id="ID4EKF"></a>

   

## <a name="customizing-dev-home"></a>Personalizando a Dev Home  
   
  
A Dev Home foi projetada para ser personalizável e pessoal. Você pode configurar o aplicativo de acordo com seu fluxo de trabalho e, em seguida, salve isso como um espaço de trabalho. Espaço de trabalho pode ser exportado e importado, permitindo que você copie o layout para outros consoles como necessário. Essas opções estão localizadas no menu principal em **espaço de trabalho**. O arquivo exportado estarão localizado na unidade do sistema transitório no `Dev Home\Workspaces` diretório.   
 
<a id="ID4EVF"></a>

   

### <a name="resizing-and-reordering-tools"></a>Redimensionando e reordenando ferramentas  
   
  
Para alterar o tamanho ou a posição de uma ferramenta, use o botão de menu de contexto (botão modo de exibição em seu controlador) enquanto o título tem o foco. No menu de contexto, selecione **Mover** ou **Redimensionar**.   
 ![](images/dev_home_6.png)  
<a id="ID4EEG"></a>

   

### <a name="changing-theme-color-and-background-image"></a>Alterando a cor do tema e a imagem do plano de fundo  
   
  
No Menu principal, você pode selecionar o **espaço de trabalho** e, em seguida, **alterar a cor de tema**. Selecione uma nova cor e **Salvar** para atualizar a cor do tema usada para realçar o foco.   
 ![](images/dev_home_7.png)  
<a id="ID4EVG"></a>

   

### <a name="setting-the-default-application-for-a-package"></a>Configurando o aplicativo padrão para um pacote  
   
  
Se um pacote contém vários aplicativos, Dev Home permitirá que você defina o aplicativo padrão para ser iniciado. Realce o pacote no inicializador e pressione **o botão para abrir a lista de aplicativos disponíveis** . Realce aquele que você deseja definir como padrão e pressione o botão **modo de exibição** e, em seguida, escolha **Definir como padrão** no menu de contexto.   
 ![](images/dev_home_setdefault.png)  
<a id="ID4EGH"></a>

   

### <a name="using-dev-home-to-register-and-launch-titles-from-a-network-share"></a>Usar a Dev Home para registrar e iniciar títulos de um compartilhamento de rede  
   
  
Do iniciador, na parte inferior da lista de jogos de aplicativos instalados e você pode selecionar a opção de **registrar um jogo de um compartilhamento de rede** para executar uma versão de arquivo flexível de um título remotamente.   
 ![](images/dev_home_8.png)   
  
Em seguida, você pode inserir o caminho de rede para o arquivo appxmanifest XML para o título que você deseja registrar. Dev Home tentativa de registrar o título usando as credenciais existentes para esse compartilhamento e se for necessário solicitará para novas credenciais de rede. Se você precisar acessar compartilhamentos adicionais (por exemplo, para acessar vinculado de maneira simbólica recursos em um servidor separado), em seguida, você precisará adicioná-las por meio da opção abaixo.   
   
  
Você pode gerenciar essas credenciais armazenadas (e adicionar demais) no console por meio da opção **Gerenciar credenciais de rede** do menu principal.   
 ![](images/dev_home_9.png)   
  
Você pode exibir as credenciais atualmente no console, editar credenciais selecionando o caminho da credencial e clicando em **um** botão e remover uma credencial selecionando o link de remover e clicando em **um** botão.   
   
<a id="ID4EGAAC"></a>

   

## <a name="in-this-section"></a>Nesta seção  
  
[Página inicial (Dev Home)](devhome-home.md)  


&nbsp;&nbsp;Fornece acesso rápido às tarefas que são executadas rotineiramente em um console de desenvolvimento. 
  
  
[Página (Dev Home) do Xbox Live](devhome-live.md)  


&nbsp;&nbsp;Captura informações multijogador e exibe o status atual do serviço Xbox Live. 
  
  
[Página de configurações (Dev Home)](devhome-settings.md)  


&nbsp;&nbsp;Fornece acesso às várias configurações para o console de desenvolvimento. 
  
  
[Captura de mídia página (Dev Home)](devhome-capture.md)  


&nbsp;&nbsp;A página **de captura de mídia** de Dev Home capturas de vídeo do título que está sendo executado no console. 
  
  
[Página de rede (Dev Home)](devhome-networking.md)  


&nbsp;&nbsp;Simula várias condições de redes para fins de solução de problemas. 
  
  
[Página de desempenho (Dev Home)](devhome-performance.md)  


&nbsp;&nbsp;Simula várias condições de uso de CPU para fins de solução de problemas e atividade de disco. 
 