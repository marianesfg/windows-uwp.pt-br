---
author: Xansky
ms.assetid: 9ca1f880-2ced-46b4-8ea7-aba43d2ff863
description: Saiba mais sobre problemas conhecidos para a versão atual das bibliotecas do SDK do Microsoft Advertising.
title: Problemas conhecidos e solução de problemas para anúncios em aplicativos
ms.author: mhopkins
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, anúncios, publicidade, problemas conhecidos, solução de problemas
ms.localizationpriority: medium
ms.openlocfilehash: d1b3b1fb68ed246d6a5a8334c5cf4d1c0754b719
ms.sourcegitcommit: ed0304b8a214c03b8aab74b8ef12c9f82b8e3c5f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/19/2018
ms.locfileid: "7292945"
---
# <a name="known-issues-and-troubleshooting-for-ads-in-apps"></a>Problemas conhecidos e solução de problemas para anúncios em aplicativos

Esta tópico lista os problemas conhecidos com a versão atual do SDK do Microsoft Advertising. Para obter instruções adicionais de solução de problemas, consulte os seguintes tópicos.

* [Guia de solução de problemas em HTML e JavaScript](html-and-javascript-troubleshooting-guide.md)
* [Guia de solução de problemas de XAML e C#](xaml-and-c-troubleshooting-guide.md)

## <a name="adcontrol-interface-unknown-in-xaml"></a>Interface AdControl desconhecida em XAML

A marcação de XAML para um [AdControl](https://docs.microsoft.com/uwp/api/microsoft.advertising.winrt.ui.adcontrol) pode mostrar incorretamente uma linha curva azul indicando que a interface é desconhecida. Isso ocorre apenas no direcionamento x86 e pode ser ignorado.

## <a name="lasterror-from-previous-ad-request"></a>lastError da solicitação de anúncio anterior

Se houver um **lastError** restante da solicitação de anúncio anterior, o evento poderá ser disparado duas vezes durante a próxima chamada de anúncios. Embora a nova solicitação de anúncio ainda será feita e poderá render um anúncio válido, esse comportamento pode causar confusão.

## <a name="interstitial-ads-and-navigation-buttons-on-phones"></a>Anúncios intersticiais e botões de navegação em telefones

Em telefones (ou emuladores) que têm os botões **Voltar**, **Iniciar** e **Pesquisar** de software em vez de botões de hardware, o temporizador de contagem regressiva e botões de clique para anúncios intersticiais podem ficar obscuros.

## <a name="recently-created-ads-are-not-being-served-to-your-app"></a>Anúncios criados recentemente não estão sendo fornecidos para seu aplicativo

Se você tiver criado um anúncio recentemente (menos de um dia), talvez ele não fique disponível imediatamente. Se o conteúdo editorial do anúncio for aprovado, ele será disponibilizado depois que o servidor de publicidade processá-lo e o anúncio estiver disponível como inventário.

## <a name="no-ads-are-shown-in-your-app"></a>Nenhum anúncio é mostrado em seu aplicativo

Há muitos motivos para você não ver anúncios, incluindo erros de rede. Outros motivos podem incluir:

* Selecionar uma unidade de anúncio no Partner Center com um tamanho maior ou menor que o tamanho do **AdControl** no código do seu aplicativo.

* Os anúncios não aparecerão se você estiver usando um [valor de modo de teste](set-up-ad-units-in-your-app.md#test-ad-units) para seu ID de unidade de anúncios ao executar um aplicativo dinâmico.

* Se você criou uma nova ID de unidade de anúncios na última meia hora, talvez você não a veja até que os servidores propaguem novos dados por meio do sistema. Os IDs existentes que tenham mostrado anúncios antes devem mostrar anúncios imediatamente.

Se você pode ver anúncios de teste no aplicativo, seu código está funcionando e é capaz de exibir anúncios. Se você tiver problemas, entre em contato com o [suporte do produto](https://developer.microsoft.com/en-us/windows/support). Nessa página, escolha **Anúncios em Apps**.

Você também pode postar uma pergunta no [fórum](http://go.microsoft.com/fwlink/p/?LinkId=401266).

## <a name="test-ads-are-showing-in-your-app-instead-of-live-ads"></a>Anúncios de teste estão aparecendo em seu aplicativo em vez de anúncios ativos

Anúncios de teste podem ser mostrados, mesmo quando você está esperando anúncios ativos. Isso pode acontecer nos seguintes cenários:

* Nossa plataforma de publicidade não pode verificar ou encontrar a ID do aplicativo ao vivo usada na loja. Nesse caso, quando uma unidade de anúncios é criada por um usuário, seu status pode iniciar como dinâmico (não teste) mas passará para status de teste 6 horas após a primeira solicitação de anúncio. Ele mudará novamente para ativo se não houver nenhum solicitação dos aplicativos de teste por 10 dias.

* Aplicativos de sideload ou aplicativos que estão em execução no emulador não mostrarão anúncios ativos.

Quando uma unidade de anúncio em tempo real estiver fornecendo anúncios de teste, o status da unidade de anúncio mostra **ativo e fornecendo anúncios de teste** no Partner Center. Isso não se aplica atualmente aos aplicativos de telefone.


<span id="reference_errors"/>

## <a name="reference-errors-caused-by-targeting-any-cpu-in-your-project"></a>Erros de referência causados pelo direcionamento de Nenhuma CPU em seu projeto

Ao usar as bibliotecas do SDK do Microsoft Advertising, você não pode direcionar para **Qualquer CPU** em seu projeto. Se o seu projeto for direcionado para a plataforma **Any CPU**, você poderá ver um aviso depois de adicionar a referência semelhante a esta.

![referenceerror\ solutionexplorer](images/13-19629921-023c-42ec-b8f5-bc0b63d5a191.jpg)

Para remover esse aviso, atualize seu projeto para usar uma saída de compilação específica da arquitetura (por exemplo, **x86**). Use o **Gerenciador de Configurações** para definir os destinos de plataforma para depuração e configurações de versão.

![configurationmanagerwin10](images/13-87074274-c10d-4dbd-9a06-453b7184f8de.png)

Quando você cria pacotes do aplicativo para envio de armazenamento (como mostrado nas imagens a seguir), certifique-se de incluir as arquiteturas para as quais você pretende direcionar. Você pode optar por ignorar x64 se pretende executar compilações x86 no sistema operacional x64.

![projectstorecreateapppackages](images/13-a99b05a4-8917-4c53-822e-2548fadf828a.png)

![createapppackages](images/13-16280cb1-a838-42b9-9256-eac7f33f5603.png)

## <a name="z-order-in-javascripthtml-apps"></a>Ordem Z em aplicativos JavaScript/HTML

Os aplicativos JavaScript/HTML não devem colocar elementos no intervalo de 10 MAX reservado da ordem z. A única exceção é uma sobreposição de interrupção, como uma notificação de chamada de entrada para um aplicativo do Skype.

<span id="bkmk-ui"/>

## <a name="do-not-use-borders"></a>Não use bordas

Definir propriedades relacionadas a borda herdada do **AdControl** da classe pai fará com que o posicionamento de anúncios fique incorreto.

## <a name="more-information"></a>Mais informações

Para obter mais informações sobre os problemas conhecidos mais recentes e publicar perguntas relacionadas às bibliotecas do SDK do Microsoft Advertising, visite o [fórum](http://go.microsoft.com/fwlink/p/?LinkId=401266).

 

 
