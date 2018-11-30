---
Description: This topic explains the general concept of qualifiers, how to use them, and the purpose of each of the qualifier names.
title: Personalizar os recursos de idioma, escala, alto contraste e outros qualificadores
template: detail.hbs
ms.date: 10/10/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: ac61d57a965e3a35c6eb7cfaf17d0f4ef2a02501
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8211090"
---
# <a name="tailor-your-resources-for-language-scale-high-contrast-and-other-qualifiers"></a>Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores

Este tópico descreve o conceito geral dos qualificadores de recurso, como usá-los e a finalidade de cada nome de qualificador. Consulte [**ResourceContext.QualifierValues**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues) para obter uma tabela de referência de todos os valores de qualificador possíveis.

Seu app pode carregar ativos e recursos que são personalizados com base em contextos de tempo de execução, como idioma, alto contraste [exibir fator de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor) e muitos outros. Para isso, nomeie as pastas ou os arquivos dos recursos de modo que correspondam aos nomes e valores de qualificador desses contextos. Por exemplo, talvez seja necessário que o app carregue um conjunto diferente de ativos de imagem no modo de alto contraste.

Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md).

## <a name="qualifier-name-qualifier-value-and-qualifier"></a>Nome do qualificador, valor do qualificador e qualificador

Um nome de qualificador é uma chave mapeada para um conjunto de valores de qualificador. Estes são o nome e os valores de qualificador para contraste.

| Contexto | Nome de qualificador | Valores de qualificador |
| :--------------- | :--------------- | :--------------- |
| A configuração de alto contraste | contraste | padrão, alto, preto e branco |

Você pode combina um nome de qualificador com um valor de qualificador para formar um qualificador. `<qualifier name>-<qualifier value>` é o formato de um qualificador. `contrast-standard` é um exemplo de qualificador.

Portanto, para alto contraste, o conjunto de qualificadores é `contrast-standard`, `contrast-high`, `contrast-black` e `contrast-white`. Os nomes e os valores de qualificador não diferenciam maiúsculas de minúsculas. Por exemplo, `contrast-standard` e `Contrast-Standard` são o mesmo qualificador.

## <a name="use-qualifiers-in-folder-names"></a>Usar qualificadores em nomes de pasta

Este é um exemplo do uso de qualificadores para nomear pastas que contêm arquivos de ativo. Use qualificadores em nomes de pasta, se você tiver vários arquivos de ativo por qualificador. Dessa forma, você definirá o qualificador uma vez no nível da pasta, e o qualificador aplicará isso a tudo que estiver contido na pasta.

```console
\Assets\Images\contrast-standard\<logo.png, and other image files>
\Assets\Images\contrast-high\<logo.png, and other image files>
\Assets\Images\contrast-black\<logo.png, and other image files>
\Assets\Images\contrast-white\<logo.png, and other image files>
```

Se você nomear as pastas como no exemplo acima, o app usará a configuração de alto contraste para carregar arquivos de recurso na pasta nomeada para o qualificador apropriado. Portanto, se a configuração for Preto em Alto Contraste, os arquivos de recurso na pasta `\Assets\Images\contrast-black` serão carregados. Se a configuração for Nenhum (ou seja, se o computador não estiver no modo de alto contraste), os arquivos de recurso na pasta `\Assets\Images\contrast-standard` serão carregados.

## <a name="use-qualifiers-in-file-names"></a>Usar qualificadores em nomes de arquivo

Em vez de criar e nomear pastas, você pode usar um qualificador para nomear os próprios arquivos de recurso. Talvez você prefira fazer isso se tiver apenas um arquivo de recurso por qualificador. Veja um exemplo a seguir.

```console
\Assets\Images\logo.contrast-standard.png
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.contrast-black.png
\Assets\Images\logo.contrast-white.png
```

O arquivo cujo nome contém o qualificador mais apropriado para a configuração é o que será carregado. Essa lógica de correspondência funciona da mesma forma para nomes de arquivo e nomes de pasta.

## <a name="reference-a-string-or-image-resource-by-name"></a>Referência a um recurso de cadeia de caracteres ou imagem pelo nome

Consulte [Fazer referência a um identificador de recurso de cadeia de caracteres na marcação XAML](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-xaml-markup), [Fazer referência a um identificador de recurso de cadeia de caracteres no código](localize-strings-ui-manifest.md#refer-to-a-string-resource-identifier-from-code) e [Fazer referência a uma imagem ou outro ativo no código e na marcação XAML](images-tailored-for-scale-theme-contrast.md#reference-an-image-or-other-asset-from-xaml-markup-and-code).

## <a name="actual-and-neutral-qualifier-matches"></a>Correspondências de qualificador reais e neutras
Você não precisa fornecer um arquivo de recurso para *cada* valor de qualificador. Por exemplo, se você achar que só precisa de um ativo visual para alto contraste e outro para o contraste padrão, nomeie esses ativos da seguinte forma.

```console
\Assets\Images\logo.contrast-high.png
\Assets\Images\logo.png
```

O primeiro nome de arquivo contém o qualificador `contrast-high`. Esse qualificador é uma correspondência *real* para qualquer configuração de alto contraste quando o alto contraste está *ativado*. Em outras palavras, é uma correspondência aproximada e, portanto, preferencial. Uma correspondência *real* só ocorrerá se o qualificador contiver um valor *real*, como este. Neste caso, `high` é um valor *real* para `contrast`.

O arquivo chamado `logo.png` não contém nenhum qualificador de contraste. A ausência de um qualificador é um valor *neutro*. Se nenhuma correspondência preferencial for encontrada, o valor neutro servirá como uma correspondência de fallback. Neste exemplo, se o alto contraste for *desativado*, não há nenhuma correspondência real. A correspondência *neutra* é a melhor correspondência que pode ser encontrada e, portanto, o ativo `logo.png` será carregado.

Se você alterar o nome do `logo.png` para `logo.contrast-standard.png`, o nome do arquivo conterá um valor de qualificador real. Com o alto contraste desativado, haverá uma correspondência real com `logo.contrast-standard.png`, que é o arquivo de ativo a ser carregado. Assim, os mesmos arquivos serão carregados, nas mesmas condições, mas devido às diferentes correspondências.

Se você só precisar de um conjunto de ativos para alto contraste e outro para contraste padrão, poderá usar nomes de pasta, em vez de nomes de arquivo. Nesse caso, a omissão do nome da pasta resultará em uma correspondência neutra.

```console
\Assets\Images\contrast-high\<logo.png, and other images to load when high contrast theme is not None>
\Assets\Images\<logo.png, and other images to load when high contrast theme is None>
```

Para obter mais detalhes sobre como funciona a correspondência de qualificador, consulte [Sistema de Gerenciamento de Recursos](resource-management-system.md).

## <a name="multiple-qualifiers"></a>Vários qualificadores

Você pode combinar qualificadores em nomes de pasta e de arquivo. Por exemplo, convém que o app carregue ativos de imagem quando o modo de alto contraste estiver ativado *e* o fator de escala de exibição for 400. Uma maneira de fazer isso é por meio de pastas aninhadas.

```console
\Assets\Images\contrast-high\scale-400\<logo.png, and other image files>
```

Para que `logo.png` e outros arquivos sejam carregados, as configurações devem corresponder a *ambos* os qualificadores.

Outra opção é combinar vários qualificadores em um único nome de pasta.

```console
\Assets\Images\contrast-high_scale-400\<logo.png, and other image files>
```

Em um nome de pasta, combine vários qualificadores separados com um sublinhado. `<qualifier1>[_<qualifier2>...]` é o formato.

Você pode combinar vários qualificadores em um nome de arquivo no mesmo formato.

```console
\Assets\Images\logo.contrast-high_scale-400.png
```

Dependendo das ferramentas e do fluxo de trabalho usado na criação de ativos ou do que você achar mais fácil de ler e/ou gerenciar, escolha uma única estratégia de nomenclatura para todos os qualificadores ou combine-os para diferentes qualificadores.

## <a name="alternateform"></a>AlternateForm

O qualificador `alternateform` é usado para oferecer uma forma alternativa de recurso para uma finalidade específica. Isso normalmente é usado apenas por desenvolvedores de apps japoneses para fornecer uma cadeia de caracteres furigana cujo valor `msft-phonetic` seja reservado (consulte a seção "Suporte a Furigana em cadeias de caracteres japonesas que podem ser classificadas" em [Como se preparar para a localização](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)).

O sistema de destino ou o app deve fornecer um valor que servirá de base para a correspondência dos qualificadores `alternateform`. Não use o prefixo `msft-` em seus próprios valores de qualificador `alternateform` personalizados.

## <a name="configuration"></a>Configuração

Dificilmente você precisará do nome de qualificador `configuration`. Ele pode ser usado para especificar os recursos aplicáveis apenas a um determinado ambiente em tempo de criação, como recursos somente de teste.

O qualificador `configuration` é usado para carregar um recurso que melhor corresponda ao valor da variável de ambiente `MS_CONFIGURATION_ATTRIBUTE_VALUE`. Portanto, você pode definir a variável para o valor de cadeia de caracteres atribuído aos recursos relevantes; por exemplo, `designer` ou `test`.

## <a name="contrast"></a>Contraste

O qualificador `contrast` é usado para fornecer os recursos que melhor correspondam às configurações de alto contraste.

## <a name="custom"></a>Personalizado

O app pode definir um valor para o qualificador `custom`; em seguida, os recursos que melhor correspondem a esse valor serão carregados. Por exemplo, convém carregar os recursos com base na licença do app. Quando o app for iniciado, ele verificará a respectiva licença e a usará como o valor para o qualificador `custom` chamando [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_), conforme mostrado no exemplo de código.

```csharp
public void SetLicenseLevel(BrandID brand)
{
    if (brand == BrandID.Premium)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Premium", ResourceQualifierPersistence.LocalMachine);
    }
    else if (brand == BrandID.Standard)
    {
        ResourceContext.SetGlobalQualifierValue("Custom", " Standard", ResourceQualifierPersistence.LocalMachine);
    }
    else
    {
        ResourceContext.SetGlobalQualifierValue("Custom", "Trial", ResourceQualifierPersistence.LocalMachine);
    }
}
```

Nesse cenário, você informará os nomes de recursos que incluem os qualificadores `custom-premium`, `custom-standard` e `custom-trial`.

## <a name="devicefamily"></a>DeviceFamily

Dificilmente você precisará do nome de qualificador `devicefamily`. Você pode e deve evitar usá-lo sempre que possível, pois existem técnicas que você pode usar no lugar dele que são muito mais convenientes e eficazes. Essas técnicas são descritas em [Detectando a plataforma em que o app está sendo executado](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on) e [Código adaptável da versão](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).

Mas, como último recurso, é possível usar os qualificadores devicefamily para nomear pastas que armazenarão as exibições XAML (uma exibição XAML é um arquivo XAML que contém controles e layout de interface do usuário).

```console
\devicefamily-desktop\<MainPage.xaml, and other markup files to load when running on a desktop computer>
\devicefamily-mobile\<MainPage.xaml, and other markup files to load when running on a phone>
```

Outra alternativa é nomear os arquivos.

```console
\MainPage.devicefamily-desktop.xaml
\MainPage.devicefamily-mobile.xaml
```

Em ambos os casos, cada cópia de `MainPage.[<qualifier>].xaml` compartilha um `MainPage.xaml.cs` comum, que permanece com nome, local e conteúdo inalterados no projeto.

Você também pode usar um qualificador devicefamily para nomear um arquivo de recursos (`.resw`) ou uma pasta. Por exemplo, quando o app estiver em execução na família de dispositivos móveis, o elemento de interface do usuário `<TextBlock x:Uid="DeviceFriendlyName"/>` usará os recursos de texto e de primeiro plano definidos no arquivo `Resources.devicefamily-mobile.resw`, se ele contiver

```xml
<data name="DeviceFriendlyName.Foreground">
    <value>Red</value>
</data>
<data name="DeviceFriendlyName.Text">
    <value>Mobile device</value>
</data>
```

Para obter mais informações sobre como usar um arquivo de recursos, consulte [Localizar as cadeias de caracteres da interface do usuário](localize-strings-ui-manifest.md).

## <a name="dxfeaturelevel"></a>DXFeatureLevel

Dificilmente você precisará do nome de qualificador `dxfeaturelevel`. Ele foi projetado para ser usado com ativos de jogo do Direct3D, para fazer com que os recursos de versão anterior sejam carregados para que correspondam a uma configuração de hardware de versão anterior específica do tempo. Mas a prevalência dessa configuração de hardware está tão lenta agora que recomendamos que você não use este qualificador.

## <a name="homeregion"></a>HomeRegion

O qualificador `homeregion` corresponde à configuração do usuário referente ao país ou à região. Ele representa o local de residência do usuário. Os valores incluem qualquer [marca de região BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) válida. Ou seja, qualquer código de região de duas letras **ISO 3166-1 alpha-2**, além do conjunto de códigos geográficos de três dígitos **ISO 3166-1 numeric** para regiões compostas (consulte [composição M49 de códigos de região da Divisão de Estatísticas das Nações Unidas](http://go.microsoft.com/fwlink/p/?linkid=247929)). Códigos para "Indicadores econômicos selecionados e outros agrupamentos" não são válidos.

## <a name="language"></a>Idioma

O qualificador `language` corresponde à configuração de idioma de exibição. Os valores incluem qualquer [marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) válida. Para obter uma lista de idiomas, consulte [Registro da submarca de idioma IANA](http://go.microsoft.com/fwlink/p/?linkid=227303).

Se você deseja que o app ofereça suporte a diferentes idiomas de exibição, e você tiver literais de cadeia de caracteres no código ou na marcação XAML, retire essas cadeias de caracteres do código/marcação e insira-as em um arquivo de recursos (`.resw`). Em seguida, você poderá fazer uma cópia traduzida desse arquivo de recursos para cada idioma ao qual o app ofereça suporte.

Você normalmente usa um qualificador `language` para nomear as pastas que contêm os arquivos de recursos (`.resw`).

```console
\Strings\language-en\Resources.resw
\Strings\language-ja\Resources.resw
```

É possível omitir a parte `language-` de um qualificador `language` (ou seja, o nome do qualificador). Você não pode fazer isso com os outros tipos de qualificadores, e você só pode fazer isso em um nome de pasta.

```console
\Strings\en\Resources.resw
\Strings\ja\Resources.resw
```

Em vez de nomear pastas, você poderá usar os qualificadores `language` para nomear os próprios arquivos de recursos.

```console
\Strings\Resources.language-en.resw
\Strings\Resources.language-ja.resw
```

Consulte [Localizar as cadeias de caracteres de interface do usuário](localize-strings-ui-manifest.md) para obter mais informações sobre como tornar o app localizável usando recursos de cadeia de caracteres e como fazer referência a um recurso de cadeia de caracteres no app.

## <a name="layoutdirection"></a>LayoutDirection

O qualificador `layoutdirection` corresponde à direção de layout da configuração de idioma de exibição. Por exemplo, talvez uma imagem precise ser espelhada para um idioma com leitura da direita para a esquerda, como o árabe ou o hebraico. Os painéis e as imagens de layout da interface do usuário responderão à direção do layout adequadamente se você definir a propriedade [FlowDirection](/uwp/api/Windows.UI.Xaml.FrameworkElement.FlowDirection) (consulte [Ajustar layout e fontes e fornecer suporte a RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)). No entanto, o qualificador `layoutdirection` destina-se aos casos em que a simples inversão não é adequada e permite que você responda à direcionalidade da ordem de leitura e do alinhamento de texto de forma mais geral.

## <a name="scale"></a>Escala

O Windows seleciona automaticamente um fator de escala para cada monitor com base em seu DPI (pontos por polegada) e na distância de exibição do dispositivo. Consulte [Pixels efetivos e fator de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor). Você deve criar suas imagens em vários tamanhos recomendados (pelo menos 100, 200 e 400) para que o Windows possa escolher o tamanho perfeito ou pode usar o tamanho mais próximo e dimensioná-las. Para que o Windows possa identificar qual arquivo físico contém o tamanho correto da imagem para o fator de escala de exibição, use um qualificador `scale`. A escala de um recurso corresponde ao valor de [DisplayInformation.ResolutionScale](/uwp/api/windows.graphics.display.displayinformation.ResolutionScale) ou ao próximo recurso com a maior escala.

Aqui está um exemplo de como definir o qualificador no nível da pasta.

```console
\Assets\Images\scale-100\<logo.png, and other image files>
\Assets\Images\scale-200\<logo.png, and other image files>
\Assets\Images\scale-400\<logo.png, and other image files>
```

Este exemplo o define no nível do arquivo.

```console
\Assets\Images\logo.scale-100.png
\Assets\Images\logo.scale-200.png
\Assets\Images\logo.scale-400.png
```

Para obter informações sobre qualificar um recurso para `scale` e `targetsize`, consulte [Qualificar um recurso de imagem para tamanho alvo](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="targetsize"></a>TargetSize

O qualificador `targetsize` é usado basicamente para especificar [ícones de associação de tipo de arquivo](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh127427) ou [ícones de protocolo](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/bb266530) a serem exibidos no Explorador de Arquivos. O valor do qualificador representa o comprimento do lado de uma imagem quadrada em pixels brutos (físicos). O recurso cujo valor coincide com a configuração de exibição no Explorador de Arquivos é carregado ou, na ausência de uma correspondência exata, o recurso com o próximo valor maior é carregado.

Você pode definir os ativos que representam os vários tamanhos do valor de qualificador `targetsize` para o ícone do app (`/Assets/Square44x44Logo.png`) na guia Ativos Visuais do designer de manifesto do pacote do app.

Para obter informações sobre qualificar um recurso para `scale` e `targetsize`, consulte [Qualificar um recurso de imagem para tamanho alvo](images-tailored-for-scale-theme-contrast.md#qualify-an-image-resource-for-targetsize).

## <a name="theme"></a>Tema

O qualificador `theme` é usado para fornecer os recursos que melhor correspondem à configuração de modo de app padrão ou a substituição do app usando [Application.RequestedTheme](/uwp/api/windows.ui.xaml.application?branch=master.RequestedTheme).

## <a name="important-apis"></a>APIs importantes

* [ResourceContext.QualifierValues](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.QualifierValues)
* [SetGlobalQualifierValue](/uwp/api/windows.applicationmodel.resources.core.resourcecontext#Windows_ApplicationModel_Resources_Core_ResourceContext_SetGlobalQualifierValue_System_String_System_String_Windows_ApplicationModel_Resources_Core_ResourceQualifierPersistence_)

## <a name="related-topics"></a>Tópicos relacionados

* [Pixels efetivos e fator de escala](../design/layout/screen-sizes-and-breakpoints-for-responsive-design.md#effective-pixels-and-scale-factor)
* [Sistema de Gerenciamento de Recursos](resource-management-system.md)
* [Como se preparar para a localização](https://msdn.microsoft.com/en-us/library/windows/apps/xaml/hh967762)
* [Detectando a plataforma em que o app está sendo executado](../porting/wpsl-to-uwp-input-and-sensors.md#detecting-the-platform-your-app-is-running-on)
* [Visão geral das famílias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview)
* [Localizar as cadeias de caracteres de interface do usuário](localize-strings-ui-manifest.md)
* [BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Composição M49 de códigos de região da Divisão de Estatísticas das Nações Unidas](http://go.microsoft.com/fwlink/p/?linkid=247929)
* [Registro da submarca de idioma IANA](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Ajustar layout e fontes e fornecer suporte a RTL](../design/globalizing/adjust-layout-and-fonts--and-support-rtl.md)
