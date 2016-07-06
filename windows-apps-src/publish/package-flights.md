---
author: jnHs
Description: "Se seu aplicativo usa um AdMediatorControl ou AdControl para exibir anúncios na barra de notificação, você pode aumentar sua taxa de preenchimento de anúncio e receita mostrando anúncios afiliados da Microsoft em seu aplicativo."
title: "Pacotes de pré-lançamento"
ms.assetid: 5B094822-A8DE-4EE3-B55D-3E306C04EE79
ms.sourcegitcommit: 9e62a7aa18950f7e1cc26b42762e3bb937c389ac
ms.openlocfilehash: c538da2a58f38925938b9e28ec7ca65cdb9858a3

---

# Pacotes de pré-lançamento

Você pode usar pacotes de pré-lançamento para distribuir pacotes que são dados apenas para um grupo de teste limitado. 

Os pacotes de pré-lançamento permitem que você forneça diferentes pacotes ao conjunto designado de testadores sem prejudicar a experiência de seus outros clientes. Apenas os pacotes são diferentes; o armazenamento de listagem detalhes será o mesmo para todos os seus clientes.

Os pacotes de pré-lançamento devem passar pelo [processo de certificação](the-app-certification-process.md), exatamente como um envio de versão completa normal. Se, mais tarde, você decidir que quer tornar pacotes de um pacote de pré-lançamento disponíveis para todos seus clientes, pode puxar esses pacotes para seu envio de versão completa como descrito abaixo.

Quando você configura pacotes de pré-lançamento, pode escolher as pessoas específicas que devem receber pacotes específicos, adicionando-as a um grupo de versão de pré-lançamento. Qualquer pessoa em um grupo de versão de pré-lançamento que estiver usando um dispositivo executando uma versão do Windows 10 que ofereça suporte a pacotes de pré-lançamento (build Windows.Desktop 10586 ou posterior; build Windows.Mobile 10586.63 ou posterior) receberá pacotes dos pacote de pré-lançamentos que você atribuir a esse grupo em particular. Qualquer pessoa que não tenha sido adicionada a um de seus grupos de versão de pré-lançamento ou que esteja usando um dispositivo que não oferece suporte a pacotes de pré-lançamento receberá pacotes do envio de versão completa.

Depois de publicar um envio para seu aplicativo, você verá a seção **Pacotes de pré-lançamento** na página de visão geral do aplicativo. Clique em **Novo pacote de pré-lançamento** para começar. Se ainda não tiver configurado grupos de versões de pré-lançamento, você será solicitado a criar um antes de continuar.

## Criar um novo grupo de versões de pré-lançamento

Os grupos de versões de pré-lançamento permitem que você especifique as pessoas que gostaria de incluir no grupo. Para obter os pacotes lançados, cada pessoa deve ser autenticada na Loja usando uma conta da Microsoft associada ao endereço de email fornecido por você e usar um dispositivo Windows 10 (como especificado acima) para baixar o aplicativo.

Ao criar um grupo de versões de pré-lançamento, você deve fornecer um nome. Cada grupo de versões de pré-lançamento deve conter pelo menos um endereço de email, com no máximo 10.000 endereços de email. Você pode inserir endereços de email diretamente no campo de (separados por espaços, vírgulas ou ponto-e-vírgula) ou pode clicar no link **Importar CSV** para criar o grupo de versões de pré-lançamento em uma lista de endereços de email em um arquivo .csv.

Clique em **Criar grupo** para salvar o grupo e continuar configurando o pacote de pré-lançamento.

> **Importante** Certifique-se de que você obteve o consentimento que for necessário das pessoas que acrescentar ao grupo de envio de versão de pré-lançamento e que elas entenderam que receberão pacotes diferentes do seu envio de versão completa. 
> Também seria bom levar em consideração como as pessoas em seu pacote de pré-lançamento podem lhe dar a opinião delas sobre o aplicativo. Sugerimos [adicionar um controle ao seu aplicativo para iniciar o Hub de Feedback](../monetize/launch-feedback-hub-from-your-app.md), para que os clientes possam dar sua opinião diretamente. Assim, você pode analisar os comentários no [relatório de feedback](feedback-report.md)) do aplicativo.

Para editar o grupo de versão de pré-lançamento mais tarde, você pode clicar em **Exportar .csv** para salvar suas informações de grupo em um arquivo .csv. Faça suas alterações nesse arquivo, clique em **Importar .csv** para usar a nova versão para atualizar a associação do grupo. Observe que pode levar até 30 minutos para que as alterações na associação do grupo de versão de pré-lançamento sejam implementadas. Se você adicionar pessoas a um grupo de versão de pré-lançamento depois de publicar um pacote de pré-lançamento associado, os pacotes serão enviados para o novo pessoal automaticamente; você não precisa criar e publicar um novo envio para esse pacote de pré-lançamento. 

## Criar um novo pacote de pré-lançamento

Após criar seu primeiro grupo de versão de pré-lançamento, você verá uma página em que pode acrescentar detalhes para completar a configuração. Você precisará nomear o pacote de pré-lançamento e especificar pelo menos um grupo de versão de pré-lançamento. Se quiser configurar um novo grupo, você pode fazer isso nesta página.

Clique em **Criar versão de pré-lançamento** depois de digitar o nome e selecionar os grupos de versões de pré-lançamento. Você não poderá alterar esses detalhes posteriormente (embora nada impeça de excluir e criar um novo pacote de pré-lançamento para usar em vez disso).

> Observação  Se você tiver mais de um pacote de pré-lançamento, será necessário atribuir uma classificação para cada um. Para obter mais informações, consulte Adicionar e classificar pacotes de pré-lançamento adicionais abaixo.

## Especificar pacotes para incluir em seu pacote de pré-lançamento

Depois que tiver salvado os detalhes do pacote de pré-lançamento, você verá a página de visão geral. Clique em **Pacotes** para especificar os pacotes que você gostaria de incluir no versão de pré-lançamento.

Você tem a opção de selecionar pacotes que estavam associados a um envio publicado anterior (um envio de versão completa ou um dos seus pacotes de pré-lançamento, se você tiver mais de um). Caso precise [carregar novos pacotes](upload-app-packages.md) para usar para esse pacote de pré-lançamento, pode carregá-los aqui (usando o mesmo processo de quando carrega pacotes de aplicativo para um envio de versão completa comum). Clique em **Salvar** quando terminar de especificar os pacotes a serem incluídos no pacote de pré-lançamento.

Se seu aplicativo der suporte a várias famílias de dispositivos, verifique se que você incluiu pacotes para dar suporte ao mesmo conjunto de famílias de dispositivos na sua versão de pré-lançamento. As pessoas em seus grupos de versão de pré-lançamento **só** poderão obter pacotes dessa versão. Elas não poderão acessar nenhum pacote de outras versões de pré-lançamento nem de seu envio de versão completa. 

Lembre-se também de que suas informações de listagem da Loja vêm de seu envio de versão completa, incluindo quais famílias de dispositivos são compatíveis com seu aplicativo. os clientes em seus grupos de versão de pré-lançamento só poderão baixar o aplicativo em uma família de dispositivos que seja compatível com seu envio de versão completa. Para obter mais informações, consulte [Suporte à família de dispositivos](#device-family-support). 

## Configurar opções adicionais de pacote de pré-lançamento

Por padrão, seu pacote de pré-lançamento será publicado e disponibilizado para o seu grupo de versões de pré-lançamento assim que passar pelo processo de certificação. Se você quiser alterar a [data de publicação](set-app-pricing-and-availability.md#publish-date) ou quiser adicionar [notas para certificação](notes-for-certification.md), faça isso na seção **Opções**. Clique em **Salvar** para retornar à página de visão geral do pacote de pré-lançamento. 

## Enviar seu pacote de pré-lançamento para a Loja

Quando tiver especificado os pacotes e configurado todas as opções necessárias, clique em **Enviar para a Loja**. Seu pacote de pré-lançamento passará, então, pelo [processo de certificação de aplicativo](the-app-certification-process.md). Os pacotes incluídos no pacote de pré-lançamento devem estar de acordo com as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx), assim como todos os envios.

As pessoas em seus grupos de versão de pré-lançamento associados a esse pacote de pré-lançamento que já possuem seu aplicativo receberão agora uma atualização usando os pacotes incluídos em seu pacote de pré-lançamento. Se essas pessoas ainda não tiverem seu aplicativo, elas receberão os pacotes de seu pacote de pré-lançamento quando o instalarem. 

> Observação  As pessoas que tiverem um pacote que está disponível apenas em um pacote de pré-lançamento podem dar ao aplicativo uma classificação por estrelas e deixar opiniões, mas elas não serão exibidas para outros clientes. (Isso exclui o legado 7.x e pacotes XAP 8.0. Classificações e opiniões deixadas por membros de seus grupos de versão de pré-lançamento usando esses pacotes ficarão visíveis para outros clientes). Você pode ver os comentários de todos os clientes, incluindo aqueles em seus grupos de versão de pré-lançamento, nos relatórios de Classificações e opiniões do aplicativo.

## Suporte à família de dispositivos

Na maioria dos casos, convém incluir pacotes que dão suporte ao mesmo conjunto de famílias de dispositivos compatíveis com o envio de versão completa. A disponibilidade da família de dispositivos para um aplicativo sempre se baseará no envio de versão completa, o cliente estando ou não em um grupo de versão de pré-lançamento.

**Se seu envio de versão completa der suporte a uma família de dispositivos incompatível com o seu pacote de pré-lançamento**, as pessoas em seu grupo de versão de pré-lançamento não poderão baixar o aplicativo nessa família de dispositivos. Por exemplo, se seu envio de versão completa incluir pacotes Móveis e de Desktop e, em seguida, você criar um pacote de pré-lançamento que inclua apenas um pacote Móvel, as pessoas no seu grupo de versão de pré-lançamento só poderão baixar o aplicativo em dispositivos móveis, mesmo que você tenha um pacote de desktop disponível para os clientes que não estão na versão de pré-lançamento. Mesmo se você estiver usando apenas o pacote de versão de pré-lançamento para testar alterações em seu pacote móvel, você deve incluir o pacote de desktop do seu envio de versão completa no pacote de pré-lançamento para que os clientes do grupo de versão de pré-lançamento possam baixar seu aplicativo em dispositivos desktop.

**Se seu pacote de pré-lançamento der suporte a uma família de dispositivos incompatível com seu envio de versão completa**, ninguém conseguirá baixar o aplicativo nessa família de dispositivos, estejam ou não em seu grupo de versão de pré-lançamento. Por exemplo, se o seu envio de versão completa incluir apenas um pacote móvel e, em seguida, você criar um pacote de pré-lançamento que inclua pacotes móveis e desktop, as pessoas em seu grupo de versão de pré-lançamento ainda só poderão baixar o aplicativo em dispositivos móveis. O pacote de desktop não será oferecido a ninguém, nem mesmo às pessoas em seu grupo de versão de pré-lançamento. Se você quiser disponibilizar um pacote de desktop para pessoas em seu grupo de versão de pré-lançamento, você precisará primeiro atualizar seu envio de versão completa para incluir um pacote de desktop. Para proporcionar a melhor experiência para todos os clientes do seu aplicativo, seu envio de versão completa deve dar suporte às mesma famílias de dispositivos que o seu pacote de pré-lançamento. 

**Observação**  Os pacotes adicionados ao seus pacotes de pré-lançamento podem dar suporte a qualquer versão de sistema operacional (ou qualquer compilação do Windows 10), mas como observado acima, as pessoas em grupos de versão de pré-lançamento devem usar um dispositivo que esteja executando uma versão do Windows 10 que dê suporte a pacotes de pré-lançamento (Windows.Desktop compilação 10586 ou posterior; Windows.Mobile compilação 10586.63 ou posterior) para obter pacotes do pacote de pré-lançamento.

## Atualizar ou modificar seu pacote de pré-lançamento

Para criar um novo envio para um pacote de pré-lançamento existente, clique em **Atualizar**, perto do nome da versão de pré-lançamento, na página de visão geral do aplicativo. Em seguida, você pode carregar novos pacotes (e remover os desnecessários), assim como faria com um envio de versão completa. Faça as alterações necessárias e, depois, clique em **Enviar para a Loja**, para enviar o pacote de pré-lançamento atualizado para o [processo de certificação de aplicativo](the-app-certification-process.md).

Para modificar uma versão de pré-lançamento existente sem criar e enviar uma nova atualização, clique em **Modificar**, perto do nome da versão de pré-lançamento. Isso permite que você altere detalhes como os grupos, o nome e a classificação da versão de pré-lançamento, sem precisar que o pacote de pré-lançamento passe pelo processo de certificação novamente.

## Adicionar e classificar pacotes de pré-lançamento adicionais

Você pode criar diversos pacotes de pré-lançamento para o mesmo aplicativo, a fim de distribuir vários pacotes diferentes para grupos de clientes diversificados. 

Assim que tiver criado seu primeiro pacote de pré-lançamento, você cria outro seguindo o processo descrito acima. A única diferença é que, se você já criou um pacote de pré-lançamento, precisará especificar a ordem de prioridade de todos os pacotes de pré-lançamento na seção **Classificação**. Isso permite que a Loja determine qual pacote entregar a qualquer cliente individual, mesmo que eles estejam em mais de um de seus grupos de versão de pré-lançamento. As pessoas em seus grupos de versão de pré-lançamento sempre receberão o pacote de pré-lançamento mais bem classificado disponível para elas, mesmo que um pacote de pré-lançamento com classificação inferior contenha pacotes com um número de versão maior.

Por padrão, seu novo pacote de pré-lançamento será melhor classificado. Se você quiser alterar sua classificação, mova-o para baixo (ou faça backup) para colocá-lo no local correto entre seus outros pacotes de pré-lançamento.

O envio de versão completa é sempre classificado como o mais baixo. Ou seja, as pessoas que não estiverem em nenhum de seus grupos de versão de pré-lançamento podem receber apenas pacotes de seu envio de versão completa por meio da Loja. As pessoas em um grupo de versão de pré-lançamento sempre receberão pacotes do pacote de pré-lançamento melhor classificado disponível para elas (mas nunca do envio de versão completa). Isso proporciona flexibilidade para determinar como distribuir seus pacotes para as pessoas que podem ser membros de mais de um de seus grupos de versão de pré-lançamento.

Por exemplo, digamos que você deseja criar dois pacote de pré-lançamento além de seu envio de versão completa normal: aquele que é relativamente estável e está pronto para teste com um público amplo, e aquele do qual você não está seguro e deseja limitar a apenas poucos testadores. Você poderia criar um grupo de versão de pré-lançamento chamado Testador e incluí-lo em um pacote de pré-lançamento chamado Versão de Pré-lançamento de Testador, em seguida, criar um grupo de versão de pré-lançamento chamado Entusiastas com uma associação maior e incluí-lo em outro pacote de pré-lançamento chamado Versão de Pré-lançamento de Entusiasta. Se você classificar a Versão de Pré-lançamento de Testador mais alto do que a Versão de Pré-lançamento de Entusiasta, será possível usar pacotes nos quais você confia totalmente na Versão de Pré-lançamento de Entusiasta, enquanto usa pacotes mais arriscados destinados apenas aos Testadores na Versão de Pré-lançamento de Testador. Os membros do seu grupo Testadores sempre receberão os pacotes que você fornecer na Versão de Pré-lançamento de Testador, mesmo se eles pertencerem ao grupo Entusiastas. (E mais tarde, se for concluído que os pacotes na Versão de Pré-lançamento de Testador estiverem se saindo bem, você poderia atualizar a Versão de Pré-lançamento de Entusiasta para usar os pacotes originalmente distribuídos para a Versão de Pré-lançamento — e talvez finalmente usar esses pacotes em seu envio de versão completa).

## Disponibilizar os pacotes de um pacote de pré-lançamento para todos os seus clientes

Se você decidir que um ou mais dos pacotes incluídos em um pacote de pré-lançamento publicado deve ser disponibilizado para os clientes que não estão em um grupo de versão de pré-lançamento, você pode atualizar seu envio de versão completa para usar esses pacotes, sem ter que carregar os mesmos pacotes novamente. 

Quando você criar seu novo envio na página [**Pacotes**](upload-app-packages.md), verá um menu suspenso com a opção de copiar pacotes de um de seus pacotes de pré-lançamento. Selecione o pacote de pré-lançamento que tiver os pacotes que você deseja puxar. Em seguida, você pode selecionar qualquer um ou todos os pacotes para incluir no envio de versão completa.

Observe que todas as mesmas regras de validação do pacote serão aplicadas, mesmo durante o uso de pacotes de um envio publicado anteriormente. 

## Excluir um pacote de pré-lançamento

Para excluir um pacote de pré-lançamento ao qual você não quer mais dar suporte, clique no nome dele na página de visão geral do aplicativo. Na página de visão geral da versão de pré-lançamento, clique em **Modificar**, em seguida, clique no link **Excluir** para excluir o pacote de pré-lançamento. (Se você tiver um envio não publicado do pacote de pré-lançamento em andamento, você precisará exclui-lo primeiro). Isso pode levar até 30 minutos para ser concluído.

Quando você exclui um pacote de pré-lançamento, todos os clientes que tiverem pacotes distribuídos nesse pacote de pré-lançamento receberão uma atualização de aplicativo, caso haja um pacote com um número de versão maior (ou assim que esse pacote ficar disponível). Caso eles desinstalem o aplicativo e o instalem novamente, isso será tratado como uma nova aquisição e eles receberão a versão superior disponível no momento. 



<!--HONumber=Jun16_HO5-->


