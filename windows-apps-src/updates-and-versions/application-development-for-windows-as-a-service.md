---
title: "Desenvolvimento de aplicativos para Windows como serviço (Windows 10)"
description: "Dissocie a versão do aplicativo e o suporte das compilações específicas do Windows."
author: jdeckerMS
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: 72ac67b17fc519d374798e5121b309f664ff6b1b

---

# Desenvolvimento de aplicativos para Windows como serviço

**Aplicável a**
-   Windows 10
-   Windows 10 Mobile
-   Windows 10 IoT Core 

No ambiente de hoje, onde as expectativas do usuário com frequência são definidas por experiências centradas no dispositivo, os ciclos de produtos completos precisam ser medido em meses, não anos. Além disso, novas versões devem ser disponibilizadas continuamente e devem ser implantadas com um impacto mínimo sobre os usuários. A Microsoft desenvolveu o Windows10 para atender a estes requisitos implementando uma nova abordagem à inovação, desenvolvimento e entrega denominada [Windows como um serviço (WaaS)](https://technet.microsoft.com/itpro/windows/manage/introduction-to-windows-10-servicing). A chave para habilitar ciclos de produtos significativamente mais curtos e ao mesmo tempo manter os níveis de alta qualidade foi uma abordagem inovadora centrada na comunidade para a execução de testes que a Microsoft implementou para o Windows 10. A comunidade, conhecida como Participantes do Programa Windows Insider, é composta por milhões de usuários em todo o mundo. Quando os participantes do Programa Windows Insider optam por ingressarem na comunidade, eles testam várias compilações ao longo do curso do ciclo de vida do produto e fornecem feedback à Microsoft por meio de uma metodologia interativa denominada de liberação de versões de pré-lançamento.

As compilações distribuídas como Programa Windows Insider fornecem à equipe de engenharia do Windows dados significativos relativos ao desempenho das compilações em uso real. O Programa Windows Insider também permite que a Microsoft teste compilações nos mais diversos tipos de hardware, aplicativos e ambientes de rede e identifique problemas de forma muito mais rápida. Como um resultado, a Microsoft acredita que a concentração da comunidade na liberação de versões de pré-lançamento possibilitarão um ritmo mais rápido de entrega de inovações e uma versão pública de melhor qualidade do que nunca.

## Regularidades e tipos de versão do Windows 10

Embora a Microsoft libere versões de pré-lançamento compiladas para o participante do programa Windows Insider, a Microsoft publicará amplamente dois tipos de versões do Windows 10 ao público em base contínua:

As **Atualizações de recursos** instalam as versões mais recentes de novos recursos, experiências e funcionalidades em dispositivos que já estão executando o Windows 10. Como as atualizações de recursos contém uma cópia completa do Windows, elas são também o que os usuários costumam usar para instalar o Windows10 em dispositivos existentes executando o Windows7 ou o Windows8.1 e em novos dispositivos nos quais não há nenhum sistema operacional instalado. A Microsoft espera publicar uma média de uma a duas novas atualizações de recursos por ano.

**Atualizações de qualidade** entregam resoluções de problemas de segurança e outras correções de bugs importantes. As atualizações de qualidade serão fornecidas para melhorar cada recurso atualmente no suporte, em uma frequência de uma ou mais vezes por mês. A Microsoft continuará a publicar atualizações de qualidade no processo de atualização das terças-feiras (algumas vezes referida como Patch da terça-feira) Além disso, a Microsoft pode publicar atualizações de qualidade adicionais para o Windows10 fora do processo de atualização das terças-feiras quando for necessário para atender as necessidades do cliente.

Durante o desenvolvimento do Windows10, a Microsoft aperfeiçoou o ciclo de liberação e engenharia de produto do Windows de forma que podemos entregar os recursos, experiências e funcionalidades desejadas pelos clientes de forma mais rápida que nunca. Também criamos novas formas para entregar e instalar as atualizações de recursos e as atualizações de qualidade que simplificam as implantações e gerenciamento existente, ampliando a base de funcionários que podem ficar atualizados com as mais recentes funcionalidades e experiências do Windows e um custo de propriedade menor. Portanto, implementamos novas opções de serviço - referidas como Branch atual (CB, Current Branch), Branch atual para negócios (CBB, Current Branch for Business) e Branch de serviços de longo prazo (LTSB, Long-Term Servicing Branch) - que oferecem soluções pragmáticas para manter mais dispositivos mais atualizados nos ambientes corporativos que era possível anteriormente.

A seguinte exibição da tabela descreve os vários branches de serviços e seus atributos chaves.

| Opção de serviço                  | Disponibilidade de novas atualizações de recursos para instalação     | Período mínimo do tempo de vida de manutenção | Principais benefícios                                                                              | Edições com suporte                                                                         |
|-----------------------------------|-----------------------------------------------------------|--------------------------------------|-------------------------------------------------------------------------------------------|--------------------------------------------------------------------------------------------|
| Ramificação Atual (CB)               | Imediatamente após a primeira publicação pela Microsoft            | Aproximadamente 4 meses               | Disponibiliza novos recursos para os usuários assim que possível                                 | Home, Pro, Education, Enterprise, Mobile, IoT Core, Windows10 IoT Core Pro (IoT Core Pro) |
| Branch Atual para Negócios (CBB) | Aproximadamente 4 meses após a primeira publicação pela Microsoft | Aproximadamente 8 meses               | Fornece tempo adicional para testar novas atualizações de recursos antes da implantação                   | Pro, Education, Enterprise, Mobile Enterprise, IoT Core Pro                                |
| Branch de Serviços de Longo Prazo (LTSB) | Imediatamente após a publicação pela Microsoft                  | 10 anos                             | Permite a implementação a longo prazo de versões selecionadas do Windows 10 em configurações com poucas alterações | Branch de Serviços de Longo Prazo Corporativo                                                                            |
 
Para obter mais informações, consulte [Opções de serviços do Windows10 para atualizações e upgrades](https://technet.microsoft.com/itpro/windows/manage/introduction-to-windows-10-servicing).

## Oferecendo suporte a aplicativos no Windows como serviço

A abordagem tradicional para dar suporte a aplicativos tem sido lançar uma nova versão do aplicativo em resposta a uma versão do Windows. Isso pressupõe que há alterações significativas no sistema operacional subjacente que podem causar uma regressão ao aplicativo. Esse modelo envolve um ciclo de desenvolvimento e validação dedicado que exige que nossos parceiros de ISV se alinhem com a regularidade de lançamentos do Windows.

No modelo do Windows como um serviço, a Microsoft está fazendo um compromisso para manter a compatibilidade do sistema operacional subjacente. Isso significa que a Microsoft fará um esforço concentrado para garantir que não há nenhuma alteração significativa que afeta negativamente o ecossistema do aplicativo. Nesse cenário, quando houver um lançamento de uma compilação do Windows, a maioria dos aplicativos (aqueles sem nenhuma dependência do kernel) continuarão a funcionar.

Tendo em vista essa alteração, a Microsoft recomenda que nossos parceiros de ISV dissociem seus lançamentos e suporte de aplicativos das compilações específicas do Windows. Nossos clientes mútuos são atendidos melhor por uma abordagem de ciclo de vida do aplicativo. Isso significa que, quando uma versão do aplicativo for lançada, ela terá suporte por um determinado período, independentemente do número de compilações do Windows lançadas nesse intervalo de tempo. O ISV faz um compromisso para oferecer suporte para essa versão específica do aplicativo enquanto ela tiver suporte no ciclo de vida. A Microsoft segue uma abordagem semelhante ao ciclo de vida do Windows que pode ser referenciado [aqui](http://go.microsoft.com/fwlink/?LinkID=780549).

Essa abordagem reduz a sobrecarga de manutenção de um agendamento de aplicativo que se alinha com os lançamentos do Windows. O parceiros ISV devem estar liberados para lançar recursos ou atualizações em seu próprio ritmo. Achamos que nossos parceiros podem manter sua base de clientes atualizada com as últimas atualizações do aplicativo independente de uma versão do Windows. Além disso, nossos clientes não precisam procurar uma instrução de suporte explícita sempre que uma compilação do Windows for lançada. Aqui está um exemplo de uma instrução de suporte que aborda como um aplicativo pode ter suporte em versões diferentes do sistema operacional:

| Exemplo de uma instrução de suporte de ciclo de vida do aplicativo                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                     |
|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| A Contoso é uma empresa de desenvolvimento de software e é a proprietária do popular aplicativo Mojave que é compartilhado principalmente no espaço corporativo. A Contoso libera sua próxima versão principal Mojave 14.0 e declara suporte base por um período de três anos a partir da data de lançamento. Durante o suporte base todas as atualizações e suporte são complementares para o produto licenciado. A Contoso também declara um suporte estendido adicional de dois anos em que os clientes podem comprar atualizações e suporte para um período de cortesia. Além da data de término do suporte estendido esta versão do produto não é mais suportada. Durante o período de suporte base que a Contoso dará suporte ao Mojave 14.0 em todas as compilações lançadas do Windows. A Contoso também lançará atualizações para o Mojave conforme necessário e independente das versões do produto do Windows. |
 
Nas seções a seguir, você encontrará informações adicionais sobre as etapas da Microsoft para manter a compatibilidade do sistema operacional subjacente. Você também encontrará orientações sobre as etapas que você pode tomar para ajudar a manter a compatibilidade do ecossistema do sistema operacional e aplicativo combinados. Há uma seção sobre como aproveitar as compilações de liberação de versões de pré-lançamento do Windows para detectar regressões aplicativo antes de uma compilação do Windows ser lançada. Por fim, nós descrevemos como usamos uma instrumentação e abordagem orientadas por telemetria para aumentar a qualidade das compilações do Windows. Nós recomendamos que as ISVs adotem uma abordagem semelhante em seu portfólio de aplicativos.

## Principais alterações desde o Windows7 para garantir a compatibilidade de aplicativo

Entendemos que compatibilidade é importante para os desenvolvedores. Os ISVs e desenvolvedores querem garantir que seus aplicativos serão executados como esperado em todas as versões compatíveis do sistema operacional Windows. Os consumidores e empresas têm um investimento chave aqui — eles querem garantir que os aplicativos pelos quais eles pagaram continuarão a funcionar. Nós sabemos que compatibilidade é o critério principal para decisões de compra. Aplicativos que são escritos com base em práticas recomendadas levarão a muito menos variação de código quando uma nova versão do Windows for lançada e reduzirá a fragmentação — esses aplicativos têm um investimento em engenharia reduzido para manter e um tempo mais rápido no mercado.

No cronograma do Windows7, compatibilidade era muito mais uma abordagem reativa. No Windows8, começamos a observar isso de forma diferente, trabalhando dentro do Windows para garantir que a compatibilidade foi projetada no lugar de ser considerada algo secundário. Windows10 é a versão mais compatível por design do sistema operacional até a data. Aqui estão algumas maneiras chave como conseguimos isso:
-   **Telemetria de aplicativos**: isso nos ajuda a compreender a popularidade do aplicativo no ecossistema do Windows para instruir o teste de compatibilidade.
-   **Parcerias ISV**: trabalhar diretamente com parceiros externos para fornecer a eles dados e ajudá-los a corrigir problemas que nossos usuários experimentam.
-   **Revisões de design e detecção de upstream**: uma parceria com equipes de recursos para reduzir o número de alterações significativas no Windows. Análise de compatibilidade é uma entrada pela qual as nossas equipes de recurso devem passar.
-   **Comunicação**: maior controle sobre alterações da API e melhor comunicação.
-   **Liberação de versões de pré-lançamento e loop de feedback**: os usuários do Windows insider recebem as compilações das versões de pré-lançamento liberadas que ajudam a melhorar a nossa capacidade de localizar problemas de compatibilidade antes de uma compilação final ser lançada para os clientes. Esse processo de feedback não só expõe os bugs, mas garante que entregamos os recursos que nossos usuários querem.

## Práticas recomendadas para compatibilidade de aplicativos

A Microsoft usa diagnósticos e dados de uso para identificar e solucionar problemas, melhorar nossos produtos e serviços e oferecer aos nossos usuários experiências personalizadas. Os dados de uso que coletamos também se estendem aos aplicativos que são executados nos computadores do ecossistema do Windows. Com base no que nossos clientes usam, criamos nossa lista para testar esses aplicativos, dispositivos e drivers em relação às novas versões do sistema operacional do Windows. O Windows10 foi a versão mais compatível do Windows até o momento, com mais de 90% compatibilidade contra milhares de aplicativos populares. A equipe de compatibilidade do Windows normalmente entra em contato com nossos parceiros de ISV para fornecer comentários, caso ocorram problemas, para que possamos compartilhar as soluções. Em condições ideais, gostaríamos que nossos clientes comuns conseguissem atualizar o Windows perfeitamente e sem perder a funcionalidade em seu sistema operacional ou dos aplicativos que eles dependem para a sua produtividade ou diversão.

As seções a seguir contêm algumas práticas recomendadas que a Microsoft recomenda para que você pode garantir que seus aplicativos sejam compatíveis com o Windows10.

### Verificação de versão do Windows

A versão do sistema operacional foi incrementado com o Windows10. Isso significa que o número de versão interno foi alterado para 10.0. Como sempre, não medimos esforços para manter a compatibilidade de aplicativos e dispositivos após uma mudança de versão do sistema operacional. Para a maioria das categorias de aplicativos (sem nenhuma dependência de kernel) a mudança não afetará negativamente a funcionalidade do aplicativo e os aplicativos existentes continuarão a funcionar bem no Windows10.

A manifestação dessa alteração é específica do aplicativo. Isso significa que qualquer aplicativo que verifica especificamente para a versão do sistema operacional obterá um número de versão superior, que pode levar a uma ou mais das seguintes situações:
-   Os instaladores de aplicativos podem não ser capazes de instalar o aplicativo e os aplicativos talvez não consigam iniciar.
-   Os aplicativos podem ficar instáveis ou falharem.
-   Os aplicativos podem gerar as mensagens de erro, mas continuam a funcionar adequadamente.

Alguns aplicativos realizam uma verificação da versão e simplesmente passam um aviso para os usuários. No entanto, há aplicativos que são associados rigidamente a uma verificação de versão (nos drivers, ou no modo de kernel para evitar a detecção). Nesses casos, o aplicativo falhará se for encontrada uma versão incorreta. Em vez de uma verificação de versão, nós recomendamos um dos procedimentos a seguir:
-   Se o aplicativo é dependente da funcionalidade de uma API específica, garanta que você selecione a versão correta da API.
-   Certifique-se de detectar a alteração via APISet ou outra API pública e não use a versão como um proxy para algum recurso ou correção. Se houverem alterações significativas e uma seleção adequada não for exposta, então isto é um bug.
-   Certifique-se de que o aplicativo NÃO verifica a versão de maneira indefinida, como por meio do registro, versões do arquivo, deslocamentos, modo de kernel, drivers ou outros meios. Se o aplicativo absolutamente precisa verificar a versão, use as APIs GetVersion, que devem retornar o número principal, secundário e de compilação da versão.
-   Se você estiver usando a API [GetVersion](http://go.microsoft.com/fwlink/?LinkID=780555), lembre-se de que o comportamento dessa API mudou desde o Windows8.1.

Se você possui aplicativos como aplicativos antimalware ou firewall, você deve trabalhar por meio dos canais de feedback usuais e por meio do programa Windows Insider.

### APIs não documentadas

Seus aplicativos não devem chamar APIs do Windows não documentadas ou depender da exportação de um arquivo específico do Windows ou de chaves do registro. Isso pode levar a perda da funcionalidade, perda de dados e possíveis problemas de segurança. Se a funcionalidade que o seu aplicativo requer não estiver disponível, isso é uma oportunidade de fornecer comentários por meio dos canais de comentários usuais e por meio do programa Windows Insider.

### Desenvolver aplicativos Centennial e na Plataforma Universal do Windows (UWP)

Incentivamos todos os ISVs de aplicativos para Win32 a desenvolver na [Plataforma Universal do Windows(UWP)](http://go.microsoft.com/fwlink/?LinkID=780560) e, especificamente, aplicativos [Centennial](http://go.microsoft.com/fwlink/?LinkID=780562) mais adiante. Há excelentes benefícios para desenvolver esses pacotes de aplicativos em lugar de usar os instaladores Win32 tradicionais. Os aplicativos UWP também têm suporte na [Windows Store](http://go.microsoft.com/fwlink/?LinkID=780563), portanto, é mais fácil para você atualizar os usuários para uma versão consistente automaticamente, reduzindo os custos de suporte.

Se os seus tipos de aplicativos Win32 não funcionam com o modelo do Centennial, é altamente recomendável que você use o instalador correto e certifique-se de que isto é completamente testado. Um instalador é a primeira experiência do seu cliente ou usuário com o seu aplicativo, portanto certifique-se de que ele funciona bem. Geralmente, isto não funciona bem ou ele não foi totalmente testado para todos os cenários. O [Kit de Certificação de Aplicativos Windows](http://go.microsoft.com/fwlink/?LinkID=780565) pode ajudá-lo testar a instalação e desinstalação do seu aplicativo Win32 e ajudá-lo a identificar uso de APIs não documentadas, bem como outros problemas básicos de práticas recomendadas relacionadas ao desempenho, antes dos usuários o utilizarem.

**Práticas recomendadas:**
-   Use instaladores que funcionam para versões de 32 bits e 64 bits do Windows.
-   Projete seus instaladores para execução em vários cenários (nível de usuário ou máquina).
-   Mantenha todos os recursos redistribuíveis do Windows no pacote original – se ele for remontado, é possível que isso causará a perda do instalador.
-   Programe o tempo de desenvolvimento para seus instaladores, eles geralmente são ignorados como um produto durante o ciclo de vida de desenvolvimento de software.

## Estratégias de teste otimizadas e liberação de versões de pré-lançamento

A liberação de versões de pré-lançamento do sistema operacional Windows refere-se às compilações provisórias disponíveis para os participantes do programa Windows Insiders antes de uma compilação final ser lançada para a população geral. Quanto mais participantes do programa Insiders recebam essa versão de pré-lançamento dessas compilações temporárias, mais feedback recebemos sobre a qualidade da compilação, compatibilidade, etc. e isso ajuda a melhorar a qualidade das compilações finais. Você pode participar deste programa de liberação de versões de pré-lançamento para garantir que seus aplicativos funcionem conforme o esperado nas compilações iterativas do sistema operacional. Também recomendamos que você forneça comentários sobre como essas compilações de versões de pré-lançamento estão trabalhando para você, problemas que ocorrerem e assim por diante.

Se seu aplicativo está na Loja, você pode liberar a versão de pré-lançamento do seu aplicativo através da Loja, o que significa que o seu aplicativo estará disponível para os nossos participantes do Programa Windows Insider para instalação. Os usuários podem instalar seu aplicativo e você pode receber comentários preliminar sobre o seu aplicativo antes de lançá-lo para a população geral. As seções a seguir descrevem as etapas para testar seus aplicativos com base em compilações de versões de pré-lançamento do Windows.

### Etapa 1: torne-se um usuário do Windows Insider e participe da liberação de versões de pré-lançamento
Como um [participante do programa Windows Insider,](http://go.microsoft.com/fwlink/p/?LinkId=521639) você pode ajudar a moldar o futuro do Windows — seus comentários nos ajudarão a melhorar os recursos e funcionalidades na plataforma. Essa é uma comunidade vibrante onde você pode se conectar com outros entusiastas, participar de fóruns, trocar conselhos e saber mais sobre os futuros eventos apenas para o programa Insider.

Assim que você tenha acesso para as versões prévias do Windows10, Windows10 Mobile e o emulador e o SDK do Windows mais recentes, você terá todas as ferramentas à sua disposição para desenvolver ótimos aplicativos e explorar o que há de novo na Plataforma Universal do Windows e da Windows Store.

Isso também é uma grande oportunidade para criar ótimo hardware, com versões prévias dos kits de desenvolvimento de hardware para que você pode desenvolver drivers universais para Windows. O IoT Core Insider Preview também está disponível em placas de desenvolvimento para IoT compatíveis, para que você possa criar incríveis soluções conectadas usando a Plataforma Universal do Windows.

Antes de se tornar um participante do programa Windows Insider, observe que a participação destina-se para os usuários que:
-   Quiser experimentar um software que ainda está em desenvolvimento.
-   Deseja compartilhar comentários sobre o software e a plataforma.
-   Não se importar com grandes quantidades de atualizações nem com um design de interface do usuário que mude consideravelmente com o passar do tempo.
-   Conhecer realmente um computador e se sentir confortável em solucionar problemas, fazer backup de dados, formatar um disco rígido, instalar um sistema operacional do zero ou restaurar o sistema antigo se for necessário.
-   Souber o que é um arquivo ISO e como usá-lo.
-   Não instalá-lo no seu computador ou dispositivo do dia a dia.

### Etapa 2: Testar seus cenários

Depois de ter atualizado para uma compilação liberada de versão de pré-lançamento, a seguir estão alguns casos de teste de amostra para ajudá-lo a começar a usar testes e reunir comentários. Para a maioria desses testes, certifique-se de que abrange os sistemas x86 e AMD64.
**Teste de instalação limpa:** em uma instalação limpa do Windows10, certifique-se de que seu aplicativo é totalmente funcional. Se o seu aplicativo falhar neste teste e no teste de atualização, é provável que o problema é causado pelas alterações do sistema operacional subjacente ou falhas no aplicativo. Se depois de investigação, o primeiro for o caso, certifique-se de usar o Programa Windows Insider para fornecer feedback e compartilhar soluções.

**Teste de atualização:** verifique se o seu aplicativo funciona após a atualização de uma versão de nível inferior do Windows (isto é, Windows7 ou Windows8.1) para Windows10. Seu aplicativo não deve provocar reversões durante a atualização e deve continuar a funcionar como esperado após a atualização — isso é essencial para obter uma experiência perfeita de atualização.

**Teste de reinstalação:** certifique-se de que a funcionalidade do aplicativo possa ser restaurada por meio da reinstalação do seu aplicativo após a atualização do computador para o Windows10 de um sistema operacional de nível inferior. Se seu aplicativo não passar no teste de atualização e você não for capaz de reduzir a causa desses problemas, é possível que uma reinstalação possa restaurar a perda de funcionalidade. Não passar em um teste de reinstalação indica que partes do aplicativo podem não terem sido migradas para o Windows10.

**Teste de recursos do dispositivo\\sistema operacional:** certifique-se de que seu aplicativo funciona como esperado se seu aplicativo depende da funcionalidade específica no sistema operacional. Áreas comuns para testes incluem o seguinte, geralmente em relação a uma seleção de modelos de computador comumente usados para garantir uma cobertura:
-   Áudio
-   Funcionalidade do dispositivo USB (teclado, mouse, cartão de memória, disco rígido externo e assim por diante)
-   Bluetooth
-   Gráficos\\tela (com vários monitores, projeção, rotação da tela e assim por diante)
-   Tela touch (orientação, teclado virtual, caneta, gestos e assim por diante)
-   Touchpad (botões esquerdo\\direito, toque, rolagem e assim por diante)
-   Caneta (toque simples\\duplo, pressionar, segurar, borracha e assim por diante)
-   Impressão\\Scanner
-   Sensores (acelerômetro, fusão e assim por diante)
-   Câmera

### Etapa 3: fornecer comentários

Conte como seu aplicativo está se comportando em relação às compilações de versões de pré-lançamento liberadas. Na medida que você descobrir problemas com seu aplicativo durante o teste, registre os bugs através do portal do parceiro se você tem acesso, ou por meio do seu representante da Microsoft. Recomendamos essas informações para que possamos construir uma experiência de qualidade para os nossos usuários juntos.

### Etapa 4: Registre-se no Windows10
O site [Pronto para o Windows10](http://go.microsoft.com/fwlink/?LinkID=780580) é um diretório de software que suporta o Windows 10. Destina-se a administradores de TI em empresas e organizações em todo o mundo que estão considerando o Windows 10 para suas implantações. Os administradores de TI podem verificar o site para ver se o software implantado em sua empresa é suportado no Windows10.

## Tópicos relacionados
[Opções de serviços do Windows10 para atualizações e upgrade](https://technet.microsoft.com/itpro/windows/manage/introduction-to-windows-10-servicing)



<!--HONumber=Aug16_HO5-->

