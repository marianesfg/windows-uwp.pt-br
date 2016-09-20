---
title: Desbloqueio do Windows com dispositivos complementares (IoT)
description: "Um dispositivo complementar é um dispositivo que pode atuar em conjunto com sua área de trabalho do Windows 10 para melhorar a experiência de autenticação do usuário. Usando a Estrutura de Dispositivo Complementar, um dispositivo complementar pode fornecer uma experiência avançada para o Microsoft Passport mesmo quando o Windows Hello não está disponível (por exemplo, se a área de trabalho do Windows 10 não tiver uma câmera para autenticação de face ou dispositivo de leitor de impressão digital, por exemplo)."
author: awkoren
ms.sourcegitcommit: a6265ca66a1a9d729465845da1014d1aff0e7d4d
ms.openlocfilehash: 18102d6277ff1c66ebd147b5c1fd2f2d6c91edd1

---
# Desbloqueio do Windows com dispositivos complementares (IoT)

Um dispositivo complementar é um dispositivo que pode atuar em conjunto com sua área de trabalho do Windows 10 para melhorar a experiência de autenticação do usuário. Usando a Estrutura de Dispositivo Complementar, um dispositivo complementar pode fornecer uma experiência avançada para o Microsoft Passport mesmo quando o Windows Hello não está disponível (por exemplo, se a área de trabalho do Windows 10 não tiver uma câmera para autenticação de face ou dispositivo de leitor de impressão digital, por exemplo).

> 
            **Observação** A Estrutura de Dispositivo Complementar é um recurso especializado não disponível para todos os desenvolvedores de aplicativos. Para usar essa estrutura, seu aplicativo deve ser especificamente provisionado pela Microsoft e listar a funcionalidade restrita *secondaryAuthenticationFactor* em seu manifesto. Para obter aprovação, entre em contato com [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com).

## Introdução

> Para uma visão geral em vídeo, consulte a sessão [Desbloqueio do Windows com dispositivos complementares IoT](https://channel9.msdn.com/Events/Build/2016/P491) da compilação 2016 no Channel 9.

### Casos de uso

Há várias maneiras de usar a Estrutura de Dispositivo Complementar para criar uma excelente experiência de desbloqueio do Windows com um dispositivo complementar. Por exemplo, os usuários podem:

- Anexar seus dispositivos complementares ao computador via USB, tocar no botão do dispositivo complementar e desbloquear automaticamente seus computadores.
- Carregar um telefone no bolso que já esteja emparelhado com o computador via Bluetooth. Ao pressionarem a barra de espaço em seus computadores, seus telefones recebem uma notificação. Basta aprová-la para desbloquear o computador.
- Tocar seus dispositivos complementares em um leitor de NFC para desbloquear rapidamente seus computadores.
- Usar um acessório de fitness que já tenha autenticado o usuário. Ao se aproximarem do computador e realizarem um gesto especial (como bater palmas), o computador será desbloqueado.

### Dispositivos complementares habilitados para biometria

Se o dispositivo complementar for compatível com biometria, em alguns casos a [Windows Biometric Framework](https://msdn.microsoft.com/library/windows/hardware/mt608302(v=vs.85).aspx) pode ser uma solução melhor do que a Estrutura de Dispositivo Complementar. Entre em contato com [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com) e ajudaremos você a escolher a abordagem correta.

### Componentes da solução

O diagrama abaixo mostra os componentes da solução e quem é responsável por criá-los.

![visão geral da estrutura](images/companion-device-1.png)

A Estrutura de Dispositivo Complementar é implementada como um serviço em execução no Windows (chamado de Serviço de Autenticação Complementar neste artigo). Esse serviço é responsável por gerar um token de desbloqueio que precisa ser protegido por uma chave HMAC armazenada no dispositivo complementar. Isso garante que o acesso ao token de desbloqueio exija a presença do dispositivo complementar. Por cada tupla (computador, usuário do Windows), haverá um único token de desbloqueio.

A integração com a Estrutura de Dispositivo Complementar requer:

- Um aplicativo de dispositivo complementar [UPW (Plataforma Universal do Windows)](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) para o dispositivo complementar, baixado na Windows Store. 
- A capacidade de criar duas chaves HMAC de 256 bits no dispositivo complementar e de gerar a HMAC com ele (usando SHA-256).
- Configurações de segurança na área de trabalho do Windows 10 corretamente definidas. O Serviço de Autenticação Complementar exigirá que esse PIN seja configurado antes que qualquer dispositivo complementar possa ser conectado a ele. Os usuários devem configurar um PIN via Configurações > Contas > Opções de entrada.

Além dos requisitos acima, o aplicativo de dispositivo complementar é responsável por:

- Experiência do usuário e identidade visual do registro inicial e, mais tarde, o cancelamento do registro do dispositivo complementar.
- Execução em segundo plano, descoberta do dispositivo complementar, comunicação com esse dispositivo e também com o Serviço de Autenticação Complementar.
- Tratamento de erros

Normalmente, dispositivos complementares são fornecidos com um aplicativo para configuração inicial, como configurar um acessório de fitness pela primeira vez. A funcionalidade descrita neste documento pode fazer parte desse aplicativo, e um aplicativo separado não será necessário.  

### Sinais do usuário

Cada dispositivo complementar deve ser combinado com um aplicativo que dê suporte a três sinais do usuário. Esses sinais podem ser no formato de uma ação ou de um gesto.

- 
            **Sinal de intenção**: permite que o usuário mostre sua intenção de desbloquear, por exemplo, pressionando um botão no dispositivo complementar. O sinal de intenção deve ser coletado no lado do **dispositivo complementar**.
- 
            **Sinal de presença do usuário**: prova a presença do usuário. O dispositivo complementar pode, por exemplo, exigir um PIN antes de poder ser usado para desbloquear o computador (isso não deve ser confundido com o PIN do computador) ou pode exigir o pressionamento de um botão.
- 
            **Sinal de diferenciação**: remove a ambiguidade de qual área de trabalho do Windows 10 o usuário deseja desbloquear quando várias opções estão disponíveis para o dispositivo complementar.

Qualquer número desses sinais de usuário pode ser combinado em um único sinal. Sinais de presença e intenção do usuário devem ser necessários em cada uso.

### Registro e comunicação futura entre um computador e dispositivos complementares

Antes que um dispositivo complementar possa ser conectado à Estrutura de Dispositivo Complementar, ele precisa ser registrado nessa estrutura. A experiência de registro fica completamente a cargo do aplicativo de dispositivo complementar.

O relacionamento registrado entre o dispositivo complementar e o dispositivo de área de trabalho do Windows 10 pode ser de um-para-muitos (ou seja, um dispositivo complementar pode ser usado para vários dispositivos de área de trabalho do Windows 10). No entanto, cada dispositivo complementar só pode ser usado para um usuário em cada dispositivo de área de trabalho do Windows 10.   

Para que um dispositivo complementar possa se comunicar com um computador, é necessário chegar a um acordo acerca do transporte a ser usado. Essa escolha fica a cargo do aplicativo de dispositivo complementar. A Estrutura de Dispositivo Complementar não impõe nenhuma limitação sobre o tipo de transporte (USB, NFC, WiFi, BT, BLE etc) ou protocolo em uso entre o dispositivo complementar e o aplicativo de dispositivo complementar no lado do dispositivo de área de trabalho do Windows 10. No entanto, ela sugere certas considerações de segurança para a camada de transporte, conforme descrito na seção "Requisitos de segurança" deste documento. É responsabilidade do provedor do dispositivo fornecer esses requisitos. A estrutura não os fornece para você.


## Modelo de interação do usuário

### Descoberta, instalação e registro inicial do aplicativo de dispositivo complementar

Um fluxo de trabalho do usuário típico é o seguinte:

- O usuário configura o PIN em cada um dos dispositivos de área de trabalho do Windows 10 de destino que ele deseja desbloquear com esse dispositivo complementar.
- O usuário executa o aplicativo de dispositivo complementar em seus dispositivos de área de trabalho do Windows 10 para registrar seu dispositivo complementar na área de trabalho do Windows 10.

Observações:

- Recomendamos que a descoberta, o download e a ativação do aplicativo de dispositivo complementar seja simplificada e, se possível, automatizada (por exemplo, o aplicativo pode ser baixado ao tocar o dispositivo complementar em um leitor de NFC no lado do dispositivo de área de trabalho do Windows 10). No entanto, isso é responsabilidade do dispositivo complementar e do aplicativo de dispositivo complementar.
- Em um ambiente corporativo, o aplicativo de dispositivo complementar pode ser implantado via MDM.
- O aplicativo de dispositivo complementar é responsável por mostrar ao usuário mensagens de erro que ocorrem como parte do registro.

### Protocolo de registro e cancelamento de registro

O diagrama a seguir ilustra como o dispositivo complementar interage com o Serviço de Autenticação Complementar durante o registro.  

![fluxo de registro](images/companion-device-2.png)

Há duas chaves usadas em nosso protocolo:

- Chave de dispositivo (**devicekey**): usada para proteger tokens de desbloqueio necessários para o computador desbloquear o Windows.
- A chave de autenticação (**authkey**): usada para autenticar mutuamente o dispositivo complementar e o Serviço de Autenticação Complementar.

A chave de dispositivo e as chaves de autenticação são trocadas na ocasião do registro entre o aplicativo de dispositivo complementar e o dispositivo complementar. Como resultado, o aplicativo de dispositivo complementar e o dispositivo complementar devem usar um transporte seguro para proteger chaves.

Além disso, observe que, embora o diagrama acima mostre duas chaves HMAC geradas no dispositivo complementar, também é possível que o aplicativo as gere e as envie ao dispositivo complementar para armazenamento.

### Iniciando fluxos de autenticação

Há duas maneiras de o usuário iniciar o fluxo de entrada na área de trabalho do Windows 10 usando a Estrutura de Dispositivo Complementar (isto é, fornecer um sinal de intenção):

- Abra a tampa do notebook ou pressione a tecla de espaço ou passe o dedo para cima no computador.
- Execute uma ação ou um gesto no lado do dispositivo complementar.

Cabe ao dispositivo complementar selecionar qual deles é o ponto de partida. A Estrutura de Dispositivo Complementar informará o aplicativo de dispositivo complementar quando a primeira opção ocorrer. Para a segunda opção, o aplicativo de dispositivo complementar deve consultar o dispositivo complementar para ver se esse evento foi capturado. Isso garante que o dispositivo complementar colete o sinal de intenção antes que o desbloqueio seja bem-sucedido.

### Provedor de credenciais de dispositivo complementar

Há um novo provedor de credenciais no Windows 10 que lida com todos os dispositivos complementares.

O provedor de credenciais de dispositivo complementar é responsável por iniciar a tarefa em segundo plano do dispositivo complementar por meio do acionamento de um gatilho. O gatilho é definido pela primeira vez quando o computador desperta e uma tela de bloqueio é exibida. A segunda vez é quando o computador está entrando na interface de usuário de logon e o Provedor de Credenciais de Dispositivo Complementar é o bloco selecionado.

A biblioteca auxiliar do aplicativo de dispositivo complementar escutará a mudança de status da tela de bloqueio e enviará o evento correspondente à tarefa de segundo plano do dispositivo complementar.

Se houver várias tarefas de segundo plano do dispositivo complementar, a primeira delas que terminar o processo de autenticação desbloqueará o computador. O serviço de autenticação de dispositivo complementar ignorará qualquer chamada de autenticação restante.

A experiência no lado do dispositivo complementar é mantida e gerenciada pelo aplicativo de dispositivo complementar. A Estrutura de Dispositivo Complementar não tem controle sobre essa parte da experiência do usuário. Mais especificamente, o provedor de autenticação complementar informa o aplicativo de dispositivo complementar (por meio de seu aplicativo em segundo plano) sobre mudanças de estado na interface de usuário de logon (por exemplo, a tela de bloqueio acabou de ser ativada ou o usuário acabou de dispensá-la pressionando a barra de espaço), e é responsabilidade do aplicativo de dispositivo complementar criar uma experiência com base nisso (por exemplo, quando o usuário pressionar a barra de espaço e dispensar a tela de desbloqueio, começar a procurar o dispositivo via USB).

A Estrutura de Dispositivo Complementar fornecerá um estoque de texto e mensagens de erro (traduzidos) para escolha pelo aplicativo de dispositivo complementar. Esses elementos serão exibidos na parte superior da tela de bloqueio (ou na interface de usuário de logon). Para saber mais, consulte a seção Lidando com mensagens e erros.

### Protocolo de autenticação

Depois que a tarefa em segundo plano associada a um aplicativo de dispositivo complementar é iniciada por gatilho, ela é responsável por solicitar que o dispositivo complementar ajude a calcular dois valores HMAC:
- O HMAC da chave de dispositivo com um nonce.
- O HMAC da chave de autenticação com o primeiro valor HMAC concatenado com um nonce gerado pelo Serviço de Autenticação Complementar.

O segundo valor é usado pelo serviço para autenticar o dispositivo e também para evitar ataques de reprodução no canal de transporte.

![fluxo de registro](images/companion-device-3.png)

## Gerenciamento do ciclo de vida

### Registre uma vez e use em qualquer lugar

Sem um servidor back-end, os usuários devem registrar seus dispositivos complementares em cada dispositivo de área de trabalho do Windows 10 separadamente.

Um fornecedor de dispositivo complementar ou OEM pode implementar um serviço Web para circular o estado de registro entre áreas de trabalho do Windows 10 ou dispositivos móveis do usuário. Para obter mais detalhes, consulte a seção Serviço de roaming, revogação e filtro.

### Gerenciamento de PIN

Antes que um dispositivo complementar possa ser usado, um PIN precisa ser configurado no dispositivo de área de trabalho do Windows 10. Isso garante que o usuário tenha um backup no caso de seu dispositivo complementar não funcionar. O PIN é algo que o Windows gerencia e que os aplicativos nunca veem. Para alterá-lo, o usuário navega até Configurações > Contas > Opções de entrada.

### Gerenciamento e políticas

Os usuários podem remover um dispositivo complementar de áreas de trabalho do Windows 10 executando o aplicativo de dispositivo complementar nesse dispositivo de área de trabalho.

As empresas têm duas opções para controlar a Estrutura de Dispositivo Complementar:

- Ativar ou desativar o recurso
- Definir a lista branca de dispositivos complementares permitidos usando o cofre de aplicativo do Windows

A Estrutura de Dispositivo Complementar não dá suporte a uma maneira centralizada de manter o inventário de dispositivos complementares disponíveis ou a um método para filtrar ainda mais quais instâncias de um tipo de dispositivo complementar são permitidas (por exemplo, apenas dispositivos complementares com números de série entre X e Y são permitidos). No entanto, os desenvolvedores de aplicativos podem criar um serviço para fornecer essa funcionalidade. Para obter mais detalhes, consulte a seção Serviço de roaming, revogação e filtro.

### Revogação

A Estrutura de Dispositivo Complementar não dá suporte à remoção de um dispositivo complementar de um dispositivo de área de trabalho do Windows 10 específico remotamente. Em vez disso, os usuários podem remover o dispositivo complementar por meio do aplicativo de dispositivo complementar em execução nessa área de trabalho do Windows 10.

No entanto, fornecedores de dispositivos complementares podem criar um serviço para fornecer a funcionalidade de revogação remota. Para obter mais detalhes, consulte a seção Serviço de roaming, revogação e filtro.

### Serviços de roaming e filtro

Fornecedores de dispositivos complementares podem implementar um serviço Web que pode ser usado para os seguintes cenários:

- Um serviço de filtro para uma empresa: uma empresa pode limitar o conjunto de dispositivos complementares que podem funcionar em seu ambiente para selecionar apenas alguns de um fornecedor específico. Por exemplo, a empresa Contoso pode encomendar do Fornecedor X 10.000 dispositivos complementares do Modelo Y e garantir que apenas esses dispositivos funcionarão no domínio da Contoso (e não em qualquer outro modelo de dispositivo do Fornecedor X).
- Inventário: uma empresa pode determinar a lista de dispositivos complementares existente usados em um ambiente corporativo.
- Revogação em tempo real: se um funcionário comunicar que seu dispositivo complementar foi perdido ou roubado, o serviço Web poderá ser usado para revogar esse dispositivo.
- Roaming: basta que um usuário registre seu dispositivo complementar, e ele funcionará em todas as suas áreas de trabalho do Windows 10 e dispositivos móveis.

A implementação desses recursos requer que aplicativo de dispositivo complementar faça uma verificação com o serviço Web na ocasião do registro e do uso. O aplicativo de dispositivo complementar pode ser otimizado para cenários de logon em cache, por exemplo, exigindo a verificação com o serviço Web apenas uma vez por dia (às custas de prolongar o tempo de revogação a até um dia).  

## Modelo de API da Estrutura de Dispositivo Complementar

### Visão geral

Um aplicativo complementar deve conter dois componentes: um aplicativo de primeiro plano com uma interface de usuário responsável para registrar e cancelar o registro do dispositivo e uma tarefa em segundo plano que manipula a autenticação.

O fluxo geral da API é o seguinte:

1. Registrar o dispositivo complementar
    * Verifique se o dispositivo está próximo e veja sua capacidade (se necessário)
    * Gere duas chaves HMAC (no lado do dispositivo complementar ou no lado do aplicativo)
    * Chame RequestStartRegisteringDeviceAsync
    * Chame FinishRegisteringDeviceAsync
    * Certifique-se de que aplicativo de dispositivo complementar armazene chaves HMAC (se permitido) e descarte suas cópias
2. Registrar sua tarefa em segundo plano
3. Aguarde o evento certo na tarefa em segundo plano
    * WaitingForUserConfirmation: aguarde este evento se a ação ou o gesto do usuário no lado do dispositivo complementar for necessária para iniciar o fluxo de autenticação
    * CollectingCredential: aguarde esse evento se o dispositivo complementar depender da ação ou do gesto do usuário no lado do computador para iniciar o fluxo de autenticação (por exemplo, pressionando a barra de espaço)
    * Outro gatilho, como um cartão inteligente: certifique-se de consultar o estado atual de autenticação chamar as APIs certas.
4. Manter o usuário informado sobre mensagens de erro ou próximas etapas necessárias chamando ShowNotificationMessageAsync. Chame essa API somente depois que um sinal de intenção for coletado
5. Desbloquear
    * Certifique-se de que os sinais de intenção e presença do usuário tenham sido coletados
    * Chame StartAuthenticationAsync
    * Comunique-se com o dispositivo complementar para realizar operações de HMAC necessárias
    * Chame FinishAuthenticationAsync
6. Cancelar o registro de um dispositivo complementar quando o usuário solicitar (por exemplo, se ele tiver perdido esse dispositivo)
    * Enumere o dispositivo associado para o usuário conectado via FindAllRegisteredDeviceInfoAsync
    * Cancele seu registro usando UnregisterDeviceAsync

### Registro e cancelamento do registro

O registro requer duas chamadas de API para o Serviço de Autenticação Complementar: RequestStartRegisteringDeviceAsync e FinishRegisteringDeviceAsync.

Antes que qualquer uma dessas chamadas seja feita, o aplicativo de dispositivo complementar deve garantir que o dispositivo complementar esteja disponível. Se o dispositivo complementar for responsável pela geração de chaves HMAC (chaves de autenticação e dispositivo), o aplicativo de dispositivo complementar também deverá solicitar que o dispositivo complementar as gere antes de fazer qualquer uma das duas chamadas acima. Se o aplicativo de dispositivo complementar for responsável pela geração de chaves HMAC, ele deverá fazer isso antes de fazer as duas chamadas acima.

Além disso, como parte da primeira chamada à API (RequestStartRegisteringDeviceAsync), o aplicativo de dispositivo complementar deve decidir sobre as funcionalidades do dispositivo e estar preparado para transmiti-las como parte da chamada à API. Por exemplo, se dispositivo complementar dá suporte ao armazenamento seguro para chaves HMAC. Se o mesmo aplicativo de dispositivo complementar for usado para gerenciar várias versões do mesmo dispositivo complementar e essas funcionalidades forem modificadas (e exigirem uma consulta de dispositivo para chegarem a uma decisão), convém que essa consulta ocorra antes que a primeira chamada de API seja feita.   

A primeira API (RequestStartRegisteringDeviceAsync) retornará um identificador usado pela segunda API (FinishRegisteringDeviceAsync). A primeira chamada para registro iniciará o prompt de PIN para garantir que o usuário esteja presente. Se nenhum PIN estiver configurado, essa chamada falhará. O aplicativo de dispositivo complementar também pode consultar se o PIN está configurado ou não por meio da chamada KeyCredentialManager.IsSupportedAsync. A chamada RequestStartRegisteringDeviceAsync também poderá falhar se a política tiver desabilitado o uso do dispositivo complementar.

O resultado da primeira chamada é retornado por meio da enumeração SecondaryAuthenticationFactorRegistrationStatus:

```C#
{
    Failed = 0,         // Something went wrong in the underlying components
    Started,            // First call succeeded
    CanceledByUser,     // User cancelled PIN prompt
    PinSetupRequired,   // PIN is not set up
    DisabledByPolicy,   // Companion device framework or this app is disabled
}
```

A segunda chamada (FinishRegisteringDeviceAsync) conclui o registro. Como parte do processo de registro, o aplicativo de dispositivo complementar pode armazenar dados de configuração de dispositivos complementares com o Serviço de Autenticação Complementar. Há um limite de tamanho de 4K para esses dados. Esses dados estarão disponíveis para o aplicativo de dispositivo complementar na ocasião da autenticação. Esses dados podem ser usados, por exemplo, para conexão com o dispositivo complementar, como um endereço MAC, ou se o dispositivo complementar não tiver um armazenamento e quiser usar o computador para armazenamento. Observe que todos os dados confidenciais armazenados como parte dos dados de configuração devem ser criptografados com uma chave conhecida somente pelo dispositivo complementar. Além disso, considerando que os dados de configuração são armazenados por um serviço Windows, eles estão disponíveis ao aplicativo de dispositivo complementar em vários perfis de usuário.

O aplicativo de dispositivo complementar pode chamar AbortRegisteringDeviceAsync para cancelar o registro e transmitir um código de erro. O Serviço de Autenticação Complementar registrará o erro nos dados de telemetria. Um bom exemplo para essa chamada seria quando algo deu errado com o dispositivo complementar e ele não pôde concluir o registro (por exemplo, não é possível armazenar chaves HMAC ou a conexão BT foi perdida).

O aplicativo de dispositivo complementar deve fornecer uma opção para o usuário cancelar o registra de seu dispositivo complementar na área de trabalho do Windows 10 (por exemplo, se ele tiver perdido seu dispositivo complementar ou comprado uma versão mais recente). Quando o usuário selecionar essa opção, o aplicativo de dispositivo complementar deverá chamar UnregisterDeviceAsync. Essa chamada pelo aplicativo de dispositivo complementar disparará o serviço de autenticação de dispositivo complementar de forma a excluir todos os dados (incluindo chaves HMAC) correspondentes à ID de dispositivo específica e ao AppId do aplicativo de chamada no lado do computador. Essa chamada de API não tenta excluir chaves HMAC no lado do aplicativo de dispositivo complementar ou no lado do dispositivo complementar. Isso é deixado para o aplicativo de dispositivo complementar implementar.

O aplicativo de dispositivo complementar é responsável por mostrar mensagens de erro que ocorrem nas fases de registro e de cancelamento de registro.

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using Windows.UI.Popups;

namespace SecondaryAuthFactorSample
{
    public class DeviceRegistration
    {

        public void async OnRegisterButtonClick()
        {
            //
            // Pseudo function, the deviceId should be retrieved by the application from the device
            //
            string deviceId = await ReadSerialNumberFromDevice();

            IBuffer deviceKey = CryptographicBuffer.GenerateRandom(256/8);
            IBuffer mutualAuthenticationKey = CryptographicBuffer.GenerateRandom(256/8);

            SecondaryAuthenticationFactorRegistration registrationResult =
                await SecondaryAuthenticationFactorRegistration.RequestStartRegisteringDeviceAsync(
                    deviceId,  // deviceId: max 40 wide characters. For example, serial number of the device
                    SecondaryAuthenticaitonFactorDeviceCapabilities.SupportSecureStorage |
                        SecondaryAuthenticaitonFactorDeviceCapabilities.SupportSha2 |
                        SecondaryAuthenticaitonFactorDeviceCapabilities.StoreKeys,
                    "My test device 1", // deviceFriendlyName: max 64 wide characters. For example: John's card
                    "SAMPLE-001", // deviceModelNumber: max 32 wide characters. The app should read the model number from device.
                    deviceKey,
                    mutualAuthenticationKey);

            switch(registerResult.Status)
            {
            case SecondaryAuthenticationFactorRegistrationStatus.Started:
                //
                // Pseudo function:
                // The app needs to retrieve the value from device and set into opaqueBlob
                //
                IBuffer deviceConfigData = ReadConfigurationDataFromDevice();

                if (deviceConfigData != null)
                {
                    await registrationResult.Registration.FinishRegisteringDeviceAsync(deviceConfigData); //config data limited to 4096 bytes
                    MessageDialog dialog = new MessageDialog("The device is registered correctly.");
                    await dialog.ShowAsync();
                }
                else
                {
                    await registrationResult.Registration.AbortRegisteringDeviceAsync("Failed to connect to the device");
                    MessageDialog dialog = new MessageDialog("Failed to connect to the device.");
                    await dialog.ShowAsync();
                }
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.CanceledByUser:
                MessageDialog dialog = new MessageDialog("You didn't enter your PIN.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.PinSetupRequired:
                MessageDialog dialog = new MessageDialog("Please setup PIN in settings.");
                await dialog.ShowAsync();
                break;

            case SecondaryAuthenticationFactorRegistrationStatus.DisabledByPolicy:
                MessageDialog dialog = new MessageDialog("Your enterprise prevents using this device to sign in.");
                await dialog.ShowAsync();
                break;
            }
        }

        public void async UpdateDeviceList()
        {
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticaitonFactorDeviceFindScope.User);

            if (deviceInfoList.Count > 0)
            {
                foreach (SecondaryAuthenticationFactorInfo deviceInfo in deviceInfoList)
                {
                    //
                    // Add deviceInfo.FriendlyName and deviceInfo.DeviceId into a combo box
                    //
                }
            }
        }

        public void async OnUnregisterButtonClick()
        {
            string deviceId;
            //
            // Read the deviceId from the selected item in the combo box
            //
            await SecondaryAuthenticationFactorRegistration.UnregisterDeviceAsync(deviceId);
        }
    }
}
```

### Autenticação

A autenticação exige duas chamadas de API para o Serviço de Autenticação Complementar: StartAuthenticationAsync e FinishAuthencationAsync.

A primeira API de inicialização retornará um identificador usado pela segunda API.  A primeira chamada retorna, entre outras coisas, um nonce que, quando concatenado com outros elementos, precisa ser codificado em HMAC com a chave de dispositivo armazenada no dispositivo complementar. A segunda chamada retorna os resultados do HMAC com a chave de dispositivo e pode terminar em uma autenticação bem-sucedida (isto é, o usuário verá sua área de trabalho).

A primeira API de inicialização (StartAuthenticationAsync) poderá falhar se a política tiver desabilitado esse dispositivo complementar após o registro inicial. Ela também poderá falhar se a chamada de API tiver sido feita fora dos estados WaitingForUserConfirmation ou CollectingCredential (informações adicionais sobre isso mais adiante nesta seção). Ela também poderá falhar se um aplicativo de dispositivo complementar com registro cancelado a chamar. A enumeração SecondaryAuthenticationFactorAuthenticationStatus resume os possíveis resultados:

```C#
{
    Failed = 0,                     // Something went wrong in the underlying components
    Started,
    UnknownDevice,                  // Companion device app is not registered with framework
    DisabledByPolicy,               // Policy disabled this device after registration
    InvalidAuthenticationStage,     // Companion device framework is not currently accepting
                                    // incoming authentication requests
}
```

A segunda chamada de API (FinishAuthencationAsync) poderá falhar se o nonce que foi fornecido na primeira chamada estiver expirado (20 segundos). A enumeração SecondaryAuthenticationFactorFinishAuthenticationStatus captura os possíveis resultados.

```C#
{
    Failed = 0,     // Something went wrong in the underlying components
    Completed,      // Success
    NonceExpired,   // Nonce is expired
}
```

O cronograma das duas chamadas de API (StartAuthenticationAsync e FinishAuthencationAsync) precisa estar alinhado a como o dispositivo complementar coleta sinais de intenção, presença do usuário e diferenciação (veja Sinais do usuário para obter mais detalhes). Por exemplo, a segunda chamada não deve ser enviada até que o sinal de intenção esteja disponível. Em outras palavras, o computador não deverá ser desbloqueado se um usuário não tiver expressado a intenção de fazer isso. Para explicar melhor o processo, suponha que proximidade do Bluetooth seja usada para o desbloqueio do computador. Nesse caso, um sinal de intenção explícito deve ser coletado para evitar que o computador seja desbloqueado se o usuário se aproximar dele no caminho para a cozinha. Além disso, o nonce retornado pela primeira chamada está associado ao tempo (20 segundos) e expirará depois de um determinado período. Como resultado, a primeira chamada só deverá ser feita quando o aplicativo de dispositivo complementar tiver uma boa indicação da presença do dispositivo complementar, por exemplo, o dispositivo complementar é inserido na porta USB ou toca o leitor de NFC. Com o Bluetooth, é necessário ter cautela para evitar afetar a atividade da bateria no lado do computador ou outras atividades Bluetooth que estejam ocorrendo nesse ponto ao verificar a presença do dispositivo complementar. Além disso, se o sinal de presença do usuário tiver que ser fornecido (por exemplo, digitando um PIN), convém que a primeira chamada de autenticação seja feita somente após a coleta desse sinal.

A Estrutura de Dispositivo Complementar ajuda o aplicativo de dispositivo complementar a tomar decisões bem-informadas no que diz respeito a quando fazer as duas chamadas acima, fornecendo uma imagem completa de onde usuário se encontra no fluxo de autenticação. A Estrutura de Dispositivo Complementar oferece essa funcionalidade notificações de mudança de estado de bloqueio para a tarefa em segundo plano do aplicativo.

![fluxo do dispositivo complementar](images/companion-device-4.png)

Estes são os detalhes de cada um desses estados:

| Estado                         | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| WaitingForUserConfirmation    | Esse evento de notificação de mudança de estado é disparado quando a tela de bloqueio é ativada (por exemplo, o usuário pressionou Windows + L). Convém não solicitar mensagens de erro relacionadas a dificuldades de encontrar um dispositivo nesse estado. Em geral, convém apenas mostrar apenas mensagens quando o sinal de intenção está disponível. O aplicativo de dispositivo complementar deverá fazer a primeira chamada de API para autenticação nesse estado se o dispositivo complementar coletar um sinal de intenção (por exemplo, tocar no leitor de NFC, pressionar um botão no dispositivo complementar ou realizar um gesto específico, como bater palmas) e se a tarefa em segundo plano do aplicativo de dispositivo complementar receber uma indicação do dispositivo complementar de que esse sinal de intenção foi detectado. Caso contrário, se o aplicativo de dispositivo complementar depender do computador para iniciar o fluxo de autenticação (fazendo com que o usuário passe o dedo para cima na tela de desbloqueio ou pressione a barra de espaço), ele precisará aguardar o próximo estado (CollectingCredential).    |
| CollectingCredential          | Esse evento de notificação de mudança de estado é disparado quando o usuário abre a tampa do notebook, pressiona qualquer tecla no teclado ou passe o dedo para cima na tela de desbloqueio. Se o dispositivo complementar depender das ações acima para iniciar a coleta do sinal de intenção, o aplicativo de dispositivo complementar deverá iniciar a sua coleta (por exemplo, por meio de um pop-up no dispositivo complementar perguntando se o usuário deseja desbloquear o computador). Isso seria um bom momento para fornecer casos de erro se o aplicativo de dispositivo complementar precisar que o usuário forneça um sinal de presença no dispositivo complementar (como digitar um PIN nesse dispositivo).                                                                                                                                                                                                                                                                                                                                               |
| Suspendingauthentication      | Quando o aplicativo de dispositivo complementar recebe esse estado, significa que o Serviço de Autenticação Complementar parou de aceitar solicitações de autenticação.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              |
| CredentialCollected           | Significa que outro aplicativo de dispositivo complementar chamou a segunda API e que o Serviço de Autenticação Complementar está verificando o que foi enviado. Nesse ponto, o Serviço de Autenticação Complementar não está aceitando outras solicitações de autenticação, a menos que aquela atualmente enviada não passe na verificação. O aplicativo de dispositivo complementar deve permanecer em sintonia até que o próximo estado seja atingido.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CredentialAuthenticated       | Isso significa que a credencial enviada funcionou. O credentialAuthenticated tem uma ID de dispositivo do dispositivo complementar que obteve êxito. O aplicativo de dispositivo complementar deve verificar para ter certeza de que o seu dispositivo associado foi o vencedor. Se esse não for o caso, o aplicativo de dispositivo complementar deverá evitar a exibição de qualquer fluxo pós-autenticação (como mensagem de êxito no dispositivo complementar ou talvez uma vibração nesse dispositivo). Observe que, se a credencial enviada não tiver funcionado, o estado mudará para CollectingCredential.                                                                                                                                                                                                                                                                                                                                                                                        |
| StoppoingAuthentication       | A autenticação foi bem-sucedida, e o usuário visualizou a área de trabalho. Tempo para eliminar sua tarefa em segundo plano                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                          |



Aplicativos de dispositivo complementar só devem chamar as duas APIs de autenticação nos dois primeiros estados.  Aplicativos de dispositivo complementar devem verificar em que cenário esse evento está sendo disparado. Há duas possibilidades: desbloqueio ou pós-desbloqueio. Atualmente, apenas há suporte para desbloqueio. Em versões futuras, é possível que haja suporte para cenários pós-desbloqueio. A enumeração SecondaryAuthenticationFactorAuthenticationScenario captura estas duas opções:

```C#
{
    SignIn = 0,         // Running under lock screen mode
    CredentialPrompt,   // Running post unlock
}
```

Amostra de código completa:

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.Security.Cryptography;
using System.Threading;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public sealed class AuthenticationTask : IBackgroundTask
    {
        private string _deviceId;
        private static AutoResetEvent _exitTaskEvent = new AutoResetEvent(false);
        private static IBackgroundTaskInstance _taskInstance;
        private BackgroundTaskDeferral _deferral;

        private void Authenticate()
        {
            int retryCount = 0;

            while (retryCount < 3)
            {
                //
                // Pseudo code, the svcAuthNonce should be passed to device or generated from device
                //
                IBuffer svcAuthNonce = CryptographicBuffer.GenerateRandom(256/8);

                SecondaryAuthenticationFactorAuthenticationResult authResult = await
                    SecondaryAuthenticationFactorAuthentication.StartAuthenticationAsync(
                        _deviceId,
                        svcAuthNonce);
                if (authResult.Status != SecondaryAuthenticationFactorAuthenticationStatus.Started)
                {
                    SecondaryAuthenticationFactorAuthenticationMessage message;
                    switch (authResult.Status)
                    {
                        case SecondaryAuthenticationFactorAuthenticationStatus.DisabledByPolicy:
                            message = SecondaryAuthenticationFactorAuthenticationMessage.DisabledByPolicy;
                            break;
                        case SecondaryAuthenticationFactorAuthenticationStatus.InvalidAuthenticationStage:
                            // The task might need to wait for a SecondaryAuthenticationFactorAuthenticationStageChangedEvent
                            break;
                        default:
                            return;
                    }

                    // Show error message. Limited to 512 characters wide
                    await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(null, message);
                    return;
                }

                //
                // Pseudo function:
                // The device calculates and returns sessionHmac and deviceHmac
                //
                await GetHmacsFromDevice(
                    authResult.Authentication.ServiceAuthenticationHmac,
                    authResult.Authentication.DeviceNonce,
                    authResult.Authentication.SessionNonce,
                    out deviceHmac,
                    out sessionHmac);
                if (sessionHmac == null ||
                    deviceHmac == null)
                {
                    await authResult.Authentication.AbortAuthenticationAsync(
                        "Failed to read data from device");
                    return;
                }

                SecondaryAuthenticationFactorFinishAuthenticationStatus status =
                    await authResult.Authentication.FinishAuthencationAsync(deviceHmac, sessionHmac);
                if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.NonceExpired)
                {
                    retryCount++;
                    continue;
                }
                else if (status == SecondaryAuthenticationFactorFinishAuthenticationStatus.Completed)
                {
                    // The credential data is collected and ready for unlock
                    return;
                }
            }
        }

        public void OnAuthenticationStageChanged(
            object sender,
            SecondaryAuthenticationFactorAuthenticationStageChangedEventArgs args)
        {
            // The application should check the args.StageInfo.Stage to determine what to do in next. Note that args.StageInfo.Scenario will have the scenario information (SignIn vs CredentialPrompt).

            switch(args.StageInfo.Stage)
            {
            case SecondaryAuthenticationFactorAuthenticationStage.WaitingForUserConfirmation:
                // Show welcome message
                await SecondaryAuthenticationFactorAuthentication.ShowNotificationMessageAsync(
                    null,
                    SecondaryAuthenticationFactorAuthenticationMessage.WelcomeMessageSwipeUp);
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CollectingCredential:
                // Authenticate device
                Authenticate();
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.CredentialAuthenticated:
                if (args.StageInfo.DeviceId = _deviceId)
                {
                    // Show notification on device about PC unlock
                }
                break;

            case SecondaryAuthenticationFactorAuthenticationStage.StoppingAuthentication:
                // Quit from background task
                _exitTaskEvent.Set();
                break;
            }

            Debug.WriteLine("Authentication Stage = " + args.StageInfo.AuthenticationStage.ToString());
        }

        //
        // The Run method is the entry point of a background task.
        //
        public void Run(IBackgroundTaskInstance taskInstance)
        {
            _taskInstance = taskInstance;
            _deferral = taskInstance.GetDeferral();

            // Register canceled event for this task
            taskInstance.Canceled += TaskInstanceCanceled;

            // Find all device registred by this application
            IReadOnlyList<SecondaryAuthenticationFactorInfo> deviceInfoList =
                await SecondaryAuthenticationFactorRegistration.FindAllRegisteredDeviceInfoAsync(
                    SecondaryAuthenticaitonFactorDeviceFindScope.AllUsers);

            if (deviceInfoList.Count == 0)
            {
                // Quit the task silently
                return;
            }
            _deviceId = deviceInfoList[0].DeviceId;
            Debug.WriteLine("Use first device '" + _deviceId + "' in the list to signin");

            // Register AuthenticationStageChanged event
            SecondaryAuthenticationFactorRegistration.AuthenticationStageChanged += OnAuthenticationStageChanged;

            // Wait the task exit event
            _exitTaskEvent.WaitOne();

            _deferral.Complete();
        }

        void TaskInstanceCanceled(IBackgroundTaskInstance sender, BackgroundTaskCancellationReason reason)
        {
            _exitTaskEvent.Set();
        }
    }
}
```

### Registrar uma tarefa em segundo plano

Quando o aplicativo de dispositivo complementar registra o primeiro dispositivo complementar, ele também deve registrar seu componente de tarefa em segundo plano que transmitirá informações de autenticação entre o dispositivo e o serviço de autenticação do dispositivo complementar.

```C#
using System;
using Windows.Security.Authentication.Identity.Provider;
using Windows.Storage.Streams;
using Windows.ApplicationModel.Background;

namespace SecondaryAuthFactorSample
{
    public class BackgroundTaskManager
    {
        // Register background task
        public static async Task<IBackgroundTaskRegistration> GetOrRegisterBackgroundTaskAsync(
            string bgTaskName,
            string taskEntryPoint)
        {
            // Check if there's an existing background task already registered
            var bgTask = (from t in BackgroundTaskRegistration.AllTasks
                          where t.Value.Name.Equals(bgTaskName)
                          select t.Value).SingleOrDefault();
            if (bgTask == null)
            {
                BackgroundAccessStatus status =
                    BackgroundExecutionManager.RequestAccessAsync().AsTask().GetAwaiter().GetResult();

                if (status == BackgroundAccessStatus.Denied)
                {
                    Debug.WriteLine("Background Execution is denied.");
                    return null;
                }

                var taskBuilder = new BackgroundTaskBuilder();
                taskBuilder.Name = bgTaskName;
                taskBuilder.TaskEntryPoint = taskEntryPoint;
                taskBuilder.SetTrigger(new SecondaryAuthenticationFactorAuthenticationTrigger());
                bgTask = taskBuilder.Register();
                // Background task is registered
            }

            bgTask.Completed += BgTask_Completed;
            bgTask.Progress += BgTask_Progress;

            return bgTask;
        }
    }
}
```

### Erros e mensagens

A Estrutura de Dispositivo Complementar é responsável por fornecer feedback para o usuário sobre o êxito ou a falha do processo de entrada. A Estrutura de Dispositivo Complementar fornecerá um estoque de texto e mensagens de erro (traduzidos) para escolha pelo aplicativo de dispositivo complementar. Esses elementos serão exibidos na interface de usuário de logon.

![erro de dispositivo complementar](images/companion-device-5.png)

Aplicativos de dispositivo complementar podem usar ShowNotificationMessageAsync para mostrar mensagens ao usuário como parte da interface de usuário de logon. Chame essa API quando um sinal de intenção estiver disponível. Observe que esse sinal de intenção sempre deve ser coletado no lado do dispositivo complementar.

Existem dois tipos de mensagens: orientações e erros.

Mensagens de orientação foram projetadas para mostrar ao usuário como iniciar o processo de desbloqueio. Essas mensagens são apenas mostradas ao usuário uma vez, após o primeiro registro do dispositivo.

Mensagens de erro sempre são mostradas. Elas serão mostradas ao usuário por 5 segundos, desaparecendo em seguida. Considerando que o sinal de intenção deve ser coletado antes da exibição de mensagens ao usuário e que o usuário fornecerá essa intenção somente usando um desses dispositivos complementares, não deve haver uma situação em que vários dispositivos complementares competem para mostrar mensagens de erro. Como resultado, a Estrutura de Dispositivo Complementar não mantém filas. Quando um chamador solicitar uma mensagem de erro, ela será mostrada por 5 segundos, e todas as outras solicitações para mostrar mensagens de erro nesses 5 segundos serão ignoradas. Após os 5 segundos, surgirá a oportunidade para outro chamador mostrar uma mensagem de erro. Proibimos que qualquer chamador obstrua o canal de erros.

Mensagens de erro e orientações estão estruturadas da seguinte maneira. O nome do dispositivo é um parâmetro transmitido pelo aplicativo de dispositivo complementar como parte de ShowNotificationMessageAsync.

**Diretrizes**

- "Passe o dedo para cima ou pressione a barra de espaço para entrar com o *nome do dispositivo*"
- "Toque *nome do dispositivo* no leitor de NFC para entrar."
- "Procurando *nome do dispositivo*..."
- "Conecte *nome do dispositivo* a uma porta USB para entrar."

**Erros**

- "Veja *nome do dispositivo* para obter instruções de entrada."
- "Ative o Bluetooth para usar *nome do dispositivo* para entrar."
- "Ative o NFC para usar *nome do dispositivo* para entrar."
- "Conecte-se a uma rede Wi-Fi para usar *nome do dispositivo* para entrar."
- "Toque em *nome do dispositivo* novamente."
- "Sua empresa impede entradas com *nome do dispositivo*. Use outra opção de entrada."
- "Toque em *nome do dispositivo* para entrar."
- "Coloque o dedo sobre *nome do dispositivo* para entrar."
- "Passe o dedo em *nome do dispositivo* para entrar."
- "Não foi possível entrar com *nome do dispositivo*. Use outra opção de entrada."
- "Houve um erro. Use outra opção de entrada e, em seguida, configure o *nome do dispositivo* novamente."
- "Tente novamente."
- "Diga sua senha falada em *nome do dispositivo*."
- "Pronto para entrar com *nome do dispositivo*."
- "Use outra opção de entrada pela primeira vez e, em seguida, você pode usar o *nome do dispositivo* para entrar."

### Enumerando dispositivos registrados

O aplicativo de dispositivo complementar pode enumerar a lista de dispositivos complementares registrados por meio da chamada FindAllRegisteredDeviceInfoAsync. Essa API dá suporte a dois tipos de consulta definidos por meio da enumeração SecondaryAuthenticaitonFactorDeviceFindScope:

```C#
{
    User = 0,
    AllUsers,
}
```

O primeiro escopo retorna a lista de dispositivos para o usuário conectado. O segundo retorna a lista de todos os usuários nesse computador. O primeiro escopo deve ser usado na ocasião do cancelamento do registro para evitar o cancelamento do registro do dispositivo complementar de outro usuário. O outro deve ser usado em vez de autenticação ou registro: momento do registro, essa enumeração pode evitar aplicativo tentar registrar o mesmo dispositivo complementar duas vezes.

Observe que, mesmo que o aplicativo não realize essa verificação, o computador rejeitará o registro do mesmo dispositivo complementar mais de uma vez. No momento da autenticação, usar o escopo AllUsers ajuda a aplicativo de dispositivo complementar a oferecer suporte ao fluxo de troca de usuários: fazer logon do usuário A quando o usuário B está conectado (isso requer que ambos os usuários tenham instalado o aplicativo de dispositivo complementar e que o usuário A tenha registrado seus dispositivos complementares no computador que, por sua vez, deve estar na tela de bloqueio (ou na tela de logon)).

## Requisitos de segurança

O serviço de autenticação complementar fornece as seguintes proteções de segurança.

- Malwares em um dispositivo da área de trabalho do Windows 10 em execução como um usuário de privilégios médios ou um contêiner de aplicativo não podem usar o dispositivo complementar para acessar chaves de credenciais do usuário (armazenadas como parte do Microsoft Passport) no computador silenciosamente.
- Um usuário mal-intencionado em um dispositivo de área de trabalho do Windows 10 não pode usar o dispositivo complementar que pertence a outro usuário nesse dispositivo de área de trabalho do Windows 10 para obter acesso silencioso às chaves de credenciais de usuário dele (no mesmo dispositivo de área de trabalho do Windows 10).
- Malwares no dispositivo complementar não podem obter acesso silencioso a chaves de credenciais de usuário no dispositivo de área de trabalho do Windows 10, incluindo o aproveitamento da funcionalidade ou do código desenvolvido especificamente para a Estrutura de Dispositivo Complementar.
- Um usuário mal-intencionado não pode desbloquear dispositivos de área de trabalho do Windows 10 capturando o tráfego entre o dispositivo complementar e o dispositivo de área de trabalho do Windows 10 e reproduzindo-o posteriormente. O uso de nonce, authkey e HMAC em nosso protocolo garante proteção contra ataques de repetição.
- Malwares ou usuários mal-intencionados em um computador invasor não podem usar o dispositivo complementar para obter acesso ao computador do usuário honesto. Isso é feito através de uma autenticação mútua entre o Serviço de Autenticação Complementar e o dispositivo complementar por meio do uso de authkey e HMAC em nosso protocolo.

O segredo para se obter as proteções de segurança enumeradas acima é proteger as chaves HMAC contra acesso não autorizado e também verificar a presença do usuário. Mais especificamente, os seguintes requisitos devem ser atendidos:

- Fornecer proteção contra clonagem do dispositivo complementar
- Fornecer proteção contra interceptações ao enviar chaves HMAC no momento do registro para o computador
- Garantir que o sinal de presença do usuário esteja disponível.



<!--HONumber=Jun16_HO4-->


