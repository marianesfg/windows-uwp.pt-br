---
author: mtoepke
title: Objeto de aplicativo e DirectX
description: A Plataforma Universal do Windows (UWP) com jogos em DirectX não usa muitos elementos e objetos da interface do usuário do Windows.
ms.assetid: 46f92156-29f8-d65e-2587-7ba1de5b48a6
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, directx, objeto de app
ms.localizationpriority: medium
ms.openlocfilehash: fcbe68516e3ad8b2643faf68900e3305f18e8bbf
ms.sourcegitcommit: 9354909f9351b9635bee9bb2dc62db60d2d70107
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/16/2018
ms.locfileid: "4682387"
---
# <a name="the-app-object-and-directx"></a>Objeto de aplicativo e DirectX



A Plataforma Universal do Windows (UWP) com jogos em DirectX não usa muitos elementos e objetos da interface do usuário do Windows. Em vez disso, por serem executados em um nível mais baixo na pilha do Windows Runtime, eles devem interoperam com a estrutura da IU de uma maneira mais básica: acessando e interoperando com o objeto do aplicativo diretamente. Saiba como e quando essa interoperação ocorre e como você, como um desenvolvedor de DirectX, pode usar esse modelo no desenvolvimento do seu aplicativo UWP de forma eficaz.

Consulte o [Glossário de elementos gráficos do Direct3D](../graphics-concepts/index.md) para obter informações sobre termos desconhecidos gráficos ou conceitos que você encontrar durante a leitura.

## <a name="the-important-core-user-interface-namespaces"></a>Os importantes namespaces de interface do usuário básico


Primeiramente, vejamos os namespaces do Windows Runtime que você deve incluir (com **using**) no seu aplicativo UWP. Vamos entrar um pouco em detalhes.

-   [**Windows.ApplicationModel.Core**](https://msdn.microsoft.com/library/windows/apps/br205865)
-   [**Windows.ApplicationModel.Activation**](https://msdn.microsoft.com/library/windows/apps/br224766)
-   [**Windows.UI.Core**](https://msdn.microsoft.com/library/windows/apps/br208383)
-   [**Windows.System**](https://msdn.microsoft.com/library/windows/apps/br241814)
-   [**Windows.Foundation**](https://msdn.microsoft.com/library/windows/apps/br226021)

> **Observação**   Se você não estiver desenvolvendo um aplicativo UWP, use os componentes da interface do usuário fornecidos nas bibliotecas específicas de JavaScript ou XAML e os namespaces em vez dos tipos fornecidos nesses namespaces.

 

## <a name="the-windows-runtime-app-object"></a>Objeto de aplicativo do Windows Runtime


Em seu aplicativo UWP, é melhor ter uma janela e um provedor de visualização para a exibição e ao qual conectar sua cadeia de troca (buffers de vídeo). Você também pode conectar essa exibição aos eventos específicos de janela para o seu aplicativo em execução. Para obter uma janela pai para o objeto do aplicativo, definido pelo tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), crie um tipo que implemente o [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482), como fizemos no trecho de código anterior.

Aí vai o conjunto básico de etapas para obter uma janela usando a estrutura principal da IU:

1.  Crie um tipo que implemente o [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478). Esta é a sua visualização.

    Neste tipo, defina:

    -   Um método [**Initialize**](https://msdn.microsoft.com/library/windows/apps/hh700495) que usa uma instância de [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) como parâmetro. Você pode obter uma instância desse tipo chamando [**CoreApplication.CreateNewView**](https://msdn.microsoft.com/library/windows/apps/dn297278). O objeto do aplicativo o chamará quando o aplicativo for inicializado.
    -   Um método [**SetWindow**](https://msdn.microsoft.com/library/windows/apps/hh700509) que usa uma instância de [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) como parâmetro. Você pode obter uma instância desse tipo acessando a propriedade [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) na sua nova instância [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017).
    -   Um método [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501) que usa uma cadeia de caracteres como ponto de entrada como parâmetro único. O objeto de aplicativo fornece a cadeia de ponto de entrada quando você chama esse método. É aqui que você configura os recursos. Você cria os recursos do seu dispositivo aqui. O objeto do aplicativo o chamará quando o aplicativo for inicializado.
    -   Um método [**Run**](https://msdn.microsoft.com/library/windows/apps/hh700505) que ativa o objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) e inicia o despachante do evento de janela. O objeto do aplicativo o chamará quando o processo do aplicativo iniciar.
    -   Um método [**Uninitialize**](https://msdn.microsoft.com/library/windows/apps/hh700523) que limpa as configurações de recursos ao chamar o [**Load**](https://msdn.microsoft.com/library/windows/apps/hh700501). O objeto do aplicativo esse método quando o aplicativo for fechado.

2.  Crie um tipo que implemente o [**IFrameworkViewSource**](https://msdn.microsoft.com/library/windows/apps/hh700482). Esta é o seu provedor de modo de exibição.

    Neste tipo, defina:

    -   Um método chamado [**CreateView**](https://msdn.microsoft.com/library/windows/apps/hh700491) que devolve uma instância da sua implementação [**IFrameworkView**](https://msdn.microsoft.com/library/windows/apps/hh700478), como criada na etapa 1.

3.  Passe uma instância do provedor de modo de exibição para [**CoreApplication.Run**](https://msdn.microsoft.com/library/windows/apps/hh700469) de **main**.

Com essas etapas em mente, vamos ver mais opções existentes para desenvolver essa abordagem.

## <a name="core-user-interface-types"></a>Tipos de interfaces do usuário básicas


Aqui estão os principais tipos de interface do usuário no Windows Runtime que podem ajudá-lo:

-   [**Windows.ApplicationModel.Core.CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017)
-   [**Windows.UI.Core.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225)
-   [**Windows.UI.Core.CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211)

Você pode usar esses tipos para acessar a exibição do seu aplicativo, em especial, os bits que desenham os conteúdos da janela pai do aplicativo, e manipular os eventos disparados pela janela. O processo da janela do aplicativo é um *single-threaded apartment de aplicativo* (ASTA) que fica isolado e manipula todos retornos de chamadas.

A exibição de seu aplicativo é gerada pelo provedor de exibição da janela do aplicativo, e na maioria dos casos será implementada por um pacote de estrutura específica ou pelo próprio sistema, assim você não precisa implementá-la. Para o DirectX, você precisa implementar um provedor de modo de exibição dinâmica, como discutido anteriormente. Há uma relação um a um específica entre os seguintes componentes e comportamentos:

-   Uma exibição do aplicativo, representada pelo tipo [**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) e que define o(s) método(s) para a atualização da janela.
-   Um ASTA, a atribuição do que define o comportamento de threading do aplicativo. Você não pode criar instâncias de tipo atribuídos a COM STA em um ASTA.
-   Um provedor de modo de exibição, que o seu aplicativo obtém do sistema ou que você implementa.
-   Uma janela pai, representada pelo tipo [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225).
-   Obtendo todos os eventos de ativação. Tanto exibições quanto janelas têm eventos de ativação separados.

Em resumo, o objeto de aplicativo fornece uma fábrica de provedores de visualização. Ele cria um provedor de modo de exibição e uma instância de uma janela pai para o aplicativo. O provedor de modo de exibição define o modo de exibição do aplicativo para a janela pai do aplicativo. Agora, discutiremos as especificidades da janela de exibição e da janela pai.

## <a name="coreapplicationview-behaviors-and-properties"></a>Comportamentos e propriedades do CoreApplicationView


[**CoreApplicationView**](https://msdn.microsoft.com/library/windows/apps/br225017) representa a exibição atual do aplicativo. O singleton do aplicativo cria a exibição do aplicativo durante a inicialização, mas a exibição se mantém inativa até ser ativada. Você pode obter o [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) que mostra a exibição acessando a propriedade [**CoreApplicationView.CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br225019) nele, e pode manipular eventos de ativação e desativação para a exibição registrando delegados com o evento [**CoreApplicationView.Activated**](https://msdn.microsoft.com/library/windows/apps/br225018).

## <a name="corewindow-behaviors-and-properties"></a>Comportamentos e propriedades do CoreWindow


A janela pai, que é uma instância [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225), é criada e passada para o provedor de modo de exibição quando o objeto do aplicativo é inicializado. Se o aplicativo tem uma janela para ser exibida, ele o fará. Caso contrário, ele simplesmente inicializará a exibição.

[**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) oferece vários eventos específicos a entradas e comportamentos de janela básicos. Você pode tratar desses eventos registrando seus próprios delegados com eles.

Você também pode obter o despachante do evento de janela para a janela acessando a propriedade [**CoreWindow.Dispatcher**](https://msdn.microsoft.com/library/windows/apps/br208264), que fornece uma instância do [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211).

## <a name="coredispatcher-behaviors-and-properties"></a>Comportamentos e propriedades do CoreDispatcher


Você pode determinar o comportamento de threading de despache de eventos para uma janela com o tipo [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). Nesse tipo, há um método particularmente importante: o método [**CoreDispatcher.ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215), que inicia o processamento do evento de janela. Chamar esse método pela opção errada do seu aplicativo pode levar a todo tipo de comportamentos de processamento de eventos inesperados.

| Opção CoreProcessEventsOption                                                           | Descrição                                                                                                                                                                                                                                  |
|------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [**CoreProcessEventsOption.ProcessOneAndAllPending**](https://msdn.microsoft.com/library/windows/apps/br208217) | Despache todos os eventos disponíveis atualmente na fila. Se não houver eventos pendentes, aguarde o próximo novo evento.                                                                                                                                 |
| [**CoreProcessEventsOption.ProcessOneIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | Despache um evento se ele estiver pendente na fila. Se não houver eventos pendentes, não aguarde um novo evento ser gerado. Retorne imediatamente.                                                                                          |
| [**CoreProcessEventsOption.ProcessUntilQuit**](https://msdn.microsoft.com/library/windows/apps/br208217)        | Aguarde novos eventos e despache todos os eventos disponíveis. Siga esse comportamento até a janela fechar ou o aplicativo chamar o método [**Close**](https://msdn.microsoft.com/library/windows/apps/br208260) na instância [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225). |
| [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217)     | Despache todos os eventos disponíveis atualmente na fila. Se não houver eventos pendentes, retorne imediatamente.                                                                                                                                          |

 

UWP com uso do DirectX deve usar a opção [**CoreProcessEventsOption.ProcessAllIfPresent**](https://msdn.microsoft.com/library/windows/apps/br208217) para evitar comportamentos de bloqueio que possam interromper atualizações dos elementos gráficos.

## <a name="asta-considerations-for-directx-devs"></a>Considerações sobre ASTA para desenvolvedores DirectX


O objeto do aplicativo que define a representação do tempo de execução do seu aplicativo UWP e DirectX usa um modelo de threading single-threaded apartment de aplicativo (ASTA) para hospedar as exibições de interface do usuário do seu aplicativo. Se você está desenvolvendo um aplicativo UWP e DirectX, conhece as propriedades de um ASTA porque qualquer thread que você despacha do seu aplicativo UWP e DirectX deve usar as APIs [**Windows::System::Threading**](https://msdn.microsoft.com/library/windows/apps/br229642) ou o [**CoreWindow::CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211). (Você pode obter o objeto [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para o ASTA chamando o [**CoreWindow::GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589) pelo seu aplicativo).

É importante estar ciente de que, como desenvolvedor de um aplicativo UWP DirectX, você deve habilitar o thread do seu aplicativo para despachar threads de MTA definindo **Platform::MTAThread** como **main()**.

```cpp
[Platform::MTAThread]
int main(Platform::Array<Platform::String^>^)
{
    auto myDXAppSource = ref new MyDXAppSource(); // your view provider factory 
    CoreApplication::Run(myDXAppSource);
    return 0;
}
```

Quando o objeto do seu aplicativo UWP DirectX é ativado, ele cria o ASTA que será usado na exibição da IU. O novo thread do ASTA chama fábrica do seu provedor de modo de exibição para criar provedor de modo de exibição para o objeto do seu aplicativo. Dessa forma, o código do provedor de modo de exibição será executado naquele thread do ASTA.

Além disso, qualquer thread que você criar a partir do ASTA deve estar em um MTA. Observe que qualquer thread MTA que você criar ainda pode criar problemas de reentrância e resultar em deadlock.

Se você estiver fazendo a portabilidade de um código existente para ser executado no thread do ASTA, leve o seguinte em consideração:

-   Espere primitivos, como o [**CoWaitForMultipleObjects**](https://msdn.microsoft.com/library/windows/desktop/hh404144), se comportarem de maneira diferente em um ASTA de em um STA.
-   O loop modal de chamada COM opera de forma diferente em um ASTA. Não é mais possível receber chamadas não relacionadas enquanto uma chamada de saída está em andamento. Por exemplo, o comportamento a seguir cria um deadlock de um ASTA (e cria imediatamente a falha do aplicativo):
    1.  O ASTA chama um objeto MTA e passa um ponteiro de interface P1.
    2.  Posteriormente, o ASTA chama o mesmo objeto MTA. O objeto MTA chama P1 antes de retornar ao ASTA.
    3.  P1 não pode entrar no ASTA, uma vez que ele está bloqueado fazendo uma chamada não relacionada. Entretanto, o thread do MTA fica bloqueado quando tenta chamar o P1.

    Você pode resolver isso:
    -   Usando o padrão **async** definido na Biblioteca de padrões paralelos (PPLTasks.h)
    -   Chamando o [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) a partir do ASTA do seu aplicativo (o thread principal do seu aplicativo) o quanto antes para permitir chamadas arbitrárias.

    Ou seja, você não pode depender da entrega imediata de chamadas não relacionadas para o ASTA do seu aplicativo. Para saber mais sobre chamadas assíncronas, leia [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

De forma geral, quando for projetar o seu aplicativo UWP, use o [**CoreDispatcher**](https://msdn.microsoft.com/library/windows/apps/br208211) para o [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) do seu aplicativo e o [**CoreDispatcher::ProcessEvents**](https://msdn.microsoft.com/library/windows/apps/br208215) para manipular todos os threads de interface do usuário em vez de tentar criar e gerenciar os threads do MTA por si só. Quando precisar de um thread separado que não puder manipular como o **CoreDispatcher**, use padrões assíncronos e siga as orientações mencionadas para evitar problemas de reentrância.

 

 




