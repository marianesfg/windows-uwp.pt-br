---
author: jnHs
Description: When you finish creating your app's submission and click Submit to the Store, the submission enters the certification step.
title: O processo de certificação de aplicativos
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.author: wdg-dev-content
ms.date: 07/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, publicação, pré-processamento, certificação, lançamento, pendentes, enviar, publicar, status, tempo
ms.localizationpriority: medium
ms.openlocfilehash: 8372f316786d83d72dff8ef7a0a8fd53e5390743
ms.sourcegitcommit: 7aa1933e6970f878faf50d59e1f799b90afd7cc7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/05/2018
ms.locfileid: "3374549"
---
# <a name="the-app-certification-process"></a>O processo de certificação de aplicativos

Quando você terminar de criar o envio do seu aplicativo, clique em **Enviar à Store**, o envio entrará na etapa de certificação. Esse processo geralmente é concluído dentro de algumas horas, mas em alguns casos pode demorar até três dias úteis. Depois que seu envio for aprovado na certificação, pode levar até 24 horas para que os clientes vejam os detalhes do aplicativo para um novo envio, ou para um envio atualizado com as alterações para pacotes. Se a atualização é alterado apenas detalhes de listagem da loja, será concluído o processo de publicação em menos de uma hora.  Você será notificado quando o envio for publicado, e o status do aplicativo no painel será **Na loja**.

## <a name="preprocessing"></a>Pré-processamento

Depois que você carrega os pacotes do aplicativo com êxito e envia o aplicativo para certificação, os pacotes são colocados em fila para testes automatizados. Exibiremos uma mensagem se detectamos eventuais erros durante o pré-processamento. Para obter mais informações sobre possíveis erros, consulte [Resolver erros de envio](resolve-submission-errors.md).

## <a name="certification"></a>Certificação

Durante essa fase, vários testes são realizados:

-   **Testes de segurança:** o primeiro teste verifica se há vírus e malware nos pacotes do aplicativo. Se o aplicativo falhar no teste, você precisará conferir o sistema de desenvolvimento executando o software antivírus mais recente e, em seguida, recompilar o pacote do aplicativo em um sistema limpo.
-   **Testes de conformidade técnica:** a conformidade técnica é testada pelo Kit de Certificação de Aplicativos Windows. (Você deve sempre garantir o [teste do aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviá-lo à loja).
-   **Conformidade de conteúdo:** o tempo necessário varia dependendo da complexidade do aplicativo, da quantidade de conteúdo visual e de quantos aplicativos foram enviados recentemente. Não se esqueça de fornecer todas as informações importantes aos testadores na página [Notas para certificação](notes-for-certification.md).

Após a conclusão do processo de certificação, você receberá um relatório de certificação informando se o aplicativo passou ou não na certificação. Se ele não for aprovado, o relatório indicará em qual teste ele falhou ou qual [política](https://docs.microsoft.com/legal/windows/agreements/store-policies) não foi atendida. Depois de corrigir o problema, você pode criar um novo envio para o seu aplicativo para iniciar o processo de certificação novamente.

## <a name="release"></a>Versão

Quando seu aplicativo for aprovado na certificação, ele estará pronto para mover para o processo de **publicação** .

- Se você indicou que seu envio deve ser publicado assim que possível (a opção padrão), o processo de publicação começará imediatamente.
- Se esta for a primeira vez que você publicou o aplicativo e você especificou uma **Data de lançamento** na seção [Agendar](configure-precise-release-scheduling.md#release) , o aplicativo ficará disponível de acordo com suas seleções de **Data de lançamento** .
- Se você já usou [Opções de suspensão de publicação](manage-submission-options.md#publishing-hold-options) para especificar que ele não deve ser lançado até uma data específica, vamos aguardar até essa data para começar o processo de publicação, a menos que você selecione **Alterar data do lançamento**.
- Se você já usou [Opções de suspensão de publicação](manage-submission-options.md#publishing-hold-options) para especificar que você deseja publicar o envio manualmente, nós não iniciar o processo de publicação até que você selecionar **Publicar agora** (ou selecione a **Data de lançamento de alteração** e selecionar uma data específica).


## <a name="publishing"></a>Publicação

Os pacotes do aplicativo são assinados digitalmente para ficarem protegidos contra falsificação após serem lançados. Após o início dessa fase, você não pode mais cancelar o envio nem alterar a data de lançamento do aplicativo.

Para novos aplicativos e atualizações que incluem as alterações dos pacotes do aplicativo, o processo de publicação será concluído dentro de 24 horas. Para atualizações somente alterar as opções, como detalhes de listagem da loja, mas não alteram os pacotes do aplicativo, o processo de publicação levará menos de uma hora.

Enquanto seu aplicativo está na fase de publicação, o link **Mostrar detalhes** na coluna Status de envio do seu aplicativo permite que você saiba quando seus novos pacotes e detalhes de listagem da loja estão disponíveis para clientes em cada uma das suas versões do sistema operacional com suporte. As etapas que ainda não foram concluídas mostrarão **Pendente** . Seu aplicativo permanecerá na fase de publicação até a conclusão do processo, isso significa que os novos pacotes e/ou detalhes estão disponíveis para todos os clientes em potencial do seu aplicativo.

## <a name="in-the-store"></a>Na Store 

Depois de passar pelas etapas acima com êxito, o status do envio será alterado de **Publicação** para **Na Store**. Seu envio será disponibilizado na Microsoft Store para que os clientes baixem (a menos que você tenha escolhido outra opção de [Detectabilidade](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nós também fazemos verificações específicas nos aplicativos após eles serem publicados, para que possamos identificar problemas potenciais e garantir que seu aplicativo esteja em conformidade com todas as [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Se encontrarmos quaisquer problemas, você será notificado sobre o erro e como corrigi-lo, se aplicável, ou se ele foi removido da loja.

 

 

 




