---
author: TylerMSFT
title: "Iniciar o aplicativo padrão para um URI"
description: "Saiba como iniciar o aplicativo padrão para um Uniform Resource Identifier (URI). Os URIs permitem iniciar outro aplicativo para realizar uma tarefa específica. Este tópico também apresenta uma visão geral dos muitos esquemas de URI compilados no Windows."
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.sourcegitcommit: 9011d2e2e1e51edc89851e815d31e13390c24f96
ms.openlocfilehash: d454317d135e2b2b952c16fb00685e34b489865c

---

# Iniciar o aplicativo padrão para um URI


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-   [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
-   [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Saiba como iniciar o aplicativo padrão para um URI (Uniform Resource Identifier). Os URIs permitem iniciar outro aplicativo para realizar uma tarefa específica. Este tópico também apresenta uma visão geral dos muitos esquemas de URI compilados no Windows. Você também pode iniciar URIs personalizados. Para obter mais informações sobre como registrar um esquema de URI personalizado e identificar a ativação de URI, consulte [Identificar ativação de URI](handle-uri-activation.md).

## Como iniciar um URI


Os esquemas de URI permitem que você abra aplicativos clicando em hiperlinks. Assim como você pode iniciar um novo email usando **mailto:**, também é possível abrir o navegador da Web padrão usando **http:**. Este tópico descreve alguns dos esquemas de URI integrados ao Windows:

-   O [esquema de URI ms-settings:](#settings) inicia o aplicativo Configurações do Windows
-   O [esquema de URI ms-store:](#store) inicia o aplicativo da Windows Store
-   O [esquema de URI http:](#browser) inicia o navegador da Web padrão
-   O [esquema de URI mailto:](#email) inicia o aplicativo de email padrão
-   Os [esquemas de URI bingmaps:, ms-drive-to: e ms-walk-to:](#maps) iniciam o aplicativo Mapas do Windows

Por exemplo, o URI a seguir abre o navegador padrão e exibe o site do Bing.

`http://bing.com`

Você também pode iniciar esquemas de URI personalizados. Se não houver aplicativo instalado para manipular esse URI, você poderá recomendar um aplicativo para o usuário instalar. Para obter mais informações, consulte [Recomendar um aplicativo](#recommend).

Em geral, seu aplicativo não pode selecionar o aplicativo que foi iniciado. O usuário determina o aplicativo que é iniciado. Mais de um aplicativo pode registrar para manipular o mesmo esquema de URI. A exceção a isso é para esquemas de URI reservados. Os registros de esquemas de URI reservados são ignorados. Para obter a lista completa de esquemas de URI reservados, consulte [Manipular a ativação do URI](handle-uri-activation.md). Em casos onde mais de um aplicativo pode ter registrado o mesmo esquema de URI, seu aplicativo pode recomendar um aplicativo específico para ser iniciado. Para obter mais informações, consulte [Recomendar um aplicativo](#recommend).

### Chamar LaunchUriAsync

Use o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar um URI. Ao chamar esse método, seu aplicativo precisa estar em primeiro plano, ou seja, visível para o usuário. Essa exigência ajuda a garantir que o usuário permaneça no controle. Para que essa exigência seja atendida, você deve vincular todas as inicializações de URI diretamente à interface do usuário do seu aplicativo. O usuário sempre deve executar alguma ação para iniciar uma inicialização de URI. Se você tentar iniciar um URI e seu aplicativo não estiver em primeiro plano, a inicialização falhará e o retorno de chamada de erro será invocado.

Primeiro, crie um objeto [**System.Uri**](https://msdn.microsoft.com/en-us/library/windows/apps/system.uri.aspx) para representar o URI e passe-o para o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476). Use o resultado de retorno para ver se a chamada foi bem-sucedida, conforme mostrado no exemplo a seguir.

```cs
private async void launchURI_Click(object sender, RoutedEventArgs e)
{
   // The URI to launch
   var uriBing = new Uri(@"http://www.bing.com");

   // Launch the URI
   var success = await Windows.System.Launcher.LaunchUriAsync(uriBing);

   if (success)
   {
      // URI launched
   }
   else
   {
      // URI launch failed
   }
}
```

Em alguns casos, o sistema operacional solicitará que o usuário veja se realmente quer alternar entre aplicativos.

![uma caixa de diálogo de aviso é sobreposta em um plano de fundo esmaecido do aplicativo. a caixa de diálogo pergunta ao usuário se ele quer alternar entre aplicativos e tem botões 'sim' e 'não' na parte inferior direita. o botão 'não' está realçado.](images/warningdialog.png)

Se você quiser que esse prompt ocorra sempre, use a propriedade [**Windows.System.LauncherOptions.TreatAsUntrusted**](https://msdn.microsoft.com/library/windows/apps/hh701442) para indicar que o sistema operacional deve exibir um aviso.

```cs
// The URI to launch
var uriBing = new Uri(@"http://www.bing.com");

// Set the option to show a warning
var promptOptions = new Windows.System.LauncherOptions();
promptOptions.TreatAsUntrusted = true;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriBing, promptOptions);
```

### Recomendar um aplicativo

Em alguns casos, pode ser que o usuário não tenha um aplicativo instalado para manipular o URI que você está iniciando. Por padrão, nesses casos, o sistema operacional oferece ao usuário um link para pesquisar o aplicativo apropriado na loja. Se você quiser dar ao usuário uma recomendação específica sobre qual aplicativo deve ser adquirido nesse cenário, pode transmitir tal recomendação junto com o URI que você está iniciando.

Recomendações também são úteis quando mais de um aplicativo é registrado para manipular um esquema de URI. Ao recomendar um aplicativo específico, o Windows abrirá esse aplicativo se ele já estiver instalado.

Para fazer uma recomendação, chame o método [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) com [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) definido como o nome da família do pacote do aplicativo na loja que você quer recomendar. O sistema operacional usará essas informações para substituir a opção geral de pesquisar um aplicativo na loja por uma opção específica para adquirir o aplicativo recomendado na loja.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### Definir a preferência de exibição restante

Os aplicativos de origem que chamam [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) podem solicitar que eles permaneçam na tela após a inicialização de um arquivo. Por padrão, o Windows tenta compartilhar todo o espaço disponível igualmente entre o aplicativo de origem e o aplicativo de destino que manipula o URI. Aplicativos de origem podem usar a propriedade [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) para indicar ao sistema operacional que eles preferem que sua janela de aplicativo ocupe mais ou menos espaço disponível. O **DesiredRemainingView** também pode ser usado para indicar que o aplicativo de origem não precisa permanecer na tela depois da inicialização do URI e pode ser completamente substituído pelo aplicativo de destino. Esta propriedade especifica somente o tamanho da janela preferido do aplicativo de chamada. Ele não especifica o comportamento de outros aplicativos que podem acontecer de também estar na tela ao mesmo tempo.

**Observação**  O Windows leva em conta vários fatores diferentes ao determinar o tamanho da janela final do aplicativo de origem, por exemplo, a preferência do aplicativo de origem, o número de aplicativos na tela, a orientação da tela e assim por diante. Definindo [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314), você não garante um comportamento de janelas específico para o aplicativo de origem.

 

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## Esquemas de URI do aplicativo Mapas do Windows


Seu aplicativo pode usar os esquemas de URI **bingmaps:**, **ms-drive-to:** e **ms-walk-to:** para [iniciar o aplicativo Mapas do Windows](launch-maps-app.md) para especificar mapas, trajetos e resultados de pesquisa específicos. Por exemplo, o URI a seguir abre o aplicativo Mapas do Windows e exibe um mapa centralizado na cidade de Nova York.

`bingmaps:?cp=40.726966~-74.006076`

![um exemplo do aplicativo mapas do windows.](images/mapnyc.png)

Para obter mais informações, consulte [Iniciar o aplicativo Mapas do Windows](launch-maps-app.md). Para usar o controle de mapa em seu próprio aplicativo, consulte [Exibir mapas em modos de exibição 2D, 3D e Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695).

## Esquema de URI do aplicativo Configurações do Windows


Seu aplicativo pode usar o esquema de URI **ms-settings:** para [iniciar o aplicativo Configurações do Windows](launch-settings-app.md). A inicialização do aplicativo Configurações é uma parte importante da escrita de um aplicativo com reconhecimento de privacidade. Se seu aplicativo não pode acessar um recurso confidencial, é recomendável fornecer ao usuário um link conveniente para as configurações de privacidade desse recurso. Por exemplo, o URI a seguir abre o aplicativo Configurações e exibe as configurações de privacidade da câmera.

`ms-settings:privacy-webcam`

![configurações de privacidade da câmera.](images/privacyawarenesssettingsapp.png)

Para obter mais informações, consulte [Iniciar o aplicativo Configurações do Windows](launch-settings-app.md) e [Diretrizes de aplicativos com reconhecimento de privacidade](https://msdn.microsoft.com/library/windows/apps/hh768223).

## Esquema de URI do aplicativo da Windows Store


Seu aplicativo pode usar o esquema de URI **ms-windows-store:** para [Iniciar o aplicativo da Windows Store](launch-store-app.md). Abra páginas de detalhes do produto, páginas de análise do produto, páginas de pesquisa etc. Por exemplo, o URI a seguir abre o aplicativo da Windows Store e inicia a página inicial da Loja.

`ms-windows-store://home/`

Para obter mais informações, consulte [Iniciar o aplicativo da Windows Store](launch-store-app.md).

## Esquema do URI do aplicativo de Chamada


Seu aplicativo pode usar o esquema de URI **ms-call:** para iniciar o aplicativo de Chamada.

| Esquema de URI       | Resultados                               |
|------------------|---------------------------------------|
| ms-call:settings | Inicia a página de configurações do aplicativo de Chamada. |

 

## Esquema do URI do aplicativo Chat


Seu aplicativo pode usar os esquemas de URI **ms-chat:** para iniciar o aplicativo Mensagens.

| Esquema de URI                               | Resultados                                                                                                                                                                                |
|------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ms-chat:                                 | Inicia o aplicativo Mensagens.                                                                                                                                                            |
| ms-chat:?ContactID={contacted}           | Permite que o aplicativo de mensagens seja iniciado com informações de um contato específico.                                                                                               |
| ms-chat:?Body={body}                     | Permite que o aplicativo de mensagens seja iniciado com uma cadeia de caracteres para usar como o conteúdo da mensagem.                                                                                    |
| ms-chat:?Addresses={address}&Body={body} | Permite que o aplicativo de mensagens seja iniciado com informações de um determinado endereço e com uma cadeia de caracteres para usar como o conteúdo da mensagem. Observação: os endereços podem ser concatenados. |
| ms-chat:?TransportId={transportId}       | Permite que o aplicativo de mensagens seja iniciado com uma ID de transporte específica.                                                                                                        |

 

## Esquema do URI de email


Seu aplicativo pode usar os esquemas de URI **mailto:** para iniciar o aplicativo de email padrão.

| Esquema de URI               | Resultados                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mailto:                  | Inicia o aplicativo de email padrão.                                                                                                                             |
| mailto:\[endereço de email\] | Inicia o aplicativo de email e cria uma nova mensagem com o endereço de email especificado na linha Para. Observe que o email não é enviado até que o usuário toque em Enviar. |

 

## Esquema de URI HTTP


Seu aplicativo pode usar os esquemas de URI **http:** para iniciar o navegador da Web padrão.

| Esquema de URI | Resultados                           |
|------------|-----------------------------------|
| http:      | Inicia o navegador da Web padrão. |

 

## Esquema de URI do aplicativo Números nas Proximidades


Seu aplicativo pode usar o esquema de URI **ms-yellowpage:** para iniciar o aplicativo Números nas Proximidades.

| Esquema de URI                                            | Resultados                                                                               |
|-------------------------------------------------------|---------------------------------------------------------------------------------------|
| ms-yellowpage:?input=\[keyword\]&method=\[String|T9\] | Inicia o aplicativo de pesquisa de POI (pontos de interesse) instalado que oferece suporte a esse novo URI. |

 

## Esquema de URI do aplicativo Pessoas


Seu aplicativo pode usar o esquema de URI **ms-people:** para iniciar o aplicativo Pessoas.

Para obter mais informações, consulte [Iniciar o aplicativo Pessoas](launch-people-apps.md).

 

 



<!--HONumber=Jun16_HO4-->


