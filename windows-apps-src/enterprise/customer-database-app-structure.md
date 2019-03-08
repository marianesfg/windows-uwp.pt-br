---
title: Estrutura de aplicativo de banco de dados do cliente
description: Examine a estrutura do tutorial do banco de dados do cliente, e o motivo pelo qual ele foi construído como ele é.
keywords: Enterprise, tutorial, cliente, dados, crud
ms.date: 05/07/2018
ms.topic: article
ms.localizationpriority: med
ms.openlocfilehash: b1f8f8c8a2fd1522d8c304a45514d5257543f222
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57656611"
---
# <a name="customer-database-app-structure"></a>Estrutura de aplicativo de banco de dados do cliente

Aplicativos de linha de negócios complexos geralmente têm muitas páginas e recursos e uma ótima várias linhas de código. Por isso, é de vital importância que você crie seu aplicativo em torno de uma estrutura previsível. Há vários padrões de design de aplicativo que são adequados para aplicativos empresariais, mas eles são criados em todo o objetivo de tornar mais fácil de entender e trabalhar com um aplicativo de larga escala.

Enquanto o [tutorial de banco de dados do cliente](customer-database-tutorial.md) apresenta um aplicativo de página única para simplificar, ele implementa o padrão de design de aplicativo Model-View-ViewModel (MVVM) para demonstrar essas ideias em ação. Como o nome sugere, o padrão de design MVVM separa a lógica do aplicativo principal em três categorias:

* Os modelos são as classes que contêm os dados do aplicativo.
* As exibições são a interface do usuário de uma determinada página.
* ViewModels fornecer a lógica do aplicativo. Isso pode incluir o tratamento de ações do usuário do modo de exibição e/ou gerenciamento de interações com os modelos.

Enquanto este aplicativo não é um exemplo perfeito e architypical do MVVM, mostra os principais princípios de separação de preocupações em ação. [Confira aqui, o aplicativo.](https://github.com/Microsoft/windows-tutorials-customer-database)

## <a name="application-structure"></a>Estrutura de aplicativo

Depois de abrir o aplicativo, começar examinando o **Gerenciador de soluções.** Alguns dos que você vê, deve haver familiar se você já trabalhou com um aplicativo UWP antes, mas você também verá uma coleção de pastas que armazenam as partes do componente do aplicativo.

![Aplicativo de ponto de partida no Gerenciador de soluções](images/customer-database-tutorial/solution-explorer.png)

### <a name="views"></a>Modos de exibição

Interface do usuário de todas as do aplicativo é definido na pasta de modos de exibição. Como nosso tutorial é um aplicativo de página única no momento, isso significa que há apenas uma exibição - **CustomerListPage**. Ele tem a marcação XAML UI e xaml.cs de lógica - esses dois arquivos formam um modo de exibição. Você adicionará os elementos de interface do usuário para **CustomerListPage.xaml**.

> [!NOTE]
> Você pode notar que este aplicativo não tem uma MainPage. Isso ocorre porque podemos especificar em **App.xaml.cs** que o aplicativo deve inicializar **CustomerListPage** no início.

### <a name="viewmodels"></a>ViewModels

Embora esse aplicativo tem apenas um modo de exibição, tem duas ViewModels. Por que é isso?

**CustomerListPageViewModel.cs** é um padrão ViewModel do padrão MVVM. É onde a lógica fundamental da página do aplicativo está localizada e a página você trabalhará com a maior parte do tutorial. Cada ação de interface do usuário tomada pelo usuário é passada por meio da exibição para esse ViewModel para processamento.

**CustomerViewModel.cs**, no entanto, não está associado a qualquer modo de exibição específico. Em vez disso, ele associa um conceito de programação (quais propriedades foram editadas) com os dados contidos no modelo individual do cliente.

### <a name="models"></a>Modelos

Este aplicativo contém três modelos, que armazenam os dados do aplicativo e fornecem interfaces para interagir com o repositório. Embora essas sejam as partes essenciais do aplicativo, eles não são algo que você vai editar diretamente no tutorial.

É mais importante **Customer.cs**, que descreve a estrutura de dados do cliente que você usará no tutorial.

> [!NOTE]
> O tutorial ignora a *Email* e *Phone* propriedades do objeto do cliente. Se você quiser ir além do que é apresentado aqui, adicionar essas duas propriedades a interface do usuário do aplicativo é uma boa primeira etapa.

### <a name="repository"></a>Repositório

A pasta do repositório contém classes que construir e interagem com o banco de dados SQLite local. Para o tutorial, o banco de dados SQLite é apresentado como-está. Enquanto você adicionará código para **CustomerListPageViewModel.cs** para chamar os métodos definidos por essas classes, você não precisa fazer nenhuma alteração para configurá-los.

Para obter mais informações em SQLite na UWP, [consulte este artigo](../data-access/sqlite-databases.md).

Se você tentar a seção "Continuar" do tutorial, isso é onde você criará uma classe para se conectar ao banco de dados remoto do REST. Ele também implementará o **ICustomerRepository** interface definida na seção de modelos, mas ele se parecerá muito diferente de sua contraparte do SQLite.

### <a name="other-elements"></a>Outros elementos

Como é comum para aplicativos UWP, o comportamento de inicialização do aplicativo é definido na **App.xaml.cs** classe. A maior parte do código aqui é o código padrão para qualquer aplicativo da UWP. Mas já fizemos algumas pequenas alterações:

* Especificamos que o aplicativo deve exibir **CustomerListPage** na inicialização.
* Criamos um objeto de repositório, que manterá a fonte de dados que estamos usando.
* Adicionamos uma **SQLiteDatabase** método, que inicializa o banco de dados local e o define como o repositório especificado.

Se você tentar a seção "Continuar", você adicionará um método semelhante para inicializar um objeto de repositório de REST. Como queremos tenha separado nossas preocupações e estiver usando a mesma interface definida para operações de SQLite e REST, esse será o único código existente, que você precisará ser alterado para usar o REST em vez do SQLite em seu aplicativo.

## <a name="next-steps"></a>Próximas etapas

Se você já concluiu o tutorial, você pode conferir a [aplicativo de exemplo completo](https://github.com/Microsoft/Windows-appsample-customers-orders-database) para ver como esses recursos são implementados em uma escala maior.

Caso contrário, agora que você sabe por que tudo o que é onde ele está, você deve [para o tutorial retorno](customer-database-tutorial.md) e trabalhe com a estrutura que abordamos apenas.