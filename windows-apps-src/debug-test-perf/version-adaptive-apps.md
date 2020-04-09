---
title: Aplicativos adaptáveis de versão
description: Saiba como tirar proveito das novas APIs e manter a compatibilidade com versões anteriores
ms.date: 05/17/2019
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: b947d0b6cc83dc6bca45efb7103a933e79972e3b
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317457"
---
# <a name="version-adaptive-apps-use-new-apis-while-maintaining-compatibility-with-previous-versions"></a>Aplicativos adaptáveis de versão: use as novas APIs e mantenha a compatibilidade com versões anteriores

Cada versão do SDK do Windows 10 adiciona novas funcionalidades interessantes que você vai querer aproveitar. No entanto, nem todos os seus clientes atualizarão seus dispositivos para a versão mais recente do Windows 10 ao mesmo tempo, e você quer garantir que seu aplicativo funcione na maior variedade possível de dispositivos. Aqui, mostraremos como projetar seu aplicativo de modo que ele seja executado em versões anteriores do Windows 10, mas também tire proveito dos novos recursos, sempre que o aplicativo for executado em um dispositivo com a atualização mais recente instalada.

Há três etapas que devem ser seguidas a fim de garantir que o aplicativo seja compatível com a mais ampla gama de dispositivos Windows 10.

- Primeiro, configure seu projeto do Visual Studio para acessar as APIs mais recentes. Isso afeta o que acontece quando você compila o aplicativo.
- Segundo, execute verificações de runtime para garantir que você só chama APIs que estão presentes no dispositivo em que seu aplicativo esteja sendo executado.
- Terceiro, teste seu aplicativo na versão mínima e na versão de destino do Windows 10.

## <a name="configure-your-visual-studio-project"></a>Configurar seu projeto no Visual Studio

A primeira etapa para dar suporte a várias versões do Windows 10 é especificar as versões de *Destino* e *Mínima* com suporte do SO/SDK em seu projeto do Visual Studio.

- *Target*: a versão do SDK na qual o Visual Studio compila o código do aplicativo e executa todas as ferramentas. Todas as APIs e recursos nesta versão do SDK estão disponíveis no código do aplicativo no momento da compilação.
- *Mínimo*: versão do SDK que dá suporte à versão mais antiga do sistema operacional na qual seu aplicativo possa ser executado (e que será implantado pela loja) e a versão que o Visual Studio compila o código de marcação do aplicativo. 

Durante o runtime, o aplicativo será executado com a versão do sistema operacional que for implantado, portanto o aplicativo gerará exceções se você usar recursos ou chamar as APIs que não estejam disponíveis nessa versão. Mostramos a você como usar verificações de runtime para chamar as APIs corretas mais adiante neste artigo.

As configurações de Destino e Mínima especificam os términos de um intervalo das versões do SO/SDK. No entanto, se você testar o aplicativo na versão mínima, pode ter certeza de que ele será executado em qualquer versão entre a Mínima e a de Destino.

> [!TIP]
> O Visual Studio não avisa você sobre compatibilidade de API. É sua responsabilidade testar e garantir que o aplicativo tenha o desempenho esperado entre todas as versões de sistema operacional, incluindo a Mínima e a de Destino.

Quando você cria um novo projeto no Visual Studio 2015, Atualização 2 ou posterior, você será solicitado a definir as versões de Destino e Mínima que são compatíveis com seu aplicativo. Por padrão, a versão de Destino é a maior versão instalada do SDK e a versão Mínima é a menor versão instalada do SDK. Você só poderá escolher as versões de Destino e Mínima de acordo com as versões do SDK que estão instaladas em seu computador. 

![Definir o SDK de destino no Visual Studio](images/vs-target-sdk-1.png)

Normalmente, recomendamos que você deixe os padrões. Entretanto, se você tiver uma versão Preview do SDK instalada e estiver escrevendo um código para produção, altere a versão de Destino do SDK do Preview para a última versão oficial do SDK. 

Para alterar a versão Mínima e de Destino de um projeto que já tenha sido criado no Visual Studio, vá para Projeto -> Propriedades -> guia Aplicativo -> Direcionamento.

![Alterar o SDK de destino no Visual Studio](images/vs-target-sdk-2.png)

Para referência, a tabela a seguir mostra os números de build para cada SDK.

| Nome amigável | Versão | Build do sistema operacional/SDK |
| ---- | ---- | ---- |
| RTM | 1507 | 10240 |
| Atualização de novembro | 1511 | 10586 |
| Atualização de aniversário | 1607 | 14393 |
| Atualização para criadores | 1703 | 15063 |
| Fall Creators Update | 1709 | 16299 |
| Atualização de abril de 2018 | 1803 | 17134 |
| Atualização de outubro de 2018 | 1809 | 17763 |
| Atualização de maio de 2019 | 1903 | 18362 |

Você pode baixar qualquer versão do SDK do [arquivo de emulador e do SDK do Windows](https://developer.microsoft.com/windows/downloads/sdk-archive). Você pode baixar o SDK mais recente do Windows Insider Preview na seção de desenvolvedor do site [Windows Insider](https://insider.windows.com/for-developers/).

 Para obter mais informações sobre as atualizações do Windows 10, confira [Informações sobre versões do Windows 10](https://www.microsoft.com/itpro/windows-10/release-information). Para obter informações importantes sobre o ciclo de vida de suporte do Windows 10, confira a [Ficha informativa do ciclo de vida do Windows](https://support.microsoft.com/help/13853/windows-lifecycle-fact-sheet).

## <a name="perform-api-checks"></a>Executar verificações de API

A chave para aplicativos adaptáveis de versão é a combinação dos contratos de API e a classe [ApiInformation](https://docs.microsoft.com/uwp/api/windows.foundation.metadata.apiinformation). Essa classe permite detectar se um contrato de API, tipo ou membro especificado está presente para que você possa fazer chamadas de API com segurança em uma variedade de dispositivos e versões de sistema operacional.

### <a name="api-contracts"></a>Contratos de API

O conjunto de APIs dentro de uma família de dispositivos é separado em subdivisões conhecidas como contratos de API. Você pode usar o método **ApiInformation.IsApiContractPresent** para testar a presença de um contrato de API. Isso é útil se você deseja testar a presença de muitas APIs na mesma versão de um contrato de API.

```csharp
    bool isScannerDeviceContract_1_Present =
        Windows.Foundation.Metadata.ApiInformation.IsApiContractPresent
            ("Windows.Devices.Scanners.ScannerDeviceContract", 1);
```

O que é um contrato de API? Basicamente, um contrato de API representa um recurso – um conjunto de APIs relacionadas que, juntas, oferecem uma funcionalidade específica. Um contrato de API hipotético pode representar um conjunto de APIs que contém duas classes, cinco interfaces, uma estrutura, duas enumerações e assim por diante.

Tipos logicamente relacionados são agrupados em um contrato de API e, a partir do Windows 10, cada API do Windows Runtime é um membro de algum contrato de API. Com contratos de API, você verifica a disponibilidade de uma API ou recurso específico no dispositivo, conferindo efetivamente as funcionalidades de um dispositivo, em vez de verificar um dispositivo ou sistema operacional específico. Uma plataforma que implementa qualquer API em um contrato de API é necessária para implementar cada API nesse contrato. Isso significa que você pode testar se o sistema operacional em execução dá suporte a um contrato de API específico e, em caso afirmativo, chamar qualquer uma das APIs desse contrato sem verificar cada uma delas individualmente.

O maior e mais usado contrato de API é o **Windows.Foundation.UniversalApiContract**. Ele contém a maioria das APIs da Plataforma Universal do Windows. A documentação [Contratos de API e SDKs de extensão de família de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/) descreve a variedade de contratos de API disponíveis. Você verá que a maioria deles representa um conjunto de APIs relacionadas funcionalmente.

> [!NOTE]
> Se você tiver instalado um Software Development Kit do Windows (SDK do Windows) em versão prévia que ainda não foi documentado, também poderá encontrar informações sobre o suporte a contrato de API no arquivo "Platform.xml", localizado na pasta de instalação do SDK em "\(Arquivos de Programas (x86))\Windows Kits\10\Platforms\<platform>\<SDK version>\Platform.xml".

### <a name="version-adaptive-code-and-conditional-xaml"></a>Código adaptável de versão e XAML condicional

Em todas as versões do Windows 10, é possível usar a classe ApiInformation em uma condição no código para testar a presença da API que você deseja chamar. Em seu código adaptável, você pode usar vários métodos da classe, como IsTypePresent, IsEventPresent, IsMethodPresent e IsPropertyPresent, para testar as APIs na granularidade necessária.

Para obter mais informações e exemplos, confira **[Código adaptável de versão](version-adaptive-code.md)** .

Se a versão mínima do seu aplicativo for o build 15063 (atualização para criadores) ou posterior, será possível usar *XAML condicional* para definir propriedades e instanciar objetos na marcação sem a necessidade de usar code-behind. A XAML condicional fornece uma forma de usar o método ApiInformation.IsApiContractPresent na marcação.

Para saber mais e ver exemplos, confira **[XAML condicional](conditional-xaml.md)** .

## <a name="test-your-version-adaptive-app"></a>Testar seu aplicativo adaptável de versão

Ao usar código adaptável de versão ou XAML condicional para escrever um aplicativo adaptável de versão, é preciso testá-lo em um dispositivo que execute a versão mínima e em um dispositivo que execute a versão de destino do Windows 10.

Não é possível testar todos os caminhos de códigos condicionais em um único dispositivo. Para garantir que todos os caminhos de código sejam testados, você precisa implantar e testar seu aplicativo em um dispositivo remoto (ou máquina virtual) que execute a versão mínima do sistema operacional compatível.
Para obter mais informações sobre depuração remota, confira [Implantar e depurar aplicativos UWP](deploying-and-debugging-uwp-apps.md).

## <a name="related-articles"></a>Artigos relacionados

- [O que é um aplicativo UWP](https://docs.microsoft.com/windows/uwp/get-started/universal-application-platform-guide)
- [Detectar recursos dinamicamente com contratos de API](https://blogs.windows.com/buildingapps/2015/09/15/dynamically-detecting-features-with-api-contracts-10-by-10/)
- [Contratos de API](https://channel9.msdn.com/Events/Build/2015/3-733) (Vídeo do build 2015)
