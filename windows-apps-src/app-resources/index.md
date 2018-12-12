---
Description: This section shows you how to author, package, and consume your app's string, image, and file resources.
title: Os recursos de app e o Sistema de Gerenciamento de Recursos
label: Intro
template: detail.hbs
ms.date: 10/20/2017
ms.topic: article
keywords: windows 10, uwp, recurso, imagem, ativo, MRT, qualificador
ms.localizationpriority: medium
ms.openlocfilehash: a5af904c099b92e399f169221cae3122f358be19
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8938806"
---
# <a name="app-resources-and-the-resource-management-system"></a>Os recursos de app e o Sistemas de Gerenciamento de Recursos


Esta seção mostra como criar, empacotar e consumir recursos de arquivo, imagem e cadeia de caracteres do app. Por exemplo, você pode empacotar um arquivo com seu jogo casual que contém uma definição dos níveis do jogo e carregar o arquivo em tempo de execução. Mostramos também como manter os recursos independentemente da lógica do app facilita a localização e a personalização do app para diferentes localidades, telas de dispositivo, configurações de acessibilidade e outros contextos de usuário e computador. Recursos como cadeias de caracteres e imagens normalmente precisam existir em vários idiomas, escala e variantes de contraste. Para recursos desse tipo, você tem o suporte do [Sistema de Gerenciamento de Recursos](resource-management-system.md).

Existem dois tipos de recurso de app.
- Um recurso de arquivo é um recurso armazenado como um arquivo no disco. O recurso de arquivo pode conter uma imagem de bitmap, XAML, XML, HTML ou qualquer outro tipo de dados.
- Um recurso inserido é um recurso incorporado em algum arquivo de recurso que pode receber outros itens. O exemplo mais comum é um recurso de cadeia de caracteres inserido em um arquivo de recursos (.resw ou. resjson).

Para obter mais informações sobre a proposta de valor de localização do app, consulte [Globalização e localização](../design/globalizing/globalizing-portal.md).

| Artigo | Descrição |
|---------|-------------|
| [Sistema de Gerenciamento de Recursos](resource-management-system.md) | No momento da compilação, o Sistema de Gerenciamento de Recursos cria um índice de todas as variantes dos recursos que são empacotados com o app. Em tempo de execução, o sistema detecta as configurações do usuário e da máquina que estão em vigor e carrega os recursos que representam a melhor correspondência para essas configurações. |
| [Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe](how-rms-matches-and-chooses-resources.md) | Quando um recurso é solicitado, pode haver vários candidatos que correspondam em um certo grau ao contexto de recurso atual. O Sistema de Gerenciamento de Recursos analisará todos os candidatos e determinará o melhor deles para ser retornado. Este tópico descreve detalhadamente esse processo e fornece exemplos. |
| [Como o Sistema de Gerenciamento de Recursos faz a correspondência de marcas de idioma](how-rms-matches-lang-tags.md) | O tópico anterior ([Como o Sistema de Gerenciamento de Recursos faz a correspondência dos recursos e os escolhe](how-rms-matches-and-chooses-resources.md)) analisa a correspondência de qualificador em geral. Este tópico aborda a correspondência de marca de idioma mais detalhadamente. |
| [Personalizar os recursos para idioma, escala, alto contraste e outros qualificadores](tailor-resources-lang-scale-contrast.md) | Este tópico descreve o conceito geral dos qualificadores de recurso, como usá-los e a finalidade de cada nome de qualificador. |
| [Localizar cadeias de caracteres na interface do usuário e no manifesto do pacote do aplicativo](localize-strings-ui-manifest.md) | Se você deseja que o app ofereça suporte a diferentes idiomas de exibição e houver literais de cadeia de caracteres no código, na marcação XAML ou no manifesto do pacote de aplicativos, mova essas cadeias de caracteres para um arquivo de recursos (.resw). Em seguida, você poderá fazer uma cópia traduzida desse arquivo de recursos para cada idioma ao qual o app ofereça suporte. |
| [Carregar imagens e ativos personalizados para escala, tema, alto contraste e outros](images-tailored-for-scale-theme-contrast.md) | O app pode carregar arquivos de recurso de imagem contendo imagens personalizadas para fator de escala de exibição, tema, alto contraste e outros contextos de tempo de execução. |
| [Esquemas de URI](uri-schemes.md) | Há vários esquemas de URI (Uniform Resource Identifier) que você pode usar para fazer referência aos arquivos que vêm no conjunto de aplicativo, nas pastas de dados do seu aplicativo ou na nuvem. Você também pode usar um esquema de URI para fazer referência a cadeias de caracteres carregadas dos arquivos de recursos (.resw) do app. |
| [Especificar os recursos padrão usados pelo app](specify-default-resources-installed.md) | Se o app não tiver recursos que correspondam às configurações específicas de um dispositivo de cliente, os recursos padrão do app serão usados. Este tópico explica como especificar quais são esses recursos padrão. |
| [Compilar recursos no pacote de aplicativos, e não em um pacote de recursos](build-resources-into-app-package.md) | Alguns tipos de apps (dicionários multilíngues, ferramentas de tradução etc.) precisam substituir o comportamento padrão de um lote de aplicativo e compilar recursos no pacote de aplicativos, em vez de tê-los em pacotes de recursos separados. Este tópico explica como fazer isso. |
| [APIs de índice de recurso do pacote (PRI) e sistemas de compilação personalizados](pri-apis-custom-build-systems.md) | Com as [APIs de índice de recurso do pacote (PRI)](https://msdn.microsoft.com/library/windows/desktop/mt845690), você pode desenvolver um sistema de compilação personalizado para recursos do aplicativo UWP. O sistema de compilação será capaz de criar, controlar a versão e despejar os arquivos de índice de recurso do pacote (PRI) (como XML) em qualquer nível de complexidade exigido pelo aplicativo UWP. |
| [Compilar recursos manualmente com o MakePri.exe](compile-resources-manually-with-makepri.md) | O MakePri.exe é uma ferramenta de linha de comando que você pode usar para criar e despejar arquivos PRI. Ele é integrado como parte do MSBuild no Microsoft Visual Studio, mas pode ser útil para criar pacotes manualmente ou com um sistema de compilação personalizado. |
| [Use o Sistema de Gerenciamento de Recursos do Windows 10 em um app ou jogo herdado](using-mrt-for-converted-desktop-apps-and-games.md) | Empacotando o app ou jogo .NET ou Win32 como um pacote AppX, você pode aproveitar o Sistema de Gerenciamento de Recursos para carregar recursos de app personalizados para o contexto de tempo de execução. Este tópico detalhado descreve as técnicas. |

Consulte também [Suporte a blocos e notificações do sistema para idioma, escala e alto contraste](../design/shell/tiles-and-notifications/tile-toast-language-scale-contrast.md).
