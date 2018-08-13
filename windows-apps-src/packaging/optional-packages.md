---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Pacotes opcionais e conjunto de criação relacionado
description: Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original.
ms.author: lahugh
ms.date: 04/05/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, pacotes opcionais, conjunto relacionado, extensão do pacote, o visual studio
ms.localizationpriority: medium
ms.openlocfilehash: d66a511211396190393e31bfd553149a1e89fad0
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "406076"
---
# <a name="optional-packages-and-related-set-authoring"></a>Pacotes opcionais e conjunto de criação relacionado
Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estas são úteis para download de conteúdo (DLC), dividindo um aplicativo grande para restrições de tamanho, ou para qualquer conteúdo adicional de remessa separam do seu aplicativo original.

Os conjuntos de relacionados são uma extensão da pacotes opcionais--elas permitem que você aplique um conjunto estrito de versões entre os pacotes principais e opcionais. Eles também permitem que você carregar código nativo (C++) de pacotes opcionais. 

## <a name="prerequisites"></a>Pré-requisitos

- 2017 do Visual Studio, versão 15.1
- Windows 10, versão 1703
- Windows 10, versão 1703 SDK

Para obter todas as ferramentas de desenvolvimento mais recentes, consulte [Downloads e ferramentas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar um aplicativo que usa pacotes opcionais e/ou conjuntos relacionados para o Microsoft Store, você precisará de permissão. Pacotes opcionais e conjuntos relacionados podem ser usados para Linha de Negócios (LOB) ou aplicativos empresariais sem permissão do Centro de Desenvolvimento se não forem enviados para a Store. Consulte [Suporte do desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter permissão para enviar um aplicativo que usa pacotes opcionais e conjuntos relacionados.

### <a name="code-sample"></a>Exemplo de código
Enquanto você estiver lendo este artigo, é recomendável que você acompanhe a [amostra de código do pacote opcional](https://github.com/AppInstaller/OptionalPackageSample) no GitHub para uma compreensão prática dos pacotes como opcionais e relacionadas ao trabalho de conjuntos de dentro do Visual Studio.

## <a name="optional-packages"></a>Pacotes opcionais
Para criar um pacote opcional no Visual Studio, você precisará:
1. Verifique se o da seu aplicativo **Versão de Min da plataforma de destino** estiver definida como: 10.0.15063.0.
2. Em seu projeto de **pacote principal** , abra o `Package.appxmanifest` arquivo. Navegue até a guia "Empacotamento" e anote o **nome do pacote família**, que é tudo antes do caractere "_".
3. A partir de seu projeto de **pacote opcional** , clique com botão direito do `Package.appxmanifest` e selecione **Abrir com > Editor XML (texto)**.
4. Localize o `<Dependencies>` elemento no arquivo. Adicione o seguinte:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Substituir `[MainPackageDependency]` com seu **nome de família do pacote** da etapa 2. Isso especificará que seu **pacote opcional** depende seu **pacote principal**.

Uma vez que as dependências do seu pacote configurar das etapas 1 a 4, você pode continuar a desenvolver como faria normalmente. Se você gostaria de carga do código do pacote de opcional para o pacote principal, você precisará criar um conjunto relacionado. Consulte a seção [relacionado ao define](#related_sets) para obter mais detalhes.

Visual Studio pode ser configurado para implantar seu pacote principal novamente a cada vez que você implantar um pacote opcional. Para definir a dependência de compilação no Visual Studio, você deve:

- Clique com botão direito do projeto de pacote opcional e selecione **dependências Construir > dependências do projeto …**
- Verifique o projeto de pacote principal e selecione "Okey". 

Agora, sempre que você insira F5 ou cria um projeto de pacote opcional, Visual Studio iremos construir o projeto de pacote principal pela primeira vez. Isso garantirá que seu projeto principal e projetos opcionais estão sincronizados.

## Conjuntos relacionados<a name="related_sets"></a>

Se você deseja carregar o código de um pacote opcional para o pacote principal, você precisará criar um conjunto relacionado. Para criar um conjunto relacionado, seu principal e o pacote opcional devem ser fortemente agrupados. Os metadados para conjuntos relacionados é especificado no `.appxbundle` arquivo do pacote do principal. Visual Studio ajuda a obter os metadados correto em seus arquivos. Para configurar a solução do seu aplicativo para conjuntos relacionados, siga estas etapas:

1. Clique com botão direito do projeto de pacote principal, selecione **Adicionar > Novo Item …**
2. Na janela, pesquisar os modelos instalados para ". txt" e adicione um novo arquivo de texto.
> [!IMPORTANT]
> O novo arquivo de texto deve ser nomeado: `Bundle.Mapping.txt`.
3. No `Bundle.Mapping.txt` arquivo você especificará os caminhos relativos a qualquer projetos de pacote opcional ou pacotes externos. Uma amostra `Bundle.Mapping.txt` arquivo deve ser parecida com isso:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Quando sua solução é configurada dessa forma, o Visual Studio criará um manifesto de pacote para o pacote principal com todos os metadados necessários para conjuntos de relacionados. 

Observe que, como pacotes opcionais, um `Bundle.Mapping.txt` arquivo para conjuntos relacionados só funcionará em Windows 10, versão 1703. Além disso, a versão de Min da plataforma de destino do seu aplicativo deve ser definida como 10.0.15063.0.

## Problemas conhecidos<a name="known_issues"></a>

Não é suportado no momento ao depurar um projeto opcional do conjunto relacionado no Visual Studio. Para contornar esse problema, você pode implantar e início a ativação (Ctrl + F5) e anexar manualmente o depurador a um processo. Para anexar o depurador, acesse o menu "Debug" no Visual Studio, selecione "Anexar ao processo …" e anexar o depurador ao **processo do aplicativo principal**.