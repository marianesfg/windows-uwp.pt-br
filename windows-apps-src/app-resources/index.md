---
Description: Esta seção mostra como criar, empacotar e consumir recursos de arquivo, imagem e cadeia de caracteres do aplicativo.
title: Os recursos de aplicativo e o Sistema de Gerenciamento de Recursos
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: 5b45f33e9849a46e22250640b88a85ea16143231
ms.sourcegitcommit: f727b68e86a86c94eff00f67ed79a1c12666e7bc
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "77089312"
---
# <a name="app-resources-and-the-resource-management-system"></a>Os recursos de aplicativo e o Sistema de Gerenciamento de Recursos


Esta seção mostra como criar, empacotar e consumir recursos de arquivo, imagem e cadeia de caracteres do aplicativo. Por exemplo, você pode empacotar um arquivo com seu jogo casual que contém uma definição dos níveis do jogo e carregar o arquivo em tempo de execução. Mostramos também como manter os recursos, independentemente da lógica do aplicativo, facilita a localização e a personalização do aplicativo para diferentes localidades, telas de dispositivo, configurações de acessibilidade e outros contextos de usuário e computador. Recursos como cadeias de caracteres e imagens normalmente precisam existir em vários idiomas, escala e variantes de contraste. Para recursos desse tipo, você tem o suporte do [Sistema de Gerenciamento de Recursos](resource-management-system.md).

Existem dois tipos de recurso de aplicativo.
- Um recurso de arquivo é um recurso armazenado como um arquivo no disco. O recurso de arquivo pode conter uma imagem de bitmap, XAML, XML, HTML ou qualquer outro tipo de dados.
- Um recurso inserido é um recurso incorporado em algum arquivo de recurso que pode receber outros itens. O exemplo mais comum é um recurso de cadeia de caracteres inserido em um arquivo de recursos (.resw ou. resjson).

Para obter mais informações sobre a proposta de valor de localização do aplicativo, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md).

| Artigo | Descrição |
|---------|-------------|
| [Sistema de Gerenciamento de Recursos](resource-management-system.md) | No momento do build, o Sistema de Gerenciamento de Recursos cria um índice de todas as variantes dos recursos que são empacotados com o aplicativo. Em tempo de execução, o sistema detecta as configurações do usuário e do computador que estão em vigor e carrega os recursos que representam a melhor correspondência para essas configurações. |
| [Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe](how-rms-matches-and-chooses-resources.md) | Quando um recurso é solicitado, pode haver vários candidatos que correspondam em um certo grau ao contexto de recurso atual. O Sistema de Gerenciamento de Recursos analisará todos os candidatos e determinará o melhor deles para ser retornado. Este tópico descreve detalhadamente esse processo e fornece exemplos. |
| [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](how-rms-matches-lang-tags.md) | O tópico anterior ([Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe](how-rms-matches-and-chooses-resources.md)) analisa a correspondência de qualificador em geral. Este tópico aborda a correspondência de marcação de idioma mais detalhadamente. |
| [Personalizar os recursos de idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md) | Este tópico descreve o conceito geral dos qualificadores de recurso, como usá-los e a finalidade de cada nome de qualificador. |
| [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md) | Se você deseja que o aplicativo dê suporte a diferentes idiomas de exibição e tem literais de cadeia de caracteres no código, na marcação XAML ou no manifesto do pacote do aplicativo, mova essas cadeias de caracteres para um arquivo de recursos (.resw). Em seguida, você poderá fazer uma cópia traduzida desse arquivo de recursos para cada idioma ao qual o aplicativo dê suporte. |
| [Carregar imagens e ativos personalizados para escala, tema, alto contraste e outros](images-tailored-for-scale-theme-contrast.md) | O aplicativo pode carregar arquivos de recurso de imagem contendo imagens personalizadas para fator de escala de exibição, tema, alto contraste e outros contextos de runtime. |
| [Esquemas de URI](uri-schemes.md) | Há vários esquemas de URI (Uniform Resource Identifier) que você pode usar para fazer referência aos arquivos que vêm no conjunto de aplicativo, nas pastas de dados do seu aplicativo ou na nuvem. Você também pode usar um esquema de URI para fazer referência a cadeias de caracteres carregadas dos arquivos de recursos (.resw) do aplicativo. |
| [Especificar os recursos padrão usados pelo aplicativo](specify-default-resources-installed.md) | Se o aplicativo não tiver recursos que correspondam às configurações específicas de um dispositivo de cliente, os recursos padrão do aplicativo serão usados. Este tópico explica como especificar quais são esses recursos padrão. |
| [Crie recursos no pacote do aplicativo, não em um pacote de recursos](build-resources-into-app-package.md) | Alguns tipos de aplicativos (dicionários multilíngues, ferramentas de tradução etc.) precisam substituir o comportamento padrão de um lote de aplicativo e criar recursos no pacote do aplicativo, em vez de tê-los em pacotes de recursos separados. Este tópico explica como fazer isso. |
| [APIs de PRI (índice de recurso do pacote) e sistemas de build personalizados](pri-apis-custom-build-systems.md) | Com as [APIs de PRI (índice de recurso do pacote)](https://docs.microsoft.com/windows/desktop/menurc/pri-indexing-reference), você pode desenvolver um sistema de build personalizado para recursos do aplicativo UWP. O sistema de build será capaz de criar, controlar a versão e despejar os arquivos de PRI (índice de recurso do pacote) (como XML) em qualquer nível de complexidade exigido pelo aplicativo UWP. |
| [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md) | O MakePri.exe é uma ferramenta de linha de comando que você pode usar para criar e despejar arquivos PRI. Ele é integrado como parte do MSBuild no Microsoft Visual Studio, mas pode ser útil para criar pacotes manualmente ou com um sistema de build personalizado. |
| [Use o Sistema de Gerenciamento de Recursos do Windows 10 em um aplicativo ou jogo herdado](using-mrt-for-converted-desktop-apps-and-games.md) | Empacotando o aplicativo ou jogo .NET ou Win32 como um pacote .msix ou .appx, você pode aproveitar o Sistema de Gerenciamento de Recursos para carregar recursos de aplicativo personalizados para o contexto de runtime. Este tópico detalhado descreve as técnicas. |

Consulte também [Suporte a blocos e notificações do sistema para idioma, escala e alto contraste](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).
