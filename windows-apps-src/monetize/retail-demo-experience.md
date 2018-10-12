---
author: joannaleecy
title: Adicionar recursos de demonstração (RDX) de varejo ao seu aplicativo
description: Prepare seu aplicativo para o modo de demonstração de varejo, ajudando demonstrar seu aplicativo no chão de vendas de varejo.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, aplicativo de demonstração varejo
ms.localizationpriority: medium
ms.openlocfilehash: 152c775c1b69bfd82d8969aed7e638f98646bdd7
ms.sourcegitcommit: d10fb9eb5f75f2d10e1c543a177402b50fe4019e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/12/2018
ms.locfileid: "4563803"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Adicionar recursos de demonstração (RDX) de varejo ao seu aplicativo

Inclua um modo de demonstração de varejo no seu aplicativo do Windows para que clientes que experimentar computadores e dispositivos no chão de vendas podem ir para a direita em.

Quando os clientes estão em uma loja de varejo, eles esperam poder experimentar demonstrações de computadores e dispositivos. Eles normalmente passam uma parte considerável do tempo experimentando aplicativos por meio da [experiência de demonstração de revenda (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Você pode configurar seu aplicativo para fornecer experiências diferentes enquanto nos modos _normal_ ou de _varejo_ . Por exemplo, se o aplicativo é iniciado com um processo de configuração, você pode ignorar no modo de varejo e preencher o aplicativo com as configurações de dados e o padrão de exemplo para que eles podem ir para a direita em.

Da perspectiva de nossos clientes, há apenas um aplicativo. Para ajudar os clientes a distinção entre os dois modos, é recomendável que enquanto seu aplicativo está no modo de varejo, ele mostra a palavra "Retail" com destaque na barra de título ou em um local adequado.

Além dos requisitos de Microsoft Store para aplicativos, aplicativos com reconhecimento RDX também devem ser compatíveis com a instalação RDX, limpeza e processos de atualização para garantir que os clientes tenham uma experiência consistentemente positiva na loja de varejo.

## <a name="design-principles"></a>Princípios do design

* **Mostre o melhor**. Use a experiência de demonstração de revenda para demonstrar por que seu aplicativo é demais. É provável que isso na primeira vez que seu cliente verá seu aplicativo, logo, mostre a melhor parte!

* **Mostre-rápido**. Os clientes podem ser impacientes - Quanto mais rápido um usuário puder experimentar o valor real do aplicativo, melhor.

* **Mantenha a história simples**. A experiência de demonstração de revenda é eleva o valor de seu aplicativo.

* **Foco na experiência**. Dê ao usuário tempo para interpretar o conteúdo. Embora mostrar a eles a melhor parte seja importante, projetar pausas indicadas pode ajudá-los no aproveitamento máximo da experiência.

## <a name="technical-requirements"></a>Requisitos técnicos

Como aplicativos com reconhecimento RDX se destinam a demostrar o melhor do seu aplicativo para clientes comerciais, eles devem atender aos requisitos técnicos e respeitem regulamentações de privacidade que a Microsoft Store tem para todos os aplicativos de experiência de demonstração de varejo.

Isso pode ser usado como uma lista de verificação para ajudar você a se preparar para o processo de validação e esclarecimento no processo de teste. Esses requisitos precisam ser mantidos, não apenas para o processo de validação, mas para todo o tempo de vida do aplicativo de experiência de demonstração de revenda; desde que o aplicativo continue em execução nos dispositivos de demonstração de revenda.

### <a name="critical-requirements"></a>Requisitos críticos

Aplicativos com reconhecimento RDX que não atendam a esses requisitos críticos serão removidos de todos os dispositivos de demonstração de revenda assim que possível.

* **Não solicitar informações de identificação pessoais (PII)**. Isso inclui informações de logon, informações de conta da Microsoft ou contato detalhes.

* **Experiência livre de erros**. O aplicativo deve ser executado sem erros. Além disso, nenhum pop-up de erro ou notificação deve ser mostrado para clientes que usem os dispositivos de demonstração de revenda. Erros se refletem negativamente no aplicativo em si, sua marca, marca do dispositivo, marca do fabricante do dispositivo e marca da Microsoft.

* **Os aplicativos pago devem ter um modo de avaliação**. O aplicativo precisa ser um gratuito ou incluir um [modo de avaliação](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Os clientes não esperam pagar por uma experiência em uma loja de revenda.

### <a name="high-priority-requirements"></a>Requisitos de alta prioridade

Aplicativos com reconhecimento RDX que não atendam a esses requisitos de alta prioridade precisam ser investigados em busca de uma correção imediata. Se nenhuma correção imediata for encontrada, esse aplicativo poderá ser removido de todos os dispositivos de demonstração de revenda.

* **Memorable experiência offline**. Seu aplicativo precisa apresentar uma excelente experiência offline porque cerca de 50% dos dispositivos permanecem offline em locais de revenda. Isso é para garantir que os clientes que interajam com o aplicativo offline ainda sejam capazes de ter uma experiência significativa e positiva.

* **Experiência de conteúdo atualizado**. Seu aplicativo nunca deve ser solicitar atualizações on-line. Se as atualizações forem necessárias, elas devem ser executadas silenciosamente.

* **Nenhuma comunicação anônima**. Como um cliente usando um dispositivo de demonstração de revenda é um usuário anônimo, ele não devem ser capaz de mensagem ou o compartilhamento de conteúdo do dispositivo.

* **Fornecer experiências consistentes, usando o processo de limpeza**. Todos os clientes devem ter a mesma experiência quando usarem um dispositivo de demonstração de revenda. Seu aplicativo deve usar o [processo de limpeza](#clean-up-process) para retornar ao mesmo estado padrão depois de cada uso. Não queremos que o próximo cliente veja o que o cliente para trás. Isso inclui placares, conquistas e desbloqueios.

* **Conteúdo apropriado à idade**. Todo o conteúdo do aplicativo precisa ser um adolescente ou inferior categoria de classificação. Para saber mais, consulte [obter classificados como seu aplicativo pelo IARC](https://www.globalratings.com/for-developers.aspx) e [classificações ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Requisitos de prioridade média

A equipe da Loja de revenda do Windows pode entrar em contato com desenvolvedores diretamente para marcar uma reunião sobre como corrigir esses problemas.

* **Capacidade de executar com êxito usando uma grande variedade de dispositivos**. Aplicativos devem ser bem executados em todos os dispositivos, incluindo dispositivos com especificações de baixo nível. Se o aplicativo é instalado em dispositivos que não atendam às especificações mínimas, o aplicativo precisará informar claramente o usuário sobre isso. Os requisitos de dispositivo mínimos devem ser conhecidos de maneira que o aplicativo possa ser sempre executado com alto desempenho.

* **Atender aos requisitos de tamanho do aplicativo de loja de varejo**. O aplicativo deve ser menor do que 800 MB. Contate a equipe da loja de varejo do Windows diretamente para saber mais se seu aplicativo com reconhecimento de RDX não atende aos requisitos de tamanho.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>RetailInfo API: Preparar seu código para o modo de demonstração

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
A propriedade [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) na classe de utilitário [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) , que faz parte do namespace [Windows](https://docs.microsoft.com/uwp/api/windows.system.profile) no SDK do Windows 10, é usada como um indicador booliano para especificar em qual caminho de código que seu aplicativo é executado – o normal _ _modo ou o modo de _varejo_ .

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync(“demo”);
}

StorageFile file = await folder.GetFileAsync(“hello.txt”);
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync(“demo”).then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync(“hello.txt”);
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync(“hello.txt”).then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log(“Retail mode is enabled.”);
} else {
    Console.log(“Retail mode is not enabled.”);
}
```

### <a name="retailinfoproperties"></a>Retailinfo. Properties

Quando [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) retorna verdadeiro, é possível consultar um conjunto de propriedades sobre o dispositivo usando [**RetailInfo.Properties**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.properties) para criar uma experiência de demonstração de varejo mais personalizada. Entre essas propriedades estão [**ManufacturerName**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.manufacturername), [**Screensize**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.screensize), [**Memory**](https://docs.microsoft.com/uwp/api/windows.system.profile.knownretailinfoproperties.memory) etc.

```csharp
using Windows.UI.Xaml.Controls;
using Windows.System.Profile

TextBlock priceText = new TextBlock();
priceText.Text = RetailInfo.Properties[KnownRetailInfo.Price];
// Assume infoPanel is a StackPanel declared in XAML
this.infoPanel.Children.Add(priceText);
```

```cpp
using namespace Windows::UI::Xaml::Controls;
using namespace Windows::System::Profile;

TextBlock ^manufacturerText = ref new TextBlock();
manufacturerText.set_Text(RetailInfo::Properties[KnownRetailInfoProperties::Price]);
// Assume infoPanel is a StackPanel declared in XAML
this->infoPanel->Children->Add(manufacturerText);
```

```javascript
var pro = Windows.System.Profile;
console.log(pro.retailInfo.properties[pro.KnownRetailInfoProperties.price);
```

#### <a name="idl"></a>IDL

```
//  Copyright (c) Microsoft Corporation. All rights reserved.
//
//  WindowsRuntimeAPISet

import "oaidl.idl";
import "inspectable.idl";
import "Windows.Foundation.idl";
#include <sdkddkver.h>

namespace Windows.System.Profile
{
    runtimeclass RetailInfo;
    runtimeclass KnownRetailInfoProperties;

    [version(NTDDI_WINTHRESHOLD), uuid(0712C6B8-8B92-4F2A-8499-031F1798D6EF), exclusiveto(RetailInfo)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IRetailInfoStatics : IInspectable
    {
        [propget] HRESULT IsDemoModeEnabled([out, retval] boolean *value);
        [propget] HRESULT Properties([out, retval, hasvariant] Windows.Foundation.Collections.IMapView<HSTRING, IInspectable *> **value);
    }

    [version(NTDDI_WINTHRESHOLD), uuid(50BA207B-33C4-4A5C-AD8A-CD39F0A9C2E9), exclusiveto(KnownRetailInfoProperties)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    interface IKnownRetailInfoPropertiesStatics : IInspectable
    {
        [propget] HRESULT RetailAccessCode([out, retval] HSTRING *value);
        [propget] HRESULT ManufacturerName([out, retval] HSTRING *value);
        [propget] HRESULT ModelName([out, retval] HSTRING *value);
        [propget] HRESULT DisplayModelName([out, retval] HSTRING *value);
        [propget] HRESULT Price([out, retval] HSTRING *value);
        [propget] HRESULT IsFeatured([out, retval] HSTRING *value);
        [propget] HRESULT FormFactor([out, retval] HSTRING *value);
        [propget] HRESULT ScreenSize([out, retval] HSTRING *value);
        [propget] HRESULT Weight([out, retval] HSTRING *value);
        [propget] HRESULT DisplayDescription([out, retval] HSTRING *value);
        [propget] HRESULT BatteryLifeDescription([out, retval] HSTRING *value);
        [propget] HRESULT ProcessorDescription([out, retval] HSTRING *value);
        [propget] HRESULT Memory([out, retval] HSTRING *value);
        [propget] HRESULT StorageDescription([out, retval] HSTRING *value);
        [propget] HRESULT GraphicsDescription([out, retval] HSTRING *value);
        [propget] HRESULT FrontCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT RearCameraDescription([out, retval] HSTRING *value);
        [propget] HRESULT HasNfc([out, retval] HSTRING *value);
        [propget] HRESULT HasSdSlot([out, retval] HSTRING *value);
        [propget] HRESULT HasOpticalDrive([out, retval] HSTRING *value);
        [propget] HRESULT IsOfficeInstalled([out, retval] HSTRING *value);
        [propget] HRESULT WindowsVersion([out, retval] HSTRING *value);
    }

    [version(NTDDI_WINTHRESHOLD), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IRetailInfoStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass RetailInfo
    {
    }

    [version(NTDDI_WINTHRESHOLD), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD)]
    [version(NTDDI_WINTHRESHOLD, Platform.WindowsPhone), static(IKnownRetailInfoPropertiesStatics, NTDDI_WINTHRESHOLD, Platform.WindowsPhone)]
    [threading(both)]
    [marshaling_behavior(agile)]
    runtimeclass KnownRetailInfoProperties
    {
    }
}
```

## <a name="cleanup-process"></a>Processo de limpeza

Limpeza começa dois minutos depois que um comprador interrompe a interação com o dispositivo. Reproduz a demonstração de varejo, e o Windows começa redefinir os dados de exemplo nos contatos, fotos e outros aplicativos. Dependendo do dispositivo, isso pode levar entre 1 a 5 minutos para totalmente Redefinir tudo volta ao normal. Isso garante que todos os clientes na loja de varejo podem ir até a um dispositivo e ter a mesma experiência ao interagir com o dispositivo.

Etapa 1: limpeza
* Todos os aplicativos Win32 e da Loja são fechados
* Todos os arquivos em pastas conhecidas como __Imagens__, __Vídeos__, __Música__, __Documentos__, __SavedPictures__, __CameraRoll__, __Área de Trabalho__ e __Downloads__ são excluídos
* Estados de roaming não estruturados e estruturados são excluídos
* Estados locais estruturados são excluídos

Etapa 2: instalação
* Para dispositivos offline: as pastas permanecem vazias
* Para dispositivos online: os ativos de demonstração de varejo podem ser enviados para o dispositivo pela Microsoft Store

### <a name="store-data-across-user-sessions"></a>Armazenar dados entre sessões de usuário

Para armazenar dados entre sessões de usuário, você pode armazenar informações em __Temporaryfolder__ como o processo de limpeza padrão não exclui automaticamente dados nessa pasta. Observe que as informações armazenadas usando *LocalState* são excluídas durante o processo de limpeza.

### <a name="customize-the-cleanup-process"></a>Personalizar o processo de limpeza

Para personalizar o processo de limpeza, implemente o `Microsoft-RetailDemo-Cleanup` serviço de aplicativo em seu aplicativo.

Cenários onde uma lógica de limpeza personalizada é necessária estão a execução de uma instalação ampla, o download e armazenamento em cache dados ou não que desejam *LocalState* dados a ser excluído.

Etapa 1: Declare o serviço _Microsoft-RetailDemo-Cleanup_ no manifesto do aplicativo.
``` CSharp
  <Applications>
      <Extensions>
        <uap:Extension Category="windows.appService" EntryPoint="MyCompany.MyApp.RDXCustomCleanupTask">
          <uap:AppService Name="Microsoft-RetailDemo-Cleanup" />
        </uap:Extension>
      </Extensions>
   </Application>
  </Applications>

```

Etapa 2: Implemente a lógica de limpeza personalizada sob a função de caso _AppdataCleanup_ usando o modelo de exemplo abaixo.
``` CSharp
using System;
using System.IO;
using System.Runtime.Serialization.Json;
using System.Threading;
using System.Threading.Tasks;
using Windows.ApplicationModel.AppService;
using Windows.ApplicationModel.Background;
using Windows.Foundation.Collections;
using Windows.Storage;

namespace MyCompany.MyApp
{
    public sealed class RDXCustomCleanupTask : IBackgroundTask
    {
        BackgroundTaskCancellationReason _cancelReason = BackgroundTaskCancellationReason.Abort;
        BackgroundTaskDeferral _deferral = null;
        IBackgroundTaskInstance _taskInstance = null;
        AppServiceConnection _appServiceConnection = null;

        const string MessageCommand = "Command";

        public void Run(IBackgroundTaskInstance taskInstance)
        {
            // Get the deferral object from the task instance, and take a reference to the taskInstance;
            _deferral = taskInstance.GetDeferral();
            _taskInstance = taskInstance;
            _taskInstance.Canceled += new BackgroundTaskCanceledEventHandler(OnCanceled);

            AppServiceTriggerDetails appService = _taskInstance.TriggerDetails as AppServiceTriggerDetails;
            if ((appService != null) && (appService.Name == "Microsoft-RetailDemo-Cleanup"))
            {
                _appServiceConnection = appService.AppServiceConnection;
                _appServiceConnection.RequestReceived += _appServiceConnection_RequestReceived;
                _appServiceConnection.ServiceClosed += _appServiceConnection_ServiceClosed;
            }
            else
            {
                _deferral.Complete();
            }
        }

        void _appServiceConnection_ServiceClosed(AppServiceConnection sender, AppServiceClosedEventArgs args)
        {
        }

        async void _appServiceConnection_RequestReceived(AppServiceConnection sender, AppServiceRequestReceivedEventArgs args)
        {
            //Get a deferral because we will be calling async code
            AppServiceDeferral requestDeferral = args.GetDeferral();
            string command = null;
            var returnData = new ValueSet();

            try
            {
                ValueSet message = args.Request.Message;
                if (message.ContainsKey(MessageCommand))
                {
                    command = message[MessageCommand] as string;
                }

                if (command != null)
                {
                    switch (command)
                    {
                        case "AppdataCleanup":
                            {
                                // Do custom clean up logic here
                                break;
                            }
                    }
                }
            }
            catch (Exception e)
            {
            }
            finally
            {
                requestDeferral.Complete();
                // Also release the task deferral since we only process one request per instance.
                _deferral.Complete();
            }
        }

        private void OnCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _cancelReason = reason;
        }
    }
}
```

## <a name="related-links"></a>Links relacionados

* [Armazene e recupere dados de aplicativo](https://msdn.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Como criar e consumir um serviço de aplicativo](https://msdn.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localização do conteúdo do aplicativo](https://msdn.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Experiência de demonstração de revenda (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
