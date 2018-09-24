---
author: stevewhims
Description: MakePri.exe has the set of commands createconfig, dump, new, resourcepack, and versioned. This topic details their use.
title: Opções de linha de comando do MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 04/10/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: c0a3892348baff56bbef8d40dd9aade4e612c50d
ms.sourcegitcommit: 194ab5aa395226580753869c6b66fce88be83522
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/24/2018
ms.locfileid: "4148645"
---
# <a name="makepriexe-command-line-options"></a>Opções de linha de comando do MakePri.exe

[MakePri.exe](compile-resources-manually-with-makepri.md) tem o conjunto de comandos `createconfig`, `dump`, `new`, `resourcepack` e `versioned`. Este tópico descreve detalhadamente as opções de linha de comando para seu uso.

> [!NOTE]
> MakePri.exe é instalado quando você verificar a opção de **SDK do Windows para aplicativos gerenciados do UWP** ao instalar o Software Development Kit do Windows. Ele é instalado no caminho `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (bem como nas pastas nomeadas para as outras arquiteturas). Por exemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

## <a name="makepri-commands"></a>Comandos MakePri

Execute `MakePri.exe help` para ver os comandos que você pode usar com o MakePri.exe.

```
C:\>makepri help

Usage:
------
    MakePri.exe <command> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ /in PackageName

Description:
------------
    Creates, dumps, and performs utility functions on a PRI file. A PRI file is 
    an index of application resources, such as strings and image files.

Command Options:
----------------
    MakePri.exe createconfig   Creates a PRI config file for use with other
                               commands
    MakePri.exe dump           Dumps the contents of a PRI file
    MakePri.exe new            Creates a new PRI file from scratch
    MakePri.exe resourcepack   Creates a PRI file that contains additional
                               resource variants for a base PRI file
    MakePri.exe versioned      Creates a PRI file based on a previous version

Help:
-----
    MakePri.exe help           Show this help page
    MakePri.exe <command> /?   Shows detailed help for <command>

    For example,
    MakePri.exe createconfig /?
```

## <a name="createconfig-command"></a>Comando Createconfig

O comando `createconfig` cria um arquivo de configuração PRI novo e inicializado definindo os padrões de qualificador especificados. Execute `MakePri.exe createconfig /?` para ver uma ajuda detalhada deste comando.

```
C:\>makepri createconfig /?

Usage:
------
    MakePri.exe createconfig /cf <config file destination> /dq
    <default qualifiers> [options]

Example:
--------
    MakePri.exe createconfig /cf C:\MyApp\priconfig.xml /dq lang-en-US /o /pv 10.0.0

Description:
------------
    Creates a PRI configuration file at <config file destination> with default 
    qualifiers specified by <default qualifiers>. Multiple qualifiers are separated 
    by underscores (_)

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file output location
    /Default(dq)      : <QUALIFIERS> The default qualifiers to set in the
                        configuration file. A language qualifier is required

Options:
--------
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /Platform(pv)     : <VERSION> Platform version to use for generated
                        configuration file

    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    QUALIFIERS        - a valid qualifier token
                        (for example, lang-en-US_scale-100_contrast-high)

Help:
-----
    /Help(h, ?)       : Display the usage help text
```

## <a name="dump-command"></a>Comando dump

O comando `dump` gera um arquivo xml despejado contendo uma lista de todos os recursos em um arquivo PRI especificado. Execute `MakePri.exe dump /?` para ver uma ajuda detalhada deste comando.

> [!NOTE]
> Um pacote de recursos sem esquema é aquele que foi criado com a opção *omitSchemaFromResourcePacks* no arquivo de configuração PRI. Para despejar um pacote de recursos sem esquema, use a opção `/es <main_package_PRI_file>`. Se você não especificar o arquivo principal, verá a mensagem de erro "*The resources.pri in the package was corrupted so encryption failed (error PRI222: 0xdef0000f - Unspecified error occurred)*".

```
C:\>makepri dump /?

Usage:
------
    MakePri.exe dump [options]

Example:
--------
    MakePri.exe dump /if C:\MyApp\resources.pri /of C:\resources.pri.xml

Description:
------------
    Outputs a dumped xml file at <output file> containing a list of all 
    resources in <index file>.

Options:
--------
    /DumpType(dt)       : <STRING> Format of the dumped file, default is
                          Basic
    /ExtensionDll(ex)   : <FILEPATH> Location of the Resource Management System
                          environment extension DLL. This DLL must be signed by a
                          Microsoft-issued certificate. Default is an empty path
                          (no DLL will be used)
    /ExternalSchema(es) : <FILEPATH> Location of the external schema file
    /IndexFile(if)      : <FILEPATH> Location of the PRI file to dump from.
                          Default is .\resources.pri
    /OutputFile(of)     : <FILEPATH> Output location of the dump file, default
                          is .\[indexfile].xml
    /OutputOptions(oo)  : <OPTIONS> Options to provide detailed control over
                          contents of XML output files.
    /Overwrite(o)       : Overwrite an existing output file of the same name
                          without prompting
    /Verbose(v)         : Causes verbose messages to be output to the console

    Dump Type:
        Either 'Basic', 'Detailed', 'Schema', or 'Summary'

    FILEPATH            - a path to a file, either relative to the current
                          directory or absolute
Help:
-----
    /Help(h, ?)         : Display the usage help text
```

## <a name="new-command"></a>Comando new

O comando `new` cria um novo arquivo PRI indexando os arquivos do projeto, conforme indicado pelo arquivo de configuração. Execute `MakePri.exe new /?` para ver uma ajuda detalhada deste comando.

```
C:\>makepri new /?

Usage:
------
    MakePri.exe new /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe new /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src\ 
    /mn C:\MyApp\AppXManifest.xml /o /of C:\MyApp\src\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in
    <project root> and its subdirectories as directed by <config file>. The
    index will be assigned <index name> to reference resources in the app

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use the
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. Default is not set.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexName(in)    : <STRING> Name for the generated index of resources.
                        Typically matches the AppX package name, class library
                        simple name, etc. May be supplied via the
                        [manifest] parameter.
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /Manifest(mn)     : <FILEPATH> Location of the application or component's
                        manifest. This parameter is ignored if [indexname]
                        is given. Default is [projectroot]\AppXManifest.xml
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console
    /VersionMajor(vma): <INTEGER> [Deprecated] Major version number for
                        index, default is 1

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="resourcepack-command"></a>Comando ResourcePack

O comando `resourcepack` cria um novo arquivo PRI indexando os arquivos do projeto, conforme indicado pelo arquivo de configuração. Um arquivo PRI de pacote de recursos contém apenas as variantes adicionais de recursos já especificadas em um arquivo PRI existente. Execute `MakePri.exe resourcepack /?` para ver uma ajuda detalhada deste comando.

```
C:\>makepri resourcepack /?

Usage:
------
    MakePri.exe resourcepack /pr <project root> /cf <config file> [options]

Example:
--------
    MakePri.exe resourcepack /cf C:\MyAppEs\priconfig.xml /pr C:\MyAppEs\src\ 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyAppEs\resources.pri

Description:
------------
    Creates a PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>. A 
    resource pack PRI file contains only additional variants of resources 
    already specified in <index file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file.
                        Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        .\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="versioned-command"></a>Comando versioned

O comando `versioned` cria um arquivo PRI com controle de versão indexando os arquivos do projeto, conforme indicado pelo arquivo de configuração. Execute `MakePri.exe versioned /?` para ver uma ajuda detalhada deste comando.

```
C:\>makepri versioned /?

Usage:
------
    MakePri.exe versioned /cf <config file> /pr <project root> [options]

Example:
--------
    MakePri.exe versioned /cf C:\MyApp\priconfig.xml /pr C:\MyApp\src 
    /if C:\MyApp\1.2\resources.pri /o /of C:\MyApp\src\resources.pri /o

Description:
------------
    Creates a versioned PRI file at <output file> by indexing all files in 
    <project root> and its subdirectories as directed by <config file>.

Required Parameters:
--------------------
    /ConfigXml(cf)    : <FILEPATH> Configuration file location. Use
                        'Makepri.exe createconfig' command to generate one
    /ProjectRoot(pr)  : <FOLDERPATH> Root location of project files

Options:
--------
    /AutoMerge(am)    : This flag is not recommended for normal use with AppX
                        packages. It causes Makepri.exe to set the auto
                        merge flag within the PRI file. By default it is set
                        to same as the base PRI file.
    /ExtensionDll(ex) : <FILEPATH> Location of the Resource Management System
                        environment extension DLL. This DLL must be signed by
                        a Microsoft-issued certificate. Default is an empty path
                        (no DLL will be used)
    /IndexFile(if)    : <FILEPATH> Location of the base PRI or XML schema file
                        to version from. Default is <ProjectRoot>\resources.pri
    /IndexLog(il)     : <FILEPATH> XML Log of indexed resources, no file
                        generated by default
    /IndexOptions(io) : <OPTIONS> Options to provide detailed control over
                        behavior of resource indexers.
    /MappingFile(mf)  : <MAPPINGFILETYPE> Generate a mapping file in the given
                        file format.
    /OutputFile(of)   : <FILEPATH> Output location of PRI file, default is
                        [current directory]\resources.pri
    /Overwrite(o)     : Overwrite an existing output file of the same name
                        without prompting
    /ReverseMap(rm)   : Generate a reverse mapping section in the PRI file
                        which can be used for debugging purposes.
    /SchemaFile(sf)   : <FILEPATH> Output location of XML resource schema
                        description.
    /Verbose(v)       : Causes verbose messages to be output to the console

    FOLDERPATH        - a path to a folder
    FILEPATH          - a path to a file, either relative to the current
                        directory or absolute
    MAPPINGFILETYPE   - Supported File type(s): 'AppX'

Help:
-----
    /Help(h, ?)        : Display the usage help text
```

## <a name="47extensiondllex"></a>&#47;ExtensionDll(ex)

Você usa a opção de DLL de extensão (/ex) com `createconfig`, `dump`, `new`, `resourcepack` e `versioned` para especificar o local da DLL de extensão do ambiente do Sistema de Gerenciamento de Recursos.

## <a name="logging47metadata-file"></a>Arquivo logging&#47;metadata

O MakePri pode incluir informações específicas de um pacote de recursos no arquivo de metadados do indexador. Este é um exemplo de arquivo de log de `resources.pri` com os arquivos PRI de recurso `german.pri` e `highresolution.pri`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<root>
  <package filename="resources.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-100" src="" type="Path">
      <value>logo.scale-100.jpg</value>
    </instance>
    <instance itemname="resources\string2" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-en-us" src="C:\Users\alias\Desktop\repro\app4\project\en-us\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="german.pri">
    <instance itemname="resources\string2" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value2</value>
    </instance>
    <instance itemname="resources\string1" qualifiers="Language-de-de" src="C:\Users\alias\Desktop\repro\app4\project\de-de\resources.resw" type="String">
      <value>value1</value>
    </instance>
  </package>
  <package filename="highresolution.pri">
    <instance itemname="Files\logo.jpg" qualifiers="Scale-200" src="" type="Path">
      <value>logo.scale-200.jpg</value>
    </instance>
  </package>
</root>
```

## <a name="47indexfileif-option"></a>Opção &#47;IndexFile(if)

Use a opção de arquivo de índice (/if) com `dump`, `resourcepack` e `versioned` para especificar um arquivo PRI de entrada.

Para `resourcepack` e `versioned`, em vez de fornecer um arquivo PRI como parâmetro de entrada para /IndexFile(if), você poderá fornecer um arquivo de esquema.

```
/IndexFile(if) <FILEPATH>
```

**FILEPATH** é um token que especifica o local do arquivo PRI de entrada ou do arquivo de esquema PRI.

## <a name="47mappingfilemf-option"></a>Opção &#47;MappingFile(mf)

Use a opção de arquivo de mapeamento (/mf) com `new`, `resourcepack` e `versioned` para gerar um arquivo de mapeamento. O [MakeAppx.exe](../packaging/create-app-package-with-makeappx-tool.md) usa o arquivo de mapeamento para gerar pacotes de aplicativos.

```
/MappingFile(mf) <MAPPINGFILETYPE>
```

**MAPPINGFILETYPE** é um token que especifica o formato do arquivo de mapeamento. O único formato válido com suporte é `appx`.

```
/mf appx
```

Este é um conteúdo de exemplo de um arquivo de mapeamento principal.

```
"ResourceDimensions"                   "language-de-de"
```

E este é um conteúdo de exemplo de um arquivo de mapeamento de pacote de recursos.

```
"ResourceId"                           "Resources184.la5decaf08"
"ResourceDimensions"                   "language-de-de"
```

## <a name="output-summary"></a>Resumo de saída

Se os pacotes de recursos forem criados, o resumo de saída de MakePRI.exe será mais detalhado. Veja um exemplo a seguir.

```
Index Pass Completed: ResourcePackTests\TestApp_ResourcePack
Language Qualifiers: fr-FR, de-DE

Finished building
Version: 1.0
Resource Map Name: AppTest
Named Resources: 11

Resource PRI: fr-FR.pri
Version: 1.0
Resource Candidates: 4
Language: fr-FR

Resource PRI: de-DE.pri
Version: 1.0
Resource Candidates: 4
Language: de-DE

Output File(s) at TempTestResults
Successfully Completed
```

## <a name="47overwriteo-option"></a>Opção &#47;Overwrite(o)

Se a opção de substituição (/o) não for fornecida, e os arquivos de saída especificados já existirem, MakePri.exe exigirá uma confirmação antes da substituição.

```
Following file(s) already exist at output location:
<file(s)>
Overwrite these file(s)? [Y]es (any other key to cancel):
```

## <a name="47outputfileof-option"></a>Opção &#47;OutputFile(of)

Use a opção de arquivo de saída (/of) com `dump`, `new`, `resourcepack` e `versioned` para especificar o local de saída e o nome do arquivo PRI a ser gerado. Se MakePri.exe gerar mais de um arquivo PRI de recurso, ele os armazenará na pasta pai do arquivo de destino. Por exemplo, se você especificar `/of MyParentFolder\TargetFile.pri`, MakePri.exe gerará `TargetFile.language-en.pri` e `TargetFile.scale-100.pri`, juntamente com `TargetFile.pri` em `ParentFolder`.

Esta é uma condição de erro de exemplo e a mensagem de erro correspondente.

| Condição de erro | Mensagem de erro |
| --------------- | ------------- |
| O nome do arquivo de saída é igual a um dos nomes de pacote de recursos na configuração. | Configuração inválida: o nome de pacote de recursos <resource pack name> não pode ser igual ao arquivo de saída <outputfilename.pri>. |

## <a name="reversemaprm-option"></a>Opção /ReverseMap(rm)

Use a opção de mapa reverso (/rm) com `new`, `resourcepack` e `versioned` para gerar uma seção de mapeamento reverso no arquivo PRI, que pode ser usada para depuração.

## <a name="47schemafilesf-option"></a>Opção &#47;SchemaFile(sf)

Use a opção de arquivo de esquema (/sf) com `new`, `resourcepack` e `versioned` para gravar um arquivo de esquema no local especificado.

Para `resourcepack` e `versioned`, em vez de fornecer um arquivo PRI como parâmetro de entrada para /IndexFile(if), você poderá fornecer um arquivo de esquema.

```
/SchemaFile(sf) <FILEPATH>
```

**FILEPATH** é um token que especifica o local onde o arquivo de esquema será gravado.

Este é um exemplo de arquivo de esquema.

```xml
<PriInfo>
    <ResourceMap name="IndexName" resourceVersion="1.0"> 
        <ResourceMapSubtree name="Resources" index="1">
            <NamedResource name="String1" index="1"/>
            <NamedResource name="String2" index="1"/>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="Files" index="2">
            <NamedResource name="logo.png" index="2"/>
            <ResourceMapSubtree name="images" index="3">
                <NamedResource name="success.png" index="3"/>
                <NamedResource name="error.png" index="3"/>
            </ResourceMapSubtree>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

## <a name="47versionmajorvma-is-deprecated"></a>&#47;VersionMajor(vma) está obsoleto

A opção de versão principal (/vma) (para o comando `new`) está obsoleta; seu uso resultará nesta mensagem de aviso.

```
'VersionMajor (vma)' input parameter has been deprecated. Please specify major version in the configuration file using 'majorVersion' attribute on 'resources' node.
```

Para fornecer o número de versão principal, use o atributo [resources@majorVersion](makepri-exe-configuration.md) no arquivo de configuração.

## <a name="related-topics"></a>Tópicos relacionados

* [MakePri.exe](compile-resources-manually-with-makepri.md)
