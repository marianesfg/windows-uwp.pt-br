---
author: mcleblanc
ms.assetid: B48E21AB-0EA5-444B-8333-393DD8D1B76D
title: Armazenamento compartilhado corporativo
description: O armazenamento compartilhado corporativo define localizações de dados locais para aplicativos de linha de negócios para compartilhar dados.
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 5236767d4c02d873106c7b1799c8428d84cccd53
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6846878"
---
# <a name="enterprise-shared-storage"></a>Armazenamento compartilhado corporativo

O armazenamento compartilhado consiste em duas localizações nas quais aplicativos com a funcionalidade restrita **enterpriseDeviceLockdown** e um certificado corporativo têm total acesso de leitura e gravação. Observe que a funcionalidade **enterpriseDeviceLockdown** permite que os aplicativos usem a API de bloqueio do dispositivo e acessem as pastas de armazenamento compartilhado corporativo. Para saber mais sobre a API, consulte o namespace [**Windows.Embedded.DeviceLockdown**](http://go.microsoft.com/fwlink/?LinkId=699331).  

Essas localizações são definidas na unidade local:
- \Data\SharedData\Enterprise\Persistent
- \Data\SharedData\Enterprise\Non-Persistent

## <a name="scenarios"></a>Cenários

O armazenamento compartilhado corporativo dá suporte aos cenários a seguir.

- Você pode compartilhar dados em uma instância de um aplicativo, entre instâncias do mesmo aplicativo ou até mesmo entre aplicativos pressupondo que eles tenham a funcionalidade e o certificado apropriados.
- Você pode armazenar dados no disco rígido local, na pasta \Data\SharedData\Enterprise\Persistent, e eles manterão a persistência mesmo após a redefinição do dispositivo.
- Manipule arquivos, incluindo a leitura, a gravação e a exclusão de arquivos, em um dispositivo por meio do serviço MDM (Gerenciamento de Dispositivo Móvel). Para saber mais sobre como usar o armazenamento compartilhado corporativo por meio do serviço MDM, consulte [EnterpriseExtFileSystem CSP](http://go.microsoft.com/fwlink/?LinkId=699333).

## <a name="access-enterprise-shared-storage"></a>Acessar o armazenamento compartilhado corporativo

O exemplo a seguir mostra como declarar a funcionalidade para acessar o armazenamento compartilhado corporativo no manifesto do pacote e como acessar as pastas de armazenamento compartilhadas usando a classe Windows.Storage.StorageFolder.

No seu manifesto de pacote do aplicativo, inclua a seguinte funcionalidade:

```xml
<Package
  xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
  xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
  xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
  xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
  IgnorableNamespaces="uap mp rescap">

…

<Capabilities>
    <rescap:Capability Name="enterpriseDeviceLockdown"/>
</Capabilities>
```

Para acessar a localização dos dados compartilhados, seu aplicativo pode usar o código a seguir.

```csharp
using System;
using System.Collections.Generic;
using System.Diagnostics;
using Windows.Storage;

…

// Get the Enterprise Shared Storage folder.
var enterprisePersistentFolderRoot = @"C:\Data\SharedData\Enterprise\Persistent";

StorageFolder folder =
    await StorageFolder.GetFolderFromPathAsync(enterprisePersistentFolderRoot);

// Get the files in the folder.
IReadOnlyList<StorageFile> sortedItems =
    await folder.GetFilesAsync();

// Iterate over the results and print the list of files
// to the Visual Studio Output window.
foreach (StorageFile file in sortedItems)
    Debug.WriteLine(file.Name + ", " + file.DateCreated);
```

