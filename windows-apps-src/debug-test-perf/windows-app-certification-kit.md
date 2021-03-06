---
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Kit de Certificação de Aplicativos Windows
description: Para aumentar as chances de seu aplicativo ser publicado na Microsoft Store ou obter a certificação do Windows, valide e teste-o localmente antes de enviá-lo para certificação. Este tópico mostra como instalar e executar o Kit de Certificação de Aplicativos Windows.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, app certification
ms.localizationpriority: medium
ms.openlocfilehash: 174ff4e588d75293ecb729312883f4792196c87a
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79110099"
---
# <a name="windows-app-certification-kit"></a>Kit de Certificação de Aplicativos Windows

Para fazer com que seu aplicativo seja [Certificado para Windows](/windows/win32/win_cert/windows-certification-portal) ou prepará-lo para [Publicação na Microsoft Store](/windows/uwp/publish/app-submissions), primeiro você precisa validá-lo e testá-lo localmente. Este tópico mostra como instalar e executar o [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) para garantir que o aplicativo seja seguro e eficiente.

## <a name="prerequisites"></a>Pré-requisitos

Pré-requisitos para testar um Aplicativo Universal do Windows:

- Você precisa instalar e executar o Windows 10.
- Você precisa instalar o [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/downloads/app-certification-kit/), incluído no Software Development Kit (SDK) do Windows para o Windows 10.
- Você deve [habilitar o dispositivo para desenvolvimento](/windows/uwp/get-started/enable-your-device-for-development).
- Você deve implantar o aplicativo do Windows que deseja testar no seu computador.

> [!NOTE]
> **Atualizações no local:** a instalação de um [Kit de Certificação de Aplicativos Windows](https://developer.microsoft.com/windows/develop/app-certification-kit) mais recente substituirá qualquer versão anterior do kit que estiver instalada.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Validar seu aplicativo do Windows usando o Kit de Certificação de Aplicativos Windows interativamente

1. No menu **Iniciar**, procure por **Aplicativos**, localize **Kits do Windows** e clique em **Kit de Certificação de Aplicativos Windows**.

2. No Kit de Certificação de Aplicativos Windows, selecione a categoria de validação que você deseja executar. Por exemplo: se você estiver validando um aplicativo do Windows, selecione **Validar um aplicativo do Windows**.

    Você pode navegar diretamente para o aplicativo em teste ou escolher o aplicativo em uma lista na interface do usuário. Quando o Kit de Certificação de Aplicativos Windows é executado pela primeira vez, a interface do usuário lista todos os aplicativos do Windows instalados no computador. Nas execuções subsequentes, a interface do usuário exibirá os aplicativos do Windows que você validou. Se o aplicativo que você deseja testar não estiver listado, clique em **Meu aplicativo não está na lista** para obter uma lista completa de todos os aplicativos instalados no sistema.

3. Depois de inserir ou selecionar o aplicativo que você deseja testar, clique em **Avançar**.

4. Na próxima tela, você verá o fluxo de trabalho de teste que se alinha ao tipo de aplicativo que você está testando. Se um teste estiver esmaecido na lista, ele não é aplicável ao seu ambiente. Por exemplo, se você estiver testando um Windows 10 app no Windows 7, somente os testes estáticos serão aplicadas ao fluxo de trabalho. Observe que a Microsoft Store pode aplicar todos os testes desse fluxo de trabalho. Selecione os testes que você deseja executar e clique em **Avançar**.

    O Kit de Certificação de Aplicativos Windows começa a validar o aplicativo.

5. No prompt após o teste, digite o caminho para a pasta em que deseja salvar o relatório do teste.

    O Kit de Certificação de Aplicativos Windows cria um HTML junto com um relatório XML e salva-o nessa pasta.

6. Abra o arquivo de relatório e examine os resultados do teste.

> [!NOTE]
> Se você usar o Visual Studio, execute o Kit de Certificação de Aplicativos Windows quando criar o pacote do aplicativo. Consulte [Empacotando aplicativos UWP](/windows/msix/package/packaging-uwp-apps) para saber como.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Validar seu aplicativo do Windows usando o Kit de Certificação de Aplicativos Windows a partir de uma linha de comando

> [!IMPORTANT]
> O Kit de Certificação de Aplicativos Windows deve ser executado no contexto de uma sessão do usuário ativa.

1. Na janela de comando, navegue até o diretório que contém o Kit de Certificação de Aplicativos Windows.

    **Observação**   O caminho padrão é C:\\Arquivos de Programas\\Kits do Windows\\10\\Kit de Certificação de Aplicativos\\.

2. Insira os seguintes comandos nesta ordem para testar um aplicativo que já está instalado no computador de teste:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Ou use os comandos a seguir se o aplicativo não estiver instalado. O Kit de Certificação de Aplicativos Windows abrirá o pacote e aplicará o fluxo de trabalho de teste apropriado:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3. Quando o teste for concluído, abra o arquivo de relatório denominado `[report file name]` e examine os resultados.

**Observação**  O Kit de Certificação de Aplicativos Windows pode ser executado em um serviço, mas o serviço deve iniciar o processo do kit dentro de uma sessão de usuário ativa e não pode ser executado em Session0.

**Observação**   Para saber mais sobre a linha de comando do Kit de Certificação de Aplicativos Windows, insira o comando `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Testando com um computador com baixo consumo de energia

O limites do teste de desempenho do Kit de Certificação de Aplicativos Windows são baseados no desempenho de um computador com baixo consumo de energia.

As características do computador em que o teste é realizado podem afetar os resultados do teste. Para determinar se o desempenho do aplicativo atende às [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies), recomendamos que você teste o aplicativo em um computador com baixo consumo de energia, tal como um computador baseado no processador Intel Atom com uma resolução de tela de 1.366 x 768 (ou superior) e um disco rígido rotacional (em vez de um disco rígido de estado sólido).

As características de desempenho podem mudar ao longo do tempo para acompanhar a evolução dos computadores com baixo consumo de energia. Confira as [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies) mais recentes e teste o aplicativo com a versão mais atual do Kit de Certificação de Aplicativos Windows para garantir que ele atenda aos últimos requisitos de desempenho.

## <a name="related-topics"></a>Tópicos relacionados

- [Testes do Kit de Certificação de Aplicativos Windows](windows-app-certification-kit-tests.md)
- [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies)
