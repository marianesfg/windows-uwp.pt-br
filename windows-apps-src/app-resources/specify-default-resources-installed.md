---
author: stevewhims
Description: If your app doesn't have resources that match the particular settings of a customer device, then the app's default resources are used. This topic explains how to specify what those default resources are.
title: Especificar os recursos padrão usados pelo app
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 6f88a6d6be6bf938564f0a31eac118983c256a7e
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1395985"
---
# <a name="specify-the-default-resources-that-your-app-uses"></a>Especificar os recursos padrão usados pelo app

Se o app não tiver recursos que correspondam às configurações específicas de um dispositivo de cliente, os recursos padrão do app serão usados. Este tópico explica como especificar quais são esses recursos padrão.

Quando um usuário instala o app da Microsoft Store, as configurações no dispositivo do cliente são comparadas com os recursos disponíveis do app. Essa comparação é feita para que somente os recursos apropriados precisem ser baixados e instalados para esse usuário. Por exemplo, as cadeias de caracteres e imagens mais apropriadas para as preferências de idioma do usuário e a resolução e as configurações de DPI do dispositivo são usadas. Por exemplo, `200` é o valor padrão para `scale`, mas você pode substituir esse padrão se desejar.

Até mesmo para recursos que não tenham seus próprios pacotes de recursos (como imagens personalizadas para configurações de alto contraste), você poderá especificar quais recursos padrão o app deve usar em tempo de execução se um recurso correspondente às configurações do usuário não puder ser encontrado. Por exemplo, `standard` é o valor padrão para `contrast`, mas você pode substituir esse padrão se desejar.

Esses padrões são especificados sob a forma de valores de qualificador de recurso padrão. Para obter uma explicação do que são os qualificadores de recurso, seu uso e sua finalidade, consulte [Personalizar os recursos de idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md).

Você pode configurar o que são esses padrões de duas maneiras. Você pode adicionar um arquivo de configuração ao projeto ou editar o arquivo de projeto diretamente. Use a alternativa com a qual se sentir mais confortável ou a que funcionar melhor no seu sistema de compilação.

## <a name="option-1-use-priconfigdefaultxml-to-specify-default-qualifier-values"></a>Opção 1. Use priconfig.default.xml para especificar valores de qualificador padrão

1. No Visual Studio, adicione um novo item ao projeto. Escolha Arquivo XML e nomeie o arquivo como `priconfig.default.xml`.
2. No Gerenciador de Soluções, selecione `priconfig.default.xml` e verifique a janela Propriedades. A Ação de Build do arquivo deve ser definida como Nenhuma, enquanto Copy to Output Directory definida como Do not copy.
3. Substitua o conteúdo do arquivo por este XML.
   ```xml
   <default>
      <qualifier name="Language" value="LANGUAGE-TAG(S)" />
      <qualifier name="Contrast" value="standard" />
      <qualifier name="Scale" value="200" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
   
   **Observação** O valor `LANGUAGE-TAG(S)` precisa ser sincronizado com o idioma padrão do app. Se essa for uma única [marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302), o idioma padrão do app precisará ter a mesma marca. Se for uma lista separada por vírgulas de marcas de idioma, o idioma do padrão do app precisará ser a primeira marca na lista. Defina o idioma padrão do app no campo **Idioma Padrão** da guia **Aplicativo** no arquivo de origem do manifesto do pacote de aplicativos (`Package.appxmanifest`).

4. Cada elemento `<qualifier>` informa ao Visual Studio qual valor será usado como padrão para cada nome de qualificador. Com o conteúdo de arquivo que você tem até agora, o comportamento do Visual Studio não foi alterado. Em outras palavras, o Visual Studio *já estava se comportando como se* esse arquivo estivesse presente com esse conteúdo, pois isso é o padrão. Portanto, para substituir um padrão pelo seu próprio valor padrão, você precisará alterar um valor no arquivo. Este é um exemplo da aparência do arquivo se você tiver editado os primeiros três valores.
   ```xml
   <default>
      <qualifier name="Language" value="de-DE" />
      <qualifier name="Contrast" value="black" />
      <qualifier name="Scale" value="400" />
      <qualifier name="HomeRegion" value="001" />
      <qualifier name="TargetSize" value="256" />
      <qualifier name="LayoutDirection" value="LTR" />
      <qualifier name="DXFeatureLevel" value="DX9" />
      <qualifier name="Configuration" value="" />
      <qualifier name="AlternateForm" value="" />
   </default>
   ```
5. Salve e feche o arquivo, e recompile o projeto.

Para confirmar que os padrões substituídos estão sendo levados em consideração, procure o arquivo `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml` e confirme se seu conteúdo corresponde às suas substituições. Em caso afirmativo, você configurou com êxito os valores de qualificador dos recursos que o app usará por padrão. Se não for encontrada uma correspondência para as configurações do usuário, os recursos usados serão aqueles cujos nomes de arquivo ou pasta contenham os valores de qualifer padrão que você definiu aqui.

### <a name="how-does-this-work"></a>Como isso funciona?

Em segundo plano, o Visual Studio inicia uma ferramenta chamada `MakePri.exe` para gerar um arquivo conhecido como índice de recurso do pacote (PRI), que descreve todos os recursos do app, incluindo a indicação de quais são os recursos padrão. Para obter detalhes sobre essa ferramenta, consulte [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md). O Visual Studio passa um arquivo de configuração para o `MakePri.exe`. O conteúdo do arquivo `priconfig.default.xml` é usado como elemento `<default>` desse arquivo de configuração, que é a parte que especifica o conjunto de valores de qualificador considerados como padrão. Assim, a adição e a edição de `priconfig.default.xml` acaba influenciando o conteúdo do arquivo de índice de recurso do pacote que o Visual Studio gera para o app e inclui no pacote de aplicativos.

**Observação** Sempre que você alterar o valor do elemento `<qualifier name="Language" ... />`, precisará sincronizar essa alteração com o idioma padrão do app. Isso acontece para que os recursos de idioma indexados no arquivo PRI do app correspondam ao idioma do padrão do manifesto do app. O valor no elemento `<qualifier name="Language" ... />` substitui o valor no manifesto em relação ao conteúdo do `<ProjectFolder>\obj\<ReleaseConfiguration folder>\priconfig.xml`, mas esse arquivo e o manifesto do app devem ser correspondentes.

### <a name="using-a-different-file-name-than-priconfigdefaultxml"></a>Usando um nome de arquivo diferente de `priconfig.default.xml`

Se você nomear o arquivo `priconfig.default.xml`, o Visual Studio o reconhecerá e o usará automaticamente. Se você atribuir outro nome a ele, será necessário informar o Visual Studio. No arquivo de projeto, entre as marcas de abertura e fechamento do primeiro elemento `<PropertyGroup>`, adicione esse XML.

```xml
<AppxPriConfigXmlDefaultSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlDefaultSnippetPath>
```

Substitua `FILE-PATH-AND-NAME` pelo caminho e nome do arquivo.

## <a name="option-2-use-your-project-file-to-specify-default-qualifier-values"></a>Opção 2. Usar o arquivo de projeto para especificar valores de qualificador padrão

Esta é uma alternativa para a Opção 1. Depois que você entender como funciona a Opção 1, poderá optar pela Opção 2, se ela for mais adequada ao seu fluxo de trabalho de desenvolvimento e/ou compilação.

No arquivo de projeto, entre as marcas de abertura e fechamento do primeiro elemento `<PropertyGroup>`, adicione esse XML.

```xml
<AppxDefaultResourceQualifiers>Language=LANGUAGE-TAG(S)|Contrast=standard|Scale=200|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Este é um exemplo da aparência do arquivo depois que você tiver editado os primeiros três valores.

```xml
<AppxDefaultResourceQualifiers>Language=de-DE|Contrast=black|Scale=400|HomeRegion=001|TargetSize=256|LayoutDirection=LTR|DXFeatureLevel=DX9|Configuration=|AlternateForm=</AppxDefaultResourceQualifiers>
```

Salve e feche o arquivo, e recompile o projeto.

**Observação** Sempre que você alterar o valor `Language=`, precisará sincronizar essa alteração com o idioma padrão do app no designer do manifesto (abrindo `Package.appxmanifest`).

## <a name="related-topics"></a>Tópicos relacionados

* [Personalizar os recursos de idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md)
* [Marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md)
