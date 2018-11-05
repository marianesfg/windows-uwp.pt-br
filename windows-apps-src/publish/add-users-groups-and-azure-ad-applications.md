---
author: jnHs
Description: You can add users, groups, and Azure AD applications to your Partner Center account.
title: Adicionar usuários, grupos e aplicativos Azure AD à sua conta do Partner Center
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, aplicativo do azure ad, aad, usuário, grupo, vários usuários, multiusuário
ms.localizationpriority: medium
ms.openlocfilehash: 2821132944a20260d0005f8925c23ab48581a9e2
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6035671"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Adicionar usuários, grupos e aplicativos Azure AD à sua conta do Partner Center

A seção de **usuários** do [Partner Center](https://partner.microsoft.com/dashboard) (em **configurações da conta**) permite que você use o Azure Active Directory para adicionar usuários à sua conta do Partner Center. Cada usuário recebe uma função (ou conjunto de permissões personalizadas) que define o acesso à conta. Você também pode adicionar [grupos de usuários](#groups) e [aplicativos Azure AD](#azure-ad-applications) para conceder acesso à sua conta do Partner Center.

Depois que os usuários são adicionados à conta, você pode [editar detalhes da conta](#edit), alterar [funções e permissões](set-custom-permissions-for-account-users.md) ou [remover usuários](#remove).

> [!IMPORTANT]
> Para adicionar usuários à sua conta, você deve primeiro [associar sua conta do Partner Center com locatário do Azure Active Directory da sua organização](associate-azure-ad-with-dev-center.md). 

Ao adicionar usuários, você precisará especificar o acesso à sua conta do Partner Center, atribuindo a eles uma [função ou um conjunto de permissões personalizados](set-custom-permissions-for-account-users.md). 

Tenha em mente que todos os usuários do Partner Center (incluindo grupos e aplicativos Azure AD) devem ter uma conta ativa em [um locatário do Azure AD está associado à sua conta do Partner Center](associate-azure-ad-with-dev-center.md). O gerenciamento de usuários é realizado em um locatário de cada vez. Você deve entrar com uma conta de gerenciador para o locatário em que você deseja adicionar ou editar usuários. Criar um novo usuário no Partner Center também criará uma conta de usuário no locatário do Azure AD ao qual você está conectado e fazer alterações em um nome de usuário no Partner Center fazer as mesmas alterações no locatário do Azure AD da sua organização.

> [!NOTE]
> Se sua organização usa a [integração de diretório](http://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar o serviço de diretório local com o Azure AD, você não será capaz de criar novos usuários, grupos ou aplicativos Azure AD no Partner Center. Você (ou outro admin em seu diretório local) precisará criá-los diretamente no diretório local antes de você poderá ver e adicioná-los no Partner Center.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Adicionar usuários à sua conta do Partner Center

Para adicionar usuários à sua conta do Partner Center, vá para a página de **usuários** em **configurações de conta** e selecione **Adicionar usuários.** Você deve estar conectado com uma conta de gerenciador do locatário do Azure AD em que você deseja trabalhar. 

### <a name="add-existing-users"></a>Adicionar usuários existentes 

Você pode selecionar os usuários que já existem no locatário da sua organização e conceda a eles acesso à sua conta do Partner Center. 

<span id="from-directory" />

1.  Selecione o ícone de engrenagem (perto do canto superior direito do Partner Center) e, em seguida, selecione **as configurações do desenvolvedor**. No menu **configurações** , selecione **os usuários**.
2.  Na página **Usuários**, selecione **Adicionar usuários**. 
3.  Selecione um ou mais usuários na lista exibida. Você pode usar a caixa de pesquisa para procurar usuários específicos.
    > [!TIP]
    > Se você selecionar mais de um usuário para adicionar à sua conta do Partner Center, você deve atribuir a eles a mesma função ou conjunto de permissões personalizados. Para adicionar vários usuários com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.
4.  Quando tiver terminado de selecionar usuários, clique em **Adicionar selecionado**.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os usuários selecionados.
6.  Clique em **Salvar**.

### <a name="additional-methods-for-adding-users"></a>Outros métodos para adicionar usuários

Se você estiver conectado com uma conta de Gerenciador que também tem permissões de [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) do locatário do Azure AD estiver trabalhando em, você terá opções adicionais para adicionar usuários à sua conta do Partner Center. Selecione uma das seguintes opções:

-   **Adicionar usuários existentes**: escolha os usuários que já existem no diretório da organização e conceda a eles acesso à sua conta do Partner Center, usando o método descrito acima.
-   **Criar novos usuários**: Crie novas contas de usuário para adicionar ao diretório da organização tanto e à conta do Partner Center
-   **Convidar usuários externos**: envie convites por email aos usuários que não estão no diretório da organização atualmente. Eles serão convidados a acessar sua conta do Partner Center, e uma nova conta de [usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) será criada para eles no locatário do Azure AD.

<span id="new-user" />

### <a name="create-new-users"></a>Criar novos usuários

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global em seu locatário do Azure AD para criar novos usuários.

1.  Na página de **usuários** (em **configurações da conta**), selecione **Adicionar usuários**e escolha **criar novos usuários**.
2.  Insira o nome, o sobrenome e o nome de usuário do novo usuário.
3.  Se quiser que o novo usuário tenha uma [Conta de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) no diretório da organização, marque a caixa rotulada **Tornar esse usuário um Administrador Global no Azure AD, com controle total sobre todos os recursos de diretório**. Isso dará ao usuário acesso completo a todos os recursos administrativos no Azure AD de sua empresa. Eles poderá adicionar e gerenciar usuários no diretório da organização (embora não no Partner Center, a menos que você conceda à conta as [funções/permissões](set-custom-permissions-for-account-users.md)apropriadas). Se você marcar essa caixa, será necessário fornecer um **Email de recuperação de senha** para o usuário.
4.  Se você marcou a caixa **Tornar este usuário um administrador Global em seu Azure AD**, insira um email que o usuário pode usar caso precise recuperar a senha.
5.  Na seção **Associação de grupo**, selecione qualquer grupo ao qual o novo usuário deve pertencer.
6.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
7.  Clique em **Salvar**.
8.  Na página de confirmação, você verá as informações de logon do novo usuário, incluindo uma senha temporária. Certifique-se de anotar essas informações e fornecê-las ao novo usuário, já que você não poderá acessar a senha temporária depois que sair dessa página.


<span id="email" />

### <a name="invite-outside-users"></a>Convidar usuários externos

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global no seu locatário do Azure AD para convidar usuários externos.

1.  Na página de **usuários** (em **configurações da conta**), selecione **Adicionar usuários**e escolha **Convidar usuários por email**.
1.  Insira um ou mais endereços de email (até dez), separados por vírgulas ou pontos e vírgulas.
2.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
3.  Clique em **Salvar**.

Os usuários que você convidou receberão um convite por email para participar da sua conta, e uma nova conta de [usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) será criada para eles no seu locatário do Azure AD. Cada usuário precisa aceitar o convite para que possa acessar sua conta.

Se você precisar enviar um convite novamente, localize o usuário na página **Usuários** e selecione o endereço de email desse usuário (ou o texto que diz **Convite pendente**). Em seguida, na parte inferior da página, clique em **Reenviar o convite**.

> [!IMPORTANT]
> Os usuários externos que você convida para ingressar em sua conta do Partner Center pode ser atribuída as mesmas funções e permissões de outros usuários. No entanto, os usuários externos não poderão executar determinadas tarefas no Visual Studio, como associar um app à Store ou criar pacotes para carregar na Store. Se um usuário precisar executar essas tarefas, escolha **Criar novos usuários** em vez de **Convidar usuários externos**. (Se você não quiser adicionar esses usuários ao seu locatário do Azure AD existente, poderá [criar um novo locatário](../publish/associate-azure-ad-with-dev-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) e então criar novas contas de usuário para eles no locatário). 


### <a name="changing-a-users-directory-password"></a>Como alterar a senha do diretório de um usuário

Se um dos usuários precisar alterar a senha, ele pode fazer isso sozinho se você tiver fornecido um **Email de recuperação de senha** ao criar a conta de usuário. Você também pode atualizar a senha do usuário seguindo as etapas a seguir (se você estiver conectado com uma conta de administrador global no seu locatário do Azure AD para alterar a senha do usuário). Observe que isso irá alterar a senha do usuário em seu locatário Azure AD, juntamente com a senha que ele usa para acessar o Partner Center. 

1.  Na página de **usuários** (em **configurações da conta**), selecione o nome da conta de usuário que você deseja editar.
2.  Selecione o botão **Redefinir senha** na parte inferior da página.
3.  Uma página de confirmação será exibida mostrando as informações de logon do usuário, incluindo uma senha temporária.

    > [!IMPORTANT]
    >  Certifique-se de imprimir ou copiar essas informações e fornecê-las ao usuário, já que você não conseguirá acessar a senha temporária depois que sair dessa página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Adicionar grupos à sua conta do Partner Center

Você pode adicionar um grupo do diretório da organização à sua conta do Partner Center. Quando você fizer isso, cada usuário que é um membro desse grupo poderá acessá-lo com as permissões associadas à função atribuída do grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Adicionar grupos do diretório da sua organização

1.  Selecione o ícone de engrenagem (perto do canto superior direito do Partner Center) e, em seguida, selecione **as configurações do desenvolvedor**. No menu **configurações** , selecione **os usuários**.
2. Na página de **usuários** , selecione **Adicionar grupos**.
2.  Selecione um ou mais grupos na lista exibida. Você pode usar a caixa de pesquisa para procurar grupos específicos.
    > [!TIP]
    > Se você selecionar mais de um grupo para adicionar à sua conta do Partner Center, você deve atribuir a eles a mesma função ou conjunto de permissões personalizados. Para adicionar vários grupos com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

3.  Quando tiver terminado de selecionar grupos, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os grupos selecionados. Todos os membros do grupo poderão acessar sua conta do Partner Center com as permissões que aplicar ao grupo, independentemente dos funções/permissões associadas à sua conta individual.
5.  Clique em **Salvar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Criar uma nova conta de grupo no diretório da organização e adicioná-lo à sua conta do Partner Center

Se você deseja conceder acesso ao Partner Center para um novo grupo, você pode criar um novo grupo na seção de **usuários** . Observe que o novo grupo será criado no diretório da organização, não apenas em sua conta do Partner Center.

1.  Na página de **usuários** (em **configurações do desenvolvedor**), clique em **Adicionar grupos**.
2.  Na próxima página, selecione o **novo grupo**.
3.  Insira o nome de exibição para o novo grupo.
4.  Especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o grupo. Todos os membros do grupo poderão acessar sua conta do Partner Center com as permissões que aplicar ao grupo, independentemente dos funções/permissões associadas à sua conta individual.
5.  Selecione os usuários para atribuir ao novo grupo na lista que aparece. Você pode usar a caixa de pesquisa para procurar usuários específicos.
6.  Quando terminar de selecionar usuários, clique em **Adicionar selecionado** para adicioná-los ao novo grupo.
7.  Clique em **Salvar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Adicionar aplicativos Azure AD à sua conta do Partner Center

Você pode permitir que aplicativos ou serviços que fazem parte da Azure sua organização AD para acessar sua conta do Partner Center. Essas contas de usuário de aplicativo do Azure AD podem ser usadas para chamar as APIs REST fornecidas pelo [serviços da Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Adicionar aplicativos do Azure AD do diretório da sua organização

1.  1.  Selecione o ícone de engrenagem (perto do canto superior direito do Partner Center) e, em seguida, selecione **as configurações do desenvolvedor**. No menu **configurações** , selecione **os usuários**.
2. Na página **Usuários**, selecione **Adicionar aplicativos Azure AD**.
3.  Selecione um ou mais aplicativos Azure AD na lista exibida. Você pode usar a caixa de pesquisa para procurar aplicativos Azure AD específicos.
    > [!TIP]
    > Se você selecionar mais de um aplicativo do Azure AD para adicionar à sua conta do Partner Center, você deve atribuir a eles a mesma função ou conjunto de permissões personalizados. Para adicionar vários aplicativos Azure AD com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

4.  Quando tiver terminado de selecionar aplicativos Azure AD, clique em **Adicionar selecionado**.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os aplicativos Azure AD selecionados.
6.  Clique em **Salvar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Crie um novo aplicativo Azure AD da conta no diretório da organização e adicioná-lo à sua conta do Partner Center

Se você deseja conceder acesso ao Partner Center para um novo Azure AD conta do aplicativo, você pode criar uma na seção de **usuários** . Observe que isso criará uma nova conta no diretório da organização, não apenas em sua conta do Partner Center.

> [!TIP]
> Se você estiver principalmente usando este aplicativo do Azure AD para autenticação do Partner Center e não precisa que os usuários acessem diretamente, insira qualquer endereço válido para o **URL de resposta** e **URI da ID do aplicativo**, desde que esses valores não são usados por outro do Azure Aplicativo de anúncio em seu diretório.

1.  Na página de **usuários** (em **configurações da conta**), selecione **Adicionar aplicativos Azure AD**.
2.  Na próxima página, selecione o **aplicativo de novo Azure AD**.
3.  Insira a **URL de Resposta** para o novo aplicativo Azure AD. Essa é a URL onde os usuários podem entrar e usar seu aplicativo Azure AD (às vezes, também conhecida como a URL do Aplicativo ou a URL de Logon). A **URL de Resposta** não pode ter mais de 256 caracteres e deve ser exclusiva em seu diretório.
4.  Insira o **URI da ID do Aplicativo** para o novo aplicativo Azure AD. Ele é um identificador lógico do aplicativo Azure AD apresentado quando ele envia uma solicitação de logon único para o Azure AD. Observe que o **URI da ID do Aplicativo** deve ser exclusivo para cada aplicativo Azure AD no diretório, e não pode ter mais de 256 caracteres. Para saber mais sobre o **URI da ID do Aplicativo**, consulte [Como integrar os aplicativos com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o aplicativo Azure AD.
6.  Clique em **Salvar**.

Após adicionar ou criar um aplicativo Azure AD, você pode retornar à seção **Usuários** e selecionar o nome do aplicativo para analisar as configurações do aplicativo, incluindo a ID do locatário, a ID do cliente, a URL de resposta e o URI da ID do aplicativo.

> [!NOTE]
> Se você pretende usar as APIs REST fornecidas pelos [serviços da Microsoft Store](../monetize/using-windows-store-services.md), precisará dos valores de ID do locatário e ID do cliente mostrados nesta página para obter um token de acesso do Azure AD que pode ser usado para autenticar as chamadas para os serviços.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Gerenciar chaves para um aplicativo Azure AD

Se o aplicativo Azure AD ler e gravar dados no Microsoft Azure AD, ele precisará de uma chave. Você pode criar chaves para um aplicativo Azure AD ao editar suas informações no Partner Center. Você também pode remover as chaves que não são mais necessárias.

1.  Na página de **usuários** (em **configurações da conta**), selecione o nome do aplicativo do Azure AD.
    > [!TIP]
    > Ao clicar no nome do aplicativo Azure AD, você verá todas as suas chaves ativas, incluindo a data de criação e de expiração da chave. Para remover uma chave que não é mais necessária, clique em **Remover**.

2.  Para adicionar uma nova chave, selecione **Adicionar nova chave**.
3.  Você verá uma tela mostrando os valores **ID do Cliente** e **Chave**.
    > [!IMPORTANT]
    > Certifique-se de imprimir ou copiar essas informações, já que você não conseguirá acessá-la novamente depois que sair dessa página.

4.  Se você quiser criar mais chaves, selecione **Adicionar outra chave**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar um usuário, grupo ou aplicativo Azure AD

Depois de adicionar usuários, grupos e/ou aplicativos Azure AD à sua conta do Partner Center, você pode fazer alterações nas informações da conta. 

> [!IMPORTANT]
> As alterações feitas nas [funções e permissões](set-custom-permissions-for-account-users.md) afetará apenas o acesso ao Partner Center. Todas as outras alterações (por exemplo, alterar o nome do usuário ou associação de grupo, ou a URL de resposta e URI da ID do aplicativo para um aplicativo Azure AD) serão refletidas no locatário do Azure AD da sua organização, bem como em sua conta do Partner Center. 

1.  Na página de **usuários** (em **configurações da conta**), selecione o nome do usuário, grupo ou conta de aplicativo do Azure AD que você deseja editar.
2.  Faça as alterações desejadas. Os itens que você pode editar são:
    -   Para um **usuário**, você pode editar o nome, o sobrenome ou o nome de usuário do usuário. Você também pode selecionar ou desmarcar grupos na seção **Associação de grupo** para atualizar a participação no grupo.
    -   Para um **grupo**, você pode editar o nome do grupo. (para atualizar a associação de grupo, os usuários que você deseja adicionar ou remover do grupo e fazer alterações para editar a seção **Membros do grupo**.)
    -   Para um **aplicativo Azure AD**, você pode inserir novos valores para a **URL de resposta** ou **URI da ID do aplicativo**.
    Lembre-se de que essas alterações serão efetuadas no diretório da sua organização, bem como em sua conta do Partner Center.
3.  Para alterações relacionadas ao acesso ao Partner Center, marque ou desmarque as funções que deseja aplicar, ou selecione **Personalizar permissões** e faça as alterações desejadas. Essas alterações afetam somente o Partner Center acessar e não mudará as permissões no locatário do Azure AD da sua organização.
3.  Clique em **Salvar**.


## <a name="view-history-for-account-users"></a>Exibir histórico dos usuários da conta

Como proprietário da conta, você pode exibir o histórico de navegação detalhado de qualquer usuário que você adicionou à conta.

Na página **usuários** (em **configurações da conta**), selecione o link mostrado em **última atividade** do usuário cujo histórico de navegação você deseja revisar. Você poderá exibir os URLs de todas as páginas que o usuário visitou nos últimos 30 dias.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Remover usuários, grupos e aplicativos Azure AD

Para remover um usuário, grupo ou aplicativo Azure AD de sua conta do Partner Center, selecione o link **Remover** que aparece ao lado do nome na página **usuários** . Depois de confirmar que você deseja removê-la, esse usuário, grupo ou aplicativo Azure AD poderá mais acessar sua conta do Partner Center (a menos que você adicioná-lo novamente mais tarde).

> [!IMPORTANT]
> Remover um usuário, grupo ou aplicativo Azure AD significa que ele não terá acesso à sua conta do Partner Center. Isso **não** exclui o usuário, grupo ou aplicativo Azure AD do diretório da organização.

 

