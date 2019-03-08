---
Description: Quando você concluir a criação de envio do seu aplicativo e clique em Enviar para a Store, o envio insere a etapa de certificação.
title: O processo de certificação de aplicativos
ms.assetid: 0DCB4344-224D-4E5A-899F-FF7A89F23DBC
ms.date: 10/31/2018
ms.topic: article
keywords: o Windows 10, uwp, publicar, pré-processamento, certificação, liberar, pendente, enviar, publicar, status, tempo
ms.localizationpriority: medium
ms.openlocfilehash: 733d5ff882d7ed7c574f6fe6fedd28b79c3913d9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57597071"
---
# <a name="the-app-certification-process"></a>O processo de certificação de aplicativos

Quando você terminar de criar o envio do seu aplicativo, clique em **Enviar à Store**, o envio entrará na etapa de certificação. Esse processo geralmente é concluído dentro de algumas horas, mas em alguns casos pode demorar até três dias úteis. Depois que o envio for aprovado na certificação, pode levar até 24 horas para os clientes vejam a listagem do aplicativo para um envio de novo, ou para um envio atualizado com alterações aos pacotes. Se sua atualização altera somente Store detalhes da listagem, o processo de publicação será concluído em menos de uma hora.  Você será notificado quando seu envio é publicado, e o status do aplicativo no painel de controle serão **the Store em**.

## <a name="preprocessing"></a>Pré-processamento

Depois que você carrega os pacotes do aplicativo com êxito e envia o aplicativo para certificação, os pacotes são colocados em fila para testes automatizados. Exibiremos uma mensagem se detectamos eventuais erros durante o pré-processamento. Para obter mais informações sobre possíveis erros, consulte [Resolver erros de envio](resolve-submission-errors.md).

## <a name="certification"></a>Certificação

Durante essa fase, vários testes são realizados:

-   **Testes de segurança:** Esse primeiro teste verifica pacotes do seu aplicativo de vírus e malwares. Se o aplicativo falhar no teste, você precisará conferir o sistema de desenvolvimento executando o software antivírus mais recente e, em seguida, recompilar o pacote do aplicativo em um sistema limpo.
-   **Testes de conformidade técnica:** Conformidade técnica é testada pelo Kit de certificação de aplicativos do Windows. (Você deve sempre garantir o [teste do aplicativo com o Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) antes de enviá-lo à loja).
-   **Conformidade do conteúdo:** A quantidade de tempo que necessário para isso varia dependendo da complexidade de seu aplicativo está, quanto conteúdo visual, ele tem e quantos aplicativos tenham sido enviados recentemente. Não se esqueça de fornecer todas as informações importantes aos testadores na página [Notas para certificação](notes-for-certification.md).

Após a conclusão do processo de certificação, você receberá um relatório de certificação informando se o aplicativo passou ou não na certificação. Se ele não for aprovado, o relatório indicará em qual teste ele falhou ou qual [política](https://docs.microsoft.com/legal/windows/agreements/store-policies) não foi atendida. Depois de corrigir o problema, você pode criar um novo envio para o seu aplicativo para iniciar o processo de certificação novamente.

## <a name="release"></a>Versão

Quando seu aplicativo for aprovado na certificação, ele estará pronto para mover para o **publicação** processo.

- Se você indicou que o seu envio deve ser publicado mais breve possível (a opção padrão), o processo de publicação será iniciada imediatamente.
- Se esta for a primeira vez que você publicou o aplicativo, e você especificou uma **data de lançamento** na [agenda](configure-precise-release-scheduling.md#release) seção, o aplicativo será disponibilizado acordo com seu **dadatadelançamento**seleções.
- Se você já usou [mantenha a opções de publicação](manage-submission-options.md#publishing-hold-options) para especificar que ele não deve ser liberado até uma determinada data, vamos aguardar até essa data para iniciar o processo de publicação, a menos que você selecione **alterar a data de lançamento**.
- Se você já usou [mantenha a opções de publicação](manage-submission-options.md#publishing-hold-options) para especificar que você deseja publicar o envio manualmente, não iniciaremos o processo de publicação até que você selecione **publicar agora** (ou selecione **alteração Data de lançamento** e selecione uma data específica).


## <a name="publishing"></a>Publicação

Os pacotes do aplicativo são assinados digitalmente para ficarem protegidos contra falsificação após serem lançados. Após o início dessa fase, você não pode mais cancelar o envio nem alterar a data de lançamento do aplicativo.

Para novos aplicativos e atualizações que incluem alterações aos pacotes do aplicativo, o processo de publicação será concluído dentro de 24 horas. Para obter atualizações que apenas alterar as opções como Store detalhes da listagem, mas não alteram os pacotes do aplicativo, o processo de publicação levará menos de uma hora.

Enquanto o aplicativo está na fase de publicação, o **Mostrar detalhes** link na coluna Status de envio do seu aplicativo permite que você saiba quando seus novos pacotes e Store listando os detalhes estão disponíveis para clientes em cada um dos seu sistema operacional com suporte versões. As etapas que ainda não foram concluídas mostrarão **Pendente** . Seu aplicativo permanecerá na fase de publicação até que o processo for concluído, o que significa que os novos pacotes e/ou listando os detalhes está disponível para todos os clientes em potencial do seu aplicativo.

## <a name="in-the-store"></a>Na Loja 

Depois de passar pelas etapas acima com êxito, o status do envio será alterado de **Publicação** para **Na Loja**. Seu envio será disponibilizado na Microsoft Store para que os clientes baixem (a menos que você tenha escolhido outra opção de [Detectabilidade](choose-visibility-options.md#discoverability)). 

> [!NOTE]
> Nós também fazemos verificações específicas nos aplicativos após eles serem publicados, para que possamos identificar problemas potenciais e garantir que seu aplicativo esteja em conformidade com todas as [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies). Se encontrarmos quaisquer problemas, você será notificado sobre o erro e como corrigi-lo, se aplicável, ou se ele foi removido da loja.

 

 

 




