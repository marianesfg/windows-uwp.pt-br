---
author: KarlErickson
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Vinculação de dados e MVVM
description: Vinculação de dados é o núcleo do padrão de design de arquitetura Model-View-ViewModel (MVVM) da interface do usuário e permite que o acoplamento entre o código de interface do usuário e não da interface do usuário.
ms.author: karler
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: eda370db8b68232066052cca3d0abfa6e3876167
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5408064"
---
# <a name="data-binding-and-mvvm"></a>Vinculação de dados e MVVM

Model-View-ViewModel (MVVM) é um padrão de design de arquitetura da interface do usuário para a separação de código de interface do usuário e não da interface do usuário. Com o MVVM, defina sua interface do usuário declarativamente em XAML e usar a marcação de associação de dados para vinculá-lo a outras camadas que contêm dados e comandos. A infraestrutura de associação de dados fornece um acoplamento que mantém a interface do usuário e os dados vinculados sincronizados e roteia entrada do usuário para os comandos apropriados. 

Porque ele fornece acoplamento, o uso da vinculação de dados reduz o disco rígidas dependências entre diferentes tipos de código. Isso torna mais fácil alterar as unidades de código individuais (métodos, classes, controles, etc.) sem causar efeitos colaterais indesejados em outras unidades. Essa separação é um exemplo da *separação de preocupações*, que é um conceito importante em muitos padrões de design. 

## <a name="benefits-of-mvvm"></a>Benefícios do MVVM

Separação seu código tem muitos benefícios, incluindo:

* Habilitando um estilo de codificação iterativo, exploratório. Alteração que é isolada é menos arriscado e mais fácil de experimentar.
* Testes de unidade pressuposições. Unidades de código que são isoladas uns dos outros podem ser testadas individualmente e fora ambientes de produção.
* Oferece suporte a colaboração em equipe. Código dissociado que segue interfaces bem projetadas pode ser desenvolvido pela indivíduos separados ou equipes e integrado mais tarde.
* Melhorando a capacidade de manutenção. Corrigir bugs no código dissociado é menor probabilidade de causar regressões em outro código.

Ao contrário da MVVM, um aplicativo com uma estrutura de "code-behind" mais convencional normalmente usa ligação de dados para dados somente para exibição e responde à entrada do usuário manipulando eventos expostos pelos controles diretamente. Os manipuladores de eventos são implementados em arquivos code-behind (por exemplo, MainPage.xaml.cs) e estão geralmente intimamente ligados para os controles, normalmente que contém o código que manipula a interface do usuário diretamente. Isso torna difícil ou impossível substituir um controle sem precisar atualizar o código de manipulação de eventos. Com essa arquitetura, arquivos code-behind geralmente acumulam código que não está diretamente relacionado à interface do usuário, como o código de acesso de banco de dados, o que acaba sendo duplicada e modificados para uso com outras páginas.

## <a name="app-layers"></a>Camadas do aplicativo

Ao usar o padrão MVVM, um aplicativo é dividido em camadas a seguir:

* A camada do **modelo** define os tipos que representam seus dados corporativos. Isso inclui tudo que é necessário para modelar o domínio do aplicativo principal e geralmente inclui lógica principal do aplicativo. Essa camada é completamente independente do modo de exibição e camadas do modelo de exibição e geralmente reside parcialmente na nuvem. Dada uma camada de modelo totalmente implementado, você pode criar vários cliente diferente aplicativos se preferir, como aplicativos UWP e web que funcionam com os mesmos dados subjacentes.
* A camada de **modo de exibição** define a interface do usuário usando a marcação XAML. A marcação inclui expressões de associação de dados (por exemplo, [x: Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) que definem a conexão entre vários membros do modelo de exibição e o modelos e componentes de interface do usuário específicos. Arquivos code-behind às vezes são usados como parte da camada de modo de exibição para conter o código adicional necessário para personalizar ou manipular a interface do usuário, ou para extrair os dados de argumentos de manipulador de eventos antes de chamar um método de modelo de exibição que executa o trabalho. 
* A camada de **modelo de exibição** fornece os destinos de vinculação de dados para o modo de exibição. Em muitos casos, o modelo de exibição expõe o modelo diretamente ou fornece membros que encapsulam membros do modelo de específicas. O modelo de exibição também pode definir membros para controlar os dados que são relevantes para a interface do usuário, mas não para o modelo, como a ordem de exibição de uma lista de itens. O modelo de exibição também serve como um ponto de integração com outros serviços como o código de acesso de banco de dados. Para projetos simples, talvez você não precise uma camada de modelo separado, mas apenas um modelo de exibição que encapsula todos os dados que você precisa. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básico e avançado

Assim como acontece com qualquer padrão de design, há mais de uma maneira de implementar o MVVM, e muitas técnicas diferentes são consideradas parte do MVVM. Por esse motivo, há várias estruturas diferentes de MVVM de terceiros com suporte as várias plataformas com base em XAML, incluindo a UWP. No entanto, essas estruturas geralmente incluem vários serviços para a implementação de arquitetura dissociada, tornando a definição exata de MVVM um pouco ambíguo. 

Embora sofisticadas estruturas MVVM podem ser muito úteis, especialmente para projetos de grande porte, normalmente há um custo associado à adoção qualquer padrão específico ou técnica, e os benefícios não são sempre claros, dependendo da escala e o tamanho do seu projeto. Felizmente, você pode adotar somente essas técnicas que fornecem um benefício tangível e claro e Ignorar outros até que você precisa deles. 

Em particular, você pode obter muitos benefícios bastando compreensão e aplicação de toda a capacidade de associação de dados e separar a lógica do seu aplicativo em camadas descritas anteriormente. Isso pode ser alcançado usando somente os recursos fornecidos pelo SDK do Windows e sem o uso de estruturas de dados externas. Em particular, a [extensão de marcação {x: Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) facilita a vinculação de dados e maior desempenho que em plataformas anteriores do XAML, eliminando a necessidade de uma grande parte do código clichê necessário anteriormente.

Para obter diretrizes adicionais sobre como usar o MVVM básico, out-of-a-box, confira a [amostra do banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) no GitHub. Muitas das outras [amostras de aplicativos UWP](https://github.com/Microsoft?q=windows-appsample
) também usam uma arquitetura MVVM básica e o [exemplo de aplicativo de tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp) inclui code-behind e versões MVVM, com as anotações que descrevem a [conversão MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Consulte também

### <a name="topics"></a>Tópicos

[Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Exemplos

[Exemplo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Amostra de estoque VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemplo de aplicativo de tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp)  
