---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: Use estes métodos na API de envio da Microsoft Store para gerenciar pacotes de pré-lançamento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows.
title: Gerenciar pacotes de pré-lançamento
ms.author: mcleans
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, API de envio da Microsoft Store, pré-lançamento
ms.localizationpriority: medium
ms.openlocfilehash: 7b5e3c100ece6ec79abad0efbf4797b0102959cb
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1661866"
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
<td align="left"><a href="get-a-flight.md">Obter um pacote de pré-lançamento</a></td>
</tr>
<tr>
<td align="left">POST</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights</td>
<td align="left"><a href="create-a-flight.md">Criar um pacote de pré-lançamento</a></td>
</tr>
<tr>
<td align="left">DELETE</td>
<td align="left">https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}</td>
<td align="left"><a href="delete-a-flight.md">Excluir um pacote de pré-lançamento</a></td>
</tr>
</tbody>
</table>

## <a name="prerequisites"></a>Pré-requisitos

Se você ainda não tiver feito isso, preencha todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Microsoft Store antes de tentar usar qualquer um desses métodos.

## <a name="related-topics"></a>Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Microsoft Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)