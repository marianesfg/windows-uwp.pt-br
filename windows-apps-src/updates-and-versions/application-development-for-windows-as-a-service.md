---
title: Desenvolvimento de aplicativos para Windows como serviço
description: Dissocie a versão do aplicativo e o suporte de builds específicos do Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: f384ca56-f2b2-4793-b251-f7f5735376bb
ms.localizationpriority: medium
ms.openlocfilehash: 47be38c6d7a5374b06789beede02647ef9b264d8
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "75737629"
---
# <a name="application-development-for-windows-as-a-service"></a>Desenvolvimento de aplicativos para Windows como serviço

**Aplica-se a**
-   Windows 10
-   Windows 10 Mobile
-   Windows 10 IoT Core 

No ambiente de hoje, em que as expectativas do usuário com frequência são definidas por experiências centradas no dispositivo, os ciclos de produtos completos precisam ser medido em meses, não anos. Além disso, novas versões devem ser disponibilizadas continuamente e implantadas com um impacto mínimo sobre os usuários. A Microsoft desenvolveu o Windows 10 para atender a estes requisitos implementando uma nova abordagem à inovação, desenvolvimento e entrega denominada [WaaS (Windows como serviço)](https://docs.microsoft.com/windows/deployment/update/waas-overview). A chave para habilitar ciclos de produtos significativamente menores enquanto mantém os níveis de alta qualidade, é uma abordagem centrada na comunidade inovadora para testes que a Microsoft implementou para o Windows 10. A comunidade, conhecida como Participantes do Programa Windows Insider, é composta por milhões de usuários em todo o mundo. Quando os participantes do Programa Windows Insider optam por ingressarem na comunidade, eles testam vários builds ao longo do curso do ciclo de vida do produto e fornecem feedback à Microsoft por meio de uma metodologia interativa denominada de liberação de versões de pré-lançamento.

Os builds distribuídos como versões de pré-lançamento dão à equipe de engenharia do Windows dados significativos referentes à qualidade do desempenho dos builds em uso efetivo. A liberação de versões de pré-lançamento com os Participantes do Programa Windows Insider também permite que a Microsoft teste builds nos mais diversos tipos de hardware, aplicativos e ambientes de rede e identifique problemas de forma muito mais rápida. Como um resultado, a Microsoft acredita que a concentração da comunidade na liberação de versões de pré-lançamento possibilitarão um ritmo mais rápido de entrega de inovações e uma versão pública de melhor qualidade do que nunca.

## <a name="windows-10-release-types-and-cadences"></a>Regularidades e tipos de versão do Windows 10

Embora a Microsoft libere builds de versões de pré-lançamento para o participante do programa Windows Insider, a Microsoft publicará amplamente dois tipos de versões do Windows 10 ao público em base contínua:

As **Atualizações de recursos** instalam as versões mais recentes de novos recursos, experiências e funcionalidades em dispositivos que já estão executando o Windows 10. Como as atualizações de recursos contém uma cópia completa do Windows, elas são também o que os usuários costumam usar para instalar o Windows 10 em dispositivos existentes executando o Windows 7 ou o Windows 8.1 e em novos dispositivos nos quais não há nenhum sistema operacional instalado. A Microsoft espera publicar as atualizações semestralmente. 

As **atualizações de qualidade** entregam resoluções de problemas de segurança e outras correções de bugs importantes. As atualizações de qualidade serão fornecidas para melhorar cada recurso atualmente no suporte, em uma frequência de uma ou mais vezes por mês. A Microsoft continuará a publicar atualizações de qualidade no processo de atualização das terças-feiras (algumas vezes referida como Patch da terça-feira). Além disso, a Microsoft pode publicar atualizações de qualidade adicionais para o Windows 10 fora do processo de atualização das terças-feiras quando for necessário para atender as necessidades do cliente.

Durante o desenvolvimento do Windows 10, a Microsoft aperfeiçoou o ciclo de liberação e engenharia de produto do Windows de forma que podemos entregar os recursos, experiências e funcionalidades desejadas pelos clientes de forma mais rápida que nunca. Também criamos novas formas para entregar e instalar as atualizações de recursos e as atualizações de qualidade que simplificam as implantações e gerenciamento existente, ampliando a base de funcionários que podem ficar atualizados com as mais recentes funcionalidades e experiências do Windows e um custo de propriedade menor. Portanto, implementamos novas opções de manutenção – referidas como Canal Semestral e LTSC (Canal de Manutenção em Longo Prazo) – que oferecem soluções pragmáticas para manter mais dispositivos mais atualizados nos ambientes corporativos do que era possível anteriormente.

A tabela a seguir descreve os vários canais de manutenção e seus principais atributos.

| Opção de manutenção | Disponibilidade de novas atualizações de recursos para instalação | Tempo de vida da manutenção | Principais benefícios | Edições compatíveis |
| --- | --- | --- | --- | --- |
| Canal Semestral (Direcionado) | Imediatamente após a primeira publicação pela Microsoft | 18 meses | Disponibiliza novos recursos para os usuários assim que possível | Home, Pro, Education, Enterprise, Mobile, IoT Core, Windows 10 IoT Core Pro (IoT Core Pro) |
| Canal Semestral | Aproximadamente 4 meses após a primeira publicação pela Microsoft | 18 meses a contar da primeira publicação | Fornece tempo adicional para testar novas atualizações de recursos antes da implantação | Pro, Education, Enterprise, Mobile Enterprise, IoT Core Pro |
| LTSC (Canal de Manutenção em Longo Prazo) | Imediatamente após a publicação pela Microsoft | 10 anos | Permite a implantação a longo prazo de versões selecionadas do Windows 10 em configurações com poucas alterações | Enterprise LTSB |

Para obter mais informações, confira [Opções de manutenção do Windows 10 para atualizações e upgrades](https://docs.microsoft.com/windows/deployment/update/waas-overview#servicing-channels).

## <a name="supporting-apps-in-windows-as-a-service"></a>Dar suporte a aplicativos no Windows como serviço

A abordagem tradicional para dar suporte a aplicativos tem sido lançar uma nova versão do aplicativo em resposta a uma versão do Windows. Isso pressupõe que há alterações da falha no sistema operacional subjacente que podem causar uma regressão ao aplicativo. Esse modelo envolve um ciclo de desenvolvimento e validação dedicado que exige que nossos parceiros de ISV se alinhem com a regularidade de lançamentos do Windows.

No modelo do Windows como um serviço, a Microsoft está fazendo um compromisso para manter a compatibilidade do sistema operacional subjacente. Isso significa que a Microsoft fará um esforço concentrado para garantir que não há nenhuma alteração significativa que afeta negativamente o ecossistema do aplicativo. Nesse cenário, quando houver um lançamento de um build do Windows, a maioria dos aplicativos (aqueles sem nenhuma dependência do kernel) continuarão a funcionar.

Tendo em vista essa alteração, a Microsoft recomenda que nossos parceiros de ISV dissociem seus lançamentos e suporte de aplicativos dos builds específicos do Windows. Nossos clientes mútuos são atendidos melhor por uma abordagem de ciclo de vida do aplicativo. Isso significa que, quando uma versão do aplicativo for lançada, ela terá suporte por um determinado período, independentemente do número de builds do Windows lançados nesse intervalo de tempo. O ISV fará um compromisso para fornecer suporte para essa versão específica do aplicativo enquanto ela tiver suporte no ciclo de vida. A Microsoft segue uma abordagem semelhante ao ciclo de vida do Windows que pode ser referenciada [aqui](https://support.microsoft.com/hub/4095338/microsoft-lifecycle-policy?C2=14019).

Essa abordagem reduz a sobrecarga de manutenção de um agendamento de aplicativo que se alinha com os lançamentos do Windows. O parceiros ISV devem estar liberados para lançar recursos ou atualizações em seu próprio ritmo. Achamos que nossos parceiros podem manter sua base de clientes atualizada com as últimas atualizações do aplicativo independentemente de um lançamento do Windows. Além disso, nossos clientes não precisam procurar uma instrução de suporte explícita sempre que um build do Windows for lançado. Veja um exemplo de uma instrução de suporte que aborda como um aplicativo pode ter suporte em versões diferentes do sistema operacional:

| Exemplo de uma instrução de suporte de ciclo de vida do aplicativo | |
| --- | --- |
| A Contoso é uma empresa de desenvolvimento de software e é a proprietária do popular aplicativo Mojave compartilhado principalmente no espaço corporativo. A Contoso libera sua próxima versão principal Mojave 14.0 e declara suporte base por um período de três anos a partir da data de lançamento. Durante o suporte base, todas as atualizações e suporte são gratuitas para o produto licenciado. A Contoso também declara um suporte estendido adicional de dois anos em que os clientes podem comprar atualizações e suporte para um período de cortesia. Além da data de término do suporte estendido, não há mais suporte para esta versão do produto. Durante o período de suporte base, a Contoso dará suporte ao Mojave 14.0 em todos os builds do Windows lançados. A Contoso também lançará atualizações para o Mojave conforme necessário e independentemente das versões do produto do Windows. | |

Nas seções a seguir, você encontrará mais informações sobre as etapas da Microsoft para manter a compatibilidade do sistema operacional subjacente. Você também encontrará orientações sobre as etapas que você pode executar para ajudar a manter a compatibilidade do ecossistema do sistema operacional e do aplicativo combinados. Há uma seção sobre como aproveitar os builds de liberação de versões de pré-lançamento do Windows para detectar regressões do aplicativo antes de um build do Windows ser lançado. Por fim, nós descrevemos como usamos uma instrumentação e abordagem orientadas por telemetria para aumentar a qualidade dos builds do Windows. Nós recomendamos que os ISVs adotem uma abordagem semelhante em seu portfólio de aplicativos.

## <a name="key-changes-since-windows7-to-ensure-app-compatibility"></a>Principais alterações desde o Windows 7 para garantir a compatibilidade de aplicativo

Entendemos que compatibilidade é importante para os desenvolvedores. Os ISVs e desenvolvedores querem verificar se os aplicativos deles serão executados como esperado em todas as versões compatíveis do sistema operacional Windows. Os consumidores e as empresas têm um importante investimento aqui — eles querem a certeza de que os aplicativos pelos quais eles pagaram continuarão funcionando. Nós sabemos que compatibilidade é o principal critério para decisões de compra. Aplicativos escritos com base em melhores práticas oferecerão muito menos variação de código quando uma nova versão do Windows for lançada e reduzirão a fragmentação — esses aplicativos têm um investimento em engenharia reduzido para manter e um tempo mais rápido no mercado.

No cronograma do Windows 7, compatibilidade era muito mais uma abordagem reativa. No Windows 8 começamos a observar isso de forma diferente, trabalhando no Windows para garantir que a compatibilidade fosse projetada como um item primordial, e não secundário. Windows 10 é a versão mais compatível por design do sistema operacional até a data. Veja algumas importantes maneiras por meio das quais conseguimos isso:
-   **Telemetria do aplicativo**: isso nos ajuda a compreender a popularidade do aplicativo no ecossistema do Windows para instruir o teste de compatibilidade.
-   **Parcerias ISV**: trabalhe diretamente com parceiros externos para fornecer dados a eles e ajudá-los a corrigir problemas que nossos usuários enfrentam.
-   **Revisões de design, detecção de upstream**: faça parceria com equipes de recursos para reduzir o número de alterações significativas no Windows. A análise de compatibilidade é uma entrada pela qual as nossas equipes de recurso devem passar.
-   **Comunicação**: controle mais rígido sobre alterações da API e melhor comunicação.
-   **Liberação de versões de pré-lançamento e loop de comentários**: os participantes do programa Windows Insider recebem as compilações das versões de pré-lançamento liberadas que ajudam a melhorar a nossa capacidade de localizar problemas de compatibilidade antes que uma compilação final seja lançada para os clientes. Esse processo de comentários não só expõe os bugs, mas verifica se entregamos os recursos que nossos usuários querem.

## <a name="best-practices-for-app-compatibility"></a>Melhores práticas para compatibilidade de aplicativos

A Microsoft usa dados de diagnóstico e de uso para identificar e solucionar problemas, melhorar nossos produtos e serviços e oferecer aos nossos usuários experiências personalizadas. Os dados de uso que coletamos também se estendem aos aplicativos executados nos computadores do ecossistema do Windows. Com base no que nossos clientes usam, criamos nossa lista para testar esses aplicativos, dispositivos e drivers em relação às novas versões do sistema operacional do Windows. O Windows 10 foi a versão mais compatível do Windows até o momento, com mais de 90% compatibilidade contra milhares de aplicativos populares. A equipe de Compatibilidade do Windows normalmente entra em contato com nossos parceiros de ISV para fornecer comentários, caso ocorram problemas, para que possamos compartilhar as soluções. Em condições ideais, gostaríamos que nossos clientes comuns conseguissem atualizar o Windows perfeitamente e sem perder a funcionalidade em seu sistema operacional ou dos aplicativos que eles dependem para a sua produtividade ou diversão.

As seções a seguir contêm algumas melhores práticas que a Microsoft recomenda para que você pode garantir que seus aplicativos sejam compatíveis com o Windows 10.

### <a name="windows-version-check"></a>Verificação de versão do Windows

A versão do sistema operacional foi incrementada com o Windows 10. Isso significa que o número de versão interno foi alterado para 10.0. Como sempre, não medimos esforços para manter a compatibilidade de aplicativos e dispositivos após uma mudança de versão do sistema operacional. Para a maioria das categorias de aplicativo (sem nenhuma dependência de kernel), a mudança não afetará negativamente a funcionalidade do aplicativo, e os aplicativos existentes continuarão funcionando bem no Windows 10.

A manifestação dessa alteração é específica do aplicativo. Isso significa que qualquer aplicativo que verifica especificamente para a versão do sistema operacional obterá um número de versão superior, que pode levar a uma ou mais das seguintes situações:
-   Os instaladores de aplicativos podem não ser capazes de instalar o aplicativo e os aplicativos talvez não consigam iniciar.
-   Os aplicativos podem ficar instáveis ou falhar.
-   Os aplicativos podem gerar mensagens de erro, mas continuar funcionando adequadamente.

Alguns aplicativos realizam uma verificação de versão e simplesmente passam um aviso para os usuários. No entanto, há aplicativos associados rigidamente a uma verificação de versão (nos drivers ou no modo de kernel para evitar a detecção). Nesses casos, o aplicativo falhará se for encontrada uma versão incorreta. Em vez de uma verificação de versão, recomendamos um dos procedimentos a seguir:
-   Se o aplicativo for dependente da funcionalidade de uma API específica, selecione a versão correta da API.
-   Detecte a alteração via APISet ou outra API pública e não use a versão como um proxy para algum recurso ou correção. Se houver alterações da falha e uma seleção adequada não for exposta, então isso será um bug.
-   Verifique se o aplicativo NÃO confere a versão de maneiras estranhas, como por meio do Registro, de versões de arquivo, deslocamentos, modo de kernel, drivers ou outros meios. Se o aplicativo certamente precisar verificar a versão, use as APIs GetVersion, que devem retornar o número principal, secundário e de build.
-   Se você estiver usando a API [GetVersion](https://docs.microsoft.com/windows/win32/api/sysinfoapi/nf-sysinfoapi-getversion?redirectedfrom=MSDN), lembre-se de que o comportamento dessa API mudou desde o Windows 8.1.

Se você tiver aplicativos como antimalware ou de firewall, deverá trabalhar por meio dos canais de comentários usuais e por meio do programa Windows Insider.

### <a name="undocumented-apis"></a>APIs não documentadas

Seus aplicativos não devem chamar APIs do Windows não documentadas ou depender da exportação de um arquivo específico do Windows ou de chaves do registro. Isso pode levar a perda da funcionalidade, perda de dados e possíveis problemas de segurança. Se a funcionalidade que o seu aplicativo requer não estiver disponível, será uma oportunidade de fornecer comentários por meio dos canais de comentários usuais e por meio do programa Windows Insider.

### <a name="develop-universal-windows-platform-uwp-and-centennial-apps"></a>Desenvolver aplicativos da UWP (Plataforma Universal do Windows) e Centennial

Incentivamos todos os ISVs de aplicativos para Win32 a desenvolver na [UWP (Plataforma Universal do Windows)](https://blogs.windows.com/windowsdeveloper/2016/02/25/an-update-on-the-developer-opportunity-and-windows-10/) e, especificamente, aplicativos [Centennial](https://channel9.msdn.com/Events/Build/2015/2-692) mais adiante. Há excelentes benefícios para desenvolver esses pacotes de aplicativos em vez de usar os instaladores Win32 tradicionais. Os aplicativos UWP também têm suporte na [Microsoft Store](https://blogs.windows.com/windowsdeveloper/2016/02/04/windows-store-trends-february-2016/), portanto, é mais fácil para você atualizar os usuários para uma versão consistente automaticamente, reduzindo os custos de suporte.

Se os seus tipos de aplicativos Win32 não funcionam com o modelo do Centennial, é altamente recomendável usar o instalador correto e verificar se isso foi completamente testado. Um instalador é a primeira experiência do seu cliente ou usuário com o seu aplicativo. Portanto, verifique se ele funciona bem. Geralmente, isso não funciona bem ou não foi totalmente testado para todos os cenários. O [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) pode ajudar você testar a instalação e desinstalação do seu aplicativo Win32 e a identificar uso de APIs não documentadas, bem como outros problemas básicos de práticas recomendadas relacionadas ao desempenho, antes dos usuários o utilizarem.

**Práticas recomendadas:**
-   Use instaladores que funcionam para versões de 32 bits e 64 bits do Windows.
-   Projete seus instaladores para execução em vários cenários (nível de usuário ou máquina).
-   Mantenha todos os recursos redistribuíveis do Windows no pacote original – se forem remontados, é possível que isso cause a perda do instalador.
-   Agende o tempo de desenvolvimento para seus instaladores. Eles geralmente são ignorados como um produto durante o ciclo de vida de desenvolvimento de software.

## <a name="optimized-test-strategies-and-flighting"></a>Estratégias de teste otimizadas e liberação de versões de pré-lançamento

A liberação de versões de pré-lançamento do sistema operacional Windows refere-se aos builds provisórios disponíveis para os participantes do programa Windows Insider antes de um build final ser lançado para a população geral. Quanto mais participantes do programa Windows Insider receberem essa versão de pré-lançamento desses builds temporários, mais comentários receberemos sobre a qualidade do build, a compatibilidade etc., e isso ajudará a melhorar a qualidade dos builds finais. Você pode participar desse programa de liberação de versões de pré-lançamento para verificar se seus aplicativos funcionam conforme o esperado nos builds iterativos do sistema operacional. Também recomendamos que você forneça comentários sobre como esses builds de versões de pré-lançamento estão funcionando para você, sobre problemas que ocorrerem e assim por diante.

Se seu aplicativo estiver na Store, você poderá liberar a versão de pré-lançamento do seu aplicativo por meio dela, o que significa que o seu aplicativo estará disponível para os nossos participantes do Programa Windows Insider para instalação. Os usuários podem instalar seu aplicativo e você pode receber comentários preliminares sobre ele antes de lançá-lo para a população geral. As seções a seguir descrevem as etapas para testar seus aplicativos com base em builds de versões de pré-lançamento do Windows.

### <a name="step-1-become-a-windows-insider-and-participate-in-flighting"></a>Etapa 1: Tornar-se um participantes do programa Windows Insider e participar da liberação de versões de pré-lançamento
Como um [participante do programa Windows Insider,](https://insider.windows.com/) você pode ajudar a moldar o futuro do Windows — seus comentários nos ajudarão a melhorar os recursos e funcionalidades na plataforma. Essa é uma comunidade vibrante em que você pode se conectar com outros entusiastas, participar de fóruns, trocar conselhos e saber mais sobre os futuros eventos apenas para o programa Insider.

Assim que você tiver acesso às versões prévias do Windows 10, Windows 10 Mobile, bem como do Emulador e do SDK mais recentes do Windows, você terá todas as ferramentas à sua disposição para desenvolver ótimos aplicativos e explorar as novidades da Plataforma Universal do Windows e da Microsoft Store.

Isso também é uma grande oportunidade para criar um ótimo hardware, com builds de versão prévia dos kits de desenvolvimento de hardware para que você possa desenvolver drivers universais para Windows. O IoT Core Insider Preview também está disponível em placas de desenvolvimento para IoT compatíveis, para que você possa criar incríveis soluções conectadas usando a Plataforma Universal do Windows.

Antes de se tornar um participante do programa Windows Insider, observe que a participação destina-se para os usuários que:
-   Querem experimentar um software que ainda está em desenvolvimento.
-   Desejam compartilhar comentários sobre o software e a plataforma.
-   Não se importam com grandes quantidades de atualizações nem com um design de interface do usuário que mude consideravelmente com o passar do tempo.
-   Conhecem realmente um computador e se sentem confortáveis em solucionar problemas, fazer backup de dados, formatar um disco rígido, instalar um sistema operacional do zero ou restaurar o sistema antigo se for necessário.
-   Sabem o que é um arquivo ISO e como usá-lo.
-   Não o estão instalando no computador ou dispositivo do dia a dia.

### <a name="step-2-test-your-scenarios"></a>Etapa 2: Testar seus cenários

Depois de ter atualizado para um build de versão de pré-lançamento, os casos a seguir são alguns casos de teste de exemplo para ajudar você a começar a usar testes e reunir comentários. Para a maioria desses testes, verifique se você abrange os sistemas x86 e AMD64.
**Teste de instalação limpa:** Em uma instalação limpa do Windows 10, verifique se seu aplicativo é totalmente funcional. Se o seu aplicativo falhar neste teste e no teste de atualização, é provável que o problema seja causado pelas alterações do sistema operacional subjacente ou bugs no aplicativo. Se depois da investigação o primeiro for o caso, use o Programa Windows Insider para fornecer comentários e compartilhar soluções.

**Teste de atualização:** Verificar se o seu aplicativo funciona após a atualização de uma versão de nível inferior do Windows (isto é, Windows 7 ou Windows 8.1) para Windows 10. Seu aplicativo não deve provocar reversões durante a atualização e deve continuar funcionando como esperado após a atualização — isso é fundamental para obter uma experiência perfeita de atualização.

**Teste de reinstalação:** Verifique se a funcionalidade do aplicativo pode ser restaurada por meio da reinstalação do seu aplicativo após a atualização do computador para o Windows 10 de um sistema operacional de nível inferior. Se seu aplicativo não passar no teste de atualização e você não for capaz de reduzir a causa desses problemas, possivelmente uma reinstalação poderá restaurar a perda de funcionalidade. Não passar em um teste de reinstalação indica que partes do aplicativo podem não terem sido migradas para o Windows 10.

**Teste dos recursos do dispositivo\\sistema operacional:** Verifique se seu aplicativo funciona como esperado caso seu aplicativo dependa da funcionalidade específica no sistema operacional. Áreas comuns para testes incluem o seguinte, geralmente em relação a uma seleção de modelos de computador comumente usados para garantir uma cobertura:
-   Áudio
-   Funcionalidade do dispositivo USB (teclado, mouse, cartão de memória, disco rígido externo e assim por diante)
-   Bluetooth
-   Gráficos\\tela (com vários monitores, projeção, rotação da tela etc.)
-   Tela touch (orientação, teclado virtual, caneta, gestos e assim por diante)
-   Touchpad (botões esquerdo\\direito, toque, rolagem etc.)
-   Caneta (toque simples\\duplo, pressionar, segurar, borracha etc.)
-   Impressão\\digitalização
-   Sensores (acelerômetro, fusão e assim por diante)
-   Câmera

### <a name="step-3-provide-feedback"></a>Etapa 3: Fornecer comentários

Conte como seu aplicativo está se comportando em relação às compilações de versões de pré-lançamento liberadas. À medida que você descobrir problemas com seu aplicativo durante o teste, registre os bugs por meio do portal do parceiro, se você tiver acesso, ou por meio do seu representante da Microsoft. Recomendamos essas informações para que possamos construir uma experiência de qualidade para os nossos usuários juntos.


## <a name="related-topics"></a>Tópicos relacionados
[Opções de manutenção do Windows 10 para atualizações e upgrade](https://docs.microsoft.com/windows/manage/introduction-to-windows-10-servicing)
