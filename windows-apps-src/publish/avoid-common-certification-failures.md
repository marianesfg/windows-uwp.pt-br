---
author: jnHs
Description: "Analise esta lista para ajudar a evitar problemas que, frequentemente, impedem aplicativos de serem certificados ou que podem ser identificados durante uma verificação pontual após o aplicativo ser publicado."
title: "Evitar erros comuns de certificação"
ms.assetid: 9E9E3841-2F9B-42D4-B5F8-4C7C31E42E3D
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 6af2842eeb5eeebfc9ffc5a7a3ec98fdcebddb74

---

# Evitar erros comuns de certificação


Analise esta lista para ajudar a evitar problemas que, frequentemente, impedem aplicativos de serem certificados ou que podem ser identificados durante uma verificação pontual após o aplicativo ser publicado.

> 
            **Observação**  Você também deve analisar as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944) para garantir que seu aplicativo atenda a todos os requisitos listados aqui.

 

-   Envie seu aplicativo apenas quando ele estiver pronto. Fique à vontade para usar a descrição de seu aplicativo para mencionar recursos futuros, mas não deixe seções incompletas, links para páginas da Web em construção ou qualquer outra coisa que possa dar ao cliente a impressão de que seu aplicativo está incompleto.

-   
            [Teste seu aplicativo com o Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) antes de enviá-lo.

-   Teste seu aplicativo em diversas configurações diferentes para ter certeza de que ele esteja o mais estável possível.

-   Certifique-se de que ele não falhe sem conectividade de rede. Mesmo que seja necessária uma conexão para usar o aplicativo de fato, ele deve funcionar corretamente quando não há conexão.
-   Assegure que a descrição de seu aplicativo represente claramente o que seu aplicativo faz. Para obter ajuda, veja nossa orientação sobre [como elaborar uma ótima descrição de aplicativo](write-a-great-app-description.md).

-   Certifique-se de fornecer respostas completas e precisas para todas as perguntas na seção [Classificações etárias](age-ratings.md).

-   Se seu aplicativo usa APIs de comércio do namespace [**Windows.ApplicationModel.Store**](https://msdn.microsoft.com/library/windows/apps/br225197), certifique-se de testar o aplicativo e verificar se ele manipula exceções típicas. Além disso, certifique-se de que seu aplicativo use a classe [**CurrentApp**](https://msdn.microsoft.com/library/windows/apps/hh779765) (não a classe [**CurrentAppSimulator**](https://msdn.microsoft.com/library/windows/apps/hh779766), que é para testar somente finalidades).

-   Não [declare seu aplicativo como acessível](app-declarations.md#this-app-has-been-tested-to-meet-accessibility-guidelines), a menos que você tenha feito a engenharia e os testes especificamente para cenários de acessibilidade.

-   Certifique-se de [fornecer todas as informações necessárias](notes-for-certification.md) para uso do aplicativo, como nome de usuário e senha da conta de teste (caso seu aplicativo exija o logon em um serviço) ou etapas necessárias para acessar recursos ocultos ou bloqueados.

 

 







<!--HONumber=Jun16_HO4-->


