---
title: Permitir aplicativos para sites que usam manipuladores de URI do aplicativo
description: Proporcione o envolvimento do usuário com seu aplicativo ao oferecer suporte ao recurso de Aplicativos para sites.
keywords: Vinculação profunda do Windows
ms.date: 08/25/2017
ms.topic: article
ms.assetid: 260cf387-88be-4a3d-93bc-7e4560f90abc
ms.localizationpriority: medium
ms.openlocfilehash: 5807cdc19e4b38c8cc8fa4ca45c4ef47e79b7742
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282253"
---
# <a name="enable-apps-for-websites-using-app-uri-handlers"></a>Permitir aplicativos para sites que usam manipuladores de URI do aplicativo

Os aplicativos para sites associam seu aplicativo a um site para quando alguém abrir um link para seu site, o aplicativo é iniciado em vez de abrir o navegador. Caso seu aplicativo não seja instalado, o site abre o navegador normalmente. Os usuários podem confiar nessa experiência porque apenas proprietários conteúdo verificados podem se registrar para um link. Os usuários serão capazes de verificar todos os links da Web para o aplicativo registrados em Configurações > Aplicativos > Aplicativos para sites.

Para habilitar a vinculação de aplicativos à Web, você precisará:
- Identificar os URIs que seu aplicativo manipulará no arquivo de manifesto
- Um arquivo JSON que define a associação entre seu aplicativo e o site. com o Nome da Família de Pacotes do aplicativo na mesma raiz de host que a declaração de manifesto do aplicativo.
- Manipule a ativação no aplicativo.

> [!Note]
> Começando com a atualização do Windows 10 para criadores, os links com suporte clicados no Microsoft Edge inicializarão o aplicativo correspondente. Os links com suporte clicados em outros navegadores (por exemplo, Internet Explorer etc.), manterão você na experiência de navegação.

## <a name="register-to-handle-http-and-https-links-in-the-app-manifest"></a>Registre-se para manipular links http e https no manifesto do aplicativo

Seu aplicativo deve identificar os URIs para os sites que ele manipulará. Para fazer isso, adicione o registro de extensão **Windows.appUriHandler** no arquivo de manifesto do aplicativo **Package. appxmanifest**.

Por exemplo, se o endereço do site for "msn.com", faça a seguinte entrada no manifesto do aplicativo:

```xml
<Applications>
  <Application ... >
      ...
      <Extensions>
         <uap3:Extension Category="windows.appUriHandler">
          <uap3:AppUriHandler>
            <uap3:Host Name="msn.com" />
          </uap3:AppUriHandler>
        </uap3:Extension>
      </Extensions>
  </Application>
</Applications>
```

A declaração acima registra seu aplicativo para lidar com os links do host especificado. Se seu site tiver vários endereços (por exemplo: m.example.com, www\.example.com e example.com), adicione uma entrada de `<uap3:Host Name=... />` separada dentro do `<uap3:AppUriHandler>` para cada endereço.

## <a name="associate-your-app-and-website-with-a-json-file"></a>Associar seu aplicativo e o site com um arquivo JSON

Para garantir que somente seu aplicativo possa abrir conteúdo em seu site, inclua o nome de família do pacote do seu aplicativo em um arquivo JSON localizado na raiz do servidor Web ou no diretório conhecido no domínio. Isso significa que seu site autoriza que os aplicativos listados abram conteúdo no seu site. Você pode encontrar o nome de família do pacote na seção Pacotes no designer de manifesto do aplicativo.

>[!Important]
> O arquivo JSON não deve ter um sufixo de arquivo .json.

Crie um arquivo JSON (sem a extensão de arquivo .json) chamado **windows-app-web-link** e forneça o nome de família do pacote do aplicativo. Por exemplo:

``` JSON
[{
  "packageFamilyName": "Your app's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 }]
```

O Windows estabelecerá uma conexão https com seu site e procurará o arquivo JSON correspondente em seu servidor Web.

### <a name="wildcards"></a>Curingas

O exemplo de arquivo JSON acima demonstra o uso de curingas. Os curingas permitem que você dê suporte a uma ampla variedade de links com menos linhas de código. A vinculação de aplicativos à Web dá suporte a dois tipos de curingas no arquivo JSON:

| **Amplia** | **Descrição**               |
|--------------|-------------------------------|
| **\***       | Representa qualquer subcadeia de caracteres      |
| **?**        | Representa um único caractere |

Por exemplo, dado `"excludePaths" : [ "/news/*", "/blog/*" ]` no exemplo acima, seu aplicativo dará suporte a todos os caminhos que começam com o endereço do site (por exemplo, msn.com), **exceto** aqueles em `/news/` e `/blog/`. **MSN.com/Weather.html** terá suporte, mas não **MSN.com/News/topnews.html**.

### <a name="multiple-apps"></a>Vários aplicativos

Se você tiver dois aplicativos que gostaria de vincular ao seu site, indique os dois nomes de família do pacote do aplicativo em seu arquivo JSON **windows-app-web-link**. Os dois aplicativos podem ter suporte. O usuário poderá escolher qual o link padrão se os dois forem instalados. Se desejar alterar o link padrão posteriormente, poderá fazê-lo em **Configurações > Aplicativos para Sites**. Os desenvolvedores também podem alterar o arquivo JSON a qualquer momento e ver a alteração no mesmo dia, mas até oito dias após a atualização.

``` JSON
[{
  "packageFamilyName": "Your apps's package family name, e.g MyApp_9jmtgj1pbbz6e",
  "paths": [ "*" ],
  "excludePaths" : [ "/news/*", "/blog/*" ]
 },
 {
  "packageFamilyName": "Your second app's package family name, for example, MyApp2_8jmtgj2pbbz6e",
  "paths": [ "/example/*", "/links/*" ]
 }]
```

Para proporcionar a melhor experiência para seus usuários, use caminhos excluídos para garantir que o conteúdo somente online seja excluído dos caminhos com suporte em seu arquivo JSON.

Caminhos excluídos são verificados primeiro e se houver uma correspondência, a página correspondente será aberta com o navegador em vez do aplicativo designado. No exemplo acima, '/News/\*' inclui todas as páginas sob esse caminho enquanto '/News\*' (nenhuma barra invertida ' News ') inclui todos os caminhos em ' News\*', como ' newslocal/', ' newsinternational/' e assim por diante.

## <a name="handle-links-on-activation-to-link-to-content"></a>Manipular links na ativação para vincular ao conteúdo

Navegue até **App.xaml.cs** na solução do Visual Studio do seu aplicativo e em **OnActivated ()** , adicione manipulação para conteúdo vinculado. No exemplo a seguir, a página que é aberta no aplicativo depende do caminho URI:

``` CS
protected override void OnActivated(IActivatedEventArgs e)
{
    Frame rootFrame = Window.Current.Content as Frame;
    if (rootFrame == null)
    {
        ...
    }

    // Check ActivationKind, Parse URI, and Navigate user to content
    Type deepLinkPageType = typeof(MainPage);
    if (e.Kind == ActivationKind.Protocol)
    {
        var protocolArgs = (ProtocolActivatedEventArgs)e;        
        switch (protocolArgs.Uri.AbsolutePath)
        {
            case "/":
                break;
            case "/index.html":
                break;
            case "/sports.html":
                deepLinkPageType = typeof(SportsPage);
                break;
            case "/technology.html":
                deepLinkPageType = typeof(TechnologyPage);
                break;
            case "/business.html":
                deepLinkPageType = typeof(BusinessPage);
                break;
            case "/science.html":
                deepLinkPageType = typeof(SciencePage);
                break;
        }
    }

    if (rootFrame.Content == null)
    {
        // Default navigation
        rootFrame.Navigate(deepLinkPageType, e);
    }

    // Ensure the current window is active
    Window.Current.Activate();
}
```

**Importante** Certifique-se de substituir a lógica `if (rootFrame.Content == null)` final por `rootFrame.Navigate(deepLinkPageType, e);` conforme mostrado no exemplo acima.

## <a name="test-it-out-local-validation-tool"></a>Teste: ferramenta de validação de local

Você pode testar a configuração do seu aplicativo e do site executando a ferramenta de verificação de registro de host de aplicativo que está disponível em:

% WINDIR%\\system32\\**AppHostRegistrationVerifier. exe**

Teste a configuração do seu aplicativo e do site executando essa ferramenta com os seguintes parâmetros:

**AppHostRegistrationVerifier. exe** *nome do host packagefamilyname FilePath*

-   Nome do host: seu site (por exemplo, microsoft.com)
-   Nome da Família de Pacotes (PFN): o PFN do seu aplicativo
-   Caminho do arquivo: o arquivo JSON para validação local (por exemplo, C:\\SomeFolder\\Windows-app-web-link)

Se a ferramenta não retornar qualquer coisa, a validação funcionará nesse arquivo quando for carregado. Se houver um código de erro, ele não funcionará.

Você pode habilitar a seguinte chave do registro para forçar o caminho correspondente para aplicativos intermediados como parte da validação local:

`HKCU\Software\Classes\LocalSettings\Software\Microsoft\Windows\CurrentVersion\
AppModel\SystemAppData\YourApp\AppUriHandlers`

KeyName: valor de `ForceValidation`: `1`

## <a name="test-it-web-validation"></a>Teste: validação Web

Feche o aplicativo para verificar se ele é ativado quando você clica em um link. Em seguida, copie o endereço de um dos caminhos com suporte no seu site. Por exemplo, se o endereço do seu site for "msn.com" e um dos caminhos de suporte for "caminho1", você usaria `http://msn.com/path1`

Verifique se seu aplicativo é fechado. Pressione a **tecla Windows + R** para abrir a caixa de diálogo **Executar** e cole o link na janela. Seu aplicativo deve ser iniciado em vez do navegador da Web.

Além disso, você pode testar seu aplicativo iniciando-o em outro aplicativo com a API [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync). Você pode usar essa API para testar em telefones também.

Se você quiser seguir a lógica de ativação de protocolo, defina um ponto de interrupção no manipulador de eventos **OnActivated**.

## <a name="appurihandlers-tips"></a>Dicas sobre AppUriHandlers:

- Certifique-se de especificar somente links que seu aplicativo possa manipular.
- Liste todos os hosts aos quais você dará suporte.  Observe que www\.example.com e example.com são hosts diferentes.
- Os usuários podem escolher qual aplicativo preferem para lidar com sites em Configurações.
- Seu arquivo JSON deve ser carregado para um servidor https.
- Se você precisar alterar os caminhos aos quais deseja dar suporte, você pode republicar seu arquivo JSON sem precisar republicar seu aplicativo. Os usuários verão as alterações em 1 a 8 dias.
- Todos os aplicativos de sideload com AppUriHandlers terão links validados para o host na instalação. Você não precisa ter um arquivo JSON carregado para testar o recurso.
- Esse recurso funciona sempre que seu aplicativo é iniciado com um aplicativo UWP [LaunchUriAsync](https://docs.microsoft.com/uwp/api/windows.system.launcher.launchuriasync) ou um aplicativo da área de trabalho do Windows iniciado com [ShellExecuteEx](https://docs.microsoft.com/windows/desktop/api/shellapi/nf-shellapi-shellexecuteexa). Se a URL corresponder a um manipulador de URI do aplicativo registrado, o aplicativo será iniciado em vez do navegador.

## <a name="see-also"></a>Consulte também

[Projeto de exemplo de aplicativo para Web](https://github.com/project-rome/AppUriHandlers/tree/master/NarwhalFacts)
[registro do windows.protocol](https://docs.microsoft.com/uwp/schemas/appxpackage/appxmanifestschema/element-protocol)
[Manipular a ativação de URI](https://docs.microsoft.com/windows/uwp/launch-resume/handle-uri-activation)
[Amostra de execução de associação](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching) ilustra como usar a API LaunchUriAsync ().
