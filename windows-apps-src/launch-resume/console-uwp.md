---
title: Criar um aplicativo de console da Plataforma Universal do Windows
description: Este tópico descreve como escrever um aplicativo UWP que é executado em uma janela do console.
keywords: console uwp
ms.date: 08/02/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: c2dba15d78301c84f4064bcd6548d44e3c17beb2
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66366351"
---
# <a name="create-a-universal-windows-platform-console-app"></a>Criar um aplicativo de console da Plataforma Universal do Windows

Este tópico descreve como criar uma [ C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) ou C++/CX aplicativo de console de plataforma Universal do Windows (UWP).

Começando com o Windows 10, versão 1803, você pode escrever C++/WinRT ou C++aplicativos que são executados em uma janela de console, como uma janela do console DOS ou o PowerShell do console /CX UWP. Aplicativos de console usando a janela console para entrada e saída e pode usar [tempo de execução C Universal](/cpp/c-runtime-library/reference/crt-alphabetical-function-reference) funções, como **printf** e **getchar**. Aplicativos de console UWP podem ser publicados na Microsoft Store. Eles têm uma entrada na lista de aplicativos e um bloco principal que pode ser fixado no menu Iniciar. Aplicativos de console UWP podem ser iniciados no menu Iniciar, embora você normalmente irá iniciá-los na linha de comando.

Para ver um em ação, aqui está um vídeo sobre como criar um aplicativo de Console do UWP.

> [!VIDEO https://www.youtube.com/embed/bwvfrguY20s]

## <a name="use-a-uwp-console-app-template"></a>Usar um modelo de aplicativo de console UWP 

Para criar um aplicativo de console UWP, primeiro instale os **Modelos de projeto de aplicativo de console (Universal)** , disponíveis no [Visual Studio Marketplace](https://marketplace.visualstudio.com/items?itemName=AndrewWhitechapelMSFT.ConsoleAppUniversal). Os modelos instalados, em seguida, estão disponíveis sob **novo projeto** > **instalado** > **outras linguagens**  >  **Visual C++**  > **Windows Universal** como **do Console do aplicativo C + c++ /CLI WinRT (Windows Universal)** e **Console aplicativo C + c++ /CLI CX (Windows Universal )** .

## <a name="add-your-code-to-main"></a>Adicione o código ao main()

Os modelos adicionam **Program.cpp**, que contém a função `main()`. Aqui é onde começa a execução em um aplicativo de console UWP. Acesse os argumentos de linha de comando com os parâmetros `__argc` e `__argv`. O aplicativo de console UWP sai quando o controle retorna de `main()`.

O exemplo a seguir do **Program.cpp** é adicionado pela **c++ do aplicativo de Console c++ /CLI WinRT** modelo:

```cppwinrt
#include "pch.h"

using namespace winrt;

// This example code shows how you could implement the required main function
// for a Console UWP Application. You can replace all the code inside main
// with your own custom code.

int __cdecl main()
{
    // You can get parsed command-line arguments from the CRT globals.
    wprintf(L"Parsed command-line arguments:\n");
    for (int i = 0; i < __argc; i++)
    {
        wprintf(L"__argv[%d] = %S\n", i, __argv[i]);
    }

    // Keep the console window alive in case you want to see console output when running from within Visual Studio
      wprintf(L"Press 'Enter' to continue: ");
    getchar();
}
```

## <a name="uwp-console-app-behavior"></a>Comportamento do aplicativo de console UWP

Um aplicativo de console UWP pode acessar o sistema de arquivos do diretório de onde ele é executado, e abaixo. Isso é possível porque o modelo adiciona a extensão [AppExecutionAlias](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap5-appexecutionalias) ao arquivo Package.appxmanifest de seu aplicativo. Essa extensão também permite que o usuário digite o alias em uma janela do console para iniciar o aplicativo. O aplicativo não precisa estar no caminho do sistema para iniciar.

Além disso, você pode dar amplo acesso ao sistema de arquivos ao seu aplicativo de console UWP, adicionando a funcionalidade restrita `broadFileSystemAccess` conforme descrito em [Permissões de acesso a arquivos](https://docs.microsoft.com/windows/uwp/files/file-access-permissions). Esse recurso funciona com as APIs no namespace [**Windows.Storage**](https://docs.microsoft.com/uwp/api/Windows.Storage).

Mais de uma instância de um aplicativo de console UWP pode ser executada de uma vez porque o modelo adiciona a funcionalidade [SupportsMultipleInstances](multi-instance-uwp.md) ao arquivo Package.appxmanifest de seu aplicativo.

O modelo também adiciona a funcionalidade `Subsystem="console"` ao arquivo Package.appxmanifest, que indica que esse aplicativo UWP é um aplicativo de console. Observe os prefixos `desktop4` e iot2 `namespace`. Só há suporte para aplicativos de console UWP na área de trabalho e projetos da Internet das Coisas (IoT).

```xml
<Package
  ...
  xmlns:desktop4="http://schemas.microsoft.com/appx/manifest/desktop/windows10/4" 
  xmlns:iot2="http://schemas.microsoft.com/appx/manifest/iot/windows10/2" 
  IgnorableNamespaces="uap mp uap5 desktop4 iot2">
  ...
  <Applications>
    <Application Id="App"
      ...
      desktop4:Subsystem="console" 
      desktop4:SupportsMultipleInstances="true" 
      iot2:Subsystem="console" 
      iot2:SupportsMultipleInstances="true"  >
      ...
      <Extensions>
          <uap5:Extension 
            Category="windows.appExecutionAlias" 
            Executable="YourApp.exe" 
            EntryPoint="YourApp.App">
            <uap5:AppExecutionAlias desktop4:Subsystem="console">
              <uap5:ExecutionAlias Alias="YourApp.exe" />
            </uap5:AppExecutionAlias>
          </uap5:Extension>
      </Extensions>
    </Application>
  </Applications>
    ...
</Package>
```

## <a name="additional-considerations-for-uwp-console-apps"></a>Considerações adicionais para aplicativos de console UWP

- Somente C++/WinRT e C++/CX aplicativos UWP podem ser aplicativos de console.
- Os aplicativos de console UWP devem destinar-se ao tipo de projeto Área de trabalho ou IoT.
- Aplicativos de console UWP não podem criar uma janela. Eles não podem usar MessageBox(), ou Location() ou qualquer outra API que pode criar uma janela por qualquer motivo, como prompts de consentimento do usuário.
- Aplicativos de console UWP não podem consumir tarefas em segundo plano nem servem como uma tarefa em segundo plano.
- Com exceção de [Ativação de linha de comando](https://blogs.windows.com/buildingapps/2017/07/05/command-line-activation-universal-windows-apps/#5YJUzjBoXCL4MhAe.97), os aplicativos de console UWP não dão suporte a contratos de ativação, incluindo associação de arquivo, associação de protocolo etc.
- Embora os aplicativos de console UWP ofereçam suporte a várias instâncias, não dão suporte a [Redirecionamento de várias instâncias](multi-instance-uwp.md)
- Para obter uma lista de APIs do Win32 que estão disponíveis para aplicativos UWP, consulte [APIs Win32 e COM para aplicativos UWP](https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps)