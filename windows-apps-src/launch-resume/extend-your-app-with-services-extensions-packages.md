---
author: TylerMSFT
title: Estender seu aplicativo com serviços de aplicativo, extensões e pacotes
description: Saiba como criar uma tarefa em segundo plano que é executada quando seu aplicativo da loja da UWP (Plataforma Universal do Windows) é atualizado.
ms.author: twhitney
ms.date: 05/7/2018
ms.topic: article
keywords: windows 10, uwp, estender, dividir, serviço de aplicativo, pacote, extensão
ms.localizationpriority: medium
ms.openlocfilehash: 624d52ff96fb2537afa3affb2d842aa29e1e6667
ms.sourcegitcommit: e38b334edb82bf2b1474ba686990f4299b8f59c7
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6841878"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Estender seu aplicativo com serviços de aplicativo, extensões e pacotes

Há diferentes tecnologias no Windows 10 que ajudam você a estender e separar os componentes do aplicativo. Esta tabela deve ajudar você a determinar qual tecnologia deve ser usada para seu cenário. Ela é seguida por uma breve descrição dos cenários e tecnologias.

| Cenário                           | Pacote de recursos   | Pacote de ativos      | Pacote opcional   | Pacote simples        | Extensão de aplicativo      | Serviço de aplicativo        | Instalação de streaming  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Plug-ins de código de terceiros            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| Plug-ins de código em processamento              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Ativos de experiência do usuário (cadeias de caracteres/imagens)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Conteúdo sob demanda <br/> (por exemplo, Níveis adicionais) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Licenciamento e aquisição separados |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| Aquisição no aplicativo                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Otimizar o tempo de instalação              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Reduzir o volume de disco              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Otimizar o empacotamento                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Reduzir o tempo de publicação             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Descrições de cenário (as linhas na tabela acima)

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

**Reduzir o volume de disco** reduz o tamanho de um aplicativo, incluindo somente os aplicativos ou recursos necessários.

**Otimizar o empacotamento** Otimiza o processo de empacotamento do aplicativo para aplicativos complexos ou de grande escala.

**Reduzir o tempo de publicação** Minimiza o tempo necessário para publicar seu aplicativo na Store, no compartilhamento local ou no servidor Web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descrições de tecnologia (as colunas na tabela acima)

**Pacote de recursos**

Pacotes de recursos são apenas pacotes de ativos que permitem ao aplicativo se adaptar a vários tamanhos de tela e idiomas do sistema. O pacote de recursos visa o idioma do usuário, a escala do sistema e os recursos do DirectX, permitindo que o aplicativo seja adaptado para diversos cenários de usuário. Embora um pacote do aplicativo possa conter vários recursos, o sistema operacional baixa apenas os recursos relevantes por dispositivo do usuário, economizando espaço em disco e largura de banda.

**Pacote de ativos** Os pacotes de ativos são uma fonte comum e centralizada de arquivos executáveis ou não-executáveis usados pelo seu aplicativo. Em geral, são arquivos sem processamento ou linguagem específica. Por exemplo, isso pode incluir uma coleção de fotos no pacote de um ativo e vídeos em outro pacote de ativo, que são usados pelo aplicativo. Por exemplo, isso pode incluir uma coleção de imagens no pacote de ativos e vídeos em outro pacote de ativos. Se o aplicativo oferece suporte a várias arquitetura e linguagens, esses ativos podem ser incluídos no pacote de arquitetura ou recursos, mas isso também significa que os ativos seriam duplicados muitas vezes em pacotes de arquitetura diferentes, ocupando espaço em disco. Se os pacotes de ativos forem usados, eles devem ser incluídos somente no pacote do aplicativos uma vez. Consulte [Introdução aos pacotes de ativos](../packaging/asset-packages.md) para saber mais.

**Pacote opcional**

Os pacotes opcionais são usados para complementar ou estender a funcionalidade original do pacote do aplicativo. É possível publicar um aplicativo, seguido da publicação de pacotes opcionais em um momento posterior, ou publicar pacotes de aplicativo e opcionais simultaneamente. Ao estender seu aplicativo por meio de um pacote opcional, você tem as vantagens de distribuir e monetizar conteúdo como um pacote de aplicativo separado. Os pacotes opcionais geralmente devem ser desenvolvidos pelo desenvolvedor do aplicativo original, pois são executados com a identidade do aplicativo principal (diferente das extensões de aplicativo). Dependendo de como você define o pacote opcional, você pode carregar código, ativos ou código e ativos do pacote opcional para o aplicativo principal. Se você estiver procurando melhorar seu aplicativo com conteúdo que pode ser monetizado, licenciado, e pacotes distribuídos separadamente, então os pacotes opcionais podem ser a escolha certa para você. Para obter detalhes de implementação, consulte [Criação de pacotes opcionais e conjunto relacionado](https://docs.microsoft.com/windows/uwp/packaging/optional-packages).

**Pacote simples**
[Pacotes simples do aplicativo](../packaging/flat-bundles.md) são semelhantes aos regulares, exceto que em vez de incluírem todos os pacotes de aplicativo na pasta, o pacote simples contém apenas *referências* aos pacotes de aplicativo. Por conter referências aos pacotes de aplicativo em vez dos próprios arquivos, um pacote simples reduz o tempo necessário para empacotar e baixar um aplicativo.

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
[Introdução aos pacotes de ativo](../packaging/asset-packages.md)  
[Criação do pacote com o layout de empacotamento](../packaging/packaging-layout.md)  
[Criação de pacotes opcionais e conjunto relacionado](https://docs.microsoft.com/windows/uwp/packaging/optional-packages)  
[Desenvolvendo com os pacotes de ativo e dobramento de pacote](../packaging/package-folding.md)  
[Instalação de streaming de aplicativo UWP](https://docs.microsoft.com/windows/uwp/packaging/streaming-install)  
[Pacotes do lote simples de aplicativo](../packaging/flat-bundles.md)  
[Namespace Windows.ApplicationModel.AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Namespace Windows.ApplicationModel.Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  