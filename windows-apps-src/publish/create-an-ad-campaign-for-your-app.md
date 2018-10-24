---
author: JnHs
Description: You can create an ad campaign using the Dev Center dashboard to help promote your app and grow your app's user base.
title: Criar uma campanha publicitária para seu app
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.author: wdg-dev-content
ms.date: 05/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, anúncios, campanhas, promover
ms.localizationpriority: medium
ms.openlocfilehash: 8c7e7c1e2cd9a2ca083cef5fed27a9067cf1e7e7
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5437596"
---
# <a name="create-an-ad-campaign-for-your-app"></a>Criar uma campanha publicitária para seu app

Você pode criar uma campanha publicitária usando o painel do Centro de Desenvolvimento para promover seu aplicativo e ampliar a base de usuários de seu aplicativo. Por padrão, escolheremos o público-alvo para seus anúncios com base nas configurações de seu aplicativo no painel do Centro de Desenvolvimento, mas, opcionalmente, você pode definir seu próprio público. Você também pode usar um conjunto padrão de modelos de anúncios ou carregar seus próprios designs de anúncios. Para obter mais detalhes sobre campanhas publicitárias, consulte [Perguntas comuns sobre campanhas publicitárias](common-questions.md).

Você pode criar campanhas publicitárias somente para aplicativos que passaram na fase final de publicação do [processo de certificação de aplicativo](the-app-certification-process.md).

> [!NOTE]
> Esta seção da documentação descreve como criar uma campanha publicitária no painel do Centro de Desenvolvimento. Como alternativa, você pode usar a [API de promoções da Microsoft Store](../monetize/run-ad-campaigns-using-windows-store-services.md) para criar e gerenciar as campanhas publicitárias de forma programática.

## <a name="instructions"></a>Instruções

Veja aqui como criar uma campanha publicitária para promover um aplicativo.

1.  No menu de navegação esquerdo do painel, expanda **Atrair** e, em seguida, selecione **Campanhas de anúncios**.
2.  Selecione **Criar campanha** (ou se você criou campanhas antes, selecione **Nova campanha**).
3.  Na página seguinte, na seção **Objetivo da campanha**, escolha um destes procedimentos:
    * **Aumentar as instalações do seu aplicativo**. Selecione essa opção se sua campanha publicitária tiver o objetivo de levar as pessoas a instalar seu aplicativo.
    * **Aumentar o envolvimento em seu aplicativo**. Selecione essa opção se a campanha publicitária tiver como objetivo o aumento da utilização do aplicativo pelos clientes. Ao selecionar essa opção, você pode direcionar sua campanha publicitária aos [segmentos de clientes](create-customer-segments.md) específicos que você definir.

4.  Selecione o aplicativo que você deseja promover com essa campanha. Observe que o aplicativo já deve estar disponível na Loja.
5.  Examine o nome fornecido para a campanha no campo **Nome da campanha** e faça as alterações, se desejado.
6.  Em **Tipo de campanha**, escolha uma destas opções:
    * **Anúncios pagos**: esses anúncios serão veiculados em qualquer aplicativo que corresponda ao dispositivo e à categoria do seu aplicativo. Para novas campanhas criadas após 9 de janeiro de 2017, esses anúncios também serão exibidos no MSN.com, Outlook.com, Skype e outras propriedades premium da Microsoft. Campanhas de promoção de aplicativos destinados a propriedades premium da Microsoft e são conhecidas como campanhas *universais*.
    * **Anúncios da comunidade (gratuitos)**: esses anúncios serão veiculados em aplicativos publicados por outros desenvolvedores que também criam campanhas publicitárias da comunidade. Antes de poder selecionar essa opção, você deve optar por mostrar anúncios da comunidade na página **Monetizar** -> **Anúncios no app**. Para saber mais, consulte [Sobre anúncios da comunidade](about-community-ads.md).
    * **Anúncio interno (gratuito)**: os anúncios serão veiculados somente em seus apps que correspondem ao tipo de dispositivo do app anunciado. Anúncios domésticos são gratuitos. Para saber mais, veja [Sobre anúncios domésticos](about-house-ads.md).

7.  Para campanhas publicitárias pagas, confirme a **Duração de campanha** (o período de tempo em que seu orçamento da campanha será gasto). A opção padrão é **Mensal**, ou seja, o orçamento da campanha será gasto por mês de forma recorrente até você parar a campanha. Como alternativa, se você tiver uma conta premium, selecione **Personalizado** para especificar um intervalo de data e hora personalizado durante o qual o orçamento da campanha será gasto. Para obter mais informações sobre contas premium, consulte [Perguntas comuns sobre campanhas publicitárias](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).

8.  Confirme suas informações de orçamento e pagamento. (se você estiver criando uma campanha interna ou da comunidade, essas opções são serão exibidas, já que essas campanhas são gratuitas).
    * Em **Orçamento**, use o controle deslizante para definir o valor em dinheiro que você deseja gastar a cada mês para veicular o anúncio (ou o orçamento total, se você tiver selecionado uma duração de campanha personalizada).

        O orçamento mensal é proporcional ao mês em que a campanha publicitária é criada. Em outras palavras, se você criar uma campanha publicitária na metade de um mês, será cobrado pela metade de seu orçamento mensal nesse mês.

    * Especifique um método de pagamento para a campanha publicitária ao clicar em **Adicionar novo método de pagamento** e preencha os detalhes da sua conta. Se você já tiver fornecido um método de pagamento, é possível selecionar **Escolher um método de pagamento diferente** se for necessário atualizá-lo. O país/região do endereço de cobrança do seu método de pagamento deve coincidir com o país/região associado à conta do Centro de Desenvolvimento.

    * Se você recebeu um cupom de um representante da Microsoft para pagar uma campanha publicitária, clique em **Usar um cupom**, insira o código do cupom e clique em **Aplicar** para aplicar o cupom à campanha.

    Ao terminar, clique em **Salvar e avançar** para ir até a etapa **Público**. Esta etapa não está disponível para campanhas de anúncios internos, pois são executados somente em seus próprios aplicativos.

9.  Na página **Público**, mostraremos as configurações de público recomendadas para a campanha. Como opção, você pode ajustar essas informações:
    * **Países/regiões**: escolha até cinco países ou regiões em que você deseja que o anúncio apareça. Para obter uma lista de países ou regiões com suporte, consulte [Perguntas comuns sobre campanhas publicitárias](common-questions.md#where-will-my-ad-appear).

    * **Dispositivos**: escolha os tipos de dispositivo em que você deseja que esses anúncios apareçam. Somente os tipos de dispositivo suportados por seu app são mostrados.

    * **Surface**: selecione **Universal** para permitir que o anúncio aparece nos aplicativos, além de MSN.com, Outlook.com, Skype e outras propriedades premium da Microsoft. Escolha **Aplicativo** se você deseja que o anúncio apareça somente em aplicativos.

    * **Sistema operacional**: escolha os sistemas operacionais nos quais seu anúncio deve aparecer. Somente os sistemas operacionais com suporte pelo seu aplicativo são mostrados.

    * **Gênero**: optar por restringir a audiência do anúncio por gênero.

    * **Faixa etária**: escolha a faixa etária do público-alvo desejado.

    Esta seção também exibe um gráfico **Alcance estimado**. Esse gráfico mostra o público-alvo que você pode esperar alcançar com suas seleções de direcionamento atuais como um percentual de todos os usuários do aplicativo habilitado por anúncio do Windows nos mercados selecionados.

10.  Se você escolheu **Aumentar o envolvimento do app** como seu objetivo de campanha, poderá selecionar um dos segmentos de cliente para direcionar. Os anúncios criados usando essa campanha serão mostrados apenas para os clientes que estão incluídos no segmento. Apenas um segmento pode ser selecionado por campanha publicitária. Para obter informações sobre segmentos, consulte [Criar segmentos de clientes](create-customer-segments.md). Ao terminar, clique em **Salvar e avançar** para ir até a etapa **Design do anúncio**. Esta etapa não está disponível para campanhas de anúncios internos, porque eles são executados somente em seus próprios aplicativos.

11.  Na página **Design de anúncio**, escolha uma destas opções:
    * **Gerado automaticamente**. Essa é a opção padrão, e permite que você criar um anúncio a partir dos modelos padrão. Você pode fazer seleções a fim de personalizar o conteúdo do anúncio, e visualizaremos a aparência do anúncio com base em suas opções (automaticamente atualizadas à medida que você faz as seleções).
        * Na lista suspensa **Idioma**, selecione o idioma do anúncio. O texto para o selo da Microsoft Store será exibido no idioma selecionado.
        * Para adicionar uma linha extra de texto ao anúncio, digite o texto no campo **Slogan personalizado**.
            > [!NOTE]
            > O texto que você insere aqui deve estar traduzido no idioma selecionado. O slogan personalizado será rejeitado se o texto não estiver de acordo com as [Políticas do Bing Ads](http://go.microsoft.com/fwlink?LinkId=398341). Consulte esta página para obter orientação sobre estilo e conteúdo não permitido.
        * Para personalizar mais o anúncio, expanda **Personalizar design de anúncios/Ver todos os tamanhos de anúncio** e escolha um dos seguintes itens:
            * **Cor da tela de fundo**. Escolha entre as opções disponíveis.
            * **Imagens**. Escolha uma das imagens disponíveis (tiradas da listagem da Loja do aplicativo).
            * **Mostrar a classificação do meu aplicativo**. Marque essa caixa de seleção se você quiser mostrar a classificação do aplicativo.
            * **Mostrar que meu aplicativo é gratuito**. Se o aplicativo for gratuito em todos os mercados selecionados, você também terá a opção de marcar essa caixa de seleção.
            * **Chamada para ação**. Se você escolheu **Aumentar o envolvimento do app** como objetivo da campanha, defina o botão de chamada para ação de seu anúncio como **Abrir**, **Reproduzir**, **Ler**, **Escutar** ou **Comprar**.  

    * **Personalizado**. Escolha essa opção para usar seus próprios designs de anúncios. Observe que, se você selecionou um segmento de cliente anteriormente, você deverá usar criativos personalizados. Você pode carregar arquivos diferentes para cada um dos tamanhos de anúncios disponíveis. Os arquivos devem cumprir os requisitos e as diretrizes a seguir:
        * Cada arquivo deve ser um .png ou .jpg com 40 KB ou menos.
        * Seus designs de anúncios devem atender aos requisitos especificados na [Política de Aceitação de Criatividade da Microsoft](http://go.microsoft.com/fwlink?LinkId=532595).
        * O conteúdo em seus designs de anúncios deve ser relevante para o aplicativo que você está promovendo. Designs de anúncios não relacionadas ao aplicativo não serão distribuídos a anúncios em outros aplicativos.
        * Todo o conteúdo em seus designs de anúncios deve ser claramente legível. Por exemplo, o conteúdo não deve ser desfocado, pixelado ou ampliado.

12.  Se você tiver uma [conta premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), pode usar a caixa **URL de destino** para controlar o que acontece quando um cliente clica em seu anúncio.
    * Se você deixar a caixa vazia, quando um cliente clicar em seu anúncio, a listagem da Loja do seu app será exibida.
    * Se você estiver usando o Kochava ou o Tune para medir análises de instalação de seu app, insira URL de rastreamento de instalação do Kochava ou do Tune. Quando você salvar a campanha, a URL de rastreamento será validada para garantir que ela seja resolvida como a página de listagem para seu app na Microsoft Store. Para obter mais informações sobre o controle de instalação com Kochava e Tune, consulte a documentação do [Kochava](http://support.kochava.com/) e do [Tune](https://help.tune.com/).
    * Se você escolheu **Aumentar o envolvimento do app** como objetivo da campanha, pode especificar um [URI de um link profundo](../launch-resume/handle-uri-activation.md) para redirecionar os clientes no segmento selecionado para uma página específica em seu app.
    * Se você especificar um destino que não seja a página de descrição do seu app ou uma página dentro de seu app, sua campanha será pausada automaticamente.

13.  Por fim, clique em **Revisão** para confirmar as configurações de sua campanha publicitária e, se for uma campanha publicitária paga, o orçamento e as informações de pagamento. Clique em **Confirmar** e seus anúncios começarão a aparecer normalmente nos dispositivos em algumas horas!

## <a name="review-ad-campaign-performance"></a>Analisar o desempenho da campanha publicitária

Para ver o desempenho das campanhas publicitárias, retorne à página **Campanhas publicitárias**. Selecione **Filtros de seção** para definir o escopo que está incluído no relatório por **Data**, **Objetivo da campanha**, **Nome do aplicativo**, **Tipo de campanha** ou **Status**. Além de ver informações sobre **Impressões**, **Cliques**, **Conversões** e **Gastos** da sua campanha, você pode usar o relatório para **Pausar** ou **Retomar** uma campanha. Para saber mais, consulte [Relatório de campanhas publicitárias](promote-your-app-report.md).

Para editar uma campanha, selecione seu nome na lista.

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciando sua campanha publicitária](managing-your-ad-campaign.md)
* [Sobre anúncios domésticos](about-house-ads.md)
* [Relatório de promoção do seu app](promote-your-app-report.md)
* [Perguntas comuns sobre campanhas publicitárias](common-questions.md)
