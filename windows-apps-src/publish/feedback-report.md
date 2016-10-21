---
author: jnHs
Description: "O relatório de comentários no painel do Centro de Desenvolvimento do Windows permite que você veja problemas, sugestões e aprovações que os clientes do Windows 10 enviaram através do Hub de Feedback."
title: "Relatório de comentários"
ms.assetid: 9EA8B456-CA57-40CE-A55B-7BFDC55CA8A8
translationtype: Human Translation
ms.sourcegitcommit: 70020d3c6e0fb0fea321ce1951720803fd25f9c0
ms.openlocfilehash: e6266ff7c45a49b3eece8ffaf3d0603d55a04761

---

# Relatório de comentários

O **relatório de comentários** no painel do Centro de Desenvolvimento do Windows permite que você veja problemas, sugestões e aprovações que os clientes do Windows 10 enviaram através do Hub de Feedback. Você pode exibir esses dados em seu painel ou exportar os dados para exibição offline.

Incentivar os clientes a fornecerem comentários sobre seu aplicativo é uma ótima maneira de aprender sobre os problemas e recursos que são mais importantes para eles. Quando seus clientes sabem que podem enviar comentários diretamente, eles têm menos probabilidade de deixar esses comentários como uma análise negativa.

> **Observação** Você também pode [responder aos comentários](respond-to-customer-feedback.md) diretamente desse relatório para mostrar aos clientes que a opinião deles é ouvida.

Você pode usar a API de comentários no [Microsoft Store Services SDK](http://aka.ms/store-em-sdk) para permitir que os clientes [iniciem diretamente o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md). Lembre-se de que qualquer cliente que tenha baixado seu aplicativo em um dispositivo Windows 10 que dê suporte a comentários Hub tem a capacidade de deixar comentários sobre ele usando o aplicativo de Hub de Feedback. Por isso, você poderá ver comentários de clientes nesse relatório, mesmo que não tenha solicitado comentários especificamente dentro de seu aplicativo.

> **Dica** O relatório de comentários se torna especialmente importante se você estiver usando o [pacote de liberação de versões de pré-lançamento](package-flights.md), uma vez que o relatório de comentários mostrará o pacote específico que cada cliente tinha instalado em seu dispositivo quando deixou os comentários.

## Exibir detalhes de comentários

Na seção **Detalhes** desse relatório, você encontrará os comentários individuais deixados por seus clientes. À esquerda do texto dos comentários, você verá o número de vezes que os comentários tiveram votos a favor de outros clientes no Hub de Feedback. Você pode classificar os comentários de três maneiras:

- **Votos a favor** (padrão): mostra os comentários que tiveram votos a favor de outros clientes, começando com os comentários com mais votos a favor.
- **Mais populares**: mostra os comentários que tiveram votos a favor por outros clientes nos últimos sete dias, começando com os comentários que obtiveram a atividade mais recente.
- **Mais recentes**: mostra todos os comentários, começando com os comentários deixados mais recentemente.

Ao lado de cada comentário, você verá a data em que os comentários foram deixados e o tipo de comentário. Você também verá o mercado do cliente, o pacote específico de seu aplicativo que foi instalado no dispositivo que estava sendo usado quando deixaram os comentários, o tipo de dispositivo e o **Usuário do Windows Insider** se o cliente que enviou os comentários for participante do Programa Windows Insider.


## Aplicar filtros

Perto da parte superior da página, você pode expandir **Aplicar filtros** para filtrar todos os dados dessa página.

> **Dica** Se você não vir nenhum comentário na página, verifique se seus filtros não excluíram todos os comentários. Por exemplo, se você filtrar por um **Tipo de dispositivo** não compatível com seu aplicativo, você não verá nenhum comentário

- **Data**: O filtro padrão é **Todo o tempo**. Você pode selecionar intervalos de tempo mais curtos de **Últimos 30 dias** até **Últimos 12 meses**.
- **Tipo de comentários**: a configuração padrão é **Todos**. Você pode selecionar **Problema** ou **Sugestão** para mostrar apenas esse tipo de comentários.
- **Tipo de dispositivo**: a configuração padrão é **Todos os dispositivos**. Você pode selecionar **Telefone**, **PC** ou **Tablet** para mostrar apenas comentários à esquerda desse tipo de dispositivo.
- **Versão do pacote**: a configuração padrão é **Todos os pacotes**. Você pode selecionar um de seus pacotes para mostrar apenas os comentários deixados por clientes que estavam usando esse pacote específico quando deixaram comentários.
- **Mercado**: a configuração padrão é **Todos os mercados**. Você pode escolher um específico para mostrar apenas os comentários dos clientes desse mercado.
- **Grupo**: a configuração padrão é **Todos**. Você pode optar por exibir apenas os comentários enviados por [Usuários do Windows Insider](http://insider.windows.com).

## Traduzindo comentários

Por padrão, críticas que não foram escritas em seu idioma preferencial são traduzidas para você. Se você preferir, a tradução dos comentários poderá ser desabilitada desmarcando a caixa de seleção **Traduzir Críticas** na parte superior direita, acima da lista de comentários.

Observe que os comentários são traduzidos por um sistema de tradução automática, e a tradução resultante pode não ser precisa. O texto original será fornecido, se você quiser compará-lo com a tradução, ou traduzi-lo por meio de algum outro meio.

## Iniciando o Hub de Feedback diretamente em seu aplicativo

Como observado acima, recomendamos a incorporação de um link para o Hub de Feedback diretamente em seu aplicativo para incentivar os clientes a fornecer comentários. Para obter mais informações, consulte [Iniciar o Hub de Feedback em seu aplicativo](../monetize/launch-feedback-hub-from-your-app.md).



<!--HONumber=Aug16_HO5-->


