---
title: Desbloqueio do Windows com dispositivos complementares (IoT) do Windows Hello
description: Dispositivo complementar do Windows Hello é um dispositivo que pode atuar em conjunto com sua área de trabalho do Windows 10 para melhorar a experiência de autenticação do usuário. Usando a estrutura de dispositivo complementar do Windows Hello, um dispositivo complementar pode fornecer uma experiência avançada para o Windows Hello mesmo quando a biometria não está disponível (por exemplo, se a área de trabalho do Windows 10 não tiver uma câmera para autenticação de face ou dispositivo leitor de impressão digital, por exemplo).
ms.date: 02/08/2017
ms.topic: article
keywords: Windows 10, uwp, segurança
ms.assetid: 89f3d331-20cd-457b-83e8-1a22aaab2658
ms.localizationpriority: medium
ms.openlocfilehash: b33cf07ef10d0891f2747a06caf098b7d37b62f3
ms.sourcegitcommit: c01c29cd97f1cbf050950526e18e15823b6a12a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/05/2018
ms.locfileid: "8712812"
---
# <a name="windows-unlock-with-windows-hello-companion-iot-devices"></a>Desbloqueio do Windows com dispositivos complementares (IoT) do Windows Hello

Dispositivo complementar do Windows Hello é um dispositivo que pode atuar em conjunto com sua área de trabalho do Windows 10 para melhorar a experiência de autenticação do usuário. Usando a estrutura de dispositivo complementar do Windows Hello, um dispositivo complementar pode fornecer uma experiência avançada para o Windows Hello mesmo quando a biometria não está disponível (por exemplo, se a área de trabalho do Windows 10 não tiver uma câmera para autenticação de face ou dispositivo leitor de impressão digital, por exemplo).

> **Observação** A estrutura de dispositivo complementar do Windows Hello é um recurso especializado não disponível para todos os desenvolvedores de aplicativos. Para usar essa estrutura, seu app deve ser especificamente provisionado pela Microsoft e listar a funcionalidade restrita *secondaryAuthenticationFactor* em seu manifesto. Para obter aprovação, entre em contato com [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com).

## <a name="introduction"></a>Introdução

> Para uma visão geral em vídeo, consulte a sessão [Desbloqueio do Windows com dispositivos complementares IoT](https://channel9.msdn.com/Events/Build/2016/P491) da compilação 2016 no Channel 9.

> Para obter exemplos de código, consulte o repositório Github de estrutura de dispositivo complementar do [Windows Hello](https://github.com/Microsoft/companion-device-framework).

### <a name="use-cases"></a>Casos de uso

Há várias maneiras de usar a estrutura de dispositivo complementar do Windows Hello para criar uma excelente experiência de desbloqueio do Windows com um dispositivo complementar. Por exemplo, os usuários podem:

- Anexar seus dispositivos complementares ao computador via USB, tocar no botão do dispositivo complementar e desbloquear automaticamente seus computadores.
- Carregar um telefone no bolso que já esteja emparelhado com o computador via Bluetooth. Ao pressionarem a barra de espaço em seus computadores, seus telefones recebem uma notificação. Basta aprová-la para desbloquear o computador.
- Tocar seus dispositivos complementares em um leitor de NFC para desbloquear rapidamente seus computadores.
- Usar um acessório de fitness que já tenha autenticado o usuário. Ao se aproximarem do computador e realizarem um gesto especial (como bater palmas), o computador será desbloqueado.

### <a name="biometric-enabled-windows-hello-companion-devices"></a>Dispositivos complementares do Windows Hello habilitados com biometria

Se o dispositivo complementar for compatível com biometria, em alguns casos a [Windows Biometric Framework](https://msdn.microsoft.com/library/windows/hardware/mt608302(v=vs.85).aspx) pode ser uma solução melhor do que a estrutura de dispositivo complementar do Windows Hello. Entre em contato com [cdfonboard@microsoft.com](mailto:cdfonboard@microsoft.com) e ajudaremos você a escolher a abordagem correta.

### <a name="components-of-the-solution"></a>Componentes da solução

O diagrama abaixo mostra os componentes da solução e quem é responsável por criá-los.

![visão geral da estrutura](images/companion-device-1.png)

A estrutura de dispositivo complementar do Windows Hello é implementada como um serviço em execução no Windows (chamado de Serviço de Autenticação Complementar neste artigo). Esse serviço é responsável por gerar um token de desbloqueio que precisa ser protegido por uma chave HMAC armazenada no dispositivo complementar do Windows Hello. Isso garante que o acesso ao token de desbloqueio exija a presença do dispositivo complementar do Windows Hello. Por cada tupla (computador, usuário do Windows), haverá um único token de desbloqueio.

A integração com a Estrutura de dispositivo complementar do Windows Hello requer:

- Um aplicativo de dispositivo complementar [UPW (Plataforma Universal do Windows)](https://msdn.microsoft.com/windows/uwp/get-started/universal-application-platform-guide) para o dispositivo complementar do Windows Hello, baixado na Windows Store. 
- A capacidade de criar duas chaves HMAC de 256 bits no dispositivo complementar do Windows Hello e de gerar a HMAC com ele (usando SHA-256).
- Configurações de segurança na área de trabalho do Windows 10 corretamente definidas. O Serviço de Autenticação Complementar exigirá que esse PIN seja configurado antes que qualquer dispositivo complementar do Windows Hello possa ser conectado a ele. Os usuários devem configurar um PIN via Configurações > Contas > Opções de entrada.

Além dos requisitos acima, o aplicativo de dispositivo complementar do Windows Hello é responsável por:

- Experiência do usuário e identidade visual do registro inicial e, mais tarde, o cancelamento do registro do dispositivo complementar do Windows Hello.
- Execução em segundo plano, descoberta do dispositivo complementar do Windows Hello, comunicação com esse dispositivo e também com o Serviço de Autenticação Complementar.
- Tratamento de erros

Normalmente, dispositivos complementares são fornecidos com um aplicativo para configuração inicial, como configurar um acessório de fitness pela primeira vez. A funcionalidade descrita neste documento pode fazer parte desse aplicativo, e um aplicativo separado não será necessário.  

### <a name="user-signals"></a>Sinais do usuário

Cada dispositivo complementar do Windows Hello deve ser combinado com um aplicativo que dê suporte a três sinais do usuário. Esses sinais podem ser no formato de uma ação ou de um gesto.

- **Sinal de intenção**: permite que o usuário mostre sua intenção de desbloquear, por exemplo, pressionando um botão no dispositivo complementar do Windows Hello. O sinal de intenção deve ser coletado no lado do **dispositivo complementar do Windows Hello**.
- **Sinal de presença do usuário**: prova a presença do usuário. O dispositivo complementar do Windows Hello pode, por exemplo, exigir um PIN antes de poder ser usado para desbloquear o computador (isso não deve ser confundido com o PIN do computador) ou pode exigir o pressionamento de um botão.
- **Sinal de diferenciação**: remove a ambiguidade de qual área de trabalho do Windows 10 o usuário deseja desbloquear quando várias opções estão disponíveis para o dispositivo complementar do Windows Hello.

Qualquer número desses sinais de usuário pode ser combinado em um único sinal. Sinais de presença e intenção do usuário devem ser necessários em cada uso.

### <a name="registration-and-future-communication-between-a-pc-and-windows-hello-companion-devices"></a>Registro e comunicação futura entre um computador e dispositivos complementares do Windows Hello

Antes que um dispositivo complementar do Windows Hello possa ser conectado à estrutura de dispositivo complementar do Windows Hello, ele precisa ser registrado nessa estrutura. A experiência de registro fica completamente a cargo do aplicativo de dispositivo complementar do Windows Hello.

O relacionamento registrado entre o dispositivo complementar do Windows Hello e o dispositivo de área de trabalho do Windows 10 pode ser de um-para-muitos (ou seja, um dispositivo complementar pode ser usado para vários dispositivos de área de trabalho do Windows 10). No entanto, cada dispositivo complementar do Windows Hello só pode ser usado para um usuário em cada dispositivo de área de trabalho do Windows 10.   

Para que um dispositivo complementar do Windows Hello possa se comunicar com um computador, é necessário chegar a um acordo acerca do transporte a ser usado. Essa escolha fica a cargo do aplicativo de dispositivo complementar do Windows Hello. A estrutura de dispositivo complementar do Windows Hello não impõe nenhuma limitação sobre o tipo de transporte (USB, NFC, WiFi, BT, BLE etc) ou protocolo em uso entre o dispositivo complementar do Windows Hello e o aplicativo de dispositivo complementar do Windows Hello no lado do dispositivo de área de trabalho do Windows 10. No entanto, ela sugere certas considerações de segurança para a camada de transporte, conforme descrito na seção "Requisitos de segurança" deste documento. É responsabilidade do provedor do dispositivo fornecer esses requisitos. A estrutura não os fornece para você.


## <a name="user-interaction-model"></a>Modelo de interação do usuário

### <a name="windows-hello-companion-device-app-discovery-installation-and-first-time-registration"></a>Descoberta, instalação e registro inicial do aplicativo de dispositivo complementar do Windows Hello

Um fluxo de trabalho do usuário típico é o seguinte:

- O usuário configura o PIN em cada um dos dispositivos de área de trabalho do Windows 10 de destino que ele deseja desbloquear com esse dispositivo complementar do Windows Hello.
- O usuário executa o aplicativo de dispositivo complementar do Windows Hello em seus dispositivos de área de trabalho do Windows 10 para registrar seu dispositivo complementar do Windows Hello na área de trabalho do Windows 10.

Observações:

- Recomendamos que a descoberta, o download e a ativação do aplicativo de dispositivo complementar do Windows Hello seja simplificada e, se possível, automatizada (por exemplo, o aplicativo pode ser baixado ao tocar o dispositivo complementar do Windows Hello em um leitor de NFC no lado do dispositivo de área de trabalho do Windows 10). No entanto, isso é responsabilidade do dispositivo complementar do Windows Hello e do aplicativo de dispositivo complementar do Windows Hello.
- Em um ambiente corporativo, o aplicativo de dispositivo complementar do Windows Hello pode ser implantado via MDM.
- O aplicativo de dispositivo complementar do Windows Hello é responsável por mostrar ao usuário mensagens de erro que ocorrem como parte do registro.

### <a name="registration-and-de-registration-protocol"></a>Protocolo de registro e cancelamento de registro

O diagrama a seguir ilustra como o dispositivo complementar do Windows Hello interage com o Serviço de Autenticação Complementar durante o registro.  

![fluxo de registro](images/companion-device-2.png)

Há duas chaves usadas em nosso protocolo:

- Chave de dispositivo (**devicekey**): usada para proteger tokens de desbloqueio necessários para o computador desbloquear o Windows.
- A chave de autenticação (**authkey**): usada para autenticar mutuamente o dispositivo complementar do Windows Hello e o Serviço de Autenticação Complementar.

A chave de dispositivo e as chaves de autenticação são trocadas na ocasião do registro entre o aplicativo de dispositivo complementar do Windows Hello e o dispositivo complementar do Windows Hello. Como resultado, o aplicativo de dispositivo complementar do Windows Hello e o dispositivo complementar do Windows Hello devem usar um transporte seguro para proteger chaves.

Além disso, observe que, embora o diagrama acima mostre duas chaves HMAC geradas no dispositivo complementar do Windows Hello, também é possível que o aplicativo as gere e as envie ao dispositivo complementar do Windows Hello para armazenamento.

### <a name="starting-authentication-flows"></a>Iniciando fluxos de autenticação

Há duas maneiras de o usuário iniciar o fluxo de entrada na área de trabalho do Windows 10 usando a estrutura de dispositivo complementar do Windows Hello (isto é, fornecer um sinal de intenção):

- Abra a tampa do notebook ou pressione a barra de espaço ou passe o dedo para cima no computador.
- Execute uma ação ou um gesto no lado do dispositivo complementar do Windows Hello.

Cabe ao dispositivo complementar do Windows Hello selecionar qual deles é o ponto de partida. A estrutura de dispositivo complementar do Windows Hello informará o aplicativo de dispositivo complementar quando a primeira opção ocorrer. Para a segunda opção, o aplicativo de dispositivo complementar do Windows Hello deve consultar o dispositivo complementar para ver se esse evento foi capturado. Isso garante que o dispositivo complementar do Windows Hello colete o sinal de intenção antes que o desbloqueio seja bem-sucedido.

### <a name="windows-hello-companion-device-credential-provider"></a>Provedor de credenciais do dispositivo complementar do Windows Hello

Há um novo provedor de credenciais no Windows 10 que lida com todos os dispositivos complementares do Windows Hello.

O provedor de credenciais de dispositivo complementar do Windows Hello é responsável por iniciar a tarefa em segundo plano do dispositivo complementar por meio do acionamento de um gatilho. O gatilho é definido pela primeira vez quando o computador desperta e uma tela de bloqueio é exibida. A segunda vez é quando o computador está entrando na interface de usuário de logon e o provedor de credenciais de dispositivo complementar do Windows Hello é o bloco selecionado.

A biblioteca auxiliar do aplicativo de dispositivo complementar do Windows Hello escutará a mudança de status da tela de bloqueio e enviará o evento correspondente à tarefa de segundo plano do dispositivo complementar do Windows Hello.

Se houver várias tarefas de segundo plano do dispositivo complementar do Windows Hello, a primeira delas que terminar o processo de autenticação desbloqueará o computador. O serviço de autenticação de dispositivo complementar ignorará qualquer chamada de autenticação restante.

A experiência no lado do dispositivo complementar do Windows Hello é mantida e gerenciada pelo aplicativo de dispositivo complementar do Windows Hello. A estrutura de dispositivo complementar do Windows Hello não tem controle sobre essa parte da experiência do usuário. Mais especificamente, o provedor de autenticação complementar informa o aplicativo de dispositivo complementar do Windows Hello (por meio de seu aplicativo em segundo plano) sobre mudanças de estado na interface de usuário de logon (por exemplo, a tela de bloqueio acabou de ser ativada ou o usuário acabou de dispensá-la pressionando a barra de espaço), e é responsabilidade do aplicativo de dispositivo complementar do Windows Hello criar uma experiência com base nisso (por exemplo, quando o usuário pressionar a barra de espaço e dispensar a tela de desbloqueio, começar a procurar o dispositivo via USB).

A estrutura de dispositivo complementar do Windows Hello fornecerá um estoque de texto e mensagens de erro (traduzidos) para escolha pelo aplicativo de dispositivo complementar do Windows Hello. Esses elementos serão exibidos na parte superior da tela de bloqueio (ou na interface de usuário de logon). Para saber mais, consulte a seção Lidando com mensagens e erros.

### <a name="authentication-protocol"></a>Protocolo de autenticação

Depois que a tarefa em segundo plano associada a um aplicativo de dispositivo complementar do Windows Hello é iniciada por gatilho, ela é responsável por solicitar ao dispositivo complementar do Windows Hello para validar um valor HMAC calculado pelo Serviço de autenticação complementar e ajudar a calcular dois valores HMAC:
- Validate Service HMAC = HMAC(authentication key, service nonce || device nonce || session nonce).
- Calcule o HMAC da chave de dispositivo com um nonce.
- Calcule o HMAC da chave de autenticação com o primeiro valor HMAC concatenado com um nonce gerado pelo Serviço de Autenticação Complementar.

O segundo valor calculado é usado pelo serviço para autenticar o dispositivo e também para evitar ataques de reprodução no canal de transporte.

![fluxo de registro](images/companion-device-3.png)

## <a name="lifecycle-management"></a>Gerenciamento do ciclo de vida

### <a name="register-once-use-everywhere"></a>Registre uma vez e use em qualquer lugar

Sem um servidor back-end, os usuários devem registrar seus dispositivos complementares do Windows Hello em cada dispositivo de área de trabalho do Windows 10 separadamente.

Um fornecedor de dispositivo complementar ou OEM pode implementar um serviço Web para circular o estado de registro entre áreas de trabalho do Windows 10 ou dispositivos móveis do usuário. Para obter mais detalhes, consulte a seção Serviço de roaming, revogação e filtro.

### <a name="pin-management"></a>Gerenciamento de PIN

Antes que um dispositivo complementar possa ser usado, um PIN precisa ser configurado no dispositivo de área de trabalho do Windows 10. Isso garante que o usuário tenha um backup no caso de seu dispositivo complementar do Windows Hello não funcionar. O PIN é algo que o Windows gerencia e que os aplicativos nunca veem. Para alterá-lo, o usuário navega até Configurações > Contas > Opções de entrada.

### <a name="management-and-policy"></a>Gerenciamento e políticas

Os usuários podem remover um dispositivo complementar do Windows Hello de áreas de trabalho do Windows 10 executando o aplicativo de dispositivo complementar do Windows Hello nesse dispositivo de área de trabalho.

As empresas têm duas opções para controlar a estrutura de dispositivo complementar do Windows Hello:

- Ativar ou desativar o recurso
- Definir a lista branca de dispositivos complementares do Windows Hello permitidos usando o cofre de aplicativo do Windows

A estrutura de dispositivo complementar do Windows Hello não dá suporte a uma maneira centralizada de manter o inventário de dispositivos complementares disponíveis ou a um método para filtrar ainda mais quais instâncias de um tipo de dispositivo complementar do Windows Hello são permitidas (por exemplo, apenas dispositivos complementares com números de série entre X e Y são permitidos). No entanto, os desenvolvedores de aplicativos podem criar um serviço para fornecer essa funcionalidade. Para obter mais detalhes, consulte a seção Serviço de roaming, revogação e filtro.

### <a name="revocation"></a>Revogação

A estrutura de dispositivo complementar do Windows Hello não dá suporte à remoção de um dispositivo complementar de um dispositivo de área de trabalho do Windows 10 específico remotamente. Em vez disso, os usuários podem remover o dispositivo complementar do Windows Hello por meio do aplicativo de dispositivo complementar do Windows Hello em execução nessa área de trabalho do Windows 10.

No entanto, fornecedores de dispositivos complementares podem criar um serviço para fornecer a funcionalidade de revogação remota. Para obter mais detalhes, consulte a seção Serviço de roaming, revogação e filtro.

### <a name="roaming-and-filter-services"></a>Serviços de roaming e filtro

Fornecedores de dispositivos complementares podem implementar um serviço Web que pode ser usado para os seguintes cenários:

- Um serviço de filtro para uma empresa: uma empresa pode limitar o conjunto de dispositivos complementares do Windows Hello que podem funcionar em seu ambiente para selecionar apenas alguns de um fornecedor específico. Por exemplo, a empresa Contoso pode encomendar do Fornecedor X 10.000 dispositivos complementares do Modelo Y e garantir que apenas esses dispositivos funcionarão no domínio da Contoso (e não em qualquer outro modelo de dispositivo do Fornecedor X).
- Inventário: uma empresa pode determinar a lista de dispositivos complementares existente usados em um ambiente corporativo.
- Revogação em tempo real: se um funcionário comunicar que seu dispositivo complementar foi perdido ou roubado, o serviço Web poderá ser usado para revogar esse dispositivo.
- Roaming: basta que um usuário registre seu dispositivo complementar, e ele funcionará em todas as suas áreas de trabalho do Windows 10 e dispositivos móveis.

A implementação desses recursos requer que aplicativo de dispositivo complementar do Windows Hello faça uma verificação com o serviço Web na ocasião do registro e do uso. O aplicativo de dispositivo complementar do Windows Hello pode ser otimizado para cenários de logon em cache, por exemplo, exigindo a verificação com o serviço Web apenas uma vez por dia (às custas de prolongar o tempo de revogação a até um dia).  

## <a name="windows-hello-companion-device-framework-api-model"></a>Modelo de API da estrutura de dispositivo complementar do Windows Hello

### <a name="overview"></a>Visão geral

Um aplicativo de dispositivo complementar do Windows Hello deve conter dois componentes: um aplicativo de primeiro plano com uma interface de usuário responsável para registrar e cancelar o registro do dispositivo e uma tarefa em segundo plano que manipula a autenticação.

O fluxo geral da API é o seguinte:

1. Registrar o dispositivo complementar do Windows Hello
    * Verifique se o dispositivo está próximo e veja sua capacidade (se necessário)
    * Gere duas chaves HMAC (no lado do dispositivo complementar ou no lado do aplicativo)
    * Chame RequestStartRegisteringDeviceAsync
    * Chame FinishRegisteringDeviceAsync
    * Certifique-se de que aplicativo de dispositivo complementar do Windows Hello armazene chaves HMAC (se permitido) e ele descarte suas cópias
2. Registrar sua tarefa em segundo plano
3. Aguarde o evento certo na tarefa em segundo plano
    * WaitingForUserConfirmation: aguarde este evento se a ação ou o gesto do usuário no lado do dispositivo complementar do Windows Hello for necessária para iniciar o fluxo de autenticação
    * CollectingCredential: aguarde esse evento se o dispositivo complementar do Windows Hello depender da ação ou do gesto do usuário no lado do computador para iniciar o fluxo de autenticação (por exemplo, pressionando a barra de espaço)
    * Outro gatilho, como um cartão inteligente: certifique-se de consultar o estado atual de autenticação chamar as APIs certas.
4. Manter o usuário informado sobre mensagens de erro ou próximas etapas necessárias chamando ShowNotificationMessageAsync. Chame essa API somente depois que um sinal de intenção for coletado
5. Desbloquear
    * Certifique-se de que os sinais de intenção e presença do usuário tenham sido coletados
    * Chame StartAuthenticationAsync
    * Comunique-se com o dispositivo complementar para realizar operações de HMAC necessárias
    * Chame FinishAuthenticationAsync
6. Cancelar o registro de um dispositivo complementar do Windows Hello quando o usuário solicitar (por exemplo, se ele tiver perdido esse dispositivo)
    * Enumere o dispositivo associado do Windows Hello para o usuário conectado via FindAllRegisteredDeviceInfoAsync
    * Cancele seu registro usando UnregisterDeviceAsync

### <a name="registration-and-de-registration"></a>Registro e cancelamento do registro

O registro requer duas chamadas de API para o Serviço de Autenticação Complementar: RequestStartRegisteringDeviceAsync e FinishRegisteringDeviceAsync.

Antes que qualquer uma dessas chamadas seja feita, o aplicativo de dispositivo complementar do Windows Hello deve garantir que o dispositivo complementar do Windows Hello esteja disponível. Se o dispositivo complementar do Windows Hello for responsável pela geração de chaves HMAC (chaves de autenticação e dispositivo), o aplicativo de dispositivo complementar do Windows Hello também deverá solicitar que o dispositivo complementar as gere antes de fazer qualquer uma das duas chamadas acima. Se o aplicativo de dispositivo complementar do Windows Hello for responsável pela geração de chaves HMAC, ele deverá fazer isso antes de fazer as duas chamadas acima.

Além disso, como parte da primeira chamada à API (RequestStartRegisteringDeviceAsync), o aplicativo de dispositivo complementar do Windows Hello deve decidir sobre as funcionalidades do dispositivo e estar preparado para transmiti-las como parte da chamada à API. Por exemplo, se dispositivo complementar do Windows Hello dá suporte ao armazenamento seguro para chaves HMAC. Se o mesmo aplicativo de dispositivo complementar do Windows Hello for usado para gerenciar várias versões do mesmo dispositivo complementar e essas funcionalidades forem modificadas (e exigirem uma consulta de dispositivo para chegarem a uma decisão), convém que essa consulta ocorra antes que a primeira chamada de API seja feita.   

A primeira API (RequestStartRegisteringDeviceAsync) retornará um identificador usado pela segunda API (FinishRegisteringDeviceAsync). A primeira chamada para registro iniciará o prompt de PIN para garantir que o usuário esteja presente. Se nenhum PIN estiver configurado, essa chamada falhará. O aplicativo de dispositivo complementar do Windows Hello também pode consultar se o PIN está configurado ou não por meio da chamada KeyCredentialManager.IsSupportedAsync. A chamada RequestStartRegisteringDeviceAsync também poderá falhar se a política tiver desabilitado o uso do dispositivo complementar do Windows Hello.

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

A segunda chamada (FinishRegisteringDeviceAsync) conclui o registro. Como parte do processo de registro, o aplicativo de dispositivo complementar do Windows Hello pode armazenar dados de configuração de dispositivos complementares com o Serviço de Autenticação Complementar. Há um limite de tamanho de 4K para esses dados. Esses dados estarão disponíveis para o aplicativo de dispositivo complementar do Windows Hello na ocasião da autenticação. Esses dados podem ser usados, por exemplo, para conexão com o dispositivo complementar do Windows Hello, como um endereço MAC, ou se o dispositivo complementar do Windows Hello não tiver um armazenamento e quiser usar o computador para armazenamento. Observe que todos os dados confidenciais armazenados como parte dos dados de configuração devem ser criptografados com uma chave conhecida somente pelo dispositivo complementar do Windows Hello. Além disso, considerando que os dados de configuração são armazenados por um serviço Windows, eles estão disponíveis ao aplicativo de dispositivo complementar do Windows Hello em vários perfis de usuário.

O aplicativo de dispositivo complementar do Windows Hello pode chamar AbortRegisteringDeviceAsync para cancelar o registro e transmitir um código de erro. O Serviço de Autenticação Complementar registrará o erro nos dados de telemetria. Um bom exemplo para essa chamada seria quando algo deu errado com o dispositivo complementar do Windows Hello e ele não pôde concluir o registro (por exemplo, não é possível armazenar chaves HMAC ou a conexão BT foi perdida).

O aplicativo de dispositivo complementar do Windows Hello deve fornecer uma opção para o usuário cancelar o registra de seu dispositivo complementar do Windows Hello na área de trabalho do Windows 10 (por exemplo, se ele tiver perdido seu dispositivo complementar ou comprado uma versão mais recente). Quando o usuário selecionar essa opção, o aplicativo de dispositivo complementar do Windows Hello deverá chamar UnregisterDeviceAsync. Essa chamada pelo aplicativo de dispositivo complementar do Windows Hello disparará o serviço de autenticação de dispositivo complementar de forma a excluir todos os dados (incluindo chaves HMAC) correspondentes à ID de dispositivo específica e ao AppId do aplicativo de chamada no lado do computador. Essa chamada de API não tenta excluir chaves HMAC no lado do aplicativo de dispositivo complementar do Windows Hello ou no lado do dispositivo complementar. Isso é deixado para o aplicativo de dispositivo complementar do Windows Hello implementar.

O aplicativo de dispositivo complementar do Windows Hello é responsável por mostrar mensagens de erro que ocorrem nas fases de registro e de cancelamento de registro.

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
                    SecondaryAuthenticationFactorDeviceCapabilities.SecureStorage |
                        SecondaryAuthenticationFactorDeviceCapabilities.HMacSha256 |
                        SecondaryAuthenticationFactorDeviceCapabilities.StoreKeys,
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
                    SecondaryAuthenticationFactorDeviceFindScope.User);

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

### <a name="authentication"></a>Autenticação

A autenticação exige duas chamadas de API para o Serviço de Autenticação Complementar: StartAuthenticationAsync e FinishAuthencationAsync.

A primeira API de inicialização retornará um identificador usado pela segunda API.  A primeira chamada retorna, entre outras coisas, um nonce que, quando concatenado com outros elementos, precisa ser codificado em HMAC com a chave de dispositivo armazenada no dispositivo complementar do Windows Hello. A segunda chamada retorna os resultados do HMAC com a chave de dispositivo e pode terminar em uma autenticação bem-sucedida (isto é, o usuário verá sua área de trabalho).

A primeira API de inicialização (StartAuthenticationAsync) poderá falhar se a política tiver desabilitado esse dispositivo complementar do Windows Hello após o registro inicial. Ela também poderá falhar se a chamada de API tiver sido feita fora dos estados WaitingForUserConfirmation ou CollectingCredential (informações adicionais sobre isso mais adiante nesta seção). Ela também poderá falhar se um aplicativo de dispositivo complementar com registro cancelado a chamar. A enumeração SecondaryAuthenticationFactorAuthenticationStatus resume os possíveis resultados:

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

O cronograma das duas chamadas de API (StartAuthenticationAsync e FinishAuthencationAsync) precisa estar alinhado a como o dispositivo complementar do Windows Hello coleta sinais de intenção, presença do usuário e diferenciação (veja Sinais do usuário para obter mais detalhes). Por exemplo, a segunda chamada não deve ser enviada até que o sinal de intenção esteja disponível. Em outras palavras, o computador não deverá ser desbloqueado se um usuário não tiver expressado a intenção de fazer isso. Para explicar melhor o processo, suponha que proximidade do Bluetooth seja usada para o desbloqueio do computador. Nesse caso, um sinal de intenção explícito deve ser coletado para evitar que o computador seja desbloqueado se o usuário se aproximar dele no caminho para a cozinha. Além disso, o nonce retornado pela primeira chamada está associado ao tempo (20 segundos) e expirará depois de um determinado período. Como resultado, a primeira chamada só deverá ser feita quando o aplicativo de dispositivo complementar do Windows Hello tiver uma boa indicação da presença do dispositivo complementar, por exemplo, o dispositivo complementar é inserido na porta USB ou toca o leitor de NFC. Com o Bluetooth, é necessário ter cautela para evitar afetar a atividade da bateria no lado do computador ou outras atividades Bluetooth que estejam ocorrendo nesse ponto ao verificar a presença do dispositivo complementar do Windows Hello. Além disso, se o sinal de presença do usuário tiver que ser fornecido (por exemplo, digitando um PIN), convém que a primeira chamada de autenticação seja feita somente após a coleta desse sinal.

A estrutura de dispositivo complementar do Windows Hello ajuda o aplicativo de dispositivo complementar do Windows Hello a tomar decisões bem-informadas no que diz respeito a quando fazer as duas chamadas acima, fornecendo uma imagem completa de onde usuário se encontra no fluxo de autenticação. A estrutura de dispositivo complementar do Windows Hello oferece essa funcionalidade notificações de mudança de estado de bloqueio para a tarefa em segundo plano do aplicativo.

![fluxo do dispositivo complementar](images/companion-device-4.png)

Estes são os detalhes de cada um desses estados:

| Estado                         | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                               |
|----------------------------   |-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------    |
| WaitingForUserConfirmation    | Esse evento de notificação de mudança de estado é disparado quando a tela de bloqueio é ativada (por exemplo, o usuário pressionou Windows + L). Convém não solicitar mensagens de erro relacionadas a dificuldades de encontrar um dispositivo nesse estado. Em geral, convém apenas mostrar apenas mensagens quando o sinal de intenção está disponível. O aplicativo de dispositivo complementar do Windows Hello deverá fazer a primeira chamada de API para autenticação nesse estado se o dispositivo complementar coletar um sinal de intenção (por exemplo, tocar no leitor de NFC, pressionar um botão no dispositivo complementar ou realizar um gesto específico, como bater palmas) e se a tarefa em segundo plano do aplicativo de dispositivo complementar receber uma indicação do dispositivo complementar do Windows Hello de que esse sinal de intenção foi detectado. Caso contrário, se o aplicativo de dispositivo complementar do Windows Hello depender do computador para iniciar o fluxo de autenticação (fazendo com que o usuário passe o dedo para cima na tela de desbloqueio ou pressione a barra de espaço), ele precisará aguardar o próximo estado (CollectingCredential).     |
| CollectingCredential          | Esse evento de notificação de mudança de estado é disparado quando o usuário abre a tampa do notebook, pressiona qualquer tecla no teclado ou passe o dedo para cima na tela de desbloqueio. Se o dispositivo complementar do Windows Hello depender das ações acima para iniciar a coleta do sinal de intenção, o aplicativo de dispositivo complementar do Windows Hello deverá iniciar a sua coleta (por exemplo, por meio de um pop-up no dispositivo complementar perguntando se o usuário deseja desbloquear o computador). Isso seria um bom momento para fornecer casos de erro se o aplicativo de dispositivo complementar do Windows Hello precisar que o usuário forneça um sinal de presença no dispositivo complementar (como digitar um PIN nesse dispositivo).                                                                                                                                                                                                                                                                                                                                            |
| SuspendingAuthentication      | Quando o aplicativo de dispositivo complementar do Windows Hello recebe esse estado, significa que o Serviço de Autenticação Complementar parou de aceitar solicitações de autenticação.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
| CredentialCollected           | Significa que outro aplicativo de dispositivo complementar do Windows Hello chamou a segunda API e que o Serviço de Autenticação Complementar está verificando o que foi enviado. Nesse ponto, o Serviço de Autenticação Complementar não está aceitando outras solicitações de autenticação, a menos que aquela atualmente enviada não passe na verificação. O aplicativo de dispositivo complementar do Windows Hello deve permanecer em sintonia até que o próximo estado seja atingido.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                   |
| CredentialAuthenticated       | Isso significa que a credencial enviada funcionou. O credentialAuthenticated tem uma ID de dispositivo do dispositivo complementar do Windows Hello que obteve êxito. O aplicativo de dispositivo complementar do Windows Hello deve verificar para ter certeza de que o seu dispositivo associado foi o vencedor. Se esse não for o caso, o aplicativo de dispositivo complementar do Windows Hello deverá evitar a exibição de qualquer fluxo pós-autenticação (como mensagem de êxito no dispositivo complementar ou talvez uma vibração nesse dispositivo). Observe que, se a credencial enviada não tiver funcionado, o estado mudará para CollectingCredential.                                                                                                                                                                                                                                                                                                                                                                                       |
| StoppingAuthentication        | A autenticação foi bem-sucedida, e o usuário visualizou a área de trabalho. Tempo para encerrar sua tarefa em segundo plano. Antes de sair da tarefa em segundo plano, cancele explicitamente o registro do manipulador StageEvent. Isso ajudará a tarefa em segundo plano a sair rapidamente.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       |



Os aplicativos de dispositivo complementar do Windows Hello só devem chamar as duas APIs de autenticação nos dois primeiros estados. Aplicativos de dispositivo complementar do Windows Hello devem verificar em que cenário esse evento está sendo disparado. Há duas possibilidades: desbloqueio ou pós-desbloqueio. Atualmente, apenas há suporte para desbloqueio. Em versões futuras, é possível que haja suporte para cenários pós-desbloqueio. A enumeração SecondaryAuthenticationFactorAuthenticationScenario captura estas duas opções:

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
                    SecondaryAuthenticationFactorDeviceFindScope.AllUsers);

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

### <a name="register-a-background-task"></a>Registrar uma tarefa em segundo plano

Quando o aplicativo de dispositivo complementar do Windows Hello registra o primeiro dispositivo complementar, ele também deve registrar seu componente de tarefa em segundo plano que transmitirá informações de autenticação entre o dispositivo e o serviço de autenticação do dispositivo complementar.

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

### <a name="errors-and-messages"></a>Erros e mensagens

A estrutura de dispositivo complementar do Windows Hello é responsável por fornecer feedback para o usuário sobre o êxito ou a falha do processo de entrada. A estrutura de dispositivo complementar do Windows Hello fornecerá um estoque de texto e mensagens de erro (traduzidos) para escolha pelo aplicativo de dispositivo complementar do Windows Hello. Esses elementos serão exibidos na interface de usuário de logon.

![erro de dispositivo complementar](images/companion-device-5.png)

Aplicativos de dispositivo complementar do Windows Hello podem usar ShowNotificationMessageAsync para mostrar mensagens ao usuário como parte da interface de usuário de logon. Chame essa API quando um sinal de intenção estiver disponível. Observe que esse sinal de intenção sempre deve ser coletado no lado do dispositivo complementar do Windows Hello.

Existem dois tipos de mensagens: orientações e erros.

Mensagens de orientação foram projetadas para mostrar ao usuário como iniciar o processo de desbloqueio. Essas mensagens são apenas mostradas ao usuário uma vez na tela de bloqueio, após o primeiro registro do dispositivo. Essas mensagens continuam a ser mostradas na tela de bloqueio.

As mensagens de erro são sempre mostradas e serão exibidas após fornecer um sinal de intenção. Considerando que o sinal de intenção deve ser coletado antes da exibição de mensagens ao usuário e que o usuário fornecerá essa intenção somente usando um desses dispositivos complementares do Windows Hello, não deve haver uma situação em que vários dispositivos complementares do Windows Hello competem para mostrar mensagens de erro. Como resultado, a estrutura de dispositivo complementar do Windows Hello não mantém filas. Quando um chamador solicitar uma mensagem de erro, ela será mostrada por 5 segundos, e todas as outras solicitações para mostrar mensagens de erro nesses 5 segundos serão ignoradas. Após os 5 segundos, surgirá a oportunidade para outro chamador mostrar uma mensagem de erro. Proibimos que qualquer chamador obstrua o canal de erros.

Mensagens de erro e orientações estão estruturadas da seguinte maneira. O nome do dispositivo é um parâmetro transmitido pelo aplicativo de dispositivo complementar como parte de ShowNotificationMessageAsync.

**Diretrizes**

- "Passe o dedo para cima ou pressione a barra de espaço para entrar com o *nome do dispositivo*"
- "Configurando o dispositivo complementar. Aguarde ou use outra opção de entrada".
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

### <a name="enumerating-registered-devices"></a>Enumerando dispositivos registrados

O aplicativo de dispositivo complementar do Windows Hello pode enumerar a lista de dispositivos complementares registrados por meio da chamada FindAllRegisteredDeviceInfoAsync. Essa API dá suporte a dois tipos de consulta definidos por meio da enumeração SecondaryAuthenticationFactorDeviceFindScope:

```C#
{
    User = 0,
    AllUsers,
}
```

O primeiro escopo retorna a lista de dispositivos para o usuário conectado. O segundo retorna a lista de todos os usuários nesse computador. O primeiro escopo deve ser usado na ocasião do cancelamento do registro para evitar o cancelamento do registro do dispositivo complementar do Windows Hello de outro usuário. O outro deve ser usado em vez de autenticação ou registro: momento do registro, essa enumeração pode evitar aplicativo tentar registrar o mesmo dispositivo complementar do Windows Hello duas vezes.

Observe que, mesmo que o aplicativo não realize essa verificação, o computador rejeitará o registro do mesmo dispositivo complementar do Windows Hello mais de uma vez. No momento da autenticação, usar o escopo AllUsers ajuda a aplicativo de dispositivo complementar do Windows Hello a oferecer suporte ao fluxo de troca de usuários: fazer logon do usuário A quando o usuário B está conectado (isso requer que ambos os usuários tenham instalado o aplicativo de dispositivo complementar do Windows Hello e que o usuário A tenha registrado seus dispositivos complementares no computador que, por sua vez, deve estar na tela de bloqueio (ou na tela de logon)).

## <a name="security-requirements"></a>Requisitos de segurança

O serviço de autenticação complementar fornece as seguintes proteções de segurança.

- Malwares em um dispositivo da área de trabalho do Windows 10 em execução como um usuário de privilégios médios ou um contêiner de aplicativo não podem usar o dispositivo complementar do Windows Hello para acessar chaves de credenciais do usuário (armazenadas como parte do do Windows Hello) no computador silenciosamente.
- Um usuário mal-intencionado em um dispositivo de área de trabalho do Windows 10 não pode usar o dispositivo complementar do Windows Hello que pertence a outro usuário nesse dispositivo de área de trabalho do Windows 10 para obter acesso silencioso às chaves de credenciais de usuário dele (no mesmo dispositivo de área de trabalho do Windows 10).
- Malwares no dispositivo complementar do Windows Hello não podem obter acesso silencioso a chaves de credenciais de usuário no dispositivo de área de trabalho do Windows 10, incluindo o aproveitamento da funcionalidade ou do código desenvolvido especificamente para a estrutura de dispositivo complementar do Windows Hello.
- Um usuário mal-intencionado não pode desbloquear dispositivos de área de trabalho do Windows 10 capturando o tráfego entre o dispositivo complementar do Windows Hello e o dispositivo de área de trabalho do Windows 10 e reproduzindo-o posteriormente. O uso de nonce, authkey e HMAC em nosso protocolo garante proteção contra ataques de repetição.
- Malwares ou usuários mal-intencionados em um computador invasor não podem usar o dispositivo complementar do Windows Hello para obter acesso ao computador do usuário honesto. Isso é feito através de uma autenticação mútua entre o Serviço de Autenticação Complementar e o dispositivo complementar do Windows Hello por meio do uso de authkey e HMAC em nosso protocolo.

O segredo para se obter as proteções de segurança enumeradas acima é proteger as chaves HMAC contra acesso não autorizado e também verificar a presença do usuário. Mais especificamente, os seguintes requisitos devem ser atendidos:

- Fornecer proteção contra clonagem do dispositivo complementar do Windows Hello
- Fornecer proteção contra interceptações ao enviar chaves HMAC no momento do registro para o computador
- Garantir que o sinal de presença do usuário esteja disponível
