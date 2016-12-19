---
author: TylerMSFT
title: "Iniciar o app Configurações do Windows"
description: "Saiba como iniciar o aplicativo Configurações do Windows a partir de seu aplicativo. Este tópico descreve o esquema de URI ms-settings. Use esse esquema de URI para iniciar o app Configurações do Windows para páginas de configurações específicas."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
translationtype: Human Translation
ms.sourcegitcommit: 1135feec72510e6cbe955161ac169158a71097b9
ms.openlocfilehash: f762d7eb70a0e9119f32350a815691109f994c75

---

# <a name="launch-the-windows-settings-app"></a>Iniciar o app Configurações do Windows

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

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

Use os seguintes URIs para abrir várias páginas do aplicativo Configurações. A coluna SKUs compatíveis indica se a página de configurações existe no Windows 10 para edições desktop (Home, Pro, Enterprise e Education), no Windows 10 Mobile ou em ambos.

<table border="1">
    <tr>
        <th>Categoria</th>
        <th>Página de configurações</th>
        <th>SKUs compatíveis</th>
        <th>URI</th>
    </tr>
    <tr>
        <td>Home page</td>
        <td>Página de aterrissagem para configurações</td>
        <td>Ambos</td>
        <td>ms-settings:</td>
    </tr>
    <tr>
        <td rowspan="10">Sistema</td>
        <td>Tela</td>
        <td>Ambos</td>
        <td>ms-settings:screenrotation</td>
    </tr>
    <tr>
        <td>Notificações e ações</td>
        <td>Ambos</td>
        <td>ms-settings:notifications</td>
    </tr>
    <tr>
        <td>Telefone</td>
        <td>Somente celular</td>
        <td>ms-settings:phone</td>
    </tr>
    <tr>
        <td>Mensagens</td>
        <td>Somente celular</td>
        <td>ms-settings:messaging</td>
    </tr>
    <tr>
        <td>Economia de Bateria</td>
        <td>Ambos<br>Disponível somente em dispositivos que possuem bateria, como um tablet</td>
        <td>ms-settings:batterysaver</td>
    </tr>
    <tr>
        <td>Uso da bateria</td>
        <td>Ambos<br>Disponível somente em dispositivos que possuem bateria, como um tablet</td>
        <td>ms-settings:batterysaver-usagedetails</td>
    </tr>
    <tr>
        <td>Ligar/Desligar e suspensão</td>
        <td>Somente desktop</td>
        <td>ms-settings:powersleep</td>
    </tr>
    <tr>
        <td>Sobre</td>
        <td>Ambos</td>
        <td>ms-settings:about</td>
    </tr>
    <tr>
        <td>Criptografia</td>
        <td>Ambos</td>
        <td>ms-settings:deviceencryption</td>
    </tr>
    <tr>
        <td>Mapas offline</td>
        <td>Ambos</td>
        <td>ms-settings:maps</td>
    </tr>
    <tr>
        <td rowspan="4">Dispositivos</td>
        <td>Câmera padrão</td>
        <td>Somente celular</td>
        <td>ms-settings:camera</td>
    </tr>
    <tr>
        <td>Bluetooth</td>
        <td>Somente desktop</td>
        <td>ms-settings:bluetooth</td>
    </tr>
    <tr>
        <td>Dispositivos conectados</td>
        <td>Somente desktop</td>
        <td>ms-settings:connecteddevices</td>
    </tr>
    <tr>
        <td>Mouse e touchpad</td>
        <td>Ambos<br>Configurações do touchpad disponíveis somente em dispositivos que possuem touchpad</td>
        <td>ms-settings:mousetouchpad</td>
    </tr>
    <tr>
        <td rowspan="3">Rede e sem fio</td>
        <td>NFC</td>
        <td>Ambos</td>
        <td>ms-settings:nfctransactions</td>
    </tr>
    <tr>
        <td>Wi-Fi</td>
        <td>Ambos</td>
        <td>ms-settings:network-wifi</td>
    </tr>
    <tr>
        <td>Modo avião</td>
        <td>Ambos</td>
        <td>ms-settings:network-airplanemode</td>
    </tr>
    <tr>
        <td rowspan="5">Rede e Internet</td>
        <td>Uso de dados</td>
        <td>Ambos</td>
        <td>ms-settings:datausage</td>
    </tr>
    <tr>
        <td>Rede Celular e SIM</td>
        <td>Ambos</td>
        <td>ms-settings:network-cellular</td>
    </tr>
    <tr>
        <td>Hotspot móvel</td>
        <td>Ambos</td>
        <td>ms-settings:network-mobilehotspot</td>
    </tr>
    <tr>
        <td>Proxy</td>
        <td>Somente desktop</td>
        <td>ms-settings:network-proxy</td>
    </tr>
    <tr>
        <td>Status</td>
        <td>Somente desktop</td>
        <td>ms-settings:network-status</td>
    </tr>
    <tr>
        <td rowspan="5">Personalização</td>
        <td>Personalização (categoria)</td>
        <td>Ambos</td>
        <td>ms-settings:personalization</td>
    </tr>
    <tr>
        <td>Em segundo plano</td>
        <td>Somente desktop</td>
        <td>ms-settings:personalization-background</td>
    </tr>
    <tr>
        <td>Cores</td>
        <td>Ambos</td>
        <td>ms-settings:personalization-colors</td>
    </tr>
    <tr>
        <td>Sons</td>
        <td>Somente celular </td>
        <td>ms-settings:sounds</td>
    </tr>
    <tr>
        <td>Tela de bloqueio</td>
        <td>Ambos</td>
        <td>ms-settings:lockscreen</td>
    </tr>
    <tr>
        <td rowspan="7">Contas</td>
        <td>Acessar trabalho ou escola</td>
        <td>Ambos</td>
        <td>ms-settings:workplace</td>
    </tr>
    <tr>
        <td>Contas de email e aplicativo</td>
        <td>Ambos</td>
        <td>ms-settings:emailandaccounts</td>
    </tr>
    <tr>
        <td>Família e outras pessoas</td>
        <td>Ambos</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>Opções de entrada</td>
        <td>Ambos</td>
        <td>ms-settings:signinoptions</td>
    </tr>
    <tr>
        <td>Sincronizar suas configurações</td>
        <td>Ambos</td>
        <td>ms-settings:sync</td>
    </tr>
    <tr>
        <td>Outras pessoas</td>
        <td>Ambos</td>
        <td>ms-settings:otherusers</td>
    </tr>
    <tr>
        <td>Suas informações</td>
        <td>Ambos</td>
        <td>ms-settings:yourinfo</td>
    </tr>
    <tr>
        <td rowspan="2">Hora e idioma</td>
        <td>Data e hora</td>
        <td>Ambos</td>
        <td>ms-settings:dateandtime</td>
    </tr>
    <tr>
        <td>Região e idioma</td>
        <td>Somente desktop</td>
        <td>ms-settings:regionlanguage</td>
    </tr>
    <tr>
        <td rowspan="7">Facilidade de acesso</td>
        <td>Narrador</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-narrator</td>
    </tr>
    <tr>
        <td>Lupa</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-magnifier</td>
    </tr>
    <tr>
        <td>Alto contraste </td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-highcontrast</td>
    </tr>
    <tr>
        <td>Legenda oculta</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-closedcaptioning</td>
    </tr>
    <tr>
        <td>Teclado</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-keyboard</td>
    </tr>
    <tr>
        <td>Mouse</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-mouse</td>
    </tr>
    <tr>
        <td>Outras opções</td>
        <td>Ambos</td>
        <td>ms-settings:easeofaccess-otheroptions</td>
    </tr>
    <tr>
        <td rowspan="15">Privacidade</td>
        <td>Localização</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-location</td>
    </tr>
    <tr>
        <td>Câmera</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-webcam</td>
    </tr>
    <tr>
        <td>Microfone</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-microphone</td>
    </tr>
    <tr>
        <td>Movimento</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-motion</td>
    </tr>
    <tr>
        <td>Controle por voz, escrita à tinta e digitação</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-speechtyping</td>
    </tr>
    <tr>
        <td>Informações da conta</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-accountinfo</td>
    </tr>
    <tr>
        <td>Contatos</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-contacts</td>
    </tr>
    <tr>
        <td>Calendário</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-calendar</td>
    </tr>
    <tr>
        <td>Histórico de chamadas</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-callhistory</td>
    </tr>
    <tr>
        <td>Email</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-email</td>
    </tr>
    <tr>
        <td>Mensagens</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-messaging</td>
    </tr>
    <tr>
        <td>Rádios</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-radios</td>
    </tr>
    <tr>
        <td>Aplicativos em segundo plano</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-backgroundapps</td>
    </tr>
    <tr>
        <td>Outros dispositivos</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-customdevices</td>
    </tr>
    <tr>
        <td>Comentários e diagnóstico</td>
        <td>Ambos</td>
        <td>ms-settings:privacy-feedback</td>
    </tr>
    <tr>
        <td>Atualização e segurança</td>
        <td>Para desenvolvedores</td>
        <td>Ambos</td>
        <td>ms-settings:developers</td>
    </tr>
</table><br/>



<!--HONumber=Dec16_HO1-->


