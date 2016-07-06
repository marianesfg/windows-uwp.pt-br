---
author: Mtoepke
title: Configurar seu ambiente de desenvolvimento da UWP no Xbox
description: Etapas para configurar e testar sua UWP no ambiente de desenvolvimento do Xbox.
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: d56206f990e7885af4935401356bd3a2ce2cd292

---

# Configurar seu ambiente de desenvolvimento da UWP no Xbox

A Plataforma Universal do Windows (UWP) no ambiente de desenvolvimento do Xbox consiste em um computador de desenvolvimento conectado a um console Xbox One por meio de uma rede local.
O computador de desenvolvimento requer o Windows 10, o Visual Studio 2015 Atualização 2, o Windows 10 SDK Preview (compilação 14295) e uma variedade de ferramentas de suporte.


Este artigo discute as etapas para configurar e testar seu ambiente de desenvolvimento.

## Instalação do Visual Studio

1. Instale o Visual Studio 2015 Atualização 2 ou versão posterior. Para saber mais e obter informações de instalação, consulte [Downloads e ferramentas para o Windows 10](https://dev.windows.com/downloads).

1. Ao instalar o Visual Studio 2015 Atualização 2, certifique-se de que a caixa de seleção **Ferramentas de Desenvolvimento de Aplicativos Universais do Windows** esteja marcada.

  ![Instalar o Visual Studio 2015 Atualização 2](images/vs_install_tools.png)

## Instalação do Windows 10 SDK

Instale a versão prévia mais recente do SDK do Windows 10. Para obter informações de instalação, consulte [Baixe atualizações do Insider Preview para desenvolvedores](http://go.microsoft.com/fwlink/p/?LinkId=780552).

  > **Importante**
            &nbsp;&nbsp;Você precisa instalar o SDK mais recente, mas _não_ precisa instalar a versão mais recente do Windows Insider Preview do sistema operacional.

## Configurando o Xbox One

Para que você possa implantar um aplicativo em seu Xbox One, precisa ter um usuário conectado ao console. Você pode usar sua conta do Xbox Live existente ou criar uma nova conta para seu console em modo de desenvolvimento. 

## Criar seu primeiro aplicativo

1. Verifique se o computador de desenvolvimento está na mesma rede local que o console Xbox One de destino. Em geral, isso significa que eles devem usar o mesmo roteador e devem estar na mesma sub-rede. Uma conexão de rede com fio é recomendada.

1. Verifique se o seu console Xbox One está no Modo de Desenvolvedor.  Para saber mais, consulte [Habilitando o Modo de Desenvolvedor no Xbox One](devkit-activation.md).

1. Decida qual linguagem de programação você deseja usar para o seu aplicativo UWP.

1. No computador de desenvolvimento, selecione **Novo Projeto** e depois **Windows / Universal / Aplicativo em Branco**.

### Iniciando um projeto C#

  ![Caixa de diálogo Novo Projeto](images/vs_universal_blank.jpg)

1. Selecione as opções padrão na caixa de diálogo **Novo Projeto do Windows Universal**. Se a caixa de diálogo **Modo de Desenvolvedor** aparecer, clique em **OK**. Um novo aplicativo em branco é criado.

1. Configure seu ambiente de desenvolvimento para depuração remota:

  1. Clique com o botão direito do mouse no projeto e selecione **Propriedades**.
  1. Na guia **Depuração**, mude **Dispositivo de destino** para **Computador Remoto**.
  1. Em **Computador remoto**, insira o endereço IP do sistema ou o nome do host do console Xbox One. Para saber mais sobre como obter o endereço IP ou o nome do host, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).
  1. Na lista suspensa **Modo de Autenticação**, selecione **Universal (Protocolo Descriptografado)**.

    ![Páginas de Propriedades de BlankApp em C#](images/vs_remote.jpg)

### Iniciando um projeto C++

  ![Projeto C++](images/vs_universal_cpp_blank.jpg)

1. Selecione as opções padrão na caixa de diálogo **Novo Projeto do Windows Universal**. Se a caixa de diálogo **Modo de Desenvolvedor** aparecer, clique em **OK**. Um novo aplicativo em branco é criado.

1. Configure seu ambiente de desenvolvimento para depuração remota:

   1. Clique com o botão direito do mouse no projeto e selecione **Propriedades**.
   1. Na guia **Depuração**, mude **Depurador a iniciar** para **Computador Remoto**.
   1. Em **Nome do Computador**, insira o endereço IP do sistema ou o nome do host do console Xbox One. Para saber mais sobre como obter o endereço IP ou o nome do host, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).
   1. Na lista suspensa **Tipo de Autenticação**, selecione **Universal (Protocolo Descriptografado)**.

    ![Páginas de Propriedades de BlankApp em C++](images/vs_remote_cpp.jpg)

### Emparelhar seu dispositivo com o Visual Studio usando um PIN

1. Salve suas configurações e verifique se o console Xbox One está no Modo de Desenvolvedor.

1. Pressione F5.

1. Se esta for a sua primeira implantação, você verá uma caixa de diálogo do Visual Studio solicitando o emparelhamento do dispositivo com o uso de um PIN.

  1. Para obter um PIN, abra **Dev Home** na tela inicial do seu console Xbox One.
  1. Selecione **Emparelhar com o Visual Studio**.

    ![Caixa de diálogo Emparelhar com o Visual Studio](images/devhome_visualstudio.png)

  1. Insira seu PIN na caixa de diálogo **Emparelhar com o Visual Studio**. O PIN a seguir é apenas um exemplo. O seu será diferente.

    ![Caixa de diálogo Emparelhar com o PIN do Visual Studio](images/devhome_pin.png)

  1. Erros de implantação, se houver, aparecerão na janela **Saída**.

Parabéns! Você criou e implantou com êxito o seu primeiro aplicativo UWP no Xbox!



## Consulte também
- [Habilitando o Modo de Desenvolvedor no Xbox One](devkit-activation.md)  
- [Downloads e ferramentas para o Windows 10](https://dev.windows.com/downloads)  
- [Baixe atualizações do Insider Preview para desenvolvedores](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md) 
- [UWP no Xbox One](index.md)

----



<!--HONumber=Jun16_HO5-->


