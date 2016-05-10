---
ms.assetid: C9787269-B54F-4FFA-A884-D4A3BF28F80D
title: O que é um aplicativo da Plataforma Universal do Windows (UWP)?
description: Saiba mais sobre os diferentes tipos de aplicativos que chamamos de Aplicativos Universais do Windows - aplicativos da Windows Store, aplicativos da Loja do Windows Phone e aplicativos do Windows Runtime.
---

# O que é um aplicativo da Plataforma Universal do Windows (UWP)?

Um aplicativo da Plataforma Universal do Windows (UWP) é uma experiência do Windows que se baseia na Plataforma Universal do Windows (UWP), que foi introduzida no Windows 8 como Windows Runtime. Na essência dos aplicativos UWP está a ideia de que os usuários querem que suas *experiências* sejam móveis em todos os seus dispositivos, e querem usar o dispositivo que for mais conveniente ou produtivo para a tarefa em questão.

O Windows 10 facilita ainda mais o desenvolvimento de aplicativos para a UWP com apenas um conjunto de APIs, um pacote do aplicativo e uma loja para alcançar todos os dispositivos Windows 10: computadores, tablets, telefones e muito mais. É fácil dar suporte a vários tamanhos de tela e também a vários modelos de interação, seja com toque, com mouse e teclado, com um controlador de jogo ou com uma caneta.

![Dispositivos Windows](images/1894834-hig-device-primer-01-500.png)

##Então o que é um aplicativo UWP?


O que torna um aplicativo UWP especial? Estas são algumas das características que tornam os aplicativos UWP no Windows 10 diferentes.

-   Você segmenta famílias de dispositivos, e não um sistema operacional.

    Uma família de dispositivos identifica as APIs, as características do sistema e os comportamentos esperados entre dispositivos na família de dispositivos. Ela também determina o conjunto de dispositivos nos quais seu aplicativo pode ser instalado na loja.

-   Os aplicativos são empacotados e distribuídos usando-se o formato de empacotamento .AppX.

    Todos os aplicativos UWP são distribuídos como um pacote AppX. Isso oferece um mecanismo de instalação confiável e garante que seus aplicativos possam ser implantados e atualizados de forma integrada.

-   Existe uma loja para todos os dispositivos.

    Depois de se registrar como um desenvolvedor de aplicativos, você poderá enviar seu aplicativo para a loja e disponibilizá-lo em todas as famílias de dispositivos ou apenas as escolhidas. Você envia e gerencia todos os aplicativos para dispositivos Windows em um só lugar.

-   Há uma superfície de API comum entre várias famílias de dispositivos.

    As APIs básicas da Plataforma Universal do Windows (UWP) são iguais para todas as famílias de dispositivos Windows. Se usar apenas as APIs básicas, o aplicativo será executado em qualquer dispositivo Windows 10.

-   Os SDKs de extensão fazem seu aplicativo brilhar em dispositivos especializados.

    Os SDKs de extensão agregam APIs especializadas para cada família de dispositivos. Caso seu aplicativo se destine a uma família de dispositivos específica, você pode fazê-lo brilhar usando essas APIs. Você ainda pode ter um pacote do aplicativo executado em todos os dispositivos verificando em qual família de dispositivos seu aplicativo está em execução antes de chamar uma API de extensão.

-   Controles adaptáveis e entrada

    Os elementos da interface do usuário usam *pixels efetivos* (consulte [Design responsivo 101 para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/Dn958435)), assim eles se adaptam automaticamente com base no número de pixels da tela disponível no dispositivo. E eles funcionam bem com vários tipos de entrada, como teclado, mouse, toque, caneta e controladores do Xbox One. Caso você precise personalizar mais sua interface do usuário para um tamanho de tela ou dispositivo específico, novos painéis de layout e ferramentas ajudam a adaptar a interface do usuário aos dispositivos nos quais seu aplicativo pode ser executado.

Para obter uma visão mais detalhada da UWP, consulte [Guia para Aplicativos da Plataforma Universal do Windows](universal-application-platform-guide.md).

## Use uma linguagem que você já conhece


Você pode criar aplicativos UWP usando as linguagens de programação mais comuns, como C# ou Visual Basic com XAML, JavaScript com HTML ou C++ com DirectX e/ou a linguagem XAML. Também é possível escrever componentes em uma linguagem e usá-los em um aplicativo escrito em outra linguagem.

Os aplicativos UWP usam o Tempo de Execução do Windows, uma API nativa interna do sistema operacional. A API é Implementada em C++ e é compatível com C#, Visual Basic, C++ e JavaScript de uma forma que parece natural para cada linguagem.

O Microsoft Visual Studio 2015 oferece um modelo de aplicativo UWP para cada linguagem que permite criar um único projeto para todos os dispositivos. Quando seu trabalho estiver concluído, você poderá produzir um pacote do aplicativo e enviá-lo à Windows Store diretamente do Visual Studio, para disponibilizar o aplicativo aos clientes em qualquer dispositivo Windows 10.

## Os aplicativos UWP ganham vida no Windows


No Windows, o seu aplicativo pode fornecer informações importantes em tempo real aos usuários, além de fazê-los voltar para saberem mais. Na economia moderna dos aplicativos, seu aplicativo precisa ser atraente para ser importante na vida de seus usuários. O Windows oferece vários recursos para ajudar a que os usuários retornem ao seu aplicativo:

-   Os blocos dinâmicos e a tela de bloqueio mostram informações contextualmente relevantes e oportunas rapidamente.
-   As notificações por push apresentam alertas em tempo real e de última hora para chamar a atenção do seu usuário quando ele precisar.

-   A Central de Ações é o local onde você pode organizar e exibir notificações e conteúdo que exigem ação dos usuários.

-   Execução em plano de fundo e gatilhos dão vida ao seu aplicativo quando os usuários precisam.

-   O aplicativo pode usar dispositivos de voz e Bluetooth LE para ajudar os usuários a interagir com o mundo ao redor.

Finalmente, é possível usar os dados móveis e o Cofre de Credenciais do Windows para proporcionar uma experiência de roaming consistente em todas as telas do Windows onde os usuários executam seu aplicativo. Os dados móveis oferecem uma maneira fácil de armazenar as preferências e configurações de um usuário na nuvem, sem a necessidade de ter de construir a sua própria infraestrutura de sincronização. Além disso, é possível armazenar as credenciais do usuário no Cofre de Credenciais, onde segurança e confiabilidade são as principais prioridades.

##  Rentabilize seu aplicativo à sua maneira


No Windows, é possível rentabilizar o seu aplicativo a sua maneira: em vários telefones, tablets, computadores e outros dispositivos. Oferecemos uma série de formas de ganhar dinheiro com seu aplicativo e os serviços que ele proporciona. Você só precisa escolher o que faz sentido para o seu aplicativo:

-   Um download pago é a opção mais simples. Só precisa dizer sue preço.
-   As avaliações são uma excelente maneira de permitir que os usuários experimentem seu aplicativo antes de comprá-lo, proporcionando uma descoberta e uma conversão mais fáceis do que as opções "freemium" mais tradicionais.
-   A compra no aplicativo oferece o de flexibilidade ao monetizar seu aplicativo.

## Vamos começar


Para obter uma visão mais detalhada da UWP, leia o [Guia para Aplicativos da Plataforma Universal do Windows](universal-application-platform-guide.md). Em seguida, confira [Prepare-se para começar](get-set-up.md) para baixar as ferramentas de que você precisa para iniciar a criação de aplicativos.

## Tópicos relacionados


* [Guia para Aplicativos da Plataforma Universal do Windows](universal-application-platform-guide.md)
* [Preparar-se](get-set-up.md)


<!--HONumber=Mar16_HO1-->


