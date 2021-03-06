---
Description: Com as APIs de PRI (índice de recurso do pacote), você pode desenvolver um sistema de build personalizado para recursos do aplicativo UWP. O sistema de compilação será capaz de criar, controlar a versão e despejar os arquivos PRI em qualquer nível de complexidade exigido pelo aplicativo UWP.
title: Indexação de recursos de pacote e sistemas de Build personalizados
template: detail.hbs
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 860455eceef2c61ef0a591fcd791506d9b290af9
ms.sourcegitcommit: 963316e065cf36c17b6360c3f89fba93a1a94827
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/06/2020
ms.locfileid: "82868890"
---
# <a name="package-resource-indexing-pri-apis-and-custom-build-systems"></a>APIs de índice de recurso do pacote (PRI) e sistemas de compilação personalizados
Com as [APIs de PRI (índice de recurso do pacote)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference), você pode desenvolver um sistema de build personalizado para recursos do aplicativo UWP. O sistema de build será capaz de criar, controlar a versão e despejar os arquivos de PRI (índice de recurso do pacote) (como XML) em qualquer nível de complexidade exigido pelo aplicativo UWP. Se você tiver um sistema de compilação personalizado que use atualmente a ferramenta de linha de comando MakePri.exe (consulte [Compilar recursos manualmente com o MakePri.exe](makepri-exe-command-options.md)), para melhorar o desempenho e o controle, é recomendável passar a chamar as APIs de PRI, em vez de chamar o MakePri.exe.

As APIs de PRI foram incorporadas no SDK do Windows para Windows 10, versão 1803. As APIs assumem a forma de APIs do Windows Win32, o que significa que você tem algumas opções para chamá-las. Você pode chamá-las diretamente de um app Win32, por meio de [invocação de plataforma](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live) em um app .NET ou até mesmo em um aplicativo UWP.

Os cenários deste tópico demonstram a chamada de APIs de PRI em um projeto de Aplicativo de Console do Windows Visual C++ Win32. Para obter informações contextuais, consulte [Sistema de Gerenciamento de Recursos](resource-management-system.md).

> [!NOTE]
> Essa limitação dificilmente será um problema, já que você provavelmente não enviará o app do sistema de compilação personalizado para a Microsoft Store. Mas, se você optar por desenvolver o sistema de compilação personalizado sob a forma de um aplicativo UWP, ele será um aplicativo UWP incomum que você não poderá enviar à Microsoft Store. Isso ocorre porque um aplicativo UWP que usa a invocação de plataforma não passa na certificação da Microsoft Store. Observe que, nesse caso, as chamadas de invocação de plataforma *existirão apenas no sistema de compilação personalizado*, e *não* no aplicativo UWP enviado (o aplicativo para o qual você está criando arquivos PRI).

## <a name="scenario-walkthroughs"></a>Passo a passo do cenário
|Tópico|Descrição|
|-|-|
|[Cenário 1: gerar um arquivo PRI a partir de recursos de cadeia de caracteres e arquivos de ativo](pri-apis-scenario-1.md)|Nesse cenário, criaremos um novo app para representar nosso sistema de compilação personalizado. Criaremos um indexador de recurso e adicionaremos cadeias de caracteres e outros tipos de recursos a ele. Em seguida, geraremos e despejaremos um arquivo PRI.|

## <a name="important-apis"></a>APIs importantes
* [Referência ao índice de recurso do pacote (PRI)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference)

## <a name="related-topics"></a>Tópicos relacionados
* [Compilar recursos manualmente com o MakePri.exe](makepri-exe-command-options.md)
* [Consumindo funções de DLL não gerenciadas](/dotnet/framework/interop/consuming-unmanaged-dll-functions?branch=live)
* [Sistema de Gerenciamento de Recursos](resource-management-system.md)
