---
Description: Saiba como adicionar experiências modernas para usuários do Windows 10 em um aplicativo de área de trabalho que você pacoteu em um pacote de aplicativo do Windows.
title: Modernizar aplicativos de desktop empacotados
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: ec1c56f205b270262f618ffb46db16b38276975d
ms.sourcegitcommit: d7eccdb27c22bccac65bd014e62b6572a6b44602
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2019
ms.locfileid: "73142512"
---
# <a name="features-that-require-package-identity"></a>Recursos que exigem a identidade do pacote

Se você quiser atualizar seu aplicativo de desktop com [experiências modernas do Windows 10](index.md), muitos recursos estarão disponíveis apenas em aplicativos de área de trabalho que têm a [identidade do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Há várias maneiras de conceder identidade de pacote a um aplicativo de área de trabalho:

* Empacote-o em um [pacote MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX é um formato de pacote de aplicativo moderno que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, WPF, Windows Forms e aplicativos Win32. Ele fornece uma experiência de instalação e atualização robusta, um modelo de segurança gerenciado com um sistema de funcionalidade flexível, suporte para o Microsoft Store, gerenciamento corporativo e muitos modelos de distribuição personalizados. Para obter mais informações, consulte [empacotar aplicativos da área de trabalho](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) na documentação do MSIX.
* Se não for possível adotar o empacotamento MSIX para implantar seu aplicativo de área de trabalho, começando no Build do Windows 10 Insider Preview 10.0.19000.0, você poderá conceder a identidade do pacote criando um *pacote MSIX esparso* que contenha apenas um manifesto de pacote. Para obter mais informações, consulte [conceder identidade a aplicativos de área de trabalho não empacotados](grant-identity-to-nonpackaged-apps.md).

Se seu aplicativo de área de trabalho tiver uma identidade de pacote, você poderá usar os recursos a seguir em seu aplicativo.

## <a name="use-uwp-apis-that-require-package-identity"></a>Usar APIs UWP que exigem a identidade do pacote

A lista de APIs UWP a seguir exige que a identidade do pacote seja usada em um aplicativo de área de trabalho: [lista de APIs](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Se seu aplicativo precisar ser integrado ao sistema (por exemplo: estabelecer regras de firewall), descreva as coisas no manifesto do pacote do seu aplicativo e o sistema fará o restante. Para a maioria dessas tarefas, você não precisará escrever qualquer código. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário faz logon, integrar seu aplicativo no explorador de arquivos e adicionar seu aplicativo a uma lista de destinos de impressão que aparecem em outros aplicativos.

Para obter mais informações, consulte [integrar seu aplicativo de área de trabalho com extensões de pacote](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Em geral, você deve primeiro determinar se pode adicionar sua experiência [aprimorando](desktop-to-uwp-enhance.md) seu aplicativo de área de trabalho existente com APIs UWP. Se você precisar usar um componente UWP, para obter a experiência, poderá adicionar um projeto UWP à sua solução e usar os serviços de aplicativos para se comunicar entre o aplicativo da área de trabalho e o componente UWP.

Para obter mais informações, consulte [estender seu aplicativo de área de trabalho com componentes UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuir

Se você empacotar seu aplicativo em um pacote MSIX, poderá distribuí-lo publicando-o na Microsoft Store ou fazendo o Sideload para outros sistemas.

Consulte [distribuir seu aplicativo de área de trabalho empacotado](desktop-to-uwp-distribute.md).
