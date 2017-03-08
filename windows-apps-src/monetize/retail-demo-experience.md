---
author: joannaleecy
title: "Criar um aplicativo de experiência de demonstração de revenda"
description: "Crie um aplicativo de experiência de demonstração de varejo (RDX), que é um aplicativo único capaz de ser iniciado nos modos de demonstração de revenda e normal"
ms.assetid: f83f950f-7fdd-4f18-8127-b92a8f400061
ms.author: joanlee
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, aplicativo de demonstração varejo"
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 843f98782410559d47bdb8dc0b23b50fa96552d6
ms.lasthandoff: 02/07/2017

---
#  <a name="create-a-retail-demo-experience-rdx-app"></a>Crie um aplicativo de experiência de demonstração de varejo (RDX)

Quando os clientes entram em uma loja de varejo ou localização, os clientes esperam encontrar os computadores e celulares mais recentes na vitrine, e esses dispositivos na vitrine são conhecidos como dispositivos de demonstração de varejo.
Os dispositivos de demonstração de revenda e o conteúdo instalado neles são amplamente responsáveis pela experiência do cliente nas lojas porque os clientes normalmente passam uma parte considerável do tempo experimentando esses dispositivos.

Os aplicativos instalados nesses computadores e celulares em lojas de revenda devem ser aplicativos de experiência de demonstração de revenda (RDX). Este artigo apresenta uma visão geral de como projetar e desenvolver uma versão de demonstração de revenda de um aplicativo a ser instalado em computadores e dispositivos de demonstração móveis em uma loja de revenda.

Um aplicativo de experiência de demonstração de revenda acompanha um build único que pode ser iniciado em um dos dois modos diferentes _normal_ ou _revenda_.
Do ponto de vista dos clientes, existe apenas um aplicativo e, para ajudar os clientes na distinção entre as duas versões, é recomendável para o aplicativo em execução no modo de revenda exibir as palavras "Retail" em destaque na barra de título ou em um local indicado.

Além dos requisitos da Loja para aplicativos, aplicativos RDX também devem ser totalmente compatíveis com o sistema de configuração, limpeza e atualização dos dispositivos de demonstração de revenda para garantir que os clientes tenham uma experiência consistentemente positiva na loja de revenda.

## <a name="design-principles"></a>Princípios de design

### <a name="show-your-best"></a>Mostre o melhor

Use a experiência de demonstração de revenda para demonstrar por que o aplicativo é demais.  Trata-se, provavelmente, da primeira vez em que o cliente verá o aplicativo, logo, mostre a ele a melhor parte!

### <a name="show-it-fast"></a>Mostre-a rapidamente

Os clientes podem ser impacientes - Quanto mais rápido um usuário puder experimentar o valor real do aplicativo, melhor.

### <a name="keep-the-story-simple"></a>Mantenha a história simples

Lembre-se de que a experiência de demonstração de revenda eleva o valor agregado do aplicativo.

### <a name="focus-on-the-experience"></a>Concentre-se na experiência

Dê ao usuário tempo para interpretar o conteúdo.  Embora mostrar a eles a melhor parte seja importante, projetar pausas indicadas pode ajudá-los no aproveitamento máximo da experiência.

## <a name="technical-requirements"></a>Requisitos técnicos

Como se destinam a demostrar o melhor do aplicativo para clientes de revenda, é essencial que aplicativos de experiência de demonstração de revenda atendam a esses requisitos técnicos e respeitem regulamentações de privacidade que a Loja tenha para todos os aplicativos de experiência de demonstração de revenda.
Isso também pode ser usado como uma lista de verificação para ajudar você a se preparar para o processo de validação e esclarecimento no processo de teste. Esses requisitos precisam ser mantidos, não apenas para o processo de validação, mas para todo o tempo de vida do aplicativo de experiência de demonstração de revenda; desde que o aplicativo continue em execução nos dispositivos de demonstração de revenda.

### <a name="critical-level-requirements"></a>Requisitos de nível crítico

Os aplicativos RDX que não atenderem a esses requisitos críticos serão removidos de todos os dispositivos de demonstração de revenda assim que possível.

* Sem solicitação de informações de identificação pessoal (PII)

    O aplicativo não tem permissão para solicitar aos clientes qualquer informação de identificação pessoal.  Isso inclui todas as informações de conta da Microsoft, detalhes de contato etc.

* Experiência livre de erros

    O aplicativo deve ser executado sem erros. Além disso, nenhum pop-up de erro ou notificação deve ser mostrado para clientes que usem os dispositivos de demonstração de revenda. Isso é importante porque queremos demostrar o melhor para os clientes, e o melhor deve estar livre de erros.
    Outro motivo é que erros se refletem negativamente no aplicativo, na marca, no dispositivo no qual o aplicativo está em execução, na marca do fabricante do dispositivo e também na marca da Microsoft.

* Os aplicativos pagos devem ter um modo de avaliação

    Para os aplicativos serem instalado em dispositivos de demonstração de revenda, o aplicativo precisa ser um aplicativo gratuito ou ter um modo de avaliação estabelecido.  Os clientes não esperam pagar por uma experiência em uma loja de revenda. Para obter mais informações, consulte [Excluir ou limitar recursos em uma versão de avaliação](https://msdn.microsoft.com/windows/uwp/monetize/exclude-or-limit-features-in-a-trial-version-of-your-app)

### <a name="high-priority-requirements"></a>Requisitos de alta prioridade

Os aplicativos RDX que não atendam a esses requisitos de alta prioridade precisam ser investigados em busca de uma correção imediata. Se nenhuma correção imediata for encontrada, esse aplicativo poderá ser removido de todos os dispositivos de demonstração de revenda.

* Experiência offline memorável

    O aplicativo de experiência de demonstração de revenda precisa apresentar uma excelente experiência offline porque cerca de 50% dos dispositivos permanecem offline em locais de revenda. Isso é para garantir que os clientes que interajam com o aplicativo offline ainda sejam capazes de ter uma experiência significativa e positiva.

* Experiência de conteúdo atualizado

    Para proporcionar uma ótima experiência, o aplicativo precisa estar sempre atualizado e os clientes jamais devem ser solicitados a fazerem atualizações de aplicativos quando o aplicativo estiver online.

* Nenhuma comunicação anônima

    Como um cliente usando um dispositivo de demonstração de revenda é um usuário anônimo, ele não deve ser capaz de enviar mensagens nem de compartilhar conteúdo usando o dispositivo.

* Proporcione uma experiência consistente utilizando o processo de limpeza

    Todos os clientes devem ter a mesma experiência quando usarem um dispositivo de demonstração de revenda. O aplicativo deve utilizar o [processo de limpeza](#clean-up-process) para retornar ao mesmo estado padrão depois de cada uso porque não queremos que o próximo cliente veja o que o cliente mais recente deixou.  Isso inclui placares, conquistas e desbloqueios.

* Conteúdo apropriado à idade

    Todo o conteúdo do aplicativo de experiência de demonstração de revenda deve receber uma categoria de classificação para adolescente ou inferior. Para obter mais informações, consulte [Classificação do aplicativo pelo IARC](https://www.globalratings.com/for-developers.aspx) e [Classificações ESRB](https://www.esrb.org/ratings/ratings_guide.aspx).

### <a name="medium-priority-requirements"></a>Requisitos de prioridade média

A equipe da Loja de revenda do Windows pode entrar em contato com desenvolvedores diretamente para marcar uma reunião sobre como corrigir esses problemas.

* Capacidade de executar com êxito usando uma grande variedade de dispositivos

    Os aplicativos de experiência de demonstração de revenda devem ser bem executados em todos os dispositivos, inclusive em dispositivos com especificações de baixo nível. Se o aplicativo de experiência de demonstração de revenda for instalado em dispositivos que não atendam às especificações mínimas para executar o aplicativo, o aplicativo precisará informar claramente o usuário sobre isso. Os requisitos de dispositivo mínimos devem ser conhecidos de maneira que o aplicativo possa ser sempre executado com alto desempenho.

* Atenda aos requisitos de tamanho do aplicativo de loja de revenda

    O aplicativo deve ser menor do que 800 MB. Entre em contato com a equipe da Loja de revenda do Windows diretamente para saber mais se o aplicativo de experiência de demonstração de revenda não atende aos requisitos de tamanho.

## <a name="preparing-codebase-for-retail-demo-mode-development"></a>Preparação da base de código para o desenvolvimento do modo de demonstração de revenda

A propriedade [**IsDemoModeEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.isdemomodeenabled.aspx) na classe de utilitário [**RetailInfo**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.aspx), que faz parte do namespace [Windows](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.aspx) no SDK do Windows 10, é usada como um indicador Booliano para especificar em qual caminho de código o aplicativo é executado – o modo _normal_ ou o modo _revenda_.

Quando [**RetailInfo.IsDemoModeEnabled**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.isdemomodeenabled.aspx) retorna verdadeiro, é possível consultar um conjunto de propriedades sobre o dispositivo usando [**RetailInfo.Properties**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.retailinfo.properties.aspx) para criar uma experiência de demonstração de revenda mais personalizada. Entre essas propriedades estão [**ManufacturerName**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.manufacturername.aspx), [**Screensize**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.screensize.aspx), [**Memory**](https://msdn.microsoft.com/library/windows/apps/windows.system.profile.knownretailinfoproperties.memory.aspx) etc.


## <a name="clean-up-process"></a>Processo de limpeza

O processo de limpeza é usado para redefinir automaticamente as configurações padrão originais dos dispositivos de demonstração de revenda quando não há interação com o dispositivo por uma duração fixa. Isso é para garantir que todos os usuários na loja de varejo possam ir até a um dispositivo e ter a experiência desejada padrão exata na interação com o dispositivo. Durante o desenvolvimento de um aplicativo de experiência de demonstração de revenda, é importante entender quando e como o processo de limpeza é disparado, o que acontece durante o processo de limpeza padrão e aprender a personalizar esse processo de limpeza de acordo com os requisitos da experiência de demonstração de revenda desejada.

### <a name="when-does-clean-up-begin"></a>Quando a limpeza começa?

A sequência de limpeza começará depois de um determinado período de ociosidade do dispositivo. O tempo ocioso começa a contar quando não há entrada de toque, mouse e teclado no dispositivo.

#### <a name="desktoppc"></a>Área de trabalho/PC

Depois de 120 segundos de tempo ocioso, o vídeo do aplicativo de tempo ocioso começará a ser reproduzido no dispositivo. Depois de 5 segundos, o processo de limpeza será iniciado.

#### <a name="phone"></a>Telefone

Depois de 60 segundos de tempo ocioso, o vídeo do aplicativo de tempo ocioso começará a ser reproduzido no dispositivo, e o processo de limpeza será iniciado imediatamente.

### <a name="what-happens-during-a-default-clean-up-process"></a>O que acontece durante um processo de limpeza padrão?

#### <a name="step-1-clean-up"></a>Etapa 1: limpar
* Todos os aplicativos Win32 e da Loja são fechados
* Todos os arquivos em pastas conhecidas como __Imagens__, __Vídeos__, __Música__, __Documentos__, __SavedPictures__, __CameraRoll__, __Área de Trabalho__ e __Downloads__ são excluídos
* Estados de roaming não estruturados e estruturados são excluídos
* Estados locais estruturados são excluídos

#### <a name="step-2-set-up"></a>Etapa 2: Configurar
* Para dispositivos offline: as pastas permanecem vazias
* Para dispositivos online: os ativos de demonstração de revenda podem ser enviados para o dispositivo pela Windows Store

### <a name="how-to-store-data-across-user-sessions"></a>Como armazenar dados entre sessões de usuário?

Se quiser armazenar dados entre sessões de usuário, você poderá armazenar informações em __ApplicationData.Current.TemporaryFolder__ porque o processo de limpeza padrão não exclui automaticamente dados nessa pasta. As informações armazenadas usando-se *LocalState* são excluídas durante o processo de limpeza.

### <a name="how-to-customize-the-clean-up-process"></a>Como personalizar o processo de limpeza?

Se quiser personalizar o processo de limpeza, você precisará implementar o serviço de aplicativo `Microsoft-RetailDemo-Cleanup` no aplicativo.

Entre os cenários onde uma lógica de limpeza personalizada é necessária estão a execução de uma instalação cara, o download e o armazenamento em cache de dados ou a falta de intenção de que os dados de *LocalState* sejam excluídos.

Etapa 1: Declarar o serviço _Microsoft-RetailDemo-Cleanup_ no manifesto do aplicativo.
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

Etapa 2: Implementar a lógica de limpeza personalizada na função case _AppdataCleanup_ usando o modelo de exemplo abaixo.
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


 

 

