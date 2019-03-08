---
title: Em destaque Stats e placares de líderes de 2017
description: Saiba como configurar o Xbox Live em destaque Stats e placares de líderes 2017 no Partner Center
ms.assetid: e0f307d2-ea02-48ea-bcdf-828272a894d4
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox Live, Xbox, jogos, uwp, windows 10, Xbox um, estatísticas em destaque e placares de líderes, placares de líderes, estatísticas de 2017, Partner Center
ms.openlocfilehash: e152ea8aa745536f0b7843f4f7d372a79dc3223f
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655101"
---
# <a name="configuring-featured-stats-and-leaderboards-2017-in-partner-center"></a>Configurando as estatísticas em destaque e placares de líderes 2017 no Partner Center

Para um jogo interagir com o serviço de estatísticas, um stat precisa ser definida no [Partner Center](https://partner.microsoft.com/dashboard). Todas as estatísticas em destaque aparecerá na GameHub, o que torna a agir automaticamente como um placar de líderes. Armazenaremos o valor bruto, no entanto, o jogo terá a lógica para determinar se um novo valor deve ser fornecido.

![Captura de tela da página realizações no Hub de jogo](../../images/dev-center/featured-stats-and-leaderboards/featured-stats-and-leaderboards-2.png) a figura anterior mostra a aparência de estatísticas em destaque no GameHub do seu título. As estatísticas em destaque são mostradas withing caixa vermelha.

Com o 2017 de plataforma de dados, você só precisará configurar uma estatística que é usada para um placar de líderes social que estiver em destaque na página de GameHub um jogador.

Você pode usar o Partner Center para configurar um stat em destaque e placar de líderes que está associado com seu jogo. Adicione configuração fazendo o seguinte:

1. Navegue até a **estatísticas em destaque e placares de líderes** seção em seu título, localizado sob **Services** > **Xbox Live**  >  **Em destaque Stats e placares de líderes**.
2. Clique o **New** botão que abrirá um formulário modal. Depois de preencher, clique em **salvar**.

![Imagem da nova caixa de diálogo stat/placar de líderes em destaque](../../images/dev-center/featured-stats-and-leaderboards/featured-stats.png)

O **nome de exibição** campo é o que os usuários verão no GameHub. Essa cadeia de caracteres pode ser localizada na **localizar cadeias de caracteres** da configuração do serviço Xbox Live.

O **ID** campo é o nome stat e é como você fará referência a seu stat ao atualizá-lo no seu código. Consulte a [Atualizando estatísticas](../../leaderboards-and-stats-2017/player-stats-updating.md) para obter mais detalhes.

O **formato** é o formato de dados de stat. As opções incluem inteiro, Decimal, percentual, intervalo de tempo curto, longo período de tempo e cadeia de caracteres.

Cada **formato** opção fornecerá algumas informações sobre os valores aceitáveis ou formatação sob a operação de soltar para baixo quando selecionado.

* Estatísticas de inteiro aceitam números inteiros como 1, 2 ou 100.
* Estatísticas de decimais aceitam números fracionários com duas casas decimais como 1,05 ou 12.00
* Estatísticas de porcentagem aceitam números inteiros entre 0 e 100. '% s' será acrescentado ao final do número inteiro. (por exemplo, 0%, 100%)
* Estatísticas de intervalo de tempo curto seguem o formato hh: mm ss como 02:10:30 e solicitará que você forneça uma unidade de tempo para a estatística.   As unidades de tempo disponíveis são milissegundos, segundos, minutos, horas e dias.
* Timespan longo stat seguem o formato de Xd Xh Xm, como 1d 2h 10m, esta estatística também pedirá que você forneça uma unidade de tempo para a estatística.

O **classificação** campo permite que você altere a ordem de classificação de placar de líderes deve estar em ordem crescente ou decrescente.

Observe os seguintes requisitos ao configurar um placar de líderes e um stat em destaque:

| Tipo de desenvolvedor | Requisito | Limite |
|----------------|-------------|-------|
| Programa de Criadores do Xbox Live | Não há nenhum requisito para designar qualquer estatísticas como estatísticas em destaque | 20 |
| ID@Xbox e parceiros da Microsoft | Você deve designar pelo menos 3 estatísticas em destaque | 20 |

## <a name="next-steps"></a>Próximas etapas

Em seguida, você precisará atualizar as estatísticas do seu código.  Ver [Atualizando estatísticas](../../leaderboards-and-stats-2017/player-stats-updating.md) para obter mais detalhes.
