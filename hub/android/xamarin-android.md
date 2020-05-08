---
title: Criar um aplicativo Android simples com o Xamarin. Android
description: Como começar a escrever aplicativos Android com o Xamarin. Android
author: hickeys
ms.author: hickeys
manager: jken
ms.topic: article
keywords: Android, Windows, xamarin. Android, tutorial, XAML
ms.date: 04/28/2020
ms.openlocfilehash: c731b5f96243333e4a4ad150de499ac9459113bc
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255200"
---
# <a name="get-started-developing-for-android-using-xamarinandroid"></a>Comece a desenvolver para Android usando o Xamarin. Android

Este guia irá ajudá-lo a começar a usar o Xamarin. Android no Windows para criar um aplicativo de plataforma cruzada que funcionará em dispositivos Android.

Neste artigo, você criará um aplicativo simples do Android usando o Xamarin. Android e o Visual Studio 2019.

## <a name="requirements"></a>Requisitos

Para usar este tutorial, você precisará do seguinte:

- Windows 10
- [Visual Studio 2019: Comunidade, Professional ou Enterprise](https://visualstudio.microsoft.com/downloads/) (consulte a observação)
- A carga de trabalho "desenvolvimento móvel com .NET" para o Visual Studio 2019

> [!NOTE]
> Este guia funcionará com o Visual Studio 2017 ou 2019. Se você estiver usando o Visual Studio 2017, algumas instruções poderão estar incorretas devido a diferenças de interface do usuário entre as duas versões do Visual Studio.

Você também terá um telefone Android ou um emulador configurado no qual executar seu aplicativo. Consulte [Configurando um emulador do Android](emulator.md).

## <a name="create-a-new-xamarinandroid-project"></a>Criar um novo projeto Xamarin.Android

Inicie o Visual Studio. Selecione Arquivo > novo projeto de > para criar um novo projeto.

Na caixa de diálogo novo projeto, selecione o modelo **aplicativo do Android (Xamarin)** e clique em **Avançar**.

Nomeie o projeto **TimeChangerAndroid** e clique em **criar**.

Na caixa de diálogo novo aplicativo de plataforma cruzada, selecione **aplicativo em branco**. Na **versão mínima do Android**, selecione **Android 5,0 (pirulito)**. Clique em **OK**.

O Xamarin criará uma nova solução com um único projeto chamado **TimeChangerAndroid**.

## <a name="create-a-ui-with-xaml"></a>Criar uma interface do usuário com XAML

No diretório **Resources\layout** do seu projeto, abra **activity_main. xml**. O XML nesse arquivo define a primeira tela que um usuário verá ao abrir o timechanger.

A interface do usuário do timechanger é simples. Ele exibe a hora atual e tem botões para ajustar o tempo em incrementos de uma hora. Ele usa um vertical `LinearLayout` para alinhar o tempo acima dos botões e um `LinearLayout` horizontal para organizar os botões lado a lado. O conteúdo é centralizado na tela definindo o atributo **Android: gravidade** como **Center** na vertical `LinearLayout`.

Substitua o conteúdo de **activity_main. xml** pelo código a seguir.

```xml
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical"
    android:gravity="center">
    <TextView
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:text="At runtime, I will display current time"
        android:id="@+id/timeDisplay"
    />
    <LinearLayout
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"
        android:orientation="horizontal">
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Up"
            android:id="@+id/upButton"/>
        <Button
            android:layout_width="wrap_content"
            android:layout_height="wrap_content"
            android:text="Down"
            android:id="@+id/downButton"/>
    </LinearLayout>
</LinearLayout>
```

Neste ponto, você pode executar o **TimeChangerAndroid** e ver a interface do usuário que você criou. Na próxima seção, você adicionará funcionalidade à sua interface do usuário exibindo a hora atual e habilitando os botões para executar uma ação.

## <a name="add-logic-code-with-c"></a>Adicionar código lógico com C #

Abra **MainActivity.cs**. Esse arquivo contém a lógica code-behind que adicionará funcionalidade à interface do usuário.

### <a name="set-the-current-time"></a>Definir a hora atual

Primeiro, obtenha uma referência para o `TextView` que exibirá a hora. Use **FindViewById** para pesquisar todos os elementos da interface do usuário para aquele com o **Android: ID** correto (que `"@+id/timeDisplay"` foi definido como no XML da etapa anterior). Isso será o `TextView` que exibirá a hora atual.

```csharp
var timeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
```

Os controles da interface do usuário devem ser atualizados no thread da interface do usuário. As alterações feitas de outro thread podem não atualizar corretamente o controle conforme ele é exibido na tela. Como não há nenhuma garantia de que esse código sempre estará em execução no thread da interface do usuário, use o método **RunOnUiThread** para verificar se as atualizações são exibidas corretamente. Este é o método `UpdateTimeLabel` completo.

```csharp
private void UpdateTimeLabel(object state = null)
{
    RunOnUiThread(() =>
    {
        TimeDisplay.Text = DateTime.Now.ToLongTimeString();
    });
}
```

### <a name="update-the-current-time-once-every-second"></a>Atualizar a hora atual uma vez a cada segundo

Neste ponto, a hora atual será precisa para, no máximo, um segundo depois que TimeChangerAndroid for iniciado. O rótulo deve ser atualizado periodicamente para manter o tempo preciso. Um objeto de **timer** chamará periodicamente um método de retorno de chamada que atualiza o rótulo com a hora atual.

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
TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
```

### <a name="create-the-button-click-event-handlers"></a>Criar manipuladores de eventos de clique no botão

Todos os botões para cima e para baixo precisam ser incrementados ou decrementados na propriedade HourOffset e chamam UpdateTimeLabel.

```csharp
public void UpButton_Click(object sender, System.EventArgs e)
{
    HourOffset++;
    UpdateTimeLabel();
}
```

### <a name="wire-up-the-up-and-down-buttons-to-their-corresponding-event-handlers"></a>Vincular os botões acima e abaixo aos manipuladores de eventos correspondentes

Para associar os botões a seus manipuladores de eventos correspondentes, primeiro use FindViewById para localizar os botões por suas IDs. Depois de ter uma referência ao objeto Button, você pode adicionar um manipulador de eventos ao seu `Click` evento.

```csharp
Button upButton = FindViewById<Button>(Resource.Id.upButton);
upButton.Click += UpButton_Click;
```

## <a name="completed-mainactivitycs-file"></a>Arquivo MainActivity.cs concluído

Quando tiver terminado, MainActivity.cs deverá ter esta aparência:

```csharp
using Android.App;
using Android.OS;
using Android.Support.V7.App;
using Android.Runtime;
using Android.Widget;
using System;
using System.Threading;

namespace TimeChangerAndroid
{
    [Activity(Label = "@string/app_name", Theme = "@style/AppTheme", MainLauncher = true)]
    public class MainActivity : AppCompatActivity
    {
        public TextView TimeDisplay { get; private set; }
        public int HourOffset { get; private set; }

        protected override void OnCreate(Bundle savedInstanceState)
        {
            base.OnCreate(savedInstanceState);

            // Set the view from the "main" layout resource
            SetContentView(Resource.Layout.activity_main);

            var clockRefresh = new Timer(dueTime: 0, period: 1000, callback: UpdateTimeLabel, state: null);

            Button upButton = FindViewById<Button>(Resource.Id.upButton);
            upButton.Click += OnUpButton_Click;

            Button downButton = FindViewById<Button>(Resource.Id.downButton);
            downButton.Click += OnDownButton_Click;

            TimeDisplay = FindViewById<TextView>(Resource.Id.timeDisplay);
        }

        private void UpdateTimeLabel(object state = null)
        {
            // Timer callbacks run on a background thread, but UI updates must run on the UI thread.
            RunOnUiThread(() =>
            {
                TimeDisplay.Text = DateTime.Now.AddHours(HourOffset).ToLongTimeString();
            });
        }

        public void OnUpButton_Click(object sender, System.EventArgs e)
        {
            HourOffset++;
            UpdateTimeLabel();
        }

        public void OnDownButton_Click(object sender, System.EventArgs e)
        {
            HourOffset--;
            UpdateTimeLabel();
        }
    }
}
```

## <a name="run-your-app"></a>Executar seu aplicativo

Para executar o aplicativo, pressione **F5** ou clique em Depurar > iniciar a depuração. Dependendo de como o [depurador está configurado](emulator.md), seu aplicativo será iniciado em um dispositivo ou em um emulador.

## <a name="related-links"></a>Links relacionados

- [Teste em um dispositivo Android ou emulador](emulator.md).
- [Criar um aplicativo de exemplo do Android usando o Xamarin. Forms](xamarin-forms.md)
