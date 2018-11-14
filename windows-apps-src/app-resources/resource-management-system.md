---
author: stevewhims
Description: At build time, the Resource Management System creates an index of all the different variants of the resources that are packaged up with your app. At run-time, the system detects the user and machine settings that are in effect and loads the resources that are the best match for those settings.
title: Sistema de Gerenciamento de Recursos
template: detail.hbs
ms.author: stwhi
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: b80eda57ff700d732ba2402582ed6402acca4fc6
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6659974"
---
# <a name="resource-management-system"></a>Sistema de Gerenciamento de Recursos
O Sistema de Gerenciamento de Recursos tem recursos em tempo de compilação e em tempo de execução. No momento da compilação, o sistema cria um índice de todas as variantes dos recursos que são empacotados com o app. Trata-se do índice de recurso do pacote, conhecido também como PRI, que também está incluído no pacote do app. No momento da execução, o sistema detecta as configurações do usuário e da máquina que estão em vigor, consulta as informações no PRI e carrega automaticamente os recursos que oferecem a melhor correspondência para essas configurações.

## <a name="package-resource-index-pri-file"></a>Arquivo PRI (Índice de Recurso do Pacote)
Cada pacote de aplicativo deve conter um índice binário dos recursos no app. Esse índice é criado no momento da compilação e está contido em um ou mais arquivos de índice de recurso do pacote (PRI).

- Um arquivo PRI contém recursos de cadeia de caracteres reais e um conjunto indexado de caminhos de arquivo que fazem referência a vários arquivos do pacote.
- Um pacote geralmente contém um único arquivo PRI por idioma, chamado resources.pri.
- O arquivo resources.pri na raiz de cada pacote é carregado automaticamente quando o [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) é instanciado.
- Os arquivos PRI podem ser criados e despejados com a ferramenta [MakePRI.exe](compile-resources-manually-with-makepri.md).
- Para o desenvolvimento comum de apps, você não precisará do MakePRI.exe, pois ele já está integrado ao fluxo de trabalho de compilação do Visual Studio. E o Visual Studio oferece suporte à edição de arquivos PRI em uma interface do usuário dedicada. No entanto, os tradutores e as ferramentas que eles usam talvez precisem do MakePRI.exe.
- Cada arquivo PRI contém uma coleção nomeada de recursos, conhecida como mapa de recursos. Quando um arquivo PRI de um pacote é carregado, o nome do mapa de recursos é verificado para constar se corresponde ao nome de identidade do pacote.
- Os arquivos PRI contêm somente dados, portanto, não use o formato de arquivo PE. Eles são criados especificamente para terem somente dados como formato de recurso do Windows. Eles substituem os recursos contidos em DLLs no modelo de app do Win32.
- O limite de tamanho em um arquivo PRI é 64 KB.

## <a name="uwp-api-access-to-app-resources"></a>Acesso da API da UWP aos recursos do app

### <a name="basic-functionality-resourceloader"></a>Funcionalidade básica (ResourceLoader)
A maneira mais simples de acessar os recursos de app de forma programática é usando o namespace [**Windows.ApplicationModel.Resources**](/uwp/api/windows.applicationmodel.resources?branch=live) e a classe [**ResourceLoader**](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live). **ResourceLoader** fornece acesso básico aos recursos de cadeia de caracteres a partir do conjunto de arquivos de recurso, bibliotecas referenciadas ou outros pacotes.

### <a name="advanced-functionality-resourcemanager"></a>Funcionalidade avançada (ResourceManager)
A classe [**ResourceManager**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live) (no namespace [**Windows.ApplicationModel.Resources.Core**](/uwp/api/windows.applicationmodel.resources.core?branch=live)) fornece informações adicionais sobre recursos, como enumeração e inspeção. Isso vai além do que a classe **ResourceLoader** oferecer.

Um objeto [**NamedResource**](/uwp/api/windows.applicationmodel.resources.core.namedresource?branch=live) representa um recurso lógico individual com vários idiomas ou outras variantes qualificadas. Ele descreve a exibição lógica do ativo ou do recurso, com um identificador de recurso de cadeia de caracteres, como `Header1`, ou um nome de arquivo de recurso, como `logo.jpg`.

O objeto [**ResourceCandidate**](/uwp/api/windows.applicationmodel.resources.core.resourcecandidate?branch=live) representa um valor de recurso concreto único e seus qualificadores, como a cadeia de caracteres "Hello World" para o inglês ou "logo.scale-100.jpg" como um recurso de imagem qualificado específico da resolução **scale-100**.

Os recursos disponíveis para um app são armazenados em coleções hierárquicas, que você pode acessar com um objeto [**ResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemap?branch=live). A classe **ResourceManager** fornece acesso às várias instâncias de **ResourceMap** de nível superior usadas pelo app, que correspondem aos vários pacotes do app. O valor [**MainResourceMap**](/uwp/api/windows.applicationmodel.resources.core.resourcemanager.MainResourceMap) corresponde ao mapa de recursos para o pacote de aplicativos atual e exclui quaisquer pacotes de estrutura referenciados. Cada **ResourceMap** é nomeado de acordo com o nome do pacote especificado no manifesto do pacote. Em um **ResourceMap** estão as subárvores (consulte [**ResourceMap.GetSubtree**](/uwp/api/windows.applicationmodel.resources.core.resourcemap.getsubtree?branch=live)), que ainda contêm os objetos **NamedResource**. As subárvores normalmente correspondem aos arquivos de recurso que contêm o recurso. Para obter mais informações, consulte [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote de aplicativos](localize-strings-ui-manifest.md) e [Carregar imagens e ativos personalizados para escala, tema, alto contraste e outros](images-tailored-for-scale-theme-contrast.md).

Veja um exemplo.

```csharp
// using Windows.ApplicationModel.Resources.Core;
ResourceMap resourceMap =  ResourceManager.Current.MainResourceMap.GetSubtree("Resources");
ResourceContext resourceContext = ResourceContext.GetForCurrentView()
var str = resourceMap.GetValue("String1", resourceContext).ValueAsString;
```

**Observação** O identificador de recurso é tratado como um fragmento de URI (Uniform Resource Identifier), sujeito à semântica de URI. Por exemplo, `GetValue("Caption%20")` é tratado como `GetValue("Caption ")`. Não use "?" ou "#" nos identificadores de recurso, pois eles encerram a avaliação do caminho de recurso. Por exemplo, "MyResource?3" é tratado como "MyResource".

Além de dar suporte ao acesso a recursos de cadeia de caracteres de um app, o **ResourceManager** também mantém a capacidade de enumerar e inspecionar os diversos recursos de arquivo. Para evitar colisões entre arquivos e outros recursos originados em um arquivo, os caminhos de arquivo indexados residem em uma subárvore "Files" reservada do **ResourceMap**. Por exemplo, o arquivo `\Images\logo.png` corresponde ao nome de recurso `Files/images/logo.png`.

As APIs [**StorageFile**](/uwp/api/Windows.Storage.StorageFile?branch=live) tratam as referências a arquivos como recursos de forma transparente e são apropriadas para cenários de uso típicos. O **ResourceManager** só deve ser usado em cenários avançados; por exemplo, quando você quer substituir o contexto atual.

### <a name="resourcecontext"></a>ResourceContext
Os candidatos a recursos são escolhidos com base em um [**ResourceContext**](/uwp/api/Windows.ApplicationModel.Resources.Core.ResourceContext?branch=live) específico, que é uma coleção de valores de qualificador de recurso (idioma, escala, contraste etc.). Um contexto padrão usa a configuração atual do aplicativo para cada valor de qualificador, a menos que seja substituído. Por exemplo, recursos como imagens podem ser qualificados para escala, o que varia de um monitor para o outro e, portanto, de uma exibição de aplicativo para outra. Por esse motivo, cada exibição de aplicativo tem um contexto padrão distinto. O contexto padrão de uma determinada exibição pode ser obtido por meio de [**ResourceContext.GetForCurrentView**](/uwp/api/windows.applicationmodel.resources.core.resourcecontext.GetForCurrentView). Sempre que você recupera um candidato a recurso, deve passar uma instância **ResourceContext** para obter o valor mais apropriado para uma determinada exibição.

## <a name="important-apis"></a>APIs importantes
* [ResourceLoader](/uwp/api/windows.applicationmodel.resources.resourceloader?branch=live)
* [ResourceManager](/uwp/api/windows.applicationmodel.resources.core.resourcemanager?branch=live)
* [ResourceContext](/uwp/api/windows.applicationmodel.resources.core.resourcecontext?branch=live)

## <a name="related-topics"></a>Tópicos relacionados
* [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md)
* [Carregar imagens e ativos personalizados para escala, tema, alto contraste e outros](images-tailored-for-scale-theme-contrast.md)
