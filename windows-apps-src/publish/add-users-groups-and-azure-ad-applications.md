---
Description: Você pode adicionar usuários, grupos e aplicativos do Azure AD para sua conta no Partner Center.
title: Adicionar usuários, grupos e aplicativos do Azure AD para sua conta no Partner Center
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, uwp, aplicativo do azure ad, aad, usuário, grupo, vários usuários, com vários usuários
ms.localizationpriority: medium
ms.openlocfilehash: ddbe47d94e17db0d272aedcff56df95fccf3434d
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63787290"
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-partner-center-account"></a>Adicionar usuários, grupos e aplicativos do Azure AD para sua conta no Partner Center

O **os usuários** seção [Partner Center](https://partner.microsoft.com/dashboard) (sob **configurações de conta**) permite que você use o Azure Active Directory para adicionar usuários à sua conta do Partner Center. Cada usuário recebe uma função (ou conjunto de permissões personalizadas) que define o acesso à conta. Você também pode adicionar [grupos de usuários](#groups) e [aplicativos do Azure AD](#azure-ad-applications) para conceder a eles acesso à sua conta do Partner Center.

Depois que os usuários são adicionados à conta, você pode [editar detalhes da conta](#edit), alterar [funções e permissões](set-custom-permissions-for-account-users.md) ou [remover usuários](#remove).

> [!IMPORTANT]
> Para adicionar usuários à sua conta, você deve primeiro [associar sua conta no Partner Center ao locatário do Azure Active Directory da sua organização](associate-azure-ad-with-partner-center.md). 

Ao adicionar usuários, você precisará especificar o acesso a sua conta no Partner Center atribuindo-lhes uma [função ou um conjunto de permissões personalizadas](set-custom-permissions-for-account-users.md). 

Lembre-se de que todos os usuários do Partner Center (incluindo grupos e aplicativos do Azure AD) devem ter uma conta ativa [um locatário do AD do Azure que está associado com sua conta no Partner Center](associate-azure-ad-with-partner-center.md). O gerenciamento de usuários é realizado em um locatário de cada vez. Você deve entrar com uma conta de gerenciador para o locatário em que você deseja adicionar ou editar usuários. Criando um novo usuário no Partner Center também criará uma conta para que o usuário no locatário do Azure AD ao qual você está conectado e fazendo alterações em um nome de usuário no Partner Center fazer as mesmas alterações no locatário do Azure AD da sua organização.

> [!NOTE]
> Se sua organização usa [integração de diretórios](https://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar o serviço de diretório local com o Azure AD, você não poderá criar novos usuários, grupos ou aplicativos do Azure AD no Partner Center. Você (ou outro administrador em seu diretório local) será necessário criá-los diretamente no diretório local antes de você poderá ver e adicioná-los no Partner Center.


<span id="users" />

## <a name="add-users-to-your-partner-center-account"></a>Adicionar usuários à sua conta no Partner Center

Para adicionar usuários à sua conta no Partner Center, vá para o **os usuários** página no **configurações de conta** e selecione **adicionar usuários.** Você deve estar conectado com uma conta de gerenciador do locatário do Azure AD em que você deseja trabalhar. 

### <a name="add-existing-users"></a>Adicionar usuários existentes 

Você pode selecionar os usuários que já existem no locatário da sua organização e concede acesso à sua conta no Partner Center. 

<span id="from-directory" />

1.  Selecione o ícone de engrenagem (perto do canto superior direito do Partner Center) e, em seguida, selecione **configurações do desenvolvedor**. No **as configurações** menu, selecione **usuários**.
2.  Na página **Usuários**, selecione **Adicionar usuários**. 
3.  Selecione um ou mais usuários na lista exibida. Você pode usar a caixa de pesquisa para procurar usuários específicos.
    > [!TIP]
    > Se você selecionar mais de um usuário para adicionar à sua conta no Partner Center, você deve atribui-los a mesma função ou conjunto de permissões personalizadas. Para adicionar vários usuários com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.
4.  Quando tiver terminado de selecionar usuários, clique em **Adicionar selecionado**.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os usuários selecionados.
6.  Clique em **Salvar**.

### <a name="additional-methods-for-adding-users"></a>Outros métodos para adicionar usuários

Se você estiver conectado com uma conta do Gerenciador que também tem [administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) permissões para o locatário do Azure AD que você está trabalhando, você terá opções adicionais para adicionar usuários à sua conta no Partner Center. Selecione uma das seguintes opções:

-   **Adicione usuários existentes**: Escolha os usuários que já existem no diretório da sua organização e concede acesso à sua conta no Partner Center, usando o método descrito acima.
-   **Criar novos usuários**: Criar contas de usuário totalmente novo para adicionar ao diretório da sua organização de ambos e sua conta no Partner Center
-   **Convidar usuários externos**: Envie email convites para usuários que não estão atualmente no diretório da sua organização. Eles serão convidados a acessar sua conta no Partner Center e uma nova [usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) conta será criada para eles no seu locatário do Azure AD.

<span id="new-user" />

### <a name="create-new-users"></a>Criar novos usuários

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global em seu locatário do Azure AD para criar novos usuários.

1.  Do **os usuários** página (sob **configurações de conta**), selecione **adicionar usuários**, em seguida, escolha **criar novos usuários**.
2.  Insira o nome, o sobrenome e o nome de usuário do novo usuário.
3.  Se quiser que o novo usuário tenha uma [Conta de administrador global](https://docs.microsoft.com/azure/active-directory/users-groups-roles/directory-assign-admin-roles) no diretório da organização, marque a caixa rotulada **Tornar esse usuário um Administrador Global no Azure AD, com controle total sobre todos os recursos de diretório**. Isso dará ao usuário acesso completo a todos os recursos administrativos no Azure AD de sua empresa. Eles poderão adicionar e gerenciar usuários no diretório da sua organização (embora não no parceiro do Datacenter, a menos que você conceda à conta apropriada [permissões defunção/](set-custom-permissions-for-account-users.md)). Se você marcar essa caixa, você precisará fornecer um **Email de recuperação de senha** para o usuário.
4.  Se você marcou a caixa **Tornar este usuário um administrador Global em seu Azure AD**, insira um email que o usuário pode usar caso precise recuperar a senha.
5.  Na seção **Associação de grupo**, selecione qualquer grupo ao qual o novo usuário deve pertencer.
6.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
7.  Clique em **Salvar**.
8.  Na página de confirmação, você verá as informações de logon do novo usuário, incluindo uma senha temporária. Certifique-se de anotar essas informações e fornecê-las ao novo usuário, já que você não poderá acessar a senha temporária depois que sair dessa página.


<span id="email" />

### <a name="invite-outside-users"></a>Convidar usuários externos

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global no seu locatário do Azure AD para convidar usuários externos.

1.  Do **os usuários** página (sob **configurações de conta**), selecione **adicionar usuários**, em seguida, escolha **convidar usuários por email**.
1.  Insira um ou mais endereços de email (até dez), separados por vírgulas ou ponto e vírgulas.
2.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
3.  Clique em **Salvar**.

Os usuários que você convidou receberão um convite por email para participar da sua conta, e uma nova conta de [usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) será criada para eles no seu locatário do Azure AD. Cada usuário precisará aceitar o convite antes que possa acessar sua conta.

Se você precisar enviar um convite novamente, localize o usuário na página **Usuários** e selecione o endereço de email desse usuário (ou o texto que diz **Convite pendente**). Em seguida, na parte inferior da página, clique em **Reenviar convite**.

> [!IMPORTANT]
> Fora de usuários que você convidar para ingressar em sua conta no Partner Center pode ser atribuída as mesmas funções e permissões que os outros usuários. No entanto, os usuários externos não poderão executar determinadas tarefas no Visual Studio, como associar um app à Store ou criar pacotes para carregar na Store. Se um usuário precisar executar essas tarefas, escolha **Criar novos usuários** em vez de **Convidar usuários externos**. (Se você não quiser adicionar esses usuários ao seu locatário do Azure AD existente, poderá [criar um novo locatário](../publish/associate-azure-ad-with-partner-center.md#create-a-brand-new-azure-ad-to-associate-with-your-partner-center-account) e então criar novas contas de usuário para eles no locatário). 


### <a name="changing-a-users-directory-password"></a>Alterando a senha do diretório de um usuário

Se um dos usuários precisar alterar a senha, ele pode fazer isso sozinho se você tiver fornecido um **Email de recuperação de senha** ao criar a conta de usuário. Você também pode atualizar a senha do usuário seguindo as etapas a seguir (se você estiver conectado com uma conta de administrador global no seu locatário do Azure AD para alterar a senha do usuário). Observe que isso alterará a senha do usuário no locatário do AD do Azure, junto com a senha que eles usam para acessar o Centro de parceiros. 

1.  Dos **os usuários** página (sob **configurações de conta**), selecione o nome da conta de usuário que você deseja editar.
2.  Selecione o **redefinição de senha** botão na parte inferior da página.
3.  Uma página de confirmação será exibida mostrando as informações de logon do usuário, incluindo uma senha temporária.

    > [!IMPORTANT]
    >  Não se esqueça de imprimir ou copiar essas informações e fornecê-la para o usuário, pois você não poderá acessar a senha temporária depois que você sair desta página.

<span id="groups" />

## <a name="add-groups-to-your-partner-center-account"></a>Adicionar grupos para sua conta no Partner Center

Você pode adicionar um grupo do diretório da sua organização para sua conta no Partner Center. Quando você fizer isso, cada usuário que é um membro desse grupo poderá acessá-lo com as permissões associadas à função atribuída do grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Adicionar grupos do diretório de sua organização

1.  Selecione o ícone de engrenagem (perto do canto superior direito do Partner Center) e, em seguida, selecione **configurações do desenvolvedor**. No **as configurações** menu, selecione **usuários**.
2. Dos **os usuários** página, selecione **adicionar grupos**.
2.  Selecione um ou mais grupos na lista exibida. Você pode usar a caixa de pesquisa para procurar grupos específicos.
    > [!TIP]
    > Se você selecionar mais de um grupo para adicionar à sua conta no Partner Center, você deve atribui-los a mesma função ou conjunto de permissões personalizadas. Para adicionar vários grupos com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

3.  Quando tiver terminado de selecionar grupos, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os grupos selecionados. Todos os membros do grupo será capazes de acessar sua conta no Partner Center com as permissões que aplicar ao grupo, independentemente das permissões ou funções associados com sua conta individual.
5.  Clique em **Salvar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Criar uma nova conta de grupo no diretório da sua organização e adicioná-lo à sua conta no Partner Center

Se você deseja conceder acesso do Partner Center para um novo grupo, você pode criar um novo grupo na **usuários** seção. Observe que o novo grupo será criado no diretório da sua organização, não apenas na sua conta no Partner Center.

1.  Dos **os usuários** página (sob **configurações do desenvolvedor**), clique em **adicionar grupos**.
2.  Na próxima página, selecione **novo grupo**.
3.  Insira o nome de exibição para o novo grupo.
4.  Especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o grupo. Todos os membros do grupo será capazes de acessar sua conta no Partner Center com as permissões que aplicar ao grupo, independentemente das permissões ou funções associados com sua conta individual.
5.  Selecione os usuários para atribuir ao novo grupo na lista que aparece. Você pode usar a caixa de pesquisa para procurar usuários específicos.
6.  Quando terminar de selecionar usuários, clique em **Adicionar selecionado** para adicioná-los ao novo grupo.
7.  Clique em **Salvar**.


<span id="azure-ad-applications" />

## <a name="add-azure-ad-applications-to-your-partner-center-account"></a>Adicionar aplicativos do Azure AD para sua conta no Partner Center

Você pode permitir que aplicativos ou serviços que fazem parte de Azure sua organização AD para acessar sua conta no Partner Center. Essas contas de usuário de aplicativo do Azure AD podem ser usadas para chamar as APIs REST fornecidas pelo [serviços da Microsoft Store](../monetize/using-windows-store-services.md).


### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Adicionar aplicativos Azure AD do diretório da organização

1.  1.  Selecione o ícone de engrenagem (perto do canto superior direito do Partner Center) e, em seguida, selecione **configurações do desenvolvedor**. No **as configurações** menu, selecione **usuários**.
2. Na página **Usuários**, selecione **Adicionar aplicativos Azure AD**.
3.  Selecione um ou mais aplicativos Azure AD na lista exibida. Você pode usar a caixa de pesquisa para procurar aplicativos Azure AD específicos.
    > [!TIP]
    > Se você selecionar mais de um aplicativo do Azure AD para adicionar à sua conta no Partner Center, você deve atribui-los a mesma função ou conjunto de permissões personalizadas. Para adicionar vários aplicativos Azure AD com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

4.  Quando tiver terminado de selecionar aplicativos Azure AD, clique em **Adicionar selecionado**.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os aplicativos Azure AD selecionados.
6.  Clique em **Salvar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-partner-center-account"></a>Criar um novo aplicativo do Azure AD da conta no diretório da sua organização e adicioná-lo à sua conta no Partner Center

Se você deseja conceder acesso do Partner Center para uma conta de aplicativo de marca nova do Azure AD, você pode criar uma na **usuários** seção. Observe que isso irá criar uma nova conta no diretório da sua organização, não apenas em sua conta no Partner Center.

> [!TIP]
> Se você principalmente estiver usando este aplicativo do Azure AD para autenticação do Partner Center e não é necessário que os usuários acessá-la diretamente, você pode inserir qualquer endereço válido para o **URL de resposta** e **URI da ID do aplicativo**, desde como esses valores não são usados por outro aplicativo do Azure AD em seu diretório.

1.  Dos **os usuários** página (sob **configurações de conta**), selecione **adicionar aplicativos do Azure AD**.
2.  Na próxima página, selecione **aplicativo de novo do Azure AD**.
3.  Insira a **URL de Resposta** para o novo aplicativo Azure AD. Essa é a URL onde os usuários podem entrar e usar seu aplicativo Azure AD (às vezes, também conhecida como a URL do Aplicativo ou a URL de Logon). A **URL de Resposta** não pode ter mais de 256 caracteres e deve ser exclusiva em seu diretório.
4.  Insira o **URI da ID do Aplicativo** para o novo aplicativo Azure AD. Ele é um identificador lógico do aplicativo Azure AD apresentado quando ele envia uma solicitação de logon único para o Azure AD. Observe que o **URI da ID do Aplicativo** deve ser exclusivo para cada aplicativo Azure AD no diretório, e não pode ter mais de 256 caracteres. Para saber mais sobre o **URI da ID do Aplicativo**, consulte [Como integrar os aplicativos com o Azure Active Directory](https://docs.microsoft.com/azure/active-directory/develop/active-directory-integrating-applications#changing-the-application-registration-to-support-multi-tenant).
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o aplicativo Azure AD.
6.  Clique em **Salvar**.

Após adicionar ou criar um aplicativo Azure AD, você pode retornar à seção **Usuários** e selecionar o nome do aplicativo para analisar as configurações do aplicativo, incluindo a ID do locatário, a ID do cliente, a URL de resposta e o URI da ID do aplicativo.

> [!NOTE]
> Se você pretende usar as APIs REST fornecidas pelos [serviços da Microsoft Store](../monetize/using-windows-store-services.md), precisará dos valores de ID do locatário e ID do cliente mostrados nesta página para obter um token de acesso do Azure AD que pode ser usado para autenticar as chamadas para os serviços.   

<span id="manage-keys" />

### <a name="manage-keys-for-an-azure-ad-application"></a>Gerenciar chaves para um aplicativo Azure AD

Se o aplicativo Azure AD ler e gravar dados no Microsoft Azure AD, ele precisará de uma chave. Você pode criar chaves para um aplicativo do Azure AD por meio da edição de suas informações no Partner Center. Você também pode remover as chaves que não são mais necessárias.

1.  Dos **os usuários** página (sob **configurações de conta**), selecione o nome do aplicativo do Azure AD.
    > [!TIP]
    > Quando você clica no nome do aplicativo do Azure AD, você verá todas as chaves do Active Directory para o aplicativo do Azure AD, incluindo a data em que a chave foi criada e quando ele expirará. Para remover uma chave que não é mais necessária, clique em **Remover**.

2.  Para adicionar uma nova chave, selecione **adicionar nova chave**.
3.  Você verá uma tela mostrando os valores **ID do Cliente** e **Chave**.
    > [!IMPORTANT]
    > Não se esqueça de imprimir ou copiar essas informações, pois você não poderá acessá-lo novamente depois que você sair desta página.

4.  Se você quiser criar mais chaves, selecione **adicionar outra chave**.

<span id="edit" />

## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar um usuário, grupo ou aplicativo Azure AD

Depois de adicionar os usuários, grupos e/ou aplicativos do Azure AD para sua conta no Partner Center, você pode fazer alterações às suas informações de conta. 

> [!IMPORTANT]
> As alterações feitas [funções ou permissões](set-custom-permissions-for-account-users.md) afetará apenas o acesso do Partner Center. Todas as outras alterações (como alterar um nome de usuário ou a associação de grupo, ou a URL de resposta e o URI da ID do aplicativo para um aplicativo do Azure AD) serão refletidas no locatário de AD do Azure da sua organização, bem como em sua conta no Partner Center. 

1.  Dos **os usuários** página (sob **configurações de conta**), selecione o nome do usuário, grupo ou conta de aplicativo do Azure AD que você deseja editar.
2.  Faça as alterações desejadas. Os itens que você pode editar são:
    -   Para um **usuário**, você pode editar o nome, o sobrenome ou o nome de usuário do usuário. Você também pode selecionar ou desmarcar grupos na seção **Associação de grupo** para atualizar a participação no grupo.
    -   Para um **grupo**, você pode editar o nome do grupo. (para atualizar a associação de grupo, os usuários que você deseja adicionar ou remover do grupo e fazer alterações para editar a seção **Membros do grupo**.)
    -   Para um **aplicativo Azure AD**, você pode inserir novos valores para a **URL de resposta** ou **URI da ID do aplicativo**.
    Lembre-se de que essas alterações serão feitas no diretório da sua organização, bem como em sua conta no Partner Center.
3.  Para que as alterações relacionadas ao acesso do Partner Center, marque ou desmarque as funções que você deseja aplicar, ou selecione **personalizar permissões** e faça as alterações desejadas. Essas alterações afetam apenas o Partner Center acessar e não alterará as permissões no locatário do Azure AD da sua organização.
3.  Clique em **Salvar**.


## <a name="view-history-for-account-users"></a>Exibir histórico dos usuários da conta

Como proprietário da conta, você pode exibir o histórico de navegação detalhado de qualquer usuário que você adicionou à conta.

No **os usuários** página (sob **configurações de conta**), selecione o link mostrado sob **última atividade** para o usuário cujo histórico de navegação que você gostaria de examinar. Você poderá exibir os URLs de todas as páginas que o usuário visitou nos últimos 30 dias.

<span id="remove" />

## <a name="remove-users-groups-and-azure-ad-applications"></a>Remover usuários, grupos e aplicativos Azure AD

Para remover um usuário, grupo ou aplicativo do Azure AD da sua conta no Partner Center, selecione a **remova** link que aparece por nome na **usuários** página. Depois de confirmar que você deseja removê-lo, esse usuário, grupo ou aplicativo do Azure AD não será capaz de acessar sua conta do Partner Center (a menos que você adicioná-lo novamente mais tarde).

> [!IMPORTANT]
> Remover um usuário, grupo ou aplicativo do Azure AD significa que ele não terá mais acesso à sua conta do Partner Center. Isso **não** exclui o usuário, grupo ou aplicativo Azure AD do diretório da organização.

 

