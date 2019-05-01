---
Description: Ao enviar seu aplicativo, você tem a opção de usar a página Notas para certificação para fornecer informações adicionais aos testadores de certificação. Essas informações podem ajudar a garantir que seu aplicativo seja testado corretamente.
title: Notas para certificação
ms.assetid: 4A740A5F-F39F-4FE2-9391-EE00DB46B25A
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, notas para testadores
ms.localizationpriority: medium
ms.openlocfilehash: 2fb465705e2d9a9ef653fe4b31490ed2af6afe4f
ms.sourcegitcommit: fca0132794ec187e90b2ebdad862f22d9f6c0db8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/24/2019
ms.locfileid: "63827529"
---
# <a name="notes-for-certification"></a>Notas para certificação


Ao enviar seu aplicativo, você tem a opção de usar a página **Notas para certificação** para fornecer informações adicionais aos testadores de certificação. Essas informações podem ajudar a garantir que seu aplicativo seja testado corretamente. A inclusão dessas notas é particularmente importante para os produtos que usam os Serviços Xbox Live e/ou que exigem o logon em uma conta. Se não pudermos testar seu envio por completo, ele poderá falhar na certificação.

Certifique-se de incluir (se aplicável ao seu aplicativo):

-   **Nomes de usuário e senhas para contas de teste**: Se seu aplicativo requer que os usuários façam logon um serviço ou conta de mídia social, forneça o nome de usuário e senha para uma conta de teste. Os testadores de certificação usarão a conta ao examinar o aplicativo.

-   **As etapas para acessar recursos ocultos ou bloqueados**: Descrevem brevemente como os testadores podem acessar quaisquer recursos, modos ou conteúdo que não sejam óbvio. Os aplicativos que parecerem incompletos podem não ser certificados.

-   **Etapas para verificar o uso de áudio de fundo**: Se o seu aplicativo permite que o áudio seja executado em segundo plano, os testadores podem precisar de instruções sobre como acessar esse recurso para que eles possam confirmar se ele funciona corretamente.

-  **Espera-se as diferenças no comportamento com base na região ou outras configurações de cliente**: Por exemplo, se os clientes em diferentes regiões verá conteúdo diferente, verifique se chamar isso para que os testadores entenderam as diferenças e examine adequadamente.

-   **Informações sobre o que é alterado em uma atualização do aplicativo**: Para atualizações de aplicativos já publicados, talvez seja conveniente informar aos testadores o que foi alterado, principalmente se os seus pacotes forem os mesmos e você está apenas alterando um detalhe do aplicativo (como adicionar mais capturas de tela, alterar a categoria do aplicativo ou editar a descrição).

-   **A data em que você está inserindo as notas de**: Isso é particularmente importante se você estiver usando uma área restrita para desenvolvimento no Partner Center (por exemplo, esse é o caso para qualquer jogo que se integra com o Xbox Live), desde as notas que você insira quando a publicação para uma área restrita permanecerá quando você solicitar certificação. Ver a data ajuda os testadores a avaliar se há algum problema temporário que não se aplica mais.

-  **Qualquer outra coisa você acha que os testadores precisa compreender sobre o envio**

Ao considerar o que escrever, lembre-se:

-   **Uma pessoa real será Leia estas notas.** Os testadores valorizarão uma observação educada e instruções úteis.

-   **Ser sucinta e manter as instruções simples.** Se você realmente precisar explicar algo em detalhes, poderá incluir a URL a uma página com mais informações. No entanto, tenha em mente que os clientes do seu app não verão essas notas. Se você achar que precisa fornecer instruções complicadas para testar seu app, considere simplificá-lo para que os clientes (e os testadores) saibam como usá-lo.

-   **Serviços e componentes externos devem estar online e disponível.** Se o app precisar se conectar a um serviço para funcionar, verifique se o serviço estará online e disponível. Inclua qualquer informação sobre o serviço que sejam necessárias aos testadores, como as informações de logon. Se o aplicativo não puder se conectar a um serviço necessário para o teste, ele poderá ser reprovado na certificação.

 

 




