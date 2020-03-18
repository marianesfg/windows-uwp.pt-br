---
description: Uma introdução ao C++/WinRT&mdash;uma projeção de linguagem C++ padrão para APIs do Windows Runtime.
title: Introdução ao C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, standard, c++, cpp, winrt, projection, introduction
ms.localizationpriority: medium
ms.openlocfilehash: fd267f96ca6931252ab3130d363447ae79820108
ms.sourcegitcommit: ca1b5c3ab905ebc6a5b597145a762e2c170a0d1c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/13/2020
ms.locfileid: "79209131"
---
# <a name="introduction-to-cwinrt"></a>Introdução ao C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/X41j_gzSwOY]

&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do WinRT (Windows Runtime), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Com o C++/WinRT, você pode criar e consumir APIs do Windows Runtime usando qualquer compilador C++17 em conformidade com os padrões. O SDK do Windows inclui o C++/WinRT; ele foi introduzido na versão 10.0.17134.0 (Windows 10, versão 1803).

O C++/WinRT é a substituição recomendada da Microsoft para o [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) projeção de linguagem e a [WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). A lista completa dos [tópicos sobre o C++/WinRT](index.md#topics-about-cwinrt) inclui informações sobre interoperação com e portabilidade, C++/CX e WRL.

> [!IMPORTANT]
> Algumas das partes mais importantes do C++/WinRT a serem consideradas são descritas nas seções [Suporte do SDK para C++/WinRT](#sdk-support-for-cwinrt) e [Suporte do Visual Studio para C++/WinRT, XAML, a extensão do VSIX e o pacote NuGet](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Projeções de linguagem
O Windows Runtime baseia-se nas APIs do COM (Component Object Model) e foi projetado para ser acessado por meio de *projeções de linguagem*. Uma projeção oculta os detalhes do COM e oferece uma experiência de programação mais natural para uma linguagem específica.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>A projeção de linguagem C++/WinRT no conteúdo de referência de API da UWP no Windows
Quando você estiver navegando nas [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/), clique na caixa de combinação **Linguagem** no canto superior direito e selecione **C++/WinRT** para exibir os blocos de sintaxe de API conforme aparecem na projeção de linguagem do C++/WinRT.

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Suporte do Visual Studio para C++/WinRT, XAML, a extensão do VSIX e o pacote NuGet
Para obter suporte do Visual Studio, você precisará do Visual Studio 2019 ou Visual Studio 2017 (pelo menos versão 15.6; é recomendável pelo menos a versão 15.7). No instalador do Visual Studio, instale a carga de trabalho de **Desenvolvimento para a Plataforma Universal do Windows**. Em **Detalhes da Instalação** > **Desenvolvimento com a Plataforma Universal do Windows** , marque as opções **Ferramentas da Plataforma Universal do Windows do C++ (v14x)** , se você ainda não fez isso. No Windows, em **Configurações** > **Atualizar \& Segurança** > **Para desenvolvedores**, escolha a opção **Modo de desenvolvedor** em vez da opção **Aplicativos sideload**.

Embora seja recomendável que você desenvolva com as versões mais recentes do Visual Studio e o SDK do Windows, se você estiver usando uma versão do C++/WinRT fornecida com o SDK do Windows antes de 10.0.17763.0 (Windows 10, versão 1809), para usar os cabeçalhos de namespaces do Windows mencionados acima, será necessária uma versão de destino mínima do SDK do Windows em seu projeto do 10.0.17134.0 (Windows 10, versão 1803).

Convém baixar e instalar a última versão do [C++/WinRT Visual Studio Extension (VSIX)](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264) do [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- A extensão do VSIX oferece modelos de projeto e item do C++/WinRT no Visual Studio, para que você possa começar com o desenvolvimento C++/WinRT.
- Além disso, ele também oferece visualização de depuração nativa do Visual Studio (natvis) dos tipos projetados do C++/WinRT, fornecendo uma experiência semelhante à depuração de C#. Natvis é automático para builds de depuração. Você pode escolher builds de versão definindo o símbolo WINRT_NATVIS.

Os modelos de projeto do Visual Studio para C++/WinRT são descritos nas seções a seguir. Quando você cria um novo projeto C++/WinRT com a última versão da extensão do VSIX instalada, o novo projeto C++/WinRT instala automaticamente o [pacote NuGet Microsoft.Windows.CppWinRT](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). O pacote NuGet **Microsoft.Windows.CppWinRT** fornece suporte de build do C++/WinRT (propriedades e destinos do MSBuild), tornando seu projeto portátil entre um computador de desenvolvimento e um agente de build (no qual apenas o pacote NuGet, e não a extensão do VSIX, está instalado).

Como alternativa, você pode converter um projeto existente, instalando manualmente o pacote NuGet **Microsoft.Windows.CppWinRT**. Depois de instalar (ou atualizar para) a última versão da extensão do VSIX, abra o projeto existente no Visual Studio, clique em **Projeto** \> **Gerenciar Pacotes NuGet...** \> **Procurar**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e clique em **Instalar** para instalar o pacote do projeto. Depois de adicionar o pacote, você receberá o suporte do MSBuild para C++/WinRT no projeto, incluindo a invocação da ferramenta `cppwinrt.exe`.

> [!IMPORTANT]
> Se você tiver projetos criados com (ou atualizados para funcionar com) uma versão da extensão do VSIX anterior a 1.0.190128.4, confira [Versões anteriores da extensão do VSIX](#earlier-versions-of-the-vsix-extension). Esta seção contém informações importantes sobre a configuração de seus projetos, que você precisará saber para atualizá-los para usar a última versão da extensão do VSIX.

- Como o C++/WinRT usa recursos do padrão C++17, o pacote NuGet define a propriedade do projeto **C/C++**  > **Linguagem** > **Padrão da Linguagem C++**  > **Padrão ISO C++17 (/std:c++17)** no Visual Studio.
- Ele também adiciona a opção do compilador [/bigobj](/cpp/build/reference/bigobj-increase-number-of-sections-in-dot-obj-file).
- Ele adiciona a opção do compilador [/await](/cpp/build/reference/await-enable-coroutine-support) para habilitar `co_await`.
- Ele instrui o compilador XAML para emitir o gerador de código C++/WinRT.
- Você também poderá definir o **Modo de conformidade: Sim (/permissive--)** , que restringe ainda mais o seu código para que fique em conformidade com os padrões.
- Outra propriedade de projeto a ser considerada é **C/C++**  > **Geral** > **Tratar Avisos como Erros**. Defina-a como **Sim (/WX)** ou **Não (/WX-)** para fazer um teste. Às vezes, os arquivos de origem gerados pela ferramenta `cppwinrt.exe` geram avisos até você adicionar a implementação a eles.

Com o sistema definido conforme descrito acima, você poderá criar e compilar ou abrir um projeto do C++/WinRT no Visual Studio e implantá-lo.

Da 2.0 em diante, o pacote NuGet **Microsoft.Windows.CppWinRT** inclui a ferramenta `cppwinrt.exe`. Você pode apontar para a ferramenta `cppwinrt.exe` em um metadados do Windows Runtime (`.winmd`) a fim de gerar uma biblioteca C++ padrão com base em cabeçalho e arquivo que *projeta* as APIs descritas nos metadados para consumo pelo código do C++/WinRT. Os arquivos de metadados (`.winmd`) do Windows Runtime fornecem uma maneira canônica de descrever uma superfície de API do Windows Runtime. Ao apontar `cppwinrt.exe` nos metadados, você pode gerar uma biblioteca para uso com qualquer classe de tempo de execução implementada em um componente secundário ou de terceiros do Windows Runtime ou implementar em seu próprio aplicativo. Para saber mais, confira [Consumir APIs com C++/WinRT](consume-apis.md).

Com o C++/WinRT, você também pode implementar suas próprias classes de runtime usando o C++ padrão, sem precisar recorrer à programação de estilo COM. Para uma classe de runtime, você descreve apenas os tipos em um arquivo IDL e `midl.exe` e `cppwinrt.exe` geram os arquivos de código-fonte clichê de implementação para você. Você também pode implementar apenas interfaces derivando uma classe base do C++/WinRT. Para obter mais informações, confira [Criar APIs com C++/WinRT](author-apis.md).

Para obter uma lista de opções de personalização para o conjunto de ferramentas `cppwinrt.exe` por meio das propriedades do projeto, confira o [arquivo Leiame](https://github.com/microsoft/xlang/tree/master/src/package/cppwinrt/nuget/readme.md#customizing) do pacote NuGet Microsoft.Windows.CppWinRT.

Você pode identificar um projeto que usa o suporte do MSBuild C++/WinRT pela presença do pacote NuGet **Microsoft.Windows.CppWinRT** instalado dentro do projeto.

Veja os modelos de projeto do Visual Studio fornecidos pela extensão do VSIX.

### <a name="blank-app-cwinrt"></a>Aplicativo em Branco (C++/WinRT)
Um modelo de projeto para um aplicativo UWP (Plataforma Universal do Windows) que tem uma interface do usuário XAML.

O Visual Studio fornece suporte ao compilador XAML para gerar stubs de implementação e cabeçalho do arquivo da linguagem IDL (`.idl`) que são a base de cada arquivo de marcação XAML. Em um arquivo IDL, defina qualquer classe de runtime local que você deseja referenciar nas páginas XAML do seu aplicativo e crie o projeto uma vez para gerar modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo stub como referência ao implementar suas classes de runtime local. Confira [Como fatorar classes de runtime em arquivos MIDL (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

O suporte de superfície de design do XAML no Visual Studio de 2019 para C++/WinRT está perto da paridade com o C#. No Visual Studio de 2019, você pode usar a guia **Eventos** da janela **Propriedades** para adicionar manipuladores de eventos dentro de um projeto do C++/WinRT. Você também pode adicionar manipuladores de eventos ao seu código manualmente&mdash;. Confira [Processar eventos usando delegados em C++/WinRT](handle-events.md) para obter mais informações.

### <a name="core-app-cwinrt"></a>Aplicativo de núcleo (C++/WinRT)
Um modelo de projeto para um aplicativo da UWP (Plataforma Universal do Windows) que não usa XAML.

Em vez disso, ele usa o cabeçalho de namespace do Windows do C++/WinRT para o namespace Windows.ApplicationModel.Core. Após a compilação e a execução, clique em um espaço vazio para adicionar um quadrado colorido; em seguida, clique em um quadrado colorido para arrastá-lo.

### <a name="windows-console-application-cwinrt"></a>Aplicativo de Console do Windows (C++/WinRT)
Um modelo de projeto para um aplicativo cliente do C++/WinRT para Área de Trabalho do Windows, com uma interface do usuário de console.

### <a name="windows-desktop-application-cwinrt"></a>Aplicativos da área de trabalho do Windows (C++/WinRT)
Um modelo de projeto para um aplicativo cliente do C++/WinRT para a Área de Trabalho do Windows, que exibe um Windows Runtime [Windows.Foundation.Uri](/uwp/api/windows.foundation.uri) dentro de um Win32 **MessageBox**.

### <a name="windows-runtime-component-cwinrt"></a>componente do Windows Runtime (C++/WinRT)
Um modelo de projeto para um componente; normalmente para consumo de uma UWP (Plataforma Universal do Windows).

Esse modelo demonstra a cadeia de ferramentas `midl.exe` > `cppwinrt.exe`, na qual os metadados do Windows Runtime (`.winmd`) são gerados com base na IDL. Depois, os stubs de implementação e de cabeçalho são gerados com base nesses metadados.

Em um arquivo IDL, defina as classes de runtime no componente, na interface padrão e em qualquer outra interface implementada por ele. Compile o projeto uma vez para gerar `module.g.cpp`, `module.h.cpp`, modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de runtime no componente. Confira [Como fatorar classes de runtime em arquivos MIDL (.idl)](/windows/uwp/cpp-and-winrt-apis/author-apis#factoring-runtime-classes-into-midl-files-idl).

Agrupe o binário compilado do componente do Windows Runtime e seu `.winmd` com o aplicativo UWP os consumindo.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versões anteriores da extensão do VSIX
É recomendável que você instale (ou atualizar para) a última versão da [extensão do VSIX](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264). Ela é configurada para se atualizar por padrão. Se você fizer isso e tiver projetos que foram criados com uma versão da extensão do VSIX anterior à 1.0.190128.4, esta seção conterá informações importantes sobre como atualizar os projetos para trabalhar com a nova versão. Se você não atualizar, ainda encontrará informações úteis nesta seção.

Em termos de versões compatíveis do SDK do Windows e do Visual Studio e de configuração do Visual Studio, as informações na seção [Suporte do Visual Studio para C++/WinRT, XAML, a extensão do VSIX e o pacote NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) acima aplica-se a versões anteriores da extensão do VSIX. As informações abaixo descrevem as diferenças importantes em relação ao comportamento e à configuração de projetos criados com (ou atualizado para trabalhar com) versões anteriores.

### <a name="created-earlier-than-101810022"></a>Criado antes de 1.0.181002.2
Se o projeto foi criado com uma versão da extensão do VSIX anterior à 1.0.181002.2, o suporte de compilação do C++/WinRT foi incorporado a essa versão da extensão do VSIX. O projeto tem a propriedade `<CppWinRTEnabled>true</CppWinRTEnabled>` definida no arquivo `.vcxproj`.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Você pode atualizar o projeto instalando manualmente o pacote NuGet **Microsoft.Windows.CppWinRT**. Depois de instalar (ou atualizar para) a última versão da extensão do VSIX, abra o projeto no Visual Studio, clique em **Projeto** \> **Gerenciar Pacotes NuGet...** \> **Procurar**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e clique em **Instalar** para instalar o pacote para seu projeto.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Criado com (ou atualizado para) entre 1.0.181002.2 e 1.0.190128.3
Se o projeto foi criado com uma versão da extensão do VSIX entre 1.0.181002.2 e 1.0.190128.3, inclusive, o pacote NuGet **Microsoft.Windows.CppWinRT** foi instalado no projeto automaticamente pelo modelo do projeto. Talvez você também tenha atualizado um projeto antigo para usar uma versão da extensão do VSIX nesse intervalo. Caso afirmativo, &mdash;como o suporte de build também ainda estava presente nas versões da extensão do VSIX nesse intervalo&mdash;, o projeto atualizado pode ou não ter o pacote NuGet **Microsoft.Windows.CppWinRT** instalado.

Para atualizar seu projeto, siga as instruções na seção anterior e verifique se o projeto tem o pacote NuGet **Microsoft.Windows.CppWinRT** instalado.

### <a name="invalid-upgrade-configurations"></a>Configurações de atualização inválidas
Com a versão mais recente da extensão do VSIX, não é válido para um projeto ter a propriedade `<CppWinRTEnabled>true</CppWinRTEnabled>` se ele também não tem o pacote NuGet **Microsoft.Windows.CppWinRT** instalado. Um projeto com essa configuração gera a mensagem de erro de build "O C++/WinRT VSIX não fornece mais suporte ao build do projeto.  Adicione uma referência de projeto ao pacote NuGet Microsoft.Windows.CppWinRT".

Conforme mencionado acima, um projeto do C++/WinRT agora precisa ter o pacote NuGet instalado nele.

Uma vez que o elemento `<CppWinRTEnabled>` agora é obsoleto, opcionalmente, você pode editar seu `.vcxproj` e excluir o elemento. Não é estritamente necessário, mas é uma opção.

Além disso, se o `.vcxproj` contiver `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, será possível removê-lo para que você possa criar sem a necessidade de instalar a extensão do VSIX do C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Suporte a SDK para C++/WinRT
Embora agora esteja presente apenas por motivos de compatibilidade, da versão 10.0.17134.0 em diante (Windows 10, versão 1803), o SDK do Windows contém uma biblioteca C++ padrão com base em cabeçalho e arquivo para consumir APIs primárias do Windows (APIs do Windows Runtime em namespaces do Windows). Esses cabeçalhos estão dentro da pasta `%WindowsSdkDir%Include<WindowsTargetPlatformVersion>\cppwinrt\winrt`. Do SDK do Windows versão 10.0.17763.0 em diante (Windows 10, versão 1809), esses cabeçalhos são gerados para você dentro da pasta *$(GeneratedFilesDir)* do seu projeto.

Novamente para compatibilidade, o SDK do Windows também vem com a ferramenta `cppwinrt.exe`. No entanto, é recomendável instalar e usar a versão mais recente do `cppwinrt.exe`, que é incluído com o pacote NuGet **Microsoft.Windows.CppWinRT**. Esse pacote e `cppwinrt.exe` são descritos nas seções acima.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados na projeção C++/WinRT
Na programação do C++/WinRT, você pode usar os recursos de linguagem C++ padrão e [tipos de dados do C++ padrão e do C++/WinRT](std-cpp-data-types.md)&mdash;incluindo alguns tipos de dados da Biblioteca padrão do C++. Mas, você também ficará a par de alguns tipos de dados personalizados na projeção e poderá optar por usá-los. Por exemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) no exemplo do código de início rápido em [Introdução ao C++/WinRT](get-started.md).

[**winrt::com_array**](/uwp/cpp-ref-for-winrt/com-array) é outro tipo que você provavelmente usará em algum momento. Mas a probabilidade de você usar diretamente um tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) é menor. Ou você poderá optar por não usá-lo para que não tenha nenhum código a ser alterado se um tipo equivalente aparecer na Biblioteca Padrão C++.

> [!WARNING]
> Também existem tipos que você poderá ver se investigar detalhadamente os cabeçalhos de namespace do C++/WinRT do Windows. Um exemplo é **winrt::param::hstring**, mas há exemplos de coleção também. Elas existem exclusivamente para otimizar a associação de parâmetros de entrada e geram grandes melhorias de desempenho, além de transformar a maioria dos padrões de chamadas em "trabalho" para os tipos e contêineres de C++ padrão relacionados. Esses tipos são usados apenas pela projeção em casos em que agregam mais valor. São altamente otimizados e não são para uso geral; não tente usá-los. Você também não deve usar qualquer item do namespace `winrt::impl`, pois são tipos de implementação e, portanto, estão sujeitos a mudanças. Você deve continuar a usar tipos padrão ou tipos do [namespace winrt](/uwp/cpp-ref-for-winrt/winrt).
>
> Confira também [Passagem de parâmetros para o limite do ABI](/windows/uwp/cpp-and-winrt-apis/pass-parms-to-abi).

## <a name="important-apis"></a>APIs importantes
* [winrt::hstring struct](/uwp/cpp-ref-for-winrt/hstring)
* [namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Extensão do Visual Studio Extension (VSIX) para C++/WinRT](https://marketplace.visualstudio.com/items?itemName=CppWinRTTeam.cppwinrt101804264)
* [Introdução ao C++/WinRT](get-started.md)
* [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md)
* [Processamento da cadeia de caracteres em C++/WinRT](strings.md)
* [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/)
