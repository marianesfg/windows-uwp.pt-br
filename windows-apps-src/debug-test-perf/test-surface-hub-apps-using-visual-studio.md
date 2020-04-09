---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Testar aplicativos Surface Hub usando o Visual Studio
description: O simulador do Visual Studio fornece um ambiente para projetar, desenvolver, depurar e testar aplicativos UWP, incluindo aplicativos criados para o Surface Hub.
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 37c7f9edbaee008b6e16ef2ca202ff5cbcf39ca2
ms.sourcegitcommit: 6f32604876ed480e8238c86101366a8d106c7d4e
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/21/2019
ms.locfileid: "67317501"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Testar aplicativos Surface Hub usando o Visual Studio
O simulador do Visual Studio fornece um ambiente onde você pode projetar, desenvolver, depurar e testar os aplicativos de Plataforma Universal do Windows (UWP), incluindo os aplicativos que você criou para o Microsoft Surface Hub. O simulador não usa a mesma interface do usuário que o Surface Hub, mas é útil para testar a aparência e o comportamento de seu aplicativo com o tamanho de tela e a resolução do Surface Hub.

Para obter mais informações gerais sobre a ferramenta de simulação, confira [Executar aplicativos UWP no simulador](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Adicionar resoluções do Surface Hub ao simulador
Para adicionar resoluções do Surface Hub ao simulador:

1. Crie uma configuração para o Surface Hub 55" salvando o código XML a seguir em um arquivo chamado *HardwareConfigurations SurfaceHub55.xml*.  

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub55</Name>
            <DisplayName>Surface Hub 55"</DisplayName>
            <Resolution>
                <Height>1080</Height>
                <Width>1920</Width>
            </Resolution>
            <DeviceSize>55</DeviceSize>
            <DeviceScaleFactor>100</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

2. Crie uma configuração para o Surface Hub 84" salvando o XML a seguir em um arquivo chamado *HardwareConfigurations-SurfaceHub84.xml*.

    ```xml
    <?xml version="1.0" encoding="UTF-8"?>
    <ArrayOfHardwareConfiguration xmlns:xsd="http://www.w3.org/2001/XMLSchema"
                                  xmlns:xsi="http://www.w3.org/2001/XMLSchema-instance">
        <HardwareConfiguration>
            <Name>SurfaceHub84</Name>
            <DisplayName>Surface Hub 84"</DisplayName>
            <Resolution>
                <Height>2160</Height>
                <Width>3840</Width>
            </Resolution>
            <DeviceSize>84</DeviceSize>
            <DeviceScaleFactor>150</DeviceScaleFactor>
        </HardwareConfiguration>
    </ArrayOfHardwareConfiguration>
    ```

3. Copie os dois arquivos XML em *C:\Arquivos de Programa (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;número de versão&gt;\HardwareConfigurations*.

   > [!NOTE]
   > São necessários privilégios administrativos para salvar arquivos nessa pasta.

4. Execute o aplicativo no simulador do Visual Studio. Clique no botão **Alterar Resolução** na paleta e selecione uma configuração do Surface Hub da lista.

    ![Resoluções de simulador do Visual Studio](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Ative o modo tablet](https://support.microsoft.com/help/17210/windows-10-use-your-pc-like-a-tablet) para simular melhor a experiência em um Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Implantar aplicativos em um dispositivo Surface Hub do Visual Studio
Implantar manualmente um aplicativo em um Surface Hub é simples.

### <a name="enable-developer-mode"></a>Habilitar o modo de desenvolvedor
Por padrão, o Surface Hub instala apenas aplicativos da Microsoft Store. Para instalar aplicativos assinados por outras fontes, você deve habilitar o modo de desenvolvedor.

> [!NOTE]
> Após habilitar o modo de desenvolvedor, você precisará redefinir o Surface Hub se quiser desativá-lo novamente. Redefinir o dispositivo remove todas as configurações e arquivos de usuário local e reinstala o Windows.

1. Usando o menu **Iniciar** do Surface Hub, abra o aplicativo Configurações.

   > [!NOTE]
   > São necessários privilégios administrativos para acessar o aplicativo Configurações no Surface Hub.

2. Navegue até **Atualização e segurança\> Para desenvolvedores**.

3. Escolha **Modo de desenvolvedor** e aceite o prompt de aviso.

### <a name="deploy-your-app-from-visual-studio"></a>Implantar seu aplicativo no Visual Studio
Para obter mais informações sobre o processo de implantação em geral, confira [Implantar e depurar aplicativos UWP](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Esse recurso requer Visual Studio 2015 Atualização 1 ou posterior, no entanto, recomendamos que você use a versão mais recente e mais atualizada do Visual Studio. Uma instância atualizada do Visual Studio fornecerá todas as atualizações mais recentes de desenvolvimento e segurança.

1. Navegue até a lista suspensa de destino de depuração ao lado do botão **Iniciar Depuração** e selecione **Máquina Remota**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Lista suspensa de destinos de depuração do Visual Studio](images/vs-debug-target.png)

2. Insira o endereço IP do Surface Hub. Garanta que o modo de autenticação **Universal** seja selecionado.

   > [!TIP] 
   > Depois de habilitar o modo de desenvolvedor, você pode encontrar o endereço IP do Surface Hub na tela de boas-vindas.

3. Selecione **Iniciar Depuração (F5)** para implantar e depurar seu aplicativo no Surface Hub ou pressione CTRL+F5 para implantar apenas seu aplicativo.

   > [!TIP]
   > Se o Surface Hub estiver exibindo a tela de boas-vindas, descarte-a escolhendo qualquer botão.
