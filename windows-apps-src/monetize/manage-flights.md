---
author: mcleanbyron
ms.assetid: 37F2C162-4910-4336-BEED-8536C88DCA65
description: "Use estes métodos na API de envio da Windows Store para gerenciar pacotes de pré-lançamento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows."
title: "Gerenciar pacotes de pré-lançamento usando a API de envio da Windows Store"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: de1c23d721cee67d813520e3a23eb553cd90b7e9

---

# Gerenciar pacotes de pré-lançamento usando a API de envio da Windows Store




Use os métodos a seguir na API de envio da Windows Store para gerenciar pacotes de pré-lançamento dos aplicativos que estão registrados em sua conta do Centro de Desenvolvimento do Windows. Para obter uma introdução à API de envio da Windows Store, incluindo os pré-requisitos para usar a API, consulte [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md).

>**Observação**&nbsp;&nbsp;Estes métodos só podem ser usados para contas do Centro de Desenvolvimento do Windows que receberam permissões para usar a API de envio da Windows Store. Nem todas as contas têm essa permissão habilitada. Esses métodos só podem ser usados para obter, criar ou excluir os pacotes de pré-lançamento. Para criar envios para pacotes de pré-lançamento, consulte os métodos em [Gerenciar envios do pacote de pré-lançamento](manage-flight-submissions.md).

| Método        | URI    | Descrição                                                                 |
|---------------|--------|-----------------------------------------------------------------------------|
| GET | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Obtém dados para um pacote de pré-lançamento de um aplicativo que está registrado em sua conta do Centro de Desenvolvimento do Windows. Para obter mais informações, consulte [Obter um pacote de pré-lançamento](get-a-flight.md). |
| POST | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights``` | Cria um novo pacote de pré-lançamento. Para saber mais, veja [Criar um pacote de pré-lançamento](create-a-flight.md).|
| DELETE | ```https://manage.devcenter.microsoft.com/v1.0/my/applications/{applicationId}/flights/{flightId}``` | Exclui um pacote de pré-lançamento. Para saber mais, veja [Excluir um pacote de pré-lançamento](delete-a-flight.md). |


## Pré-requisitos

Se você não tiver feito isso, conclua todos os [pré-requisitos](create-and-manage-submissions-using-windows-store-services.md#prerequisites) da API de envio da Windows Store antes de tentar usar qualquer um desses métodos.

## Tópicos relacionados

* [Criar e gerenciar envios usando serviços da Windows Store](create-and-manage-submissions-using-windows-store-services.md)
* [Gerenciar envios de pacote de pré-lançamento](manage-flight-submissions.md)



<!--HONumber=Aug16_HO5-->


