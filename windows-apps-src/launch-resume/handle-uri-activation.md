---
author: TylerMSFT
title: Manipular a ativação do URI
description: Saiba como registrar um aplicativo para ser o manipulador padrão de um nome de esquema de URI (Uniform Resource Identifier).
ms.assetid: 92D06F3E-C8F3-42E0-A476-7E94FD14B2BE
ms.author: twhitney
ms.date: 07/05/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 41c7286493e08fd62ad4b207d0e014dd4fbd5318
ms.sourcegitcommit: 3727445c1d6374401b867c78e4ff8b07d92b7adc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/29/2018
ms.locfileid: "2905982"
---
# <a name="handle-uri-activation"></a>Tratar a ativação do URI

**APIs importantes**

-   [**Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224742)
-   [**Windows.UI.Xaml.Application.OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330)

Saiba como registrar um aplicativo para ser o manipulador padrão de um nome de esquema de URI (Uniform Resource Identifier). Os aplicativos da área de trabalho do Windows e da Plataforma Universal do Windows (UWP) podem ser registrados para ser um manipulador padrão de um nome de esquema de URI. Se o usuário escolher seu aplicativo como o manipulador padrão para um nome de esquema de URI, seu aplicativo será ativado toda vez que esse tipo de URI for iniciado.

Recomendamos que você só se registre para um nome de esquema de URI se quiser manipular todas as inicializações de URI desse tipo de URI. Se você decidir se registrar para um nome de esquema de URI, você deve fornecer ao usuário final a funcionalidade esperada quando o seu aplicativo for ativado para esse esquema de URI. Por exemplo, um aplicativo que se registra para o nome de esquema de URI mailto: deve ser aberto para uma nova mensagem de email para que o usuário possa criar um novo email. Para saber mais sobre associações de URIs, consulte [Diretrizes e lista de verificação para tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

Estas etapas mostram como se registrar para um nome de esquema de URI personalizado, `alsdk://`, e como ativar seu aplicativo quando o usuário inicia um URI `alsdk://`.

> [!NOTE]
> Em aplicativos UWP, determinados URIs e extensões de arquivo são reservados para uso por aplicativos nativos e o sistema operacional. Tentativas de registrar seu aplicativo com um URI ou extensão de arquivo reservada serão ignoradas. Veja [Nomes de esquemas de URI e tipos de arquivos reservados](reserved-uri-scheme-names.md) para obter uma lista, em ordem alfabética, de esquemas de URI que você não pode registrar para seus aplicativos UWP porque eles são reservados ou proibidos.

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Etapa 1: especificar o ponto de extensão no manifesto do pacote

O aplicativo recebe os eventos de ativação somente para os nomes de esquema de URI listados no manifesto do pacote. Veja como indicar se seu aplicativo manipula o nome de esquema de URI `alsdk`.

1. No **Gerenciador de Soluções**, clique duas vezes em package.appxmanifest para abrir o designer de manifesto. Selecione a guia **Declarações** no menu suspenso **Declarações disponíveis**, selecione **Protocolo** e clique em **Adicionar**.

    Esta é uma breve descrição de cada um dos campos que você pode preencher no designer de manifesto do protocolo (consulte [**Manifesto de pacote AppX**](https://msdn.microsoft.com/library/windows/apps/dn934791) para obter detalhes):

| Campo | Descrição |
|-------|-------------|
| **Logotipo** | Especifique o logotipo usado para identificar o nome do esquema de URI em [Definir programas padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154) no **Painel de Controle**. Se nenhum logotipo for especificado, o logotipo pequeno do aplicativo será usado. |
| **Nome de Exibição** | Especifique o nome de exibição para identificar o nome de esquema de URI em [Definir programas padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154) no **Painel de Controle**. |
| **Nome** | Escolha um nome para o esquema de URI. |
|  | **Observação**  O nome precisa estar completamente em letras minúsculas. |
|  | **Tipos de arquivos reservados e proibidos** Consulte [Nomes e tipos de arquivos de esquema de URI reservados](reserved-uri-scheme-names.md) para obter uma lista em ordem alfabética de esquemas de Uri que você não pode registrar para seus aplicativos UWP porque são reservados ou proibidos. |
| **Executável** | Especifica a executável de inicialização padrão para o protocolo. Se não for especificado, o executável do aplicativo será usado. Se especificado, a cadeia de caracteres deverá ter entre 1 e 256 caracteres, terminar com ".exe" e não conter os seguintes caracteres: &gt;, &lt;, :, ", &#124;, ? ou \*. Se especificado, o **Ponto de entrada** também será usado. Se o **Ponto de entrada** não for especificado, o ponto de entrada definido para o aplicativo será usado. |
| **Ponto de entrada** | Especifica a tarefa que manipula a extensão de protocolo. Normalmente é o nome totalmente qualificado do namespace de um tipo do Windows Runtime. Se não for especificado, o ponto de entrada do aplicativo será usado. |
| **Página inicial** | A página da Web que manipula o ponto de extensibilidade. |
| **Grupo de recursos** | Uma marca que você pode usar para ativações de extensão de grupo em conjunto para fins de gerenciamento de recursos. |
| **Exibição desejada** (somente Windows) | Especifique o campo **Exibição Desejada** para indicar a quantidade de espaço que a janela do aplicativo precisa para ser iniciada para o nome do esquema URI. Os valores possíveis para **Exibição desejada** são **Default**, **UseLess**, **UseHalf**, **UseMore** ou **UseMinimum**.<br/>**Observação**  O Windows leva em conta vários fatores diferentes ao determinar o tamanho final da janela do aplicativo de destino, por exemplo, a preferência do aplicativo de origem, o número de aplicativos na tela, a orientação da tela e assim por diante. A configuração de **Exibição Desejada** não garante um comportamento de janelas específico para o aplicativo de destino.<br/>**Família de dispositivos móveis: Exibição Desejada** não tem suporte na família de dispositivos móveis. |

2. Insira `images\Icon.png` como o **Logotipo**.
3. Insira `SDK Sample URI Scheme` como o **Nome de exibição**.
4. Insira `alsdk` como o **Nome**.
5. Pressione Ctrl+S para salvar a alteração no package.appxmanifest.

    Isso adiciona ao manifesto do pacote um elemento [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) semelhante a este. A categoria **windows.protocol** indica se o aplicativo manipula o nome do esquema de URI `alsdk`.

```xml
    <Applications>
        <Application Id= ... >
            <Extensions>
                <uap:Extension Category="windows.protocol">
                  <uap:Protocol Name="alsdk">
                    <uap:Logo>images\icon.png</uap:Logo>
                    <uap:DisplayName>SDK Sample URI Scheme</uap:DisplayName>
                  </uap:Protocol>
                </uap:Extension>
          </Extensions>
          ...
        </Application>
   <Applications>
```

## <a name="step-2-add-the-proper-icons"></a>Etapa 2: adicionar os ícones apropriados

Os aplicativos que se tornam padrão para um nome de esquema de URI têm seus ícones exibidos em vários locais por todo o sistema, como no painel de controle de programas padrão. Inclua um ícone 44x44 com seu projeto para essa finalidade. Combine a aparência do logotipo do bloco do aplicativo e use a cor de fundo do aplicativo em vez de deixar o ícone transparente. Faça o logotipo se estender até a borda sem preenchê-la. Teste seus ícones em telas em segundo plano brancas. Consulte [Diretrizes para ativos de bloco e ícone](https://docs.microsoft.com/windows/uwp/shell/tiles-and-notifications/app-assets) para obter mais detalhes sobre ícones.

## <a name="step-3-handle-the-activated-event"></a>Etapa 3: manipular o evento ativado

O manipulador de eventos [**OnActivated**](https://msdn.microsoft.com/library/windows/apps/br242330) recebe todos os eventos de ativação. A propriedade **Kind** indica o tipo de evento de ativação. Este exemplo é configurado para manipular os eventos de ativação [**Protocolo**](https://msdn.microsoft.com/library/windows/apps/xaml/windows.applicationmodel.activation.activationkind.aspx#Protocol).

```csharp
public partial class App
{
   protected override void OnActivated(IActivatedEventArgs args)
  {
      if (args.Kind == ActivationKind.Protocol)
      {
         ProtocolActivatedEventArgs eventArgs = args as ProtocolActivatedEventArgs;
         // TODO: Handle URI activation
         // The received URI is eventArgs.Uri.AbsoluteUri
      }
   }
}
```

```vb
Protected Overrides Sub OnActivated(ByVal args As Windows.ApplicationModel.Activation.IActivatedEventArgs)
   If args.Kind = ActivationKind.Protocol Then
      ProtocolActivatedEventArgs eventArgs = args As ProtocolActivatedEventArgs
      
      ' TODO: Handle URI activation
      ' The received URI is eventArgs.Uri.AbsoluteUri
 End If
End Sub
```

```cppwinrt
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs const& args)
{
    if (args.Kind() == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
    {
        auto protocolActivatedEventArgs{ args.as<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs>() };
        // TODO: Handle URI activation  
        auto receivedURI{ protocolActivatedEventArgs.Uri().RawUri() };
    }
}
```

```cpp
void App::OnActivated(Windows::ApplicationModel::Activation::IActivatedEventArgs^ args)
{
   if (args->Kind == Windows::ApplicationModel::Activation::ActivationKind::Protocol)
   {
      Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^ eventArgs =
          dynamic_cast<Windows::ApplicationModel::Activation::ProtocolActivatedEventArgs^>(args);
      
      // TODO: Handle URI activation  
      // The received URI is eventArgs->Uri->RawUri
   }
}
```

> [!NOTE]
> Quando iniciado por contrato de protocolo, verifique se o que botão Voltar leva o usuário voltar para a tela que iniciou o aplicativo e não o conteúdo do aplicativo anterior.

O código a seguir inicia de maneira programática o aplicativo por meio do seu URI:

```csharp
   // Launch the URI
   var uri = new Uri("alsdk:");
   var success = await Windows.System.Launcher.LaunchUriAsync(uri)
```

Para obter mais detalhes sobre como iniciar um aplicativo por meio de um URI, consulte [Iniciar o aplicativo padrão para um URI](launch-default-app.md).

É recomendável que os aplicativos criem um novo [**Quadro**](https://msdn.microsoft.com/library/windows/apps/br242682) XAML para cada evento de ativação que abre uma nova página. Dessa forma, o backstack de navegação do novo **Quadro** XAML não terá nenhum conteúdo anterior que o aplicativo possa ter na janela atual quando suspenso. Os aplicativos que decidem usar um único **Quadro** XAML para Contratos de Inicialização e Arquivo devem limpar as páginas do diário de navegação do **Quadro** antes de navegar para uma nova página.

Quando iniciado por Ativação de protocolo, os aplicativos devem considerar incluir uma interface do usuário que permita ao usuário voltar para o início da página do aplicativo.

## <a name="remarks"></a>Comentários

Qualquer aplicativo ou site pode usar seu nome de esquema de URI, inclusive os maliciosos. Assim, qualquer dado que você obtenha no URI pode vir de uma fonte não confiável. Recomendamos que você nunca execute uma ação permanente com base nos parâmetros recebidos no URI. Por exemplo, os parâmetros de URI podem ser usados para iniciar o aplicativo em uma página de conta de usuário, mas nunca devem ser usados para modificar diretamente a conta do usuário.

> [!NOTE]
> Se você estiver criando um novo nome de esquema URI para seu aplicativo, certifique-se de seguir as orientações na [RFC 4395](http://go.microsoft.com/fwlink/p/?LinkID=266550). Isso garante que seu nome atenda aos padrões de esquemas de URI.

> [!NOTE]
> Quando iniciado por contrato de protocolo, verifique se o que botão Voltar leva o usuário voltar para a tela que iniciou o aplicativo e não o conteúdo do aplicativo anterior.

É recomendável que os aplicativos criem um novo [**Quadro**](https://msdn.microsoft.com/library/windows/apps/br242682) XAML para cada evento de ativação que abre um novo destino de Uri. Dessa forma, o backstack de navegação do novo **Quadro** XAML não terá nenhum conteúdo anterior que o aplicativo possa ter na janela atual quando suspenso.

Se decidir que deseja que seus aplicativos usem um único [**Quadro**](https://msdn.microsoft.com/library/windows/apps/br242682) XAML para Contratos de Inicialização e Protocolo, limpe as páginas do diário de navegação do **Quadro** antes de navegar para uma nova página. Quando iniciado por Contrato de Protocolo, considere incluir uma interface do usuário em seus aplicativos que permita que o usuário volte ao início do aplicativo.

## <a name="related-topics"></a>Tópicos relacionados

### <a name="complete-sample-app"></a>Aplicativo de exemplo completo

- [Amostra de inicialização de associação](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/AssociationLaunching)

### <a name="concepts"></a>Conceitos

- [Programas padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154)
- [Tipo de arquivo e modelo de associações de URIs](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>Tarefas

- [Iniciar o aplicativo padrão para um URI](launch-default-app.md)
- [Manipular a ativação do arquivo](handle-file-activation.md)

### <a name="guidelines"></a>Diretrizes

- [Diretrizes para tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Referência

- [Manifesto do pacote AppX](https://msdn.microsoft.com/library/windows/apps/dn934791)
- [Windows.ApplicationModel.Activation.ProtocolActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224742)
- [Windows.UI.Xaml.Application.OnActivated](https://msdn.microsoft.com/library/windows/apps/br242330)
