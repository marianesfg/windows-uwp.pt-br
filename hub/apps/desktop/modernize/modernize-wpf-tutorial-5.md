---
description: Este tutorial demonstra como adicionar interfaces do usuário XAML UWP, criar pacotes MSIX e incorporar outros componentes modernos em seu aplicativo do WPF.
title: Empacotar e implantar com MSIX
ms.topic: article
ms.date: 06/27/2019
ms.author: mcleans
author: mcleanbyron
keywords: Windows 10, uwp, do windows forms, wpf, Ilhas de xaml
ms.localizationpriority: medium
ms.custom: RS5, 19H1
ms.openlocfilehash: d11ef296b690297d33ebd5d366c2594f70b6d10b
ms.sourcegitcommit: 1eec0e4fd8a5ba82803fdce6e23fcd01b9488523
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67420095"
---
# <a name="part-5-package-and-deploy-with-msix"></a>Parte 5: Empacotar e implantar com MSIX

Essa é a parte final de um tutorial que demonstra como modernizar um aplicativo da área de trabalho do WPF de exemplo chamado Contoso despesas. Para obter uma visão geral do tutorial, os pré-requisitos e instruções para baixar o aplicativo de exemplo, consulte [Tutorial: Modernize um aplicativo WPF](modernize-wpf-tutorial.md). Este artigo pressupõe que você já concluiu [parte 4](modernize-wpf-tutorial-4.md).

Na [parte 4](modernize-wpf-tutorial-4.md) você aprendeu que algumas APIs do WinRT, incluindo as notificações de API, exigem a identidade do pacote antes que possam ser usados em um aplicativo. Você pode obter a identidade do pacote ao empacotar as despesas de Contoso usando [MSIX](https://docs.microsoft.com/windows/msix), o formato de empacotamento introduzido no Windows 10 para empacotar e implantar aplicativos do Windows. MSIX fornece vantagens para a tabela tanto para desenvolvedores e profissionais de TI, incluindo:

- Espaço de armazenamento e de utilização de rede otimizado.
- Concluir a limpeza desinstalar, graças a um contêiner leve, onde o aplicativo é executado. Não há chaves do registro e arquivos temporários são deixados no sistema.
- Separa as atualizações do sistema operacional de personalizações e atualizações de aplicativos.
- Simplifica a instalação, atualização e o processo de desinstalação. 

Nesta parte do tutorial, você aprenderá como empacotar o aplicativo de despesas de Contoso em um pacote MSIX.

## <a name="package-the-application"></a>Empacotar o aplicativo

2019 do Visual Studio fornece uma maneira fácil de empacotar um aplicativo da área de trabalho usando o projeto de empacotamento de aplicativo do Windows. 

1. Na **Gerenciador de soluções**, clique com botão direito do **ContosoExpenses** solução e escolha **Adicionar -> Novo projeto**.

    ![Adicionar novo projeto](images/wpf-modernize-tutorial/AddNewProject.png)

3. No **adicionar um novo projeto** caixa de diálogo, pesquise por `packaging`, escolha o **Windows Application Packaging Project** modelo de projeto na C# categoria e clique em **próximo** .

    ![Projeto de empacotamento de aplicativo do Windows](images/wpf-modernize-tutorial/WAP.png)

4. Nomeie o novo projeto `ContosoExpenses.Package` e clique em **criar**.

5. Selecione **Windows 10, versão 1903 (10.0; Build 18362)** para ambos os **versão de destino** e **versão mínima** e clique em **Okey**.

    O **ContosoExpenses.Package** projeto é adicionado para o **ContosoExpenses** solução. Esse projeto inclui um [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root), que descreve o aplicativo e alguns ativos padrão que são usados para itens como o ícone no menu Programas e o bloco na tela inicial. No entanto, ao contrário de um projeto UWP, o projeto de empacotamento não contém código. Sua finalidade é empacotar um aplicativo de área de trabalho existente.

6. No **ContosoExpenses.Package** do projeto, clique com botão direito do **aplicativos** nó e escolha **adicionar referência**. Esse nó Especifica quais aplicativos em sua solução serão incluídos no pacote.

7. Na lista de projetos, selecione **ContosoExpenses.Core** e clique em **Okey**.

8. Expanda o **aplicativos** nó e confirme se o a **ContosoExpense.Core** projeto é referenciado e realçado em negrito. Isso significa que ele será usado como um ponto de partida para o pacote.

9. Clique com botão direito do **ContosoExpenses.Package** do projeto e escolha **definir como projeto de inicialização**.

10. Pressione **F5** para iniciar o aplicativo empacotado no depurador.

Neste ponto, você pode observar algumas alterações que indicam que o aplicativo está agora em execução como empacotado:

- O ícone na barra de tarefas ou no menu Iniciar agora está ativo padrão que está incluído em cada **Windows Application Packaging Project**.
- Se o botão direito do mouse a **ContosoExpense.Package** aplicativo listado no menu Iniciar, você verá opções que normalmente são reservadas para aplicativos baixados da Microsoft Store, tais como **configurações do aplicativo**, **Taxa e revisão** e **compartilhamento**.

    ![ContosoExpenses no Menu Iniciar](images/wpf-modernize-tutorial/StartMenu.png)

- Se você quiser desinstalar o aplicativo, você pode clique com botão direito **ContosoExpense.Package** no menu Iniciar e escolha **desinstalar**. O aplicativo será removido imediatamente, sem deixar nenhum muito antigos no sistema.

## <a name="test-the-notification"></a>A notificação de teste

Agora que você empacotou o aplicativo de despesas da Contoso com MSIX, você pode testar o cenário de notificação que não estava funcionando no final da [parte 4](modernize-wpf-tutorial-4.md).

1. No aplicativo de despesas de Contoso, escolha um funcionário da lista e, em seguida, clique no **adicionar novo despesa** botão. 
2. Preencha todos os campos no formulário e pressione **salvar**.
3. Confirme que você vê uma notificação do sistema operacional.

![Notificação do sistema](images/wpf-modernize-tutorial/ToastNotification.png)
