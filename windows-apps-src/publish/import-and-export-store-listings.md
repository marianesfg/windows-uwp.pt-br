---
author: jnHs
Description: "Você também pode criar listagens da Loja para seus aplicativos sem utilizar o painel do Centro de Desenvolvimento ao exportar suas listagens em um arquivo .csv, inserindo as informações e os ativos, e importando o arquivo atualizado."
title: Importar e exportar as listagens da Loja
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 691074727bab67e616541b393468eb70f0b20a05
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2017
---
# <a name="import-and-export-store-listings"></a>Importar e exportar as listagens da Loja

Você pode criar [listagens da Loja](create-app-store-listings.md) para seus aplicativos sem utilizar o painel do Centro de Desenvolvimento ao exportar suas listagens em um arquivo .csv, inserindo as informações e os ativos, e importando o arquivo atualizado. Você pode usar esse método para criar listagens novas ou atualizar listagens já criadas. 

Observe que você não pode usar esse método para criar ou atualizar [listagens da Loja de específicas de plataforma](create-platform-specific-store-listings.md) para o aplicativo.

> [!TIP]
> Você também pode usar esse recurso para importar e exportar os detalhes da listagem da Loja de um complemento. Para complementos, o processo funciona da mesma maneira exceto que [somente os campos relevantes para complementos](#add-ons) são incluídos.

## <a name="export-listings"></a>Exportar listagens

Na página de visão geral do envio de um aplicativo, clique em **Exportar listagem** (na seção **Listagens da loja**) para gerar um arquivo .csv codificado em UTF-8. Salve esse arquivo em um local no seu computador.

Você pode usar o Microsoft Excel ou outro editor para editar o arquivo. Observe que as versões do Excel para Office 365 permitem que você salve um arquivo .csv como **UTF-8 CSV (delimitado por vírgula) (*.csv)**, mas outras versões podem não oferecer suporte a isso. Você pode encontrar detalhes sobre quais versões do Excel oferecem suporte a esse recurso no [Boletim de novos recursos do Excel 2016](https://support.office.com/en-us/article/What-s-new-in-Excel-2016-for-Windows-5fdb9208-ff33-45b6-9e08-1f5cdb3a6c73) e saber mais sobre como codificar UTF-8 em vários editores de codificação [aqui](https://help.surveygizmo.com/help/encode-an-excel-file-to-utf-8-or-utf-16).
      
Se você ainda não criou todas as listagem para seu produto, o arquivo .CSV exportado não conterá todos os dados personalizados. Você verá colunas para **Campo**, **ID**, **Tipo** e **Padrão**, além de linhas que correspondem a todos os itens que podem ser exibidos em um repositório de listagem.

Se você já criou listagens (ou se já tiver carregado pacotes), também é possível ver colunas identificadas com códigos de idioma-localidade que correspondem ao idioma para cada detalhe que você criou (ou que detectamos nos seus pacotes), bem como quaisquer informações de detalhes que você forneceu anteriormente.
     
Veja uma visão geral do que está contido em cada uma das colunas no arquivo .CSV exportado:
- A coluna **Campo** contém um nome associado a cada parte de uma listagem da Loja. Elas correspondem aos mesmos itens que você pode fornecer ao criar listagens da Loja no painel, embora alguns dos nomes sejam um pouco diferentes. Para itens que você pode inserir mais de um do mesmo tipo, é possível ver diversas linhas, até o número máximo que você pode fornecer. Por exemplo, para **Recursos do aplicativo**, você verá **Recurso1**, **Recurso2** etc. até **Recurso20** (desde que você possa fornecer até 20 recursos do aplicativo).
- A coluna **ID** contém um número que o Centro de Desenvolvimento associa a cada campo. 
- A coluna **Tipo** fornece diretrizes gerais sobre qual tipo de informações devem ser fornecidas para o campo, como **Texto** ou **Caminho relativo (ou URL para o arquivo no Centro de Desenvolvimento)**. 
- A coluna **Padrão** (e qualquer outra coluna identificada com códigos de idioma-localidade) representam o texto ou ativos associados a cada parte da listagem da loja. Você pode editar os campos nessas colunas para fazer atualizações em suas listagens da Loja.

>[!IMPORTANT]
> Não altere qualquer uma das informações nas colunas **Campo**, **ID** ou **Tipo**. As informações nessas colunas devem ser alteradas para que o arquivo importado seja processado.

## <a name="update-listing-info"></a>Atualizar informações de listagem

Depois de exportar as listagens e salvar o arquivo .csv, você pode editar as informações de listagem diretamente no arquivo .csv. 

Juntamente com a coluna **padrão**, cada idioma para o qual você criou uma listagem tem sua própria coluna. As alterações feitas em uma coluna serão aplicadas à sua descrição nesse idioma. Você pode criar listagens de novos idiomas ao adicionar o código de idioma-local na próxima coluna vazia na linha superior. Para obter uma lista de códigos de idioma-local válidos, consulte [Idiomas suportados](supported-languages.md).

Você pode usar a coluna **padrão** para inserir informações que você deseja compartilhar em todas as descrições do aplicativo. Se o campo de determinado idioma for deixado em branco, as informações da coluna padrão serão usadas para esse idioma. Você pode substituir esse campo para um idioma específico ao inserir informações diferentes para esse idioma.

A maioria dos campos de listagem da Loja é opcional. A **Descrição** e uma captura de tela são necessárias para cada detalhe; para idiomas sem pacotes associados, você também deverá fornecer um **Título** para indicar quais dos seus nomes de aplicativo reservado devem ser usados nessa lista. Para todos os outros campos, você pode deixar o campo vazio se você não quiser incluí-lo na listagem. Lembre-se de que se você deixar um campo para um idioma específico, analisaremos para ver se há informações nesse campo na coluna padrão. Em caso afirmativo, essas informações serão usadas. 

Por exemplo, imagine o seguinte exemplo: 

![Exemplo de listagem exportado](images/listingimport.png)
     
- O texto "Descrição padrão" será usado para o campo **Descrição** nas listagens em en-us e fr-fr. No entanto, o campo **Descrição** na listagem de es-es usaria o texto "Descrição em espanhol". 
- Para o campo **Notas de versão**, o texto "Notas de versão em inglês" serão usadas para en-us e o texto "Notas de versão em francês" será ser usado para fr-fr. Entretanto, nenhum notas de versão serão exibida para es-es.

Se você não quiser fazer quaisquer edições em um campo específico, você pode excluir a linha inteira da planilha, **exceto as linhas de trailers, bem como miniaturas e títulos associados**. Diferente para esses itens, excluir uma linha não afetará os dados associados a esse campo em suas listagens. Isso permite remover quaisquer linhas que você não pretende editar, portanto,é possível se concentrar nos campos em que está fazendo alterações.

A exclusão das informações em um campo para um idioma, sem remover a linha inteira, funciona de maneira diferente dependendo do campo. Para campos cujo **Tipo** é **Texto**, a exclusão das informações em um campo apenas remove essa entrada da listagem nesse idioma.  Entretanto, a exclusão das informações em um campo de uma imagem, como uma captura de tela ou um logotipo, não terão nenhum efeito; a imagem anterior ainda será usada, exceto se você pode removê-la editando diretamente no Centro de Desenvolvimento. A exclusão das informações de um campo de trailer realmente remove esse trailer do Centro de Desenvolvimento, portanto, certifique-se de que ter uma cópia de todos os arquivos necessários antes de fazer isso.

Diversos campos em suas listagens exportadas exigem a entrada de texto, como aqueles no exemplo acima, **Descrição** e **Notas de versão**. Para esses tipos de campos, digite o texto apropriado no campo para cada idioma. Certifique-se de seguir o tamanho e outros requisitos de cada campo. Para saber mais sobre esses requisitos, consulte [Criar listagens da Loja do aplicativo](create-app-store-listings.md).

O fornecimento de informações para os campos que correspondem aos ativos, como imagens e trailers, é um pouco mais complicado. Em vez de **Texto**, o **Tipo** desses ativos é o **Caminho relativo (ou URL do arquivo no Centro de Desenvolvimento)**. 
     
Se você já tiver carregado ativos para a listagem da Loja, esses ativos serão representados por uma URL. Essas URLs podem ser reutilizadas em várias descrições de um produto ou mesmo em produtos diferentes na mesma conta de desenvolvedor, portanto, você pode copiar essas URLs para reutilizá-las em um campo diferente, se desejar.

> [!TIP]
> Para confirmar qual ativo corresponde a uma URL, é possível inserir a URL em um navegador para exibir a imagem (ou baixar o vídeo do trailer).  Você deve estar conectado à sua conta do Centro de Desenvolvimento para que a URL funcione.

Se você quiser usar um novo ativo ainda não adicionado ao Centro de Desenvolvimento, é possível fazer isso ao importar as listagens como uma pasta, em vez de um arquivo .csv único. É necessário criar uma pasta com o arquivo .csv. Em seguida, adicione as imagens à mesma pasta, na pasta raiz ou em uma subpasta. Você deverá inserir o caminho completo, incluindo o nome da pasta raiz, no campo.

> [!TIP]
> Para obter os melhores resultados ao importar as listagens como uma pasta, certifique-se de usar a versão mais recente do Microsoft Edge, Chrome ou Firefox.

Por exemplo, se a pasta raiz for **my_folder** e você quiser usar uma imagem chamada **screenshot1.png** para **DesktopScreenshot1**, é possível adicionar screenshot1.png na raiz da pasta e, em seguida, inserir **my_folder/screenshot1.png** no campo **DesktopScreenshot1**. Se você criou uma pasta de imagens na pasta raiz e, em seguida, inseriu screenshot1.jpg lá, é necessário inserir **my_folder/images/screenshot1.png**. Observe que depois de importar as listagens usando uma pasta, os caminhos para as imagens serão convertidos em URLs para os arquivos no Centro de Desenvolvimento na próxima vez que você exportar as listagens. É possível copiar e colar essas URLs para usá-las novamente (por exemplo, para usar os mesmos ativos em vários idiomas de listagem). 

> [!IMPORTANT]
> Se a listagem exportada inclui trailers, lembre-se de que excluir a URL do trailer ou a imagem em miniatura do seu arquivo .csv remove completamente o arquivo excluído do painel, e você não poderá acessá-lo lá (exceto quando também é usado em outra listagem em que ainda não tenha sido excluído). 

## <a name="import-listings"></a>Importar listagens

Depois de inserir todas as alterações no arquivo .csv (e incluir quaisquer ativos que você deseja carregar), é necessário salvar o arquivo antes de carregá-lo. Se você estiver usando uma versão do Microsoft Excel com suporte à codificação UTF-8, certifique-se de selecionar **Salvar como** e usar o formato **UTF-8 CSV (delimitado por vírgula) (*. csv)**. Se você usar um editor diferente para exibir e editar o arquivo .csv, verifique se que o arquivo está codificado em UTF-8 antes de carregar.

Quando você estiver pronto para carregar o arquivo .csv atualizado e importar seus dados de listagem, selecione **Importar listagens** na página de visão geral do envio. Se você estiver importando apenas um arquivo .csv, escolha **Importar. csv**, navegue até o arquivo e clique em **Abrir**. Se você estiver importando uma pasta com arquivos de imagem, escolha Importar pasta, navegue até a pasta e clique em **Selecionar pasta**. Verifique se há apenas um arquivo .csv na pasta, juntamente com quaisquer ativos que estiver carregando. 

Conforme processamos o arquivo .csv importado, você verá uma barra de progresso que reflete o status de importação e validação. Isso pode demorar alguns minutos, especialmente se você tiver muitas listagens e/ou arquivos de imagem. 

Em caso de problemas, você verá uma observação indicando que é necessário fazer as atualizações necessárias e tentar novamente. Selecione o link **Exibir erros** para verificar quais campos são inválidos e o motivo. Você precisa corrigir esses problemas no arquivo .csv (ou substituir qualquer ativo inválido) e, em seguida, importar suas listagens novamente.

> [!TIP]
> É possível acessar essas informações novamente mais tarde por meio do link **Exibir erros da última importação**.

Nenhuma das informações do arquivo .csv serão salvas no Centro de Desenvolvimento até que todos os erros no arquivo sejam resolvidos, até mesmo para campos sem erros. Depois de importar um arquivo .csv sem erros, as informações de listagem fornecidas serão salvas no Centro de Desenvolvimento e usadas para esse envio.

Você pode continuar a fazer atualizações nas listagens importando outro arquivo .csv atualizado ou fazendo alterações diretamente no Centro de Desenvolvimento.

## <a name="add-ons"></a>Complementos

Para complementos, a importação e a exportação de listagens da Loja usam o mesmo processo descrito acima, exceto que você verá apenas os três campos relevantes para [Listagens da Loja de complementos](create-add-on-store-listings.md): **Descrição**, **Título** e **StoreLogo300x300** (conhecido como **Ícone** na página de listagem da Loja no Centro de Desenvolvimento). O campo **Título** é obrigatório e os outros dois campos são opcionais.

Observe que você deve importar e exportar listagens da Loja separadamente para cada complemento em seu aplicativo, navegando até a página de visão geral de envio do complemento.

