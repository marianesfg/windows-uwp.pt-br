---
author: stevewhims
Description: This topic describes the format-specific indexers used by the MakePri.exe tool to generate its index of resources.
title: Indexadores específicos de formato do MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 439a69da400caaa9ae509a121f2aa7336853d2ca
ms.sourcegitcommit: 086001cffaf436e6e4324761d59bcc5e598c15ea
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/27/2018
ms.locfileid: "5698847"
---
# <a name="makepriexe-format-specific-indexers"></a>Indexadores específicos de formato do MakePri.exe

Este tópico descreve os indexadores específicos de formato usados pela ferramenta [MakePri.exe](compile-resources-manually-with-makepri.md) para gerar seu índice de recursos.

> [!NOTE]
> MakePri.exe é instalado quando você verificar a opção de **SDK do Windows para aplicativos gerenciados do UWP** ao instalar o Software Development Kit do Windows. Ele é instalado no caminho `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (bem como nas pastas nomeadas para as outras arquiteturas). Por exemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

O MakePri.exe é normalmente usado com o comando `new`, `versioned` ou `resourcepack`. Consulte [Opções de linha de comando do MakePri.exe](makepri-exe-command-options.md). Nesses casos, ele indexa os arquivos de origem para gerar um índice de recursos. O MakePri.exe usa vários indexadores individuais para ler diferentes arquivos de recurso de origem ou contêineres de recursos. O indexador mais simples é o indexador de pasta, que indexa o conteúdo de uma pasta, como imagens `.jpg` ou `.png`.

Você identifica indexadores específicos de formato especificando elementos `<indexer-config>` em um elemento `<index>` do [Arquivo de configuração. do MakePri.exe](makepri-exe-configuration.md). O atributo `type` identifica o indexador específico de formato utilizado.

Os contêineres de recursos encontrados durante a indexação geralmente têm seu conteúdo indexado em vez de serem adicionados ao índice. Por exemplo, os arquivos `.resjson` que o indexador de pasta encontra podem ser indexados ainda por um indexador `.resjson`; nesse caso, o próprio arquivo `.resjson` não aparece no índice. **Observação** um elemento `<indexer-config>` do indexador associado a esse contêiner deve ser incluído no arquivo de configuração para que isso aconteça.

Geralmente, os qualificadores encontrados em uma entidade contentora, como uma pasta ou um arquivo `.resw`, são aplicados a todos os recursos nela contidos, como os arquivos armazenados na pasta ou as cadeias de caracteres inseridas no arquivo `.resw`.

## <a name="folder"></a>Folder

O indexador de pasta é identificado por um atributo `type` de FOLDER. Ele indexa o conteúdo de uma pasta e determina os qualificadores de recursos a partir dos nomes de pasta e de arquivo. Seu elemento de configuração está em conformidade com o esquema a seguir.

```xml
<xs:schema attributeFormDefault=\"unqualified\" elementFormDefault=\"qualified\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\">\
    <xs:simpleType name=\"ExclusionTypeList\">\
        <xs:restriction base=\"xs:string\">\
            <xs:enumeration value=\"path\"/>\
            <xs:enumeration value=\"extension\"/>\
            <xs:enumeration value=\"name\"/>\
            <xs:enumeration value=\"tree\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:complexType name=\"FolderExclusionType\">\
        <xs:attribute name=\"type\" type=\"ExclusionTypeList\" use=\"required\"/>\
        <xs:attribute name=\"value\" type=\"xs:string\" use=\"required\"/>\
        <xs:attribute name=\"doNotTraverse\" type=\"xs:boolean\" use=\"required\"/>\
        <xs:attribute name=\"doNotIndex\" type=\"xs:boolean\" use=\"required\"/>\
    </xs:complexType>\
    <xs:simpleType name=\"IndexerConfigFolderType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((f|F)(o|O)(l|L)(d|D)(e|E)(r|R))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:sequence>\
                <xs:element name=\"exclude\" type=\"FolderExclusionType\" minOccurs=\"0\" maxOccurs=\"unbounded\"/>\
            </xs:sequence>\
            <xs:attribute name=\"type\" type=\"IndexerConfigFolderType\" use=\"required\"/>\
            <xs:attribute name=\"foldernameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"filenameAsQualifier\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>
```

O atributo `qualifierDelimiter` especifica o caractere depois do qual os qualificadores serão especificados em um nome de arquivo, ignorando a extensão. O padrão é ".".

## <a name="pri"></a>PRI

O indexador PRI é identificado por um atributo `type` do PRI. Ele indexa o conteúdo de um arquivo PRI. Normalmente, você o utiliza ao indexar o recurso contido em outro assembly, DLL, SDK ou biblioteca de classes no PRI do app.

Todos os nomes de recurso, qualificadores e valores contidos no arquivo PRI são mantidos diretamente no novo arquivo PRI. Entretanto, o mapa de recursos de nível superior não é mantido no PRI final. Os mapas de recursos são mesclados.

```xml
<xs:schema id=\"prifile\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigPriType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((p|P)(r|R)(i|I))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigPriType\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

## <a name="priinfo"></a>PriInfo

O indexador PriInfo é identificado por um atributo `type` de PRIINFO. Ele indexa o conteúdo de um arquivo de despejo detalhado. Você gera um arquivo de despejo detalhado executando `makepri dump` com a opção `/dt detailed`. O elemento de configuração do indexador está em conformidade com o esquema a seguir.

```xml
<xs:schema id="priinfo" xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified">
  <xs:simpleType name="IndexerConfigPriInfoType">
    <xs:restriction base="xs:string">
      <xs:pattern value="((p|P)(r|R)(i|I)(i|I)(n|N)(f|F)(o|O))"/>
    </xs:restriction>
  </xs:simpleType>
  <xs:element name="indexer-config">
    <xs:complexType>
      <xs:attribute name="type" type="IndexerConfigPriInfoType" use="required"/>
      <xs:attribute name="emitStrings" type="xs:boolean" use="optional"/>
      <xs:attribute name="emitPaths" type="xs:boolean" use="optional"/>
    </xs:complexType>
  </xs:element>
</xs:schema>
```

Este elemento de configuração permite atributos opcionais para configurar o comportamento do indexador PriInfo. O valor padrão de `emitStrings` e de `emitPaths` é `true`. Se `emitStrings` for `true`, os candidatos a recursos com o atributo `type` definido como "String" serão incluídos no índice; caso contrário, eles serão excluídos. Se 'emitPaths' for `true`, os candidatos a recursos com o atributo `type` definido como "Path" serão incluídos no índice; caso contrário, eles serão excluídos.

Veja uma configuração de exemplo que inclui tipos de recurso String, mas ignora tipos de recurso Path.

```xml
<indexer-config type="priinfo" emitStrings="true" emitPaths="false" />
```

Para ser indexado, um arquivo de despejo deve terminar com a extensão `.pri.xml` e estar em conformidade com o esquema a seguir.

```xml
<xs:schema xmlns:xs="http://www.w3.org/2001/XMLSchema" elementFormDefault="qualified" >\
  <xs:simpleType name="candidateType">\
    <xs:restriction base="xs:string">\
      <xs:pattern value="Path|String"/>\
    </xs:restriction>\
  </xs:simpleType>\
  <xs:complexType name="scopeType">\
    <xs:sequence>\
      <xs:element name="ResourceMapSubtree" type="scopeType" minOccurs="0" maxOccurs="unbounded"/>\
      <xs:element name="NamedResource" minOccurs="0" maxOccurs="unbounded">\
        <xs:complexType>\
          <xs:sequence>\
            <xs:element name="Decision" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:any processContents="skip" minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:anyAttribute processContents="skip" />\
              </xs:complexType>\
            </xs:element>\
            <xs:element name="Candidate" minOccurs="0" maxOccurs="unbounded">\
              <xs:complexType>\
                <xs:sequence>\
                  <xs:element name="QualifierSet" minOccurs="0" maxOccurs="unbounded">\
                    <xs:complexType>\
                      <xs:sequence>\
                        <xs:element name="Qualifier" minOccurs="0" maxOccurs="unbounded">\
                          <xs:complexType>\
                            <xs:attribute name="name" type="xs:string" use="required" />\
                            <xs:attribute name="value" type="xs:string" use="required" />\
                            <xs:attribute name="priority" type="xs:integer" use="required" />\
                            <xs:attribute name="scoreAsDefault" type="xs:decimal" use="required" />\
                            <xs:attribute name="index" type="xs:integer" use="required" />\
                          </xs:complexType>\
                        </xs:element>\
                      </xs:sequence>\
                      <xs:anyAttribute processContents="skip" />\
                    </xs:complexType>\
                  </xs:element>\
                  <xs:element name="Value" type="xs:string"  minOccurs="0" maxOccurs="unbounded"/>\
                </xs:sequence>\
                <xs:attribute name="type" type="candidateType" use="required" />\
              </xs:complexType>\
            </xs:element>\
          </xs:sequence>\
          <xs:attribute name="name" use="required" type="xs:string" />\
          <xs:anyAttribute processContents="skip" />\
        </xs:complexType>\
      </xs:element>\
    </xs:sequence>\
    <xs:attribute name="name" use="required" type="xs:string" />\
    <xs:anyAttribute processContents="skip" />\
  </xs:complexType>\
  <xs:element name="PriInfo">\
    <xs:complexType>\
      <xs:sequence>\
        <xs:element name="PriHeader" >\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs ="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="QualifierInfo">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip" />\
            </xs:sequence>\
          </xs:complexType>\
        </xs:element>\
        <xs:element name="ResourceMap">\
          <xs:complexType>\
            <xs:sequence>\
              <xs:element name="VersionInfo">\
                <xs:complexType>\
                  <xs:anyAttribute processContents="skip" />\
                </xs:complexType>\
              </xs:element>\
              <xs:element minOccurs="0" maxOccurs="unbounded" name="ResourceMapSubtree" type="scopeType" />\
            </xs:sequence>\
            <xs:attribute name="name" type="xs:string" use="required" />\
            <xs:anyAttribute processContents="skip" />\
          </xs:complexType>\
        </xs:element>\
      </xs:sequence>\
    </xs:complexType>\
  </xs:element>\
</xs:schema>
```

O MakePri.exe oferece suporte aos tipos de despejo "Basic", "Detailed", "Schema" e "Summary". Para configurar o MakePri.exe para emitir o tipo de despejo que o indexador PriInfo pode ler, inclua "/DumpType Detailed" ao usar o comando `dump`.

Vários elementos do arquivo `.pri.xml` são ignorados pelo MakePri.exe. Esses elementos são calculados durante a indexação ou são especificados no arquivo de configuração MakePri.exe. Os nomes de recurso, qualificadores e valores contidos no arquivo de despejo são mantidos diretamente no novo arquivo PRI. Entretanto, o mapa de recursos de nível superior não é mantido no PRI final. Os mapas de recursos são mesclados como parte da indexação.

Este é um exemplo de um recurso do tipo String válido em um arquivo de despejo.

```xml
<NamedResource name="SampleString " index="96" uri="ms-resource://SampleApp/resources/SampleString ">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
  </Decision>
  <Candidate type="String">
    <QualifierSet index="1">
      <Qualifier name="Language" value="EN-US" priority="900" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>A Sample String Value</Value>
  </Candidate>
</NamedResource>
```

Este é um exemplo de um recurso do tipo Path válido com dois candidatos em um arquivo de despejo.

```xml
<NamedResource name="Sample.png" index="77" uri="ms-resource://SampleApp/Files/Images/Sample.png">
  <Decision index="2">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="0.7" index="2"/>
    </QualifierSet>
  </Decision>
  <Candidate type="Path">
    <QualifierSet index="1">
      <Qualifier name="Scale" value="180" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-180.png</Value>
  </Candidate>
  <Candidate type="Path">
    <QualifierSet index="2">
      <Qualifier name="Scale" value="140" priority="500" scoreAsDefault="1.0" index="1"/>
    </QualifierSet>
    <Value>Images\Sample.scale-140.png</Value>
  </Candidate>
</NamedResource>
```

## <a name="resfiles"></a>ResFiles

O indexador ResFiles é identificado por um atributo `type` de RESFILES. Ele indexa o conteúdo de um arquivo `.resfiles`. Seu elemento de configuração está em conformidade com o esquema a seguir.

```xml
<xs:schema id=\"resx\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResFilesType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(f|F)(i|I)(l|L)(e|E)(s|S))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResFilesType\" use=\"required\"/>\
            <xs:attribute name=\"qualifierDelimiter\" type=\"xs:string\" use=\"required\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

O arquivo `.resfiles` é um arquivo de texto que contém uma lista simples de caminhos de arquivo. Um arquivo `.resfiles` pode conter comentários "//". Veja um exemplo a seguir.

```
Strings\component1\fr\elements.resjson
Images\logo.scale-100.png
Images\logo.scale-140.png
Images\logo.scale-180.png
```

## <a name="resjson"></a>ResJSON

O indexador ResJSON é identificado por um atributo `type` de RESJSON. Ele indexa o conteúdo de um arquivo `.resjson`, que é um arquivo de recurso de cadeia de caracteres. Seu elemento de configuração está em conformidade com o esquema a seguir.

```xml
<xs:schema id=\"resjson\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResJsonType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(j|J)(s|S)(o|O)(n|N))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResJsonType\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

Um arquivo `.resjson` contém o texto JSON (consulte [O tipo de mídia de aplicativo/json para JavaScript Object Notation (JSON)](http://www.ietf.org/rfc/rfc4627.txt)). O arquivo deve conter um único objeto JSON com propriedades hierárquicas. Cada propriedade deve ser outro objeto JSON ou um valor de cadeia de caracteres.

As propriedades JSON com nomes que começam com um sublinhado ("_") não são compiladas no arquivo PRI final, mas são mantidas no arquivo de log.

O arquivo também pode conter comentários "//" que são ignorados durante a análise.

O atributo `initialPath` coloca todos os recursos nesse caminho inicial, inserindo-o antes do nome do recurso. Normalmente, você usaria isso ao indexar recursos de bibliotecas de classes. O padrão é em branco.

## <a name="resw"></a>ResW

O indexador ResW é identificado por um atributo `type` de RESW. Ele indexa o conteúdo de um arquivo `.resw`, que é um arquivo de recurso de cadeia de caracteres. Seu elemento de configuração está em conformidade com o esquema a seguir.

```xml
<xs:schema id=\"resw\" xmlns:xs=\"http://www.w3.org/2001/XMLSchema\" elementFormDefault=\"qualified\">\
    <xs:simpleType name=\"IndexerConfigResxType\">\
        <xs:restriction base=\"xs:string\">\
            <xs:pattern value=\"((r|R)(e|E)(s|S)(w|W))\"/>\
        </xs:restriction>\
    </xs:simpleType>\
    <xs:element name=\"indexer-config\">\
        <xs:complexType>\
            <xs:attribute name=\"type\" type=\"IndexerConfigResxType\" use=\"required\"/>\
            <xs:attribute name=\"convertDotsToSlashes\" type=\"xs:boolean\" use=\"required\"/>\
            <xs:attribute name=\"initialPath\" type=\"xs:string\" use=\"optional\"/>\
        </xs:complexType>\
    </xs:element>\
</xs:schema>\
```

O arquivo `.resw` é um arquivo XML em conformidade com o esquema a seguir.

```xml
  <xsd:schema id="root" xmlns="" xmlns:xsd="http://www.w3.org/2001/XMLSchema" xmlns:msdata="urn:schemas-microsoft-com:xml-msdata">
    <xsd:import namespace="http://www.w3.org/XML/1998/namespace" />
    <xsd:element name="root" msdata:IsDataSet="true">
      <xsd:complexType>
        <xsd:choice maxOccurs="unbounded">
          <xsd:element name="metadata">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" />
              </xsd:sequence>
              <xsd:attribute name="name" use="required" type="xsd:string" />
              <xsd:attribute name="type" type="xsd:string" />
              <xsd:attribute name="mimetype" type="xsd:string" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="assembly">
            <xsd:complexType>
              <xsd:attribute name="alias" type="xsd:string" />
              <xsd:attribute name="name" type="xsd:string" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="data">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
                <xsd:element name="comment" type="xsd:string" minOccurs="0" msdata:Ordinal="2" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" msdata:Ordinal="1" />
              <xsd:attribute name="type" type="xsd:string" msdata:Ordinal="3" />
              <xsd:attribute name="mimetype" type="xsd:string" msdata:Ordinal="4" />
              <xsd:attribute ref="xml:space" />
            </xsd:complexType>
          </xsd:element>
          <xsd:element name="resheader">
            <xsd:complexType>
              <xsd:sequence>
                <xsd:element name="value" type="xsd:string" minOccurs="0" msdata:Ordinal="1" />
              </xsd:sequence>
              <xsd:attribute name="name" type="xsd:string" use="required" />
            </xsd:complexType>
          </xsd:element>
        </xsd:choice>
      </xsd:complexType>
    </xsd:element>
  </xsd:schema>
```

O atributo `convertDotsToSlashes` converte todos os caracteres de ponto (".") encontrados nos nomes de recurso (atributos de nome de elemento de dados) em barras ("/"), exceto quando os caracteres de ponto estão entre "[" e "]".

O atributo `initialPath` coloca todos os recursos nesse caminho inicial, inserindo-o antes do nome do recurso. Normalmente, você usa isso ao indexar recursos de bibliotecas de classes. O padrão é em branco.

## <a name="related-topics"></a>Tópicos relacionados

* [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md)
* [Opções de linha de comando do MakePri.exe](makepri-exe-command-options.md)
* [Arquivo de configuração MakePri.exe](makepri-exe-configuration.md)
* [O tipo de mídia de aplicativo/json para JavaScript Object Notation (JSON)](http://www.ietf.org/rfc/rfc4627.txt)