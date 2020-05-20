---
title: Lista de pacotes NuGet da Biblioteca de Interface do Usuário do Windows
description: Lista os pacotes NuGet da Biblioteca de Interface do Usuário do Windows
ms.topic: article
ms.date: 04/15/2020
keywords: windows 10, uwp, sdk do kit de ferramentas
ms.openlocfilehash: 2bda405977733a6191c4434fd8bd2c63b2ce10ce
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580703"
---
# <a name="windows-ui-library-nuget-packages"></a>Pacotes NuGet da Biblioteca de Interface do Usuário do Windows

O NuGet é um gerenciador de pacotes padrão para aplicativos .Net incorporado ao Visual Studio. Na sua solução aberta, escolha o menu *Ferramentas*, *Gerenciador de Pacotes NuGet*, *Gerenciar pacotes NuGet para a solução…* para abrir a interface do usuário.  Insira um dos nomes de pacote abaixo para procurá-lo online.

| Nome do pacote NuGet | Descrição |
| --- | --- |
| [Microsoft.UI.Xaml](https://www.nuget.org/packages/Microsoft.UI.Xaml/) | Controles para aplicativos UWP. Inclui APIs destes namespaces: [Microsoft.UI.Xaml](/uwp/api/microsoft.ui.xaml), [Microsoft.UI.Xaml.Automation.Peers](/uwp/api/microsoft.ui.xaml.automation.peers), [Microsoft.Ui.Xaml.Controls](/uwp/api/microsoft.ui.xaml.controls), [Microsoft.UI.Xaml.Controls.Primitives](/uwp/api/microsoft.ui.xaml.controls.primitives), [Microsoft.UI.Xaml.CustomAttributes](/uwp/api/microsoft.ui.xaml.customattributes), [Microsoft.UI.Xaml.Media](/uwp/api/microsoft.ui.xaml.media), [Microsoft.Ui.Xaml.XamlTypeInfo](/uwp/api/microsoft.ui.xaml.xamltypeinfo) |
| [Microsoft.UI.Xaml.Core.Direct](https://www.nuget.org/packages/Microsoft.UI.Xaml.Core.Direct) | Permite que você use APIs de [XamlDirect](/uwp/api/microsoft.ui.xaml.core.direct.xamldirect) em versões anteriores do Windows 10 sem a necessidade de escrever código especial para lidar com várias versões de destino do Windows 10. |


## <a name="search-in-visual-studio"></a>Pesquisar no Visual Studio

Ao pesquisar no gerenciador de pacotes do Visual Studio, você verá uma lista semelhante a esta (os números das versões podem ser diferentes, mas os nomes serão os mesmos).

![](images/NugetPackages.png)

## <a name="update-nuget-packages"></a>Atualizar Pacotes NuGet

Atualizamos regularmente a Biblioteca de Interface do Usuário do Windows com novos controles, serviços, APIs e, mais importante, correções de bugs. Para verificar se você tem a versão mais recente, abra o projeto no Visual Studio, escolha o menu **Ferramentas**, selecione **Gerenciador de Pacotes NuGet** -> **Gerenciar pacotes NuGet para a solução…** e selecione a guia *Atualizações*. Selecione o pacote que você quer atualizar e clique em Instalar para atualizar para a versão mais recente.

## <a name="getting-started"></a>Introdução

Leia [Introdução à Biblioteca de Interface do Usuário do Windows](getting-started.md) para ver mais instruções sobre como usar esses Pacotes NuGet em seus projetos.
