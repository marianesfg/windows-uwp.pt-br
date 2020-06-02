---
title: Criar o manifesto do pacote
description: ''
author: denelon
ms.author: denelon
ms.date: 04/29/2020
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: 8eceb29abbdc7f765628dbd8dbd6f6d0be21f132
ms.sourcegitcommit: e2689c72d5b381eafdb1075090d1961f4c1cb37a
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/27/2020
ms.locfileid: "84055150"
---
# <a name="create-your-package-manifest"></a>Criar o manifesto do pacote

[!INCLUDE [preview-note](../../includes/package-manager-preview.md)]

Se você desejar enviar um pacote de software para o [repositório do Gerenciador de Pacotes do Windows](repository.md), comece criando um manifesto do pacote. O manifesto é um arquivo YAML que descreve o aplicativo a ser instalado.

Este artigo descreve o conteúdo de um manifesto do pacote para o Gerenciador de Pacotes do Windows.

## <a name="yaml-basics"></a>Noções básicas sobre YAML

O formato YAML foi escolhido para manifestos do pacote devido à facilidade relativa de legibilidade por humanos à consistência com outras ferramentas de desenvolvimento da Microsoft. Se você não estiver familiarizado com a sintaxe do YAML, poderá aprender as noções básicas em [Aprender sobre o YAML em Y minutos](https://learnxinyminutes.com/docs/yaml/).

> [!NOTE]
> No momento, os manifestos do Gerenciador de Pacotes do Windows não dão suporte a todos os recursos do YAML. Os recursos do YAML sem suporte incluem âncoras, chaves complexas e conjuntos.

## <a name="conventions"></a>Convenções

Essas convenções são usadas neste artigo:

* À esquerda de `:`, há uma palavra-chave literal usada em definições de manifesto.
* À direita de `:`, há um tipo de dados. O tipo de dados pode ser um tipo primitivo como **cadeia de caracteres** ou uma referência a uma estrutura avançada definida em outro lugar neste artigo.
* A notação `[` *datatype* `]` indica uma matriz do tipo de dados mencionado. Por exemplo, `[ string ]` é uma matriz de cadeias de caracteres.
* A notação `{` *datatype* `:` *datatype* `}` indica um mapeamento de um tipo de dados para outro. Por exemplo, `{ string: string }` é um mapeamento de cadeias de caracteres para cadeias de caracteres.

## <a name="manifest-contents"></a>Conteúdo do manifesto

Um manifesto do pacote deve incluir um conjunto de itens necessários e também pode incluir outros itens opcionais que podem ajudar a aprimorar a experiência do cliente com a instalação do seu software. Esta seção fornece breves resumos do esquema de manifesto necessário e dos esquemas de manifesto completos, além de exemplos de cada um.

Cada campo no arquivo de manifesto deve ser escrito na formatação Pascal e não pode ser duplicado.

Para obter uma lista completa e descrições de itens em um manifesto, confira a [especificação de manifesto](https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md) no repositório [https://github.com/microsoft/winget-cli](https://github.com/microsoft/winget-cli).

### <a name="minimal-required-schema"></a>Esquema mínimo necessário

#### <a name="minimal-required-schema"></a>[Esquema mínimo necessário](#tab/minschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
Version: string # Version numbering format.
License: string # The open source license or copyright.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Installers:
  - Arch: string # Enumeration of supported architectures.
  - URL: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
ManifestVersion: 0.1.0
```

#### <a name="example"></a>[Exemplo](#tab/minexample/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

### <a name="complete-schema"></a>Esquema completo

#### <a name="complete-schema"></a>[Esquema completo](#tab/compschema/)

```yaml
Id: string # Publisher.package format.
Publisher: string # The name of the publisher.
Name: string # The name of the application.
AppMoniker: string # The common name someone may use to search for the package.
Version: string # Version numbering format for package version.
Channel: string # A string representing the flight ring.
License: string # The open source license or copyright.
LicenseUrl: string # Valid secure URL to license.
MinOSVersion: string # Version numbering format for minimum version of Windows supported.
Description: string # Description of the package.
Homepage: string # Valid secure URL for the package.
Tags: list # Additional strings a user would use to search for the package.
FileExtensions: list # List of file extensions the package could support.
Protocols: list # List of protocols the package provides a handler for.
Commands: list # List of commands or aliases the user would use to run the package.
InstallerType: string # Enumeration of supported installer types (exe, msi, msix, inno, wix, nullsoft, appx).
Custom: string # Custom switches passed to the installer.
Silent: string # Switches passed to the installer for silent installation.
SilentWithProgress: string # Switches passed to the installer for non-interactive install.
Interactive: string # Experimental.
Language: string # Experimental.
Log: string # Specifies log redirection switches and path.
InstallLocation: string # Specifies alternate location to install package.
Installers: # Nested map of keys for specific installer.
  - Arch: string # Enumeration of supported architectures.
  - URL: string # Path to download installation file.
  - Sha256: string # SHA256 calculated from installer.
  - SignatureSha256: string # SHA256 calculated from signature file's hash of MSIX file.
  - Switches: # Collection of entries to override root keys. The primary supported values are: Custom, Silent, SilentWithProgress, Interactive. For a complete list see the specification at https://github.com/microsoft/winget-cli/blob/master/doc/ManifestSpecv0.1.md.
  - Scope: string # Experimental.
  - SystemAppId: string # Experimental.
Localization: # Nested map of keys for localization.
  - Language: string # Locale for display fields and localized URLs.
ManifestVersion: string # Version number format for manifest version.
```

#### <a name="good-example"></a>[Exemplo bom](#tab/good/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

#### <a name="better-example"></a>[Exemplo melhor](#tab/better/)

```yaml
Id: microsoft.teams
Publisher: Microsoft Corporation
Name: Microsoft Teams
Version: 1.3.0.4461
AppMoniker: teams
MinOSVersion: 10.0.0.0
Description: The hub for teamwork in Microsoft 365
Homepage: https://www.microsoft.com/microsoft/teams
License: Copyright (c) Microsoft Corporation. All rights reserved.
LicenseUrl: https://docs.microsoft.com/en-us/MicrosoftTeams/assign-teams-licenses
InstallerType: exe
Installers:
  - Arch: x64
    Url: https://statics.teams.cdn.office.net/production-windows-x64/1.3.00.4461/Teams_windows_x64.exe
    Sha256: 712f139d71e56bfb306e4a7b739b0e1109abb662dfa164192a5cfd6adb24a4e1
ManifestVersion: 0.1.0
```

* * *

> [!NOTE]
> Se o instalador for um .exe e tiver sido criado usando o Nullsoft ou o Inno, será possível especificar esses valores em vez disso. Quando o Nullsoft ou o Inno forem especificados, o cliente definirá automaticamente os comportamentos de instalação silenciosa e silenciosa com progresso para o instalador.

## <a name="tips-and-best-practices"></a>Dicas e melhores práticas

* Para obter a melhor experiência do cliente quando ele localizar e instalar seu software, recomendamos que você inclua o máximo possível de itens opcionais além do esquema necessário. Por exemplo, o campo `AppMoniker` é opcional. No entanto, se você incluir esse campo, os clientes verão os resultados associados ao valor `AppMoniker` ao executarem o comando [search](../winget/search.md) (por exemplo, **vscode** para **Visual Studio Code**). Se houver apenas um aplicativo com o valor `AppMoniker` especificado, os clientes poderão instalar seu aplicativo especificando o moniker em vez da ID totalmente qualificada.
* O `Id` deve ser exclusivo. Não é possível ter vários envios com o mesmo identificador de pacote. Evite espaços, pois isso exigirá que os usuários coloquem `Id` entre aspas ao usar o cliente do [winget](../index.md).
* Evite criar várias pastas do editor. Por exemplo, não crie "Contoso Ltd" se já houver uma pasta "Contoso". Evite também espaços ao criar pastas.
* Todos os pacotes devem ser enviados com uma instalação silenciosa, se possível. Se você tiver um executável que não dá suporte a uma instalação silenciosa, a experiência do usuário será diminuída.
* Limite o comprimento de cadeias de caracteres em seu manifesto a 100 caracteres antes de uma quebra de linha.
* Quando houver mais de um tipo de instalador para a versão do pacote especificada, uma instância de `InstallerType` poderá ser colocada em cada um dos `Installers`.
