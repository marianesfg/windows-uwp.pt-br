---
author: jnHs
Description: "Quando você terminar de criar o envio do seu aplicativo, clique em Enviar à Loja. Ele entrará na etapa de certificação."
title: "O processo de certificação de aplicativos"
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.sourcegitcommit: 4ea19e85d1e151dd1e03d5acf085c186613be35f
ms.openlocfilehash: 579d1ef306123f765e19fc9ab3b02c064b690aee

---

# O processo de certificação de aplicativos


Quando você terminar de criar o envio do seu aplicativo, clique em **Enviar à Loja**. Ele entrará na etapa de certificação. Esse processo geralmente é concluído dentro de algumas horas, mas em alguns casos pode demorar até um dia útil. Depois que seu envio é aprovado na certificação, pode levar até 16 horas para que os clientes vejam os detalhes do aplicativo (ou suas atualizações para um aplicativo publicado anteriormente) na loja. Você verá uma notificação quando seu envio for publicado e estiver disponível para os clientes, e o status do aplicativo no painel será **Na Loja**.

## Pré-processamento

Depois que você carrega os pacotes do aplicativo com êxito e envia o aplicativo para certificação, os pacotes são colocados em fila para testes automatizados. Exibiremos uma mensagem se detectamos eventuais erros durante o pré-processamento. Para obter mais informações sobre possíveis erros, consulte [Resolver erros de envio](resolve-submission-errors.md).

## Certificação

Durante essa fase, vários testes são realizados:

-   
            **Testes de segurança:** o primeiro teste verifica se há vírus e malware nos pacotes do aplicativo. Se o aplicativo falhar no teste, você precisará conferir o sistema de desenvolvimento executando o software antivírus mais recente e, em seguida, recompilar o pacote do aplicativo em um sistema limpo.
-   
            **Testes de conformidade técnica:** a conformidade técnica é testada pelo Kit de Certificação de Aplicativos Windows. (Você deve sempre garantir o [teste do aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviá-lo à loja).
-   
            **Conformidade de conteúdo:** o tempo necessário varia dependendo da complexidade do aplicativo, da quantidade de conteúdo visual e de quantos aplicativos foram enviados recentemente. Não se esqueça de fornecer todas as informações importantes aos testadores na página [Notas para certificação](notes-for-certification.md).

Após a conclusão do processo de certificação, você receberá um relatório de certificação informando se o aplicativo passou ou não na certificação. Se ele não for aprovado, o relatório indicará em qual teste ele falhou ou qual [política](https://msdn.microsoft.com/library/windows/apps/dn764944) não foi atendida. Depois de corrigir o problema, você pode criar um novo envio para o seu aplicativo para iniciar o processo de certificação novamente.

## Versão

Quando seu aplicativo for aprovado na certificação, ele estará pronto para ser movido para o processo **Publicação**. Se você indicou que seu envio deve ser publicado assim que possível, isso acontecerá imediatamente. Se você tiver especificado que ele não deve ser lançado antes de uma data específica, vamos aguardar até essa data, a menos que você clique no link **Change release date**. Se você indicou que deseja publicar o envio manualmente, não o publicaremos até você indicar que deveríamos, clicando no botão **Publicar agora**, ou se você clicar no link **Alterar data do lançamento** e selecionar uma data específica.

## Publicação

Os pacotes do aplicativo são assinados digitalmente para ficarem protegidos contra falsificação após serem lançados. Após o início dessa fase, você não pode mais cancelar o envio nem alterar a data de lançamento do aplicativo.

Enquanto seu aplicativo está na fase de publicação, o link **Mostrar detalhes** na coluna Status de envio do seu aplicativo o avisará quando seus novos pacotes e detalhes de listagem da Loja forem disponibilizados para clientes em cada uma das versões de seu sistema operacional com suporte. Seu aplicativo permanecerá na fase de publicação até os novos pacotes e detalhes estarem disponíveis para todos os clientes em potencial do seu aplicativo, o que pode levar até 16 horas. 

## Na Loja 

Depois de passar pelas etapas acima com êxito, o status do envio será alterado de **Publicação** para **Na Loja**. Seu envio estarão disponível na Windows Store para download (a menos que você tenha escolhido outra opção de [distribuição e a visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility)). 


            **Observação**  Também fazemos verificações específicas nos aplicativos após eles serem publicados, para que possamos identificar problemas potenciais e garantir que seu aplicativo esteja em conformidade com todas as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944). Se encontrarmos quaisquer problemas, você será notificado sobre o erro e como corrigi-lo, se aplicável, ou se ele foi removido da loja.

 

 

 







<!--HONumber=Jun16_HO5-->


