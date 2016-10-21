---
ms.assetid: 4b0c86d3-f05b-450b-bf9c-6ab4d3f07d31
description: "Este mapa fornece uma visão geral dos recursos empresariais principais para aplicativos do Windows 10 e da Plataforma Universal do Windows (UWP)."
title: Enterprise
author: awkoren
translationtype: Human Translation
ms.sourcegitcommit: 8f4b9e7b1b30beb8974a17af77e4d7138bd8f829
ms.openlocfilehash: 75a7723fb8934a59d44da2f075184dd6bbd85d45

---

# Enterprise


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este mapa fornece uma visão geral dos recursos empresariais principais para aplicativos do Windows 10 e da Plataforma Universal do Windows (UWP). O Windows 10 permite gravar uma vez e fazer a implantação em todos os dispositivos, criando um aplicativo personalizado para cada dispositivo. Isso permite criar as ótimas experiências que os seus usuários esperam e, ao mesmo tempo, fornecer controle sobre a segurança, o gerenciamento e a configuração exigidos pela sua organização.

**Observação**  Este artigo é voltado para desenvolvedores de aplicativos UWP empresariais. Para saber sobre o desenvolvimento para a UWP geral, consulte os [Guias de instruções para aplicativos do Windows 10](https://msdn.microsoft.com/library/windows/apps/mt244352). Para saber sobre o desenvolvimento para WPF, Windows Forms ou Win32, visite o [centro de desenvolvimento da área de trabalho](https://dev.windows.com/desktop). Para saber sobre recursos para profissionais de TI, como a implantação no Windows 10 ou gerenciamento de recursos de segurança empresarial, consulte [Windows 10 no TechNet](https://msdn.microsoft.com/library/dn986868).

 

## Segurança


O Windows 10 oferece um pacote de recursos de segurança para que os desenvolvedores de aplicativos protejam a identidade de seus usuários, a segurança de redes corporativas e todos os dados de negócios armazenados nos dispositivos. Uma novidade no Windows 10 é o Microsoft Passport, uma alternativa de senha de dois fatores fácil de implementar com acesso por PIN ou pelo Windows Hello, que fornece segurança de nível empresarial e permite o reconhecimento por impressão digital, rosto e íris.

| Tópico | Descrição |
|-------|-------------|
| [Introdução ao desenvolvimento de aplicativos seguros do Windows](https://msdn.microsoft.com/library/windows/apps/mt622741) | Este artigo introdutório explica vários recursos de segurança do Windows nos estágios de autenticação, dados em trânsito e dados em repouso. Ele também descreve como você pode integrar esses estágios em seus aplicativos. Ela abrange uma ampla variedade de tópicos e destina-se principalmente a ajudar arquitetos de aplicativos a entender melhor os recursos do Windows que facilitam e agilizam a criação de aplicativos da Plataforma Universal do Windows. |
| [Autenticação e identidade do usuário](https://msdn.microsoft.com/library/windows/apps/mt270184) | Os aplicativos UWP têm várias opções de autenticação do usuário, que são descritas neste artigo. Para a empresa, o novo recurso Microsoft Passport é altamente recomendável. O Microsoft Passport substitui senhas por uma autenticação forte de dois fatores (2FA), que verifica as credenciais existentes e cria uma credencial específica para o dispositivo, protegida por um gesto do usuário (biométrico ou por meio de PIN), uma experiência conveniente e altamente segura. |
| [Criptografia](https://msdn.microsoft.com/library/windows/apps/mt270191) | A seção sobre criptografia fornece uma visão geral dos recursos de criptografia disponíveis para aplicativos UWP. Os artigos abordam desde instruções introdutórias passo a passo sobre como criptografar dados corporativos confidenciais facilmente até tópicos avançados, como a manipulação de chaves de criptografia e como trabalhar com MACs, hashes e assinaturas. |
| [Proteção de Informações do Windows (WIP)](wip-hub.md) | Este é um tópico central que abrange toda a imagem do desenvolvedor de como a Proteção de Informações do Windows (WIP) se relaciona com arquivos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio. |

 

## Bancos de dados e vinculação de dados


A vinculação de dados é uma maneira de a interface do usuário do aplicativo exibir dados de uma origem externa, como um banco de dados e, opcionalmente, manter esses dados sincronizados. A vinculação de dados permite separar a questão dos dados da questão da interface do usuário, e isso resulta em um modelo conceitual mais simples, bem como melhor legibilidade, capacidade de teste e capacidade de manutenção do seu aplicativo.

| Tópico | Descrição |
|-------|-------------|
| [Visão geral da vinculação de dados](https://msdn.microsoft.com/library/windows/apps/mt269383) | Este tópico mostra como associar um controle (ou outro elemento da interface do usuário) a um único item ou um controle de itens a uma coleção de itens em um aplicativo da Plataforma Universal do Windows (UWP). Este tópico também mostra como controlar a renderização de itens, implementar uma exibição de detalhes com base em uma seleção e converter dados para exibição. |
| [Entity Framework 7 para UWP](https://msdn.microsoft.com/library/windows/apps/mt592863) | A execução de consultas complexas em grandes conjuntos de dados é enormemente simplificada com o uso do Entity Framework 7, que dá suporte à UWP. Neste passo a passo, você criará um aplicativo UWP que acessa dados básicos em um banco de dados SQLite local usando o Entity Framework. |
| [Banco de dados local do SQLite](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/10) | Este vídeo é um guia do desenvolvedor abrangente para usar o SQLite, a solução recomendada para bancos de dados de aplicativo locais. Visite [SQLite](https://www.sqlite.org/download.html) para baixar a versão mais recente para UWP ou use a versão fornecida com o SDK do Windows 10. |

 

## Serialização de dados e de rede


Os aplicativos de linha de negócios com frequência precisam se comunicar com uma variedade de outros sistemas ou armazenar dados neles. Normalmente, isso é feito por meio da conexão a um serviço de rede (usando protocolos como REST ou SOAP) e serialização ou desserialização de dados em um formato comum. Trabalhando com redes e serialização de dados em aplicativos UWP semelhantes a aplicativos do WPF, WinForms e ASP.NET. Consulte os artigos a seguir para obter mais informações.

| Tópico | Descrição |
|-------|-------------|
| [Noções básicas de rede](https://msdn.microsoft.com/library/windows/apps/mt280233) | Estas instruções passo a passo explicam conceitos básicos de rede relevantes para todos os aplicativos UWP, independentemente dos protocolos de comunicação em uso.  |
| [Qual tecnologia de rede?](https://msdn.microsoft.com/library/windows/apps/mt280235) | Uma rápida visão geral das tecnologias de rede disponíveis para aplicativos UWP, com sugestões sobre como escolher as tecnologias mais adequadas ao seu aplicativo. |
| [Serialização de XML e SOAP](https://msdn.microsoft.com/library/90c86ass.aspx) | A serialização XML converte objetos em um fluxo XML que obedece a uma determinada linguagem de definição de esquema XML (XSD). Para converter XML em uma classe fortemente tipada, você pode usar a classe nativa [XDocument](https://msdn.microsoft.com/library/system.xml.linq.xdocument.aspx) ou uma biblioteca externa. |
| [Serialização JSON](https://msdn.microsoft.com/library/windows/apps/br240639) | A serialização JSON (JavaScript Object Notation) é um formato popular para comunicação com APIs REST. O [Newtonsoft Json.NET](http://www.newtonsoft.com/json), com suporte total para aplicativos UWP. |

 

## Dispositivos


Para fazer integração com ferramentas de linha de negócios, como impressoras, scanners de código de barras ou leitores de cartão inteligente, talvez seja necessário integrar dispositivos externos ou sensores no seu aplicativo. Aqui estão alguns exemplos de recursos que você pode adicionar ao seu aplicativo usando a tecnologia descrita nesta seção.

| Tópico  | Descrição |
|--------|-------------|
| [Enumerar dispositivos](https://msdn.microsoft.com/library/windows/apps/mt187355) | Este artigo explica como usar o namespace [Windows.Devices.Enumeration](https://msdn.microsoft.com/library/windows/apps/br225459) para localizar dispositivos que estejam conectados internamente ao sistema, externamente ou que possam ser detectados por protocolos de rede ou sem fio. Comece aqui se você estiver criando algum aplicativo que funcione com dispositivos. |
| [Impressão e digitalização](https://msdn.microsoft.com/library/windows/apps/mt204544) | Descreve como imprimir e digitalizar a partir do seu aplicativo, incluindo como conectar e trabalhar com dispositivos de negócios, como sistemas de ponto de venda (POS), impressoras de recibo e scanners alimentadores de alta capacidade. |
| [Bluetooth](https://msdn.microsoft.com/library/windows/apps/mt270288) | Além de usar conexões Bluetooth tradicionais para enviar e receber dados ou controlar dispositivos, o Windows 10 permite usar Bluetooth de baixa energia para enviar ou receber beacons em segundo plano. Use isso para exibir notificações ou habilitar a funcionalidade quando um usuário se aproximar ou sair de um local específico. |
| [Armazenamento compartilhado corporativo](enterprise-shared-storage.md) | Em cenários de bloqueio de dispositivo, saiba como os dados podem ser compartilhados dentro do mesmo aplicativo, entre as instâncias de um aplicativo ou até mesmo entre aplicativos. |

 

## Direcionamento de dispositivo


Muitos usuários hoje levam para o trabalho o próprio celular ou tablet, com diversos fatores forma e tamanhos de tela. Com a Plataforma Universal do Windows (UWP), você pode gravar um único aplicativo de linha de negócios que será executado diretamente em todos os tipos de dispositivos diferentes, incluindo PCs desktop e telas de PPI, permitindo que você maximize o alcance do seu aplicativo e a eficiência do seu código.

| Tópico | Descrição |
|-------|-------------|
| [Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631) | Neste guia introdutório, você vai se familiarizar com a Plataforma Universal do Windows 10. Entre outras coisas, ele define família de dispositivos e orienta como decidir para qual direcionar, apresenta novos painéis e controles de interface do usuário, que permitem que você adapte a sua interface do usuário a diferentes fatores forma de dispositivo diferentes, e explica como entender e controlar a superfície de API que está disponível para seu aplicativo. |
| [Amostra de código de interface do usuário XAML adaptável](http://go.microsoft.com/fwlink/p/?LinkId=619992) | Essa amostra de código mostra todas as opções de layout e controles para seu aplicativo possíveis, independentemente do tipo de dispositivo, e permite que você interaja com os painéis para mostrar como obter qualquer layout que estiver procurando. Além de mostrar como cada controle responde a diferentes fatores forma, o próprio aplicativo é dinâmico e mostra vários métodos para alcançar a interface do usuário adaptável. |

 

## Implantação


Você tem opções para distribuir aplicativos aos usuários da sua organização. Você pode usar a Windows Store para Empresas, o gerenciamento de dispositivo móvel existente, ou adicionar aplicativos a dispositivos por sideload. Você também pode disponibilizar seus aplicativos ao público geral na Windows Store.

| Tópico | Descrição |
|-------|-------------|
| [Distribuir aplicativos LOB para empresas](https://msdn.microsoft.com/library/windows/apps/mt608995) | Você pode publicar aplicativos de linha de negócios diretamente para as empresas por meio da Windows Store para Empresas, para aquisição por volume, sem tornar os aplicativos amplamente disponíveis ao público. |
| [Fazer o sideload de aplicativos](https://technet.microsoft.com/library/mt269549) | Ao fazer sideload de um aplicativo, você implanta um pacote do aplicativo assinado em um dispositivo. Você mantém a assinatura, a hospedagem e a implantação desses aplicativos. O processo de sideload de aplicativos é simplificado para Windows 10.             |
| [Publicar aplicativos na Windows Store](https://dev.windows.com/publish) | A Windows Store unificada permite que você publique e gerencie todos os seus aplicativos para todos os dispositivos Windows. Personalize a disponibilidade de seu aplicativo com o preço por mercado, controles de distribuição e visibilidade, além de outras opções. |

 

## Padrões e práticas


Bases de código para grande escala, aplicativos de nível corporativo podem ficar difíceis de gerenciar. O Prism é uma estrutura para criar aplicativos XAML com acoplamento flexível e que possam ser mantidos e testados no WPF, na UWP do Windows 10 e no Xamarin Forms. O Prism fornece a implementação de uma coleção de padrões de design que são úteis para escrever aplicativos de XAML bem estruturados e que possam ser mantidos, incluindo o MVVM, injeção de dependência, comandos, EventAggregator, entre outros.

Para obter mais informações sobre o Prism, consulte [GitHub repo](https://github.com/PrismLibrary/Prism).

 

 



<!--HONumber=Aug16_HO5-->


