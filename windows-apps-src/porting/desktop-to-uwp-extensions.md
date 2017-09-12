---
author: normesta
Description: "Você pode usar as extensões para integrar seu aplicativo da área de trabalho empacotado com o Windows 10 de maneiras predefinidas."
Search.Product: eADQiWindows 10XVcnh
title: Integrar seu app com o Windows 10 (Ponte de Desktop)
ms.author: normesta
ms.date: 05/25/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 0a8cedac-172a-4efd-8b6b-67fd3667df34
ms.openlocfilehash: 0c3427a7b49b17fda9a3ba0680e59b134732e9fa
ms.sourcegitcommit: 38ef208ef457ce1857038c9cde3658c884d29b75
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/13/2017
---
# <a name="integrate-your-app-with-windows-10-desktop-bridge"></a>Integrar seu app com o Windows 10 (Ponte de Desktop)

Use extensões para integrar seu aplicativo com o Windows 10 de maneiras predefinidas.

Por exemplo, use uma extensão para criar uma exceção de firewall, faça seu aplicativo o aplicativo padrão para um tipo de arquivo ou aponte telas para a versão empacotada do seu aplicativo. Para usar uma extensão, basta adicionar alguns XML ao arquivo de manifesto do pacote do seu aplicativo. Nenhum código é necessário.

Este tópico descreve essas extensões e as tarefas que você pode executar usando-as.

## <a name="transition-users-to-your-app"></a>Transição de usuários para o seu aplicativo

Ajude na transição dos usuários para o aplicativo empacotado.

* [Aponte os blocos Iniciar e os botões da barra de tarefas existentes para o seu app empacotado](#point)
* [Faça o seu aplicativo empacotado abrir arquivos em vez do seu aplicativo da área de trabalho](#make)
* [Associe seu app empacotado a um conjunto de tipos de arquivos](#associate)
* [Adicione opções aos menus de contexto dos arquivos que possuem um determinado tipo de arquivo](#add)
* [Abra certos tipos de arquivos diretamente usando um URL](#open)

<span id="point" />
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

Encontre a referência do esquema completo [aqui ](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-rescap3-desktopappmigration).

|Nome | Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.desktopAppMigration``.
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

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="make" />
### <a name="make-your-packaged-app-open-files-instead-of-your-desktop-app"></a>Faça o seu aplicativo empacotado abrir arquivos em vez do seu aplicativo da área de trabalho

Você pode certificar-se de que os usuários abrem seu novo app empacotado por padrão para tipos específicos de arquivos ao invés de abrir a versão da área de trabalho do seu app.

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
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. Essa Id é usada internamente para gerar um [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) com hash associado à sua associação de tipo de arquivo. Você pode usar este Id para gerenciar as alterações nas versões futuras do seu aplicativo. |
|MigrationProgId |O [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) que descreve o aplicativo, o componente e a versão do aplicativo da área de trabalho do qual você quer herdar associações de arquivo.|

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

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="associate" />
### <a name="associate-your-packaged-app-with-a-set-of-file-types"></a>Associe seu app empacotado a um conjunto de tipos de arquivos

Seu app empacotado pode ser associado a extensões de tipo de arquivo. Se um usuário clicar com o botão direito do mouse em um arquivo e depois selecionar a opção **Abrir com**, seu aplicativo aparecerá na lista de sugestões.

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
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. Essa Id é usada internamente para gerar um [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) com hash associado à sua associação de tipo de arquivo. Você pode usar este Id para gerenciar as alterações nas versões futuras do seu aplicativo.   |
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

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="add" />
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
|Categoria | Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|Verb |O nome que aparece no menu de contexto do Explorador de arquivos. Essa sequência é localizável usando ```ms-resource```.|
|Id |A Id exclusiva do verbo. Se seu aplicativo for um aplicativo UWP, isso será transmitido para seu aplicativo como parte de seus argumentos do evento de ativação para que ele possa manipular a seleção do usuário de forma adequada. Se o seu app for um app empacotado totalmente confiável, ele receberá parâmetros (veja o próximo marcador). |
|Parâmetros |A lista de parâmetros de argumento e valores associados ao verbo. Se o seu aplicativo for um app empacotado totalmente confiável, esses parâmetros serão passados para o app como eventos quando o app for ativado. Você pode personalizar o comportamento do seu aplicativo com base em diferentes verbos de ativação. Se uma variável pode conter um caminho de arquivo, envolva o valor do parâmetro entre aspas. Isso evitará quaisquer problemas que aconteçam nos casos em que o caminho inclua espaços. Se o seu aplicativo for um aplicativo UWP, você não pode passar os parâmetros. O aplicativo recebe o Id em vez disso (veja a bala anterior).|
|Estendido |Especifica que o verbo só aparece se o usuário mostrar o menu de contexto segurando a tecla **Shift** antes de clicar com o botão direito no arquivo. Esse atributo é opcional e o padrão é um valor **False** (por exemplo, mostrar sempre o verbo) se não listado. Você especifica esse comportamento individualmente para cada verbo (exceto para "Abrir", que sempre será **False**).|

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

[Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition)

<span id="open" />
### <a name="open-certain-types-of-files-directly-by-using-a-url"></a>Abra certos tipos de arquivos diretamente usando um URL

Você pode certificar-se de que os usuários abrem seu novo app empacotado por padrão para tipos específicos de arquivos ao invés de abrir a versão da área de trabalho do seu app.

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
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|UseUrl |Indica se deve abrir arquivos diretamente de um destino de URL. Se você não definir esse valor, as tentativas do seu aplicativo para abrir um arquivo usando uma URL fazem com que o sistema primeiro faça o download do arquivo localmente. |
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

* [Criar a exceção de firewall para o seu aplicativo](#rules)

<span id="rules" />
### <a name="create-firewall-exception-for-your-app"></a>Criar a exceção de firewall para o seu aplicativo

Se o seu aplicativo precisar de comunicação através de uma porta, você pode adicionar seu aplicativo à lista de exceções de firewall.

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
|Categoria |Sempre ``windows.firewallRules``|
|Executável |O nome do arquivo executável que deseja adicionar à lista de exceções de firewall |
|Direção |Indica se a regra é uma regra de entrada ou de saída |
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

## <a name="integrate-with-file-explorer"></a>Integração com o Explorador de arquivos

Ajude os usuários organizar seus arquivos e interagir com eles de maneiras familiares.

* [Defina como seu aplicativo se comporta quando os usuários selecionam e abrem vários arquivos ao mesmo tempo](#define)
* [Mostrar o conteúdo do arquivo em uma imagem em miniatura dentro do Explorador de Arquivos](#show)
* [Mostrar o conteúdo do arquivo em um painel de visualização do Explorador de Arquivos](#preview)
* [Permita que os usuários agrupem arquivos usando a coluna Tipo no Explorador de Arquivos](#enable)
* [Disponibilize as propriedades dos arquivos para pesquisa, índice, diálogos de propriedade e o painel de detalhes](#make-file-properties)

<span id="define" />
### <a name="define-how-your-app-behaves-when-users-select-and-open-multiple-files-at-the-same-time"></a>Defina como seu aplicativo se comporta quando os usuários selecionam e abrem vários arquivos ao mesmo tempo

Especifique como seu aplicativo se comporta quando um usuário abre vários arquivos simultaneamente.

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
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|MultiSelectModel |Consulte abaixo |
|FileType |As extensões de arquivo relevante. |

**MultSelectModel**

Os aplicativos da área de trabalho empacotados têm as mesmas três opções do que os aplicativos da área de trabalho comuns.

 * ``Player``: O aplicativo é ativado uma vez. Todos os arquivos selecionados são passados para o seu aplicativo como parâmetros de argumento.
 * ``Single``: seu aplicativo é ativado uma vez para o primeiro arquivo selecionado. Outros arquivos são ignorados.
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

Se o usuário abrir 15 ou menos arquivos, a opção padrão para o atributo **MultiSelectModel** é *Player *. Caso contrário, o valor padrão é *Documento*. Os aplicativos UWP sempre são iniciados como *Player*.

<span id="show" />
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
            Clsid  ="[Clsid  ]"
            Cutoff="[Cutoff]"
            Treatment="[Treatment]" />
    </FileTypeAssociation>
</Extension>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. |
|FileType |As extensões de arquivo relevante. |
|Clsid   |A ID de classe do seu aplicativo. |
|Corte |O tamanho abaixo do qual uma imagem em miniatura não é usada. Consulte [dimensionamento e cache de miniaturas](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#cache) |
|Tratamento |O [adorno de miniatura](https://msdn.microsoft.com/library/windows/desktop/cc144118.aspx#adornments) que define a aparência do ícone em miniatura. |

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
              Clsid  ="20000000-0000-0000-0000-000000000001"
              Cutoff="20x20"
              Treatment="Video Sprockets" />
            </uap3:FileTypeAssociation>
         </uap::Extension>
      </Extensions>
    </Application>
  </Applications>
</Package>
```

<span id="preview" />
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
|Categoria |Sempre ``windows.fileTypeAssociation``.
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

<span id="enable" />
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
|Categoria |Sempre ``windows.fileTypeAssociation``.
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
<span id="make-file-properties" />
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
        <DesktopPropertyHandler Clsid ="[Clsid ]"/>
    </uap:FileTypeAssociation>
</uap:Extension>
```
**Descrições de elemento-chave e atributo**

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
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

<span id="start" />
## <a name="start-your-app-in-different-ways"></a>Inicie seu aplicativo de maneiras diferentes

* [Inicie seu aplicativo usando um protocolo](#protocol)
* [Inicie seu aplicativo usando um alias](#alias)
* [Inicie um arquivo executável quando os usuários fazem logon no Windows](#executable)
* [Reinicie automaticamente depois de receber uma atualização da Windows Store](#updates)

<span id="protocol" />
### <a name="start-your-app-by-using-a-protocol"></a>Inicie seu aplicativo usando um protocolo

As associações de protocolos podem habilitar outros programas e componentes do sistema para interoperarem com seu app empacotado. Quando seu app empacotado é iniciado usando um protocolo, você pode especificar parâmetros específicos para passar aos seus argumentos de evento de ativação para que ele possa se comportar de acordo. Os parâmetros têm suporte somente aos apps empacotados de confiança total. Os aplicativos UWP não podem usar parâmetros.  

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
|Categoria |Sempre ``windows.protocol``.
|Nome |O nome do protocolo. |
|Parâmetros |A lista de parâmetros e valores a serem passados para o seu aplicativo como argumentos de evento quando o aplicativo está ativado. Se uma variável pode conter um caminho de arquivo, envolva o valor do parâmetro entre aspas. Isso evitará quaisquer problemas que aconteçam nos casos em que o caminho inclua espaços. |

### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  IgnorableNamespaces="uap3">
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
<span id="alias" />
### <a name="start-your-app-by-using-an-alias"></a>Inicie seu aplicativo usando um alias

Usuários e outros processos podem usar um alias para iniciar seu aplicativo sem precisar especificar o caminho completo para seu aplicativo. Você pode especificar esse nome de alias.

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
|Categoria |Sempre ``windows.appExecutionAlias``.
|Executável |O caminho relativo para o executável para iniciar quando o alias é invocado. |
|Alias |O nome abreviado para o seu aplicativo. Deve sempre terminar com a extensão ".exe". Você só pode especificar um único alias de execução de aplicativo para cada aplicativo no pacote. Se vários aplicativos se registrarem para o mesmo alias, o sistema irá chamar o último que foi registrado. Portanto, escolha um alias exclusivo que outros aplicativos provavelmente não substituirão.
|

#### <a name="example"></a>Exemplo

```XML
<Package
  xmlns:uap3="http://schemas.microsoft.com/appx/manifest/uap/windows10/3"
  xmlns:desktop="http://schemas.microsoft.com/appx/manifest/desktop/windows10"
  IgnorableNamespaces="uap3, desktop">
  ...
  <uap3:Extension
        Category="windows.appExecutionAlias"
        Executable="exes\launcher.exe"
        EntryPoint="Windows.FullTrustApplication">
        <uap3:AppExecutionAlias>
            <desktop:ExecutionAlias Alias="Contoso.exe" />
        </uap3:AppExecutionAlias>
  </uap3:Extension>
...
</Package>
```

Encontre a referência do esquema completo [aqui](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap-filetypeassociation).

|Nome |Descrição |
|-------|-------------|
|Categoria |Sempre ``windows.fileTypeAssociation``.
|Nome |Uma Id exclusiva para o seu aplicativo. Essa Id é usada internamente para gerar um [identificador programático (ProgID)](https://msdn.microsoft.com/library/windows/desktop/cc144152.aspx) com hash associado à sua associação de tipo de arquivo. Você pode usar esta Id para gerenciar as alterações nas versões futuras do seu app.   |
|FileType |A extensão de arquivo suportada por seu aplicativo. |

<span id="executable" />
### <a name="start-an-executable-file-when-users-log-into-windows"></a>Iniciar um arquivo executável quando os usuários fazem logon no Windows

As tarefas de inicialização permitem que seu aplicativo execute um executável automaticamente sempre que um usuário faz logon.

> [!NOTE]
> O usuário deve iniciar seu aplicativo pelo menos uma vez para registrar essa tarefa de inicialização.

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
|Categoria |Sempre ``windows.startupTask``.|
|Executável |O caminho relativo para o arquivo executável para começar. |
|TaskId |Um identificador exclusivo para sua tarefa. Usando esse identificador, seu aplicativo pode chamar as APIs na classe [Windows.ApplicationModel.StartupTask](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.StartupTask) para habilitar ou desabilitar uma tarefa de inicialização de forma programática. |
|Habilitado |Indica se a tarefa inicialmente é iniciada ou desabilitada. As tarefas habilitadas serão executadas na próxima vez que o usuário efetuar o login (a menos que o usuário o desabilite). |
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

<span id="updates" />
### <a name="restart-automatically-after-receiving-an-update-from-the-windows-store"></a>Reinicie automaticamente depois de receber uma atualização da Windows Store

Se seu app estiver aberto quando os usuários instalarem uma atualização para ele, o app será fechado.

Se você quiser que o app reinicie após a conclusão da atualização, chame a função [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) em cada processo que você deseja reiniciar.

Cada janela ativa em seu aplicativo recebe uma mensagem [WM_QUERYENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376890.aspx). Neste ponto, seu app pode chamar a função [RegisterApplicationRestart](https://msdn.microsoft.com/library/windows/desktop/aa373347.aspx) novamente para atualizar a linha de comando, se necessário.

Quando cada janela ativa em seu app receber a mensagem [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx), seu app deverá salvar os dados e desligar.

>[!NOTE]
As janelas ativas também receberão a mensagem [WM_CLOSE](https://msdn.microsoft.com/library/windows/desktop/ms632617.aspx) caso o app não manipule a mensagem [WM_ENDSESSION](https://msdn.microsoft.com/library/windows/desktop/aa376889.aspx).

Neste ponto, seu app tem 30 segundos para fechar seu próprio processos ou a plataforma os encerrará de forma imposta.

Após a conclusão da atualização, seu app será reiniciado.

## <a name="work-with-other-applications"></a>Trabalhar com outros aplicativos

Integre-se com outros aplicativos, inicie outros processos ou compartilhe informações.

* [Faça o seu aplicativo aparecer como o alvo de impressão em aplicativos que suportem a impressão](#printing)
* [Compartilhe fontes com outros aplicativos do Windows](#fonts)
* [Inicie um processo Win32 a partir de um aplicativo Universal Windows Platform (UWP)](#win32-process)

<span id="printing" />
### <a name="make-your-app-appear-as-the-print-target-in-applications-that-support-printing"></a>Faça o seu aplicativo aparecer como o alvo de impressão em aplicativos que suportem a impressão

Se os usuários desejarem imprimir dados de outro aplicativo, como o Bloco de notas, você pode fazer com que seu aplicativo apareça como um alvo de impressão na lista de destinos de impressão disponíveis do aplicativo.

Você terá que modificar seu aplicativo para que ele receba dados de impressão no formato XML Paper Specification (XPS).

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

<span id="fonts" />
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

Encontre a referência do esquema completo [aqui](https://review.docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap4-sharedfonts).


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
<span id="win32-process" />
### <a name="start-a-win32-process-from-a-universal-windows-platform-uwp-app"></a>Inicie um processo Win32 a partir de um aplicativo Universal Windows Platform (UWP)

Inicie um processo do Win32 que é executado em confiança total.

#### <a name="xml-namespaces"></a>Namespaces do XML

http://schemas.microsoft.com/appx/manifest/desktop/windows10

#### <a name="elements-and-attributes-of-this-extension"></a>Elementos e atributos da extensão

```XML
<xtension Category="windows.fullTrustProcess" Executable="[executable file]">
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

Basta criar um pacote de ponte de desktop para seu aplicativo Win32. Em seguida, adicione esta extensão ao arquivo de pacote do seu aplicativo UWP. Essas extensões indicam que deseja iniciar um arquivo executável no pacote de bridge de desktop.  Se você quiser se comunicar entre seu aplicativo UWP e seu aplicativo Win32, você pode configurar um ou mais [serviços de aplicativo](../launch-resume/app-services.md) para fazer isso. Você pode ler mais sobre esse cenário [aqui ](https://blogs.msdn.microsoft.com/appconsult/2016/12/19/desktop-bridge-the-migrate-phase-invoking-a-win32-process-from-a-uwp-app/).

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para perguntas específicas**

Nossa equipe monitora os [sinalizadores de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.
