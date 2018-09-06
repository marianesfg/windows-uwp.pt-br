---
author: TylerMSFT
title: O que é um aplicativo da Plataforma Universal do Windows (UWP)?
description: Saiba mais sobre os aplicativos UWP (Plataforma Universal do Windows) que podem ser executados em uma ampla variedade de dispositivos nos quais o Windows 10 esteja em execução.
ms.assetid: 59849197-B5C7-493C-8581-ADD6F5F8800B
ms.author: twhitney
ms.date: 5/7/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, universal
ms.localizationpriority: medium
ms.openlocfilehash: 7f0168f0a1baef5e68bccdf0a33c3ac7eb7683a7
ms.sourcegitcommit: 914b38559852aaefe7e9468f6f53a7465bf36e30
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/06/2018
ms.locfileid: "3394444"
---
# <a name="whats-a-universal-windows-platform-uwp-app"></a>O que é um aplicativo da Plataforma Universal do Windows (UWP)?

![Os aplicativos da Plataforma Universal do Windows são executados em uma variedade de dispositivos, são compatíveis com interface do usuário adaptável, entrada do usuário natural, um repositório, um centro de desenvolvimento e serviços de nuvem](images/universalapps-overview.png)

Um aplicativo UWP:

- É seguro: os aplicativos UWP declaram quais dados e recursos de dispositivo eles acessam. O usuário deve autorizar esse acesso.
- Pode usar uma API comum em todos os dispositivos que executam o Windows 10.
- Pode usar funcionalidades específicas do dispositivo e adaptar a interface do usuário a diferentes tamanhos, resoluções e DPI de tela de dispositivo.
- Está disponível na Microsoft Store em todos os dispositivos (ou apenas naqueles especificados por você) que são executados no Windows 10. A Microsoft Store fornece várias maneiras de ganhar dinheiro com seu aplicativo.
- Pode ser instalado e não instalado sem risco de que o computador seja danificado.
- É envolvente: use blocos dinâmicos, notificações por push e atividades do usuário que interagem com o Continue de onde você parou do Linha do Tempo do Windows e da Cortana para envolver os usuários.
- É programável em C#, C++, Visual Basic e Javascript. Para a interface do usuário, use XAML, HTML ou DirectX.

Vamos dar uma olhada nesses casos de forma mais detalhada.

## <a name="secure"></a>Seguro

Em seu manifesto, os aplicativos UWP declaram as funcionalidades do dispositivo de que precisam, como acesso a microfone, localização, webcam, dispositivos USB, arquivos e assim por diante. O usuário deve confirmar e autorizar esse acesso antes que o aplicativo receba a funcionalidade.

## <a name="a-common-api-surface-across-all-devices"></a>Uma superfície de API comum entre todos os dispositivos

O Windows 10 apresenta a UWP (Plataforma Universal do Windows), que fornece uma plataforma de aplicativo comum em cada dispositivo que executa o Windows 10. As APIs básicas da UWP são as mesas em todos os dispositivos Windows. Se o aplicativo usa apenas as APIs básicas, ele será executado em qualquer dispositivo Windows 10, independentemente de você estar direcionando um computador desktop, Xbox, fone de ouvido de realidade mista e assim por diante.

Um aplicativo UWP escrito em C++/WinRT ou C++/CX tem acesso às APIs do Win32 que fazem parte da UWP. Essas APIs do Win32 são implementadas por todos os dispositivos Windows 10.

## <a name="extension-sdks-expose-the-unique-capabilities-of-specific-device-types"></a>Os SDKs de extensão expõem as únicas funcionalidades de tipos de dispositivos específicos

Se você estiver direcionado as APIs universais, o aplicativo pode funcionar em todos os dispositivos que executam o Windows 10. No entanto, é possível que seu aplicativo UWP aproveite as APIs específicas do dispositivo.

Os SDKs de extensão permitem que você chame as APIs especializadas para diferentes dispositivos. Por exemplo, se o aplicativo UWP direcionar um dispositivo IoT, você pode adicionar o SDK de extensão de IoT a seu projeto para direcionar recursos específicos para os dispositivos IoT. Para obter mais informações sobre como adicionar os SDKs de extensão, consulte a seção **SDKs de extensão** em [Visão geral das famílias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#extension-sdks).

Você pode escrever o aplicativo para que seja executado somente em um tipo específico de dispositivo e depois limitar sua distribuição no Microsoft Store apenas a esse tipo de dispositivo. Ou você pode testar condicionalmente a presença de uma API em tempo de execução e adaptar o comportamento do aplicativo de acordo. Para obter mais informações, consulte a seção **Escrevendo código** em [Visão geral das famílias de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview#writing-code).<br>

O vídeo a seguir fornece uma breve visão geral das famílias de dispositivos e codificação adaptativa:
<iframe src="https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-UWP-and-Device-Families/player" width="640" height="360" allowFullScreen frameBorder="0"></iframe>

## <a name="adaptive-controls-and-input"></a>Controles adaptáveis e entrada

Os elementos de interface do usuário respondem ao tamanho e DPI da tela em que o aplicativo está sendo executado, ajustando seu layout e escala. Os aplicativos UWP funcionam bem com vários tipos de entrada, como teclado, mouse, toque, caneta e controladores do Xbox One. Caso você precise personalizar mais sua interface do usuário para um tamanho de tela ou dispositivo específico, novos painéis de layout e ferramentas ajudam a criar a interface do usuário que pode se adaptar aos diferentes dispositivos e fatores forma nos quais os aplicativos podem ser executados.

![Dispositivos Windows](images/1894834-hig-device-primer-01-500.png)

O Windows ajuda a direcionar sua interface do usuário para vários dispositivos com os seguintes recursos:

- Controles universais e painéis de layout ajudarão a otimizar a interface do usuário para a resolução de tela do dispositivo. Por exemplo, controles como botões e controles deslizantes adaptam-se automaticamente ao tamanho da tela do dispositivo e à densidade do DPI. Os painéis de layout ajudam a ajustar o layout do conteúdo com base no tamanho da tela. O dimensionamento adaptável ajusta a resolução e as diferenças de DPI entre dispositivos.
- A manipulação de entrada comum permite que você receba a entrada por toque, caneta, mouse, teclado ou controle, como um controle Xbox da Microsoft.
- As ferramentas que ajudam você a criar a interface do usuário que pode se adaptar a diferentes resoluções de tela.

Alguns aspectos da interface do usuário do seu aplicativo se adaptará automaticamente em todos os dispositivos. No entanto, o design de experiência do usuário do aplicativo talvez precise se adaptar dependendo do dispositivo em que o app está sendo executado. Por exemplo, um aplicativo de fotos pode se adaptar a sua interface do usuário quando executado em um dispositivo pequeno e portátil para garantir que esse uso seja ideal para uso com uma única mão. Quando um app de fotos é executado em um computador desktop, a interface do usuário deve se adaptar para aproveitar ao máximo o espaço adicional na tela.

## <a name="theres-one-store-for-all-devices"></a>Existe uma loja para todos os dispositivos

Um armazenamento unificado de aplicativos disponibilizará seu aplicativo nos dispositivos Windows 10, como computador, tablet, Xbox, HoloLens, Surface Hub e dispositivos IoT (Internet das Coisas). Você poderá enviar seu aplicativo para a loja e disponibilizá-lo em todas os tipos de dispositivos ou apenas os escolhidos. Você envia e gerencia todos os aplicativos para dispositivos Windows em um só lugar. Você tem um aplicativo de área de trabalho do C++ que deseja modernizar com recursos UWP e vender na Microsoft Store? Tudo bem.

Os aplicativos UWP integram-se a [Application Insights](http://azure.microsoft.com/services/application-insights/) para telemetria detalhada e análise, uma ferramenta crucial para entender os usuários e melhorar os aplicativos.

### <a name="monetize-your-app"></a>Monetizar seu aplicativo

Você pode escolher como vai monetizar seu aplicativo. Há várias formas de ganhar dinheiro com seu aplicativo. Você só precisa escolher o que faz sentido para o seu aplicativo, por exemplo:

- Um download pago é a opção mais simples. Só precisa dizer o preço.
- As avaliações permitem que os usuários experimentem o aplicativo antes de comprá-lo, proporcionando uma descoberta e uma conversão mais fáceis do que as opções "freemium" mais tradicionais.
- Preços de venda para incentivar os usuários.
- Compras no aplicativo e anúncios também estão disponíveis.

### <a name="apps-from-the-microsoft-store-provide-a-seamless-install-uninstall-and-upgrade-experience"></a>Os aplicativos da Microsoft Store fornecem uma perfeita experiência de instalação, desinstalação e atualização

Todos os aplicativos UWP são distribuídos por meio de um sistema de empacotamento que protege o usuário, o dispositivo e o sistema. Os usuários jamais precisarão se arrepender da instalação de um aplicativo porque os aplicativos UWP podem ser desinstalados sem deixar qualquer coisa para trás, exceto documentos criados com o aplicativo.

Os aplicativos podem ser implantados e atualizados perfeitamente. O empacotamento de aplicativo pode ser modularizado de modo que seja possível baixar o conteúdo e as extensões sob demanda.

## <a name="deliver-relevant-real-time-info-to-your-users-to-keep-them-coming-back"></a>Fornecer informações importantes em tempo real aos usuários para fazê-los voltar

Há várias formas de manter os usuários interessados no seu aplicativo UWP:

- Os blocos dinâmicos e os blocos da tela de bloqueio mostram informações contextualmente relevantes e oportunas sobre o aplicativo rapidamente.
- Notificações por push que apresentam alertas em tempo real para chamar a atenção do seu usuário.
- As atividades do usuário permitem que os usuários recomecem de onde pararam no aplicativo, mesmo nos dispositivos.
- A central de ações organiza notificações do aplicativo.
- Execução em plano de fundo e gatilhos colocam seu aplicativo em ação quando os usuários precisam.
- O aplicativo pode usar dispositivos de voz e Bluetooth LE para ajudar os usuários a interagir com o mundo ao redor.
- Integre a Cortana para adicionar a funcionalidade de comando de voz a seu aplicativo.

##  <a name="use-a-language-you-already-know"></a>Use uma linguagem que você já conhece

Os aplicativos UWP usam o Windows Runtime, a API nativa fornecida pelo sistema operacional. Essa API é implementado em C++ e tem suporte em C#, Visual Basic, C++ e JavaScript. Algumas opções para criar aplicativos UWP incluem:

- Interface do usuário XAML e C#, VB ou C++
- Interface do usuário do DirectX e C++
- JavaScript e HTML

## <a name="links-to-help-you-get-going"></a>Links para ajudá-lo a começar

### <a name="get-set-up"></a>Prepare-se

Confira [Prepare-se](get-set-up.md) a fim de baixar as ferramentas necessárias para começar a criar aplicativos e, em seguida, [crie seu primeiro aplicativo](your-first-app.md).

### <a name="design-your-app"></a>Projete seu aplicativo

O sistema de design da Microsoft chama-se Fluent. O Sistema de Design Fluente é um conjunto de recursos UWP combinado a práticas recomendadas para criação de aplicativos que funcionam perfeitamente em todos os tipos de dispositivos com Windows. As experiências do Fluente adaptam-se a e funcionam normalmente em dispositivos, de tablets a notebooks, de computadores a televisores, e em dispositivos de realidade virtual. Consulte [O Sistema de Design Fluente para aplicativos UWP](https://docs.microsoft.com/windows/uwp/design/fluent-design-system) para obter uma introdução ao Design Fluent.

Um bom [projeto](http://go.microsoft.com/fwlink/?LinkId=258848) é o processo de decisão sobre como os usuários vão interagir com seu aplicativo, bem como será sua aparência e como vai funcionar. A experiência do usuário desempenha uma função enorme em determinar o quão feliz as pessoas serão com seu aplicativo, portanto não economize nesta etapa. [Noções básicas de design](https://dev.windows.com/design) apresenta você ao design de um aplicativo Universal do Windows. Consulte a [Introdução aos aplicativos da Plataforma Universal do Windows (UWP) para os designers](https://msdn.microsoft.com/library/windows/apps/dn958439) para obter informações sobre como projetar aplicativos UWP que encantam seus usuários. Antes de começar a codificar, consulte a [cartilha de dispositivos](../design/devices/index.md) para ajudá-lo a pensar sobre a experiência de interação quanto ao uso do seu aplicativo em todos os fatores forma diferentes de destino.

Além da interação em diferentes dispositivos, [planeje seu aplicativo](https://msdn.microsoft.com/library/windows/apps/hh465427) para adotar os benefícios de trabalhar em vários dispositivos. Por exemplo:

- Projete o fluxo de trabalho usando [Noções básicas de design de navegação para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn958438) para acomodar dispositivos móveis com tela pequena e grande. [Dispor a interface do usuário](https://msdn.microsoft.com/library/windows/apps/dn958435) para responder a diferentes tamanhos de tela e resoluções.

- Considere como você vai acomodar vários tipos de entrada. Consulte as [Diretrizes para interações](https://msdn.microsoft.com/library/windows/apps/dn611861) para saber como os usuários podem interagir com seu aplicativo usando [Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), [Controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121), [Interações por toque](https://msdn.microsoft.com/library/windows/apps/hh465370), o [Teclado virtual](https://msdn.microsoft.com/library/windows/apps/hh972345) e muito mais.  Ou consulte as [Diretrizes de texto e entrada de texto](https://msdn.microsoft.com/library/windows/apps/dn611864) para obter mais experiências de interação tradicionais.

### <a name="add-services"></a>Adicionar serviços

- Use [serviços de nuvem](http://go.microsoft.com/fwlink/?LinkId=526377) para sincronização entre dispositivos.
- Saiba como [se conectar a serviços Web](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504) para melhorar a experiência do aplicativo.
- Saiba como [Adicionar a Cortana a seu aplicativo](https://mva.microsoft.com/training-courses/integrating-cortana-in-your-apps-8487?l=20D3s5Xz_5904984382) para que seu aplicativo possa responder a comandos de voz.
- Inclua [Notificações por push](https://msdn.microsoft.com/library/windows/apps/mt187203) e [compras no aplicativo](https://msdn.microsoft.com/library/windows/apps/mt219684) em seu planejamento. Esses recursos devem funcionar em todos os dispositivos.

### <a name="submit-your-app-to-the-store"></a>Enviar seu aplicativo para a loja

O novo painel unificado do Centro de Desenvolvimento do Windows permite que você gerencie e envie todos os seus aplicativos para dispositivos Windows em um local. Veja [Usando o painel do Centro de Desenvolvimento do Windows unificado](../publish/using-the-windows-dev-center-dashboard.md) para aprender a enviar seus aplicativos para publicação na Microsoft Store.

Os novos recursos simplificam os processos, dando mais controle para você. Você também encontrará [relatórios de análise](https://msdn.microsoft.com/library/windows/apps/mt148522) detalhados, [detalhes de pagamento](https://msdn.microsoft.com/library/windows/apps/dn986925) combinados, formas de [promover seu aplicativo e atrair os seus clientes](https://msdn.microsoft.com/library/windows/apps/mt148526), e muito mais.

Para mais material introdutório, consulte [Uma Introdução sobre Como Compilar Aplicativos para Dispositivos Windows 10](https://msdn.microsoft.com/magazine/dn973012.aspx)

### <a name="more-advanced-topics"></a>Tópicos mais avançados

- Saiba como usar [Atividades do usuário](https://blogs.windows.com/buildingapps/2017/12/19/application-engagement-windows-timeline-user-activities/#tHuZ6tLPtCXqYKvw.97) para que a atividade do usuário no aplicativo apareça no recurso Continue de onde você parou do Linha do Tempo do Windows e da Cortana.
- Saiba como usar [Blocos, selos e notificações para aplicativos UWP](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/).
- Para obter a lista completa de APIs do Win32 disponíveis para aplicativos UWP, consulte [Conjuntos de APIs para aplicativos UWP](https://msdn.microsoft.com/library/windows/desktop/mt186421) e [Dlls para aplicativos UWP](https://msdn.microsoft.com/library/windows/desktop/mt186422).
- Consulte [Aplicativos Universais do Windows em .NET](https://blogs.msdn.microsoft.com/dotnet/2015/07/30/universal-windows-apps-in-net) para obter uma visão geral de criar aplicativos UWP em .NET.
- Para obter uma lista de tipos .NET que você pode usar em um aplicativo UWP, consulte [.NET para aplicativos UWP](https://msdn.microsoft.com/library/mt185501.aspx)
- [.NET Native - o que significa para os desenvolvedores da Plataforma Universal do Windows (UWP)](https://blogs.windows.com/buildingapps/2015/08/20/net-native-what-it-means-for-universal-windows-platform-uwp-developers/#TYsD3tJuBJpK3Hc7.97)
- Saiba como adicionar experiências modernas para usuários do Windows 10 ao aplicativo da área de trabalho existente e distribuí-lo na Microsoft Store com a [Ponte de Desktop](https://developer.microsoft.com/windows/bridges/desktop).

## <a name="how-the-universal-windows-platform-relates-to-windows-runtime-apis"></a>Como a plataforma universal do Windows se relaciona com APIs do Windows Runtime
Se você estiver criando um aplicativo Universal Windows Platform (UWP), você pode obter muitos quilometragem e conveniência fora tratar os termos "Universal Windows Platform (UWP)" e "Windows Runtime (WinRT)" como sinônimos mais ou menos. Mas *é* possível examinar nos bastidores da tecnologia e determinar apenas qual é a diferença entre essas ideias. Se você estiver curioso que, em seguida, essa última seção é para você.

O tempo de execução do Windows e APIs do WinRT, são uma evolução das APIs do Windows. Originalmente, o Windows foi programados por meio simples, as APIs do Win32 estilo C. Para aqueles foram adicionados COM APIs ([DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) sendo um exemplo proeminente). Windows Forms, WPF, .NET e linguagens gerenciadas trazido sua própria maneira de escrever aplicativos do Windows e seu próprio tipo de tecnologia de API. O Windows Runtime é, nos bastidores, o próximo estágio de inicialização de COM. Na camada de interface binária (ABI) do aplicativo real, suas raízes no COM ficam visíveis. Mas o tempo de execução do Windows foi projetado para ser pode ser chamada em uma grande variedade de linguagens de programação diferentes. E pode ser chamada de maneira que seja muito natural para cada um desses idiomas. Com essa finalidade, acesso ao tempo de execução do Windows é disponibilizado por meio do que é conhecido como projeções de linguagem. Há uma projeção de linguagem de tempo de execução do Windows em c#, no Visual Basic, em C++ padrão, em JavaScript e assim por diante. Além disso, uma vez empacotados adequadamente (consulte [Ponte de Desktop](/windows/uwp/porting/desktop-to-uwp-root)), você pode chamar APIs do WinRT de um aplicativo compilado em um de uma grande variedade de modelos de aplicativo: WPF, WinForms, .NET e Win32.

E, claro, você pode chamar APIs do WinRT do seu aplicativo UWP. UWP é um modelo de aplicativo criado com o Windows Runtime base. Tecnicamente, o modelo de aplicativo UWP é com base em [CoreApplication](/uwp/api/windows.applicationmodel.core.coreapplication), embora detalhes podem estar oculta de você, dependendo de sua escolha de linguagem de programação. Como este tópico foi explicado, de um valor proposta ponto de Vista, a UWP se compromete a escrever um único binário que pode, você deve escolher, ser publicado na Microsoft Store e executado em qualquer um de uma grande variedade de fatores forma de dispositivo. O alcance do dispositivo do seu aplicativo UWP depende do subconjunto de APIs de UWP que você limite o seu aplicativo para chamar ou que você chamar condicionalmente.

Felizmente, esta seção foi bem-sucedida na descrevendo a diferença entre a tecnologia subjacente APIs do Windows Runtime e o mecanismo e o valor de negócios da plataforma universal do Windows.
