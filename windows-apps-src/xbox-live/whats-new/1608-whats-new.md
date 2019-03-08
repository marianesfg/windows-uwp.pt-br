---
title: O que há de novo para o Xbox Live SDK – agosto de 2016
description: O que há de novo para o Xbox Live SDK – agosto de 2016
ms.assetid: fa52e7bd-2c2c-4c25-94ab-761036a7ca79
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: 2a498fea1ed0974935a273c9ee72ba2c95d15959
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57627901"
---
# <a name="whats-new-for-the-xbox-live-sdk---august-2016"></a>O que há de novo para o Xbox Live SDK – agosto de 2016

Consulte a [What's New - junho de 2016](1606-whats-new.md) artigo para o que foi adicionado na versão de junho de 2016.

## <a name="os-and-tool-support"></a>Sistema operacional e a ferramenta de suporte
O Xbox Live SDK dá suporte ao Windows 10 RTM [versão 10.0.10240] e o Visual Studio 2015 RTM [versão 14.0.23107.0].

## <a name="documentation"></a>Documentação
- Se você estiver escrevendo um aplicativo UWP e estiver implementando a capacidade de convidar usuários para um jogo, há instruções sobre o ```.appxmanifest``` as alterações necessárias na [configurar seu AppXManifest para vários jogadores](../multiplayer/service-configuration/configure-your-appxmanifest-for-multiplayer.md).  Isso foi discutido anteriormente a [fóruns](https://forums.xboxlive.com) e o [portabilidade de código ao vivo xbox da era para uwp](../using-xbox-live/porting-xbox-live-code-from-xdk-to-uwp.md) artigo
- O [Introdução ao Gerenciador de Social](../social-platform/intro-to-social-manager.md) artigo foi atualizado para refletir as alterações recentes de API e fornecem mais informações sobre códigos de retorno para algumas das funções.

## <a name="unity-samples"></a>Exemplos de Unity
Alguns exemplos de novos foram adicionados para desenvolvedores do Unity escrevendo um aplicativo UWP.
- Agora há uma versão do Unity do exemplo Social, você pode encontrá-lo sob o diretório de exemplos/Social/Unity.
- Também é um exemplo que descrevem como usar o armazenamento conectado.  Consulte GameSave/Samples/Unity para o exemplo.
Há uma versão do Unity do AchievementsLeaderboard sob AchievementsLeaderboard/Samples/Unity

## <a name="social-manager"></a>Gerenciador de social
Além das atualizações de documentação mencionadas acima, há algumas novas APIs que foram adicionados.  Elas são descritas abaixo, e você pode ver mais detalhes em social_manager.h

- Adicionada nova API que permite atualização de redes sociais grupos sem recriação de:

```cpp
    _XSAPIIMP xbox_live_result<void> update_social_user_group(
        _In_ const std::shared_ptr<xbox_social_user_group>& group,
        _In_ const std::vector<string_t>& users
        );
```
- Uma atualização concluída do grupo de redes sociais é indicada pelo ```social_user_group_updated``` evento


## <a name="multiplayer"></a>Multijogadores
Navegação aprimorada sessão agora está disponível e você pode usar novas APIs com vários participantes para utilizá-lo.

Usando as novas APIs, você pode filtrar as marcas, cadeias de caracteres e outros dados avançados para permitir que os usuários localizar mais facilmente uma sessão que deseja executar.

Publicaremos uma documentação abrangente mais nos próximos meses, mas brevemente agora você pode associar a um "identificador de pesquisa" com uma sessão de MPSD usando ```set_search_handle``` e, em seguida, os usuários podem pesquisar sessões usando um mecanismo robusto de filtragem chamando seu título ```get_search_handles```

As novas APIs estão listadas abaixo.  Tente-os e, se você tiver algum problema, poste um thread de suporte na [fóruns](https://forums.xboxlive.com) ou entre em contato para sua mãe.  Temos exemplos de como usar essas APIs em breve.

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> set_search_handle(
    _In_ multiplayer_search_handle_request searchHandleRequest
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<std::vector<multiplayer_search_handle_details>>> get_search_handles(
    _In_ const string_t& serviceConfigurationId,
    _In_ const string_t& sessionTemplateName,
    _In_ const string_t& orderBy,
    _In_ bool orderAscending,
    _In_ const string_t& searchQuery
    );
```

```cpp
_XSAPIIMP pplx::task<xbox_live_result<void>> clear_search_handle(_In_ const string_t& handleId);
```

### <a name="xbox-integrated-multiplayer"></a>Xbox integrado com vários participantes

Incluímos a documentação para o Xbox integrado com vários participantes (XIM) API.  A própria API estará disponível em uma versão subsequente do Xbox Live SDK, mas a documentação e o cabeçalho estão sendo disponibilizados para visualização.

XIM é uma interface independente para adicionar facilmente com vários participantes comunicação de rede e bate-papo em tempo real ao seu jogo com a potência de serviços do Xbox Live.

Essa visualização da documentação da API é compartilhada aqui para incentivar a consulta e os comentários dos clientes. Falamos sobre essa API anteriormente em Xfest 2016, e você pode ver arquivados [materiais de apresentação no site de desenvolvedor do parceiro gerenciado](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Pages/Xfest2016.aspx) da palestra "Turnkey com vários participantes de rede e bate-papo". Observe que esta documentação de visualização é apenas para a API do C++. Equivalentes do WinRT para C# e outros idiomas serão lançados mais tarde no ano.

Se você estiver interessado em recursos do XIM, tiver comentários ou outras perguntas sobre esse projeto, fique à vontade postar na [Fórum de desenvolvedores do Xbox](https://forums.xboxlive.com/) ou entre em contato por meio de seu gerente de conta de desenvolvedor.

Você pode ver essa nova documentação em xbox_integrated_multiplayer.chm na pasta Docs do Xbox Live SDK.  O arquivo de inclusão está disponível como uma visualização em \include\xim\XboxIntegratedMultiplayer.h.  
