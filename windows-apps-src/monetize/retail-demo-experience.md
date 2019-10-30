---
title: Adicionar recursos de demonstração de varejo (RDX) ao seu aplicativo
description: Prepare seu aplicativo para o modo de demonstração de varejo, ajudando a demonstrar seu aplicativo no chão de vendas de varejo.
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, aplicativo de demonstração varejo
ms.localizationpriority: medium
ms.openlocfilehash: 5be39760ee2b8837cfb9b0809a354262e790970b
ms.sourcegitcommit: 5dfa98a80eee41d97880dba712673168070c4ec8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/29/2019
ms.locfileid: "73051997"
---
# <a name="add-retail-demo-rdx-features-to-your-app"></a>Adicionar recursos de demonstração de varejo (RDX) ao seu aplicativo

Inclua um modo de demonstração de varejo em seu aplicativo do Windows para que os clientes que experimentarem PCs e dispositivos na área de vendas possam começar imediatamente.

Quando os clientes estão em uma loja de varejo, eles esperam poder experimentar demonstrações de PCs e dispositivos. Em geral, eles gastam uma parte considerável do seu tempo experimentando aplicativos por meio da [experiência de demonstração de varejo (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience).

Você pode configurar seu aplicativo para fornecer experiências diferentes enquanto estiver nos modos _normal_ ou de _varejo_ . Por exemplo, se seu aplicativo começar com um processo de instalação, você poderá ignorá-lo no modo de varejo e preencher previamente o aplicativo com dados de exemplo e configurações padrão para que eles possam ir diretamente.

Da perspectiva dos nossos clientes, há apenas um aplicativo. Para ajudar os clientes a distinguir entre os dois modos, recomendamos que, enquanto seu aplicativo estiver no modo de varejo, ele mostre a palavra "varejo" em destaque na barra de título ou em um local adequado.

Além dos requisitos de Microsoft Store para aplicativos, os aplicativos com reconhecimento de RDX também devem ser compatíveis com os processos de instalação, limpeza e atualização do RDX para garantir que os clientes tenham uma experiência consistentemente positiva na loja de varejo.

## <a name="design-principles"></a>Princípios do design

* **Mostre o melhor**. Use a experiência de demonstração de varejo para demonstrar por que seu aplicativo Rocks. Isso é provavelmente a primeira vez que o cliente verá seu aplicativo, Então mostre a melhor parte!

* **Mostre isso rapidamente**. Os clientes podem ser impacientes - Quanto mais rápido um usuário puder experimentar o valor real do aplicativo, melhor.

* **Mantenha a história simples**. A experiência de demonstração de varejo é uma inclinação do elevador para o valor do seu aplicativo.

* **Concentre-se na experiência**. Dê ao usuário tempo para interpretar o conteúdo. Embora mostrar a eles a melhor parte seja importante, projetar pausas indicadas pode ajudá-los no aproveitamento máximo da experiência.

## <a name="technical-requirements"></a>Requisitos técnicos

Como os aplicativos com reconhecimento de RDX destinam-se a apresentar o melhor de seu aplicativo aos clientes de varejo, eles devem atender aos requisitos técnicos e aderir às leis de privacidade que o Microsoft Store tem para todos os aplicativos de experiência de demonstração de varejo.

Isso pode ser usado como uma lista de verificação para ajudá-lo a se preparar para o processo de validação e para fornecer clareza no processo de teste. Esses requisitos precisam ser mantidos, não apenas para o processo de validação, mas para todo o tempo de vida do aplicativo de experiência de demonstração de revenda; desde que o aplicativo continue em execução nos dispositivos de demonstração de revenda.

### <a name="critical-requirements"></a>Requisitos críticos

Aplicativos com reconhecimento de RDX que não atendem a esses requisitos críticos serão removidos de todos os dispositivos de demonstração de varejo assim que possível.

* **Não peça informações de identificação pessoal (PII)** . Isso inclui informações de logon, informações de conta Microsoft ou detalhes de contato.

* **Experiência sem erros**. O aplicativo deve ser executado sem erros. Além disso, nenhum pop-up de erro ou notificação deve ser mostrado para clientes que usem os dispositivos de demonstração de revenda. Os erros refletem negativamente no próprio aplicativo, sua marca, a marca do dispositivo, a marca manufacturer's do dispositivo e a marca da Microsoft.

* Os **aplicativos pagos devem ter um modo de avaliação**. Seu aplicativo precisa ser gratuito ou incluir um [modo de avaliação](https://docs.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app). Os clientes não esperam pagar por uma experiência em uma loja de revenda.

### <a name="high-priority-requirements"></a>Requisitos de alta prioridade

Aplicativos com reconhecimento de RDX que não atendem a esses requisitos de alta prioridade precisam ser investigados para uma correção imediatamente. Se nenhuma correção imediata for encontrada, esse aplicativo poderá ser removido de todos os dispositivos de demonstração de revenda.

* Tenha **memorizado a experiência offline**. Seu aplicativo precisa demonstrar uma ótima experiência offline, pois cerca de 50% dos dispositivos estão offline em locais de varejo. Isso é para garantir que os clientes que interajam com o aplicativo offline ainda sejam capazes de ter uma experiência significativa e positiva.

* **Experiência de conteúdo atualizada**. Seu aplicativo nunca deve ser solicitado a fazer atualizações quando estiver online. Se forem necessárias atualizações, elas deverão ser executadas silenciosamente.

* **Nenhuma comunicação anônima**. Como um cliente que usa um dispositivo de demonstração de varejo é um usuário anônimo, ele não deve ser capaz de receber mensagens ou compartilhar conteúdo do dispositivo.

* **Forneça experiências consistentes usando o processo de limpeza**. Todos os clientes devem ter a mesma experiência quando usarem um dispositivo de demonstração de revenda. Seu aplicativo deve usar o [processo de limpeza](#cleanup-process) para retornar ao mesmo estado padrão após cada uso. Não queremos que o próximo cliente Veja o que o último cliente deixou para trás. Isso inclui placares, conquistas e desbloqueios.

* **Conteúdo apropriado da idade**. Todo o conteúdo do aplicativo precisa ser atribuído a uma categoria adolescentes ou de classificação mais baixa. Para saber mais, Confira como [obter seu aplicativo classificado por](https://www.globalratings.com/for-developers.aspx) [classificações](https://www.esrb.org/ratings/ratings_guide.aspx)IARC e ESRB.

### <a name="medium-priority-requirements"></a>Requisitos de prioridade média

A equipe da Loja de revenda do Windows pode entrar em contato com desenvolvedores diretamente para marcar uma reunião sobre como corrigir esses problemas.

* **Capacidade de executar com êxito em uma variedade de dispositivos**. Os aplicativos devem executar bem em todos os dispositivos, incluindo dispositivos com especificações de low-end. Se o aplicativo estiver instalado em dispositivos que não atenderam às especificações mínimas, o aplicativo precisará informar claramente ao usuário sobre isso. Os requisitos de dispositivo mínimos devem ser conhecidos de maneira que o aplicativo possa ser sempre executado com alto desempenho.

* **Atenda aos requisitos de tamanho do aplicativo da loja de varejo**. O aplicativo deve ser menor do que 800 MB. Entre em contato com a equipe do Windows varejo Store diretamente para obter mais discussões se seu aplicativo com reconhecimento de RDX não atender aos requisitos de tamanho.

## <a name="retailinfo-api-preparing-your-code-for-demo-mode"></a>API RetailInfo: preparando seu código para o modo de demonstração

### <a name="isdemomodeenabled"></a>IsDemoModeEnabled
A propriedade [**IsDemoModeEnabled**](https://docs.microsoft.com/uwp/api/windows.system.profile.retailinfo.isdemomodeenabled) na classe de utilitário [**RetailInfo**](https://docs.microsoft.com/uwp/api/Windows.System.Profile.RetailInfo) , que faz parte do namespace [Windows. System. Profile](https://docs.microsoft.com/uwp/api/windows.system.profile) no SDK do Windows 10, é usada como um indicador booliano para especificar o caminho de código em que seu aplicativo é executado-o _normal_ modo ou o modo de _varejo_ .

``` csharp
using Windows.Storage;

StorageFolder folder = ApplicationData.Current.LocalFolder;

if (Windows.System.Profile.RetailInfo.IsDemoModeEnabled) 
{
    // Use the demo specific directory
    folder = await folder.GetFolderAsync("demo");
}

StorageFile file = await folder.GetFileAsync("hello.txt");
// Now read from file
```

``` cpp
using namespace Windows::Storage;

StorageFolder^ localFolder = ApplicationData::Current->LocalFolder;

if (Windows::System::Profile::RetailInfo::IsDemoModeEnabled) 
{
    // Use the demo specific directory
    create_task(localFolder->GetFolderAsync("demo").then([this](StorageFolder^ demoFolder)
    {
        return demoFolder->GetFileAsync("hello.txt");
    }).then([this](task<StorageFile^> fileTask)
    {
        StorageFile^ file = fileTask.get();
    });
    // Do something with file
}
else
{
    create_task(localFolder->GetFileAsync("hello.txt").then([this](StorageFile^ file)
    {
        // Do something with file
    });
}
```

``` javascript
if (Windows.System.Profile.retailInfo.isDemoModeEnabled) {
    console.log("Retail mode is enabled.");
} else {
    Console.log("Retail mode is not enabled.");
}
```

### <a name="retailinfoproperties"></a>RetailInfo. Properties

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

#### <a name="idl"></a>INSERI

```cpp
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

A limpeza começa dois minutos depois que um comprador para de interagir com o dispositivo. A demonstração de varejo é reproduzida e o Windows começa a redefinir os dados de exemplo nos contatos, Fotos e outros aplicativos. Dependendo do dispositivo, isso pode levar entre 1-5 minutos para redefinir completamente tudo de volta para normal. Isso garante que todos os clientes na loja de varejo possam ir até um dispositivo e ter a mesma experiência ao interagir com o dispositivo.

Etapa 1: limpar
* Todos os aplicativos Win32 e da Loja são fechados
* Todos os arquivos em pastas conhecidas como __Imagens__, __Vídeos__, __Música__, __Documentos__, __SavedPictures__, __CameraRoll__, __Área de Trabalho__ e __Downloads__ são excluídos
* Estados de roaming não estruturados e estruturados são excluídos
* Estados locais estruturados são excluídos

Etapa 2: instalação
* Para dispositivos offline: as pastas permanecem vazias
* Para dispositivos online: os ativos de demonstração de varejo podem ser enviados para o dispositivo pela Microsoft Store

### <a name="store-data-across-user-sessions"></a>Armazenar dados entre sessões de usuário

Para armazenar dados em sessões de usuário, você pode armazenar informações em __applicationData. Current. TemporaryFolder__ , pois o processo de limpeza padrão não exclui automaticamente os dados nessa pasta. Observe que as informações armazenadas usando o *localstate* são excluídas durante o processo de limpeza.

### <a name="customize-the-cleanup-process"></a>Personalizar o processo de limpeza

Para personalizar o processo de limpeza, implemente o serviço de aplicativo `Microsoft-RetailDemo-Cleanup` em seu aplicativo.

Os cenários em que uma lógica de limpeza personalizada é necessária incluem a execução de uma configuração extensiva, o download e o armazenamento em cache de dados, ou não a exclusão de dados *localstate* a serem excluídos.

Etapa 1: declare o serviço _Microsoft-RetailDemo-Cleanup_ no manifesto do aplicativo.
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

Etapa 2: implemente a lógica de limpeza personalizada na função de caso _AppdataCleanup_ usando o modelo de exemplo abaixo.
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

* [Armazenar e recuperar dados de aplicativo](https://docs.microsoft.com/windows/uwp/app-settings/store-and-retrieve-app-data)
* [Como criar e consumir um serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)
* [Localizando conteúdo do aplicativo](https://docs.microsoft.com/windows/uwp/globalizing/globalizing-portal)
* [Experiência de demonstração de varejo (RDX)](https://docs.microsoft.com/windows-hardware/customize/desktop/retail-demo-experience)
