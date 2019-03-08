---
title: Verificar configurações de privilégios de usuários no Unity
description: Saiba como verificar as configurações de privilégio do entrou na conta do Xbox Live.
ms.assetid: ''
ms.date: 10/26/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, contas, contas de teste, controles dos pais, privilégios de usuário, imposição proibe, venda adicional
ms.openlocfilehash: b55ebf9b53cadf2e57317347adce19c3578f9d56
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57620401"
---
# <a name="check-user-privilege-settings-in-unity"></a>Verificar configurações de privilégios de usuários no Unity
No Xbox Live, conta de cada usuário autenticado tem privilégios associados. Privilégios de controlam quais recursos do Xbox Live um usuário pode acessar em um determinado ponto no tempo. Alguns desses privilégios são recursos controlados pelo sistema, enquanto outros podem ser associados com jogos específicos ou assinaturas de extensão. Além disso, os controles dos pais e bans pelo uso emitidas pela equipe de imposição do Xbox Live podem restringir os privilégios de um usuário. Esses privilégios abrangem um número de cenários comuns, incluindo o conteúdo gerado pelo usuário com vários participantes, o acesso, bate-papo, ou para transmissão de vídeo. Jogos usam essas informações para tomar decisões de controle e personalização do acesso.

## <a name="prerequisites"></a>Pré-requisitos
Para determinar as configurações de privilégios de usuário, você deve ter configurado o seu jogo para autenticação com o Xbox Live e entrar com êxito um usuário.

>[!IMPORTANT]
> Se você estiver testando seu jogo no editor do Unity, o seu jogo não está conectado ao serviço Xbox Live e está usando dados falsos para simular uma conexão. Isso resulta em um valor nulo quando verificar se há privilégios. Para testar com dados reais, executar um build da plataforma Universal do Windows do seu jogo do Unity e abra o arquivo de projeto gerado no Visual Studio.

Os artigos a seguir descrevem as etapas que você pode executar:

* [Entrar no Xbox Live no Unity (compilação e teste de entrada)](unity-prefabs-and-sign-in.md#build-and-test-sign-in)
* [Testar sua compilação de jogo do Unity no Visual Studio](test-visual-studio-build.md)

## <a name="determine-privileges"></a>Determinar os privilégios
Privilégios do usuário são executados no token recebido no momento da autenticação. No Unity, você pode acessar a lista de privilégios que um usuário tem no `XboxLiveUser` de classe, depois que o usuário entrou com êxito. Privilégios são armazenados como uma única cadeia de caracteres separada por um espaço. Por exemplo, você poderá ver que um usuário com os seguintes privilégios:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

Debug.Log(XboxLiveUser.User.Privileges);

//Console would read:
// Privileges: "188 191 192 193 194 195 196 198 199 200 201 203 204 205 206 207 208 211 214 215 216 217 220 224 227 228 235 238 245 247 249 252 254 255"
```

Se você quiser procurar uma permissão específica, você pode verificar se o `Privileges` propriedade contém o código associado:

```csharp
public XboxLiveUserInfo XboxLiveUser;

//sign in is done and the user has been successfully signed in

if (XboxLiveUser.User.Privileges.Contains("247"))
{
    Debug.Log("User has the user_created_content privilege");
}
```

## <a name="privilege-codes"></a>Códigos de privilégio
O exemplo a seguir é uma lista de códigos de privilégio possíveis que podem ser retornados.

| Código  | Privilégio  | Descrição   |
|------ |-----------------------------  |-------------------    |
| 190   | Difusão             | Pode transmitir ao vivo jogo.     |
| 197   | view_friends_list     | Pode exibir a lista de amigos do outro usuário.   |
| 198   | game_dvr              | Upload pode registradas no jogo vídeos para a nuvem.      |
| 199   | share_kinect_content          | Kinect conteúdo registrado pode ser carregado para a nuvem para o usuário e acessíveis para qualquer pessoa. |
| 203   | multiplayer_parties           | Pode ingressar em uma sessão de terceiros.     |
| 205   | communication_voice_ingame    | Podem participar em bate-papo durante as partes e sessões de jogos com vários participantes.    |
| 206   | communication_voice_skype     | Pode usar a comunicação de voz com Skype no Xbox One.   |
| 207   | cloud_gaming_manage_session   | Pode alocar e gerenciar um cluster de computação de nuvem para uma sessão de jogo hospedada.    |
| 208   | cloud_gaming_join_session     | Pode ingressar em uma sessão de computação de nuvem.     |
| 209   | cloud_saved_games     | Pode salvar jogos título no armazenamento em nuvem.    |
| 211   | share_content     | Pode compartilhar conteúdo com outras pessoas.    |
| 214   | premium_content   | Pode comprar, baixe e inicie o conteúdo premium disponível com a assinatura do Xbox Live Gold.     |
| 219   | subscription_content  | Pode comprar e baixar o conteúdo de assinatura premium e usar os recursos de assinatura premium.     |
| 220   | social_network_sharing    | Pode compartilhar informações sobre o andamento em redes sociais.    |
| 224   | premium_video     | Pode acessar os serviços de vídeo premium.    |
| 235   | purchase_content  | Pode comprar o conteúdo.     |
| 247   | user_created_content  | Pode baixar e exibir o usuário online criado conteúdo.    |
| 249   | profile_viewing   | Pode exibir perfis do outro usuário.   |
| 252   | Comunicações    | Pode usar texto assíncrono de mensagens com qualquer pessoa.    |
| 254   | multiplayer_sessions  | Pode ingressar em uma sessão com vários participantes para um jogo.   |
| 255   | add_friend    | Pode seguir outros usuários do Xbox Live e adicionar amigos do Xbox Live.   |
