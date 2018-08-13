---
author: JordanEllis6809
title: Unity - Controle de versão do seu projeto UWP
description: Versão do seu projeto UWP Unity.
ms.openlocfilehash: 3b796c31e6b284cea628ba68a34799cf9317ee2e
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.locfileid: "200782"
---
# <a name="unity-version-control-your-uwp-project"></a>Unity: controle de versão do seu projeto UWP

Ainda não criou seu jogo Unity para Xbox usando a Plataforma Universal do Windows (UWP)?  Consulte primeiro [Como trazer jogos Unity para a UWP no Xbox](development-lanes-unity.md).

Há alguns motivos diferentes pelos quais você adicionaria partes do seu diretório UWP gerado ao controle de versão, uma delas é a adição de dependências (por exemplo, SDK do Xbox Live).  Usaremos esse cenário como exemplo desse tutorial, e esperamos que ele ajude você a resolver necessidades individuais do seu projeto.

***Aviso de isenção: usaremos Git como nossa solução de controle de versão.  Se a sua for diferente, os conceitos ainda deverão ser os mesmos.***

Para atualizar sua memória, veja como é o diretório de nosso jogo, ***ScrapyardPhoenix***, atualmente:

![Pasta de destino da compilação](images/build-destination.png)

E esta é a aparência de nosso diretório UWP:

![Solução do VS da UWP](images/uwp-vs-solution.png)

Nesse diretório, estamos preocupados somente com uma pasta, a pasta ***ScrapyardPhoenix*** (inserir o nome do seu jogo aqui).  O restante pode ser ignorado no nosso controle de versão.

***Não conhece um arquivo .gitignore?  Consulte [gitignore](https://git-scm.com/docs/gitignore).***

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

`Assets` | ***Incluir*** | Contém imagens da Windows Store  
`Data`   | ***Ignorar*** | Onde o Unity compila seu projeto (Cenas, Sombreadores, Scripts, Prefabs, etc.).  
`Dependencies` | ***Incluir*** | Essa pasta foi criada para manter todas as dependências de UWP (por exemplo, XboxLiveSDK.dll)  
`Properties` | ***Incluir*** | Contém configurações mais avançadas que podem ser modificadas pelo desenvolvedor.  
`Unprocessed` | ***Ignorar*** | Contém arquivos Unity `.dll` e `.pdb`  

## <a name="files"></a>Arquivos  

`App.cs` | ***Incluir*** | Ponto de entrada para seu aplicativo UWP; Isso pode ser modificado e estendido com outros arquivos de origem  
`Package.appxmanifest` | ***Incluir*** | Manifesto do pacote para seu AppX  
`project.json` | ***Incluir*** | Descreve os pacotes NuGet de que seu `*.csproj` depende  
`ScrapyardPhoenix.csproj` | ***Incluir*** | Descreve o destino de compilação UWP e você adicionar outras dependências ao seu UWP projeto, esse arquivo `*.csproj` terá essas informações  
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
- [Trazendo jogos existentes para o Xbox](development-lanes-landing.md)
- [UWP no Xbox One](index.md)
