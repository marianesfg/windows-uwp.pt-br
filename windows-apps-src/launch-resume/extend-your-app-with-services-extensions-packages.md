---
author: TylerMSFT
title: Estender seu aplicativo com serviços de aplicativo, extensões e pacotes
description: Saiba como criar uma tarefa em segundo plano que é executada quando seu aplicativo da loja da UWP (Plataforma Universal do Windows) é atualizado.
ms.author: twhitney
ms.date: 05/21/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, estender, dividir, serviço de aplicativo, pacote, extensão
ms.localizationpriority: medium
ms.openlocfilehash: 2721f9d8f768cabb0e07c0cd2cfcfcbf9255cd70
ms.sourcegitcommit: 6618517dc0a4e4100af06e6d27fac133d317e545
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/28/2018
ms.locfileid: "1689612"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Estender seu aplicativo com serviços de aplicativo, extensões e pacotes

Há diferentes tecnologias no Windows 10 que ajudam você a estender e separar os componentes do aplicativo. Esta tabela deve ajudar você a determinar qual tecnologia deve ser usada para seu cenário. Ela é seguida por uma breve descrição dos cenários e tecnologias.


| Cenário                           | Pacote de recursos | Pacote opcional | Extensão de aplicativo    | Serviço de aplicativo      | Instalação de streaming |
|------------------------------------|:----------------:|:----------------:|:----------------:|:----------------:|:-----------------:|
| Plug-ins de código de terceiros            |                  |                  | :heavy_check_mark: |                  |                   |
| Plug-ins de código em processamento              |                  | :heavy_check_mark: |                  |                  |                   |
| Ativos de experiência do usuário (cadeias de caracteres/imagens)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                  | :heavy_check_mark: |
| Conteúdo sob demanda <br/> (por exemplo, Níveis adicionais) |    | :heavy_check_mark: | :heavy_check_mark: |                  | :heavy_check_mark: |
| Licenciamento e aquisição separados |                  | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                   |
| Aquisição no aplicativo                 |                  | :heavy_check_mark: | :heavy_check_mark: |                  |                   |
| Otimizar o tempo de instalação              | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                  | :heavy_check_mark: |

## <a name="scenario-descriptions-rows-in-the-table"></a>Descrições de cenário (linhas na tabela)

**Plug-ins de terceiros**  

Código que você pode baixar da loja e executar no seu aplicativo. Por exemplo, extensões do navegador Microsoft Edge.

**Plug-ins de código em processamento**  

Código que é executado em processo com seu aplicativo. Há suporte somente para C++. Também pode incluir o conteúdo. Como o código é executado no processo, é considerado um nível mais alto de confiança. Você pode optar por não abrir esse tipo de extensão para terceiros.

**Ativos de experiência do usuário (cadeia de caracteres/imagens)**  

Ativos de interface do usuário, como cadeias de caracteres localizadas, imagens e qualquer outro conteúdo de interface do usuário que você deseja incluir com base na localidade ou qualquer outro motivo.

**Conteúdo sob demanda**  

Conteúdo que você deseja baixar mais tarde. Por exemplo, compras no aplicativo que permitem a você baixar novos níveis, capas ou funcionalidades.

**Licenciamento e aquisição separados**  

A capacidade de licenciar e adquirir conteúdo independentemente do aplicativo.

**Aquisição no aplicativo**  

Indica se há suporte programático para adquirir o conteúdo no aplicativo.

**Otimizar o tempo de instalação**

Oferece a funcionalidade para reduzir o tempo necessário para adquirir o aplicativo da loja e iniciar a execução.

## <a name="technology-descriptions-columns-in-the-table"></a>Descrições de tecnologia (colunas na tabela)

**Pacote de recursos**

Pacotes de recursos são apenas pacotes de ativos que permitem ao aplicativo se adaptar a vários tamanhos de tela e idiomas do sistema. O pacote de recursos visa o idioma do usuário, a escala do sistema e os recursos do DirectX, permitindo que o aplicativo seja adaptado para diversos cenários de usuário. Embora um pacote do aplicativo possa conter vários recursos, o sistema operacional baixa apenas os recursos relevantes por dispositivo do usuário, economizando espaço em disco e largura de banda.

**Pacote opcional**

Os pacotes opcionais são usados para complementar ou estender a funcionalidade original do pacote do aplicativo. É possível publicar um aplicativo, seguido da publicação de pacotes opcionais em um momento posterior, ou publicar pacotes de aplicativo e opcionais simultaneamente. Ao estender seu aplicativo por meio de um pacote opcional, você tem as vantagens de distribuir e monetizar conteúdo como um pacote de aplicativo separado. Os pacotes opcionais geralmente devem ser desenvolvidos pelo desenvolvedor do aplicativo original, pois são executados com a identidade do aplicativo principal (diferente das extensões de aplicativo). Dependendo de como você define o pacote opcional, você pode carregar código, ativos ou código e ativos do pacote opcional para o aplicativo principal. Se você estiver procurando melhorar seu aplicativo com conteúdo que pode ser monetizado, licenciado, e pacotes distribuídos separadamente, então os pacotes opcionais podem ser a escolha certa para você. Para obter detalhes de implementação, consulte [Criação de pacotes opcionais e conjunto relacionado](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Extensão de aplicativo**

[Extensões de aplicativos](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permitem que seu aplicativo UWP hospede conteúdo fornecido por outros aplicativos UWP. Descubra, enumere e acesse conteúdo somente leitura desses aplicativos.

Se um aplicativo oferece suporte a extensões, qualquer desenvolvedor pode enviar uma extensão para o aplicativo. Assim, o aplicativo host precisa ser robusto quando carrega uma extensão com a qual ainda não foi testada previamente. As extensões devem ser consideradas não confiáveis.

Os aplicativos não podem carregar o código a partir das extensões. Se você precisar de execução de código, considere os Serviços de aplicativo.

**Serviço de Aplicativo**

Os serviços de aplicativo do Windows permitem a comunicação entre aplicativos, permitindo que o aplicativo UWP forneça serviços a outros aplicativos universais do Windows. Os serviços de aplicativo permitem que você crie serviços sem interface do usuário que os aplicativos podem chamar no mesmo dispositivo e, a partir do Windows 10, versão 1607, nos dispositivos remotos. Consulte [Criar e consumir um serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obter mais detalhes.

Os serviços de aplicativo são aplicativos UWP que fornecem serviços a outros aplicativos UWP. Eles são análogos aos serviços da Web em um dispositivo. Um serviço de aplicativo é executado como uma tarefa em segundo plano no aplicativo host e pode fornecer seu serviço a outros aplicativos. Por exemplo, um serviço de aplicativo pode fornecer um serviço de scanner de código de barras que outros aplicativos podem usar. Ou talvez um conjunto de aplicativos Enterprise tenha um serviço de verificação ortográfica que está disponível para os outros aplicativos no pacote.

**Instalação de streaming de aplicativo UWP**

A Instalação de streaming é uma maneira de otimizar o modo como seu aplicativo é disponibilizado aos usuários. Em vez de aguardar até que o aplicativo inteiro seja baixado antes que você poder usá-lo, os usuários podem interagir com o aplicativo assim que uma parte necessária for baixada. Cabe a você, como um desenvolvedor, segmentar seu aplicativo em uma seção obrigatória para ativação básica e iniciar e conteúdo adicional para o restante do aplicativo. Consulte [Instalar streaming de aplicativo UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install) para obter mais informações e detalhes sobre a implementação.

## <a name="see-also"></a>Consulte também

[Criar e consumir um serviço de app](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Criação de pacotes opcionais e conjunto relacionado](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Namespace Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
[Instalação de streaming de aplicativo UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Namespace Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)    
