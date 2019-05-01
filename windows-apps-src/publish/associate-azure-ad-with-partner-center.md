---
Description: Para adicionar e gerenciar usuários de conta, você deve primeiro associar sua conta do Centro de parceiro do Active Directory do Azure da sua organização.
title: Associar o Active Directory do Azure com sua conta no Partner Center
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, azure ad, locatário do azure, locatário do aad, locatário do azure ad, gerenciamento de locatário, locatários
ms.localizationpriority: medium
ms.openlocfilehash: 5d830fb3a984faecd93519b4e682ade59077d2d6
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787224"
---
# <a name="associate-azure-active-directory-with-your-partner-center-account"></a>Associar o Active Directory do Azure com sua conta no Partner Center

Para [adicionar e gerenciar usuários de conta](add-users-groups-and-azure-ad-applications.md), você deve primeiro associar sua conta do Centro de parceiro do Active Directory do Azure da sua organização. 

[Partner Center](https://partner.microsoft.com/dashboard) aproveita o Azure AD para acesso à conta de multiusuário e gerenciamento. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo locatário do AD do Azure de dentro do Partner Center sem nenhum custo adicional.

> [!TIP]
> Este tópico é específico para o programa de desenvolvedores de aplicativos do Windows no [Partner Center](https://partner.microsoft.com/dashboard), mas associando um locatário e gerenciando usuários funcionam da mesma forma para contas no programa de aplicativo da área de trabalho do Windows (consulte [Windows Programa de aplicativo de área de trabalho](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users) para obter mais informações) e no programa de desenvolvimento de Hardware do Windows (em que faz referência à **Manager** função também se aplica a contas de Hardware com a **administrador**  função; consulte [painel administração](https://docs.microsoft.com/windows-hardware/drivers/dashboard/dashboard-administration) para obter mais informações).

Um único locatário do Azure AD pode ser associado a várias contas do Partner Center. Você só precisa ter um locatário do Azure AD associado a sua conta no Partner Center para adicionar vários usuários de conta, mas você também tem a opção de adicionar vários locatários do AD do Azure para uma única conta do Partner Center. Qualquer usuário com o **Manager** função na conta do Partner Center terá a opção de adicionar e remover locatários do Azure AD da conta.

> [!IMPORTANT]
> Depois de associar sua conta no Partner Center com seu locatário do AD do Azure, para adicionar e gerenciar usuários de conta em que o locatário, você precisará entrar no Centro de parceiro como um usuário no mesmo locatário que tem o **Manager** função.


## <a name="associate-your-partner-center-account-with-your-organizations-existing-azure-ad-tenant"></a>Associar sua conta no Partner Center ao locatário do Azure AD existente de sua organização

Se sua organização já usa o Azure AD, siga estas etapas para vincular sua conta no Partner Center.

1.  Partir [Partner Center](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem (perto do canto superior direito do painel de controle) e, em seguida, selecione **configurações do desenvolvedor**. No **as configurações** menu, selecione **locatários**.
2.  Selecione **associe AD do Azure com sua conta no Partner Center**.
3.  Insira suas credenciais do Azure AD para o locatário que deseja associar.
4.  Revise os nomes da organização e do domínio da conta do inquilino do Azure AD. Para concluir a associação, selecione **Confirmar**.
5.  Se a associação for bem-sucedida, em seguida, estará pronto para adicionar e gerenciar usuários de conta de **usuários** seção no Partner Center.

> [!IMPORTANT]
> Para criar novos usuários ou realizar outras alterações no Azure AD, você precisará entrar no locatário do Azure AD usando uma conta que tenha [permissão de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para esse locatário. No entanto, você não precisa permissão de administrador global para associar o locatário ou para adicionar os usuários que já existem nesse locatário para sua conta no Partner Center.


## <a name="create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account"></a>Criar um novo do Azure AD para associar sua conta no Partner Center

Se você precisar configurar um novo AD do Azure para vincular com sua conta no Partner Center, siga estas etapas.

1.  Partir [Partner Center](https://partner.microsoft.com/dashboard), selecione o ícone de engrenagem (perto do canto superior direito do painel de controle) e, em seguida, selecione **configurações do desenvolvedor**. No **as configurações** menu, selecione **locatários**.
2.  Selecione **Criar novo Azure AD**.
3.  Insira as informações de diretório para seu novo Azure AD:
    - **Nome de domínio**: O nome exclusivo que usaremos para seu domínio do AD do Azure, junto com ". onmicrosoft.com". Por exemplo, se você inseriu "exemplo", seu domínio do Azure AD seria "example.onmicrosoft.com".
    - **Entre em contato com o email**: Um endereço de email onde podemos contatá-lo sobre a sua conta se necessário.
    - **Informações de conta de usuário de administrador global**: O primeiro nome, último nome, nome de usuário e senha que você deseja usar para a nova conta de administrador global.
4.  Clique em **Criar** para confirmar as novas informações de domínio e conta.
5.  Entre com o nome de usuário e senha do novo Azure AD para começar a [adicionar e gerenciar usuários de contas adicionais](add-users-groups-and-azure-ad-applications.md).


## <a name="manage-azure-ad-tenant-associations"></a>Gerenciar associações de locatário do Azure AD

Depois que você associou um locatário do AD do Azure com sua conta no Partner Center, você pode adicionar novos locatários ou remover locatários existentes do **locatários** página.


### <a name="add-multiple-azure-ad-tenants-to-your-partner-center-account"></a>Adicionar vários locatários do AD do Azure à sua conta no Partner Center

Qualquer usuário que tenha o **Manager** função para uma conta no Partner Center pode associar locatários do Azure AD com a conta.

Para associar um novo locatário, selecione **Associar outro locatário do Azure AD** e depois siga as etapas indicadas acima. Vale observar que você precisará informar suas credenciais no locatário do Azure AD que deseja associar.


### <a name="remove-an-azure-ad-tenant-from-your-partner-center-account"></a>Remover um locatário do AD do Azure de sua conta no Partner Center

Qualquer usuário que tenha o **Manager** função para uma conta no Partner Center pode remover locatários do Azure AD da conta.

> [!IMPORTANT]
> Quando você remove um locatário, todos os usuários que foram adicionados à conta do Partner Center desse locatário não poderá entrar na conta. 

Para remover um locatário, localizar seu nome na **locatários** página (na **configurações de conta**), em seguida, selecione **remover**. Você precisará confirmar que deseja remover o locatário. Quando você fizer isso, não há usuários nesse locatário será capazes de entrar na conta do Partner Center e quaisquer permissões que você configurou para os usuários serão removidos.

> [!TIP]
> Você não pode remover um locatário se você está inscrito no Partner Center usando uma conta no mesmo locatário. Para remover um locatário, você deve entrar no Partner Center como um **Manager** para outro locatário que está associado com a conta. Se houver apenas um locatário associado com a conta, esse locatário só poderá ser removido após o acesso à conta da Microsoft que abriu a conta.


