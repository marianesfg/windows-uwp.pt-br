---
author: jnHs
Description: "Adicione os usuários à sua conta do Centro de Desenvolvimento e atribua a eles funções com permissões específicas."
title: "Gerenciar usuários de contas"
ms.assetid: 9245F0D0-7D8F-4741-AFB4-FBA5601D0A9B
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7eaddcfc2d02805e60043132328ef482872a9000
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-account-users"></a>Gerenciar usuários de contas


Você pode usar o Active Directory do Azure para adicionar usuários à sua conta do Centro de Desenvolvimento. Cada usuário recebe uma função que oferece um conjunto específico de permissões para a conta. Você também pode atribuir uma função a um grupo de usuários, ou a um aplicativo Azure AD.

> **Importante**  Para adicionar e gerenciar usuários da conta, você deve primeiro associar sua conta do Centro de Desenvolvimento ao Active Directory do Azure de sua organização. Isso exige que você entre no Azure AD com uma conta de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654). Depois de estabelecer essa associação, você não poderá removê-la sem contatar o suporte.

## <a name="associate-your-dev-center-account-with-your-organizations-azure-active-directory"></a>Associar sua conta do Centro de Desenvolvimento ao Active Directory do Azure de sua organização

O Centro de Desenvolvimento do Windows aproveita o Active Directory do Azure para o gerenciamento de vários usuários e a atribuição de funções. Se sua organização já usa o Office 365 ou outros serviços comerciais da Microsoft, você já tem Azure AD. Caso contrário, você pode criar um novo Azure AD no Centro de Desenvolvimento sem nenhum custo adicional.

Observe que apenas uma conta do Centro de Desenvolvimento pode ser associada a um Azure AD. Da mesma forma, apenas um Azure AD pode ser associado uma conta do Centro de Desenvolvimento.

> **Observação** Se os usuários que você deseja adicionar não fazem parte do Azure AD da organização e não deseja criar novas contas do Azure AD para eles, é possível [convidar usuários por email](#add-and-manage-account-users).

### <a name="associate-your-dev-center-account-with-your-organizations-existing-azure-ad"></a>Associar sua conta do Centro de Desenvolvimento ao Azure AD existente de sua organização

Se sua organização já usa o Azure AD, siga estas etapas para vincular sua conta do Centro de Desenvolvimento.

1.  Acesse suas **Configurações da conta** e clique em **Gerenciar usuários**.
2.  Clique no botão **Associar o Azure AD com a sua conta do Centro de Desenvolvimento**.
3.  Entre em sua conta do Azure AD. Essa conta deve ter permissões de [Administrador global](http://go.microsoft.com/fwlink/?LinkId=746654) para configurar a associação.
4.  Revise os nomes da organização e do domínio da conta do Azure AD. Para concluir a associação, clique em **Confirmar**.
5.  Se a associação for bem-sucedida, você estará pronto para adicionar e gerenciar usuários da conta na página **Gerenciar usuários** da sua conta, conforme descrito nas seções abaixo.

### <a name="create-a-brand-new-azure-ad-to-associate-with-your-dev-center-account"></a>Criar um novo Azure AD para associar à sua conta do Centro de Desenvolvimento

Se você precisa configurar um novo Azure AD para vincular com sua conta do Centro de Desenvolvimento, siga estas etapas.

1.    Acesse suas **Configurações da conta** e clique em **Gerenciar usuários**.
2.    Clique no botão **Criar novo Azure AD**.
3.    Insira as informações de diretório para seu novo Azure AD:
 - **Nome de domínio**: o nome exclusivo que usaremos para seu domínio do Azure AD, junto com ". onmicrosoft.com". Por exemplo, se você inseriu "exemplo", seu domínio do Azure AD seria "example.onmicrosoft.com".
 - **Email de contato**: um endereço de email onde possamos contatá-lo sobre a sua conta, se necessário.
 - **Informações de conta de usuário do administrador global**: o nome, sobrenome, nome de usuário e senha que você deseja usar para a nova conta de administrador.
4.    Clique em **Criar** para confirmar as novas informações de domínio e conta.
5.    Entre com o nome de usuário e senha do novo Azure AD para começar a adicionar e gerenciar usuários de contas adicionais na página **Gerenciar usuários** da sua conta, conforme descrito nas seções a seguir.


> **Importante**  Depois de associar sua conta do Centro de Desenvolvimento ao Azure AD, você sempre precisará entrar no Centro de Desenvolvimento usando a conta de administrador global do Azure AD (e não uma conta pessoal da Microsoft) para adicionar e gerenciar usuários de contas.

## <a name="add-and-manage-account-users-groups-and-azure-ad-applications"></a>Adicionar e gerenciar usuários da conta, grupos e aplicativos Azure AD

Depois de estabelecer a associação, você pode adicionar usuários, grupos e aplicativos Azure AD à sua conta. Pode também alterar funções, editar detalhes da conta ou remover usuários.

> **Observação**  Se a sua organização usa a [integração de diretório](http://go.microsoft.com/fwlink/p/?LinkID=724033) para sincronizar o serviço de diretório local com o Azure AD, não será possível criar novos usuários, grupos nem aplicativos do Azure AD no Centro de Desenvolvimento. Você (ou outro admin em seu diretório local) precisará criá-los diretamente no diretório local antes de vê-los e adicioná-los no Centro de Desenvolvimento.

Ao gerenciar usuários, tenha o seguinte em mente:

-   Todos os usuários do Centro de Desenvolvimento devem ter uma conta ativa no Azure AD da organização.
-   A criação de um **novo** usuário ou grupo no Centro de Desenvolvimento também os adicionará ao Azure AD da organização.
-   Fazer alterações no nome de um usuário ou grupo no Centro de Desenvolvimento fará essas alterações no Azure AD de sua organização.
-   Os usuários (incluindo grupos e aplicativos Azure AD) serão capazes de acessar toda a conta do Centro de Desenvolvimento com as permissões associadas à função atribuída. Se você [personalizar permissões](set-custom-permissions-for-account-users.md), você pode limitar o acesso do usuário de forma que ele só possa trabalhar com aplicativos e/ou complementos específicos.
-   Você pode permitir que um usuário, grupo ou aplicativo Azure AD tenha acesso à funcionalidade de mais de uma função selecionando várias funções, ou usando [permissões personalizadas](set-custom-permissions-for-account-users.md) para conceder o acesso que você queira.
-   Um usuário com uma determinada função (ou conjunto de [permissões personalizadas](set-custom-permissions-for-account-users.md)) também pode fazer parte de um grupo que tenha uma função diferente (ou conjunto de permissões). Nesse caso, o usuário terá acesso a toda a funcionalidade associada ao grupo e à conta individual.

## <a name="roles-and-permissions"></a>Funções e permissões

Ao adicionar um usuário, grupo ou aplicativo Azure AD, você deve especificar as permissões deles. Você pode fazer isso por conceder-lhes uma **função padrão**, ou por [personalizar suas permissões](set-custom-permissions-for-account-users.md).

A menos que você use permissões personalizadas, cada usuário, grupo ou aplicativo Azure AD que você adicionar a uma conta deve receber pelo menos uma das funções padrão a seguir. Cada função tem um conjunto específico de permissões para executar determinadas funções na conta. 

> **Observação**  O proprietário da conta é a pessoa que a criou com uma conta da Microsoft (em vez de usuários adicionados por meio do Azure AD). Esse proprietário da conta é a única pessoa com acesso completo à conta, incluindo a capacidade de excluir aplicativos, criar e editar todos os usuários da conta e alterar todas as informações financeiras e configurações da conta. 

| Função                 | Descrição              |
|----------------------|--------------------------|
| Gerente              | Tem acesso completo à conta, exceto para alterar configurações de imposto e pagamento. Isso inclui o gerenciamento de usuários no Centro de Desenvolvimento, mas observe que a capacidade de criar e excluir usuários é dependente da permissão da conta no Azure AD. Ou seja, se um usuário receber a função Gerente, mas não tiver permissões de administrador no Azure AD da organização, ele não poderá criar novos usuários ou excluir usuários do diretório (mas poderá alterar a função do Centro de Desenvolvimento do usuário). |
| Desenvolvedor            | Pode carregar pacotes e enviar aplicativos e complementos, além de exibir o [Relatório de uso](usage-report.md) para obter detalhes de telemetria. Não pode exibir informações financeiras ou configurações da conta.   |
| Colaborador comercial | Pode exibir relatórios de [Integridade](health-report.md) e de [Uso](usage-report.md). Não é possível criar ou enviar produtos, alterar configurações de conta ou exibir informações financeiras.                                         |
| Colaborador financeiro  | Pode exibir [relatórios de pagamento](payout-summary.md), informações financeiras e relatórios de aquisição. Não pode fazer alterações em aplicativos, complementos ou configurações da conta.                                                                                                                                   |
| Comerciante             | Pode [responder às avaliações dos clientes](respond-to-customer-reviews.md) e exibir [relatórios analíticos](analytics.md) não financeiros. Não pode fazer alterações em aplicativos, complementos ou configurações da conta.      |

A tabela a seguir mostra alguns dos recursos específicos disponíveis para cada uma dessas funções (e para o proprietário da conta).

|                                 |    Proprietário da conta                 |    Gerente                       |    Desenvolvedor                     |    Colaborador comercial    |    Colaborador financeiro    |    Comerciante                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Relatório de aquisição           |    Pode exibir                      |    Pode exibir                      |     Sem acesso                    |     Sem acesso              |    Pode exibir               |    Sem acesso                     |
|    Relatório/respostas de comentários    |    Pode exibir e enviar comentários    |    Pode exibir e enviar comentários    |    Pode exibir e enviar comentários    |     Sem acesso              |     Sem acesso             |    Pode exibir e enviar comentários    |
|    Relatório de integridade                |    Pode exibir                      |    Pode exibir                      |    Pode exibir                      |    Pode exibir                |     Sem acesso             |    Sem acesso                     |
|    Relatório de uso                 |    Pode exibir                      |    Pode exibir                      |    Pode exibir                      |    Pode exibir                |     Sem acesso             |    Sem acesso                     |
|    Conta de pagamento               |    Pode atualizar                    |    Sem acesso                     |    Sem acesso                     |    Sem acesso               |    Pode exibir               |    Sem acesso                     |
|    Perfil de imposto                  |    Pode atualizar                    |    Sem acesso                     |    Sem acesso                     |    Sem acesso               |    Pode exibir               |    Sem acesso                     |
|    Resumo de pagamentos               |    Pode exibir                      |    Sem acesso                     |    Sem acesso                     |    Sem acesso               |    Pode exibir               |    Sem acesso                     |

Se nenhuma das funções padrão forem apropriadas, ou se você deseja limitar o acesso a aplicativos e/ou complementos específicos, você pode conceder permissões personalizadas ao usuário clicando em **Personalizar permissões**. Para obter mais informações, consulte [Definir permissões personalizadas para usuários de contas](set-custom-permissions-for-account-users.md).

## <a name="add-and-manage-account-users"></a>Adicionar e gerenciar usuários da conta

Para identificar os usuários que você deseja adicionar à sua conta do Centro de Desenvolvimento e atribuir a eles uma função, clique em **Adicionar usuários**.

Você pode adicionar um ou mais usuários do diretório da organização à sua conta do Centro de Desenvolvimento. Observe que, quando você adiciona mais de um usuário ao mesmo tempo, deve atribuir a mesma função. Se desejar adicionar usuários, mas atribuir a eles funções diferentes, repita as etapas abaixo para cada função.

Como alternativa, você pode convidar os usuários a acessarem sua conta sem adicioná-los ao diretório da organização.

**Adicionar usuários do diretório de sua organização**

1.  Na página **Gerenciar usuários**, clique em **Adicionar usuários**.
2.  Selecione um ou mais usuários na lista exibida. Você pode usar a caixa de pesquisa para procurar usuários específicos.
3.  Quando tiver terminado de selecionar usuários, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, selecione uma ou mais funções para atribuir a esse conjunto de usuários.
5.  Clique em **Salvar**.

Se desejar conceder acesso ao Centro de Desenvolvimento a uma nova conta de usuário, você poderá criar uma nova conta na seção **Gerenciar usuários**. 

Por padrão, o botão de opção **Adicionar ao Azure AD** é selecionado. Deixar essa opção selecionada criará uma nova conta no diretório de sua organização e adicionará esse usuário à sua conta do Centro de Desenvolvimento. Se você não quiser criar novas contas no diretório da organização, mas deseja permitir que os usuários acessem sua conta usando as contas da Microsoft deles para fazer logon, selecione **Convidar usuários por email**.

Se quiser que o novo usuário tenha uma [Conta de administrador global](http://go.microsoft.com/fwlink/p/?LinkId=746654) no diretório da organização, marque a caixa rotulada **Tornar esse usuário um Administrador Global no Azure AD, com controle total sobre todos os recursos de diretório**. Isso dará ao usuário acesso completo a todos os recursos administrativos no Azure AD de sua empresa. Eles poderão adicionar e gerenciar usuários no diretório da organização (mas não no Centro de Desenvolvimento, a menos que você conceda à conta as [funções/permissões](#roles-and-permissions) adequadas. Se você marcar essa caixa, você precisará fornecer um **Email de recuperação de senha** para o usuário.

**Criar uma nova conta de usuário no Centro de Desenvolvimento e no diretório da organização**

1.  Na página **Gerenciar usuários**, clique em **Adicionar usuários**.
2.  Na próxima página, clique em **Novo usuário**.
3.  Certifique-se de que o botão de opção **Adicionar ao Azure AD** seja selecionado.
4.  Insira o nome, o sobrenome e o nome de usuário do novo usuário.
5.  Insira um email que o usuário possa usar se precisar recuperar a senha. Isso só será necessário se você tiver marcado a caixa **Tornar esse usuário um administrador global no Azure AD**.
6.  Na seção **Funções**, selecione uma ou mais funções para atribuir ao novo usuário, ou atribua permissões personalizadas.
7.  Na seção **Associação de grupo**, selecione qualquer grupo ao qual o novo usuário deve pertencer.
8.  Clique em **Salvar**.
9.  Na página de confirmação, você verá as informações de logon do novo usuário, incluindo uma senha temporária. Certifique-se de anotar essas informações e fornecê-las ao novo usuário, já que você não poderá acessar a senha temporária depois que sair dessa página.

**Criar uma nova conta de usuário no Centro de Desenvolvimento sem adicionar o usuário ao diretório da organização**

1.  Na página **Gerenciar usuários**, clique em **Adicionar usuários**.
2.  Na próxima página, clique em **Novo usuário**.
3.  Selecione o botão de opção **Convidar usuários por email**.
3.  Insira um ou mais endereços de email (até dez), separados por vírgulas ou ponto e vírgulas.
4.  Na seção **Funções**, selecione uma ou mais funções para atribuir ao novo usuário, ou atribua permissões personalizadas.
6.  Clique em **Salvar**.

Os usuários que você convidou receberão um email com um convite para acessar sua conta do Centro de Desenvolvimento. Cada usuário precisará aceitar o convite antes que possa acessar sua conta. Para reenviar o convite, localize o usuário em sua página **Gerenciar usuários** e clique no endereço de email dele (ou no texto que diz **Convite pendente**) para editar a conta. Em seguida, na parte inferior da página, clique em **Reenviar convite**.

Você pode fazer alterações nas contas de usuários que tiver adicionado à sua conta do Centro de Desenvolvimento na seção **Gerenciar usuários**. Observe que as alterações no nome do usuário ou na associação a um grupo serão refletidas no diretório da organização, não apenas em sua conta do Centro de desenvolvimento. As alterações feitas na função de um usuário só afetarão o acesso ao Centro de Desenvolvimento.

**Editar uma conta de usuário**

1.  Na página **Gerenciar usuários**, clique no nome da conta de usuário que deseja editar.
2.  Faça qualquer uma das seguintes alterações:
    -   Edite o nome, o sobrenome ou o nome de usuário do usuário. Lembre-se de que essas alterações serão feitas no diretório da organização.
    -   Na seção **Funções**, marque ou desmarque as funções que deseja adicionar ou remover para o usuário, ou atribua permissões personalizadas.
    -   Na seção **Associação de grupo**, marque ou desmarque os grupos em que o usuário deve ingressar ou ser removido. Lembre-se de que essas alterações serão feitas no diretório da organização.

3.  Clique em **Salvar**.

Se você precisar alterar a senha para uma conta de usuário que adicionou à sua conta do Centro de Desenvolvimento, isso pode ser feito na seção **Gerenciar usuários**. Observe que isso mudará a senha do diretório do usuário, não apenas a senha de acesso ao Centro de Desenvolvimento.

**Alterando a senha do diretório de um usuário**

Caso tenha fornecido um **Email de recuperação de senha** ao criar a conta de usuário, eles poderão restaurar sua própria senha. Você também pode atualizar a senha do usuário seguindo as etapas abaixo.

1.  Na página **Gerenciar usuários**, clique no nome da conta de usuário que deseja editar.
2.  Clique no botão **Redefinir senha** na parte inferior da página.
3.  Uma página de confirmação será exibida mostrando as informações de logon do usuário, incluindo uma senha temporária.

   > **Importante**  Certifique-se de imprimir ou copiar essas informações e fornecê-las ao usuário, já que você não conseguirá acessar a senha temporária depois de sair dessa página.

## <a name="add-and-manage-groups"></a>Adicionar e gerenciar grupos

Quando você adicionar um grupo do diretório da organização à sua conta do Centro de Desenvolvimento, cada usuário que for um membro desse grupo poderá acessá-lo, com as permissões associadas à função atribuída do grupo. Tenha em mente que todas as alterações feitas em grupos (incluindo seu nome ou sua associação) serão refletidas no diretório da organização.

Observe que, quando você adiciona mais de um grupo ao mesmo tempo, deve atribuir a mesma função. Se você deseja adicionar grupos, mas atribuir a eles funções diferentes, repita as etapas abaixo para cada função.

**Adicionar grupos do diretório de sua organização**

1.  Na página **Gerenciar usuários**, clique em **Adicionar grupos**.
2.  Selecione um ou mais grupos na lista exibida. Você pode usar a caixa de pesquisa para procurar grupos específicos.
3.  Quando tiver terminado de selecionar grupos, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, selecione uma ou mais funções para atribuir a esse conjunto de grupos, ou atribua permissões personalizadas.
5.  Clique em **Salvar**.

Se desejar conceder acesso ao Centro de Desenvolvimento a um novo grupo, você poderá criar um novo grupo na seção **Gerenciar usuários**. Observe que o novo grupo será criado no diretório da organização, não apenas em sua conta do Centro de Desenvolvimento.

**Criar uma nova conta de grupo**

1.  Na página **Gerenciar usuários**, clique em **Adicionar grupos**.
2.  Na próxima página, clique em **Novo grupo**.
3.  Insira o nome de exibição para o novo grupo.
4.  Selecione uma ou mais funções para atribuir ao novo grupo, ou atribua permissões personalizadas. Todos os membros do grupo poderão acessar sua conta do Centro de Desenvolvimento com as permissões associadas à função
5.  Selecione um ou mais usuários na lista exibida. Você pode usar a caixa de pesquisa para procurar usuários específicos.
6.  Quando tiver terminado de selecionar usuários, clique em **Adicionar selecionado**.
7.  Clique em **Salvar**.

Você pode fazer alterações nas contas de grupos que tiver adicionado à sua conta do Centro de Desenvolvimento na seção **Gerenciar usuários**. Observe que as alterações no nome do grupo e na associação serão refletidas no diretório da organização, não apenas em sua conta do Centro de desenvolvimento. As alterações feitas na função de um grupo afetará apenas o acesso ao Centro de Desenvolvimento do grupo.

**Editar uma conta de grupo**

1.  Na página **Gerenciar usuários**, clique no nome da conta de grupo que deseja editar.
2.  Para alterar informações do grupo, faça as alterações desejadas no nome do grupo. Lembre-se de que essas alterações serão feitas no diretório da organização.
3.  Para alterar a função do grupo, marque ou desmarque as funções que deseja aplicar ao grupo, ou atribua permissões personalizadas.
4.  Clique em **Salvar**.

## <a name="add-and-manage-azure-ad-applications"></a>Adicionar e gerenciar aplicativos Azure AD

Você pode permitir que os aplicativos ou os serviços que fazem parte do Azure AD da organização acessem a conta do Centro de Desenvolvimento.

Observe que, quando você adiciona mais de um aplicativo Azure AD ao mesmo tempo, deve atribuir a mesma função. Se você deseja adicionar grupos, mas atribuir a eles funções diferentes, repita as etapas abaixo para cada função.

**Adicionar aplicativos Azure AD do diretório da organização**

1.  Na página **Gerenciar usuários**, clique em **Adicionar aplicativos Azure AD**.
2.  Selecione um ou mais aplicativos Azure AD na lista exibida. Você pode usar a caixa de pesquisa para procurar aplicativos Azure AD específicos.
3.  Quando tiver terminado de selecionar aplicativos Azure AD, clique em **Adicionar selecionado**.
4.  Na seção **Funções**, selecione uma ou mais funções para atribuir a esse conjunto de aplicativos Azure AD, ou atribua permissões personalizadas.
5.  Clique em **Salvar**.

Se você deseja conceder acesso ao Centro de Desenvolvimento para uma nova conta do aplicativo Azure AD, pode criar uma na seção **Gerenciar usuários**. Observe que isso criará uma nova conta no diretório da organização, não apenas na conta do Centro de Desenvolvimento.

> **Dica** Se você usa esse aplicativo do Azure AD principalmente para autenticação do Centro de Desenvolvimento e não precisa que os usuários o acessem diretamente, insira qualquer endereço válido para **URL de resposta** e **URI da ID do Aplicativo**, contanto que esses valores não sejam usados por nenhum outro aplicativo do Azure AD no seu diretório.

**Criar um novo aplicativo Azure AD**

1.  Na página **Gerenciar usuários**, clique em **Adicionar aplicativos Azure AD**.
2.  Na próxima página, clique em **Novo aplicativo Azure AD**.
3.  Insira a **URL de Resposta** para o novo aplicativo Azure AD. Essa é a URL onde os usuários podem entrar e usar seu aplicativo Azure AD (às vezes, também conhecida como a URL do Aplicativo ou a URL de Logon). A **URL de Resposta** não pode ter mais de 256 caracteres.
4.  Insira o **URI da ID do Aplicativo** para o novo aplicativo Azure AD. Ele é um identificador lógico do aplicativo Azure AD apresentado quando ele envia uma solicitação de logon único para o Azure AD. Observe que o **URI da ID do Aplicativo** deve ser exclusivo para cada aplicativo Azure AD no diretório, e não pode ter mais de 256 caracteres.
5.  Na seção **Funções**, selecione uma ou mais funções para atribuir ao novo aplicativo Azure AD, ou atribua permissões personalizadas.
6.  Clique em **Salvar**.

Depois de adicionar ou criar um aplicativo do Azure AD, você pode retornar para a seção **Gerenciar usuários** e clicar no nome do aplicativo para examinar as configurações do aplicativo, incluindo a ID de locatário, ID do cliente, URL de resposta e URI da ID do aplicativo.

> **Observação** Se pretender usar as APIs REST fornecidas pelos [serviços do Windows Store](../monetize/using-windows-store-services.md), você precisará dos valores de ID de locatário e ID do cliente mostrados nesta página para obter um token de acesso do Azure AD que pode ser usado para autenticar as chamadas para os serviços.   

Você pode fazer alterações nos aplicativos Azure AD adicionados à conta do Centro de Desenvolvimento na seção **Gerenciar usuários**. Observe que as alterações na URL de Resposta e no URI da ID do Aplicativo serão refletidas no diretório da organização, não apenas na conta do Centro de Desenvolvimento. As alterações de função afetarão apenas as permissões do aplicativo Azure AD no Centro de Desenvolvimento.

**Editar um aplicativo Azure AD**

1.  Na página **Gerenciar usuários**, clique no nome da conta do aplicativo Azure AD que deseja editar.
2.  Para alterar a **URL de Resposta** ou o **URI da ID do Aplicativo**, insira os novos valores aqui. Lembre-se de que essas alterações serão feitas no diretório da organização.
3.  Para alterar a função do aplicativo Azure AD, marque ou desmarque as funções que deseja aplicar, ou atribua permissões personalizadas.
4.  Clique em **Salvar**.

Se o aplicativo Azure AD ler e gravar dados no Microsoft Azure AD, ele precisará de uma chave. Você pode criar chaves para um aplicativo Azure AD ao editar suas informações no Centro de Desenvolvimento. Você também pode remover as chaves que não são mais necessárias.

**Gerenciar chaves para um aplicativo Azure AD**

1.  Na página **Gerenciar usuários**, clique no nome do aplicativo Azure AD.

    > **Dica**  Ao clicar no nome do aplicativo Azure AD, você verá todas as suas chaves ativas, incluindo a data de criação e de expiração da chave. Para remover uma chave que não é mais necessária, clique em **Remover**.

2.  Para adicionar uma nova chave, clique em **Adicionar nova chave**.

3.  Você verá uma tela mostrando os valores **ID do Cliente** e **Chave**.

    > **Importante**  Certifique-se de imprimir ou copiar essas informações, já que você não conseguirá acessá-la novamente depois que sair dessa página.

4.  Se quiser criar mais chaves, clique em **Adicionar outra chave**.

## <a name="view-history-for-account-users"></a>Exibir histórico dos usuários da conta

Como um proprietário da conta, você pode exibir o histórico de navegação detalhado de quaisquer usuários adicionais adicionados à conta.

Na página **Gerenciar usuários**, clique no link mostrado em **Última atividade** do usuário cujo histórico de navegação você deseja revisar. Você poderá exibir os URLs de todas as páginas que o usuário visitou nos últimos 30 dias.

## <a name="removing-users-groups-and-azure-ad-applications"></a>Removendo usuários, grupos e apps Azure AD

Para remover um usuário, um grupo ou um aplicativo Azure AD de sua conta do Centro de Desenvolvimento, clique no link **Remover** que aparece ao lado do nome na página **Gerenciar usuários**. Depois de confirmar que deseja removê-la, esse usuário, grupo ou aplicativo do Azure AD não poderá mais acessar sua conta do Centro de Desenvolvimento (a menos que você a adicione novamente mais tarde).

> **Observação**  Remover um usuário, um grupo ou um aplicativo Azure AD significa que ele não terá mais acesso à sua conta do Centro de Desenvolvimento. Isso não exclui o usuário, grupo ou aplicativo Azure AD do diretório da organização.

 

 

 
