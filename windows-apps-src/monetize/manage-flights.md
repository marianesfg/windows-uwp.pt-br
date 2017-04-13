---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Use estes métodos na API de envio da Windows Store para gerenciar pacotes de pré-lançamento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: "Gerenciar pacotes de pré-lançamento"
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, API de envio da Windows Store, pré-lançamento"
ms.openlocfilehash: b560c2c12dc2fd7984287d039d20c121698aef85
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="manage-package-flights"></a>Gerenciar pacotes de pré-lançamento

Use os métodos a seguir na API de envio da Windows Store para gerenciar pacotes de pré-lançamento para os aplicativos. Para obter uma introdução à API de envio da Windows Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Essa permissão está sendo habilitada para contas de desenvolvedor em estágios, e nem todas as contas têm essa permissão habilitado no momento. Para solicitar acesso anterior, fazer logon no painel do Centro de Desenvolvimento, clique em **Comentários** na parte inferior do painel, selecione **API de envio** para a área de comentários e envie sua solicitação. Você receberá um email quando essa permissão for habilitada em sua conta.

Esses métodos só podem ser usados para obter, criar ou excluir os pacotes de pré-lançamento. Para criar envios para pacotes de pré-lançamento, consulte os métodos em [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).

<table>
<colgroup>
<col width="10%" />
<col width="30%" />
<col width="60%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Método</th>
<th align="left">URI</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">GET</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[Obter um pacote de pré-lançamento](get-a-flight.md)</td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights```</td>
<td align="left">[Criar um pacote de pré-lançamento](create-a-flight.md)</td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}```</td>
<td align="left">[Excluir um pacote de pré-lançamento](delete-a-flight.md)</td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Pré-requisitos

Se você ainda não tiver feito isso, preencha todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Windows Store antes de tentar usar qualquer um desses métodos.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)
