---
author: shawjohn
Description: "Saiba como criar segmentos de cliente para que você possa segmentar um subconjunto de sua base de clientes para fins promocionais ou de envolvimento."
title: Criar segmentos de cliente
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.openlocfilehash: 2ba423d7f7624575e7cf743e9589d31a90d05295
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="create-customer-segments"></a>Criar segmentos de cliente

Há momentos em que você talvez queira direcionar um subconjunto de sua base de clientes para fins promocionais e de envolvimento. Você pode fazer isso no Centro de Desenvolvimento do Windows com a criação de um tipo de [grupo de clientes](create-customer-groups.md) conhecido como *segmento* que inclui os clientes do Windows 10 que satisfazem os critérios demográficos ou de receita escolhidos.

Por exemplo, você poderia criar um segmento que inclua apenas os clientes que têm 50 anos ou mais, ou que inclua os clientes que gastaram mais de US$ 10 na Windows Store. Você também pode combinar esses critérios e criar um segmento que inclui todos os clientes com mais de 50 anos que gastaram mais de US$ 10 na Loja. Oferecemos alguns modelos de segmento para ajudá-lo a começar, mas você pode definir e combinar os critérios da maneira que quiser.

> **Dica** Os segmentos podem ser usados para [enviar notificações por push direcionadas](send-push-notifications-to-your-apps-customers.md) a um grupo de clientes como parte de uma campanha de envolvimento.

## <a name="to-create-a-customer-segment"></a>Para criar um segmento de cliente

1.    No [painel do Centro de Desenvolvimento do Windows](https://developer.microsoft.com/dashboard/overview), selecione **Clientes** no menu principal.
2.    Na página **Grupos de clientes**, siga um destes procedimentos:
 - Na seção **Meu grupos de clientes**, selecione **Criar novo grupo** para definir um segmento do zero. Verifique se **Segmento** está selecionado na lista suspensa **Tipo de grupo**.
 - Na seção **Modelos de segmento**, selecione **Cópia para usar um segmento predefinido** que você pode usar como está ou modificar para atender às suas necessidades.
3.    Na lista **Incluir clientes desse aplicativo**, selecione um dos seus aplicativos para direcionar.
4.    Na caixa **Nome do segmento**, escolha um nome para o segmento.
5.    Na seção **Definir condições de inclusão**, escolha os critérios de filtro para o segmento.

    Você pode escolher entre uma variedade de critérios de filtro, incluindo **Fonte de aquisição**, **Aquisições**, **Demografia**, **Classificação**, **Aquisições da Loja**, **Compras na Loja** e **Gasto da Loja**.

    Por exemplo, se você quiser criar um segmento que inclua somente os clientes do app com 18 a 24 anos, selecione os critérios de filtro [**Demografia**] [**Faixa etária**] [**é**] [**18 a 24**] nas listas suspensas.

    Você pode criar segmentos mais complexos usando consultas E/OU para incluir ou excluir clientes com base em vários atributos. Para adicionar uma consulta OU, selecione **+ instrução OU**. Para adicionar uma consulta ADICIONAR, selecione **Adicionar outro filtro**.

    Portanto, se você quisesse refinar esse segmento para incluir somente clientes masculinos que estão na faixa etária especificada, selecione **Adicionar outro filtro** e, em seguida, selecione os critérios de filtro adicionais [**Demográfico**] [**Sexo**] [**é**] [**Masculino**]. Para este exemplo, a **Definição do segmento** exibiria **Faixa etária == 18 a 24 && Sexo == Masculino**.

    ![Exemplo de critérios de filtro de um segmento](images/create-segment-inclusions.png)
6. Selecione **Salvar**.

> **Importante** Não será possível usar um segmento que inclui poucos clientes. Se sua definição de segmento não incluir suficiente clientes, você poderá ajustar os critérios de segmento ou tentar novamente mais tarde, quando seu aplicativo talvez tiver adquirido mais clientes que atendem aos critérios de segmento.

Observações importantes sobre segmentos de cliente:
- Depois de salvar um segmento, leva 24 horas para você poder usá-lo para [notificações por push direcionadas](send-push-notifications-to-your-apps-customers.md).
- Os resultados de segmento são atualizados diariamente. Assim, você pode ver a contagem total de clientes em um segmento mudar de um dia para outro conforme os clientes passam a satisfazer os critérios de segmento.
- A maioria desses atributos é calculada usando todos os dados históricos, embora haja algumas exceções. Por exemplo, **Data de aquisição do aplicativo**, **ID da campanha**, **Data de visualização da página da Loja** e **Domínio de URI de referência** são limitados aos últimos 90 dias de dados.
- O segmento incluirá apenas os clientes que compraram o aplicativo no Windows 10. Se seu aplicativo oferecer suporte a versões mais antigas do sistema operacional, os clientes que usam essas versões mais antigas do sistema operacional não serão incluídos em nenhum segmento que você criar.
- Os segmentos excluem automaticamente todos os clientes com menos de 17 anos.


## <a name="app-statistics"></a>Estatísticas de aplicativo

A seção **Estatísticas de aplicativo** do segmento fornece algumas informações sobre seu aplicativo, bem como o tamanho do segmento que acabou de criar.

Observe que **Clientes disponíveis do aplicativo** não reflete o número real dos clientes que compraram o aplicativo, mas apenas o número de clientes que estão disponíveis para serem incluídos em segmentos (ou seja, os clientes que podemos comprovar que satisfazem os requisitos de idade, adquiriram seu aplicativo no Windows 10 e que estão associados a uma conta da Microsoft válida).

Se você exibir os resultados e **Clientes neste segmento** for **Pequeno**, o segmento não inclui clientes suficientes e o segmento é marcado como inativo. Segmentos inativos não podem ser usados para notificações ou outros recursos. Você poderá ativar e usar um segmento seguindo um destes procedimentos:

- Na seção **Definir condições de inclusão**, ajuste os critérios de filtro para que o segmento inclua mais clientes.
- Na página **Grupos de clientes**, na seção **Segmentos inativos**, selecione **Atualizar** para ver se o segmento atualmente contém clientes suficientes. Essa tática poderá funcionar, por exemplo, se os clientes que satisfizerem os critérios de segmento tiverem baixado seu aplicativo desde que o primeiro segmento foi criado.
