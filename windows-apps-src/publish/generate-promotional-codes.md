---
author: jnHs
Description: "É possível pode gerar códigos promocionais para um aplicativo ou um IAP (produto no aplicativo) que você tiver publicado na Windows Store."
title: "Gerar códigos promocionais"
ms.assetid: 9B632266-64EC-4D62-A4C4-55B6643D8750
ms.sourcegitcommit: df994a3bbda9f6d4df0ee2dd6c2cb646d91a0bfc
ms.openlocfilehash: bfea802fb4a6efcfe34121802ce49f20a9b0305e

---

# Gerar códigos promocionais


É possível pode gerar códigos promocionais para um aplicativo ou um IAP (produto no aplicativo) que você tiver publicado na Windows Store. Códigos promocionais são um modo fácil de oferecer a usuários influentes acesso gratuito ao seu aplicativo ou IAP. Você também pode usar códigos promocionais em situações de atendimento ao cliente oferecendo aos usuários acesso gratuito ao seu aplicativo ou IAP, ou para [testes beta](beta-testing-and-targeted-distribution.md) com o Windows 10.

Cada código promocional tem uma URL resgatável exclusiva correspondente que você pode distribuir a um usuário. O usuário pode simplesmente clicar na URL para resgatar o código e instalar seu aplicativo ou IAP da Windows Store.

No painel do Centro de Desenvolvimento do Windows, você pode:

-   Solicitar um conjunto de códigos promocionais para seu aplicativo.
-   Baixar um pedido atendido de códigos promocionais.
-   Revise o uso de código promocional para seus aplicativos, incluindo:
    -   Resumos de pedidos de código promocional para todos os seus aplicativos (na página **Visão geral do painel**) e para cada aplicativo individualmente (na página **Visão geral do aplicativo** para cada aplicativo).
    -   Um resumo detalhado dos pedidos de código promocional para cada aplicativo (na página **Códigos promocionais** para cada aplicativo).

> **Observação**  Você pode gerar códigos promocionais mesmo se tiver selecionado a opção **Ocultar este aplicativo e evitar a aquisição. Os clientes com um código promocional ainda poderão baixá-lo em dispositivos Windows 10** na página do painel [Preço e disponibilidade](set-app-pricing-and-availability.md) de seu aplicativo. Seu aplicativo deve passar a fase final de publicação do [processo de certificação de aplicativo](the-app-certification-process.md) antes que os usuários possam resgatar um código promocional para instalá-lo.

## Políticas para códigos promocionais


Lembre-se das seguintes políticas para códigos promocionais:

-   Você pode gerar códigos promocionais para qualquer aplicativo ou IAP publicado na Windows Store. Os usuários podem resgatar os códigos em qualquer versão do Windows que seja compatível com seu aplicativo ou IAP.
-   Os códigos promocionais expiram seis meses após a data do pedido.
-   Para cada um de seus aplicativos ou IAPs, você pode gerar até 500 códigos promocionais a cada seis meses. O período de seis meses começa quando o primeiro pedido de código promocional é enviado.
-   Você deve seguir os requisitos definidos no [Contrato de Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058), incluindo a seção **3k. Códigos promocionais**.

## Solicitar códigos promocionais


Para solicitar códigos promocionais para um aplicativo ou IAP publicado na Windows Store:

1.  No painel do Centro de Desenvolvimento do Windows, siga um destes procedimentos:
    -   Na página **Visão geral do aplicativo** para o seu aplicativo, localize a seção **Códigos promocionais** e clique em **Solicitar códigos**.
    -   Em qualquer página do painel de seu aplicativo, no menu de navegação à esquerda, expanda **Monetização** e clique em **Códigos promocionais**. Na página **Códigos promocionais**, clique em **Solicitar códigos**.

2.  Na página **Pedido de novos códigos promocionais**, insira o seguinte:
    -   Selecione o aplicativo ou IAP para o qual você deseja gerar códigos.
    -   Especifique um nome para o pedido. Você pode usar esse nome para diferenciar entre diferentes pedidos de códigos quando for analisar os dados de uso dos códigos promocionais.
    -   Especifique a quantidade de códigos para solicitar.
    -   Especifique quando os códigos promocionais devem se tornar ativos. Para escolher uma determinada data e hora inicial, desmarque a caixa de seleção **Os códigos são ativados imediatamente**.
    -   Especifique quando os códigos promocionais devem expirar. Para escolher uma data e uma hora de expiração específicas, desmarque a caixa de seleção **Os códigos expiram após 6 meses**.

3.  Clique em **Solicitar códigos**. O pedido é enviado e o painel leva você à página **Códigos promocionais**, onde o novo pedido é listado como **Pendente** na tabela de resumo de pedidos de códigos promocionais.

Em geral, os códigos promocionais ficam disponíveis para download dentro de 60 minutos após o pedido ser efetuado, embora alguns pedidos possam levar mais tempo para serem processados. Após o pedido ser atendido e os códigos se tornarem disponíveis para download, o status do pedido muda para **Disponível**.

## Baixar e distribuir códigos promocionais


Para baixar um pedido atendido de códigos promocionais e distribuir os códigos a usuários do seu aplicativo:

1.  No painel do Centro de Desenvolvimento do Windows, retorne à página **Códigos promocionais** do seu aplicativo (expanda **Monetização** e clique em **Códigos promocionais**).
2.  Confirme se o status do seu pedido é **Disponível**. Clique no link **Baixar** correspondente ao seu pedido e salve o arquivo fornecido em seu computador. Esse arquivo contém informações sobre seu pedido de códigos promocionais no formato TSV (valores separados por tabulações).
3.  Abra o arquivo TSV no editor de sua escolha. Para a melhor experiência, abra o arquivo TSV em um aplicativo que possa exibir os dados em uma estrutura tabular, como o Microsoft Excel. No entanto, você também pode abrir o arquivo em qualquer editor de texto.

    O arquivo contém as seguintes colunas de dados para cada código:

    -   **Nome do produto**: o nome do aplicativo ou IAP ao qual o código esteja associado.
    -   **Nome do pedido**: o nome do pedido no qual esse código foi atendido.
    -   **Código promocional**: o código em si. Consiste em uma sequência 5x5 de caracteres alfanuméricos separados por hífens. Por exemplo:

        DM3GY-M2GYM-6YMW6-4QHHT-23W2Z

    -   **URL resgatável**: a URL que um usuário possa usar para resgatar o código e instalar seu aplicativo ou IAP. A URL tem o seguinte formato:

        https://account.microsoft.com/billing/redeem?mstoken=&lt;promotional_code>

    -   **Data de início**: a data de início da validade do código.
    -   **Data de validade**: a data de validade desse código.
    -   **ID do Código**: uma ID exclusiva para esse código.
    -   **ID do Pedido**: uma ID exclusiva para o pedido em que esse código foi atendido.
    -   **Fornecido a**: um campo vazio que você pode preencher com um valor que identifique o usuário ao qual forneceu o código.
    -   **Disponível**: o número de códigos ainda disponíveis para resgatar.
    -   **Resgatado**: o número de códigos que foram resgatados.

4.  Distribua as URLs resgatáveis aos seus usuários por meio de qualquer formato de comunicação de sua preferência (como email, SMS ou cartões impressos). Recomendamos que sua comunicação inclua o seguinte:
    -   Uma explicação do aplicativo ou IAP ao qual o código promocional se aplica e, opcionalmente, uma descrição do motivo pelo qual o usuário está recebendo o código.
    -   A URL resgatável do código.
    -   Instruções que orientam o usuário a visitar a URL resgatável, fazer logon usando sua conta da Microsoft e seguir as instruções para baixar e instalar o aplicativo.

## Experiência do usuário de resgate do código


Depois que você distribui uma URL resgatável a um usuário, as etapas a seguir descrevem a experiência que o usuário seguirá para resgatar seu aplicativo.

1.  O usuário clica na URL resgatável.

    O navegador é aberto na página autenticada **Resgatar seu código** em <https://account.microsoft.com/billing/redeem>. Esta página inclui uma descrição do aplicativo que o usuário está prestes a resgatar.

2.  O usuário clica em **Resgatar.**

    O navegador vai até uma página **Obrigado** com um link **Obter*****&lt;o nome do aplicativo&gt;***.

    > **Observação**  Os usuários receberão um erro nessa etapa se seu aplicativo ainda não tiver sido publicado.

3.  A usuário clica em **Obter*****&lt;o nome do aplicativo&gt;***.

4.  Se o usuário estiver em um computador com a Windows Store para Windows 10 ou Windows 8.1 instalada, a Windows Store será aberta na página de visão geral do aplicativo. O usuário pode clicar em **Instalar** para instalar o aplicativo sem custos.

    Se o usuário estiver em um computador ou dispositivo que não tenha a Windows Store instalada, o navegador abrirá a página da Web da Windows Store do aplicativo. O usuário pode clicar em **Instalar** para instalar o aplicativo sem custos.

    > **Observação**  Em alguns casos, a página do aplicativo pode exibir um botão **Comprar** em vez de **Instalar**, mesmo que o aplicativo tenha sido resgatado com êxito pelo código promocional. O usuário pode clicar em **Comprar** para instalar o aplicativo sem custos.

## Analisar seus códigos promocionais


Há diversas maneiras de analisar o uso dos seus códigos promocionais.

-   Para analisar um resumo de pedidos de códigos promocionais para todos os seus aplicativos, visite a página **Visão geral do painel** e localize a seção **Códigos promocionais** nessa página. Esta seção exibe os códigos promocionais ativos restantes para todos os seus aplicativos, o número total de códigos promocionais resgatados para todos os seus aplicativos e o número total de pedidos de códigos promocionais que você efetuou para todos os seus aplicativos.
-   Para analisar um resumo de pedidos de códigos promocionais para um aplicativo específico, navegue até a página **Visão geral do aplicativo** referente ao aplicativo e localize a seção **Códigos promocionais** nessa página. Esta seção exibe os códigos promocionais ativos restantes para o aplicativo, o número total de códigos promocionais resgatados para o aplicativo e o número total de pedidos de códigos promocionais que você efetuou para o aplicativo.
-   Para analisar um resumo detalhado de pedidos de códigos promocionais para um aplicativo específico, navegue até a página **Códigos promocionais** do seu aplicativo (expanda **Monetização** e clique em **Códigos promocionais**). Você pode examinar os seguintes detalhes para todos os códigos promocionais atuais e inativos para o aplicativo:
    -   Nome do pedido
    -   Nome do aplicativo ou IAP
    -   Data do pedido
    -   Data de validade
    -   Status

Você também pode baixar um pedido ativo dessa tabela.

 

 







<!--HONumber=Jun16_HO4-->


