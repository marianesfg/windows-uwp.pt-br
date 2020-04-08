---
description: Esse tutorial demonstra como adicionar interfaces de usuário XAML da UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Empacotar e implantar com o MSIX
ms.topic: article
ms.date: 01/23/2020
ms.author: mcleans
author: mcleanbyron
keywords: windows 10, uwp, windows forms, wpf, ilhas xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: 27906d9d389c065ab1fdf7124151cd1915f850eb
ms.sourcegitcommit: 8a88a05ad89aa180d41a93152632413694f14ef8
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/24/2020
ms.locfileid: "76726009"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Parte 5: Empacotar e implantar com o MSIX

Esta é a parte final de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso Expenses. Para obter uma visão geral do tutorial, dos pré-requisitos e das instruções para baixar o aplicativo de exemplo, confira [Tutorial: modernizar um aplicativo do WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu a [parte 4](modernize-wpf-tutorial-4.md).

Na [parte 4](modernize-wpf-tutorial-4.md), você aprendeu que algumas APIs do WinRT, incluindo a API de notificações, exigem a identidade do pacote antes que elas possam ser usadas em um aplicativo. Você pode obter o identificador de pacote ao empacotar o Contoso Expenses usando [MSIX](https://docs.microsoft.com/windows/msix), o formato de empacotamento introduzido no Windows 10 para empacotar e implantar aplicativos do Windows. O MSIX fornece vantagens para os desenvolvedores e os profissionais de TI, incluindo:

- Uso otimizado de rede e espaço de armazenamento.
- Desinstalação limpa completa, graças a um contêiner leve em que o aplicativo é executado. Nenhuma chave do Registro e nenhum arquivo temporário é deixado no sistema.
- Dissociação das atualizações do sistema operacional de atualizações e personalizações do aplicativo.
- Simplificação do processo de instalação, atualização e desinstalação.

Nesta parte do tutorial, você aprenderá a empacotar o aplicativo Contoso Expenses em um pacote MSIX.

## <a name="package-the-application"></a>Empacotar o aplicativo

O Visual Studio 2019 fornece um modo fácil de empacotar um aplicativo da área de trabalho usando o Projeto de Empacotamento de Aplicativo do Windows. 

1. No **Gerenciador de Soluções**, clique com o botão direito do mouse na solução **ContosoExpenses** e escolha **Adicionar -> Novo projeto**.

    ![Adicionar novo projeto](images/wpf-modernize-tutorial/AddNewProject.png)

3. Na caixa de diálogo **Adicionar um novo projeto**, procure `packaging`, escolha o modelo de projeto **Projeto de Empacotamento de Aplicativo do Windows** na categoria C# e clique em **Avançar**.

    ![Projeto de Empacotamento de Aplicativo do Windows](images/wpf-modernize-tutorial/WAP.png)

4. Nomeie o novo projeto como `ContosoExpenses.Package` e clique em **Criar**.

5. Selecione **Windows 10, versão 1903 (10.0; Build 18362)** para a **Versão de destino** e a **Versão mínima** e clique em **OK**.

    O projeto **ContosoExpenses.Package** é adicionado à solução **ContosoExpenses**. Este projeto inclui um [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), que descreve o aplicativo e alguns ativos padrão que são usados para itens como o ícone no menu Programas e o bloco na tela inicial. No entanto, ao contrário de um projeto da UWP, o projeto de empacotamento não contém código. Seu objetivo é empacotar um aplicativo da área de trabalho existente.

6. No projeto **ContosoExpenses.Package**, clique com o botão direito do mouse no nó **Aplicativos** e escolha **Adicionar referência**. Esse nó especifica quais aplicativos em sua solução serão incluídos no pacote.

6. Na lista de projetos, selecione **ContosoExpenses.Core** e clique em **OK**.

7. Expanda o nó **Aplicativos** e confirme se o projeto **ContosoExpense.Core** é referenciado e realçado em negrito. Isso significa que ele será usado como um ponto de partida para o pacote.

8. Clique com o botão direito do mouse no projeto **ContosoExpenses.Package** e escolha **Definir como Projeto de Inicialização**.

9. Pressione **F5** para iniciar o aplicativo empacotado no depurador.

Nesse ponto, você pode observar algumas alterações que indicam que o aplicativo agora está sendo executado como empacotado:

- O ícone na barra de tarefas ou no menu Iniciar agora é o ativo padrão que é incluído em cada **Projeto de Empacotamento de Aplicativo do Windows**.
- Se você clicar com o botão direito do mouse no aplicativo **ContosoExpense.Package** listado no menu Iniciar, você observará opções que normalmente são reservadas para aplicativos baixados da Microsoft Store, como **Configurações do aplicativo**, **Taxa e revisão** e **Compartilhamento**.

    ![ContosoExpenses no menu Iniciar](images/wpf-modernize-tutorial/StartMenu.png)

- Se você quiser desinstalar o aplicativo, clique com o botão direito do mouse em **ContosoExpense.Package** no menu Iniciar e escolha **Desinstalar**. O aplicativo será removido imediatamente, sem sair do restante do sistema.

## <a name="test-the-notification"></a>Testar a notificação

Agora que você empacotou o aplicativo Contoso Expenses com o MSIX, você pode testar o cenário de notificação que não estava funcionando no final da [parte 4](modernize-wpf-tutorial-4.md).

1. No aplicativo Contoso Expenses, escolha um funcionário na lista e clique no botão **Adicionar nova despesa**.
2. Preencha todos os campos no formulário e pressione **Salvar**.
3. Confirme se você vê uma notificação do sistema operacional.

![Notificação do sistema](images/wpf-modernize-tutorial/ToastNotification.png)
