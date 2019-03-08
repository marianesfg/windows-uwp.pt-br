---
ms.assetid: 3a59ff5e-f491-491c-81b1-6aff15886aad
title: Criação de pacotes opcionais e conjunto relacionado
description: Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp, pacotes opcionais, conjunto relacionado, extensão de pacote, visual studio
ms.localizationpriority: medium
ms.openlocfilehash: f62d6c99acc75033403fac7a498308cea6f7d3f8
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57594021"
---
# <a name="optional-packages-and-related-set-authoring"></a>Criação de pacotes opcionais e conjunto relacionado
Os pacotes opcionais contêm conteúdo que pode ser integrado com um pacote principal. Estes são úteis para o conteúdo para download (DLC), dividindo um aplicativo grande para restrições de tamanho ou para enviar qualquer conteúdo adicional para separado do seu aplicativo original.

Conjuntos relacionados são uma extensão de pacotes opcionais - eles permitem que você imponha um conjunto rigoroso de versões em pacotes principais e opcionais. Eles também permitem carregar código nativo (C ++) de pacotes opcionais. 

## <a name="prerequisites"></a>Pré-requisitos

- Visual Studio 2017, versão 15.1
- Windows 10, versão 1703
- Windows 10, versão 1703 SDK

Para obter todas as ferramentas de desenvolvimento mais recentes, consulte [Downloads e ferramentas para Windows 10](https://developer.microsoft.com/windows/downloads).

> [!NOTE]
> Para enviar um aplicativo que usa pacotes opcionais e/ou conjuntos relacionados para o Microsoft Store, você precisará de permissão. Pacotes opcionais e conjuntos relacionados podem ser usados para aplicativos de linha de negócios (LOB) ou enterprise sem permissão do Partner Center se eles não são enviados para a Store. Consulte [Suporte do desenvolvedor Windows](https://developer.microsoft.com/windows/support) para obter permissão para enviar um aplicativo que usa pacotes opcionais e conjuntos relacionados.

### <a name="code-sample"></a>Exemplo de código
Enquanto você está lendo este artigo, recomenda-se que você acompanhe o [exemplo de código do pacote opcional](https://github.com/AppInstaller/OptionalPackageSample) no GitHub para obter um conhecimento prático de como os pacotes opcionais e conjuntos relacionados funcionam no Visual Studio.

## <a name="optional-packages"></a>Pacotes opcionais
Para criar um pacote opcional no Visual Studio, você precisará:
1. Verifique se seu aplicativo **versão mínima da plataforma destino** é definida como: 10.0.15063.0 ou superior.
2. Em seu projeto **pacote principal**, abra o arquivo `Package.appxmanifest`. Navegue até a guia "Pacotes" e anote seu **nome de família do pacote**, que é tudo antes do caractere "_".
3. Em seu projeto **pacote opcional**, clique com botão direito em `Package.appxmanifest` e selecione **Abrir com > Editor XML (texto)**.
4. Localize o elemento `<Dependencies>` no arquivo. Adicione o seguinte:

```XML
<uap3:MainPackageDependency Name="[MainPackageDependency]"/>
```

Substitua `[MainPackageDependency]`com seu **nome de família do pacote** a partir da etapa 2. Isso especifica que seu **pacote opcional** depende seu **pacote principal**.

Depois de ter suas dependências de pacotes configuradas nos Passos 1 a 4, você pode continuar desenvolvendo como você faria normalmente. Se você quiser carregar o código do pacote opcional no pacote principal, você precisará criar um conjunto relacionado. Consulte a seção [Conjuntos relacionados](#related_sets) para obter mais detalhes.

O Visual Studio pode ser configurado para implementar novamente seu pacote principal sempre que você implantar um pacote opcional. Para definir a dependência de compilação no Visual Studio, você deve:

- Clique com o botão direito no projeto de pacote opcional e selecione **Dependências da compilação > Dependências do projeto...**
- Verifique se o projeto do pacote principal e selecione "OK". 

Agora, toda vez que você pressionar F5 ou criar um projeto de pacote opcional, o Visual Studio irá construir o projeto do pacote principal primeiro. Isso garante que seu projeto principal e projetos opcionais estejam em sincronia.

## Conjuntos relacionados<a name="related_sets"></a>

Se você quiser carregar o código do pacote opcional no pacote principal, você precisará criar um conjunto relacionado. Para compilar um conjunto relacionado, o pacote principal e o pacote opcional devem ser bem acoplados. Os metadados para conjuntos relacionados é especificado no arquivo. appxbundle ou .msixbundle do pacote principal. O Visual Studio ajuda você a obter os metadados corretos em seus arquivos. Para configurar a solução do seu aplicativo para conjuntos relacionados, use as seguintes etapas:

1. Clique com botão direito do projeto do pacote principal, selecione **Adicionar > Novo Item...**
2. Na janela, pesquise os modelos instalados para ". txt" e adicione um novo arquivo de texto.
> [!IMPORTANT]
> O novo arquivo de texto deve ser nomeado: `Bundle.Mapping.txt`.

3. No arquivo `Bundle.Mapping.txt`, você especificará caminhos relativos para todos os projetos de pacote opcional ou pacotes externos. Um arquivo `Bundle.Mapping.txt` de amostra deve ser algo parecido com isto:

```syntax
[OptionalProjects]
"..\ActivatableOptionalPackage1\ActivatableOptionalPackage1.vcxproj"
"..\ActivatableOptionalPackage2\ActivatableOptionalPackage2.vcxproj"

[ExternalPackages]
"..\ActivatableOptionalPackage1\x86\Release\ActivatableOptionalPackage3_1.1.1.0\ ActivatableOptionalPackage3_1.1.1.0.appx"
```

Quando sua solução for configurada dessa maneira, o Visual Studio criará um manifesto de pacote para o pacote principal com todos os metadados necessários para conjuntos relacionados. 

Observe que, como pacotes opcionais, um `Bundle.Mapping.txt` arquivo conjuntos relacionados de só funcionará no Windows 10, versão 1703 ou posterior. Além disso, Min versão da plataforma de destino do seu aplicativo deve ser definido como 10.0.15063.0 ou superior.

## Problemas conhecidos<a name="known_issues"></a>

A depuração de um projeto opcional de conjunto relacionado não é atualmente suportada no Visual Studio. Para resolver este problema, você pode implantar e iniciar a ativação (Ctrl + F5) e anexar manualmente o depurador a um processo. Para anexar o depurador, vá no menu "Depurar" no Visual Studio, selecione "Anexar ao Processo ..." e anexe o depurador ao **processo principal do aplicativo**.