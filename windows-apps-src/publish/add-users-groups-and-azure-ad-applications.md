---
author: jnHs
Description: "Você pode adicionar usuários, grupos e aplicativos do Azure AD à sua conta do Centro de Desenvolvimento."
title: "Adicionar usuários, grupos e aplicativos do Azure AD à sua conta do Centro de Desenvolvimento"
ms.author: wdg-dev-content
ms.date: 07/17/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32f62d6022d075e71ce6bcc15e1603a2e11a68a5
ms.sourcegitcommit: 23cda44f10059bcaef38ae73fd1d7c8b8330c95e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/19/2017
---
# <a name="add-users-groups-and-azure-ad-applications-to-your-dev-center-account"></a>Adicionar usuários, grupos e aplicativos do Azure AD à sua conta do Centro de Desenvolvimento

A seção [Gerenciar usuários](manage-account-users.md) do Centro de Desenvolvimento permite que você use o Azure Active Directory para adicionar usuários à sua conta do Centro de Desenvolvimento. Cada usuário recebe uma função (ou conjunto de permissões personalizadas) que define o acesso à conta. Você pode adicionar [grupos de usuários](#groups) e [aplicativos Azure AD](#azure-ad-applications) para conceder acesso à conta do Centro de Desenvolvimento.

Depois que os usuários são adicionados à conta, você pode [editar detalhes da conta](#edit), alterar [funções e permissões](set-custom-permissions-for-account-users.md) ou [remover usuários](#remove).

> [!IMPORTANT]
> Para adicionar usuários à sua conta, você deve primeiro [associar a conta do Centro de Desenvolvimento ao locatário do Azure Active Directory da organização](associate-azure-ad-with-dev-center.md). 

Ao adicionar usuários, lembre-se do seguinte. (se aplicam a grupos e aplicativos Azure AD, bem como os usuários individuais.)

-   Todos os usuários do Centro de Desenvolvimento devem ter uma conta ativa no diretório da organização. Criar um novo usuário no Centro de Desenvolvimento também cria uma conta de usuário no locatário do Azure AD da sua organização.
-   [Fazer alterações](#edit) no nome de um usuário no Centro de Desenvolvimento fará essas alterações no locatário do Azure AD da organização.
-   Ao adicionar usuários, você deverá especificar o acesso à conta do Centro de Desenvolvimento, atribuindo uma [função ou um conjunto de permissões personalizados](set-custom-permissions-for-account-users.md). 

> [!NOTE]
> Se a sua organização usa a [integração de diretório](http://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar o serviço de diretório local com o Azure AD, não será possível criar novos usuários, grupos ou aplicativos do Azure AD no Centro de Desenvolvimento. Você (ou outro admin em seu diretório local) precisará criá-los diretamente no diretório local antes de vê-los e adicioná-los no Centro de Desenvolvimento.


<span id="users" />
## <a name="add-users-to-your-dev-center-account"></a>Adicionar usuários à sua conta do Centro de Desenvolvimento

Você pode adicionar usuários individuais à sua conta do Centro de Desenvolvimento em qualquer uma das seguintes maneiras:
-   Adicionar usuários que já existem no diretório da organização
-   Criar um novo usuário para adicionar ao diretório e à conta do Centro de Desenvolvimento da organização
-   Adicione os usuários existentes que não estão no diretório da organização


<span id="from-directory" />
### <a name="add-users-from-your-organizations-directory"></a>Adicionar usuários do diretório de sua organização

1.  Na página **Gerenciar usuários**, clique em **Adicionar usuários**.
2.  Selecione um ou mais usuários na lista exibida. Você pode usar a caixa de pesquisa para procurar usuários específicos.
    > [!TIP]
    > Se você selecionar mais de um usuário para adicionar à sua conta do Centro de Desenvolvimento, você deve atribuir a eles a mesma função ou conjunto de permissões personalizados. Para adicionar vários usuários com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

3.  Quando tiver terminado de selecionar usuários, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os usuários selecionados.
5.  Clique em **Salvar**.


<span id="new-user" />
### <a name="create-a-new-user-account-in-your-organizations-directory-and-add-them-to-your-dev-center-account"></a>Crie uma nova conta de usuário no diretório de sua organização e adicione-a à conta do Centro de Desenvolvimento.

Se você deseja conceder acesso ao Centro de Desenvolvimento para uma nova conta no locatário do Azure AD, é possível criar uma seção **Gerenciar usuários** ao clicar em **Novo usuário**. 

> [!IMPORTANT]
> Você deve estar conectado com uma conta de administrador global em seu locatário Azure AD para adicionar novos usuários ao diretório.

Se quiser que o novo usuário tenha uma [Conta de administrador global](http://go.microsoft.com/fwlink/p/?LinkId=746654) no diretório da organização, marque a caixa rotulada **Tornar esse usuário um Administrador Global no Azure AD, com controle total sobre todos os recursos de diretório**. Isso dará ao usuário acesso completo a todos os recursos administrativos no Azure AD de sua empresa. Eles poderão adicionar e gerenciar usuários no diretório da organização (embora não no Centro de Desenvolvimento, a menos que você conceda à conta as [funções/permissões](set-custom-permissions-for-account-users.md) adequadas). Se você marcar essa caixa, você precisará fornecer um **Email de recuperação de senha** para o usuário.

1.  Na página **Gerenciar usuários**, clique em **Adicionar usuários**.
2.  Na próxima página, clique em **Novo usuário**.
3.  Certifique-se de que o botão de opção **Adicionar ao Azure AD** é selecionado para criar uma nova conta no diretório de sua organização e adicionará esse usuário à sua conta do Centro de Desenvolvimento. 
4.  Insira o nome, o sobrenome e o nome de usuário do novo usuário.
5.  Insira um email que o usuário possa usar se precisar recuperar a senha. Isso só será necessário se você tiver marcado a caixa **Tornar esse usuário um administrador global no Azure AD**.
6.  Na seção **Associação de grupo**, selecione qualquer grupo ao qual o novo usuário deve pertencer.
7.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
8.  Clique em **Salvar**.
9.  Na página de confirmação, você verá as informações de logon do novo usuário, incluindo uma senha temporária. Certifique-se de anotar essas informações e fornecê-las ao novo usuário, já que você não poderá acessar a senha temporária depois que sair dessa página.


<span id="email" />
### <a name="add-a-user-from-outside-of-your-azure-ad-tenant-to-your-dev-center-account-and-your-directory"></a>Adicionar um usuário fora do locatário Azure AD à sua conta do Centro de Desenvolvimento e seu diretório

Para convidar usuários que não sejam parte do seu locatário do Azure AD à sua conta, siga as etapas abaixo. Os usuários receberão um convite por email para participar da conta e uma nova conta de [Usuário convidado](https://docs.microsoft.com/azure/active-directory/active-directory-b2b-what-is-azure-ad-b2b) será criada para eles em seu locatário Azure AD. 

1.  Na página **Gerenciar usuários**, clique em **Adicionar usuários**.
2.  Na próxima página, clique em **Novo usuário**.
3.  Selecione o botão de opção **Convidar usuários por email**.
3.  Insira um ou mais endereços de email (até dez), separados por vírgulas ou ponto e vírgulas.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o usuário.
6.  Clique em **Salvar**.

Os usuários que você convidou receberão um email com um convite para acessar sua conta do Centro de Desenvolvimento. Cada usuário precisará aceitar o convite antes que possa acessar sua conta.

Se você precisar enviar um convite novamente, localize o usuário em sua página **Gerenciar usuários** e selecione o endereço de email dele (ou no texto que diz **Convite pendente**) para editar a conta. Em seguida, na parte inferior da página, clique em **Reenviar convite**.


### <a name="changing-a-users-directory-password"></a>Alterando a senha do diretório de um usuário

Se você precisar alterar a senha para uma conta de usuário que adicionou à sua conta do Centro de Desenvolvimento, isso pode ser feito na seção **Gerenciar usuários**. Observe que isso altera a senha do usuário no locatário Azure AD, junto com a senha que eles usam para acessar o Centro de Desenvolvimento. 

Caso tenha fornecido um **Email de recuperação de senha** ao criar a conta de usuário, eles poderão restaurar sua própria senha. Você também pode atualizar a senha do usuário seguindo as etapas abaixo.

1.  Na página **Gerenciar usuários**, clique no nome da conta de usuário que deseja editar.
2.  Clique no botão **Redefinir senha** na parte inferior da página.
3.  Uma página de confirmação será exibida mostrando as informações de logon do usuário, incluindo uma senha temporária.
    > [!IMPORTANT]
    >  Certifique-se de imprimir ou copiar essas informações e fornecê-las ao usuário, já que você não conseguirá acessar a senha temporária depois que sair dessa página.

<span id="groups" />
## <a name="add-groups-to-your-dev-center-account"></a>Adicionar grupos à sua conta do Centro de Desenvolvimento

Você pode adicionar um grupo do diretório da organização à sua conta do Centro de Desenvolvimento. Quando você fizer isso, cada usuário que é um membro desse grupo poderá acessá-lo com as permissões associadas à função atribuída do grupo.

### <a name="add-groups-from-your-organizations-directory"></a>Adicionar grupos do diretório de sua organização

1.  Na página **Gerenciar usuários**, clique em **Adicionar grupos**.
2.  Selecione um ou mais grupos na lista exibida. Você pode usar a caixa de pesquisa para procurar grupos específicos.
    > [!TIP]
    > Se você selecionar mais de um grupo para adicionar à sua conta do Centro de Desenvolvimento, você deve atribuir a eles a mesma função ou conjunto de permissões personalizados. Para adicionar vários grupos com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

3.  Quando tiver terminado de selecionar grupos, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os grupos selecionados. Todos os membros do grupo poderão acessar sua conta do Centro de Desenvolvimento com as permissões que você aplica ao grupo, independentemente das funções/permissões associado à sua conta individual.
5.  Clique em **Salvar**.


### <a name="create-a-new-group-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Crie uma nova conta de grupo no diretório de sua organização e adicione-o à conta do Centro de Desenvolvimento.

Se desejar conceder acesso ao Centro de Desenvolvimento a um novo grupo, você poderá criar um novo grupo na seção **Gerenciar usuários**. Observe que o novo grupo será criado no diretório da organização, não apenas em sua conta do Centro de Desenvolvimento.

1.  Na página **Gerenciar usuários**, clique em **Adicionar grupos**.
2.  Na próxima página, clique em **Novo grupo**.
3.  Insira o nome de exibição para o novo grupo.
4.  Especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o grupo. Todos os membros do grupo poderão acessar sua conta do Centro de Desenvolvimento com as permissões que você aplica ao grupo, independentemente das funções/permissões associado à sua conta individual.
5.  Selecione os usuários para atribuir ao novo grupo na lista que aparece. Você pode usar a caixa de pesquisa para procurar usuários específicos.
6.  Quando terminar de selecionar usuários, clique em **Adicionar selecionado** para adicioná-los ao novo grupo.
7.  Clique em **Salvar**.


<span id="azure-ad-applications" />
## <a name="add-azure-ad-applications-to-your-dev-center-account"></a>Adicionar aplicativos Azure AD à sua conta do Centro de Desenvolvimento

Você pode permitir que os aplicativos ou os serviços que fazem parte do Azure AD da organização acessem a conta do Centro de Desenvolvimento.

### <a name="add-azure-ad-applications-from-your-organizations-directory"></a>Adicionar aplicativos Azure AD do diretório da organização

1.  Na página **Gerenciar usuários**, clique em **Adicionar aplicativos Azure AD**.
2.  Selecione um ou mais aplicativos Azure AD na lista exibida. Você pode usar a caixa de pesquisa para procurar aplicativos Azure AD específicos.
    > [!TIP]
    > Se você selecionar mais de um aplicativo Azure AD para adicionar à sua conta do Centro de Desenvolvimento, você deve atribuir a eles a mesma função ou conjunto de permissões personalizados. Para adicionar vários aplicativos Azure AD com permissões/funções diferentes, repita as etapas abaixo para cada função ou um conjunto de permissões personalizados.

3.  Quando tiver terminado de selecionar aplicativos Azure AD, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para os aplicativos Azure AD selecionados.
5.  Clique em **Salvar**.


### <a name="create-a-new-azure-ad-application-account-in-your-organizations-directory-and-add-it-to-your-dev-center-account"></a>Crie uma nova conta de aplicativo Azure AD no diretório de sua organização e adicione-o à conta do Centro de Desenvolvimento.

Se você deseja conceder acesso ao Centro de Desenvolvimento para uma nova conta do aplicativo Azure AD, pode criar uma na seção **Gerenciar usuários**. Observe que isso criará uma nova conta no diretório da organização, não apenas na conta do Centro de Desenvolvimento.

> [!TIP]
> Se você usa esse aplicativo Azure AD principalmente para autenticação do Centro de Desenvolvimento e não precisa que os usuários o acessem diretamente, insira qualquer endereço válido para a **URL de resposta** e **URI da ID do Aplicativo**, contanto que esses valores não sejam usados por nenhum outro aplicativo do Azure AD no seu diretório.

1.  Na página **Gerenciar usuários**, clique em **Adicionar aplicativos Azure AD**.
2.  Na próxima página, clique em **Novo aplicativo Azure AD**.
3.  Insira a **URL de Resposta** para o novo aplicativo Azure AD. Essa é a URL onde os usuários podem entrar e usar seu aplicativo Azure AD (às vezes, também conhecida como a URL do Aplicativo ou a URL de Logon). A **URL de Resposta** não pode ter mais de 256 caracteres.
4.  Insira o **URI da ID do Aplicativo** para o novo aplicativo Azure AD. Ele é um identificador lógico do aplicativo Azure AD apresentado quando ele envia uma solicitação de logon único para o Azure AD. Observe que o **URI da ID do Aplicativo** deve ser exclusivo para cada aplicativo Azure AD no diretório, e não pode ter mais de 256 caracteres.
5.  Na seção **Funções**, especifique [funções ou permissões personalizadas](set-custom-permissions-for-account-users.md) para o aplicativo Azure AD.
6.  Clique em **Salvar**.

Depois de adicionar ou criar um aplicativo do Azure AD, você pode retornar para a seção **Gerenciar usuários** e clicar no nome do aplicativo para examinar as configurações do aplicativo, incluindo a ID de locatário, ID do cliente, URL de resposta e URI da ID do aplicativo.

> [!NOTE]
> Se você pretende usar as APIs REST fornecidas pelos [serviços da Windows Store](../monetize/using-windows-store-services.md), você precisa dos valores de ID de locatário e ID do cliente mostrados nesta página para obter um token de acesso do Azure AD que pode ser usado para autenticar as chamadas para os serviços.   

<span id="manage-keys" />
### <a name="manage-keys-for-an-azure-ad-application"></a>Gerenciar chaves para um aplicativo Azure AD

Se o aplicativo Azure AD ler e gravar dados no Microsoft Azure AD, ele precisará de uma chave. Você pode criar chaves para um aplicativo Azure AD ao editar suas informações no Centro de Desenvolvimento. Você também pode remover as chaves que não são mais necessárias.

1.  Na página **Gerenciar usuários**, clique no nome do aplicativo Azure AD.
    > [!TIP]
    > Ao clicar no nome do aplicativo Azure AD, você verá todas as suas chaves ativas, incluindo a data de criação e de expiração da chave. Para remover uma chave que não é mais necessária, clique em **Remover**.

2.  Para adicionar uma nova chave, clique em **Adicionar nova chave**.
3.  Você verá uma tela mostrando os valores **ID do Cliente** e **Chave**.
    > [!IMPORTANT]
    > Certifique-se de imprimir ou copiar essas informações, já que você não conseguirá acessá-la novamente depois que sair dessa página.

4.  Se quiser criar mais chaves, clique em **Adicionar outra chave**.

<span id="edit" />
## <a name="edit-a-user-group-or-azure-ad-application"></a>Editar um usuário, grupo ou aplicativo Azure AD

Depois de adicionar usuários, grupos e/ou aplicativos Azure AD à conta do Centro de Desenvolvimento, é possível fazer alterações nas informações da conta. 

> [!IMPORTANT]
> As alterações feitas nas funções e permissões afetam somente o acesso ao Centro de Desenvolvimento. Todas as outras alterações (por exemplo, alterar o nome do usuário ou associação de grupo, ou a URL de resposta e URI da ID do aplicativo para um aplicativo Azure AD) serão refletidas no locatário do Azure AD do sua organização, bem como em sua conta do Centro de Desenvolvimento. 

1.  Na página **Gerenciar usuários**, clique no nome da conta do usuário, grupo ou aplicativo Azure AD que deseja editar.
2.  Faça as alterações desejadas. Os itens que você pode editar são:
    -   Para um **usuário**, você pode editar o nome, o sobrenome ou o nome de usuário do usuário. Você também pode selecionar ou desmarcar grupos na seção **Associação de grupo** para atualizar a participação no grupo.
    -   Para um **grupo**, você pode editar o nome do grupo. (para atualizar a associação de grupo, os usuários que você deseja adicionar ou remover do grupo e fazer alterações para editar a seção **Membros do grupo**.)
    -   Para um **aplicativo Azure AD**, você pode inserir novos valores para a **URL de resposta** ou **URI da ID do aplicativo**.
    Lembre-se de que essas alterações serão efetuadas no diretório da organização, bem como na conta do Centro de Desenvolvimento.
3.  Para alterações relacionadas ao acesso do Centro de Desenvolvimento, marque ou desmarque as funções que você deseja aplicar ou selecione **Personalizar permissões** e faça as alterações desejadas. Essas alterações afetam somente o acesso do Centro de Desenvolvimento e não mudará as permissões no locatário do Azure AD da sua organização.
3.  Clique em **Salvar**.


## <a name="view-history-for-account-users"></a>Exibir histórico dos usuários da conta

Como um proprietário da conta, você pode exibir o histórico de navegação detalhado de quaisquer usuários adicionais adicionados à conta.

Na página **Gerenciar usuários**, clique no link mostrado em **Última atividade** do usuário cujo histórico de navegação você deseja revisar. Você poderá exibir os URLs de todas as páginas que o usuário visitou nos últimos 30 dias.

<span id="remove" />
## <a name="remove-users-groups-and-azure-ad-applications"></a>Remover usuários, grupos e aplicativos Azure AD

Para remover um usuário, um grupo ou um aplicativo Azure AD de sua conta do Centro de Desenvolvimento, clique no link **Remover** que aparece ao lado do nome na página **Gerenciar usuários**. Depois de confirmar que deseja removê-la, esse usuário, grupo ou aplicativo do Azure AD não poderá mais acessar sua conta do Centro de Desenvolvimento (a menos que você a adicione novamente mais tarde).

> [!IMPORTANT] 
> Remover um usuário, um grupo ou um aplicativo Azure AD significa que ele não terá mais acesso à sua conta do Centro de Desenvolvimento. Isso **não** exclui o usuário, grupo ou aplicativo Azure AD do diretório da organização.

 

