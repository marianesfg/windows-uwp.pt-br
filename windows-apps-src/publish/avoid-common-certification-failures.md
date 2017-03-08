---
author: jnHs
Description: "Analise esta lista para ajudar a evitar problemas que, frequentemente, impedem apps de serem certificados ou que podem ser identificados durante uma verificação pontual após o app ser publicado."
title: "Evitar erros comuns de certificação"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: f8b61d14b46614680b84da5aa7e4413159a0cfb1
ms.lasthandoff: 02/07/2017

---

# <a name="avoid-common-certification-failures"></a>Evitar erros comuns de certificação


Analise esta lista para ajudar a evitar problemas que, frequentemente, impedem apps de serem certificados ou que podem ser identificados durante uma verificação pontual após o app ser publicado.

> **Observação**  Você também deve analisar as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944) para garantir que seu app atenda a todos os requisitos listados aqui.

 

-   Envie seu app apenas quando ele estiver pronto. Fique à vontade para usar a descrição de seu app para mencionar recursos futuros, mas não deixe seções incompletas, links para páginas da Web em construção ou qualquer outra coisa que possa dar ao cliente a impressão de que seu app está incompleto.

-   [Teste seu app com o Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) antes de enviá-lo.

-   Teste seu app em diversas configurações diferentes para ter certeza de que ele esteja o mais estável possível.

-   Certifique-se de que ele não falhe sem conectividade de rede. Mesmo que seja necessária uma conexão para usar o app de fato, ele deve funcionar corretamente quando não há conexão.
-   Assegure que a descrição de seu app represente claramente o que seu app faz. Para obter ajuda, veja nossa orientação sobre [como elaborar uma ótima descrição de app](write-a-great-app-description.md).

-   Certifique-se de fornecer respostas completas e precisas para todas as perguntas na seção [Classificações etárias](age-ratings.md).

-   Se seu app usa APIs de comércio do namespace [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197), certifique-se de testar o app e verificar se ele manipula exceções típicas. Além disso, certifique-se de que seu app use a classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (não a classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), que é para testar somente finalidades).

-   Não [declare seu app como acessível](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que você tenha feito a engenharia e os testes especificamente para cenários de acessibilidade.

-   Certifique-se de [fornecer todas as informações necessárias](notes-for-certification.md) para uso do app, como nome de usuário e senha da conta de teste (caso seu app exija o logon em um serviço) ou etapas necessárias para acessar recursos ocultos ou bloqueados.

 

 





