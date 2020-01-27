---
description: Este tutorial demonstra como adicionar interfaces de usuário do UWP XAML, criar pacotes do MSIX e incorporar outros componentes modernos ao seu aplicativo do WPF.
title: Empacotar e implantar com o MSIX
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, UWP, Windows Forms, WPF, Ilhas XAML
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 27906d9d389c065ab1fdf7124151cd1915f850eb
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726009"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Parte 5: empacotar e implantar com MSIX

Esta é a parte final de um tutorial que demonstra como modernizar um aplicativo de área de trabalho WPF de exemplo chamado contoso despesas. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo, consulte [tutorial: modernizar um aplicativo do WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu a [parte 4](modernize-wpf-tutorial-4.md).

Na [parte 4](modernize-wpf-tutorial-4.md) , você aprendeu que algumas APIs do WinRT, incluindo a API de notificações, exigem a identidade do pacote antes que elas possam ser usadas em um aplicativo. Você pode obter a identidade do pacote empacotando as despesas da contoso usando o [MSIX](https://docs.microsoft.com/windows/msix), o formato de empacotamento introduzido no Windows 10 para empacotar e implantar aplicativos do Windows. O MSIX fornece vantagens para os desenvolvedores e profissionais de ti, incluindo:

- Uso otimizado de rede e espaço de armazenamento.
- Conclua a desinstalação limpa, graças a um contêiner leve em que o aplicativo é executado. Nenhuma chave do registro e arquivos temporários são deixados no sistema.
- Dissocia as atualizações do sistema operacional de atualizações e personalizações do aplicativo.
- Simplifica o processo de instalação, atualização e desinstalação.

Nesta parte do tutorial, você aprenderá a empacotar o aplicativo de despesas da Contoso em um pacote MSIX.

## <a name="package-the-application"></a>Empacotar o aplicativo

O Visual Studio 2019 fornece uma maneira fácil de empacotar um aplicativo de área de trabalho usando o projeto de empacotamento de aplicativos do Windows. 

1. Em **Gerenciador de soluções**, clique com o botão direito do mouse na solução **ContosoExpenses** e escolha **Adicionar-> novo projeto**.

    ![Adicionar novo projeto](images/wpf-modernize-tutorial/AddNewProject.png)

3. Na caixa de diálogo **Adicionar um novo projeto** , procure `packaging`, escolha o modelo projeto de **projeto de pacote de aplicativos** do C# Windows na categoria e clique em **Avançar**.

    ![Projeto de empacotamento de aplicativos do Windows](images/wpf-modernize-tutorial/WAP.png)

4. Nomeie o novo projeto `ContosoExpenses.Package` e clique em **criar**.

5. Selecione **Windows 10, versão 1903 (10,0; Compile 18362)** para a **versão de destino** e a **versão mínima** e clique em **OK**.

    O projeto **ContosoExpenses. Package** é adicionado à solução **ContosoExpenses** . Este projeto inclui um [manifesto de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), que descreve o aplicativo e alguns ativos padrão que são usados para itens como o ícone no menu programas e o bloco na tela inicial. No entanto, ao contrário de um projeto UWP, o projeto de empacotamento não contém código. Seu objetivo é empacotar um aplicativo de área de trabalho existente.

6. No projeto **ContosoExpenses. Package** , clique com o botão direito do mouse no nó **aplicativos** e escolha **Adicionar referência**. Esse nó especifica quais aplicativos em sua solução serão incluídos no pacote.

6. Na lista de projetos, selecione **ContosoExpenses. Core** e clique em **OK**.

7. Expanda o nó **aplicativos** e confirme se o projeto **ContosoExpense. Core** é referenciado e realçado em negrito. Isso significa que ele será usado como um ponto de partida para o pacote.

8. Clique com o botão direito do mouse no projeto **ContosoExpenses. Package** e escolha **definir como projeto de inicialização**.

9. Pressione **F5** para iniciar o aplicativo empacotado no depurador.

Neste ponto, você pode observar algumas alterações que indicam que o aplicativo agora está sendo executado como empacotado:

- O ícone na barra de tarefas ou no menu Iniciar agora é o ativo padrão que é incluído em cada **projeto de empacotamento de aplicativos do Windows**.
- Se você clicar com o botão direito do mouse no aplicativo **ContosoExpense. Package** listado no menu Iniciar, observará opções que normalmente são reservadas para aplicativos baixados do Microsoft Store, como **configurações do aplicativo**, **taxa e revisão** e **compartilhamento**.

    ![ContosoExpenses no menu iniciar](images/wpf-modernize-tutorial/StartMenu.png)

- Se você quiser desinstalar o aplicativo, clique com o botão direito do mouse em **ContosoExpense. Package** no menu iniciar e escolha **desinstalar**. O aplicativo será removido imediatamente, sem sair do restante do sistema.

## <a name="test-the-notification"></a>Testar a notificação

Agora que você pacoteu o aplicativo de despesas da contoso com o MSIX, você pode testar o cenário de notificação que não estava funcionando no final da [parte 4](modernize-wpf-tutorial-4.md).

1. No aplicativo de despesas da Contoso, escolha um funcionário na lista e clique no botão **Adicionar nova despesa** .
2. Preencha todos os campos no formulário e pressione **salvar**.
3. Confirme que você vê uma notificação do sistema operacional.

![Notificação do sistema](images/wpf-modernize-tutorial/ToastNotification.png)
