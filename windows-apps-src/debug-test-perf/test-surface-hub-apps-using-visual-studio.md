---
author: PatrickFarley
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Testar aplicativos Surface Hub usando o Visual Studio
description: O simulador do Visual Studio fornece um ambiente para projetar, desenvolver, depurar e testar aplicativos UWP, incluindo aplicativos criados para o Surface Hub.
ms.author: pafarley
ms.date: 10/26/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 63214ce47bffc5a0b13f421e5185d06cd810ea34
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7434473"
---
# <a name="test-surface-hub-apps-using-visual-studio"></a>Testar aplicativos Surface Hub usando o Visual Studio
O simulador do Visual Studio fornece um ambiente onde você pode projetar, desenvolver, depurar e testar os aplicativos UWP (Plataforma Universal do Windows), incluindo os aplicativos que você criou para o Microsoft Surface Hub. O simulador não usa a mesma interface de usuário que o Surface Hub, mas é útil para testar seu aplicativo a aparência e comportamento com tamanho de tela do Surface Hub e resolução.

Para obter mais informações sobre a ferramenta de simulador em geral, consulte [executar aplicativos UWP no simulador](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator).

## <a name="add-surface-hub-resolutions-to-the-simulator"></a>Adicionar resoluções do Surface Hub ao simulador
Para adicionar resoluções do Surface Hub ao simulador:

1. Crie uma configuração para o 55" Surface Hub salvando o seguinte código XML em um arquivo chamado *HardwareConfigurations Surfacehub55*.  

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

2. Crie uma configuração para o 84" Surface Hub salvando o seguinte código XML em um arquivo chamado *HardwareConfigurations-Surfacehub84*.

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

3. Copie os dois arquivos XML em *C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;número da versão&gt;\HardwareConfigurations*.

   > [!NOTE]
   > São necessários privilégios administrativos para salvar arquivos nessa pasta.

4. Execute o aplicativo no simulador do Visual Studio. Clique no botão **Alterar Resolução** na paleta e selecione uma configuração do Surface Hub da lista.

    ![Resoluções de simulador do Visual Studio](images/vs-simulator-resolutions.png)

   > [!TIP]
   > [Ativar o modo de Tablet](http://windows.microsoft.com/windows-10/getstarted-like-a-tablet) para melhor simular a experiência de um Surface Hub.

## <a name="deploy-apps-to-a-surface-hub-device-from-visual-studio"></a>Implantar aplicativos em um dispositivo Surface Hub do Visual Studio
Implantar manualmente um aplicativo em um Surface Hub é um processo simples.

### <a name="enable-developer-mode"></a>Habilitar o modo de desenvolvedor
Por padrão, o Surface Hub só instala aplicativos da Microsoft Store. Para instalar aplicativos assinados por outras fontes, você deve habilitar o modo de desenvolvedor.

> [!NOTE]
> Após a habilitação do modo de desenvolvedor, você precisará redefinir o Surface Hub se você deseja desativá-lo novamente. Redefinir o dispositivo remove todas as configurações e arquivos de usuário local e reinstala o Windows.

1. Usando o menu **Iniciar** do Surface Hub, abra o aplicativo Configurações.

   > [!NOTE]
   > São necessários privilégios administrativos para acessar o aplicativo configurações no Surface Hub.

2. Navegue até **atualização e segurança \ > para desenvolvedores**.

3. Escolha **Modo de desenvolvedor** e aceite o prompt de aviso.

### <a name="deploy-your-app-from-visual-studio"></a>Implantar seu aplicativo no Visual Studio
Para obter mais informações sobre o processo de implantação em geral, consulte [Implantando e Depurando aplicativos UWP](https://msdn.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > [!NOTE]
   > Esse recurso requer o Visual Studio 2015 atualização 1 ou posterior, no entanto, recomendamos que você use a versão mais recente mais atualizada do Visual Studio. Uma instância do Visual Studio atualizada será gibe você todo o desenvolvimento mais recente e atualizações de segurança.

1. Navegue até a lista suspensa de destino de depuração ao lado do botão **Iniciar Depuração** e selecione **Máquina Remota**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Lista suspensa de destinos de depuração do Visual Studio](images/vs-debug-target.png)

2. Insira o endereço IP do Surface Hub. Garanta que o modo de autenticação **Universal** seja selecionado.

   > [!TIP] 
   > Depois que você tiver habilitado o modo de desenvolvedor, você pode encontrar o endereço IP do Surface Hub na tela de boas-vindas.

3. Selecione **Iniciar depuração (F5)** para implantar e depurar seu aplicativo no Surface Hub ou pressione Ctrl + F5 para apenas implantar seu aplicativo.

   > [!TIP]
   > Se o Surface Hub estiver exibindo a tela de boas-vindas, ignore-o escolhendo qualquer botão.
