---
description: Uma introdução ao C++/WinRT, uma projeção de linguagem C++ padrão para APIs do Windows Runtime.
title: Introdução ao C++/WinRT
ms.date: 01/31/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção, introdução
ms.localizationpriority: medium
ms.openlocfilehash: 883463f291864016ebc32f2d510936452c931366
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57649691"
---
# <a name="introduction-to-cwinrt"></a>Introdução ao C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/nOFNc2uTmGs]

C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Com o C++/WinRT, você pode criar e consumir APIs do Windows Runtime usando qualquer compilador C++17 compatível com padrões. O SDK do Windows inclui o C++/WinRT; ele foi introduzido na versão 10.0.17134.0 (Windows 10, versão 1803).

C + + c++ /CLI WinRT é a substituição recomendada da Microsoft para o [C + + c++ /CLI CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) projeção de linguagem e o [biblioteca de modelos em C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live). A lista completa das [tópicos sobre C + + c++ /CLI WinRT](index.md#topics-about-cwinrt) inclui informações sobre interoperação com e portabilidade do, C + + c++ /CLI CX e WRL.

> [!IMPORTANT]
> Duas das partes mais importantes do C + + c++ /CLI WinRT a serem consideradas são descritas nas seções [dar suporte a SDK para C + + c++ /CLI WinRT](#sdk-support-for-cwinrt) e [suporte do Visual Studio para C + + c++ /CLI WinRT XAML, a extensão do VSIX, NuGet e o pacote](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package).

## <a name="language-projections"></a>Projeções de linguagem
O Windows Runtime baseia-se nas COM (Component Object Model) APIs e ele foi projetado para ser acessado por meio de *projeções de linguagem*. Uma projeção oculta os detalhes do COM e oferece uma experiência de programação mais natural para uma linguagem específica.

### <a name="the-cwinrt-language-projection-in-the-windows-uwp-api-reference-content"></a>A projeção de linguagem C++/WinRT no conteúdo de referência de API da UWP no Windows
Quando você estiver navegando nas [APIs da UWP no Windows](https://docs.microsoft.com/uwp/api/), clique na caixa de combinação **Linguagem** no canto superior direito e selecione **C++/WinRT** para exibir os blocos de sintaxe de API conforme aparecem na projeção de linguagem C++/WinRT.

## <a name="sdk-support-for-cwinrt"></a>Suporte a SDK para C++/WinRT
A partir da versão 10.0.17134.0 (Windows 10, versão 1803), o SDK do Windows contém uma biblioteca C++ padrão com base em cabeçalho e arquivo para consumir APIs primárias do Windows (APIs do Windows Runtime em namespaces do Windows). O C++/WinRT também vem com a ferramenta `cppwinrt.exe`, que você pode apontar para um arquivo de metadados do Windows Runtime (`.winmd`) a fim de gerar uma biblioteca C++ padrão com base em cabeçalho e arquivo que *projeta* as APIs descritas nos metadados para consumo pelo código do C++/WinRT. Os arquivos de metadados (`.winmd`) do Windows Runtime fornecem uma maneira canônica de descrever uma superfície de API do Windows Runtime. Ao apontar `cppwinrt.exe` nos metadados, você pode gerar uma biblioteca para uso com qualquer classe de tempo de execução implementada em um componente secundário ou de terceiros do Windows Runtime, ou implementar em seu próprio aplicativo. Para obter mais informações, consulte [Consumir APIs com C++/WinRT](consume-apis.md).

Com o C++/WinRT, você também pode implementar suas próprias classes de tempo de execução usando o C++ padrão, sem precisar recorrer à programação de estilo COM. Para uma classe de tempo de execução, você descreve apenas os tipos em um arquivo IDL e `midl.exe` e `cppwinrt.exe` geram os arquivos de código-fonte clichê de implementação para você. Você também pode implementar apenas interfaces derivando uma classe base do C++/WinRT. Para obter mais informações, consulte [Criar APIs com C++/WinRT](author-apis.md).

## <a name="visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package"></a>Suporte do Visual Studio para C + + c++ /CLI WinRT, XAML, a extensão do VSIX e o pacote do NuGet
Para obter suporte do Visual Studio, além de uma versão de destino mínima do SDK do Windows do 10.0.17134.0 (Windows 10, versão 1803), será necessário o Visual Studio 2017 (pelo menos versão 15.6; é recomendável pelo menos 15.7), ou o Visual Studio de 2019. Se você ainda não instalou-lo, você precisará instalar o **ferramentas C++ da plataforma Windows Universal** opção dentro do instalador do Visual Studio. E, no Windows **as configurações** > **Update \& segurança** > **para desenvolvedores**, escolha o **Developer modo** opção em vez de **Sideload de aplicativos** opção.

Você precisará baixar e instalar a versão mais recente do [C + + c++ /CLI WinRT extensão VSIX (Visual Studio)](https://aka.ms/cppwinrt/vsix) da [Visual Studio Marketplace](https://marketplace.visualstudio.com/).

- A extensão do VSIX oferece C + + c++ /CLI modelos de projeto e item do WinRT no Visual Studio, para que você pode começar com C + + c++ desenvolvimento WinRT.
- Além disso, ele oferece visualização de depuração nativa do Visual Studio (natvis) do C + + c++ /CLI WinRT projetado tipos; fornecendo uma experiência semelhante à C# depuração. Natvis é automático para compilações de depuração. Você pode escolher compilações de liberação definindo o símbolo WINRT_NATVIS.

Os modelos de projeto do Visual Studio para C + + c++ /CLI WinRT são descritos abaixo. Quando você cria uma nova C + c++ /CLI WinRT projeto com a versão mais recente da extensão do VSIX instalado, a nova C + c++ /CLI instala automaticamente o projeto do WinRT a [pacote do Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/). O **Microsoft.Windows.CppWinRT** pacote do NuGet fornece C + + c++ /CLI WinRT criar suporte (Propriedades do MSBuild e destinos), tornando seu projeto portátil entre um computador de desenvolvimento e um agente de compilação (no qual apenas o pacote do NuGet, e não é a extensão do VSIX, estiver instalado).

> [!IMPORTANT]
> Se você tiver projetos que foram criados com (ou atualizados para funcionar com) uma versão da extensão do VSIX anteriormente que 1.0.190128.4, em seguida, confira [versões anteriores da extensão do VSIX](#earlier-versions-of-the-vsix-extension). Essa seção contém informações importantes sobre a configuração de seus projetos, o qual você precisará saber para atualizá-los para usar a versão mais recente da extensão do VSIX.

Porque C + + c++ /CLI WinRT usa recursos a partir do padrão C + + 17, ele precisa de propriedade do projeto **C/C++** > **idioma** > **padrão de linguagem C++**  >  **ISO padrão c++17 (/ std:c++17 + + 17)**. Você também poderá definir **modo de conformidade: Sim (/permissive--)**, que restringe adicionalmente o seu código seja compatível com os padrões.

Outra propriedade de projeto a ser considerada é **C/C++** > **Geral** > **Tratar Avisos como Erros**. Defina-a como **Sim (/WX)** ou **Não (/WX-)** para fazer um teste. Às vezes, os arquivos de origem gerados pela ferramenta `cppwinrt.exe` geram avisos até você adicionar a implementação a eles.

Com o sistema definido até conforme descrito acima, você poderá criar e compilar ou abrir um C + +, c++ /CLI WinRT de projeto no Visual Studio e implantá-lo.

Como alternativa, você pode converter um projeto existente, Instalando manualmente os **Microsoft.Windows.CppWinRT** pacote do NuGet. Após instalar (ou atualizar para) a versão mais recente da extensão do VSIX, abra o projeto existente no Visual Studio, clique em **Project** \> **gerenciar pacotes NuGet...** \> **Navegue**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **instalar** para instalar o pacote para o projeto. Depois de adicionar o pacote, você obterá C + + c++ /CLI WinRT MSBuild suporte para o projeto, incluindo como chamar o `cppwinrt.exe` ferramenta.

Você pode identificar um projeto que usa o C + + c++ /CLI WinRT MSBuild suporte pela presença do **Microsoft.Windows.CppWinRT** pacote NuGet instalado dentro do projeto.

Aqui estão os modelos de projeto do Visual Studio fornecidos pela extensão do VSIX.

### <a name="windows-console-application-cwinrt"></a>Aplicativo de Console do Windows (C++/WinRT)
Um modelo de projeto para um aplicativo cliente C++/WinRT para Área de Trabalho do Windows, com uma interface de usuário de console.

### <a name="blank-app-cwinrt"></a>Aplicativo em Branco (C++/WinRT)
Um modelo de projeto para um aplicativo UWP (Plataforma Universal do Windows) que tem uma interface do usuário XAML.

O Visual Studio fornece suporte ao compilador XAML para gerar stubs de implementação e cabeçalho a partir do arquivo de linguagem IDL (`.idl`) que são a base de cada arquivo de marcação XAML. Em um arquivo IDL, defina qualquer classe de tempo de execução local que você deseja referenciar nas páginas XAML do seu app e crie o projeto uma vez para gerar modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de tempo de execução local. É recomendável declarar cada classe de tempo de execução em seu próprio arquivo IDL.

Suporte à superfície de design XAML do Visual Studio para C + + c++ /CLI WinRT está perto de paridade com o C#. Uma exceção é o **eventos** guia o **propriedades** janela. Com um C# projeto, você pode usar essa guia para adicionar manipuladores de eventos; com C + + / projeto do WinRT, o recurso não está presente. Mas vê [manipular eventos usando delegados em C + + c++ /CLI WinRT](handle-events.md) para obter informações sobre como adicionar manipuladores de eventos ao seu código.

### <a name="core-app-cwinrt"></a>App Core (C++/WinRT)
Um modelo de projeto para um app da Plataforma Universal do Windows (UWP) que não usa XAML.

Em vez disso, ele usa o cabeçalho de namespace do Windows do C++/WinRT para o namespace Windows.ApplicationModel.Core. Após a compilação e a execução, clique em um espaço vazio para adicionar um quadrado colorido; em seguida, clique em um quadrado colorido para arrastá-lo.

### <a name="windows-runtime-component-cwinrt"></a>Componente do Tempo de Execução do Windows (C++/WinRT)
Um modelo de projeto para um componente; normalmente para consumo de uma Plataforma Universal do Windows (UWP).

Esse modelo demonstra a cadeia de ferramentas `midl.exe` > `cppwinrt.exe`, na qual os metadados do Windows Runtime (`.winmd`) são gerados a partir da IDL. Depois, os stubs de implementação e de cabeçalho são gerados a partir desses metadados.

Em um arquivo IDL, defina as classes de tempo de execução no componente, na interface padrão e em qualquer outra interface implementada por ele. Compile o projeto uma vez para gerar `module.g.cpp`, `module.h.cpp`, modelos de implementação em `Generated Files` e definições de tipo de stub em `Generated Files\sources`. Em seguida, use essas definições de tipo de stub para referência ao implementar as classes de tempo de execução no componente. É recomendável declarar cada classe de tempo de execução em seu próprio arquivo IDL.

Agrupe o binário interno do componente do Tempo de Execução do Windows e seu `.winmd` com o aplicativo UWP que os utiliza.

## <a name="earlier-versions-of-the-vsix-extension"></a>Versões anteriores da extensão do VSIX
É recomendável que você instale (ou atualizar para) a versão mais recente do [extensão do VSIX](https://aka.ms/cppwinrt/vsix). Ele é configurado para se atualizar por padrão. Se você faz isso, e você tiver projetos que foram criados com uma versão da extensão do VSIX anteriores à 1.0.190128.4 e, em seguida, esta seção contém informações importantes sobre como atualizar os projetos para trabalhar com a nova versão. Se você não atualizar, em seguida, você ainda encontrará as informações nesta seção úteis.

Em termos de suporte do SDK do Windows e versões do Visual Studio e configuração do Visual Studio, as informações na [suporte do Visual Studio para C + + c++ /CLI WinRT, XAML, a extensão do VSIX e o pacote NuGet](#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) seção acima aplica-se a anteriormente versões da extensão do VSIX. As informações abaixo descreve as diferenças importantes em relação ao comportamento e configuração de projetos criados com (ou atualizado para trabalhar com) anteriormente versões.

### <a name="created-earlier-than-101810022"></a>Criado anteriormente que 1.0.181002.2
Se seu projeto foi criado com uma versão da extensão do VSIX anteriores à 1.0.181002.2, em seguida, C + + c++ /CLI suporte de build do WinRT foi incorporado a essa versão da extensão do VSIX. O projeto tem o `<CppWinRTEnabled>true</CppWinRTEnabled>` propriedade definida no `.vcxproj` arquivo.

```xml
<Project ...>
    <PropertyGroup Label="Globals">
        <CppWinRTEnabled>true</CppWinRTEnabled>
...
```

Você pode atualizar seu projeto Instalando manualmente os **Microsoft.Windows.CppWinRT** pacote do NuGet. Após instalar (ou atualizar) a versão mais recente da extensão do VSIX, abra o projeto no Visual Studio, clique em **Project** \> **gerenciar pacotes NuGet...** \> **Navegue**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **instalar** para instalar o pacote para seu projeto.

### <a name="created-with-or-upgraded-to-between-101810022-and-101901283"></a>Criado com (ou atualizado para) entre 1.0.181002.2 e 1.0.190128.3
Se seu projeto foi criado com uma versão da extensão do VSIX entre 1.0.181002.2 e 1.0.190128.3, inclusive, em seguida, a **Microsoft.Windows.CppWinRT** pacote do NuGet foi instalado no projeto automaticamente pelo projeto modelo. Você talvez também tenha atualizado um projeto antigo para usar uma versão da extensão do VSIX nesse intervalo. Se, em seguida, você fez&mdash;desde que o suporte de build também foi ainda presente nas versões da extensão do VSIX nesse intervalo&mdash;o projeto atualizado pode ou não ter o **Microsoft.Windows.CppWinRT** pacote do NuGet instalado.

Para atualizar seu projeto, siga as instruções na seção anterior e certifique-se de que seu projeto tenha o **Microsoft.Windows.CppWinRT** pacote NuGet instalado.

### <a name="invalid-upgrade-configurations"></a>Configurações de atualização inválidas
Com a versão mais recente da extensão do VSIX, não é válido para um projeto ter o `<CppWinRTEnabled>true</CppWinRTEnabled>` propriedade se ele também não tiver o **Microsoft.Windows.CppWinRT** pacote NuGet instalado. Um projeto com essa configuração produz a mensagem de erro de compilação, "C + + / VSIX WinRT não fornece suporte de build do projeto.  Adicione uma referência de projeto para o pacote Microsoft.Windows.CppWinRT Nuget."

Conforme mencionado acima, a C + + / WinRT projeto agora precisa ter o pacote NuGet instalado nele.

Uma vez que o `<CppWinRTEnabled>` elemento agora é obsoleto, opcionalmente, você pode editar seu `.vcxproj`e excluir o elemento. Não é estritamente necessário, mas é uma opção.

Além disso, se sua `.vcxproj` contém `<RequiredBundles>$(RequiredBundles);Microsoft.Windows.CppWinRT</RequiredBundles>`, em seguida, você pode removê-lo para que você possa criar sem a necessidade de C + c++ /CLI a instalação de extensão do VSIX WinRT.

## <a name="custom-types-in-the-cwinrt-projection"></a>Tipos personalizados na projeção C++/WinRT
No C + c++ /CLI WinRT de programação, você pode usar os recursos de linguagem C++ padrão e [tipos de dados padrão do C++ e C + + c++ /CLI WinRT](std-cpp-data-types.md)&mdash;incluindo alguns tipos de dados da biblioteca padrão C++. Mas, você também ficará a par de alguns tipos de dados personalizados na projeção e poderá optar por usá-los. Por exemplo, usamos [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring) no exemplo do código de início rápido em [Introdução com C++/WinRT](get-started.md).

[**WinRT::com_array** ](/uwp/cpp-ref-for-winrt/com-array) é outro tipo que você está acostumado a usar em algum momento. Mas a probabilidade de você usar diretamente um tipo como [**winrt::array_view**](/uwp/cpp-ref-for-winrt/array-view) é menor. Ou você pode optar por não usá-lo para que não tenha nenhum código a ser alterado se um tipo equivalente aparecer na Biblioteca Padrão C++.

> [!WARNING]
> Também existem tipos que você poderá ver se investigar detalhadamente os cabeçalhos de namespace do C++/WinRT do Windows. Um exemplo é **winrt::param::hstring**, mas há exemplos de coleção também. Elas existem exclusivamente para otimizar a associação de parâmetros de entrada e geram melhorias de desempenho grande, além de transformar a maioria dos padrões de chamadas em "trabalho" para os tipos e contêineres de C++ padrão relacionados. Esses tipos são usados apenas pela projeção em casos onde agregarem o maior valor. São altamente otimizados e não são para uso geral; não tente usá-los. Você também não deve usar qualquer item do namespace `winrt::impl`, pois são tipos de implementação e, portanto, estão sujeitos a mudanças. Você deve continuar a usar tipos padrão ou tipos do [winrt namespace](/uwp/cpp-ref-for-winrt/winrt).

## <a name="important-apis"></a>APIs Importantes
* [struct WinRT::hstring](/uwp/cpp-ref-for-winrt/hstring)
* [winrt namespace](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [C + + / extensão do WinRT Visual Studio (VSIX)](https://aka.ms/cppwinrt/vsix)
* [Introdução ao C + + c++ /CLI WinRT](get-started.md)
* [Tipos de dados padrão do C++ e C + + c++ /CLI WinRT](std-cpp-data-types.md)
* [Cadeia de caracteres de tratamento em C + + c++ /CLI WinRT](strings.md)
* [APIs de UWP do Windows](https://docs.microsoft.com/uwp/api/)
