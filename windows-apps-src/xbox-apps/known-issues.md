---
author: Mtoepke
title: Problemas conhecidos com a UWP no Xbox One Developer Preview
description: 
area: Xbox
ms.sourcegitcommit: bdf7a32d2f0673ab6c176a775b805eff2b7cf437
ms.openlocfilehash: 9a9180f8d6fcd51808310a7f8fbac986ca9c3817

---

# Problemas conhecidos com a UWP no Xbox One Developer Preview

Este tópico descreve problemas conhecidos com a UWP no Xbox One Developer Preview. Para saber mais sobre essa visualização de desenvolvedor, consulte [UWP no Xbox](index.md). 

\[Se você chegou até aqui a partir de um link em um tópico de referência de API e estiver procurando informações sobre a API da família de dispositivos Universal, consulte [Recursos da UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/?LinkID=760755).\]

A Atualização do Sistema do Xbox Developer Preview inclui software de pré-lançamento experimental e inicial. Isso significa que alguns aplicativos e jogos populares não funcionarão conforme o esperado, e você poderá experimentar falhas ocasionais e perda de dados. Se você sair da Developer Preview, o console será restaurado para as configurações de fábrica e você precisará reinstalar seus jogos, aplicativos e conteúdo.

Para os desenvolvedores, isso significa que nem todas as ferramentas de desenvolvedor e APIs funcionarão como o esperado. Nem todos os recursos pretendidos para versão final estão incluídos ou estão com qualidade de versão. 
**Especificamente, isso significa que o desempenho do sistema nesta visualização não reflete o desempenho do sistema da versão final.**

A lista a seguir destaca alguns problemas conhecidos que podem ocorrer nessa versão, embora essa não seja uma lista completa. 

**Queremos receber seu feedback**, portanto, relate todos os problemas que você encontrar no fórum [Developing Universal Windows apps](https://social.msdn.microsoft.com/Forums/windowsapps/en-US/home?forum=wpdevelop). 

Se você ficar preso, leia as informações neste tópico, consulte [Perguntas frequentes](frequently-asked-questions.md)e use os fóruns para pedir ajuda.


<!--## Developing games-->

## Suporte ao modo de mouse

A partir desta versão prévia, o _modo de mouse_ é habilitado por padrão para aplicativos XAML e Web hospedados. Todos os aplicativos que não recusaram a opção receberão um ponteiro de mouse, semelhante ao do navegador Edge do Xbox.

**É altamente recomendável que os desenvolvedores desativem o modo de mouse e otimizem a navegação pelo controlador (X-Y).**

Para desativar o modo de mouse em XAML, siga este exemplo:

```code
public App() {
    this.InitializeComponent();
    this.RequiresPointerMode = Windows.UI.Xaml.ApplicationRequiresPointerMode.WhenRequested;
    this.Suspending += OnSuspending;
}
```

Para desativar modo de mouse em um aplicativo HTML/Javascript, siga este exemplo:

```code
// Turn off mouse mode
navigator.gamepadInputEmulation = "keyboard";
```

> **Observação**
            &nbsp;&nbsp;Nesta versão prévia para desenvolvedores, quando o modo de mouse é ativado, o movimento panorâmico com o joystick direito no controlador pode causar o desligamento do console. Se você encontrar esse problema, reinicie seu console.

Para obter informações sobre o suporte ao modo de mouse, consulte o tópico [Projetando para TV e Xbox](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode). Este tópico inclui informações sobre como habilitar e desabilitar o modo de mouse, para que você possa escolher o comportamento certo para seu aplicativo.

## Você deve ter um usuário conectado para implantar um aplicativo (erro 0x87e10008)

Agora os aplicativos exigem que o usuário esteja conectado para poderem ser iniciados (você deve ter um usuário conectado para poder Iniciar a Depuração (F5) do VS 2015.) A mensagem de erro atual recebida do Visual Studio não é intuitiva:
 
![Não é possível ativar o aplicativo da Windows Store](images/windows-store-app-activation-error.jpg)
 
Para contornar esse problema, entre com um usuário do Xbox Shell ou DevHome antes de implantar seu aplicativo.
 
## Limites de memória para aplicativos em segundo plano ainda não foram impostos
 
O limite de 128 MB para aplicativos em execução em segundo plano não é obrigatório nesta versão prévia. Isso significa que se seu aplicativo exceder 128 MB quando estiver em execução em segundo plano, ele ainda poderá alocar memória.
 
Atualmente, não há uma solução alternativa para esse problema. Você deve controlar o uso da memória de acordo; em uma versão prévia futura, seu aplicativo receberá falhas de alocação de memória se exceder o limite de 128 MB.
 
## A implantação a partir do VS falha com os Controles dos Pais ativados

A inicialização do aplicativo a partir do VS falhará se o console tiver os Controles dos Pais ativados em Configurações.

Para contornar esse problema, desabilite temporariamente os Controles dos Pais ou:
1. Implante seu aplicativo no console com os Controles dos Pais desativados
2. Ative os Controles dos Pais
3. Inicie seu aplicativo a partir do console
4. Digite um PIN ou uma senha para permitir a inicialização do aplicativo
5. O aplicativo será iniciado
6. Feche o aplicativo
7. Inicie a partir do VS usando F5, e o aplicativo será iniciado sem aviso

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

## Suporte ao DirectX 12

A UWP no Xbox One dá suporte ao DirectX 11 Feature Level 10. Não há suporte para o DirectX 12 no momento. O Xbox One, como todos os consoles de jogos tradicionais, é um componente especializado de hardware que requer um SDK específico para acessar todo o seu potencial. Se você estiver trabalhando em um jogo que requeira acesso ao potencial máximo do hardware do Xbox One, pode se registrar no programa [ID@XBOX](http://www.xbox.com/en-us/Developers/id) para obter acesso a esse SDK, que inclui suporte ao DirectX 12.

<!-- ### Xbox One Developer Preview disables game streaming to Windows 10

Activating the Xbox One Developer Preview on your console will prevent you from streaming games from your Xbox One to the Xbox app on Windows 10, even if your console is set to retail mode. 
To restore the game streaming feature, you must leave the developer preview. -->

## Problema conhecido com a área segura de TV

Por padrão, a área de exibição para aplicativos UWP no Xbox deve ser de baixo-relevo na área de segurança da TV. No entanto, o Xbox One Developer Preview contém um bug conhecido que faz com que a área segura de TV comece em [0, 0] e não em [_offset_, _offset_].

> **Observação**
            &nbsp;&nbsp;Isso se aplica somente a aplicativos UWP em Javascript.

A maneira mais fácil de contornar esse problema é desabilitar a área segura de TV, conforme mostrado no seguinte exemplo de JavaScript.

    var applicationView = Windows.UI.ViewManagement.ApplicationView.getForCurrentView();

    applicationView.setDesiredBoundsMode(Windows.UI.ViewManagement.ApplicationViewBoundsMode.useCoreWindow);

Para obter mais informações sobre a área segura da TV, consulte [Projetando para TV e Xbox](https://msdn.microsoft.com/windows/uwp/input-and-devices/designing-for-tv).

<!--## System resources for UWP apps and games on Xbox One

UWP apps and games running on Xbox One share resources with the system and other apps, and so the system governs the resources that are available to any one game or app. 
If you are running into memory or performance issues, this may be why. 
For more details, see [System resources for UWP apps and games on Xbox One](system-resource-allocation.md).-->


## Rede usando soquetes tradicionais

Nessa Developer Preview, o acesso à rede de entrada e de saída do console que usa soquetes TCP/UDP tradicionais (WinSock, Windows.Networking.Sockets) não está disponível. Os desenvolvedores ainda podem usar HTTP e WebSockets. 


## Cobertura de API UWP

Nem todas as APIs UWP têm suporte no Xbox. Consulte [Recursos da UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/p/?LinkId=760755) para obter a lista de APIs que sabemos que não funcionam. Se você encontrar problemas com outras APIs, informe-os nos fóruns. 

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


## Visualização do Windows Device Portal (WDP)

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

### A interface do usuário do WDP não é exibida corretamente no Internet Explorer 7 

Por padrão, a interface do usuário do WDP não é exibida corretamente no navegador ao usar o Internet Explorer 7. Você pode corrigir isso desativando o Modo de Exibição de Compatibilidade do Internet Explorer 7 para WDP.

### Navegar para o WDP provoca um aviso de certificado

Você receberá um aviso sobre o certificado que foi fornecido, semelhante à captura de tela a seguir, porque o certificado de segurança assinado por seu console do Xbox One não é considerado um publicador confiável conhecido. Clique em "Continuar para este site" para acessar o Windows Device Portal.

![Aviso de certificado de segurança do site](images/security_cert_warning.jpg)

<!--## Dev Home

Occasionally, selecting the “Manage Windows Device Portal” option in Dev Home will cause Dev Home to silently exit to the Home screen. 
This is caused by a failure in the WDP infrastructure on the console and can be resolved by restarting the console.-->

## Veja também
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Jun16_HO4-->


