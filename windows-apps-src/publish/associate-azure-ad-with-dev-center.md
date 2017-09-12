---
author: jnHs
Description: "Para adicionar e gerenciar usuários da conta, você deve primeiro associar sua conta do Centro de Desenvolvimento ao Azure Active Directory de sua organização."
title: Associar sua conta do Centro de Desenvolvimento ao Azure Active Directory
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: ace9470cc707206461baa8c3dd72828ea68a8eb4
ms.sourcegitcommit: eaacc472317eef343b764d17e57ef24389dd1cc3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/17/2017
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>Associar sua conta do Centro de Desenvolvimento ao Azure Active Directory

Para [adicionar e gerenciar usuários da conta](add-users-groups-and-azure-ad-applications.md), você deve primeiro associar sua conta do Centro de Desenvolvimento ao Azure Active Directory da organização. 

O Centro de Desenvolvimento do Windows aproveita o Azure AD para o gerenciamento de vários usuários e a atribuição de funções. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo inquilino do Azure AD no Centro de Desenvolvimento sem nenhum custo adicional.

> [!IMPORTANT]
> Para associar sua conta do Centro de Desenvolvimento ao Azure AD, você deve fazer se conectar ao locatário do Azure AD com uma conta de [administrador global](http://go.microsoft.com/fwlink/?LinkId=746654).
> 
> Depois de associar sua conta do Centro de Desenvolvimento ao Azure AD, você sempre precisará entrar no Centro de Desenvolvimento usando a conta de administrador global do Azure AD (e não uma conta pessoal da Microsoft) para adicionar e gerenciar usuários de contas.

Observe que apenas uma conta do Centro de Desenvolvimento pode ser associada a um inquilino do Azure AD. Da mesma forma, apenas um inquilino do Azure AD pode ser associado uma conta do Centro de Desenvolvimento. Depois de estabelecer a associação, você não poderá removê-la sem contatar o suporte.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associar sua conta do Centro de Desenvolvimento ao inquilino do Azure AD existente da organização

Se sua organização já usa o Azure AD, siga estas etapas para vincular sua conta do Centro de Desenvolvimento.

1.  Acesse suas **Configurações da conta** e clique em **Gerenciar usuários**.
2.  Clique no botão **Associar o Azure AD com a sua conta do Centro de Desenvolvimento**.
3.  Entre em sua conta do Azure AD. Essa conta deve ter permissões de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para configurar a associação.
4.  Revise os nomes da organização e do domínio da conta do inquilino do Azure AD. Para concluir a associação, clique em **Confirmar**.
5.  Se a associação for bem-sucedida, você estará pronto para adicionar e gerenciar usuários da conta na seção **Gerenciar usuários** do Centro de Desenvolvimento.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Criar um novo Azure AD para associar à sua conta do Centro de Desenvolvimento

Se você precisa configurar um novo Azure AD para vincular com sua conta do Centro de Desenvolvimento, siga estas etapas.

1.  Acesse suas **Configurações da conta** e clique em **Gerenciar usuários**.
2.  Clique no botão **Criar novo Azure AD**.
3.  Insira as informações de diretório para seu novo Azure AD:
 - **Nome de domínio**: o nome exclusivo que usaremos para seu domínio do Azure AD, junto com ". onmicrosoft.com". Por exemplo, se você inseriu "exemplo", seu domínio do Azure AD seria "example.onmicrosoft.com".
 - **Email de contato**: um endereço de email onde possamos contatá-lo sobre a sua conta, se necessário.
 - **Informações de conta de usuário do administrador global**: o nome, sobrenome, nome de usuário e senha que você deseja usar para a nova conta de administrador global.
4.  Clique em **Criar** para confirmar as novas informações de domínio e conta.
5.  Entre com o nome de usuário e senha do novo Azure AD para começar a [adicionar e gerenciar usuários de contas adicionais](add-users-groups-and-azure-ad-applications.md).



