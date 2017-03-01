---
author: jnHs
Description: "Ao enviar seu app, você tem a opção de usar a página Notas para certificação para fornecer informações adicionais aos testadores de certificação. Essas informações podem ajudar a garantir que seu app seja testado corretamente."
title: "Notas para certificação"
ms.assetid: 4A740A5F-F39F-4FE2-9391-EE00DB46B25A
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 92a110879f6ff92b3e1e41964ff6acea6b7cb5ec
ms.lasthandoff: 02/07/2017

---

# <a name="notes-for-certification"></a>Notas para certificação


Ao enviar seu app, você tem a opção de usar a página **Notas para certificação** para fornecer informações adicionais aos testadores de certificação. Essas informações podem ajudar a garantir que seu app seja testado corretamente.

Certifique-se de incluir (se aplicável ao seu app):

-   **Nomes de usuário e senhas para testar as contas**

    Se o seu app requer que os usuários façam logon em um serviço, forneça o nome de usuário e a senha para uma conta de teste. Os testadores de certificação usarão a conta ao examinar o app.

-   **Etapas para acessar recursos ocultos ou bloqueados**

    Se o seu app tem recursos que podem não ser óbvios para os testadores, descreva brevemente como eles podem acessar esses recursos. Os apps que parecerem incompletos podem não ser certificados.

-   **Etapas para verificar o uso de áudio em segundo plano**

    Se o seu app permite que o áudio seja executado em segundo plano, os testadores podem precisar de instruções sobre como acessar esse recurso para que eles possam confirmar se ele funciona corretamente.

-   **Informações sobre o que mudou em uma atualização de app**

    Para atualizações de apps já publicados, talvez seja conveniente informar aos testadores o que foi alterado, principalmente se os seus pacotes forem os mesmos e você está apenas alterando um detalhe do app (como adicionar mais capturas de tela, alterar a categoria do app ou editar a descrição).

Ao considerar o que escrever, lembre-se:

-   **Uma pessoa real lerá essas notas.** Os testadores valorizarão uma observação educada e instruções úteis.
-   **Seja sucinto e crie instruções simples.** Se você realmente precisar dar mais explicações, poderá incluir um link para uma página com mais informações. Lembre-se de que os clientes do seu app não verão esses detalhes. Se precisar fornecer instruções complicadas para testar seu app, considere simplificá-lo para que os clientes saibam como usá-lo.
-   **Serviços e componentes externos devem estar online e disponíveis.** Se o app se conecta a um serviço como parte de sua função, verifique se o serviço estará online e disponível. Inclua informações sobre o serviço necessárias aos testadores. Se o app não puder se conectar a um serviço necessário para o teste, ele poderá ser reprovado na certificação.

 

 





