---
title: Rede para jogos
description: Saiba como desenvolver e incorporar recursos de rede em seu jogo com o DirectX.
ms.assetid: 212eee15-045c-8ba1-e274-4532b2120c55
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, rede, directx
ms.localizationpriority: medium
ms.openlocfilehash: 2697564e703cfe290e33f204125346f3e8bad8ac
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/29/2018
ms.locfileid: "7991403"
---
# <a name="networking-for-games"></a>Rede para jogos



Saiba como desenvolver e incorporar recursos de rede em seu jogo com o DirectX.

## <a name="concepts-at-a-glance"></a>Conceitos básicos


Diversos recursos de rede podem ser usados em seu jogo em DirectX, seja um simples jogo individual ou jogos com grandes quantidades de jogadores. O uso mais simples da rede seria para armazenar nomes de usuários e pontuações de jogos em um servidor de rede central.

APIs de rede são necessárias em jogos com vários usuários que usam o modelo de infraestrutura (cliente-servidor ou internet ponto a ponto) e também em jogos ad hoc (ponto a ponto local). Para jogos com vários jogadores baseados em servidor, um servidor de jogos central costuma lidar com a maioria das operações dos jogos e o aplicativo de jogo cliente é usado para entradas, exibição de gráficos, reprodução de áudio e outros recursos. A velocidade e a latência das transferências da rede são questões importantes para uma experiência de jogo satisfatória.

Nos jogos ponto a ponto, o aplicativo de cada jogador lida com a entrada e os gráficos. Na maioria dos casos, os jogadores estão próximos uns dos outros, então a latência da rede deve ser menor, mas não deixa de ser uma questão importante. Como descobrir pontos e estabelecer uma conexão torna-se uma questão importante.

No caso de jogos com um único jogador, um servidor ou serviço da Web central costuma ser usado para armazenar nomes de usuários, pontuações de jogos e outros tipos de informação. Nesses jogos, a velocidade e a latência das transferências da rede são menos importantes pois não afetam diretamente o funcionamento do jogo.

As condições da rede podem mudar a qualquer momento, então qualquer jogo que usa APIs de rede precisa lidar com as exceções de rede que podem ocorrer. Para saber mais sobre como lidar com as exceções de rede, consulte [Noções básicas de rede](https://msdn.microsoft.com/library/windows/apps/mt280233).

Firewalls e proxies da Web são comuns e podem afetar a capacidade de usar os recursos da rede. Um jogo que usa rede precisa estar preparado para lidar apropriadamente com firewalls e proxies.

No caso de dispositivos móveis, é importante monitorar os recursos de rede disponíveis e ter um comportamento apropriado em redes limitadas onde os custos de roaming ou dados podem ser significativos.

O isolamento de rede faz parte do modelo de segurança do aplicativo usado pelo Windows. O Windows descobre ativamente os limites da rede e impõe as restrições de acesso para isolamento de rede. Os aplicativos devem declarar funcionalidades de isolamento de rede para definir o escopo do acesso à rede. Sem declarar essas funcionalidades, seu aplicativo não terá acesso aos recursos da rede. Para saber mais sobre como o Windows impõe o isolamento de rede para os aplicativos, consulte [Como configurar recursos de isolamento de rede](https://msdn.microsoft.com/library/windows/apps/hh770532).

## <a name="design-considerations"></a>Considerações de design


Diversas APIs de rede podem ser usadas em jogos DirectX. Portanto, é importante escolher a API correta. O Windows oferece suporte para uma série de APIs de rede que seu aplicativo pode usar para se comunicar com outros computadores e dispositivos pela Internet ou por redes privadas. Sua primeira etapa é calcular quais recursos de rede seu aplicativo precisa.

Estas são algumas das APIs de rede mais populares para jogos:

-   TCP e soquetes - Fornece uma conexão confiável. Use o TCP para operações de jogos que não precisem de segurança. O TCP permite que o servidor seja facilmente dimensionado, então ele costuma ser usado em jogos que usam o modelo de infraestrutura (cliente-servidor ou Internet ponto a ponto). O TCP também pode ser usado por jogos ad hoc (ponto a ponto local) via Wi-Fi Direct e Bluetooth. O TCP costuma ser usado para movimento de objetos do jogo, interação entre os personagens, bate-papo com texto e outras operações. A classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) fornece um soquete TCP que pode ser usado nos jogos da Microsoft Store. A classe **StreamSocket** é usada com as classes relacionadas [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) no namespace.
-   TCP e soquetes usando SSL - Fornece uma conexão confiável que impede interceptações. Use conexões TCP com SSL para operações de jogos que precisem de segurança. A criptografia e a sobrecarga de SSL aumentam o custo de latência e desempenho, então ela é usada apenas quando a segurança é necessária. O TCP com SSL costuma ser usado para logon, compra e troca de ativos, criação e gerenciamento de personagens do jogo. A classe [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) fornece um soquete TCP que dá suporte à SSL.
-   UDP e soquetes - Fornece transferências de rede não confiáveis com baixa sobrecarga. O UDP é usado para operações de jogos que exigem baixa latência e podem tolerar alguma perda no pacote. Também costuma ser usado para jogos de luta, tiros e habilidades, áudio de rede e bate-papo de voz. A classe [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319) fornece um soquete UDP que pode ser usado nos jogos da Microsoft Store. A classe **DatagramSocket** é usada com as classes relacionadas [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) no namespace.
-   HTTP Cliente - Fornece uma conexão confiável para servidores HTTP. O cenário de rede mais comum é para acessar um site da web para recuperar ou armazenar informações. Um exemplo simples seria um jogo que usa um site da web para armazenar informações de usuários e pontuações de jogos. Quando usado com SSL para segurança, um cliente HTTP pode ser usado para logon, compra e troca de ativos, criação e gerenciamento de personagens do jogo. A classe [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) fornece uma HTTP API moderna de cliente para uso em jogos da Microsoft Store. A classe **HttpClient** é usada com as classes relacionadas [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692) no namespace.

## <a name="handling-network-exceptions-in-your-directx-game"></a>Manipulando exceções de rede em seu jogo em DirectX


Uma exceção de rede em seu jogo em DirectX pode indicar um problema ou uma falha grave. As exceções podem ocorrer por vários motivos ao usar APIs de rede. Em geral, a exceção pode resultar de alterações na conectividade de rede ou de outros problemas de rede com o host ou servidor remoto.

Alguns motivos de exceções ao usar APIs de rede incluem:

-   Entradas do usuário contento erros durante a pesquisa por nomes de host ou URIs e não são válidas.
-   Falha nas resoluções de nomes durante a pesquisa por nome de host ou URi.
-   Perda ou mudança de conectividade de rede.
-   Falhas na conexão de rede ao usar soquetes ou as APIs de cliente HTTP.
-   Erros de servidor de rede ou ponto de extremidade remoto.
-   Diversos erros de rede.

As exceções por causa de erros de rede (por exemplo, perda ou mudança de conectividade, falhas na conexão e no servidor) podem acontecer a qualquer momento. Esses erros geram exceções. Se uma exceção não for manipulada pelo aplicativo, ela poderá fazer seu aplicativo inteiro ser encerrado pelo tempo de execução.

É necessário escrever um código para tratar exceções quando você chama a maioria dos métodos de rede assíncronos. Quando acontece alguma exceção, às vezes o método de rede pode ser repetido como uma forma de resolver o problema. Outras vezes, o aplicativo tem de se planejar para continuar sem conectividade de rede, usando os dados que já foram armazenados em cache.

Geralmente, os aplicativos UWP (Plataforma Universal do Windows) lançam uma única exceção. Seu manipulador de exceção recupera informações mais detalhadas sobre a causa da exceção para entender melhor a falha e tomar as medidas adequadas.

Quando ocorre uma exceção no jogo DirectX que é um aplicativo UWP, o valor de **HRESULT** da causa do erro é recuperado. O arquivo de inclusão *Winerror.h* contém uma lista grande de valores **HRESULT** possíveis, incluindo erros de rede.

As APIs de rede permitem diferentes métodos de recuperação das informações detalhadas sobre a causa da exceção.

-   Um método para recuperar o valor de **HRESULT** do erro que causa a exceção. A lista de possíveis valores **HRESULT** é grande e inespecífica. O valor de **HRESULT** pode ser recuperado usando qualquer uma das APIs de rede.
-   Um método auxiliar que converte o valor **HRESULT** em um valor de enumeração. A lista de possíveis valores de enumeração é específica e relativamente pequena. Um método de ajuda está disponível para as classes de soquete em [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960).

### <a name="exceptions-in-windowsnetworkingsockets"></a>Exceções em Windows.Networking.Sockets

O construtor da classe [**HostName**](https://msdn.microsoft.com/library/windows/apps/br207113) usado com soquetes pode gerar uma exceção se a cadeia de caracteres passada não for um nome de host válido (ou seja, contém caracteres que não são permitidos em um nome de host). Se um aplicativo obtiver entrada do usuário para o **HostName** para uma conexão no mesmo nível, o construtor deverá estar em um bloco try/catch. Se uma exceção for gerada, o aplicativo poderá notificar o usuário e solicitar um novo nome de host.

Adicionar código para validar uma cadeia para um nome de host do usuário

```cpp

    // Define some variables at the class level.
    Windows::Networking::HostName^ remoteHost;

    bool isHostnameFromUser = false;
    bool isHostnameValid = false;

    ///...

    // If the value of 'remoteHostname' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^hostString = remoteHostname;

    try 
    {
        remoteHost = ref new Windows::Networking:Host(hostString);
        isHostnameValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad hostname, please re-enter a valid hostname.";
        return;
    }

    isHostnameFromUser = true;


    // ... Continue with code to execute with a valid hostname.
```

O namespace [**Windows.Networking.Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960) possui métodos auxiliares e enumerações práticas para resolver erros durante o uso de soquetes. Eles são úteis para resolver exceções de rede específicas de uma outra forma em seu aplicativo.

Um erro encontrado na operação [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319), [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882) ou [**StreamSocketListener**](https://msdn.microsoft.com/library/windows/apps/br226906) resulta em uma exceção sendo gerada. A causa da exceção é um valor de erro representado como um valor **HRESULT**. O método [**SocketError.GetStatus**](https://msdn.microsoft.com/library/windows/apps/hh701462) é usado para converter um erro de rede de uma operação de soquete em um valor de enumeração [**SocketErrorStatus**](https://msdn.microsoft.com/library/windows/apps/hh701457). A maioria dos valores de enumeração **SocketErrorStatus** corresponde a um erro retornado pela operação nativa de soquetes do Windows. Um aplicativo pode filtrar por um valor específico de enumeração **SocketErrorStatus** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para erros de validação de parâmetro, um aplicativo também pode usar o **HRESULT** baseado na exceção para obter informações mais detalhadas sobre o erro causador da exceção. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror. h*. Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**.

Adicionar código para trabalhar com exceções ao tentar fazer uma conexão de soquete de fluxo

```cpp
using namespace Windows::Networking;
using namespace Windows::Networking::Sockets;

    
    // Define some more variables at the class level.

    bool isSocketConnected = false
    bool retrySocketConnect = false;

    // The number of times we have tried to connect the socket.
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid remoteHost and serviceName parameter.
    // The hostname can contain a name or an IP address.
    // The servicename can contain a string or a TCP port number.

    StreamSocket ^ socket = ref new StreamSocket();
    SocketErrorStatus errorStatus; 
    HResult hr;

    // Save the socket, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("clientSocket", socket);

    // Connect to the remote server. 
    create_task(socket->ConnectAsync(
            remoteHost,
            serviceName,
            SocketProtectionLevel::PlainSocket)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isSocketConnected = true;
            // Mark the socket as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
            errorStatus = SocketStatus::GetStatus(hr); 
            if (errorStatus != Unknown)
            {
                                                                switch (errorStatus) 
                   {
                    case HostNotFound:
                        // If the hostname is from the user, this may indicate a bad input.
                        // Set a flag to ask the user to re-enter the hostname.
                        isHostnameValid = false;
                        return;
                        break;
                    case ConnectionRefused:
                        // The server might be temporarily busy.
                        retrySocketConnect = true;
                        return;
                        break; 
                    case NetworkIsUnreachable: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case UnreachableHost: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    case NetworkIsDown: 
                        // This could be a connectivity issue.
                        retrySocketConnect = true;
                        break;
                    // Handle other errors. 
                    default: 
                        // The connection failed and no options are available.
                        // Try to use cached data if it is available. 
                        // You may want to tell the user that the connect failed.
                        break;
                }
                }
                else 
                {
                    // Received an Hresult that is not mapped to an enum.
                    // This could be a connectivity issue.
                    retrySocketConnect = true;
                }
            }
        });
    }

```

### <a name="exceptions-in-windowswebhttp"></a>Exceções em Windows.Web.Http

O construtor da classe [**Windows::Foundation::Uri**](https://msdn.microsoft.com/library/windows/apps/br225998) usado com [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) pode gerar uma exceção se a cadeia passada não for uma URI válida (ou seja, contém caracteres que não são permitidos em uma URI). No C++, não há nenhum método para tentar analisar uma cadeia de caracteres para um URI. Se um aplicativo receber entrada do usuário para o **Windows::Foundation::Uri**, o construtor deverá estar em um bloco try/catch. Se uma exceção for gerada, o aplicativo poderá notificar o usuário e solicitar uma nova URI.

Seu aplicativo também deve verificar se o esquema na URI é HTTP ou HTTPS visto que esses são os únicos esquemas aos quais o [**Windows::Web::Http::HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) dá suporte.

Adicionar código para validar uma cadeia para uma URI do usuário

```cpp

    // Define some variables at the class level.
    Windows::Foundation::Uri^ resourceUri;

    bool isUriFromUser = false;
    bool isUriValid = false;

    ///...

    // If the value of 'inputUri' is set by the user in a control as input 
    // and is therefore untrusted input and could contain errors. 
    // If we can't create a valid hostname, we notify the user in statusText 
    // about the incorrect input.

    String ^uriString = inputUri;

    try 
    {
        isUriValid = false;
        resourceUri = ref new Windows::Foundation:Uri(uriString);

        if (resourceUri->SchemeName != "http" && resourceUri->SchemeName != "https")
        {
            statusText->Text = "Only 'http' and 'https' schemes supported. Please re-enter URI";
            return;
        }
        isUriValid = true;
    }
    catch (InvalidArgumentException ^ex)
    {
        statusText->Text = "You entered a bad URI, please re-enter Uri to continue.";
        return;
    }

    isUriFromUser = true;


    // ... Continue with code to execute with a valid URI.
```

O namespace [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/windows.web.http.aspx) não tem uma função de conveniência. Portanto, o aplicativo que usa [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) e outras classes nesse namespace precisa usar o valor **HRESULT**.

Nos aplicativos em C++, [**Platform::Exception**](https://msdn.microsoft.com/library/windows/apps/hh755825.aspx) representa um erro durante a execução do aplicativo quando há uma exceção. A propriedade [**Platform::Exception::HResult**](https://msdn.microsoft.com/library/windows/apps/hh763371.aspx) retorna o **HRESULT** atribuído à exceção específica. A propriedade [**Platform::Exception::Message**](https://msdn.microsoft.com/library/windows/apps/hh763375.aspx) retorna a cadeia de caracteres fornecida pelo sistema que está associada ao valor de **HRESULT**. Os valores possíveis de **HRESULT** estão listados no arquivo de cabeçalho *Winerror.h*. Um aplicativo pode filtrar por valores específicos de **HRESULT** para modificar o comportamento do aplicativo, dependendo da causa da exceção.

Para a maioria dos erros de validação de parâmetro, o **HRESULT** retornado é **E\_INVALIDARG**. Para algumas chamadas de método ilícitas, o **HRESULT** retornado é **E\_ILLEGAL\_METHOD\_CALL**.

Adicionar código para trabalhar com exceções ao tentar usar [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639) para se conectar a um servidor HTTP

```cpp
using namespace Windows::Foundation;
using namespace Windows::Web::Http;
    
    // Define some more variables at the class level.

    bool isHttpClientConnected = false
    bool retryHttpClient = false;

    // The number of times we have tried to connect the socket
    unsigned int retryConnectCount = 0;

    // The maximum number of times to retry a connect operation.
    unsigned int maxRetryConnectCount = 5; 
    ///...

    // We pass in a valid resourceUri parameter.
    // The URI must contain a scheme and a name or an IP address.

    HttpClient ^ httpClient = ref new HttpClient();
    HResult hr;

    // Save the httpClient, so any subsequent steps can use it.
    CoreApplication::Properties->Insert("httpClient", httpClient);

    // Send a GET request to the HTTP server. 
    create_task(httpClient->GetAsync(resourceUri)).then([this] (task<void> previousTask)
    {
        try
        {
            // Try getting all exceptions from the continuation chain above this point.
            previousTask.get();

            isHttpClientConnected = true;
            // Mark the HttClient as connected. We do not really care about the value of the property, but the mere 
            // existence of  it means that we are connected.
            CoreApplication::Properties->Insert("connected", nullptr);
        }
        catch (Exception^ ex)
        {
            hr = ex.HResult;
                                                switch (errorStatus) 
               {
                case WININET_E_NAME_NOT_RESOLVED:
                    // If the Uri is from the user, this may indicate a bad input.
                    // Set a flag to ask user to re-enter the Uri.
                    isUriValid = false;
                    return;
                    break;
                case WININET_E_CANNOT_CONNECT:
                    // The server might be temporarily busy.
                    retryHttpClientConnect = true;
                    return;
                    break; 
                case WININET_E_CONNECTION_ABORTED: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case WININET_E_CONNECTION_RESET: 
                    // This could be a connectivity issue.
                    retryHttpClientConnect = true;
                    break;
                case INET_E_RESOURCE_NOT_FOUND: 
                    // The server cannot locate the resource specified in the uri.
                    // If the Uri is from user, this may indicate a bad input.
                    // Set a flag to ask the user to re-enter the Uri
                    isUriValid = false;
                    return;
                    break;
                // Handle other errors. 
                default: 
                    // The connection failed and no options are available.
                    // Try to use cached data if it is available. 
                    // You may want to tell the user that the connect failed.
                    break;
            }
            else 
            {
                // Received an Hresult that is not mapped to an enum.
                // This could be a connectivity issue.
                retrySocketConnect = true;
            }
        }
    });
    

```

## <a name="related-topics"></a>Tópicos relacionados


**Outros recursos**

* [Conectando-se com um soquete de datagrama](https://msdn.microsoft.com/library/windows/apps/xaml/jj635238)
* [Conectando-se a um recurso de rede com um soquete de fluxo](https://msdn.microsoft.com/library/windows/apps/xaml/jj150599)
* [Conectando-se aos serviços de rede](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
* [Conectando-se a serviços Web](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
* [Noções básicas de rede](https://msdn.microsoft.com/library/windows/apps/mt280233)
* [Como configurar recursos de isolamento de rede](https://msdn.microsoft.com/library/windows/apps/hh770532)
* [Como habilitar loopback e depurar o isolamento de rede](https://msdn.microsoft.com/library/windows/apps/hh780593)

**Referência**

* [**DatagramSocket**](https://msdn.microsoft.com/library/windows/apps/br241319)
* [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639)
* [**StreamSocket**](https://msdn.microsoft.com/library/windows/apps/br226882)
* [**Windows::Web::Http**](https://msdn.microsoft.com/library/windows/apps/dn279692)
* [**Windows::Networking::Sockets**](https://msdn.microsoft.com/library/windows/apps/br226960)

**Exemplos**

* [Exemplo de DatagramSocket](http://go.microsoft.com/fwlink/p/?LinkID=243037)
* [Amostra de HttpClient]( http://go.microsoft.com/fwlink/p/?linkid=242550)
* [Exemplo de proximidade](http://go.microsoft.com/fwlink/p/?linkid=245082)
* [Exemplo do StreamSocket](http://go.microsoft.com/fwlink/p/?linkid=243037)
