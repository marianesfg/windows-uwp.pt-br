---
description: Saiba como tornar seu aplicativo inclusivo e acessível para pessoas ao redor do mundo.
keywords: acessibilidade do aplicativo UWP, globalização, aplicativos de design inclusivo, requisitos de aplicativo de acessibilidade
title: Usabilidade em aplicativos UWP – Desenvolvimento de aplicativos do Windows
template: detail.hbs
ms.date: 10/18/2017
ms.topic: article
ms.assetid: e6bb3464-dd8e-402c-9c56-dd9e51002a49
ms.localizationpriority: medium
ms.openlocfilehash: c725839a29c093c78eb977538da4c43d906051c6
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "80614956"
---
# <a name="usability-for-uwp-apps"></a>Usabilidade para aplicativos UWP

São os pequenos toques, uma atenção extra aos detalhes, que podem transformar uma boa experiência do usuário em uma experiência do usuário realmente inclusiva que atenda as necessidades dos usuários em todo o mundo.

As instruções de design e codificação nesta seção podem tornar seu aplicativo UWP mais inclusivo adicionando recursos de acessibilidade, habilitando a globalização e localização, permitindo que os usuários personalizem sua experiência e fornecendo ajuda quando os usuários precisarem.

## <a name="accessibility"></a>Acessibilidade

Acessibilidade significa tornar os seus aplicativos utilizáveis por pessoas que possuem limitações que impeçam o uso de interfaces de usuário convencionais. Para algumas situações, os requisitos de acessibilidade são impostos por lei. No entanto, é uma boa ideia resolver os problemas de acessibilidade independentemente dos requisitos legais, para que seus aplicativos tenham a maior audiência possível.

[Portal de acessibilidade](../accessibility/accessibility.md)

<!--
<ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-overview.md">Accessibility overview</a></b> <br/> This article is an overview of the concepts and technologies related to accessibility scenarios for UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/designing-inclusive-software.md">Designing inclusive software</a></b><br/>Learn about evolving inclusive design with Universal Windows Platform (UWP) apps for Windows 10.  Design and build inclusive software with accessibility in mind.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/developing-inclusive-windows-apps.md">Developing inclusive Windows apps</a></b><br/> This article is a roadmap for developing accessible UWP apps.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-testing.md">Accessibility testing</a> </b><br/>Testing procedures to follow to ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-in-the-store.md">Accessibility in the Store</a></b><br/>Describes the requirements for declaring your UWP app as accessible in the Microsoft Store.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessibility-checklist.md">Accessibility checklist</a></b><br/>Provides a checklist to help you ensure that your UWP app is accessible.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/basic-accessibility-information.md">Expose basic accessibility information</a></b><br/>Basic accessibility info is often categorized into name, role, and value. This topic describes code to help your app expose the basic information that assistive technologies need.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/keyboard-accessibility.md">Keyboard accessibility</a></b><br/>If your app does not provide good keyboard access, users who are blind or have mobility issues can have difficulty using your app or may not be able to use it at all.</p>
                    </div>
                </div>
            </div>
        </div>
    </li> 
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/high-contrast-themes.md">High-contrast themes</a></b><br/>Describes the steps needed to ensure your UWP app is usable when a high-contrast theme is active. </p>
                    </div>
                </div>
            </div>
        </div>
    </li>         
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/accessible-text-requirements.md">Accessible text requirements</a></b><br/>This topic describes best practices for accessibility of text in an app, by assuring that colors and backgrounds satisfy the necessary contrast ratio. This topic also discusses the Microsoft UI Automation roles that text elements in a UWP app can have, and best practices for text in graphics.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/practices-to-avoid.md">Accessibility practices to avoid</a></b><br/>Lists the practices to avoid if you want to create an accessible UWP app.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/custom-automation-peers.md">Custom automation peers</a></b><br/>Describes the concept of automation peers for UI Automation, and how you can provide automation support for your own custom UI class.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../accessibility/control-patterns-and-interfaces.md">Control patterns and interfaces</a></b><br/>Lists the Microsoft UI Automation control patterns, the classes that clients use to access them, and the interfaces providers use to implement them.</p>                    
                    </div>
                </div>
            </div>
        </div>
    </li>     
</ul>
-->

## <a name="app-settings"></a>Configurações de aplicativo

As configurações do aplicativo permitem que o usuário personalize seu aplicativo, otimizando-o para suas necessidades e preferências individuais. Fornecer as configurações certas e armazená-las adequadamente podem tornar uma experiência de usuário ótima ainda melhor.

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../app-settings/guidelines-for-app-settings.md">Diretrizes</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Práticas recomendadas para criar e exibir configurações de aplicativo.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../app-settings/store-and-retrieve-app-data.md">Armazenar e recuperar dados de aplicativo</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Como armazenar e recuperar dados de aplicativo locais, em roaming e temporários.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/guidelines-for-app-settings.md">Guidelines</a></b><br/>Best practices for creating and displaying app settings.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../app-settings/store-and-retrieve-app-data.md">Store and retrieve app data</a></b><br/>How to store and retrieve local, roaming, and temporary app data.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
</ul> -->

## <a name="globalization-and-localization"></a>Globalização e localização

O Windows é usado no mundo todo por públicos diversificados em termos de idioma, região e cultura. Seus usuários falam uma variedade de idiomas diferentes e em vários países e regiões diferentes. Alguns usuários falam mais de um idioma. Assim, seu aplicativo é executado em configurações que envolvem muitas permutações de configurações do sistema de idioma, região e cultura. Aumente o mercado potencial para seu aplicativo desenvolvendo-o para ser prontamente adaptável usando *globalização* e *localização*.

<a href="../globalizing/globalizing-portal.md">Portal de globalização e localização</a>

## <a name="in-app-help"></a>Ajuda no aplicativo
Não importa quão bem você projetou o aplicativo, alguns usuários precisarão de um pouco mais de ajuda.

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/guidelines-for-app-help.md">Diretrizes da ajuda ao aplicativo</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Os aplicativos podem ser complexos e o fornecimento de ajuda eficaz pode melhorar consideravelmente a experiência dos usuários.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/instructional-ui.md">Interface do usuário instrucional</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Às vezes, pode ser útil ensinar o usuário sobre funções em seu aplicativo que podem não ser óbvias, como interações de toque específicas. Nesses casos, você precisa apresentar instruções para o usuário por meio da interface do usuário, para que ele possa descobrir e usar os recursos que talvez tenha ignorado.</p>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/in-app-help.md">Ajuda no aplicativo</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Na maioria das vezes, é melhor que a ajuda seja exibida dentro do aplicativo e quando o usuário optar por exibi-la. Considere as diretrizes a seguir ao criar a ajuda no aplicativo.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="../in-app-help/external-help.md">Ajuda externa</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Na maioria das vezes, é melhor que a ajuda seja exibida dentro do aplicativo e quando o usuário optar por exibi-la. Considere as diretrizes a seguir ao criar a ajuda no aplicativo.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/guidelines-for-app-help.md">Guidelines for app help</a></b><br/>Applications can be complex, and providing effective help for your users can greatly improve their experience.
</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/instructional-ui.md">Instructional UI</a></b><br/>Sometimes it can be helpful to teach the user about functions in your app that might not be obvious to them, such as specific touch interactions. In these cases, you need to present instructions to the user through the UI so they can discover and use features they might have missed.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/in-app-help.md">In-app help</a></b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
<p><b><a href="../in-app-help/external-help.md">External help</a></b><br/>Most of the time, it's best for help to be displayed within the app, and to be displayed when the user chooses to view it. Consider the following guidelines when creating in-app help.</p>
                    </div>
                </div>
            </div>
        </div>
    </li>        
</ul> -->

