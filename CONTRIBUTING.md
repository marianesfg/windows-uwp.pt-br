---
ms.openlocfilehash: a91c080805bca5d536aad3755ca7edf052d1fe0e
ms.sourcegitcommit: b8087f8b6cf8367f8adb7d6db4581d9aa47b4861
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/27/2019
ms.locfileid: "67414054"
---
# <a name="contributing-to-uwp-conceptual-documentation"></a>Contribuir para a documentação conceitual da UWP

Obrigado por seu interesse na documentação da UWP (Plataforma Universal do Windows)! Agradecemos seus comentários, edições e adições aos nossos documentos.

## <a name="writing-content"></a>Escrever conteúdo

Nossa documentação é escrita em Markdown, uma sintaxe de estilo de texto leve. Se você não está familiarizado com Markdown, [aprenda as noções básicas no GitHub](https://guides.github.com/features/mastering-markdown/). Caso esteja incerto, copie o estilo de formatação de outras páginas em nossos documentos.

## <a name="public-contributions"></a>Contribuições públicas

Se você **não** é um funcionário da Microsoft, pode contribuir por meio do [repositório de conteúdo público](https://github.com/MicrosoftDocs/windows-uwp). As contribuições públicas são apropriadas para alterações e esclarecimentos em páginas existentes.

### <a name="editing-a-file"></a>Editar um arquivo

Se você já está no repositório de conteúdo público, comece indo até o arquivo que deseja alterar. A partir daí, selecione o ícone de lápis acima do conteúdo exibido para começar a editar.

Como alternativa, se você estiver exibindo uma página em docs.microsoft.com, é possível selecionar o botão **Editar** na parte superior direita da página. Isso o redirecionará para o arquivo de origem associado no repositório.

Ao começar a edição, o GitHub bifurca automaticamente o repositório oficial em sua conta pessoal do GitHub, onde é possível fazer suas alterações. Ao terminar, envie uma solicitação de pull para a ramificação **docs**.

### <a name="pull-requests"></a>Solicitações pull

Após enviar sua solicitação de pull, ela é avaliada em relação a uma lista de verificação de qualidade do conteúdo para garantir que atende aos nossos padrões básicos. Se for aprovada, é atribuída a um membro da equipe de documentação da UWP para análise adicional. Se falhar, você será informado de quais alterações deve fazer.

O(s) revisor(es) atribuído(s) podem aprovar ou rejeitar a solicitação de pull ou trabalhar com você para fazer mais alterações.

## <a name="internal-contributions"></a>Contribuições internas

Se você é um funcionário da Microsoft, pode contribuir por meio do [repositório de conteúdo privado](https://github.com/microsoftdocs/windows-uwp-pr). É possível encontrar diretrizes sobre como usar este repositório no [Guia de criação do Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/?branch=master). A documentação dos recursos futuros deve ser enviada somente por meio do repositório privado.

### <a name="editing-a-file"></a>Editar um arquivo

Como no repositório público, é possível fazer pequenas alterações no repositório privado em seu navegador, sem a necessidade de criar um clone local. Você **deve** certificar-se de que está contribuindo na ramificação correta. Para saber mais sobre como criar sua ramificação pessoal, confira [as instruções no Guia de criação para Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/branches?branch=master).

### <a name="making-substantial-changes"></a>Fazer alterações significativas

Para fazer alterações mais extensas em um artigo existente, adicionar ou alterar as imagens, ou contribuir com um novo artigo, crie um clone local do repositório de conteúdo privado. Para saber mais, siga [as instruções no Guia de criação para Windows](https://review.docs.microsoft.com/windows-authoring-guide/uwp/conceptual/).

### <a name="pull-requests"></a>Solicitações pull

Ao criar uma solicitação de pull no repositório interno, certifique-se de mesclar sua ramificação pessoal à ramificação na qual o repositório foi criado.

Após você enviar sua solicitação de pull, ela será avaliada com uma [Mesclagem de PR](https://review.docs.microsoft.com/help/contribute/prmerger-overview?branch=master) para garantir que atenda a nossos padrões básicos. Se ela for aprovada, você poderá comentar `#sign-off` para passá-la para um membro da equipe de documentação da UWP para análise adicional. Se falhar, você será informado de quais alterações deve fazer para aprovar.

O(s) revisor(es) atribuído(s) podem aprovar ou rejeitar a solicitação de pull ou trabalhar com você para fazer mais alterações. Os revisores não mesclarão a solicitação de pull até que você a aprove.

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Usar problemas para fornecer comentários sobre a documentação conceitual da UWP

Se quiser fornecer comentários sobre os documentos ao invés de fazer edições por conta própria, é possível [criar um problema no repositório público](https://github.com/MicrosoftDocs/windows-uwp/issues). Selecione a guia **Problemas** e selecione o botão **Novo problema**. Não deixe de incluir o título do tópico e a URL da página. O problema será atribuído aos membros da equipe de documentação da UWP para revisão.

* Para problemas internos, use a [Ferramenta de Solicitação de Conteúdo WDG](https://aka.ms/pubrequest).
