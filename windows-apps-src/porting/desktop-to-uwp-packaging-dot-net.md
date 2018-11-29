---
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop application.
Search.Product: eADQiWindows 10XVcnh
title: Empacotar um aplicativo da área de trabalho usando o Visual Studio
ms.date: 08/30/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: e867377c5961277d140173ab0de86d9f89197086
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "8191278"
---
# <a name="package-a-desktop-application-by-using-visual-studio"></a>Empacotar um aplicativo da área de trabalho usando o Visual Studio

Você pode usar o Visual Studio para gerar um pacote para seu aplicativo da área de trabalho. Em seguida, você pode publicar esse pacote para a Microsoft Store ou carregue-lo em um ou mais computadores.

A versão mais recente do Visual Studio fornece uma nova versão do projeto empacotamento que elimina todas as etapas manuais que costumavam ser necessárias para empacotar seu aplicativo. Basta adicionar um projeto de empacotamento, referenciar seu projeto de desktop e depois pressionar F5 para depurar seu aplicativo. Sem ajustes manuais. Essa nova experiência simplificada é uma grande melhoria em relação à experiência disponível na versão anterior do Visual Studio.

>[!IMPORTANT]
>A capacidade de criar um pacote de aplicativo do Windows para seu aplicativo da área de trabalho (também conhecido como a ponte de Desktop) foi introduzida no Windows 10, versão 1607, e pode ser usada somente em projetos para atualização de aniversário do Windows 10 (10.0; Build 14393) ou uma versão posterior no Visual Studio.

## <a name="first-prepare-your-application"></a>Primeiro, prepare seu aplicativo

Examine este guia antes de começar a criar um pacote para seu aplicativo: [preparar para empacotar um aplicativo da área de trabalho](desktop-to-uwp-prepare.md).

<a id="new-packaging-project"/>

## <a name="create-a-package"></a>Crie um pacote

1. No Visual Studio, abra a solução que contém seu projeto de aplicativo da área de trabalho.

2. Adicione um **Projeto de empacotamento de aplicativo do Windows** à sua solução.

   Você não terá que adicionar nenhum código a ele. Ele existe apenas para gerar um pacote para você. Vamos nos referir a esse projeto como o "projeto de empacotamento".

   ![Projeto de empacotamento](images/desktop-to-uwp/packaging-project.png)

   >[!NOTE]
   >Este projeto aparece apenas no Visual Studio 2017 versão 15.5 ou superior.

3. Defina a **Versão de destino** desse projeto como qualquer versão que deseja, mas certifique-se de definir a **Versão mínima** como **Atualização de Aniversário do Windows 10**.

   ![Caixa de diálogo do seletor de versão de empacotamento](images/desktop-to-uwp/packaging-version.png)

4. No projeto de empacotamento, clique com botão direito na pasta **Aplicativos** e escolha **Adicionar referência**.

   ![Adicionar referência de projeto](images/desktop-to-uwp/add-project-reference.png)

5. Escolha seu projeto de aplicativo da área de trabalho e, em seguida, escolha o botão **OK**.

   ![Projeto de desktop](images/desktop-to-uwp/reference-project.png)

   Você pode incluir vários aplicativos da área de trabalho em seu pacote, mas apenas um deles é iniciado quando os usuários escolhem o bloco do seu aplicativo. No nó **Aplicativos**, clique com botão direito no aplicativo que você quer que os usuários iniciem ao escolherem o bloco do aplicativo e escolha **Definir como ponto de entrada**.

   ![Definir ponto de entrada](images/desktop-to-uwp/entry-point-set.png)

6. Crie o projeto de empacotamento para garantir que nenhum erro apareça.  Se você receber erros, abra o **Gerenciador de configuração** e certifique-se de que seus projetos direcionados à mesma plataforma.

   ![Gerenciador de configuração](images/desktop-to-uwp/config-manager.png)

7. Use o assistente [Criar pacotes de aplicativo](../packaging/packaging-uwp-apps.md) para gerar um arquivo appxupload.

   Você pode carregar esse arquivo diretamente para a loja.

**Vídeo**

&nbsp;
> [!VIDEO https://www.youtube.com/embed/fJkbYPyd08w]

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Executar, depurar ou testar seu aplicativo da área de trabalho**

Consulte [Executar, depurar e testar um aplicativo da área de trabalho empacotado](desktop-to-uwp-debug.md)

**Aprimorar seu aplicativo da área de trabalho ao adicionar APIs UWP**

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](desktop-to-uwp-enhance.md)

**Estender seu aplicativo da área de trabalho adicionando projetos UWP e componentes de tempo de execução do Windows**

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](desktop-to-uwp-extend.md).

**Distribua seu aplicativo**

Consulte [distribuir um aplicativo da área de trabalho empacotado](desktop-to-uwp-distribute.md)
