---
title: Iniciar o app Configurações do Windows
description: Saiba como iniciar o app Configurações do Windows a partir de seu app. Este tópico descreve o esquema de URI ms-settings. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas.
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.date: 01/08/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 87074e9871fae6be32e2862fc547097898baede8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57646951"
---
# <a name="launch-the-windows-settings-app"></a>Iniciar o app Configurações do Windows


**APIs importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Saiba como iniciar o aplicativo Configurações do Windows. Este tópico descreve o **ms-configurações:** Esquema de URI. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas.

A inicialização do aplicativo Configurações é uma parte importante da escrita de um aplicativo com detecção de privacidade. Se seu aplicativo não pode acessar um recurso confidencial, é recomendável fornecer ao usuário um link conveniente para as configurações de privacidade desse recurso. Para obter mais informações, consulte [Diretrizes para aplicativos com detecção de privacidade](https://msdn.microsoft.com/library/windows/apps/hh768223).

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

Como alternativa, seu aplicativo pode chamar o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar o aplicativo **Configurações** do código. Este exemplo mostra como iniciar a página de configurações de privacidade da câmera usando o URI `ms-settings:privacy-webcam`.

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

O código acima inicia a página de configurações de privacidade da câmera:

![configurações de privacidade da câmera.](images/privacyawarenesssettingsapp.png)

Para obter mais informações como URIs de inicialização, consulte [Iniciar o aplicativo padrão para um URI](launch-default-app.md).

## <a name="ms-settings-uri-scheme-reference"></a>ms-settings: Referência de esquema URI

Use os seguintes URIs para abrir várias páginas do aplicativo Configurações.

> Observe que a disponibilidade de uma página de configurações varia de acordo com a SKU do Windows. Nem todas as páginas de configurações disponíveis no Windows 10 para desktop estão disponíveis no Windows 10 Mobile e vice-versa. A coluna de anotações também captura requisitos adicionais que devem ser atendidos para uma página para estar disponível.

<!-- TODO: 
* ms-settings:controlcenter
* ms-settings:cortana-windowssearch
* ms-settings:holographic
* ms-settings:keyboard-advanced
* ms-settings:regionlanguage-adddisplaylanguage (crashed)
* ms-settings:regionlanguage-setdisplaylanguage (crashed)
* ms-settings:signinoptions-launchpinenrollment
* ms-settings:storagecleanup
* ms-settings:update-security -->

## <a name="accounts"></a>Contas

|Página de configurações| URI |
|-------------|-----|
| Acessar trabalho ou escola | ms-settings:workplace |
| Contas de email e aplicativo  | ms-settings:emailandaccounts |
| Família e outras pessoas | ms-settings:otherusers |
| Configurar um quiosque | ms-settings:assignedaccess |
| Opções de entrada | ms-settings:signinoptions<br>ms-settings:signinoptions-dynamiclock |
| Sincronizar suas configurações | ms-settings:sync |
| Instalação do Windows Hello | ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment |
| Suas informações | ms-settings:yourinfo |

## <a name="apps"></a>Aplicativos

|Página de configurações| URI |
|-------------|-----|
| Aplicativos e recursos | ms-settings:appsfeatures |
| Recursos do aplicativo | ms-settings:appsfeatures-app (Restaurar, gerenciar complementos & conteúdo para download, etc. para o aplicativo)|
| Aplicativos para sites | ms-settings:appsforwebsites |
| Aplicativos padrão | ms-settings:defaultapps |
| Gerenciar recursos opcionais | ms-settings:optionalfeatures |
| Mapas offline | ms-settings:maps<br/>MS-: mapas de configurações-downloadmaps (mapas de Download) |
| Aplicativos de inicialização | ms-settings:startupapps |
| Reprodução de vídeo | ms-settings:videoplayback |

## <a name="cortana"></a>Cortana

|Página de configurações| URI |
|-------------|-----|
| & Histórico de permissões | ms-settings:cortana-permissions |
| Mais detalhes | ms-settings:cortana-moredetails |
| Cortana em meus dispositivos | ms-settings:cortana-notifications |
| Converse com a Cortana | ms-settings:cortana-language<br/>ms-settings:cortana<br/>ms-settings:cortana-talktocortana |

> [!NOTE] 
> Esta seção de configurações na área de trabalho será chamada pesquisa quando o computador está definido como regiões em que a Cortana não está disponível no momento ou Cortana foi desabilitada. Nesse caso, páginas específicas de Cortana (Cortana em todos os meus dispositivos) e falar Cortana não serão listadas. 

## <a name="devices"></a>Dispositivos

|Página de configurações| URI |
|-------------|-----|
| Áudio e fala | ms-settings:holographic-audio (disponível somente se o aplicativo do Portal de Realidade Misturada é instalado — disponíveis na Microsoft Store) |
| AutoPlay | ms-settings:autoplay |
| Bluetooth | ms-settings:bluetooth |
| Dispositivos conectados | ms-settings:connecteddevices |
| Câmera padrão | MS-câmera: configurações (**preteridos no Windows 10, versão posterior e 1809**) |
| Mouse e touchpad | ms-settings:mousetouchpad (configuração de touchpad disponíveis somente em dispositivos que possuem touchpad) |
| Caneta e Windows Ink | ms-settings:pen |
| Impressoras e scanners | ms-settings:printers |
| Touchpad | ms-settings:devices-touchpad (disponível somente se houver hardware de touchpad) |
| Digitação | ms-settings:typing |
| USB | ms-settings:usb |
| Roda | ms-settings:wheel (disponível somente se discagem rápida está emparelhada) |
| Seu telefone | ms-settings:mobile-devices  |

## <a name="ease-of-access"></a>Facilidade de acesso

|Página de configurações| URI |
|-------------|-----|
| Áudio | ms-settings:easeofaccess-audio |
| Legendas ocultas | ms-settings:easeofaccess-closedcaptioning |
| Filtros de cores | ms-settings:easeofaccess-colorfilter |
| Tamanho do cursor & ponteiro | ms-settings:easeofaccess-cursorandpointersize |
| Vídeo | ms-settings:easeofaccess-display |
| Controle com os olhos | ms-settings:easeofaccess-eyecontrol |
| Fontes | ms-settings:fonts |
| Alto contraste | ms-settings:easeofaccess-highcontrast |
| Headset para holografia | ms-settings:holographic-headset (exige hardware holográfico) |
| Teclado | ms-settings:easeofaccess-keyboard |
| Lupa | ms-settings:easeofaccess-magnifier |
| Mouse | ms-settings:easeofaccess-mouse |
| Narrador | ms-settings:easeofaccess-narrator |
| Outras opções | MS-configurações: easeofaccess-otheroptions (**preteridos no Windows 10, versão posterior e 1809**) |
| Controle por voz | ms-settings:easeofaccess-speechrecognition |

## <a name="extras"></a>Extras

|Página de configurações| URI |
|-------------|-----|
| Extras | ms-settings:extras (disponível apenas se "settings apps" estiver instalada, por ex. por terceiros) |

## <a name="gaming"></a>Jogos

|Página de configurações| URI |
|-------------|-----|
| Difundindo | ms-settings:gaming-broadcasting |
| Barra de jogos | ms-settings:gaming-gamebar |
| DVR de Jogos | ms-settings:gaming-gamedvr |
| Modo de Jogo | ms-settings:gaming-gamemode |
| Jogando em modo de tela inteira | ms-settings:quietmomentsgame |
| TruePlay | MS-configurações: jogos-trueplay (**preteridos no Windows 10, versão posterior e 1809**) |
| Rede Xbox | ms-settings:gaming-xboxnetworking |

## <a name="home-page"></a>Home page

|Página de configurações| URI |
|-------------|-----|
| Página inicial de configurações | ms-settings: |


## <a name="network--internet"></a>Rede e Internet

|Página de configurações| URI |
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
| Status | ms-settings:network-status<br/>ms-settings:network |
| VPN | ms-settings:network-vpn |
| Wi-Fi | ms-settings:network-wifi (disponível somente se o dispositivo tiver um adaptador de rede Wi-Fi) |
| Chamada de Wi-Fi | ms-settings:network-wificalling (disponível somente se a chamada de Wi-Fi estiver habilitada) |

## <a name="personalization"></a>Personalização

|Página de configurações| URI |
|-------------|-----|
| Histórico | ms-settings:personalization-background |
| Escolher quais pastas são exibidas em Iniciar | ms-settings:personalization-start-places |
| Cores | ms-settings:personalization-colors<br/>ms-settings:colors |
| Noções básicas | MS-configurações: personalização-rapidamente (**preteridos no Windows 10, versão posterior e 1809**) |
| Tela de bloqueio | ms-settings:lockscreen |
| Barra de navegação | MS-configurações: personalização-barra de navegação (**preteridos no Windows 10, versão posterior e 1809**) |
| Personalização (categoria) | ms-settings:personalization |
| Início | ms-settings:personalization-start |
| Barra de tarefas | ms-settings:taskbar |
| Temas | ms-settings:themes |

## <a name="phone"></a>Phone

|Página de configurações| URI |
|-------------|-----|
| Seu telefone | ms-settings:mobile-devices<br/>ms-settings:mobile-devices-addphone<br/>MS-configurações: mobile-dispositivos-addphone-direto (abre **seu telefone** app) |

## <a name="privacy"></a>Privacidade

|Página de configurações| URI |
|-------------|-----|
| Aplicativos para acessório | MS-configurações: privacidade-accessoryapps (**preteridos no Windows 10, versão posterior e 1809**) |
| Informações da conta | ms-settings:privacy-accountinfo |
| Histórico de atividades | ms-settings:privacy-activityhistory |
| ID de anúncio | MS-configurações: privacidade-advertisingid (**preteridos no Windows 10, versão posterior e 1809**) |
| Diagnóstico de aplicativo | ms-settings:privacy-appdiagnostics |
| Downloads automáticos de arquivos | ms-settings:privacy-automaticfiledownloads |
| Aplicativos em segundo plano | ms-settings:privacy-backgroundapps |
| Calendário | ms-settings:privacy-calendar |
| Histórico de chamadas | ms-settings:privacy-callhistory |
| Câmera | ms-settings:privacy-webcam |
| Contatos | ms-settings:privacy-contacts |
| Documentos | ms-settings:privacy-documents |
| Email | ms-settings:privacy-email |
| Eye tracker | ms-settings:privacy-eyetracker (exige hardware eyetracker) |
| Comentários e diagnóstico | ms-settings:privacy-feedback |
| Sistema de arquivos | ms-settings:privacy-broadfilesystemaccess |
| Geral | ms-settings:privacy-general |
| Location | ms-settings:privacy-location |
| Sistema de mensagens | ms-settings:privacy-messaging |
| Microfone | ms-settings:privacy-microphone |
| Movimento | ms-settings:privacy-motion |
| Notificações | ms-settings:privacy-notifications |
| Outros dispositivos | ms-settings:privacy-customdevices |
| Imagens | ms-settings:privacy-pictures |
| Chamadas telefônicas | MS-configurações: privacidade-chamada telefônica (**preteridos no Windows 10, versão posterior e 1809**) |
| Rádios | ms-settings:privacy-radios |
| Controle por voz, escrita à tinta e digitação |ms-settings:privacy-speechtyping |
| Tarefas | ms-settings:privacy-tasks |
| Vídeos | ms-settings:privacy-videos |

## <a name="surface-hub"></a>Surface Hub

|Página de configurações| URI |
|-------------|-----|
| Contas | ms-settings:surfacehub-accounts |
| Limpeza de sessão | ms-settings:surfacehub-sessioncleanup |
| Conferência | ms-settings:surfacehub-calling |
| Gerenciamento de dispositivo em equipe | ms-settings:surfacehub-devicemanagenent |
| Tela de boas-vindas | ms-settings:surfacehub-welcome |

## <a name="system"></a>Sistema

|Página de configurações| URI |
|-------------|-----|
| Sobre | ms-settings:about |
| Configurações avançadas de vídeo | ms-settings:display-advanced (disponível somente em dispositivos que suporte avançado a opções de exibição) |
| Economia de Bateria | ms-settings:batterysaver (disponível somente em dispositivos que possuem bateria, como um tablet) |
| Configurações de economia de bateria | ms-settings:batterysaver-settings (disponível somente em dispositivos que possuem bateria, como um tablet) |
| Uso da bateria | ms-settings:batterysaver-usagedetails (disponível somente em dispositivos que possuem bateria, como um tablet) |
| Área de Transferência | ms-settings:clipboard |
| Vídeo | ms-settings:display |
| Locais de salvamento padrão | ms-settings:savelocations |
| Vídeo | ms-settings:screenrotation |
| Duplicando minha tela | ms-settings:quietmomentspresentation |
| Durante estes horários | ms-settings:quietmomentsscheduled |
| Criptografia | ms-settings:deviceencryption |
| Assistente de foco | ms-settings:quiethours <br> ms-settings:quietmomentshome |
| Configurações de elementos gráficos | ms-settings:display-advancedgraphics (disponível somente em dispositivos que suportam opções gráficas avançadas) |
| Sistema de mensagens | ms-settings:messaging |
| Multitarefa | ms-settings:multitasking |
| Configurações de luz noturna | ms-settings:nightlight |
| Phone | ms-settings:phone-defaultapps |
| Projetando neste computador | ms-settings:project |
| Experiências compartilhadas | ms-settings:crossdevice |
| Modo tablet | ms-settings:tabletmode |
| Barra de tarefas | ms-settings:taskbar |
| Notificações e ações | ms-settings:notifications |
| Área de Trabalho Remota | ms-settings:remotedesktop |
| Phone | MS-configurações: telefone (**preteridos no Windows 10, versão posterior e 1809**) |
| Ligar/Desligar e suspensão | ms-settings:powersleep |
| Som | ms-settings:sound |
| Armazenamento | ms-settings:storagesense |
| Sensor de Armazenamento | ms-settings:storagepolicies |

## <a name="time-and-language"></a>Hora e idioma

|Página de configurações| URI |
|-------------|-----|
| Data e hora | ms-settings:dateandtime |
| Configurações de IME do Japão | ms-settings:regionlanguage-jpnime (disponível se o editor de método de entrada do Microsoft Japão está instalado) |
| Idioma | ms-settings:keyboard<br/>ms-settings:regionlanguage<br/>ms-settings:regionlanguage-bpmfime<br/>ms-settings:regionlanguage-cangjieime<br/>ms-settings:regionlanguage-chsime-pinyin-domainlexicon<br/>ms-settings:regionlanguage-chsime-pinyin-keyconfig<br/>ms-settings:regionlanguage-chsime-pinyin-udp<br/>ms-settings:regionlanguage-chsime-wubi-udp<br/>ms-settings:regionlanguage-quickime |
| Configurações de IME Pinyin | ms-settings:regionlanguage-chsime-pinyin (disponível se o editor de método de entrada do Microsoft Japão está instalado) |
| Controle por voz | ms-settings:speech |
| Configurações do Wubi IME  | ms-settings:regionlanguage-chsime-wubi (disponível se o editor de método de entrada do Microsoft Wubi está instalado) |

## <a name="update--security"></a>Atualização e segurança

|Página de configurações| URI |
|-------------|-----|
| Ativação | ms-settings:activation |
| Backup | ms-settings:backup |
| Otimização de Entrega | ms-settings:delivery-optimization |
| Localizar Meu Dispositivo | ms-settings:findmydevice |
| Para desenvolvedores | ms-settings:developers |
| Recuperação | ms-settings:recovery |
| Solução de problemas | ms-settings:troubleshoot |
| Segurança do Windows | ms-settings:windowsdefender |
| Programa Windows Insider | ms-settings:windowsinsider (somente presente se o usuário está inscrito no WIP)<br/>ms-settings:windowsinsider-optin |
| Windows Update | ms-settings:windowsupdate<br>ms-settings:windowsupdate-action |
| Opções Avançadas do Windows Update | ms-settings:windowsupdate-options |
| Opções de reinicialização do Windows Update | ms-settings:windowsupdate-restartoptions |
| Visualização de histórico do Windows Update | ms-settings:windowsupdate-history |

## <a name="user--accounts"></a>Contas de usuário

|Página de configurações| URI |
|-------------|-----|
| Provisionamento | ms-settings:workplace-provisioning (disponível somente se a empresa tiver implantado um pacote de provisionamento) |
| Provisionamento | ms-settings:provisioning (disponível somente em dispositivo móvel e se a empresa tiver implantado um pacote de provisionamento) |
| Windows Anywhere | ms-settings:windowsanywhere (o dispositivo deve ser em qualquer lugar-compatíveis com Windows) |
