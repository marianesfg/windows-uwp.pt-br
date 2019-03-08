---
title: Usar o pacote NuGet do Xbox Live com o XDK
description: Saiba como usar o pacote NuGet de API do Xbox Live para desenvolver os títulos XDK.
ms.assetid: 2c5ae514-393d-48bb-afd8-a897d35f7938
ms.date: 04/04/2017
ms.topic: article
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, o NuGet
ms.localizationpriority: medium
ms.openlocfilehash: c955ca42c09075e5125683588c335cfa47443f00
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57655031"
---
# <a name="use-the-xbox-live-api-nuget-package-to-develop-xdk-titles"></a>Usar o pacote NuGet de API do Xbox Live para desenvolver títulos XDK

### <a name="1--ensure-you-have-the-latest-nuget-package-manager-installed"></a>1.  Verifique se que você tem o Gerenciador de pacotes do NuGet mais recente instalado
1.  Verificar a versão atual:
    - Na barra de menus, selecione Ferramentas -> extensões e atualizações.
    - Sob a aba instalado, procure `NuGet Package Manager`
![](../images/nuget/nuget_uwp_install_1.png)
2.  Para atualizar sua versão atual:
    - Na barra de menus, selecione Ferramentas -> extensões e atualizações.
    - Em atualizações -> guia de galeria do Visual Studio, selecione `Update`
![](../images/nuget/nuget_uwp_install_2.png)

### <a name="2--add-reference-to-the-project"></a>2.  Adicionar a referência ao projeto
1.  Adicionar a referência ao projeto
    1.  Clique com o botão direito em sua solução do projeto e selecione "Gerenciar pacotes NuGet"
<br/>
![](../images/nuget/nuget_xbox_install_4.png)
1.  Pesquise `Xbox Live` e selecione o pacote apropriado e clique em `Install`.
  - A API de serviços do Xbox possui tipos para UWP e XDK, em C++ e do WinRT.  
  - Escolher entre `Microsoft.Xbox.Live.SDK.*.UWP` e `Microsoft.Xbox.Live.SDK.*.XboxOneXDK`.  `XboxOneXDK` é para ID@Xbox e desenvolvedores gerenciados que estão usando um Xbox XDK.  `UWP` é para jogos UWP que podem ser executados no PC, o Xbox One ou Windows Phone.  Você pode ler mais sobre a execução de UWP no Xbox One em [https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started](https://docs.microsoft.com/en-us/windows/uwp/xbox-apps/getting-started)
  - Escolher entre `Microsoft.Xbox.Live.SDK.Cpp.*` e `Microsoft.Xbox.Live.SDK.WinRT.*`. `Cpp` é para os mecanismos de jogos C++ usando as APIs do Xbox Live.  `WinRT` é para mecanismos de jogos escritos com C++, C#, ou Javascript usando as APIs do Xbox Live.  Ao usar o WinRT com um mecanismo de C++, você usaria C + + c++ /CX que usa chapéus (^).  `Cpp` é a API recomendada para uso para mecanismos de jogos do C++.    
![](../images/nuget/nuget_xbox_install_5.png)
![](../images/nuget/nuget_uwp_install_7.png)
1. Depois de aceitar a licença TOS, aguarde até que o pacote foi adicionado com êxito.  Você verá esse log na janela de saída do Gerenciador de pacotes:

```
========== Finished ==========
```

### <a name="3--optionally-include-header"></a>3.  Opcionalmente, inclua o cabeçalho
* Para `Microsoft.Xbox.Live.SDK.Cpp.*` projetos com base em `#include <xsapi\services.h>` na fonte do seu projeto.
* Para `Microsoft.Xbox.Live.SDK.WinRT.*` com base em projetos, há necessidade de incluir todos os cabeçalhos.   
