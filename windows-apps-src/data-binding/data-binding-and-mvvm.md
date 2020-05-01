---
ms.assetid: F46306EC-DFF3-4FF0-91A8-826C1F8C4A52
title: Vinculação de dados e MVVM
description: A associação de dados está no núcleo do padrão de design de arquitetura da interface do usuário do MVVM (Model-View-ViewModel) e permite um acoplamento flexível entre a interface do usuário e código que não é da interface do usuário.
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 931f2fcbcdbf58b9dc2ca40403d7466b620a8991
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "63798111"
---
# <a name="data-binding-and-mvvm"></a>Vinculação de dados e MVVM

O MVVM (Model-View-ViewModel) é um padrão de design de arquitetura de interface do usuário para desacoplamento da interface do usuário e de código que não é da interface do usuário. Com o MVVM, você define sua interface do usuário declarativamente em XAML e usa a marcação da associação de dados para vinculá-la a outras camadas que contêm dados e comandos. A infraestrutura de associação de dados fornece um acoplamento flexível que mantém a interface do usuário e os dados vinculados sincronizados e roteia a entrada de usuários para os comandos apropriados. 

Como oferece acoplamento flexível, o uso da associação de dados reduz as dependências rígidas entre diferentes tipos de código. Isso facilita a alteração de unidades de código individuais (métodos, classes, controles etc.) sem causar efeitos colaterais indesejados em outras unidades. Esse desacoplamento é um exemplo da *separação de preocupações*, que é um conceito importante em muitos padrões de design. 

## <a name="benefits-of-mvvm"></a>Benefícios do MVVM

O desacoplamento do código tem muitos benefícios, o que inclui:

* Habilitar um estilo de codificação iterativo e exploratório. A alteração isolada é menos arriscada e mais fácil de experimentar.
* Simplificar testes de unidade. As unidades de código isoladas umas das outras podem ser testadas individualmente e fora dos ambientes de produção.
* Dar suporte à equipe de colaboração. O código desacoplado que segue interfaces bem projetadas pode ser desenvolvido por indivíduos ou equipes separadas e integrado posteriormente.
* Aumentar a facilidade de manutenção. É menos provável que a correção de bugs em código desacoplado cause regressões em outro código.

Em contraste com o MVVM, um aplicativo com uma estrutura "code-behind" mais convencional normalmente usa a associação de dados somente em dados de exibição e responde à entrada do usuário manipulando diretamente os eventos expostos pelos controles. Os manipuladores de eventos são implementados em arquivos code-behind (como MainPage.xaml.cs) e costumam ser acoplados de forma rígida aos controles e conter código que manipula diretamente a interface do usuário. Isso dificulta ou impossibilita a substituição de um controle sem ter que atualizar o código de manipulação de eventos. Com essa arquitetura, os arquivos code-behind geralmente acumulam o código que não está relacionado de forma direta à interface do usuário, como o código de acesso ao banco de dados, que acaba sendo duplicado e modificado para uso com outras páginas.

## <a name="app-layers"></a>Camadas de aplicativo

Ao usar o padrão MVVM, um aplicativo é dividido nas seguintes camadas:

* A camada **model** define os tipos que representam seus dados corporativos. Isso inclui tudo o que é necessário para modelar o domínio principal do aplicativo e, muitas vezes, também inclui a lógica do aplicativo principal. Essa camada é completamente independente das camadas view e view-model e, muitas vezes, reside parcialmente na nuvem. Dada uma camada de modelo totalmente implementada, você poderá criar vários aplicativos cliente diferentes, se preferir, como aplicativos UWP e Web que funcionam com os mesmos dados subjacentes.
* A camada **view** define a interface do usuário usando marcação XAML. A marcação inclui expressões de associação de dados (como [x:Bind](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)) que definem a conexão entre componentes específicos da interface do usuário e vários membros de model e view-model. Os arquivos code-behind às vezes são usados como parte da camada de exibição a fim de conter código adicional necessário para personalizar ou manipular a interface do usuário ou extrair dados de argumentos do manipulador de eventos antes de chamar um método view-model que executa o trabalho. 
* A camada **view-model** fornece destinos de associação de dados para a exibição. Em muitos casos, view-model expõe o modelo diretamente ou fornece membros que encapsulam membros específicos do modelo. O view-model também pode definir membros para manter o controle dos dados que forem relevantes para a interface do usuário, mas não para o modelo, como a ordem de exibição de uma lista de itens. O view-model também serve como um ponto de integração com outros serviços, como o código de acesso ao banco de dados. Para projetos simples, talvez você não precise de uma camada de modelo separada, mas apenas do view-model que encapsula todos os dados necessários. 

## <a name="basic-and-advanced-mvvm"></a>MVVM básico e avançado

Como em qualquer padrão de design, há mais de uma maneira de implementar o MVVM, e várias técnicas diferentes são consideradas parte do MVVM. Por esse motivo, há várias estruturas de MVVM de terceiros diferentes que dão suporte a várias plataformas baseadas em XAML, incluindo a UWP. No entanto, essas estruturas geralmente incluem vários serviços para implementar a arquitetura desacoplada, tornando a definição exata do MVVM um pouco ambígua. 

Embora estruturas sofisticadas de MVVM possam ser muito úteis, especialmente para projetos em escala empresarial, costuma haver um custo associado com a adoção de determinado padrão ou técnica, e nem sempre os benefícios são claros, dependendo da escala e do tamanho do projeto. Felizmente, você pode adotar apenas as técnicas que fornecem um benefício claro e tangível e ignorar outras até precisar delas. 

Em particular, você pode obter muitos benefícios ao compreender e aplicar todo o poder da associação de dados e separar a lógica do aplicativo nas camadas descritas anteriormente. Isso pode ser feito usando apenas os recursos fornecidos pelo SDK do Windows e sem usar estruturas externas. Em especial, a [extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension) torna a associação de dados mais fácil e com melhor desempenho do que nas plataformas XAML anteriores, eliminando a necessidade de muitos códigos genéricos que eram necessários anteriormente.

Para obter orientações adicionais sobre como usar o MVVM básico pronto para uso, confira o [Exemplo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database) no GitHub. Muitos dos outros [exemplos de aplicativo UWP](https://github.com/Microsoft?q=windows-appsample
) também usam uma arquitetura MVVM básica, e o [Exemplo de Aplicativo de Tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp) inclui versões de code-behind e MVVM, com observações que descrevem a [conversão de MVVM](https://github.com/Microsoft/Windows-appsample-trafficapp/blob/MVVM/MVVM.md). 

## <a name="see-also"></a>Veja também

### <a name="topics"></a>Tópicos

[Vinculação de dados em detalhes](https://docs.microsoft.com/windows/uwp/data-binding/data-binding-in-depth)  
[Extensão de marcação {x:Bind}](https://docs.microsoft.com/windows/uwp/xaml-platform/x-bind-markup-extension)  

### <a name="samples"></a>Amostras

[Exemplo de banco de dados de pedidos de clientes](https://github.com/Microsoft/Windows-appsample-customers-orders-database)  
[Exemplo de inventário de VanArsdel](https://github.com/Microsoft/InventorySample)  
[Exemplo de aplicativo de tráfego](https://github.com/Microsoft/Windows-appsample-trafficapp)  
