---
author: jnHs
Description: "O painel do Centro de Desenvolvimento do Windows oferece a opção de tornar seu aplicativo disponível apenas para pessoas específicas para que você possa fazer com que testadores o testem antes de oferecê-lo ao público."
title: "Teste da versão beta e distribuição específica"
ms.assetid: 38E4ED22-D6C1-40D8-9B16-6B3E51BD962E
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: a544565bf7bb82f2be1ded3e60836d5d191c6e93

---

# Teste da versão beta e distribuição específica


Não importa se você testou seu aplicativo com cuidado extremo, não há nada como a experiência concreta de outras pessoas o testarem. O painel do Centro de Desenvolvimento do Windows oferece a opção de tornar seu aplicativo disponível apenas para pessoas específicas para que você possa fazer com que testadores o testem antes de oferecê-lo ao público. Seus testadores podem descobrir problemas que você ignorou, como erros ortográficos, fluxo confuso do aplicativo ou erros que poderiam causar falhas no aplicativo. Você terá a chance de corrigir esses problemas antes de lançar o aplicativo para o público, o que resultará em um produto final apurado.

Fornecemos várias maneiras de limitar a distribuição dos seus aplicativos somente para os testadores, sem a necessidade de criar uma versão separada do seu aplicativo com um nome e um identificador de pacote diferentes. É claro, você pode criar um aplicativo separado apenas para testes se preferir. Se fizer isso, certifique-se de dar um nome diferente do que você pretende usar como o nome do aplicativo final público.

O método de distribuição do seu aplicativo para os testadores depende dos sistemas operacionais para os quais o aplicativo se destina. Abaixo, você encontrará opções que funcionam tanto para o Windows 10 quanto para o Windows Phone 8.1 e versões anteriores.

## Disponibilizando seu aplicativo para testadores em dispositivos Windows 10

Fornecemos duas opções que permitem limitar a distribuição dos seus aplicativos para apenas algumas pessoas em dispositivos Windows 10.

### Pacotes de pré-lançamento

Se você já publicou uma versão do seu aplicativo, pode criar pacotes de pré-lançamento para distribuir um conjunto diferente de pacotes a pessoas especificadas. Você pode criar vários pacotes de pré-lançamento para o mesmo aplicativo a serem usados com diferentes grupos de pessoas. Esta é uma ótima maneira de experimentar diferentes pacotes simultaneamente e permite que você extraia pacotes de uma versão de pré-lançamento para o seu envio de versão completa caso decida que eles estão prontos para distribuição a todos os usuários.

Para saber mais, veja [Pacotes de pré-lançamento](package-flights.md).

### Ocultando o aplicativo na Loja e usando códigos promocionais

Se você deseja limitar a distribuição de um aplicativo para apenas um determinado grupo de testadores, sem primeiro publicar um envio de disponibilidade geral, pode usar o mesmo [processo de envio de aplicativo](app-submissions.md) válido para qualquer outro aplicativo enviado. Para permitir que apenas algumas pessoas obtenham o aplicativo gratuitamente e impedir que outros clientes vejam sua listagem ou façam seu download, siga este procedimento:

-   No seu envio, na página **Preço e disponibilidade**, escolha **Ocultar esse aplicativo e evitar aquisição. Clientes com um código promocional ainda podem baixá-lo em dispositivos Windows 10**, na seção [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility). Isso impede que qualquer pessoa encontre seu aplicativo na Loja por meio de pesquisa ou navegação.
-   Depois que o aplicativo for aprovado na certificação, [gere códigos promocionais](generate-promotional-codes.md) para o aplicativo e distribua-os aos seus testadores. Você pode gerar até 250 códigos promocionais para um único aplicativo em um período de seis meses. Esses códigos fornecem aos testadores um link direto para os detalhes do aplicativo e permitem que eles o baixem gratuitamente, mesmo se você tiver definido um preço para ele quando criou seu envio.

Depois de distribuir os links de código promocional para os testadores, eles podem baixar seu aplicativo gratuitamente, experimentar e fornecer feedback para ajudar a melhorar o aplicativo. Em seguida, quando estiver pronto para disponibilizar seu aplicativo para o público, basta criar um novo envio e alterar a opção **Distribuição e visibilidade** para **Qualquer pessoa pode encontrar seu aplicativo na Loja** (junto com quaisquer outras alterações que você queira fazer).

Veja algumas coisas para se ter em mente ao fazer isso:

-   Você pode dar aos testadores uma versão atualizada do seu aplicativo a qualquer momento. Basta criar um novo envio. Certifique-se de manter a opção **Distribuição e visibilidade** definida como **Ocultar esse aplicativo e evitar aquisição. Clientes com um código promocional ainda podem baixá-lo em dispositivos Windows 10**. Os testadores receberão a atualização após passar pelo processo de certificação, mas nenhuma outra pessoa a receberá.
-   Seus testadores devem ter um dispositivo Windows 10 no qual possam instalar o aplicativo. (Contudo, seu aplicativo não precisa incluir pacotes do Windows 10 para usar esse método de teste).
-   Você pode criar mais [códigos promocionais](generate-promotional-codes.md) para distribuir a qualquer momento (até 250 códigos a cada seis meses).
-   Você não pode revogar o acesso ao aplicativo que os testadores baixá-lo. Depois que eles baixarem o aplicativo, poderão continuar a usá-lo, e eles obterão as atualizações que você publicar posteriormente.
-   Você precisa determinar como deseja receber feedback dos testadores. Recomendamos fornecer um link de email ou site no aplicativo beta para que eles possam fornecer comentários facilmente.
-   Você pode examinar [relatórios analíticos](analytics.md) do seu aplicativo, incluindo classificações ou críticas deixadas pelos testadores.
-   Você pode incluir produtos no aplicativo (IAPs) ao distribuir seu aplicativo para os testadores. Como você provavelmente não deseja cobrá-los, defina o preço para os IAPs como Gratuito enquanto estiver fazendo o teste. Em seguida, quando você disponibilizar o aplicativo para outros clientes, poderá criar um novo envio para cada IAP para alterar o preço.

## Métodos adicionais limitados para distribuir aplicativos aos testadores

Você também pode limitar a distribuição do seu aplicativo para apenas um grupo definido de pessoas, usando as opções adicionais na seção [Distribuição e a visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) da página **Preço e disponibilidade** de um envio de aplicativo. Lembre-se de que essas opções não funcionam para clientes em todos os sistemas operacionais. As opções acima são recomendadas quando você está testando seu aplicativo em dispositivos Windows 10.

Se você escolher qualquer uma dessas opções acima, sempre poderá enviar uma atualização e definir a opção [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) como **Disponibilizar este aplicativo na Loja** quando você estiver pronto para finalizar o período de teste e disponibilizar o aplicativo amplamente. Você não precisa alterar o nome do aplicativo e criar um aplicativo completamente separado (a menos que você prefira fazer isso).

### Distribuição direcionada a clientes com um link para os detalhes do aplicativo

Com essa opção, apenas as pessoas com um link direto para os detalhes do aplicativo podem baixá-lo. Você pode encontrar esse link na página [Identidade do aplicativo](view-app-identity-details.md) no painel (use a **URL para Windows Phone** ou **URL para Windows 10**). Os clientes não serão capazes de encontrar o aplicativo pesquisando ou navegando na loja, mas qualquer pessoa com o link pode baixá-lo. (Observe que seu aplicativo deve ter o preço estabelecido como **Grátis** para que os testadores possam baixá-lo sem custo.)

Para usar essa opção, selecione **Ocultar esse aplicativo na Loja. Clientes com um link direto para a listagem do aplicativo ainda podem baixá-lo, exceto no Windows 8 e no Windows 8.1:** na seção [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) da página **Preço e visibilidade** quando for enviar o aplicativo.

> **Importante**  Essa opção não funciona para testadores no Windows 8 ou no Windows 8.1.

### Distribuição direcionada a clientes com endereços de email especificados

Para testar no Windows Phone 8.1 e em versões anteriores, essa opção oferece uma maneira de limitar a distribuição de seu aplicativo. Apenas as pessoas cujo email (associado a suas contas da Microsoft) você inserir na caixa poderão baixar seu aplicativo usando o link direto para sua listagem.

> **Importante**  As pessoas com os endereços de email que você inserir só poderão baixar o aplicativo em dispositivos que executam o Windows Phone 8.1 ou versões anteriores.
 
Você pode encontrar o link direto de seu aplicativo na página [Identidade do aplicativo](view-app-identity-details.md) no painel (use a **URL para o Windows Phone**). Nenhum cliente poderá encontrar o aplicativo pesquisando ou navegando na Loja e, mesmo que tenham o link para a listagem de seu aplicativo, não poderão baixá-lo a menos que estejam usando uma conta da Microsoft associada a um email que você forneceu quando enviou esse aplicativo.

> **Observação**  Se você usar essa opção, ainda poderá tornar o aplicativo disponível para testadores em dispositivos Windows 10, [gerando códigos promocionais](generate-promotional-codes.md) como descrito acima. Qualquer pessoa com um dos códigos promocionais de seu aplicativo pode baixá-lo em um dispositivo Windows 10, mesmo que você não tenha inserido o email.

Para usar essa opção, selecione **Ocultar esse aplicativo e torná-lo disponível apenas para pessoas especificadas abaixo, que podem baixá-lo em dispositivos Windows Phone 8.x. Um código promocional pode ser usado para baixar esse aplicativo em dispositivos Windows 10** na seção [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) da página **Preço e visibilidade** quando for enviar o aplicativo.

Se você escolher essa opção, tenha o seguinte em mente:

-   Essa opção só pode ser selecionada se você nunca tiver publicado o aplicativo com a opção [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) definida como **Qualquer pessoa pode encontrar seu aplicativo na Loja**.
-   O aplicativo deve ter o preço estabelecido como **Grátis** para que os testadores possam baixá-lo sem custo.
-   Seus testadores só podem baixar o aplicativo no Windows Phone 8.1 e versões anteriores. Os testadores deverão ter um dispositivo Windows Phone comercial para usar o aplicativo, mas o dispositivo não precisa ser desbloqueado ou registrado.
-   Os testadores precisarão ter uma conta da Microsoft para acessar a Windows Store e baixar seu aplicativo. Você precisará saber o email associado à conta da Microsoft de cada testador para adicioná-los à sua lista. Para criar uma nova conta da Microsoft, os testadores podem acessar a página [Configuração da conta da Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=618945).
-   Você pode fornecer até 10.000 endereços de email na caixa de texto.
-   Os endereços de email devem ser separados por ponto e vírgula.
-   Você pode adicionar outros endereços mais tarde, mas será necessário criar um novo envio para fazer isso.
-   Você não pode revogar o acesso ao aplicativo que os testadores baixá-lo. Depois que eles baixarem o aplicativo, os testadores poderão continuar a usá-lo e receberão as atualizações que você enviar.



<!--HONumber=Jun16_HO4-->


