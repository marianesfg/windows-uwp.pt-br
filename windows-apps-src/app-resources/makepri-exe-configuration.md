---
author: stevewhims
Description: This topic describes the schema of the MakePri.exe XML configuration file.
title: Arquivo de configuração MakePri.exe
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 512880b7a7ea955a45697762cbbdb7f74ac70102
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4500325"
---
# <a name="makepriexe-configuration-file"></a>Arquivo de configuração MakePri.exe

Este tópico descreve o esquema do [arquivo de configuração XML MakePri.exe](compile-resources-manually-with-makepri.md); conhecido também como arquivo de configuração PRI. A ferramenta MakePri.exe tem um [comando createconfig](makepri-exe-command-options.md#createconfig-command) que você pode usar para criar um arquivo de configuração PRI novo e inicializado.

> [!NOTE]
> MakePri.exe é instalado quando você verificar a opção de **SDK do Windows para aplicativos gerenciados do UWP** ao instalar o Software Development Kit do Windows. Ele é instalado no caminho `%WindowsSdkDir%bin\<WindowsTargetPlatformVersion>\x64\makepri.exe` (bem como nas pastas nomeadas para as outras arquiteturas). Por exemplo, `C:\Program Files (x86)\Windows Kits\10\bin\10.0.17713.0\x64\makepri.exe`.

O arquivo de configuração PRI determina quais recursos serão indexados e como isso será feito. O XML de configuração deve estar de acordo com o esquema a seguir.

```xml
<?xml version="1.0" encoding="utf-8"?>
<xs:schema attributeFormDefault="unqualified" elementFormDefault="qualified" xmlns:xs="http://www.w3.org/2001/XMLSchema">
  <xs:element name="resources">
    <xs:complexType>
      <xs:sequence>
        <xs:element name="packaging" maxOccurs="1" minOccurs="0">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="autoResourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:attribute name="qualifier" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
              <xs:element name="resourcePackage" maxOccurs="unbounded" minOccurs="0">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element name="qualifierSet" maxOccurs="unbounded" minOccurs="0">
                      <xs:complexType>
                        <xs:attribute name="definition" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                  <xs:attribute name="name" type="xs:string" use="required" />
                </xs:complexType>
              </xs:element>
            </xs:sequence>
          </xs:complexType>
        </xs:element>
        <xs:element maxOccurs="unbounded" name="index">
          <xs:complexType>
            <xs:sequence>
              <xs:element name="qualifiers" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="default" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:element minOccurs="1" maxOccurs="unbounded" name="qualifier">
                      <xs:complexType>
                        <xs:attribute name="name" type="xs:string" use="required" />
                        <xs:attribute name="value" type="xs:string" use="required" />
                      </xs:complexType>
                    </xs:element>
                  </xs:sequence>
                </xs:complexType>
              </xs:element>
              <xs:element name="indexer-config" minOccurs="0" maxOccurs="unbounded">
                <xs:complexType>
                  <xs:sequence>
                    <xs:any minOccurs="0" maxOccurs="unbounded" processContents="skip"/>
                  </xs:sequence>
                  <xs:attribute name="type" type="xs:string" use="required" />
                  <xs:anyAttribute processContents="skip"/>
                </xs:complexType>
              </xs:element>
            </xs:sequence>
            <xs:attribute name="root" type="xs:string" use="required" />
            <xs:attribute name="startIndexAt" type="xs:string" use="required" />
          </xs:complexType>
        </xs:element>
      </xs:sequence>
      <xs:attribute name="isDeploymentMergeable" type="xs:boolean" use="optional" />
      <xs:attribute name="majorVersion" type="xs:positiveInteger" use="optional" />
      <xs:attribute name="targetOsVersion" type="xs:string" use="optional" />
    </xs:complexType>
  </xs:element>
</xs:schema>
```

- O elemento `default` especifica o contexto (idioma, escala, contraste etc.) que deve ser usado para resolver recursos quando o contexto de tempo de execução não corresponder a nenhum candidato a recurso. Como esse contexto é especificado em tempo de compilação e não muda, os recursos são resolvidos para esse contexto à medida que os qualificadores são criados. A pontuação correspondente é armazenada no momento da compilação. Cada qualificador deve ter algum valor especificado. Consulte [ResourceContext](resource-management-system.md#resourcecontext) para obter detalhes sobre como os recursos são escolhidos.
- O elemento `index` define as passagens de indexação discreta executadas sobre os ativos. Cada passagem de indexação determina os [indexadores específicos de formato](makepri-exe-format-specific-indexers.md) a serem usados e quais recursos serão indexados.
- O elemento `qualifiers` define os qualificadores iniciais do primeiro arquivo ou pasta que outros recursos herdarão. Cada elemento de qualificador deve ter um nome e um valor válidos (consulte [Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md)).
- O atributo `root` é a raiz de caminho do arquivo físico da passagem de índice. Ele pode ser relativo ou absoluto. Se relativo, é acrescentado à raiz do projeto que você fornece na linha de comando. Se absoluto, ele é usado diretamente como a raiz da passagem de índice. Barras ou barras invertidas são aceitáveis. As barras à direita são cortadas. A raiz da passagem de índice determina a pasta à qual todos os recursos são considerados relativos.
- O atributo `startIndexAt` é o arquivo ou a pasta semente inicial usado na indexação. Ele é relativo à raiz da passagem de índice. Um valor vazio assumirá a pasta raiz da passagem de índice.

## <a name="default-pri-config-file"></a>Arquivo de configuração PRI padrão

O MakePri.exe gera este arquivo de configuração PRI novo e inicializado quando o [comando createconfig](makepri-exe-command-options.md#createconfig-command) é emitido.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<resources targetOsVersion="10.0.0" majorVersion="1">
  <packaging>
    <autoResourcePackage qualifier="Language"/>
    <autoResourcePackage qualifier="Scale"/>
    <autoResourcePackage qualifier="DXFeatureLevel"/>
  </packaging>
  <index root="\" startIndexAt="\">
    <default>
      <qualifier name="Language" value="en-US"/>
      <qualifier name="Contrast" value="standard"/>
      <qualifier name="Scale" value="100"/>
      <qualifier name="HomeRegion" value="001"/>
      <qualifier name="TargetSize" value="256"/>
      <qualifier name="LayoutDirection" value="LTR"/>
      <qualifier name="Theme" value="dark"/>
      <qualifier name="AlternateForm" value=""/>
      <qualifier name="DXFeatureLevel" value="DX9"/>
      <qualifier name="Configuration" value=""/>
      <qualifier name="DeviceFamily" value="Universal"/>
      <qualifier name="Custom" value=""/>
    </default>
    <indexer-config type="folder" foldernameAsQualifier="true" filenameAsQualifier="true" qualifierDelimiter="."/>
    <indexer-config type="resw" convertDotsToSlashes="true" initialPath=""/>
    <indexer-config type="resjson" initialPath=""/>
    <indexer-config type="PRI"/>
  </index>
  <!--<index startIndexAt="Start Index Here" root="Root Here">-->
  <!--        <indexer-config type="resfiles" qualifierDelimiter="."/>-->
  <!--        <indexer-config type="priinfo" emitStrings="true" emitPaths="true" emitEmbeddedData="true"/>-->
  <!--</index>-->
</resources>
```

## <a name="packaging-element"></a>Elemento packaging

O elemento `packaging` define as informações de divisão do PRI. O esquema do elemento `packaging` é definido para a configuração automática (suporte a `autoResourcePackage` com uma dimensão específica) e a configuração manual.

Este exemplo mostra como usar `autoResourcePackage` com uma dimensão específica.

```xml
    <packaging>
        <autoResourcePackage qualifier="Language"/>
        <autoResourcePackage qualifier="Scale"/>
        <autoResourcePackage qualifier="DXFeatureLevel"/>
    </packaging>
```

Este exemplo mostra como usar o `resourcePackage` manual.

```xml
  <packaging>
    <resourcePackage name="Germany">
      <qualifierSet definition="lang-de-de"/>
      <qualifierSet definition="lang-es-es"/>
    </resourcePackage>  
    <resourcePackage name="France">
      <qualifierSet definition="lang-fr-fr"/>
    </resourcePackage>  
    <resourcePackage name="HighRes1">
      <qualifierSet definition="scale-200"/>
    </resourcePackage>
    <resourcePackage name="HighRes2">
      <qualifierSet definition="scale-400"/>
    </resourcePackage>
  </packaging>
```

O MakePri.exe não bloqueia explicitamente a geração de arquivos de recursos PRI com qualquer dimensão específica. As restrições com determinado conjunto de dimensões são definidas e implementadas externamente pelo MakeAppx.exe ou por outras ferramentas no pipeline.

O MakePri.exe analisa o elemento `packaging` após todos os nós `index` para preencher todos os qualificadores padrão. O MakePri.exe coleta as informações analisadas nessas estruturas de dados.

```csharp
enum ResourcePackageMode
{
    None,
    AutoPackQualifier,
    ManualPack
}

ResourcePackageMode eResourcePackageMode;
list<string> RPQualifierList; // To store AutoResourcePackage Qualifiers
map<string, list<string>> RPNameToQSIMap; // To store ResourcePackage name to QualifierSet list mapping.
```

## <a name="resourcesisdeploymentmergeable-attribute"></a>Atributo resources@isDeploymentMergeable

Este atributo define um sinalizador no arquivo PRI gerado

- Mesclagem da implantação para identificar se esse arquivo PRI pode ser mesclado.
- O GetFullyQualifiedReference retorna um erro quando esse sinalizador é definido e o gerenciador de recursos foi inicializado com um arquivo.

O valor padrão desse atributo é `true`. O MakePri.exe só definirá o sinalizador no PRI se você definir o Windows10 como destino.

É recomendável omitir `isDeploymentMergeable` (ou defini-lo explicitamente para `true`) na criação de pacote de recursos se você definir o Windows 10 como destino.

O MakePri.exe adicionará o valor `isDeploymentMergeable` ao arquivo de despejo se `makepri dump` for executado com a opção `/dt detailed`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <IsDeploymentMergeable>true</IsDeploymentMergeable>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="resourcesmajorversion-attribute"></a>Atributo resources@majorVersion

O valor padrão deste atributo é 1. Se você fornecer um valor explícito e também usar a opção de linha de comando `/VersionMajor(vma)` obsoleta na ferramenta MakePri.exe, o valor no arquivo de configuração terá precedência.

Veja um exemplo a seguir.

```xml
<resources majorVersion="2">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

## <a name="resourcestargetosversion-attribute"></a>Atributo resources@targetOsVersion

Indica a versão do sistema operacional de destino. A tabela a seguir mostra os valores compatíveis; o valor padrão é 6.3.0.

| Valor | Significado |
| ----- | ------- |
| 10.0.0 | Windows 10 |
| 6.3.0 (padrão) | Windows 8.1 |
| 6.2.1 | Windows 8 |

Veja um exemplo a seguir.

```xml
<resources targetOsVersion="10.0.0">
  <packaging ... />
  <index root="\" startIndexAt="\">
    ...
  </index>
</resources>
```

**Observação** Windows é compatível com versões anteriores de arquivos PRI, mas nem sempre é compatível com versões posteriores.

O MakePri.exe adicionará o valor `targetOsVersion` ao arquivo de despejo se `makepri dump` for executado com a opção `/dt detailed`.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <PriHeader>
        ...
        <TargetOS version="10.0.0"/>
        ...
    </PriHeader>
  ...
</PriInfo>
```

## <a name="validation-error-messages"></a>Mensagens de erro de validação

Estas são algumas condições de erro de exemplo, e a mensagem de erro correspondente.

| Condição | Gravidade | Mensagem |
| --------- | -------- | ------- |
| Uma targetOsVersion diferente de um dos valores com suporte é especificada. | Erro | Configuração Inválida: targetOsVersion inválida especificada. |
| Uma targetOsVersion "6.2.1" é especificada e um elemento `packaging` está presente. | Erro | Configuração Inválida: nó 'Packaging' incompatível com esta targetOsVersion. |
| Mais de um modo encontrado na configuração. Por exemplo, Manual e AutoResourcePackage especificados. | Erro | Configuração Inválida: o nó 'packaging' não pode ter mais de um modo de operação. |
| Um qualificador padrão foi listado no pacote de recursos. | Erro | Configuração inválida: <Qualifiername>=<QualifierValue> é um qualificador padrão e seus candidatos não podem ser adicionados a um pacote de recursos. |
| O qualificador AutoResourcePackage inclui vários qualificadores. Por exemplo, language_scale. | Erro | Configuração Inválida: AutoResourcePackage com vários qualificadores não permitido. |
| QualifierSet de ResourcePackage com vários qualificadores. Por exemplo, language-en-us_scale-100 | Erro | Configuração Inválida: QualifierSet com vários qualificadores não permitido. |
| Nome de resourcepack duplicado encontrado. | Erro | Configuração Inválida: nome de pacote de recursos <rpname> duplicado. |
| Mesmo conjunto de qualificadores definido em dois pacotes de recursos. | Erro | Configuração inválida: várias instâncias de QualifierSet "<qualifier tags>" encontradas. |
| Nenhum candidato encontrado para o QualifierSet listado no nó 'ResourcePackage'. | Aviso | Configuração inválida: Nenhum candidato encontrado para <Resource Package Name>. |
| Nenhum candidato encontrado para o qualificador listado no nó ‘AutoResourcePackage’. | Aviso | Configuração inválida: Nenhum candidato encontrado para o qualificador <qualifier name>. O pacote de recursos não foi gerado. |
| Nenhum dos modos foi encontrado. Ou seja, um nó 'packaging' vazio foi encontrado. | Aviso | Configuração inválida: nenhum modo de empacotamento especificado. |

## <a name="related-topics"></a>Tópicos relacionados

* [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md)
* [Opções de linha de comando do MakePri.exe &mdash; comando createconfig](makepri-exe-command-options.md#createconfig-command)
* [Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md)
* [Sistema de Gerenciamento de Recursos &mdash; ResourceContext](resource-management-system.md#resourcecontext)