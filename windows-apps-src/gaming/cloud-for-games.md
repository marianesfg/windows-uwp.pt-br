---
author: joannaleecy
title: "Usando serviços de nuvem para jogos UWP"
description: "Saiba mais sobre a implementação de nuvem como um back-end para seus jogos UWP."
ms.assetid: 1a7088e0-0d7b-11e6-8e05-0002a5d5c51b
translationtype: Human Translation
ms.sourcegitcommit: 0b2d81daa8bd0fd5694b81fa14fcd064e1600d35
ms.openlocfilehash: b23c33fac9ac8fe5e2d5563a0af6824c82a3969b

---
#  Usando serviços de nuvem para jogos UWP

A UWP (Plataforma Universal do Windows) no Windows 10 oferece um conjunto de APIs que podem ser usadas para desenvolver jogos entre dispositivos da Microsoft. Ao desenvolver jogos para diferentes dispositivos e plataformas, você pode usar um back-end de nuvem para ajudar a escalonar seu jogo de acordo com a demanda.

##  O que é computação em nuvem?

A computação em nuvem utiliza recursos de TI e aplicativos sob demanda pela internet para armazenar e processar dados para seus dispositivos. O termo _nuvem_ é uma metáfora para a disponibilidade de inúmeros recursos externamente (recursos não locais) que você pode acessar de locais não específicos.
O princípio da computação em nuvem oferece uma nova forma na qual os recursos e o software podem ser consumidos. Os usuários não precisam mais pagar pelos recursos ou produtos completos antecipadamente, mas, em vez disso, são capazes de consumir a plataforma, o software e os recursos como um serviço. Os provedores de nuvem geralmente cobram seus clientes de acordo com as ofertas de planos de uso ou serviços.

##  Por que usar serviços de nuvem?

Uma vantagem de usar os serviços de nuvem para jogos é que você não precisa investir em servidores de hardware físico antecipadamente; precisa apenas pagar de acordo com os planos de uso ou serviços em uma etapa posterior. É uma maneira de ajudar a gerenciar os riscos envolvidos no desenvolvimento de um novo título de jogo. 

Outra vantagem é que seu jogo pode explorar inúmeros recursos de nuvem para obter escalabilidade (gerenciar efetivamente quaisquer picos repentinos no número de jogadores simultâneos, intensos cálculos de jogo em tempo real ou requisitos de dados). Isso mantém o desempenho do seu jogo estável o tempo todo. Além disso, os recursos de nuvem podem ser acessados de qualquer dispositivo em execução em qualquer plataforma em qualquer lugar do mundo, o que significa que você é capaz de levar seu jogo a todos globalmente.

É importante oferecer uma incrível experiência de jogo aos seus jogadores. Como os servidores de jogos executados na nuvem são independentes de atualizações no cliente, eles podem oferecer a você um ambiente mais seguro e controlado para o jogo em geral.   Você também pode obter consistência de jogo pela nuvem nunca confiando no cliente e tendo a lógica do jogo no servidor. As conexões serviço a serviço também podem ser configuradas para permitir uma experiência de jogo mais integrada; exemplos incluem a vinculação de compras no jogo a vários métodos de pagamento, a ponte para diferentes de redes de jogos e o compartilhamento de atualizações no jogo em portais populares de redes sociais, como o Facebook e o Twitter. 

Você também pode usar servidores de nuvem dedicados para criar um grande mundo de jogo persistente, criar uma comunidade de jogadores, coletar e analisar dados de jogadores ao longo do tempo para melhorar a experiência de jogo e otimizar o modelo de design de monetização do seu jogo.

Além disso, os jogos que exigem funcionalidades de intenso gerenciamento de dados do jogo, como jogos sociais com mecânica assíncrona de vários jogadores, podem ser implementados com serviços de nuvem.

##  Como as empresas de jogos usam a tecnologia de nuvem

Saiba como outros desenvolvedores implementaram soluções de nuvem em seus jogos.

<table>
    <colgroup>
    <col width="10%" />
    <col width="30%" />
    <col width="30%" />
    <col width="30%" />
    </colgroup>
    <tr class="header">
        <th>Desenvolvedor</th>
        <th>Descrição</th>
        <th>Principais cenários de jogo</th>
        <th>Saiba mais</th>
    </tr>
    <tr>
        <td>[343 Industries](https://www.halowaypoint.com/)</td>
        <td>_Halo 5: Guardians_ implementou [Halo: Spartan Companies](https://www.halowaypoint.com/spartan-companies) como sua plataforma de jogo social usando o Banco de Dados de Documentos do Microsoft Azure, que foi selecionado por sua velocidade e flexibilidade devido às funcionalidades de indexação automática.</td>
        <td>
            <ul>
                <li>Camada de dados escalonável para lidar com o gerenciamento/criação de grupos para jogos multijogadores <li>Integração de jogo e mídia social <li>Consultas em tempo real de dados por meio de vários atributos <li>Sincronização de conquistas e estatísticas de jogo </ul>
        </td>
        <td>
            <ul>
                <li>[Jogo social implementado com o Banco de Dados de Documentos do Azure](https://azure.microsoft.com/blog/how-halo-5-guardians-implemented-social-gameplay-using-azure-documentdb/)</td>
            </ul>
    </tr>
    <tr>
        <td>[Illyriad Games](http://web.ageofascent.com/)</td>
        <td>A Illyriad Games criou o _Age of Ascent_, um jogo épico de espaço 3D MMO (multijogador massivo online) que pode ser executado em dispositivos com navegadores modernos. Portanto, esse jogo pode ser executado em computadores, notebooks, telefones celulares e outros dispositivos móveis sem plug-ins. O jogo usa ASP.NET Core, HTML5, WebGL e Microsoft Azure.</td>
        <td>
            <ul>
                <li>Jogo baseado em navegador de plataforma cruzada <li>Grande mundo único, aberto e persistente <li>Manipula intensos cálculos de jogo em tempo real <li>É escalonado com o número de jogadores </ul>
        </td>
        <td>
            <ul>
                <li>[Gerenciar componentes de jogo como microsserviços usando o Azure Service Fabric (vídeo)](https://channel9.msdn.com/Events/Build/2016/KEY02#time=57m20s)  
                <li>[Entrevista com desenvolvedores do Age of Ascent (vídeo)](https://channel9.msdn.com/Shows/Azure-Friday/Age-of-Ascent-from-Illyriad-Powered-by-Azure-Service-Fabric-and-ASPNET)
            </ul>
        </td>
    </tr>
    <tr>
        <td>[Next Games](http://www.nextgames.com/)</td>
        <td>A Next Games é a criadora do videogame _The Walking Dead: No Man's Land_, que se baseia na série original da AMC. O jogo Walking Dead usou o Azure como back-end. Ele teve 1.000.000 de downloads no fim de semana de lançamento e, durante a primeira semana, o jogo ficou em primeiro lugar em aplicativos gratuitos para iPhone e iPad na App Store dos EUA, primeiro lugar em aplicativos gratuitos em 12 países e regiões e primeiro lugar em jogos gratuitos em 13 países e regiões.
        </td>
        <td>
            <ul>
                <li>Plataforma cruzada <li>Multijogador baseado em turnos <li>Desempenho de escala elástica </ul>
        </td>
        <td>
            <ul>
                <li>[Entrevista com Kalle Hiitola, CTO da Next Games (vídeo)](https://channel9.msdn.com/Blogs/AzureDocumentDB/azure-documentdb-walking-dead)
                <li>[Walking Dead usa o Banco de Dados de Documentos para um ciclo de desenvolvimento mais rápido e um jogo mais envolvente](https://azure.microsoft.com/blog/the-walking-dead-no-mans-land-game-soars-to-1-with-azure-documentdb/)
            </ul>
    </tr>
    </td>
        <td>[Pixel Squad](http://www.crimecoast.com/)</td>
        <td>A Pixel Squad desenvolveu _Crime Coast_ usando o mecanismo de jogo Unity e o Azure. _Crime Coast_ é um jogo de estratégia social disponível nas plataformas Android, iOS e Windows. O Armazenamento de Blobs do Azure, o Cache Redis do Azure Gerenciado, uma matriz VMs IIS de carga balanceada e o Hub de Notificação da Microsoft foram usados no jogo. Saiba como eles gerenciaram o escalonamento e lidaram com o inesperado número de 5.000 jogadores simultâneos.
        </td>
        <td>
            <ul>
                <li>Plataforma cruzada <li>Jogo online multijogador <li>Escala com o número de jogadores </ul>
        </td>
        <td>
            <ul>
                <li>[Como o jogo MMO Crime Coast usou os Serviços de Nuvem do Azure](https://channel9.msdn.com/Blogs/The-Game-Blog/BizSpark-Interview-with-Pixel-Squad-How-the-used-Azure-Cloud-Services-to-make-an-MMO-with-a-3-man-te)
            </ul>
        </td>
    </tr> 
</table>

    
### Outros links

* [Azure como o molho secreto de Hitcents, Game Troopers e InnoSpark](http://news.microsoft.com/features/game-developers-use-microsoft-azure-as-secret-sauce-for-scale-and-growth-2/)
* [Startups de jogos no programa Bizspark usando o Azure](https://blogs.technet.microsoft.com/bizspark_featured_startups/2015/09/25/azure-open-for-gaming-startups/)


## Como projetar seu back-end de nuvem

Enquanto produtores e designers de jogos discutem quais recursos e funcionalidades são necessários no jogo, é bom começar a considerar como você deseja projetar sua infraestrutura de jogo. A Nuvem do Azure pode ser usada como seu back-end de jogo quando você quer desenvolver jogos para vários dispositivos e em diferentes plataformas importantes.

### Guias de aprendizagem passo a passo

* [Laboratórios de código do Build 2016: usar o back-end do Serviço de Aplicativo do Microsoft Azure e do Microsoft SQL Azure para salvar a pontuação do jogo](https://github.com/Microsoft-Build-2016/CodeLabs-GameDev-6-Azure)
* [Projetar a estratégia de interação móvel do seu jogo](https://azure.microsoft.com/documentation/articles/mobile-engagement-gaming-scenario/)
* [Usando o Azure Mobile Engagement para implantação Unity iOS](https://azure.microsoft.com/documentation/articles/mobile-engagement-unity-ios-get-started/)

### Noções básicas de IaaS, PaaS ou SaaS

Primeiro, você precisa pensar no nível de serviço que é mais adequado para seu jogo. Saber as diferenças dos três serviços a seguir pode ajudá-lo a determinar a abordagem que você deseja usar na criação do seu back-end.

* [IaaS (infraestrutura como serviço)](https://azure.microsoft.com/overview/what-is-iaas/)

    A IaaS (infraestrutura como serviço) é uma infraestrutura de computação instantânea, provisionada e gerenciada pela Internet. Imagine ter a possibilidade de muitos computadores prontamente disponíveis para a rápida escala horizontal ou vertical, dependendo da demanda. A IaaS ajuda você a evitar o custo e a complexidade de comprar e gerenciar seus próprios servidores físicos e outra infraestrutura de datacenter.

* [PaaS (plataforma como serviço)](https://azure.microsoft.com/overview/what-is-paas/)

    A PaaS (plataforma como serviço) é como a IaaS, mas também inclui o gerenciamento de infraestrutura como servidores, armazenamento e rede. Portanto, além de não comprar servidores físicos e infraestrutura de datacenter, você também não precisa comprar e gerenciar licenças de software, infraestrutura de aplicativos subjacente, middleware, ferramentas de desenvolvimento ou outros recursos.

* SaaS (software como serviço)

    O SaaS (software como serviço) normalmente é um aplicativo já criado para você e hospedado em uma plataforma de nuvem existente. Ele foi projetado para facilitar ainda mais para você o início da execução do seu jogo no serviço.


### Projetar sua infraestrutura de jogo usando o Azure

A seguir estão algumas maneiras de usar as ofertas de nuvem do Azure para um jogo. O Azure funciona com Windows, Linux e tecnologias conhecidas de software livre, como Ruby, Python, Java e PHP. Para saber mais, consulte [Nuvem do Azure](https://azure.microsoft.com).

| Requisitos                 | Cenários de atividade                            | Oferta de produto                      | Funcionalidades do produto                               |
|-----------------------------------|-----------------------------------------------|---------------------------------------|----------------------------------------------------|
| Hospedar seu domínio na nuvem     | Responder a consultas do DNS com eficiência            | [DNS do Azure](https://azure.microsoft.com/services/dns/) | Hospedar seu domínio com alto desempenho e disponibilidade  |
| Entrada, verificação de identidade      | O jogador entra e sua identidade é autenticada  | [Azure Active Directory](https://azure.microsoft.com/services/active-directory/) | Logon único em qualquer aplicativo da Web local e na nuvem com autenticação multifator            |
| Jogo usando o modelo IaaS (infraestrutura como serviço)      | O jogo é hospedado em máquinas virtuais na nuvem       | [VMs do Azure](https://azure.microsoft.com/services/virtual-machines/) | Escala de uma a milhares de instâncias de máquina virtual como servidores de jogo com redes virtuais internas e balanceamento de carga; consistência híbrida com sistemas locais           |
| Jogos na Web ou em dispositivos móveis usando PaaS (plataforma como serviço)            | O jogo é hospedado em uma plataforma gerenciada                | [Serviço de Aplicativo do Azure](https://azure.microsoft.com/services/app-service/) | PaaS para sites ou jogos em dispositivos móveis (o que significa VMs do Azure com middleware/ferramentas de desenvolvimento/BI/gerenciamento de banco de dados)   |
| Armazenamento em nuvem para dados de jogo       | Os dados de jogo mais recentes são armazenados na nuvem e enviados a dispositivos cliente | [Armazenamento de Blobs do Azure](https://azure.microsoft.com/services/storage/blobs/)| Nenhuma restrição nos tipos de arquivo que podem ser armazenados; armazenamento de objetos para grandes volumes de dados não estruturados, como imagens, áudio, vídeo e muito mais.  |
| Tabelas de armazenamento de dados temporários| Transações do jogo (mudanças em estados do jogo) são armazenadas temporariamente em tabelas | [Armazenamento de Tabelas do Azure](https://azure.microsoft.com/services/storage/tables/)| Os dados do jogo podem ser armazenados em um esquema flexível de acordo com as necessidades do jogo |
| Transações/solicitações do jogo em fila| As transações do jogo são processadas na forma de uma fila | [Armazenamento de Filas do Azure](https://azure.microsoft.com/services/storage/queues/)| As filas absorvem aumentos súbitos de tráfego e podem impedir que os servidores sejam sobrecarregados por uma inundação repentina de solicitações durante o jogo   |
| Banco de dados de jogo relacional escalonável| Armazenamento estruturado de dados relacionais como transações no jogo no banco de dados | [Banco de Dados do SQL Azure](https://azure.microsoft.com/services/sql-database/)| Banco de dados SQL como serviço ([Comparação com o SQL em uma VM](https://azure.microsoft.com/documentation/articles/data-management-azure-sql-database-and-sql-server-iaas/))  |
| Banco de dados de jogos de baixa latência distribuído e escalonável| Leitura, gravação e consulta de dados de jogo e jogadores rápidas com flexibilidade de esquema | [Banco de Dados de Documentos do Azure](https://azure.microsoft.com/services/documentdb/)| Banco de dados como serviço de documentos NoSQL de baixa latência   |
| Usar o próprio datacenter com serviços do Azure | O jogo é recuperado de seu próprio datacenter e enviado aos dispositivos cliente | [Azure Stack](https://azure.microsoft.com/overview/azure-stack/) | Permite que sua organização forneça serviços do Azure no próprio datacenter para ajudá-lo a obter mais  |
| Transferência de blocos de dados grandes| Arquivos grandes, como imagem, áudio e vídeo de jogos, podem ser enviados aos usuários a partir do local POP da CDN (Rede de Distribuição de Conteúdo) mais próxima com a CDN do Azure    | [Rede de Distribuição de Conteúdo do Azure](https://azure.microsoft.com/services/cdn/) | Criada em uma topologia de rede moderna de nós centralizados grandes, a CDN do Azure manipula picos súbitos de tráfego e cargas pesadas para aumentar drasticamente a velocidade e a disponibilidade, o que resulta em melhorias significativas na experiência do usuário  |
| Baixa latência               | Executa o armazenamento em cache para criar jogos rápidos e escalonáveis com mais controle e isolamento garantido de dados; também pode ser usada para aprimorar o recurso de correspondência do jogo. | [Cache Redis do Azure](https://azure.microsoft.com/services/cache/) | Acesso a dados de baixa latência e alta taxa de transferência para ativar aplicativos do Azure rápidos e escalonáveis.  |
| Alta escalabilidade, baixa latência | Manipula flutuações no número de usuários do jogo com leituras e gravações de baixa latência | [Azure Service Fabric](https://azure.microsoft.com/services/service-fabric/) | Capacidade de ativar os cenários mais complexos de baixa latência com uso intenso de dados e escalonar de forma confiável para manipular mais usuários por vez. O Service Fabric permite que você crie jogos sem precisar criar um armazenamento ou cache separado, conforme necessário para aplicativos sem estado |
| Capacidade de coletar milhões de eventos por segundo de dispositivos                         | Log de milhões de eventos por segundo de dispositivos | [Hubs de Eventos do Azure](https://azure.microsoft.com/services/event-hubs/) | Ingestão de telemetria em escala de nuvem de jogos, sites, aplicativos e dispositivos  |
| Processamento de dados de jogos em tempo real  | Executar a análise de dados de jogadores em tempo real para melhorar o jogo| [Azure Stream Analytics](https://azure.microsoft.com/services/stream-analytics/) | Processamento de fluxo em tempo real na nuvem  |
| Desenvolver jogo preditivo         | Criar um jogo dinâmico personalizado com base nos dados do jogador  | [Aprendizado de Máquina do Azure](https://azure.microsoft.com/services/machine-learning/) | Um serviço de nuvem totalmente gerenciado que permite que você compile, implante e compartilhe soluções de análise preditiva  |
| Coletar e analisar dados de jogos| Intenso processamento de dados paralelo de bancos de dados relacionais e não relacionais | [Data Warehouse do Azure](https://azure.microsoft.com/services/sql-data-warehouse/)| Depósito de dados como serviço elástico com recursos de classe empresarial   |
| Criar campanhas de marketing para aumentar o uso e a retenção  | Enviar notificações por push a jogadores direcionados para gerar interesse e incentivar ações específicas do jogo de acordo com a análise de dados | [Mobile Engagement](https://azure.microsoft.com/services/mobile-engagement/) |  Aumentar o tempo de jogo e a retenção de usuários em todas as principais plataformas — iOS, Android, Windows, Windows Phone |


##  Recursos para startups e desenvolvedores

* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)

    O Microsoft BizSpark é um programa global que ajuda startups a serem bem-sucedidas fornecendo acesso gratuito a serviços de nuvem do Azure, software e suporte. Os membros do BizSpark recebem cinco assinaturas do Visual Studio Enterprise com MSDN, cada uma com um crédito Azure mensal de US$ 150. Isso totaliza US$ 750/mês em todas as cinco para os desenvolvedores gastarem em serviços do Azure. O BizSpark está disponível para startups privadas com menos de 5 anos de operação e com receita anual inferior a US$ 1 milhão. A Microsoft acredita que, ao colaborar com o sucesso das startups, ela está ajudando a criar uma valiosa parceria de longo prazo.
    
* [ID@Xbox](http://www.xbox.com/Developers/id)

    Se você quiser adicionar recursos do Xbox Live, como jogo multijogador, correspondência em plataforma cruzada, pontuação do jogador, conquistas e placares de líderes ao seu jogo do Windows 10, inscreva-se com o ID@Xbox para obter as ferramentas e o suporte de que precisa para soltar sua criatividade e maximizar seu sucesso. Antes de requerer o ID@Xbox, registre uma conta de desenvolvedor no [Centro de Desenvolvimento do Windows](https://developer.microsoft.com/windows/programs/join).

## Software como serviço para back-end de jogo

Estas são algumas empresas que oferecem back-end de nuvem para jogos com base nos principais provedores de serviços de nuvem, para permitir que você se concentre no desenvolvimento do seu jogo.

* [GameSparks](http://www.gamesparks.com/)

    GameSparks é uma plataforma de desenvolvimento baseada em nuvem para desenvolvedores de jogos, permitindo que eles criem tudo no servidor de seus jogos

* [Photon Engine](https://www.photonengine.com/en/Photon)

    Photon é uma plataforma multijogador e um mecanismo rede independente para jogos. Ela oferece o Photon Cloud, que fornece SaaS (software como serviço) e, como tal, é um serviço totalmente gerenciado. Você pode se concentrar completamente no cliente do seu aplicativo durante a hospedagem; quem cuida das operações do servidor e do escalonamento é a Exit Games.

* [Playfab](https://playfab.com/)

    A Playfab oferece tecnologia de back-end e gerenciamento de jogos ao vivo de classe internacional ao seu dispositivo móvel, computador ou jogo de console de modo simples e rápido.

## Links relacionados

* [Guia de desenvolvimento de jogos do Windows 10](https://msdn.microsoft.com/windows/uwp/gaming/e2e)
* [Microsoft Azure](https://azure.microsoft.com/)
* [Microsoft BizSpark](https://www.microsoft.com/bizspark/)
* [ID@Xbox](http://www.xbox.com/Developers/id)


 

 



<!--HONumber=Aug16_HO3-->


