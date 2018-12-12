---
title: Automatizar a inicialização de aplicativos UWP (Plataforma Universal do Windows) para Windows 10
description: Os desenvolvedores podem usar a ativação de protocolos e a ativação de inicialização para automatizar a inicialização de seus aplicativos UWP ou jogos para testes automatizados.
ms.topic: article
ms.localizationpriority: medium
ms.date: 02/08/2017
ms.openlocfilehash: fb68b4bbd1b751591e9f336efe5dad3c22b3bf92
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8937195"
---
# <a name="automate-launching-windows-10-uwp-apps"></a>Automatizar a inicialização de aplicativos UWP do Windows 10

## <a name="introduction"></a>Introdução

Os desenvolvedores têm várias opções para realizar a inicialização automatizada de aplicativos UWP (Plataforma Universal do Windows). Neste documento, exploraremos métodos de inicialização de um aplicativo usando a ativação de protocolos e a ativação de inicialização.

*Ativação de protocolo* permite que um aplicativo registrou como um manipulador para um determinado protocolo. 

*Ativação de inicialização* é a inicialização normal de um aplicativo, tal como iniciar no bloco do aplicativo.

Com cada método de ativação, você tem a opção de usar a linha de comando ou um aplicativo inicializador. Em todos os métodos de ativação, se o aplicativo estiver sendo executado no momento, a ativação trará o aplicativo para o primeiro plano (reativando-o) e fornecerá os novos argumentos de ativação. Isso oferece flexibilidade ao uso de comandos de ativação para fornecer novas mensagens para o aplicativo. É importante observar que o projeto precisa ser compilado e implantado para que o método de ativação execute o aplicativo recém-atualizado. 

## <a name="protocol-activation"></a>Ativação de protocolos

Siga estas etapas para configurar a ativação de protocolo de aplicativos: 

1. Abra o arquivo **Package.appxmanifest** no Visual Studio.
2. Selecione a guia **Declarações**.
3. No menu suspenso **Declarações Disponíveis**, selecione **Protocolo** e **Adicionar**.
4. Em **Propriedades**, no campo **Nome**, insira um nome exclusivo para iniciar o aplicativo. 

    ![Ativação de protocolos](images/automate-uwp-apps-1.png)

5. Salve o arquivo e implante o projeto. 
6. Depois que o projeto tiver sido implantado, o protocolo de ativação deve ser definido. 
7. Vá para **Painel de Controle\Todos os Itens do Painel de Controle\Programas Padrão** e selecione **Associar tipo de arquivo ou protocolo com um programa específico**. Role para a seção **Protocolos** para ver se o protocolo está listado. 

Agora que a ativação de protocolo está configurada, você tem duas opções (a linha de comando ou o aplicativo inicializador) para ativar o aplicativo usando o protocolo. 

### <a name="command-line"></a>Linha de comando

O aplicativo pode ser ativado por protocolo usando a linha de comando com o comando iniciar seguido pelo nome do protocolo definido anteriormente, dois-pontos (":") e parâmetros. Os parâmetros podem ser qualquer cadeia de caracteres arbitrária. No entanto, para aproveitar os recursos de URI (identificador de recurso uniforme), é aconselhável seguir o formato de URI padrão: 

  ```
  scheme://username:password@host:port/path.extension?query#fragment
  ```

O objeto Uri tem métodos para analisar uma cadeia de caracteres do URI nesse formato. Para obter mais informações, consulte [Classe Uri (MSDN)](https://msdn.microsoft.com/library/windows/apps/windows.foundation.uri.aspx). 

Exemplos:

  ```
  >start bingnews:
  >start myapplication:protocol-parameter
  >start myapplication://single-player/level3?godmode=1&ammo=200
  ```

A ativação de linha de comando do protocolo dá suporte a caracteres Unicode até um limite de 2038 caracteres no URI bruto. 

### <a name="launcher-application"></a>Aplicativo inicializador

Para iniciar, crie um aplicativo separado que dê suporte à API do WinRT. O código C++ para a inicialização com a ativação de protocolo em um programa inicializador é mostrado no exemplo a seguir, onde **PackageURI** é o URI do aplicativo com argumentos. Por exemplo `myapplication:` ou `myapplication:protocol activation arguments`.

```
bool ProtocolLaunchURI(Platform::String^ URI)
{
       IAsyncOperation<bool>^ protocolLaunchAsyncOp;
       try
       {
              protocolLaunchAsyncOp = Windows::System::Launcher::LaunchUriAsync(ref new 
Uri(URI));
       }
       catch (Platform::Exception^ e)
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI Exception Thrown: " 
+ e->ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }

       concurrency::create_task(protocolLaunchAsyncOp).wait();

       if (protocolLaunchAsyncOp->Status == AsyncStatus::Completed)
       {
              bool LaunchResult = protocolLaunchAsyncOp->GetResults();
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI 
+ " completed. Launch result " + LaunchResult + "\n";
              OutputDebugString(dbgStr->Data());
              return LaunchResult;
       }
       else
       {
              Platform::String^ dbgStr = "ProtocolLaunchURI " + URI + " failed. Status:" 
+ protocolLaunchAsyncOp->Status.ToString() + " ErrorCode:" 
+ protocolLaunchAsyncOp->ErrorCode.ToString() + "\n";
              OutputDebugString(dbgStr->Data());
              return false;
       }
}
```
A ativação de protocolo com o aplicativo inicializador tem as mesmas limitações de argumentos que a ativação de protocolo com a linha de comando. Ambas dão suporte a caracteres Unicode até um limite de 2038 caracteres no URI bruto. 

## <a name="launch-activation"></a>Ativação de inicialização

Você também pode iniciar o aplicativo usando a ativação de inicialização. Nenhuma configuração é necessária, mas a AUMID (ID do Modelo do Usuário do Aplicativo) do aplicativo UWP é necessária. A AUMID é o nome de família do pacote seguido por um ponto de exclamação e a ID do aplicativo. 

A melhor maneira de obter o nome de família do pacote é concluir estas etapas:

1. Abra o arquivo **Package.appxmanifest**.
2. Na guia **Empacotamento**, insira o **Nome do pacote**.

    ![Ativação de inicialização](images/automate-uwp-apps-2.png)

3. Se o **Nome de família do pacote** não estiver listado, abra o PowerShell e execute `>get-appxpackage MyPackageName` para encontrar o **PackageFamilyName**.

A ID do aplicativo pode ser encontrada no arquivo **Package.appxmanifest** (aberto no modo de exibição XML) sob o elemento `<Applications>`.

### <a name="command-line"></a>Linha de comando

Uma ferramenta para executar uma ativação de inicialização de um aplicativo UWP é instalada com o SDK do Windows 10. Ela pode ser executada na linha de comando, e isso faz a AUMID do aplicativo ser iniciada como um argumento.

```
C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe <AUMID>
```

Ela deveria ter a seguinte aparência:

```
"C:\Program Files (x86)\Windows Kits\10\App Certification Kit\microsoft.windows.softwarelogo.appxlauncher.exe" MyPackageName_ph1m9x8skttmg!AppId
```

Essa opção não dá suporte a argumentos de linha de comando. 

### <a name="launcher-application"></a>Aplicativo inicializador

Você pode criar um aplicativo separado que dê suporte ao uso de COM na inicialização. O exemplo a seguir mostra o código C++ a ser iniciado com a ativação de inicialização em um programa inicializador. Com esse código, você pode criar um objeto **ApplicationActivationManager** e chamar **ActivateApplication** passando a AUMID encontrada anteriormente e quaisquer argumentos. Para obter mais informações sobre os outros parâmetros, consulte[Método IApplicationActivationManager::ActivateApplication (MSDN)](https://msdn.microsoft.com/library/windows/desktop/hh706903(v=vs.85).aspx).

```
#include <ShObjIdl.h>
#include <atlbase.h>

HRESULT LaunchApp(LPCWSTR AUMID)
{
     HRESULT hr = CoInitializeEx(nullptr, COINIT_APARTMENTTHREADED);
     if (FAILED(hr))
     {
            wprintf(L"LaunchApp %s: Failed to init COM. hr = 0x%08lx \n", AUMID, hr);
     }
     {
            CComPtr<IApplicationActivationManager> AppActivationMgr = nullptr;
            if (SUCCEEDED(hr))
            {
                   hr = CoCreateInstance(CLSID_ApplicationActivationManager, nullptr,  
CLSCTX_LOCAL_SERVER, IID_PPV_ARGS(&AppActivationMgr));
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to create Application Activation 
Manager. hr = 0x%08lx \n", AUMID, hr);
                   }
            }
            if (SUCCEEDED(hr))
            {
                   DWORD pid = 0;
                   hr = AppActivationMgr->ActivateApplication(AUMID, nullptr, AO_NONE, 
&pid);
                   if (FAILED(hr))
                   {
                         wprintf(L"LaunchApp %s: Failed to Activate App. hr = 0x%08lx 
\n", AUMID, hr);
                   }
            }
     }
     CoUninitialize();
     return hr;
}
```

É importante notar que esse método oferece suporte a argumentos transmitidos, ao contrário do método de inicialização anterior (ou seja, usando a linha de comando).

## <a name="accepting-arguments"></a>Aceitando argumentos

Para aceitar argumentos passados na ativação do aplicativo UWP, você deve adicionar algum código ao aplicativo. Para determinar se ocorreu a ativação de protocolo ou a ativação de inicialização, substitua o evento **OnActivated** e verifique o tipo de argumento e, em seguida, obtenha a cadeia de caracteres bruta ou os valores pré-analisados do objeto Uri. 

Este exemplo mostra como obter a cadeia de caracteres bruta.

```
void OnActivated(IActivatedEventArgs^ args)
{
        // Check for launch activation
        if (args->Kind == ActivationKind::Launch)
        {
            auto launchArgs = static_cast<LaunchActivatedEventArgs^>(args); 
            Platform::String^ argval = launchArgs->Arguments;
            // Manipulate arguments …
        }

        // Check for protocol activation
        if (args->Kind == ActivationKind::Protocol)
        {
            auto protocolArgs = static_cast< ProtocolActivatedEventArgs^>(args);
            Platform::String^ argval = protocolArgs->Uri->ToString();
            // Manipulate arguments …
        }
}
```

## <a name="summary"></a>Resumo
Em resumo, você pode usar vários métodos para iniciar o aplicativo UWP. Dependendo dos requisitos e casos de uso, métodos diferentes podem ser mais adequados que outros. 

## <a name="see-also"></a>Consulte também
- [UWP no Xbox One](index.md)

