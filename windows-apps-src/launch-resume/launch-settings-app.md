---
title: Iniciar o app Configurações do Windows
description: Saiba como iniciar o aplicativo Configurações do Windows a partir de seu aplicativo. Este tópico descreve o esquema de URI ms-settings. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 04/19/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.custom: 19H1
dev_langs:
- csharp
- cppwinrt
ms.openlocfilehash: 1ef32816d04516bf4c8ce8677d7f682d2d59f657
ms.sourcegitcommit: db48036af630f33f0a2f7a908bfdfec945f3c241
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/05/2020
ms.locfileid: "84437165"
---
# <a name="launch-the-windows-settings-app"></a>Iniciar o app Configurações do Windows

**APIs importantes**

-   [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync)
-   [**PreferredApplicationPackageFamilyName**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.preferredapplicationpackagefamilyname)
-   [**DesiredRemainingView**](https://docs.microsoft.com/uwp/api/windows.system.launcheroptions.desiredremainingview)

Saiba como iniciar o aplicativo Configurações do Windows. Este tópico descreve o esquema de URI **ms-settings:**. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas.

A inicialização do aplicativo Configurações é uma parte importante da escrita de um aplicativo com detecção de privacidade. Se seu aplicativo não pode acessar um recurso confidencial, é recomendável fornecer ao usuário um link conveniente para as configurações de privacidade desse recurso. Para obter mais informações, consulte [Diretrizes para aplicativos com detecção de privacidade](https://docs.microsoft.com/windows/uwp/security/index).

## <a name="how-to-launch-the-settings-app"></a>Como iniciar o aplicativo Configurações

Para iniciar o aplicativo **Configurações**, use o esquema de URI `ms-settings:`, como mostrado nos exemplos a seguir.

Neste exemplo, um controle de hiperlink XAML é usado para iniciar a página de configurações de privacidade do microfone usando o URI `ms-settings:privacy-microphone`.

```xml
<!--Set Visibility to Visible when access to the microphone is denied -->
<TextBlock x:Name="LocationDisabledMessage" FontStyle="Italic"
                 Visibility="Collapsed" Margin="0,15,0,0" TextWrapping="Wrap" >
          <Run Text="This app is not able to access the microphone. Go to " />
              <Hyperlink NavigateUri="ms-settings:privacy-microphone">
                  <Run Text="Settings" />
              </Hyperlink>
          <Run Text=" to check the microphone privacy settings."/>
</TextBlock>
```

Como alternativa, seu aplicativo pode chamar o método [**LaunchUriAsync**](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) para iniciar o aplicativo **Configurações** do código. Este exemplo mostra como iniciar a página de configurações de privacidade da câmera usando o URI `ms-settings:privacy-webcam`.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```
```cppwinrt
bool result = co_await Windows::System::Launcher::LaunchUriAsync(Windows::Foundation::Uri(L"ms-settings:privacy-webcam"));
```

O código acima inicia a página de configurações de privacidade da câmera:

![configurações de privacidade da câmera.](images/privacyawarenesssettingsapp.png)

Para obter mais informações como URIs de inicialização, consulte [Iniciar o aplicativo padrão para um URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>Referência de esquema de URI ms-settings:

Use os seguintes URIs para abrir várias páginas do aplicativo Configurações.

> Observe que a disponibilidade de uma página de configurações varia de acordo com a SKU do Windows. Nem todas as páginas de configurações disponíveis no Windows 10 para desktop estão disponíveis no Windows 10 Mobile e vice-versa. A coluna de anotações também captura requisitos adicionais que devem ser atendidos para uma página para estar disponível.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>Contas

|Página Configurações| URI |
|-------------|-----|
| Acesso corporativo ou de estudante | ms-settings:workplace |
| Contas de email e do aplicativo  | ms-settings:emailandaccounts |
| Família e outras pessoas | ms-settings:otherusers |
| Configurar um quiosque | MS-Settings: assignedaccess |
| Opções de entrada | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| Sincronizar suas configurações | ms-settings:sync |
| Instalação do Windows Hello | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| Suas informações | ms-settings:yourinfo |

## <a name="apps"></a>Aplicativos

|Página Configurações| URI |
|-------------|-----|
| Aplicativos e Recursos | ms-settings:appsfeatures |
| Recursos do aplicativo | ms-settings:appsfeatures-app (Restaurar, gerenciar complementos & conteúdo para download, etc. para o aplicativo)|
| Aplicativos para sites | ms-settings:appsforwebsites |
| Aplicativos padrão | ms-settings:defaultapps |
| Gerenciar recursos opcionais | ms-settings:optionalfeatures |
| Mapas offline | ms-settings:maps<br/>MS-Settings: Maps-downloadmaps (mapas de download) |
| Aplicativos de inicialização | ms-settings:startupapps |
| Reprodução de vídeo | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|Página Configurações| URI |
|-------------|-----|
| Cortana em meus dispositivos | ms-settings:cortana-notifications |
| Mais detalhes | ms-settings:cortana-moredetails |
| Histórico de & de permissões | ms-settings:cortana-permissions |
| Pesquisando no Windows | MS-Settings: Cortana-WindowsSearch |
| Converse com a Cortana | ms-settings:cortana-language<br/>MS-Settings: Cortana<br/>MS-Settings: Cortana-talktocortana |

> [!NOTE] 
> Esta seção de configurações na área de trabalho será chamada de pesquisa quando o PC for definido como regiões em que a Cortana não está disponível no momento ou que a Cortana foi desabilitada. As páginas específicas da Cortana (Cortana em meus dispositivos e conversa com a Cortana) não serão listadas nesse caso. 

## <a name="devices"></a>Dispositivos

|Página Configurações| URI |
|-------------|-----|
| Reprodução Automática | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| Dispositivos conectados | ms-settings:connecteddevices |
| Câmera padrão | MS-Settings: câmera (**preterida no Windows 10, versão 1809 e posterior**) |
| Mouse e touchpad | ms-settings:mousetouchpad (configuração de touchpad disponíveis somente em dispositivos que possuem touchpad) |
| Caneta e Windows Ink | ms-settings:pen |
| Impressoras e scanners | ms-settings:printers |
| Touchpad | ms-settings:devices-touchpad (disponível somente se houver hardware de touchpad) |
| Digitação | ms-settings:typing |
| USB | ms-settings:usb |
| Wheel | ms-settings:wheel (disponível somente se discagem rápida está emparelhada) |
| Seu telefone | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>Facilidade de Acesso

|Página Configurações| URI |
|-------------|-----|
| Áudio | ms-settings:easeofaccess-audio |
| Legenda oculta | ms-settings:easeofaccess-closedcaptioning |
| Filtros de cores | MS-Settings: easeofaccess-colorfilter |
| Tamanho de ponteiro e de cursor | MS-Settings: easeofaccess-cursorandpointersize |
| Exibir | ms-settings:easeofaccess-display |
| Controle com os olhos | ms-settings:easeofaccess-eyecontrol |
| Fontes | ms-settings:fonts |
| Alto contraste | ms-settings:easeofaccess-highcontrast |
| Teclado | ms-settings:easeofaccess-keyboard |
| Lupa | ms-settings:easeofaccess-magnifier |
| Mouse | ms-settings:easeofaccess-mouse |
| Narrador | ms-settings:easeofaccess-narrator |
| Outras opções | MS-Settings: easeofaccess-otheroptions (**preterido no Windows 10, versão 1809 e posterior**) |
| Fala | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>Extras

|Página Configurações| URI |
|-------------|-----|
| Extras | MS-Settings: extras (disponível somente se "configurações de aplicativos" estiverem instaladas, por exemplo, por um terceiro) |

## <a name="gaming"></a>Jogos

|Página Configurações| URI |
|-------------|-----|
| Difundindo | ms-settings:gaming-broadcasting |
| Barra de jogos | ms-settings:gaming-gamebar |
| DVR de Jogos | ms-settings:gaming-gamedvr |
| Modo de Jogo | ms-settings:gaming-gamemode |
| Jogando em modo de tela inteira | ms-settings:quietmomentsgame |
| TruePlay | MS-Settings: Gaming-trueplay (**preterido no Windows 10, versão 1809 e posterior**) |
| Rede Xbox | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>Página inicial

|Página Configurações| URI |
|-------------|-----|
| Página inicial de configurações | ms-settings: |

## <a name="mixed-reality"></a>Realidade misturada

> [!NOTE]
> Essas configurações só estarão disponíveis se o aplicativo do portal da realidade misturada estiver instalado.

| Página Configurações | URI |
|---------------|-----|
| Áudio e fala | ms-settings:holographic-audio |
| Ambiente | MS-Settings: privacy-Holographic-Environment |
| Tela do headset | MS-Settings: Holographic-Headset |
| Desinstalar | MS-Settings: Holographic-Management |

## <a name="network--internet"></a>Rede e Internet

|Página Configurações| URI |
|-------------|-----|
| Modo avião | ms-settings:network-airplanemode<br/>ms-settings:proximity |
| Rede Celular e SIM | ms-settings:network-cellular |
| Uso de dados | ms-settings:datausage |
| Conexão discada | ms-settings:network-dialup |
| DirectAccess | ms-settings:network-directaccess (disponível somente se DirectAccess estiver habilitada) |
| Ethernet | ms-settings:network-ethernet |
| Gerenciar redes conhecidas | ms-settings:network-wifisettings |
| Hotspot móvel | ms-settings:network-mobilehotspot |
| NFC | ms-settings:nfctransactions |
| Proxy | ms-settings:network-proxy |
| Status | ms-settings:network-status<br/>MS-configurações: rede |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (disponível somente se o dispositivo tiver um adaptador de rede Wi-Fi) |
| Chamada de Wi-Fi | ms-settings:network-wificalling (disponível somente se a chamada de Wi-Fi estiver habilitada) |

## <a name="personalization"></a>Personalização

|Página Configurações| URI |
|-------------|-----|
| Segundo plano | ms-settings:personalization-background |
| Escolher quais pastas são exibidas em Iniciar | ms-settings:personalization-start-places |
| Cores | ms-settings:personalization-colors<br/>MS-Settings: cores |
| Noções básicas | MS-Settings: visão geral (**preterida no Windows 10, versão 1809 e posterior**) |
| Tela de bloqueio | ms-settings:lockscreen |
| Barra de navegação | MS-Settings: personalização-barra de navegação (**preterida no Windows 10, versão 1809 e posterior**) |
| Personalização (categoria) | ms-settings:personalization |
| Iniciar | ms-settings:personalization-start |
| Barra de tarefas | ms-settings:taskbar |
| Temas | ms-settings:themes |

## <a name="phone"></a>Telefone

|Página Configurações| URI |
|-------------|-----|
| Seu telefone | ms-settings:mobile-devices<br/>MS-Settings: dispositivos móveis-hiperphone<br/>MS-Settings: dispositivos móveis-hiperphone-Direct (abre **seu aplicativo de telefone** ) |

## <a name="privacy"></a>Privacidade

|Página Configurações| URI |
|-------------|-----|
| Aplicativos para acessório | MS-Settings: privacy-accessoryapps (**preterido no Windows 10, versão 1809 e posterior**) |
| Informações da conta | ms-settings:privacy-accountinfo |
| Histórico de atividades | ms-settings:privacy-activityhistory |
| ID de Anúncio | MS-Settings: privacy-advertisingid (**preterido no Windows 10, versão 1809 e posterior**) |
| Diagnósticos do aplicativo | ms-settings:privacy-appdiagnostics |
| Downloads automáticos de arquivos | ms-settings:privacy-automaticfiledownloads |
| Aplicativos em Segundo Plano | ms-settings:privacy-backgroundapps |
| Calendário | ms-settings:privacy-calendar |
| Histórico de chamadas | ms-settings:privacy-callhistory |
| Câmera | ms-settings:privacy-webcam |
| Contacts | ms-settings:privacy-contacts |
| Documentos | ms-settings:privacy-documents |
| Email | ms-settings:privacy-email |
| Eye tracker | ms-settings:privacy-eyetracker (exige hardware eyetracker) |
| Comentários e diagnóstico | ms-settings:privacy-feedback |
| Sistema de arquivos | ms-settings:privacy-broadfilesystemaccess |
| Geral | MS-Settings: privacidade ou MS-Settings: privacidade-geral |
| Digitação de & de tinta |ms-settings:privacy-speechtyping |
| Location | ms-settings:privacy-location |
| Mensagens | ms-settings:privacy-messaging |
| Microfone | ms-settings:privacy-microphone |
| Movimento | ms-settings:privacy-motion |
| Notificações | ms-settings:privacy-notifications |
| Outros dispositivos | ms-settings:privacy-customdevices |
| Chamadas telefônicas | MS-Settings: privacidade – telefonemas |
| Imagens | ms-settings:privacy-pictures |
| Rádios | ms-settings:privacy-radios |
| Fala | MS-configurações: privacidade – fala |
| Tarefas | ms-settings:privacy-tasks |
| vídeos | ms-settings:privacy-videos |
| Ativação de voz | MS-Settings: privacy-voiceactivation |

## <a name="surface-hub"></a>Hub de Superfície

|Página Configurações| URI |
|-------------|-----|
| Contas | ms-settings:surfacehub-accounts |
| Limpeza de sessão | ms-settings:surfacehub-sessioncleanup |
| Conferência | ms-settings:surfacehub-calling |
| Gerenciamento de dispositivo em equipe | ms-settings:surfacehub-devicemanagenent |
| Tela de boas-vindas | ms-settings:surfacehub-welcome |

## <a name="system"></a>Sistema

|Página Configurações| URI |
|-------------|-----|
| Sobre | ms-settings:about |
| Configurações avançadas de vídeo | ms-settings:display-advanced (disponível somente em dispositivos que suporte avançado a opções de exibição) |
| Preferências de volume e dispositivo do aplicativo | MS-Settings: apps-volume (**adicionado no Windows 10, versão 1903**)|
| Economia de Bateria | ms-settings:batterysaver (disponível somente em dispositivos que possuem bateria, como um tablet) |
| Configurações de economia de bateria | ms-settings:batterysaver-settings (disponível somente em dispositivos que possuem bateria, como um tablet) |
| Uso da bateria | ms-settings:batterysaver-usagedetails (disponível somente em dispositivos que possuem bateria, como um tablet) |
| Área de Transferência | MS-Settings: área de transferência |
| Exibir | ms-settings:display |
| Locais de salvamento padrão | ms-settings:savelocations |
| Exibir | ms-settings:screenrotation |
| Duplicando minha tela | ms-settings:quietmomentspresentation |
| Durante estes horários | ms-settings:quietmomentsscheduled |
| Criptografia | ms-settings:deviceencryption |
| Assistente de foco | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| Configurações de elementos gráficos | ms-settings:display-advancedgraphics (disponível somente em dispositivos que suportam opções gráficas avançadas) |
| Mensagens | ms-settings:messaging |
| Multitarefa | ms-settings:multitasking |
| Configurações de luz noturna | ms-settings:nightlight |
| Telefone | ms-settings:phone-defaultapps |
| Projetando neste computador | ms-settings:project |
| Experiências compartilhadas | ms-settings:crossdevice |
| Modo tablet | ms-settings:tabletmode |
| Barra de tarefas | ms-settings:taskbar |
| Notificações e ações | ms-settings:notifications |
| Área de Trabalho Remota | ms-settings:remotedesktop |
| Telefone | MS-Settings: telefone (**preterido no Windows 10, versão 1809 e posterior**) |
| Energia e suspensão | ms-settings:powersleep |
| Som | MS-Settings: som |
| Armazenamento | ms-settings:storagesense |
| Sensor de armazenamento | ms-settings:storagepolicies |

## <a name="time-and-language"></a>Hora e idioma

|Página Configurações| URI |
|-------------|-----|
| Data e hora | ms-settings:dateandtime |
| Configurações de IME do Japão | ms-settings:regionlanguage-jpnime (disponível se o editor de método de entrada do Microsoft Japão está instalado) |
| Região | MS-Settings: regionformatting |
| Linguagem | MS-Settings: teclado<br/>ms-settings:regionlanguage<br/>MS-Settings: regionlanguage-bpmfime<br/>MS-Settings: regionlanguage-cangjieime<br/>MS-Settings: regionlanguage-chsime-pinyin-domainlexicon<br/>MS-Settings: regionlanguage-chsime-pinyin-keyconfig<br/>MS-Settings: regionlanguage-chsime-pinyin-UDP<br/>MS-Settings: regionlanguage-chsime-Wubi-UDP<br/>MS-Settings: regionlanguage-quickime |
| Configurações de IME Pinyin | ms-settings:regionlanguage-chsime-pinyin (disponível se o editor de método de entrada do Microsoft Japão está instalado) |
| Fala | ms-settings:speech |
| Configurações do Wubi IME  | ms-settings:regionlanguage-chsime-wubi (disponível se o editor de método de entrada do Microsoft Wubi está instalado) |

## <a name="update--security"></a>Atualização e segurança

|Página Configurações| URI |
|-------------|-----|
| Ativação | ms-settings:activation |
| Backup | ms-settings:backup |
| Otimização de Entrega | ms-settings:delivery-optimization |
| Localizar meu dispositivo | ms-settings:findmydevice |
| Para desenvolvedores | ms-settings:developers |
| Recuperação | ms-settings:recovery |
| Solucionar problemas | ms-settings:troubleshoot |
| Segurança do Windows | ms-settings:windowsdefender |
| Programa Windows Insider | ms-settings:windowsinsider (somente presente se o usuário está inscrito no WIP)<br/>MS-Settings: windowsinsider-Optin |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Opções Avançadas do Windows Update | ms-settings:windowsupdate-options |
| Opções de reinicialização do Windows Update | ms-settings:windowsupdate-restartoptions |
| Visualização de histórico do Windows Update | ms-settings:windowsupdate-history |

## <a name="user-accounts"></a>Contas de usuário

|Página Configurações| URI |
|-------------|-----|
| Provisionamento | ms-settings:workplace-provisioning (disponível somente se a empresa tiver implantado um pacote de provisionamento) |
| Provisionamento | ms-settings:provisioning (disponível somente em dispositivo móvel e se a empresa tiver implantado um pacote de provisionamento) |
| Windows Anywhere | ms-settings:windowsanywhere (o dispositivo deve ser em qualquer lugar-compatíveis com Windows) |
