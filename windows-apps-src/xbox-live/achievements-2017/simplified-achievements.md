---
title: Conquistas de 2017
description: Conquistas de 2017
ms.assetid: d424db04-328d-470c-81d3-5d4b82cb792f
ms.date: 04/04/2017
ms.topic: article
keywords: xbox live, xbox, jogos, uwp, windows 10, xbox one
ms.localizationpriority: medium
ms.openlocfilehash: bfc67f6aca27abf095a89c451111e6429bca82e1
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57590981"
---
# <a name="achievements-2017"></a>Conquistas de 2017

O sistema realizações 2017 permite que os desenvolvedores de jogos usam um modelo de chamada direto para desbloquear realizações de novos jogos do Xbox Live no Xbox One, Windows 10, Windows 10 Phone, Android e iOS.

## <a name="introduction"></a>Introdução

Com o Xbox One, introduzimos um novo sistema Cloud-Powered realizações que capacita os desenvolvedores de jogos para direcionar os dados para suas funcionalidades Xbox Live, como estatísticas do usuário, conquistas, recursos avançados de presença e com vários participantes, simplesmente enviando eventos de telemetria do jogo. Isso abriu uma infinidade de novos benefícios – um único evento pode atualizar dados para vários recursos do Xbox Live; Configuração do Xbox Live reside no servidor, em vez de no cliente; e muito mais.

Nos anos após o lançamento do Xbox One, ouvimos intimamente comentários dos desenvolvedores de jogos e os desenvolvedores têm consistentemente compartilhados a seguir:

1.  **Desejo de desbloqueio de medalhas por meio de um padrão de chamada direta.** Muitos desenvolvedores criam jogos para várias plataformas, incluindo as versões anteriores do Xbox, e os sistemas de medalha semelhante nessas plataformas usam um método direto de chamada. Suporte direto desbloquear chamadas no Xbox One e outras plataformas de Xbox gen atual seriam facilidade suas necessidades de desenvolvimento de jogos de plataforma cruzada e os custos de tempo de desenvolvimento.

2.  **Minimize a complexidade da configuração.** Com o sistema Cloud-Powered realizações, uma realização desbloquear lógica deve ser configurado no Xbox Live, para que os serviços saibam como interpretar os dados de estatísticas do título e quando desbloquear a realização de um usuário. Isso foi feito por meio de uma nova seção de regras de medalha da de uma realização configuração de que não existia anteriormente. Enquanto ter desbloquear lógica na nuvem pode ser muito poderosa, esse requisito de configuração adicionais adiciona complexidade para o design e criação de realizações de um título.

3.  **Difícil de solucionar problemas.** Embora o sistema Cloud-Powered realizações apresenta uma variedade de recursos úteis, ele também pode ser mais difícil para o jogo desenvolvedores para validar e solucionar problemas com suas realizações, pois medalha desbloqueia são disparados indiretamente por regras que em tempo real no serviço em vez de diretamente controlado pelo jogo em si.

Vale a pena observar que os desenvolvedores de jogos também repetidamente compartilharam comentários que eles gostaram dos ajustes e o valor de determinados recursos que foram introduzidos juntamente com o sistema Cloud-Powered realizações:

1.  Novos recursos de experiência de usuário, como a progressão de medalha, atualizações em tempo real, as remunerações de arte de conceito e lançamento desbloqueia no feed de atividades.

2.  Melhorias de configuração como uma configuração de serviço em vez de uma configuração local que deve ser incluída no pacote do jogo (ou seja, gameconfig, XLAST, SPA, etc.) e a capacidade de editar facilmente medalha de cadeias de caracteres & imagens depois que o jogo foi enviado.

Com o 2017 conquistas, estamos criando uma substituição de Cloud-Powered realizações sistema existente para títulos de futuros usar o que torna ainda mais fácil para os desenvolvedores de jogos do Xbox configurar as conquistas, integrar medalha desbloqueia & atualizações no código de jogos e validar que as realizações estão funcionando conforme o esperado.

## <a name="whats-different-with-achievements-2017"></a>Qual é a diferença com o 2017 conquistas

|                          | Sistema realizações 2017        | Sistema de conquistas em nuvem      |
|--------------------------|---------------------------------------|----------------------------------------|
| Desbloqueio de gatilho           | Diretamente por meio da chamada à API                 | Indiretamente por meio de eventos de telemetria        |
| Desbloquear proprietário             | Título                                 | Xbox Live                              |
| Configuração            | Cadeias de caracteres, imagens, recompensas              | Cadeias de caracteres, imagens, recompensas, desbloquear regras \[+ stats + eventos\]                    |
| Progressão              | Com suporte <br>*diretamente por meio da chamada à API*                | Com suporte <br> *indiretamente por meio de eventos de telemetria*       |
| Atividade em tempo real (RTA) | Com suporte                             | Com suporte                              |
| Desafios               | Sem suporte   | Com suporte                      |

## <a name="title-requirements"></a>Requisitos do título

A seguir estão os requisitos de qualquer título que usará o sistema realizações 2017.

1.  **Deve ser um novo título (não liberado).** Títulos que já foram liberados e estiver usando o sistema Cloud-Powered realizações não são qualificados. Para obter mais informações, consulte [por que não é possível títulos existentes "migrar" no novo sistema realizações 2017?](#_Why_can’t_existing)

2.  **Deve usar o XDK de agosto de 2016 ou mais recente.** A API Update_Achievement foi lançada em agosto de 2016 XDK.

3.  **Deve ser um título do XDK ou UWP.** O sistema realizações 2017 não está disponível para plataformas herdadas, incluindo Xbox 360, Windows 8.x ou mais, nem do Windows Phone 8 ou mais antigo.

## <a name="updateachievement-api"></a>Update_Achievement API

Depois que suas realizações são configuradas por meio de XDP ou [UDC](../configure-xbl/dev-center/achievements-in-udc.md) e publicado em sua área restrita de desenvolvimento, o título pode desbloqueá-los chamando a API Update_Achievement.

A API está disponível no XDK e o Xbox Live SDK.

### <a name="api-signature"></a>Assinatura de API

A assinatura de API é da seguinte maneira:

```c++
/// <summary>
    /// Allow achievement progress to be updated and achievements to be unlocked.  
    /// This API will work even when offline. On PC and Xbox One, updates will be posted by the system when connection is re-established even if the title isn't running
    /// </summary>
    /// <param name="xboxUserId">The Xbox User ID of the player.</param>
    /// <param name="titleId">The title ID.</param>
    /// <param name="serviceConfigurationId">The service configuration ID (SCID) for the title.</param>
    /// <param name="achievementId">The achievement ID as defined by XDP or Partner Center.</param>
    /// <param name="percentComplete">The completion percentage of the achievement to indicate progress.
    /// Valid values are from 1 to 100. Set to 100 to unlock the achievement.  
    /// Progress will be set by the server to the highest value sent</param>
    /// <remarks>
    /// Returns a task<T> object that represents the state of the asynchronous operation.
    ///
    /// This method calls V2 GET /users/xuid({xuid})/achievements/{scid}/update
    /// </remarks>
    _XSAPIIMP pplx::task<xbox::services::xbox_live_result<void>> update_achievement(
        _In_ const string_t& xboxUserId,
        _In_ uint32_t titleId,
        _In_ const string_t& serviceConfigurationId,
        _In_ const string_t& achievementId,
        _In_ uint32_t percentComplete
        );
```

`xbox::services::xbox_live_result<T>` é a chamada de retornada para todas as chamadas de API do C++ Xbox Live Services.

Para obter mais informações, confira a conversa Xfest 2015, "XSAPI: C++, nenhuma exceção!"<br>
[vídeo](https://go.microsoft.com/?linkid=9888207) |  [slides](https://developer.xboxlive.com/en-us/platform/documentlibrary/events/Documents/Xfest_2015/Xbox_Live_Track/XSAPI_Cpp_No_Exceptions.pptx)

### <a name="unlocking-via-updateachievement-api"></a>Desbloqueando via Update_Achievement API

Para desbloquear uma realização, defina as *percentComplete* a 100.

Se o usuário estiver online, a solicitação será enviada imediatamente para o serviço conquistas do Xbox Live e disparará as seguintes experiências de usuário:

-   O usuário receberá uma notificação de medalha desbloqueada;

-   A realização especificada será exibido como desbloqueado na lista de medalha do usuário para o título;

-   A realização desbloqueada será adicionada ao feed de atividades do usuário.

> *Observação: Não haverá nenhuma diferença visível nas experiências de usuário para realizações que usam o sistema realizações 2017 e as realizações Cloud-Powered.*

Se o usuário estiver offline, a solicitação de desbloqueio será enfileirada localmente no dispositivo do usuário. Quando o dispositivo do usuário restabeleceu a conectividade de rede, a solicitação será automaticamente enviado para o serviço realizações – Observe: nenhuma ação é necessária do jogo para disparar esse – e as experiências de usuário acima serão feita como descrito.

### <a name="updating-completion-progress-via-updateachievement-api"></a>Atualizando o progresso de conclusão por meio da API Update_Achievement

Para atualizar o progresso de um usuário para desbloquear uma realização, defina as *percentComplete* para o número apropriado de inteiro entre 1 e 100.

Progresso de uma realização pode aumentar somente. Se *percentComplete* é definido como um número menor do que a realização 's última *percentComplete* valor, a atualização será ignorada. Por exemplo, se a realização *percentComplete* tivesse sido definido previamente para 75, enviando uma atualização com um valor de 25 será ignorada e a realização ainda será exibida como 75% concluída.

Se *percentComplete* é definido como 100, veja o desbloquear.

Se *percentComplete* é definido como um número maior que 100, a API irá se comportar como se você defini-lo a exatamente a 100.

## <a name="frequently-asked-questions"></a>Perguntas frequentes

### <a name="span-idwhyarechallenges-classanchorspancan-i-ship-my-title-using-the-achievements-2017-system-yet"></a><span id="_Why_are_Challenges" class="anchor"></span>Posso enviar meu título ainda usando o sistema realizações 2017?

Com certeza! Todos os novos títulos são incentivados a utilizar o 2017 conquistas e convidados a virem sistema em vez do sistema Cloud-Powered conquistas.

### <a name="why-are-challenges-not-supported-in-the-achievements-2017-system"></a>Por que os desafios não têm suporte no sistema realizações 2017?

Dados de uso em jogos do Xbox mostrou que a implementação atual e a apresentação dos desafios não atender a uma necessidade para a maioria dos desenvolvedores de jogos. Vamos continuar coletando comentários neste espaço e entrada de desenvolvedor e se esforçar para fornecer recursos futuros que estão mais no ponto com as necessidades do desenvolvedor. O recém-lançado Xbox Arena é uma direção de novo, mas semelhante, de jogos de um exemplo de um recurso que introduz novas funcionalidades de concorrência para Xbox.

### <a name="can-i-still-add-new-achievements-every-calendar-quarter-if-my-title-is-using-the-achievements-2017-system"></a>Pode ainda adicionar novo realizações cada trimestre do calendário se meu título é usando o sistema de realizações 2017?

Sim. A política de realizações permanece inalterada.

### <a name="span-idwhycantexisting-classanchorspanwhy-cant-existing-titles-migrate-onto-the-new-achievements-2017-system"></a><span id="_Why_can’t_existing" class="anchor"></span>Por que não é possível títulos existentes "migrar" no novo sistema realizações 2017?

Para a maioria dos títulos existentes 'migração' para o sistema realizações 2017 não seria limitada a simplesmente Atualizando suas configurações de serviço e alternando gravações do evento para realização desbloquear a chamadas – Embora essas alterações sozinhas seria muito caro e levaria com um risco significativo de erro e comportamento indesejado que pode resultar nas realizações irremediavelmente sejam desfeitas. Em vez disso, a maioria dos títulos existentes também tem usuários com os dados existentes. A tentativa de converter um jogo em tempo real que já está usando as realizações Cloud-Powered sistema seria não apenas ser um esforço muito dispendioso, para o desenvolvedor e o Xbox, mas seria prejudicar significativamente perfis existentes dos usuários e/ou experiências de jogos.

### <a name="if-my-title-was-released-using-the-cloud-powered-achievements-system-can-any-future-dlc-for-the-title-switch-to-achievements-2017"></a>Se meu título foi lançado usando o sistema Cloud-Powered realizações, qualquer DLC futuras para o título alternar para realizações 2017?

Todas as conquistas para um título devem usar o mesmo sistema conquistas. Seja qual for o sistema realizações é usado pelo realizações do jogo base é o sistema que deve ser usado para todas as conquistas futuras para o título.

### <a name="while-testing-achievements-in-my-dev-sandbox-can-i-mix-and-match-between-using-the-achievements-2017-system-and-the-cloud-powered-achievements-system"></a>Durante o teste conquistas em minha área restrita de desenvolvimento, é possível misturar e corresponder entre usar o sistema de realizações 2017 e o sistema Cloud-Powered realizações?

Não. Todas as conquistas para um título devem usar o mesmo sistema conquistas.

### <a name="does-achievements-2017-also-include-offline-unlocks"></a>Realizações 2017 também inclui offline desbloqueia?

Se o título desbloqueia uma realização, enquanto o dispositivo estiver offline, a API Update_Achievement automaticamente da fila as solicitações de desbloqueio offline e será a sincronização automática com o Xbox Live quando o dispositivo restabeleceu a conectividade de rede, semelhante à atual Experiência de off-line em nuvem realizações do sistema. Realizações desbloqueia será ocorram enquanto o usuário estiver offline.

### <a name="i-see-a-new-achievementupdate-event-in-xdp-if-my-title-uses-that-event-does-that-mean-it-has-achievements-2017"></a>Eu vejo um novo evento de "AchievementUpdate" no XDP. Se o título da minha usa esse evento, isso significa que ele tem realizações 2017?

O *AchievementUpdate* evento base é exigido pelo Xbox Live para fins de back-end. Você pode ignorar com segurança-lo. Se seu título configura um evento usando esse tipo de evento de base, essas gravações de eventos serão ignoradas pelo Xbox Live. Títulos que são criados no sistema Cloud-Powered realizações devem continuar a configurar seus eventos, usando os outros tipos de evento de base. Não precisam configurar o títulos que são criados no sistema 2017 realizações *qualquer* eventos para fins de realização.
