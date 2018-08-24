---
author: rido-min
Description: This guide explains how to configure your Visual Studio Solution to optimize the application binaries with native images.
Search.Product: eADQiWindows 10XVcnh
title: Otimizar seus aplicativos de área de trabalho do .NET com imagens nativas
ms.author: normesta
ms.date: 06/11/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, imagem nativa compilador
ms.localizationpriority: medium
ms.openlocfilehash: d98b576fb51a8f9507802796ab359d0d00d21998
ms.sourcegitcommit: c6d6f8b54253e79354f8db14e5cf3b113a3e5014
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/24/2018
ms.locfileid: "2839088"
---
# <a name="optimize-your-net-desktop-apps-with-native-images"></a>Otimizar seus aplicativos de área de trabalho do .NET com imagens nativas

> [!NOTE]
> Algumas informações estão relacionadas a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não oferece nenhuma garantia, explícita ou implícita, com relação às informações fornecidas aqui.

Você pode melhorar o tempo de inicialização do seu aplicativo do .NET Framework, pré-compilando seus binários. Você pode usar esta tecnologia nos aplicativos grandes que você empacotar e distribuir por meio da loja do Windows. Em alguns casos, tiver observamos uma melhoria de desempenho de 20%. Você pode aprender mais sobre esta tecnologia na [Visão geral técnica](https://github.com/dotnet/coreclr/blob/master/Documentation/botr/readytorun-overview.md).

Podemos tiver lançado uma versão de avaliação do compilador imagem nativa como um [pacote do NuGet](https://www.nuget.org/packages/Microsoft.DotNet.Framework.NativeImageCompiler). Você pode aplicar este pacote a qualquer aplicativo do .NET Framework que tem como destino o .NET Framework versão 4.6.2 ou posterior. Esse pacote adiciona uma etapa de compilação de postagem que inclui uma carga nativa para todos os binários usados por seu aplicativo. Essa carga otimizada será carregada quando o aplicativo é executado no .NET 4.7.2 e acima enquanto versões anteriores ainda carregará o código MSIL.

O [.NET framework 4.7.2](https://blogs.msdn.microsoft.com/dotnet/2018/04/30/announcing-the-net-framework-4-7-2/) está incluído no [2018 10 de abril de Windows update](https://blogs.windows.com/windowsexperience/2018/04/30/how-to-get-the-windows-10-april-2018-update/). Você também poderá instalar essa versão do .NET Framework no PC que executam o Windows 7 + e Windows Server 2008 R2 +.

> [!IMPORTANT]
> Se você quiser produzir imagens nativas para seu aplicativo empacotados pelo projeto de empacotamento de aplicativo do Windows, certifique-se definir a versão mínima de plataforma de destino do projeto para a atualização de aniversário no Windows.

## <a name="how-to-produce-native-images"></a>Como produzir imagens nativas

Siga estas instruções para configurar seus projetos.

1. Configurar a estrutura de destino como 4.6.2 ou acima

2. Configurar a plataforma de destino como x86 ou x64 

3. Adicione os pacotes do NuGet.

4. Crie um Build de lançamento.

## <a name="configure-the-target-framework-as-462-or-above"></a>Configurar a estrutura de destino como 4.6.2 ou acima

Para configurar seu projeto de destino do .NET Framework 4.6.2, você precisará das ferramentas de desenvolvimento do .NET Framework 4.6.2 ou mais recente. Essas ferramentas estão disponíveis por meio do instalador do Visual Studio, como componentes opcionais sob a carga de trabalho de área de trabalho de desenvolvimento do .NET:

![Instalar o .NET 4.6.2 ferramentas de desenvolvimento](images/desktop-to-uwp/install-4.6.2-devpack.png)

Como alternativa, você pode obter os pacotes de desenvolvedor do .NET de:[https://www.microsoft.com/net/download/visual-studio-sdks](https://www.microsoft.com/net/download/visual-studio-sdks)

## <a name="configure-the-target-platform-as-x86-or-x64"></a>Configurar a plataforma de destino como x86 ou x64

O compilador de imagem nativa otimiza o código para uma plataforma de determinado. Para usá-lo, você precisará configurar seu aplicativo para uma plataforma específica, como x86 ou x64 de destino.

Se você tiver vários projetos em sua solução, apenas o entrada ponto projeto (mais provável o projeto que produz um arquivo executável) deve ser compilado como x86 ou x64. Referenciado de projeto principal de binários adicionais serão processados com a arquitetura especificada no projeto principal, mesmo se eles são compilados como AnyCPU.

Para configurar seu projeto:

1. Clique com o botão sua solução e selecione **Configuration Manager**.

2. Selecione **< novos.. >** no menu suspenso de **plataforma** ao lado do nome do projeto que produz seu arquivo executável.

3. Na caixa de diálogo **Nova plataforma de projeto** , certifique-se de que a lista suspensa **Configurações de cópia do** está definida como **Qualquer CPU**.

![Configurar x86](images/desktop-to-uwp/configure-x86.png)

Repita essa etapa para `Release/x64` se você quiser produzir x64 binários.

>[!IMPORTANT]
> Configuração de AnyCPU não é suportada pelo compilador imagem nativa.

## <a name="add-the-nuget-packages"></a>Adicione os pacotes do NuGet

O compilador de imagem nativa é fornecido como um pacote de NuGet que você precisa adicionar ao projeto do Visual Studio que produz o arquivo executável. Esse é normalmente seu projeto do Windows Forms ou WPF. Use este comando do PowerShell para fazer isso.

```PS
PM> Install-Package Microsoft.DotNet.Framework.NativeImageCompiler -Version 0.0.1-prerelease-00002  -PRE
```

> [!NOTE]
> Os pacotes de visualização são publicados em NuGet.org como não listados. Você não encontrará-los NuGet.org navegação ou usando o Gerenciador de pacotes de UI no Visual Studio. No entanto, você poderá instalá-los no Console do Gerenciador de pacotes e quando você restaurar a partir de uma máquina diferente. Será feita os pacotes totalmente acessíveis quando publicamos a primeira versão de não-preview.

## <a name="create-a-release-build"></a>Criar um Build de lançamento

O pacote NuGet configura o projeto para executar a ferramenta adicional para compilações lançadas. Essa ferramenta adiciona o código nativo para os mesmos binários.
Para verificar se a ferramenta tiver processado os binários, é possível revisar a compilação de saída para certificar-se de que inclui uma mensagem como esta:

```
Native image obj\x86\Release\\R2R\DesktopApp1.exe generated successfully.
```

## <a name="faq"></a>Perguntas frequentes

**Q. Os novos binários funcionam em máquinas sem o .NET Framework 4.7.2?**

A. Otimizada binários serão beneficiados com as melhorias ao executar com o .NET Framework 4.7.2. Clientes que executam versões anteriores do .NET framework carregará o código MSIL não otimizados do binário.

**Q. Como fornecer comentários ou relatar problemas?**

A. Relate algum problema usando a ferramenta de comentários no 2017 do Visual Studio. [Obter mais informações](https://docs.microsoft.com/visualstudio/ide/how-to-report-a-problem-with-visual-studio-2017).

**Q. Qual é o impacto da adição de imagem nativa para os binários existentes?**

A. Os binários otimizados contêm o código gerenciado e nativo, tornando os arquivos finais maior.

**Q. Posso liberar binários usando esta tecnologia?**

A. Esta versão inclui uma licença de ativação que você pode usar hoje.
