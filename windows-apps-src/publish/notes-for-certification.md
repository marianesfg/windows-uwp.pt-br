---
author: jnHs
Description: As you submit your app, you have the option to use the Notes for certification page to provide additional info to the certification testers. This info can help ensure that your app is tested correctly.
title: Notas para certificação
ms.assetid: 4A740A5F-F39F-4FE2-9391-EE00DB46B25A
ms.author: wdg-dev-content
ms.date: 04/03/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, notas para testadores
ms.localizationpriority: medium
ms.openlocfilehash: 741925a3ac49273bd7ba826dfbadd2e18724f307
ms.sourcegitcommit: 4f6dc806229a8226894c55ceb6d6eab391ec8ab6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/20/2018
ms.locfileid: "4085671"
---
# <a name="notes-for-certification"></a>Notas para certificação


Ao enviar seu aplicativo, você tem a opção de usar a página **Notas para certificação** para fornecer informações adicionais aos testadores de certificação. Essas informações podem ajudar a garantir que seu aplicativo seja testado corretamente. A inclusão dessas notas é particularmente importante para os produtos que usam os Serviços Xbox Live e/ou que exigem o logon em uma conta. Se não pudermos testar seu envio por completo, ele poderá falhar na certificação.

Certifique-se de incluir (se aplicável ao seu aplicativo):

-   **Nomes de usuário e senhas para testar contas** Se o seu aplicativo exigir que os usuários façam logon em uma conta de mídia social ou de serviço, forneça o nome de usuário e a senha para uma conta de teste. Os testadores de certificação usarão a conta ao examinar o aplicativo.

-   **As etapas para acessar recursos ocultos ou bloqueados** Descreva brevemente como os testadores podem acessar quaisquer recursos, modos ou conteúdo que talvez não sejam óbvios. Os aplicativos que parecerem incompletos podem não ser certificados.

-   **Etapas para verificar o uso de áudio em segundo plano** Se o seu app permitir que o áudio seja executado em segundo plano, talvez os testadores precisem de instruções sobre como acessar esse recurso para que eles possam confirmar se ele funciona corretamente.

-  **Diferenças esperadas no comportamento com base na região ou em outras configurações de cliente** Por exemplo, se clientes em diferentes regiões verão conteúdo diferente, deixe isso claro de forma que os testadores entendam as diferenças e analisem adequadamente.

-   **Informações sobre o que foi alterado em uma atualização do app** Para fazer atualizações de apps já publicados, talvez seja conveniente informar aos testadores o que foi alterado, principalmente se os seus pacotes forem os mesmos e se você está apenas alterando um detalhe do aplicativo (como adicionar mais capturas de tela, alterar a categoria do aplicativo ou editar a descrição).

-   **A data em que você está inserindo as notas** Isso é especialmente importante se você estiver usando uma área restrita de desenvolvimento no Centro de Desenvolvimento (por exemplo, esse é o caso para qualquer jogo que se integra ao Xbox Live), já que as notas inseridas ao publicar em uma área restrita permanecerão quando você solicitar a certificação. Ver a data ajuda os testadores a avaliar se há algum problema temporário que não se aplica mais.

-  **Qualquer outra coisa que você ache necessário que os testadores entendam sobre o seu envio**

Ao considerar o que escrever, lembre-se:

-   **Uma pessoa real lerá essas notas.** Os testadores valorizarão uma observação educada e instruções úteis.
-   **Seja sucinto e crie instruções simples.** Se você realmente precisar explicar algo em detalhes, poderá incluir a URL a uma página com mais informações. No entanto, tenha em mente que os clientes do seu app não verão essas notas. Se você achar que precisa fornecer instruções complicadas para testar seu app, considere simplificá-lo para que os clientes (e os testadores) saibam como usá-lo.
-   **Serviços e componentes externos devem estar online e disponíveis.** Se o app precisar se conectar a um serviço para funcionar, verifique se o serviço estará online e disponível. Inclua qualquer informação sobre o serviço que sejam necessárias aos testadores, como as informações de logon. Se o aplicativo não puder se conectar a um serviço necessário para o teste, ele poderá ser reprovado na certificação.

 

 




