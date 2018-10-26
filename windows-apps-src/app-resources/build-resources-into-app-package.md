---
author: stevewhims
Description: Some kinds of apps (multilingual dictionaries, translation tools, etc.) need to override the default behavior of an app bundle, and build resources into the app package instead of having them in separate resource packages. This topic explains how to do that.
title: Compilar recursos no pacote de aplicativos, e não em um pacote de recursos
template: detail.hbs
ms.author: stwhi
ms.date: 11/14/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 61b526cd7aa2da8733457b16dd0487ef4ead9cca
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5555772"
---
# <a name="build-resources-into-your-app-package-instead-of-into-a-resource-pack"></a>Compilar recursos no pacote de aplicativos, e não em um pacote de recursos

Alguns tipos de apps (dicionários multilíngues, ferramentas de tradução etc.) precisam substituir o comportamento padrão de um lote de aplicativo e compilar recursos no pacote de aplicativos, em vez de tê-los em pacotes de recursos separados. Este tópico explica como fazer isso.

Por padrão, quando você compila um [lote de aplicativo (.appxbundle)](../packaging/packaging-uwp-apps.md), somente os recursos padrão de idioma, escala e nível de recurso DirectX estão integrados ao pacote de aplicativos. Os recursos traduzidos, e seus recursos personalizados para escalas não padrão e/ou níveis de recurso DirectX, são integrados aos pacotes de recursos e são apenas baixados para dispositivos que precisam deles. Se um cliente estiver comprando o app na Microsoft Store usando um dispositivo com uma preferência de idioma definida como Espanhol, somente seu app e o pacote de recursos em espanhol serão baixados e instalados. Se o mesmo usuário alterar posteriormente sua preferência de idioma para Francês em **Configurações**, o pacote de recursos em francês do app será baixado e instalado. O mesmo se aplicará aos recursos qualificados para escala e nível de recurso DirectX. Na maioria dos apps, esse comportamento constitui uma eficiência valiosa e é exatamente o que você e o cliente *querem* que aconteça.

Mas, se o app permitir que o usuário altere o idioma em tempo real no app (e não por meio de **Configurações**), esse comportamento padrão não será adequado. Na verdade, você quer que todos os recursos de idioma sejam baixados e instalados incondicionalmente junto com o app uma vez e, depois, permaneçam no dispositivo. Você deseja compilar todos esses recursos no pacote de aplicativos, e não em pacotes de recursos separados.

**Observação** A inclusão de recursos em um pacote de aplicativos basicamente aumenta o tamanho do app. É por isso que só vale a pena fazer isso se a natureza do app assim o exigir. Caso contrário, você não precisará fazer nada, a não ser compilar um lote de aplicativo regular como de costume.

Você pode configurar o Visual Studio para compilar recursos no pacote de aplicativos de duas maneiras. Você pode adicionar um arquivo de configuração ao projeto ou editar o arquivo de projeto diretamente. Use a alternativa com a qual se sentir mais confortável ou a que funcionar melhor no seu sistema de compilação.

## <a name="option-1-use-priconfigpackagingxml-to-build-resources-into-your-app-package"></a>Opção 1. Use priconfig.packaging.xml para compilar recursos no pacote de aplicativos

1. No Visual Studio, adicione um novo item ao projeto. Escolha Arquivo XML e nomeie o arquivo como `priconfig.packaging.xml`.
2. No Gerenciador de Soluções, selecione `priconfig.packaging.xml` e verifique a janela Propriedades. A Ação de Build do arquivo deve ser definida como Nenhuma, enquanto Copy to Output Directory definida como Do not copy.
3. Substitua o conteúdo do arquivo por este XML.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Language" />
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
4. Cada elemento `<autoResourcePackage>` instrui o Visual Studio a dividir automaticamente os recursos do nome de qualificador fornecido em pacotes de recursos separados. Isso é denominado *separação automática*. Com o conteúdo de arquivo que você tem até agora, o comportamento do Visual Studio não foi alterado. Em outras palavras, o Visual Studio *já estava se comportando como se* esse arquivo estivesse presente com esse conteúdo, pois isso é o padrão. Se você não quiser que o Visual Studio seja dividido automaticamente em um nome de qualificador, exclua o elemento `<autoResourcePackage>` do arquivo. O arquivo terá esta aparência se você quiser que todos os recursos de idioma sejam integrados ao pacote de aplicativos, em vez de serem divididos automaticamente em pacotes de recursos separados.
   ```xml
   <packaging>
      <autoResourcePackage qualifier="Scale" />
      <autoResourcePackage qualifier="DXFeatureLevel" />
   </packaging>
   ```
5. Salve e feche o arquivo, e recompile o projeto.

Para confirmar que suas opções de divisão automática estão sendo levadas em consideração, procure o arquivo `<ProjectFolder>\obj\<ReleaseConfiguration folder>\split.priconfig.xml` e confirme se seu conteúdo corresponde às suas opções. Em caso afirmativo, você configurou com êxito o Visual Studio para compilar os recursos de sua preferência no pacote de aplicativos.

Há uma etapa final que você precisa executar. **Mas somente se você tiver excluído o nome de qualificador `Language`**. Você precisa especificar a união de todos os idiomas com suporte do app como o padrão de idioma do app. Para obter detalhes, consulte [Especificar os recursos padrão usados pelo app](specify-default-resources-installed.md). É isso que o `priconfig.default.xml` conteria se você estivesse incluindo recursos para inglês, espanhol e francês no pacote de aplicativos.

```xml
   <default>
      <qualifier name="Language" value="en;es;fr" />
      ...
   </default>
```

### <a name="how-does-this-work"></a>Como isso funciona?

Em segundo plano, o Visual Studio inicia uma ferramenta chamada `MakePri.exe` para gerar um arquivo conhecido como índice de recurso do pacote, que descreve todos os recursos do app, incluindo a indicação de quais nomes de qualificador de recurso servirão como base para a divisão automática. Para obter detalhes sobre essa ferramenta, consulte [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md). O Visual Studio passa um arquivo de configuração para o `MakePri.exe`. O conteúdo do arquivo `priconfig.packaging.xml` é usado como elemento `<packaging>` desse arquivo de configuração, que é a parte que determina a divisão automática. Assim, a adição e a edição de `priconfig.packaging.xml` acaba influenciando o conteúdo do arquivo de índice de recurso do pacote que o Visual Studio gera para o app, bem como o conteúdo dos pacotes contidos no lote de aplicativo.

### <a name="using-a-different-file-name-than-priconfigpackagingxml"></a>Usando um nome de arquivo diferente de `priconfig.packaging.xml`

Se você nomear o arquivo `priconfig.packaging.xml`, o Visual Studio o reconhecerá e o usará automaticamente. Se você atribuir outro nome a ele, será necessário informar o Visual Studio. No arquivo de projeto, entre as marcas de abertura e fechamento do primeiro elemento `<PropertyGroup>`, adicione esse XML.

```xml
<AppxPriConfigXmlPackagingSnippetPath>FILE-PATH-AND-NAME</AppxPriConfigXmlPackagingSnippetPath>
```

Substitua `FILE-PATH-AND-NAME` pelo caminho e nome do arquivo.

## <a name="option-2-use-your-project-file-to-build-resources-into-your-app-package"></a>Opção 2. Usar o arquivo de projeto para compilar recursos no pacote de aplicativos

Esta é uma alternativa para a Opção 1. Depois que você entender como funciona a Opção 1, poderá optar pela Opção 2, se ela for mais adequada ao seu fluxo de trabalho de desenvolvimento e/ou compilação.

No arquivo de projeto, entre as marcas de abertura e fechamento do primeiro elemento `<PropertyGroup>`, adicione esse XML.

```xml
<AppxBundleAutoResourcePackageQualifiers>Language|Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Esta será a aparência do arquivo depois que você excluir o primeiro nome de qualificador.

```xml
<AppxBundleAutoResourcePackageQualifiers>Scale|DXFeatureLevel</AppxBundleAutoResourcePackageQualifiers>
```

Salve e feche o arquivo, e recompile o projeto.

Há uma etapa final que você precisa executar. **Mas somente se você tiver excluído o nome de qualificador `Language`**. Você precisa especificar a união de todos os idiomas com suporte do app como o padrão de idioma do app. Para obter detalhes, consulte [Especificar os recursos padrão usados pelo app](specify-default-resources-installed.md). É isso que o arquivo de projeto conteria se você estivesse incluindo recursos para inglês, espanhol e francês no pacote de aplicativos.

```xml
<AppxDefaultResourceQualifiers>Language=en;es;fr</AppxDefaultResourceQualifiers>
```

## <a name="related-topics"></a>Tópicos relacionados

* [União de um aplicativo UWP com o Visual Studio](../packaging/packaging-uwp-apps.md)
* [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md)
* [Especificar os recursos padrão usados pelo app](specify-default-resources-installed.md)