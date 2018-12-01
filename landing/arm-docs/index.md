---
layout: LandingPage
description: Esta página fornece as informações para você começar a desenvolver aplicativos UWP e win32 ARM64.
title: Windows 10 em execução no ARM
author: msatranjr
ms.author: misatran
ms.date: 05/08/2018
ms.localizationpriority: medium
ms.topic: article
keywords: Windows 10 no ARM, ARM, criação de aplicativos do win32 ARM64, criando drivers ARM64
ms.openlocfilehash: 83f2a0d03040a682e6965558174294fe27e21bfb
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8340100"
---
# <a name="windows-10-on-arm"></a>Windows 10 em execução no ARM
Windows 10 é executado em computadores com processadores ARM. Esta página fornece as informações para que você saiba mais sobre a plataforma e começar a desenvolver aplicativos. Também recomendamos que você forneça seus comentários usando os links na parte inferior da página.

## <a name="introductory-videos"></a>Vídeos introdutórios
Assista e saiba como o Windows 10 é executado no ARM.

<ul class="cols cols3">
    <li>
        <a href="https://youtu.be/OZtVBDeVqCE"><img alt="Building ARM64 Win32 C++ apps video" src="./images/Arm64Scaled.png" /></a>
        <h3>Criando aplicativos de C++ Win32 ARM64</h3><p>Saiba como instalar as ferramentas de ARM64 para o Visual Studio. Em seguida, nós o guiaremos você pelas etapas de criação e compilando um novo projeto de 64 ARM.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Build/2018/BRK2438"><img alt="Build 2018 Windows 10 on ARM for developers" src="./images/buildVideoStillScaled.png" /></a>
        <h3>Build 2018 Windows 10 no ARM para desenvolvedores</h3><p>Saiba mais sobre o Windows 10 em dispositivos ARM, como a mágica do x86 funciona de emulação e, finalmente, como enviar e criar aplicativos para Windows 10 no ARM. Será mostraremos como criar aplicativos ARM64 para área de trabalho e UWP.</p>
    </li>
    <li>
        <a href="https://channel9.msdn.com/Events/Ch9Live/Windows-Community-Standup/Kevin-Gallo-January-2018"><img alt="Community standup video featuring Kevin Gallo" src="./images/communityStandupStillScaled.png" /></a>
        <h3>Comunidade do Windows com Kevin Gallo Standup</h3><p>Obter profunda compreensão de como o Windows 10 é executado no ARM64 e obter uma noção aplicativos e experiências nesta plataforma.</p>
    </li>
</ul>

## <a name="understanding-windows-10-on-arm"></a>Noções básicas sobre o Windows 10 no ARM
Acessar Meus a plataforma, observando a esses recursos.

<ul class="cardsF panelContent cols cols2">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm" title="Comece agora" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Get started icon" src="/media/common/i_get-started.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Introdução ao Windows 10 no ARM</h3>
                    <p class="x-hidden-focus">Confira a documentação para compreender as Noções básicas.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-x86-emulation" title="O tópico sobre x86 emulação" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="x86 emulation icon" src="/media/common/i_advanced.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Saiba como x86 funciona de emulação</h3>
                    <p class="x-hidden-focus">Descubra tudo sobre esse recurso fundamental do Windows 10 no ARM.</p>
                </div>
            </div>
        </div>
    </li>
    <!--<li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.msdn.microsoft.com/harip/" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="" src="/media/common/i_blog.svg?branch=master" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Read the Kernel blog</h3>
                    <p class="x-hidden-focus">Get a deep understanding of the Windows by reading articles that are written by the creators of the kernel.</p>
                </div>
            </div>
        </div>
    </li>-->
</ul>

## <a name="developing-for-windows-10-on-arm"></a>Desenvolvendo para Windows 10 no ARM
Inicie a adaptar seus aplicativos para Windows 10 no ARM e tirar proveito dos recursos disponíveis lá.  

<ul class="cardsF panelContent cols cols3">
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="https://blogs.windows.com/buildingapps/?p=52087" title="Criando aplicativos ARM64" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Build ARM64 Win32 apps blog icon" src="/media/common/i_build.svg" data-linktype="external" />
                    </div>
                    </a>
                <div class="cardText">
                    <h3>Criando aplicativos de ARM64 com o SDK</h3>
                    <p class="x-hidden-focus">Confira esta postagem no blog onde vamos examinar compilando seus aplicativos como ARM64 para são executados nativamente no Windows 10 no ARM.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-arm32" title="Solucionar problemas de aplicativos arm32" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="UWP apps on ARM icon" src="/media/common/i_code-edit.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Aplicativos UWP no ARM</h3>
                    <p class="x-hidden-focus">Siga essas diretrizes para definir seus aplicativos da plataforma Universal do Windows (UWP) para o sucesso.</p>                    
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/debugger/debugging-arm64" title="Depurando aplicativos ARM64" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                             <img class="cardImage" role="presentation" alt="Debugging on ARM icon" src="/media/common/i_debug.svg" data-linktype="external" />
                    </div>
                </a>
                <div class="cardText">
                    <h3>Depuração no ARM</h3>
                    <p class="x-hidden-focus">Obter o código funcionando normalmente no Windows 10 no ARM.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows-hardware/drivers/develop/building-arm64-drivers" title="Criando drivers ARM64" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="Building ARM64 drivers icon" src="/media/common/i_drivers.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>Criando drivers ARM64 com o WDK</h3>
                    <p class="x-hidden-focus">Recompile seus drivers para ARM64.</p>
                </div>
            </div>
        </div>
    </li>
    <li>
        <div class="cardSize">
            <div class="cardPadding">
                <a class="card" href="/windows/uwp/porting/apps-on-arm-troubleshooting-x86" title="Solução de problemas x86 aplicativos" data-linktype="absolute-path">
                    <div class="cardImageOuter">
                            <img class="cardImage" role="presentation" alt="x86 apps on ARM icon" src="/media/common/i_code-blocks.svg" data-linktype="external" />
                            </a>
                    </div>
                </a>
                <div class="cardText">
                    <h3>x86 aplicativos no ARM</h3>
                    <p class="x-hidden-focus">Desenvolver seu x86 aplicativos realizem seu melhor no Windows 10 no ARM.</p>
                </div>
            </div>
        </div>
    </li>
</ul>

<!--## Other videos
<ul class="cols cols4">
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
<li>
        <a href="#"><img alt="" src="./images/dummyStillScaled.png" /></a>
            <p>TBD</p>    
    </li>
</ul>-->

## <a name="let-us-know-if-you-have-feedback"></a>Conte-nos sua se você tiver comentários
Estamos continuamente aperfeiçoando nosso produto por seus comentários aproveitando e de nossos clientes existentes. Se você já tem uma ideia, está travados em um problema ou apenas deseja compartilhar como muito bem sua experiência é, esses links ajudará você.

<ul class="cardsM cols cols3">
<li>
        <a class="card" href="feedback-hub://?tabid=2&contextid=803" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Feedback hub icon" src="/media/common/i_feedback.svg" data-linktype="external" />
            <div class="cardText">
                <h3>Usar o hub de feedback</h3>
                <p>Podemos perder algo? Você tem uma ótima ideia? Conte no Hub de Feedback.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="mailto:woafeedback@microsoft.com" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Report a bug icon" src="/media/common/i_mail.svg" data-linktype="external" />
            <div class="cardText">
                <h3>Relatar um bug</h3>
                <p>Encontre um bug em nossa plataforma? Envie o email com os detalhes.</p>
            </div>
        </a>
    </li>
    <li>
        <a class="card" href="https://github.com/MicrosoftDocs/windows-uwp/tree/docs/landing/arm-docs" data-linktype="absolute-path">
            <img class="cardImage" role="presentation" alt="Give doc feedback icon" src="/media/common/i_form.svg" data-linktype="external" />
            <div class="cardText">
                <h3>Fornecer comentários de documento</h3>
                <p>Você encontrou um problema com nossos documentos? Você deseja para que possamos fazer algo mais clara? Crie um problema no repositório do GitHub nossos documentos.</p>
            </div>
        </a>
    </li>
</ul>