---
author: TylerMSFT
title: Atualizar um bloco dinâmico de uma tarefa em segundo plano
description: Use uma tarefa em segundo plano para atualizar o bloco dinâmico de seu aplicativo com novo conteúdo.
Search.SourceType: Video
ms.assetid: 9237A5BD-F9DE-4B8C-B689-601201BA8B9A
ms.author: twhitney
ms.date: 01/11/2018
ms.topic: article
keywords: Windows 10, uwp, tarefa em segundo plano
ms.localizationpriority: medium
ms.openlocfilehash: 3c379097efaef65357bc1c6b036695ef84671ea6
ms.sourcegitcommit: 70ab58b88d248de2332096b20dbd6a4643d137a4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/01/2018
ms.locfileid: "5929157"
---
# <a name="update-a-live-tile-from-a-background-task"></a>Atualizar um bloco dinâmico de uma tarefa em segundo plano

**APIs Importantes**

-   [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794)
-   [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)

Use uma tarefa em segundo plano para atualizar o bloco dinâmico de seu aplicativo com novo conteúdo.

Veja um vídeo que mostra como adicionar blocos dinâmicos aos aplicativos.

<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Updating-a-live-tile-from-a-background-task/player" width="720" height="405" allowFullScreen="true" frameBorder="0"></iframe>

## <a name="create-the-background-task-project"></a>Criar o projeto de tarefa em segundo plano  

Para habilitar um bloco dinâmico para seu aplicativo, adicione um novo projeto Componente do Tempo de Execução do Windows à sua solução. Trata-se de um assembly separado, que o SO carrega e executa em segundo plano quando um usuário instala o aplicativo.

1.  No Gerenciador de Soluções, clique com o botão direito do mouse na solução, aponte para **Adicionar** e, em seguida, clique em **Novo Projeto**.
2.  Na caixa de diálogo **Adicionar Novo Projeto**, selecione o modelo **Componente do Tempo de Execução do Windows** na seção **Instalado &gt; Outros idiomas &gt; Visual C# &gt; Windows Universal**.
3.  Chame o projeto de BackgroundTasks e clique ou toque em **OK**. O Microsoft Visual Studio adiciona o novo projeto à solução.
4.  No projeto principal, adicione uma referência ao projeto BackgroundTasks.

## <a name="implement-the-background-task"></a>Implementar a tarefa em segundo plano


Implemente a interface de [**IBackgroundTask**](https://msdn.microsoft.com/library/windows/apps/br224794) para criar uma classe que atualize o bloco dinâmico de seu aplicativo. A operação em segundo plano ocorre no método Run. Neste caso, a tarefa obtém um feed de sindicalização para os blogs do MSDN. Para impedir que a tarefa feche antecipadamente enquanto o código assíncrono ainda está em execução, obtenha um adiamento.

1.  No Gerenciador de Soluções, renomeie o arquivo gerado automaticamente, Class1.cs, para BlogFeedBackgroundTask.cs.
2.  Em BlogFeedBackgroundTask.cs, substitua o código gerado automaticamente pelo código do stub para a classe **BlogFeedBackgroundTask**.
3.  Na implementação do método Run, adicione o código dos métodos **GetMSDNBlogFeed** e **UpdateTile**.

```cs
using System;
using System.Collections.Generic;
using System.Diagnostics;
using System.Linq;
using System.Text;
using System.Threading.Tasks;

// Added during quickstart
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.UI.Notifications;
using Windows.Web.Syndication;

namespace BackgroundTasks
{
    public sealed class BlogFeedBackgroundTask  : IBackgroundTask
    {
        public async void Run( IBackgroundTaskInstance taskInstance )
        {
            // Get a deferral, to prevent the task from closing prematurely
            // while asynchronous code is still running.
            BackgroundTaskDeferral deferral = taskInstance.GetDeferral();

            // Download the feed.
            var feed = await GetMSDNBlogFeed();

            // Update the live tile with the feed items.
            UpdateTile( feed );

            // Inform the system that the task is finished.
            deferral.Complete();
        }

        private static async Task<SyndicationFeed> GetMSDNBlogFeed()
        {
            SyndicationFeed feed = null;

            try
            {
                // Create a syndication client that downloads the feed.  
                SyndicationClient client = new SyndicationClient();
                client.BypassCacheOnRetrieve = true;
                client.SetRequestHeader( customHeaderName, customHeaderValue );

                // Download the feed.
                feed = await client.RetrieveFeedAsync( new Uri( feedUrl ) );
            }
            catch( Exception ex )
            {
                Debug.WriteLine( ex.ToString() );
            }

            return feed;
        }

        private static void UpdateTile( SyndicationFeed feed )
        {
            // Create a tile update manager for the specified syndication feed.
            var updater = TileUpdateManager.CreateTileUpdaterForApplication();
            updater.EnableNotificationQueue( true );
            updater.Clear();

            // Keep track of the number feed items that get tile notifications.
            int itemCount = 0;

            // Create a tile notification for each feed item.
            foreach( var item in feed.Items )
            {
                XmlDocument tileXml = TileUpdateManager.GetTemplateContent( TileTemplateType.TileWideText03 );

                var title = item.Title;
                string titleText = title.Text == null ? String.Empty : title.Text;
                tileXml.GetElementsByTagName( textElementName )[0].InnerText = titleText;

                // Create a new tile notification.
                updater.Update( new TileNotification( tileXml ) );

                // Don't create more than 5 notifications.
                if( itemCount++ > 5 ) break;
            }
        }

        // Although most HTTP servers do not require User-Agent header, others will reject the request or return
        // a different response if this header is missing. Use SetRequestHeader() to add custom headers.
        static string customHeaderName = "User-Agent";
        static string customHeaderValue = "Mozilla/5.0 (compatible; MSIE 10.0; Windows NT 6.2; WOW64; Trident/6.0)";

        static string textElementName = "text";
        static string feedUrl = @"http://blogs.msdn.com/b/MainFeed.aspx?Type=BlogsOnly";
    }
}
```

## <a name="set-up-the-package-manifest"></a>Configurar o manifesto do pacote


Para configurar o manifesto do pacote, abra-o e adicione uma nova declaração de tarefa em segundo plano. Configure o ponto de entrada da tarefa para o nome da classe, inclusive seu namespace.

1.  No Gerenciador de Soluções, abra Package.appxmanifest.
2.  Clique ou toque na guia **Declarações**.
3.  Em **Declarações Disponíveis**, selecione **BackgroundTasks** e clique em **Adicionar**. O Visual Studio adiciona **BackgroundTasks** a **Declarações Suportadas**.
4.  Em **Tipos de tarefa suportados**, certifique-se de que **Temporizador** esteja marcado.
5.  Em **Configurações de aplicativo**, defina o ponto de entrada como **BackgroundTasks.BlogFeedBackgroundTask**.
6.  Clique ou toque na guia **Interface de Usuário do Aplicativo**.
7.  Defina **Notificações de tela de bloqueio** como **Notificação e Texto de Bloco**.
8.  Defina um caminho como um ícone de 24 x 24 pixels no campo **Logotipo de notificação** .
    **Importante**este ícone deve usar apenas pixels monocromáticos e transparentes.
9.  No campo **Logotipo pequeno**, defina um caminho como um ícone de 30 x 30 pixels.
10. No campo **Logotipo largo** , defina um caminho como um ícone de 310 x 150 pixels.

## <a name="register-the-background-task"></a>Registrar a tarefa em segundo plano


Crie um [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768) para registrar a tarefa.

> **Observação**a partir do Windows 8.1, parâmetros de registro de tarefa em segundo plano são validados no momento do registro. Um erro será retornado se algum parâmetro de registro for inválido. Seu aplicativo deve ser capaz de manipular cenários em que o registro de tarefas em segundo plano apresenta falha, por exemplo, use uma instrução condicional para verificar se há erros de registro e tente novamente o registro com falha usando valores de parâmetros diferentes.
 

Na página principal de seu aplicativo, adicione o método **RegisterBackgroundTask** e chame-o no manipulador de eventos **OnNavigatedTo**.

```cs
using System;
using System.Collections.Generic;
using System.IO;
using System.Linq;
using System.Threading.Tasks;
using Windows.ApplicationModel.Background;
using Windows.Data.Xml.Dom;
using Windows.Foundation;
using Windows.Foundation.Collections;
using Windows.UI.Xaml;
using Windows.UI.Xaml.Controls;
using Windows.UI.Xaml.Controls.Primitives;
using Windows.UI.Xaml.Data;
using Windows.UI.Xaml.Input;
using Windows.UI.Xaml.Media;
using Windows.UI.Xaml.Navigation;
using Windows.Web.Syndication;

// The Blank Page item template is documented at http://go.microsoft.com/fwlink/p/?LinkID=234238

namespace ContosoApp
{
    /// <summary>
    /// An empty page that can be used on its own or navigated to within a Frame.
    /// </summary>
    public sealed partial class MainPage : Page
    {
        public MainPage()
        {
            this.InitializeComponent();
        }

        /// <summary>
        /// Invoked when this page is about to be displayed in a Frame.
        /// </summary>
        /// <param name="e">Event data that describes how this page was reached.  The Parameter
        /// property is typically used to configure the page.</param>
        protected override void OnNavigatedTo( NavigationEventArgs e )
        {
            this.RegisterBackgroundTask();
        }


        private async void RegisterBackgroundTask()
        {
            var backgroundAccessStatus = await BackgroundExecutionManager.RequestAccessAsync();
            if( backgroundAccessStatus == BackgroundAccessStatus.AllowedMayUseActiveRealTimeConnectivity ||
                backgroundAccessStatus == BackgroundAccessStatus.AllowedWithAlwaysOnRealTimeConnectivity )
            {
                foreach( var task in BackgroundTaskRegistration.AllTasks )
                {
                    if( task.Value.Name == taskName )
                    {
                        task.Value.Unregister( true );
                    }
                }

                BackgroundTaskBuilder taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = taskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger( new TimeTrigger( 15, false ) );
                var registration = taskBuilder.Register();
            }
        }

        private const string taskName = "BlogFeedBackgroundTask";
        private const string taskEntryPoint = "BackgroundTasks.BlogFeedBackgroundTask";
    }
}
```

## <a name="debug-the-background-task"></a>Depurar a tarefa em segundo plano


Para depurar a tarefa em segundo plano, defina um ponto de interrupção no método Run da tarefa. Na barra de tarefas **Local do Depurador**, selecione a tarefa em segundo plano. Isso fará com que o sistema chame o método Run imediatamente.

1.  Defina um ponto de interrupção no método Run da tarefa.
2.  Pressione F5 ou toque em **Depurar &gt; Iniciar Depuração** para implantar e executar o aplicativo.
3.  Depois que o aplicativo iniciar, volte para o Visual Studio.
4.  Verifique se a barra de ferramentas **Local do Depurador** está visível. Ela está no menu **Exibir &gt; Barras de Ferramentas**.
5.  Na barra de ferramentas **Local do Depurador** , clique na lista suspensa **Suspender** e selecione **BlogFeedBackgroundTask**.
6.  O Visual Studio suspenderá a execução no ponto de interrupção.
7.  Pressione F5 ou toque em **Depurar &gt; Continuar** para continuar a executar o aplicativo.
8.  Pressione Shift+F5 ou toque em **Depurar &gt; Parar Depuração** para parar a depuração.
9.  Retorne ao bloco do aplicativo na tela inicial. Após alguns segundos, as notificações do bloco serão exibidas no bloco do aplicativo.

## <a name="related-topics"></a>Tópicos relacionados


* [**BackgroundTaskBuilder**](https://msdn.microsoft.com/library/windows/apps/br224768)
* [**TileUpdateManager**](https://msdn.microsoft.com/library/windows/apps/br208622)
* [**TileNotification**](https://msdn.microsoft.com/library/windows/apps/br208616)
* [Torne seu aplicativo compatível com tarefas em segundo plano](support-your-app-with-background-tasks.md)
* [Diretrizes e lista de verificação de blocos e notificações](https://msdn.microsoft.com/library/windows/apps/hh465403)

 

 
