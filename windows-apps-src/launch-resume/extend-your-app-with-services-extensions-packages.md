---
title: Estender seu aplicativo com serviços, extensões e pacotes
description: Descreve como criar uma tarefa em segundo plano que é executada quando seu aplicativo da loja Plataforma Universal do Windows (UWP) é atualizado.
ms.date: 05/07/2018
ms.topic: article
keywords: windows 10, uwp, estender, dividir, serviço de aplicativo, pacote, extensão
ms.localizationpriority: medium
ms.openlocfilehash: d9a98ef8e0ec53668277face05d83c08f6421cb7
ms.sourcegitcommit: c7e10793cbef55ace959ac8fc6ddd08e683602bd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/31/2019
ms.locfileid: "73329510"
---
# <a name="extend-your-app-with-services-extensions-and-packages"></a>Estender seu aplicativo com serviços, extensões e pacotes

Há muitas tecnologias no Windows 10 para estender e modelar seu aplicativo. Esta tabela deve ajudá-lo a determinar qual tecnologia você deve usar dependendo dos requisitos. Ela é seguida por uma breve descrição dos cenários e tecnologias.

| Cenário                           | Pacote de recursos   | Pacote de ativos      | Pacote opcional   | Pacote simples        | Extensão de aplicativo      | Serviço de aplicativo        | Instalação de streaming  |
|------------------------------------|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|:------------------:|
| Plug-ins de código de terceiros            |                    |                    |                    |                    | :heavy_check_mark: |                    |                    |
| Plug-ins de código em processamento              |                    |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Ativos de experiência do usuário (cadeias de caracteres/imagens)         | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Conteúdo sob demanda <br/> (por exemplo, níveis adicionais) |      |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Licenciamento e aquisição separados |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: | :heavy_check_mark: |                    |
| Aquisição no aplicativo                 |                    |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |
| Otimizar o tempo de instalação              | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |                    | :heavy_check_mark: |
| Reduzir o volume de disco              | :heavy_check_mark: |                    | :heavy_check_mark: |                    |                    |                    |                    |
| Otimizar o empacotamento                 |                    | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |
| Reduzir o tempo de publicação             | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: | :heavy_check_mark: |                    |                    |                    |

## <a name="scenario-descriptions-the-rows-in-the-table-above"></a>Descrições de cenário (as linhas na tabela acima)

**Plug-ins de terceiros**  

Código que você pode baixar da loja e executar no seu aplicativo. Por exemplo, extensões do navegador Microsoft Edge.

**Plug-ins de código em processo**  

Código que é executado em processo com seu aplicativo. Também pode incluir o conteúdo. Como o código é executado no processo, é considerado um nível mais alto de confiança. Você pode optar por não expor esse tipo de extensibilidade a terceiros.

**Ativos de UX (cadeias de caracteres/imagens)**  

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

**Otimizar empacotamento** Otimiza o processo de empacotamento de aplicativos para aplicativos de grande escala ou complexos.

**Reduzir o tempo de publicação** Minimiza o tempo necessário para publicar seu aplicativo na Store, no compartilhamento local ou no servidor Web.

## <a name="technology-descriptions-the-columns-in-the-table-above"></a>Descrições de tecnologia (as colunas na tabela acima)

**Pacote de recursos**

Pacotes de recursos são apenas pacotes de ativos que permitem ao aplicativo se adaptar a vários tamanhos de tela e idiomas do sistema. O pacote de recursos visa o idioma do usuário, a escala do sistema e os recursos do DirectX, permitindo que o aplicativo seja adaptado para diversos cenários de usuário. Embora um pacote do aplicativo possa conter vários recursos, o sistema operacional baixa apenas os recursos relevantes por dispositivo do usuário, economizando espaço em disco e largura de banda.

**Pacote de ativos** Os pacotes de ativos são uma fonte comum e centralizada de arquivos executáveis ou não executáveis para uso pelo seu aplicativo. Normalmente, esses arquivos não são do processador ou específicos do idioma. Por exemplo, isso pode incluir uma coleção de fotos no pacote de um ativo e vídeos em outro pacote de ativo, que são usados pelo aplicativo. Se seu aplicativo oferecer suporte a várias arquiteturas e vários idiomas, esses ativos poderão ser incluídos no pacote de arquitetura ou no pacote de recursos, mas isso também significa que os ativos seriam duplicados várias vezes em vários pacotes de arquitetura, levando espaço em disco. Se os pacotes de ativos forem usados, eles devem ser incluídos somente no pacote do aplicativos uma vez. Consulte [Introdução aos pacotes de ativos](/windows/msix/package/asset-packages) para saber mais.

**Pacote opcional**

Os pacotes opcionais são usados para complementar ou estender a funcionalidade original do pacote do aplicativo. É possível publicar um aplicativo, seguido da publicação de pacotes opcionais em um momento posterior, ou publicar pacotes de aplicativo e opcionais simultaneamente. Ao estender seu aplicativo por meio de um pacote opcional, você tem as vantagens de distribuir e monetizar conteúdo como um pacote de aplicativo separado. Os pacotes opcionais geralmente devem ser desenvolvidos pelo desenvolvedor do aplicativo original, pois são executados com a identidade do aplicativo principal (diferente das extensões de aplicativo). Dependendo de como você define o pacote opcional, você pode carregar código, ativos ou código e ativos do pacote opcional para o aplicativo principal. Se você precisar aprimorar seu aplicativo com conteúdo que possa ser monetizadas, licenciado e distribuído separadamente, os pacotes opcionais podem ser a escolha certa para você. Para obter detalhes de implementação, consulte [Criação de pacotes opcionais e conjunto relacionado](/windows/msix/package/optional-packages).

**Pacote simples**
[Pacotes simples do aplicativo](/windows/msix/package/flat-bundles.md) são semelhantes aos regulares, exceto que em vez de incluírem todos os pacotes de aplicativo na pasta, o pacote simples contém apenas *referências* aos pacotes de aplicativo. Por conter referências aos pacotes de aplicativo em vez dos próprios arquivos, um pacote simples reduz o tempo necessário para empacotar e baixar um aplicativo.

**Extensão do aplicativo**

[Extensões de aplicativos](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions) permitem que seu aplicativo UWP hospede conteúdo fornecido por outros aplicativos UWP. Descubra, enumere e acesse conteúdo somente leitura desses aplicativos.

Se um aplicativo oferece suporte a extensões, qualquer desenvolvedor pode enviar uma extensão para o aplicativo. Assim, o aplicativo host precisa ser robusto quando carrega uma extensão com a qual ainda não foi testada previamente. As extensões devem ser consideradas não confiáveis.

Os aplicativos não podem carregar o código a partir das extensões. Se você precisar de execução de código, considere os Serviços de aplicativo.

**Serviço de aplicativo**

Os serviços de aplicativos do Windows permitem a comunicação entre aplicativos, permitindo que seu aplicativo UWP forneça serviços para outro aplicativo universal do Windows. Os serviços de aplicativo permitem que você crie serviços sem interface do usuário que os aplicativos podem chamar no mesmo dispositivo e, a partir do Windows 10, versão 1607, nos dispositivos remotos. Consulte [Criar e consumir um serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service) para obter mais detalhes.

Os serviços de aplicativo são aplicativos UWP que fornecem serviços a outros aplicativos UWP. Eles são análogos aos serviços da Web em um dispositivo. Um serviço de aplicativo é executado como uma tarefa em segundo plano no aplicativo host e pode fornecer seu serviço a outros aplicativos. Por exemplo, um serviço de aplicativo pode fornecer um serviço de scanner de código de barras que outros aplicativos podem usar. Ou talvez um conjunto de aplicativos Enterprise tenha um serviço de verificação ortográfica que está disponível para os outros aplicativos no pacote.

**Instalação de streaming de aplicativo UWP**

A Instalação de streaming é uma maneira de otimizar o modo como seu aplicativo é disponibilizado aos usuários. Em vez de aguardar até que o aplicativo inteiro seja baixado antes que você poder usá-lo, os usuários podem interagir com o aplicativo assim que uma parte necessária for baixada. Cabe a você, como um desenvolvedor, segmentar seu aplicativo em uma seção obrigatória para ativação básica e iniciar e conteúdo adicional para o restante do aplicativo. Consulte [Instalar streaming de aplicativo UWP](/windows/msix/package/streaming-install) para obter mais informações e detalhes sobre a implementação.

## <a name="see-also"></a>Consulte também

[Criar e consumir um serviço de aplicativo](https://docs.microsoft.com/windows/uwp/launch-resume/how-to-create-and-consume-an-app-service)  
[Introdução aos pacotes de ativos](/windows/msix/package/asset-packages)  
[Criação de pacote com o layout de empacotamento](/windows/msix/package/packaging-layout)  
[Criação de pacotes opcionais e conjunto relacionado](/windows/msix/package/optional-packages)  
[Desenvolvendo com pacotes de ativos e dobramento de pacote](/windows/msix/package/package-folding)  
[Instalação de streaming de aplicativo UWP](/windows/msix/package/streaming-install)  
[Pacotes de aplicativos de pacote simples](/windows/msix/package/flat-bundles)  
[Namespace Windows. ApplicationModel. AppService](https://docs.microsoft.com/uwp/api/Windows.ApplicationModel.AppService)  
[Namespace do Windows. ApplicationModel. Extensions](https://docs.microsoft.com/uwp/api/windows.applicationmodel.appextensions)  
