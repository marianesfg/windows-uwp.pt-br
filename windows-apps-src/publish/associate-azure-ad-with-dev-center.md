---
Description: In order to add and manage account users, you must first associate your Partner Center account with your organization's Azure Active Directory.
title: Associar o Azure Active Directory à sua conta do Partner Center
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, locatário do azure, locatário do aad, locatário do azure ad, gerenciamento de locatário, locatários
ms.localizationpriority: medium
ms.openlocfilehash: 9f807799740d7e832da2f6a6fa3ea63e00deaee4
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7993552"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associar o Azure Active Directory à sua conta do Partner Center

Na ordem para [Adicionar e gerenciar usuários da conta](add-users-groups-and-azure-ad-applications.md), você deve primeiro associar sua conta do Partner Center Azure Active Directory da sua organização. 

[Partner Center](https://partner.microsoft.com/dashboard) aproveita o Azure AD para acesso de contas de vários usuários e gerenciamento. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo inquilino do Azure AD dentro do Partner Center sem nenhum custo adicional.

> [!TIP]
> Este tópico é específico para o programa de desenvolvedor de aplicativos do Windows no [Partner Center](https://partner.microsoft.com/dashboard), mas a associação de um locatário e gerenciamento de usuários funcionam da mesma forma para contas no programa de aplicativo de área de trabalho do Windows (consulte o [Programa de aplicativo de área de trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) obter mais informações) e no programa de desenvolvedor de Hardware do Windows (onde referências para a função **gerente** também se aplicariam a contas de Hardware com a função de **administrador** ; consulte [Administração do painel](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) para obter mais informações).

Um único locatário do Azure AD pode ser associado a várias contas do Partner Center. Você só precisa ter um locatário do Azure AD associado à sua conta do Partner Center para adicionar vários usuários da conta, mas você também tem a opção de adicionar vários locatários do Azure AD para uma única conta do Partner Center. Qualquer usuário com a função **gerente** de conta do Partner Center terá a opção de adicionar e remover locatários do Azure AD da conta.

> [!IMPORTANT]
> Depois de associar sua conta do Partner Center com seu locatário Azure AD, para adicionar e gerenciar usuários de contas nesse locatário, você precisará entrar no Partner Center como um usuário no mesmo locatário que tem a função **gerente** .


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associar sua conta do Partner Center ao locatário do Azure AD existente da sua organização

Se sua organização já usa o Azure AD, siga estas etapas para vincular sua conta do Partner Center.

1.  No [Partner Center](https://partner.microsoft.com/dashboard) , selecione o ícone de engrenagem (perto do canto superior direito do painel) e, em seguida, selecione **as configurações do desenvolvedor**. No menu **configurações** , selecione **locatários**.
2.  Selecione **associar o Azure AD com sua conta do Partner Center**.
3.  Insira suas credenciais do Azure AD para o locatário que deseja associar.
4.  Revise os nomes da organização e do domínio da conta do inquilino do Azure AD. Para concluir a associação, selecione **Confirmar**.
5.  Se a associação for bem-sucedida, você estará pronto para adicionar e gerenciar usuários da conta na seção de **usuários** no Partner Center.

> [!IMPORTANT]
> Para criar novos usuários ou realizar outras alterações no Azure AD, você precisará entrar no locatário do Azure AD usando uma conta que tenha [permissão de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para esse locatário. No entanto, você não precisa permissão de administrador global para associar o locatário ou adicionar usuários que já existem nesse locatário à sua conta do Partner Center.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Criar um novo Azure AD para associar à sua conta do Partner Center

Se você precisar configurar um novo Azure AD para vincular com sua conta do Partner Center, siga estas etapas.

1.  No [Partner Center](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem (perto do canto superior direito do painel) e, em seguida, selecione **as configurações do desenvolvedor**. No menu **configurações** , selecione **locatários**.
2.  Selecione **Criar novo Azure AD**.
3.  Insira as informações de diretório para seu novo Azure AD:
    - **Nome de domínio**: o nome exclusivo que usaremos para seu domínio do Azure AD, junto com ". onmicrosoft.com". Por exemplo, se você inseriu "exemplo", seu domínio do Azure AD seria "example.onmicrosoft.com".
    - **Email de contato**: um endereço de email onde possamos contatá-lo sobre a sua conta, se necessário.
    - **Informações de conta de usuário do administrador global**: o nome, sobrenome, nome de usuário e senha que você deseja usar para a nova conta de administrador global.
4.  Clique em **Criar** para confirmar as novas informações de domínio e conta.
5.  Entre com o nome de usuário e senha do novo Azure AD para começar a [adicionar e gerenciar usuários de contas adicionais](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gerenciar associações de locatário do Azure AD

Após ter associado um locatário do Azure AD com sua conta do Partner Center, você pode adicionar novos locatários ou remover locatários existentes da página **locatários** .


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Adicionar vários locatários do Azure AD à sua conta do Partner Center

Qualquer usuário que tenha a função **gerente** para uma conta do Partner Center pode associar locatários do Azure AD com a conta.

Para associar um novo locatário, selecione **Associar outro locatário do Azure AD** e depois siga as etapas indicadas acima. Vale observar que você precisará informar suas credenciais no locatário do Azure AD que deseja associar.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Remover um locatário do Azure AD da sua conta do Partner Center

Qualquer usuário que tenha a função **gerente** para uma conta do Partner Center pode remover locatários do Azure AD da conta.

> [!IMPORTANT]
> Quando você remove um locatário, todos os usuários que foram adicionados à conta do Partner Center do locatário não será capazes de entrar conta. 

Para remover um locatário, encontre o nome na página **locatários** (nas **configurações da conta**) e selecione **Remover**. Você precisará confirmar que deseja remover o locatário. Depois que você fizer isso, nenhum usuário no locatário não poderão entrar na conta do Partner Center e qualquer permissões que você configurou para os usuários serão removidas.

> [!TIP]
> Você não pode remover um locatário se estiver atualmente conectado ao Partner Center usando uma conta no mesmo locatário. Para remover um locatário, você deve entrar Partner Center como um **gerente** para outro locatário que esteja associado à conta. Se houver apenas um locatário associado com a conta, esse locatário só poderá ser removido após o acesso à conta da Microsoft que abriu a conta.


