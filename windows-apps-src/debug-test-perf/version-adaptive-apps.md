---
title: Aplicativos adaptáveis de versão
description: Saiba como tirar proveito das novas APIs e manter a compatibilidade com versões anteriores
ms.date: 09/17/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 8bbe7875fcd35003b1ec35b02da0f1a86a54ef31
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7842391"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>Aplicativos adaptáveis de versão: use as novas APIs e mantenha a compatibilidade com versões anteriores

Cada versão do SDK do Windows 10 adiciona novas funcionalidades interessantes que você vai querer aproveitar. No entanto, nem todos os seus clientes atualizarão seus dispositivos para a versão mais recente do Windows 10 ao mesmo tempo, e você quer garantir que seu aplicativo funcione na maior variedade possível de dispositivos. Aqui, mostraremos como projetar seu aplicativo de modo que ele seja executado em versões anteriores do Windows 10, mas também tire proveito dos novos recursos, sempre que o aplicativo for executado em um dispositivo com a atualização mais recente instalada.

Há 3 etapas que devem ser seguidas a fim de garantir que o aplicativo seja compatível com a mais ampla gama de dispositivos Windows 10.

- Primeiro, configure seu projeto do Visual Studio para acessar as APIs mais recentes. Isso afeta o que acontece quando você compila o aplicativo.
- Segundo, execute verificações de tempo de execução para garantir que você só chama APIs que estão presentes no dispositivo em que seu aplicativo esteja sendo executado.
- Terceiro, teste seu aplicativo na versão mínima e na versão de destino do Windows 10.

## <a name="configure-your-visual-studio-project"></a>Configurar seu projeto no Visual Studio

A primeira etapa para dar suporte a várias versões do Windows 10 é especificar as versões de *Destino* e *Mínima* com suporte do SO/SDK em seu projeto do Visual Studio.

- *Destino*: a versão do SDK na qual o Visual Studio compila o código do aplicativo e executa todas as ferramentas. Todas as APIs e recursos nesta versão do SDK estão disponíveis no código do aplicativo no momento da compilação.
- *Mínima*: versão do SDK que dá suporte à versão mais antiga do sistema operacional na qual seu aplicativo possa ser executado (e que será implantado pela loja) e a versão que o Visual Studio compila o código de marcação do aplicativo. 

Durante o tempo de execução, o aplicativo será executado com a versão do sistema operacional que for implantado, portanto o aplicativo gerará exceções se você usar recursos ou chamar as APIs que não estejam disponíveis nessa versão. Mostramos a você como usar verificações de tempo de execução para chamar as APIs corretas mais adiante neste artigo.

As configurações de Destino e Mínima especificam os términos de um intervalo das versões do SO/SDK. No entanto, se você testar o aplicativo na versão mínima, pode ter certeza de que ele será executado em qualquer versão entre a Mínima e a de Destino.

> [!TIP]
> O Visual Studio não avisa você sobre compatibilidade de API. É sua responsabilidade testar e garantir que o aplicativo tenha o desempenho esperado entre todas as versões de sistema operacional, incluindo a Mínima e a de Destino.

Quando você cria um novo projeto no Visual Studio 2015, Atualização 2 ou posterior, você será solicitado a definir as versões de Destino e Mínima que são compatíveis com seu aplicativo. Por padrão, a versão de Destino é a maior versão instalada do SDK e a versão Mínima é a menor versão instalada do SDK. Você só poderá escolher as versões de Destino e Mínima de acordo com as versões do SDK que estão instaladas em seu computador. 

![Definir o SDK de destino no Visual Studio](images/vs-target-sdk-1.png)

Normalmente, recomendamos que você deixe os padrões. Entretanto, se você tiver uma versão Preview do SDK instalada e estiver escrevendo um código para produção, altere a versão de Destino do SDK do Preview para a última versão oficial do SDK. 

Para alterar a versão Mínima e de Destino de um projeto que já tenha sido criado no Visual Studio, vá para Projeto -> Propriedades -> guia Aplicativo -> Direcionamento.

![Alterar o SDK de destino no Visual Studio](images/vs-target-sdk-2.png)

Para referência, a tabela a seguir mostra os números de compilação para cada SDK.

| Nome amigável | Versão | Compilação do sistema operacional/SDK |
| ---- | ---- | ---- |
| RTM | 1507 | 10240 |
| Atualização de novembro | 1511 | 10586 |
| Atualização de aniversário | 1607 | 14393 |
| Atualização dos criadores | 1703 | 15063 |
| Atualização do Fall Creators | 1709 | 16299 |
| Atualização de abril de 2018 | 1803 | 17134 |
| Atualização de outubro de 2018 | 1809 | _Insider Preview_ |

Você pode baixar qualquer versão do SDK do [arquivo de emulador e do SDK do Windows](https://developer.microsoft.com/downloads/sdk-archive). Você pode baixar o SDK mais recente do Windows Insider Preview na seção de desenvolvedor do site [Participante do Insider Program](https://insider.windows.com/Home/BuildWithWindows).

 Para obter mais informações sobre atualizações do Windows 10, consulte as [informações de versão do Windows 10](https://technet.microsoft.com/windows/release-info). Para obter informações importantes sobre o Windows 10 do ciclo de vida de suporte, consulte a [planilha de fato de ciclo de vida do Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

## <a name="perform-api-checks"></a>Executar verificações de API

A chave para aplicativos adaptáveis de versão é a combinação dos contratos de API e a classe [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation). Essa classe permite detectar se um contrato de API especificado, tipo ou membro está presente para que você possa fazer, com segurança, chamadas de API em uma variedade de dispositivos e versões de sistema operacional.

### <a name="api-contracts"></a>Contratos de API

O conjunto de APIs dentro de uma família de dispositivos é subdividido em subdivisões conhecidas como contratos de API. Você pode usar o método **ApiInformation.IsApiContractPresent** para testar a presença de um contrato de API. Isso é útil se você deseja testar a presença de muitas APIs que existem na mesma versão de um contrato de API.

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

O que é um contrato de API? Basicamente, um contrato de API representa um recurso – um conjunto de APIs relacionadas que juntas oferecem uma funcionalidade específica. Um contrato de API hipotético poderia representar um conjunto de APIs que contém duas classes, cinco interfaces, uma estrutura, duas enumerações e assim por diante.

Tipos logicamente relacionados são agrupados em um contrato de API, e, a partir do Windows 10, cada API do Windows Runtime é um membro de algum contrato de API. Com contratos de API, você está verificando a disponibilidade de um recurso específico ou uma API no dispositivo, verificando efetivamente as funcionalidades de um dispositivo em vez de verificar um dispositivo específico ou sistema operacional. Uma plataforma que implementa qualquer API em um contrato de API é necessária para implementar cada API nesse contrato de API. Isso significa que você pode testar se o sistema operacional em execução oferece suporte a um contrato de API específico e, em caso afirmativo, chamar qualquer uma das APIs nesse contrato de API sem verificar cada uma delas individualmente.

O maior e mais comumente usado contrato de API é o **Windows.Foundation.UniversalApiContract**. Ele contém a maioria das APIs na Plataforma Universal do Windows. A documentação [Contratos de API e SDKs de extensão de família de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/) descreve a variedade de contratos de API disponíveis. Você verá que a maioria deles representa um conjunto de APIs relacionadas funcionalmente.

> [!NOTE]
> Se tiver um Software Development Kit do Windows (SDK do Windows) de visualização instalado que ainda não foi documentado, você também pode encontrar informações sobre o suporte de contrato de API no arquivo "Platform.xml" localizado na pasta Instalação do SDK em "\(Program Files (x86))\Windows Kits\10\Platforms\<platform>\<SDK version>\Platform.xml".

### <a name="version-adaptive-code-and-conditional-xaml"></a>Código adaptável de versão e XAML condicional

Em todas as versões do Windows 10, é possível usar a classe ApiInformation em uma condição no código para testar a presença da API que você deseja chamar. Em seu código adaptável, você pode usar vários métodos da classe, como IsTypePresent, IsEventPresent, IsMethodPresent e IsPropertyPresent, para testar as APIs na granularidade necessária.

Para obter mais informações e exemplos, consulte **[Código adaptável de versão](version-adaptive-code.md)**.

Se a versão mínima do seu aplicativo for a compilação 15063 (atualização dos criadores) ou posterior, é possível usar *XAML condicional* para definir propriedades e instanciar objetos na marcação sem a necessidade de usar code-behind. A XAML condicional fornece uma forma de usar o método ApiInformation.IsApiContractPresent na marcação.

Para saber mais e ver exemplos, consulte **[XAML condicional](conditional-xaml.md)**.

## <a name="test-your-version-adaptive-app"></a>Testar seu aplicativo adaptável de versão

Ao usar código adaptável de versão ou XAML condicional para escrever um aplicativo adaptável de versão, é preciso testá-lo em dispositivo executando a versão mínima e em um dispositivo executando a versão de destino do Windows 10.

Não é possível testar todos os caminhos de códigos condicionais em um único dispositivo. Para garantir que todos os caminhos de código sejam testados, você precisa implantar e testar seu aplicativo em um dispositivo remoto (ou máquina virtual) executando a versão mínima do sistema operacional compatível.
Para obter mais informações sobre a depuração remota, consulte [Implantando e depurando aplicativos UWP](deploying-and-debugging-uwp-apps.md).

## <a name="related-articles"></a>Artigos relacionados

- [O que é um aplicativo UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detectando dinamicamente recursos com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contratos de API](https://channel9.msdn.com/Events/Build/2015/3-733) (Vídeo da Compilação 2015)