---
author: PatrickFarley
ms.assetid: 78D833B9-E528-4BCA-9C48-A757F17E6C22
title: Kit de Certificação de Aplicativos Windows
description: Para seu aplicativo as chances de serem publicados na Microsoft Store, ou se tornando certificados do Windows, valide e teste-o localmente antes de enviá-lo para certificação. Este tópico mostra como instalar e executar o Kit de Certificação de Aplicativos Windows.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, certificação de aplicativos
ms.localizationpriority: medium
ms.openlocfilehash: b7a72a89704aa3768cc43cdfbb75b620bae303e3
ms.sourcegitcommit: 2a63ee6770413bc35ace09b14f56b60007be7433
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/12/2018
ms.locfileid: "3935549"
---
# <a name="windows-app-certification-kit"></a>Kit de Certificação de Aplicativos Windows



Para obter o aplicativo [Windows certificados](https://msdn.microsoft.com/windows/desktop/jj134964.aspx) ou prepará-lo para [publicação na Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Hh694062), você deve validar e teste-o localmente primeiro. Este tópico mostra como instalar e executar o [Kit de certificação de aplicativo do Windows](http://go.microsoft.com/fwlink/p/?LinkID=309666) para garantir que seu aplicativo é seguro e eficiente.

## <a name="prerequisites"></a>Pré-requisitos

Pré-requisitos para testar um Aplicativo Universal do Windows:

-   Você deve instalar e executar o Windows 10.
-   Você deve instalar o [Kit de Certificação de Aplicativos Windows versão 10]( http://go.microsoft.com/fwlink/p/?LinkID=309666), incluído no Software Development Kit do Windows (SDK do Windows) para Windows 10.
-   Você deve [habilitar o dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development).
-   Você deve implantar o aplicativo do Windows que deseja testar no seu computador.

**Uma observação sobre atualizações in-loco**

A instalação de um [Kit de Certificação de Aplicativos Windows]( http://go.microsoft.com/fwlink/p/?LinkID=309666) mais recente substituirá qualquer versão anterior do kit que esteja instalada no computador.

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-interactively"></a>Validar seu aplicativo do Windows usando o Kit de Certificação de Aplicativos Windows interativamente

1.  No menu **Iniciar**, procure por **Aplicativos**, localize **Kits do Windows** e clique em **Kit de Certificação de Aplicativos Windows**.

2.  No Kit de Certificação de Aplicativos Windows, selecione a categoria de validação que você deseja executar. Por exemplo: se você estiver validando um aplicativo do Windows, selecione **Validar um aplicativo do Windows**.

    Você pode navegar diretamente para o aplicativo em teste ou escolher o aplicativo em uma lista na interface do usuário. Quando o Kit de Certificação de Aplicativos Windows é executado pela primeira vez, a interface do usuário lista todos os aplicativos do Windows instalados no computador. Nas execuções subsequentes, a interface do usuário exibirá os aplicativos do Windows que você validou. Se o aplicativo que você deseja testar não estiver listado, clique em **Meu aplicativo não está na lista** para obter uma lista completa de todos os aplicativos instalados no sistema.

3.  Depois de inserir ou selecionar o aplicativo que você deseja testar, clique em **Avançar**.

4.  Na próxima tela, você verá o fluxo de trabalho de teste que se alinha ao tipo de aplicativo que você está testando. Se um teste estiver esmaecido na lista, ele não é aplicável ao seu ambiente. Por exemplo, se você estiver testando um Windows 10 app no Windows 7, somente os testes estáticos serão aplicadas ao fluxo de trabalho. Observe que a Microsoft Store pode aplicar todos os testes desse fluxo de trabalho. Selecione os testes que você deseja executar e clique em **Avançar**.

    O Kit de Certificação de Aplicativos Windows começa a validar o aplicativo.

5.  No prompt após o teste, digite o caminho para a pasta em que deseja salvar o relatório do teste.

    O Kit de Certificação de Aplicativos Windows cria um HTML junto com um relatório XML e salva-o nessa pasta.

6.  Abra o arquivo de relatório e examine os resultados do teste.

**Observação**  Se você usa o Visual Studio, execute o Kit de Certificação de Aplicativos Windows ao criar o pacote do aplicativo. Consulte [Empacotando aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/Mt627715) para saber como.

 

## <a name="validate-your-windows-app-using-the-windows-app-certification-kit-from-a-command-line"></a>Validar seu aplicativo do Windows usando o Kit de Certificação de Aplicativos Windows a partir de uma linha de comando

**Importante**  O Kit de Certificação de Aplicativos Windows deve executar no contexto de uma sessão de usuário ativa.

1.  Na janela de comando, navegue até o diretório que contém o Kit de Certificação de Aplicativos Windows.

    **Observação**   O caminho padrão é C:\\Program Files\\Windows Kits\\10\\App Certification Kit\\.

2.  Insira os seguintes comandos nesta ordem para testar um aplicativo que já está instalado no computador de teste:

    `appcert.exe reset`

    `appcert.exe test -packagefullname [package full name] -reportoutputpath [report file name]`

    Ou use os comandos a seguir se o aplicativo não estiver instalado. O Kit de Certificação de Aplicativos Windows abrirá o pacote e aplicará o fluxo de trabalho de teste apropriado:

    `appcert.exe reset`

    `appcert.exe test -appxpackagepath [package path] -reportoutputpath [report file name]`

3.  Quando o teste for concluído, abra o arquivo de relatório denominado `[report file name]` e examine os resultados.

**Observação**  O Kit de Certificação de Aplicativos Windows pode ser executado em um serviço, mas o serviço deve iniciar o processo do kit dentro de uma sessão de usuário ativa e não pode ser executado em Session0.

**Observação**   Para saber mais sobre a linha de comando do Kit de Certificação de Aplicativos Windows, insira o comando `appcert.exe /?`

## <a name="testing-with-a-low-power-computer"></a>Testando com um computador com baixo consumo de energia

O limites do teste de desempenho do Kit de Certificação de Aplicativos Windows são baseados no desempenho de um computador com baixo consumo de energia.

As características do computador em que o teste é realizado podem afetar os resultados do teste. Para determinar se o desempenho do seu aplicativo atende às [Políticas da Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Dn764944), recomendamos que você teste seu aplicativo em um computador de baixo consumo de energia, como um computador Intel Atom baseado no processador com uma resolução de tela de 1366 x 768 (ou superior) e uma rotação disco rígido unidade (em vez de um disco rígido de estado sólido).

As características de desempenho podem mudar ao longo do tempo para acompanhar a evolução dos computadores com baixo consumo de energia. Consulte as mais recentes [Políticas da Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Dn764944) e testar seu aplicativo com a versão mais atual do Kit de certificação de aplicativo do Windows para certificar-se de que seu aplicativo esteja em conformidade com os requisitos de desempenho mais recentes.

## <a name="related-topics"></a>Tópicos relacionados

* [Testes do Kit de Certificação de Aplicativos Windows](windows-app-certification-kit-tests.md)
* [Políticas da Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 

 




