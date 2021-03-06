---
Description: Você pode usar pacotes de pré-lançamento para distribuir pacotes que são dados apenas para um grupo de teste limitado.
title: Pacotes de pré-lançamento
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.date: 03/07/2019
ms.topic: article
keywords: windows 10, uwp, liberação de versões de pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: abd4b26d0943bf032f93f62719959ef6650e0d65
ms.sourcegitcommit: 978df7dfd3813de51609b6a44aedcd402083a5fd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/11/2019
ms.locfileid: "66826141"
---
# <a name="package-flights"></a>Pacotes de pré-lançamento

Você pode usar o pacote voos para distribuir pacotes específicos a um grupo limitado de testadores. Os pacotes que já publicados para a Store serão usados para os clientes, então não será afetada se sua experiência.

Com o pacote de voos, apenas os pacotes são diferentes; o detalhes da listagem de Store será o mesmo para todos os seus clientes. Qualquer pessoa no seu grupo de voo receberá os pacotes que você incluir em voo pacote, enquanto os clientes que não estão no grupo de voo continuam recebendo seus pacotes regulares (não vazados).  Se você decidir posteriormente que deseja disponibilizar esses pacotes de um voo de pacote para todos os seus clientes, você pode facilmente use os mesmos pacotes em um envio não vazados. Observe que o pacote voos devem passar o [processo de certificação](the-app-certification-process.md), da mesma forma que qualquer envio.

Quando você configura os voos de pacote, você pode especificar as pessoas que devem receber pacotes específicos adicionando-os para um **grupo de usuários conhecidos** (também conhecido como grupo de voo). Qualquer pessoa em um grupo de versão de pré-lançamento que estiver usando um dispositivo executando uma versão do Windows 10 que ofereça suporte a pacotes de pré-lançamento (build Windows.Desktop 10586 ou posterior; build Windows.Mobile 10586.63 ou posterior; ou Xbox One) receberá os pacotes dos pacote de pré-lançamentos que você atribuir a esse grupo em particular. (Voos seu pacote podem incluir os pacotes direcionados a qualquer versão do sistema operacional, incluindo o Windows 8.1 / Windows Phone 8.1 ou anterior, se seu aplicativo publicado anteriormente já oferece suporte a eles). Qualquer pessoa que não tenha sido adicionada a um de seus grupos de versão de pré-lançamento ou que esteja usando um dispositivo que não oferece suporte a pacotes de pré-lançamento receberá pacotes do envio de versão completa.

> [!IMPORTANT] 
> Em dispositivos desktop e móveis, as pessoas nos grupos de versão de pré-lançamento receberão os pacotes no pré-lançamento automaticamente sempre que você fornecer atualizações. No entanto, **pessoas nos grupos de versão de pré-lançamento que estiverem usando dispositivos Xbox precisarão verificar atualizações manualmente** para receber os pacotes mais recentes, certificando-se de que eles estejam conectados ao dispositivo usando a conta da Microsoft (com o endereço de email associado incluído no grupo de usuários conhecidos).

Observe que pacotes de pré-lançamento não serão distribuídos pela [Microsoft Store para Empresas](https://businessstore.microsoft.com/store) e pela [Microsoft Store para Educação](https://educationstore.microsoft.com/store). Isso ocorre porque as pessoas nos grupos de usuários conhecidos devem estar conectadas usando as contas da Microsoft para receber um pacote de pré-lançamento. Todas as aquisições feitas por meio da Microsoft Store para Empresas ou Microsoft Store para Educação receberão os pacotes de versão completa.

> [!TIP]
> Os pacotes de pré-lançamento oferecem pacotes apenas aos clientes selecionados que você especificar. Para distribuir pacotes para uma seleção aleatória de clientes em uma porcentagem especificada, você pode usar uma [distribuição de pacote gradual](gradual-package-rollout.md). Você também pode combinar distribuição com seus pacotes de pré-lançamento se quiser distribuir gradualmente uma atualização para um de seus grupos de versão de pré-lançamento.
>
> Diferente dos pacotes de pré-lançamento, as seleções de distribuição de pacote gradual se aplicam aos clientes que adquirem o aplicativo por meio da Microsoft Store para Empresas e da Microsoft Store para Educação. 

> [!TIP]
> Considere como as pessoas em seu pacote de pré-lançamento poderão apresentar a opinião delas sobre o aplicativo. Sugerimos [adicionar um controle ao seu aplicativo para iniciar o Hub de Feedback](../monetize/launch-feedback-hub-from-your-app.md), para que os clientes possam dar sua opinião diretamente. Assim, você pode analisar os comentários no [relatório de feedback](feedback-report.md)) do aplicativo.


## <a name="create-a-new-package-flight"></a>Criar um novo pacote de pré-lançamento

Depois de publicar um envio para seu aplicativo, você verá a seção **Pacotes de pré-lançamento** na página de visão geral do aplicativo. Clique em **Novo pacote de pré-lançamento** para começar.

Se ainda não criou quaisquer grupos de versões de usuários conhecidos, você será solicitado a criar um antes de continuar. Para obter mais informações, consulte [Criar um novo grupo usuários conhecido](create-known-user-groups.md). Você pode criar um novo grupo de usuários conhecido diretamente a partir desta página ao selecionar **Criar um grupo de versão de pré-lançamento**.

Na página de criação do pacote de pré-lançamento, é necessário inserir um nome para a versão de pré-lançamento e especificar pelo menos um grupo de versão de pré-lançamento. Ao concluir, selecione **Criar versão de pré-lançamento**. Você não poderá alterar esses detalhes posteriormente (embora se você não estiver satisfeito quando as informações inseridas, é possível excluir e criar um novo pacote de pré-lançamento).

> [!NOTE]
> Se tiver mais de um pacote de pré-lançamento, você precisará atribuir uma classificação para cada um deles. Para obter mais informações, consulte [voos de classificação adicional e adicionar pacote](#add-and-rank-additional-package-flights) abaixo.


## <a name="specify-packages-to-include-in-your-package-flight"></a>Especificar pacotes para incluir em seu pacote de pré-lançamento

Depois que tiver salvado os detalhes do pacote de pré-lançamento, você verá a página de visão geral. Clique em **Pacotes** para especificar os pacotes que você gostaria de incluir no versão de pré-lançamento. Você pode incluir os pacotes direcionados a qualquer versão do sistema operacional que dá suporte a seu aplicativo.

Você tem a opção de selecionar pacotes que estavam associados a um envio publicado anterior (um envio de versão completa ou um dos seus pacotes de pré-lançamento, se você tiver mais de um). Se você precisar carregar novos pacotes para usar para este voo de pacote, você poderá carregá-los aqui (usando o [mesmo processo que quando você carrega pacotes de aplicativos para um envio regular não vazados](upload-app-packages.md)). Clique em **Salvar** quando terminar de especificar os pacotes a serem incluídos no pacote de pré-lançamento.

Se seu aplicativo der suporte a várias famílias de dispositivos, verifique se que você incluiu pacotes para dar suporte ao mesmo conjunto de famílias de dispositivos na sua versão de pré-lançamento. As pessoas em seus grupos de versão de pré-lançamento **só** poderão obter pacotes dessa versão. Elas não poderão acessar nenhum pacote de outras versões de pré-lançamento nem de seu envio de versão completa. 

Lembre-se também que sua Store informações sobre a listagem e disponibilidade de família do dispositivo é baseada no seu envio não vazados. Os clientes nos grupos de versão de pré-lançamento só poderão baixar o aplicativo em uma família de dispositivos que seja compatível com o envio de versão completa. Para obter mais informações, consulte [Suporte à família de dispositivos](#device-family-support). 


## <a name="gradual-package-rollout"></a>Distribuição de pacote gradual

Por padrão, os pacotes no envio serão disponibilizados para todos no grupo de versão de pré-lançamento ao mesmo tempo. Para alterar isso, você pode marcar a caixa que diz **Roll out update gradually after this submission is published (to Windows 10 customers only)** . Você pode escolher uma porcentagem de pessoas no grupo de versão de pré-lançamento para receber os pacotes do envio novo, de maneira que possa monitorar comentários e dados analíticos para se certificar da atualização antes de implantá-la mais amplamente no resto do grupo de versão de pré-lançamento. Você pode aumentar a porcentagem (ou parar a atualização) sempre sem precisar criar um novo envio para o pacote de pré-lançamento. 

> [!IMPORTANT]
> Ao distribuir gradualmente os pacotes em um voo de pacote, as pessoas que não estão incluídas na porcentagem que obtém seus novos pacotes obterá os pacotes do envio de voo de pacote anterior (a menos que haja um voo com classificação mais alta disponível para eles).

Para obter mais informações, consulte [Distribuição gradual de pacote](gradual-package-rollout.md).


## <a name="configure-additional-package-flight-options"></a>Configurar opções adicionais de pacote de pré-lançamento

Por padrão, seu pacote de pré-lançamento será publicado e disponibilizado para o seu grupo de versões de pré-lançamento assim que passar pelo processo de certificação. Se você quiser alterar o [data de publicação](set-app-pricing-and-availability.md#publish-date), você pode fazer isso na **Flight opções** seção. Clique em **Salvar** para retornar à página de visão geral do pacote de pré-lançamento. 


## <a name="submit-your-package-flight-to-the-store"></a>Enviar seu pacote de pré-lançamento para a Loja

Quando tiver especificado os pacotes e configurado todas as opções necessárias, clique em **Enviar para a Loja**. Seu pacote de pré-lançamento passará, então, pelo [processo de certificação de aplicativo](the-app-certification-process.md). Observe que os pacotes incluídos no seu voo de pacote devem ser compatíveis com o [Microsoft Store políticas](store-policies.md), assim como acontece com todos os envios.

As pessoas em seus grupos de versão de pré-lançamento associados a esse pacote de pré-lançamento que já possuem seu aplicativo receberão agora uma atualização usando os pacotes incluídos em seu pacote de pré-lançamento. Se essas pessoas ainda não tiverem seu aplicativo, elas receberão os pacotes de seu pacote de pré-lançamento quando o instalarem. 

> [!NOTE]
> As pessoas que têm um pacote que está disponível somente em um pacote de pré-lançamento podem dar uma classificação por estrelas ao aplicativo e deixar opiniões, mas as classificações e avaliações delas não serão exibidas para outros clientes. (Isso exclui 7.x herdados ou os pacotes XAP 8.0; classificações e revisões deixado por membros dos grupos de voo usando esses pacotes serão visíveis para outros clientes). Você pode ver as classificações e comentários de todos os clientes, inclusive aqueles em seus grupos de voo, o **revisões** e **comentários** relatórios para o aplicativo.


## <a name="device-family-support"></a>Suporte à família de dispositivos

Na maioria dos casos, convém incluir pacotes que dão suporte ao mesmo conjunto de famílias de dispositivos compatíveis com o envio de versão completa. A disponibilidade da família de dispositivos para um aplicativo sempre se baseará no envio de versão completa, o cliente estando ou não em um grupo de versão de pré-lançamento.

**Se seu envio de versão completa der suporte a uma família de dispositivos incompatível com o seu pacote de pré-lançamento**, as pessoas em seu grupo de versão de pré-lançamento não poderão baixar o aplicativo nessa família de dispositivos. Por exemplo, se seu envio de versão completa incluir pacotes Móveis e de Desktop e, em seguida, você criar um pacote de pré-lançamento que inclua apenas um pacote Móvel, as pessoas no seu grupo de versão de pré-lançamento só poderão baixar o aplicativo em dispositivos móveis, mesmo que você tenha um pacote de desktop disponível para os clientes que não estão na versão de pré-lançamento. Mesmo se você estiver usando apenas o pacote de versão de pré-lançamento para testar alterações em seu pacote móvel, você deve incluir o pacote de desktop do seu envio de versão completa no pacote de pré-lançamento para que os clientes do grupo de versão de pré-lançamento possam baixar seu aplicativo em dispositivos desktop.

**Se seu pacote de pré-lançamento der suporte a uma família de dispositivos incompatível com seu envio de versão completa**, ninguém conseguirá baixar o aplicativo nessa família de dispositivos, estejam ou não em seu grupo de versão de pré-lançamento. Por exemplo, se o seu envio de versão completa incluir apenas um pacote móvel e, em seguida, você criar um pacote de pré-lançamento que inclua pacotes móveis e desktop, as pessoas em seu grupo de versão de pré-lançamento ainda só poderão baixar o aplicativo em dispositivos móveis. O pacote de desktop não será oferecido a ninguém, nem mesmo às pessoas em seu grupo de versão de pré-lançamento. Se você quiser disponibilizar um pacote de desktop para pessoas em seu grupo de versão de pré-lançamento, você precisará primeiro atualizar seu envio de versão completa para incluir um pacote de desktop. Para proporcionar a melhor experiência para todos os clientes do seu aplicativo, seu envio de versão completa deve dar suporte às mesma famílias de dispositivos que o seu pacote de pré-lançamento. 

> [!NOTE]
> Os pacotes adicionados ao seus pacotes de pré-lançamento podem dar suporte a qualquer versão de sistema operacional (ou qualquer compilação do Windows 10), mas como observado acima, as pessoas em grupos de versão de pré-lançamento devem usar um dispositivo que esteja executando uma versão do Windows 10 que dê suporte a pacotes de pré-lançamento (Windows.Desktop compilação 10586 ou posterior; Windows.Mobile compilação 10586.63 ou posterior) para obter pacotes do pacote de pré-lançamento.


## <a name="update-or-modify-your-package-flight"></a>Atualizar ou modificar seu pacote de pré-lançamento

Para criar um novo envio por um voo de pacote já publicado, clique em **atualização** próximo ao nome do voo em sua página de visão geral do aplicativo. Em seguida, você pode carregar novos pacotes (e remover os desnecessários), assim como faria com um envio de versão completa. Faça as alterações necessárias e, depois, clique em **Enviar para a Loja**, para enviar o pacote de pré-lançamento atualizado para o [processo de certificação de aplicativo](the-app-certification-process.md).

Para modificar uma versão de pré-lançamento existente sem criar e enviar uma nova atualização, clique em **Modificar**, perto do nome da versão de pré-lançamento. Isso permite que você altere detalhes como os grupos, o nome e a classificação da versão de pré-lançamento, sem precisar que o pacote de pré-lançamento passe pelo processo de certificação novamente. Observe que, se você tiver uma atualização em andamento, ou se o voo de pacote não foi publicado ainda, você não verá os **modificar** opção. 


## <a name="add-and-rank-additional-package-flights"></a>Adicionar e classificar pacotes de pré-lançamento adicionais

Você pode criar diversos pacotes de pré-lançamento para o mesmo aplicativo, a fim de distribuir vários pacotes diferentes para grupos de clientes diversificados. 

Assim que tiver criado seu primeiro pacote de pré-lançamento, você cria outro seguindo o processo descrito acima. A única diferença é que, se você já criou um pacote de pré-lançamento, precisará especificar a ordem de prioridade de todos os pacotes de pré-lançamento na seção **Classificação**. Isso permite que o Store determinar qual pacote para dar a qualquer cliente individual, se eles estiverem em mais de um dos seus grupos de voo. As pessoas em seus grupos de versão de pré-lançamento sempre receberão o pacote de pré-lançamento mais bem classificado disponível para elas, mesmo que um pacote de pré-lançamento com classificação inferior contenha pacotes com um número de versão maior.

Por padrão, seu novo pacote de pré-lançamento será melhor classificado. Se você quiser alterar sua classificação, mova-o para baixo (ou faça backup) para colocá-lo no local correto entre seus outros pacotes de pré-lançamento.

Observe que o seu envio não vazados sempre classificados do mais baixo (1). Ou seja, as pessoas que não estiverem em nenhum de seus grupos de versão de pré-lançamento podem receber apenas pacotes de seu envio de versão completa por meio da Loja. As pessoas em um grupo de voo sempre obterá pacotes de voo com maior classificação pacote disponível para eles (mas nunca o envio não vazados, já que ele tem a classificação mais baixa). Isso proporciona flexibilidade para determinar como distribuir seus pacotes para as pessoas que podem ser membros de mais de um de seus grupos de versão de pré-lançamento.

Por exemplo, digamos que você deseja criar dois pacote de pré-lançamento além de seu envio de versão completa normal: aquele que é relativamente estável e está pronto para teste com um público amplo, e aquele do qual você não está seguro e deseja limitar a apenas poucos testadores. Você poderia criar um grupo de versão de pré-lançamento chamado Testador e incluí-lo em um pacote de pré-lançamento chamado Versão de Pré-lançamento de Testador, em seguida, criar um grupo de versão de pré-lançamento chamado Entusiastas com uma associação maior e incluí-lo em outro pacote de pré-lançamento chamado Versão de Pré-lançamento de Entusiasta. Se você classificar a Versão de Pré-lançamento de Testador mais alto do que a Versão de Pré-lançamento de Entusiasta, será possível usar pacotes nos quais você confia totalmente na Versão de Pré-lançamento de Entusiasta, enquanto usa pacotes mais arriscados destinados apenas aos Testadores na Versão de Pré-lançamento de Testador. Os membros do seu grupo Testadores sempre receberão os pacotes que você fornecer na Versão de Pré-lançamento de Testador, mesmo se eles pertencerem ao grupo Entusiastas. (E mais tarde, se for concluído que os pacotes na Versão de Pré-lançamento de Testador estiverem se saindo bem, você poderia atualizar a Versão de Pré-lançamento de Entusiasta para usar os pacotes originalmente distribuídos para a Versão de Pré-lançamento — e talvez finalmente usar esses pacotes em seu envio de versão completa).


## <a name="make-packages-from-a-package-flight-available-to-all-your-customers"></a>Disponibilizar os pacotes de um pacote de pré-lançamento para todos os seus clientes

Se você decidir que um ou mais dos pacotes incluídos em um pacote de pré-lançamento publicado deve ser disponibilizado para os clientes que não estão em um grupo de versão de pré-lançamento, você pode atualizar seu envio de versão completa para usar esses pacotes, sem ter que carregar os mesmos pacotes novamente. 

Quando você criar seu novo envio na página [**Pacotes**](upload-app-packages.md), verá um menu suspenso com a opção de copiar pacotes de um de seus pacotes de pré-lançamento. Selecione o pacote de pré-lançamento que tiver os pacotes que você deseja puxar. Em seguida, você pode selecionar qualquer um ou todos os pacotes para incluir no envio de versão completa.

Observe que todas as mesmas regras de validação do pacote serão aplicadas, mesmo durante o uso de pacotes de um envio publicado anteriormente. 


## <a name="delete-a-package-flight"></a>Excluir um pacote de pré-lançamento

Para excluir um pacote de pré-lançamento ao qual você não quer mais dar suporte, clique no nome dele na página de visão geral do aplicativo. Na página de visão geral da versão de pré-lançamento, clique em **Modificar**, em seguida, clique no link **Excluir** para excluir o pacote de pré-lançamento. (Se você tiver um envio não publicado do voo de pacote em andamento, você precisará excluir esse envio pela primeira vez.) Ele pode levar até 30 minutos para que isso seja concluída.

Quando você exclui um pacote de pré-lançamento, todos os clientes que tiverem pacotes distribuídos nesse pacote de pré-lançamento receberão uma atualização de aplicativo, caso haja um pacote com um número de versão maior (ou assim que esse pacote ficar disponível). Caso eles desinstalem o aplicativo e o instalem novamente, isso será tratado como uma nova aquisição e eles receberão a versão superior disponível no momento. 
