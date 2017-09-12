---
author: jnHs
Description: "Analise esta lista para ajudar a evitar problemas que, frequentemente, impedem aplicativos de serem certificados ou que podem ser identificados durante uma verificação pontual após o aplicativo ser publicado."
title: "Evitar erros comuns de certificação"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.author: wdg-dev-content
ms.date: 06/19/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 92e1b35c8eba7f1f3d3a32f0c994d3353a065419
ms.sourcegitcommit: fadde8afee46238443ec1cb71846d36c91db9fb9
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2017
---
# <a name="avoid-common-certification-failures"></a>Evitar erros comuns de certificação


Analise esta lista para ajudar a evitar problemas que, frequentemente, impedem aplicativos de serem certificados ou que podem ser identificados durante uma verificação pontual após o aplicativo ser publicado.

> [!NOTE]
> Certifique-se de analisar as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944) para garantir que seu aplicativo atenda a todos os requisitos listados aqui.

-   Envie seu aplicativo apenas quando ele estiver pronto. Fique à vontade para usar a descrição de seu aplicativo para mencionar recursos futuros, mas não deixe seções incompletas, links para páginas da Web em construção ou qualquer outra coisa que possa dar ao cliente a impressão de que seu aplicativo está incompleto.

-   [Teste seu aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviá-lo.

-   Teste seu aplicativo em diversas configurações diferentes para ter certeza de que ele esteja o mais estável possível.

-   Verifique se o aplicativo não falha sem conectividade de rede. Mesmo que seja necessária uma conexão para usar o aplicativo de fato, ele deve funcionar corretamente quando não há conexão.

-   [Forneça todas as informações necessárias](notes-for-certification.md) para uso do aplicativo, como nome de usuário e senha da conta de teste (caso seu aplicativo exija o logon em um serviço) ou etapas necessárias para acessar recursos ocultos ou bloqueados.

-   Inclua uma [política de privacidade](create-app-store-listings.md#privacy-policy) se o aplicativo exigir; por exemplo, se o aplicativo acessar qualquer tipo de informações pessoais de qualquer forma ou como exigido por lei. Para ajudar determinar se o aplicativo requer uma política de privacidade, consulte o [Contrato de Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058) e as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944).

-   Assegure que a descrição de seu aplicativo represente claramente o que seu aplicativo faz. Para obter ajuda, veja nossa orientação sobre [como elaborar uma ótima descrição de aplicativo](write-a-great-app-description.md).

-   Forneça respostas completas e precisas para todas as perguntas na seção [Classificações etárias](age-ratings.md).

-   Não [declare seu aplicativo como acessível](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que você tenha feito a engenharia e os testes especificamente para cenários de acessibilidade.

-   Se seu aplicativo usa APIs de comércio do namespace [**Windows.ApplicationModel.Store**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store), certifique-se de testar o aplicativo e verificar se ele manipula exceções típicas. Além disso, certifique-se de que seu aplicativo usa a classe [**CurrentApp**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentApp) e não [**CurrentAppSimulator**](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.Store.CurrentAppSimulator), que é usada somente para testes. (observe que se o aplicativo for destinado ao Windows 10, versão 1607 ou posterior, recomendamos que você use membros do namespace [Windows.Services.Store](https://docs.microsoft.com/uwp/api/windows.services.store) em vez do namespace Windows.ApplicationModel.Store).


 

 




