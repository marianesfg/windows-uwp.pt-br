---
title: Desenvolvimento Android nativo no Windows
description: Comece a desenvolver aplicativos Android nativos no Windows.
author: mattwojo
ms.author: mattwoj
manager: jken
ms.topic: article
keywords: Android, Windows, Android Studio, Visual Studio, jogo Android do c++, Windows Defender, emulador, dispositivo virtual, instalar, Java, Kotlin
ms.date: 04/28/2020
ms.openlocfilehash: 31cdd5124c659b5d35a7e8192db1e87821b891f4
ms.sourcegitcommit: 24b19e7ee06e5bb11a0dae334806741212490ee9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "82255220"
---
# <a name="get-started-with-native-android-development-on-windows"></a>Introdução ao desenvolvimento do Android nativo no Windows

Este guia vai ajudá-lo a começar a usar o Windows para criar aplicativos Android nativos. Se você preferir uma solução de plataforma cruzada, consulte [visão geral do desenvolvimento do Android no Windows](./overview.md) para obter um breve resumo de algumas opções.

A maneira mais direta de criar um aplicativo Android nativo é usar o Android Studio com [Java ou Kotlin](#java-or-kotlin), embora também seja possível [usar C ou C++ para desenvolvimento Android](#use-c-or-c-for-android-game-development) se você tiver uma finalidade específica. As ferramentas SDK Android Studio compilam seu código, dados e arquivos de recurso em um pacote Android de arquivo, arquivo. apk. Um arquivo APK contém todo o conteúdo de um aplicativo Android e é o arquivo que os dispositivos com Android usam para instalar o aplicativo.

## <a name="install-android-studio"></a>Instalar Android Studio

Android Studio é o ambiente de desenvolvimento integrado oficial para o sistema operacional Android do Google. [Baixe a versão mais recente do Android Studio para Windows](https://developer.android.com/studio).

- Se você baixou um arquivo. exe (recomendado), clique duas vezes para iniciá-lo.
- Se você baixou um arquivo. zip, descompacte o ZIP, copie a pasta Android-Studio na pasta arquivos de programas e, em seguida, abra a pasta bin do Android-Studio > e inicie o studio64. exe (para computadores de 64 bits) ou o Studio. exe (para computadores de 32 bits).

Siga o assistente de instalação no Android Studio e instale os pacotes do SDK que ele recomenda. À medida que novas ferramentas e outras APIs estiverem disponíveis, Android Studio o notificará com um pop-up ou verificará se há atualizações selecionando **ajuda** > **para atualização**.

## <a name="create-a-new-project"></a>Criar um novo projeto

Selecione **arquivo** > **novo** > **novo projeto**.

Na janela **escolher seu projeto** , você poderá escolher entre esses modelos:

- **Atividade básica**: cria um aplicativo simples com uma barra de aplicativos, um botão de ação flutuante e dois arquivos de layout: um para a atividade e outro para separar o conteúdo de texto.

- **Atividade vazia**: cria uma atividade vazia e um único arquivo de layout com conteúdo de texto de exemplo.

- **Atividade de navegação inferior**: cria uma barra de navegação inferior padrão para uma atividade. Consulte o [componente de navegação inferior](https://material.io/guidelines/components/bottom-navigation.html) nas diretrizes de design de material do Google.

- Saiba mais sobre como [selecionar um modelo de atividade](https://developer.android.com/studio/projects/templates#SelectTemplate) no Android Studio docs.

Os modelos são normalmente usados para adicionar atividades a módulos de aplicativo novos e existentes. Por exemplo, para criar uma tela de logon para os usuários do seu aplicativo, adicione uma atividade com o [modelo de atividade de logon](https://developer.android.com/studio/projects/templates#LoginActivity).

> [!NOTE]
> O sistema operacional Android é baseado na ideia de **componentes** e usa a **atividade** de termos e a **intenção** de definir interações. Uma **atividade** representa uma única tarefa focada que um usuário pode fazer. Uma **atividade** fornece uma janela para criar a interface do usuário usando classes baseadas na classe **View** . Há um ciclo de vida para **atividades** no sistema operacional Android, definido por um conjunto de seis retornos de `onCreate()`chamada `onStart()`: `onResume()`, `onPause()` `onStop()`,,, `onDestroy()`e. Os componentes de atividade interagem uns com os outros usando objetos de **intenção** . A intenção define a atividade para iniciar ou descreve o tipo de ação a ser executada (e o sistema seleciona a atividade apropriada para você, que pode até mesmo ser de um aplicativo diferente). Saiba mais sobre [atividades](https://developer.android.com/reference/android/app/Activity), o [ciclo de vida da atividade](https://developer.android.com/guide/components/activities/activity-lifecycle)e as [intenções](https://developer.android.com/reference/android/content/Intent.html) nos documentos do Android.

### <a name="java-or-kotlin"></a>Java ou Kotlin

O **Java** tornou-se uma linguagem em 1991, desenvolvida pelo que era a Sun Microsystems, mas que agora pertence à Oracle. Ele se tornou uma das linguagens de programação mais populares e poderosas com uma das maiores comunidades de suporte do mundo. O Java é baseado em classe e orientado a objeto, projetado para ter o menor número possível de dependências de implementação. A sintaxe é semelhante a C e C++, mas tem menos instalações de nível baixo do que qualquer uma delas.

O **Kotlin** foi anunciado pela primeira vez como uma nova linguagem de software livre por JetBrains em 2011 e foi incluído como uma alternativa ao Java em Android Studio desde 2017. Em maio de 2019, o Google anunciou o Kotlin como seu idioma preferido para desenvolvedores de aplicativos Android, portanto, apesar de ser uma linguagem mais nova, ele também tem uma comunidade de suporte forte e foi identificado como uma das linguagens de programação cada vez mais crescentes. O Kotlin é de plataforma cruzada, digitada estaticamente e projetada para interoperar totalmente com o Java.

O Java é mais amplamente usado para uma gama mais ampla de aplicativos e oferece alguns recursos que o Kotlin não, como exceções marcadas, tipos primitivos que não são classes, membros estáticos, campos não privados, tipos curinga e operadores ternários. O Kotlin é especificamente projetado para o e recomendado pelo Android. Ele também oferece alguns recursos que o Java não, como referências nulas controladas pelo sistema de tipos, sem tipos brutos, matrizes invariáveis, tipos de função apropriados (em oposição às conversões de SAM do Java), use a variação de site sem curingas, difusões inteligentes e muito mais. A documentação do Kotlin oferece [uma visão mais detalhada da comparação com o Java](https://kotlinlang.org/docs/reference/comparison-to-java.html).

### <a name="minimum-api-level"></a>Nível mínimo de API

Você precisará decidir o nível mínimo de API para seu aplicativo. Isso determina a qual versão do Android seu aplicativo dará suporte. Níveis de API inferiores são mais antigos e, portanto, geralmente dão suporte a mais dispositivos, mas níveis de API mais altos são mais recentes e, portanto, fornecem mais recursos.

![Tela de seleção de API de Android Studio mínima](../images/android-minimum-api-selection.png)

Selecione o link **ajude-me escolher** para abrir um gráfico de comparação mostrando a distribuição de suporte a dispositivos e os principais recursos associados à versão da plataforma.

![Tela de comparação mínima de API Android Studio](../images/android-minimum-api-selection-2.png)

### <a name="instant-app-support-and-androidx-artifacts"></a>Suporte a aplicativos instantâneos e artefatos Androidx

Você pode observar uma caixa de seleção para **dar suporte a aplicativos instantâneos** e outro para **usar artefatos androidx** em suas opções de criação de projeto. O *suporte aos aplicativos instantâneos* não é verificado e o *androidx* é verificado como o padrão recomendado.

Google Play **aplicativos instantâneos** fornecem uma maneira para as pessoas tentarem um aplicativo ou jogo sem instalá-lo primeiro. Esses aplicativos instantâneos podem ser exibidos em toda a Play Store, Google Search, redes sociais e em qualquer lugar que você compartilhar um link. Ao marcar a caixa **suporte para aplicativos instantâneos** , você está solicitando que Android Studio inclua o SDK de desenvolvimento instantâneo Google Play com seu projeto. Para saber mais sobre [Google Play aplicativos instantâneos](https://developer.android.com/topic/google-play-instant) e como [criar um pacote de aplicativo habilitado para instantâneo](https://developer.android.com/topic/google-play-instant/getting-started/instant-enabled-app-bundle), consulte a documentação do Android.

Os **artefatos do AndroidX** representam a nova versão da biblioteca de suporte do Android e fornecem compatibilidade com versões anteriores do Android. AndroidX fornece um namespace consistente começando com a cadeia de caracteres AndroidX para todos os pacotes disponíveis.

> [!NOTE]
> AndroidX agora é a biblioteca padrão. Para desmarcar essa caixa e usar a biblioteca de suporte anterior requer a remoção do último SDK do Android Q. Consulte [desmarque usar artefatos Androidx](https://stackoverflow.com/questions/56580980/uncheck-use-androidx-artifacts) em StackOverflow para obter instruções, mas primeiro Observe que os pacotes de biblioteca de suporte anteriores foram mapeados nos pacotes Androidx. * correspondentes. Para obter um mapeamento completo de todas as classes antigas e criar artefatos para os novos, consulte [migrando para o AndroidX](https://developer.android.com/jetpack/androidx/migrate).

## <a name="project-files"></a>Arquivos de projeto

A janela do **projeto** Android Studio, contém os seguintes arquivos (certifique-se de que a exibição do Android esteja selecionada no menu suspenso):

**aplicativo > Java > com. example. meuprimeiroapp > MainActivity**

A atividade principal e o ponto de entrada para seu aplicativo. Quando você cria e executa seu aplicativo, o sistema inicia uma instância dessa atividade e carrega seu layout.

**layout de > res > do aplicativo > activity_main. xml**

O arquivo XML que define o layout da interface do usuário da atividade. Ele contém um elemento TextView com o texto "Olá, Mundo"

**manifestos de > de aplicativo > AndroidManifest. xml**

O arquivo de manifesto que descreve as características fundamentais do aplicativo e de cada um de seus componentes.

**Gradle scripts > Build. gradle**

Há dois arquivos com este nome: "projeto: meu primeiro aplicativo", para todo o projeto e "módulo: aplicativo", para cada módulo de aplicativo. Um novo projeto terá inicialmente apenas um módulo. Use o Build. File do módulo para controlar como o plug-in gradle compila seu aplicativo. Saiba mais nos documentos do Android, [configure seu](https://developer.android.com/studio/build/index) artigo de Build.

## <a name="use-c-or-c-for-android-game-development"></a>Use C ou C++ para desenvolvimento de jogos Android

O sistema operacional Android foi projetado para dar suporte a aplicativos escritos em Java ou Kotlin, beneficiando-se de ferramentas incorporadas na arquitetura do sistema. Muitos recursos do sistema, como a interface do usuário do Android e o tratamento de intenções, são expostos apenas por meio de interfaces Java. Há algumas instâncias em que você talvez queira **usar código C ou C++ por meio do NDK (Kit de desenvolvimento nativo) do Android** , apesar de alguns dos desafios associados. O desenvolvimento de jogos é um exemplo, já que os jogos normalmente usam lógica de renderização personalizada escrita em OpenGL ou Vulkan e se beneficiam de uma infinidade de bibliotecas C focadas no desenvolvimento de jogos. Usar C ou C++ também *pode* ajudá-lo a compactar o desempenho extra de um dispositivo para obter baixa latência ou executar aplicativos de computação intensiva, como simulações de física. No entanto, o NDK **não é apropriado para a maioria dos programadores Android iniciantes** . A menos que você tenha uma finalidade específica para usar o NDK, é recomendável ficar com Java, Kotlin ou uma das [estruturas de plataforma cruzada](./overview.md).

Para criar um novo projeto com suporte a C/C++:

- Na seção **escolher seu projeto** do assistente de Android Studio, selecione o tipo de projeto *C++ * nativo*. Selecione **Avançar**, preencha os campos restantes e, em seguida, selecione **Avançar** novamente.

- Na seção **Personalizar suporte a c++** do assistente, você pode personalizar seu projeto com o campo **padrão do c++** . Use a lista suspensa para selecionar a padronização do C++ que você deseja usar. A seleção de **ferramentas padrão** usa a configuração padrão de CMake. Selecione **Concluir**.

- Uma vez que Android Studio cria o novo projeto, você pode encontrar uma pasta **cpp** no painel **projeto** que contém os arquivos de origem nativos, os cabeçalhos, os scripts de compilação para CMake ou NDK-Build, e bibliotecas predefinidas que fazem parte do seu projeto. Você também pode encontrar um arquivo de origem C++ de `native-lib.cpp`exemplo,, `src/main/cpp/` na pasta que fornece uma `stringFromJNI()` função simples que retorna a cadeia de caracteres "Olá de C++". Além disso, você encontrará um script de Build [`CMakeLists.txt`](https://developer.android.com/studio/projects/configure-cmake.html)do cmake,, no diretório raiz do seu módulo, necessário para criar sua biblioteca nativa.

Para saber mais, confira o tópico documentos do Android: [Adicionar código C e C++ ao seu projeto](https://developer.android.com/studio/projects/add-native-code). Para obter exemplos, consulte os [exemplos de NDK do Android com o repositório de integração do Android Studio C++](https://github.com/android/ndk-samples) no github. Para compilar e executar um jogo C++ no Android, use a [API de serviços de jogos Google Play](https://developers.google.com/games/services/cpp/gettingStartedAndroid).

## <a name="design-guidelines"></a>Diretrizes de design

Os usuários do dispositivo esperam que os aplicativos pareçam e se comportem de uma determinada maneira... seja passando o dedo ou tocando ou usando controles de voz, os usuários terão expectativas específicas sobre o que o aplicativo deve ter e como usá-lo. Essas expectativas devem permanecer consistentes para reduzir a confusão e a frustração. O Android oferece um guia para essas expectativas de plataforma e dispositivo que combina a base de design do Google material para Visual e padrões de navegação, juntamente com diretrizes de qualidade para compatibilidade, desempenho e segurança.

Saiba mais na [documentação de design do Android](https://developer.android.com/design).

### <a name="fluent-design-system-for-android"></a>Sistema de design fluente para Android

A Microsoft também oferece diretrizes de design com o objetivo de fornecer uma experiência integrada em todo o portfólio de aplicativos móveis da Microsoft.

O [sistema de design fluente para o design do Android](https://www.microsoft.com/design/fluent/#/android) e a criação de aplicativos personalizados que são nativamente Android enquanto ainda são fluentes de forma exclusiva.

- [Kit de ferramentas do Sketch](https://aka.ms/fluenttoolkits/android/sketch)
- [Kit de ferramentas figma](https://aka.ms/fluenttoolkits/android/figma)
- [Fonte do Android](https://fonts.google.com/specimen/Roboto)
- [Diretrizes de interface do usuário do Android](https://developer.android.com/design/)
- [Diretrizes para ícones de aplicativos Android](https://developer.android.com/guide/practices/ui_guidelines/icon_design)

## <a name="additional-resources"></a>Recursos adicionais

- [Conceitos básicos do aplicativo Android](https://developer.android.com/guide/components/fundamentals)

- [Desenvolver aplicativos de tela dupla para Android e obter o SDK do dispositivo Surface Duo](https://docs.microsoft.com/dual-screen/android/)

- [Adicionar exclusões do Windows Defender para melhorar o desempenho](defender-settings.md)

- [Habilitar o suporte de virtualização para melhorar o desempenho do emulador](emulator.md#enable-virtualization-support)
