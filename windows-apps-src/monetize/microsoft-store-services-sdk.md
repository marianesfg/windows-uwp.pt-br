---
author: mcleanbyron
Description: "O Microsoft Store Services SDK fornece bibliotecas e ferramentas que você pode usar para adicionar recursos aos seus apps que ajudam você a ganhar mais dinheiro e clientes."
title: Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 7a8dcb282ea3df16ee8a12247a07af27cbf65b3a
ms.lasthandoff: 02/07/2017

---

# <a name="microsoft-store-services-sdk"></a>Microsoft Store Services SDK

O Microsoft Store Services SDK fornece recursos que ajudarão você a ganhar mais dinheiro e interagir com clientes nos apps da Plataforma Universal do Windows (UWP), como exibir anúncios e executar experimentos A/B em seus apps. Esse SDK é uma extensão do Visual Studio 2015 e versões posteriores do Visual Studio.

## <a name="scenarios-supported-by-the-sdk"></a>Cenários suportados pelo SDK

O SDK atualmente suporta os seguintes cenários para apps UWP. O SDK será desenvolvido com o tempo para suportar novos cenários de interação e monetização. Para obter a documentação de referência sobre as APIs no SDK, consulte [Referência de API do Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

|  Cenário  |  Descrição   |
|------------|----------------|
|  [Executar experimentos em seu app UWP com testes A/B](run-app-experiments-with-a-b-testing.md)    |  Execute testes A/B em seu app UWP (Plataforma Universal do Windows) para medir a eficácia de recursos em alguns clientes antes de liberar os recursos para todos. Depois de definir um experimento em seu painel do Centro de Desenvolvimento, use a classe [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obter variações de sua experiência em seu app, use esses dados para modificar o comportamento do recurso que você está testando e, em seguida, use o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) para enviar eventos de exibição e de conversão ao Centro de Desenvolvimento. Por fim, use o painel para exibir os resultados e gerenciar a experiência.  |
|  [Inicie o Hub de Feedback do seu app UWP](launch-feedback-hub-from-your-app.md)    |  Use a classe [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) em seu app UWP para direcionar os clientes do Windows 10 ao Hub de Feedback, onde eles podem enviar problemas, sugestões e aprovações. Em seguida, gerencie esses comentários no [Relatório de comentários](../publish/feedback-report.md) no painel do Centro de Desenvolvimento. |
|  [Configure seu app UWP para receber notificações por push do Centro de Desenvolvimento](configure-your-app-to-receive-dev-center-notifications.md)    |  Use a classe [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) em seu app UWP para registrar o app para receber notificações por push direcionadas que você envia para seus clientes usando o painel do Centro de Desenvolvimento do Windows.  |
|   [Registre eventos personalizados em seu app UWP para o relatório de uso no Centro de Desenvolvimento](log-custom-events-for-dev-center.md)   |  Use a classe [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) em seu app UWP para registrar eventos personalizados que são associados a seu app no Centro de Desenvolvimento. Em seguida, examine o total de ocorrências de seus eventos personalizados na seção **Eventos personalizados** do [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento.  |
|  [Apresentar anúncios em seu app UWP](display-ads-in-your-app.md)    |  Use os controles [AdControl](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.adcontrol.aspx) ou [InterstitialAd](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.interstitialad.aspx) em seu app UWP para aumentar sua receita exibindo anúncios em faixa ou anúncios intersticiais.<br/><br/>**Observação**&nbsp;&nbsp;O Microsoft Store Services SDK só dá suporte a apps UWP para Windows 10. Para exibir anúncios em apps Windows 8.1 e no Windows Phone 8. x, use o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk).  |

<span id="prerequisites" />
## <a name="prerequisites"></a>Pré-requisitos

O Microsoft Store Services SDK exige:

* Visual Studio 2015 ou uma versão posterior.
* Ferramentas do Visual Studio para apps universais do Windows instalados com sua versão do Visual Studio.

>**Observação**&nbsp;&nbsp;Para instalar o SDK com o Visual Studio 2015, você deve ter a versão 1.1 ou posterior das Ferramentas do Visual Studio para Aplicativos Universais do Windows instalada. Para obter mais informações sobre essa atualização das Ferramentas do Visual Studio para Aplicativos Universais do Windows, consulte as [notas de versão](http://go.microsoft.com/fwlink/?LinkID=624516).

<span id="install" />
## <a name="install-the-sdk"></a>Instalar o SDK

Há duas opções para instalar o Microsoft Store Services SDK para uso com o Visual Studio 2015 (ou uma versão posterior) em seu computador de desenvolvimento:

* **Instalador MSI**&nbsp;&nbsp;Você pode instalar o SDK por meio do instalador MSI disponível [aqui](http://aka.ms/store-em-sdk). Com essa opção, as bibliotecas do SDK estão instaladas em um local compartilhado no computador de desenvolvimento, de modo que elas podem ser referenciadas por qualquer projeto UWP no Visual Studio.
* **Pacote NuGet**&nbsp;&nbsp;Você pode instalar as bibliotecas do SDK para um projeto UWP específico no Visual Studio usando o NuGet. Com essa opção, as bibliotecas do SDK são instaladas somente para o projeto no qual você instalou o pacote NuGet.

A Microsoft lança periodicamente novas versões do Microsoft Store Services SDK com aperfeiçoamentos de desempenho e novos recursos. Se você tiver projetos existentes que usam o SDK e deseja usar a versão mais recente, baixe e instale a versão mais recente do SDK em seu computador de desenvolvimento.

>**Observação**&nbsp;&nbsp;Para instalar o SDK com o Visual Studio 2015, você deve ter a versão 1.1 ou posterior das Ferramentas do Visual Studio para Aplicativos Universais do Windows instalada. Para obter mais informações sobre essa atualização das Ferramentas do Visual Studio para Aplicativos Universais do Windows, consulte as [notas de versão](http://go.microsoft.com/fwlink/?LinkID=624516).

<span id="install-msi" />
### <a name="install-via-msi"></a>Instalar por meio de MSI

Para instalar o Microsoft Store Services SDK por meio do instalador MSI:

1.  Feche todas as instâncias do Visual Studio 2015 (ou uma versão posterior). Se você já instalou qualquer versão anterior do SDK do Microsoft Advertising, do SDK do Universal AD Client, da extensão Ad Mediator ou do SDK do Microsoft Store Engagement and Monetization, desinstale essas versões do SDK agora.

2.    Abra uma janela do **Prompt de Comando** e execute esses comandos para limpar quaisquer versões mais antigas do SDK do Advertising que possam ter sido instaladas com o Visual Studio, mas que podem não aparecer na lista de programas instalados em seu computador:
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Baixe e instale o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). A instalação pode demorar alguns minutos. Aguarde até o processo terminar.

4.  Reinicie o Visual Studio.

5.  Se você tiver um projeto existente que referencie bibliotecas de qualquer versão anterior do Microsoft Store Services SDK, do SDK do Microsoft Advertising, do SDK do Universal Ad Client ou do SDK do Microsoft Store Engagement and Monetization, nós recomendamos que você abra seu projeto no Visual Studio e limpe e recrie seu projeto (em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do seu projeto e escolha **Limpar** e, em seguida, clique com o botão direito do mouse novamente no nó do seu projeto e escolha **Recriar**).

  Caso contrário, se você estiver usando o SDK pela primeira vez em seu projeto, você estará pronto para [adicionar as referências de biblioteca do Microsoft Store Services SDK apropriadas ao seu projeto](#references).

<span id="install-nuget" />
### <a name="install-via-nuget"></a>Instalar por meio do NuGet

Para instalar as bibliotecas do Microsoft Store Services SDK para um projeto específico por meio do NuGet:

1.  Feche todas as instâncias do Visual Studio 2015 (ou uma versão posterior). Se você já instalou qualquer versão anterior do SDK do Microsoft Advertising, do SDK do Universal AD Client, da extensão Ad Mediator ou do SDK do Microsoft Store Engagement and Monetization, desinstale essas versões do SDK agora.

2.    Abra uma janela do **Prompt de Comando** e execute esses comandos para limpar quaisquer versões mais antigas do SDK do Advertising que possam ter sido instaladas com o Visual Studio, mas que podem não aparecer na lista de programas instalados em seu computador:
  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Inicie o Visual Studio e abra o projeto no qual você deseja usar as bibliotecas do Microsoft Store Services SDK.

  >**Observação**&nbsp;&nbsp;Se seu projeto já inclui referências de biblioteca de uma instalação MSI anterior do SDK, remova essas referências do seu projeto. Essas referências terão ícones de aviso ao lado delas porque as bibliotecas a que fazem referência foram removidas nas etapas anteriores.

4. No Visual Studio, clique em **Projeto** e **Gerenciar Pacotes NuGet**.

5. Na caixa de pesquisa, digite **Microsoft.Services.Store.SDK** e instale o pacote Microsoft.Services.Store.SDK.

  >**Observação**&nbsp;&nbsp;Se a janela **Saída** relatar um erro *Pacote-Instalação* que indica que o caminho especificado é muito longo, talvez seja necessário configurar o NuGet para extrair pacotes em um local alternativo com um caminho mais curto do que o local padrão. Para fazer isso, adicione o valor ```repositoryPath``` a um arquivo nuget.config em seu computador e o atribua a um caminho de pasta curto onde os pacotes NuGet possam ser extraídos. Para obter mais informações, consulte [este artigo](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) na documentação do NuGet. Você também pode tentar mover seu projeto do Visual Studio para uma pasta alternativa com um caminho mais curto.

6. Feche seu projeto e depois reabra-o.

7.  Se seu projeto já faz referência a bibliotecas de uma versão anterior do Microsoft Store Services SDK que foi instalado por meio do NuGet e você atualizou seu projeto para uma versão mais recente do SDK, nós recomendamos que você limpe e recrie seu projeto (em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do seu projeto e escolha **Limpar** e, em seguida, clique com o botão direito do mouse novamente no nó do seu projeto e escolha **Recriar**).

  Caso contrário, se você estiver usando o SDK pela primeira vez em seu projeto, você estará pronto para [adicionar as referências de biblioteca do Microsoft Store Services SDK apropriadas ao seu projeto](#references).

<span id="references" />
## <a name="add-sdk-library-references-to-your-project"></a>Adicione as referências de biblioteca do SDK ao seu projeto

Depois de instalar o Microsoft Store Services SDK por meio do instalador MSI ou NuGet, siga estas instruções para referenciar as bibliotecas do SDK em seu projeto UWP.

1. Abra seu projeto no Visual Studio.

  >**Observação**&nbsp;&nbsp;Se seu projeto for um app JavaScript que tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**).

2. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

3. Em **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e, em seguida, marque a caixa de seleção ao lado de um dos seguintes itens.

  * Para usar as APIs no namespace [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx) para cenários de compromisso do cliente, marque a caixa de seleção ao lado de **Microsoft Engagement Framework**. Escolha esta opção se você quiser [executar experimentos A/B](run-app-experiments-with-a-b-testing.md), [iniciar o Hub de Feedback](launch-feedback-hub-from-your-app.md), [receber notificações por push direcionadas do Centro de Desenvolvimento](configure-your-app-to-receive-dev-center-notifications.md) ou [registrar eventos personalizados no Centro de Desenvolvimento](log-custom-events-for-dev-center.md).

  * Para usar as APIs para [exibir anúncios em faixa ou anúncios intersticiais em seu app](display-ads-in-your-app.md), marque a caixa de seleção ao lado de **SDK do Microsoft Advertising para XAML** ou **SDK do Microsoft Advertising para JavaScript**, dependendo do tipo de projeto.

3. Clique em **OK**.

>**Observação**&nbsp;&nbsp;Se você instalou as bibliotecas do SDK por meio do NuGet, seu projeto conterá uma referência **Microsoft.Services.Store.SDK** além do **SDK do Microsoft Advertising para XAML** ou **SDK do Microsoft Advertising para JavaScript**. A referência **Microsoft.Services.Store.SDK** representa o pacote NuGet (em vez de bibliotecas dentro dela), e você pode ignorá-la.

<span id="framework" />
## <a name="understanding-framework-packages-in-the-sdk"></a>Noções básicas sobre pacotes de estrutura no SDK

As seguintes bibliotecas no Microsoft Store Services SDK são configuradas como *pacotes de estrutura*:

* Microsoft.Advertising.dll. Essa biblioteca contém as APIs de publicidade nos namespaces [Microsoft.Advertising](https://msdn.microsoft.com/library/windows/apps/mt313187.aspx) e [Microsoft.Advertising.WinRT.UI](https://msdn.microsoft.com/library/windows/apps/microsoft.advertising.winrt.ui.aspx).
* Microsoft.Services.Store.Engagement.dll. Esta biblioteca contém as APIs no namespace [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx).

Isso significa que, após um usuário instalar uma versão do seu app que usa essas bibliotecas, essas bibliotecas são atualizadas automaticamente em seus dispositivos por meio do Windows Update sempre que publicarmos novas versões das bibliotecas com correções e melhorias de desempenho. Isso ajuda a garantir que seus clientes sempre terão a versão mais recente das bibliotecas instaladas nos dispositivos deles.

Se nós lançamos uma nova versão do SDK que apresenta novas APIs ou recursos dessas bibliotecas, você precisará instalar a versão mais recente do SDK para usar esses recursos. Nesse cenário, você também precisa publicar seu app atualizado na Loja.

## <a name="related-topics"></a>Tópicos relacionados

* [Referência de API do Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Executar experimentos com testes A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar o Hub de Feedback do seu app](launch-feedback-hub-from-your-app.md)
* [Configure seu app para receber notificações por push do Centro de Desenvolvimento](configure-your-app-to-receive-dev-center-notifications.md)
* [Registrar eventos personalizados para o Centro de Desenvolvimento](log-custom-events-for-dev-center.md)
* [Apresentar anúncios em seus apps](display-ads-in-your-app.md)

