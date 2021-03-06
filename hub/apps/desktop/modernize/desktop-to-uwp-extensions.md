---
Description: Você pode usar as extensões para integrar seu aplicativo da área de trabalho empacotado ao Windows 10 de maneiras predefinidas.
title: Modernizar aplicativos da área de trabalho existentes usando a Ponte de Desktop
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: b048de69a8a259509e5a5c54c5f1d61675a25a18
ms.sourcegitcommit: e51f9489d8c977c3498afb1a75c91f96ac3a642b
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/26/2020
ms.locfileid: "83854712"
---
# <a name="integrate-your-desktop-app-with-windows-10-and-uwp"></a>Integre seu aplicativo da área de trabalho ao Windows 10 e a UWP

Se o aplicativo da área de trabalho tiver um [identificador de pacote](modernize-packaged-apps.md), você poderá usar extensões para integrá-lo ao Windows 10 usando extensões predefinidas no [manifesto do pacote](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Por exemplo, use uma extensão para criar uma exceção de firewall, tornar seu aplicativo o aplicativo padrão para um tipo de arquivo ou apontar blocos de início para o aplicativo. Para usar uma extensão, basta adicionar alguns XML ao arquivo de manifesto do pacote do seu aplicativo. Nenhum código é necessário.

Este artigo descreve essas extensões e as tarefas que você pode executar usando-as.

> [!NOTE]
> Os recursos descritos neste artigo exigem que seu aplicativo da área de trabalho tenha um [identificador de pacote](modernize-packaged-apps.md), seja [empacotando o aplicativo da área de trabalho em um pacote MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root) ou [concedendo a identidade do aplicativo usando um pacote esparso](grant-identity-to-nonpackaged-apps.md).

## <a name="transition-users-to-your-app"></a>Transição de usuários para o aplicativo

Ajude os usuários com a transição para o aplicativo empacotado.

* [Aponte os blocos Iniciar e os botões da barra de tarefas existentes para o aplicativo empacotado](#point)
* [Faça o aplicativo empacotado abrir arquivos em vez do seu aplicativo da área de trabalho](#make)
* [Associe o aplicativo empacotado a um conjunto de tipos de arquivos](#associate)
* [Adicione opções aos menus de contexto dos arquivos que têm um determinado tipo de arquivo](#add)
* [Abra certos tipos de arquivos diretamente usando um URL](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Aponte os blocos Iniciar e os botões da barra de tarefas existentes para o aplicativo empacotado

Os usuários podem ter colocado seu aplicativo desktop na barra de tarefas ou no menu Iniciar. Você pode apontar esses atalhos para seu novo aplicativo empacotado.

#### <a name="xml-namespace"></a>Namespace de XML

`http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.desktopAppMigration">
    <DesktopAppMigration>
        <DesktopApp AumId="[your_app_aumid]" />
        <DesktopApp ShortcutPath="[path]" />
    </DesktopAppMigration>
</Extension>

```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Nome | Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.desktopAppMigration``.
|AumID |A ID de modelo de usuário do aplicativo do seu aplicativo empacotado. |
|ShortcutPath |O caminho para arquivos .lnk que iniciam a versão desktop do seu aplicativo. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="rescap3">
  <Applications>
    <Application>
      <Extensions>
        <rescap3:Extension Category="windows.desktopAppMigration">
          <rescap3:DesktopAppMigration>
            <rescap3:DesktopApp AumId="[your_app_aumid]" />
            <rescap3:DesktopApp ShortcutPath="%USERPROFILE%\Desktop\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%APPDATA%\Microsoft\Windows\Start Menu\Programs\[my_app].lnk" />
            <rescap3:DesktopApp ShortcutPath="%PROGRAMDATA%\Microsoft\Windows\Start Menu\Programs\[my_app_folder]\[my_app].lnk"/>
         </rescap3:DesktopAppMigration>
        </rescap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemplo relacionado

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Faça o aplicativo empacotado abrir arquivos em vez do seu aplicativo da área de trabalho

Você pode certificar-se de que os usuários abrem seu novo aplicativo empacotado por padrão para tipos específicos de arquivos ao invés de abrir a versão da área de trabalho do seu aplicativo.

Para fazer isso, você especifica o [identificador programático (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) de cada aplicativo do qual você deseja herdar associações de arquivos.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|MigrationProgId |O [identificador programático (ProgID)](https://docs.microsoft.com/windows/desktop/shell/fa-progids) que descreve o aplicativo, o componente e a versão do aplicativo da área de trabalho do qual você quer herdar associações de arquivo.|

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:rescap3="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap3, rescap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <rescap3:MigrationProgIds>
              <rescap3:MigrationProgId>Foo.Bar.1</rescap3:MigrationProgId>
              <rescap3:MigrationProgId>Foo.Bar.2</rescap3:MigrationProgId>
            </rescap3:MigrationProgIds>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemplo relacionado

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Associe o aplicativo empacotado a um conjunto de tipos de arquivos

Seu aplicativo empacotado pode ser associado a extensões de tipo de arquivo. Se um usuário clicar com o botão direito do mouse em um arquivo e depois selecionar a opção **Abrir com**, seu aplicativo aparecerá na lista de sugestões.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome | O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços.   |
|FileType |A extensão de arquivo compatível com seu aplicativo. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="mediafiles">
            <uap:SupportedFileTypes>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemplo relacionado

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Adicione opções aos menus de contexto dos arquivos que têm um determinado tipo de arquivo

Na maioria dos casos, os usuários, clique duas vezes arquivos para abri-los. Se os usuários clicarem com o botão direito do mouse em um arquivo, várias opções aparecerão.

Você pode adicionar opções de menu. Essas opções oferecem aos usuários outras formas de interagir com seu arquivo, como imprimir, editar ou visualizar o arquivo.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria | Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|Verb |O nome que aparece no menu de contexto do Explorador de Arquivos. Essa sequência é localizável usando ```ms-resource```.|
|Id |A Id exclusiva do verbo. Se o aplicativo for um aplicativo UWP, isso será transmitido para seu aplicativo como parte de seus argumentos do evento de ativação para que ele possa manipular a seleção do usuário de maneira adequada. Se o seu aplicativo for um aplicativo empacotado totalmente confiável, ele receberá parâmetros (veja o próximo marcador). |
|Parâmetros |A lista de parâmetros de argumento e valores associados ao verbo. Se o seu aplicativo for um aplicativo empacotado totalmente confiável, esses parâmetros serão passados para o aplicativo como eventos quando o aplicativo for ativado. Você pode personalizar o comportamento do seu aplicativo com base em diferentes verbos de ativação. Se uma variável puder conter um caminho de arquivo, envolva o valor do parâmetro entre aspas. Isso evitará problemas nos casos em que o caminho inclua espaços. Se o seu aplicativo for um aplicativo UWP, você não poderá passar os parâmetros. O aplicativo recebe a Id em vez disso (veja a bala anterior).|
|Estendido |Especifica que o verbo só aparece se o usuário mostra o menu de contexto segurando a tecla **Shift** antes de clicar com o botão direito do mouse no arquivo. Esse atributo é opcional e o padrão é um valor **False** (por exemplo, mostrar sempre o verbo) se não listado. Você especifica esse comportamento individualmente para cada verbo (com exceção de "Abrir", que é sempre **False**).|

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" Parameters="/e &quot;%1&quot;">Edit</uap3:Verb>
              <uap3:Verb Id="Print" Extended="true" Parameters="/p &quot;%1&quot;">Print</uap3:Verb>
            </uap2:SupportedVerbs>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemplo relacionado

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Abra certos tipos de arquivos diretamente usando um URL

Você pode certificar-se de que os usuários abrem seu novo aplicativo empacotado por padrão para tipos específicos de arquivos ao invés de abrir a versão da área de trabalho do seu aplicativo.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|UseUrl |Indica se deve abrir arquivos diretamente de um destino de URL. Se você não definir esse valor, as tentativas do seu aplicativo para abrir um arquivo usando uma URL farão com que o sistema primeiro faça o download do arquivo localmente. |
|Parâmetros | Parâmetros opcionais. |
|FileType |As extensões de arquivo relevante. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap3">
  <Applications>
      <Application>
        <Extensions>
          <uap:Extension Category="windows.fileTypeAssociation">
            <uap3:FileTypeAssociation Name="myfiletypes" UseUrl="true" Parameters="%1">
              <uap:SupportedFileTypes>
                <uap:FileType>.txt</uap:FileType>
                <uap:FileType>.doc</uap:FileType>
              </uap:SupportedFileTypes>
            </uap3:FileTypeAssociation>
          </uap:Extension>
        </Extensions>
      </Application>
    </Applications>
</Package>
```

## <a name="perform-setup-tasks"></a>Executar tarefas de configuração

* [Criar a exceção de firewall para o seu aplicativo](#rules)
* [Colocar os arquivos DLL em qualquer pasta do pacote](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>Criar a exceção de firewall para o seu aplicativo

Se o seu aplicativo precisar de comunicação através de uma porta, você poderá adicionar seu aplicativo à lista de exceções de firewall.

#### <a name="xml-namespace"></a>Namespace de XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.firewallRules">
  <FirewallRules Executable="[executable file name]">
    <Rule
      Direction="[Direction]"
      IPProtocol="[Protocol]"
      LocalPortMin="[LocalPortMin]"
      LocalPortMax="LocalPortMax"
      RemotePortMin="RemotePortMin"
      RemotePortMax="RemotePortMax"
      Profile="[Profile]"/>
  </FirewallRules>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-firewallrules).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.firewallRules``|
|Executável |O nome do arquivo executável que deseja adicionar à lista de exceções de firewall |
|Direção |Indica se a regra é de entrada ou de saída |
|IPProtocol |O protocolo de comunicação |
|LocalPortMin |O número da porta inferior em uma variedade de números de porta locais. |
|LocalPortMax |O número de porta mais alto de uma variedade de números de porta local. |
|RemotePortMax |O número da porta inferior em uma variedade de números de porta remota. |
|RemotePortMax |O número de porta mais alto de uma variedade de números de porta remota. |
|Perfil |O tipo de rede |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Extensions>
    <desktop2:Extension Category="windows.firewallRules">
      <desktop2:FirewallRules Executable="Contoso.exe">
          <desktop2:Rule Direction="in" IPProtocol="TCP" Profile="all"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="domain"/>
          <desktop2:Rule Direction="in" IPProtocol="UDP" LocalPortMin="1337" LocalPortMax="1338" Profile="public"/>
          <desktop2:Rule Direction="out" IPProtocol="UDP" LocalPortMin="1339" LocalPortMax="1340" RemotePortMin="15"
                         RemotePortMax="19" Profile="domainAndPrivate"/>
          <desktop2:Rule Direction="out" IPProtocol="GRE" Profile="private"/>
      </desktop2:FirewallRules>
  </desktop2:Extension>
</Extensions>
</Package>
```

<a id="load-paths" />

### <a name="place-your-dll-files-into-any-folder-of-the-package"></a>Colocar os arquivos DLL em qualquer pasta do pacote

Use uma extensão para identificar essas pastas. Dessa forma, o sistema pode localizar e carregar os arquivos que você colocar nelas. Pense nessa extensão como um substituto da variável de ambiente _%PATH%_ .

Se você não usar essa extensão, o sistema pesquisará o grafo de dependência de pacote do processo, a pasta raiz do pacote e o diretório do sistema ( _%SystemRoot%\system32_) nessa ordem. Para saber mais, confira [Ordem de pesquisa de aplicativos do Windows](https://docs.microsoft.com/windows/desktop/Dlls/dynamic-link-library-search-order).

Cada pacote pode conter apenas uma dessas extensões. Isso significa que você pode adicionar uma delas ao pacote principal e, em seguida, adicionar uma a cada um dos [pacotes opcionais e conjuntos relacionados](/windows/msix/package/optional-packages).

#### <a name="xml-namespace"></a>Namespace de XML

`http://schemas.microsoft.com/appx/manifest/uap/windows10/6`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

Declare essa extensão no nível do pacote do manifesto do aplicativo.

```XML
<Extension Category="windows.loaderSearchPathOverride">
  <LoaderSearchPathOverride>
    <LoaderSearchPathEntry FolderPath="[path]"/>
  </LoaderSearchPathOverride>
</Extension>

```

|Nome | Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.loaderSearchPathOverride``.
|FolderPath | O caminho da pasta que contém os arquivos dll. Especifica um caminho relativo à pasta raiz do pacote. Você pode especificar até cinco caminhos em uma extensão. Se você quiser que o sistema procure arquivos na pasta raiz do pacote, use uma cadeia de caracteres vazia para um desses caminhos. Não inclua caminhos duplicados e certifique-se de que seus caminhos não contenham barras ou barras invertidas à esquerda e à direita. <br><br> O sistema não pesquisa subpastas, portanto, certifique-se de listar explicitamente cada pasta que contém os arquivos DLL que você deseja que o sistema carregue.|

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/6"
  IgnorableNamespaces="uap6">
  ...
    <Extensions>
      <uap6:Extension Category="windows.loaderSearchPathOverride">
        <uap6:LoaderSearchPathOverride>
          <uap6:LoaderSearchPathEntry FolderPath=""/>
          <uap6:LoaderSearchPathEntry FolderPath="folder1/subfolder1"/>
          <uap6:LoaderSearchPathEntry FolderPath="folder2/subfolder2"/>
        </uap6:LoaderSearchPathOverride>
      </uap6:Extension>
    </Extensions>
...
</Package>
```

## <a name="integrate-with-file-explorer"></a>Integração ao Explorador de Arquivos

Ajude os usuários organizar seus arquivos e interagir com eles de maneiras familiares.

* [Definir como seu aplicativo se comporta quando os usuários selecionam e abrem vários arquivos ao mesmo tempo](#define)
* [Mostrar o conteúdo do arquivo em uma imagem em miniatura dentro do Explorador de Arquivos](#show)
* [Mostrar o conteúdo do arquivo em um painel de visualização do Explorador de Arquivos](#preview)
* [Permitir que os usuários agrupem arquivos usando a coluna Tipo no Explorador de Arquivos](#enable)
* [Disponibilizar as propriedades dos arquivos para pesquisa, índice, diálogos de propriedade e o painel de detalhes](#make-file-properties)
* [Especificar um manipulador de menu de contexto para um tipo de arquivo](#context-menu)
* [Fazer os arquivos de seu serviço de nuvem aparecerem no Explorador de Arquivos](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definir como seu aplicativo se comporta quando os usuários selecionam e abrem vários arquivos ao mesmo tempo

Especificar como seu aplicativo se comporta quando um usuário abre vários arquivos simultaneamente.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]" MultiSelectModel="[SelectionModel]">
        <SupportedVerbs>
            <Verb Id="Edit" MultiSelectModel="[SelectionModel]">Edit</Verb>
        </SupportedVerbs>
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|MultiSelectModel |Veja abaixo |
|FileType |As extensões de arquivo relevante. |

**MultiSelectModel**

Os aplicativos da área de trabalho empacotados têm as mesmas três opções do que os aplicativos da área de trabalho comuns.

* ``Player``: o aplicativo é ativado uma vez. Todos os arquivos selecionados são passados para o seu aplicativo como parâmetros de argumento.
* ``Single``: o aplicativo é ativado uma vez para o primeiro arquivo selecionado. Outros arquivos são ignorados.
* ``Document``: uma nova instância separada do seu aplicativo é ativada para cada arquivo selecionado.

 Você pode definir preferências diferentes para diferentes tipos de arquivo e ações. Por exemplo, você pode abrir *Documentos* no modo *Documento* e *Imagens* no modo *Player*.

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap, uap2, uap3">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes" MultiSelectModel="Document">
            <uap2:SupportedVerbs>
              <uap3:Verb Id="Edit" MultiSelectModel="Player">Edit</uap3:Verb>
              <uap3:Verb Id="Preview" MultiSelectModel="Document">Preview</uap3:Verb>
            </uap2:SupportedVerbs>
            <uap:SupportedFileTypes>
              <uap:FileType>.txt</uap:FileType>
            </uap:SupportedFileTypes>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Se o usuário abrir 15 ou menos arquivos, a opção padrão para o atributo **MultiSelectModel** será *Player*. Caso contrário, o valor padrão é *Documento*. Os aplicativos UWP sempre são iniciados como *Player*.

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Mostrar o conteúdo do arquivo em uma imagem em miniatura dentro do Explorador de Arquivos

Permita que os usuários vejam uma imagem em miniatura do conteúdo do arquivo quando o ícone do arquivo aparecer no tamanho médio, grande ou extra grande.

#### <a name="xml-namespace"></a>Namespace de XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <ThumbnailHandler
            Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|FileType |As extensões de arquivo relevante. |
|Clsid   |A ID de classe do seu aplicativo. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap2:SupportedFileTypes>
            <desktop2:ThumbnailHandler
              Clsid  ="20000000-0000-0000-0000-000000000001"  />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="preview" />

### <a name="show-file-contents-in-the-preview-pane-of-file-explorer"></a>Mostrar o conteúdo do arquivo em um painel de visualização do Explorador de Arquivos

Permita que os usuários visualizem os conteúdos de um arquivo no painel de visualização do Explorador de Arquivos.

#### <a name="xml-namespace"></a>Namespace de XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/2`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <DesktopPreviewHandler Clsid  ="[Clsid  ]" />
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|FileType |As extensões de arquivo relevante. |
|Clsid   |A ID de classe do seu aplicativo. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap2="http://schemas.microsoft.com/appx/manifest/uap/windows10/2"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap2, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap2SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
                </uap2SupportedFileTypes>
              <desktop2:DesktopPreviewHandler Clsid ="20000000-0000-0000-0000-000000000001" />
           </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="enable" />

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Permitir que os usuários agrupem arquivos usando a coluna Tipo no Explorador de Arquivos

Você pode associar um ou mais valores predefinidos para seus tipos de arquivo com o campo **Tipo**.

No Explorador de Arquivos, os usuários podem agrupar esses arquivos usando esse campo. Componentes do sistema também usam esse campo para várias finalidades, como de indexação.

Para obter mais informações sobre o campo **Tipo** e os valores que você pode usar para esse campo, confira [usando nomes de tipo](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names).

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
        <KindMap>
            <Kind value="[KindValue]">
        </KindMap>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|FileType |As extensões de arquivo relevante. |
|value |Um [valor de tipo](https://docs.microsoft.com/windows/desktop/properties/building-property-handlers-user-friendly-kind-names) válido |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3"
  IgnorableNamespaces="uap, rescap">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
           <uap:FileTypeAssociation Name="mediafiles">
             <uap:SupportedFileTypes>
               <uap:FileType>.m4a</uap:FileType>
               <uap:FileType>.mta</uap:FileType>
             </uap:SupportedFileTypes>
             <rescap:KindMap>
               <rescap:Kind value="Item">
               <rescap:Kind value="Communications">
               <rescap:Kind value="Task">
             </rescap:KindMap>
          </uap:FileTypeAssociation>
      </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="make-file-properties" />

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Disponibilizar as propriedades dos arquivos para pesquisa, índice, diálogos de propriedade e o painel de detalhes

#### <a name="xml-namespace"></a>Namespace de XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10`
* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[Name]">
        <SupportedFileTypes>
            <FileType>.bar</FileType>
        </SupportedFileTypes>
        <DesktopPropertyHandler Clsid ="[Clsid]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |O nome da associação de tipo de arquivo. Você pode usar esse nome para organizar e agrupar tipos de arquivo. O nome deve ter todos os caracteres minúsculos sem espaços. |
|FileType |As extensões de arquivo relevante. |
|Clsid  |A ID de classe do seu aplicativo. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="uap, uap3, desktop2">
  <Applications>
    <Application>
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap3:FileTypeAssociation Name="myfiletypes">
            <uap:SupportedFileTypes>
              <uap:FileType>.bar</uap:FileType>
            </uap:SupportedFileTypes>
            <desktop2:DesktopPropertyHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="context-menu" />

### <a name="specify-a-context-menu-handler-for-a-file-type"></a>Especificar um manipulador de menu de contexto para um tipo de arquivo

Se o aplicativo da área de trabalho definir um [manipulador de menu de contexto](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers), use essa extensão para registrar o manipulador de menu.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/foundation/windows10`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10/4`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extensions>
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="[AppID]" DisplayName="[DisplayName]">
                <com:Class Id="[Clsid]" Path="[Path]" ThreadingModel="[Model]"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type="[Type]">
                <desktop4:Verb Id="[ID]" Clsid="[Clsid]" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
</Extensions>
```

Encontre a referência de esquema completa aqui: [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) e [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus).

#### <a name="instructions"></a>Instruções

Para registrar seu manipulador de menu de contexto, siga estas instruções.

1. Em seu aplicativo da área de trabalho, implemente um [manipulador de menu de contexto](https://docs.microsoft.com/windows/desktop/shell/context-menu-handlers) implementando a interface [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand) ou [IExplorerCommandState](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommandstate). Para obter um exemplo, confira o exemplo de código [ExplorerCommandVerb](https://github.com/microsoft/Windows-classic-samples/tree/master/Samples/Win7Samples/winui/shell/appshellintegration/ExplorerCommandVerb). Certifique-se de definir um GUID de classe para cada um dos objetos de implementação. Por exemplo, o código a seguir define uma ID de classe para uma implementação de [IExplorerCommand](https://docs.microsoft.com/windows/desktop/api/shobjidl_core/nn-shobjidl_core-iexplorercommand).

    ```cpp
    class __declspec(uuid("d0c8bceb-28eb-49ae-bc68-454ae84d6264")) CExplorerCommandVerb;
    ```

2. No manifesto do pacote, especifique uma extensão de aplicativo [com:ComServer](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-com-comserver) que registra um servidor alternativo COM com a ID de classe de sua implementação do manipulador do menu de contexto.

    ```xml
    <com:Extension Category="windows.comServer">
        <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler">
                <com:Class Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
        </com:ComServer>
    </com:Extension>
    ```

2. No manifesto do pacote, especifique uma extensão de aplicativo [desktop4:FileExplorerContextMenus](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop4-fileexplorercontextmenus) que registra a implementação do manipulador do menu de contexto.

    ```xml
    <desktop4:Extension Category="windows.fileExplorerContextMenus">
        <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".rar">
                <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
        </desktop4:FileExplorerContextMenus>
    </desktop4:Extension>
    ```

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4"
  IgnorableNamespaces="desktop4">
  <Applications>
    <Application>
      <Extensions>
        <com:Extension Category="windows.comServer">
          <com:ComServer>
            <com:SurrogateServer AppId="d0c8bceb-28eb-49ae-bc68-454ae84d6264" DisplayName="ContosoHandler"">
              <com:Class Id="Id="d0c8bceb-28eb-49ae-bc68-454ae84d6264" Path="ExplorerCommandVerb.dll" ThreadingModel="STA"/>
            </com:SurrogateServer>
          </com:ComServer>
        </com:Extension>
        <desktop4:Extension Category="windows.fileExplorerContextMenus">
          <desktop4:FileExplorerContextMenus>
            <desktop4:ItemType Type=".contoso">
              <desktop4:Verb Id="Command1" Clsid="d0c8bceb-28eb-49ae-bc68-454ae84d6264" />
            </desktop4:ItemType>
          </desktop4:FileExplorerContextMenus>
        </desktop4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Fazer os arquivos de seu serviço de nuvem aparecerem no Explorador de Arquivos

Registre os manipuladores implementados em seu aplicativo. Você também pode adicionar opções de menu de contexto que aparecem quando os usuários clicam com o botão direito do mouse nos arquivos baseados em nuvem no Explorador de Arquivos.

#### <a name="xml-namespace"></a>Namespace de XML

* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.cloudfiles" >
    <CloudFiles IconResource="[Icon]">
        <CustomStateHandler Clsid ="[Clsid]"/>
        <ThumbnailProviderHandler Clsid ="[Clsid]"/>
        <ExtendedPropertyhandler Clsid ="[Clsid]"/>
        <CloudFilesContextMenus>
            <Verb Id ="Command3" Clsid= "[GUID]">[Verb Label]</Verb>
        </CloudFilesContextMenus>
    </CloudFiles>
</Extension>

```

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.cloudfiles``.
|iconResource |O ícone que representa seu serviço de provedor de arquivo em nuvem. Este ícone é exibido no painel de navegação do Explorador de Arquivos.  Os usuários escolhem esse ícone para mostrar arquivos de seu serviço de nuvem. |
|CustomStateHandler Clsid |A ID de classe do aplicativo que implementa o CustomStateHandler. O sistema usa essa ID de classe para solicitar estados personalizados e colunas para arquivos na nuvem. |
|ThumbnailProviderHandler Clsid |A ID de classe do aplicativo que implementa o ThumbnailProviderHandler. O sistema usa essa ID de classe para solicitar imagens em miniatura para arquivos na nuvem. |
|ExtendedPropertyHandler Clsid |A ID de classe do aplicativo que implementa o ExtendedPropertyHandler.  O sistema usa essa ID de classe para solicitar propriedades estendidas para um arquivo de nuvem. |
|Verb |O nome que aparece no menu de contexto do Explorador de Arquivos para arquivos fornecidos pelo seu serviço de nuvem. |
|Id |A ID exclusiva do verbo. |

#### <a name="example"></a>Exemplo

```XML
<Package
    xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
    IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <Extension Category="windows.cloudfiles" >
            <CloudFiles IconResource="images\Wide310x150Logo.png">
                <CustomStateHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ThumbnailProviderHandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <ExtendedPropertyhandler Clsid ="20000000-0000-0000-0000-000000000001"/>
                <desktop:CloudFilesContextMenus>
                    <desktop:Verb Id ="keep" Clsid=
                       "20000000-0000-0000-0000-000000000001">
                       Always keep on this device</desktop:Verb>
                </desktop:CloudFilesContextMenus>
            </CloudFiles>
          </Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="start" />

## <a name="start-your-application-in-different-ways"></a>Iniciar o aplicativo de maneiras diferentes

* [Iniciar o aplicativo usando um protocolo](#protocol)
* [Iniciar o aplicativo usando um alias](#alias)
* [Iniciar um arquivo executável quando os usuários entrarem no Windows](#executable)
* [Permitir que os usuários iniciem seu aplicativo quando conectarem um dispositivo ao computador](#autoplay)
* [Reiniciar automaticamente depois de receber uma atualização da Microsoft Store](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>Iniciar o aplicativo usando um protocolo

As associações de protocolos podem habilitar outros programas e componentes do sistema para interoperarem com seu aplicativo empacotado. Quando seu aplicativo empacotado é iniciado usando um protocolo, você pode especificar parâmetros específicos para passar aos seus argumentos de evento de ativação para que ele possa se comportar de acordo. Os parâmetros têm suporte somente aos aplicativos empacotados de confiança total. Os aplicativos UWP não podem usar parâmetros.

#### <a name="xml-namespace"></a>Namespace de XML

`http://schemas.microsoft.com/appx/manifest/uap/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension
    Category="windows.protocol">
  <Protocol
      Name="[Protocol name]"
      Parameters="[Parameters]" />
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-protocol).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.protocol``.
|Nome |O nome do protocolo. |
|Parâmetros |A lista de parâmetros e valores a serem passados para o seu aplicativo como argumentos de evento quando o aplicativo está ativado. Se uma variável puder conter um caminho de arquivo, envolva o valor do parâmetro entre aspas. Isso evitará problemas nos casos em que o caminho inclua espaços. |

### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  <Applications>
    <Application>
      <Extensions>
        <uap3:Extension
          Category="windows.protocol">
          <uap3:Protocol
            Name="myapp-cmd"
            Parameters="/p &quot;%1&quot;" />
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>Iniciar o aplicativo usando um alias

Usuários e outros processos podem usar um alias para iniciar seu aplicativo sem precisar especificar o caminho completo para seu aplicativo. Você pode especificar esse nome de alias.

#### <a name="xml-namespaces"></a>Namespaces XML

* `http://schemas.microsoft.com/appx/manifest/uap/windows10/3`
* `http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension
    Category="windows.appExecutionAlias"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
    <AppExecutionAlias>
        <desktop:ExecutionAlias Alias="[AliasName]" />
    </AppExecutionAlias>
</Extension>
```

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.appExecutionAlias``.
|Executável |O caminho relativo para o executável para iniciar quando o alias é invocado. |
|Alias |O nome curto do seu aplicativo. Sempre deve terminar com a extensão ".exe". Você só pode especificar um único alias de execução de aplicativo para cada aplicativo no pacote. Se vários aplicativos se registrarem para o mesmo alias, o sistema irá chamar o último que foi registrado. Portanto, escolha um alias exclusivo que outros aplicativos provavelmente não substituirão.
|

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
  <Applications>
    <Application>
      <Extensions>
         <uap3:Extension
                Category="windows.appExecutionAlias"
                Executable="exes\launcher.exe"
                EntryPoint="Windows.FullTrustApplication">
            <uap3:AppExecutionAlias>
                <desktop:ExecutionAlias Alias="Contoso.exe" />
            </uap3:AppExecutionAlias>
        </uap3:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Iniciar um arquivo executável quando os usuários entrarem no Windows

As tarefas de inicialização permitem que seu aplicativo execute um executável automaticamente sempre que um usuário faz logon.

> [!NOTE]
> O usuário deve iniciar seu aplicativo pelo menos uma vez para registrar essa tarefa de inicialização.

Seu aplicativo pode declarar várias tarefas de inicialização. Cada tarefa é iniciada de maneira independente. Todas as tarefas de inicialização serão exibidas no Gerenciador de Tarefas na guia **Inicialização** com o nome especificado no manifesto do aplicativo e no ícone do aplicativo. O Gerenciador de Tarefas analisará automaticamente o impacto de inicialização de suas tarefas.

Os usuários podem desabilitar manualmente tarefas de inicialização do seu aplicativo usando o Gerenciador de tarefas. Se o usuário desabilitar uma tarefa, você não poderá reabilitá-la de maneira programática.

#### <a name="xml-namespace"></a>Namespace de XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension
    Category="windows.startupTask"
    Executable="[ExecutableName]"
    EntryPoint="Windows.FullTrustApplication">
  <StartupTask
      TaskId="[TaskID]"
      Enabled="true"
      DisplayName="[DisplayName]" />
</Extension>
```

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.startupTask``.|
|Executável |O caminho relativo para o arquivo executável para começar. |
|TaskId |Um identificador exclusivo para sua tarefa. Usando esse identificador, seu aplicativo pode chamar as APIs na classe [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) para habilitar ou desabilitar uma tarefa de inicialização de maneira programática. |
|Habilitada |Indica se a tarefa é iniciada habilitada ou desabilitada. As tarefas habilitadas serão executadas na próxima vez em que o usuário fizer logon (a menos que o usuário as tenha desabilitado). |
|DisplayName |O nome da tarefa que aparece no Gerenciador de Tarefas. Você pode localizar essa cadeia de caracteres usando ```ms-resource```. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="desktop">
  <Applications>
    <Application>
      <Extensions>
        <desktop:Extension
          Category="windows.startupTask"
          Executable="bin\MyStartupTask.exe"
          EntryPoint="Windows.FullTrustApplication">
        <desktop:StartupTask
          TaskId="MyStartupTask"
          Enabled="true"
          DisplayName="My App Service" />
        </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
 </Package>
```

<a id="autoplay" />

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permitir que os usuários iniciem seu aplicativo quando conectarem um dispositivo ao computador

A Reprodução Automática pode apresentar seu aplicativo como uma opção quando um usuário conecta um dispositivo ao computador.

#### <a name="xml-namespace"></a>Namespace de XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/3`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.autoPlayHandler">
  <AutoPlayHandler>
    <InvokeAction ActionDisplayName="[action string]" ProviderDisplayName="[name of your app/service]">
      <Content ContentEvent="[Content event]" Verb="[any string]" DropTargetHandler="[Clsid]" />
      <Content ContentEvent="[Content event]" Verb="[any string]" Parameters="[Initialization parameter]"/>
      <Device DeviceEvent="[Device event]" HWEventHandler="[Clsid]" InitCmdLine="[Initialization parameter]"/>
    </InvokeAction>
  </AutoPlayHandler>
```

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.autoPlayHandler``.
|ActionDisplayName |Uma cadeia de caracteres que representa a ação que os usuários podem realizar com um dispositivo que conectam a um computador (por exemplo: "Importar arquivos" ou "Reproduzir vídeo"). |
|ProviderDisplayName | Uma cadeia de caracteres que representa seu aplicativo ou serviço (por exemplo: "Player de vídeo Contoso"). |
|ContentEvent |O nome de um evento de conteúdo que faz com que os usuários sejam avisados com ``ActionDisplayName`` e ``ProviderDisplayName``. Um evento de conteúdo é gerado quando um dispositivo de volume, como um cartão de memória de câmera, um pen drive ou um DVD, for inserido no computador. Você pode encontrar a lista completa desses eventos [aqui](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verb |A configuração Verbo identifica um valor que é passado ao seu aplicativo para a opção selecionada. Você pode especificar várias ações de inicialização para um evento de Reprodução Automática e usar a configuração Verbo para determinar qual opção u usuário selecionou para seu aplicativo. Você pode descobrir a opção selecionada pelo usuário verificando a propriedade verb dos argumentos do evento de inicialização passados para seu aplicativo. Também pode usar qualquer valor para a configuração Verbo exceto open, que está reservado. |
|DropTargetHandler |A ID de classe do aplicativo que implementa a interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017). Arquivos da mídia removível são transmitidos para o método [Drop](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) da implementação [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017).  |
|Parâmetros |Você não precisa implementar a interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) para todos os eventos de conteúdo. Para qualquer um dos eventos de conteúdo, você pode fornecer parâmetros de linha de comando em vez de implementar a interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017). Para esses eventos, a Reprodução Automática iniciará seu aplicativo usando os parâmetros de linha de comando. Você pode analisar esses parâmetros no código de inicialização do seu aplicativo para determinar se ele foi iniciado por Reprodução Automática e, em seguida, fornecer sua implementação personalizada. |
|DeviceEvent |O nome de um evento de dispositivo que faz com que os usuários sejam avisados com ``ActionDisplayName`` e ``ProviderDisplayName``. Um evento de dispositivo é gerado quando um dispositivo é conectado ao computador. Eventos de dispositivo começam com a cadeia de caracteres ``WPD`` e você pode encontrá-los listados [aqui](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |A ID de classe do aplicativo que implementa a interface [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |
|InitCmdLine |O parâmetro de cadeia de caracteres que você deseja transmitir para o método [Inicializar](https://docs.microsoft.com/windows/desktop/api/shobjidl/nf-shobjidl-ihweventhandler-initialize) da interface [IHWEventHandler](https://docs.microsoft.com/windows/desktop/api/shobjidl/nn-shobjidl-ihweventhandler). |

### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:desktop3="http://schemas.microsoft.com/appx/manifest/desktop/windows10/3"
  IgnorableNamespaces="desktop3">
  <Applications>
    <Application>
      <Extensions>
        <desktop3:Extension Category="windows.autoPlayHandler">
          <desktop3:AutoPlayHandler>
            <desktop3:InvokeAction ActionDisplayName="Import my files" ProviderDisplayName="ms-resource:AutoPlayDisplayName">
              <desktop3:Content ContentEvent="ShowPicturesOnArrival" Verb="show" DropTargetHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8"/>
              <desktop3:Content ContentEvent="PlayVideoFilesOnArrival" Verb="play" Parameters="%1" />
              <desktop3:Device DeviceEvent="WPD\ImageSource" HWEventHandler="CD041BAE-0DEA-4472-9B7B-C98043D26EA8" InitCmdLine="/autoplay"/>
            </desktop3:InvokeAction>
          </desktop3:AutoPlayHandler>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="updates" />

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Reiniciar automaticamente depois de receber uma atualização da Microsoft Store

Se o aplicativo estiver aberto quando os usuários instalarem uma atualização para ele, o aplicativo será fechado.

Se você quiser que o aplicativo reinicie após a conclusão da atualização, chame a função [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) em cada processo que você deseja reiniciar.

Cada janela ativa em seu aplicativo recebe uma mensagem [WM_QUERYENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-queryendsession). Neste ponto, seu aplicativo pode chamar a função [RegisterApplicationRestart](https://docs.microsoft.com/windows/desktop/api/winbase/nf-winbase-registerapplicationrestart) novamente para atualizar a linha de comando, se necessário.

Quando cada janela ativa em seu aplicativo receber a mensagem [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession), seu aplicativo deverá salvar os dados e desligar.

>[!NOTE]
As janelas ativas também receberão a mensagem [WM_CLOSE](https://docs.microsoft.com/windows/desktop/winmsg/wm-close) caso o aplicativo não manipule a mensagem [WM_ENDSESSION](https://docs.microsoft.com/windows/desktop/Shutdown/wm-endsession).

Neste ponto, seu aplicativo tem 30 segundos para fechar os próprios processos ou a plataforma os encerrará de maneira imposta.

Após a conclusão da atualização, seu aplicativo será reiniciado.

## <a name="work-with-other-applications"></a>Trabalhar com outros aplicativos

Integre-se a outros aplicativos, inicie outros processos ou compartilhe informações.

* [Faça o seu aplicativo aparecer como o alvo de impressão em aplicativos com suporte para impressão](#printing)
* [Compartilhe fontes com outros aplicativos do Windows](#fonts)
* [Inicie um processo Win32 de um aplicativo da UWP (Plataforma Universal do Windows)](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Faça o seu aplicativo aparecer como o alvo de impressão em aplicativos com suporte para impressão

Se os usuários desejarem imprimir dados de outro aplicativo, como o Bloco de notas, você pode fazer com que seu aplicativo apareça como um alvo de impressão na lista de destinos de impressão disponíveis do aplicativo.

Você terá que modificar seu aplicativo para que ele receba dados de impressão no formato XML Paper Specification (XPS).

#### <a name="xml-namespaces"></a>Namespaces XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.appPrinter">
    <AppPrinter
        DisplayName="[DisplayName]"
        Parameters="[Parameters]" />
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-desktop2-appprinter).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.appPrinter``.
|DisplayName |O nome que você quer que apareça na lista de destinos de impressão para um aplicativo. |
|Parâmetros |Os parâmetros que seu aplicativo requer para lidar apropriadamente com a solicitação. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:desktop2="http://schemas.microsoft.com/appx/manifest/desktop/windows10/2"
  IgnorableNamespaces="desktop2">
  <Applications>
  <Application>
    <Extensions>
      <desktop2:Extension Category="windows.appPrinter">
        <desktop2:AppPrinter
          DisplayName="Send to Contoso"
          Parameters="/insertdoc %1" />
      </desktop2:Extension>
    </Extensions>
  </Application>
</Applications>
</Package>
```

Encontrar um exemplo que usa essa extensão [aqui](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/PrintToPDF)

<a id="fonts" />

### <a name="share-fonts-with-other-windows-applications"></a>Compartilhe fontes com outros aplicativos do Windows

Compartilhe suas fontes personalizadas com outros aplicativos do Windows.

#### <a name="xml-namespaces"></a>Namespaces XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10/2`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.sharedFonts">
    <SharedFonts>
      <Font File="[FontFile]" />
    </SharedFonts>
  </Extension>
```

Encontre a referência do esquema completo [aqui](/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.sharedFonts``.
|Arquivo |O arquivo que contém as fontes que você deseja compartilhar. |

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap4="http://schemas.microsoft.com/appx/manifest/uap/windows10/4"
  IgnorableNamespaces="uap4">
  <Applications>
    <Application>
      <Extensions>
        <uap4:Extension Category="windows.sharedFonts">
          <uap4:SharedFonts>
            <uap4:Font File="Fonts\JustRealize.ttf" />
            <uap4:Font File="Fonts\JustRealizeBold.ttf" />
          </uap4:SharedFonts>
        </uap4:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<a id="win32-process" />

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Inicie um processo Win32 de um aplicativo da UWP (Plataforma Universal do Windows)

Inicie um processo do Win32 que é executado em confiança total.

#### <a name="xml-namespaces"></a>Namespaces XML

`http://schemas.microsoft.com/appx/manifest/desktop/windows10`

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fullTrustProcess" Executable="[executable file]">
  <FullTrustProcess>
    <ParameterGroup GroupId="[GroupID]" Parameters="[Parameters]"/>
  </FullTrustProcess>
</Extension>
```

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fullTrustProcess``.
|GroupID |Uma cadeia de caracteres que identifica um conjunto de parâmetros que você deseja passar para o executável. |
|Parâmetros |Parâmetros que você deseja passar para o executável. |

#### <a name="example"></a>Exemplo

```XML
<Package xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
         xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
         xmlns:rescap=
"http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
         xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10">
  ...
  <Capabilities>
      <rescap:Capability Name="runFullTrust"/>
  </Capabilities>
  <Applications>
    <Application>
      <Extensions>
          <desktop:Extension Category="windows.fullTrustProcess" Executable="fulltrustprocess.exe">
              <desktop:FullTrustProcess>
                  <desktop:ParameterGroup GroupId="SyncGroup" Parameters="/Sync"/>
                  <desktop:ParameterGroup GroupId="OtherGroup" Parameters="/Other"/>
              </desktop:FullTrustProcess>
           </desktop:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

Essa extensão pode ser útil se você deseja criar uma interface do usuário da Plataforma Universal do Windows que é executada em todos os dispositivos, mas deseja que os componentes do seu aplicativo Win32 continuem funcionando com confiança total.

Crie o pacote do aplicativo do Windows para seu aplicativo Win32. Em seguida, adicione esta extensão ao arquivo de pacote do seu aplicativo UWP. Essas extensões indicam que você deseja iniciar um arquivo executável no pacote do aplicativo do Windows.  Se você quiser se comunicar entre seu aplicativo UWP e seu aplicativo Win32, você poderá configurar um ou mais [serviços de aplicativos](/windows/uwp/launch-resume/app-services) para fazer isso. Você pode ler mais sobre esse cenário [aqui](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Próximas etapas

Tem dúvidas? Pergunte-nos no Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).
