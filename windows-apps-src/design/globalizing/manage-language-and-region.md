---
Description: Este tópico define a lista de idiomas do perfil de usuário de termos, lista de idiomas de manifesto de aplicativo e lista de idiomas de tempo de execução do aplicativo. Vamos usar esses termos neste tópico e em outros tópicos nessa área de recurso, por isso, é importante saber o que significam.
title: Noções básicas sobre idiomas de perfil de usuário e idiomas de manifesto do app
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
template: detail.hbs
ms.date: 11/08/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: d782e8cd64cb976df964c72199964c1d349d527e
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57605571"
---
# <a name="understand-user-profile-languages-and-app-manifest-languages"></a>Noções básicas sobre idiomas de perfil de usuário e idiomas de manifesto do app
Um usuário do Windows pode usar **Configurações** > **Hora e Idioma** > **Região e idioma** para configurar uma lista ordenada de idiomas de preferência de exibição, ou um único idioma de preferência de exibição. Um idioma pode ter uma variante regional. Por exemplo, você pode selecionar o espanhol falado na Espanha, o espanhol falado no México, o espanhol falado nos Estados Unidos, entre outros.

Além disso, em **Configurações** > **Hora e idioma** > **Região e idioma**, mas separado do idioma, o usuário pode especificar sua localização (conhecida como região) no mundo. Observe que a configuração do idioma de exibição (e a variante regional) não é determinante da configuração de região, e vice-versa. Por exemplo, um usuário pode estar vivendo atualmente na França, mas escolher como idioma de exibição preferencial do Windows o espanhol (México).

Para aplicativos UWP, um idioma é representado como uma [marca de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302). Por exemplo, a marca de idioma BCP-47 "en-US" corresponde ao inglês (Estados Unidos) em **Configurações**. APIs UWP apropriadas aceitam e retornam representações em cadeia de marcas de idioma BCP-47.

Veja também o [Registro da submarca de idioma IANA](https://go.microsoft.com/fwlink/p/?linkid=227303).

As três seções a seguir definem os termos "lista de idiomas do perfil do usuário", "lista de idiomas de manifesto do app" e "lista de idiomas de tempo de execução do aplicativo". Vamos usar esses termos neste tópico e em outros tópicos nessa área de recurso, por isso, é importante saber o que significam.

## <a name="user-profile-language-list"></a>Lista de idiomas do perfil do usuário
A lista de idiomas do perfil do usuário é o nome da lista que está configurada pelo usuário em **Configurações** > **Hora e idioma** > **Região e idioma** > **Idiomas**. No código, você pode usar a propriedade [**GlobalizationPreferences.Languages**](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages) para acessar a lista de idiomas do perfil do usuário como lista somente de leitura de cadeias de caracteres, em que cada cadeia é uma única [marca de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302), como "en-US" ou "ja-JP".

```csharp
    IReadOnlyList<string> userLanguages = Windows.System.UserProfile.GlobalizationPreferences.Languages;
```

## <a name="app-manifest-language-list"></a>Lista de idiomas de manifesto do app
A lista de idiomas de manifesto do app é a lista de idiomas para os quais o seu app declara (ou irá declarar) suporte. Essa lista cresce à medida que você progride no app pelo ciclo de vida de desenvolvimento até a localização.

A lista é determinada no tempo de compilação, mas você tem duas opções de controlar exatamente como isso acontece. Uma opção é permitir que o Visual Studio determine a lista entre os arquivos no seu projeto. Para fazer isso, primeiro defina o **Idioma padrão** do seu app na guia **Aplicativo** no seu arquivo de origem do manifesto do pacote do aplicativo(`Package.appxmanifest`). Em seguida, confirme se o mesmo arquivo contém essa configuração (que ocorre por padrão).

```xml
  <Resources>
    <Resource Language="x-generate" />
  </Resources>
```

Cada vez que o Visual Studio gera seu arquivo de manifesto do pacote do app (`AppxManifest.xml`), ele expande esse único elemento `Resource` do arquivo de origem em uma união de todos os qualificadores de idiomas encontrados no seu projeto (veja [Personalizar os recursos de acordo com idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)). Por exemplo, se você já começou a localização e possui cadeia de caracteres, imagem e/ou recursos de arquivo cujos nomes de pastas ou arquivos incluem "en-US", "ja-JP" e "fr-FR", seu arquivo `AppxManifest.xml` criado conterá o seguinte (a primeira entrada na lista é o idioma padrão que você definir).

```xml
  <Resources>
    <Resource Language="EN-US" />
    <Resource Language="JA-JP" />
    <Resource Language="FR-FR" />
  </Resources>
```

A outra opção é substituir o único elemento "x-generate" `<Resource>` em seu arquivo de origem do manifesto do pacote do aplicativo (`Package.appxmanifest`) pela lista expandida de elementos `<Resource>` (tendo o cuidado de listar o idioma padrão primeiro). Essa opção envolve mais trabalho de manutenção, mas pode ser uma opção apropriada para você se você usa um sistema de compilação personalizado.

Em primeiro lugar, sua lista de idiomas de manifesto do app conterá somente um idioma. Talvez seja en-US. No entanto, por fim, &mdash;conforme você configurar manualmente seu manifesto, ou conforme você adicionar recursos traduzidos ao seu projeto&mdash; essa lista crescerá.

Quando seu app está na Microsoft Store, os idiomas na lista de idiomas de manifesto do app são os exibidos aos clientes. Para uma lista de marcas de idioma BCP-47 com suporte, especificamente, pela Microsoft Store, consulte [Idiomas com suporte](../../publish/supported-languages.md).

No código, você pode usar a propriedade [**ApplicationLanguages.ManifestLanguages**](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages) para acessar a lista de idiomas de manifesto do app como lista somente de leitura de cadeias de caracteres, em que cada cadeia é uma única marca de idioma BCP-47.

```csharp
    IReadOnlyList<string> userLanguages = Windows.Globalization.ApplicationLanguages.ManifestLanguages;
```

## <a name="app-runtime-language-list"></a>Lista de idiomas de tempo de execução do app
A lista de interesse do terceiro idioma é a interseção entre as duas listas que acabamos de descrever. No tempo de execução, a lista de idiomas para os quais o app tiver declarado suporte (a lista de idiomas de manifesto do app) é comparada à lista de idiomas para o qual o usuário declarou preferência (a lista de idiomas do perfil do usuário). A lista de idiomas de tempo de execução do app é definida para essa interseção (se a intersecção não estiver vazia), ou apenas para o idioma padrão do app (se a intersecção estiver vazia).

Mais especificamente, a lista de idiomas do tempo de execução do app é composta por esses itens.

1.  **Substituição do Idioma Principal (Opcional)**. A [**PrimaryLanguageOverride**](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride) é uma configuração de substituição simples para aplicativos que permitirão aos usuários fazer sua própria escolha de idioma independente ou para aplicativos que, por motivos realmente importantes, substituirão as opções de idioma padrão. Para obter mais informações, consulte o [Exemplo de recursos e localização de aplicativos](https://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Os idiomas do usuário com suporte pelo aplicativo**. Esta é a lista de idiomas do perfil do usuário filtrada pela lista de idiomas de manifesto do app. Filtrar os idiomas do usuário por aqueles suportados pelo aplicativo mantém a consistência entre os SDKs (software development kits), bibliotecas de classes, pacotes de estrutura dependente e o aplicativo.
3.  **Se 1 e 2 estão vazios, o padrão ou o primeiro idioma suportado pelo app**. Se a lista de idiomas do perfil do usuário não contém nenhum idioma com suporte pelo aplicativo, o idioma de tempo de execução do app será o primeiro idioma suportado pelo aplicativo.

No código, você pode usar a propriedade [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) para acessar a lista de idiomas de tempo de execução do app na forma de cadeia contendo uma lista delimitada por ponto e vírgula de marcas de idioma BCP-47.

```csharp
    string runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().QualifierValues["Language"];
```

Você também pode acessá-lo como lista somente leitura de cadeias de caracteres, cada um contendo uma única marca de idioma BCP-47. Você pode usar a propriedade [**ResourceContext.Languages**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages) ou [**ApplicationLanguages.Languages**](/uwp/api/windows.globalization.applicationlanguages.Languages) para fazer isso.

```csharp
    IReadOnlyList<string> runtimeLanguages = Windows.ApplicationModel.Resources.Core.ResourceContext.GetForCurrentView().Languages;

    runtimeLanguages = Windows.Globalization.ApplicationLanguages.Languages;
```

A lista de idiomas do tempo de execução do app determina os recursos que o Windows carrega no seu app, além do(s) idioma(s) usado(s) para formatar as datas, as horas e os números, além de outros componentes. Consulte [Globalize seus formatos data/hora/número](use-global-ready-formats.md).

**Observação** se o idioma do perfil do usuário e o idioma de manifesto do app são variantes regionais um do outro, a variante regional do usuário é usada como idioma de tempo de execução do app. Por exemplo, se o usuário prefere en-GB e o app der suporte a en-US, o idioma de tempo de execução do app será en-GB. Isso garante que as datas, as horas e os números sejam formatados da maneira mais próxima às expectativas do usuário (en-GB), mas os recursos localizados ainda sejam carregados (devido à correspondência do idioma) no idioma do aplicativo com suporte (en-US).

## <a name="qualify-resource-files-with-their-language"></a>Qualificar arquivos de recurso com o respectivo idioma
Nomeie seus arquivos de recurso ou suas pastas, com qualificadores de recursos de idiomas. Para aprender mais sobre qualificadores de recursos, consulte [Personalizar os recursos de acordo com idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)). Um arquivo de recurso pode ser uma imagem (ou outro ativo), ou ele pode ser um arquivo de contêiner de recurso, como um *. resw* que contém cadeias de caracteres de texto.

**Observação** até mesmo os recursos de linguagem padrão do seu aplicativo devem especificar o qualificador de idioma. Por exemplo, se o idioma padrão de seu aplicativo for inglês (Estados Unidos), em seguida, qualificar seus ativos como `\Assets\Images\en-US\logo.png`.

- Windows executa a correspondência complexas, incluindo entre variantes regionais como en-US e en-GB. Portanto, inclua a marca de subpropriedades de região conforme apropriado. Confira [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](../../app-resources/how-rms-matches-lang-tags.md).
- Especifique uma marca de subpropriedades de script de idioma no qualificador quando não há nenhum valor de suprimir Script definido para o idioma. Por exemplo, em vez de zh-CN ou zh-TW, usar zh-Hant, zh-Hant-TW ou zh-Hans (para obter mais detalhes, consulte o [registro de submarca de idioma IANA](https://go.microsoft.com/fwlink/p/?linkid=227303)).
- Para idiomas que têm um dialeto padrão único, não é necessário incluir o qualificador de região. Por exemplo, use ja em vez de ja-JP.
- Algumas ferramentas e outros componentes como os tradutores automáticos podem encontrar marcas de idioma específicas, como informações de dialetos regionais, que são úteis para entender os dados.

### <a name="not-all-resources-need-to-be-localized"></a>Nem todos os recursos precisam ser localizados

Localização não pode ser necessária para todos os recursos.

- No mínimo, verifique se que todos os recursos existem no idioma padrão.
- Um subconjunto de alguns recursos talvez seja suficiente para uma linguagem intimamente relacionada (localização parcial). Por exemplo, você pode não localizar toda interface do usuário do app para catalão se o seu app tem um conjunto completo de recursos em espanhol. Para usuários que falam catalão e, em seguida, espanhol, os recursos que não estão disponíveis em catalão aparecem em espanhol.
- Alguns recursos podem exigir exceções para idiomas específicos, enquanto a maioria dos outros recursos são mapeados para um recurso comum. Nesse caso, marque o recurso deve ser usado para todos os idiomas com a marca de idioma indeterminado 'und'. Windows interpreta a marca de idioma 'und' como um caractere curinga (semelhante a '\*') em que ele corresponda ao idioma do aplicativo superior após qualquer outra correspondência específica. Por exemplo, se alguns recursos forem diferentes para finlandês, mas o restante dos recursos for o mesmo para todos os idiomas, o recurso do finlandês deverá ser marcado com a marca do idioma finlandês, e o restante deverá ser marcado com 'und'.
- Para recursos com base em um script de idioma, como uma fonte ou a altura do texto, usam a marca de idioma indeterminado com um script especificado: ' - und&lt;script&gt;'. Por exemplo, para fontes latinas, use `und-Latn\\fonts.css` e para fontes cirílicas use `und-Cryl\\fonts.css`.

## <a name="set-the-http-accept-language-request-header"></a>Defina o cabeçalho da solicitação HTTP Accept-Language
Considere se os serviços Web usados têm a mesma extensão de localização do seu app. As solicitações de HTTP feitas a partir de aplicativos UWP e aplicativos de Área de Trabalho em solicitações de Web típicas e XMLHttpRequest (XHR), use o cabeçalho da solicitação HTTP Accept-Language padrão. Por padrão, o cabeçalho HTTP é definido para a lista de idiomas do perfil do usuário. Cada idioma da lista é expandido ainda mais para incluir neutralidades de idioma e pesos (q). Por exemplo, a lista de idiomas de um usuário consistindo em fr-FR e en-US resulta em um cabeçalho da solicitação HTTP Accept-Language de fr-FR, fr, en-US, en ("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3"). Entretanto, se o seu app de condições climáticas (por exemplo) estiver exibindo uma interface do usuário em francês (França), mas o idioma principal do usuário na lista de preferências for alemão, será necessário solicitar o serviço explicitamente em francês (França) para que permaneça consistente com o seu app.

## <a name="apis-in-the-windowsglobalization-namespace"></a>APIs no namespace Windows.Globalization
Normalmente, as APIs no namespace [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live) usam a lista de idiomas do tempo de execução do app para determinar o idioma. Se nenhum dos idiomas tiver um formato de correspondência, a localidade do usuário será usada. Esta é a mesma localidade usada no relógio do sistema. A localidade do usuário está disponível no **as configurações** > **hora e idioma** > **região e idioma**  >  **Adicional data, hora e configurações regionais** > **região: Alterar a data, hora ou formatos de número**. As APIs **Windows.Globalization** também têm substituições para especificar uma lista de idiomas a ser usada em vez da lista de idiomas do tempo de execução do app.

Usando a classe [**Idioma**](/uwp/api/windows.globalization.language?branch=live), você pode inspecionar detalhes sobre um idioma específico, como o script do idioma, o nome de exibição e o nome nativo.

## <a name="use-geographic-region-when-appropriate"></a>Use a região geográfica quando adequado
Em **Configurações** > **Hora e idioma** > **Região e idioma** > **País ou região**, o usuário pode especificar a localização. Você pode usar essas configurações, em vez do idioma, para escolher o conteúdo a ser exibido para o usuário. Por exemplo, um app de notícias pode exibir como padrão o conteúdo dessa região.

No código, você pode acessar essa configuração usando a propriedade [**GlobalizationPreferences.HomeGeographicRegion**](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion).

Usando a classe [**GeographicRegion**](/uwp/api/windows.globalization.geographicregion?branch=live), você pode inspecionar os detalhes sobre uma região específica, como o nome de exibição, o nome nativo e as moedas em uso.

## <a name="examples"></a>Exemplos
A seguinte tabela contém exemplos do que o usuário veria na sua interface do usuário do app em várias configurações de idioma e região.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Lista de idiomas de manifesto do app</th>
<th align="left">Lista de idiomas do perfil do usuário</th>
<th align="left">Substituição do idioma principal do aplicativo (opcional)</th>
<th align="left">Lista de idiomas de tempo de execução do app</th>
<th align="left">O que o usuário vê no aplicativo</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Inglês (GB) (padrão); Alemão (Alemanha)</td>
<td align="left">Inglês (GB)</td>
<td align="left">nenhuma</td>
<td align="left">Inglês (GB)</td>
<td align="left">INTERFACE DO USUÁRIO: Inglês (GB)<br>As datas/horas/números: Inglês (GB)</td>
</tr>
<tr>
<td align="left">Alemão (Alemanha) (padrão); Francês (França); Italiano (Itália)</td>
<td align="left">Francês (Áustria)</td>
<td align="left">nenhuma</td>
<td align="left">Francês (Áustria)</td>
<td align="left">INTERFACE DO USUÁRIO: Francês (França) (fallback de francês (Áustria))<br>As datas/horas/números: Francês (Áustria)</td>
</tr>
<tr>
<td align="left">Inglês (EUA) (padrão); Francês (França); Inglês (Reino Unido)</td>
<td align="left">Inglês (Canadá); Francês (Canadá)</td>
<td align="left">nenhuma</td>
<td align="left">Inglês (Canadá); Francês (Canadá)</td>
<td align="left">INTERFACE DO USUÁRIO: Inglês (EUA) (fallback de inglês (Canadá))<br>As datas/horas/números: Inglês (Canadá)</td>
</tr>
<tr>
<td align="left">Espanhol (Espanha) (padrão); Espanhol (México); Espanhol (América Latina); Português (Brasil)</td>
<td align="left">Inglês (EUA)</td>
<td align="left">nenhuma</td>
<td align="left">Espanhol (Espanha)</td>
<td align="left">INTERFACE DO USUÁRIO: Espanhol (Espanha) (usa padrão desde nenhum fallback disponível em inglês)<br>Datas/horas/números: Espanhol (Espanha)</td>
</tr>
<tr>
<td align="left">Catalão (padrão); Espanhol (Espanha); Francês (França)</td>
<td align="left">Catalão; Francês (França)</td>
<td align="left">nenhuma</td>
<td align="left">Catalão; Francês (França)</td>
<td align="left">INTERFACE DO USUÁRIO: Principalmente catalão e alguns francês (França) porque nem todas as cadeias de caracteres estão em catalão<br>As datas/horas/números: Catalão</td>
</tr>
<tr>
<td align="left">Inglês (Reino Unido) (padrão); Francês (França); Alemão (Alemanha)</td>
<td align="left">Alemão (Alemanha); Inglês (Reino Unido)</td>
<td align="left">Inglês (Reino Unido) (escolhido pelo usuário na interface do usuário do aplicativo)</td>
<td align="left">Inglês (Reino Unido); Alemão (Alemanha)</td>
<td align="left">INTERFACE DO USUÁRIO: Inglês (GB) (substituição de idioma)<br>Datas/horas/números: Inglês (GB)</td>
</tr>
</tbody>
</table>

>[!NOTE]
> Para obter uma lista dos códigos de país/região padrão usadas pela Microsoft, consulte o [lista oficial de país/região](https://globalready.azurewebsites.net/marketreadiness/OfficialCountryregion).

## <a name="important-apis"></a>APIs Importantes
* [GlobalizationPreferences.Languages](/uwp/api/windows.system.userprofile.globalizationpreferences.Languages)
* [ApplicationLanguages.ManifestLanguages](/uwp/api/windows.globalization.applicationlanguages.ManifestLanguages)
* [PrimaryLanguageOverride](/uwp/api/Windows.Globalization.ApplicationLanguages.PrimaryLanguageOverride)
* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [ResourceContext.Languages](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.Languages)
* [ApplicationLanguages.Languages](/uwp/api/windows.globalization.applicationlanguages.Languages)
* [Windows.Globalization](/uwp/api/windows.globalization?branch=live)
* [Idioma](/uwp/api/windows.globalization.language?branch=live)
* [GlobalizationPreferences.HomeGeographicRegion](/uwp/api/windows.system.userprofile.globalizationpreferences.HomeGeographicRegion)
* [GeographicRegion](/uwp/api/windows.globalization.geographicregion?branch=live)

## <a name="related-topics"></a>Tópicos relacionados
* [Marca de idioma BCP-47](https://go.microsoft.com/fwlink/p/?linkid=227302)
* [Registro de submarca de idioma IANA](https://go.microsoft.com/fwlink/p/?linkid=227303)
* [Personalizar os recursos de idioma, escala, alto contraste e outros qualificadores](../../app-resources/tailor-resources-lang-scale-contrast.md)
* [Idiomas com suporte](../../publish/supported-languages.md)
* [Globalizar seus formatos de data/hora/número](use-global-ready-formats.md)
* [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](../../app-resources/how-rms-matches-lang-tags.md)

## <a name="samples"></a>Exemplos
* [Exemplo de localização e recursos de aplicativo](https://go.microsoft.com/fwlink/p/?linkid=231501)
