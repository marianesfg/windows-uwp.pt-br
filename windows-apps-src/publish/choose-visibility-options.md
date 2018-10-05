---
author: jnHs
Description: Set restrictions on how your app can be discovered and acquired, including whether people can find your app in the Store or see its Store listing at all.
title: Escolher as opções de visibilidade
ms.author: wdg-dev-content
ms.date: 08/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, visibilidade, audiência particular, disponível, detectável
ms.localizationpriority: medium
ms.openlocfilehash: 07986353be41fcc9ef9dd9406fb0b30c4aa3d7f2
ms.sourcegitcommit: 63cef0a7805f1594984da4d4ff2f76894f12d942
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/05/2018
ms.locfileid: "4392612"
---
# <a name="choose-visibility-options"></a>Escolher as opções de visibilidade


A seção **Visibilidade** da [página Preço e disponibilidade](set-app-pricing-and-availability.md) permite que você defina restrições sobre como seu app pode ser descoberto e adquirido. Isso lhe dá a opção de especificar se as pessoas podem encontrar seu app na Store ou ver sua listagem da Store.

Há duas seções separadas dentro da seção Visibilidade: **Audiência** e **Detectabilidade**. 

## <a name="audience"></a>Audiência

A seção Audiência permite especificar se você deseja restringir a visibilidade de seu envio para uma audiência específica definida.


### <a name="public-audience"></a>Audiência pública

Por padrão, a listagem da Store do seu app estará visível para uma **Audiência pública**. Isso é adequado para a maioria dos envios, a menos que você queira limitar quem pode ver os detalhes do app a pessoas específicas. Você também pode usar as opções da seção [Detectabilidade](#discoverability) para restringir a detectabilidade, se quiser.

> [!IMPORTANT]
> Se você enviar um produto com essa opção definida como **Audiência pública**, não poderá escolher **Audiência particular** em um envio posterior.


### <a name="private-audience"></a>Audiência particular

Se você quiser que a listagem do app seja visível somente para as pessoas selecionadas especificadas, escolha **Audiência particular**. Com essa opção, o app não será detectável ou estará disponível para qualquer pessoa que não seja uma das pessoas nos grupos especificados. Essa opção é frequentemente usada para [testes beta](beta-testing-and-targeted-distribution.md), já que permite que você distribua seu app para os testadores sem que ninguém mais possa obter o app, ou até mesmo ver sua listagem da Store (mesmo que consigam digitar sua URL de listagem da Store).

Quando você escolher **Audiência particular**, será necessário especificar pelo menos um grupo de pessoas que deverá receber seu app. Você pode escolher entre um [grupo de usuários conhecidos](create-known-user-groups.md) existente ou pode selecionar **Criar um novo grupo** para definir um novo grupo. Você precisará inserir os endereços de email associados à conta da Microsoft de cada pessoa que quiser incluir no grupo. Para obter mais informações, consulte [Criar um novo grupo usuários conhecido](create-known-user-groups.md).

Depois que seu envio for publicado, as pessoas do grupo que você especificar poderão exibir a listagem do app e baixar o app, desde que estejam conectados com a conta da Microsoft associada ao endereço de email inserido e que estejam executando o Windows 10, versão 1607 ou posterior (incluindo o Xbox One). No entanto, as pessoas que não estiverem em sua audiência particular não poderão exibir a listagem do app ou baixar o app, independentemente da versão do sistema operacional que estejam executando. Você pode publicar envios atualizados para a audiência particular, que serão distribuídos para membros da audiência da mesma maneira como uma atualização de app normal (mas ainda não estarão disponíveis para qualquer pessoa que não estiver em sua audiência particular, a menos que você altere sua seleção de audiência). 

Se você pretende disponibilizar o app para uma audiência geral em uma determinada data e hora, pode marcar a caixa rotulada **Tornar este produto público em** ao criar o envio. Insira a data e a hora (em UTC) quando quiser que o produto se torne disponível para o público. Lembre-se do seguinte:

- A data e a hora que você selecionar serão aplicadas a todos os mercados. Se você quiser personalizar a agenda de lançamento para mercados diferentes, não use essa caixa. Em vez disso, crie um novo envio que altere sua configuração para **Audiência pública** e então use as opções de [Agendar](configure-precise-release-scheduling.md) para especificar seu cronograma de lançamento.
- Inserir uma data para **Tornar este produto público em** não se aplica à Microsoft Store para Empresas e/ou à Microsoft Store para Educação. Para nos permitir oferecer seu app a esses clientes por meio do licenciamento organizacional, você precisará criar um novo envio com **Audiência pública** selecionado (e [licenciamento organizacional](organizational-licensing.md) habilitado).
- Após a data e a hora selecionadas, todos os envios futuros usarão **Audiência pública**.

Se você não especificar uma data e hora para disponibilizar seu app para uma audiência pública, sempre poderá fazer isso mais tarde criando um novo envio e alterando a configuração do seu da sua audiência de **Audiência particular** para **Audiência pública**. Quando você fizer isso, tenha em mente que seu app poderá passar por um processo de certificação adicional, portanto esteja preparado para solucionar os novos problemas de certificação que possam surgir. 

Aqui estão algumas coisas importantes para ter em mente ao escolher distribuir seu app para uma audiência particular:
- As pessoas em sua audiência particular serão capazes de obter o app usando um link específico para a listagem da Store do seu app que exige que eles se conectem com a conta da Microsoft para exibi-lo. Esse link é fornecido quando você seleciona **Audiência pública**. Você também pode encontrá-lo na página [Identidade do aplicativo](view-app-identity-details.md) na **URL se o seu app só estiver visível para determinadas pessoas (requer autenticação)**. Dê aos seus testadores esse link, e não a URL regular para sua listagem da Store.  
- A menos que você escolha uma opção em **Detectabilidade** que impeça isso, as pessoas em sua audiência particular poderão encontrar seu app pesquisando no app da Microsoft Store. No entanto, a listagem da Web não será detectável por meio de pesquisa, até mesmo para as pessoas nessa audiência. 
- Você não conseguirá [configurar as datas de lançamento na seção Agendar](configure-precise-release-scheduling.md) da **página Preço e disponibilidade**, já que seu app não será lançado para os clientes fora da sua audiência particular.
- Outras seleções feitas serão aplicadas às pessoas nessa audiência. Por exemplo, se você escolher um preço diferente de **Gratuito**, as pessoas em sua audiência particular terão de pagar o preço para adquirir o app. 
- Se você quiser distribuir pacotes diferentes para pessoas diferentes em sua audiência particular, após o envio inicial use [pacotes de pré-lançamento](package-flights.md) para distribuir atualizações de pacote diferentes em subconjuntos de sua audiência particular. Você pode criar grupos de usuários conhecidos adicionais para definir quem deve obter uma versão de pré-lançamento do pacote específico.
- Você pode editar a associação dos grupos de usuários conhecidos em sua audiência particular. No entanto, tenha em mente que se você remover alguém que estava no grupo e que já baixou seu app, ele ainda poderá usar o app, mas não obterá as atualizações fornecidas (a menos que você escolha **Audiência pública** em uma data posterior).
- Seu app não estará disponível por meio da Microsoft Store para Empresas e/ou a Microsoft Store para Educação, independentemente de suas configurações de licenciamento organizacional, até mesmo para as pessoas em sua audiência particular.
- Embora a Store garanta que seu app só esteja visível e disponível para as pessoas conectadas com uma conta da Microsoft adicionada à sua audiência particular, não podemos impedir que essas pessoas compartilhem informações ou capturas de tela fora de sua audiência particular. Quando a confidencialidade for essencial, certifique-se de que a sua audiência particular inclua apenas as pessoas que você confia que não compartilharão os detalhes de seu app com outras pessoas.
- Informe aos seus testadores como eles podem enviar comentários para você. Você provavelmente não deseja que eles deixem comentários no Hub de Feedback, porque qualquer outro cliente poderia vê-los. Considere a inclusão de um link para que eles possam enviar emails ou fazer comentários de alguma outra forma.
- As avaliações escritas por pessoas em seu público particular estarão disponíveis para exibição. No entanto, essas análises não serão publicadas na listagem da Store do seu app, mesmo depois que o envio for movido para **Audiência pública**. Você pode ler avaliações escritas por seu público particular exibindo o [Relatório de avaliações](reviews-report.md) no Centro de Desenvolvimento, mas não pode baixar esses dados ou usar a [API de análise da Microsoft Store](../monetize/access-analytics-data-using-windows-store-services.md) para acessar programaticamente essas avaliações.
- Quando você move um app de **Audiência particular** para **Audiência pública**, a **Data do lançamento** mostrada na listagem da Store será a data em que ele foi publicado pela primeira vez para a audiência pública.

## <a name="discoverability"></a>Detectabilidade

As seleções na seção **Detectabilidade** indicam como os clientes podem descobrir e adquirir seu app. 

> [!IMPORTANT]
> Se você selecionar **Audiência pública**, suas seleções de **Detectabilidade** só se aplicarão às pessoas da sua audiência particular. Os clientes que não estão nos grupos especificados não poderão obter o app ou até mesmo ver sua listagem. 


### <a name="make-this-product-available-and-discoverable-in-the-store"></a>Disponibilizar este produto e torná-lo detectável na Store

Essa é a opção padrão. Deixe esta opção selecionada se quiser que seu aplicativo seja listado na loja para os clientes encontrarem via link direto do aplicativo e/ou por outros métodos, como pesquisa, navegação e inclusão em listas comissionadas. 

### <a name="make-this-product-available-but-not-discoverable-in-the-store"></a>Disponibilizar este produto mas não torná-lo detectável na Store

Quando você selecionar essa opção, seu app não poderá ser encontrado na Store por clientes pesquisando ou navegando; a única maneira de obter a listagem do seu app é por link direto. 

> [!TIP]
> Se você não quiser que a listagem fique visíveis para o público, mesmo com um link direto, escolha **Audiência pública** na seção **Audiência**, conforme descrito acima.

Você também deve escolher uma das seguintes opções para especificar como seu app pode ser adquirido:


>[!IMPORTANT]
> Cada uma dessas opções limita as versões de sistema operacional no qual os clientes podem adquirir o aplicativo. Leia as descrições atentamente para garantir que você sabe quais versões do sistema operacional são compatíveis. 

- **Somente link direto: qualquer cliente com um link direto para a listagem do produto pode baixá-lo, exceto no Windows 8.x.** Qualquer cliente que obtiver a listagem do seu app por meio de um link direto poderá baixá-lo em dispositivos que executam o Windows 10 ou em dispositivos que executam o Windows Phone 8.1 ou versões anteriores (mas não em dispositivos que executam o Windows 8.x).
- **Parar a aquisição: os clientes com um link direto poderão ver a listagem da Loja do produto, mas só poderão baixá-lo se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10.** Mesmo que um cliente tenha um link direto, não é possível baixar o aplicativo, a menos que ele tenha um [código promocional](generate-promotional-codes.md) e esteja usando um dispositivo Windows 10. Se um cliente tiver um código promocional, ele poderá usar o link e o código para acessar seu app gratuitamente (somente no Windows 10), mesmo que você não esteja oferecendo-o a outros clientes. Não há nenhuma outra forma de obter seu aplicativo a não ser por meio de um código promocional.

> [!TIP]
> Se você desejar parar completamente de oferecer um aplicativo para quaisquer novos clientes, é necessário selecionar **Tornar aplicativo indisponível** na página de visão geral. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Microsoft Store, e os novos clientes não poderão obtê-lo (exceto com um [código promocional](generate-promotional-codes.md) e se estiver no dispositivo Windows 10). Esta ação substituirá as seleções de **Visibilidade** no seu envio. Para disponibilizar o aplicativo para novos clientes novamente (de acordo com seleções de **Visibilidade**), você pode a qualquer momento clicar em **Tornar aplicativo disponível** na página de visão geral do aplicativo. Para obter mais informações, consulte [Como remover um aplicativo da Store](guidance-for-app-package-management.md#removing-an-app-from-the-store).




