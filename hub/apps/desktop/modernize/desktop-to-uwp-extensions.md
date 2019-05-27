---
Description: Você pode usar as extensões para integrar seu aplicativo da área de trabalho empacotado com o Windows 10 de maneiras predefinidas.
title: Integrar seu aplicativo empacotado para desktops com Windows 10 e UWP (ponte de Desktop)
ms.date: 04/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 063106362a36aa32d0874be15dd589b5fb573409
ms.sourcegitcommit: d1c3e13de3da3f7dce878b3735ee53765d0df240
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/24/2019
ms.locfileid: "66214943"
---
# <a name="integrate-your-packaged-desktop-app-with-windows-10-and-uwp"></a>Integrar seu aplicativo empacotado de desktop com Windows 10 e UWP

Se você [empacotar o aplicativo da área de trabalho em um contêiner de MSIX](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root), você pode usar extensões para integrar seu aplicativo de área de trabalho empacotado com o Windows 10 por meio de extensões predefinidas no [domanifestodepacotedoaplicativo](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/schema-root).

Por exemplo, use uma extensão para criar uma exceção de firewall, tornar o aplicativo padrão para um tipo de arquivo de seu aplicativo ou aponte blocos Iniciar para a versão empacotada do aplicativo. Para usar uma extensão, basta adicionar alguns XML ao arquivo de manifesto do pacote do seu aplicativo. Nenhum código é necessário.

Este artigo descreve essas extensões e as tarefas que você pode executar ao usá-los.

> [!NOTE]
> Os recursos descritos neste artigo exigem que você crie um pacote de aplicativo do Windows para o seu aplicativo da área de trabalho. Se você ainda não fez isso, consulte [empacotar aplicativos da área de trabalho](https://docs.microsoft.com/windows/msix/desktop/desktop-to-uwp-root).

## <a name="transition-users-to-your-app"></a>Transição de usuários para o seu aplicativo

Ajude na transição dos usuários para o aplicativo empacotado.

* [Aponte blocos existentes de início e de botões de barra de tarefas para seu aplicativo empacotado](#point)
* [Tornar seu aplicativo empacotado abrir arquivos, em vez de seu aplicativo de desktop](#make)
* [Associar o aplicativo empacotado com um conjunto de tipos de arquivo](#associate)
* [Adicionar opções para os menus de contexto de arquivos que têm um determinado tipo de arquivo](#add)
* [Abrir determinados tipos de arquivos diretamente por meio de uma URL](#open)

<a id="point" />

### <a name="point-existing-start-tiles-and-taskbar-buttons-to-your-packaged-app"></a>Aponte os blocos Iniciar e os botões da barra de tarefas existentes para o seu app empacotado

Seus usuários podem ter colocado seu aplicativo desktop na barra de tarefas ou no menu Iniciar. Você pode apontar esses atalhos para seu novo app empacotado.

#### <a name="xml-namespace"></a>Namespace XML

http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

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
|Category |Sempre ``windows.desktopAppMigration``.
|AumID |A ID de modelo de usuário do aplicativo do seu app empacotado. |
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

#### <a name="related-sample"></a>Exemplos relacionados

[Visualizador de imagens do WPF com a transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="make" />

### <a name="make-your-packaged-application-open-files-instead-of-your-desktop-app"></a>Tornar seu aplicativo empacotado abrir arquivos, em vez de seu aplicativo de desktop

Você pode ter certeza de que os usuários abram o novo aplicativo empacotado por padrão para tipos específicos de arquivos em vez de abrir a versão da área de trabalho do seu aplicativo.

Para fazer isso, você especifica o [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) de cada aplicativo do qual você deseja herdar associações de arquivos.

#### <a name="xml-namespaces"></a>Namespaces do XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
<FileTypeAssociation Name="[AppID]">
         <MigrationProgIds>
            <MigrationProgId>"[ProgID]"</MigrationProgId>
        </MigrationProgIds>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. Essa Id é usada internamente para gerar um [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) com hash associado à sua associação de tipo de arquivo. Você pode usar essa Id para gerenciar as alterações em versões futuras do seu aplicativo. |
|MigrationProgId |O [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) que descreve o aplicativo, o componente e a versão do aplicativo da área de trabalho do qual você deseja herdar as associações de arquivo.|

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
          <uap3:FileTypeAssociation Name="Contoso">
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

#### <a name="related-sample"></a>Exemplos relacionados

[Visualizador de imagens do WPF com a transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="associate" />

### <a name="associate-your-packaged-application-with-a-set-of-file-types"></a>Associar o aplicativo empacotado com um conjunto de tipos de arquivo

O aplicativo empacotado pode ser associada com extensões de tipo de arquivo. Se um usuário clica um arquivo e, em seguida, seleciona os **abrir com** opção, o aplicativo aparecer na lista de sugestões.

#### <a name="xml-namespace"></a>Namespace XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedFileTypes>
            <FileType>"[file extension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. Essa Id é usada internamente para gerar um [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) com hash associado à sua associação de tipo de arquivo. Você pode usar essa Id para gerenciar as alterações em versões futuras do seu aplicativo.   |
|FileType |A extensão de arquivo suportada por seu aplicativo. |

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
          <uap3:FileTypeAssociation Name="Contoso">
            <uap:SupportedFileTypes>
            <uap:FileType>.txt</uap:FileType>
            <uap:FileType>.avi</uap:FileType>
            </uap:SupportedFileTypes>
          </uap3:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

#### <a name="related-sample"></a>Exemplos relacionados

[Visualizador de imagens do WPF com a transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="add" />

### <a name="add-options-to-the-context-menus-of-files-that-have-a-certain-file-type"></a>Adicione opções aos menus de contexto dos arquivos que possuem um determinado tipo de arquivo

Na maioria dos casos, os usuários, clique duas vezes arquivos para abri-los. Se os usuários clicarem com o botão direito do mouse em um arquivo, várias opções aparecerão.

Você pode adicionar opções de menu. Essas opções oferecem aos usuários outras formas de interagir com seu arquivo, como imprimir, editar ou visualizar o arquivo.

#### <a name="xml-namespaces"></a>Namespaces do XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
        <SupportedVerbs>
           <Verb Id="[ID]" Extended="[Extended]" Parameters="[parameters]">"[verb label]"</Verb>
        </SupportedVerbs>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Category | Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|Verb |O nome que aparece no menu de contexto do Explorador de arquivos. Essa sequência é localizável usando ```ms-resource```.|
|Id |A Id exclusiva do verbo. Se seu aplicativo é um aplicativo UWP, isso é passado para o seu aplicativo como parte de seus argumentos do evento de ativação para que ele possa manipular a seleção do usuário adequadamente. Se seu aplicativo for um aplicativo empacotado de confiança total, ele recebe parâmetros em vez disso (veja o próximo marcador). |
|Parâmetros |A lista de parâmetros de argumento e valores associados ao verbo. Se seu aplicativo é um aplicativo empacotado de confiança total, esses parâmetros são passados para o aplicativo como argumentos do evento quando o aplicativo é ativado. Você pode personalizar o comportamento do seu aplicativo com base nos verbos de ativação diferentes. Se uma variável pode conter um caminho de arquivo, envolva o valor do parâmetro entre aspas. Isso evitará quaisquer problemas que aconteçam nos casos em que o caminho inclua espaços. Se seu aplicativo é um aplicativo UWP, você não pode passar parâmetros. O aplicativo recebe o Id em vez disso (veja a bala anterior).|
|Estendido |Especifica que o verbo só aparece se o usuário mostrar o menu de contexto segurando a tecla **Shift** antes de clicar com o botão direito no arquivo. Esse atributo é opcional e o padrão é um valor **False** (por exemplo, mostrar sempre o verbo) se não listado. Você especifica esse comportamento individualmente para cada verbo (com exceção de "Abrir", que é sempre **False**).|

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
          <uap3:FileTypeAssociation Name="Contoso">
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

#### <a name="related-sample"></a>Exemplos relacionados

[Visualizador de imagens do WPF com a transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<a id="open" />

### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Abra certos tipos de arquivos diretamente usando um URL

Você pode ter certeza de que os usuários abram o novo aplicativo empacotado por padrão para tipos específicos de arquivos em vez de abrir a versão da área de trabalho do seu aplicativo.

#### <a name="xml-namespaces"></a>Namespaces do XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3"

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" UseUrl="true" Parameters="%1">
        <SupportedFileTypes>
            <FileType>"[FileExtension]"</FileType>
        </SupportedFileTypes>
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|UseUrl |Indica se deve abrir arquivos diretamente de um destino de URL. Se você não definir esse valor, as tentativas por seu aplicativo para abrir um arquivo usando uma causa de URL o sistema para o download primeiro o arquivo localmente. |
|Parâmetros |parâmetros opcionais. |
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
            <uap3:FileTypeAssociation Name="documenttypes" UseUrl="true" Parameters="%1">
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

* [Criar a exceção de firewall para seu aplicativo](#rules)
* [Coloque os arquivos DLL em qualquer pasta do pacote](#load-paths)

<a id="rules" />

### <a name="create-firewall-exception-for-your-app"></a>Criar a exceção de firewall para o seu aplicativo

Se seu aplicativo requer comunicação por meio de uma porta, você pode adicionar seu aplicativo à lista de exceções do firewall.

#### <a name="xml-namespace"></a>Namespace XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

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
|Category |Sempre ``windows.firewallRules``|
|Executável |O nome do arquivo executável que deseja adicionar à lista de exceções de firewall |
|Direction |Indica se a regra é uma regra de entrada ou de saída |
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

Se você não usar essa extensão, o sistema pesquisará o gráfico de dependência de pacote do processo, a pasta raiz do pacote e o diretório do sistema ( _%SystemRoot%\system32_) nessa ordem. Para saber mais, consulte [Ordem de pesquisa de aplicativos do Windows](https://msdn.microsoft.com/library/windows/desktop/ms682586.aspx#_search_order_for_windows_store_apps).

Cada pacote pode conter apenas uma dessas extensões. Isso significa que você pode adicionar uma delas ao pacote principal e, em seguida, adicionar uma a cada um dos [pacotes opcionais e conjuntos relacionados](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

#### <a name="xml-namespace"></a>Namespace XML

http://schemas.microsoft.com/appx/manifest/uap/windows10/6

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
|Category |Sempre ``windows.loaderSearchPathOverride``.
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

## <a name="integrate-with-file-explorer"></a>Integração com o Explorador de arquivos

Ajude os usuários organizar seus arquivos e interagir com eles de maneiras familiares.

* [Definir como seu aplicativo se comporta quando os usuários selecionam e abrirem vários arquivos ao mesmo tempo](#define)
* [Mostrar o conteúdo do arquivo em uma imagem em miniatura dentro do Explorador de arquivos](#show)
* [Mostrar o conteúdo do arquivo em um painel de visualização do Explorador de arquivos](#preview)
* [Permitir que os usuários para agrupar arquivos por meio da coluna de tipo no Explorador de arquivos](#enable)
* [Disponibilizar as propriedades de arquivo para pesquisa, índice, caixas de diálogo de propriedade e o painel de detalhes](#make-file-properties)
* [Exibir arquivos de seu serviço de nuvem no Explorador de arquivos](#cloud-files)

<a id="define" />

### <a name="define-how-your-application-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Definir como seu aplicativo se comporta quando os usuários selecionam e abrirem vários arquivos ao mesmo tempo

Especifique como o seu aplicativo se comporta quando um usuário abrir vários arquivos simultaneamente.

#### <a name="xml-namespaces"></a>Namespaces do XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]" MultiSelectModel="[SelectionModel]">
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
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|MultiSelectModel |Consulte abaixo |
|FileType |As extensões de arquivo relevante. |

**MultSelectModel**

Os aplicativos da área de trabalho empacotados têm as mesmas três opções do que os aplicativos da área de trabalho comuns.

* ``Player``: Seu aplicativo é ativado uma vez. Todos os arquivos selecionados são passados para o aplicativo como parâmetros de argumento.
* ``Single``: Seu aplicativo é ativado uma vez para o primeiro arquivo selecionado. Outros arquivos são ignorados.
* ``Document``: Uma nova instância separada do seu aplicativo é ativada para cada arquivo selecionado.

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
          <uap3:FileTypeAssociation Name="myapp" MultiSelectModel="Document">
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

Se o usuário abrir 15 ou menos arquivos, a opção padrão para o atributo **MultiSelectModel** é *Player* . Caso contrário, o valor padrão é *Documento*. Os aplicativos UWP sempre são iniciados como *Player*.

<a id="show" />

### <a name="show-file-contents-in-a-thumbnail-image-within-file-explorer"></a>Mostrar o conteúdo do arquivo em uma imagem em miniatura dentro do Explorador de Arquivos

Permita que os usuários vejam uma imagem em miniatura do conteúdo do arquivo quando o ícone do arquivo aparecer no tamanho médio, grande ou extra grande.

#### <a name="xml-namespace"></a>Namespace XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
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
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
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
          <uap3:FileTypeAssociation Name="Contoso">
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

#### <a name="xml-namespace"></a>Namespace XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/2
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
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
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
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
          <uap3:FileTypeAssociation Name="Contoso">
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

### <a name="enable-users-to-group-files-by-using-the-kind-column-in-file-explorer"></a>Permita que os usuários agrupem arquivos usando a coluna Tipo no Explorador de Arquivos

Você pode associar um ou mais valores predefinidos para seus tipos de arquivo com o campo **Tipo**.

No Explorador de Arquivos, os usuários podem agrupar esses arquivos usando esse campo. Componentes do sistema também usam esse campo para várias finalidades, como de indexação.

Para obter mais informações sobre o campo **Tipo** e os valores que você pode usar para esse campo, consulte [usando nomes de tipo](https://msdn.microsoft.com/library/windows/desktop/cc144136.aspx).

#### <a name="xml-namespaces"></a>Namespaces do XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities/3

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<Extension Category="windows.fileTypeAssociation">
    <FileTypeAssociation Name="[AppID]">
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
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|FileType |As extensões de arquivo relevante. |
|value |Um [valor de tipo](https://msdn.microsoft.com/en-us/library/windows/desktop/cc144136.aspx#kind_hierarchy) válido |

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
           <uap:FileTypeAssociation Name="Contoso">
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

### <a name="make-file-properties-available-to-search-index-property-dialogs-and-the-details-pane"></a>Disponibilize as propriedades dos arquivos para pesquisa, índice, diálogos de propriedade e o painel de detalhes

#### <a name="xml-namespace"></a>Namespace XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10
* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<uap:Extension Category="windows.fileTypeAssociation">
    <uap:FileTypeAssociation Name="[AppID]">
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
|Category |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
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
          <uap3:FileTypeAssociation Name="Contoso">
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

<a id="cloud-files" />

### <a name="make-files-from-your-cloud-service-appear-in-file-explorer"></a>Fazer os arquivos de seu serviço de nuvem aparecerem no Explorador de Arquivos

Registre os manipuladores implementados em seu aplicativo. Você também pode adicionar opções de menu de contexto que aparecem quando os usuários clicam com o botão direito nos arquivos com base em nuvem no Explorador de Arquivos.

#### <a name="xml-namespace"></a>Namespace XML

* http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|Category |Sempre ``windows.cloudfiles``.
|iconResource |O ícone que representa seu serviço de provedor de arquivo em nuvem. Este ícone é exibido no painel de navegação do Explorador de Arquivos.  Os usuários escolhem esse ícone para mostrar arquivos de seu serviço de nuvem. |
|CustomStateHandler Clsid |A ID de classe do aplicativo que implementa o CustomStateHandler. O sistema usa esse ID de classe para solicitar estados personalizados e colunas para arquivos na nuvem. |
|ThumbnailProviderHandler Clsid |A ID de classe do aplicativo que implementa o ThumbnailProviderHandler. O sistema usa esse ID de classe para solicitar imagens em miniatura para arquivos na nuvem. |
|ExtendedPropertyHandler Clsid |A ID de classe do aplicativo que implementa o ExtendedPropertyHandler.  O sistema usa esse ID de classe para solicitar propriedades estendidas para um arquivo de nuvem. |
|Verb |O nome que aparece no menu de contexto do Explorador de Arquivos para arquivos fornecidos pelo seu serviço de nuvem. |
|Id |O ID exclusivo do verbo. |

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
* [Iniciar seu aplicativo usando um alias](#alias)
* [Inicie um arquivo executável quando os usuários fazem logon no Windows](#executable)
* [Permitir que os usuários iniciar o aplicativo quando se conectam a um dispositivo ao seu PC](#autoplay)
* [Reiniciar automaticamente depois de receber uma atualização da Microsoft Store](#updates)

<a id="protocol" />

### <a name="start-your-application-by-using-a-protocol"></a>Iniciar o aplicativo usando um protocolo

As associações de protocolos podem habilitar outros programas e componentes do sistema para interoperarem com seu app empacotado. Quando o aplicativo empacotado é iniciado usando um protocolo, você pode especificar parâmetros específicos para passar para seus argumentos de evento de ativação para que ele pode se comportar adequadamente. Os parâmetros têm suporte somente aos apps empacotados de confiança total. Os aplicativos UWP não podem usar parâmetros.

#### <a name="xml-namespace"></a>Namespace XML

http://schemas.microsoft.com/appx/manifest/uap/windows10/3

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
|Category |Sempre ``windows.protocol``.
|Nome |O nome do protocolo. |
|Parâmetros |A lista de parâmetros e valores a serem passados para o seu aplicativo como argumentos de evento quando o aplicativo é ativado. Se uma variável pode conter um caminho de arquivo, envolva o valor do parâmetro entre aspas. Isso evitará quaisquer problemas que aconteçam nos casos em que o caminho inclua espaços. |

### <a name="example"></a>Exemplo

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

<a id="alias" />

### <a name="start-your-application-by-using-an-alias"></a>Iniciar seu aplicativo usando um alias

Usuários e outros processos podem usar um alias para iniciar o aplicativo sem ter que especificar o caminho completo para seu aplicativo. Você pode especificar esse nome de alias.

#### <a name="xml-namespaces"></a>Namespaces do XML

* http://schemas.microsoft.com/appx/manifest/uap/windows10/3
* http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|Category |Sempre ``windows.appExecutionAlias``.
|Executável |O caminho relativo para o executável para iniciar quando o alias é invocado. |
|Alias |O nome abreviado para o seu aplicativo. Sempre deve terminar com a extensão ".exe". Você só pode especificar um único alias de execução de aplicativo para cada aplicativo no pacote. Se vários aplicativos se registrarem para o mesmo alias, o sistema irá chamar o último que foi registrado. Portanto, escolha um alias exclusivo que outros aplicativos provavelmente não substituirão.
|

#### <a name="example"></a>Exemplo

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
 
...
</Package>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

<a id="executable" />

### <a name="start-an-executable-file-when-users-log-into-windows"></a>Inicie um arquivo executável quando os usuários entrarem no Windows

Tarefas de inicialização permitem que seu aplicativo para executar um executável automaticamente sempre que um usuário faz logon.

> [!NOTE]
> O usuário tem que iniciar o aplicativo pelo menos uma vez para registrar a tarefa de inicialização.

Seu aplicativo pode declarar várias tarefas de inicialização. Cada tarefa é iniciada de maneira independente. Todas as tarefas de inicialização serão exibidas no Gerenciador de Tarefas na guia **Inicialização** com o nome especificado no manifesto do aplicativo e no ícone do aplicativo. O Gerenciador de Tarefas analisará automaticamente o impacto de inicialização de suas tarefas.

Os usuários podem desativar manualmente tarefas de inicialização do seu aplicativo usando o Gerenciador de tarefas. Se o usuário desabilitar uma tarefa, você não pode programaticamente reativá-la.

#### <a name="xml-namespace"></a>Namespace XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|Category |Sempre ``windows.startupTask``.|
|Executável |O caminho relativo para o arquivo executável para começar. |
|TaskId |Um identificador exclusivo para sua tarefa. Usando esse identificador, seu aplicativo pode chamar as APIs [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) classe para habilitar ou desabilitar uma tarefa de inicialização de forma programática. |
|Enabled |Indica se a tarefa inicialmente é iniciada ou desabilitada. As tarefas habilitadas serão executadas na próxima vez em que o usuário fizer logon (a menos que o usuário as tenha desabilitado). |
|DisplayName |O nome da tarefa que aparece no Gerenciador de tarefas. Você pode localizar essa cadeia de caracteres usando ```ms-resource```. |

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

### <a name="enable-users-to-start-your-application-when-they-connect-a-device-to-their-pc"></a>Permitir que os usuários iniciar o aplicativo quando se conectam a um dispositivo ao seu PC

Reprodução automática pode apresentar seu aplicativo como uma opção quando um usuário se conecta a um dispositivo para seus PCs.

#### <a name="xml-namespace"></a>Namespace XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/3

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
|Category |Sempre ``windows.autoPlayHandler``.
|ActionDisplayName |Uma cadeia de caracteres que representa a ação que os usuários podem executar com um dispositivo que se conectarem a um computador (por exemplo: "Arquivos de importação" ou "Reproduzir o vídeo"). |
|ProviderDisplayName | Uma cadeia de caracteres que representa seu aplicativo ou serviço (por exemplo: "Player de vídeo Contoso"). |
|ContentEvent |O nome de um evento de conteúdo que faz com que os usuários sejam avisados com ``ActionDisplayName`` e ``ProviderDisplayName``. Um evento de conteúdo é gerado quando um dispositivo de volume, como um cartão de memória de câmera, um pen drive ou um DVD, for inserido no computador. Você pode encontrar a lista completa desses eventos [aqui](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference).  |
|Verb |A configuração de verbo identifica um valor que é passado para o seu aplicativo para a opção selecionada. Você pode especificar várias ações de inicialização para um evento de Reprodução Automática e usar a configuração Verbo para determinar qual opção u usuário selecionou para seu aplicativo. Você pode descobrir a opção selecionada pelo usuário verificando a propriedade verb dos argumentos do evento de inicialização passados para seu aplicativo. Também pode usar qualquer valor para a configuração Verbo exceto open, que está reservado. |
|DropTargetHandler |A ID de classe do aplicativo que implementa o [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) interface. Arquivos da mídia removível são transmitidos para o método [Soltar](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget.drop?view=visualstudiosdk-2017#Microsoft_VisualStudio_OLE_Interop_IDropTarget_Drop_Microsoft_VisualStudio_OLE_Interop_IDataObject_System_UInt32_Microsoft_VisualStudio_OLE_Interop_POINTL_System_UInt32__) da implementação [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017).  |
|Parâmetros |Você não precisa implementar a interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017) para todos os eventos de conteúdo. Para qualquer um dos eventos de conteúdo, você pode fornecer parâmetros de linha de comando em vez de implementar a interface [IDropTarget](https://docs.microsoft.com/dotnet/api/microsoft.visualstudio.ole.interop.idroptarget?view=visualstudiosdk-2017). Para esses eventos, a reprodução automática iniciará o aplicativo usando esses parâmetros de linha de comando. Você pode analisar esses parâmetros no código de inicialização do seu aplicativo para determinar se ele foi iniciado por reprodução automática e, em seguida, fornecer sua implementação personalizada. |
|DeviceEvent |O nome de um evento de dispositivo que faz com que os usuários sejam avisados com ``ActionDisplayName`` e ``ProviderDisplayName``. Um evento de dispositivo é gerado quando um dispositivo é conectado ao computador. Eventos de dispositivo começam com a cadeia de caracteres ``WPD`` e você pode encontrá-los listados [aqui](https://docs.microsoft.com/windows/uwp/launch-resume/auto-launching-with-autoplay#autoplay-event-reference). |
|HWEventHandler |A ID de classe do aplicativo que implementa o [IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx) interface. |
|InitCmdLine |O parâmetro de cadeia de caracteres que você deseja transmitir para o método [Inicializar](https://msdn.microsoft.com/en-us/library/windows/desktop/bb775495.aspx) da interface [IHWEventHandler](https://msdn.microsoft.com/library/windows/desktop/bb775492.aspx). |

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

### <a name="restart-automatically-after-receiving-an-update-from-the-microsoft-store"></a>Reinicie automaticamente depois de receber uma atualização da Microsoft Store

Se seu aplicativo estiver aberto quando os usuários instalarem uma atualização a ele, o aplicativo é fechado.

Se você quiser que esse aplicativo reiniciar após a atualização for concluída, chame o [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) função em cada processo que você deseja reiniciar.

Cada janela ativa no seu aplicativo recebe um [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx) mensagem. Neste ponto, seu aplicativo pode chamar o [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) função novamente para atualizar a linha de comando, se necessário.

Quando cada janela ativa no seu aplicativo recebe o [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) mensagem, seu aplicativo deve salvar os dados e desligar.

>[!NOTE]
As janelas ativas também recebem o [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) da mensagem, caso o aplicativo não lida com o [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx) mensagem.

Neste ponto, seu aplicativo tem 30 segundos para fechar a seus próprios processos ou a plataforma encerrá-los de modo forçado.

Após a atualização for concluída, seu aplicativo é reiniciado.

## <a name="work-with-other-applications"></a>Trabalhar com outros aplicativos

Integre-se com outros aplicativos, inicie outros processos ou compartilhe informações.

* [Tornar seu aplicativo aparecem como o destino de impressão em aplicativos que dão suporte à impressão](#printing)
* [Fontes de compartilhamento com outros aplicativos do Windows](#fonts)
* [Iniciar um processo do Win32 de um aplicativo da plataforma Universal do Windows (UWP)](#win32-process)

<a id="printing" />

### <a name="make-your-application-appear-as-the-print-target-in-applications-that-support-printing"></a>Tornar seu aplicativo aparecem como o destino de impressão em aplicativos que dão suporte à impressão

Quando os usuários desejam imprimir os dados de outro aplicativo, como o bloco de notas, você pode fazer com que seu aplicativo aparecem como um destino de impressão na lista de destinos de impressão disponíveis do aplicativo.

Você precisará modificar seu aplicativo para que ele receba dados de impressão no formato XML Paper Specification (XPS).

#### <a name="xml-namespaces"></a>Namespaces do XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

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
|Category |Sempre ``windows.appPrinter``.
|DisplayName |O nome que você quer que apareça na lista de destinos de impressão para um aplicativo. |
|Parâmetros |Quaisquer parâmetros que seu aplicativo requer para lidar adequadamente com a solicitação. |

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

#### <a name="xml-namespaces"></a>Namespaces do XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10/2

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
|Category |Sempre ``windows.sharedFonts``.
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

### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Inicie um processo Win32 a partir de um aplicativo Universal Windows Platform (UWP)

Inicie um processo do Win32 que é executado em confiança total.

#### <a name="xml-namespaces"></a>Namespaces do XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

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
|Category |Sempre ``windows.fullTrustProcess``.
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

Esta extensão pode ser útil se você quiser criar uma interface de usuário de plataforma Windows Universal que é executado em todos os dispositivos, mas deseja que os componentes do aplicativo Win32 continuar em execução em confiança total.

Basta crie um pacote de aplicativo do Windows para seu aplicativo do Win32. Em seguida, adicione esta extensão ao arquivo de pacote do seu aplicativo UWP. Estas extensões indica que você deseja iniciar um arquivo executável no pacote de aplicativo do Windows.  Se você quiser se comunicar entre seu aplicativo UWP e seu aplicativo Win32, você pode configurar um ou mais [serviços de aplicativo](/windows/uwp/launch-resume/app-services.md) para fazer isso. Você pode ler mais sobre esse cenário [aqui ](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
