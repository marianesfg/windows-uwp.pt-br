---
author: seksenov
title: "Aplicativos Web hospedados - Acessando os recursos da Plataforma Universal do Windows (UWP) e APIs de tempo de execução"
description: "Acessar recursos nativos da Plataforma Universal do Windows (UWP) e as APIs do Windows 10 Runtime, incluindo os comandos de voz da Cortona, Blocos Dinâmicos, ACURs para segurança, OpenID e OAuth e tudo em JavaScript remoto."
kw: Hosted Web Apps, Accessing Windows 10 features from remote JavaScript, Building a Win10 Web Application, Windows JavaScript Apps, Microsoft Web Apps, HTML5 app for PC, ACUR URI Rules for Windows App, Call Live Tiles with web app, Use Cortana with web app, Access Cortana from website, msapplication-cortanavcd
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: a7f7dccb9c7461e482bd43c8f370a2a7244eb735

---

# Acessar recursos da Plataforma Universal do Windows (UWP)

Seu aplicativo da web pode ter acesso completo à Plataforma Universal do Windows (UWP), ativar recursos nativos em dispositivos Windows, [beneficiar-se da segurança do Windows](#keep-your-app-secure-setting-application-content-uri-rules-acurs), [chamar APIs do Windows Runtime](#call-windows-runtime-apis) diretamente do script hospedado em um servidor, aproveitar a [integração com a Cortana](#integrate-cortana-voice-commands)e usar um [provedor de autenticação on-line](#web-authentication-broker). 
              [Também há suporte para aplicativos híbridos](#create-hybrid-apps-packaged-web-apps-vs-hosted-web-apps), pois você pode incluir código local para ser chamado do script hospedado e gerenciar a navegação do aplicativo entre as páginas locais e remotas.

## Mantenha seu aplicativo seguro – Definir regras de URI de conteúdo do aplicativo (ACURs)

Por meio de ACURs, também conhecidos como uma lista de permissão de URL, você é capaz de dar acesso direto das URLs remotas às APIs universais do Windows a partir de HTML, CSS e JavaScript remoto. No nível do sistema operacional Windows, os limites de política corretos foram definidos para permitir que o código hospedado em seu servidor web chamem diretamente as APIs da plataforma. Você pode definir esses limites no manifesto do pacote do aplicativo quando colocar o conjunto de URLs que colocam o seu Aplicativo Web hospedado nas Regras de URI de conteúdo do aplicativo (ACURs). As regras devem incluir a página inicial do aplicativo e outras páginas que você deseja incluir como páginas do aplicativo. Opcionalmente, você pode excluir URLs específicos também.

Há várias formas de especificar uma correspondência de URL nas suas regras:

- Um nome de host exato
- Um nome de host para o qual um URI com qualquer subdomínio desse nome de host é incluído ou excluído
- Um URI exato
- Um URI exato que pode conter uma propriedade de consulta
- Um caminho parcial e um curinga para indicar uma extensão de arquivo específica para uma regra de inclusão
- Caminhos relativos para regras de exclusão

Se o usuário navegar até um URL que não está incluído em suas regras, o Windows abrirá o URL de destino em um navegador.

Veja a seguir alguns exemplos de ACURs.

```HTML
<Application
Id="App"
StartPage="http://contoso.com/home">
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="https://contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="include" Match="https://*.contoso.com/" WindowsRuntimeAccess="all" />
    <uap:Rule Type="exclude" Match="https://contoso.com/excludethispage.aspx" />
</uap:ApplicationContentUriRules>
```

## Chamar APIs de Windows Runtime

Se uma URL é definida dentro dos limites do aplicativo (ACURs), ele pode chamar APIs do Windows Runtime em JavaScript usando o atributo "WindowsRuntimeAccess". O namespace do Windows será injetado e apresentado no mecanismo de script quando uma URL com acesso apropriado for carregada no Host de aplicativo. Isso torna as APIs universais do Windows disponíveis para que scripts do aplicativo possam chamar diretamente. Como desenvolvedor, você só precisa detectar recursos para a API do Windows que você gostaria de chamar e, se disponível, ir para recursos de plataforma específicos.

Para habilitar esse recurso, você precisa especificar o atributo `(WindowsRuntimeAccess="<<level>>")` em ACURs com um destes valores:

- 
              **all**: remove o código JavaScript que tem acesso a todas às APIs UWP e aos componentes empacotados locais.
- 
              **allowForWeb**: remove o código JavaScript que tem acesso somente ao código de pacote personalizado. Acesso local à componente C++/C# personalizados.
- 
              **none**: padrão. A URL especificada não tem acesso a plataforma.

Veja um tipo de regra de exemplo:

```HTML
<uap:ApplicationContentUriRules>
    <uap:Rule Type="include" Match="http://contoso.com/" WindowsRuntimeAccess="all"  />
</uap:ApplicationContentUriRules>
```

Isso proporciona o script em execução em http://contoso.com/ acesso para namespaces do Windows Runtime e componentes personalizados incluídos no pacote. Consulte o exemplo [Windows.UI.Notifications.js](https://gist.github.com/Gr8Gatsby/3d471150e5b317eb1813#file-windows-ui-notifications-js) no GitHub para notificações do sistema.

Aqui está um exemplo de como implementar um Bloco Dinâmico e atualizá-lo a partir do JavaScript remoto:

```Javascript
function updateTile(message, imgUrl, imgAlt) {
    // Namespace: Windows.UI.Notifications

    if (typeof Windows !== 'undefined'&&
            typeof Windows.UI !== 'undefined' &&
            typeof Windows.UI.Notifications !== 'undefined') {  
        var notifications = Windows.UI.Notifications,
        tile = notifications.TileTemplateType.tileSquare150x150PeekImageAndText01,
        tileContent = notifications.TileUpdateManager.getTemplateContent(tile),
        tileText = tileContent.getElementsByTagName('text'),
        tileImage = tileContent.getElementsByTagName('image');  
        tileText[0].appendChild(tileContent.createTextNode(message || 'Demo Message'));
        tileImage[0].setAttribute('src', imgUrl || 'https://unsplash.it/150/150/?random');
        tileImage[0].setAttribute('alt', imgAlt || 'Random demo image');    
        var tileNotification = new notifications.TileNotification(tileContent);
        var currentTime = new Date();
        tileNotification.expirationTime = new Date(currentTime.getTime() + 600 * 1000);
        notifications.TileUpdateManager.createTileUpdaterForApplication().update(tileNotification);
    } else {
        //Alternative behavior

    }
}
```

Esse código produzirá um bloco que parece com isto:

![Chamar um bloco dinâmico a partir do Windows 10](images/hwa-to-uwp/hwa_livetile.png)

Chamar APIs do Windows Runtime com qualquer ambiente ou técnica é mais familiar para você, mantendo seus recursos em um servido com detecção de recurso para recursos do Windows antes de chamá-los. Se os recursos da plataforma não estiverem disponíveis e o aplicativo da web estiver sendo executado em outro host, você pode fornecer ao usuário uma experiência padrão que funciona no navegador.

## Integrar comandos de voz da Cortana

Você pode tirar proveito da integração com a Cortana especificando um arquivo de definição de comando de voz (VCD) na sua página html. O arquivo VCD é um arquivo xml que mapeia os comandos para frases específicas. Por exemplo, um usuário pode tocar no botão Iniciar e dizer "Contoso Books, mostrar best sellers" para inicializar o aplicativo Contoso Books e navegar para a página "best sellers".

Quando você adiciona uma tag de elemento `<meta>` que lista o local do seu arquivo VCD, o Windows automaticamente baixa e registra o arquivo de definição de comando de voz.

Veja um exemplo do uso da tag em uma página html em um aplicativo da Web hospedado:

```HTML
<meta name="msapplication-cortanavcd" content="http:// contoso.com/vcd.xml"/>
```

Para saber mais sobre integração com a Cortana e VCDs, consulte interações da Cortana e Voice Command Defintion (VCD) elements and attributes v1.2.

## Criar aplicativos híbridos – Aplicativos Web empacotados vs. Aplicativos Web hospedados

Você tem opções para criar seu aplicativo UWP. O aplicativo pode ser desenvolvido para download da Windows Store e hospedagem total no cliente local, frequentemente chamado de **Aplicativo Web empacotado**. Isso permite que você execute seu aplicativo offline em qualquer plataforma compatível. Ou o aplicativo pode ser um aplicativo Web totalmente hospedado que é executado em um servidor Web remoto, geralmente conhecido como **Hosted Web App**. Mas há também uma terceira opção: o aplicativo pode ser hospedado parcialmente no cliente local e parcialmente em um servidor Web remoto. Chamamos essa terceira opção de **Hybrid app**, e ela normalmente usa o componente **WebView** para fazer com que o conteúdo remoto se pareça com o conteúdo local. Aplicativos híbridos podem incluir seu código HTML5, CSS e Javascript em execução como um pacote dentro do cliente de aplicativo local e reter a capacidade de interagir com o conteúdo remoto.

## Agente de autenticação da Web

Você pode usar o agente de autenticação da Web para manipular o fluxo de logon para os usuários se tiver um provedor de identidade online que use protocolos de autenticação e autorização da Internet como OpenID ou OAuth. Especifique os URIs iniciais e finais em uma tag `<meta>` em uma página html em seu aplicativo. O Windows detecta esses URIs e os transmite para o agente de autenticação da Web para concluir o fluxo de logon. O URI inicial é composto pelo endereço para onde você envia a solicitação de autenticação para o seu provedor de identidade online junto com outras informações necessárias, como ID do aplicativo, um URI de redirecionamento para o qual o usuário é enviado após concluir a autenticação e o tipo de resposta esperado. Seu provedor deverá informar quais são os parâmetros necessários. Veja um exemplo de uso da tag `<meta>`:

```HTML
<meta name="ms-webauth-uris" content="https://<providerstartpoint>?client_id=<clientid>&response_type=token, https://<appendpoint>"/>
```

Para saber mais, consulte [Considerações do agente de autenticação da Web para provedores online](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx).

## Declarações de funcionalidades do aplicativo

Se seu aplicativo precisar de acesso programático a recursos do usuário como imagens ou músicas, ou dispositivos como uma câmera ou microfone, você deverá declarar a funcionalidade apropriada. Há três categorias de declaração de recurso de aplicativo: 

- 
              [As funcionalidades de uso geral](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#General-use_capabilities) que se aplicam à maioria dos cenários de aplicativos. 
- 
              [As funcionalidades do dispositivo](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Device_capabilities) que permitem que seu aplicativo acesse dispositivos periféricos e internos. 
- 
              [Funcionalidades de uso especiais](https://msdn.microsoft.com/library/windows/apps/Mt270968.aspx#Special_and_restricted_capabilities) que requerem uma conta empresarial especial para serem enviadas à Store para uso. 

Para saber mais sobre contas empresariais, consulte [Tipos de conta, locais e taxas](https://msdn.microsoft.com/library/windows/apps/jj863494.aspx).

> [!NOTE]
> É importante saber que, quando os clientes compram seu aplicativo na Windows Store, eles são notificados sobre todas as funcionalidades declaradas pelo aplicativo. Portanto, não use recursos de que seu aplicativo não precisa.

Solicite acesso declarando as funcionalidades no [manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/br211474.aspx) do seu aplicativo. Você pode declarar as funcionalidades gerais usando o [Designer de Manifesto](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx#Configure) no Microsoft Visual Studio ou adicioná-las manualmente ao manifesto do pacote.[Consulte Como especificar funcionalidades em um manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/br211477.aspx).

Alguns recursos fornecem acesso de aplicativos a um recurso confidencial. Esses recursos são considerados confidenciais porque podem acessar dados pessoais do usuário ou acarretar custos para o usuário. Configurações de privacidade, gerenciadas pelo aplicativo Configurações, permitem que o usuário controle dinamicamente o acesso a recursos confidenciais. Assim, é importante que seu aplicativo não suponha que um recurso confidencial sempre está disponível. Para obter mais informações sobre como acessar recursos confidenciais, consulte [Diretrizes para aplicativos com reconhecimento de privacidade](https://msdn.microsoft.com/library/windows/apps/hh768223.aspx).

## manifoldjs e o manifesto do aplicativo

Uma maneira fácil de transformar seu site em um aplicativo UWP é usar um **manifesto do aplicativo** e **manifoldjs**. O manifesto do aplicativo é um arquivo xml que contém metadados sobre o aplicativo. Ele especifica itens como o nome do aplicativo, links para recursos, o modo de exibição, URLs e outros dados que descrevem como o aplicativo deve ser implantado e executado. manifoldjs facilita esse processo, mesmo em sistemas que não oferecem suporte a aplicativos Web. Acesse [manifoldjs.com](http://www.manifoldjs.com/) para saber mais sobre seu funcionamento. Você também pode ver uma demonstração sobre manifoldjs como parte desta apresentação [Aplicativos Web do Windows 10](http://channel9.msdn.com/Events/WebPlatformSummit/2015/Hosted-web-apps-and-web-platform-innovations?wt.mc_id=relatedsession).

## Tópicos relacionados
- [API Tempo do Windows Runtime: exemplos de código JavaScript](http://rjs.azurewebsites.net/)
- [Codepen: área restrita a ser usada para chamar APIs do Windows Runtime](http://codepen.io/seksenov/pen/wBbVyb/)
- [Interações da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974231.aspx)
- [Elementos e atributos de Voice Command Definition (VCD) v1.2](https://msdn.microsoft.com/library/windows/apps/dn954977.aspx)
- [Considerações do agente de autenticação da Web para provedores online](https://msdn.microsoft.com/library/windows/apps/dn448956.aspx)
- [Declarações de funcionalidades do aplicativo](https://msdn.microsoft.com/ibrary/windows/apps/hh464936.aspx)


<!--HONumber=Jul16_HO2-->


