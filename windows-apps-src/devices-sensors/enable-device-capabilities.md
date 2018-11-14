---
author: muhsinking
ms.assetid: 949D1CE0-DD7D-420E-904D-758FADEBE85A
title: Habilitar os recursos do dispositivo
description: Este tutorial descreve como declarar recursos do dispositivo no Microsoft Visual Studio. Isso permite que o aplicativo use câmeras, microfones, sensores de localização e outros dispositivos.
ms.author: mukin
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a7250c41795373b089f7a4c76b603c169b1e4dc3
ms.sourcegitcommit: f2c9a050a9137a473f28b613968d5782866142c6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6250701"
---
# <a name="enable-device-capabilities"></a>Habilitar os recursos do dispositivo



Este tutorial descreve como declarar recursos do dispositivo no Microsoft Visual Studio. Isso permite que o aplicativo use câmeras, microfones, sensores de localização e outros dispositivos.

## <a name="specify-the-device-capabilities-your-app-will-use"></a>Especificar as funcionalidades do dispositivo que o aplicativo vai usar


Os aplicativos do Windows pedem que você especifique no manifesto do pacote do aplicativo quando vai usar determinados tipos de dispositivos. No Visual Studio, você pode declarar a maioria das funcionalidades usando o [Designer de Manifesto](https://msdn.microsoft.com/library/windows/apps/xaml/br230259.aspx) ou pode adicioná-las manualmente conforme descrito em [Como especificar funcionalidades do dispositivo no manifesto do pacote (manualmente)](https://msdn.microsoft.com/library/windows/apps/Dn263092). Este tutorial pressupõe que você esteja usando o Designer de Manifesto.

**Observação**  alguns tipos de dispositivos, como impressoras, scanners e sensores, não precisam ser declaradas no manifesto do pacote do aplicativo.

-   No Gerenciador de Soluções do Visual Studio, clique duas vezes no arquivo de manifesto do pacote **Package.appxmanifest**.
-   Abra a guia **Recursos**.
-   Selecione as funcionalidades do dispositivo que o seu aplicativo usa. Se a funcionalidade que você busca não aparecer no Designer de Manifesto, adicione-a manualmente. Para obter mais informações, consulte [Como especificar funcionalidades do dispositivo em um manifesto do pacote](https://msdn.microsoft.com/library/windows/apps/Dn263092).

| Funcionalidade do Dispositivo | Designer de Manifesto | Descrição |
|-------------------|-------------------|-------------|    
| AllJoyn | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Permite a descoberta e a interação entre aplicativos e dispositivos habilitados para AllJoyn em uma rede. Os aplicativos que acessam APIs no namespace [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) devem usar essa funcionalidade. |
| Mensagens de Chat Bloqueadas | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Permite que os aplicativos leiam mensagens SMS e MMS que foram bloqueadas pelo aplicativo Filtro de Spam. |
| Acesso a Mensagens de Chat | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Permite que os aplicativos leiam e excluam mensagens de texto. Permite que os aplicativos armazenem mensagens de chat no armazenamento de dados do sistema. |
| Geração de Código | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Permite que aplicativos gerem códigos dinamicamente. |
| Autenticação de Empresa | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Essa funcionalidade está sujeita à política da Microsoft Store. Ela oferece a funcionalidade de conexão com recursos da intranet da empresa que exijam credenciais de domínio. Essa funcionalidade não costuma ser necessária para a maioria dos aplicativos. | 
| Internet (Cliente) | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso de saída à Internet e às redes em locais públicos, como aeroportos e cafeterias. Por exemplo, redes da Intranet em que o usuário designou a rede como pública. A maioria dos aplicativos que exigem acesso à Internet deve usar essa funcionalidade. |
| Internet (Cliente e Servidor) | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso de entrada e saída para a Internet e as redes em locais públicos, como aeroportos e cafeterias. Essa funcionalidade é um superconjunto de **Internet (Cliente)**. **Internet (Cliente)** não precisa estar habilitado caso essa funcionalidade também esteja habilitada. O acesso de entrada a portas críticas está sempre bloqueado. |
| Localização| ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso à localização atual. Ela é obtida de um hardware dedicado, como um sensor de GPS no computador, ou de informações disponíveis na rede. | 
| Microfone | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso a um feed de áudio do microfone. Dessa forma, o aplicativo pode gravar usando microfones conectados. | 
| Biblioteca de Músicas | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece a funcionalidade para adicionar, alterar ou excluir arquivos na **Biblioteca de Músicas** para o computador local e os computadores do **Grupo Doméstico**. | 
| Objetos 3D | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso programático a **Objetos 3D**do usuário, permitindo que o aplicativo enumere e acesse todos os arquivos da biblioteca sem interação do usuário. Esta funcionalidade costuma ser usada em aplicativos e jogos 3D que precisam acessar toda a biblioteca de **Objetos 3D**. | 
| Telefonema | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Permite que aplicativos acessem todas as linhas telefônicas no dispositivo e executem as seguintes funções: fazer uma chamada no telefone e mostrar o discador do sistema sem avisar o usuário; acessar os metadados relacionados à linha; acessar os gatilhos relacionados à linha. Permite que o aplicativo do filtro de spam selecionado pelo usuário defina e verifique a lista de bloqueios, além das informações de origem das chamadas. | 
| Biblioteca de Imagens | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece a funcionalidade para adicionar, alterar ou excluir arquivos na **Biblioteca de Imagens** para o computador local e os computadores do **Grupo Doméstico**. | 
| Ponto de Serviço | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Fornece acesso aos periféricos de Ponto do serviço. Essa funcionalidade é necessária para acessar as APIs no namespace [**Windows.Devices.PointOfService**](https://docs.microsoft.com/uwp/api/Windows.Devices.PointOfService). | 
| Redes Privadas (Cliente e Servidor) | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso de entrada e saída a redes de Intranet que tenham um controlador de domínio autenticado ou que o usuário tenha designado como redes domésticas ou corporativas. O acesso de entrada a portas críticas está sempre bloqueado. | 
| Proximidade | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Fornece a capacidade de se conectar a dispositivos próximos ao computador por meio de NFC (transmissão de dados a curta distância). A proximidade a curta distância pode ser usada para enviar arquivos ou se comunicar usando um aplicativo no dispositivo próximo. | 
| Armazenamento Removível | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece a funcionalidade para adicionar, alterar ou excluir arquivos em dispositivos de armazenamento removível. O aplicativo só pode acessar os tipos de arquivo em armazenamento removível definidos no manifesto usando-se a declaração **Associações de Tipo de Arquivo**. O aplicativo não pode acessar o armazenamento removível em computadores do **Grupo Doméstico**. | 
| Certificados Compartilhados de Usuário | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Essa funcionalidade está sujeita à política da Microsoft Store. Ela oferece a funcionalidade para acessar certificados de software e hardware, como certificados de cartão inteligente, para validar a identidade do usuário. Quando APIs relacionadas são invocadas em tempo de execução, o usuário deve realizar uma ação (inserir cartão, selecionar certificado etc.). Essa funcionalidade não será necessária se o aplicativo incluir um certificado privado por meio de uma declaração **Certificates**. | 
| Informações de Conta de Usuário | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Dá a aplicativos a funcionalidade para acessar o nome e a imagem do usuário. Essa funcionalidade é necessária para acessar algumas APIs no namespace [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881). | 
| Biblioteca de Vídeos | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece a funcionalidade para adicionar, alterar ou excluir arquivos na **Biblioteca de Vídeos** para o computador local e os computadores do **Grupo Doméstico**. | 
| Chamada VOIP | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Permite que aplicativos acessem as APIs de chamada VOIP no namespace [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266). | 
| Webcam | ![Disponível no Designer de Manifesto](images/ap-tools.png) | Oferece acesso a uma câmera interna ou feed de vídeo da webcam conectada. Dessa forma, o aplicativo pode capturar instantâneos e filmes. | 
| USB | | Permite o acesso a dispositivos USB personalizados. Essa funcionalidade pede elementos filho. Esse recurso não tem suporte no Windows Phone. | 
| HID (Dispositivo de Interface Humana) | | Permite o acesso a HID (Dispositivos de Interface Humana). Essa funcionalidade pede elementos filho. Para obter mais informações, consulte [Como especificar funcionalidades do dispositivo em HID](https://msdn.microsoft.com/library/windows/apps/Dn263091). | 
| GATT de Bluetooth | | Permite o acesso a dispositivos LE Bluetooth através de uma coleção de serviços primários, serviços incluídos, características e descritores. Essa funcionalidade pede elementos filho. Para obter mais informações, consulte [Como especificar funcionalidades do dispositivo para Bluetooth](https://msdn.microsoft.com/library/windows/apps/Dn263090). | 
| Bluetooth RFCOMM |  | Permite o acesso a APIs compatíveis com o transporte BR/EDR (Basic Rate/Extended Data Rate) e também que o aplicativo da UWP acesse um dispositivo que implementa SPP (Serial Port Profile). Essa funcionalidade pede elementos filho. Para obter mais informações, consulte [Como especificar funcionalidades do dispositivo para Bluetooth](https://msdn.microsoft.com/library/windows/apps/Dn263090). |

## <a name="use-the-windows-runtime-api-for-communicating-with-your-device"></a>Use a API do Windows Runtime para comunicar com o seu dispositivo

A tabela a seguir conecta alguns dos recursos a APIs do Windows Runtime.

| Funcionalidade do Dispositivo        | API             | 
|--------------------------|-----------------|
| AllJoyn                  | [**Windows.Devices.AllJoyn**](https://msdn.microsoft.com/library/windows/apps/Dn894971) | 
| Mensagens de Chat Bloqueadas    | [**Windows.ApplicationModel.CommunicationBlocking**](https://msdn.microsoft.com/library/windows/apps/Dn974207) | 
| Localização                 | Veja [Visão geral de localização e mapas](https://msdn.microsoft.com/library/windows/apps/Mt219699) para saber mais. | 
| Telefonema               | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| Informações de Conta de Usuário | [**Windows.System.UserProfile**](https://msdn.microsoft.com/library/windows/apps/BR241881) | 
| Chamada VOIP             | [**Windows.ApplicationModel.Calls**](https://msdn.microsoft.com/library/windows/apps/Dn297266) | 
| USB                      | [**Windows.Devices.Usb**](https://msdn.microsoft.com/library/windows/apps/Dn278466) | 
| HID                      | [**Windows.Devices.HumanInterfaceDevice**](https://msdn.microsoft.com/library/windows/apps/Dn264174) | 
| GATT de Bluetooth           | [**Windows.Devices.Bluetooth.GenericAttributeProfile**](https://msdn.microsoft.com/library/windows/apps/Dn297685) | 
| Bluetooth RFCOMM         | [**Windows.Devices.Bluetooth.Rfcomm**](https://msdn.microsoft.com/library/windows/apps/Dn263529) | 
| Ponto de Serviço         | [**Windows.Devices.PointOfService**](https://msdn.microsoft.com/library/windows/apps/Dn298071) |

