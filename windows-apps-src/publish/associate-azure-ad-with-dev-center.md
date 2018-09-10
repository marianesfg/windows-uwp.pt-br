---
author: jnHs
Description: In order to add and manage account users, you must first associate your Dev Center account with your organization's Azure Active Directory.
title: Associar sua conta do Centro de Desenvolvimento ao Azure Active Directory
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, azure ad, locatário do azure, locatário do aad, locatário do azure ad, gerenciamento de locatário, locatários
ms.localizationpriority: medium
ms.openlocfilehash: dd729d76705849c981516109da39bbd27c140286
ms.sourcegitcommit: f5cf806a595969ecbb018c3f7eea86c7a34940f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/10/2018
ms.locfileid: "3821960"
---
# <a name="associate-azure-active-directory-with-your-dev-center-account"></a>Associar sua conta do Centro de Desenvolvimento ao Azure Active Directory

Para [adicionar e gerenciar usuários da conta](add-users-groups-and-azure-ad-applications.md), você deve primeiro associar sua conta do Centro de Desenvolvimento ao Azure Active Directory da organização. 

O Centro de Desenvolvimento do Windows aproveita o Azure AD para acesso e gerenciamento de contas de vários usuários. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo inquilino do Azure AD no Centro de Desenvolvimento sem nenhum custo adicional.

> [!TIP]
> Este tópico é específico para o programa de desenvolvedores de aplicativos do Windows, mas a associação de um locatário e o gerenciamento de usuários funcionam da mesma forma para contas no Programa de Aplicativos da Área de Trabalho do Windows (consulte [Programa de Aplicativos da Área de Trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) para obter mais informações) e no Programa de Desenvolvedores de Hardware para Windows (em que a função **Gerente** também se aplicaria a contas de hardware com a função **Administrador**; consulte [Administração do Painel](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) para obter mais informações).

Um único locatário do Azure AD pode ser associado a várias contas do Centro de Desenvolvimento. Você só precisa ter um locatário do Azure AD associado à sua conta do Centro de Desenvolvimento para adicionar vários usuários da conta. No entanto, você também tem a opção de adicionar vários locatários do Azure AD a uma única conta do Centro de Desenvolvimento. Qualquer usuário com a função **Gerente** na conta do Centro de Desenvolvimento terá a opção de adicionar e remover locatários do Azure AD.

> [!IMPORTANT]
> Após a associação da sua conta do Centro de Desenvolvimento com seu locatário do Azure AD, para adicionar e gerenciar usuários de contas nesse locatário, você precisará entrar no Centro de Desenvolvimento como um usuário no mesmo locatário que tem a função **Gerente**.


## <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associar sua conta do Centro de Desenvolvimento ao inquilino do Azure AD existente da organização

Se sua organização já usa o Azure AD, siga estas etapas para vincular sua conta do Centro de Desenvolvimento.

1.  No [painel Centro de desenvolvimento do Windows](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem (perto do canto superior direito do painel) e, em seguida, selecione **as configurações da conta**. No menu **configurações** , selecione **locatários**.
2.  Selecione **Associar sua conta do Centro de Desenvolvimento ao Azure AD**.
3.  Insira suas credenciais do Azure AD para o locatário que deseja associar.
4.  Revise os nomes da organização e do domínio da conta do inquilino do Azure AD. Para concluir a associação, selecione **Confirmar**.
5.  Se a associação for bem-sucedida, você estará pronto para adicionar e gerenciar usuários da conta na seção **Usuários** do Centro de Desenvolvimento.

> [!IMPORTANT]
> Para criar novos usuários ou realizar outras alterações no Azure AD, você precisará entrar no locatário do Azure AD usando uma conta que tenha [permissão de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para esse locatário. No entanto, você não precisará de permissão de administrador global para associar o locatário ou adicionar usuários que já existem nesse locatário à sua conta do Centro de Desenvolvimento.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Criar um novo Azure AD para associar à sua conta do Centro de Desenvolvimento

Se você precisa configurar um novo Azure AD para vincular com sua conta do Centro de Desenvolvimento, siga estas etapas.

1.  No [painel Centro de desenvolvimento do Windows](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem (perto do canto superior direito do painel) e, em seguida, selecione **as configurações da conta**. No menu **configurações** , selecione **locatários**.
2.  Selecione **Criar novo Azure AD**.
3.  Insira as informações de diretório para seu novo Azure AD:
    - **Nome de domínio**: o nome exclusivo que usaremos para seu domínio do Azure AD, junto com ". onmicrosoft.com". Por exemplo, se você inseriu "exemplo", seu domínio do Azure AD seria "example.onmicrosoft.com".
    - **Email de contato**: um endereço de email onde possamos contatá-lo sobre a sua conta, se necessário.
    - **Informações de conta de usuário do administrador global**: o nome, sobrenome, nome de usuário e senha que você deseja usar para a nova conta de administrador global.
4.  Clique em **Criar** para confirmar as novas informações de domínio e conta.
5.  Entre com o nome de usuário e senha do novo Azure AD para começar a [adicionar e gerenciar usuários de contas adicionais](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gerenciar associações de locatário do Azure AD

Após ter associado um locatário do Azure AD com sua conta do Centro de Desenvolvimento, você pode adicionar novos locatários ou remover os locatários existentes da página **Locatários**.


### <a name="add-multiple-azure-ad-tenants-to-your-dev-center-account"></a>Adicionar vários locatários do Azure AD à sua conta do Centro de Desenvolvimento

Qualquer usuário que tenha a função **Gerente** de uma conta do Centro de Desenvolvimento pode associar locatários do Azure AD com a conta.

Para associar um novo locatário, selecione **Associar outro locatário do Azure AD** e depois siga as etapas indicadas acima. Vale observar que você precisará informar suas credenciais no locatário do Azure AD que deseja associar.


### <a name="remove-an-azure-ad-tenant-from-your-dev-center-account"></a>Remover um locatário do Azure AD da conta do Centro de Desenvolvimento

Qualquer usuário que tenha a função **Gerente** de uma conta do Centro de Desenvolvimento pode remover locatários do Azure AD da conta.

> [!IMPORTANT]
> Ao remover um locatário, todos os usuários adicionados à conta do Centro de Desenvolvimento desse locatário não serão mais capazes de entrar na conta. 

Para remover um locatário, encontre o nome na página **locatários** (nas **configurações da conta**) e selecione **Remover**. Você precisará confirmar que deseja remover o locatário. Após realizar essa operação, nenhum usuário do Centro de Desenvolvimento será capaz de acessar a conta do Centro de Desenvolvimento e todas as permissões que você configurou para esse usuário serão removidas.

> [!TIP]
> Você não pode remover um locatário se estiver atualmente conectado ao Centro de Desenvolvimento usando uma conta no mesmo locatário. Para remover um locatário, você deve acessar o Centro de Desenvolvimento como **Gerente** para outro locatário que esteja associado à conta. Se houver apenas um locatário associado com a conta, esse locatário só poderá ser removido após o acesso à conta da Microsoft que abriu a conta.


