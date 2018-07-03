---
author: normesta
description: Este guia ajuda você a criar interfaces do usuário UWP fluentes diretamente em seus aplicativos do WPF e Windows Forms
title: Hospedar controles UWP em aplicativos WPF e Windows Forms
ms.author: normesta
ms.date: 05/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp, windows forms, wpf
keywords: windows 10, uwp, windows forms, wpf
ms.localizationpriority: medium
ms.openlocfilehash: 4823654bce3373ace5b04ced8ec14c4b6c1b6f1d
ms.sourcegitcommit: 3500825bc2e5698394a8b1d2efece7f071f296c1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/09/2018
ms.locfileid: "1862065"
---
# <a name="host-uwp-controls-in-wpf-and-windows-forms-applications"></a>Hospedar controles UWP em aplicativos WPF e Windows Forms

> [!NOTE]
> Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

Estamos trazendo os controles da UWP até a área de trabalho para que você possa melhorar a aparência e a funcionalidade dos aplicativos atuais do WPF ou do Windows com recursos de Design Fluente. Existem duas maneiras de fazer isso.

Primeiro, você pode adicionar controles diretamente à superfície de design do seu projeto do WPF ou do Windows Forms e usá-los como qualquer outro controle no designer.  Tente o seguinte com o novo controle **WebView**. Esse controle usa o mecanismo de renderização do Microsoft Edge e, até agora, esse controle estava disponível somente para aplicativos UWP. Você pode encontrar o **WebView** na versão mais recente do [Kit de Ferramentas da Comunidade do Windows](https://docs.microsoft.com/windows/uwpcommunitytoolkit/).

Em breve, você terá acesso a mais recursos de Design Fluente: estamos fornecendo um controle que permite a hospedagem de vários controles da UWP. Procure por este controle e muitos outros controles em versões futuras do Kit de Ferramentas da Comunidade do Windows.

Veja como esses controles são organizados na arquitetura. Os nomes usados neste diagrama estão sujeitos a alterações.  

![Arquitetura de controle de host](images/host-controls.png)

As APIs que aparecem na parte inferior do diagrama com o SDK do Windows.  Os controles que você adiciona ao seu designer são fornecidos como pacotes Nuget no Kit de Ferramentas da Comunidade do Windows.

Esses novos controles possuem limitações, portanto, antes de usá-los, revise o que ainda não tem suporte e o que está funcionando apenas com soluções alternativas.

### <a name="whats-supported"></a>O que é suportado

Na maior parte, tudo o que é compatível, a menos quando explicitamente citado na lista a seguir.

### <a name="whats-supported-only-with-workarounds"></a>O que é suportado apenas com soluções alternativas

:heavy_check_mark: hospedar vários controles de caixa de entrada em várias janelas. Você deverá colocar cada janela em seu próprio thread.

:heavy_check_mark: usando ``x:Bind`` com controles hospedados. É necessário declarar o modelo de dados em uma biblioteca .NET padrão.

:heavy_check_mark: controles de terceiros com base em C#. Se você tiver o código-fonte para um controle de terceiros, você pode compilar em relação a ele.

### <a name="whats-not-yet-supported"></a>O que ainda não tem suporte

:no_entry_sign: ferramentas de acessibilidade de funcionam sem interrupção no aplicativo e nos controles hospedados.

:no_entry_sign: conteúdo localizado nos controles que você adiciona aos aplicativos que não contêm um pacote do aplicativo do Windows.

:no_entry_sign: referências de ativo feitas em XAML nos aplicativos que não contêm um pacote do aplicativo do Windows.

:no_entry_sign: controles que respondem adequadamente às mudanças em DPI e escala.

:no_entry_sign: adicionar um controle **WebView** a um controle de usuário personalizado, (no thread, fora do thread, ou fora de proc).

:no_entry_sign: o efeito fluente de [realce de revelação](https://docs.microsoft.com/windows/uwp/design/style/reveal).

:no_entry_sign: tinta embutida, @Places e @People para controles de entrada.

:no_entry_sign: atribuir chaves de acelerador.

:no_entry_sign: controles de terceiros com base em C++.

:no_entry_sign: hospedagem de controles personalizados do usuário.

Os itens nessa lista provavelmente são alterados à medida que melhoramos a experiência fluente para a área de trabalho.  
