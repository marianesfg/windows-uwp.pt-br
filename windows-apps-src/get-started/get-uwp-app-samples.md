---
title: Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub
description: Saiba como baixar as amostras de recurso UWP do GitHub
author: JoshuaPartlow
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 393c5a81-ee14-45e7-acd7-495e5d916909
ms.openlocfilehash: e576150cd06000405fa4754e147fbc1546cdf703
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
#<a name="get-the-universal-windows-platform-uwp-samples-from-github"></a>Obtenha os exemplos da Plataforma Universal do Windows (UWP) do GitHub
Os exemplos de aplicativo UWP são disponibilizados por meio de repositórios no GitHub. Se esta for a primeira vez em que trabalha com UWP, você desejará começar pelo repositório [Microsoft/Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples "Universal Windows Platform app samples GitHub repository"), que contém exemplos que demonstram todos os recursos UWP e os padrões de uso da API.  
![Repositório de exemplos UWP do GitHub](images/GitHubUWPSamplesPage.png) Os exemplos adicionais podem ser encontrados usando-se a seção [Exemplos](https://developer.microsoft.com/windows/samples "Dev Center samples") do Centro de Desenvolvimento.  

##<a name="download-the-code"></a>Baixe o código
Para baixar as amostras, vá para o [repositório](https://github.com/Microsoft/Windows-universal-samples "repositório de GitHub de amostras de aplicativo da Plataforma Universal do Windows") e selecione **Clone ou baixar**, em seguida, **Download ZIP**. Ou, basta clicar [aqui](https://github.com/Microsoft/Windows-universal-samples/archive/master.zip "zip de exemplos de aplicativos da Plataforma Universal do Windows download de arquivo").

O arquivo zip sempre terá as amostras mais recentes. Você não precisa de uma conta do GitHub para baixá-lo. Quando uma atualização do SDK for liberada ou se você quiser escolher alterações/adições recentes, basta verificar o arquivo zip mais recente.

![Download de exemplo](images/SamplesDownloadButton.png)


> **Observação**: Os exemplos UWP precisam do Visual Studio 2015 e do SDK do Windows para serem abertos, compilados e executados. Se ainda não tiver o Visual Studio instalado, você poderá obter uma cópia gratuita do Visual Studio 2015 Community Edition com suporte para compilação de aplicativos UWP [aqui](http://go.microsoft.com/fwlink/p/?LinkID=280676 "Downloads de ferramentas de desenvolvimento do Windows").  
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
