---
author: Mtoepke
title: Problemas conhecidos com o Programa de desenvolvedor UWP no Xbox One
description: 
translationtype: Human Translation
ms.sourcegitcommit: 20ac6fb738de1c8aaf10f46c359842f31714dbbf
ms.openlocfilehash: b6fe2f90e0aff4b8e77b4c20aec0d29f2a6a36f8

---

# <a name="known-issues-with-uwp-on-xbox-developer-program"></a>Problemas conhecidos com o Programa de desenvolvedor UWP no Xbox

Este tópico descreve problemas conhecidos com o Programa de desenvolvedor UWP no Xbox One. Para saber mais sobre esse programa, consulte [UWP no Xbox](index.md). 

\[Se você chegou até aqui a partir de um link em um tópico de referência de API e estiver procurando informações sobre a API da família de dispositivos Universal, consulte [Recursos da UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/?LinkID=760755).\]

A lista a seguir destaca alguns problemas conhecidos que podem ocorrer, embora essa não seja uma lista completa. 

**Queremos receber seu feedback**, portanto, relate todos os problemas que você encontrar no fórum [Desenvolvendo aplicativos da Plataforma Universal do Windows](https://social.msdn.microsoft.com/forums/windowsapps/home?forum=wpdevelop). 

Se você ficar preso, leia as informações neste tópico, consulte [Perguntas frequentes](frequently-asked-questions.md)e use os fóruns para pedir ajuda.


<!--## Developing games-->
 
## <a name="memory-limits-for-background-apps-are-partially-enforced"></a>Limites de memória para aplicativos em segundo plano foram parcialmente impostos
 
O volume de memória máxima para aplicativos em execução em segundo plano é 128 MB. Na versão atual de UWP no Xbox One, seu aplicativo será suspenso se estiver acima esse limite quando for movido para segundo plano. Esse limite não será imposto no momento se seu aplicativo exceder o limite enquanto estiver sendo executado em segundo plano. Isso significa que, se seu aplicativo exceder 128 MB durante a execução em segundo plano, ele ainda poderá alocar memória.
 
Atualmente, não há uma solução alternativa para esse problema. Os aplicativos devem reger o uso da memória de acordo e continuar a permanecer dentro do limite de 128 MB durante a execução em segundo plano.
 
## <a name="deploying-from-vs-fails-with-parental-controls-turned-on"></a>A implantação a partir do VS falha com os Controles dos Pais ativados

A inicialização do aplicativo a partir do VS falhará se o console tiver os Controles dos Pais ativados em Configurações.

Para contornar esse problema, desabilite temporariamente os Controles dos Pais ou:
1. Implante seu aplicativo no console com os Controles dos Pais desativados.
2. Ative os Controles dos Pais.
3. Inicie seu aplicativo a partir do console.
4. Digite um PIN ou uma senha para permitir a inicialização do aplicativo.
5. O aplicativo será iniciado.
6. Feche o aplicativo.
7. Inicie a partir do VS usando F5, e o aplicativo será iniciado sem aviso.

Nesse caso, a permissão é _fixa_ até você desconectar o usuário, mesmo se você desinstalar e reinstalar o aplicativo.
 
Há outro tipo de isenção disponível apenas para contas de crianças. Uma conta de criança requer que o pai conecte-se para conceder permissão. Ao conectar, o pai tem a opção de escolher **Sempre** para permitir que a criança inicie o aplicativo. Essa isenção é armazenada na nuvem e persistirá mesmo que a criança saia e entre novamente.   

<!--### x86 vs. x64

By the time we release later this year, we will have great support for both x86 and x64, and we do support x86 in this preview. 
However, x64 has had much more testing to date (the Xbox shell and all of the apps running on the console today are x64), and so we recommend using x64 for your projects. 
This is particularly true for games.

If you decide to use x86, please report any issues you see on the forum.

Also see [Switching build flavors can cause deployment failures](known-issues.md#switching-build-flavors-can-cause-deployment-failures) later on this page.-->

<!--### Game engines

We have tested some popular game engines, but not all of them, and our test coverage for this preview has not been comprehensive. 
Your mileage may vary. 

The following game engines have been confirmed to work:
* [Construct 2](https://www.scirra.com/)

There are likely others that are working too. We would love to get your feedback on what you find. 
Please use the forum to report any issues you see.-->

## <a name="directx-12-support"></a>Suporte ao DirectX 12

A UWP no Xbox One dá suporte ao DirectX 11 Feature Level 10. Não há suporte para o DirectX 12 no momento. 

O Xbox One, como todos os consoles de jogos tradicionais, é um componente especializado de hardware que requer um SDK específico para acessar todo o seu potencial. Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, pode se registrar no programa [ID@XBOX](http://www.xbox.com/Developers/id) para obter acesso a esse SDK, que inclui suporte ao DirectX 12.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->

<!--
## Networking using traditional sockets

In this developer preview, inbound and outbound network access from the console that uses traditional TCP/UDP sockets (WinSock, Windows.Networking.Sockets) is not available. 
Developers can still use HTTP and WebSockets.
--> 

## <a name="blocked-networking-ports-on-xbox-one"></a>Portas de rede bloqueadas no Xbox One

os aplicativos da Plataforma Universal do Windows (UWP) em dispositivos Xbox One não podem se associar a portas no intervalo [49152, 65535]. Embora a associação a essas portas possa parecer ter sucesso em tempo de execução, o tráfego de rede pode diminuir silenciosamente antes de atingir seu aplicativo. Seu aplicativo deverá se associar à porta 0 sempre que possível, o que permite que o sistema selecione a porta local. Se você precisar usar uma porta específica, o número da porta deverá estar no intervalo [1025, 49151], e você deve verificar e evitar conflitos com o registro IANA. Para obter mais informações, consulte o [Nome do serviço e registro de número de porta de protocolo de transporte](http://www.iana.org/assignments/service-names-port-numbers/service-names-port-numbers.xhtml).

## <a name="uwp-api-coverage"></a>Cobertura de API UWP

Nem todas as APIs UWP têm suporte no Xbox. Para obter a lista de APIs que sabemos que não funcionam, consulte [Recursos da UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755). Se você encontrar problemas com outras APIs, informe-os nos fóruns. 

<!--## XAML controls do not look like or behave like the controls in the Xbox One shell

In this developer preview, the XAML controls are not in their final form. In particular:
* Gamepad X-Y navigation does not work reliably for all controls.
* Controls do not look like controls in the Xbox shell. This includes the control focus rectangle.
* Navigating between controls does not automatically make “navigation sounds.”

These issues will be addressed in a future developer preview.-->

<!--## Visual Studio and deployment issues

### Switching build flavors can cause deployment failures

Switching between Debug and Release builds, or between x86 and x64, or between Managed and .Net Native builds, can cause deployment failures. 

The simplest way to avoid these issues for this preview is to stick to Debug and one architecture. 

If you do hit this issue, uninstalling your app in the Collections app on your Xbox One will typically resolve it.

> ****&nbsp;&nbsp;Uninstalling your app from Windows Device Portal (WDP) will not resolve the issue.

If your issues persist, uninstall your app or game in the Collections app, leave Developer Mode, restart to Retail Mode and then switch back to Developer Mode.
You may also need to restart Visual Studio and clean your solution.

For more information, see the “Fixing deployment failures” section in [Frequently asked questions](frequently-asked-questions.md).

### Uninstalling an app while you are debugging it in Visual Studio will cause it to fail silently

Attempting to uninstall an app that is running under the debugger via the WDP “Installed Apps” tool will cause it to silently fail. 
The workaround is to stop debugging the app in Visual Studio before attempting to remove it via WDP.

### Visual Studio/Xbox PIN pairing failures

It is possible to get into a state where the PIN pairing between Visual Studio and your Xbox One gets out of sync. 
If PIN pairing fails, use the “Remove all pairings” button in Dev Home, restart Xbox One, restart your development PC, and then try again.--> 


<!--## Windows Device Portal (WDP) preview-->

<!--### Starting WDP from Dev Home crashes Dev Home

When you start WDP in Dev Home, it will cause Dev Home to crash after you have entered your user name and password and selected **Save**. 
The credentials are saved but WDP is not started. 
You can start WDP by restarting Xbox One.--> 

<!--### Disabling WDP in Dev Home does not work

If you disable WDP in Dev Home, it will be turned off. 
However, when you restart your Xbox One, WDP will be started again. 
You can work around this issue by using **Reset and keep my games & apps** to delete any stored state on your Xbox One. 
Go to Settings > System > Console info & updates > Reset console, and then select the **Reset and keep my games & apps** button.

> **Caution**&nbsp;&nbsp;Doing this will delete all saved settings on your Xbox One including wireless settings, user accounts and any game progress that has not been saved to cloud storage.

> **Caution**&nbsp;&nbsp;DO NOT select the **Reset and remove everything** button.
This will delete all of your games, apps, settings and content, deactivate Developer Mode, and remove you console from the Developer Preview group.

### The columns in the “Running Apps” table do not update predictably. 

Sometimes this is resolved by sorting a column on the table.-->

## <a name="navigating-to-wdp-causes-a-certificate-warning"></a>Navegar para o WDP provoca um aviso de certificado

Você receberá um aviso sobre o certificado que foi fornecido, semelhante à captura de tela a seguir, porque o certificado de segurança assinado por seu console do Xbox One não é considerado um publicador confiável conhecido. Para acessar o Windows Device Portal, clique em **Continuar para este site**.

![Aviso de certificado de segurança do site](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## <a name="see-also"></a>Consulte também
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Dec16_HO1-->


