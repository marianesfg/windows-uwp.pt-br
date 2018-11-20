---
author: joannaleecy
title: Usando serviços de nuvem para jogos UWP
description: Saiba mais sobre a implementação de nuvem como um back-end para seus jogos UWP.
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
ms.author: joanlee
ms.date: 03/27/2018
ms.topic: article
keywords: windows 10, uwp, jogos, serviços de nuvem
ms.localizationpriority: medium
ms.openlocfilehash: 5d15d3e6b6beb773a8d606db7a5d8a17544270be
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7422476"
---
#  <a name="using-cloud-services-for-uwp-games"></a>Usando serviços de nuvem para jogos UWP

A UWP (Plataforma Universal do Windows) no Windows 10 oferece um conjunto de APIs que podem ser usadas para desenvolver jogos entre dispositivos da Microsoft. Ao desenvolver jogos para diferentes dispositivos e plataformas, você pode usar um back-end de nuvem para ajudar a escalonar seu jogo de acordo com a demanda.

Se estiver procurando uma solução de back-end na nuvem completa para o seu jogo, consulte [Software como serviço para back-end de jogo](#software-as-a-service-for-game-backend).

##  <a name="what-is-cloud-computing"></a>O que é computação em nuvem?

A computação em nuvem utiliza recursos de TI e aplicativos sob demanda pela internet para armazenar e processar dados para seus dispositivos. O termo _nuvem_ é uma metáfora para a disponibilidade de inúmeros recursos externamente (recursos não locais) que você pode acessar de locais não específicos.
O princípio da computação em nuvem oferece uma nova forma na qual os recursos e o software podem ser consumidos. Os usuários não precisam mais pagar pelos recursos ou produtos completos antecipadamente, mas, em vez disso, são capazes de consumir a plataforma, o software e os recursos como um serviço. Os provedores de nuvem geralmente cobram seus clientes de acordo com as ofertas de planos de uso ou serviços.

##  <a name="why-use-cloud-services"></a>Por que usar serviços de nuvem?

Uma vantagem de usar os serviços de nuvem para jogos é que você não precisa investir em servidores de hardware físico antecipadamente; precisa apenas pagar de acordo com os planos de uso ou serviços em uma etapa posterior. É uma maneira de ajudar a gerenciar os riscos envolvidos no desenvolvimento de um novo título de jogo. 

Outra vantagem é que seu jogo pode explorar inúmeros recursos de nuvem para obter escalabilidade (gerenciar efetivamente quaisquer picos repentinos no número de jogadores simultâneos, intensos cálculos de jogo em tempo real ou requisitos de dados). Isso mantém o desempenho do seu jogo estável o tempo todo. Além disso, os recursos de nuvem podem ser acessados de qualquer dispositivo em execução em qualquer plataforma em qualquer lugar do mundo, o que significa que você é capaz de levar seu jogo a todos globalmente.

É importante oferecer uma incrível experiência de jogo aos seus jogadores. Como os servidores de jogos executados na nuvem são independentes de atualizações no cliente, eles podem oferecer a você um ambiente mais seguro e controlado para o jogo em geral.   Você também pode obter consistência de jogo pela nuvem nunca confiando no cliente e tendo a lógica do jogo no servidor. As conexões serviço a serviço também podem ser configuradas para permitir uma experiência de jogo mais integrada; exemplos incluem a vinculação de compras no jogo a vários métodos de pagamento, a ponte para diferentes de redes de jogos e o compartilhamento de atualizações no jogo em portais populares de redes sociais, como o Facebook e o Twitter. 

Você também pode usar servidores de nuvem dedicados para criar um grande mundo de jogo persistente, criar uma comunidade de jogadores, coletar e analisar dados de jogadores ao longo do tempo para melhorar a experiência de jogo e otimizar o modelo de design de monetização do seu jogo.

Além disso, os jogos que exigem funcionalidades de intenso gerenciamento de dados do jogo, como jogos sociais com mecânica assíncrona de vários jogadores, podem ser implementados com serviços de nuvem.

##  <a name="how-game-companies-use-the-cloud-technology"></a>Como as empresas de jogos usam a tecnologia de nuvem

Saiba como outros desenvolvedores implementaram soluções de nuvem em seus jogos.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header" align="left">
        <th>Desenvolvedor</th>
        <th>Descrição</th>
        <th>Principais cenários de jogo</th>
        <th>Saiba mais</th>
    </tr>
    <tr>
        <td><a href="https://www.tencent.com">Tencent Games</a></td>
        <td>A <b>Tencent Games</b> desenvolveu uma solução inovadora com o Azure Service Fabric que permite que os jogos de computador tradicionais sejam fornecidos como um serviço. Sua solução de jogo na nuvem um modelo "cliente fino + nuvem avançada" que executa cargas de trabalho como microsserviços no back-end.</td>
        <td>
            <ul>
                <li>Os jogos de computador tradicionais são fornecidos como jogos na nuvem para usuários em todo o mundo <li>Processo de entrega de jogo otimizada <li>Funcionalidades de jogo isoladas como microsserviços para reduzir a complexidade, diminuir a repetição de cargas de trabalho decorrente de dependências, e capacidade de atualizar os novos recursos de forma independente <li>Downloads de pacote de instalação pequeno para reproduzir o conteúdo de jogo mais recente (tamanho do pacote reduzido de GB para MB) <li>Menor custo de manutenção </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://customers.microsoft.com/story/tencent-telecommunications-azure-service-fabric-windows-server-en">A Tencent Games e a Microsoft criaram a solução de jogo na nuvem</a>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=38m33s">Criando jogos com o Service Fabric: detalhes sobre a implementação da Tencent (vídeo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="https://www.halowaypoint.com/">343 Industries</a></td>
        <td>O <b>Halo 5: Guardians</b> implementou o <a href="https://www.halowaypoint.com/spartan-companies">Halo: Spartan Companies</a> como sua plataforma de jogo social usando o Azure Cosmos DB (via API DocumentDB), que foi selecionado por sua velocidade e flexibilidade devido às funcionalidades de indexação automática.</td>
        <td>
            <ul>
                <li>Camada de dados escalonável para lidar com o gerenciamento/criação de grupos para jogos multijogadores <li>Integração de jogo e mídia social <li>Consultas em tempo real de dados por meio de vários atributos <li>Sincronização de conquistas e estatísticas de jogo </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/">Jogo social implementado com o Azure Cosmos DB (via API DocumentDB)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://web.ageofascent.com/">Illyriad Games</a></td>
        <td>A Illyriad Games criou o <b>Age of Ascent</b>, um jogo épico de espaço 3D MMO (multijogador massivo online) que pode ser executado em dispositivos com navegadores modernos. Portanto, esse jogo pode ser executado em computadores, notebooks, telefones celulares e outros dispositivos móveis sem plug-ins. O jogo usa ASP.NET Core, HTML5, WebGL e Azure.</td>
        <td>
            <ul>
                <li>Jogo baseado em navegador de plataforma cruzada <li>Grande mundo único, aberto e persistente <li>Manipula intensos cálculos de jogo em tempo real <li>É dimensionado com base no número de jogadores </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Shows/Cloud+Cover/Episode-228-Building-Games-with-Service-Fabric#time=06m52s">Criando jogos com o Service Fabric: jogo Age of Ascent MMO (vídeo)</a>
                <li><a href="https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s">Gerenciar componentes de jogo como microsserviços usando o Azure Service Fabric (vídeo)</a> 
                <li><a href="https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET">Entrevista com desenvolvedores do Age of Ascent (vídeo)</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.nextgames.com/">Next Games</a></td>
        <td>A Next Games é a criadora do videogame <b>The Walking Dead: No Man's Land</b>, que se baseia na série original da AMC. O jogo Walking Dead usou o Azure como back-end. Ele teve 1.000.000 de downloads no fim de semana de lançamento e, durante a primeira semana, o jogo ficou em primeiro lugar em aplicativos gratuitos para iPhone e iPad na App Store dos EUA, primeiro lugar em aplicativos gratuitos em 12 países e regiões e primeiro lugar em jogos gratuitos em 13 países e regiões.
        </td>
        <td>
            <ul>
                <li>Plataforma cruzada <li>Multijogador baseado em turnos <li>Desempenho de escala elástica <li>Proteção contra fraude do jogador <li>Entrega dinâmica de conteúdo </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://azure.microsoft.com/blog/how-we-built-it-next-games-global-online-gaming-platform-on-azure/">Como criamos: plataforma de jogos online global Next Games no Azure (blog com vídeo)</a>
                <li><a href="https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/">O Walking Dead usa o Azure Cosmos DB (via API DocumentDB) para um ciclo de desenvolvimento mais rápido e um jogo mais envolvente</a>
            </ul>
        </td>
    </tr>
    <tr>
        <td><a href="http://www.crimecoast.com/">Pixel Squad</a></td>
        <td>A Pixel Squad desenvolveu <b>Crime Coast</b> usando o mecanismo de jogo Unity e o Azure. <b>Crime Coast</b> é um jogo de estratégia social disponível nas plataformas Android, iOS e Windows. O Armazenamento de Blobs do Azure, o Cache Redis do Azure Gerenciado, uma matriz VMs IIS de carga balanceada e o Hub de Notificação da Microsoft foram usados no jogo. Saiba como eles gerenciaram o escalonamento e lidaram com o inesperado número de 5.000 jogadores simultâneos.
        </td>
        <td>
            <ul>
                <li>Plataforma cruzada <li>Jogo online multijogador <li>Escala com o número de jogadores </ul>
        </td>
        <td>
            <ul>
                <li><a href="https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te">Como o jogo MMO Crime Coast usou os Serviços de Nuvem do Azure</a>
            </ul>
        </td>
    </tr> 
</table>

    
### <a name="other-links"></a>Outros links

* [Hitman e Azure: criar recursos de jogos como Elusive Target que só são possíveis por meio da nuvem](https://channel9.msdn.com/Series/Hitman)
* [Azure como o molho secreto de Hitcents, Game Troopers e InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Startups de jogos no programa Bizspark usando o Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## <a name="how-to-design-your-cloud-backend"></a>Como projetar seu back-end de nuvem

Enquanto produtores e designers de jogos discutem quais recursos e funcionalidades são necessários no jogo, é bom começar a considerar como você deseja projetar sua infraestrutura de jogo. O Azure pode ser usado como back-end de jogo quando você quer desenvolver jogos para vários dispositivos e em diferentes plataformas importantes.

### <a name="understanding-iaas-paas-or-saas"></a>Noções básicas de IaaS, PaaS ou SaaS

Primeiro, você precisa pensar no nível de serviço que é mais adequado para seu jogo. Saber as diferenças dos três serviços a seguir pode ajudá-lo a determinar a abordagem que você deseja usar na criação do seu back-end.

* [IaaS (infraestrutura como serviço)](https://azure.microsoft.com/overview/what-is-iaas/)

    A IaaS (infraestrutura como serviço) é uma infraestrutura de computação instantânea, provisionada e gerenciada pela Internet. Imagine ter a possibilidade de muitos computadores prontamente disponíveis para a rápida escala horizontal ou vertical, dependendo da demanda. A IaaS ajuda você a evitar o custo e a complexidade de comprar e gerenciar seus próprios servidores físicos e outra infraestrutura de datacenter.

* [PaaS (plataforma como serviço)](https://azure.microsoft.com/overview/what-is-paas/)

    A PaaS (plataforma como serviço) é como a IaaS, mas também inclui o gerenciamento de infraestrutura como servidores, armazenamento e rede. Portanto, além de não comprar servidores físicos e infraestrutura de datacenter, você também não precisa comprar e gerenciar licenças de software, infraestrutura de aplicativos subjacente, middleware, ferramentas de desenvolvimento ou outros recursos.

* [SaaS (software como serviço)](https://azure.microsoft.com/overview/what-is-saas/)

    O SaaS (software como serviço) permite que os usuários se conectem a apps baseados na nuvem pela Internet e os utilizem. Ele fornece uma solução de software completa que você comprar de um provedor de serviços de nuvem conforme o uso.  Exemplos comuns são ferramentas de email, calendário e ferramentas do Office (como o Microsoft Office 365). Você aluga um app para uso de sua organização e os usuários se conectam a ele pela Internet, geralmente com um navegador da Web. Toda a infraestrutura subjacente, middleware, software do app e dados do app estão localizados no data center do provedor de serviços. O provedor de serviços gerencia o hardware e o software, e com o contrato de serviço apropriado, garantirá a disponibilidade e a segurança do jogo e de seus dados. O SaaS permite que sua organização comece a funcionar rapidamente com um app a um custo inicial mínimo.


### <a name="design-your-game-infrastructure-using-azure"></a>Criar sua infraestrutura de jogo usando o Azure

A seguir estão algumas maneiras de usar as ofertas de nuvem do Azure para um jogo. O Azure funciona com Windows, Linux e tecnologias conhecidas de software livre, como Ruby, Python, Java e PHP. Para saber mais, consulte [Azure para jogos](https://azure.microsoft.com/solutions/gaming/).

| Requisitos                 | Cenários de atividade                            | Oferta de produto                      | Funcionalidades do produto                                    |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hospedar seu domínio na nuvem     | Responder a consultas do DNS com eficiência            | [DNS do Azure](https://azure.microsoft.com/services/dns/) | Hospedar seu domínio com alto desempenho e disponibilidade  |
| Entrada, verificação de identidade      | O jogador entra e sua identidade é autenticada  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Logon único em qualquer aplicativo da Web local e na nuvem com autenticação multifator            | 
| Jogo usando o modelo IaaS (infraestrutura como serviço)      | O jogo é hospedado em máquinas virtuais na nuvem       | [VMs do Azure](https://azure.microsoft.com/services/virtual-machines/) | Escala de uma a milhares de instâncias de máquina virtual como servidores de jogo com redes virtuais internas e balanceamento de carga; consistência híbrida com sistemas locais           |
| Jogos na Web ou em dispositivos móveis usando PaaS (plataforma como serviço)            | O jogo é hospedado em uma plataforma gerenciada                | [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) | O PaaS para sites ou jogos em dispositivos móveis (o que significa VMs do Azure com middleware/ferramentas de desenvolvimento/BI/gerenciamento de banco de dados)   |
| Jogo de nuvem de n camadas altamente disponível e escalonável com maior controle do SO (PaaS)        | O jogo é hospedado em uma plataforma gerenciada                | [Serviço de Nuvem do Azure](https://azure.microsoft.com/services/app-service/) | O PaaS foi projetado para oferecer suporte a aplicativos escalonáveis, confiáveis e baratos   |
| Balanceamento de carga entre regiões para melhor desempenho e a disponibilidade | Encaminha as solicitações de jogo recebidas. Pode atuar como primeiro nível do balanceamento de carga.       | [Gerenciador de Tráfego do Azure](https://azure.microsoft.com/en-us/services/traffic-manager/) | Oferece várias opções de failover automático e capacidade de distribuir o tráfego igualmente ou com valores ponderados. Combina perfeitamente sistemas de nuvem e locais. |
| Armazenamento em nuvem para dados de jogo       | Os dados de jogo mais recentes são armazenados na nuvem e enviados a dispositivos cliente | [Armazenamento de Blobs do Azure](https://azure.microsoft.com/services/storage/blobs/)| Nenhuma restrição nos tipos de arquivo que podem ser armazenados; armazenamento de objetos para grandes volumes de dados não estruturados, como imagens, áudio, vídeo e muito mais.  |
| Tabelas de armazenamento de dados temporários| Transações do jogo (mudanças em estados do jogo) são armazenadas temporariamente em tabelas | [Armazenamento de Tabelas do Azure](https://azure.microsoft.com/services/storage/tables/)| Os dados do jogo podem ser armazenados em um esquema flexível de acordo com as necessidades do jogo |
| Transações/solicitações do jogo em fila| As transações do jogo são processadas na forma de uma fila | [Armazenamento de Filas do Azure](https://azure.microsoft.com/services/storage/queues/)| As filas absorvem aumentos súbitos de tráfego e podem impedir que os servidores sejam sobrecarregados por uma inundação repentina de solicitações durante o jogo   |
| Banco de dados de jogo relacional escalonável| Armazenamento estruturado de dados relacionais como transações no jogo no banco de dados | [Banco de Dados do SQL Azure](https://azure.microsoft.com/services/sql-database/)| Banco de dados SQL como serviço ([Comparação com o SQL em uma VM](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Banco de dados de jogos de baixa latência distribuído e escalonável| Leitura, gravação e consulta rápidas dos dados de jogo e de jogadores com flexibilidade de esquema | [Azure Cosmos DB](https://azure.microsoft.com/services/cosmos-db/)| Banco de dados de documentos NoSQL de baixa latência como um serviço   |
| Usar o próprio datacenter com serviços do Azure | O jogo é recuperado de seu próprio datacenter e enviado aos dispositivos cliente | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permite que sua organização forneça serviços do Azure no próprio datacenter para ajudá-lo a obter mais  |
| Transferência de blocos de dados grandes| Arquivos grandes, como imagem, áudio e vídeo de jogos, podem ser enviados aos usuários a partir do local POP da CDN (Rede de Distribuição de Conteúdo) mais próxima com a CDN do Azure    | [Rede de Distribuição de Conteúdo do Azure](https://azure.microsoft.com/services/cdn/) | Criada em uma topologia de rede moderna de nós centralizados grandes, a CDN do Azure manipula picos súbitos de tráfego e cargas pesadas para aumentar drasticamente a velocidade e a disponibilidade, o que resulta em melhorias significativas na experiência do usuário  |
| Baixa latência               | Executa o armazenamento em cache para criar jogos rápidos e escalonáveis com mais controle e isolamento garantido de dados; também pode ser usada para aprimorar o recurso de correspondência do jogo. | [Cache Redis do Azure](https://azure.microsoft.com/services/cache/) | Acesso a dados de baixa latência e alta taxa de transferência para ativar aplicativos do Azure rápidos e escalonáveis.  |
| Alta escalabilidade, baixa latência | Manipula flutuações no número de usuários do jogo com leituras e gravações de baixa latência | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Capacidade de ativar os cenários mais complexos de baixa latência com uso intenso de dados e escalonar de forma confiável para manipular mais usuários por vez. O Service Fabric permite que você crie jogos sem precisar criar um armazenamento ou cache separado, conforme necessário para aplicativos sem estado |
| Capacidade de coletar milhões de eventos por segundo de dispositivos                         | Log de milhões de eventos por segundo de dispositivos | [Hubs de Eventos do Azure](https://azure.microsoft.com/services/event-hubs/) | Ingestão de telemetria em escala de nuvem de jogos, sites, aplicativos e dispositivos  |
| Processamento de dados de jogos em tempo real  | Executar a análise de dados de jogadores em tempo real para melhorar o jogo| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Processamento de fluxo em tempo real na nuvem  |
| Desenvolver jogo preditivo         | Criar um jogo dinâmico personalizado com base nos dados do jogador  | [Aprendizado de Máquina do Azure](https://azure.microsoft.com/services/machine-learning/) | Um serviço de nuvem totalmente gerenciado que permite que você compile, implante e compartilhe soluções de análise preditiva  |
| Coletar e analisar dados de jogos| Intenso processamento de dados paralelo de bancos de dados relacionais e não relacionais | [Data Warehouse do Azure](https://azure.microsoft.com/services/sql-data-warehouse/)| Data warehouse elástico como um serviço com recursos de classe empresarial   |
| Envolver os usuários para aumentar o uso e a retenção| Enviar notificações por push direcionadas a qualquer plataforma a partir de qualquer back-end para gerar interesse e incentivar ações específicas do jogo | [Hubs de Notificação do Azure](https://azure.microsoft.com/services/notification-hubs/)| Envio de difusão sem solicitação rápido para atingir milhões de dispositivos móveis nas principais plataformas &mdash; iOS, Android, Windows, Kindle, Baidu. Seu jogo pode ser hospedado em qualquer back-end &mdash; nuvem ou local.|
| Streaming de conteúdo de mídia para público nacional e internacional com proteção de conteúdo| Trailers de jogo e clipes cinematográficos com qualidade de transmissão podem assistidos de todos os dispositivos| [Serviços de Mídia do Azure](https://azure.microsoft.com/services/media-services/)| Streaming de vídeo ao vivo e sob demanda com recursos de rede de distribuição de conteúdo integrados. Use um jogador para todas as suas necessidades de reprodução, inclui criptografia e proteção de conteúdo.| 
| Desenvolvimento, distribuição e teste beta de seus apps móveis | Testar e distribuir o app móvel. Desempenho do app e gerenciamento da experiência do usuário. | [HockeyApp](https://azure.microsoft.com/services/hockeyapp/)| Integra métricas de usuário e relatórios de falhas a uma plataforma de distribuição de app e feedback do usuário. Oferece suporte a apps Android, Cordova, iOS, OS X, Unity, Windows e Xamarin. Além disso, considere o [Visual Studio Mobile Center](https://www.visualstudio.com/vs/mobile-center/) &mdash; controle de missão para apps que combina análise avançada, relatório de falhas, notificações por push, distribuição de apps e muito mais. |
| Criar campanhas de marketing para aumentar o uso e a retenção  | Enviar notificações por push a jogadores direcionados para gerar interesse e incentivar ações específicas do jogo de acordo com a análise de dados | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) – será retirado em março de 2018 e só estará disponível para os clientes existentes no momento |  Aumentar o tempo de jogo e a retenção de usuários em todas as principais plataformas — iOS, Android, Windows, Windows Phone |


##  <a name="startup-and-developer-resources"></a>Recursos para startups e desenvolvedores

* [Microsoft para startups](https://startups.microsoft.com)

    O Microsoft para startups fornece benefícios de produto, técnicos e comerciais que ajudam a acelerar o crescimento de startups. Um dos benefícios é a obtenção de uma conta gratuita do Azure. Você tem um crédito de US$ $200 para explorar serviços por 30 dias, 12 meses de serviços populares gratuitos e sempre 25+ serviços gratuitos. Para obter mais informações, consulte [Dê vida às suas ideias de startup com uma conta gratuita do Azure](https://azure.microsoft.com/free/startups/).
    
* [Programas de desenvolvedor](e2e.md#developer-programs)

    A Microsoft oferece vários programas de desenvolvedor como [ID@Xbox](http://www.xbox.com/Developers/id) e [Programa de Criadores do Xbox Live](https://developer.microsoft.com/games/xbox/xboxlive/creator) para ajudar você a desenvolver e publicar jogos.

## <a name="learning-resources"></a>Recursos de aprendizagem

* //build 2016: [CodeLabs &mdash; Usar o back-end do Serviço de Aplicativo do Microsoft Azure e do Microsoft SQL Azure para salvar a pontuação do jogo no Unity](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* Build 2017: [experiências de jogo em nível usando o Microsoft Azure: lições aprendidas com títulos como Halo, Hitman e WalkingDead (vídeo)](https://channel9.msdn.com/Events/Build/2017/P4062)
* Conjunto reutilizável de blocos de construção, projetos, serviços e práticas recomendadas projetados para dar suporte a cargas de trabalho de jogos comuns usando o Azure no GitHub: [Blocos de construção para jogos no Azure](https://github.com/MicrosoftDX/nether)
* [Serviços de jogos no Azure (vídeos)](https://channel9.msdn.com/Series/Gaming-Services-on-Azure)

## <a name="tools-and-other-useful-links"></a>Ferramentas e outros links úteis

* [Fóruns do MSDN &mdash; plataforma Azure](https://social.msdn.microsoft.com/Forums/azure/home?category=windowsazureplatform)
* [Ferramenta de teste de carga baseado na nuvem](https://www.visualstudio.com/team-services/cloud-load-testing/)
* [SDKs e ferramentas de linha de comando](https://azure.microsoft.com/downloads/)
    
## <a name="software-as-a-service-for-game-backend"></a>Software como serviço para back-end de jogo

Atualmente, a [Playfab](https://playfab.com/) tem mais de 1.200 jogos ao vivo com 80 milhões de players ativos mensais. É uma plataforma de back-end completa que inclui as melhores LiveOps com controle em tempo real. 

É possível integrar essa solução ao seu dispositivo móvel, computador ou jogos de console usando SDKs. Há SDKs disponíveis para todos os mecanismos de jogos e plataformas populares, incluindo Android, iOS, Unreal, Unity e Windows. Para começar, consulte a [Documentation](https://api.playfab.com/).

Oferece serviços de jogos como autenticação, gerenciamento de dados de player, multijogador e análise em tempo real para ajudar a ampliar a base de usuários do seu jogo. Aproveite o poder do pipeline de dados em tempo real e as LiveOps para atrair os usuários com os itens personalizados no jogo, eventos e promoções. Você também pode realizar testes A/B, gerar relatórios, enviar notificações por push e muito mais. 

Estamos inovando e adicionando novos recursos constantemente. Para obter mais informações, consulte [Features](https://playfab.com/features/) e, para obter informações de preço, consulte [Simple pricing that scales with you](https://playfab.com/pricing/).

## <a name="related-links"></a>Links relacionados

* [Guia de desenvolvimento de jogos do Windows 10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Azure para jogos](https://azure.microsoft.com/solutions/gaming/)
* [Playfab](https://playfab.com/)
* [Microsoft para Startups](https://startups.microsoft.com)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 
