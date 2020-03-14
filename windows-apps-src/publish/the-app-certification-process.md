---
Description: Quando você terminar de criar o envio do aplicativo e clicar em enviar para a loja, o envio entrará na etapa de certificação.
title: O processo de certificação de aplicativos
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: Windows 10, UWP, publicação, pré-processamento, certificação, versão, pendente, envio, publicação, status, tempo
ms.localizationpriority: medium
ms.openlocfilehash: d88d8deeb467f186f120fb8c1e579d5c9222aaf1
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79210952"
---
# <a name="the-app-certification-process"></a>O processo de certificação de aplicativos

Quando você terminar de criar o envio do seu aplicativo, clique em **Enviar à Store**, o envio entrará na etapa de certificação. Esse processo geralmente é concluído dentro de algumas horas, mas em alguns casos pode demorar até três dias úteis. Depois que o envio passa pela certificação, pode levar até 24 horas para que os clientes vejam a listagem do aplicativo para um novo envio ou para um envio atualizado com alterações nos pacotes. Se sua atualização alterar apenas os detalhes da listagem do repositório, o processo de publicação será concluído em menos de uma hora.  Você será notificado quando o envio for publicado e o status do aplicativo no painel estará **na loja**.

## <a name="preprocessing"></a>Pré-processamento

Depois que você carrega os pacotes do aplicativo com êxito e envia o aplicativo para certificação, os pacotes são colocados em fila para testes automatizados. Exibiremos uma mensagem se detectamos eventuais erros durante o pré-processamento. Para obter mais informações sobre possíveis erros, consulte [Resolver erros de envio](resolve-submission-errors.md).

## <a name="certification"></a>Certificação

Durante essa fase, vários testes são realizados:

-   **Testes de segurança:** O primeiro teste verifica se há vírus e malware nos pacotes do aplicativo. Se o aplicativo falhar no teste, você precisará conferir o sistema de desenvolvimento executando o software antivírus mais recente e, em seguida, recompilar o pacote do aplicativo em um sistema limpo.
-   **Testes de conformidade técnica:** a conformidade técnica é testada pelo Kit de Certificação de Aplicativos Windows. (Você deve sempre garantir o [teste do aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviá-lo à loja).
-   **Conformidade de conteúdo:** o tempo necessário varia dependendo da complexidade do aplicativo, da quantidade de conteúdo visual e de quantos aplicativos foram enviados recentemente. Não se esqueça de fornecer todas as informações importantes aos testadores na página [Notas para certificação](notes-for-certification.md).

Após a conclusão do processo de certificação, você receberá um relatório de certificação informando se o aplicativo passou ou não na certificação. Se ele não for aprovado, o relatório indicará em qual teste ele falhou ou qual [política](store-policies.md) não foi atendida. Depois de corrigir o problema, você pode criar um novo envio para o seu aplicativo para iniciar o processo de certificação novamente.

## <a name="release"></a>Versão

Quando seu aplicativo passa a certificação, ele está pronto para mudar para o processo de **publicação** .

- Se você indicou que o envio deve ser publicado assim que possível (a opção padrão), o processo de publicação será iniciado imediatamente.
- Se esta for a primeira vez que você publicou o aplicativo e tiver especificado uma **data de lançamento** na seção [agenda](configure-precise-release-scheduling.md#release) , o aplicativo ficará disponível de acordo com as seleções de **data de lançamento** .
- Se você usou [Opções de suspensão de publicação](manage-submission-options.md#publishing-hold-options) para especificar que ela não deve ser liberada até uma determinada data, Aguardaremos até essa data para iniciar o processo de publicação, a menos que você selecione **alterar Data de lançamento**.
- Se você usou [Opções de suspensão de publicação](manage-submission-options.md#publishing-hold-options) para especificar que deseja publicar o envio manualmente, não iniciaremos o processo de publicação até que você selecione **Publicar agora** (ou selecione **alterar Data de liberação** e escolha uma data específica).


## <a name="publishing"></a>Publicação

Os pacotes do aplicativo são assinados digitalmente para ficarem protegidos contra falsificação após serem lançados. Após o início dessa fase, você não pode mais cancelar o envio nem alterar a data de lançamento do aplicativo.

Para novos aplicativos e atualizações que incluem alterações nos pacotes do aplicativo, o processo de publicação será concluído dentro de 24 horas. Para atualizações que só alteram opções como detalhes da listagem de armazenamento, mas não alteram os pacotes do aplicativo, o processo de publicação levará menos de uma hora.

Enquanto seu aplicativo está na fase de publicação, o link **Mostrar detalhes** na coluna status para o envio do aplicativo permite que você saiba quando os novos pacotes e detalhes da lista de armazenamento estão disponíveis para clientes em cada uma das versões do sistema operacional com suporte. As etapas que ainda não foram concluídas mostrarão **Pendente** . Seu aplicativo permanecerá na fase de publicação até que o processo seja concluído, o que significa que os novos pacotes e/ou detalhes de listagem estão disponíveis para todos os clientes potenciais do seu aplicativo.

## <a name="in-the-store"></a>Na Loja 

Depois de passar pelas etapas acima com êxito, o status do envio será alterado de **Publicação** para **Na Loja**. Seu envio será disponibilizado na Microsoft Store para que os clientes baixem (a menos que você tenha escolhido outra opção de [Detectabilidade](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nós também fazemos verificações específicas nos aplicativos após eles serem publicados, para que possamos identificar problemas potenciais e garantir que seu aplicativo esteja em conformidade com todas as [Políticas da Microsoft Store](store-policies.md). Se encontrarmos quaisquer problemas, você será notificado sobre o erro e como corrigi-lo, se aplicável, ou se ele foi removido da loja.

 

 

 




