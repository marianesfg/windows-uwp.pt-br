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
keywords: Windows 10, uwp, publicação, pré-processamento, certificação, liberar, pendentes, enviar, publicar, status, tempo
ms.localizationpriority: medium
ms.openlocfilehash: 8372f316786d83d72dff8ef7a0a8fd53e5390743
ms.sourcegitcommit: 9a17266f208ec415fc718e5254d5b4c08835150c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/28/2018
ms.locfileid: "2882734"
---
# <a name="the-app-certification-process"></a>O processo de certificação de aplicativos

Quando você terminar de criar o envio do seu aplicativo, clique em **Enviar à Store**, o envio entrará na etapa de certificação. Esse processo geralmente é concluído dentro de algumas horas, mas em alguns casos pode demorar até três dias úteis. Depois que o envio passa certificação, pode demorar até 24 horas para que os clientes vejam a listagem do aplicativo para o envio de um novo, ou para um envio atualizado com alterações aos pacotes. Se a sua atualização só altera listando os detalhes do repositório, o processo de publicação será concluído em menos de uma hora.  Você será notificado quando o envio é publicado e status do aplicativo no painel será **In the Store**.

## <a name="preprocessing"></a>Pré-processamento

Depois que você carrega os pacotes do aplicativo com êxito e envia o aplicativo para certificação, os pacotes são colocados em fila para testes automatizados. Exibiremos uma mensagem se detectamos eventuais erros durante o pré-processamento. Para obter mais informações sobre possíveis erros, consulte [Resolver erros de envio](resolve-submission-errors.md).

## <a name="certification"></a>Certificação

Durante essa fase, vários testes são realizados:

-   **Testes de segurança:** o primeiro teste verifica se há vírus e malware nos pacotes do aplicativo. Se o aplicativo falhar no teste, você precisará conferir o sistema de desenvolvimento executando o software antivírus mais recente e, em seguida, recompilar o pacote do aplicativo em um sistema limpo.
-   **Testes de conformidade técnica:** a conformidade técnica é testada pelo Kit de Certificação de Aplicativos Windows. (Você deve sempre garantir o [teste do aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviá-lo à loja).
-   **Conformidade de conteúdo:** o tempo necessário varia dependendo da complexidade do aplicativo, da quantidade de conteúdo visual e de quantos aplicativos foram enviados recentemente. Não se esqueça de fornecer todas as informações importantes aos testadores na página [Notas para certificação](notes-for-certification.md).

Após a conclusão do processo de certificação, você receberá um relatório de certificação informando se o aplicativo passou ou não na certificação. Se ele não for aprovado, o relatório indicará em qual teste ele falhou ou qual [política](https://docs.microsoft.com/legal/windows/agreements/store-policies) não foi atendida. Depois de corrigir o problema, você pode criar um novo envio para o seu aplicativo para iniciar o processo de certificação novamente.

## <a name="release"></a>Versão

Quando seu aplicativo passa certificação, está pronto para migrar para o processo de **publicação** .

- Se você tiver indicado que seu envio deve ser publicado o mais breve possível (a opção padrão), o processo de publicação será iniciado imediatamente.
- Se essa for a primeira vez ter publicado o aplicativo e você especificou uma **Data de lançamento** na seção [agenda](configure-precise-release-scheduling.md#release) , o aplicativo serão disponibilizados de acordo com suas seleções de **Data do lançamento** .
- Se você tiver usado a [publicação mantenha opções](manage-submission-options.md#publishing-hold-options) para especificar que ele não deve ser liberado até uma determinada data, vamos aguardar até essa data para começar o processo de publicação, a menos que você selecione a **Data de lançamento de alteração**.
- Se você tiver usado a [publicação mantenha opções](manage-submission-options.md#publishing-hold-options) para especificar que você deseja publicar o envio manualmente, não iniciamos o processo de publicação até que você selecione **Publicar agora** (ou selecione a **Data de lançamento de alteração** e selecione uma data específica).


## <a name="publishing"></a>Publicação

Os pacotes do aplicativo são assinados digitalmente para ficarem protegidos contra falsificação após serem lançados. Após o início dessa fase, você não pode mais cancelar o envio nem alterar a data de lançamento do aplicativo.

Para novos aplicativos e atualizações que incluem as alterações dos pacotes do aplicativo, o processo de publicação será concluído dentro de 24 horas. Para obter atualizações que apenas alteradas opções como repositório listando detalhes, mas não os pacotes do aplicativo, o processo de publicação levará menos de uma hora.

Enquanto seu aplicativo estiver na fase de publicação, o link **Mostrar detalhes** na coluna Status para envio do seu aplicativo permite que você sabe quando seus novos pacotes e o repositório listando detalhes estão disponíveis para os clientes em cada um dos seus versões suportadas do sistema operacional. As etapas que ainda não foram concluídas mostrarão **Pendente** . Seu aplicativo permanecerá na fase de publicação até que o processo for concluído, o que significa que os novos pacotes e/ou listando detalhes está disponível para todos os clientes potenciais do seu aplicativo.

## <a name="in-the-store"></a>Na Store 

Depois de passar pelas etapas acima com êxito, o status do envio será alterado de **Publicação** para **Na Store**. Seu envio será disponibilizado na Microsoft Store para que os clientes baixem (a menos que você tenha escolhido outra opção de [Detectabilidade](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nós também fazemos verificações específicas nos aplicativos após eles serem publicados, para que possamos identificar problemas potenciais e garantir que seu aplicativo esteja em conformidade com todas as [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Se encontrarmos quaisquer problemas, você será notificado sobre o erro e como corrigi-lo, se aplicável, ou se ele foi removido da loja.

 

 

 




