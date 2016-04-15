---
ms.assetid: A5320094-DF53-42FC-A6BA-A958F8E9210B
title: Testar aplicativos Surface Hub usando o Visual Studio
description: O simulador do Visual Studio fornece um ambiente para projetar, desenvolver, depurar e testar aplicativos UWP, incluindo aplicativos criados para o Surface Hub. 
---

# Testar aplicativos Surface Hub usando o Visual Studio
O simulador do Visual Studio fornece um ambiente onde você pode projetar, desenvolver, depurar e testar os aplicativos de Plataforma Universal do Windows (UWP), incluindo os aplicativos que você criou para o Microsoft Surface Hub. O simulador não usa a mesma interface de usuário que o Surface Hub, mas é útil para testar a aparência e o comportamento de seu aplicativo no tamanho de tela e resolução do Surface Hub.

Para obter mais informações, consulte [Executar aplicativos da Windows Store no simulador](https://msdn.microsoft.com/en-us/library/hh441475.aspx).

## Adicionar resoluções do Surface Hub ao simulador
Para adicionar resoluções do Surface Hub ao simulador:

1. Crie uma configuração para o 55" Surface Hub salvando o XML a seguir em um arquivo chamado **HardwareConfigurations SurfaceHub55.xml**.  

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

2. Crie uma configuração para o 84" Surface Hub salvando o XML a seguir em um arquivo chamado **HardwareConfigurations-SurfaceHub84.xml**.

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

3. Copie os dois arquivos XML em **C:\Program Files (x86)\Common Files\Microsoft Shared\Windows Simulator\\&lt;version number&gt;\HardwareConfigurations**.

   > **Observação**&nbsp;&nbsp;São necessários privilégios administrativos para salvar arquivos nessa pasta.

4. Execute o aplicativo no simulador do Visual Studio. Clique no botão **Alterar Resolução** na paleta e selecione uma configuração do Surface Hub da lista.

    ![Resoluções de simulador do Visual Studio](images/vs-simulator-resolutions.png)
    
   > **Dica**&nbsp;&nbsp; [Ative o modo Tablet](http://windows.microsoft.com/en-us/windows-10/getstarted-like-a-tablet) para melhor simular a experiência em um Surface Hub.
   
## Implantar aplicativos em um Surface Hub do Visual Studio 
Implantar manualmente um aplicativo é um processo simples.

### Habilitar o modo de desenvolvedor
Por padrão, o Surface Hub só instala aplicativos da Windows Store. Para instalar aplicativos assinados por outras fontes, você deve habilitar o modo de desenvolvedor. 

> **Observação**&nbsp;&nbsp;Após a habilitação do modo de desenvolvedor, você precisará redefinir o Surface Hub para desativá-lo novamente. Redefinir o dispositivo remove todas as configurações e arquivos de usuário local e reinstala o Windows.

1. Usando o menu **Iniciar** do Surface Hub, abra o aplicativo Configurações.

   >  **Observação**&nbsp;&nbsp; São necessários privilégios administrativos para acessar o aplicativo Configurações.
   
2. Navegue até **Atualização e segurança > Para desenvolvedores**.

3. Escolha **Modo de desenvolvedor** e aceite o prompt de aviso.

### Implantar seu aplicativo no Visual Studio
Para obter mais informações, consulte [Implantando e depurando aplicativos da Plataforma Universal do Windows (UWP)](https://msdn.microsoft.com/en-us/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps).

   > **Observação**&nbsp;&nbsp;Esse recurso requer pelo menos o **Visual Studio 2015 Update 1**.

1. Navegue até a lista suspensa de destino de depuração ao lado do botão **Iniciar Depuração** e selecione **Máquina Remota**.

    <!--lcap: in your screenshot, you have local machine selected-->

   ![Lista suspensa de destinos de depuração do Visual Studio](images/vs-debug-target.png)
   
2. Insira o endereço IP do Surface Hub. Garanta que o modo de autenticação **Universal** seja selecionado.

   > **Dica**&nbsp;&nbsp; Depois que tiver habilitado o modo de desenvolvedor, você pode encontrar o endereço IP do Surface Hub na tela de boas-vindas.
   
3. Escolha **Iniciar Depuração (F5)** para implantar e depurar seu aplicativo no Surface Hub ou pressione Ctrl + F5 para apenas implantar seu aplicativo.

   > **Dica**&nbsp;&nbsp; se o Surface Hub estiver na tela de boas-vindas, ignore-o escolhendo qualquer botão.



<!--HONumber=Mar16_HO5-->


