---
title: Obter exemplos de aplicativo UWP
description: Saiba como baixar exemplos de código UWP no GitHub.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, código de exemplo, exemplos de código
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.localizationpriority: medium
ms.openlocfilehash: ac3c99bc364e81386a362f1d1b5530bee9d462c4
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "74259519"
---
# <a name="get-uwp-app-samples"></a>Obter exemplos de aplicativo UWP

Os exemplos de aplicativo UWP (Plataforma Universal do Windows) são disponibilizados nos repositórios no GitHub. Confira [Amostras](https://developer.microsoft.com/windows/samples) para obter uma lista pesquisável e categorizada ou navegue pelo repositório [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Repositório GitHub de amostras de aplicativos da Plataforma Universal do Windows"). O repositório Windows-universal-samples contém amostras que demonstram todos os recursos UWP e seus padrões de uso de API.

![Repositório de exemplos UWP do GitHub](images/GitHubUWPSamplesPage.png)

## <a name="download-the-code"></a>Baixe o código

Para baixar as amostras, acesse o [repositório](https://github.com/Microsoft/Windows-universal-samples "Repositório GitHub de amostras de aplicativos da Plataforma Universal do Windows"). Selecione **Clonar ou baixar** e, em seguida, selecione **Baixar ZIP**. 

![Baixar exemplos](images/SamplesDownloadButton.png)

Baixe também [as amostras](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "Download do arquivo zip de amostras de aplicativos da Plataforma Universal do Windows") neste artigo.

O arquivo .zip de download de exemplos sempre conta com os exemplos mais recentes. Você não precisa de uma conta do GitHub para baixar o arquivo. Quando uma atualização do SDK for liberada ou se você quiser escolher alterações e adições recentes, basta baixar o arquivo zip mais recente.

> [!NOTE]
> Os exemplos UWP precisam do Visual Studio 2015 ou posterior e do SDK do Windows para serem abertos, criados e executados. Obtenha uma [cópia gratuita do Visual Studio Community](https://www.microsoft.com/?ref=go). O Visual Studio Community é compatível com a criação de aplicativos UWP.  
>
> Para que os exemplos funcionem corretamente, não se esqueça de descompactar todo o arquivo, e não apenas exemplos individuais. Todos os exemplos dependem da pasta SharedContent no arquivo morto. Os exemplos de recursos UWP usam arquivos vinculados no Visual Studio para reduzir a duplicação de arquivos comuns, inclusive arquivos de modelo de exemplo e ativos de imagem. Os arquivos comuns são armazenados na pasta SharedContent na raiz do repositório. Os links são usados nos arquivos de projeto para fazer referência a arquivos comuns.
> 

## <a name="open-the-samples"></a>Abrir os exemplos

Depois de baixar o arquivo .zip, abra os exemplos no Visual Studio.

1.  Antes de descompactar o arquivo, clique com o botão direito do mouse e selecione **Propriedades** > **Desbloquear** > **Aplicar**. Em seguida, descompacte o arquivo em uma pasta local no computador.

    ![Arquivo morto descompactado](images/SamplesUnzip1.png)
2.  Cada pasta da pasta Exemplos contém um exemplo de recurso UWP.

    ![Pastas de exemplos](images/SamplesUnzip2.png)
3.  Selecione um exemplo, como Altimeter. As subpastas indicam os idiomas compatíveis.

    ![Pastas de idiomas](images/SamplesUnzip3.png)
4.  Selecione a pasta do idioma desejado. No conteúdo da pasta, você verá um arquivo de solução (.sln) que pode ser aberto no Visual Studio. Por exemplo, *Altimeter.sln*.

    ![Solução do VS](images/SamplesUnzip4.png)

## <a name="give-feedback-ask-questions-and-report-issues"></a>Fazer comentários, perguntas e relatar problemas

Se você tiver problemas ou perguntas, use a guia **Problemas** no repositório para criar um problema. Faremos o possível para ajudar.

![Imagem de comentários](images/GitHubUWPSamplesFeedback.png)
