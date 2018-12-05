---
Description: Manage submission options such as publishing hold options, notes for certification, and more.
title: Gerenciar opções de envio
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, suspensão de publicação, data de publicação, enviar para publicar, aprovação de funcionalidade restrita
ms.localizationpriority: medium
ms.openlocfilehash: 3fb075a4d8766f4f9bfc352160c6a1f5d99d9a0e
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8696948"
---
# <a name="manage-submission-options"></a>Gerenciar opções de envio

A página **Opções de envio** do processo de envio do app é onde você pode fornecer mais informações para nos ajudar a testar seu produto adequadamente. Esta é uma etapa opcional, mas é recomendada para muitos envios. Opcionalmente, você também pode definir opções de suspensão de publicação, se deseja atrasar o processo de publicação.


## <a name="publishing-hold-options"></a>Opções de suspensão de publicação

Por padrão, publicaremos seu envio assim que ele passar na certificação (ou de acordo com qualquer data especificada na seção [Agendar](configure-precise-release-scheduling.md) da página **Preço e disponibilidade**). Opcionalmente, você pode optar por suspender a publicação de seu envio até uma data específica, ou até você indicar manualmente o que deve ser publicado. As opções desta seção são descritas a seguir. 


### <a name="publish-your-submission-as-soon-as-it-passes-certification-or-per-dates-you-specify"></a>Publicar seu envio assim que ele for aprovado na certificação (ou de acordo com as datas especificadas por você)

**Publicar este envio assim que ele for aprovado na certificação (ou de acordo com as datas selecionadas na seção Agendar)** é a seleção padrão e significa que seu envio iniciará o processo de publicação assim que ele for aprovado na certificação, a menos que você tenha configurado datas na seção [Agendar](configure-precise-release-scheduling.md) da página **Preço e disponibilidade**.   

Para a maioria dos envios, recomendamos deixar a seção **Opções de suspensão de publicação** definida para essa opção. Se quiser especificar determinadas datas para que seu envio seja publicado, use **Publicar o envio assim que for aprovado na certificação (ou de acordo com as datas selecionadas na seção Agendar)**. Deixar essa seção definida como a opção padrão não fará com que o envio seja publicado até as datas definidas na seção **Agendar**. As datas que você selecionou na seção **Agendar** serão usadas para determinar quando seu produto se torna disponível para os clientes na loja.


### <a name="publish-your-submission-manually"></a>Publicar seu envio manualmente

Se você ainda não quiser definir uma data de lançamento e preferir que o envio permaneça não publicado até você decidir manualmente iniciar o processo de publicação, escolha **Não publicar este envio até eu selecionar Publicar agora**. Escolher esta opção significa que seu envio não será publicado até você indicar que ele deve ser. Depois de certificação de passagens de seu envio, você pode publicá-lo selecionando **Publicar agora** na página de status da certificação ou selecionando uma data específica da mesma maneira, conforme descrito a seguir.


### <a name="start-publishing-your-submission-on-a-certain-date"></a>Iniciar a publicação de seu envio em uma data específica

Escolha **Iniciar a publicação deste envio em** para garantir que o envio não seja publicado até uma data específica. Com esta opção, seu envio será lançado assim que possível na data especificada ou depois dela A data deve ser pelo menos 24 horas no futuro. Com a data, você também pode especificar a hora em que o envio deve começar a ser publicado. 

Você pode alterar essa data de lançamento depois de enviar seu produto, desde que ele ainda não tenha entrado na etapa publicação ainda. 
 
Conforme observado anteriormente, se você quiser especificar determinadas datas de publicação para seu envio, use **Publicar este envio assim que ele for aprovado na certificação (ou de acordo com as datas selecionadas por você na seção Agendar)** e deixe **Opções de suspensão de publicação** definido como a seleção padrão. Usando a opção **Iniciar a publicação deste envio em** significa que seu envio não iniciará o processo de publicação até essa data, mas atrasos durante a certificação ou publicação podem fazer com a data real de lançamento seja posterior à data selecionada. 


## <a name="notes-for-certification"></a>Notas para certificação

Ao enviar seu aplicativo, você tem a opção de usar a seção **Notas para certificação** para fornecer informações adicionais aos testadores de certificação. Essas informações podem ajudar a garantir que seu aplicativo seja testado corretamente. 

Para obter mais informações, consulte [Notas para certificação](notes-for-certification.md).


## <a name="restricted-capabilities"></a>Funcionalidades restritas

Se detectarmos que seus pacotes declaram quaisquer [funcionalidades restritas](../packaging/app-capability-declarations.md#restricted-capabilities), você precisará fornecer as informações nesta seção para receber aprovação. Conte-nos por que seu aplicativo precisa declarar a funcionalidade e como elas são usadas para cada funcionalidade. Certifique-se de fornecer o máximo de detalhes possível para nos ajudar a entender por que precisa declarar a funcionalidade do seu produto. 

Durante o processo de certificação, nossos testadores analisarão as informações que você fornecer para determinar se o seu envio será aprovado para usar a funcionalidade. Observe que isso pode adicionar algum tempo adicional para concluir o processo de certificação do seu envio. Se nós aprovarmos o uso da funcionalidade, seu aplicativo continuará o restante do processo de certificação. Você geralmente não precisa repetir o processo de aprovação da funcionalidade ao enviar atualizações do seu aplicativo (a menos que você declare funcionalidades adicionais). 

Se nós não aprovarmos o uso da funcionalidade, seu haverá falha na certificação do seu envio e forneceremos comentários no relatório de certificação. Em seguida, você tem a opção de criar um novo envio e carregar os pacotes que não declaram a funcionalidade ou, se aplicável, resolver problemas relacionados ao uso da funcionalidade e aprovação da solicitação em um novo envio.

Observe que existem alguns recursos restritos que raramente serão aprovados. Para obter mais informações sobre cada funcionalidade restrita, consulte [Declarações de recursos de aplicativos](../packaging/app-capability-declarations.md#restricted-capabilities).

