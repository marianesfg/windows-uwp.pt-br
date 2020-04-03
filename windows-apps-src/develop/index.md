---
description: Saiba como desenvolver um aplicativo UWP.
title: Desenvolver aplicativos UWP
keywords: uwp desenvolvimento de aplicativo threading plataforma assíncrona visão geral portal desenvolver desenvolvedores
ms.date: 03/29/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 233666294555c46b5ba8b1e558eb32d6aed84e2a
ms.sourcegitcommit: 08cb5a4ca2e02179ad6b768c841fe3d5216bcae3
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/02/2020
ms.locfileid: "80614951"
---
# <a name="develop-uwp-apps"></a>Desenvolver aplicativos UWP

Artigos de instruções e código para a criação de aplicativos UWP para Windows 10.

:::row:::
    :::column:::
        <a href="/windows/uwp/get-started/universal-application-platform-guide">
            <img src="https://docs.microsoft.com//media/hubs/windows/win_developer-uwp.svg" alt="UWP overview" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/windows/uwp/get-started/universal-application-platform-guide">Visão geral da Plataforma Universal do Windows</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">É uma explicação sobre o que é UWP, como ele funciona e os recursos que ele fornece.</p>
    :::column-end:::
    :::column:::
        <a href="/windows/uwp/porting/index">
            <img src="https://docs.microsoft.com/media/illustrations/teams-fast-track.svg" alt="Porting guide" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/windows/uwp/porting/index">Guia de portabilidade</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Traga seus atuais aplicativos iOS, Android, WPF ou Windows Forms para UWP.</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <a href="/windows/uwp/get-started/universal-application-platform-guide" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">                 
                            <img src="https://docs.microsoft.com//media/hubs/windows/win_developer-uwp.svg" alt=" "/>
                        </div>
                    </div>
                    <div class="cardText">
                        <h3>Overview of the Universal Windows Platform</h3>
                        <p>An explanation of what UWP is, how it works, and the features it provides.</p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
    <li>
        <a href="/windows/uwp/porting/index" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardImageOuter">
                        <div class="cardImage" style="background-color: #f2f2f2">                
                            <img src="https://docs.microsoft.com/media/illustrations/teams-fast-track.svg" alt=" " />
                        </div>
                    </div>                
                    <div class="cardText">
                        <h3>Porting guide</h3>
                        <p>Bring your existing Windows Forms, WPF, Android, or iOS app to UWP. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>                 
</ul> -->

## <a name="api-reference"></a>Referência de API

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/uwp/api">Namespaces da UWP do Windows</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">As classes, as estruturas, as interfaces, os métodos, as propriedades e os eventos que compõem o Windows Runtime, organizados por namespace.</p>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="/uwp/schemas/">Esquemas para UWP</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Especificações de esquema XML e arquivo para aplicativos UWP (Plataforma Universal do Windows).</p>
    :::column-end:::
:::row-end:::

<!-- <ul class="panelContent cardsH" style="margin-left: 1px">
    <li>
        <a href="/uwp/api" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Windows UWP namespaces</h3>
                        <p>The classes, structures, interfaces, methods, properties, and events that make up the Windows Runtime, organized by namespace.</p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>
    <li>
        <a href="/uwp/schemas/" style="display:block">
        <div class="cardSize">
            <div class="cardPadding">
                <div class="card">
                    <div class="cardText">
                        <h3>Schemas for UWP</h3>
                        <p>File and XML schema specifications for Universal Windows Platform (UWP) apps. </p>
                    </div>
                </div>
            </div>
        </div>
        </a>
    </li>                 
</ul> -->

## <a name="articles"></a>Artigos

:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Tipos de aplicativo</h3>
        <a href="/windows/uwp/apps-for-education/">Aplicativos de educação</a><br/>
        <a href="/windows/uwp/enterprise/">Aplicativos corporativos</a><br/>
        <a href="/windows/uwp/gaming/">Aplicativos de Jogos e DirectX</a><br/>
        <a href="/microsoft-edge/progressive-web-apps">Aplicativos Web Progressivos</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Interface do Usuário do Aplicativo</h3>
        <a href="https://developer.microsoft.com/windows/apps/design">Para controles, layout, tipografia, animação, usabilidade e design de interface do usuário, veja a seção de Design e Interface do Usuário.</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Comunicação</h3>
        <a style="display:block" href="/windows/uwp/app-to-app/">Comunicação de aplicativo a aplicativo</a><br/>
        <a style="display:block" href="/windows/uwp/networking/">Serviços de rede e Web</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Dados e arquivos</h3>
        <a href="/windows/uwp/audio-video-camera/">Áudio, vídeo e câmera</a><br/>
        <a href="/windows/uwp/data-access/" style="display:block" >Acesso a dados</a><br/>
        <a href="/windows/uwp/data-binding/"style="display:block" >Associação de dados</a><br/>
        <a href="/windows/uwp/files/" style="display:block" >Arquivos, pastas e bibliotecas</a><br/>
        <a href="/windows/uwp/machine-learning/">Aprendizado de máquina</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Implantação</h3>
        <a href="/windows/uwp/updates-and-versions/choose-a-uwp-version">Escolher uma versão do UWP</a><br/>
        <a href="/windows/uwp/debug-test-perf/">Depuração, teste e desempenho</a><br/>
        <a href="/windows/uwp/monetize/">Monetização, envolvimento e serviços da Store</a><br/>
        <a href="/windows/uwp/packaging/">Empacotando aplicativos</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Plataforma</h3>
        <a href="/windows/uwp/cpp-and-winrt-apis/">C++/WinRT<</a><br/>
        <a href="/windows/uwp/launch-resume/">Início, retomada e tarefas em segundo plano</a><br/>
        <a href="/windows/uwp/security/">Segurança</a><br/>
        <a href="/windows/uwp/threading-async/">Threading e programação assíncrona</a><br/>
        <a href="/windows/uwp/composition/visual-layer">Camada visual</a><br/>
        <a href="/windows/uwp/updates-and-versions/application-development-for-windows-as-a-service">Windows como serviço</a><br/>
        <a href="/windows/uwp/winrt-components/">componentes do Windows Runtime</a><br/>
        <a href="/windows/uwp/xaml-platform/">Plataforma XAML</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Pessoas e locais</h3>
        <a href="/windows/uwp/contacts-and-calendar/">Contatos, Minhas Pessoas e calendário</a><br/>
        <a href="/windows/uwp/maps-and-location/">Mapas e localização</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Periféricos, sensores e alimentação</h3>
        <a href="/windows/uwp/contacts-and-calendar/">Visão geral</a><br/>
        <a href="/windows/uwp/devices-sensors/enable-device-capabilities">Habilitar as funcionalidades do dispositivo</a><br/>
        <a href="/windows/uwp/devices-sensors/pair-devices">Emparelhar dispositivos</a><br/>
        <a href="/windows/uwp/devices-sensors/point-of-service">Ponto de Serviço</a><br/>
        <a href="/windows/uwp/devices-sensors/sensors">Sensores</a><br/>
        <a href="/windows/uwp/devices-sensors/printing-and-scanning">Impressão</a><br/>
        <a href="/windows/uwp/devices-sensors/3d-printing">Impressão 3D<</a><br/>
        <a href="/windows/uwp/devices-sensors/nfc">NFC</a><br/>
        <a href="/windows/uwp/devices-sensors/get-battery-info">Informações de bateria</a><br/>
    :::column-end:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Portabilidade</h3>
        <a href="/windows/uwp/porting/">Visão geral</a><br/>
        <a href="/windows/uwp/porting/wpsl-to-uwp-root">Windows Phone Silverlight para UWP</a><br/>
        <a href="/windows/uwp/porting/w8x-to-uwp-root">Windows Runtime 8.x para UWP</a><br/>
        <a href="/windows/uwp/porting/desktop-to-uwp-root">Ponte de Desktop</a><br/>
        <a href="/windows/uwp/porting/desktop-to-uwp-migrate">Compartilhar código entre o desktop e o UWP</a><br/>
        <a href="/windows/uwp/porting/android-ios-uwp-map">Mapeamento do conceito para desenvolvedores do Android e iOS</a><br/>
        <a href="/windows/uwp/porting/ios-to-uwp-root">Mudar do iOS para a UWP</a><br/>
        <a href="/microsoft-edge/progressive-web-apps">Converta seu aplicativo Web em um PWA</a><br/>
        <a href="/windows/uwp/porting/apps-on-arm">Windows 10 no ARM</a><br/>
    :::column-end:::
:::row-end:::
:::row:::
    :::column:::
        <h3 style="margin-top: 10px; margin-bottom: 0px">Processos e threading</h3>
        <a href="/windows/uwp/launch-resume/">Início, retomada e tarefas em segundo plano</a><br/>
        <a href="/windows/uwp/threading-async/">Threading e programação assíncrona</a><br/><br/><br/>
    :::column-end:::
:::row-end:::


 ## <a name="samples-and-tools"></a>Exemplos e ferramentas

 :::row:::
    :::column:::
        <a href="https://developer.microsoft.com/windows/samples">
            <img src="https://docs.microsoft.com/media/illustrations/sql-database-develop.svg" alt="Samples" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="https://developer.microsoft.com/windows/samples">Amostras</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Aprenda a criar ótimos aplicativos para Windows experimentando essas amostras. Estes exemplos mostram como os recursos funcionam e ajudam a começar seus próprios aplicativos UWP.</p>
    :::column-end:::
    :::column:::
        <a href="https://developer.microsoft.com/windows/downloads">
            <img src="https://docs.microsoft.com/media/illustrations/sql-get-started-download.svg" alt="Developer tools" />
        </a><br/>
        <h3 style="margin-top: 10px; margin-bottom: 0px"><a href="https://developer.microsoft.com/windows/downloads">Ferramentas de desenvolvedor</a></h3>
        <p style="margin-top: 0px; margin-bottom: 50px">Obtenha o Visual Studio 2019, o SDK do Windows 10 e outras ferramentas para desenvolvedores.</p>
    :::column-end:::
:::row-end:::
