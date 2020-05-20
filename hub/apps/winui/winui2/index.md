---
title: Biblioteca de Interface do Usuário do Windows
description: Fornece informações sobre o desenvolvimento de aplicativos WinUI 2.x e do Windows.
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, sdk de kit de ferramentas, winui, Biblioteca de Interface do Usuário do Windows
ms.custom: RS5
ms.openlocfilehash: 9396860ac82db92f9a8f3166662b94f2776fed7d
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580243"
---
# <a name="windows-ui-library-2x"></a>Biblioteca de Interface do Usuário do Windows 2.x

![Controles de WinUI](images/winUI-library-767.png)

A Biblioteca de Interface do Usuário do Windows fornece controles nativos oficiais da Interface do Usuário do Windows, bem como outros elementos de interface do usuário para aplicativos do Windows.

Ela mantém a compatibilidade com versões anteriores do Windows 10 para que seu aplicativo funcione mesmo que os usuários não tenham o sistema operacional mais recente.

> [!NOTE]
> Confira o [WinUI 3.0 Alfa](../winui3/index.md), uma grande atualização da plataforma de interface do usuário do Windows 10 planejada para lançamento em 2020.

## <a name="features"></a>Recursos

* **Novos controles**: a Biblioteca de Interface do Usuário do Windows contém novos controles que não são fornecidos como parte da plataforma padrão do Windows.

* **Versões atualizadas de controles existentes**: a biblioteca também contém versões atualizadas de controles existentes da plataforma Windows que você pode usar com versões anteriores do Windows 10.

* **Suporte para versões anteriores do Windows 10**: as APIs da Biblioteca de Interface do Usuário do Windows funcionam em versões anteriores do Windows 10, portanto, você não precisa incluir verificações de versão ou XAML condicional para dar suporte a usuários que podem não estar executando o SO mais recente.

* **Suporte para XamlDirect**: as APIs de Xaml Direct, projetadas para desenvolvedores de middleware, oferecem acesso a recursos XAML de nível inferior que proporcionam melhor desempenho de CPU e conjunto de trabalho. O XamlDirect permite que você use APIs de XamlDirect em versões anteriores do Windows 10 sem a necessidade de escrever código especial para lidar com várias versões de destino do Windows 10.

## <a name="examples"></a>Exemplos

O aplicativo de exemplo da Galeria de Controles XAML inclui demonstrações interativas e código de exemplo para usar controles WinUI.

* Instale o aplicativo da Galeria de Controles XAML da [Microsoft Store](
https://www.microsoft.com/p/xaml-controls-gallery/9msvh128x2zt)

* A Galeria de Controles XAML também é de [software livre no GitHub](
https://github.com/Microsoft/Xaml-Controls-Gallery)

## <a name="documentation"></a>Documentação

Artigos de instruções sobre os controles da Biblioteca de Interface do Usuário do Windows estão incluídos com a [Documentação de controles da Plataforma Universal do Windows](/windows/uwp/design/controls-and-patterns/).

Os documentos da referência de API estão localizados aqui: [APIs da Biblioteca de Interface do Usuário do Windows](/uwp/api/overview/winui/).

## <a name="install-and-use-the-windows-ui-library"></a>Instalar e usar a Biblioteca de Interface do Usuário do Windows

Para obter instruções, confira [Introdução à Biblioteca de Interface do Usuário do Windows](getting-started.md).

## <a name="open-source-and-developer-roadmap"></a>Roteiro de software livre e desenvolvedor

O WinUI é um projeto de software livre hospedado no GitHub. Relatórios de bugs, solicitações de recursos e contribuições de código da comunidade são bem-vindos no [Repositório da Biblioteca de Interface do Usuário do Windows](https://aka.ms/winui).

Continuamos desenvolvendo e aprimorando o WinUI para dar suporte a mais cenários de desenvolvedor. Para obter os detalhes mais recentes sobre nossos planos para o WinUI, confira nosso [roteiro](https://github.com/microsoft/microsoft-ui-xaml/blob/master/docs/roadmap.md) no repositório da Biblioteca de Interface do Usuário do Windows.

## <a name="nuget-package-list"></a>Lista de pacotes NuGet

A Biblioteca de Interface do Usuário do Windows contém vários pacotes NuGet: [Lista de pacotes NuGet da Biblioteca de Interface do Usuário do Windows](nuget-packages.md).

## <a name="see-also"></a>Veja também

[Notas sobre a versão da Biblioteca de Interface do Usuário do Windows 2.x](release-notes/index.md)