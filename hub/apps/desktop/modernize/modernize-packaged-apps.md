---
Description: Saiba como adicionar experiências modernas para usuários do Windows 10 em um aplicativo da área de trabalho que você inseriu em um pacote do aplicativo do Windows.
title: Modernizar aplicativos empacotados da área de trabalho
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 1930d879177bc9282a3b55d019aa2bef7eb8f120
ms.sourcegitcommit: ef723e3d6b1b67213c78da696838a920c66d5d30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/02/2020
ms.locfileid: "82730087"
---
# <a name="features-that-require-package-identity"></a>Recursos que exigem a identidade do pacote

Se você quiser atualizar seu aplicativo da área de trabalho com [experiências modernas do Windows 10](index.md), muitos recursos estarão disponíveis somente em aplicativos de área de trabalho que tenham [identificador de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity). Há várias maneiras de conceder o identificador de pacote a um aplicativo da área de trabalho:

* Empacote-o em um [pacote MSIX](/windows/msix/desktop/desktop-to-uwp-root). MSIX é um formato moderno de pacote de aplicativo que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, como aplicativos WPF, Windows Forms e Win32. Ele fornece a você acesso a uma experiência robusta de instalação e atualização, um modelo de segurança gerenciado com um sistema de capacidade flexível, suporte à Microsoft Store, gerenciamento corporativo e muitos modelos de distribuição personalizados. Para obter mais informações, consulte [empacotar aplicativos da área de trabalho](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) na documentação do MSIX.
* Se você não puder adotar o empacotamento MSIX para implantar seu aplicativo da área de trabalho, no Windows 10, versão 2004 em diante, conceda o identificador de pacote criando um *pacote MSIX esparso* que contenha apenas um manifesto do pacote. Para obter mais informações, consulte [Conceder identidade a aplicativos da área de trabalho não empacotados](grant-identity-to-nonpackaged-apps.md).

Se o aplicativo da área de trabalho tiver um identificador de pacote, você poderá usar os recursos a seguir no aplicativo.

## <a name="use-windows-runtime-apis-that-require-package-identity"></a>Usar APIs do Windows Runtime que exigem o identificador de pacote

A lista de APIs do Windows Runtime a seguir exige que o identificador de pacote seja usado em um aplicativo da área de trabalho: [lista de APIs](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Caso o aplicativo precise se integrar ao sistema (por exemplo, estabelecer regras de firewall), descreva os itens no manifesto do pacote do aplicativo e o sistema vai se encarregar do resto. Não é necessário escrever códigos para a maioria dessas tarefas. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário fizer logon, integrar o aplicativo no Explorador de Arquivos e adicionar nele uma lista de destinos de impressão que aparecem em outros aplicativos.

Para obter mais informações, confira [Integrar seu aplicativo da área de trabalho a extensões de pacote](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de aplicativo moderno. Normalmente, primeiro é necessário determinar se é possível adicionar sua experiência [aprimorando](desktop-to-uwp-enhance.md) o aplicativo da área de trabalho existente com APIs do Windows Runtime. Caso seja necessário usar um componente UWP para obter a experiência, será possível adicionar um projeto UWP à solução e usar os serviços de aplicativo para fazer a comunicação entre o aplicativo da área de trabalho e o componente UWP.

Para obter mais informações, confira [Estender seu aplicativo da área de trabalho com componentes UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuir

Se você empacotar seu aplicativo em um pacote MSIX, poderá distribuí-lo publicando-o na Microsoft Store ou fazendo o sideload para outros sistemas.

Confira [Distribuir seu aplicativo da área de trabalho empacotado](desktop-to-uwp-distribute.md).
