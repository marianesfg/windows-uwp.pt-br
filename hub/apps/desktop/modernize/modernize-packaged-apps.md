---
Description: Saiba como adicionar experiências modernas para usuários do Windows 10 em um aplicativo da área de trabalho que você empacotadas em um pacote de aplicativo do Windows.
title: Modernize aplicativos empacotados de área de trabalho
ms.date: 04/22/2019
ms.topic: article
keywords: windows 10, uwp
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 191a8b8a007a866f37780a7c52cd40047dc9817f
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66215205"
---
# <a name="features-that-require-package-identity"></a>Recursos que exigem a identidade do pacote

Se você quiser atualizar seu aplicativo da área de trabalho com [experiências do Windows 10 moderno](index.md), muitos recursos estão disponíveis apenas em aplicativos da área de trabalho que são empacotados em um pacote MSIX.

MSIX é um formato de pacote de aplicativo Windows moderno que fornece uma experiência de empacotamento universal para todos os aplicativos do Windows, WPF, Windows Forms e aplicativos do Win32. Empacotar seus aplicativos da área de trabalho do Windows permite que você integre experiências modernas do Windows 10 como blocos dinâmicos e notificações em seus aplicativos. Ele também obtém acesso a uma instalação robusta e experiência de atualização, um modelo de segurança gerenciados com um sistema de capacidade flexível, suporte para a Microsoft Store, gerenciamento empresarial e muitos modelos de distribuição personalizada. Para obter mais informações, consulte [empacotar aplicativos da área de trabalho](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) na documentação do MSIX.

Se você empacotar seu aplicativo da área de trabalho, você pode usar APIs de UWP que exigem a identidade do pacote, pacote extensões e componentes do UWP em seu aplicativo empacotado. Para obter mais informações, consulte estes artigos.

## <a name="use-uwp-apis-that-require-package-identity"></a>Use as APIs de UWP que exigem a identidade do pacote

Algumas APIs de UWP requerem [identidade de pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) a ser usado em um aplicativo da área de trabalho. O pacote MSIX (incluindo o manifesto do pacote) fornece essa identidade.

Para obter mais informações, consulte [essa lista de APIs](desktop-to-uwp-supported-api.md#list-of-apis).

## <a name="integrate-with-package-extensions"></a>Integrar com extensões de pacote

Se seu aplicativo precisa integrar o sistema (por exemplo: estabelecer regras de firewall), descrevem essas coisas no manifesto do pacote do aplicativo e o sistema fará o resto. Para a maioria dessas tarefas, você não precisará escrever qualquer código. Com um pouco de XML no manifesto, você pode fazer coisas como iniciar um processo quando o usuário fizer logon, integrar seu aplicativo no Explorador de arquivos e adicionar seu aplicativo uma lista de destinos de impressão que aparecem em outros aplicativos.

Para obter mais informações, consulte [integrar seu aplicativo da área de trabalho com extensões do pacote](desktop-to-uwp-extensions.md).

## <a name="extend-with-uwp-components"></a>Estender com componentes UWP

Algumas experiências do Windows 10 (por exemplo, uma página da interface do usuário habilitada para toque) devem ser executadas dentro de um contêiner de app moderno. Em geral, você deve primeiro determinar se é possível adicionar sua experiência ao [aprimorando](desktop-to-uwp-enhance.md) seu aplicativo de área de trabalho existente com as APIs de UWP. Se você tiver que usar um componente UWP, obter a experiência, adicione um projeto UWP à sua solução e usar os serviços de aplicativo para se comunicar entre o aplicativo da área de trabalho e o componente UWP.

Para obter mais informações, consulte [estender seu aplicativo da área de trabalho com componentes UWP](desktop-to-uwp-extend.md).

## <a name="distribute"></a>Distribuir

Você pode distribuir seu aplicativo ao publicá-la a Microsoft Store ou fazendo o sideload-lo em outros sistemas.

Ver [distribuir seu aplicativo de desktop empacotado](desktop-to-uwp-distribute.md).
