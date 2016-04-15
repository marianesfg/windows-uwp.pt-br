---
ms.assetid: 25B18BA5-E584-4537-9F19-BB2C8C52DFE1
title: Declarações de funcionalidades do aplicativo
description: As funcionalidades devem ser declaradas no manifesto do pacote do aplicativo da Plataforma Universal do Windows (UWP) para acessar determinadas APIs ou recursos, como imagens, música ou dispositivos como a câmera ou o microfone.
---
# Declarações de funcionalidade do aplicativo

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

As funcionalidades devem ser declaradas no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211474) do aplicativo da Plataforma Universal do Windows (UWP) para acessar determinadas APIs ou recursos, como imagens, música ou dispositivos como a câmera ou o microfone.

O acesso a APIs ou recursos específicos é solicitado por meio da declaração das funcionalidades no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211474) no aplicativo. Você pode declarar funcionalidades gerais usando o [Designer de Manifesto](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx) no Microsoft Visual Studio, ou você pode adicioná-las manualmente. Para obter mais informações, consulte [Como especificar funcionalidades em um manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/BR211477). É importante saber que, quando os clientes compram seu aplicativo na Loja, eles são notificados sobre todas as funcionalidades declaradas pelo aplicativo. Evite declarar funcionalidades de que seu aplicativo não precisa.

Algumas funcionalidades dão aos aplicativos acesso a um *recurso confidencial*. Esses recursos são considerados confidenciais porque podem acessar dados pessoais do usuário ou acarretar custos para o usuário. Configurações de privacidade, gerenciadas pelo aplicativo Configurações, permitem que o usuário controle dinamicamente o acesso a recursos confidenciais. Assim, é importante que seu aplicativo não suponha que um recurso confidencial sempre está disponível. Para obter mais informações sobre como acessar recursos confidenciais, consulte [Diretrizes para aplicativos com reconhecimento de privacidade](https://msdn.microsoft.com/library/windows/apps/Hh768223). Funcionalidades que oferecem aplicativos com acesso a um *recurso confidencial* são indicadas por um asterisco (\*) ao lado do cenário de recurso.

Este artigo examina quatro categorias de recursos descritos abaixo.

-   As funcionalidades de uso geral que se aplicam à maioria dos cenários de aplicativos.

-   As funcionalidades do dispositivo que permitem que seu aplicativo acesse dispositivos periféricos e internos.

-   Funcionalidades de uso especiais que requerem uma conta empresarial especial para serem enviadas à Store para uso. Para saber mais sobre contas empresariais, consulte [Tipos de conta, locais e taxas](https://msdn.microsoft.com/library/windows/apps/JJ863494).

-   Recursos restritos que só estão disponíveis para a Microsoft e seus parceiros.

## Funcionalidades de uso geral

As funcionalidades de uso geral se aplicam à maioria dos cenários de aplicativo mais comuns.

<table>
        <thead>
            <tr>
                <th>Cenário da funcionalidade</th>
                <th>Uso da funcionalidade</th>
            </tr>
        </thead>
        <tbody>
            <tr>
                <td>**Música***</td>
                <td>
                    The **musicLibrary** capability provides programmatic access to the user's Music, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in jukebox apps that make use of the entire Music library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **musicLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **musicLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="musicLibrary"/&gt;
&lt;/Funcionalidades&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Imagens***</td>
                <td>
                    A funcionalidade **picturesLibrary** oferece acesso programático às Imagens do usuário, permitindo que o aplicativo enumere e acesse todos os arquivos na biblioteca sem a interação do usuário. Tipicamente, esta funcionalidade é usada em aplicativos de foto que utilizam toda a biblioteca Imagens.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **picturesLibrary** capability only when the scenarios for your app require programmatic access and can't be realized them by using the **file picker**.

                    The **picturesLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="picturesLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Vídeos***</td>
                <td>
                    The **videosLibrary** capability provides programmatic access to the user's Videos, allowing the app to enumerate and access all files in the library without user interaction. This capability is typically used in movie-playback apps that make use of the entire Videos library.

                    The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app. Declare the **videosLibrary** capability only when the scenarios for your app require programmatic access and can't be realized by using the **file picker**.

                    The **videosLibrary** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="videosLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Armazenamento removível**</td>
                <td>
                    The **removableStorage** capability provides programmatic access to files on removable storage, like USB keys and external hard drives, filtered to the file-type associations declared in the package manifest. For example, if a document-reader app declares a .doc file-type association, it can open .doc files on the removable storage device, but not other types of files. Be careful when you declare this capability, because users may include a variety of info in their removable storage devices, and will expect your app to provide a valid justification for programmatic access to the removable storage for all files of the declared type.

                    Users will expect your app to handle any file associations that you declare. So don't declare file associations that your app cannot handle responsibly. The [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847) provides a robust UI mechanism that lets users open files for use with an app.

                    Declare the **removableStorage** capability only when the scenarios for your app require programmatic access and can't be realized by using the [**file picker**](https://msdn.microsoft.com/library/windows/apps/BR207847).

                    The **removableStorage** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="removableStorage"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Internet e redes públicas***</td>
                <td>
                    There are two capabilities that provide different levels of access to the Internet and public networks.

                    The **internetClient** capability indicates that apps can receive incoming data from the Internet. Cannot act as a server. No local network access.

                    The **internetClientServer** capability indicates that apps can receive incoming data from the Internet. Can act as a server. No local network access.

                    Most apps that have a web service component will use **internetClient**. Apps that enable peer-to-peer (P2P) scenarios where the app needs to listen for incoming network connections should use **internetClientServer**. The **internetClientServer** capability includes the access that the **internetClient** capability provides, so you don't need to specify **internetClient** when you specify **internetClientServer**.
                </td>
            </tr>
            <tr>
                <td>**Homes and work networks***</td>
                <td>
                    The **privateNetworkClientServer** capability provides inbound and outbound access to home and work networks through the firewall. This capability is typically used for games that communicate across the local area network (LAN), and for apps that share data across a variety of local devices. If your app specifies **musicLibrary**, **picturesLibrary**, or **videosLibrary**, you don't need to use this capability to access the corresponding library in a Home Group. On Windows, this capability does not provide access to the Internet.
                </td>
            </tr>
            <tr>
                <td>**Appointments**</td>
                <td>
                    The **appointments** capability provides access to the user’s appointment store. This capability allows read access to appointments obtained from the synced network accounts and to other apps that write to the appointment store. With this capability, your app can create new calendars and write appointments to calendars that it creates.

                    The **appointments** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="appointments"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Contatos***</td>
                <td>
                    The **contacts** capability provides access to the aggregated view of the contacts from various contacts stores. This capability gives the app limited access (network permitting rules apply) to contacts that were synced from various networks and the local contact store.

                    The **contacts** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="contacts"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Geração de código**</td>
                <td>
                    The **codeGeneration** capability allows apps to access the following functions which provide JIT capabilities to apps.

                    - [**VirtualProtectFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169846)
                    - [**CreateFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994453)
                    - [**OpenFileMappingFromApp**](https://msdn.microsoft.com/library/windows/desktop/Mt169844)
                    - [**MapViewOfFileFromApp**](https://msdn.microsoft.com/library/windows/desktop/Hh994454)
                </td>
            </tr>
            <tr>
                <td>**AllJoyn**</td>
                <td>
                    The **allJoyn** capability allows AllJoyn-enabled apps and devices on a network to discover and interact with each other.

                    All apps that access APIs in the [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) namespace must use this capability.
                </td>
            </tr>
            <tr>
                <td>**Phone calls**</td>
                <td>
                    The **phoneCall** capability allows apps to access all of the phone lines on the device and perform the following functions.

                    - Place a call on the phone line and show the system dialer without prompting the user.
                    - Access line-related metadata.
                    - Access line-related triggers.
                    - Allows the user-selected spam filter app to set and check block list and call origin information.

                    The **phoneCall** capability must include the **uap** namespace when you declare it in your app's package manifest as shown below.
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
<pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="phoneCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                    The **phoneCallHistoryPublic** capability allows apps to read cellular and some VOIP call history information on the device. This capability also allows the app to write VOIP call history entries. This capability is required to access all members of the [**PhoneCallHistoryStore**](https://msdn.microsoft.com/library/windows/apps/Dn705931) class.
                </td>
            </tr>
            <tr>
                <td>**Pasta Chamadas Gravadas***</td>
                <td>
                    <p>A funcionalidade do dispositivo **recordedCallsFolder** permite que aplicativos acessem a pasta de chamadas gravadas.</p>
                    <p>A funcionalidade **recordedCallsFolder** deve incluir o namespace **mobile** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;mobile:Capability Name="recordedCallsFolder"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Informações sobre a conta do usuário***</td>
                <td>
                    <p>A funcionalidade **userAccountInformation** dá aos aplicativos a capacidade de acessar o nome e a imagem do usuário.</p>
                    <p>Essa funcionalidade é necessária para acessar algumas APIs no namespace Windows.System.User.</p>
                    <p>A funcionalidade **userAccountInformation** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="userAccountInformation"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Chamada VOIP**</td>
                <td>
                    <p>A funcionalidade **voipCall** permite que os aplicativos acessam as APIS de chamadas VOIP no namespace [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266).</p>
                    <p>A funcionalidade **voipCall** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="voipCall"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Objetos 3D**</td>
                <td>
                    <p>A funcionalidade **objects3D** permite que os aplicativos tenham acesso programático a arquivos de objetos 3D. Em geral, essa funcionalidade é usada em jogos e aplicativos 3D que precisam acessar toda a biblioteca de objetos 3D.</p>
                    <p>Essa funcionalidade é obrigatória para acessar a pasta que contém os objetos 3D usando APIs no namespace [**Windows.Storage**](https://msdn.microsoft.com/library/windows/apps/BR227346).</p>
                    <p>A funcionalidade **objects3D** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="objects3d"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Ler mensagens bloqueadas***</td>
                <td>
                    <p>A funcionalidade **blockedChatMessages** permite que os aplicativos leiam mensagens de SMS e MMS bloqueadas pelo aplicativo Filtro de Spam.</p>
                    <p>Essa funcionalidade é obrigatória para acessar as mensagens bloqueadas usando APIs no namespace [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p>
                    <p>A funcionalidade **blockedChatMessages** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="blockedChatMessages"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Acesso a mensagens de chat**</td>
                <td>
                    <p>A funcionalidade **chat** permite que os aplicativos leiam e excluam mensagens de texto. Essa funcionalidade também permite que os aplicativos armazenem mensagens de chat no armazenamento de dados do sistema.</p>
                    <p>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p>
                    <p>A funcionalidade **chat** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="chat"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Hardware de barramento de baixo nível IoT**</td>
                <td>
                    <p>A funcionalidade **lowLevelDevices** permite que os aplicativos que são executados em dispositivos IoT acessem o hardware de barramento de baixo nível, como GPIO, I2C, SPI, ADC e PWM.</p>
                    <p>Essa funcionalidade é necessária para acessar algumas APIs nos namespaces [**Windows.Devices.Spi**](https://msdn.microsoft.com/library/windows/apps/Dn708178).</p>
                    <p>A funcionalidade **lowLevelDevices** deve incluir o namespace **iot** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="lowLevelDevices"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
            <tr>
                <td>**Administração de sistema IoT**</td>
                <td>
                    <p>A funcionalidade **systemManagement** permite que os aplicativos tenham privilégios de administração de sistema básicos, como desligar ou reiniciar, localidade e fuso horário.</p>
                    <p>Essa funcionalidade é necessária para acessar algumas APIs no namespace [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/BR241814).</p>
                    <p>A funcionalidade **systemManagement** deve incluir o namespace **iot** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
                    <table>
                        <colgroup>
                            <col width="100%" />
                        </colgroup>
                        <thead>
                            <tr>
                                <th>XML</th>
                            </tr>
                        </thead>
                        <tbody>
                            <tr>
                                <td>
                                    <pre><code>&lt;Capabilities&gt;
    &lt;iot:Capability Name="systemManagement"/&gt;
&lt;/Capabilities&gt;</code></pre>
                                </td>
                            </tr>
                        </tbody>
                    </table>
                </td>
            </tr>
        </tbody>
</table>

 
## Funcionalidades de dispositivo

As funcionalidades de dispositivo permitem que o aplicativo acesse dispositivos periféricos e internos. As funcionalidades de dispositivo são especificadas usando o elemento **DeviceCapability** no manifesto do pacote do aplicativo. Esse elemento pode exigir elementos filho adicionais e algumas funcionalidades de dispositivo precisam ser acrescentadas manualmente ao manifesto do pacote. Para obter mais informações, consulte [How to specify device capabilities in a package manifest](https://msdn.microsoft.com/library/windows/apps/Dn263092) e [**DeviceCapability Schema reference**](https://msdn.microsoft.com/library/windows/apps/BR211430).

| Cenário da funcionalidade | Uso da funcionalidade |
|---------------------|------------------|
| **Localização**\* | A funcionalidade **localização** dá acesso a funcionalidade de localização que é recuperada de hardware dedicado, como um sensor de GPS no computador ou é derivado de informações de rede disponíveis. Os aplicativos devem tratar o caso no qual o usuário desabilitou serviços de localização no botão **Configurações**. |
| **Microfone** | A funcionalidade **microfone** dá acesso ao feed de áudio do microfone, o qual permite que o aplicativo registre o áudio de microfones conectados. Os aplicativos devem tratar o caso no qual o usuário desabilitou o microfone no botão **Configurações**. |
| **Proximidade** | A funcionalidade **proximidade** permite que vários dispositivos próximos se comuniquem entre eles. Essa funcionalidade costuma ser usada em jogos casuais de vários jogadores e em aplicativos que trocam informações. Os dispositivos tentam usar a tecnologia de comunicação que oferece a melhor conexão possível, incluindo Bluetooth, Wi-Fi e a Internet. Essa funcionalidade é usada para iniciar a comunicação entre os dispositivos. |
| **Webcam** | A funcionalidade **webcam** dá acesso ao feed de vídeo de uma câmera interna ou uma webcam externa, o que permite que o aplicativo capture fotos e vídeos. No Windows, os aplicativos devem tratar o caso no qual o usuário desabilitou a câmera no botão **Configurações**.<br/>A funcionalidade **webcam** concede acesso somente ao stream de vídeo. Para conceder acesso ao stream de áudio também, a funcionalidade **microphone** deve ser adicionada. |
| **USB** | A funcionalidade do dispositivo **usb** habilita o acesso às APIs em [Updating the app manifest package for a USB device](http://go.microsoft.com/fwlink/p/?LinkId=302259). |
| **Dispositivos de interface humana (HID)** | A funcionalidade do dispositivo **humaninterfacedevice** habilita o acesso às APIs em [How to specify device capabilities for HID](https://msdn.microsoft.com/library/windows/apps/Dn263091). |
| **Ponto de Serviço (POS)** | A funcionalidade do dispositivo **pointOfService** habilita o acesso a APIs no namespace [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071). Esse namespace permite que o aplicativo acesse scanners de código de barras de Ponto de Serviço (POS) e leitores de tarja magnética. O namespace oferece uma interface independente de fornecedor para acessar dispositivos de POS de diversos fabricantes a partir de um aplicativo da Windows Store. |
| **Bluetooth** | A funcionalidade do dispositivo **bluetooth** permite que os aplicativos se comuniquem com dispositivos bluetooth já emparelhados usando os protocolos GATT (Atributo Genérico) ou RFCOMM Taxa Básica Clássica).<br/>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.Devices.Bluetooth**](https://msdn.microsoft.com/library/windows/apps/Dn263413). |
| **Rede Wi-Fi** | A funcionalidade do dispositivo **wiFiControl** permite que os aplicativos digitalizem e se conectem a redes Wi-Fi.<br/>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.Devices.WiFi**](https://msdn.microsoft.com/library/windows/apps/Dn975224). |
| **Estado do rádio** | A funcionalidade do dispositivo **radios** permite que os aplicativos ativem/desativem os rádios Wi-Fi e Bluetooth.<br/>Essa funcionalidade é necessária para usar as APIs no namespace [**Windows.Devices.Radios**](https://msdn.microsoft.com/library/windows/apps/Dn996447).  |
| **Disco óptico** | A funcionalidade do dispositivo **optical** permite que os aplicativos acessem funções em unidades de disco óptico, como CD, DVD e Blu-ray.<br/>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.Devices.Custom**](https://msdn.microsoft.com/library/windows/apps/Dn263667). |
| **Atividade de movimento** | A funcionalidade do dispositivo **activity** permite que os aplicativos detectem o movimento atual do dispositivo.<br/>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.Devices.Sensors**](https://msdn.microsoft.com/library/windows/apps/BR206408). |

## Funcionalidades especiais e restritas

**Importante**  
As funcionalidades especiais e restritas foram desenvolvidas para cenários muito específicos. O uso dessas funcionalidades é altamente restrito e sujeito a políticas e análises adicionais de carregamento da Loja.

Há casos em que essas funcionalidades são necessárias e apropriadas, como em transações bancárias com autenticação de dois fatores, em que os usuários fornecem um cartão inteligente com um certificado digital que confirma suas identidades. Outros aplicativos podem ser projetados principalmente para clientes corporativos e talvez precisem de acesso a recursos corporativos que não podem ser acessados sem as credenciais de domínio do usuário.

Os aplicativos que utilizam as funcionalidades de uso especiais requerem uma conta empresarial para enviá-los à Store. Por outro lado, recursos restritos não requerem uma conta empresarial especial para a Loja. Eles não estão disponíveis para os desenvolvedores usarem. Recursos restritos estão disponíveis apenas aos aplicativos desenvolvidos pela Microsoft e seus parceiros. Para obter mais informações sobre contas empresariais, consulte [Account types, locations, and fees](https://msdn.microsoft.com/library/windows/apps/JJ863494).

Todas as funcionalidades restritas devem incluir o namespace **rescap** quando são declaradas no manifesto do pacote do aplicativo de maneira diferente do que outras funcionalidades. O exemplo a seguir mostra como declarar a funcionalidade **appCaptureSettings**.

```xml
<Capabilities>
    <rescap:Capability Name="appCaptureSettings"/>
</Capabilities>
```

Você também deve adicionar a declaração do namespace **xmlns:rescap** na parte superior do arquivo Package.appxmanifest file, como mostrado a seguir.

```xml
<Package
    xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
    xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
    xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
    xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
    IgnorableNamespaces="uap mp wincap rescap">
```

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Cenário da funcionalidade</th>
<th align="left">Uso da funcionalidade</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**Empresas**</td>
<td align="left"><p>As credenciais de domínio do Windows habilitam um usuário a fazer logon em recursos remotos usando suas credenciais e a agir como se um usuário tivesse fornecido os respectivos nome de usuário e senha. Geralmente, a funcionalidade especial **enterpriseAuthentication** é usada em aplicativos de linha de negócios que se conectam a servidores em uma empresa.</p>
<p>Você não precisa dessa funcionalidade para comunicação genérica pela Internet.</p>

<p>A funcionalidade especial **enterpriseAuthentication** tem a finalidade de dar suporte a aplicativos de linha de negócios comuns. Não a declare em aplicativos que não precisam de acesso a recursos corporativos. O [**seletor de arquivos**](https://msdn.microsoft.com/library/windows/apps/BR207847) oferece um mecanismo de interface do usuário robusto que permite que os usuários abram arquivos em um compartilhamento de rede network para uso com um aplicativo. Declare a funcionalidade especial **enterpriseAuthentication** somente quando os cenários do aplicativo exigirem acesso programático e você não puder realizar isso usando o **seletor de arquivos**.</p>
<p>A funcionalidade **enterpriseAuthentication** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="enterpriseAuthentication"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div>
<p>A funcionalidade **enterpriseDataPolicy** permite que os aplicativos definam e usem políticas específicas da empresa no dispositivo. Essa funcionalidade é necessária para usar todos os membros das classes a seguir.</p>
<ul>
<li>[**FileProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn705151)</li>
<li>[**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/Dn706017)</li>
<li>[**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/Dn705170)</li>
</ul></td>
</tr>
<tr class="even">
<td align="left">**Certificados de usuário compartilhados**</td>
<td align="left"><p>A funcionalidade especial **sharedUserCertificates** permite que um aplicativo adicione e acesse certificados baseados em software e hardware no repositório Usuário Compartilhado, como certificados armazenados em um cartão inteligente. Essa funcionalidade geralmente é usada em aplicativos corporativos ou financeiros que exigem um cartão inteligente para autenticação.</p>
<p>A funcionalidade **sharedUserCertificates** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="sharedUserCertificates"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="odd">
<td align="left">**Documentos***</td>
<td align="left"><p>A funcionalidade especial **documentsLibrary** dá acesso programático aos Documentos do usuário, filtrados pelas associações de tipo de arquivo declaradas no manifesto do pacote, para dar suporte ao acesso offline ao OneDrive. Por exemplo, se um aplicativo leitor de DOC declarou uma associação de tipo de arquivo .doc, ele pode abrir arquivos .doc em Documentos, mas nenhum outro tipo de arquivo.</p>
<p>Os aplicativos que declaram a funcionalidade especial **documentsLibrary** não podem acessar Documentos em computadores do Grupo Doméstico. O [file picker](https://msdn.microsoft.com/library/windows/apps/Hh465174) oferece um mecanismo de interface do usuário robusto que permite que os usuários abram arquivos para uso em um aplicativo. Declare a funcionalidade especial **documentsLibrary** somente quando não for possível usar o seletor de arquivos.</p>
<p>Para usar a funcionalidade especial **documentsLibrary**, um aplicativo deve:</p>
<ul>
<li>Facilitar o acesso offline entre plataformas a conteúdo do OneDrive específico usando IDs de recurso ou URLs do OneDrive válidas</li>
<li>Salvar automaticamente os arquivos abertos no OneDrive do usuário enquanto estiver offline</li>
</ul>
<p>Os aplicativos que usam a funcionalidade especial **documentsLibrary** com essas duas finalidades também podem, opcionalmente, usar a funcionalidade para abrir conteúdo incorporado dentro de outro documento. São aceitos somente os usos acima da funcionalidade especial **documentsLibrary**.</p>
<ul>
<li><p>O aplicativo não pode acessar a biblioteca Documentos no armazenamento interno do telefone. Se outro aplicativo criar uma pasta Documentos no cartão SD opcional, contudo, seu aplicativo poderá ver essa pasta.</p></li>
</ul>
<p>A funcionalidade **documentsLibrary** deve incluir o namespace **uap** quando for declarada no manifesto do pacote do aplicativo, como mostrado a seguir.</p>
<div class="code">
<span codelanguage="XML"></span>
<table>
<colgroup>
<col width="100%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">XML</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><pre><code>&lt;Capabilities&gt;
    &lt;uap:Capability Name="documentsLibrary"/&gt;
&lt;/Capabilities&gt;</code></pre></td>
</tr>
</tbody>
</table>
</div></td>
</tr>
<tr class="even">
<td align="left">**Configurações de DVR de jogos**</td>
<td align="left"><p>A funcionalidade restrita **appCaptureSettings** permite que os aplicativos controlem as configurações do usuário para o DVR de Jogos.</p>
<p>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.Media.Capture**](https://msdn.microsoft.com/library/windows/apps/BR226738).</p></td>
</tr>
<tr class="odd">
<td align="left">**Rede celular**</td>
<td align="left"><p>A funcionalidade restrita **cellularDeviceControl** permite que os aplicativos tenham controle sobre o dispositivo celular.</p>
<p>A funcionalidade **cellularDeviceIdentity** permite que os aplicativos acessem dados de identificação da rede celular.</p>
<p>A funcionalidade **cellularMessaging** permite que os aplicativos usem SMS e RCS.</p>
<p>Essas funcionalidades são necessárias para usar algumas APIs no namespace [**Windows.Devices.Sms**](https://msdn.microsoft.com/library/windows/apps/BR206567).</p>
<p>A partir do Windows 10, os aplicativos que chamam [**AppIDList**](https://msdn.microsoft.com/library/windows/apps/Dn393996)).</p></td>
</tr>
<tr class="even">
<td align="left">**Desbloqueio de dispositivo**</td>
<td align="left"><p>A funcionalidade restrita **deviceUnlock** permite que os aplicativos desbloqueiem um dispositivo para cenários de sideload corporativo e de desenvolvedor.</p></td>
</tr>
<tr class="odd">
<td align="left">**Blocos de SIM duplo**</td>
<td align="left"><p>A funcionalidade restrita **dualSimTiles** permite que os aplicativos criem uma entrada adicional na lista de aplicativos em dispositivos com vários SIMs.</p>
<p>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.UI.StartScreen**](https://msdn.microsoft.com/library/windows/apps/BR242235).</p></td>
</tr>
<tr class="even">
<td align="left">**Armazenamento compartilhado corporativo**</td>
<td align="left"><p>A funcionalidade restrita **enterpriseDeviceLockdown** permite que os aplicativos usem a API de bloqueio do dispositivo e acessem as pastas de armazenamento compartilhado corporativo.</p></td>
</tr>
<tr class="odd">
<td align="left">**Injeção de entrada do sistema**</td>
<td align="left"><p>A funcionalidade restrita **inputInjection** permite que os aplicativos insiram várias formas de entrada, como HID, toque, caneta, teclado ou mouse, no sistema de forma programática. Geralmente, essa funcionalidade é usada para aplicativos de colaboração que podem assumir o controle do sistema.</p>
<div class="alert">
**Observação**  Em um computador, a injeção de entrada de um aplicativo que tenha essa funcionalidade será recebida somente por processos no mesmo contêiner de aplicativo.
</div>
</td>
</tr>
<tr class="even">
<td align="left">**Observar entrada***</td>
<td align="left"><p>A funcionalidade restrita **inputObservation** permite que os aplicativos observem várias formas de dados brutos, como HID, toque, caneta, teclado ou mouse, recebidas pelo sistema independentemente de seu destino final.</p></td>
</tr>
<tr class="odd">
<td align="left">**Suprimir entrada**</td>
<td align="left"><p>A funcionalidade restrita **inputSuppression** permite que os aplicativos impede que várias formas de dados brutos, como HID, toque, caneta, teclado ou mouse, sejam recebidas pelo sistema independentemente de seu destino final.</p></td>
</tr>
<tr class="even">
<td align="left">**Aplicativo VPN**</td>
<td align="left"><p>A funcionalidade restrita **networkingVpnProvider** permite que os aplicativos tenham acesso total aos recursos de VPN, inclusive a capacidade de gerenciar conexões e fornecer funcionalidade de plug-in de VPN.</p>
<p>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.Networking.Vpn**](https://msdn.microsoft.com/library/windows/apps/Dn434040).</p></td>
</tr>
<tr class="odd">
<td align="left">**Gerenciamento de outros aplicativos**</td>
<td align="left"><p>A funcionalidade restrita **packageManagement** permite que os aplicativos gerenciem diretamente outros aplicativos.</p>
<p>A funcionalidade do dispositivo **packageQuery** permite que os aplicativos coletem informações de outros aplicativos.</p>
<p>Essas funcionalidades são necessárias para acessar alguns métodos e propriedades na classe [**PackageManager**](https://msdn.microsoft.com/library/windows/apps/BR240960).</p></td>
</tr>
<tr class="even">
<td align="left">**Projeção da tela**</td>
<td align="left"><p>A funcionalidade restrita **screenDuplication** permite que os aplicativos projetem a tela em outro dispositivo.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace DirectX.</p></td>
</tr>
<tr class="odd">
<td align="left">**UPN**</td>
<td align="left"><p>A funcionalidade restrita **userPrincipalName** permite que os aplicativos modifiquem e acessem o cache de miniaturas das fotos.</p>
<p>Essa funcionalidade é necessária para chamar a função [**GetUserNameEx**](https://msdn.microsoft.com/library/windows/desktop/ms724435).</p></td>
</tr>
<tr class="even">
<td align="left">**Carteira**</td>
<td align="left"><p>A funcionalidade restrita **walletSystem** permite que os aplicativos tenham acesso completo aos cartões de carteira armazenados.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Wallet.System**](https://msdn.microsoft.com/library/windows/apps/Mt171610).</p></td>
</tr>
<tr class="odd">
<td align="left">**Histórico de localização**</td>
<td align="left"><p>A funcionalidade restrita **locationHistory** permite que os aplicativos acessem o histórico de localização do dispositivo.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.Devices.Geolocation**](https://msdn.microsoft.com/library/windows/apps/BR225603).</p></td>
</tr>
<tr class="even">
<td align="left">**Confirmação de fechamento de aplicativo**</td>
<td align="left"><p>A funcionalidade restrita **confirmAppClose** permite que os aplicativos fechem a si mesmos e suas próprias janelas, e atrasem o fechamento do aplicativo.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.UI.ViewManagement**](https://msdn.microsoft.com/library/windows/apps/BR242295).</p></td>
</tr>
<tr class="odd">
<td align="left">**Histórico de chamadas***</td>
<td align="left"><p>A funcionalidade restrita **phoneCallHistory** permite que os aplicativos leiam o histórico de chamadas e excluam entradas do histórico.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="even">
<td align="left">**Acesso a compromissos no nível do sistema**</td>
<td align="left"><p>A funcionalidade restrita **appointmentsSystem** permite que os aplicativos leiam e modifiquem todos os compromissos no calendário do usuário.</p>
<p>Essa funcionalidade é necessária para usar algumas APIs no namespace [**Windows.ApplicationModel.Appointment**](https://msdn.microsoft.com/library/windows/apps/Dn263359).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acesso de mensagem de chat do nível de sistema***</td>
<td align="left"><p>A funcionalidade restrita **chatSystem** permite que os aplicativos leiam e gravem todas as mensagens SMS e MMS.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="even">
<td align="left">**Acesso a contatos no nível do sistema**</td>
<td align="left"><p>A funcionalidade restrita **contactsSystem** permite que os aplicativos leiam informações de contato designadas como confidenciais ou restritas e modifiquem as informações de contato existentes.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acesso ao email***</td>
<td align="left"><p>A funcionalidade restrita **email** permite que os aplicativos leiam, façam triagem e enviem emails do usuário.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285).</p></td>
</tr>
<tr class="even">
<td align="left">**Acesso ao email no nível do sistema**</td>
<td align="left"><p>A funcionalidade restrita **emailSystem** permite que os aplicativos leiam, façam triagem e enviem emails confidenciais do usuário.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Email**](https://msdn.microsoft.com/library/windows/apps/Dn631285).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acesso ao histórico de chamadas no nível do sistema**</td>
<td align="left"><p>A funcionalidade restrita **phoneCallHistorySystem** permite que os aplicativos modifiquem totalmente o histórico de chamadas alterando as entradas existentes e criando novas.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266).</p></td>
</tr>
<tr class="even">
<td align="left">**Enviar mensagens de texto***</td>
<td align="left"><p>A funcionalidade restrita **smsSend** permite que os aplicativos enviem mensagens SMS e MMS.</p>
<p>Essa funcionalidade é necessária para usar APIs no namespace [**Windows.ApplicationModel.Chat**](https://msdn.microsoft.com/library/windows/apps/Dn642321).</p></td>
</tr>
<tr class="odd">
<td align="left">**Acesso a todos os dados do usuário no nível de sistema**</td>
<td align="left"><p>A funcionalidade restrita **userDataSystem** permite que os aplicativos acessem o armazenamento de dados do sistema de dados do usuário.</p></td>
</tr>
<tr class="even">
<td align="left">**Recursos de visualização da Loja**</td>
<td align="left"><p>A funcionalidade restrita **previewStore** permite que os aplicativos recuperem e comprem SKUs de produtos no aplicativo.</p>
<p>Essa funcionalidade é necessária para usar certas APIs no namespace [**Windows.ApplicationModel.Store.Preview**](https://msdn.microsoft.com/library/windows/apps/Mt185546).</p></td>
</tr>
<tr class="odd">
<td align="left">**Configurações de entrada pela primeira vez**</td>
<td align="left"><p>A funcionalidade restrita **firstSignInSettings** permite que os aplicativos acessem as configurações de usuário definidas quando o usuário se conectou pela primeira vez no dispositivo.</p></td>
</tr>
<tr class="even">
<td align="left">**Experiência de Equipe do Windows**</td>
<td align="left"><p>O recurso restrito **teamEditionExperience** permite que aplicativos acessem APIs internas que controlam muitos aspectos experimentais de uma sessão de equipe do Windows. Uma sessão de equipe do Windows provavelmente está em execução em um dispositivo de equipe como um Microsoft Surface Hub.</p></td>
</tr>
<tr class="odd">
<td align="left">**Desbloqueio remoto**</td>
<td align="left"><p>A funcionalidade restrita **remotePassportAuthentication** permite que aplicativos acessem as credenciais que podem ser usadas para desbloquear um computador remoto.</p></td>
</tr>
<tr class="even">
<td align="left">**Visualizar composição**</td>
<td align="left"><p>A funcionalidade restrita **previewUiComposition** permite que os aplicativos visualizem o namespace [**Windows.UI.Composition**](https://msdn.microsoft.com/library/windows/apps/Dn706878) de sua interface de usuário para que possam dar feedback sobre a API antes de concluída. Para obter mais informações, contate wincomposition@microsoft.com.</p></td>
</tr>
<tr class="odd">
<td align="left">**Bloqueio de avaliação segura**</td>
<td align="left"><p>A funcionalidade restrita **secureAssessment** permite que os aplicativos bloqueiem o Windows em um modo de aplicativo único para avaliações seguras.</p></td>
</tr>
<tr class="even">
<td align="left">**Provisionamento do gerenciador de conexões**</td>
<td align="left"><p>A funcionalidade restrita **networkConnectionManagerProvisioning** permite que os aplicativos definam as políticas que conectam o dispositivo usando interfaces WWAN e WLAN. Os aplicativos que usam essa funcionalidade são criados por operadoras de celular para controlar os dispositivos que se conectam com sua rede móvel.</p></td>
</tr>
<tr class="odd">
<td align="left">**Provisionamento de plano de dados**</td>
<td align="left"><p>O recurso restrito **networkDataPlanProvisioning** permite que os aplicativos coletem informações sobre os planos de dados no dispositivo e leiam o uso da rede. Aplicativos que usam essa funcionalidade são criados por operadoras móveis para integrar o uso de dados real de seus clientes na configuração de uso de dados do sistema operacional.</p></td>
</tr>
<tr class="even">
<td align="left">**Licenciamento de software**</td>
<td align="left"><p>O recurso restrito **slapiQueryLicenseValue** permite que os aplicativos consultem políticas de licenciamento de software.</p></td>
</tr>
<tr class="odd">
<td align="left">**Execução estendida**</td>
<td align="left"><p>O recurso restrito **extendedExecutionBackgroundAudio** permite que os aplicativos reproduzam áudio quando o aplicativo não está em primeiro plano.</p>
<p>O recurso restrito **extendedExecutionCritical** permite que os aplicativos comecem uma sessão de execução crítica estendida.</p>
<p>O recurso restrito **extendedExecutionUnconstrained** permite que os aplicativos comecem uma sessão de execução irrestrita estendida.</p></td>
</tr>
<tr class="even">
<td align="left">**Gerenciamento de dispositivos móveis**</td>
<td align="left"><p>O recurso restrito **deviceManagementDmAccount** permite que os aplicativos provisionem e configurem contas Mobile Operator Open Mobile Alliance - Device Management (MO OMA-DM).</p>
<p>O recurso restrito **deviceManagementFoundation** permite que os aplicativos tenham acesso básico à infraestrutura de provedor de serviço (CSP) de configuração de gerenciamento de dispositivos móveis (MDM) no dispositivo. Observe que outros recursos são necessários para acessar CSPs específicos.</p>
<p>O recurso restrito **deviceManagementWapSecurityPolicies** permite que os aplicativos configurem serviços baseados em protocolo de aplicativo sem fio (WAP), como MMs, indicação de serviço/carregamento de serviço (SI/SL) e Open Mobile Alliance - Client Provisioning (OMA-CP).</p>
<p>O recurso restrito **deviceManagementEmailAccount** permite que os aplicativos criados por operadoras de celular adicionem e gerenciem uma conta de email em dispositivos que eles provisionem aos usuários.</p></td>
</tr>
<tr class="odd">
<td align="left">**Controle de política de pacote**</td>
<td align="left"><p>O recurso restrito **packagePolicySystem** permite que os aplicativos tenham controle das políticas de sistema relacionadas a aplicativos que estão instalados no dispositivo.</p></td>
</tr>
<tr class="even">
<td align="left">**Lista de jogos**</td>
<td align="left"><p>O recurso restrito **gameList** permite que os aplicativos obtenham uma lista de jogos conhecidos instalados no sistema.</p></td>
</tr>
<tr class="odd">
<td align="left">**Acessório Xbox**</td>
<td align="left"><p>A funcionalidade restrita **xboxAccessoryManagement** permite que os aplicativos gerenciem diretamente dispositivos Xbox que estão em conformidade com a especificação de hardware do Xbox.</p></td>
</tr>
<tr class="even">
<td align="left">**Reconhecimento de fala para acessórios**</td>
<td align="left"><p>O recurso restrito **cortanaSpeechAccessory** permite que os aplicativos invoquem e passem comandos para o Cortana.</p></td>
</tr>
<tr class="odd">
<td align="left">**Gerenciamento de acessórios**</td>
<td align="left"><p>A funcionalidade restrita **accessoryManager** permite que os aplicativos se registrem como um aplicativo acessório e ativem notificações de aplicativo específicas de maneira que elas possam ser encaminhadas aos acessórios e exibidas para o usuário.</p></td>
</tr>
<tr class="even">
<td align="left">**Acesso a driver**</td>
<td align="left"><p>A funcionalidade restrita **interopServices** permite que os aplicativos interajam diretamente com drivers.</p></td>
</tr>
<tr class="odd">
<td align="left">**Observação em primeiro plano**</td>
<td align="left"><p>A funcionalidade restrita **inputForegroundObservation** permite que aplicativos em primeiro plano interceptem a entrada por teclado e ignorem todo o processamento de entradas de teclado que não seja de aplicativo. As combinações de SAS não podem ser interceptadas por essa funcionalidade. Essa funcionalidade é necessária para acessar membros da classe [**KeyboardDeliveryInterceptor**](https://msdn.microsoft.com/library/windows/apps/Mt608395).</p></td>
</tr>
<tr class="even">
<td align="left">**Aplicativos de parceiros OEM e MO**</td>
<td align="left"><p>A funcionalidade restrita **oemDeployment** permite que os aplicativos criados por parceiros da Microsoft instalem novos aplicativos e consultem os aplicativos instalados no momento no dispositivo.</p>
<p>A funcionalidade restrita **oemPublicDirectory** permite que os aplicativos criados por parceiros da Microsoft tenham acesso à pasta de aplicativos compartilhados.</p></td>
</tr>
<tr class="odd">
<td align="left">**Licenciamento de aplicativos**</td>
<td align="left"><p>A funcionalidade restrita **appLicensing** permite que os aplicativos sejam executados sem a necessidade de uma licença. Se você declarar essa funcionalidade no seu manifesto, não poderá enviar seu aplicativo para a loja. Todas as solicitações para acessar essa funcionalidade para envio à loja sempre serão negadas.</p></td>
</tr>
<tr class="even">
<td align="left">**Sistema de localização**</td>
<td align="left"><p>A funcionalidade restrita **locationSystem** permite que os aplicativos realizem determinadas configurações de localização privilegiadas, como definir o local padrão do dispositivo. Se você declarar essa funcionalidade no seu manifesto, não poderá enviar seu aplicativo para a loja. Todas as solicitações para acessar essa funcionalidade para envio à loja sempre serão negadas.</p></td>
</tr>
</tbody>
</table>

**Observação**  
Este artigo destina-se a desenvolvedores do Windows 10 que escrevem aplicativos UWP. Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

## Tópicos relacionados

* [Designer de manifesto](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/br230259.aspx)
* [Diretrizes para aplicativos com reconhecimento da privacidade](https://msdn.microsoft.com/library/windows/apps/Hh768223)
* [Como especificar funcionalidades em um manifesto de pacote](https://msdn.microsoft.com/library/windows/apps/BR211477)
* [Como especificar as funcionalidades do dispositivo em um manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/Dn263092)
 


<!--HONumber=Mar16_HO5-->


