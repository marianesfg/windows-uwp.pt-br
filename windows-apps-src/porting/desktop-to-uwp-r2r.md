---
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: Otimizar seus aplicativos de área de trabalho .NET com imagens nativas
ms.date: 06/11/2018
ms.topic: article
keywords: Windows 10, imagem nativa compilador
ms.localizationpriority: medium
ms.openlocfilehash: 3071b843a1605d765ab5b087d5e1bfb96a220218
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7844865"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Otimizar seus aplicativos de área de trabalho .NET com imagens nativas

> [!NOTE]
> Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

Você pode melhorar o tempo de inicialização do seu aplicativo .NET Framework compilando previamente os binários. Você pode usar essa tecnologia em grandes aplicativos que você empacotar e distribuir por meio da Microsoft Store. Em alguns casos, você observamos uma melhoria no desempenho de 20%. Você pode saber mais sobre essa tecnologia na [Visão geral técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Já lançamos uma versão prévia do compilador imagem nativa como um [pacote NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Você pode aplicar esse pacote para qualquer aplicativo do .NET Framework que direciona o .NET Framework versão 4.6.2 ou posterior. Esse pacote adiciona uma etapa de compilação de post que inclui uma carga nativa para todos os binários usados pelo seu aplicativo. Essa carga otimizada será carregada quando o aplicativo é executado em .NET 4.7.2 e acima enquanto versões anteriores ainda carregará o código MSIL.

O [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) está incluído na [atualização do Windows 10 de abril de 2018](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Você também pode instalar esta versão do .NET Framework no computador que executam o Windows 7 + e Windows Server 2008 R2 +.

> [!IMPORTANT]
> Se você deseja produzir imagens nativas para seu aplicativo empacotado pelo projeto de empacotamento de aplicativo do Windows, certifique-se de definir a versão mínima da plataforma de destino do projeto para a atualização de aniversário do Windows.

## <a name="how-to-produce-native-images"></a>Como produzir imagens nativas

Siga estas instruções para configurar seus projetos.

1. Configurar a estrutura de destino como 4.6.2 ou acima

2. Configurar a plataforma de destino como x86 ou x64 

3. Adicione os pacotes NuGet.

4. Crie uma compilação de versão.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar a estrutura de destino como 4.6.2 ou acima

Para configurar seu projeto para o destino do .NET Framework 4.6.2, você precisará das ferramentas de desenvolvimento do .NET Framework 4.6.2 ou mais recente. Essas ferramentas estão disponíveis por meio do instalador do Visual Studio como componentes opcionais sob a carga de trabalho de desenvolvimento da área de trabalho do .NET:

![Instale o .NET 4.6.2 ferramentas de desenvolvimento](images/desktop-to-uwp/install-4.6.2-devpack.png)

Como alternativa, você pode obter os pacotes de desenvolvedor do .NET de:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar a plataforma de destino como x86 ou x64

O compilador de imagem nativa otimiza o código para um determinado plataforma. Para usá-lo, você precisará configurar seu aplicativo para direcionar uma plataforma específica, como x86 ou x64.

Se você tiver vários projetos em sua solução, somente entrada ponto projeto (provavelmente o projeto que produz um arquivo executável) deve ser compilado como x86 ou x64. Binários adicionais referenciados do projeto principal serão processados com a arquitetura especificada no projeto principal, mesmo se eles são compilados como AnyCPU.

Para configurar seu projeto:

1. Clique com botão direito sua solução e, em seguida, selecione **Configuration Manager**.

2. Selecione **<New.. >** no menu suspenso de **plataforma** ao lado do nome do projeto que produz seu arquivo executável.

3. Na caixa de diálogo **Novo projeto de plataforma** , certifique-se de que a lista suspensa de **Configurações de cópia de** é definida como **Any CPU**.

![Configurar x86](images/desktop-to-uwp/configure-x86.png)

Repita esta etapa para `Release/x64` se você quiser produzir x64 binários.

>[!IMPORTANT]
> Configuração de AnyCPU não é compatível com o compilador de imagem nativa.

## <a name="add-the-nuget-packages"></a>Adicione os pacotes NuGet

O compilador de imagem nativa é fornecido como um pacote NuGet que você precisará adicionar ao projeto do Visual Studio que produz o arquivo executável. Isso normalmente é o projeto Windows Forms ou WPF. Use este comando do PowerShell para fazer isso.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Os pacotes de visualização são publicados em NuGet.org como não listado. Você não encontrá-los por NuGet.org navegação ou usando o Gerenciador de pacotes UI no Visual Studio. No entanto, você pode instalá-los no Console do Gerenciador de pacotes e quando você restauração de um computador diferente. Faremos os pacotes totalmente acessíveis quando publicamos a primeira versão de visualização não.

## <a name="create-a-release-build"></a>Criar uma compilação de versão

O pacote NuGet configura o projeto para executar uma ferramenta adicional para compilações de lançamento. Essa ferramenta adiciona o código nativo para os mesmos binários.
Para verificar que a ferramenta processou os binários, você pode revisar a compilação de saída para garantir que ele inclui uma mensagem como este:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Perguntas frequentes

**Q. Os novos binários funcionam em computadores sem o .NET Framework 4.7.2?**

A. Binários otimizados se beneficiará as melhorias ao executar com o .NET Framework 4.7.2. Os clientes que executam versões anteriores do .NET framework carregará o código MSIL não otimizados do binário.

**Q. Como fornecer comentários ou relatar problemas?**

A. Relate um problema com a ferramenta de comentários no Visual Studio 2017. [Obter mais informações](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. Qual é o impacto de adição da imagem nativa binários existentes?**

A. Os binários otimizados contêm o código gerenciado e nativo, tornando os arquivos finais maior.

**Q. Poderá liberar binários usando essa tecnologia?**

A. Esta versão inclui uma licença de ativação que você pode usar hoje.
