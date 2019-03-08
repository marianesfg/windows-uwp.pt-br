---
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Use esses métodos na API de envio a Microsoft Store para gerenciar os voos de pacote para aplicativos que são registrados em sua conta no Partner Center.
title: Gerenciar pacotes de pré-lançamento
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, API de envio da Microsoft Store, pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 8678ee4d73f13e241a2c72d6dac532289af13ced
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57601241"
---
# <a name="manage-package-flights"></a>Gerenciar pacotes de pré-lançamento

Use os métodos a seguir na API de envio da Microsoft Store para gerenciar pacotes de pré-lançamento para os apps. Para obter uma introdução à API de envio da Microsoft Store, inclusive pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md).

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
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="get-a-flight.md">Obtenha um voo de pacote</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">Criar um voo de pacote</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">Excluir um voo de pacote</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Pré-requisitos

Se você ainda não tiver feito isso, preencha todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Microsoft Store antes de tentar usar qualquer um desses métodos.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando os serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de voo do pacote](manage-flight-submissions.md)
