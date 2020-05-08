---
title: Criar um aplicativo Android simples com Xamarin. Forms
description: Como começar a escrever aplicativos Android com o Xamarin. Forms
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, xamarin. Forms, XAML, tutorial
ms.date: 04/28/2020
ms.openlocfilehash: a1426bfef9863227c1ac110bc295536786695df7
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255190"
---
# <a name="get-started-developing-for-android-using-xamarinforms"></a>Comece a desenvolver para Android usando o Xamarin. Forms

Este guia irá ajudá-lo a começar a usar o Xamarin. Forms no Windows para criar um aplicativo de plataforma cruzada que funcionará em dispositivos Android.

Neste artigo, você criará um aplicativo simples do Android usando o Xamarin. Forms e o Visual Studio 2019.

## <a name="requirements"></a>Requisitos

Para usar este tutorial, você precisará do seguinte:

- Windows 10
- [Visual Studio 2019: Comunidade, Professional ou Enterprise](https://visualstudio.microsoft.com/downloads/) (consulte a observação)
- A carga de trabalho "desenvolvimento móvel com .NET" para o Visual Studio 2019

> [!NOTE]
> Este guia funcionará com o Visual Studio 2017 ou 2019. Se você estiver usando o Visual Studio 2017, algumas instruções poderão estar incorretas devido a diferenças de interface do usuário entre as duas versões do Visual Studio.

Você também terá um telefone Android ou um emulador configurado no qual executar seu aplicativo. Consulte [testar em um dispositivo Android ou emulador](emulator.md).

## <a name="create-a-new-xamarinforms-project"></a>Criar um novo projeto Xamarin. Forms

Inicie o Visual Studio. Clique em arquivo > novo projeto de > para criar um novo projeto.

Na caixa de diálogo novo projeto, selecione o modelo **aplicativo móvel (Xamarin. Forms)** e clique em **Avançar**.

Nomeie o projeto **TimeChangerForms** e clique em **criar**.

Na caixa de diálogo novo aplicativo de plataforma cruzada, selecione **em branco**. Na seção plataforma, marque **Android** e desmarque todas as outras caixas. Clique em **OK**.

O Xamarin criará uma nova solução com dois projetos: **TimeChangerForms** e **TimeChangerForms. Android.**

## <a name="create-a-ui-with-xaml"></a>Criar uma interface do usuário com XAML

Expanda o projeto **TimeChangerForms** e abra **MainPage. XAML**. O XAML nesse arquivo define a primeira tela que um usuário verá ao abrir o timechanger.

A interface do usuário do timechanger é simples. Ele exibe a hora atual e tem botões para ajustar o tempo em incrementos de uma hora. Ele usa um StackLayout vertical para alinhar o tempo acima dos botões e um StackLayout horizontal para organizar os botões lado a lado. O conteúdo é centralizado na tela definindo o **horizontaloptions** e **verticaloptions** da StackLayout vertical como **"CenterAndExpand"**.

Substitua o conteúdo de MainPage. XAML pelo código a seguir.

```xml
<?xml version="1.0" encoding="utf-8" ?>
<ContentPage xmlns="http://xamarin.com/schemas/2014/forms"
             xmlns:x="http://schemas.microsoft.com/winfx/2009/xaml"
             xmlns:d="http://xamarin.com/schemas/2014/forms/design"
             xmlns:mc="http://schemas.openxmlformats.org/markup-compatibility/2006"
             mc:Ignorable="d"
             x:Class="TimeChangerForms.MainPage">

    <StackLayout HorizontalOptions="CenterAndExpand"
                 VerticalOptions="CenterAndExpand">
        <Label x:Name="time"
               HorizontalOptions="CenterAndExpand"
               VerticalOptions="CenterAndExpand"
               Text="At runtime, this Label will display the current time.">
        </Label>
        <StackLayout Orientation="Horizontal">
            <Button HorizontalOptions="End"
                    VerticalOptions="End"
                    Text="Up"
                    Clicked="OnUpButton_Clicked"/>
            <Button HorizontalOptions="Start"
                    VerticalOptions="End"
                    Text="Down"
                    Clicked="OnDownButton_Clicked"/>
        </StackLayout>
    </StackLayout>
</ContentPage>
```

Neste ponto, a interface do usuário foi concluída. O TimeChangerForms, no entanto, não será compilado porque os métodos **UpButton_Clicked** e **DownButton_Clicked** são referenciados no XAML, mas não definidos em nenhum lugar. Mesmo que o aplicativo tenha sido executado, a hora atual não seria exibida. Na próxima seção, você corrigirá esses erros e adicionará funcionalidade à sua interface do usuário.

## <a name="add-logic-code-with-c"></a>Adicionar código lógico com C #

Na Gerenciador de Soluções, clique com o botão direito em MainPage. XAML e clique em **Exibir código**. Esse arquivo contém o code-behind que adicionará funcionalidade à interface do usuário.

### <a name="set-the-current-time"></a>Definir a hora atual

O código neste arquivo pode referenciar controles declarados no XAML usando o valor do atributo **x:Name** do controle. Nesse caso, o rótulo que exibe a hora atual é chamado `time`.

Os controles da interface do usuário devem ser atualizados no thread principal. As alterações feitas de outro thread podem não atualizar corretamente o controle conforme ele é exibido na tela. Como não há nenhuma garantia de que esse código sempre estará em execução no thread principal, use o método **BeginInvokeOnMainThread** para verificar se as atualizações são exibidas corretamente. Aqui está o método UpdateTimeLabel completo.

```csharp
private void UpdateTimeLabel(object state = null)
{
    Device.BeginInvokeOnMainThread(() =>
        {
            time.Text = DateTime.Now.ToLongTimeString();
        }
    );
}
```

### <a name="update-the-current-time-once-every-second"></a>Atualizar a hora atual uma vez a cada segundo

Neste ponto, a hora atual será precisa para, no máximo, um segundo depois que TimeChangerForms for iniciado. O rótulo deve ser atualizado periodicamente para manter o tempo preciso. Um objeto de **timer** chamará periodicamente um método de retorno de chamada que atualiza o rótulo com a hora atual.

```csharp
var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
```

### <a name="add-houroffset"></a>Adicionar HourOffset

Os botões para cima e para baixo ajustam o tempo em incrementos de uma hora. Adicione uma propriedade **HourOffset** para controlar o ajuste atual.

```csharp
public int HourOffset { get; private set; }
```

Agora, atualize o método UpdateTimeLabel para estar ciente da propriedade HourOffset.

```csharp
currentTime.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="add-button-click-event-handlers"></a>Adicionar manipuladores de eventos de clique no botão

Todos os botões para cima e para baixo precisam ser incrementados ou decrementados na propriedade HourOffset e chamam UpdateTimeLabel.

```csharp
private void UpButton_Clicked(object sender, EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

Quando tiver terminado, MainPage.xaml.cs deverá ter esta aparência:

```csharp
using System;
using System.ComponentModel;
using System.Threading;
using Xamarin.Forms;

namespace TimeChangerForms
{
    // Learn more about making custom code visible in the Xamarin.Forms previewer
    // by visiting https://aka.ms/xamarinforms-previewer
    [DesignTimeVisible(false)]
    public partial class MainPage : ContentPage
    {
        public int HourOffset { get; private set; }

        public MainPage()
        {
            InitializeComponent();
        }

        protected override void OnAppearing()
        {
            base.OnAppearing();
            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);
        }

        private void UpdateTimeLabel(object state = null)
        {
            Device.BeginInvokeOnMainThread(() =>
                {
                    time.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
                }
            );
        }

        private void OnUpButton_Clicked(object sender, EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        private void OnDownButton_Clicked(object sender, EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-the-app"></a>Executar o aplicativo

Para executar o aplicativo, pressione **F5** ou clique em Depurar > iniciar a depuração. Dependendo de como o [depurador está configurado](emulator.md), seu aplicativo será iniciado em um dispositivo ou em um emulador.

## <a name="related-links"></a>Links relacionados

- [Teste em um dispositivo Android ou emulador](emulator.md).

- [Criar um aplicativo de exemplo do Android usando o Xamarin. Android](xamarin-android.md)
