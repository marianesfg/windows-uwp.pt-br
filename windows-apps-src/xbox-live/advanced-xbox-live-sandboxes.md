---
title: Áreas restritas avançadas do Xbox Live
description: Saiba como usar as áreas restritas para isolar o conteúdo durante o desenvolvimento por parceiros gerenciados.
ms.assetid: bd8a2c51-2434-4cfe-8601-76b08321a658
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, xbox um, xdk, parceiro gerenciado, área restrita, isolamento de conteúdo
ms.localizationpriority: medium
ms.openlocfilehash: 5e95999fc132e5dcda556120e9cf42f9c302f654
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57617141"
---
# <a name="advanced-xbox-live-sandboxes"></a>Áreas restritas avançadas do Xbox Live

> [!NOTE]
> Este artigo explicam o uso avançado de áreas restritas e é aplicável principalmente para estúdios de jogos grandes que têm várias equipes e requisitos de permissões complexa.  Se você fizer parte do programa de criadores do Xbox Live ou um ID@Xbox desenvolvedor, é recomendável examinar o [introdução de áreas restritas do Xbox Live](xbox-live-sandboxes.md)

O Xbox Live *área restrita* fornece um ambiente inteiro privado para desenvolvimento. Este documento explica quais são as áreas restritas, por que eles existem, como elas se aplicam a editores e como elas afetam as equipes internas do Xbox. O público-alvo deste documento é Publicadores que criam conteúdo Xbox One e usam as áreas restritas.

## <a name="isolate-content-on-xbox-live"></a>Isolamento de conteúdo no Xbox Live

No Xbox Live, há apenas um ambiente de produção de único onde reside a todos os pré-lançamento (em desenvolvimento e beta), certificação e conteúdo de varejo.

Isolamento de conteúdo é a maneira de garantir que não há nenhum vazamentos de conteúdo do publicador em produção. Em seu núcleo, o isolamento de conteúdo garante que qualquer usuário de entidade, o dispositivo ou o título que solicita permissão para acessar um recurso (um título ou um serviço) tiver sido autorizado a acessar o recurso. Com o isolamento de conteúdo, as partições são divididas em áreas restritas onde os dados de título ou de serviço são armazenados. Em outras palavras, as políticas de autorização são definidas dentro do escopo de uma área restrita.

As áreas restritas são uma forma de particionar os dados que está em produção. Com os serviços do Xbox 360 era, PartnerNet e ProductionNet são dois ambientes diferentes. Com os serviços do Xbox One era, contém um ambiente de produção única *n* distintos ambientes virtuais, onde cada ambiente virtual é chamado em uma área restrita. Porque não há um ambiente de produção único para todo o conteúdo, as áreas restritas são realmente exclusivos ambientes virtuais onde dados gerados em um ambiente não podem ultrapassar para outro.

A figura a seguir mostra um ambiente de produção exclusivo no qual os editores podem criar áreas restritas development privada. Desenvolvimento autorizados somente contas ou kits de desenvolvimento podem acessar essas áreas restritas.

Figura 1. Áreas restritas em um ambiente de produção.

![](images/sandboxes/sandboxes_image1.png)

Assim como PublisherA tem suas áreas de segurança de desenvolvimento, outros editores têm suas próprias áreas de segurança de desenvolvimento. A mesma ID de título pode residir em áreas de segurança diferentes, mas os dados gerados para a ID de título são diferentes entre as áreas restritas.

Há duas áreas de segurança do sistema que só podem ser preenchidas pela Microsoft: CERT e varejo. Como os nomes sugerem, a área de segurança de certificado é para títulos que estão passando por antes da certificação de versão, enquanto a área restrita de varejo é a área de restrita representando dólares real que é acessível por todos os dispositivos e usuários de varejo.

Enquanto uma ID de título era anteriormente conhecido como exclusiva no Xbox Live, agora uma ID de título além de uma ID de área restrita é exclusivo. O mesmo se aplica às IDs do produto e outros espaços de ID que uma vez foram tratados como exclusivos. Agora deve ser emparelhados com uma ID de área restrita. Todos os dados no Xbox Live serão particionados principalmente pela ID de área restrita em todo o sistema.

## <a name="initial-setup-for-a-title"></a>Configuração inicial para um título

Um título nasce no Partner Center ou no Portal de desenvolvedor do Xbox (XDP). Este documento aborda títulos nascidos em XDP. Títulos são atribuídos a uma ID de título, ID de produto e uma configuração de serviço ID (SCID).

Neste novo mundo, um título ou produto por conta própria não significa nada com o Xbox Live. Porque estamos deve dar suporte a varejo simultâneo e o uso de desenvolvimento de cada título, damos suporte *instanciação* de títulos para tornar e manter as distinções necessárias. Uma instância de um título reside em uma área restrita e é aí que entram as áreas restritas.

Para criar um título em XDP, um editor cria um grupo de produtos, especifica o gênero do grupo de produtos e, em seguida, cria produtos individuais dentro dele. (Para obter mais detalhes, consulte a documentação de XDP.) O diagrama a seguir ilustra as relações entre um grupo de produtos, um produto, uma instância do produto e uma área restrita.

Figura 2. As relações entre um grupo de produtos, um produto, uma instância do produto e uma área restrita.

![](images/sandboxes/sandboxes_image2.png)

<a name="product-instances"></a>Instâncias de produto
-----------------
Um *instância product* é uma projeção de dados de título, produto e a configuração em uma área específica. Esses dados são descritos em três áreas a seguir: configuração, metadados de catálogo e binários de serviço.

### <a name="service-configuration"></a>Configuração de serviço

> Definições de configuração de serviço (eventos, estatísticas, conquistas, etc.). A configuração do serviço é definida no nível de instância do produto.

### <a name="catalog-metadata"></a>Metadados de catálogo

> Os metadados que reside no catálogo, incluindo o texto de origem de venda, ativos de arte, informações de disponibilidade/oferta, licenciamento de dados e muito mais.

### <a name="binary"></a>Binário

> Um binário pode ser representado em uma destas duas maneiras:

1.  Metadados somente para habilitar o sideload. Isso inclui a ID de conteúdo, informações de versão e as informações de licenciamento.

2.  Binário completo propagadas para uma CDN e que pode ser baixado para um cliente.

<a name="getting-the-access-right"></a>Obter o acesso correto
------------------------
Há dois tipos distintos de acesso ao seu conteúdo no Xbox One:

*Acesso de tempo de design*— acesso de um PC por meio da ferramenta XDP — permite que pessoas trabalhando em seus produtos carregar, organizar e trabalhar com conteúdo, configuração e metadados, mas não permite-los executar ou executar as instâncias de seus produtos.

*Acesso de tempo de execução*— acesso a partir de um console do Xbox — permite que os desenvolvedores, testadores, revisores e, eventualmente, seus clientes para executar e reproduzir as instâncias de product.

> [!NOTE]
> Para estar disponível para acesso de tempo de execução, uma instância do produto deve ser colocada em uma área restrita. Depois que uma compilação é colocada em uma área restrita, os usuários XDP ou dispositivos do Kit de desenvolvimento que receberam acesso a essa área restrita podem executar a instância. Para fazer isso, que fizerem logon no Xbox One por meio de um console do Xbox, usando uma das suas contas de desenvolvimento — especial contas que funcionam como virtuais usuários para acesso de tempo de execução.

Quando falamos sobre as áreas restritas, normalmente falamos sobre o acesso de tempo de execução para o conteúdo que é executado no Xbox Live. Para acessar um serviço no Xbox Live, um ID de título é obrigatório. Uma vez um **appxmanifest** contém uma ID de título, o console enviará a ID de título com o Xbox Live. Os serviços de segurança do Xbox Live não fornecerá volta um token válido, a menos que a entidade de segurança (dispositivo ou usuário) tem acesso ao título.

Esse processo de validação é o ponto crucial do isolamento de conteúdo. Quando vistos em um nível muito alto:

-   Um grupo de entidade pode conter IDs de usuário do Xbox (XUIDs), as identificações de dispositivo, as IDs de título ou IDs de serviço.

-   Uma área restrita pode conter IDs de título, as IDs de produto ou IDs (SCIDs) de configuração de serviço.

-   Um grupo de entidade receber acesso a uma área restrita.

Portanto, para um usuário ou dispositivo acessar um título de pré-lançamento em uma área restrita, acesso deve ser concedido por meio de XDP pela primeira vez.

Figura 3. Um modelo para configurar o acesso por meio de XDP.

![](images/sandboxes/sandboxes_image3.png)

A eficácia do isolamento de conteúdo é baseada no fato de que sua organização possui os seguintes processos:

-   Criando suas contas de usuário XDP, as contas de desenvolvimento que cada usuário usará para fazer logon para acesso de tempo de execução e os grupos de usuários em que cada usuário permissão de associação.

-   Criando grupos de dispositivos de consoles confiáveis.

-   Especificando para cada uma das suas áreas de segurança de desenvolvimento com precisão quais grupos de usuários e grupos de dispositivos têm acesso às instâncias do produto nele.

Um exemplo dessa configuração é ilustrado na figura abaixo.

Figura 4. As credenciais de um usuário não autorizado não conseguir obter acesso à área restrita, assim como as credenciais comuns de uma conta de usuário XDP autorizada. Somente as credenciais da conta de desenvolvimento pertencentes a conta de usuário XDP autorizada êxito na obtenção de acesso de tempo de execução para a área restrita e todas as instâncias de produtos contidos nela.

![](images/sandboxes/sandboxes_image4.png)

### <a name="dev-accounts-setup"></a>Configuração de contas de desenvolvimento

Contas de desenvolvimento no Xbox One são padrão apenas contas da Microsoft (MSA) com regras especiais aplicadas a eles. Eles são usados no Xbox Live para desenvolvimento. Uma conta de desenvolvedor:

-   Deve ser criado de XDP ou Partner Center.

-   É atribuída à função de desenvolvedor externo quando criado por editores.

-   Está vinculada à conta XDP ou conta no Partner Center que criou a conta de desenvolvimento.

-   Só pode fazer logon em kits de desenvolvimento. Logon negado para uma conta de desenvolvedor em dispositivos de varejo.

-   Pode comprar assinatura Xbox Live desenvolvedor Gold ou outras assinaturas gratuitamente para testar.

### <a name="user-group-setup"></a>Configuração do grupo de usuário

Um grupo de usuários, o primeiro tipo de grupo de entidade é uma coleção de usuários XDP. Quando os usuários XDP são adicionados aos grupos de usuários, suas contas do desenvolvimento de fluxo com esses usuários XDP.

Portanto, quando um grupo de usuários é atribuído a uma área restrita, as contas de desenvolvimento associada XDP aos usuários nesse grupo de usuários são adicionados aos grupos apropriados de entidade de segurança e os grupos de entidades obtém uma configuração de política com o conjunto fazendo a área restrita de recursos primário.

> [!NOTE]
> Os grupos de usuários que são criados para acessar as áreas restritas são os mesmos grupos de usuário que são usados para impedir o acesso a dados de configuração no XDP para grupos de produtos e produtos.

### <a name="device-setup"></a>Configuração de dispositivo

Um dispositivo também é adicionado a um grupo de entidade de segurança. Um dispositivo só pode ser usado como um kit de desenvolvimento, se um direito é adquirido por meio de Store de desenvolvedor do jogo e o dispositivo é configurado para ser um kit de desenvolvimento. Depois que um dispositivo é configurado como um kit de desenvolvimento, o dispositivo aparece na lista de dispositivos que podem ser adicionados a grupos de dispositivos.

### <a name="device-group-setup"></a>Configuração do grupo de dispositivo

Um grupo de dispositivos, o segundo tipo de grupo de entidade de segurança, pode também receber acesso a áreas restritas. A configuração é semelhante à configuração do grupo de usuário detalhada acima.

## <a name="sandboxes"></a>Áreas de Segurança

<a name="what-is-a-sandbox"></a>O que é uma área restrita?
------------------
Declarados simplesmente *uma área restrita é uma maneira de particionar dados em produção*.

<a name="why-do-we-need-sandboxes"></a>Por que precisamos áreas restritas?
-------------------------
Assim como os usuários e dispositivos acessam títulos, títulos de acessar os serviços. Apresentamos um conceito de "grupo de título" onde os conjuntos de títulos recebem acesso aos recursos de serviço.

Como há um ambiente de produção único para o Xbox One para todo o conteúdo (pré-lançamento e varejo), várias instâncias (pré-pré-lançamento/varejo) de um título devem ser impedidas de operar nas mesmas instâncias de recursos.

<a name="what-is-in-a-sandbox"></a>O que é uma área restrita?
---------------------
Uma área restrita contém uma instância do produto para cada título que é adicionado à área restrita.

<a name="what-is-a-sandbox-id"></a>O que é uma ID de área restrita?
---------------------
Uma ID de área restrita é uma unidade de particionamento de dados para um título, um produto ou uma configuração de serviço. Vários títulos podem existir na mesma área de segurança, que é um pré-requisito para que eles possam compartilhar quaisquer dados de configuração de serviço.

Uma ID de sandbox (diferencia maiusculas de minúsculas) é uma cadeia de caracteres no seguinte formato: &lt;PublisherMoniker&gt;.*n*. Uma ID de sandbox de exemplo, XLDP.5, é explicada abaixo:

-   O *moniker publisher* seja exclusivo em todos os publicadores. Então, "XLPD" é o moniker do publicador para esse publicador específico. Um moniker de publicador é criado quando um publicador é "ativado" em XDP pelo Gerenciador de conta de desenvolvedor.

-   O dígito *"n"* identifica o número da área de segurança. Nesse caso, "5" é o sexto sandbox criado para esse publicador.

Quando movem os dados do título por meio de serviços, serviços do Xbox usam a ID de área restrita para identificar exclusivamente o "ambiente" para os dados que são gerados.

<a name="what-data-is-sandboxed"></a>Quais dados estão na área restrita?
-----------------------
O diagrama a seguir mostra quais dados de usuário e o título são uma área restrita.

![](images/sandboxes/sandboxes_image5.png)

<a name="global-override-sandbox"></a>Área restrita de substituição global
-----------------------
Define a ID de área restrita em seu kit de desenvolvimento de um desenvolvedor e, portanto, define a área restrita que o kit de desenvolvimento é executado em; Isso também é conhecido como a área restrita da substituição global. Portanto, todas as solicitações feitas para serviços do Xbox Live (por exemplo, conquistas, compatibilidade, licenciamento, EDS, etc.) de títulos (aplicativos de shell e regular) no dev kit são feitas nessa área restrita.

A área restrita da substituição global também significa que somente o conteúdo ingerido na área restrita de substituição global é visível quando está sendo procurado.

<a name="types-of-sandboxes"></a>Tipos de áreas de segurança
-------------------------------------------------------------------------------------------------------------------------------------------------------
Há duas categorias diferentes de áreas restritas. Essas categorias são definidas da seguinte maneira:

-   *Áreas de segurança de publicador*. Os editores têm acesso a áreas restritas em desenvolvimento. Eles parecem XLDP.0, XLDP.1, XLDP.2, XLDP.3, etc. Isso é onde os publicadores colocaria suas instâncias de produto do título. Acesso a essas áreas restritas é restrito aos usuários/dispositivos que os editores concede acesso a

-   *Áreas de segurança da Microsoft*. Estas são as áreas de segurança internos: VAREJO e CERT somente a Microsoft tem permissão para publicar para essas áreas de segurança protegidas.

<a name="cert-sandbox"></a>Área restrita do certificado
------------
Quando um título está pronto para disponibilidade geral, ele precisa acessar por meio da certificação primeiro. A área de segurança de certificado é uma área restrita controladas pela Microsoft que somente pessoas na certificação têm acesso a. Os editores podem ver o conteúdo que eles próprios está passando por certificação.

Quaisquer instâncias de product falharem enquanto estiver na certificação podem ser trazidas de volta para uma área restrita de desenvolvimento a ser depurado e corrigido pelos fornecedores usando XDP ou Partner Center.

<a name="retail-sandbox"></a>Área restrita de varejo
--------------
A área restrita de varejo é seu destino final de todo o conteúdo que é criado para o Xbox One.

Depois que um título aprovado na certificação, ele é adicionado à área restrita de varejo. Somente conteúdo assinado verde é permitido executar na área restrita de varejo. Isso tem uma implicação importante que betas controlado por publicador também são feitas na área restrita de varejo. Dados gerados na área restrita do varejo representam dados de produção de clientes reais.

Observe que o acesso ao conteúdo é que a área restrita de varejo é ainda controlável por meio do isolamento de conteúdo.

Por exemplo, controlado por publicador betas são executados na área restrita de varejo, em que o publicador escolhe quais grupos de entidades obtém para acessar os títulos de conjunto de recursos beta definido pelo fornecedor. Os dados de serviço gerados pelos títulos de beta é prod real de dados e continua existindo depois que o título é para disponibilidade geral.

<a name="cross-sandbox-data-interaction"></a>Interação de dados de área restrita do cruzada
------------------------------
Por definição, uma área restrita é um contêiner que restringe o compartilhamento de dados. Assim, a interação de dados de área restrita do cruzada não é possível.

## <a name="organizing-your-sandboxes"></a>Organizar suas áreas de segurança

Esta seção fornece um exemplo de como um publicador pode organizar as áreas restritas. Um publicador precisa entender como usar as áreas restritas para organizar os dados.  

Os exemplos a seguir só gerenciamento de acesso de tempo de execução de mostrar com isolamento de conteúdo.

### <a name="scenario-1-two-titles-one-sandbox"></a>Cenário 1: Dois títulos, uma área de segurança

A estrutura básica para um publicador pode ser:

-   Dois títulos que estão acessíveis a todos os usuários e dispositivos de propriedade pelo publicador para o tempo de design e tempo de execução.

-   Instância de um produto por título.

Nesse caso, o publicador precisa apenas uma única área de segurança para todo o conteúdo de pré-lançamento.

O diagrama a seguir mostra um grupo de usuários. O publicador pode optar por usar um grupo de dispositivos em vez de um grupo de usuários se considerado mais fácil. Além disso, esse grupo de usuários tem acesso de tempo de execução e tempo de design a área restrita XLDP.1 e os títulos desta área restrita.

![](images/sandboxes/sandboxes_image6.png)

### <a name="scenario-2-one-title-different-teams"></a>Cenário 2: Um título, as equipes diferentes

Nesse modelo, os requisitos são:

-   Um título.

-   Equipe de desenvolvimento funciona em compilações diárias.

-   Equipe de QA funciona em LKGs semanais.

-   Equipe de desenvolvimento precisa depurar LKGs semanais em caso de bugs.

-   A equipe de finanças precisa acessar as placas de preço e outros metadados relacionados para a versão do catálogo de um título.

A figura a seguir mostra que TitleX tem duas instâncias de produto: PI-1 e 2 PI. Uma instância do produto deve estar em uma área restrita e duas instâncias de produto, o mesmo título não podem ser na mesma área restrita. Portanto, TitleX-PI-1 é em área restrita XLDP.1 e TitleX-PI-2 está na área restrita XLDP.2.

O grupo de usuários de desenvolvimento tem acesso a ambas as áreas de segurança, ao passo que o grupo de usuários de controle de qualidade tem acesso ao sandbox XLDP.2.

Além disso, o usuário de Finanças (grupo C) tem acesso de tempo de design para TitleX. Porque o grupo de usuários de finanças será normalmente faz qualquer tempo de execução de depuração de um título, eles são separados.

> [!NOTE]
> Independentemente da organização, um usuário XDP pode pertencer em mais de um grupo de usuários.

![](images/sandboxes/sandboxes_image7.png)

### <a name="scenario-3-two-titles-completely-separate"></a>Cenário 3: Dois títulos, completamente separados

Neste exemplo, os requisitos de mudam um pouco:

-   Dois títulos.

-   Acesso a cada título deve ser limitado a um determinado conjunto de indivíduos.

-   Instância de um produto por título.

-   Um grupo de usuários do administrador que precisa acessar dados de configuração do tempo de design XDP para os títulos. Os indivíduos neste grupo são todos os administradores para o publicador e podem controlar todos os dados que são publicados no catálogo (metadados de catálogo, finanças, marketing, envio de certificação, etc.)

Nesse modelo, o publicador tiver optado por manter ambos os títulos completamente separados e atribuída, portanto, esses dois títulos em duas áreas de segurança diferentes. O publicador também tiver escolhido para criar um grupo de usuário de administrador separada e atribuído acesso para os dois produtos.

![](images/sandboxes/sandboxes_image8.png)

### <a name="scenario-4-anyway-you-like-it"></a>Cenário 4: Mesmo assim você deseja

Devido ao número de conexões e manter a argumentação curta, escolhemos para mostrar somente a área restrita conexões de tempo de execução. Nada impede a adição de outras permissões de acesso de tempo de design também.

Neste exemplo, os requisitos são:

-   Apenas determinadas pessoas têm acesso a determinados títulos dentro do seu publicador.

-   O publicador trabalha com os fornecedores de empresas diferentes e esses fornecedores podem ser curto prazo.

-   O publicador deve ser capaz de descomissionar um título e fazendo isso, impedir o acesso a todos os dados que os fornecedores ou FTEs tinham acesso.

Para modelar esse requisito, uma estrutura como a mostrada abaixo pode ser adotada.

O modelo seguido abaixo é:

-   TitleX e TitleY têm apenas uma instância do produto cada em área restrita XLDP.1.

-   TitleZ tem instâncias de product dois, um em área restrita XLDP.2 e outro em área restrita XLDP.3.

-   Grupo de usuário de FTE B recebe acesso para instâncias de produto em todas as áreas de segurança.

-   Grupo de usuário do fornecedor A é um grupo de usuário somente de fornecedor que receberá o acesso à área restrita XLDP.1.

-   Grupo de dispositivos de fornecedor C é um grupo de usuário somente de fornecedor que receberá o acesso à área restrita XLDP.3.

![](images/sandboxes/sandboxes_image9.png)

## <a name="determine-the-sandbox-your-device-is-targeting"></a>Determinar a área restrita do seu dispositivo está definindo como destino

As APIs do Xbox Live contêm um singleton de configuração de aplicativo que permitirá que você veja quais sandbox seu título destina-se em tempo de execução. Isso é feito, acessando o **área restrita** propriedade de `xbox::services::xbox_live_app_config`.

C++ XDK
```cpp
auto appConfig = xbox::services::xbox_live_app_config::get_app_config_singleton();
string_t sandbox = appConfig->sandbox;
```

C# WinRT
```csharp
XboxLiveAppConfiguration appConfig = XboxLiveAppConfiguration.SingletonInstance;
string sandbox = appConfig.Sandbox;
```

> [!NOTE]
> A propriedade de área restrita não recebe um valor até que um usuário estiver conectado.

## <a name="summary"></a>Resumo

Desenvolvimento do Xbox Live fornece uma enorme oportunidade para editores a teste em produção com os serviços de qualidade de produção e contas de desenvolvedor do MSA de produção. O aumento de funcionalidade e flexibilidade requer novas etapas de configuração XDP para criar dados de título e gerenciar o acesso aos títulos ao mesmo tempo no desenvolvimento e na disponibilidade geral.

As áreas restritas são uma forma de particionar dados em produção. Porque não há um ambiente de produção único para todo o conteúdo, as áreas restritas atuam como "ambientes virtuais" em que dados gerados em um ambiente não passe para o outro.
