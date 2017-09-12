---
author: TylerMSFT
title: "Iniciar o app Configurações do Windows"
description: "Saiba como iniciar o app Configurações do Windows a partir de seu app. Este tópico descreve o esquema de URI ms-settings. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.author: twhitney
ms.date: 06/12/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 2a30e14f7c275c48f52253157fc9c67bf05d259e
ms.sourcegitcommit: 00c3f5a1208bd0125f5b275f972cf2a82d8eb9b6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/13/2017
---
# <a name="launch-the-windows-settings-app"></a>Iniciar o app Configurações do Windows

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Saiba como iniciar o aplicativo Configurações do Windows a partir de seu aplicativo. Este tópico descreve o esquema de URI **ms-settings:**. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas.

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

## <a name="ms-settings-uri-scheme-reference"></a>Referência de esquema de URI ms-settings:

Use os seguintes URIs para abrir várias páginas do aplicativo Configurações.

> Observe que a disponibilidade de uma página de configurações varia de acordo com a SKU do Windows. Nem todas as páginas de configurações disponíveis no Windows 10 para desktop estão disponíveis no Windows 10 Mobile e vice-versa. A coluna de observações também informa os requisitos adicionais que devem ser atendidos para que uma página fique disponível.

<table border="1">
 <tr>
  <th>Categoria</th>
  <th>Página de configurações</th>
  <th>URI</th>
  <th>Observações</th>
 </tr>
 <tr>
  <td rowspan="6">Contas</td>
  <td>Acessar conta corporativa ou de estudante</td>
  <td>ms-settings:workplace</td>
  <td></td>
 </tr>
 <tr>
  <td>Contas de email e aplicativo</td>
  <td>ms-settings:emailandaccounts</td>
  <td></td>
 </tr>
 <tr>
  <td>Família e outras pessoas</td>
  <td>ms-settings:otherusers</td>
  <td></td>
 </tr>
 <tr>
  <td>Opções de entrada</td>
  <td>ms-settings:signinoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Sincronizar suas configurações</td>
  <td>ms-settings:sync</td>
  <td></td>
 </tr>
 <tr>
  <td>Suas informações</td>
  <td>ms-settings:yourinfo</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="4">Aplicativos</td>
  <td>Aplicativos e recursos</td>
  <td>ms-settings:appsfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicativos para sites</td>
  <td>ms-settings:appsforwebsites</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicativos padrão</td>
  <td>ms-settings:defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicativos e recursos</td>
  <td>ms-settings:optionalfeatures</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="12">Dispositivos</td>
  <td>USB</td>
  <td>ms-settings:usb</td>
  <td></td>
 </tr>
 <tr>
  <td>Áudio e fala</td>
  <td>ms-settings:holographic-audio</td>
  <td>Disponível somente se o aplicativo do Portal de Realidade Misturada estiver instalado (disponível na Windows Store)</td>
 </tr>
 <tr>
  <td>Reprodução Automática</td>
  <td>ms-settings:autoplay</td>
  <td></td>
 </tr>
 <tr>
  <td>Touchpad</td>
  <td>ms-settings:devices-touchpad</td>
  <td>Disponível somente se houver hardware de touchpad</td>
 </tr>
 <tr>
  <td>Caneta e Windows Ink</td>
  <td>ms-settings:pen</td>
  <td></td>
 </tr>
 <tr>
  <td>Impressoras e scanners</td>
  <td>ms-settings:printers</td>
  <td></td>
 </tr>
 <tr>
  <td>Digitação</td>
  <td>ms-settings:typing</td>
  <td></td>
 </tr>
 <tr>
  <td>Roda</td>
  <td>ms-settings:wheel</td>
  <td>Disponível somente se o Dial estiver emparelhado</td>
 </tr>
 <tr>
  <td>Câmera padrão</td>
  <td>ms-settings:camera</td>
  <td></td>
 </tr>
 <tr>
  <td>Bluetooth</td>
  <td>ms-settings:bluetooth</td>
  <td></td>
 </tr>
 <tr>
  <td>Dispositivos conectados</td>
  <td>ms-settings:connecteddevices</td>
  <td></td>
 </tr>
 <tr>
  <td>Mouse e touchpad</td>
  <td>ms-settings:mousetouchpad</td>
  <td>Configurações do touchpad disponíveis somente em dispositivos que possuem um touchpad</td>
 </tr>
 <tr>
  <td rowspan="7">Facilidade de Acesso</td>
  <td>Narrador</td>
  <td>ms-settings:easeofaccess-narrator</td>
  <td></td>
 </tr>
 <tr>
  <td>Lupa</td>
  <td>ms-settings:easeofaccess-magnifier</td>
  <td></td>
 </tr>
 <tr>
  <td>Alto contraste</td>
  <td>ms-settings:easeofaccess-highcontrast</td>
  <td></td>
 </tr>
 <tr>
  <td>Legendas ocultas</td>
  <td>ms-settings:easeofaccess-closedcaptioning</td>
  <td></td>
 </tr>
 <tr>
  <td>Teclado</td>
  <td>ms-settings:easeofaccess-keyboard</td>
  <td></td>
 </tr>
 <tr>
  <td>Mouse</td>
  <td>ms-settings:easeofaccess-mouse</td>
  <td></td>
 </tr>
 <tr>
  <td>Outras opções</td>
  <td>ms-settings:easeofaccess-otheroptions</td>
 </tr>
 <tr>
  <td>Extras</td>
  <td>Extras</td>
  <td>ms-settings:extras</td>
  <td>Disponível somente se "aplicativos de configurações" estiverem instalados (por exemplo, por terceiros)</td>
 </tr>
 <tr>
  <td rowspan="4">Jogos</td>
  <td>Transmissão</td>
  <td>ms-settings:gaming-broadcasting</td>
  <td></td>
 </tr>
 <tr>
  <td>Barra de jogos</td>
  <td>ms-settings:gaming-gamebar</td>
  <td></td>
 </tr>
 <tr>
  <td>DVR de Jogos</td>
  <td>ms-settings:gaming-gamedvr</td>
  <td></td>
 </tr>
 <tr>
  <td>Modo de Jogo</td>
  <td>ms-settings:gaming-gamemode</td>
  <td></td>
 </tr>
 <tr>
  <td>Home page</td>
  <td>Página de aterrissagem para Configurações</td>
  <td>ms-settings:</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="10">Rede e Internet</td>
  <td>Ethernet</td>
  <td>ms-settings:network-ethernet</td>
  <td></td>
 </tr>
 <tr>
  <td>VPN</td>
  <td>ms-settings:network-vpn</td>
  <td></td>
 </tr>
 <tr>
  <td>Conexão discada</td>
  <td>ms-settings:network-dialup</td>
  <td></td>
 </tr>
 <tr>
  <td>DirectAccess</td>
  <td>ms-settings:network-directaccess</td>
  <td>Disponível somente se o DirectAccess estiver habilitado</td>
 </tr>
 <tr>
  <td>Chamadade Wi-Fi</td>
  <td>ms-settings:network-wificalling</td>
  <td>Disponível somente se a chamada de Wi-Fi estiver habilitada</td>
 </tr>
 <tr>
  <td>Uso de dados</td>
  <td>ms-settings:datausage</td>
  <td></td>
 </tr>
 <tr>
  <td>Rede Celular e SIM</td>
  <td>ms-settings:network-cellular</td>
  <td></td>
 </tr>
 <tr>
  <td>Hotspot móvel</td>
  <td>ms-settings:network-mobilehotspot</td>
  <td></td>
 </tr>
 <tr>
  <td>Proxy</td>
  <td>ms-settings:network-proxy</td>
  <td></td>
 </tr>
 <tr>
  <td>Status</td>
  <td>ms-settings:network-status</td>
  <td></td>
 </tr>
 <tr>
  <td>Gerenciar redes conhecidas</td>
  <td>ms-settings:network-wifisettings</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="3">Rede e acesso sem fio</td>
  <td>NFC</td>
  <td>ms-settings:nfctransactions</td>
  <td></td>
 </tr>
 <tr>
  <td>Wi-Fi</td>
  <td>ms-settings:network-wifi</td>
  <td>Disponível somente se o dispositivo tiver um adaptador de rede Wi-Fi</td>
 </tr>
 <tr>
  <td>Modo avião</td>
  <td>ms-settings:network-airplanemode</td>
  <td>Usar ms-settings:proximity no Windows 8.x</td>
 </tr>
 <tr>
  <td rowspan="10">Personalização</td>
  <td>Iniciar</td>
  <td>ms-settings:personalization-start</td>
  <td></td>
 </tr>
 <tr>
  <td>Temas</td>
  <td>ms-settings:themes</td>
  <td></td>
 </tr>
 <tr>
  <td>Tela de espera</td>
  <td>ms-settings:personalization-glance</td>
  <td></td>
 </tr>
 <tr>
  <td>Barra de navegação</td>
  <td>ms-settings:personalization-navbar</td>
  <td></td>
 </tr>
 <tr>
  <td>Personalização (categoria)</td>
   <td>ms-settings:personalization</td>
   <td></td>
 </tr>
 <tr>
  <td>Tela de fundo</td>
   <td>ms-settings:personalization-background</td>
   <td></td>
 </tr>
 <tr>
  <td>Cores</td>
   <td>ms-settings:personalization-colors</td>
   <td></td>
 </tr>
 <tr>
  <td>Sons</td>
   <td>ms-settings:sounds</td>
   <td></td>
 </tr>
 <tr>
  <td>Tela de bloqueio</td>
   <td>ms-settings:lockscreen</td>
   <td></td>
 </tr>
 <tr>
  <td>Barra de tarefas</td>
   <td>ms-settings:taskbar</td>
   <td></td>
 </tr>
 <tr>
  <td rowspan="22">Privacidade</td>
  <td>Diagnósticos do aplicativo</td>
   <td>ms-settings:privacy-appdiagnostics</td>
   <td></td>
 </tr>
 <tr>
  <td>Notificações</td>
   <td>ms-settings:privacy-notifications</td>
   <td></td>
 </tr>
 <tr>
  <td>Tarefas</td>
   <td>ms-settings:privacy-tasks</td>
   <td></td>
 </tr>
 <tr>
  <td>Geral</td>
   <td>ms-settings:privacy-general</td>
   <td></td>
 </tr>
 <tr>
  <td>Aplicativos para acessório</td>
   <td>ms-settings:privacy-accessoryapps</td>
   <td></td>
 </tr>
 <tr>
  <td>ID de anúncio</td>
   <td>ms-settings:privacy-advertisingid</td>
   <td></td>
 </tr>
 <tr>
  <td>Chamadas telefônicas</td>
   <td>ms-settings:privacy-phonecall</td>
   <td></td>
 </tr>
 <tr>
  <td>Local</td>
   <td>ms-settings:privacy-location</td>
   <td></td>
 </tr>
 <tr>
  <td>Câmera</td>
   <td>ms-settings:privacy-webcam</td>
   <td></td>
 </tr>
 <tr>
  <td>Microfone</td>
   <td>ms-settings:privacy-microphone</td>
   <td></td>
 </tr>
 <tr>
  <td>Movimento</td>
   <td>ms-settings:privacy-motion</td>
   <td></td>
 </tr>
 <tr>
  <td>Fala, escrita à tinta e digitação</td>
   <td>ms-settings:privacy-speechtyping</td>
   <td></td>
 </tr>
 <tr>
  <td>Informações da conta</td>
   <td>ms-settings:privacy-accountinfo</td>
   <td></td>
 </tr>
 <tr>
  <td>Contatos</td>
   <td>ms-settings:privacy-contacts</td>
   <td></td>
 </tr>
 <tr>
  <td>Calendário</td>
   <td>ms-settings:privacy-calendar</td>
   <td></td>
 </tr>
 <tr>
  <td>Histórico de chamadas</td>
   <td>ms-settings:privacy-callhistory</td>
   <td></td>
 </tr>
 <tr>
  <td>Email</td>
  <td>ms-settings:privacy-email</td>
  <td></td>
 </tr>
 <tr>
  <td>Mensagens</td>
    <td>ms-settings:privacy-messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>Rádios</td>
    <td>ms-settings:privacy-radios</td>
  <td></td>
 </tr>
 <tr>
  <td>Aplicativos em segundo plano</td>
    <td>ms-settings:privacy-backgroundapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Outros dispositivos</td>
    <td>ms-settings:privacy-customdevices</td>
  <td></td>
 </tr>
 <tr>
  <td>Comentários e diagnóstico</td>
    <td>ms-settings:privacy-feedback</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Surface Hub</td>
  <td>Contas</td>
    <td>ms-settings:surfacehub-accounts</td>
      <td></td>
  </tr>
  <tr>
    <td>Conferências em equipe</td>
      <td>ms-settings:surfacehub-calling</td>
      <td></td>
  </tr>
  <tr>
    <td>Gerenciamento de dispositivo em equipe</td>
      <td>ms-settings:surfacehub-devicemanagenent</td>
      <td></td>
  </tr>
  <tr>
    <td>Limpeza de sessão</td>
      <td>ms-settings:surfacehub-sessioncleanup</td>
      <td></td>
  </tr>
  <tr>
    <td>Tela de boas-vindas</td>
      <td>ms-settings:surfacehub-welcome</td>
      <td></td>
  </tr>
    <td rowspan="19">Sistema</td>
    <td>Experiências compartilhadas</td>
      <td>ms-settings:crossdevice</td>
    <td></td>
 </tr>
 <tr>
  <td>Tela</td>
    <td>ms-settings:display</td>
  <td></td>
 </tr>
 <tr>
  <td>Multitarefa</td>
    <td>ms-settings:multitasking</td>
  <td></td>
 </tr>
 <tr>
  <td>Projetando neste computador</td>
    <td>ms-settings:project</td>
  <td></td>
 </tr>
 <tr>
  <td>Modo tablet</td>
    <td>ms-settings:tabletmode</td>
  <td></td>
 </tr>
 <tr>
  <td>Barra de tarefas</td>
    <td>ms-settings:taskbar</td>
  <td></td>
 </tr>
 <tr>
  <td>Telefone</td>
    <td>ms-settings:phone-defaultapps</td>
  <td></td>
 </tr>
 <tr>
  <td>Tela</td>
    <td>ms-settings:screenrotation</td>
  <td></td>
 </tr>
 <tr>
  <td>Notificações e ações</td>
    <td>ms-settings:notifications</td>
  <td></td>
 </tr>
 <tr>
  <td>Telefone</td>
    <td>ms-settings:phone</td>
  <td></td>
 </tr>
 <tr>
  <td>Mensagens</td>
    <td>ms-settings:messaging</td>
  <td></td>
 </tr>
 <tr>
  <td>Economia de bateria</td>
  <td>ms-settings:batterysaver</td>
  <td>Disponível somente em dispositivos que possuem bateria, como um tablet</td>
 </tr>
 <tr>
  <td>Uso da bateria</td>
  <td>ms-settings:batterysaver-usagedetails</td>
  <td>Disponível somente em dispositivos que possuem bateria, como um tablet</td>
 </tr>
 <tr>
  <td>Energia e suspensão</td>
  <td>ms-settings:powersleep</td>
  <td></td>
 </tr>
 <tr>
  <td>Sobre</td>
    <td>ms-settings:about</td>
  <td></td>
 </tr>
 <tr>
  <td>Armazenamento</td>
    <td>ms-settings:storagesense</td>
  <td></td>
 </tr>
 <tr>
  <td>Sensor de armazenamento</td>
    <td>ms-settings:storagepolicies</td>
  <td></td>
 </tr>
 <tr>
  <td>Criptografia</td>
    <td>ms-settings:deviceencryption</td>
  <td></td>
 </tr>
 <tr>
  <td>Mapas offline</td>
    <td>ms-settings:maps</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="5">Hora e idioma</td>
  <td>Data e hora</td>
    <td>ms-settings:dateandtime</td>
  <td></td>
 </tr>
 <tr>
  <td>Região e idioma</td>
    <td>ms-settings:regionlanguage</td>
  <td></td>
 </tr>
 <tr>
     <td>Idioma de fala</td>
     <td>ms-settings:speech</td>
     <td></td>
 </tr>
 <tr>
     <td>Teclado Pinyin</td>
     <td>ms-settings:regionlanguage-chsime-pinyin</td>
     <td>Disponível se o editor do método de entrada Microsoft Pinyin estiver instalado</td>
 </tr>
 <tr>
     <td>Modo de entrada Wubi</td>
     <td>ms-settings:regionlanguage-chsime-wubi</td>
     <td>Disponível se o editor do método de entrada Microsoft Wubi estiver instalado</td>
 </tr>
 <tr>
  <td rowspan="13">Atualização e segurança</td>
  <td>Configuração do Windows Hello</td>
    <td>ms-settings:signinoptions-launchfaceenrollment<br>ms-settings:signinoptions-launchfingerprintenrollment</td>
  </tr>
  <tr>
    <td>Backup</td>
      <td>ms-settings:backup</td>
    <td></td>
 </tr>
 <tr>
  <td>Localizar Meu Dispositivo</td>
    <td>ms-settings:findmydevice</td>
  <td></td>
 </tr>
 <tr>
  <td>Programa Windows Insider</td>
  <td>ms-settings:windowsinsider</td>
  <td>Presente somente se o usuário estiver inscrito no WIP</td>
 </tr>
 <tr>
  <td>Windows Update</td>
  <td>ms-settings:windowsupdate</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-history</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-options</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-restartoptions</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Update</td>
    <td>ms-settings:windowsupdate-action</td>
  <td></td>
 </tr>
 <tr>
  <td>Ativação</td>
    <td>ms-settings:activation</td>
  <td></td>
 </tr>
 <tr>
  <td>Recuperação</td>
    <td>ms-settings:recovery</td>
  <td></td>
 </tr>
 <tr>
  <td>Solução de problemas</td>
    <td>ms-settings:troubleshoot</td>
  <td></td>
 </tr>
 <tr>
  <td>Windows Defender</td>
    <td>ms-settings:windowsdefender</td>
  <td></td>
 </tr>
 <tr>
  <td>Para desenvolvedores</td>
    <td>ms-settings:developers</td>
  <td></td>
 </tr>
 <tr>
  <td rowspan="2">Contas de usuário</td>
  <td>Windows Anywhere</td>
  <td>ms-settings:windowsanywhere</td>
  <td>O dispositivo deve ser compatível com Windows Anywhere</td>
 </tr>
 <tr>
  <td>Provisionamento</td>
  <td>ms-settings:workplace-provisioning</td>
  <td>Disponível somente se a empresa tiver implantado um pacote de provisionamento</td>
 </tr>
</table><br/>  
