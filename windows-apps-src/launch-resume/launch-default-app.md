---
author: TylerMSFT
title: "Iniciar o app padrão de um URI"
description: "Saiba como iniciar o app padrão para um Uniform Resource Identifier (URI). Os URIs permitem iniciar outro app para realizar uma tarefa específica. Este tópico também apresenta uma visão geral dos muitos esquemas de URI compilados no Windows."
ms.assetid: 7B0D0AF5-D89E-4DB0-9B79-90201D79974F
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: fcc1d056fc3a4cb8d57ae5082e62cf0b802bb4e7
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-default-app-for-a-uri"></a>Iniciar o app padrão de um URI

\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

**APIs importantes**

- [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476)
-  [**PreferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482)
- [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314)

Saiba como iniciar o app padrão para um URI (Uniform Resource Identifier). Os URIs permitem iniciar outro app para realizar uma tarefa específica. Este tópico também apresenta uma visão geral dos muitos esquemas de URI compilados no Windows. Você também pode iniciar URIs personalizados. Para obter mais informações sobre como registrar um esquema de URI personalizado e identificar a ativação de URI, consulte [Identificar ativação de URI](handle-uri-activation.md).


Os esquemas de URI permitem que você abra apps clicando em hiperlinks. Assim como você pode iniciar um novo email usando **mailto:**, também é possível abrir o navegador da Web padrão usando **http:**

Este tópico descreve os seguintes esquemas de URI integrados ao Windows:

| Esquema de URI | Inicia |
| -------|--------------|
|[bingmaps:, ms-drive-to: e ms-walk-to: ](#maps-app-uri-schemes) | Aplicativo Mapas |
|[http:](#http-uri-scheme) | Navegador da Web padrão |
|[mailto:](#email-uri-scheme) | Aplicativo de email padrão |
|[ms-call:](#call-app-uri-scheme) |  Aplicativo de chamada |
|[ms-chat:](#messaging-app-uri-scheme) | Aplicativo de mensagens |
|[ms-people:](#people-app-uri-scheme) | Aplicativo Pessoas |
|[ms-settings:](#settings-app-uri-scheme) | Aplicativo Configurações |
|[ms-store:](#store-app-uri-scheme)  | Aplicativo da Loja |
|[ms-tonepicker:](#tone-picker-uri-scheme) | Seletor de tom |
|[ms-yellowpage:](#nearby-numbers-app-uri-scheme) | Aplicativo Números nas Proximidades |

<br> Por exemplo, o URI a seguir abre o navegador padrão e exibe o site do Bing.

`http://bing.com`

Você também pode iniciar esquemas de URI personalizados. Se não houver app instalado para manipular esse URI, você poderá recomendar um app para o usuário instalar. Para obter mais informações, consulte [Recomendar um app se nenhum estiver disponível para manipular o URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

Em geral, seu app não pode selecionar o app que foi iniciado. O usuário determina o app que é iniciado. Mais de um app pode registrar para manipular o mesmo esquema de URI. A exceção a isso é para esquemas de URI reservados. Os registros de esquemas de URI reservados são ignorados. Para obter a lista completa de esquemas de URI reservados, consulte [Manipular a ativação do URI](handle-uri-activation.md). Em casos onde mais de um app pode ter registrado o mesmo esquema de URI, seu app pode recomendar um app específico para ser iniciado. Para obter mais informações, consulte [Recomendar um app se nenhum estiver disponível para manipular o URI](#recommend-an-app-if-one-is-not-available-to-handle-the-uri).

### <a name="call-launchuriasync-to-launch-a-uri"></a>Chamar LaunchUriAsync para iniciar um URI

Use o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) para iniciar um URI. Ao chamar esse método, seu app precisa estar em primeiro plano, ou seja, visível para o usuário. Essa exigência ajuda a garantir que o usuário permaneça no controle. Para que essa exigência seja atendida, você deve vincular todas as inicializações de URI diretamente à interface do usuário do seu app. O usuário sempre deve executar alguma ação para iniciar uma inicialização de URI. Se você tentar iniciar um URI e seu app não estiver em primeiro plano, a inicialização falhará e o retorno de chamada de erro será invocado.

Primeiro, crie um objeto [**System.Uri**](https://msdn.microsoft.com/library/windows/apps/system.uri.aspx) para representar o URI e passe-o para o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476). Use o resultado de retorno para ver se a chamada foi bem-sucedida, conforme mostrado no exemplo a seguir.

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

Em alguns casos, o sistema operacional solicitará que o usuário veja se realmente quer alternar entre apps.

![uma caixa de diálogo de aviso é sobreposta em um plano de fundo esmaecido do app. a caixa de diálogo pergunta ao usuário se ele quer alternar entre apps e tem botões 'sim' e 'não' na parte inferior direita. o botão 'não' está realçado.](images/warningdialog.png)

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

### <a name="recommend-an-app-if-one-is-not-available-to-handle-the-uri"></a>Recomendar um app se nenhum estiver disponível para manipular o URI

Em alguns casos, pode ser que o usuário não tenha um app instalado para manipular o URI que você está iniciando. Por padrão, nesses casos, o sistema operacional oferece ao usuário um link para pesquisar o app apropriado na loja. Se você quiser dar ao usuário uma recomendação específica sobre qual app deve ser adquirido nesse cenário, pode transmitir tal recomendação junto com o URI que você está iniciando.

Recomendações também são úteis quando mais de um app é registrado para manipular um esquema de URI. Ao recomendar um app específico, o Windows abrirá esse app se ele já estiver instalado.

Para fazer uma recomendação, chame o método [**Windows.System.Launcher.LaunchUriAsync(Uri, LauncherOptions)**](https://msdn.microsoft.com/library/windows/apps/hh701484) com [**LauncherOptions.preferredApplicationPackageFamilyName**](https://msdn.microsoft.com/library/windows/apps/hh965482) definido como o nome da família do pacote do app na loja que você quer recomendar. O sistema operacional usará essas informações para substituir a opção geral de pesquisar um app na loja por uma opção específica para adquirir o app recomendado na loja.

```cs
// Set the recommended app
var options = new Windows.System.LauncherOptions();
options.PreferredApplicationPackageFamilyName = "Contoso.URIApp_8wknc82po1e";
options.PreferredApplicationDisplayName = "Contoso URI Ap";

// Launch the URI and pass in the recommended app
// in case the user has no apps installed to handle the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

### <a name="set-remaining-view-preference"></a>Definir a preferência de exibição restante

Os apps de origem que chamam [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) podem solicitar que eles permaneçam na tela após a inicialização de um arquivo. Por padrão, o Windows tenta compartilhar todo o espaço disponível igualmente entre o app de origem e o app de destino que manipula o URI. Aplicativos de origem podem usar a propriedade [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314) para indicar ao sistema operacional que eles preferem que sua janela de app ocupe mais ou menos espaço disponível. O **DesiredRemainingView** também pode ser usado para indicar que o app de origem não precisa permanecer na tela depois da inicialização do URI e pode ser completamente substituído pelo app de destino. Esta propriedade especifica somente o tamanho da janela preferido do app de chamada. Ele não especifica o comportamento de outros apps que podem acontecer de também estar na tela ao mesmo tempo.

**Observação**  O Windows leva em conta vários fatores diferentes ao determinar o tamanho da janela final do app de origem, por exemplo, a preferência do app de origem, o número de apps na tela, a orientação da tela e assim por diante. Definindo [**DesiredRemainingView**](https://msdn.microsoft.com/library/windows/apps/dn298314), você não garante um comportamento de janelas específico para o app de origem.

```cs
// Set the desired remaining view.
var options = new Windows.System.LauncherOptions();
options.DesiredRemainingView = Windows.UI.ViewManagement.ViewSizePreference.UseLess;

// Launch the URI
var success = await Windows.System.Launcher.LaunchUriAsync(uriContoso, options);
```

## <a name="uri-schemes"></a>Esquemas de URI ##

Os diversos esquemas de URI descritos abaixo.
<br>

### <a name="call-app-uri-scheme"></a>Esquema do URI do app de Chamada

Seu app pode usar o esquema de URI **ms-call:** para iniciar o app de Chamada.

| Esquema de URI       | Resultado                   |
|------------------|--------------------------|
| ms-call:settings | Página de configurações do app de chamada. | 
<br>
### <a name="email-uri-scheme"></a>Esquema do URI de email

Seu app pode usar os esquemas de URI **mailto:** para iniciar o app de email padrão.

| Esquema de URI               | Resultados                                                                                                                                                     |
|--------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------|
| mailto:                  | Inicia o app de email padrão.                                                                                                                             |
| mailto:\[endereço de email\] | Inicia o app de email e cria uma nova mensagem com o endereço de email especificado na linha Para. Observe que o email não é enviado até que o usuário toque em Enviar. |
<br>
### <a name="http-uri-scheme"></a>Esquema de URI HTTP

Seu app pode usar os esquemas de URI **http:** para iniciar o navegador da Web padrão.

| Esquema de URI | Resultados                           |
|------------|-----------------------------------|
| http:      | Inicia o navegador da Web padrão. |
<br>
### <a name="maps-app-uri-schemes"></a>Esquemas de URI do app Mapas

Seu app pode usar os esquemas de URI **bingmaps:**, **ms-drive-to:** e **ms-walk-to:** para [iniciar o app Mapas do Windows](launch-maps-app.md) para especificar mapas, trajetos e resultados de pesquisa específicos. Por exemplo, o URI a seguir abre o app Mapas do Windows e exibe um mapa centralizado na cidade de Nova York.

`bingmaps:?cp=40.726966~-74.006076`

![um exemplo do app mapas do windows.](images/mapnyc.png)

Para obter mais informações, consulte [Iniciar o app Mapas do Windows](launch-maps-app.md). Para usar o controle de mapa em seu próprio app, consulte [Exibir mapas em modos de exibição 2D, 3D e Streetside](https://msdn.microsoft.com/library/windows/apps/mt219695).
<br>
### <a name="messaging-app-uri-scheme"></a>Esquema de URI do app de mensagens

Seu app pode usar os esquemas de URI **ms-chat:** para iniciar o app Mensagens do Windows.

| Esquema de URI |Resultados | |-- ---------|--------| | ms-chat:   | Inicia o app Mensagens. | | ms-chat:?ContactID={contacted}  |  Permite que o app de mensagens seja iniciado com informações de um contato específico.   | | ms-chat:?Body={body} | Permite que o app Mensagens seja iniciado com uma cadeia de caracteres a ser usada como o conteúdo da mensagem.| | ms-chat:?Addresses={address}&Body={body} | Permite que o app de mensagens seja iniciado com informações de um determinado endereço e com uma cadeia de caracteres a ser usada como o conteúdo da mensagem. Observação: os endereços podem ser concatenados. | | ms-chat:?TransportId={transportId}  | Permite que o app de mensagens seja iniciado com uma ID de transporte específica. |
<br>
### <a name="tone-picker-uri-scheme"></a>Esquema de URI de seletor de tom

Seu app pode usar o esquema de URI **ms-tonepicker:** para escolher toques, alarmes e tons de sistema. Você também pode salvar novos toques e obter o nome de exibição de um tom.

| Esquema de URI | Resultados |
|------------|---------|
| ms-tonepicker: | Escolha toques, alarmes e tons de sistema. |

Os parâmetros são transmitidos por meio de um [ValueSet](https://msdn.microsoft.com/library/windows/apps/windows.foundation.collections.valueset.aspx) para a API LaunchURI. Consulte [Escolher e salvar tons usando o esquema de URI ms-tonepicker](launch-ringtone-picker.md) para obter detalhes.

### <a name="nearby-numbers-app-uri-scheme"></a>Esquema de URI do app Números nas Proximidades
<br>
Seu app pode usar o esquema de URI **ms-yellowpage:** para iniciar o app Números nas Proximidades.

| Esquema de URI | Resultados |
|------------|---------|
| ms-yellowpage:?input=\[keyword\]&method=\[String or T9\] | Inicia o app Números nas Proximidades. `input` Refere-se à palavra-chave que você deseja pesquisar. `method` Refere-se ao tipo de pesquisa (cadeia de caracteres ou pesquisa T9). <br> Se `method` for `T9` (um tipo de teclado), `keyword` deverá ser uma cadeia de caracteres numérica que mapeia para as letras de teclado T9 a serem pesquisadas.<br>Se `method` for `String`, `keyword` será a palavra-chave a ser pesquisada. |
 
<br>
### <a name="people-app-uri-scheme"></a>Esquema de URI do app Pessoas

Seu app pode usar o esquema de URI **ms-people:** para iniciar o app Pessoas.
Para obter mais informações, consulte [Iniciar o app Pessoas](launch-people-apps.md).

<br>
### <a name="settings-app-uri-scheme"></a>Esquema de URI do app Configurações

Seu app pode usar o esquema de URI **ms-settings:** para [iniciar o app Configurações do Windows](launch-settings-app.md). A inicialização do app Configurações é uma parte importante da escrita de um app com reconhecimento de privacidade. Se seu app não pode acessar um recurso confidencial, é recomendável fornecer ao usuário um link conveniente para as configurações de privacidade desse recurso. Por exemplo, o URI a seguir abre o app Configurações e exibe as configurações de privacidade da câmera.

`ms-settings:privacy-webcam`

![configurações de privacidade da câmera.](images/privacyawarenesssettingsapp.png)

Para obter mais informações, consulte [Iniciar o app Configurações do Windows](launch-settings-app.md) e [Diretrizes de apps com reconhecimento de privacidade](https://msdn.microsoft.com/library/windows/apps/hh768223).

<br>
### <a name="store-app-uri-scheme"></a>Esquema de URI do app da Loja

Seu app pode usar o esquema de URI **ms-windows-store:** para [Iniciar o aplicativo da Windows Store](launch-store-app.md). Abra páginas de detalhes do produto, páginas de análise do produto, páginas de pesquisa etc. Por exemplo, o URI a seguir abre o aplicativo da Windows Store e inicia a página inicial da Loja.

`ms-windows-store://home/`

Para obter mais informações, consulte [Iniciar o aplicativo da Windows Store](launch-store-app.md).

