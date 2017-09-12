---
author: jnHs
Description: "O painel do Centro de Desenvolvimento do Windows oferece a opção de tornar seu aplicativo disponível apenas para pessoas específicas para que você possa fazer com que testadores o testem antes de oferecê-lo ao público."
title: "Teste beta e distribuição direcionada"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7dd0e346e6be147935503eeb0b685568bfce726c
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
# <a name="beta-testing-and-targeted-distribution"></a>Teste beta e distribuição direcionada

Não importa se você testou seu aplicativo com cuidado extremo, não há nada como a experiência concreta de outras pessoas o testarem. O painel do Centro de Desenvolvimento do Windows oferece opções para disponibilizar um aplicativo apenas para pessoas específicas; assim, os testadores podem testá-lo antes que você o ofereça ao público. Seus testadores podem descobrir problemas que você ignorou, como erros ortográficos, fluxo confuso do aplicativo ou erros que poderiam causar falhas no aplicativo. Você terá a chance de corrigir esses problemas antes de fazer o envio ao público, o que resultará em um produto final apurado.

Fornecemos várias maneiras de limitar a distribuição dos aplicativos somente aos testadores, sem precisar criar uma versão separada do aplicativo com um nome e um identificador de pacote diferentes. É claro, você pode criar um aplicativo separado apenas para testes se preferir. Se fizer isso, certifique-se de dar um nome diferente do que você pretende usar como o nome do aplicativo final público.

O método de distribuição do seu aplicativo para os testadores depende dos sistemas operacionais para os quais o aplicativo se destina. Abaixo, você encontrará opções que funcionam tanto para o Windows 10 quanto para o Windows Phone 8.1 e versões anteriores.

## <a name="making-your-app-available-to-testers-on-windows-10-devices"></a>Disponibilizando seu aplicativo para testadores em dispositivos Windows 10

Fornecemos duas opções que permitem limitar a distribuição dos seus aplicativos para apenas algumas pessoas em dispositivos Windows 10.

### <a name="package-flights"></a>Pacotes de pré-lançamento

Se você já publicou o aplicativo, é possível criar pacotes de pré-lançamento para distribuir um conjunto diferente de pacotes a pessoas específicas. Você pode até criar vários pacotes de pré-lançamento para o mesmo aplicativo a serem usados com diferentes grupos de pessoas. Esta é uma ótima maneira de experimentar diferentes pacotes simultaneamente e você pode extrair pacotes de uma versão de pré-lançamento para seu envio de versão completa, caso decida que os pacotes estão prontos para distribuição a todos os usuários.

Para saber mais, veja [Pacotes de pré-lançamento](package-flights.md).

> [!NOTE]
> Para distribuir pacotes específicos para uma seleção aleatória de clientes do Windows 10 em uma porcentagem especificada, em vez de em um grupo designado de clientes específicos, você pode usar uma [distribuição de pacote gradual](gradual-package-rollout.md). Você também pode combinar distribuição com seus pacotes de pré-lançamento se quiser distribuir gradualmente uma atualização para um de seus grupos de versão de pré-lançamento.

<span id="hide" />
### <a name="hiding-the-app-in-the-store-and-using-promotional-codes"></a>Ocultando o aplicativo na Loja e usando códigos promocionais

Se você deseja limitar a distribuição de um aplicativo para apenas um determinado grupo de testadores, **sem** primeiro publicar um envio de disponibilidade geral, pode usar o mesmo [processo de envio de aplicativo](app-submissions.md) válido para qualquer outro aplicativo enviado. Para permitir que apenas algumas pessoas obtenham o aplicativo gratuitamente e impedir que outros clientes vejam sua listagem ou façam seu download, siga este procedimento:

-   No seu envio, na seção [Visibilidade](set-app-pricing-and-availability.md#visibility) da página **Preço e disponibilidade**, selecione **Disponibilizar este produto mas não torná-lo detectável na Loja**.  Escolha a opção de **Parar a aquisição: os clientes com um link direto poderão ver a listagem da Loja do produto, mas só poderão baixá-lo se já tiverem adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10**. Isso impede que qualquer pessoa encontre seu aplicativo na Loja por meio de pesquisa ou navegação.
-   Depois que o aplicativo for aprovado na certificação, [gere códigos promocionais](generate-promotional-codes.md) para o aplicativo e distribua-os aos seus testadores. Você pode gerar códigos que permitem até 1600 resgates para um único aplicativo em um período de seis meses. Esses códigos fornecem aos testadores um link direto para os detalhes do aplicativo e permitem que eles o baixem gratuitamente, mesmo se você tiver definido um preço para ele quando criou seu envio.

Após distribuir os links de código promocional para os testadores, eles poderão testá-lo e fornecer comentários que ajudarão você a melhorar o aplicativo. Em seguida, quando você estiver pronto para disponibilizar o aplicativo para o público, crie um novo envio e altere a opção **Visibilidade** para **Disponibilizar e divulgar o aplicativo na Loja** (juntamente com quaisquer outras alterações que você queira fazer).

Veja algumas coisas para se ter em mente ao fazer isso:

-   Você pode dar aos testadores uma versão atualizada do seu aplicativo a qualquer momento. Basta criar um novo envio. Mantenha a opção **Visibilidade** definida como **Disponibilizar este produto mas não torná-lo detectável na Loja** com a opção **Parar aquisição**. Os testadores receberão a atualização após passar pelo processo de certificação, mas nenhuma outra pessoa a receberá.
-   Seus testadores devem ter um dispositivo Windows 10 no qual possam instalar o aplicativo. (Contudo, seu aplicativo não precisa incluir pacotes do Windows 10 para usar esse método de teste).
-   Você pode criar mais [códigos promocionais](generate-promotional-codes.md) para distribuir a qualquer momento (até 1600 resgates de por aplicativo a cada seis meses).
-   Você não poderá revogar o acesso ao aplicativo depois que os testadores o baixarem. Depois que eles baixarem o aplicativo, poderão continuar a usá-lo, e eles obterão as atualizações que você publicar posteriormente.
-   Você precisa determinar como deseja receber feedback dos testadores. Considere fornecer um link no aplicativo beta para permitir que os testadores forneçam comentários por email ou por meio do [Hub de Feedback](../monetize/launch-feedback-hub-from-your-app.md).
-   Você pode analisar os [relatórios analíticos](analytics.md) do aplicativo, incluindo relatórios de uso e integridade, além de classificações ou análises deixadas pelos testadores.
-   Você pode incluir complementos quando você distribuir seu aplicativo aos testadores. Como você provavelmente não deseja cobrá-los, defina o preço dos complementos como **Gratuito** enquanto estiver fazendo o teste. Em seguida, quando você disponibilizar o aplicativo para outros clientes, poderá criar um novo envio para cada complemento para alterar o preço.


## <a name="other-methods-for-distributing-apps-to-testers"></a>Métodos adicionais limitados para distribuir aplicativos aos testadores

Você também pode limitar a distribuição do seu aplicativo apenas para um grupo definido de pessoas, usando as opções adicionais na seção [Visibilidade](set-app-pricing-and-availability.md#visibility) da página **Preço e disponibilidade** de um envio de aplicativo. Lembre-se de que essas opções não funcionam para clientes em todas as versões de sistema operacional. Especificamente, nenhuma delas funciona para clientes no Windows 8 ou no Windows 8.1.

### <a name="targeted-distribution-to-customers-with-a-link-to-your-apps-listing"></a>Distribuição direcionada a clientes com um link para a listagem do aplicativo

Com essa opção, apenas as pessoas com um link direto para os detalhes do aplicativo podem baixá-lo. Você pode encontrar essa **URL** na página [Identidade do aplicativo](view-app-identity-details.md) no painel. Nenhum cliente conseguirá encontrar o aplicativo pesquisando ou navegando na Loja, mas qualquer pessoa com o link poderá baixá-lo em um dispositivo com Windows Phone 8.1 ou anteriores ou Windows 10. 

> [!NOTE]
> Para que os testadores possam baixar o aplicativo gratuitamente, defina o preço como **Gratuito**.

Para usar essa opção, selecione **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](set-app-pricing-and-availability.md#visibility) da página **Preço e disponibilidade**. Em seguida, escolha a opção de **Somente link direto: qualquer cliente com um link direto para a listagem do produto pode baixá-lo, exceto no Windows 8.x.**.  


### <a name="targeted-distribution-to-customers-with-specified-email-addresses"></a>Distribuição direcionada a clientes com endereços de email especificados

Para testar **somente no Windows Phone 8.1 e em versões anteriores**, essa opção oferece uma maneira de limitar a distribuição de seu aplicativo. Apenas as pessoas cujo email (associado a suas contas da Microsoft) você inserir na caixa poderão baixar seu aplicativo usando o link direto para sua listagem.

> [!IMPORTANT]
> As pessoas com os endereços de email que você inserir só poderão baixar o aplicativo em dispositivos que executam o Windows Phone 8.1 ou anterior.
 
Você pode encontrar o link direto do seu aplicativo na página [Identidade de aplicativo](view-app-identity-details.md) no painel. Nenhum cliente poderá encontrar o aplicativo pesquisando ou navegando na Loja e, mesmo que tenham o link para a listagem de seu aplicativo, não poderão baixá-lo a menos que estejam usando uma conta da Microsoft associada a um email que você forneceu quando enviou esse aplicativo.

> [!NOTE]
Se você usar essa opção, ainda poderá disponibilizar o aplicativo para os testadores em dispositivos Windows 10 [gerando códigos promocionais](generate-promotional-codes.md), conforme descrito anteriormente. Qualquer pessoa com um dos códigos promocionais de seu aplicativo pode baixá-lo em um dispositivo Windows 10, mesmo que você não tenha inserido o email.

Para usar essa opção, selecione **Disponibilizar este produto mas não torná-lo detectável na Loja** na seção [Visibilidade](set-app-pricing-and-availability.md#visibility) da página **Preço e disponibilidade**. Em seguida, escolha a opção de **Somente para pessoas no Windows Phone 8.x: somente as pessoas que você especificar abaixo podem baixar este produto em um dispositivo Windows Phone 8.x. Qualquer pessoa com link direto ainda pode ver a listagem do produto, e um código promocional pode ser usado para baixar o produto em dispositivos Windows 10.** 

Se você escolher essa opção, tenha o seguinte em mente:

-   Essa opção só poderá ser selecionada se você nunca tiver publicado o aplicativo com a opção [Visibilidade](set-app-pricing-and-availability.md#visibility) definida como **Disponibilizar este aplicativo na Loja**.
-   O aplicativo deve ter o preço estabelecido como **Grátis** para que os testadores possam baixá-lo sem custo.
-   Seus testadores só podem baixar o aplicativo no Windows Phone 8.1 e versões anteriores. Os testadores deverão ter um dispositivo Windows Phone comercial para usar o aplicativo, mas o dispositivo não precisa ser desbloqueado ou registrado.
-   Os testadores precisarão ter uma conta da Microsoft para acessar a Windows Store e baixar seu aplicativo. Você precisará saber o email associado à conta da Microsoft de cada testador para adicioná-los à sua lista. Para criar uma nova conta da Microsoft, os testadores podem acessar a página [Configuração da conta da Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=618945).
-   Você pode fornecer até 10.000 endereços de email na caixa de texto.
-   Os endereços de email devem ser separados por ponto e vírgula.
-   Você pode adicionar outros endereços mais tarde, mas será necessário criar um novo envio para fazer isso.
-   Você não pode revogar o acesso ao aplicativo que os testadores baixá-lo. Depois que eles baixarem o aplicativo, os testadores poderão continuar a usá-lo e receberão as atualizações que você enviar.
