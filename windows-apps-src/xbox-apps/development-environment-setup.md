---
author: Mtoepke
title: Configurar seu ambiente de desenvolvimento da UWP no Xbox
description: Etapas para configurar e testar sua UWP no ambiente de desenvolvimento do Xbox.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 8801c0d9-94a5-41a2-bec3-14f523d230df
ms.localizationpriority: medium
ms.openlocfilehash: 2234b7d39f130da03562176f0df878701d524635
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5861313"
---
# <a name="set-up-your-uwp-on-xbox-development-environment"></a>Configurar seu ambiente de desenvolvimento da UWP no Xbox

A Plataforma Universal do Windows (UWP) no ambiente de desenvolvimento do Xbox consiste em um computador de desenvolvimento conectado a um console Xbox One por meio de uma rede local.
O computador de desenvolvimento requer o Windows 10, o Visual Studio 2017 ou o Visual Studio 2015 Atualização 3, o SDK do Windows 10 compilação 14393 e uma variedade de ferramentas de suporte.


Este artigo discute as etapas para configurar e testar seu ambiente de desenvolvimento.

## <a name="visual-studio-setup"></a>Instalação do Visual Studio

1. Instale o Visual Studio 2017, Visual Studio 2015 atualização 3 ou a versão mais recente do Visual Studio. Para saber mais e obter informações de instalação, consulte [Downloads e ferramentas para o Windows 10](https://dev.windows.com/downloads). Recomendamos que você use a versão mais recente do Visual Studio para que você possa receber as atualizações mais recentes para desenvolvedores e segurança.

2. Se estiver instalando o Visual Studio 2017, escolha a carga de trabalho **Desenvolvimento da Plataforma Universal do Windows**. Se você for um desenvolvedor de C++, marque também a caixa de seleção **Ferramentas C++ da Plataforma Universal do Windows** no painel **Resumo** à direita, em **Desenvolvimento da Plataforma Universal do Windows**. Não faz parte da instalação padrão.

    ![Instalar o Visual Studio 2017](images/development-environment-setup-1.png)

    Se estiver instalando o Visual Studio 2015 Atualização 3, certifique-se de que a caixa de seleção **Ferramentas de Desenvolvimento de Aplicativos Universais do Windows** esteja marcada.

    ![Instalar o Visual Studio 2015 Atualização 2](images/vs_install_tools.png)

## <a name="windows-10-sdk-setup"></a>Instalação do Windows 10 SDK

Instale o SDK mais recente do Windows 10. Isso é fornecido com a instalação do Visual Studio, mas, se você desejar baixá-lo separadamente, consulte [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).


## <a name="enabling-developer-mode"></a>Habilitar o modo de desenvolvedor

Antes de implantar aplicativos em seu computador de desenvolvimento, você deve habilitar o modo de desenvolvedor. No aplicativo **Configurações**, navegue até **Atualização e segurança** / **Para desenvolvedores** e, em **Usar recursos de desenvolvedor**, selecione **Modo de desenvolvedor**.

## <a name="setting-up-your-xbox-one"></a>Configurando o Xbox One

Para que você possa implantar um aplicativo em seu Xbox One, precisa ter um usuário conectado ao console. Você pode usar sua conta do Xbox Live existente ou criar uma nova conta para seu console em modo de desenvolvimento. 

## <a name="create-your-first-app"></a>Crie seu primeiro aplicativo

1. Verifique se o computador de desenvolvimento está na mesma rede local que o console Xbox One de destino. Em geral, isso significa que eles devem usar o mesmo roteador e devem estar na mesma sub-rede. Uma conexão de rede com fio é recomendada.

2. Verifique se o seu console Xbox One está no Modo de Desenvolvedor.  Para saber mais, consulte [Ativação do modo de desenvolvedor do Xbox One](devkit-activation.md).

3. Decida qual linguagem de programação que você deseja usar para o seu aplicativo UWP.

4. No computador de desenvolvimento, no Visual Studio, selecione **Novo / Projeto**.

5. Na janela **Novo projeto**, selecione **Windows Universal / Aplicativo em branco (Universal Windows)**.

### <a name="starting-a-c-project"></a>Iniciando um projeto C#

  ![Caixa de diálogo Novo Projeto](images/development-environment-setup-2.png)

1. Na caixa de diálogo **Novo Projeto Universal do Windows**, selecione a compilação 14393 ou posterior na lista suspensa **Versão mínima**. Selecione o SDK mais recente na lista suspensa **Versão de destino**. Se a caixa de diálogo **Modo de Desenvolvedor** aparecer, clique em **OK**. Um novo aplicativo em branco é criado.

2. Configure seu ambiente de desenvolvimento para depuração remota:

    a. Clique com o botão direito no projeto no **Gerenciador de Soluções** e selecione **Propriedades**.

    b. Na guia **Depurar**, altere **Plataforma** para **x64**. (x86 não é uma plataforma com suporte no Xbox).

    c. Em **Opções de inicialização**, mude **Dispositivo de destino** para **Computador Remoto**.

    d. Em **Computador remoto**, insira o endereço IP do sistema ou o nome do host do console Xbox One. Para saber mais sobre como obter o endereço IP ou o nome do host, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

    e. Na lista suspensa **Modo de Autenticação**, selecione **Universal (Protocolo Descriptografado)**.

    ![Páginas de Propriedades de BlankApp em C#](images/vs_remote.jpg)

### <a name="starting-a-c-project"></a>Iniciando um projeto C++

  ![Projeto C++](images/development-environment-setup-3.png)

1. Na caixa de diálogo **Novo Projeto Universal do Windows**, selecione a compilação 14393 ou posterior na lista suspensa **Versão mínima**. Selecione o SDK mais recente na lista suspensa **Versão de destino**. Se a caixa de diálogo **Modo de Desenvolvedor** aparecer, clique em **OK**. Um novo aplicativo em branco é criado.

2. Configure seu ambiente de desenvolvimento para depuração remota:

   a. Clique com o botão direito no projeto no **Gerenciador de Soluções** e selecione **Propriedades**.

   b. Na guia **Depuração**, mude **Depurador a iniciar** para **Computador Remoto**.

   c. Em **Nome do Computador**, insira o endereço IP do sistema ou o nome do host do console Xbox One. Para saber mais sobre como obter o endereço IP ou o nome do host, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

   d. Na lista suspensa **Tipo de Autenticação**, selecione **Universal (Protocolo Descriptografado)**.

   e. Na lista suspensa **Plataforma**, selecione **x64**.

    ![Páginas de Propriedades de BlankApp em C++](images/development-environment-setup-4.png)

### <a name="pin-pair-your-device-with-visual-studio"></a>Emparelhar seu dispositivo com o Visual Studio usando um PIN

1. Salve suas configurações e verifique se o console Xbox One está no Modo de Desenvolvedor.

2. Com o projeto aberto no Visual Studio, pressione F5.

3. Se esta for a sua primeira implantação, você verá uma caixa de diálogo do Visual Studio solicitando o emparelhamento do dispositivo com o uso de um PIN.

    a. Para obter um PIN, abra **Dev Home** na tela Início do seu console Xbox One.

    b. Na guia **Início**, em **Ações rápidas**, selecione **Mostrar PIN do Visual Studio**.
  
    ![Caixa de diálogo Emparelhar com o Visual Studio](images/development-environment-setup-5.png)

    c. Insira seu PIN na caixa de diálogo **Emparelhar com o Visual Studio**. O PIN a seguir é apenas um exemplo. O seu será diferente.

    ![Caixa de diálogo Emparelhar com o PIN do Visual Studio](images/devhome_pin.png)

    d. Erros de implantação, se houver, aparecerão na janela **Saída**.

Parabéns! Você criou e implantou com êxito o seu primeiro aplicativo UWP no Xbox!

## <a name="see-also"></a>Consulte também
- [Ativação do Modo de Desenvolvedor do Xbox One](devkit-activation.md)  
- [Downloads e ferramentas para o Windows 10](https://dev.windows.com/downloads)  
- [Programa Windows Insider](http://go.microsoft.com/fwlink/?LinkId=780552)  
- [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md) 
- [UWP no Xbox One](index.md)