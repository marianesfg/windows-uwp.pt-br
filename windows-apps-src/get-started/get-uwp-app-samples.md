---
title: Obter exemplos de aplicativo UWP
description: Saiba como baixar exemplos de código UWP no GitHub
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, código de exemplo, exemplos de código
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: 7f56b0f9e4cb7f89b8bc929015ecdf6d5c64d42e
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67321073"
---
# <a name="get-uwp-app-samples"></a>Obter exemplos de aplicativo UWP

As amostras de aplicativo UWP (Plataforma Universal do Windows) são disponibilizadas por meio de repositórios no GitHub. Consulte [Amostras](https://developer.microsoft.com/windows/samples%20%22Dev%20Center%20samples%22) para ver uma lista pesquisável e categorizada ou navegue por [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "repositório de amostras de aplicativo da Plataforma Universal do Windows do GitHub"), que contém amostras que demonstram todos os recursos UWP e seus padrões de uso de API.  
![Repositório de amostras UWP do GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Baixe o código

Para baixar as amostras, vá para o [repositório](https://github.com/Microsoft/Windows-universal-samples "repositório de amostras de aplicativo da Plataforma Universal do Windows do GitHub") e selecione **Clone ou baixar**, em seguida, **Baixar ZIP**. Ou basta clicar [aqui](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Download do arquivo zip de amostras de aplicativo da Plataforma Universal do Windows").

O arquivo zip sempre terá as amostras mais recentes. Você não precisa de uma conta do GitHub para baixá-lo. Quando uma atualização do SDK for liberada ou se você quiser escolher alterações/adições recentes, basta verificar o arquivo zip mais recente.

![Download de exemplo](images/SamplesDownloadButton.png)


> [!NOTE]
> As amostras UWP precisam do Visual Studio 2015 ou posterior e do SDK do Windows para serem abertas, criadas e executadas. Você pode obter uma cópia gratuita do Visual Studio Community com suporte para a criação de aplicativos UWP [aqui](https://go.microsoft.com/fwlink/p/?LinkID=280676 "Downloads de ferramentas de desenvolvimento do Windows").  
>
> Além disso, não se esqueça de descompactar todo o arquivo morto, e não apenas exemplos individuais. Todos os exemplos dependem da pasta SharedContent no arquivo morto. Os exemplos de recursos UWP usam arquivos vinculados no Visual Studio para reduzir a duplicação de arquivos comuns, inclusive arquivos de modelo de exemplo e ativos de imagem. Esses arquivos comuns são armazenados na pasta SharedContent na raiz do repositório e são referenciados nos arquivos de projeto usando links.

Depois de baixar o arquivo zip, abra os exemplos no Visual Studio:

1.  Antes de descompactar o arquivo morto, clique com o botão direito do mouse, selecione **Propriedades** > **Desbloquear** > **Aplicar**. Em seguida, descompacte o arquivo morto para uma pasta local no computador.

    ![Arquivo morto descompactado](images/SamplesUnzip1.png)
2.  Dentro da pasta de exemplos, você verá várias pastas, cada uma contendo um exemplo de recursos UWP.

    ![Pastas de exemplos](images/SamplesUnzip2.png)

3.  Selecione um exemplo, como Altimeter, e você verá várias pastas indicando os idiomas compatíveis.

    ![Pastas de idiomas](images/SamplesUnzip3.png)

4.  Selecione o idioma que você gostaria de usar, como CS para C\#, e você verá um arquivo de solução do Visual Studio, que é possível abrir no Visual Studio.

    ![Solução do VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Fazer comentários, perguntas e relatar problemas

Se você tiver problemas ou perguntas, basta usar a guia Problemas no repositório para criar um novo problema, e faremos o possível para ajudar.

![Imagem de comentários](images/GitHubUWPSamplesFeedback.png)
