---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Vinculação de dados e MVVM
description: Vinculação de dados é o núcleo do padrão de design arquitetônicas Model-View-ViewModel (MVVM) da interface do usuário e permite que o acoplamento entre o código de interface do usuário e não da interface do usuário.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: e16c9845b52d5bd43fc02bbe92296a9682d96926
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/19/2018
ms.locfileid: "4949232"
---
# <a name="data-binding-and-mvvm"></a>Vinculação de dados e MVVM

Model-View-ViewModel (MVVM) é um padrão de design de arquitetura da interface do usuário para a separação de código de interface do usuário e não da interface do usuário. Com o MVVM, defina sua interface do usuário declarativamente em XAML e usar marcação de associação de dados para vinculá-lo a outras camadas que contêm dados e comandos. A infraestrutura de associação de dados oferece um acoplamento que mantém a interface do usuário e os dados vinculados sincronizadas e roteia entrada do usuário para os comandos apropriados. 

Porque ele fornece acoplamento, o uso da vinculação de dados reduz o disco rígidas dependências entre diferentes tipos de código. Isso torna mais fácil alterar as unidades de código de individuais (métodos, classes, controles, etc.) sem causar efeitos colaterais indesejados em outras unidades. Essa separação é um exemplo da *separação de questões*, que é um conceito importante em muitos padrões de design. 

## <a name="benefits-of-mvvm"></a>Benefícios do MVVM

Separação seu código tem muitos benefícios, incluindo:

* Habilitando um estilo de codificação iterativo, exploratório. Alteração que é isolada é mais fácil de experimentar e menos arriscado.
* Testes de unidade pressuposições. Unidades de código que são isoladas uns dos outros podem ser testadas individualmente e fora de ambientes de produção.
* Oferece suporte a colaboração em equipe. Código dissociado que segue interfaces bem projetadas pode ser desenvolvido por pessoas separadas ou equipes e integrado mais tarde.
* Melhorando a capacidade de manutenção. Corrigir bugs no código dissociado é menor probabilidade de causar regressões em outro código.

Ao contrário da MVVM, um aplicativo com uma estrutura de "code-behind" mais convencional normalmente usa ligação de dados para dados somente para exibição e responde à entrada do usuário manipulando eventos expostos pelos controles diretamente. Os manipuladores de eventos são implementados em arquivos code-behind (por exemplo, MainPage.xaml.cs) e estão geralmente intimamente ligados para os controles, normalmente que contém o código que manipula a interface do usuário diretamente. Isso torna difícil ou impossível substituir um controle sem precisar atualizar o código de manipulação de eventos. Com essa arquitetura, arquivos code-behind geralmente acumulam código que não está diretamente relacionado à interface do usuário, como o código de acesso de banco de dados, o que acaba sendo duplicada e modificados para uso com outras páginas.

## <a name="app-layers"></a>Camadas de aplicativo

Ao usar o padrão MVVM, um aplicativo é dividido em camadas a seguir:

* A camada do **modelo** define os tipos que representem seus dados corporativos. Isso inclui tudo que é necessário para modelar o domínio de aplicativo principal e geralmente inclui lógica principal do aplicativo. Essa camada é completamente independente do modo de exibição e camadas de modelo de exibição e geralmente reside parcialmente na nuvem. Dada uma camada de modelo totalmente implementado, você pode criar vários clientes diferentes aplicativos se preferir, como aplicativos UWP e web que funcionam com os mesmos dados subjacentes.
* A camada de **modo de exibição** define a interface do usuário usando a marcação XAML. A marcação inclui expressões de associação de dados (por exemplo, [x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) que definem a conexão entre vários membros de modelo de exibição e o modelos e componentes específicos de interface do usuário. Arquivos code-behind às vezes são usados como parte da camada de modo de exibição para conter código adicional necessário para personalizar ou manipular a interface do usuário, ou para extrair os dados dos argumentos de manipulador de evento antes de chamar um método de modelo de exibição que executa o trabalho. 
* A camada de **modelo de exibição** fornece os destinos de vinculação de dados para o modo de exibição. Em muitos casos, o modelo de exibição expõe o modelo diretamente ou fornece membros que encapsulam membros de modelo específico. O modelo de exibição também pode definir membros para controlar de dados que são relevantes para a interface do usuário, mas não para o modelo, como a ordem de exibição de uma lista de itens. O modelo de exibição também serve como um ponto de integração com outros serviços como o código de acesso de banco de dados. Para projetos simples, talvez você não precise uma camada de modelo separado, mas apenas um modelo de exibição que encapsula todos os dados que necessários. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básico e avançado

Assim como acontece com qualquer padrão de design, não há mais de uma maneira de implementar MVVM e muitas técnicas diferentes são consideradas parte do MVVM. Por esse motivo, há várias estruturas MVVM de terceiros diferentes dando suporte as várias plataformas com base em XAML, incluindo a UWP. No entanto, essas estruturas geralmente incluem vários serviços para a implementação de arquitetura dissociada, tornando a definição exata de MVVM um pouco ambíguo. 

Embora estruturas MVVM sofisticadas podem ser muito úteis, especialmente para projetos de grande porte, normalmente há um custo associado adotando qualquer padrão específico ou técnica e os benefícios não são sempre claros, dependendo da escala e o tamanho de seu projeto. Felizmente, você pode adotar somente essas técnicas que fornecem um benefício tangível e claro e Ignorar outros até que você precisa deles. 

Em particular, você pode obter muitos benefícios bastando compreensão e aplicação de toda a capacidade de associação de dados e separar a lógica do seu aplicativo em camadas descritas anteriormente. Isso pode ser alcançado usando somente os recursos fornecidos pelo SDK do Windows e sem o uso de estruturas de dados externas. Em particular, a [extensão de marcação {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) facilita a vinculação de dados e maior desempenho que em plataformas anteriores de XAML, eliminando a necessidade de muito código clichê necessárias anteriormente.

Para obter diretrizes adicionais sobre como usar o MVVM básico, out-of-a-box, confira a [amostra do banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) no GitHub. Muitas das outras [amostras de aplicativos UWP](https://github.com/Microsoft?q=windows-appsample
) também usam uma arquitetura MVVM básica e o [exemplo de aplicativo de tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp) inclui code-behind e versões MVVM, com as anotações que descrevem a [conversão MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Consulte também

### <a name="topics"></a>Tópicos

[Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Exemplos

[Exemplo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Amostra de inventário de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemplo de aplicativo de tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp)  
