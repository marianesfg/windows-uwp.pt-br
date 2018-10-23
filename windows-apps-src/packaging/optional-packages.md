---
author: laurenhughes
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Pacotes opcionais e conjunto de criação relacionado
description: Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, pacotes opcionais, conjunto relacionado, extensão de pacote, o visual studio
ms.localizationpriority: medium
ms.openlocfilehash: 4864bdaa1f32b980c5c8b159ca71bb6a56da4ec5
ms.sourcegitcommit: c4d3115348c8b54fcc92aae8e18fdabc3deb301d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/22/2018
ms.locfileid: "5407995"
---
# <a name="optional-packages-and-related-set-authoring"></a>Pacotes opcionais e conjunto de criação relacionado
Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Eles são úteis para conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho, ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original.

Conjuntos relacionados são uma extensão de pacotes opcionais, pois eles permitem que você aplique um conjunto estrito de versões em todos os pacotes principais e opcionais. Eles também permitem que você carregar o código nativo (C++) dos pacotes opcionais. 

## <a name="prerequisites"></a>Pré-requisitos

- Visual Studio 2017, versão 15.1
- Windows 10, versão 1703
- Windows 10, versão 1703 SDK

Para obter todas as ferramentas de desenvolvimento mais recentes, consulte [Downloads e ferramentas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar um aplicativo que usa pacotes opcionais e/ou conjuntos relacionados à Microsoft Store, você precisará de permissão. Pacotes opcionais e conjuntos relacionados podem ser usados para Linha de Negócios (LOB) ou aplicativos empresariais sem permissão do Centro de Desenvolvimento se não forem enviados para a Store. Consulte [Suporte do desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter permissão para enviar um aplicativo que usa pacotes opcionais e conjuntos relacionados.

### <a name="code-sample"></a>Exemplo de código
Enquanto você estiver lendo este artigo, é recomendável que você siga com o [exemplo de código do pacote opcional](https://github.com/AppInstaller/OptionalPackageSample) no GitHub para ter uma compreensão de pacotes opcionais como prática e relacionados conjuntos de trabalho no Visual Studio.

## <a name="optional-packages"></a>Pacotes opcionais
Para criar um pacote opcional no Visual Studio, você precisará:
1. Verifique se seu aplicativo **Versão mínima do destino da plataforma** é definida como: 10.0.15063.0.
2. No seu projeto do **pacote principal** , abra o `Package.appxmanifest` arquivo. Navegue até a guia "Packaging" e anote o **nome da família**, que é tudo antes do caractere "_".
3. No seu projeto do **pacote opcional** , clique com botão direito a `Package.appxmanifest` e selecione **Abrir com > Editor XML (texto)**.
4. Localize o `<Dependencies>` elemento no arquivo. Adicione o seguinte:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Substitua `[MainPackageDependency]` com o **nome da família** da etapa 2. Isso deve especificar que o **pacote opcional** depende do **pacote principal**.

Quando você tiver dependências do seu pacote configurar de etapas 1 a 4, você pode continuar a desenvolver como faria normalmente. Se você quiser carregar o código do pacote opcional no pacote principal, você precisará criar um conjunto relacionado. Consulte a seção [define relacionados](#related_sets) para obter mais detalhes.

O Visual Studio pode ser configurado para implantar seu pacote principal novamente sempre que você implanta um pacote opcional. Para definir a dependência de compilação no Visual Studio, você deve:

- Clique direito do mouse no projeto do pacote opcional e selecione **dependências de compilação > dependências do projeto...**
- Marque o projeto do pacote principal e selecione "Okey". 

Agora, sempre que você inserir F5 ou compilar um projeto do pacote opcional, Visual Studio criará o projeto do pacote principal pela primeira vez. Isso garantirá que o seu projeto principal e projetos opcionais são sincronizados.

## Conjuntos relacionados<a name="related_sets"></a>

Se você deseja carregar o código de um pacote opcional no pacote principal, você precisará criar um conjunto relacionado. Para criar um conjunto relacionado, seu pacote principal e pacote opcional devem ser intimamente ligadas. Os metadados de conjuntos relacionados é especificado no arquivo. appxbundle ou .msixbundle do pacote principal. Visual Studio ajuda você a obter os metadados correto em seus arquivos. Para configurar a solução do seu aplicativo para conjuntos relacionados, use as seguintes etapas:

1. Clique direito do mouse no projeto do pacote principal, selecione **Adicionar > Novo Item...**
2. Na janela, pesquisar os modelos instalados para ". txt" e adicione um novo arquivo de texto.
> [!IMPORTANT]
> O novo arquivo de texto deve ser nomeado: `Bundle.Mapping.txt`.
3. No `Bundle.Mapping.txt` arquivo que você especifica caminhos relativos para quaisquer projetos de pacote opcional ou pacotes externos. Um exemplo de `Bundle.Mapping.txt` arquivo deve parecer algo parecido com isto:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Quando sua solução é configurada dessa forma, o Visual Studio criará um manifesto de pacote para o pacote principal com todos os metadados necessários para conjuntos relacionados. 

Observe que, como pacotes opcionais, um `Bundle.Mapping.txt` arquivo para conjuntos relacionados só funcionará no Windows 10, versão 1703. Além disso, a versão de mínima da plataforma de destino do seu aplicativo deve ser definida como 10.0.15063.0.

## Problemas conhecidos<a name="known_issues"></a>

Não há suporte no momento ao depurar um projeto opcional do conjunto relacionado no Visual Studio. Para contornar esse problema, você pode implantar e iniciar a ativação (Ctrl + F5) e anexar manualmente o depurador a um processo. Para anexar o depurador, acesse o menu "Depuração" no Visual Studio, selecione "Anexar ao processo..." e anexe o depurador ao **processo de aplicativo principal**.