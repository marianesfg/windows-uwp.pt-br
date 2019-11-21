---
title: O que é um aplicativo da Plataforma Universal do Windows (UWP)?
description: Saiba mais sobre os aplicativos UWP (Plataforma Universal do Windows) que podem ser executados em uma ampla variedade de dispositivos nos quais o Windows 10 esteja em execução.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, universal
ms.localizationpriority: medium
ms.openlocfilehash: 5b0d226a9492a2218edf20e68b8216ea513ca12a
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260535"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>O que é um aplicativo da Plataforma Universal do Windows (UWP)?

![Os aplicativos da Plataforma Universal do Windows são executados em uma variedade de dispositivos, são compatíveis com interface do usuário adaptável, entrada do usuário natural, um repositório, Partner Center e serviços de nuvem](images/universalapps-overview.png)

Um aplicativo UWP:

- É seguro: os aplicativos UWP declaram quais dados e recursos de dispositivo eles acessam. O usuário deve autorizar esse acesso.
- Pode usar uma API comum em todos os dispositivos que executam o Windows 10.
- Pode usar funcionalidades específicas do dispositivo e adaptar a interface do usuário a diferentes tamanhos, resoluções e DPI de telas de dispositivos.
- Está disponível na Microsoft Store em todos os dispositivos (ou apenas naqueles especificados por você) que são executados no Windows 10. A Microsoft Store fornece várias maneiras de ganhar dinheiro com seu aplicativo.
- Pode ser instalado e desinstalado sem risco ao computador nem de causar "danos ao computador".
- É interessante: use blocos dinâmicos, notificações por push e atividades do usuário que interagem com o recurso de "Continuar do ponto em que parei" da Cortana e a Linha do Tempo do Windows para despertar o interesse dos usuários.
- É programável em C#, C++, Visual Basic e Javascript. Para a interface do usuário, use XAML, HTML ou DirectX.

Vamos analisar isso em mais detalhes.

## <a name="secure"></a>Seguro

Em seu manifesto, os aplicativos UWP declaram as funcionalidades do dispositivo de que precisam, como acesso a microfone, localização, webcam, dispositivos USB, arquivos e assim por diante. O usuário deve reconhecer e autorizar esse acesso antes que o aplicativo receba a funcionalidade.

## <a name="a-common-api-surface-across-all-devices"></a>Uma superfície de API comum entre todos os dispositivos

O Windows 10 apresenta a UWP (Plataforma Universal do Windows), que fornece uma plataforma de aplicativo comum em cada dispositivo que executa o Windows 10. As APIs básicas da UWP são as mesmas em todos os dispositivos Windows. Se o aplicativo usar apenas as APIs básicas, ele será executado em qualquer dispositivo Windows 10, independentemente de você estar direcionando um computador desktop, Xbox, headset de realidade misturada e assim por diante.

Um aplicativo UWP escrito em C++/WinRT ou C++/CX tem acesso às APIs do Win32 que fazem parte da UWP. Essas APIs do Win32 são implementadas por todos os dispositivos Windows 10.

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Os SDKs de extensão expõem as funcionalidades exclusivas de tipos de dispositivos específicos

Se você estiver direcionado as APIs universais, o aplicativo poderá ser executado em todos os dispositivos que executam o Windows 10. No entanto, é possível que seu aplicativo UWP usufrua das APIs específicas do dispositivo.

Os SDKs de extensão permitem que você chame as APIs especializadas para diferentes dispositivos. Por exemplo, se o aplicativo UWP direcionar um dispositivo IoT, você poderá adicionar o SDK de extensão de IoT a seu projeto para direcionar recursos específicos para os dispositivos IoT. Para obter mais informações sobre como adicionar os SDKs de extensão, confira a seção **SDKs de extensão** em [Visão geral das famílias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks).

Você pode escrever o aplicativo para que seja executado somente em um tipo específico de dispositivo e depois limitar sua distribuição no Microsoft Store a apenas esse tipo de dispositivo. Outra opção é testar condicionalmente a presença de uma API em runtime e adaptar o comportamento do aplicativo de acordo. Para obter mais informações, confira a seção **Escrever código** em [Visão geral das famílias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code).<br>

O vídeo a seguir apresenta uma breve visão geral das famílias de dispositivos e codificação adaptável:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>Entrada e controles adaptáveis

Os elementos de interface do usuário respondem ao tamanho e à resolução em DPI da tela em que o aplicativo está sendo executado, ajustando seu layout e escala. Os aplicativos UWP funcionam bem com vários tipos de entrada, como teclado, mouse, toque, caneta e controladores do Xbox One. Caso você precise personalizar mais sua interface do usuário para um tamanho de tela ou dispositivo específico, novos painéis de layout e ferramentas ajudarão a criar a interface do usuário que se adapte aos diferentes dispositivos e fatores forma nos quais os aplicativos podem ser executados.

![Dispositivos Windows](images/1894834-hig-device-primer-01-500.png)

O Windows ajuda a direcionar sua interface do usuário para vários dispositivos com os seguintes recursos:

- Controles universais e painéis de layout ajudarão a otimizar sua interface do usuário para a resolução de tela do dispositivo. Por exemplo, controles como botões e controles deslizantes adaptam-se automaticamente ao tamanho da tela do dispositivo e à densidade do DPI. Os painéis de layout ajudam a ajustar o layout do conteúdo com base no tamanho da tela. O dimensionamento adaptável ajusta a resolução e as diferenças de DPI entre dispositivos.
- A manipulação de entrada comum permite que você receba a entrada por toque, caneta, mouse, teclado ou por um controlador, como um controlador do Xbox da Microsoft.
- As ferramentas que ajudam você a criar a interface do usuário que pode se adaptar a diferentes resoluções de tela.

Alguns aspectos da interface do usuário do seu aplicativo se adaptará automaticamente em todos os dispositivos. O projeto de experiência do usuário do seu aplicativo, no entanto, talvez seja necessário adaptar-se dependendo do dispositivo que o aplicativo está sendo executado. Por exemplo, um aplicativo de fotos pode se adaptar a sua interface do usuário quando executado em um dispositivo pequeno e portátil para garantir que esse uso seja ideal para uso com uma única mão. Quando um aplicativo de fotos é executado em um computador desktop, a interface do usuário deve se adaptar para aproveitar ao máximo o espaço adicional na tela.

## <a name="theres-one-store-for-all-devices"></a>Existe uma loja para todos os dispositivos

Uma loja unificada de aplicativos disponibilizará seu aplicativo nos dispositivos Windows 10, como computador, tablet, Xbox, HoloLens, Surface Hub e dispositivos IoT (Internet das Coisas). Você pode enviar seu aplicativo para a loja e disponibilizá-lo em todos os tipos de dispositivos ou apenas os escolhidos. Você envia e gerencia todos os aplicativos para dispositivos Windows em um só lugar. Você tem um aplicativo da área de trabalho do C++ que deseja modernizar com recursos UWP e vender na Microsoft Store? Tudo bem.

Os aplicativos UWP integram-se ao [Application Insights](https://azure.microsoft.com/services/application-insights/) para telemetria detalhada e análise, uma ferramenta crucial para entender os usuários e melhorar os aplicativos.

### <a name="monetize-your-app"></a>Monetizar seu aplicativo

Você pode escolher como vai monetizar seu aplicativo. Há várias formas de ganhar dinheiro com seu aplicativo. Basta escolher a que funciona melhor para você, por exemplo:

- Um download pago é a opção mais simples. Você só precisa definir o preço.
- As avaliações permitem que os usuários experimentem o aplicativo antes de comprá-lo, proporcionando descoberta e conversão mais fáceis do que as opções "freemium" mais tradicionais.
- Preços de venda para incentivar os usuários.
- Compras no aplicativo e anúncios também estão disponíveis.

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>Os aplicativos da Microsoft Store fornecem uma perfeita experiência de instalação, desinstalação e atualização

Todos os aplicativos UWP são distribuídos por meio de um sistema de empacotamento que protege o usuário, o dispositivo e o sistema. Os usuários nunca vão se arrepender de instalarem um aplicativo, pois os aplicativos UWP podem ser desinstalados sem deixar nada para trás, exceto pelos documentos criados com o aplicativo.

Os aplicativos podem ser implantados e atualizados perfeitamente. O empacotamento de aplicativo pode ser modularizado de modo que seja possível baixar o conteúdo e as extensões sob demanda.

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Forneça informações importantes em tempo real aos usuários para que eles voltem

Há várias formas de manter os usuários interessados no seu aplicativo UWP:

- Os blocos dinâmicos e os blocos da tela de bloqueio mostram com rapidez informações contextualmente relevantes e oportunas sobre o aplicativo.
- Notificações por push que apresentam alertas em tempo real para chamar a atenção do seu usuário.
- As atividades do usuário permitem que os usuários recomecem do ponto em que pararam no aplicativo, mesmo em dispositivos diferentes.
- A Central de Ações organiza as notificações de seu aplicativo.
- Execução em segundo plano e gatilhos colocam seu aplicativo em ação quando os usuários precisam dele.
- O aplicativo pode usar dispositivos de voz e Bluetooth LE para ajudar os usuários a interagir com o mundo ao redor.
- Integre a Cortana para adicionar a funcionalidade de comando de voz a seu aplicativo.

##  <a name="use-a-language-you-already-know"></a>Use uma linguagem que você já conhece

Os aplicativos UWP usam o Windows Runtime, a API nativa fornecida pelo sistema operacional. Essa API é implementada em C++ e tem suporte em C#, Visual Basic, C++ e JavaScript. Algumas opções para criar aplicativos UWP incluem:

- Interface do usuário XAML e C#, VB ou C++
- Interface do usuário do DirectX e C++
- JavaScript e HTML

## <a name="links-to-help-you-get-going"></a>Links para ajudá-lo a começar

### <a name="get-set-up"></a>Prepare-se para começar

Confira [Preparar-se](get-set-up.md) para baixar as ferramentas necessárias para começar a criar aplicativos e, em seguida, [escreva seu primeiro aplicativo](your-first-app.md).

### <a name="design-your-app"></a>Projetar seu aplicativo

O sistema de design da Microsoft chama-se Fluente. O Sistema de Design Fluente é um conjunto de recursos UWP combinado a práticas recomendadas para criação de aplicativos que funcionam perfeitamente em todos os tipos de dispositivos com Windows. As experiências do Fluente adaptam-se a e funcionam normalmente em dispositivos de tablets a notebooks e de computadores a televisores, além de em dispositivos de realidade virtual. Confira [O Sistema de Design Fluente para aplicativos UWP](https://docs.microsoft.com/windows/uwp/design/fluent-design-system) para obter uma introdução ao Design Fluente.

Um bom [projeto](http://design.windows.com/) é o processo de decisão sobre como os usuários vão interagir com seu aplicativo, como será sua aparência e como ele funcionará. A experiência do usuário desempenha uma função enorme em determinar o quão feliz as pessoas serão com seu aplicativo, portanto não economize nesta etapa. As [Noções básicas de design](https://developer.microsoft.com/en-us/windows/apps/design) apresentam o design de um aplicativo Universal do Windows. Consulte a [Introdução aos aplicativos da Plataforma Universal do Windows (UWP) para os designers](https://docs.microsoft.com/windows/uwp/layout/design-and-ui-intro) para obter informações sobre como projetar aplicativos UWP que encantam seus usuários. Antes de começar a codificar, consulte a [cartilha de dispositivos](../design/devices/index.md) para ajudá-lo a pensar sobre a experiência de interação quanto ao uso do seu aplicativo em todos os fatores forma diferentes de destino.

Além da interação em diferentes dispositivos, [planeje seu aplicativo](https://docs.microsoft.com/windows/uwp/get-started/plan-your-app) para adotar os benefícios de trabalhar em vários dispositivos. Por exemplo:

- Projete o fluxo de trabalho usando [Noções básicas de design de navegação para aplicativos UWP](https://docs.microsoft.com/windows/uwp/layout/navigation-basics) para acomodar dispositivos móveis com tela pequena e grande. [Dispor a interface do usuário](https://docs.microsoft.com/windows/uwp/layout/screen-sizes-and-breakpoints-for-responsive-design) para responder a diferentes tamanhos de tela e resoluções.

- Considere como você vai acomodar vários tipos de entrada. Consulte as [Diretrizes para interações](https://docs.microsoft.com/windows/uwp/design/layout/index) para saber como os usuários podem interagir com seu aplicativo usando [Cortana](https://docs.microsoft.com/windows/uwp/input-and-devices/cortana-design-guidelines), [Controle por voz](https://docs.microsoft.com/windows/uwp/input-and-devices/speech-interactions), [Interações por toque](https://docs.microsoft.com/windows/uwp/input-and-devices/guidelines-for-user-interaction), o [Teclado virtual](https://docs.microsoft.com/windows/uwp/input-and-devices/keyboard-interactions) e muito mais.  Ou confira as [Diretrizes de texto e entrada de texto](https://docs.microsoft.com/windows/uwp/controls-and-patterns/text-controls) para obter mais experiências de interação tradicionais.

### <a name="add-services"></a>Adicionar serviços

- Use [serviços de nuvem](https://azure.microsoft.com/documentation/services/cloud-services) para sincronização entre dispositivos.
- Saiba como [conectar-se a serviços Web](https://docs.microsoft.com/previous-versions/windows/apps/hh761504(v=win.10)) para melhorar a experiência do aplicativo.
- Inclua [Notificações por push](https://docs.microsoft.com/windows/uwp/controls-and-patterns/tiles-and-notifications-windows-push-notification-services--wns--overview) e [compras no aplicativo](https://docs.microsoft.com/windows/uwp/monetize/enable-in-app-product-purchases) em seu planejamento. Esses recursos devem funcionar em todos os dispositivos.

### <a name="submit-your-app-to-the-store"></a>Envie seu aplicativo para a Loja.

O [Partner Center](https://partner.microsoft.com/dashboard) permite gerenciar e enviar todos os seus aplicativos para dispositivos Windows em um único local. Confira [Publicar aplicativos e jogos do Windows](../publish/index.md) para aprender a enviar seus aplicativos para publicação na Microsoft Store.

Os novos recursos simplificam os processos, dando mais controle para você. Você também encontrará [relatórios de análise](https://docs.microsoft.com/windows/uwp/publish/analytics) detalhados, [detalhes de pagamento](https://docs.microsoft.com/windows/uwp/publish/payout-summary) combinados, formas de [promover seu aplicativo e atrair os seus clientes](https://docs.microsoft.com/windows/uwp/publish/app-promotion-and-customer-engagement), e muito mais.

Para mais material introdutório, confira [Uma Introdução sobre como criar aplicativos para dispositivos Windows 10](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>Tópicos mais avançados

- Saiba como usar [Atividades do usuário](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) para que a atividade do usuário no aplicativo apareça no recurso Continuar do ponto em que parei da Cortana e na Linha do Tempo do Windows.
- Saiba como usar [Blocos, selos e notificações para aplicativos UWP](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/).
- Para obter a lista completa de APIs do Win32 disponíveis para aplicativos UWP, consulte [Conjuntos de APIs para aplicativos UWP](https://docs.microsoft.com/previous-versions/mt186421(v=vs.85)) e [Dlls para aplicativos UWP](https://docs.microsoft.com/previous-versions/mt186422(v=vs.85)).
- Confira [Aplicativos Universais do Windows em .NET](https://devblogs.microsoft.com/dotnet/universal-windows-apps-in-net/) para obter uma visão geral de como escrever aplicativos UWP em .NET.
- Para obter uma lista de tipos .NET que você pode usar em um aplicativo UWP, confira [.NET para aplicativos UWP](https://docs.microsoft.com/dotnet/api/index?view=dotnet-uwp-10.0)
- [Compilar aplicativos com .NET nativo](https://docs.microsoft.com/dotnet/framework/net-native/)
- Saiba como adicionar experiências modernas para usuários do Windows 10 ao aplicativo da área de trabalho existente e distribuí-lo na Microsoft Store com a [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Como a plataforma Universal do Windows se relaciona com APIs do Windows Runtime
Se você estiver criando um aplicativo da UWP (Plataforma Universal do Windows), poderá obter muita vantagem e conveniência por tratar os termos "UWP (Plataforma Universal do Windows)" e "WinRT (Windows Runtime)" como mais ou menos sinônimos. Porém, *é* possível examinar os bastidores da tecnologia e determinar qual exatamente é a diferença entre essas ideias. Se você está curioso quanto a isso, esta última seção é para você.

O Windows Runtime e APIs do WinRT são uma evolução das APIs do Windows. Originalmente, o Windows foi programado por meio de APIs Win32 em etilo C simples. A elas, foram adicionadas APIs COM (sendo [DirectX](https://docs.microsoft.com/windows/desktop/directx) um exemplo de destaque). Windows Forms, WPF, .NET e linguagens gerenciadas trouxeram sua própria maneira de escrever aplicativos do Windows e sua própria variante de tecnologia de API. O Windows Runtime é, nos bastidores, o próximo estágio do COM. Na camada ABI (interface binária de aplicativo) propriamente dita, suas raízes em COM tornam-se visíveis. Mas o Windows Runtime foi projetado para poder ser chamado de uma gama maior de diferentes linguagens de programação. E pode ser chamado de uma maneira muito natural para cada uma dessas linguagens. Para esse fim, o acesso ao Windows Runtime é disponibilizado por meio das chamadas projeções de linguagem. Há uma projeção de linguagem do Windows Runtime para C#, Visual Basic, C++ padrão, JavaScript e assim por diante. Além disso, uma vez empacotado corretamente (confira [Ponte de Desktop](/windows/uwp/porting/desktop-to-uwp-root)), você pode chamar APIs do WinRT de um aplicativo criado em um de uma ampla gama de modelos de aplicativos: Win32, .NET, WinForms e WPF.

E, claro, você pode chamar APIs do WinRT do seu aplicativo da UWP. UWP é um modelo de aplicativo criado com base no Windows Runtime. Tecnicamente, o modelo de aplicativo UWP baseia-se em [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), embora esse detalhe possa estar oculto de você, dependendo de sua escolha de linguagem de programação. Como este tópico explicou, de um ponto de vista de proposta de valor, a UWP é útil para escrever um único binário que pode, se você quiser, ser publicado na Microsoft Store e executado em uma grande variedade de fatores forma de dispositivo. O alcance de dispositivo de seu aplicativo UWP depende do subconjunto de APIs de UWP que você limita o seu aplicativo a chamar ou que você chama condicionalmente.

Espero que esta seção tenha sido bem-sucedida em descrever a diferença entre a tecnologia subjacente de APIs do Windows Runtime e o mecanismo e o valor comercial da plataforma Universal do Windows.
