---
title: Introdução às ferramentas do Xbox One
description: Ferramenta Dev Home específica ao Xbox usando o Windows Device Portal.
ms.date: 10/04/2017
ms.topic: article
keywords: windows 10, uwp, xbox one, ferramentas
ms.assetid: 6eaf376f-0d7c-49de-ad78-38e689b43658
ms.localizationpriority: medium
ms.openlocfilehash: ed106095d83ed0c6e055d22a1a0cf229380cff71
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8945888"
---
# <a name="introduction-to-xbox-one-tools"></a>Introdução às ferramentas do Xbox One

Esta seção aborda como acessar o Portal de Dispositivos do Xbox por meio do aplicativo Dev Home.

## <a name="dev-home"></a>Dev Home

A Dev Home é uma experiência de ferramentas no Kit de desenvolvimento do Xbox One criada para auxiliar a produtividade do desenvolvedor. A Dev Home oferece funcionalidade para gerenciar e configurar o kit de desenvolvimento.

A Dev Home é o aplicativo padrão que é aberto quando seu console é inicializado no Modo de Desenvolvedor. Também é possível abrir a Dev Home selecionando o bloco **Dev Home** na tela Início. Se não houver nenhum bloco presente, o console não estará no Modo de Desenvolvedor.

Para obter mais informações sobre a Dev Home, consulte [Home de desenvolvedor no Console (Dev Home)](dev-home.md).

## <a name="xbox-device-portal"></a>Portal de Dispositivos do Xbox
O Portal de Dispositivos do Xbox é uma ferramenta de gerenciamento de dispositivos baseada em navegador que permite adicionar jogos, aplicativos, contas de teste do Xbox Live, alterar áreas restritas e muito mais.

Para habilitar o Portal de Dispositivos do Xbox no seu console Xbox One:

1. Selecione o bloco **Dev Home** na tela Início.

  ![Selecione o bloco Dev Home](images/introduction-to-xbox-one-tools-1.png)

2. Na Dev Home, navegue até a guia **Início** e, na seção **Acesso Remoto**, selecione **Configurações de Acesso Remoto**.

  ![Ferramenta de gerenciamento remoto](images/introduction-to-xbox-one-tools-2.png)

3. Selecione a caixa de seleção **Habilitar Portal de Dispositivos do Xbox**.

4. Em **Autenticação**, marque a caixa de seleção **Requer autenticação para acessar remotamente este console da Web ou ferramentas de computador**.

5. Insira um **Nome de usuário** e __Senha__ e selecione **Salvar**. Estas credenciais são usados para autenticar o acesso ao kit de desenvolvimento em um navegador.

6. Selecione **Fechar** e, na guia **Início**, observe o URL listado na ferramenta **Acesso Remoto**.

7. Insira o URL no navegador. Você receberá um aviso sobre o certificado que foi fornecido, semelhante à captura de tela a seguir, porque o certificado de segurança assinado por seu console Xbox One não é considerado um publicador confiável conhecido. No Edge, clique em **Detalhes** e, em seguida, em **Ir para a página da Web** para acessar o Portal de Dispositivos do Xbox.

    ![Aviso de certificado segurança](images/introduction-to-xbox-one-tools-3.png)

8. Faça logon com as credenciais que você configurou.

## <a name="xbox-dev-mode-companion"></a>Complemento do Modo Desenvolvedor do Xbox
Complemento do modo de desenvolvimento do Xbox é uma ferramenta que permite que você trabalhe no seu console sem deixar seu computador. O aplicativo permite que você exiba a tela do console e envie a entrada para ele. Para obter mais informações, consulte [Complemento do modo de desenvolvimento do Xbox](xbox-dev-mode-companion.md).

## <a name="see-also"></a>Consulte também
- [Como usar o Fiddler com o Xbox One ao desenvolver para UWP](uwp-fiddler.md)
- [Visão geral do Windows Device Portal](../debug-test-perf/device-portal.md)
- [UWP no Xbox One](index.md)