---
title: Cadeias de caracteres localizadas
description: Saiba como localizar sequências de caracteres no Partner Center
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 11/17/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jogos, uwp, windows 10, Xbox, localizadas cadeias de caracteres, Partner Center
ms.openlocfilehash: 127f566dc5ae57b920d396623b6a84ff5d5eed96
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656591"
---
# <a name="configuring-localized-strings-in-partner-center"></a>Configurando cadeias de caracteres localizados no Partner Center

Você pode usar esta página para localizar todas as suas configurações Xbox Live para todos os idiomas que dá suporte a seu jogo. Todas as configurações de serviço que você criou em qualquer uma das páginas subsequentes do Xbox Live serão adicionadas ao arquivo que você deve baixar.

Você pode usar [Partner Center](https://partner.microsoft.com/dashboard) para configurar as cadeias de caracteres localizadas em todos os idiomas associados com seu jogo. Adicione configuração fazendo o seguinte:

1. Navegue até a **strings localizadas** seção em seu título, localizado sob **Services** > **Xbox Live** > **localizado cadeias de caracteres**.
2. Clique o **baixar** botão que irá baixar um arquivo localization.xml em seu computador local.

![Captura de tela da página de configuração de cadeias de caracteres localizadas no Partner Center](../../images/dev-center/localized-strings/localized-strings-1.png)

3. Você pode adicionar as cadeias de caracteres localizadas duplicando o <Value locale="en-US">Mazes reproduzidas</Value> marca e alterando o valor da localidade para a linguagem de sua escolha e o valor da cadeia de caracteres localizada. Você deve ter uma marca de pelo menos um valor dentro a localidade de exibição de desenvolvedor para evitar erros.

![Editar cadeias de caracteres localizadas](../../images/dev-center/localized-strings/localized-strings.gif)

4. Depois de adicionar cadeias de caracteres localizadas, você pode carregar o arquivo arrastando ou procurar seu computador local.

![Imagem do botão para carregar o arquivo localization.xml](../../images/dev-center/localized-strings/localized-strings-2.png)

Observe que os seguintes erros poderá aparecer quando você carregar o arquivo localization.xml:

| Erro | Motivo |
|---------------------------|-------------|
| Validação de XSD com falha: O elemento 'LocalizedString' no namespace 'http://config.mgt.xboxlive.com/schema/localization/1' não pode conter texto. Lista de possíveis elementos esperados: 'Value' no namespace 'http://config.mgt.xboxlive.com/schema/localization/1' | Isso ocorre quando o documento XML está malformado |
| Cadeia de localização faltar uma entrada para a localidade da exibição de desenvolvedor | Isso ocorre quando uma cadeia de caracteres localizada faltar uma entrada cuja localidade não coincide com a localidade da exibição de desenvolvimento |
| Validação de XSD com falha: O atributo 'local' é inválido – o valor ' 'é inválido de acordo com seu tipo de dados'http://config.mgt.xboxlive.com/schema/localization/1:NonEmptyString'-restrição padrão a falha. | Isso ocorre quando uma cadeia de caracteres localizada não possui o valor de localidade no <Value> tag|
