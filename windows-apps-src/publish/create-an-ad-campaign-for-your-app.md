---
author: shawjohn
Description: "Você pode criar uma campanha publicitária usando o painel do Centro de Desenvolvimento para promover seu app e ampliar a base de usuários de seu app."
title: "Criar uma campanha publicitária para seu app - Desenvolver apps UWP"
ms.assetid: 10D94929-92C4-4379-AA5F-6FEF879F2463
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, anúncios, campanhas, promover"
translationtype: Human Translation
ms.sourcegitcommit: b01924366a0bc2afabe2f381e72e45862f0dd682
ms.openlocfilehash: a4cee4bd81088fc47ee4d11525297a06381e21b8
ms.lasthandoff: 02/08/2017

---

# <a name="create-an-ad-campaign-for-your-app"></a>Criar uma campanha publicitária para seu app


Você pode criar uma campanha publicitária usando o painel do Centro de Desenvolvimento para promover seu app e ampliar a base de usuários de seu app. Por padrão, escolheremos o público-alvo para seus anúncios com base nas configurações de seu app no painel do Centro de Desenvolvimento, mas, opcionalmente, você pode definir seu próprio público. Você também pode usar um conjunto padrão de modelos de anúncios ou carregar seus próprios designs de anúncios. Para obter mais detalhes sobre campanhas publicitárias, consulte [Perguntas comuns sobre campanhas publicitárias](common-questions.md).

> **Observação**  Você pode criar campanhas publicitárias somente para apps que passaram na fase final de publicação do [processo de certificação de app](the-app-certification-process.md).

Confira aqui como criar uma campanha publicitária para promover seu app.

1.  No menu de navegação esquerdo na página de seu app no painel do Centro de Desenvolvimento, clique em **Monetização** &gt; **Promover seu app**.
2.  Realize um dos seguintes procedimentos:

    -   Se você ainda não tiver criado uma campanha publicitária para este app, a página **Promover seu app** exibe informações sobre os benefícios das campanhas publicitárias. Clique em **Iniciar** ou **Criar uma campanha publicitária**.
    -   Se você já criou uma campanha publicitária para este app, a página **Promover seu app** lista seus campanhas publicitárias existente. Clique em **Nova campanha**.
3.  Na página **Nova campanha**, na seção **Objetivo da campanha**, escolha um destes procedimentos:
    -   **Aumentar as instalações do seu app**. Selecione essa opção se sua campanha publicitária tiver o objetivo de levar as pessoas a instalar seu app.
    -   **Aumentar o envolvimento em seu app**. Selecione essa opção se sua campanha publicitária tiver o objetivo de levar seus clientes a aumentar a utilização do seu app. Ao selecionar essa opção, você pode direcionar sua campanha publicitária aos [segmentos de clientes](create-customer-segments.md) específicos que você definir.

4.  Defina as configurações gerais da campanha na seção **Detalhes da campanha**.
    -   Dê um nome para a campanha publicitária no campo **Nome da campanha**.
    -   Em **Tipo de campanha**, escolha uma destas opções:
        -   **Paga**: os anúncios serão executados em qualquer app que corresponda ao dispositivo e à categoria do seu app. Para novas campanhas criadas após 9 de janeiro de 2017, esses anúncios também serão exibidos no MSN.com, Outlook.com, Skype e outras propriedades premium da Microsoft. Campanhas de promoção de apps destinados a propriedades premium da Microsoft e a apps são conhecidas como campanhas *universais*.
        -   **Comunidade (gratuito)**: esses anúncios serão veiculados em apps publicados por outros desenvolvedores que também criam campanhas publicitárias da comunidade. Antes de poder selecionar essa opção, você deve marcar a caixa **Mostrar anúncios da comunidade em meu app** na página **Monetizar com anúncios** do painel. Para saber mais, consulte [Sobre anúncios da comunidade](about-community-ads.md).
        -   **Domésticos (gratuitos)**: esses anúncios só serão executados em seus apps (que correspondam ao dispositivo do app anunciado). Anúncios domésticos são gratuitos. Para saber mais, veja [Sobre anúncios domésticos](about-house-ads.md).
    -   Em **Duração da campanha**, escolha uma destas opções:
        - **Personalizado**. Se você escolher essa opção, o orçamento da campanha será gasto durante o intervalo de data e hora que você especificar. Essa opção só está disponível para os desenvolvedores com conta premium. Para obter mais informações sobre contas premium, consulte [Perguntas comuns sobre campanhas publicitárias](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign).
        - **Mensal**. Se você escolher essa opção, o orçamento da campanha será gasto por mês de forma recorrente até você parar a campanha.

    > **Observação**  Se seu app ainda não tiver sido publicado, você receberá uma mensagem de erro na página **Nova campanha**. Você deve aguardar seu app ser publicado para poder criar uma campanha publicitária para ele.

5.  Se você escolheu **Aumentar as instalações do app** como seu objetivo de campanha, escolheremos o público-alvo para seus anúncios com base nas configurações que você selecionou ao criar o app no painel do Centro de Desenvolvimento. Se você mesmo quiser escolher o público-alvo para os seus anúncios, selecione **Manual** para expandir a seção **Público-alvo**. Se você quiser voltar para o direcionamento padrão, selecione **Automático**.

    Se selecionar **Manual**, você poderá editar as seguintes informações de direcionamento:

    -   Escolha os países ou regiões em que você quer que esses anúncios apareçam. Você pode escolher até cinco. Para obter uma lista dos países ou regiões com suporte, consulte [Perguntas comuns sobre campanhas publicitárias](common-questions.md#where-will-my-ad-appear).
    -   Escolha os tipos de dispositivo em que você quer que esses anúncios apareçam. Somente os tipos de dispositivo suportados por seu app são mostrados.
    -   Escolha as superfícies em que você quer que esses anúncios apareçam. Se você escolher **Universal**, seu complemento aparecerá dentro de apps, além de MSN.com, Outlook.com, Skype e outras propriedades premium da Microsoft.
    -   Escolha o sistema operacional. Somente os sistemas operacionais com suporte ao seu app são mostrados.
    -   Escolha o sexo e a faixa etária de seu público-alvo desejado.

    Esta seção também exibe um gráfico **Alcance estimado**. Esse gráfico mostra a audiência que você pode alcançar com suas seleções de direcionamento atuais como um percentual de todos os usuários do app habilitado por anúncio do Windows nos mercados selecionados.

6.  Se você escolheu **Aumentar o envolvimento do app** como seu objetivo de campanha, poderá selecionar um dos segmentos de cliente para direcionar.

    > **Observação**  Os anúncios criados usando essa campanha serão mostrados apenas para os clientes que estão incluídos no segmento. Apenas um segmento pode ser selecionado por campanha publicitária. Para obter informações sobre segmentos, consulte [Criar segmentos de clientes](create-customer-segments.md).


7.  Na seção **Design de anúncio**, escolha uma destas opções:
    -   **Personalizado**. Escolha essa opção para usar seus próprios designs de anúncios. Observe que, se você selecionou um segmento de cliente na etapa 6, deverá usar criativos personalizados. Você pode carregar arquivos diferentes para cada um dos tamanhos de anúncios disponíveis. Os arquivos devem cumprir os requisitos e as diretrizes a seguir:
        -   Cada arquivo deve ser um .png ou .jpg com 40 KB ou menos.
        -   Seus designs de anúncios devem atender aos requisitos especificados na [Política de Aceitação de Criatividade da Microsoft](http://go.microsoft.com/fwlink?LinkId=532595).
        -   O conteúdo em seus designs de anúncios deve ser relevante para o app que você está promovendo. Designs de anúncios não relacionadas ao app não serão distribuídos a anúncios em outros apps.
        -   Todo o conteúdo em seus designs de anúncios deve ser claramente legível. Por exemplo, o conteúdo não deve ser desfocado, pixelado ou ampliado.
    -   **Gerado automaticamente**. Escolha essa opção para usar anúncios de uma lista de modelos padrão. Você tem as seguintes opções para personalizar o conteúdo nos anúncios. Conforme você faz as seleções, as visualizações de seus anúncios serão atualizadas automaticamente.
        -   Na lista suspensa **Idioma**, selecione o idioma dos anúncios. O texto do selo da Windows Store e seu texto de slogan personalizado (se especificado) serão exibidos no idioma que você selecionar.
        -   Para adicionar uma linha extra de texto a seu anúncio, digite o texto no campo **Slogan personalizado**.
            > **Observação**  O texto que você insere deve estar traduzido no idioma selecionado. O slogan personalizado será rejeitado se o texto não estiver de acordo com as [Políticas do Bing Ads](http://go.microsoft.com/fwlink?LinkId=398341). Consulte esta página para obter orientação sobre estilo e conteúdo não permitido.

        -   Para personalizar mais o anúncio, expanda **Personalizar design de anúncios/Ver todos os tamanhos de anúncio** e escolha um dos seguintes itens:
            - **Cor da tela de fundo**. Escolha entre as opções disponíveis.
            - **Imagens**. As imagens disponíveis são as que você associou ao seu app na Loja.
            - **Mostrar a classificação do meu app**. Marque essa caixa de seleção se você quiser mostrar a classificação do app.
            - **Mostrar que meu app é gratuito**. Se o seu app for gratuito em todos os mercados selecionados, você também terá a opção de marcar essa caixa de seleção.
            - **Chamada para ação**. Se você escolheu **Aumentar o envolvimento do app** como objetivo da campanha, defina o botão de chamada para ação de seu anúncio como **Abrir**, **Reproduzir**, **Ler**, **Escutar** ou **Comprar**.  

8.  Se você tiver uma [conta premium](common-questions.md#how-can-i-increase-the-maximum-monthly-budget-amount-allowed-for-my-ad-campaign), pode usar a caixa **URL de destino** para controlar o que acontece quando um cliente clica em seu anúncio.
    - Se você deixar a caixa vazia, quando um cliente clicar em seu anúncio, a listagem da Loja do seu app será exibida.
    - Se você estiver usando o Kochava ou o Tune para medir análises de instalação de seu app, insira URL de rastreamento de instalação do Kochava ou do Tune. Quando você salvar a campanha, a URL de rastreamento será validada para garantir que ela seja resolvida como a página de listagem para seu app na Windows Store. Para obter mais informações sobre o controle de instalação com Kochava e Tune, consulte a documentação do [Kochava](http://support.kochava.com/) e do [Tune](https://help.tune.com/).
    - Se você escolheu **Aumentar o envolvimento do app** como objetivo da campanha, pode especificar um [URI de um link profundo](../launch-resume/handle-uri-activation.md) para redirecionar os clientes no segmento selecionado para uma página específica em seu app.
    - Se você especificar um destino que não seja a página de descrição do seu app ou uma página dentro de seu app, sua campanha será pausada automaticamente.

9.  Agora, escolha as configurações financeiras de sua campanha publicitária na seção **Orçamento e pagamento**.
   > **Observação**  Se você estiver criando uma campanha doméstica ou da comunidade, a seção **Orçamento e pagamento** não será exibida, já que essas campanhas são gratuitas.

    -   Em **Orçamento**, use o controle deslizante para definir o valor em dinheiro que você deseja gastar a cada mês para executar esse anúncio.

        O orçamento mensal é proporcional ao mês em que a campanha publicitária é criada. Em outras palavras, se você criar uma campanha publicitária na metade de um mês, será cobrado pela metade de seu orçamento mensal nesse mês.

    -   Defina um método de pagamento para a sua campanha publicitária, clicando em **Adicionar novo método de pagamento** e preencha os detalhes da sua conta.
        > **Importante**  O país/região do endereço de cobrança do seu método de pagamento deve coincidir com o país/região associado à sua conta do Centro de Desenvolvimento.
    -   Se você recebeu um cupom de um representante da Microsoft para pagar uma campanha publicitária, clique em **Usar um cupom**, insira o código do cupom e clique em **Aplicar** para aplicar o cupom à campanha.

10.  Por fim, clique em **Revisão** para confirmar as configurações de sua campanha publicitária e, se for uma campanha publicitária paga, o orçamento e as informações de pagamento. Clique em **Confirmar** e seus anúncios começarão a aparecer normalmente nos dispositivos em algumas horas!
   > **Dica**  Para ver o desempenho de suas campanhas, no menu de navegação superior do painel, selecione **Promoções**. Selecione **Filtros de seção** para definir o escopo que está incluído no relatório por **Data**, **Objetivo da campanha**, **Nome do app**, **Tipo de campanha** ou **Status**. Além de ver informações sobre **Impressões**, **Cliques**, **Conversões** e **Gastos** da sua campanha, você pode usar o relatório para **Pausar** ou **Retomar** uma campanha. Para editar uma campanha, selecione seu nome na lista.

## <a name="related-topics"></a>Tópicos relacionados

* [Gerenciando sua campanha publicitária](managing-your-ad-campaign.md)
* [Sobre anúncios domésticos](about-house-ads.md)
* [Relatório de promoção do seu app](promote-your-app-report.md)
* [Perguntas comuns sobre campanhas publicitárias](common-questions.md)

