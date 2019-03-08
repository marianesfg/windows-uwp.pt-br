---
title: Unity - Controle de versão do seu projeto UWP
description: Versão do seu projeto UWP Unity.
ms.localizationpriority: medium
ms.topic: article
ms.date: 02/08/2017
ms.openlocfilehash: 064eaf42fe7d664be273cd7e2222fa5d90be1a11
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57608861"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: Seu projeto UWP de controle de versão

Ainda não criou seu jogo Unity para Xbox usando a Plataforma Universal do Windows (UWP)?  Consulte primeiro [Como trazer jogos Unity para a UWP no Xbox](development-lanes-unity.md).

Há alguns motivos diferentes pelos quais você adicionaria partes do seu diretório UWP gerado ao controle de versão, uma delas é a adição de dependências (por exemplo, SDK do Xbox Live).  Usaremos esse cenário como exemplo desse tutorial, e esperamos que ele ajude você a resolver necessidades individuais do seu projeto.

***Isenção de responsabilidade: Usaremos Git como nossa solução de controle de versão.  Se sua for diferente, os conceitos ainda devem traduzir.***

Para atualizar sua memória, veja como é o diretório de nosso jogo, ***ScrapyardPhoenix***, atualmente:

![Pasta de destino da compilação](images/build-destination.png)

E esta é a aparência de nosso diretório UWP:

![Solução do VS da UWP](images/uwp-vs-solution.png)

Nesse diretório, estamos preocupados somente com uma pasta, a pasta ***ScrapyardPhoenix*** (inserir o nome do seu jogo aqui).  O restante pode ser ignorado no nosso controle de versão.

***Familiarizado com o qual é um arquivo. gitignore?  Ver [gitignore](https://git-scm.com/docs/gitignore).***

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep... (this line will be modified and removed further down)
    !/UWP/ScrapyardPhoenix/

Selecionaremos alguns arquivos e pastas diferentes na pasta **UWP/ScrapyardPhoenix** a serem adicionados a nosso controle de versão.  Primeiro vamos examinar tudo em detalhes:

![Diretório de compilação UWP](images/uwp-build-directory.png)  

## <a name="folders"></a>Pastas  

`Assets` | ***Incluir*** | Contém imagens da Microsoft Store  
`Data`   | ***Ignorar*** | Em que o Unity compila seu projeto (cenas, sombreadores, Scripts, pré-fabricados, etc.)  
`Dependencies` | ***Incluir*** | Essa pasta é um que criei para manter todas as dependências UWP (por exemplo, XboxLiveSDK.dll)  
`Properties` | ***Incluir*** | Contém as configurações mais avançadas que podem ser modificadas pelo desenvolvedor  
`Unprocessed` | ***Ignorar*** | Contém o Unity `.dll` e `.pdb` arquivos  

## <a name="files"></a>Arquivos  

`App.cs` | ***Incluir*** | Ponto de entrada para seu aplicativo UWP. Isso pode ser modificado e estendido com outros arquivos de origem  
`Package.appxmanifest` | ***Incluir*** | Arquivo de manifesto de origem do pacote de aplicativo para seu AppX  
`project.json` | ***Incluir*** | Descreve os pacotes do NuGet seu `*.csproj` depende  
`ScrapyardPhoenix.csproj` | ***Incluir*** | Descreve o destino de build UWP; Se você adicionar dependências adicionais para a UWP do projeto, isso `*.csproj` arquivo conterá essas informações  
`ScrapyardPhoenix.csproj.user` | ***Ignorar*** | Esse arquivo contém informações de usuário local

## <a name="resulting-gitignore"></a>.gitignore resultante

    ##################################################################
    # The original .gitignore file can be found at
    # https://github.com/github/gitignore/blob/master/Unity.gitignore
    ##################################################################

    # standard ignores for a Unity Project
    ...

    # ignore the whole UWP directory
    /UWP/**

    # except we want to keep...
    !/UWP/ScrapyardPhoenix/Assets/*
    !/UWP/ScrapyardPhoenix/Dependencies/*
    !/UWP/ScrapyardPhoenix/Properties/*
    !/UWP/ScrapyardPhoenix/App.cs
    !/UWP/ScrapyardPhoenix/Package.appxmanifest
    !/UWP/ScrapyardPhoenix/project.json
    !/UWP/ScrapyardPhoenix/ScrapyardPhoenix.csproj

Agora seus colegas de equipe serão sincronizados com o projeto UWP gerado. Agora você pode ficar à vontade para adicionar outros ativos, fontes e dependências ao seu projeto UWP.

Mais alguns exemplos de controle de versão da pasta UWP podem ser encontrados [nesses exemplos](https://bitbucket.org/Unity-Technologies/windowsstoreappssamples/overview).

## <a name="adding-dependencies-to-your-uwp-app"></a>Adicionando dependências ao nosso aplicativo UWP

Adicione dependências às DLLs e WINMDs colocando-as na sua pasta**Ativos do Unity**sob um uma pasta **plug-ins**, em seguida, selecione-as e defina as suas configurações de plataforma de destino de forma adequada no Inspetor.

![Solução UWP](images/uwp-solution.PNG)

***ScrapyardPhoenix (Windows Universal)*** é o projeto ao qual você adicionaria uma referência, por exemplo, o SDK do Xbox Live.

## <a name="see-also"></a>Consulte também
- [Trazendo existente de jogos para Xbox](development-lanes-landing.md)
- [UWP no Xbox One](index.md)
