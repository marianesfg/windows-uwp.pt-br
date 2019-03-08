---
title: Xbox Arena
description: Saiba como usar a Arena do Xbox para executar torneios para o seu jogo.
ms.date: 10/12/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, arena, torneio, experiência do usuário
ms.localizationpriority: medium
ms.openlocfilehash: b08da01323d05c961005d562b70667dbbdf85437
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643761"
---
# <a name="xbox-arena"></a>Xbox Arena

Xbox Arena é uma plataforma para criar e executar torneios on-line no Xbox One e Windows 10 por meio do Xbox Live, com o objetivo de colocar a diversão dos jogos competitiva para um público amplo.
Todos os publicadores tem necessidades diferentes quando se trata de jogos competitivo e com Arena queremos ser tão flexíveis quanto possível. Portanto, há três maneiras diferentes de executar torneios para os títulos na Arena:

* Com a execução de franquia torneios, Xbox Live fornece as ferramentas e serviços para configurar e executar seus próprios torneios.

* Xbox fez parceria com líderes do setor torneio organizadores (TOs), que pode permitir que você para percorrer torneios em nome de seu título Arena. (Para obter uma lista dos organizadores de torneio com suporte, entre em contato com seu gerente de conta da Microsoft.)

* Players podem criar e executar seus próprios torneios Arena com nosso torneios gerados pelo usuário ou UGTs.

O mais importante, adicionando suporte a Arena para os títulos permite que você para tirar proveito de todos os três. Fornecemos APIs robustas que permitem títulos para promover torneios no jogo e para facilitar a transição dos participantes de e para correspondências. O Hub do Xbox Arena dá suporte a tarefas de gerenciamento de torneio, como o registro, notificações, colchetes e propagação e relatar os resultados.

> [!IMPORTANT]  
> Xbox Arena só está disponível para os parceiros gerenciados e ID@Xbox os desenvolvedores. Ele não está disponível para o programa de criadores do Xbox Live.

## <a name="a-titles-baseline-tournament-experience"></a>Experiência de torneio de linha de base do título

Se seu título integra-se com um ou mais organizadores de torneio, ele deverá fornecer uma experiência de torneio da linha de base. Você tem liberdade para integrar mais profundamente um organizador de torneio específico se você escolher, para fornecer uma experiência mais rica para competições. Mas você ainda deverá fornecer a experiência de linha de base para outros organizadores de torneio e UGTs e torneios franquia execução se você optar por executá-los.

### <a name="baseline-requirements-for-a-title"></a>Requisitos de linha de base para um título

* Entre em contato com seu gerente de conta da Microsoft para obter uma lista completa dos requisitos.

### <a name="ui-recommendations"></a>Recomendações de interface do usuário

* Identificar-se de que a correspondência é um torneio na interface do usuário.

* Em sua interface do usuário do corredor, inclua um elemento de interface do usuário que os usuários de links para o hub de torneio e/ou a página de detalhes do torneio no shell do Xbox Arena da interface do usuário ou aplicativo de organizador de torneio.



A experiência do usuário de linha de base que o título deve fornecer é simples e flexível o suficiente para trabalhar com vários formatos possíveis de concorrência. Fique à vontade para adaptar as diretrizes de experiência do usuário e a experiência de requisitos de acordo com o fluxo de interface do usuário do seu título e para garantir que um usuário suave.

Por exemplo:

* As informações necessárias não tem a ser exibido na tela principal, desde que esteja disponível em algum lugar, como em uma página de detalhes ou pop-up.

* Pode haver muitas versões de cada tela ou podem ser combinadas entre si ou com telas de jogos existentes. Por exemplo, muitos jogos incluem uma pós-correspondência de "relatório de resultado:" tela "Pronto" requisitos e da tela, que pode ser adaptado para atender tanto os "aguardando resultados".

* As telas não é necessário alterar assim que o estágio realiza. Por exemplo, se o estágio da equipe muda de "Pronto" para "Reprodução" enquanto o usuário está em uma tela "Pronta", o título não é necessário para saltar imediatamente para o jogo. Ele pode (e provavelmente deve) alternar o "aguardando para correspondência..." indicador a um botão — por exemplo, "pronto para reproduzir" — para que o usuário está no controle do fluxo e, portanto, pode ter uma melhor compreensão sobre ele. É okey para os requisitos do estágio de "Reprodução" ser adiado até que o usuário confirme a transição.


## <a name="arena-vs-title-roles"></a>Arena vs. title "funções"

Saem do jogo, à medida que eles percorre os estágios de torneio, pode ficar complicada para os usuários. Quando o processo é diferente para cada jogo jogam, há ainda menos chance de memorizar onde ir e o que esperar.

> [!TIP]
> **Recomendação de experiência do usuário**  
>
> Simplifica funções funcionais entre o jogo e a interface do usuário do Xbox Arena, mantendo-os claramente distinguíveis. Por exemplo, todas as tarefas de gerenciamento são concluídas na Arena, e todas as tarefas relacionadas a play jogos são concluídas dentro do jogo.

Função Xbox Arena (Configurando um torneio)   | Função do título (jogo)
--- | ---
<ul><li>Registro e check-in</li><li>Notificações</li><li>Propagando e colchetes</li><li>Formação da equipe</li></ul> |     <ul><li>Transição de participantes de e para a interface do usuário Arena</li><li>Identificando correspondências de torneio específicas no lobby da interface do usuário com vários participantes</li><li>Promoção e/ou a navegação de torneios no jogo</li></ul>

## <a name="engineering-guidance"></a>Orientação de engenharia

Artigo | description
--- | ---
[Integração de título da arena](arena-title-integration.md) | Saiba como integrar o suporte a Arena do Xbox em seu título.

## <a name="operations-guidance"></a>Diretrizes de operações

Artigo | description
--- | ---
[Portal do Xbox Arena operações](operations-portal.md) | Descreve o portal de operações que você pode usar para criar e gerenciar torneios oficiais para um título que é integrado com a Arena do Xbox.

## <a name="user-experience-guidance"></a>Diretrizes de experiência do usuário

Artigo | description
--- | ---
[Descobrindo torneios Xbox](discovering-xbox-tournaments.md) | Fornece dicas e recomendações para criar um usuário excelente experiência para a descoberta de torneios existentes.
[Ingressar em um torneio](arena-ux-join-tournament.md)  |  Fornece dicas e recomendações para criar um usuário excelente experiência para registrar e ingressar em um torneio.
[Contrato de correspondência](arena-ux-match-engagement.md) | Descreve os estágios de experiência do usuário de jogadores em andamento por meio de um torneio.
[Metadados de API da interface do usuário da arena](arena-apis-metadata.md)  | Descreve os metadados retornados pelas APIs Arena que pode ser usado para exibir informações no jogo sobre o estado atual do torneio.
[Notificações da arena](arena-notifications.md)  | Descreve as condições ao Xbox Arena envia uma notificação aos participantes do torneio.
[Cenários de usuário da arena](arena-user-scenarios.md)  | Descreve cenários da Arena para jogadores com base em suas motivações mais comuns.
