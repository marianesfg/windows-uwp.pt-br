---
author: QuinnRadich
title: Escolher uma versão do UWP
description: Ao escrever um aplicativo UWP no Microsoft Visual Studio, você pode escolher para qual versão você o escreverá. Saiba mais sobre a diferença entre as diferentes versões de UWP e como configurar suas escolhas em projetos novos e existentes.
ms.author: quradic
ms.date: 10/02/2018
ms.topic: article
keywords: windows 10, uwp, versão, build, versões, windows, escolher, atualizar, atualizações
ms.assetid: a8b7830f-4929-44c6-90be-91f38be5f364
ms.localizationpriority: medium
ms.openlocfilehash: 2e2b241d0369d50e600a5497811ac7d4bbb823bc
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5969384"
---
# <a name="choose-a-uwp-version"></a>Escolher uma versão do UWP

Cada versão do Windows 10 trouxe recursos novos e aprimorados para a plataforma UWP. Ao criar um aplicativo UWP no Microsoft Visual Studio, você pode escolher qual a versão alvo. Os projetos que usam [.NET Standard 2.0](https://docs.microsoft.com/dotnet/standard/net-standard) devem ter uma **Versão Mínima** do Build 16299 ou posterior.

> [!WARNING]
> Projetos UWP criados em versões atuais do Visual Studio não podem ser abertos no Visual Studio 2015.

A tabela a seguir descreve as versões disponíveis do Windows 10. Observe que esta tabela só se aplica para a construção de aplicativos UWP, que são suportados apenas no Windows 10. Você não pode desenvolver aplicativos UWP para versões mais antigas do Windows, e você deve ter [instalado a compilação apropriada do SDK](http://go.microsoft.com/fwlink/?LinkId=821431) para segmentar essa versão. 

| Versão | Descrição |
| --- | --- |
| Compilação 17763 (versão 1809) | Esta é a versão mais recente do Windows 10, lançada em outubro de 2018. **Observe que você _deve_ usar o Visual Studio 2017 para segmentar essa versão do Windows.** Alguns recursos em destaque desta versão incluem: </br> \* **Windows Machine Learning:** Windows Machine Learning tem agora oficialmente iniciado, fornecendo recursos como a avaliação mais rápida e suporte para modelos de aprendizado de máquina de última geração. Para saber mais sobre a plataforma, consulte [Windows Machine Learning](https://docs.microsoft.com/windows/ai/). </br> \* **Design fluente:** Novos recursos, como a barra de menus, submenu da barra de comandos e animações de propriedade XAML foram adicionados ao Windows 10. Veja as últimas novidades na [Visão geral do design fluente](../design/fluent-design-system/index.md). </br> Para obter informações sobre estes e muitos outros recursos adicionados nesta versão do Windows, visite [O Centro de desenvolvimento](https://developer.microsoft.com/windows/windows-10-for-developers) ou a página mais detalhada sobre [quais são as novidades no Windows 10 para desenvolvedores](../whats-new/windows-10-build-17763.md)
| Build 17134 (versão 1803) | Isso é a versão do Windows 10 foi lançada em abril de 2018. **Observe que você _deve_ usar o Visual Studio 2017 para segmentar essa versão do Windows.** Alguns recursos em destaque desta versão incluem: </br> \* **Design Fluente:** novos recursos, como a exibição em árvore, deslizar para atualizar e a exibição de navegação foram adicionados ao Windows 10. Veja as últimas novidades na [Visão geral do design fluente](../design/fluent-design-system/index.md). </br> \* **Aplicativos UWP do console:** agora você pode escrever apps de console UWP C++ /WinRT ou /CX que são executados em uma janela de console, como a janela de console do DOS ou do PowerShell. </br> Para obter informações sobre estes e muitos outros recursos adicionados nesta versão do Windows, visite [o Centro de Desenvolvimento](https://developer.microsoft.com/windows/windows-10-for-developers) ou a página mais detalhada sobre [Novidades no Windows 10 para desenvolvedores](../whats-new/windows-10-build-17134.md)
| Build 16299 (Fall Creators Update, versão 1709) | Esta versão do Windows 10 foi lançada em outubro de 2017. **Observe que você _deve_ usar o Visual Studio 2017 para segmentar essa versão do Windows.** Alguns recursos em destaque desta versão incluem: </br> \* **.NET Standard 2.0:** Aproveite um grande aumento no número de APIs .NET e incorpore seus pacotes NuGet favoritos e bibliotecas de terceiros ao .NET Standard. Veja mais detalhes e explore a documentação [aqui](https://docs.microsoft.com/dotnet/standard/net-standard). Observe que você deve definir sua **versão mínima** como Build 16299 para acessar essas novas APIs. </br> \* **Design Fluente:** Use luz, profundidade, perspectiva e movimento para aprimorar seu app e ajudar os usuários a se concentrarem nos elementos importantes da interface de usuário. </br> \* **XAML condicional:** defina com facilidade propriedades e instancie objetos com base na presença de uma API em tempo de execução, permitindo que seus aplicativos sejam executados perfeitamente em todos os dispositivos e versões. </br> Para obter informações sobre estes e muitos outros recursos adicionados nesta versão do Windows, visite [o Centro de Desenvolvimento](https://developer.microsoft.com/windows/windows-10-for-developers) ou a página mais detalhada sobre [Novidades no Windows 10 para desenvolvedores](../whats-new/windows-10-build-16299.md)
| Build 15063 (Creators Update, versão 1703) | Esta versão do Windows 10 foi lançada em março de 2017. **Observe que você _deve_ usar o Visual Studio 2017 para segmentar essa versão do Windows**. Alguns recursos em destaque desta versão incluem:  </br> \* **Análise do Ink:** O Windows Ink agora pode categorizar traços de tinta dentro dos traços de escrita ou desenho, e reconhecer texto, formas e estruturas básicas de layout. </br> \* **Windows.Ui.Composition APIs:** Combine e aplique com facilidade animações em seu aplicativo. </br> \* **Edição ao Vivo:** Edite XAML enquanto seu aplicativo está em execução, e visualize as mudanças aplicadas em tempo real. </br> Para obter informações sobre estes e muitos outros recursos adicionados nesta versão do Windows, visite [o Centro de Desenvolvimento](https://developer.microsoft.com/windows/windows-10-for-developers) ou a página mais detalhada sobre [Novidades no Windows 10 para desenvolvedores](../whats-new/windows-10-build-15063.md)  |
| Build 14393 (Atualização de Aniversário, versão 1607) | Esta versão do Windows 10 foi lançada em julho de 2016. Alguns recursos em destaque desta versão incluem: </br> \* **Windows Ink:** Novos controles InkCanvas e InkToolbar. </br> \* **APIs da Cortana:** Use as novas ações da Cortana para integrar o suporte da Cortana com funções específicas do seu aplicativo. </br> \* **Windows Hello:** O Microsoft Edge agora oferece suporte ao Windows Hello, dando aos desenvolvedores da Web acesso à autenticação biométrica. </br> Para obter informações sobre estes e muitos outros recursos adicionados nesta versão do Windows, visite [o Centro de Desenvolvimento](https://developer.microsoft.com/windows/windows-10-for-developers) ou a página mais detalhada sobre [Novidades no Windows 10 para desenvolvedores](../whats-new/windows-10-build-14393.md)  |
| Build 10586 (Atualização de novembro, versão 1511) | Esta versão do Windows 10 foi lançada em novembro de 2015. Os recursos destacados incluem a introdução das APIs de ORTC (comunicações em tempo real do objeto) para a comunicação de vídeo no Microsoft Edge e as APIs de provedores para permitir que os aplicativos usem a autenticação de rosto do Windows Hello. [Mais informações sobre os recursos introduzidos nesta compilação.](../whats-new/windows-10-build-10586.md) |
| Build 10240 (Windows 10, versão 1507) | Esta é a versão de lançamento inicial do Windows 10, em julho de 2015. [Mais informações sobre os recursos introduzidos nesta compilação.](../whats-new/windows-10-build-10240.md) |

É altamente recomendável que os novos desenvolvedores e aqueles que escrevem o código para o público em geral sempre usem a compilação mais recente do Windows (17763). Os desenvolvedores que escrevem apps Corporativos devem considerar o suporte a uma **Versão Mínima** mais antiga.

## <a name="whats-different-in-each-uwp-version"></a>O que está diferente em cada versão do UWP?

APIs novas e alteradas para UWP estão disponíveis em todas as versões sucessivas do Windows 10. Para obter informações específicas sobre quais recursos foram adicionados a qual versão, consulte [Novidades para desenvolvedores no Windows 10](../whats-new/windows-10-version-latest.md).

Para tópicos de referência que enumeram todas as famílias de dispositivos e suas versões e todos os contratos de API e suas versões, consulte [Famílias de dispositivos](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) e [Contratos de API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## <a name="net-api-availability-in-uwp-versions"></a>Disponibilidade de API .NET nas versões UWP

UWP dá suporte a um subconjunto limitado de APIs .NET, que estão disponíveis, independentemente do **Versão de destino** ou uma **Versão mínima** do seu projeto. [Esta página fornece mais informações sobre os tipos disponíveis](https://msdn.microsoft.com/library/windows/apps/xaml/mt185501(d=robot).aspx).

Se você quiser criar bibliotecas de plataforma cruzada reutilizáveis, .NET Standard é compatível com UWP. A [documentação do .NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) fornece informações no qual .NET Standard é compatível com quais versões UWP.

Se você estiver desenvolvendo um aplicativo da área de trabalho, consulte em vez disso, [as dependências e versões do .NET Framework](https://docs.microsoft.com/dotnet/framework/migration-guide/versions-and-dependencies) para obter informações detalhadas sobre a disponibilidade do .NET framework.

## <a name="choose-which-version-to-use-for-your-app"></a>Escolha qual versão deve ser usada para o seu aplicativo

No diálogo **Novo Projeto Universal do Windows** no Visual Studio, você pode escolher uma versão para **Versão de Destino** e para **Versão Mínima**. Além disso, você pode alterar a **Versão de Destino** e a **Versão Mínima** do seu aplicativo UWP na seção *aplicação* nas **Propriedades** do aplicativo.

* **Versão de Destino**. Isso define a configuração *TargetPlatformVersion* no seu arquivo de projeto. Também determina o valor do atributo *TargetDeviceFamily@MaxVersionTested* no manifesto do pacote do app. O valor que você escolher especifica a versão da plataforma UWP a qual seu projeto se destina — e, portanto, o conjunto de APIs disponíveis para seu aplicativo — portanto, recomendamos que você escolha a versão mais recente possível. Para saber mais sobre o manifesto do pacote de aplicativo e algumas diretrizes sobre como configurar a TargetDeviceFamily manualmente, consulte [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Versão mínima**. Isso define a configuração *TargetPlatformMinVersion* no seu arquivo de projeto. Também determina o valor do atributo *TargetDeviceFamily@MinVersion* no manifesto do pacote do app. O valor que você escolher especifica a versão mínima da plataforma UWP com a qual seu projeto pode trabalhar.

Lembre-se de que você está declarando que seu aplicativo funciona em qualquer versão do Windows no intervalo de **Versão mínima** até **Versão de destino**. Se as duas forem da mesma versão, então você não precisará fazer nada especial. Se eles forem diferentes, então aqui estão algumas coisas que devem ser lembradas.

* Em seu código, você pode livremente (ou seja, sem verificações de condição) chamar qualquer API que exista na versão especificada pela **Versão mínima**.
* Certifique-se de testar seu código em um dispositivo que executa a **Versão mínima** para ter certeza de que ele funciona sem a necessidade de APIs presentes apenas na **Versão de destino**.
* O valor de **Versão de destino** é usado para identificar todas as referências (contrato winmds) usadas para compilar seu projeto. Mas essas referências permitirão que você compile o código com chamadas para APIs que não existem necessariamente em dispositivos para os quais você declarou que oferece suporte (via **Versão mínima**). Portanto, qualquer API introduzida após a **Versão mínima** precisará ser chamada por meio do código adaptável. Para saber mais sobre o código adaptável, consulte [Código adaptável de versão](https://docs.microsoft.com/windows/uwp/debug-test-perf/version-adaptive-code).