---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Vinculação de dados e MVVM
description: Vinculação de dados é a essência do padrão Model-View-ViewModel (MVVM) da interface do usuário design arquitetônico e permite que um acoplamento entre o código da interface do usuário e sem interface do usuário.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57616731"
---
# <a name="data-binding-and-mvvm"></a>Vinculação de dados e MVVM

Model-View-ViewModel (MVVM) é um padrão de design de arquitetura de interface do usuário para desacoplar o código de interface do usuário e sem interface do usuário. Com o MVVM, você define sua interface do usuário declarativamente em XAML e usa a marcação de associação de dados para vinculá-lo a outras camadas que contém dados e comandos. A infra-estrutura de ligação de dados fornece um acoplamento fraco que mantém a interface do usuário e dos dados vinculados sincronizadas e roteia a entrada do usuário para os comandos apropriados. 

Porque ele fornece um acoplamento flexível, o uso da ligação de dados reduz dependências rígidas entre os diferentes tipos de código. Isso torna mais fácil alterar as unidades de código individuais (métodos, classes, controles, etc.) sem causar efeitos colaterais não intencionais em outras unidades. Essa desassociação é um exemplo de como o *separação de preocupações*, que é um conceito importante em vários padrões de design. 

## <a name="benefits-of-mvvm"></a>Benefícios do MVVM

Desacoplamento de seu código tem muitos benefícios, incluindo:

* Habilitando um estilo de codificação iterativo e exploratório. Alteração que é isolada é menos arriscado e mais fácil de experimentar.
* Simplifica testes de unidade. Unidades de código que são isoladas uns dos outros podem ser testadas individualmente e fora dos ambientes de produção.
* Suporte a colaboração em equipe. Código separado que obedece às interfaces bem projetadas pode ser desenvolvido por indivíduos diferentes ou equipes e integrado mais tarde.
* Melhorando a facilidade de manutenção. Correção de bugs no código separado é menos propenso a causar regressões em outro código.

Em contraste com o MVVM, um aplicativo com uma estrutura de "" code-behind mais convencional normalmente usa a ligação de dados para dados somente para exibição e responde à entrada do usuário manipulando os eventos expostos por controles diretamente. Os manipuladores de eventos são implementados em arquivos code-behind (por exemplo, o MainPage.xaml.cs) e são geralmente estreitamente ligados aos controles, geralmente contendo o código que manipula a interface do usuário diretamente. Isso torna difícil ou impossível substituir um controle sem ter que atualizar o código de manipulação de eventos. Com essa arquitetura, os arquivos code-behind geralmente acumularem código que não está diretamente relacionado à interface do usuário, como o código de acesso de banco de dados, o que acaba sendo duplicado e modificada para uso com outras páginas.

## <a name="app-layers"></a>Camadas de aplicativo

Ao usar o padrão MVVM, um aplicativo é dividido em camadas a seguir:

* O **modelo** camada define os tipos que representam dados de sua empresa. Isso inclui tudo que é necessário para modelar o domínio do aplicativo principal e geralmente inclui a lógica do aplicativo principal. Essa camada é completamente independente da exibição e camadas do modelo de exibição e geralmente reside parcialmente na nuvem. Dada uma camada de modelo totalmente implementada, você pode criar vários cliente diferentes aplicativos se preferir, como o UWP e aplicativos da web que funcionam com os mesmos dados subjacentes.
* O **exibição** camada define a interface do usuário usando a marcação XAML. A marcação inclui expressões de associação de dados (como [X:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) que definem a conexão entre os componentes de interface do usuário específicos e vários membros de modelo de exibição e o modelos. Arquivos code-behind, às vezes, são usados como parte da camada de exibição para conter o código adicional necessário para personalizar ou manipular a interface do usuário, ou para extrair dados de argumentos de manipulador de eventos antes de chamar um método de modelo de exibição que executa o trabalho. 
* O **modelo de exibição** camada fornece os destinos de vinculação de dados para o modo de exibição. Em muitos casos, o modelo de exibição exibe o modelo diretamente ou fornece membros que envolvem membros de modelo específico. O modelo de exibição também pode definir membros para manter o controle de dados que são relevantes para a interface do usuário, mas não para o modelo, como a ordem de exibição de uma lista de itens. O modelo de exibição também serve como um ponto de integração com outros serviços como o código de acesso de banco de dados. Para projetos simples, talvez não seja necessário uma camada de modelo separado, mas apenas um modelo de exibição que encapsula todos os dados que necessários. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básico e avançado

Assim como acontece com qualquer padrão de design, há mais de uma maneira de implementar o MVVM, e muitas técnicas diferentes são consideradas parte do MVVM. Por esse motivo, há várias estruturas MVVM de terceiros diferentes com suporte as várias plataformas com base em XAML, incluindo UWP. No entanto, essas estruturas geralmente incluem vários serviços para implementar uma arquitetura desacoplada, tornando a definição exata do MVVM um pouco ambíguo. 

Embora estruturas sofisticadas do MVVM podem ser muito útil, especialmente para projetos de escala empresarial, normalmente há um custo associado com a adoção de qualquer determinado padrão ou uma técnica e os benefícios nem sempre são claras, dependendo da escala e tamanho de seu projeto. Felizmente, você pode adotar somente essas técnicas que fornecem um benefício claro e tangível e Ignorar outros até que você precisa deles. 

Em particular, você pode obter a grande vantagem simplesmente por Noções básicas e aplicação de todo o potencial de dados de associação e separar a lógica do aplicativo em camadas descritas anteriormente. Isso pode ser obtido usando apenas os recursos fornecidos pelo SDK do Windows e sem usar qualquer estruturas externas. Em particular, o [extensão de marcação {X:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) facilita a vinculação de dados e mais alto de desempenho superior em plataformas anteriores de XAML, eliminando a necessidade de muito código clichê necessário anteriormente.

Para obter orientação adicional sobre como usar o MVVM básico, out-of-the-box, confira a [exemplo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) no GitHub. Muitos dos outros [exemplos de aplicativos UWP](https://github.com/Microsoft?q=windows-appsample
) também usar uma arquitetura básica do MVVM e o [tráfego de aplicativo de exemplo](https://github.com/Microsoft/Windows-appsample-trafficapp) inclui code-behind e versões do MVVM, com anotações que descrevem o [conversão MVVM ](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Consulte também

### <a name="topics"></a>Tópicos

[Associação de dados em camadas](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[extensão de marcação {X:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Exemplos

[Exemplo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Exemplo de inventário de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemplo de aplicativo de tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp)  
