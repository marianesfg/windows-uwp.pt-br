---
author: normesta
Description: This guide explains how to configure your Visual Studio Solution to edit, debug, and package desktop app for the Desktop Bridge.
Search.Product: eADQiWindows 10XVcnh
title: empacotar um app usando o Visual Studio (Ponte de Desktop)
ms.author: normesta
ms.date: 08/30/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.localizationpriority: medium
ms.openlocfilehash: d7ae77c499cb8398aa5557f0d422899fbe8b252d
ms.sourcegitcommit: 91511d2d1dc8ab74b566aaeab3ef2139e7ed4945
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/30/2018
ms.locfileid: "1816251"
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>empacotar um app usando o Visual Studio (Ponte de Desktop)

Você pode usar o Visual Studio para gerar um pacote para seu aplicativo da área de trabalho. Em seguida, você pode publicar esse pacote na Windows Store ou fazer o sideload dele em um ou mais computadores.

A versão mais recente do Visual Studio fornece uma nova versão do projeto empacotamento que elimina todas as etapas manuais que costumavam ser necessárias para empacotar seu aplicativo. Basta adicionar um projeto de empacotamento, referenciar seu projeto de desktop e depois pressionar F5 para depurar seu aplicativo. Sem ajustes manuais. Essa nova experiência simplificada é uma grande melhoria em relação à experiência disponível na versão anterior do Visual Studio.

>[!IMPORTANT]
>A Ponte de Desktop foi introduzida na versão 1607 do Windows 10 e pode ser usada somente em projetos para a Atualização de Aniversário do Windows 10 (10.0; compilação 14393) ou uma versão posterior no Visual Studio.

## <a name="first-consider-how-youll-distribute-your-app"></a>Primeiro, considere como você vai distribuir o aplicativo

Se você planeja publicar seu aplicativo na [Microsoft Store](https://www.microsoft.com/store/apps), comece preenchendo [este formulário](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). A Microsoft entrará em contato com você para iniciar o processo de instalação. Como parte deste processo, você irá reservar um nome na loja e obter informações que você precisará para empacotar seu aplicativo.

Além disso, certifique-se de revisar este guia antes de começar a criar um pacote para seu aplicativo: [Preparar para empacotar um aplicativo (Ponte de Desktop)](desktop-to-uwp-prepare.md).

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

6. Crie o projeto de empacotamento para garantir que nenhum erro apareça.

7. Use o assistente [Criar pacotes de aplicativo](../packaging/packaging-uwp-apps.md) para gerar um arquivo appxupload.

   Você pode carregar esse arquivo diretamente na Store.

**Vídeo**

<iframe src="https://www.youtube.com/embed/fJkbYPyd08w" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).

**Executar, depurar ou testar seu aplicativo**

Consulte [Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-debug.md)

**Aprimorar seu aplicativo da área de trabalho ao adicionar APIs UWP**

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](desktop-to-uwp-enhance.md)

**Estender seu aplicativo da área de trabalho adicionando projetos UWP e componentes de tempo de execução do Windows**

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](desktop-to-uwp-extend.md).

**Distribua seu aplicativo**

Consulte [Distribuir um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-distribute.md)
