---
author: mcleanbyron
Description: The Microsoft Store Services SDK provides libraries and tools that you can use to add features to your apps that help you make more money and gain customers.
title: Envolver os clientes com o Microsoft Store Services SDK
ms.assetid: 518516DB-70A7-49C4-B3B6-CD8A98320B9C
ms.author: mcleans
ms.date: 08/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, Microsoft Store Services SDK
ms.localizationpriority: medium
ms.openlocfilehash: ed40494b8498a1d990df0e4c041b1a81024176f5
ms.sourcegitcommit: b8c77ac8e40a27cf762328d730c121c28de5fbc4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/21/2018
ms.locfileid: "1672803"
---
# <a name="engage-customers-with-the-microsoft-store-services-sdk"></a>Envolver os clientes com o Microsoft Store Services SDK

O Microsoft Store Services SDK fornece recursos que ajudam você a interagir com clientes nos apps da UWP (Plataforma Universal do Windows), como enviar notificações direcionadas do Centro de Desenvolvimento e executar experimentos do tipo A/B em seus aplicativos. Esse SDK é uma extensão do Visual Studio 2015 e versões posteriores do Visual Studio.

> [!NOTE]
> Para exibir anúncios em seus aplicativos UWP, use o [SDK do Microsoft Advertising](http://aka.ms/ads-sdk-uwp) em vez do Microsoft Store Services SDK. As bibliotecas de publicidade foram movidas do Microsoft Store Services SDK para o SDK do Microsoft Advertising. Para obter mais informações, consulte [Exibir anúncios no seu aplicativo](display-ads-in-your-app.md).



## <a name="scenarios-supported-by-the-microsoft-store-services-sdk"></a>Cenários com suporte pelo Microsoft Store Services SDK

Atualmente, o Microsoft Store Services SDK dá suporte aos seguintes cenários para aplicativos UWP. Para obter a documentação de referência de API, consulte [Referência de API do Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx).

|  Cenário  |  Descrição   |
|------------|----------------|
|  [Executar experimentos em seu aplicativo UWP com testes A/B](run-app-experiments-with-a-b-testing.md)    |  Execute testes A/B em seu aplicativo UWP (Plataforma Universal do Windows) para medir a eficácia de recursos em alguns clientes antes de liberar os recursos para todos. Depois de definir um experimento em seu painel do Centro de desenvolvimento, use a classe [StoreServicesExperimentVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesexperimentvariation.aspx) para obter variações de sua experiência em seu aplicativo, use esses dados para modificar o comportamento do recurso que você está testando e, em seguida, use o método [LogForVariation](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.logforvariation.aspx) para enviar eventos de exibição e de conversão ao Centro de Desenvolvimento. Por fim, use o painel para exibir os resultados e gerenciar a experiência.  |
|  [Inicie o Hub de Feedback do seu aplicativo UWP](launch-feedback-hub-from-your-app.md)    |  Use a classe [StoreServicesFeedbackLauncher](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesfeedbacklauncher.aspx) em seu aplicativo UWP para direcionar os clientes do Windows 10 ao Hub de Feedback, onde eles podem enviar problemas, sugestões e aprovações. Em seguida, gerencie esses comentários no [Relatório de comentários](../publish/feedback-report.md) no painel do Centro de Desenvolvimento. |
|  [Configure seu aplicativo UWP para receber notificações por push do Centro de Desenvolvimento](configure-your-app-to-receive-dev-center-notifications.md)    |  Use a classe [StoreServicesEngagementManager](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicesengagementmanager.aspx) em seu aplicativo UWP para registrar o aplicativo para receber notificações por push direcionadas que você envia para seus clientes usando o painel do Centro de Desenvolvimento do Windows.  |
|   [Registre eventos personalizados em seu aplicativo UWP para o relatório de uso no Centro de Desenvolvimento](log-custom-events-for-dev-center.md)   |  Use a classe [StoreServicesCustomEventLogger](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.storeservicescustomeventlogger.log.aspx) em seu aplicativo UWP para registrar eventos personalizados que são associados a seu aplicativo no Centro de Desenvolvimento. Em seguida, examine o total de ocorrências de seus eventos personalizados na seção **Eventos personalizados** do [Relatório de uso](https://msdn.microsoft.com/windows/uwp/publish/usage-report) no painel do Centro de Desenvolvimento.  |

<span id="prerequisites" />

## <a name="prerequisites"></a>Pré-requisitos

O Microsoft Store Services SDK exige:

* Visual Studio 2015 ou uma versão posterior.
* Ferramentas do Visual Studio para aplicativos universais do Windows instalados com sua versão do Visual Studio.

<span id="install" />

## <a name="install-the-sdk"></a>Instalar o SDK

Há duas opções para a instalação do Microsoft Store Services SDK no computador de desenvolvimento:

* **Instalador MSI**&nbsp;&nbsp;Você pode instalar o SDK por meio do instalador MSI disponível [aqui](http://aka.ms/store-em-sdk).
* **Pacote NuGet**&nbsp;&nbsp;Você pode instalar o SDK como um pacote NuGet.

A Microsoft lança periodicamente novas versões do Microsoft Store Services SDK com aperfeiçoamentos de desempenho e novos recursos. Se você tiver projetos existentes que usam o SDK e deseja usar a versão mais recente, baixe e instale a versão mais recente do SDK em seu computador de desenvolvimento.

<span id="install-msi" />

### <a name="install-via-msi"></a>Instalar por meio de MSI

Para instalar o Microsoft Store Services SDK por meio do instalador MSI:

1.  Feche todas as instâncias do Visual Studio.

2. Se você já tiver instalado qualquer versão anterior do SDK do Microsoft Store Engagement and Monetization, do SDK do Universal Ad Client ou da extensão do Ad Mediator, desinstale essas versões do SDK agora. Opcionalmente, abra uma janela do **Prompt de Comando** e execute esses comandos para limpar quaisquer versões mais antigas do SDK do Advertising que possam ter sido instaladas com o Visual Studio, mas que podem não aparecer na lista de programas instalados em seu computador:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Baixe e instale o [Microsoft Store Services SDK](http://aka.ms/store-em-sdk). A instalação pode demorar alguns minutos. Aguarde até o processo terminar.

4.  Reinicie o Visual Studio.

5.  Se você tiver um projeto existente que faça referência a bibliotecas de qualquer versão anterior do Microsoft Store Services SDK, do SDK do Microsoft Advertising, do SDK do Universal Ad Client ou do SDK do Microsoft Store Engagement and Monetization, nós recomendamos que você abra seu projeto no Visual Studio e limpe e recrie seu projeto (em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do seu projeto e escolha **Limpar** e, em seguida, clique com o botão direito do mouse novamente no nó do seu projeto e escolha **Recriar**).

  Caso contrário, se você estiver usando o SDK pela primeira vez em seu projeto, você estará pronto para [adicionar a referência de assembly ao projeto](#references).

<span id="install-nuget" />

### <a name="install-via-nuget"></a>Instalar por meio do NuGet

Para instalar as bibliotecas de SDK da Microsoft Store Services por meio do NuGet:

1.  Feche todas as instâncias do Visual Studio.

2. Se você já tiver instalado qualquer versão anterior do SDK do Microsoft Store Engagement and Monetization, do SDK do Universal Ad Client ou da extensão do Ad Mediator, desinstale essas versões do SDK agora. Opcionalmente, abra uma janela do **Prompt de Comando** e execute esses comandos para limpar quaisquer versões mais antigas do SDK do Advertising que possam ter sido instaladas com o Visual Studio, mas que podem não aparecer na lista de programas instalados em seu computador:
    ```
    MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
    MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
    MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
    ```

3.  Inicie o Visual Studio e abra o projeto no qual você deseja usar as bibliotecas do SDK da Microsoft Store Services.
    > [!NOTE]
    > Se seu projeto já inclui referências de biblioteca de uma instalação MSI anterior do SDK, remova essas referências do seu projeto. Essas referências terão ícones de aviso ao lado delas porque as bibliotecas a que fazem referência foram removidas nas etapas anteriores.

4. No Visual Studio, clique em **Projeto** e **Gerenciar Pacotes NuGet**.

5. Na caixa de pesquisa, digite **Microsoft.Services.Store.Engagement** e instale o pacote Microsoft.Services.Store.Engagement. Quando terminar a instalação do pacote, salve sua solução.
    > [!NOTE]
    > Se a janela **Saída** relatar um erro *Pacote-Instalação* que indica que o caminho especificado é muito longo, talvez seja necessário configurar o NuGet para extrair pacotes em um local alternativo com um caminho mais curto do que o local padrão. Para fazer isso, adicione o valor ```repositoryPath``` a um arquivo nuget.config em seu computador e o atribua a um caminho de pasta curto onde os pacotes NuGet possam ser extraídos. Para obter mais informações, consulte [este artigo](http://docs.nuget.org/ndocs/consume-packages/configuring-nuget-behavior) na documentação do NuGet. Você também pode tentar mover seu projeto do Visual Studio para uma pasta alternativa com um caminho mais curto.

6. Feche a solução do Visual Studio que contém seu projeto e, em seguida, reabra a solução.

7.  Se seu projeto já faz referência a bibliotecas de uma versão anterior do Microsoft Store Services SDK que foi instalado por meio do NuGet e você atualizou seu projeto para uma versão mais recente do SDK, nós recomendamos que você limpe e recrie seu projeto (em **Gerenciador de Soluções**, clique com o botão direito do mouse no nó do seu projeto e escolha **Limpar** e, em seguida, clique com o botão direito do mouse novamente no nó do seu projeto e escolha **Recriar**).

  Caso contrário, se você estiver usando o SDK pela primeira vez em seu projeto, você estará pronto para [adicionar a referência de assembly ao projeto](#references).

<span id="references" />

## <a name="add-the-assembly-reference-to-your-project"></a>Adicione a referência de assembly ao seu projeto

Depois de instalar o Microsoft Store Services SDK por meio do instalador MSI ou NuGet, siga estas instruções para consultar o assembly do SDK em seu projeto UWP.

1. Abra seu projeto no Visual Studio.
    > [!NOTE]
    > Se seu projeto for um aplicativo JavaScript que tem direcionamento **Any CPU**, atualize-o para usar uma saída de compilação de arquitetura específica (por exemplo, **x86**).

2. No **Gerenciador de Soluções**, clique com o botão direito do mouse em **Referências** e selecione **Adicionar Referência...**

3. Em **Gerenciador de Referências**, expanda **Universal do Windows**, clique em **Extensões** e, em seguida, marque a caixa de seleção ao lado de **Microsoft Engagement Framework**. Isso permite que você use as APIs no namespace [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx).

3. Clique em **OK**.

> [!NOTE]
> Se você instalou as bibliotecas do SDK pelo NuGet, seu projeto conterá uma referência a **Microsoft.Services.Store.Engagement**. A referência a **Microsoft.Services.Store.Engagement** representa o pacote NuGet (em vez de bibliotecas dentro dela), e você pode ignorá-la.

<span id="framework" />

## <a name="understanding-framework-packages-in-the-sdk"></a>Noções básicas sobre pacotes de estrutura no SDK

A biblioteca Microsoft.Services.Store.Engagement.dll no Microsoft Store Services SDK está configurada como um *pacote de estrutura*. Esta biblioteca contém as APIs no namespace [Microsoft.Services.Store.Engagement](https://msdn.microsoft.com/library/windows/apps/microsoft.services.store.engagement.aspx).

Como essa biblioteca é um pacote de estrutura, isso significa que, após um usuário instalar uma versão do seu aplicativo que usa essa biblioteca, ela é atualizada automaticamente em seus dispositivos por meio do Windows Update sempre que publicarmos uma nova versão da biblioteca com correções e melhorias de desempenho. Isso ajuda a garantir que seus clientes sempre terão a versão mais recente da biblioteca instalada nos dispositivos deles.

Se nós lançamos uma nova versão do SDK que apresenta novas APIs ou recursos dessa biblioteca, você precisará instalar a versão mais recente do SDK para usar esses recursos. Nesse cenário, você também precisa publicar seu aplicativo atualizado na Loja.

## <a name="related-topics"></a>Tópicos relacionados

* [Referência de API do Microsoft Store Services SDK](https://msdn.microsoft.com/library/windows/apps/mt691886.aspx)
* [Executar experimentos com testes A/B](run-app-experiments-with-a-b-testing.md)
* [Iniciar o Hub de Feedback do seu aplicativo](launch-feedback-hub-from-your-app.md)
* [Configure seu aplicativo para receber notificações por push do Centro de Desenvolvimento](configure-your-app-to-receive-dev-center-notifications.md)
* [Registre eventos personalizados para o Centro de Desenvolvimento](log-custom-events-for-dev-center.md)