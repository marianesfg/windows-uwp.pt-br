---
title: Monetização para jogos
description: Implemente anúncios em faixa, anúncios intersticiais em vídeo e compras realizadas em app para jogos da Plataforma Universal do Windows (UWP) no Windows 10.
ms.assetid: 79f4e177-d8e7-45d3-8a78-31d4c2fe298a
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, monetização
ms.localizationpriority: medium
ms.openlocfilehash: baef3750b8dc076fd78c239cfc61662256e57c14
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75685248"
---
#  <a name="monetization-for-games"></a>Monetização para jogos

Como desenvolvedor de jogos, você precisa saber as opções de monetização para poder manter os negócios e continuar fazendo aquilo pelo qual tem paixão: criar jogos excelentes. Este artigo apresenta uma visão geral dos métodos de monetização para um jogo da Plataforma Universal do Windows (UWP) e como implementá-los.

Antigamente, bastaria dar um preço para o jogo e esperar que as pessoas o comprassem em uma loja. Porém, existem opções atualmente. É possível optar por distribuir um jogo para lojas de "tijolo e cimento", vendê-lo online (cópias físicas ou virtuais) ou permitir que as pessoas joguem-no gratuitamente, mas incorporando algum tipo de anúncio ou item no jogo que possam ser comprado. Jogos também não são mais apenas produtos autônomos. Eles normalmente acompanham conteúdo extra que pode ser comprado além do jogo principal.

É possível promover e monetizar um jogo UWP em uma ou mais das seguintes maneiras:
* Coloque seu jogo na Microsoft Store, que é uma loja online segura que oferece [distribuição mundial](#worldwide-distribution-channel). Jogadores em todo o mundo podem comprar o jogo online ao [preço que você definir](#set-a-price-for-your-game).
* Use APIs no SDK do Windows para criar [compras no jogo](#in-game-purchases). Os jogadores podem comprar itens dentro do jogo, ou comprar conteúdo adicional, como equipamentos extras, capas, mapas ou níveis de jogo.
* Use APIs no [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) para exibir anúncios de redes de publicidade. É possível [exibir anúncios no jogo](#display-ads-in-your-game) e oferecer a opção para os jogadores assistirem a anúncios em vídeo em troca de premiações no jogo.
* [Maximize o potencial do jogo por meio de campanhas publicitárias](#maximize-your-games-potential-through-ad-campaigns). Promova o jogo usando campanhas publicitárias pagas, comunitárias (gratuitas) ou domésticas (gratuitas) para ampliar a base de usuários.

## <a name="worldwide-distribution-channel"></a>Canal de distribuição em todo o mundo

O Microsoft Store pode disponibilizar seu jogo para download em mais de 200 países e regiões em todo o mundo, com suporte para cobrança por meio de várias formas de pagamento, incluindo Visa, MasterCard e PayPal. Para obter uma lista completa de países e regiões, consulte [definir seleção de mercado](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection).

## <a name="set-a-price-for-your-game"></a>Defina um preço para o jogo

Jogos UWP publicados na Loja podem ser _pagos_ ou _gratuitos_. Um jogo pago permite cobrar jogadores com antecedência pelo jogo a um preço definido por você, e um jogo gratuito permite que os usuários baixem e joguem o jogo sem pagar por ele.

Aqui estão alguns conceitos importantes a respeito do preço do jogo na Loja.

### <a name="base-price"></a>Preço de base

O preço base do jogo é o que determina se o jogo é categorizado como _pago_ ou _gratuito_. Você pode usar o [Partner Center](https://partner.microsoft.com/dashboard) para configurar o preço base com base no país e na região.
O processo de determinação do preço pode incluir as [responsabilidades tributárias na venda para países diferentes](https://docs.microsoft.com/windows/uwp/publish/tax-details-for-paid-apps) e [considerações sobre custo para mercados específicos](https://docs.microsoft.com/windows/uwp/publish/define-pricing-and-market-selection). Também é possível [definir preços personalizados para mercados específicos](../publish/set-and-schedule-app-pricing.md#override-base-price-for-specific-markets).

### <a name="sale-price"></a>Preço de venda

Uma maneira de promover o jogo é reduzir o preço por um tempo limitado. Também é possível definir o preço de venda como __Grátis__ para permitir que o jogo seja baixado sem pagamento.
É possível agendar campanhas de venda com antecedência definindo as datas inicial e final da venda. Para obter mais informações, consulte [Colocar aplicativos e complementos à venda](https://docs.microsoft.com/windows/uwp/publish/put-apps-and-add-ons-on-sale).

## <a name="in-game-purchases"></a>Compras no jogo

Compras no jogo são produtos comprados dentro de um jogo. Eles também são genericamente conhecidos como _compras realizadas em aplicativo_. No Microsoft Store, esses produtos são chamados _de_Complementos. Os complementos [são publicados](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions) por meio do Partner Center. Você também precisará habilitar os complementos no código do jogo.

### <a name="types-of-add-ons"></a>Tipos de complementos

É possível criar dois tipos de complementos na loja: _duráveis_ ou _consumíveis_. Duráveis são itens que persistem por um período especificado e que só podem ser comprados uma vez até expirarem. Consumíveis são itens que podem ser comprados e usados reiteradas vezes.

Ao criar consumíveis, decida como deseja acompanhá-los &mdash; ou seja, se eles são _gerenciados pelo desenvolvedor_ ou _gerenciados pela Loja_ (esse recurso estará disponível a partir do Windows 10, versão 1607). Com um consumo gerenciado por desenvolvedor, você é responsável por acompanhar o saldo do item para o jogador; com um consumo gerenciado por loja, o Microsoft Store controla o saldo do item para você. Para obter mais informações, consulte [Visão geral dos complementos consumíveis](https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases).

### <a name="create-in-game-purchases"></a>Criar compras no jogo

As compras realizadas em aplicativo e as APIs de informações de licença mais recentes fazem parte do namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) no SDK do Windows (a partir do Windows 10, versão 1607). Se você estiver desenvolvendo um novo jogo segmentado para a versão 1607 ou posterior, recomendaremos usar o namespace __Windows.Services.Store__ porque ele dá suporte aos tipos de complemento mais recentes e tem um desempenho melhor.
Ele também foi projetado para ser compatível com os tipos futuros de produtos e recursos com suporte no Partner Center e na loja. Ao desenvolver para versões anteriores do Windows 10, use o namespace [Windows.ApplicationModel.Store](https://docs.microsoft.com/uwp/api/windows.applicationmodel.store) em vez disso.

Para obter mais informações, vá até [Compras no aplicativo e avaliações](https://docs.microsoft.com/windows/uwp/monetize/in-app-purchases-and-trials).

#### <a name="simplified-purchase-example"></a>Exemplo de compra simplificada

Esta seção usa um exemplo de compra simplificada para ilustrar o uso de chamadas de método diferentes a fim de implementar o fluxo de compra.

|Ações no jogo/atividade                                                | Tarefas em segundo plano do jogo                  |
|--------------------------------------------------------------------------|----------------------------------------|
|O jogador entra em uma loja. O menu da loja aparece para exibir os complementos disponíveis e o preço de compra |  O jogo [recupera as informações do produto](https://docs.microsoft.com/windows/uwp/monetize/get-product-info-for-apps-and-add-ons) dos complementos, [determina se os complementos têm a licença indicada](https://docs.microsoft.com/windows/uwp/monetize/get-license-info-for-apps-and-add-ons) e exibe os complementos disponíveis para compra pelo jogador no menu da loja.                           |
|O jogador clica em __Comprar__ para comprar um item             |A ação __Comprar__ envia uma solicitação para comprar o item e inicia o processo de pagamento para adquiri-lo. A implementação varia de acordo com o tipo de item. Se for [um item de compra durável ou avulso](https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-purchases-of-apps-and-add-ons), o cliente só poderá ter um único item até ele expirar. Se o item for um [consumível](https://docs.microsoft.com/windows/uwp/monetize/enable-consumable-add-on-purchases), o cliente poderá ter um ou mais dele. |

### <a name="test-in-game-purchases-during-game-development"></a>Teste as compras no jogo durante o desenvolvimento do jogo

Como um complemento deve ser criado em associação a um jogo, o jogo deve ser publicado e disponibilizado na Loja. As etapas nesta seção mostram como criar complementos enquanto o jogo ainda está em desenvolvimento.
(Se o jogo concluído já estiver na Loja, será possível ignorar as três primeiras etapas e ir diretamente para [Criar um complemento na Loja](#create-an-add-on-in-the-store).)

Para criar complementos enquanto o jogo ainda estiver em desenvolvimento:
1. [Criar um pacote](#create-a-package)
2. [Publicar o jogo como oculto](#publish-the-game-as-hidden)
3. [Associar sua solução de jogos no Visual Studio à loja](#associate-your-game-solution-with-the-store)
4. [Criar um complemento na loja](#create-an-add-on-in-the-store)

#### <a name="create-a-package"></a>Crie um pacote

Para ser publicado, qualquer jogo deve atender aos requisitos mínimos da Certificação de Aplicativos Windows. É possível usar o [Kit de Certificação de Aplicativos Windows](https://docs.microsoft.com/windows/uwp/debug-test-perf/windows-app-certification-kit), que faz parte do SDK do Windows 10, para executar testes no jogo a fim de ajudar a garantir que ele esteja pronto para publicação na Loja. Se você ainda não tiver baixado o SDK do Windows 10, que inclui o Kit de Certificação de Aplicativos Windows, vá até [SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk).

Para criar um pacote que possa ser carregado na Loja:

1. Abra a solução do jogo no Visual Studio.
2. Dentro do Visual Studio, vá até __Projeto__ > __Loja__ > __Criar Pacotes de Aplicativos...__
3. Para a opção deseja __criar pacotes para carregar na Microsoft Store?__ , selecione __Sim__.
4. Entre em sua conta de desenvolvedor do [Partner Center](https://partner.microsoft.com/dashboard) . Ou [registre-se](https://developer.microsoft.com/store/register) para obter uma conta de desenvolvedor, se ainda não tiver uma.
5. Selecione um aplicativo cujo pacote de carregamento você deseja criar. Se você ainda não tiver criado um envio de aplicativo, dê um novo nome de aplicativo para criar um novo envio. Para obter mais informações, consulte [Crie seu aplicativo reservando um nome](https://docs.microsoft.com/windows/uwp/publish/create-your-app-by-reserving-a-name).
6. Depois que o pacote tiver sido criado com êxito, clique em __Iniciar o Kit de Certificação de Aplicativos Windows__ para iniciar o processo de teste.
7. Corrija todos os erros para criar um pacote de jogo.

#### <a name="publish-the-game-as-hidden"></a>Publique o jogo como oculto

1. Acesse o [Partner Center](https://partner.microsoft.com/dashboard) e entre.
2. Na página __Visão geral do painel__ ou __Todos os aplicativos__, clique no aplicativo com o qual você deseja trabalhar. Se você ainda não tiver criado um envio de aplicativo, clique em __Criar um novo aplicativo__ e reserve um nome.
3. Na página __Visão geral do aplicativo__, clique em __Iniciar seu envio__.
4. Configure esse novo envio. Na página de envio:
    * Clique em __Preço e disponibilidade__. Na seção __visibilidade__ , escolha '__ocultar este aplicativo e impedir a aquisição...__ ' para garantir que apenas sua equipe de desenvolvimento tenha acesso ao jogo. Para saber mais detalhes, vá até [Distribuição e visibilidade](https://docs.microsoft.com/windows/uwp/publish/set-app-pricing-and-availability).
    * Clique em __Propriedades__. Na seção __Categoria e subcategoria__, escolha __Jogos__ e uma subcategoria indicada para o jogo.
    * Clique em __Age ratings__. Preencha o questionário com precisão.
    * Clique em __Pacotes__. Carregue o pacote do jogo criado na etapa anterior.
5. Siga os outros avisos de envio no painel para publicar com êxito este jogo, que permanece oculto para o público.
6. Clique em __Enviar à Loja__.

Para obter mais informações, vá até [Envios de aplicativos](https://docs.microsoft.com/windows/uwp/publish/app-submissions).

Depois de ser enviado para a Loja, o jogo entrará no [processo de certificação de aplicativo](https://docs.microsoft.com/windows/uwp/publish/the-app-certification-process). Esse processo pode demorar até 16 horas até o jogo ser listado.

#### <a name="associate-your-game-solution-with-the-store"></a>Associe a solução do jogo à Loja

Com a solução do jogo aberta no Visual Studio:

1. Vá até __Projeto__ > __Loja__ > __Associar o aplicativo à Loja...__
2. Entre em sua conta de desenvolvedor do Partner Center e selecione o nome do aplicativo com o qual associar essa solução.
3. Clique duas vezes no arquivo __Package.appxmanifest.xml__ e vá até a guia __Empacotamento__ para verificar se o jogo está associado corretamente.

Se você tiver associado a solução a um jogo publicado que seja dinâmico e esteja listado na Loja, a solução terá uma licença ativa e você estará uma etapa mais próxima da criação de complementos para o jogo. Para obter mais informações, consulte [Empacotando aplicativos](https://docs.microsoft.com/windows/uwp/packaging/index).

#### <a name="create-an-add-on-in-the-store"></a>Crie um complemento na Loja

À medida que você cria complementos, certifique-se de que eles estejam associados ao envio de jogo certo. Para saber mais detalhes sobre como configurar todas as diversas informações associadas a um complemento, consulte [Envios de complemento](https://docs.microsoft.com/windows/uwp/publish/add-on-submissions).

1. Acesse o [Partner Center](https://partner.microsoft.com/dashboard) e entre.
2. Na página __Visão geral do painel__ ou __Todos os aplicativos__, clique no aplicativo para o qual você deseja criar o complemento.
3. Na página __Visão geral do aplicativo__, na seção __Complementos__, selecione __Criar um novo complemento__.
4. Selecione o tipo de produto do complemento: __consumível gerenciado pelo desenvolvedor__, __consumível gerenciado pela loja__ ou __durável__.
5. Insira uma ID do produto (product ID) exclusiva que será usada como uma variável de cadeia de caracteres durante a integração desse complemento ao código do jogo. Essa ID não será vista por consumidores. Para obter mais informações, consulte [Definir seu tipo de produto e ID do produto (product ID) do aplicativo](https://docs.microsoft.com/windows/uwp/publish/set-your-add-on-product-id).

Entre outras configurações de complementos estão:
* [Propriedades](https://docs.microsoft.com/windows/uwp/publish/enter-add-on-properties)
* [Preços e disponibilidade](https://docs.microsoft.com/windows/uwp/publish/set-add-on-pricing-and-availability)
* [Listagem de repositório](https://docs.microsoft.com/windows/uwp/publish/create-add-on-store-listings)

Se o seu jogo tiver muitos Complementos, você poderá criá-los programaticamente usando a __API de envio Microsoft Store__. Para obter mais informações, consulte [criar e gerenciar envios usando Microsoft Store Services](https://docs.microsoft.com/windows/uwp/monetize/create-and-manage-submissions-using-windows-store-services).

## <a name="display-ads-in-your-game"></a>Exiba anúncios no jogo

As bibliotecas e as ferramentas no SDK do Microsoft Advertising ajudam a configurar um serviço no jogo para receber anúncios de uma rede de publicidade. Os jogadores verão anúncios ativos e você ganhará dinheiro junto aos anunciantes quando eles virem ou interagirem com os anúncios exibidos.
Para obter mais informações, consulte [Exibir anúncios no seu aplicativo](../monetize/display-ads-in-your-app.md).

### <a name="ad-formats"></a>Formatos de anúncio

Vários tipos de anúncios podem ser exibidos usando o SDK do Microsoft Advertising:

* Anúncios em faixa &mdash; Anúncios que ocupam uma parte da tela de jogos e normalmente são colocados dentro de um jogo.
* Anúncios intersticiais em vídeo &mdash; Anúncios de tela inteira, que podem ser muito efetivos quando usados entre níveis. Se implementados corretamente, eles poderão ser menos intrusivos do que anúncios em faixa.
* Anúncios nativos: anúncios baseados em componentes, em que cada parte do criativo do anúncio (como título, imagem, descrição e texto do plano de ação) é entregue ao aplicativo como um elemento individual que pode ser integrado a seu aplicativo.

### <a name="which-ads-are-displayed"></a>Quais anúncios são exibidos?

Por padrão, o aplicativo exibirá anúncios da rede da Microsoft para anúncios pagos. Para maximizar a receita do anúncio, você pode habilitar o controle de anúncios da unidade publicitária para exibir anúncios de outras redes de publicidade pagas. Para obter mais informações sobre as ofertas atuais, consulte nossa diretriz [controle de anúncios](../publish/in-app-ads.md#mediation).

### <a name="which-markets-allow-ads-to-be-displayed"></a>Quais mercados permitem a exibição de anúncios?

Para obter a lista completa de países e regiões que dão suporte a anúncios, consulte [Mercados com suporte para redes de publicidade](../publish/in-app-ads.md#network-markets).

### <a name="apis-for-displaying-ads"></a>APIs para exibir anúncios

As classes [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol), [InterstitialAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.interstitialad) e [NativeAd](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.nativead) no SDK do Microsoft Advertising são usadas para ajudar na exibição de anúncios em jogos.

Para começar, baixe e instale o [SDK do Microsoft Advertising](https://marketplace.visualstudio.com/items?itemName=AdMediator.MicrosoftAdvertisingSDK) com o Visual Studio 2015 ou uma versão posterior. Para obter mais informações, consulte [Instalar o SDK do Microsoft Advertising](../monetize/install-the-microsoft-advertising-libraries.md).

#### <a name="implementation-guides"></a>Guias de implementação

Estas instruções passo a passo mostram como implementar anúncios usando __AdControl__, __InterstitialAd__ e __NativeAd__:

* [Criar anúncios de faixa em XAML e .NET](https://docs.microsoft.com/windows/uwp/monetize/adcontrol-in-xaml-and--net)
* [Criar anúncios em faixa em HTML5 e JavaScript](https://docs.microsoft.com/windows/uwp/monetize/adcontrol-in-html-5-and-javascript)
* [Criar anúncios do intersticial](https://docs.microsoft.com/windows/uwp/monetize/interstitial-ads)
* [Criar anúncios nativos](https://docs.microsoft.com/windows/uwp/monetize/native-ads)

Durante o desenvolvimento, é possível usar os [valores de unidade publicitária de teste](../monetize/test-mode-values.md) para ver como os anúncios são renderizados. Esses valores de unidade publicitária de teste também são usados nas instruções passo a passo acima.

Aqui estão algumas práticas recomendadas para ajudar no processo de design e implementação.

* [Práticas recomendadas para anúncios em faixa](https://docs.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)
* [Práticas recomendadas para anúncios intersticiais](https://docs.microsoft.com/windows/uwp/monetize/ui-and-user-experience-guidelines)

Para obter soluções para problemas de desenvolvimento comuns, como anúncios não exibidos, caixa preta piscando e desaparecendo ou anúncios não atualizados, consulte [Guia de solução de problemas](https://docs.microsoft.com/windows/uwp/monetize/troubleshooting-guides).

### <a name="prepare-for-release-by-replacing-ad-unit-test-values"></a>Prepare-se para o lançamento substituindo valores de teste da unidade de anúncio

Quando estiver pronto para avançar ao teste dinâmico ou para receber anúncios em jogos publicados, você deverá atualizar os valores da unidade de anúncio de teste para os valores reais fornecidos para o jogo. Para criar unidades de anúncio para o jogo, consulte [Configurar unidades de anúncios em seu aplicativo](https://docs.microsoft.com/windows/uwp/monetize/set-up-ad-units-in-your-app).

### <a name="other-ad-networks"></a>Outras redes de publicidade

Estas são outras redes de publicidade que fornecem SDKs para veiculação de anúncios em aplicativos UWP e jogos.

#### <a name="vungle"></a>Vungle

O SDK do Vungle para Windows oferece anúncios em vídeo em aplicativos e jogos. Para baixar o SDK, vá até [SDK do Vungle](https://publisher.vungle.com/sdk/).

#### <a name="smaato"></a>Smaato

A Smaato permite que os anúncios em faixa sejam incorporados a aplicativos UWP e jogos. Baixe o [SDK](https://www.smaato.com/resources/sdks/) e, para obter mais informações, consulte a [documentação](https://wiki.smaato.com/display/SPX/Windows+Phone).

#### <a name="adduplex"></a>AdDuplex

É possível usar o AdDuplex para implementar anúncios em faixa ou anúncios intersticiais no jogo.

Para saber mais sobre a integração do AdDuplex diretamente a um projeto XAML do Windows 10, vá até o site do AdDuplex:
* Anúncios em faixa: [SDK do Windows 10 para XAML](https://adduplex.zendesk.com/hc/articles/204849031-Windows-10-SDK-for-XAML-apps-installation-and-usage)
* Anúncios intersticiais: [Instalação e uso do anúncio intersticial do AdDuplex XAML do Windows 10](https://adduplex.zendesk.com/hc/articles/204849091-Windows-10-XAML-AdDuplex-Interstitial-Ad-Installation-and-Usage)

Para obter informações sobre como integrar o SDK do AdDuplex a jogos UWP do Windows 10 criados usando-se o Unity, consulte [Instalação e uso de aplicativos do SDK do Windows para Unity](https://adduplex.zendesk.com/hc/articles/207279435-Windows-10-SDK-for-Unity-apps-installation-and-usage).

## <a name="maximize-your-games-potential-through-ad-campaigns"></a>Maximize o potencial do jogo por meio de campanhas publicitárias

Dê o próximo passo na promoção do jogo usando anúncios. Quando você [criar uma campanha publicitária](https://docs.microsoft.com/windows/uwp/publish/create-an-ad-campaign-for-your-app) para o jogo, outros aplicativos e jogos exibirão anúncios promovendo o jogo.

Escolha dentre vários tipos de campanhas que possam ajudar a aumentar a base de jogadores.

|Tipo de campanha             | Anúncios nos quais o jogo deve ser exibido...                                                                                                                                                                   |
|--------------------------|--------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
|Pagos                      |Aplicativos que correspondam ao dispositivo ou à categoria do jogo.                                                                                                                                                   |
|Gratuitos da comunidade            |Aplicativos publicados por outros desenvolvedores que também optaram por campanhas de anúncio da comunidade. Para obter mais informações, consulte [Sobre anúncios de comunidade](https://docs.microsoft.com/windows/uwp/publish/about-community-ads).|
|Domésticos gratuitos                |Somente aplicativos que você tenha publicado. Para obter mais informações, consulte [Sobre anúncios domésticos](https://docs.microsoft.com/windows/uwp/publish/about-house-ads).                                                            |

## <a name="related-links"></a>Links relacionados

* [Recebendo pagamentos](https://docs.microsoft.com/windows/uwp/publish/getting-paid-apps)
* [Tipos de conta, locais e taxas](https://docs.microsoft.com/windows/uwp/publish/account-types-locations-and-fees)
* [Análise](https://docs.microsoft.com/windows/uwp/publish/analytics)
* [Globalização e localização](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Implementar uma versão de avaliação do seu aplicativo](https://docs.microsoft.com/windows/uwp/monetize/implement-a-trial-version-of-your-app)
* [Executar experimentos de aplicativos com testes A/B](https://docs.microsoft.com/windows/uwp/monetize/run-app-experiments-with-a-b-testing)
