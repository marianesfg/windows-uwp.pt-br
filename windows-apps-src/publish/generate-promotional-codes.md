---
Description: Você pode gerar códigos promocionais para um app ou complemento que você tenha publicado na Microsoft Store.
title: Gerar códigos promocionais
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, código promocional, códigos promocionais, token, tokens
ms.localizationpriority: medium
ms.openlocfilehash: 931b3abfe13a3834d991ee1a0a38c752b9e3f719
ms.sourcegitcommit: 7da28cf4f4e8390bc9a21a9488b03af39271cbbe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/28/2019
ms.locfileid: "64745035"
---
# <a name="generate-promotional-codes"></a>Gerar códigos promocionais


[Partner Center](https://partner.microsoft.com/dashboard) permite a geração de códigos promocionais para um aplicativo ou o complemento que você publicou na Microsoft Store. Códigos promocionais são um modo fácil de oferecer a usuários influentes acesso gratuito ao seu aplicativo ou complemento. Você também pode usar os códigos promocionais para cenários de serviço de cliente de endereço, dando aos usuários acesso gratuito ao seu aplicativo ou o complemento, ou para [testes beta](beta-testing-and-targeted-distribution.md) com o Windows 10. 

Cada código promocional tem uma URL pode ser trocada exclusiva correspondente que um cliente pode clicar para resgatar o código e instalar seu aplicativo ou o complemento da Microsoft Store.  Observe que o aplicativo deve passar na fase final de publicação do [processo de certificação de aplicativo](the-app-certification-process.md) para que os usuários possam resgatar um código promocional para instalá-lo.

Você pode gerar códigos de uso único (e distribuir um para cada cliente), ou você pode optar por gerar um código que pode ser usado várias vezes por um número especificado de clientes.

> [!TIP]
> Você pode usar [notificações por push direcionadas](send-push-notifications-to-your-apps-customers.md) para distribuir um código promocional a um segmento dos clientes. Ao fazer isso, use um código promocional que permite que vários clientes usem o mesmo código.


## <a name="promotional-code-policies"></a>Políticas para códigos promocionais

Lembre-se das seguintes políticas para códigos promocionais:

-   Você pode gerar códigos promocionais para qualquer app ou complemento (exceto complementos de assinatura) que você tenha publicado na Microsoft Store. Os clientes podem resgatar os códigos em qualquer versão do Windows que seja compatível com o app ou o complemento.
-   Para jogos:
    - Você pode gerar códigos promocionais até 5.000 por jogo.
    - Os códigos promocionais gerados para jogos nunca expirarem.
- Para todos os outros tipos de aplicativos ou complementos:
    - Em um período de seis meses, você pode gerar códigos promocionais de uso único até 1600 ou qualquer número de vários códigos de uso, de modo que o total permitido resgates não exceder 1600.
    - O período de 6 meses começa quando você gerar o primeiro código promocional é criado e dura por 6 meses, independentemente se deseja ou não definir uma data de vencimento anterior sobre os códigos.
    - Todos os códigos criados durante um período de seis meses existente será contam para o número de códigos gerados dentro desse período, mesmo que elas expirarão após o término do período (por exemplo, se você gerar um código no último dia da janela de seis meses, ele será será ser ainda  ser válido para um total de seis meses desde sua criação.)
-   Você deve seguir os requisitos definidos na [contrato de desenvolvedor do aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement), incluindo a seção **3 k. Códigos promocionais**.

> [!NOTE]
> Você pode usar códigos promocionais, mesmo que seu aplicativo não está disponível para clientes (ou seja, se você tiver selecionado **tornar este produto disponível, mas não podem ser descobertos no Store** com o **parar aquisição: Qualquer cliente com um link direto pode ver uma listagem de Store do produto, mas eles podem baixá-lo somente se o produto antes, de propriedade ou tiver um código promocional e estiver usando um dispositivo Windows 10** opção em seu envio [detectabilidade ](choose-visibility-options.md#discoverability) seção). Com essa opção, os clientes devem estar no Windows 10 (incluindo Xbox) para adquirir seu produto com um código promocional.


## <a name="order-promotional-codes"></a>Solicitar códigos promocionais

Para os códigos promocionais de pedido para um aplicativo ou o complemento:

1.  No menu de navegação à esquerda do [Partner Center](https://partner.microsoft.com/dashboard), expanda **Attract** e, em seguida, selecione **códigos promocionais**.

2.   Na página **Códigos promocionais**, clique em **Solicitar códigos**.

3.  Na página **Pedido de novos códigos promocionais**, insira o seguinte:
    -   Selecione o aplicativo ou complemento para o qual você deseja gerar códigos. (Observe que você não pode gerar códigos promocionais para complementos de assinatura).
    -   Especifique um nome para o pedido. Você pode usar esse nome para diferenciar entre diferentes pedidos de códigos quando for analisar os dados de uso dos códigos promocionais.
    -   Selecione o tipo de pedido. Você pode optar por gerar um conjunto de códigos promocionais que pode ser usado uma vez ou optar por gerar um código promocional que pode ser usado várias vezes.
    -   Especifique o número de códigos a ser solicitado (se estiver gerando um conjunto de códigos) ou o número de vezes que o código pode ser resgatado (se estiver gerando códigos a serem usados várias vezes).
    -   Especifique quando os códigos promocionais devem se tornar ativos. Para escolher uma determinada data e hora inicial, desmarque a caixa de seleção **Os códigos são ativados imediatamente**. Caso contrário, os códigos se tornará ativos imediatamente (embora o produto deve ter concluído o processo de publicação para que um cliente usar o código).
    -   Especifique quando os códigos promocionais devem expirar. Para escolher uma data e hora de expiração anterior a seis meses, desmarque a caixa de seleção **Os códigos expiram após 6 meses**.

4.  Clique em **Solicitar códigos**. Você será retornado à página **Códigos promocionais**, na qual poderá ver seu novo pedido na tabela de resumo de pedidos de códigos promocionais do aplicativo.


## <a name="download-and-distribute-promotional-codes"></a>Baixar e distribuir códigos promocionais

Para baixar um pedido atendido de códigos promocionais e distribuir os códigos para os clientes:

1.  No menu de navegação à esquerda do [Partner Center](https://partner.microsoft.com/dashboard), expanda **Attract** e, em seguida, selecione **códigos promocionais.**
2.  Clique no link **Download** do pedido de código promocional e salve o arquivo gerado no computador. Esse arquivo contém informações sobre seu pedido de códigos promocionais no formato (.tsv).
3.  Abra o arquivo .tsv no editor de sua escolha. Para a obter a melhor experiência, abra o arquivo .tsv em um aplicativo que possa exibir os dados em uma estrutura tabular, como o Microsoft Excel. No entanto, você também pode abrir o arquivo em qualquer editor de texto.

    O arquivo contém as seguintes colunas de dados para cada código:

    -   **Nome do produto**: O nome do aplicativo ou complemento que o código está associado.
    -   **Nome do pedido**: O nome da ordem em que esse código foi gerado.
    -   **O código promocional**: O próprio código. Consiste em uma sequência 5x5 de caracteres alfanuméricos separados por hífens. Por exemplo:  DM3GY-M2GYM-6YMW6-4QHHT-23W2Z
    -   **A URL pode ser trocada**: A URL que um cliente pode usar para resgatar o código e instalar seu aplicativo ou o complemento. A URL tem o seguinte formato: https://go.microsoft.com/fwlink/?LinkId=532540&mstoken=&lt; promotional_code >
    -   **Data de início**: A data em que esse código se tornaram ativo.
    -   **Expirar data**: A data em que esse código expira.
    -   **ID de código**: Uma ID exclusiva para esse código.
    -   **ID do pedido**: Uma ID exclusiva para a ordem em que esse código foi gerado.
    -   **Dada a**: Um campo vazio que você pode usar para controlar qual cliente que você atribuiu o código.
    -   **Disponível**: O número de vezes que o código ainda está disponível para resgatar (no momento em que o arquivo foi gerado).
    -   **Resgatado**: O número de vezes que o código foi resgatado (no momento em que o arquivo foi gerado).

4.  Distribua as URLs resgatáveis aos clientes por meio de qualquer formato de comunicação de sua preferência (por exemplo, notificações direcionadas, email, mensagens SMS ou cartões impressos). Recomendamos que sua comunicação inclua o seguinte:
    -   Uma explicação do aplicativo ou complemento ao qual o código promocional se aplica e, opcionalmente, uma descrição do motivo pelo qual o cliente está recebendo o código.
    -   A URL resgatável do código.
    -   Instruções que orientam o cliente a acessar a URL resgatável, fazer logon usando a conta da Microsoft e seguir as instruções para baixar e instalar o aplicativo.


## <a name="code-redemption-user-experience"></a>Experiência do usuário de resgate do código

Após distribuir o código promocional (ou sua URL pode ser trocada) a um cliente, eles podem clicar na URL para obter o produto gratuitamente. Clicar na URL resgatável iniciará uma página **Resgatar seu código** autenticada em <https://account.microsoft.com/billing/redeem>. Esta página inclui uma descrição do aplicativo que o usuário está prestes a resgatar. Se o cliente não estiver conectado com a conta da Microsoft, ele poderá ser solicitado a fazê-lo. O cliente também pode visitar <https://account.microsoft.com/billing/redeem> e inserir o código diretamente.

> [!IMPORTANT]
> É recomendável não distribuir códigos promocionais para seus clientes até que o produto conclua o processo de publicação (mesmo se você tiver selecionado **Disponibilizar este produto mas não torná-lo detectável na Loja**). Os clientes verão um erro se tentarem usar um código promocional para um produto que ainda não foi publicado.

Depois que o cliente clicar em **Resgatar**, a Microsoft Store abrirá a página de visão geral do app (se ele estiver em um dispositivo Windows 10 ou Windows 8.1), onde ele poderá clicar em **Instalar** para baixar e instalar o aplicativo gratuitamente. Se o cliente estiver em um computador ou dispositivo que não tenha a Microsoft Store instalada, o link iniciará a página da Web da Microsoft Store do app. O código será aplicado à conta da Microsoft do cliente, para que ele possa baixar o aplicativo em um dispositivo Windows (que é associado à mesma conta da Microsoft) gratuitamente mais tarde.

> [!NOTE]
> Em alguns casos, um cliente poderá ver uma **compre** botão em vez de **instalar**, mesmo que o aplicativo foi resgatado com êxito por meio do código promocional. O cliente pode clicar em **Comprar** para instalar o aplicativo gratuitamente.


## <a name="review-your-promotional-codes"></a>Analisar seus códigos promocionais

Para examinar um resumo detalhado de ordens de código promocional para seus aplicativos e complementos, navegue até a **códigos promocionais** página (no menu de navegação à esquerda do Partner Center, expanda **Attract** e, em seguida, selecione **Códigos promocionais**). Você pode analisar os seguintes detalhes de todos os seus códigos promocionais atuais e inativos:
-   Nome do pedido
-   Aplicativo ou complemento
-   Data de início
-   Data de validade
-   Disponível
-   Resgatado

Você também pode [baixar](#download-and-distribute-promotional-codes) um pedido desta tabela.

 
