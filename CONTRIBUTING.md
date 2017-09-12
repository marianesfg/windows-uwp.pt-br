# <a name="contributing-to-uwp-conceptual-documentation"></a>Contribuição para a documentação conceitual de UWP

Obrigado por seu interesse na documentação da Plataforma Universal do Windows (UWP)! Agradecemos seus comentários, edições e adições para nossos documentos.

Esta página aborda as etapas básicas de contribuição para a nossa documentação do desenvolvedor.

## <a name="public-and-private-repos"></a>Repositórios públicos e privados

A documentação conceitual de UWP é hospedada em dois repositórios diferentes que, posteriormente, são mesclados e atualizados para um único site: um repositório é para contribuições de todos e o outro é exclusivo para funcionários da Microsoft.

Se você ***não*** for funcionário da Microsoft, trabalhe no [repositório de conteúdo público](https://github.com/MicrosoftDocs/windows-uwp).

Se você ***for*** funcionário da Microsoft, poderá trabalhar no repositório público ou no [repositório de conteúdo privado](https://cpubwin.visualstudio.com/_git/windows-uwp). Os funcionários podem enviar por push as alterações ligeiramente mais rápido por meio da contribuição no repositório privado ou use um [branch específico](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/setup-local-repo-for-large-changes#what-branch-should-i-use-for-my-authoring) para obter as alterações que precisam permanecer sob encapsulamentos até uma data futura.

## <a name="editing-topics-on-the-public-repo"></a>Edição de tópicos sobre o repositório público

Tentamos simplificar ao máximo a edição de um arquivo existente. 
- Se você já estiver no repositório, simplesmente navegue até o arquivo e clique no botão **Editar**.  
- Como alternativa, se você estiver exibindo uma página do Docs.microsoft.com no seu navegador, clique no botão **Editar** no canto superior direito da página. Você será redirecionado para o arquivo de origem correto do Markdown no repositório, onde poderá clicar no botão **Editar**. 

O GitHub bifurca automaticamente o repositório oficial em sua conta pessoal do GitHub, onde você pode fazer suas alterações. Quando terminar, envie uma solicitação de pull volta para o branch "docs". Depois de criar a solicitação de pull, um membro da equipe de Documentação UWP analisará suas alterações. Se sua solicitação for aceita, as atualizações serão publicadas em https://docs.microsoft.com/windows/.

Você pode aprender o básico sobre o Markdown em apenas alguns minutos.  Para começar, confira [Domine o Markdown](https://guides.github.com/features/mastering-markdown/).

## <a name="making-more-substantial-changes"></a>Como fazer alterações mais substanciais

Para fazer alterações substanciais em um artigo existente, adicionar ou alterar imagens, ou para colaborar em um novo artigo, será necessário criar um novo clone local de nosso repositório privado de conteúdo. Siga as [instruções em nosso Guia de criação do Windows](https://review.docs.microsoft.com/en-us/windows-authoring-guide/uwp/conceptual/). Se você ainda não tiver configurado uma conta do GitHub e ingressado seu alias da Microsoft no domínio, [comece aqui](https://review.docs.microsoft.com/en-us/windows-authoring-guide/github-account).

## <a name="using-issues-to-provide-feedback-on-uwp-conceptual-documentation"></a>Uso de problemas para fornecer comentários sobre a documentação Conceitual da UWP

Se você quiser fornecer comentários, em vez de modificar diretamente as páginas de documentação reais, poderá [criar um problema no repositório público](https://github.com/MicrosoftDocs/windows-uwp/issues). Clique na guia "Problemas" e, em seguida, clique no botão **Novo problema**. Inclua o título do tópico e a URL da página.

Os membros da equipe de documentação da UWP analisam os problemas regularmente e farão uma triagem, os atribuirão e tratarão deles adequadamente.

*Para problemas internos, use a Ferramenta de Solicitação de Conteúdo WDG em [http://aka.ms/pubrequest](http://aka.ms/pubrequest). 
