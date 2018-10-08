---
author: jnHs
Description: You can generate promotional codes for an app or add-on that you have published in the Microsoft Store.
title: Gerar códigos promocionais
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.author: wdg-dev-content
ms.date: 08/24/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, código promocional, códigos promocionais, token, tokens
ms.localizationpriority: medium
ms.openlocfilehash: 37263794ffed6660f71c5e16195e992588c16d4a
ms.sourcegitcommit: fbdc9372dea898a01c7686be54bea47125bab6c0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/08/2018
ms.locfileid: "4416186"
---
# <a name="generate-promotional-codes"></a>Gerar códigos promocionais


Você pode gerar códigos promocionais para um app ou complemento que você tenha publicado na Microsoft Store. Códigos promocionais são um modo fácil de conceder a usuários influentes acesso gratuito ao seu app ou complemento. Você também pode usar códigos promocionais em situações de atendimento ao cliente oferecendo aos usuários acesso gratuito ao seu aplicativo ou complemento, ou para [testes beta](beta-testing-and-targeted-distribution.md) com o Windows 10. 

Cada código promocional tem uma URL resgatável exclusiva correspondente que um cliente pode clicar para resgatar o código e instalar seu aplicativo ou complemento da Microsoft Store.  Observe que o aplicativo deve passar na fase final de publicação do [processo de certificação de aplicativo](the-app-certification-process.md) para que os usuários possam resgatar um código promocional para instalá-lo.

Você pode gerar códigos de uso único (e distribuir um para cada cliente), ou você pode optar por gerar um código que pode ser usado várias vezes por um determinado número de clientes.

> [!TIP]
> Você pode usar [notificações por push direcionadas](send-push-notifications-to-your-apps-customers.md) para distribuir um código promocional a um segmento dos clientes. Ao fazer isso, use um código promocional que permite que vários clientes usem o mesmo código.


## <a name="promotional-code-policies"></a>Políticas para códigos promocionais

Lembre-se das seguintes políticas para códigos promocionais:

-   Você pode gerar códigos promocionais para qualquer app ou complemento (exceto complementos de assinatura) que você tenha publicado na Microsoft Store. Os clientes podem resgatar os códigos em qualquer versão do Windows que seja compatível com o app ou o complemento.
-   Os códigos promocionais expiram em seis meses após a data do pedido (a menos que você escolha uma data de expiração anterior).
-   Para cada um dos aplicativos ou complementos, você pode gerar códigos que permitam até 1600 resgates a cada seis meses. O período de seis meses começa quando o primeiro pedido de código promocional é enviado, mesmo se você escolher uma data de expiração anterior. O total de 1600 resgates por produto se aplica a códigos de uso único e códigos que podem ser usados várias vezes.
-   Você deve seguir os requisitos definidos no [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), incluindo a seção **3k. Códigos promocionais**.

> [!NOTE]
> Você pode usar códigos promocionais mesmo se seu aplicativo não está disponível para os clientes (ou seja, se você selecionou **disponibilizar este produto mas não detectável na loja** com a aquisição de parada **: os clientes com um link direto poderão ver da loja do produto listagem, mas eles só poderão baixá-lo se adquirido o produto antes ou tiverem um código promocional e estiverem usando um dispositivo Windows 10** opção na seção de [Descoberta](choose-visibility-options.md#discoverability) do seu envio). Com essa opção, os clientes devem estar na Windows 10 (incluindo o Xbox) para adquirir seu produto com um código promocional.


## <a name="order-promotional-codes"></a>Solicitar códigos promocionais

Para solicitar códigos promocionais para um aplicativo ou complemento:

1.  No menu de navegação esquerdo do painel do Centro de Desenvolvimento do Windows, expanda **Atrair** e selecione **Códigos promocionais**.

2.   Na página **Códigos promocionais**, clique em **Solicitar códigos**.

3.  Na página **Pedido de novos códigos promocionais**, insira o seguinte:
    -   Selecione o app ou o complemento para o qual você deseja gerar códigos. (Observe que você não pode gerar códigos promocionais para complementos de assinatura).
    -   Especifique um nome para o pedido. Você pode usar esse nome para diferenciar entre diferentes pedidos de códigos quando for analisar os dados de uso dos códigos promocionais.
    -   Selecione o tipo de pedido. Você pode optar por gerar um conjunto de códigos promocionais que pode ser usado uma vez ou optar por gerar um código promocional que pode ser usado várias vezes.
    -   Especifique o número de códigos a ser solicitado (se estiver gerando um conjunto de códigos) ou o número de vezes que o código pode ser resgatado (se estiver gerando códigos a serem usados várias vezes).
    -   Especifique quando os códigos promocionais devem se tornar ativos. Para escolher uma determinada data e hora inicial, desmarque a caixa de seleção **Os códigos são ativados imediatamente**. Caso contrário, os códigos ficarão ativos imediatamente (embora seu produto deve ter concluído o processo de publicação para que um cliente usar o código).
    -   Especifique quando os códigos promocionais devem expirar. Para escolher uma data e hora de expiração anterior a seis meses, desmarque a caixa de seleção **Os códigos expiram após 6 meses**.

4.  Clique em **Solicitar códigos**. Você será retornado à página **Códigos promocionais**, na qual poderá ver seu novo pedido na tabela de resumo de pedidos de códigos promocionais do aplicativo.


## <a name="download-and-distribute-promotional-codes"></a>Baixar e distribuir códigos promocionais

Para baixar um pedido atendido de códigos promocionais e distribuir os códigos para os clientes:

1.  No menu de navegação esquerdo do painel do Centro de Desenvolvimento do Windows, expanda **Atrair** e selecione **Códigos promocionais**.
2.  Clique no link **Download** do pedido de código promocional e salve o arquivo gerado no computador. Esse arquivo contém informações sobre seu pedido de códigos promocionais no formato (.tsv).
3.  Abra o arquivo .tsv no editor de sua escolha. Para a obter a melhor experiência, abra o arquivo .tsv em um aplicativo que possa exibir os dados em uma estrutura tabular, como o Microsoft Excel. No entanto, você também pode abrir o arquivo em qualquer editor de texto.

    O arquivo contém as seguintes colunas de dados para cada código:

    -   **Nome do produto**: o nome do aplicativo ou complemento ao qual o código está associado.
    -   **Nome do pedido**: o nome do pedido no qual esse código foi gerado.
    -   **Código promocional**: o código propriamente dito. Consiste em uma sequência 5x5 de caracteres alfanuméricos separados por hífens. Por exemplo: DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **URL Resgatável**: a URL que um cliente pode usar para resgatar o código e instalar seu aplicativo ou complemento. A URL tem o seguinte formato: http://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; promotional_code >
    -   **Data de início**: a data em que este código foi ativado.
    -   **Data de validade**: a data de validade deste código.
    -   **ID do Código**: uma ID exclusiva para este código.
    -   **ID do Pedido**: uma ID exclusiva para o pedido em que este código foi gerado.
    -   **Fornecido a**: um campo vazio que você pode usar para controlar a quais cliente forneceu o código.
    -   **Disponível**: o número de vezes que o código ainda está disponível para resgate (no momento em que o arquivo foi gerado).
    -   **Resgatado**: o número de vezes que o código foi resgatado (no momento em que o arquivo foi gerado).

4.  Distribua as URLs resgatáveis aos clientes por meio de qualquer formato de comunicação de sua preferência (por exemplo, notificações direcionadas, email, mensagens SMS ou cartões impressos). Recomendamos que sua comunicação inclua o seguinte:
    -   Uma explicação do aplicativo ou complemento ao qual o código promocional se aplica e, opcionalmente, uma descrição do motivo pelo qual o cliente está recebendo o código.
    -   A URL resgatável do código.
    -   Instruções que orientam o cliente a acessar a URL resgatável, fazer logon usando a conta da Microsoft e seguir as instruções para baixar e instalar o aplicativo.


## <a name="code-redemption-user-experience"></a>Experiência de resgate do código pelo usuário

Após distribuir um código promocional (ou sua URL resgatável) a um cliente, eles podem clicar na URL para obter o produto gratuitamente. Clicar na URL resgatável iniciará uma página **Resgatar seu código** autenticada em <https://account.microsoft.com/billing/redeem>. Esta página inclui uma descrição do aplicativo que o usuário está prestes a resgatar. Se o cliente não estiver conectado com a conta da Microsoft, ele poderá ser solicitado a fazê-lo. O cliente também pode visitar <https://account.microsoft.com/billing/redeem> e inserir o código diretamente.

> [!IMPORTANT]
> É recomendável não distribuir códigos promocionais para seus clientes até que o produto conclua o processo de publicação (mesmo se você tiver selecionado **Disponibilizar este produto mas não torná-lo detectável na Loja**). Os clientes verão um erro se tentarem usar um código promocional para um produto que ainda não foi publicado.

Depois que o cliente clicar em **Resgatar**, a Microsoft Store abrirá a página de visão geral do app (se ele estiver em um dispositivo Windows 10 ou Windows 8.1), onde ele poderá clicar em **Instalar** para baixar e instalar o aplicativo gratuitamente. Se o cliente estiver em um computador ou dispositivo que não tenha a Microsoft Store instalada, o link iniciará a página da Web da Microsoft Store do app. O código será aplicado à conta da Microsoft do cliente, para que ele possa baixar o aplicativo em um dispositivo Windows (que é associado à mesma conta da Microsoft) gratuitamente mais tarde.

> [!NOTE]
> Em alguns casos, um cliente pode ver um botão **comprar** em vez de **instalar**, mesmo que o aplicativo tenha sido resgatado com êxito o código promocional. O cliente pode clicar em **Comprar** para instalar o aplicativo gratuitamente.


## <a name="review-your-promotional-codes"></a>Analisar os códigos promocionais

Para analisar um resumo detalhado de pedidos de códigos promocionais de seus apps e complementos, navegue até a página **Códigos promocionais** (no menu de navegação esquerdo do painel do Centro de Desenvolvimento, expanda **Atrair** e clique em **Códigos promocionais**). Você pode analisar os seguintes detalhes de todos os seus códigos promocionais atuais e inativos:
-   Nome do pedido
-   Aplicativo ou complemento
-   Data de início
-   Data de validade
-   Disponível
-   Resgatado

Você também pode [baixar](#download-and-distribute-promotional-codes) um pedido desta tabela.

 
