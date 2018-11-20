---
author: laurenhughes
title: Introdução aos pacotes de ativo
description: Pacotes de ativo são um tipo de pacote que atuam como um local centralizado para arquivos comuns de um aplicativo, eliminando a necessidade de arquivos duplicados nos pacotes de arquitetura.
ms.author: lahugh
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, empacotamento, layout do pacote, pacote de ativo
ms.localizationpriority: medium
ms.openlocfilehash: 98980e67d24eb96aa55af7fefe10b5e4c2cdfa67
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7419972"
---
# <a name="introduction-to-asset-packages"></a>Introdução aos pacotes de ativo

> [!IMPORTANT]
> Se você pretender enviar seu aplicativo para a Store, precisará entrar em contato com [Suporte ao desenvolvedor Windows](https://developer.microsoft.com/windows/support) e receber aprovação para usar pacotes de ativo.

Pacotes de ativo são um tipo de pacote que atuam como um local centralizado para arquivos comuns de um aplicativo, eliminando a necessidade de arquivos duplicados nos pacotes de arquitetura. Os pacotes de ativo são semelhantes aos pacotes de recursos porque ambos foram criados para conter conteúdo estático necessário para que seu aplicativo seja executado, mas são diferentes porque todos os pacotes de ativo são sempre baixados, independentemente da arquitetura do sistema do usuário, da linguagem ou da escala de exibição.

![Diagrama de pacote de ativo](images/primary-bundle.png)

Como os pacotes de ativo contêm todos os arquivos independentes de arquitetura, linguagem e escala, aproveitar os pacotes de ativo resulta em diminuição geral do tamanho do aplicativo empacotado (porque esses arquivos não são duplicados), ajudando você a gerenciar o uso de espaço de disco de desenvolvimento local para grandes aplicativos e gerenciar os pacotes do aplicativo em geral. 

### <a name="how-do-asset-packages-affect-publishing"></a>Como os pacotes de ativo afetam a publicação?
A vantagem mais óbvia de pacotes de ativo é o tamanho reduzido de aplicativos empacotados. Pacotes de aplicativo menores aceleram o processo de publicação do aplicativo, permitindo que a Store processe menos arquivos. No entanto, isso não é o benefício mais importante dos pacotes de ativo.

Quando um pacote de ativo é criado, você pode especificar se o pacote deve ter permissão para ser executado. Como os pacotes de ativo devem conter apenas arquivos independentes da arquitetura, eles geralmente não contêm todos os arquivos .dll ou .exe. Assim, os pacotes de ativo normalmente não precisam ser executados. A importância dessa distinção é que, durante o processo de publicação, todos os pacotes executáveis devem ser examinados para garantir que não contenham malware, e esse processo de verificação leva mais tempo para pacotes maiores. No entanto, se um pacote for designado como não executável, a instalação do aplicativo garantirá que os arquivos contidos nesse pacote não possam ser executados. Essa garantia elimina a necessidade de um exame completo do pacote e reduzirá consideravelmente o tempo de varredura de malware durante a publicação do aplicativo (e nas atualizações também). Isso agiliza muito a publicação de aplicativos que usam pacotes de ativo. Observe que [pacotes simples do aplicativo](flat-bundles.md) também deve ser usado para obter esse benefício de publicação, pois isso permite que a Store processe cada arquivo do pacote. AppX ou .msix em paralelo. 


### <a name="should-i-use-asset-packages"></a>Devo usar pacotes de ativo?
Atualizar a estrutura de arquivos do seu aplicativo para aproveitar o uso de pacotes de ativo pode trazer benefícios tangíveis: redução do tamanho do pacote e iterações de desenvolvimento mais sólidas. Se todos os seus pacotes de arquitetura tiverem uma quantidade significativa de arquivos em comum ou se a maior parte do seu aplicativo for composta de arquivos não executáveis, é altamente recomendável passar mais tempo para converter usando pacotes de ativo.

No entanto, é preciso tomar cuidado porque os pacotes de ativo não são um meio para gerar opções de conteúdo do aplicativo. Os arquivos do pacote de ativo não são opcionais e **sempre** serão baixados, independentemente da arquitetura, linguagem ou escala do dispositivo de destino. Qualquer conteúdo opcional que você queira que seu aplicativo suporte deve ser implementado usando [pacotes opcionais](optional-packages.md). 


### <a name="how-to-create-an-asset-package"></a>Como criar um pacote de ativo
A maneira mais fácil para criar pacotes de ativo é usando o layout de empacotamento. No entanto, as pacotes de ativo também podem ser criados manualmente usando MakeAppx.exe. Para especificar quais arquivos devem ser incluídos no pacote do ativo, você precisará criar um "arquivo de mapeamento". Neste exemplo, o único arquivo no pacote do ativo é "Video.mp4", mas todos os arquivos do pacote de ativo devem ser listados aqui. Observe que o especificador **ResourceDimensions** em **ResourceMetadata** é omitido para pacotes de ativo (em comparação com um arquivo de mapeamento para pacotes de recursos).

```example 
[ResourceMetadata]
"ResourceId"        "Videos"

[Files]
"Video.mp4"         "Video.mp4"
```

Use este comando para criar o pacote de ativo usando MakeAppx.exe: 

```syntax 
MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.appx

...

MakeAppx.exe pack /r /m AppxManifest.xml /f MappingFile.txt /p Videos.msix

```
Deve-se notar aqui que todos os arquivos referenciados no AppxManifest (os arquivos de logotipo) não podem ser movidos para pacotes de ativo – esses arquivos devem ser duplicados em todos os pacotes de arquitetura. Os pacotes de ativo também não devem conter um resources.pri; MRT não pode ser usado para acessar arquivos do pacote de ativo. Para saber mais sobre como acessar arquivos do pacote de ativo e por que os pacotes de ativo requerem que seu aplicativo seja instalado em uma unidade NTFS, consulte [Desenvolvendo com pacotes de ativo e dobra de pacote](Package-Folding.md).

Para controlar se um pacote de ativo pode ser executado ou não, você pode usar **[uap6:AllowExecution](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-uap6-allowexecution)** no elemento **Properties** de AppxManifest. Você também deve adicionar **uap6** ao elemento **Package** de nível superior para se tornar o seguinte: 

```XML
<Package IgnorableNamespaces="uap uap6" 
xmlns:uap6="http://schemas.microsoft.com/appx/manifest/uap/windows10/6" 
xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10" 
xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10">
```

 Se não for especificado, o valor padrão de **AllowExecution** é **true** – defina-o como **false** para pacotes de ativo sem executáveis para agilizar a publicação.  



