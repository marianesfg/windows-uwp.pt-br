---
Description: Você pode adicionar usuários, grupos e aplicativos do Azure AD à sua conta do Partner Center.
title: Adicionar usuários, grupos e aplicativos do Azure AD à sua conta do Partner Center
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, aplicativo do Azure AD, AAD, usuário, grupo, vários usuários, multiusuário
ms.localizationpriority: medium
ms.openlocfilehash: 41467f51e02f3cc700e3759f33d6fd6eea3ac7a6
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260074"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Adicionar usuários, grupos e aplicativos do Azure AD à sua conta do Partner Center

A seção **usuários** do [Partner Center](https://partner.microsoft.com/dashboard) (em **configurações de conta**) permite que você use Azure Active Directory para adicionar usuários à sua conta do Partner Center. Cada usuário recebe uma função (ou conjunto de permissões personalizadas) que define o acesso à conta. Você também pode adicionar [grupos de usuários](#groups) e [aplicativos do Azure ad](#azure-ad-applications) para conceder a eles acesso à sua conta do Partner Center.

Depois que os usuários são adicionados à conta, você pode [editar detalhes da conta](#edit), alterar [funções e permissões](set-custom-permissions-for-account-users.md) ou [remover usuários](#remove).

> [!IMPORTANT]
> para adicionar usuários à sua conta, primeiro você deve [associar sua conta do Partner Center ao locatário de Azure Active Directory da sua organização](associate-azure-ad-with-partner-center.md). 

Ao adicionar usuários, você precisará especificar seu acesso à sua conta do Partner Center atribuindo-lhes uma [função ou um conjunto de permissões personalizadas](set-custom-permissions-for-account-users.md). 

Tenha em mente que todos os usuários do Partner Center (incluindo grupos e aplicativos do Azure AD) devem ter uma conta ativa em [um locatário do Azure AD que esteja associado à sua conta do Partner Center](associate-azure-ad-with-partner-center.md). O gerenciamento de usuários é realizado em um locatário de cada vez. Você deve entrar com uma conta de gerenciador para o locatário em que você deseja adicionar ou editar usuários. A criação de um novo usuário no Partner Center também criará uma conta para esse usuário no locatário do Azure AD no qual você está conectado e fazer alterações no nome de um usuário no Partner Center fará as mesmas alterações no locatário do Azure AD da sua organização.

> [!NOTE]
> Se sua organização usar a [integração de diretórios](https://docs.microsoft.com/previous-versions/azure/azure-services/jj573653(v=azure.100)?redirectedfrom=MSDN) para sincronizar o serviço de diretório local com o Azure AD, você não poderá criar novos usuários, grupos ou aplicativos do Azure AD no Partner Center. Você (ou outro administrador em seu diretório local) precisará criá-los diretamente no diretório local antes que você possa vê-los e adicioná-los no Partner Center.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Adicionar usuários à sua conta do Partner Center

Para adicionar usuários à sua conta do Partner Center, vá para a página **usuários** em **configurações da conta** e selecione **Adicionar usuários.** Você deve estar conectado com uma conta de gerenciador do locatário do Azure AD em que você deseja trabalhar. 

### <a name="add-existing-users"></a>Adicionar usuários existentes 

Você pode selecionar os usuários que já existem no locatário da sua organização e dar a eles acesso à sua conta do Partner Center. 

<span id="from-directory" />

1.  Selecione o ícone de engrenagem (próximo ao canto superior direito da central de parceiros) e, em seguida, selecione **configurações do desenvolvedor**. No menu **configurações** , selecione **usuários**.
2.  Na página **Usuários**, selecione **Adicionar usuários**. 
3.  Selecione um ou mais usuários na lista exibida. Você pode usar a caixa de pesquisa para procurar usuários específicos.
    > [!TIP]
    > Se você selecionar mais de um usuário para adicionar à sua conta do Partner Center, deverá atribuir a mesma função ou conjunto de permissões personalizadas. Para adicionar vários usuários com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.
4.  Quando tiver terminado de selecionar usuários, clique em **Adicionar selecionado**.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os usuários selecionados.
6.  Clique em **Salvar**.

### <a name="additional-methods-for-adding-users"></a>Outros métodos para adicionar usuários

Se você estiver conectado com uma conta de gerente que também tem permissões de [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) para o locatário do Azure AD no qual está trabalhando, você terá opções adicionais para adicionar usuários à sua conta do Partner Center. Selecione uma das seguintes opções:

-   **Adicionar usuários existentes**: escolha os usuários que já existem no diretório da sua organização e conceda a eles acesso à sua conta do Partner Center, usando o método descrito acima.
-   **Criar novos usuários**: crie novas contas de usuário para adicionar ao diretório da sua organização e à sua conta do Partner Center
-   **Convidar usuários externos**: envie convites por email aos usuários que não estão no diretório da organização atualmente. Eles serão convidados a acessar sua conta do Partner Center e uma nova conta de [usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) será criada para eles em seu locatário do Azure AD.

<span id="new-user" />

### <a name="create-new-users"></a>Criar novos usuários

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global em seu locatário do Azure AD para criar novos usuários.

1.  Na página **usuários** (em **configurações de conta**), selecione **Adicionar usuários**e, em seguida, escolha **criar novos usuários**.
2.  Insira o nome, o sobrenome e o nome de usuário do novo usuário.
3.  Se quiser que o novo usuário tenha uma [Conta de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) no diretório da organização, marque a caixa rotulada **Tornar esse usuário um Administrador Global no Azure AD, com controle total sobre todos os recursos de diretório**. Isso dará ao usuário acesso completo a todos os recursos administrativos no Azure AD de sua empresa. Eles poderão adicionar e gerenciar usuários no diretório da sua organização (embora não no Partner Center, a menos que você conceda à conta as [permissões/funções](set-custom-permissions-for-account-users.md)apropriadas). Se você marcar essa caixa, será necessário fornecer um **Email de recuperação de senha** para o usuário.
4.  Se você marcou a caixa **Tornar este usuário um administrador Global em seu Azure AD**, insira um email que o usuário pode usar caso precise recuperar a senha.
5.  Na seção **Associação de grupo**, selecione qualquer grupo ao qual o novo usuário deve pertencer.
6.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
7.  Clique em **Salvar**.
8.  Na página de confirmação, você verá as informações de logon do novo usuário, incluindo uma senha temporária. Certifique-se de anotar essas informações e fornecê-las ao novo usuário, já que você não poderá acessar a senha temporária depois que sair dessa página.


<span id="email" />

### <a name="invite-outside-users"></a>Convidar usuários externos

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global no seu locatário do Azure AD para convidar usuários externos.

1.  Na página **usuários** (em **configurações de conta**), selecione **Adicionar usuários**e, em seguida, escolha **convidar usuários por email**.
1.  Insira um ou mais endereços de email (até dez), separados por vírgulas ou pontos e vírgulas.
2.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
3.  Clique em **Salvar**.

Os usuários que você convidou receberão um convite por email para participar da sua conta, e uma nova conta de [usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) será criada para eles no seu locatário do Azure AD. Cada usuário precisará aceitar o convite antes que possa acessar sua conta.

Se você precisar enviar um convite novamente, localize o usuário na página **Usuários** e selecione o endereço de email desse usuário (ou o texto que diz **Convite pendente**). Em seguida, na parte inferior da página, clique em **Reenviar o convite**.

> [!IMPORTANT]
> Usuários externos que você convidar para ingressar na sua conta do Partner Center podem receber as mesmas funções e permissões que outros usuários. No entanto, os usuários externos não poderão executar determinadas tarefas no Visual Studio, como associar um app à Store ou criar pacotes para carregar na Store. Se um usuário precisar executar essas tarefas, escolha **Criar novos usuários** em vez de **Convidar usuários externos**. (Se você não quiser adicionar esses usuários ao seu locatário do Azure AD existente, poderá [criar um novo locatário](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) e então criar novas contas de usuário para eles no locatário). 


### <a name="changing-a-users-directory-password"></a>Como alterar a senha do diretório de um usuário

Se um dos usuários precisar alterar a senha, ele pode fazer isso sozinho se você tiver fornecido um **Email de recuperação de senha** ao criar a conta de usuário. Você também pode atualizar a senha do usuário seguindo as etapas a seguir (se você estiver conectado com uma conta de administrador global no seu locatário do Azure AD para alterar a senha do usuário). Observe que isso alterará a senha do usuário em seu locatário do Azure AD, juntamente com a senha que eles usam para acessar o Partner Center. 

1.  Na página **usuários** (em **configurações de conta**), selecione o nome da conta de usuário que você deseja editar.
2.  Selecione o botão **Redefinir senha** na parte inferior da página.
3.  Uma página de confirmação será exibida mostrando as informações de logon do usuário, incluindo uma senha temporária.

    > [!IMPORTANT]
    >  certifique-se de imprimir ou copiar essas informações e fornecê-las ao usuário, pois você não poderá acessar a senha temporária depois de sair dessa página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Adicionar grupos à sua conta do Partner Center

Você pode adicionar um grupo do diretório da sua organização à sua conta do Partner Center. Quando você fizer isso, cada usuário que é um membro desse grupo poderá acessá-lo com as permissões associadas à função atribuída do grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Adicionar grupos do diretório de sua organização

1.  Selecione o ícone de engrenagem (próximo ao canto superior direito da central de parceiros) e, em seguida, selecione **configurações do desenvolvedor**. No menu **configurações** , selecione **usuários**.
2. Na página **usuários** , selecione **Adicionar grupos**.
2.  Selecione um ou mais grupos na lista exibida. Você pode usar a caixa de pesquisa para procurar grupos específicos.
    > [!TIP]
    > Se você selecionar mais de um grupo para adicionar à sua conta do Partner Center, deverá atribuir a mesma função ou conjunto de permissões personalizadas. Para adicionar vários grupos com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

3.  Quando tiver terminado de selecionar grupos, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os grupos selecionados. Todos os membros do grupo poderão acessar sua conta do Partner Center com as permissões que você aplicar ao grupo, independentemente das funções/permissões associadas à sua conta individual.
5.  Clique em **Salvar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Criar uma nova conta de grupo no diretório da sua organização e adicioná-la à sua conta do Partner Center

Se você quiser conceder acesso à central de parceiros a um grupo totalmente novo, poderá criar um novo grupo na seção **usuários** . Observe que o novo grupo será criado no diretório da sua organização, não apenas na sua conta do Partner Center.

1.  Na página **usuários** (em **configurações do desenvolvedor**), clique em **Adicionar grupos**.
2.  Na página seguinte, selecione **novo grupo**.
3.  Insira o nome de exibição para o novo grupo.
4.  Especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o grupo. Todos os membros do grupo poderão acessar sua conta do Partner Center com as permissões que você aplicar ao grupo, independentemente das funções/permissões associadas à sua conta individual.
5.  Selecione os usuários para atribuir ao novo grupo na lista que aparece. Você pode usar a caixa de pesquisa para procurar usuários específicos.
6.  Quando terminar de selecionar usuários, clique em **Adicionar selecionado** para adicioná-los ao novo grupo.
7.  Clique em **Salvar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Adicionar aplicativos do Azure AD à sua conta do Partner Center

Você pode permitir que aplicativos ou serviços que fazem parte do Azure AD da sua organização acessem sua conta do Partner Center. Essas contas de usuário de aplicativo do Azure AD podem ser usadas para chamar as APIs REST fornecidas pelo [serviços da Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Adicionar aplicativos Azure AD do diretório da organização

1.  1.  Selecione o ícone de engrenagem (próximo ao canto superior direito da central de parceiros) e, em seguida, selecione **configurações do desenvolvedor**. No menu **configurações** , selecione **usuários**.
2. Na página **Usuários**, selecione **Adicionar aplicativos Azure AD**.
3.  Selecione um ou mais aplicativos Azure AD na lista exibida. Você pode usar a caixa de pesquisa para procurar aplicativos Azure AD específicos.
    > [!TIP]
    > Se você selecionar mais de um aplicativo do Azure AD para adicionar à sua conta do Partner Center, deverá atribuir a mesma função ou conjunto de permissões personalizadas. Para adicionar vários aplicativos Azure AD com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

4.  Quando tiver terminado de selecionar aplicativos Azure AD, clique em **Adicionar selecionado**.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os aplicativos Azure AD selecionados.
6.  Clique em **Salvar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Criar uma nova conta de aplicativo do Azure AD no diretório da sua organização e adicioná-la à sua conta do Partner Center

Se você quiser conceder acesso à central de parceiros a uma nova conta de aplicativo do Azure AD, poderá criar uma na seção **usuários** . Observe que isso criará uma nova conta no diretório da sua organização, não apenas na sua conta do Partner Center.

> [!TIP]
> Se você estiver usando principalmente esse aplicativo do Azure AD para a autenticação do Partner Center e não precisar que os usuários o acessem diretamente, poderá inserir qualquer endereço válido para a **URL de resposta** e o URI da **ID do aplicativo**, desde que esses valores não sejam usados por nenhum outro Azure Aplicativo do AD em seu diretório.

1.  Na página **usuários** (em **configurações de conta**), selecione **adicionar aplicativos do Azure ad**.
2.  Na página seguinte, selecione **novo aplicativo Azure ad**.
3.  Insira a **URL de Resposta** para o novo aplicativo Azure AD. Essa é a URL onde os usuários podem entrar e usar seu aplicativo Azure AD (às vezes, também conhecida como a URL do Aplicativo ou a URL de Logon). A **URL de Resposta** não pode ter mais de 256 caracteres e deve ser exclusiva em seu diretório.
4.  Insira o **URI da ID do Aplicativo** para o novo aplicativo Azure AD. Ele é um identificador lógico do aplicativo Azure AD apresentado quando ele envia uma solicitação de logon único para o Azure AD. Observe que o **URI da ID do Aplicativo** deve ser exclusivo para cada aplicativo Azure AD no diretório, e não pode ter mais de 256 caracteres. Para saber mais sobre o **URI da ID do Aplicativo**, consulte [Como integrar os aplicativos com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o aplicativo Azure AD.
6.  Clique em **Salvar**.

Após adicionar ou criar um aplicativo Azure AD, você pode retornar à seção **Usuários** e selecionar o nome do aplicativo para analisar as configurações do aplicativo, incluindo a ID do locatário, a ID do cliente, a URL de resposta e o URI da ID do aplicativo.

> [!NOTE]
> Se você pretende usar as APIs REST fornecidas pelos [serviços da Microsoft Store](../monetize/using-windows-store-services.md), precisará dos valores de ID do locatário e ID do cliente mostrados nesta página para obter um token de acesso do Azure AD que pode ser usado para autenticar as chamadas para os serviços.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Gerenciar chaves para um aplicativo Azure AD

Se o aplicativo Azure AD ler e gravar dados no Microsoft Azure AD, ele precisará de uma chave. Você pode criar chaves para um aplicativo do Azure AD editando suas informações no Partner Center. Você também pode remover as chaves que não são mais necessárias.

1.  Na página **usuários** (em **configurações de conta**), selecione o nome do aplicativo do Azure AD.
    > [!TIP]
    > ao clicar no nome do aplicativo do Azure AD, você verá todas as chaves ativas para o aplicativo do Azure AD, incluindo a data em que a chave foi criada e quando ela expirará. Para remover uma chave que não é mais necessária, clique em **Remover**.

2.  Para adicionar uma nova chave, selecione **Adicionar nova chave**.
3.  Você verá uma tela mostrando os valores **ID do Cliente** e **Chave**.
    > [!IMPORTANT]
    > certifique-se de imprimir ou copiar essas informações, pois você não poderá acessá-las novamente depois de sair dessa página.

4.  Se você quiser criar mais chaves, selecione **adicionar outra chave**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar um usuário, grupo ou aplicativo Azure AD

Depois de adicionar usuários, grupos e/ou aplicativos do Azure AD à sua conta do Partner Center, você pode fazer alterações nas informações da conta. 

> [!IMPORTANT]
> As alterações feitas às [funções ou permissões](set-custom-permissions-for-account-users.md) só afetarão o acesso do Partner Center. Todas as outras alterações (como alterar o nome de um usuário ou a associação de grupo ou a URL de resposta e o URI de ID do aplicativo para um aplicativo do Azure AD) serão refletidas no locatário do Azure AD da sua organização, bem como em sua conta do Partner Center. 

1.  Na página **usuários** (em **configurações de conta**), selecione o nome do usuário, grupo ou conta de aplicativo do Azure AD que você deseja editar.
2.  Faça as alterações desejadas. Os itens que você pode editar são:
    -   Para um **usuário**, você pode editar o nome, o sobrenome ou o nome de usuário do usuário. Você também pode selecionar ou desmarcar grupos na seção **Associação de grupo** para atualizar a participação no grupo.
    -   Para um **grupo**, você pode editar o nome do grupo. (para atualizar a associação de grupo, os usuários que você deseja adicionar ou remover do grupo e fazer alterações para editar a seção **Membros do grupo**.)
    -   Para um **aplicativo Azure AD**, você pode inserir novos valores para a **URL de resposta** ou **URI da ID do aplicativo**.
    Lembre-se de que essas alterações serão feitas no diretório da sua organização, bem como na sua conta do Partner Center.
3.  Para alterações relacionadas ao acesso do Partner Center, selecione ou desmarque as funções que você deseja aplicar ou selecione **Personalizar permissões** e faça as alterações desejadas. Essas alterações só afetam o acesso do centro de parceria e não alteram nenhuma permissão no locatário do Azure AD da sua organização.
3.  Clique em **Salvar**.


## <a name="view-history-for-account-users"></a>Exibir histórico dos usuários da conta

Como proprietário da conta, você pode exibir o histórico de navegação detalhado de qualquer usuário que você adicionou à conta.

Na página **usuários** (em **configurações de conta**), selecione o link mostrado em **última atividade** para o usuário cujo histórico de navegação você gostaria de examinar. Você poderá exibir os URLs de todas as páginas que o usuário visitou nos últimos 30 dias.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Remover usuários, grupos e aplicativos Azure AD

Para remover um usuário, grupo ou aplicativo do Azure AD da sua conta do Partner Center, selecione o link **remover** que aparece por seu nome na página **usuários** . Depois de confirmar que você deseja removê-lo, esse usuário, grupo ou aplicativo do Azure AD não poderá mais acessar sua conta do Partner Center (a menos que você a adicione novamente mais tarde).

> [!IMPORTANT]
> A remoção de um usuário, grupo ou aplicativo do Azure AD significa que ele não terá mais acesso à sua conta do Partner Center. Isso **não** exclui o usuário, grupo ou aplicativo Azure AD do diretório da organização.

 

