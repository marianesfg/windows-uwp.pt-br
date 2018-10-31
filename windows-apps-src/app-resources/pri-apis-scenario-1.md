---
author: stevewhims
Description: In this scenario, we'll make a new app to represent our custom build system. We'll create a resource indexer and add strings and other kinds of resources to it. Then we'll generate and dump a PRI file.
title: Cenário 1 Gerar um arquivo PRI de recursos de cadeia de caracteres e arquivos de ativos
template: detail.hbs
ms.author: stwhi
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 7555f4a61f7798fa32d137928cde8c042a7fcdfc
ms.sourcegitcommit: cd00bb829306871e5103db481cf224ea7fb613f0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2018
ms.locfileid: "5867195"
---
# <a name="scenario-1-generate-a-pri-file-from-string-resources-and-asset-files"></a>Cenário 1: Gerar um arquivo PRI de recursos de cadeia de caracteres e arquivos de ativos
Neste cenário, usaremos as [APIs de índice de recurso do pacote (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) para criar um novo app para representar nosso sistema de compilação personalizado. Lembre-se: a finalidade do sistema de compilação personalizado é criar os arquivos PRI para um aplicativo UWP de destino. Portanto, como parte deste passo a passo, criaremos alguns arquivos de recurso de exemplo (contendo cadeias de caracteres e outros tipos de recursos) para representar os recursos do aplicativo UWP de destino.

## <a name="new-project"></a>Novo projeto
Comece criando um novo projeto no Microsoft Visual Studio. Crie um projeto **Aplicativo de Console do Windows do Visual C++** e nomeie-o como *CBSConsoleApp* (para "app de console do sistema de compilação personalizado)".

Escolha *x64* na lista suspensa **Plataformas da Solução**.

## <a name="headers-static-library-and-dll"></a>Cabeçalhos, biblioteca estática e DLL
As APIs de PRI são declaradas no arquivo de cabeçalho MrmResourceIndexer.h (que é instalado em `%ProgramFiles(x86)%\Windows Kits\10\Include\<WindowsTargetPlatformVersion>\um\`). Abra o arquivo `CBSConsoleApp.cpp` e inclua o cabeçalho juntamente com outros cabeçalhos necessários.

```cppwinrt
#include <string>
#include <windows.h>
#include <MrmResourceIndexer.h>
```

As APIs são implementadas no MrmSupport.dll, que você acessa por meio de links para a biblioteca estática MrmSupport.lib. Abra as **Propriedades** do projeto, clique em **Vinculador** > **Entrada**, edite **AdditionalDependencies** e adicione `MrmSupport.lib`.

Compile a solução e copie `MrmSupport.dll` de `C:\Program Files (x86)\Windows Kits\10\bin\<WindowsTargetPlatformVersion>\x64\` para a pasta de saída da compilação (provavelmente `C:\Users\%USERNAME%\source\repos\CBSConsoleApp\x64\Debug\`).

Adicione a seguinte função auxiliar a `CBSConsoleApp.cpp`, já que precisaremos dela.

```cppwinrt
inline void ThrowIfFailed(HRESULT hr)
{
    if (FAILED(hr))
    {
        // Set a breakpoint on this line to catch Win32 API errors.
        throw new std::exception();
    }
}
```

Na função `main()`, adicione chamadas para inicializar e cancelar a inicialização de COM.

```cppwinrt
int main()
{
    ::ThrowIfFailed(::CoInitializeEx(nullptr, COINIT_MULTITHREADED));
    
    // More code will go here.
    
    ::CoUninitialize();
}
```

## <a name="resource-files-belonging-to-the-target-uwp-app"></a>Arquivos de recursos pertencentes ao aplicativo UWP de destino
Agora, precisaremos de alguns arquivos de recurso de exemplo (contendo cadeias de caracteres e outros tipos de recursos) para representar os recursos do aplicativo UWP de destino. Eles podem, é claro, estar em qualquer lugar no sistema de arquivos. Mas, neste passo a passo, será conveniente colocá-los na pasta de projeto CBSConsoleApp para que tudo esteja em um único lugar. Você só precisa adicionar esses arquivos de recurso ao sistema de arquivos; não os adicione ao projeto CBSConsoleApp.

Na mesma pasta que contém `CBSConsoleApp.vcxproj`, adicione uma nova subpasta chamada `UWPAppProjectRootFolder`. Nessa nova subpasta, crie esses arquivos de recurso de exemplo.

### <a name="uwpappprojectrootfoldersample-imagepng"></a>\UWPAppProjectRootFolder\sample-image.png
Esse arquivo pode conter qualquer imagem PNG.

### <a name="uwpappprojectrootfolderresourcesresw"></a>\UWPAppProjectRootFolder\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-neutral</value>
    </data>
    <data name="LocalizedString2">
        <value>LocalizedString2-neutral</value>
    </data>
    <data name="NeutralOnlyString">
        <value>NeutralOnlyString-neutral</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderde-deresourcesresw"></a>\UWPAppProjectRootFolder\de-DE\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString2">
        <value>LocalizedString2-de-DE</value>
    </data>
</root>
```

### <a name="uwpappprojectrootfolderen-usresourcesresw"></a>\UWPAppProjectRootFolder\en-US\resources.resw
```xml
<?xml version="1.0"?>
<root>
    <data name="LocalizedString1">
        <value>LocalizedString1-en-US</value>
    </data>
    <data name="EnOnlyString">
        <value>EnOnlyString-en-US</value>
    </data>
</root>
```

## <a name="index-the-resources-and-create-a-pri-file"></a>Indexe os recursos e crie um arquivo PRI
Na função `main()`, antes da chamada para inicializar o COM, declare algumas cadeias de caracteres necessárias e crie a pasta de saída no qual geraremos nosso arquivo PRI.

```cppwinrt
std::wstring projectRootFolderUWPApp{ L"UWPAppProjectRootFolder" };
std::wstring generatedPRIsFolder{ projectRootFolderUWPApp + L"\\Generated PRIs" };
std::wstring filePathPRI{ generatedPRIsFolder + L"\\resources.pri" };
std::wstring filePathPRIDumpBasic{ generatedPRIsFolder + L"\\resources-pri-dump-basic.xml" };

::CreateDirectory(generatedPRIsFolder.c_str(), nullptr);
```

Imediatamente após a chamada para inicializar COM, declare um identificador de indexador de recurso e chame [**MrmCreateResourceIndexer**]() para criar um indexador de recursos.

```cppwinrt
MrmResourceIndexerHandle indexer;
::ThrowIfFailed(::MrmCreateResourceIndexer(
    L"OurUWPApp",
    projectRootFolderUWPApp.c_str(),
    MrmPlatformVersion::MrmPlatformVersion_Windows10_0_0_0,
    L"language-en_scale-100_contrast-standard",
    &indexer));
```

Esta é uma explicação dos argumentos que estão sendo passados para **MrmCreateResourceIndexer**.

- O nome da família de pacotes de nosso aplicativo UWP de destino, que será usado como nome do mapa de recursos quando gerarmos posteriormente um arquivo PRI a partir deste indexador de recurso.
- A raiz do projeto do nosso aplicativo UWP de destino. Em outras palavras, o caminho para nossos arquivos de recurso. Especificamos isso para que possamos especificar os caminhos relativos a essa raiz nas chamadas de API subsequentes para o mesmo indexador de recurso.
- A versão do Windows que queremos atingir.
- Uma lista de qualificadores de recurso padrão.
- Um ponteiro para nosso identificador de indexador de recurso para que a função possa defini-lo.

A próxima etapa é adicionar nossos recursos ao indexador de recurso que acabamos de criar. `resources.resw` é um arquivo de recursos (.resw) que contém as cadeias de caracteres neutras do nosso aplicativo UWP de destino. Role a tela para cima (neste tópico) se você quiser ver seu conteúdo. `de-DE\resources.resw` contém nossas cadeias de caracteres em alemão, enquanto `en-US\resources.resw` contém nossas cadeias de caracteres em inglês. Para adicionar os recursos de cadeia de caracteres em um arquivo de recursos a um indexador de recurso, chame [**MrmIndexResourceContainerAutoQualifiers**](). Em terceiro lugar, chamamos a função [**MrmIndexFile**]() para um arquivo que contém um recurso de imagem neutro no indexador de recurso.

```cppwinrt
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"de-DE\\resources.resw"));
::ThrowIfFailed(::MrmIndexResourceContainerAutoQualifiers(indexer, L"en-US\\resources.resw"));
::ThrowIfFailed(::MrmIndexFile(indexer, L"ms-resource:///Files/sample-image.png", L"sample-image.png", L""));
```

Na chamada para **MrmIndexFile**, o valor L"ms-resource:///Files/sample-image.png" é o URI do recurso. O primeiro segmento de caminho é "Files"; é ele que será usado como nome de subárvore do mapa de recursos quando geramos posteriormente um arquivo PRI a partir desse indexador de recurso.

Tendo informado o indexador de recursos sobre nossos arquivos de recursos, é hora de deixá-lo gerar um arquivo PRI no disco chamando a função [**MrmCreateResourceFile**]().

```cppwinrt
::ThrowIfFailed(::MrmCreateResourceFile(indexer, MrmPackagingModeStandaloneFile, MrmPackagingOptionsNone, generatedPRIsFolder.c_str()));
```

Neste ponto, um arquivo PRI chamado `resources.pri` foi criado em uma pasta chamada `Generated PRIs`. Agora que concluímos o trabalho com o indexador de recursos, chamamos [**MrmDestroyIndexerAndMessages**]() para destruir seu identificador e liberar qualquer recurso de computador que ele tenha alocado.

```cppwinrt
::ThrowIfFailed(::MrmDestroyIndexerAndMessages(indexer));
```

Como um arquivo PRI é binário, será mais fácil exibir o que acabamos de gerar se despejarmos o arquivo PRI binário em seu equivalente XML. Uma chamada para [**MrmDumpPriFile**]() faz exatamente isso.

```cppwinrt
::ThrowIfFailed(::MrmDumpPriFile(filePathPRI.c_str(), nullptr, MrmDumpType::MrmDumpType_Basic, filePathPRIDumpBasic.c_str()));
```

Esta é uma explicação dos argumentos que estão sendo passados para **MrmDumpPriFile**.

- O caminho do arquivo PRI a ser despejado. Não estamos usando o indexador de recursos nessa chamada (acabamos de destruí-lo) e, portanto, precisamos especificar um caminho de arquivo completo.
- Nenhum arquivo de esquema. Discutiremos o que é um esquema posteriormente neste tópico.
- Apenas as informações básicas.
- O caminho de um arquivo XML a ser criado.

É isso que o arquivo PRI, despejado no XML, contém.

```xml
<?xml version="1.0" encoding="UTF-8" standalone="yes"?>
<PriInfo>
    <ResourceMap name="OurUWPApp" version="1.0" primary="true">
        <Qualifiers>
            <Language>en-US,de-DE</Language>
        </Qualifiers>
        <ResourceMapSubtree name="Files">
            <NamedResource name="sample-image.png" uri="ms-resource://OurUWPApp/Files/sample-image.png">
                <Candidate type="Path">
                    <Value>sample-image.png</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
        <ResourceMapSubtree name="resources">
            <NamedResource name="EnOnlyString" uri="ms-resource://OurUWPApp/resources/EnOnlyString">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>EnOnlyString-en-US</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString1" uri="ms-resource://OurUWPApp/resources/LocalizedString1">
                <Candidate qualifiers="Language-en-US" isDefault="true" type="String">
                    <Value>LocalizedString1-en-US</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString1-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="LocalizedString2" uri="ms-resource://OurUWPApp/resources/LocalizedString2">
                <Candidate qualifiers="Language-de-DE" type="String">
                    <Value>LocalizedString2-de-DE</Value>
                </Candidate>
                <Candidate type="String">
                    <Value>LocalizedString2-neutral</Value>
                </Candidate>
            </NamedResource>
            <NamedResource name="NeutralOnlyString" uri="ms-resource://OurUWPApp/resources/NeutralOnlyString">
                <Candidate type="String">
                    <Value>NeutralOnlyString-neutral</Value>
                </Candidate>
            </NamedResource>
        </ResourceMapSubtree>
    </ResourceMap>
</PriInfo>
```

As informações começam com um mapa de recursos, que recebe o nome da família de pacotes do nosso aplicativo UWP de destino. Incluído no mapa de recursos estão duas subárvores de mapa de recursos: uma para os recursos de arquivo que indexamos e outra para nossos recursos de cadeia de caracteres. Observe como o nome da família de pacotes foi inserido em todos os URIs de recurso.

O primeiro recurso de cadeia de caracteres é *EnOnlyString* de `en-US\resources.resw` e tem apenas um candidato (que corresponde ao qualificador *language-en-US*). Depois, temos *LocalizedString1* de `resources.resw` e de `en-US\resources.resw`. Consequentemente, ele tem dois candidatos: um *language-en-US* correspondente e um candidato neutro de fallback que corresponde a qualquer contexto. Da mesma forma, *LocalizedString2* tem dois candidatos: *language-de-DE* e neutro. E, finalmente, *NeutralOnlyString*, que só existe na forma neutra. Atribuí a ele esse nome para deixar claro que ele não deve ser localizado.

## <a name="summary"></a>Resumo
Neste cenário, mostramos como usar as [APIs de índice de recurso do pacote (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690) para criar um indexador de recursos. Adicionamos recursos de cadeia de caracteres e arquivos de ativos ao indexador de recursos. Em seguida, usamos o indexador de recursos para gerar um arquivo PRI binário. E, finalmente, despejamos o arquivo PRI binário sob a forma de XML, para que possamos confirmar que ele contém as informações esperadas.

## <a name="important-apis"></a>APIs importantes
* [Referência ao índice de recurso do pacote (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690)

## <a name="related-topics"></a>Tópicos relacionados
* [APIs de índice de recurso do pacote (PRI) e sistemas de compilação personalizados](pri-apis-custom-build-systems.md)
