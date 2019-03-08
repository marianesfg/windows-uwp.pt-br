---
title: Página de resumo do Xbox Live
description: Descreve como você pode aproveitar o modo de exibição de resumo Xbox Live
ms.assetid: ''
ms.date: 10/19/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, jogos, uwp, windows 10, Xbox one, xbox live resumo, resumo, publicar, histórico de xbox live, barra de comandos, guia Histórico e tabela de resumo
ms.openlocfilehash: 289b472939c721e5bfb373d4de62ed800840bf57
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605981"
---
# <a name="the-xbox-live-configuration-summary-page"></a>A página de resumo de configuração Xbox Live

Você pode usar [Partner Center](https://developer.microsoft.com/dashboard) para configurar o Xbox Live para seu título. Com o Partner Center, você poderá gerenciar a configuração de serviço para cada uma das áreas de segurança do seu título.
Para configurar os aspectos do seu título Xbox Live, navegue até a seção do Xbox Live em seu título, localizado sob **Services** > **Xbox Live**. Nessa página, você encontrará um instantâneo da configuração atual na caixa de proteção selecionada. Você também encontrará uma análise detalhada do que foi configurado e publicado para a área de segurança.

## <a name="sandbox-selector"></a>Seletor de área restrita

 [As áreas restritas](../../xbox-live-sandboxes.md) agora estão em um item de navegação de nível superior, você pode alternar entre ou expanda por seleção. A interface do usuário exibe áreas de segurança do título na parte superior, como guias. As informações mostradas em cada guia estão no contexto da área de segurança associado.  

![Imagem de alternar as guias da área restrita](../../images/summary/sandbox-tabs1.gif)

 Você pode adicionar áreas de segurança adicionais, selecionando o "+" que apresentará uma caixa de diálogo em que você especifique qual área restrita e área restrita do qual você deseja copiar a configuração de você gostaria de copiar a configuração.  

 ![Imagem de ampliar para uma nova guia área restrita](../../images/summary/sandbox-tabs2.gif)

## <a name="command-bar"></a>Barra de comandos

Conforme mencionado acima a página exibida sempre dentro do contexto de uma área restrita, portanto, é a barra de comandos exposta logo abaixo mostra todas as ações que você pode executar em sua área restrita de determinado. Os comandos disponíveis são:  

* **Exportar** -que fornece um arquivo zip que contém documentos configurados na área restrita.
* **Importação** -permite que você forneça um arquivo zip contendo XBL válido documenta que uma vez carregado estará disponível na área restrita.
* **Certificar** -publica sua configuração atual para a área restrita de certificação.  *Você também pode usar o botão publish e alterar o destino para o certificado para fazer isso.*
* **Histórico de** -abre uma guia que exibe informações sobre quem criou o que e quando. Você pode abrir essa guia em qualquer página e ela será filtrada para os objetos criados nessa página.
* **Publicar** -permite que você escolha sua área restrita do código-fonte e destino. Uma vez selecionada, uma validação será executada, permitindo que você saiba se é possível publicar a configuração. Se permitido, selecionar publicar irá definir sua configuração para a área restrita para que você pode testar essa configuração usando a área de segurança apropriada.  
  
  
![Imagem da barra de comandos](../../images/summary/command-bar.png)  

## <a name="summary-table"></a>Tabela de resumo

A interface do usuário agora fornece um rolo significativo backup de todas as suas configurações diferentes, permitindo uma visualização que já foi configurado, o que é opcional, e que ainda é exigido antes da publicação para o varejo.  

* **Detalhes** – descreve o que foi configurado para um determinado recurso na área restrita atual (Isso inclui os objetos que você tiver criado, mas ainda não foram publicados)
* **Desde a última publicação** – isso permitirá que você sabe quais novas configurações que você criou que não foram publicadas em sua área restrita para testar
* **Status** – informa se esse recurso está pronto para ser publicado para o varejo. Qualquer coisa rotulada como "Não está pronto para o varejo" deve ser abordada, linhas marcado como "Opcional" são a critério do desenvolvedor.

*Anteriormente, depois que você selecionou o botão"testar", executaria validação e, em seguida, apenas saber se você tivesse um problema que você precisava corrigir; Isso simplifica esse processo e cria uma melhor experiência do usuário*  
  
![Imagem da barra de comandos](../../images/summary/summary-table.png)  

## <a name="history-pane"></a>Painel de histórico

O painel de histórico exibe objetos que foram criados na área restrita e indica por quem e quando. Quando estiver na página de resumo, o painel de histórico mostrará todos os objetos criados e publicar as ações feitas na área restrita. No entanto, quando você abrir esse painel em uma página específica, como conquistas, você verá somente o histórico de medalha que lhe permite filtrar facilmente a pesquisa de histórico.  

![Imagem do painel de histórico](../../images/summary/history.png)  

## <a name="best-practices"></a>Práticas recomendadas

* Publique as alterações em sua área restrita depois de fazer algumas edições para garantir que eles estejam ao vivo em suas contas de teste e dispositivos.
* Use a nova exibição da guia, a tabela de resumo e painel de histórico para ajudá-lo a identificar rapidamente o que é publicada onde.
* Em casos extremos em que você precisa fazer comparações XML entre as áreas restritas, você pode usar o recurso de exportação de ambas as áreas restritas para obter os dois documentos e, em seguida, abri-los com uma ferramenta como o Beyond Compare.
* Exportação pode ser usada para obter uma versão local dos arquivos que pode ser confirmada para seu próprio controle do código-fonte. Dessa forma, se você cada perder qualquer configuração, na verdade, não perdê-los. Você pode levar a sua configuração local fora do seu controle de origem e importe-o novamente para a área restrita.