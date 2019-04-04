---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: Este mapa fornece uma visão geral dos principais recursos corporativos para aplicativos do Windows 10 e da UWP (Plataforma Universal do Windows).
title: Enterprise
ms.date: 08/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: fae4d5b57ac5cfb5c47fca1a2f3476cd16a56534
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57582006"
---
# <a name="enterprise"></a>Enterprise

Este artigo apresenta uma visão geral dos principais recursos corporativos fornecidos pela UWP (Plataforma Universal do Windows) para aplicativos do Windows 10. Para obter um vídeo que demonstra alguns desses recursos em detalhes, veja [Construir rapidamente aplicativos LOB com UWP e Visual Studio](https://channel9.msdn.com/Events/Build/2018/BRK3502).

<a id="template-studio" />

### <a name="windows-template-studio"></a>Windows Template Studio

O Windows Template Studio é uma extensão Visual Studio 2017 que agiliza a criação de novos aplicativos UWP (Plataforma Universal do Windows) usando uma experiência baseada em assistente. O projeto UWP resultante é um código bem formado e legível que incorpora os recursos mais recentes do Windows 10 enquanto implementa padrões comprovados e melhores práticas.

![Windows Template Studio](images/windows-template-studio.png)

Veja [Windows Template Studio](https://marketplace.visualstudio.com/items?itemName=WASTeamAccount.WindowsTemplateStudio)

<a id="desktop-style-UI" />

### <a name="controls-to-create-desktop-style-uis"></a>Controles para criar interfaces do usuário no estilo desktop

Lançamos novos controles UWP XAML que fecham a lacuna entre uma interface do usuário de aplicativo da área de trabalho tradicional e uma interface do usuário da UWP.

Por exemplo, os novos controles [MenuBar](/windows/uwp/design/controls-and-patterns/menus), [DropDownButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-drop-down-button), [SplitButton](/windows/uwp/design/controls-and-patterns/buttons#create-a-split-button) e [CommandBarFlyout](/windows/uwp/design/controls-and-patterns/command-bar-flyout) oferecem maneiras mais flexíveis de expor comandos, e a [EditableComboBox](/windows/uwp/design/controls-and-patterns/combo-box#make-a-combo-box-editable) permite ao usuário inserir valores que não estão listados em uma lista predefinida de opções.

![MenuBar](images/menu-bar.png)

<a id="enterprise" />

### <a name="controls-to-support-enterprise-scenarios"></a>Controles para dar suporte a cenários corporativos

O [DataGridView](https://docs.microsoft.com/en-us/windows/communitytoolkit/controls/datagrid) oferece uma maneira flexível de exibir um conjunto de dados em linhas e colunas.

A [TreeView](https://docs.microsoft.com/windows/uwp/design/controls-and-patterns/tree-view) habilita uma lista hierárquica com nós em expansão e em colapso que contêm itens aninhados. Ela pode ser usada para ilustrar uma estrutura de pastas ou relacionamentos aninhados em sua interface do usuário.

![Controle DataGrid](images/DataGrid.gif)


### <a name="windows-ui-library"></a>Biblioteca de Interface do Usuário do Windows

A biblioteca de interface do usuário do Windows é um conjunto de pacotes NuGet que fornecem controles e outros elementos de interface do usuário para aplicativos UWP. Ela também permite compatibilidade com versões anteriores do Windows 10 para que seu aplicativo funcione mesmo que os usuários não tenham o sistema operacional mais recente.

![Biblioteca de Interface do Usuário do Windows](images/win-ui.png)

Veja [Biblioteca de Interface do Usuário do Windows (versão prévia)](https://docs.microsoft.com/uwp/toolkits/winui/).

<a id="xaml-islands" />

### <a name="uwp-controls-in-desktop-applications"></a>Controles UWP em aplicativos da área de trabalho

O Windows 10 agora permite que você use controles em aplicativos da área de trabalho de WPF, Windows Forms e C++ Win32. Isso significa que você pode aprimorar a aparência, a percepção e a funcionalidade de seus aplicativos da área de trabalho existentes com os recursos mais recentes de interface do usuário do Windows 10 que só estão disponíveis por meio de controles UWP, como Windows Ink e controles que dão suporte ao Sistema de Design Fluente. Esse recurso é chamado de ilhas XAML.

Veja [Controles UWP em aplicativos da área de trabalho](https://docs.microsoft.com/windows/uwp/xaml-platform/xaml-host-controls).

<a id="standard" />

### <a name="net-standard-20"></a>.NET Standard 2.0

O .NET Standard inclui mais de 20.000 APIs a mais que o .NET Standard 1.x. Isso torna muito mais fácil migrar as bibliotecas do .NET Framework existentes e, em seguida, usá-las entre diferentes aplicativos .NET, incluindo o seu aplicativo UWP.

![net-standard](images/dot-net-standard-project-template.png)

Veja [Compartilhar código entre um aplicativo da área de trabalho e um aplicativo UWP](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-migrate).

<a id="sql-server" />

### <a name="sql-server-connectivity"></a>Conectividade do SQL Server

Seu aplicativo pode se conectar diretamente a um banco de dados do SQL Server e, em seguida, armazenar e recuperar dados usando classes no namespace [System.Data.SqlClient](https://docs.microsoft.com/dotnet/api/system.data.sqlclient?redirectedfrom=MSDN&view=netframework-4.7.2).

Veja [Usar um banco de dados do SQL Server em um aplicativo UWP](https://docs.microsoft.com/en-us/windows/uwp/data-access/sql-server-databases).

<a id="MSIX" />

### <a name="msix-deployment"></a>Implantação de MSIX

O MSIX é o formato de pacote do aplicativo do Windows que oferece uma experiência de empacotamento moderna para todos os aplicativos do Windows. O formato de pacote MSIX preserva a funcionalidade de pacotes existentes do aplicativo e arquivos de instalação, além de habilitar recursos de empacotamento e implantação novos e modernos para aplicativos Win32, WPF e Windows Forms.

MSIX é um formato de empacotamento criado para ser seguro, protegido e confiável, baseado em uma combinação de tecnologias de instalação .msi, .appx, App-V e ClickOnce.

![Ícone MSIX](images/MSIX-App-Package.ico)

Veja a [Documentação do MSIX](https://docs.microsoft.com/windows/msix/).

<a id="distribution" />

## <a name="security"></a>Segurança

O Windows 10 oferece um pacote de recursos de segurança para que os desenvolvedores de aplicativos protejam a identidade de seus usuários, a segurança de redes corporativas e todos os dados de negócios armazenados nos dispositivos. Uma novidade no Windows 10 é o Microsoft Passport, uma alternativa de senha de dois fatores fácil de implantar com acesso por PIN ou pelo Windows Hello, que fornece segurança de nível corporativo e dá suporte ao reconhecimento por impressão digital, rosto e íris.

| Tópico | Descrição |
|-------|-------------|
| [Introdução ao desenvolvimento de aplicativos seguros do Windows](https://msdn.microsoft.com/library/windows/apps/mt622741) | Este artigo introdutório explica vários recursos de segurança do Windows nos estágios de autenticação, dados em trânsito e dados em repouso. Ele também descreve como você pode integrar esses estágios em seus aplicativos. Ela abrange uma ampla variedade de tópicos e destina-se principalmente a ajudar arquitetos de aplicativos a entender melhor os recursos do Windows que facilitam e agilizam a criação de aplicativos da Plataforma Universal do Windows. |
| [Autenticação e identidade do usuário](https://msdn.microsoft.com/library/windows/apps/mt270184) | Os aplicativos UWP têm várias opções de autenticação do usuário, que são descritas neste artigo. Para a empresa, o novo recurso Microsoft Passport é altamente recomendável. O Microsoft Passport substitui senhas por uma autenticação forte de dois fatores (2FA), que verifica as credenciais existentes e cria uma credencial específica para o dispositivo, protegida por um gesto do usuário (biométrico ou por meio de PIN), uma experiência conveniente e altamente segura. |
| [Criptografia](https://msdn.microsoft.com/library/windows/apps/mt270191) | A seção sobre criptografia fornece uma visão geral dos recursos de criptografia disponíveis para aplicativos UWP. Os artigos abordam desde instruções introdutórias passo a passo sobre como criptografar dados corporativos confidenciais facilmente até tópicos avançados, como a manipulação de chaves de criptografia e como trabalhar com MACs, hashes e assinaturas. |
| [Proteção de Informações do Windows (WIP)](wip-hub.md) | Este é um tópico central que abrange toda a imagem do desenvolvedor de como a Proteção de Informações do Windows (WIP) se relaciona com arquivos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio. |

## <a name="data-binding-and-databases"></a>Bancos de dados e vinculação de dados

A vinculação de dados é uma maneira de a interface do usuário do aplicativo exibir dados de uma origem externa, como um banco de dados e, opcionalmente, manter esses dados sincronizados. A vinculação de dados permite separar a questão dos dados da questão da interface do usuário, e isso resulta em um modelo conceitual mais simples, bem como melhor legibilidade, capacidade de teste e capacidade de manutenção do seu aplicativo.

| Tópico | Descrição |
|-------|-------------|
| [Visão geral da vinculação de dados](https://msdn.microsoft.com/library/windows/apps/mt269383) | Este tópico mostra como associar um controle (ou outro elemento da interface do usuário) a um único item ou um controle de itens a uma coleção de itens em um aplicativo da Plataforma Universal do Windows (UWP). Este tópico também mostra como controlar a renderização de itens, implementar uma exibição de detalhes com base em uma seleção e converter dados para exibição. |
| [Entity Framework 7 para UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | A execução de consultas complexas em grandes conjuntos de dados é enormemente simplificada com o uso do Entity Framework 7, que dá suporte à UWP. Neste passo a passo, você criará um aplicativo UWP que acessa dados básicos em um banco de dados SQLite local usando o Entity Framework. |
| [Banco de dados local do SQLite](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Este vídeo é um guia do desenvolvedor abrangente para usar o SQLite, a solução recomendada para bancos de dados de aplicativo locais. Visite [SQLite](https://www.sqlite.org/download.html) para baixar a versão mais recente para UWP ou use a versão fornecida com o SDK do Windows 10. |

## <a name="networking-and-data-serialization"></a>Serialização de dados e de rede

Os aplicativos de linha de negócios com frequência precisam se comunicar com uma variedade de outros sistemas ou armazenar dados neles. Normalmente, isso é feito por meio da conexão a um serviço de rede (usando protocolos como REST ou SOAP) e serialização ou desserialização de dados em um formato comum. Trabalhando com redes e serialização de dados em aplicativos UWP semelhantes a aplicativos do WPF, WinForms e ASP.NET. Consulte os artigos a seguir para obter mais informações.

| Tópico | Descrição |
|-------|-------------|
| [Noções básicas de rede](https://msdn.microsoft.com/library/windows/apps/mt280233) | Estas instruções passo a passo explicam conceitos básicos de rede relevantes para todos os aplicativos UWP, independentemente dos protocolos de comunicação em uso.  |
| [Qual tecnologia de rede?](https://msdn.microsoft.com/library/windows/apps/mt280235) | Uma rápida visão geral das tecnologias de rede disponíveis para aplicativos UWP, com sugestões sobre como escolher as tecnologias mais adequadas ao seu aplicativo. |
| [Serialização de XML e SOAP](https://msdn.microsoft.com/library/90c86ass.aspx) | A serialização XML converte objetos em um fluxo XML que obedece a uma determinada linguagem de definição de esquema XML (XSD). Para converter XML em uma classe fortemente tipada, você pode usar a classe nativa [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) ou uma biblioteca externa. |
| [Serialização JSON](https://msdn.microsoft.com/library/windows/apps/br240639) | A serialização JSON (JavaScript Object Notation) é um formato popular para comunicação com APIs REST. O [Newtonsoft Json.NET](https://www.newtonsoft.com/json), com suporte total para aplicativos UWP. |

## <a name="devices"></a>Dispositivos

Para fazer integração com ferramentas de linha de negócios, como impressoras, scanners de código de barras ou leitores de cartão inteligente, talvez seja necessário integrar dispositivos externos ou sensores no seu aplicativo. Aqui estão alguns exemplos de recursos que você pode adicionar ao seu aplicativo usando a tecnologia descrita nesta seção.

| Tópico  | Descrição |
|--------|-------------|
| [Enumerar dispositivos](https://msdn.microsoft.com/library/windows/apps/mt187355) | Este artigo explica como usar o namespace [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) para localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio. Comece aqui se você estiver criando algum aplicativo que funcione com dispositivos. |
| [Impressão e digitalização](https://msdn.microsoft.com/library/windows/apps/mt204544) | Descreve como imprimir e digitalizar a partir do seu aplicativo, incluindo como conectar e trabalhar com dispositivos de negócios, como sistemas de ponto de venda (POS), impressoras de recibo e scanners alimentadores de alta capacidade. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | Além de usar conexões Bluetooth tradicionais para enviar e receber dados ou controlar dispositivos, o Windows 10 permite usar Bluetooth de baixa energia para enviar ou receber beacons em segundo plano. Use isso para exibir notificações ou habilitar a funcionalidade quando um usuário se aproximar ou sair de um local específico. |
| [Armazenamento compartilhado corporativo](enterprise-shared-storage.md) | Em cenários de bloqueio de dispositivo, saiba como os dados podem ser compartilhados dentro do mesmo aplicativo, entre as instâncias de um aplicativo ou até mesmo entre aplicativos. |

## <a name="device-targeting"></a>Direcionamento de dispositivo

Muitos usuários hoje levam para o trabalho o próprio celular ou tablet, com diversos fatores forma e tamanhos de tela. Com a Plataforma Universal do Windows (UWP), você pode gravar um único aplicativo de linha de negócios que será executado diretamente em todos os tipos de dispositivos diferentes, incluindo PCs desktop e telas de PPI, permitindo que você maximize o alcance do seu aplicativo e a eficiência do seu código.

| Tópico | Descrição |
|-------|-------------|
| [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631) | Neste guia introdutório, você vai se familiarizar com a Plataforma Universal do Windows 10. Entre outras coisas, ele define família de dispositivos e orienta como decidir para qual direcionar, apresenta novos painéis e controles de interface do usuário, que permitem que você adapte a sua interface do usuário a diferentes fatores forma de dispositivo diferentes, e explica como entender e controlar a superfície de API que está disponível para seu aplicativo. |
| [Exemplo de código de interface do usuário XAML adaptável](https://go.microsoft.com/fwlink/p/?LinkId=619992) | Essa amostra de código mostra todas as opções de layout e controles para seu aplicativo possíveis, independentemente do tipo de dispositivo, e permite que você interaja com os painéis para mostrar como obter qualquer layout que estiver procurando. Além de mostrar como cada controle responde a diferentes fatores forma, o próprio aplicativo é dinâmico e mostra vários métodos para alcançar a interface do usuário adaptável. |
| [Tópico do Xamarin](/xamarin/) | Xamarin para direcionar para telefone |

## <a name="deployment"></a>Implantação

Você tem opções para distribuir aplicativos aos usuários da sua organização. Você pode usar a Microsoft Store para Empresas e o gerenciamento de dispositivo móvel existente ou pode executar sideload de aplicativos para dispositivos. Você também pode disponibilizar seus aplicativos ao público em geral publicando-os na Microsoft Store.

| Tópico | Descrição |
|-------|-------------|
| [Distribua aplicativos LOB às empresas](https://msdn.microsoft.com/library/windows/apps/mt608995) | Você pode publicar aplicativos de linha de negócios diretamente para as empresas por meio da Microsoft Store para Empresas, para aquisição por volume, sem tornar os aplicativos amplamente disponíveis ao público. |
| [Sideload de apps](https://technet.microsoft.com/library/mt269549) | Ao fazer sideload de um aplicativo, você implanta um pacote do aplicativo assinado em um dispositivo. Você mantém a assinatura, a hospedagem e a implantação desses aplicativos. O processo de sideload de aplicativos é simplificado para Windows 10.             |
| [Publicar aplicativos para a Microsoft Store](https://dev.windows.com/publish) | A Microsoft Store unificada permite que você publique e gerencie todos os seus aplicativos para todos os dispositivos Windows. Personalize a disponibilidade de seu aplicativo com o preço por mercado, controles de distribuição e visibilidade além de outras opções. |

## <a name="enterprise-uwp-samples"></a>Amostras da UWP corporativa

| Tópico |  Descrição |
|------ |--------------|
| [Exemplo de inventário de VanArsdel](https://github.com/Microsoft/InventorySample) | Um aplicativo de exemplo UWP que demonstra os cenários de linha de negócios. O exemplo é baseado na criação e no gerenciamento de clientes, pedidos e produtos para a empresa fictícia VanArsdel. |
| [Exemplo de Banco de Dados de Pedidos do Cliente](https://github.com/Microsoft/Windows-appsample-customers-orders-database) | Um aplicativo de exemplo UWP que demonstra recursos úteis para desenvolvedores corporativos, como autenticação do AAD (Azure Active Directory), controles (incluindo uma grade de dados) da interface do usuário, integração de banco de dados Sqlite e SQL Azure, Entity Framework e serviços da API de nuvem. O exemplo é baseado na criação e no gerenciamento de contas de clientes, pedidos e produtos para a empresa fictícia Contoso. |

## <a name="patterns-and-practices"></a>Padrões e práticas

Bases de código para grande escala, aplicativos de nível corporativo podem ficar difíceis de gerenciar. O Prism é uma estrutura para criar aplicativos XAML com acoplamento flexível e que possam ser mantidos e testados no WPF, na UWP do Windows 10 e no Xamarin Forms. O Prism fornece a implementação de uma coleção de padrões de design que são úteis para escrever aplicativos de XAML bem estruturados e que possam ser mantidos, incluindo o MVVM, injeção de dependência, comandos, EventAggregator, entre outros.

Para obter mais informações sobre o Prism, consulte [GitHub repo](https://github.com/PrismLibrary/Prism).

 

 
