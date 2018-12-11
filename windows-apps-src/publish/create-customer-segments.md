---
Description: Learn how to create customer segments so you can target a subset of your customer base for promotional or engagement purposes.
title: Criar segmentos de cliente
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, segmento, segmentos, grupo de destino, clientes
ms.assetid: 58185f6c-d61f-478b-ab24-753d8986cd5a
ms.localizationpriority: medium
ms.openlocfilehash: d0df23f0da4efe01877c45e5b2b6b5f4e2142a92
ms.sourcegitcommit: 8921a9cc0dd3e5665345ae8eca7ab7aeb83ccc6f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8885363"
---
# <a name="create-customer-segments"></a>Criar segmentos de cliente

Há momentos em que você talvez queira direcionar um subconjunto de sua base de clientes para fins promocionais e de envolvimento. Você pode fazer isso no [Partner Center](https://partner.microsoft.com/dashboard) , criando um tipo de [grupo de clientes](create-customer-groups.md) , conhecido como um *segmento* que inclua os clientes do Windows 10 que demográficos ou os critérios de receita que você escolher.

Por exemplo, você poderia criar um segmento que inclua apenas os clientes que têm 50 anos ou mais, ou que inclua os clientes que gastaram mais de US$ 10 na Microsoft Store. Você também pode combinar esses critérios e criar um segmento que inclui todos os clientes com mais de 50 anos que gastaram mais de US$ 10 na Loja. 

Oferecemos alguns modelos de segmento para ajudá-lo a começar, mas você pode definir e combinar os critérios da maneira que quiser.

> [!TIP]
> Os segmentos podem ser usados para enviar [notificações direcionadas](send-push-notifications-to-your-apps-customers.md) ou [ofertas direcionadas](use-targeted-offers-to-maximize-engagement-and-conversions.md) para um grupo de clientes específicos como parte das campanhas de envolvimento.

Observações importantes sobre segmentos de cliente:
- Depois de salvar um segmento, leva 24 horas para você poder usá-lo para [notificações por push direcionadas](send-push-notifications-to-your-apps-customers.md).
- Os resultados de segmento são atualizados diariamente. Assim, você pode ver a contagem total de clientes em um segmento mudar de um dia para outro conforme os clientes passam a satisfazer os critérios de segmento.
- A maioria desses atributos é calculada usando todos os dados históricos, embora existam algumas exceções. Por exemplo, **Data de aquisição do aplicativo**, **ID da campanha**, **Data de visualização da página da Loja** e **Domínio de URI de referência** são limitados aos últimos 90 dias de dados.
- Os segmentos incluem somente os clientes que compraram o aplicativo no Windows 10 enquanto estavam conectados a com uma conta da Microsoft válida. 
- Os segmentos não incluem automaticamente todos os clientes com menos de 17 anos.

## <a name="to-create-a-customer-segment"></a>Para criar um segmento de cliente

1.  No [Partner Center](https://partner.microsoft.com/dashboard), expanda **envolver** no menu de navegação esquerdo e, em seguida, selecione **grupos de clientes**.
2.  Na página **Grupos de clientes**, siga um destes procedimentos:
 - Na seção **Meu grupos de clientes**, selecione **Criar novo grupo** para definir um segmento do zero. Na próxima página, selecione o botão de opção **Segmento**.
 - Na seção **Modelos de segmento**, selecione **Cópia** ao lado dos segmentos pré-definidos (que você pode usar como está ou modificar para atender às suas necessidades).
3.  Na caixa **Nome do grupo**, insira um nome para o segmento.
4.  Na lista **Incluir clientes desse aplicativo**, selecione um dos seus aplicativos para direcionar.
5.  Na seção **Definir condições de inclusão**, especifique os critérios de filtro para o segmento.

    Você pode escolher dentre uma variedade de critérios de filtro, incluindo **aquisições**, **origem de aquisição**, **demografia**, **classificação**, **previsão de rotatividade**, **compras Store**, **aquisições de armazenamento**e Store ** gastar**.

    Por exemplo, se você quiser criar um segmento que inclua somente os clientes do app com 18 a 24 anos, selecione os critérios de filtro [**Demografia**] [**Faixa etária**] [**é**] [**18 a 24**] nas listas suspensas.

    Você pode criar segmentos mais complexos usando consultas E/OU para incluir ou excluir clientes com base em vários atributos. Para adicionar uma consulta OU, selecione **+ instrução OU**. Para adicionar uma consulta ADICIONAR, selecione **Adicionar outro filtro**.

    Portanto, se você quisesse refinar esse segmento para incluir somente clientes masculinos que estão na faixa etária especificada, selecione **Adicionar outro filtro** e, em seguida, selecione os critérios de filtro adicionais [**Demográfico**] [**Sexo**] [**é**] [**Masculino**]. Para este exemplo, a **Definição do segmento** exibiria **Faixa etária == 18 a 24 && Sexo == Masculino**.

    ![Exemplo de critérios de filtro de um segmento](images/create-segment-inclusions.png)
6. Selecione **Salvar**.

> [!IMPORTANT]
> Não será possível usar um segmento que inclui poucos clientes. Se sua definição de segmento não incluir suficiente clientes, você poderá ajustar os critérios de segmento ou tentar novamente mais tarde, quando seu aplicativo talvez tiver adquirido mais clientes que atendem aos critérios de segmento.


## <a name="app-statistics"></a>Estatísticas de aplicativo

A seção **Estatísticas de aplicativo** do segmento fornece algumas informações sobre seu aplicativo, bem como o tamanho do segmento que acabou de criar.

Observe que **Clientes disponíveis do aplicativo** não reflete o número real dos clientes que compraram o aplicativo, mas apenas o número de clientes que estão disponíveis para serem incluídos em segmentos (ou seja, os clientes que podemos comprovar que satisfazem os requisitos de idade, adquiriram seu aplicativo no Windows 10 e que estão associados a uma conta da Microsoft válida).

Se você exibir os resultados e os **Clientes do segmento** forem **Pequenos**, o segmento não inclui clientes suficientes e o segmento é marcado como inativo. Segmentos inativos não podem ser usados para notificações ou outros recursos. Você poderá ativar e usar um segmento seguindo um destes procedimentos:

- Na seção **Definir condições de inclusão**, ajuste os critérios de filtro para que o segmento inclua mais clientes.
- Na página **Grupos de clientes**, selecione **Segmentos inativos** e, em seguida, selecione **Atualizar** para verificar se o segmento agora contém clientes suficiente (por exemplo, se mais clientes que atendem aos critérios de segmento baixaram seu aplicativo desde a criação do segmento ou se mais clientes atuais agora atendem aos critérios de segmento).
