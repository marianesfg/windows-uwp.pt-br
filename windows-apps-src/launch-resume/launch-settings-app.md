---
author: TylerMSFT
title: "Iniciar o aplicativo Configurações do Windows"
description: "Saiba como iniciar o aplicativo Configurações do Windows a partir de seu aplicativo. Este tópico descreve o esquema de URI ms-settings. Use esse esquema de URI para iniciar o aplicativo Configurações do Windows para páginas de configurações específicas."
ms.assetid: C84D4BEE-1FEE-4648-AD7D-8321EAC70290
ms.sourcegitcommit: 3cf9dd4ab83139a2b4b0f44a36c2e57a92900903
ms.openlocfilehash: e52a4245e8697a68bfc5c5605dc54e5ea510c662

---

# Iniciar o aplicativo Configurações do Windows


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Saiba como iniciar o aplicativo Configurações do Windows a partir de seu aplicativo. Este tópico descreve o esquema de URI **ms-settings:**. Use esse esquema de URI para iniciar o aplicativo Configurações do Windows para páginas de configurações específicas.

A inicialização do aplicativo Configurações é uma parte importante da escrita de um aplicativo com detecção de privacidade. Se seu aplicativo não pode acessar um recurso confidencial, é recomendável fornecer ao usuário um link conveniente para as configurações de privacidade desse recurso. Para obter mais informações, consulte [Diretrizes para aplicativos com detecção de privacidade](https://msdn.microsoft.com/library/windows/apps/hh768223).

## Como iniciar o aplicativo Configurações


Se as configurações de privacidade não permitem que seu aplicativo acesse um recurso confidencial, recomendamos fornecer um link conveniente para as configurações de privacidade no aplicativo **Configurações**. Isso tornará mais fácil para os usuários alterarem suas configurações.

Para iniciar diretamente o aplicativo **Configurações**, use o esquema de URI `ms-settings:`, como mostrado nos exemplos a seguir.

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

Como alternativa, seu aplicativo pode chamar o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar o aplicativo **Configurações** do código.

```cs
using Windows.System;
...
bool result = await Launcher.LaunchUriAsync(new Uri("ms-settings:privacy-webcam"));
```

Este exemplo mostra como iniciar a página de configurações de privacidade da câmera usando o URI `ms-settings:privacy-webcam`.

![configurações de privacidade da câmera.](images/privacyawarenesssettingsapp.png)

Para obter mais informações como URIs de inicialização, consulte [Iniciar o aplicativo padrão para um URI](launch-default-app.md).

## Referência de esquema de URI ms-settings:


Use os seguintes URIs para abrir várias páginas do aplicativo Configurações. A coluna SKUs compatíveis indica se a página de configurações existe no Windows 10 para edições desktop (Home, Pro, Enterprise e Education), no Windows 10 Mobile ou em ambos.

| Categoria           | Página de configurações                          | SKUs compatíveis | URI                                       |
|--------------------|----------------------------------------|----------------|-------------------------------------------|
| Página inicial          | Página de aterrissagem para configurações              | Ambos           | ms-settings:                              |
| Sistema             | Monitor                                | Ambos           | ms-settings:screenrotation                |
|                    | Notificações e ações                | Ambos           | ms-settings:notifications                 |
|                    | Telefone                                  | Somente celular    | ms-settings:phone                         |
|                    | Mensagens                              | Somente celular    | ms-settings:messaging                     |
|                    | Economia de Bateria                          | Móvel e desktop em dispositivos com bateria, como um tablet    | ms-settings:batterysaver                  |
|                    | Economia de bateria/configurações de economia de bateria | Móvel e desktop em dispositivos com bateria, como um tablet | ms-settings:batterysaver-settings         |
|                    | Economia de bateria/uso de bateria            | Móvel e desktop em dispositivos com bateria, como um tablet    | ms-settings:batterysaver-usagedetails     |
|                    | Ligar/Desligar e suspensão                          | Somente desktop   | ms-settings:powersleep                    |
|                    | Desktop: sobre                         | Ambos           | ms-settings:deviceencryption              |
|                    |                                        |                |                                           |
|                    | Celular: criptografia do dispositivo              |                |                                           |
|                    | Mapas offline                           | Ambos           | ms-settings:maps                          |
|                    | Sobre                                  | Ambos           | ms-settings:about                         |
| Dispositivos            | Câmera padrão                         | Somente celular    | ms-settings:camera                        |
|                    | Bluetooth                              | Somente desktop   | ms-settings:bluetooth                     |
|                    | Mouse e touchpad                       | Ambos           | ms-settings:mousetouchpad                 |
|                    | NFC                                    | Ambos           | ms-settings:nfctransactions               |
| Rede e sem fio | Wi-Fi                                  | Ambos           | ms-settings:network-wifi                  |
|                    | Modo avião                          | Ambos           | ms-settings:network-airplanemode          |
| Rede e Internet | Uso de dados                             | Ambos           | ms-settings:datausage                     |
|                    | Rede Celular e SIM                         | Ambos           | ms-settings:network-cellular              |
|                    | Hotspot móvel                         | Ambos           | ms-settings:network-mobilehotspot         |
|                    | Proxy                                  | Ambos           | ms-settings:network-proxy                 |
| Personalização    | Personalização (categoria)             | Ambos           | ms-settings:personalization               |
|                    | Em segundo plano                             | Somente desktop   | ms-settings:personalization-background    |
|                    | Cores                                 | Ambos           | ms-settings:personalization-colors        |
|                    | Sons                                 | Somente celular    | ms-settings:sounds                        |
|                    | Tela de bloqueio                            | Ambos           | ms-settings:lockscreen                    |
| Contas           | O email e as contas                | Ambos           | ms-settings:emailandaccounts              |
|                    | Acesso de trabalho                            | Ambos           | ms-settings:workplace                     |
|                    | Sincronize as configurações                     | Ambos           | ms-settings:sync                          |
| Hora e idioma  | Data e hora                            | Ambos           | ms-settings:dateandtime                   |
|                    | Região e idioma                      | Somente desktop   | ms-settings:regionlanguage                |
| Facilidade de acesso     | Narrador                               | Ambos           | ms-settings:easeofaccess-narrator         |
|                    | Lupa                              | Ambos           | ms-settings:easeofaccess-magnifier        |
|                    | Alto contraste                          | Ambos           | ms-settings:easeofaccess-highcontrast     |
|                    | Legenda oculta                        | Ambos           | ms-settings:easeofaccess-closedcaptioning |
|                    | Teclado                               | Ambos           | ms-settings:easeofaccess-keyboard         |
|                    | Mouse                                  | Ambos           | ms-settings:easeofaccess-mouse            |
|                    | Outras opções                          | Ambos           | ms-settings:easeofaccess-otheroptions     |
| Privacidade            | Localização                               | Ambos           | ms-settings:privacy-location              |
|                    | Câmera                                 | Ambos           | ms-settings:privacy-webcam                |
|                    | Microfone                             | Ambos           | ms-settings:privacy-microphone            |
|                    | Movimento                                 | Ambos           | ms-settings:privacy-motion                |
|                    | Controle por voz, escrita à tinta e digitação                | Ambos           | ms-settings:privacy-speechtyping          |
|                    | Informações da conta                           | Ambos           | ms-settings:privacy-accountinfo           |
|                    | Contatos                               | Ambos           | ms-settings:privacy-contacts              |
|                    | Calendário                               | Ambos           | ms-settings:privacy-calendar              |
|                    | Histórico de chamadas                           | Ambos           | ms-settings:privacy-callhistory           |
|                    | Email                                  | Ambos           | ms-settings:privacy-email                 |
|                    | Mensagens                              | Ambos           | ms-settings:privacy-messaging             |
|                    | Rádios                                 | Ambos           | ms-settings:privacy-radios                |
|                    | Aplicativos em segundo plano                        | Ambos           | ms-settings:privacy-backgroundapps        |
|                    | Outros dispositivos                          | Ambos           | ms-settings:privacy-customdevices         |
|                    | Comentários e diagnóstico                 | Ambos           | ms-settings:privacy-feedback              |
| Atualização e segurança  | Para desenvolvedores                         | Ambos           | ms-settings:developers                    |
 



<!--HONumber=Jun16_HO4-->


