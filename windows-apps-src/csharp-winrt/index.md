---
description: O C#/WinRT é um conjunto de ferramentas que fornece suporte à projeção do WinRT para código C#.
title: C#/WinRT
ms.date: 05/19/2020
ms.topic: article
keywords: windows 10, uwp, standard, c#, winrt, cswinrt, projeção
ms.localizationpriority: medium
ms.openlocfilehash: e52763d78937405b308c4c4fe06f6d231fa3abcf
ms.sourcegitcommit: d0f479f1955881afb62c2af249db5d0b053b63e5
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/19/2020
ms.locfileid: "83580273"
---
# <a name="cwinrt"></a>C#/WinRT

> [!IMPORTANT]
> O C#/WinRT está em versão prévia pública e pode ser substancialmente modificado antes da versão final. A Microsoft não faz nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.

O C#/WinRT é um kit de ferramentas empacotado em NuGet que fornece suporte à projeção do WinRT (Windows Runtime) para a linguagem C#. Uma *projeção* é uma camada de conversão, como um assembly de interoperabilidade, que permite a programação de APIs do WinRT de modo natural e familiar para a linguagem de destino. Por exemplo, a projeção do C#/WinRT oculta os detalhes de interoperabilidade entre as interfaces do C# e WinRT e fornece mapeamentos de muitos tipos do WinRT para os equivalentes apropriados do .NET, como cadeias de caracteres, URIs, tipos de valor comum e coleções genéricas.

No momento, o C#/WinRT oferece suporte para o consumo de tipos do WinRT, e a versão prévia atual permite que você [crie](#create-an-interop-assembly) e [referencie](#reference-an-interop-assembly) assemblies de interoperabilidade do WinRT. Versões futuras do C#/WinRT adicionarão suporte para a criação de tipos do WinRT no C#.

## <a name="motivation-for-cwinrt"></a>Motivação para o C#/WinRT

O [.NET Core](https://docs.microsoft.com/dotnet/core/) é o foco da plataforma .NET, e o .NET 5 é a próxima versão principal. É um runtime multiplataforma e de software livre que pode ser usado para criar aplicativos de dispositivo, de nuvem e IoT.

As versões anteriores do .NET Framework e do .NET Core tinham conhecimento interno do WinRT, uma tecnologia específica do Windows. Para dar suporte às metas de portabilidade e eficiência do .NET 5, eliminamos o suporte de projeção do WinRT do compilador e do runtime do .NET e o movemos para o kit de ferramentas do C#/WinRT. A meta do C#/WinRT é fornecer paridade com o suporte do WinRT interno fornecido por versões anteriores do compilador do C# e do runtime do .NET. Para obter detalhes, confira [Mapeamentos do .NET dos tipos do Windows Runtime](https://docs.microsoft.com/windows/uwp/winrt-components/net-framework-mappings-of-windows-runtime-types).

O C#/WinRT também dá suporte a WinUI 3.0. Essa versão do WinUI elimina os controles e recursos nativos de interface do usuário da Microsoft do sistema operacional. Isso permite que os desenvolvedores de aplicativos usem os controles e elementos visuais mais recentes no Windows 10, versão 1803 e versões posteriores.

Por fim, o C#/WinRT é um kit de ferramentas geral e destina-se a dar suporte a outros cenários em que o suporte interno para o WinRT não está disponível no compilador do C# ou no runtime do .NET. O C#/WinRT dá suporte a versões do runtime do .NET compatíveis com o .NET Standard 2.0, como Mono 5.4.

Para obter informações adicionais sobre o C#/WinRT, confira [repositório do GitHub do C#/WinRT](https://aka.ms/cswinrt/repo)

## <a name="create-an-interop-assembly"></a>Criar um assembly de interoperabilidade

As APIs do WinRT são definidas nos arquivos de metadados do Windows (*.winmd). O pacote do NuGet do C#/WinRT inclui o compilador do C#/WinRT, **cswinrt**, que pode ser usado para processar arquivos de metadados do Windows e gerar o código C# do .NET Standard 2.0. É possível compilar esses arquivos de origem para assemblies de interoperabilidade, semelhante à forma como o [C++/WinRT](../cpp-and-winrt-apis/index.md) gera cabeçalhos para a projeção da linguagem C++. Em seguida, você pode distribuir os assemblies de interoperabilidade do C#/WinRT para que os aplicativos façam referência, juntamente com o assembly de runtime do C#/WinRT.

### <a name="invoke-cswinrtexe"></a>Invocar cswinrt.exe

Para exibir opções de linha de comando, execute `cswinrt -?`. Para invocar cswinrt.exe de um projeto, recomendamos usar um arquivo Directory.Build.Targets. O fragmento de projeto a seguir demonstra uma invocação simples de **cswinrt** para gerar fontes de projeção para tipos no namespace da Contoso. Essas fontes são então incluídas no build do projeto.

```xml
  <Target Name="GenerateProjection" BeforeTargets="Build">
    <PropertyGroup>
      <CsWinRTParams>
# This sample demonstrates using a response file for cswinrt execution.
# Run "cswinrt -h" to see all command line options.
-verbose
# Include Windows SDK metadata to satisfy references to 
# Windows types from project-specific metadata.
-in 10.0.18362.0
# Don't project referenced Windows types, as these are 
# provided by the Windows interop assembly.
-exclude Windows 
# Reference project-specific winmd files, defined elsewhere,
# such as from a NuGet package.
-in @(ContosoWinMDs->'"%(FullPath)"', ' ')
# Include project-specific namespaces/types in the projection
-include Contoso 
# Write projection sources to the "Generated Files" folder,
# which should be excluded from checkin (e.g., .gitignored).
-out "$(ProjectDir)Generated Files"
      </CsWinRTParams>
    </PropertyGroup>
    <WriteLinesToFile
        File="$(CsWinRTResponseFile)" Lines="$(CsWinRTParams)"
        Overwrite="true" WriteOnlyWhenDifferent="true" />
    <Message Text="$(CsWinRTCommand)" Importance="$(CsWinRTVerbosity)" />
    <Exec Command="$(CsWinRTCommand)" />
  </Target>

  <Target Name="IncludeProjection" BeforeTargets="CoreCompile" AfterTargets="GenerateProjection">
    <ItemGroup>
      <Compile Include="$(ProjectDir)Generated Files/*.cs" Exclude="@(Compile)" />
    </ItemGroup>
  </Target>
```

### <a name="distribute-the-interop-assembly"></a>Distribuir o assembly de interoperabilidade

Um assembly de interoperabilidade normalmente é distribuído como um pacote do NuGet, juntamente com uma dependência do pacote do NuGet do C#/WinRT para o assembly de runtime do C#/WinRT necessário, **winrt.runtime.dll**. Há duas versões do assembly de runtime do C#/WinRT, uma que direciona o .NET Standard 2.0 e outra que direciona o .NET 5.0. Apenas uma delas é implantada, dependendo da estrutura de destino de um aplicativo. 

* O direcionamento do .NET Standard 2.0 é adequado para aplicativos multiplataforma de nível inferior que desejam fornecer uma funcionalidade específica no Windows.
* O direcionamento do .NET 5.0 é recomendado para aplicativos do Windows modernos que exigem a coleta de lixo correta entre referências de objeto nativo, como aplicativos XAML.

Um assembly de interoperabilidade pode garantir que a versão correta do runtime do C#/WinRT seja implantada para um aplicativo incluindo uma condição `targetFramework` no arquivo nuspec.

```xml
<?xml version="1.0" encoding="utf-8"?>
<package xmlns="http://schemas.microsoft.com/packaging/2013/05/nuspec.xsd">
  <metadata>
    <dependencies>
      <group targetFramework=".NETStandard2.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
      <group targetFramework=".NET5.0">
        <dependency id="Microsoft.Windows.CsWinRT" version="0.1.0" />
      </group>
    </dependencies>
  </metadata>
</package>
```

> [!NOTE]
> O moniker da estrutura de destino para .NET 5.0 está mudando de ".NETCoreApp5.0" para ".NET5.0". Os pré-lançamentos do C#/WinRT usarão um ou outro.

## <a name="reference-an-interop-assembly"></a>Fazer referência a um assembly de interoperabilidade

Normalmente, os assemblies de interoperabilidade do C#/WinRT são referenciados por projetos de aplicativo. Contudo, eles também podem ser referenciados, por sua vez, por assemblies de interoperabilidade intermediários. Por exemplo, o assembly de interoperabilidade do WinUI faria referência ao assembly de interoperabilidade do SDK do Windows.

Para consumir os tipos do C#/WinRT projetados, adicione uma referência ao pacote do NuGet de interoperabilidade do C#/WinRT apropriado para seu projeto. Isso faz o assembly de interoperabilidade e o assembly de runtime do C#/WinRT serem adicionados ao projeto.

Se você distribuir um componente do WinRT de terceiros sem um assembly de interoperabilidade oficial, um projeto de aplicativo poderá seguir o procedimento para [criar um assembly de interoperabilidade](#create-an-interop-assembly) a fim de gerar as próprias fontes de projeção privadas. Não recomendamos essa abordagem, porque ela pode produzir projeções conflitantes do mesmo tipo dentro de um processo. O empacotamento do NuGet, que segue o esquema de [Controle de Versão Semântica](https://semver.org), foi criado para evitar isso. Um assembly de interoperabilidade oficial de terceiros é preferencial.

### <a name="winrt-type-activation"></a>Ativação do tipo do WinRT

O C#/WinRT dá suporte à ativação de tipos do WinRT hospedados pelo sistema operacional, assim como componentes de terceiros, como [Win2D](https://www.nuget.org/packages/Win2D.uwp/). O suporte para a ativação de componentes de terceiros em um aplicativo da área de trabalho é habilitado com a [ativação do WinRT sem registro](https://blogs.windows.com/windowsdeveloper/2019/04/30/enhancing-non-packaged-desktop-apps-using-windows-runtime-components/), disponível no Windows 10, versão 1903 e versões posteriores. Isso também poderá exigir o uso do pacote [Encaminhadores VCRT](https://www.nuget.org/packages/Microsoft.VCRTForwarders.140/), se o componente tiver sido criado para direcionar aplicativos UWP.

O C#/WinRT também fornecerá um caminho de fallback de ativação se o Windows não conseguir ativar o tipo, conforme descrito acima. Nesse caso, o C#/WinRT tenta localizar uma DLL de implementação nativa com base no nome do tipo totalmente qualificado, removendo progressivamente os elementos. Por exemplo, a lógica de fallback tentaria ativar o tipo Contoso.Controls.Widget dos seguintes módulos, em sequência:

1. Contoso.Controls.Widget.dll
2. Contoso.Controls.dll
3. Contoso.dll

O C#/WinRT usa a [ordem de pesquisa alternativa LoadLibrary](https://docs.microsoft.com/windows/win32/dlls/dynamic-link-library-search-order?#alternate-search-order-for-desktop-applications) para localizar uma DLL de implementação. Um aplicativo que dependa desse comportamento de fallback deverá empacotar a DLL de implementação juntamente com o módulo do aplicativo.

## <a name="known-issues"></a>Problemas conhecidos

Há alguns problemas de desempenho conhecidos relacionados à interoperabilidade na versão prévia atual do C#/WinRT. Eles serão resolvidos antes da versão final no segundo semestre de 2020.

Se você encontrar problemas funcionais com o pacote do NuGet do C#/WinRT, o compilador cswinrt.exe ou as fontes de projeção geradas, envie os problemas para nós por meio da [página de problemas do C#/WinRT](https://github.com/microsoft/CsWinRT/issues).

## <a name="additional-resources"></a>Recursos adicionais

* [Repositório do GitHub do C#/WinRT](https://aka.ms/cswinrt/repo)
