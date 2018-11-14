---
author: mtoepke
title: Empacotar seu jogo em DirectX da Plataforma Universal do Windows (UWP)
description: Jogos da Plataforma Universal do Windows (UWP) maiores, principalmente os que dão suporte a vários idiomas com ativos específicos de região ou oferecem ativos de alta definição opcionais, podem inflar para tamanhos maiores.
ms.assetid: 68254203-c43c-684f-010a-9cfa13a32a77
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, pacote
ms.localizationpriority: medium
ms.openlocfilehash: 252f67a3cb307f10b1a973a17144f211c9c676b0
ms.sourcegitcommit: 4d88adfaf544a3dab05f4660e2f59bbe60311c00
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6459570"
---
#  <a name="package-your-universal-windows-platform-uwp-directx-game"></a>Empacotar seu jogo em DirectX da Plataforma Universal do Windows (UWP)

Jogos da Plataforma Universal do Windows (UWP) maiores, principalmente os que dão suporte a vários idiomas com ativos específicos de região ou oferecem ativos de alta definição opcionais, podem inflar para tamanhos maiores. Neste tópico, você aprenderá a usar pacotes e lotes de aplicativos para personalizar seu aplicativo de forma que os clientes recebam apenas os recursos que eles realmente precisam.

Além do modelo de pacote do aplicativo, o Windows 10 dá suporte a pacotes de aplicativo que agrupam dois tipos de pacotes:

-   Pacotes de aplicativos que contêm arquivos executáveis e bibliotecas específicos da plataforma. Tipicamente, um jogo UWP pode ter até três pacotes de aplicativo: um para cada arquitetura x86, x64 e CPU ARM. Todos os códigos e dados específicos da plataforma de hardware em questão deverão estar incluídos nesse pacote do aplicativo. O pacote do aplicativo também deve conter todos os principais ativos para que o jogo seja executado com um nível básico de fidelidade e desempenho.
-   Os pacotes de recursos contêm dados opcionais ou expandidos independentes do tipo de plataforma, como ativos do jogo (texturas, malhas, som, texto). Um jogo UWP pode ter um ou mais pacotes de recurso, incluindo pacotes de recurso para ativos ou texturas de alta definição, nível de recursos 11+ do DirectX ou ativos e recursos específicos de idioma.

Para obter mais informações sobre lotes e pacotes de aplicativos, leia [Definição dos recursos do aplicativo](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).

Embora seja possível colocar todo o conteúdo nos pacotes de aplicativos, isso é ineficiente e redundante. Por que reproduzir três vezes o mesmo arquivo grande de textura para cada plataforma, principalmente para plataformas ARM que podem não usá-lo? Uma boa meta é tentar minimizar o que seu cliente precisa baixar, para que ele possa começar a jogar logo o jogo, economizar espaço no dispositivo dele e evitar possíveis custos de largura de banda limitada.

Para usar esse recurso do instalador de aplicativo UWP, é importante levar em consideração o layout do diretório e as convenções de nomeação de arquivo para empacotamento de aplicativo e recurso bem cedo no desenvolvimento do jogo, para que suas ferramentas e sua fonte possam gerá-los corretamente de maneira que o empacotamento fique mais simples. Siga as regras descritas neste documento ao desenvolver ou configurar a criação de ativos e gerenciar ferramentas e scripts, e ao criar código que carrega ou faz referência a recursos.

## <a name="why-create-resource-packs"></a>Por que criar pacotes de recursos?


Quando você cria um aplicativo, principalmente um aplicativo de jogo que pode ser vendido em várias localidades ou uma grande variedade de plataformas de hardware UWP, muitas vezes precisa incluir várias versões de muitos arquivos para oferecer suporte a essas localidades ou plataformas. Por exemplo, se você estiver lançando seu jogo nos Estados Unidos e no Japão, talvez precise de um conjunto de arquivos de voz em inglês para as localidades en-us e de outro em japonês para a localidade jp-jp. Ou, se você quiser usar uma imagem em seu jogo para  dispositivos ARM e as plataformas x86 e x64, será necessário carregar os mesmos ativos de imagem 3 vezes, um para cada arquitetura de CPU.

Além disso, se seu jogo tem muitos recursos de alta definição que não se aplicam a plataformas com níveis de funcionalidade de DirectX mais baixos, por que inclui-los no pacote de aplicativo de linha de base e solicitar que seu usuário baixe um grande volume de componentes que o dispositivo não pode usar? Separar esses recursos de alta definição em um pacote de recursos opcional significa que os clientes com dispositivos que têm suporte para esses recursos de alta definição podem obtê-los ao custo de largura de banda (possivelmente limitada), enquanto aqueles que não têm dispositivos tão modernos podem obter o jogo mais rapidamente e com um custo de uso de rede menor.

Os possíveis conteúdos dos pacotes de recursos dos jogos incluem:

-   Ativos específicos da localidade internacional (textos, áudios ou imagens localizados)
-   Ativos em alta resolução para diferentes fatores de dimensionamento do dispositivo (1.0x, 1.4x e 1.8x)
-   Ativos em alta definição para níveis superiores de recursos do DirectX (9, 10 e 11)

Tudo isso é definido no package.appxmanifest que faz parte do seu projeto UWP e na estrutura do diretório do seu pacote final. Por causa da nova interface de usuário do Visual Studio, se você seguir o processo neste documento, não será preciso editar manualmente.

> **Importante**  o carregamento e o gerenciamento desses recursos são manipulados por meio do **Windows.ApplicationModel.Resources**\ * APIs. Se você usar essas APIs de recursos de modelo de aplicativo para carregar o arquivo correto para uma localidade, um fator de escala ou um nível de recursos de DirectX, não será preciso carregar seus ativos usando caminhos de arquivo explícitos. Em vez disso, você fornece as APIs de recursos somente com o nome de arquivo generalizado do ativo que você quer e deixa o Sistema de Gerenciamento de Recursos obter a variante certa do recurso para a plataforma e a configuração de localidade atuais do usuário (que você também pode especificar diretamente com essas mesmas APIs).

 

Os recursos para empacotamento de recursos são especificados de uma dessas duas maneiras básicas:

-   Os arquivos de recurso têm o mesmo nome de arquivo e as versões específicas do pacote de recursos são colocadas em diretórios com nomes específicos. Esses nomes de diretório são reservados pelo sistema. Por exemplo, \\en-us, \\scale-140, \\dxfl-dx11.
-   Os arquivos de ativos são armazenados em pastas com nomes arbitrários, mas os arquivos são nomeados com um rótulo comum que é anexado com cadeias de caracteres reservadas pelo sistema para denotar idiomas ou outros qualificadores. Especificamente, as cadeias de caracteres de qualificadores são afixadas ao nome de arquivo generalizado após um sublinhado (“\_”). Por exemplo, \\assets\\menu\_option1\_lang-en-us.png, \\assets\\menu\_option1\_scale-140.png, \\assets\\coolsign\_dxfl-dx11.dds. Você também pode juntar essas cadeias de caracteres. Por exemplo, \\assets\\menu\_option1\_scale-140\_lang-en-us.png.
    > **Observação**  quando usado em um nome de arquivo em vez de sozinho em um nome de pasta, um qualificador de idioma deve ter o formato "lang -<tag>", por exemplo, "lang-en-us" conforme descrito em [Personalizar os recursos para idioma, escala e outros qualificadores](../app-resources/tailor-resources-lang-scale-contrast.md).

     

Para uma especificidade adicional no empacotamento de recursos, os nomes de diretórios podem ser combinados. Contudo, eles não podem ser redundantes. Por exemplo, \\en-us\\menu\_option1\_lang-en-us.png é redundante.

Você pode especificar quaisquer nomes de subdiretório não reservados que precisar abaixo de um diretório de recurso, contanto que a estrutura do diretório seja idêntica em cada diretório de recurso. Por exemplo, \\dxfl-dx10\\assets\\textures\\coolsign.dds. Quando você carrega ou faz referência a um ativo, o nome do caminho deve ser generalizado, removendo quaisquer qualificadores de idioma, escala ou nível de recursos do DirectX, estejam eles em nós de pasta ou nos nomes de pasta. Por exemplo, para fazer referência em código a um ativo para o qual uma das variantes é \\dxfl-dx10\\assets\\textures\\coolsign.dds, use \\assets\\textures\\coolsign.dds. Da mesma forma, para se referir a um ativo com uma variante \\images\\background\_scale-140.png, use \\images\\background.png.

Aqui estão os seguintes nomes de diretório reservados e prefixos de sublinhado de nome do arquivo:

| Tipo de ativo                   | Nome do diretório do pacote de recursos                                                                                                                  | Sufixo do nome de arquivo do pacote de recursos                                                                                                    |
|------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------|----------------------------------------------------------------------------------------------------------------------------------|
| Ativos localizados             | Todos os idiomas possíveis, ou combinações de idioma e localidade, para Windows 10. (O prefixo qualificador "lang-" não é exigido em um nome de pasta). | Um "\_" seguido pelo especificador de idioma, localidade ou idioma-localidade. Por exemplo, "\_en", "\_us" ou "\_en-us", respectivamente. |
| Ativos do fator de escala        | scale-100, scale-140, scale-180. Servem para os fatores de escala de interface do usuário 1.0x, 1.4x e 1.8x, respectivamente.                                     | Um "\_" seguido por "scale-100", "scale-140" ou "scale-180".                                                                    |
| Ativos de nível de recursos do DirectX | dxfl-dx9, dxfl-dx10 e dxfl-dx11. Servem para os níveis de recursos do DirectX 9, 10 e 11, respectivamente.                                     | Um "\_" seguido por "dxfl-dx9", "dxfl-dx10" ou "dxfl-dx11".                                                                     |

 

## <a name="defining-localized-language-resource-packs"></a>Definindo pacotes de recursos de idiomas localizados


Os arquivos específicos por localidade são colocados em diretórios do projeto nomeados para o idioma (por exemplo, "en").

Ao configurar seu aplicativo para dar suporte a ativos localizados para vários idiomas:

-   Crie um subdiretório de aplicativo (ou uma versão de arquivo) para cada idioma e localidade a que você oferecerá suporte (por exemplo, en-us, jp-jp, zh-cn, fr-fr e assim por diante).
-   Durante o desenvolvimento, coloque cópias de TODOS os ativos (como arquivos de áudio, texturas e gráficos de menu localizados) no subdiretório da localidade do idioma correspondente, mesmo que eles não sejam diferentes entre os idiomas ou localidades. Para proporcionar uma excelente experiência para o usuário, garanta que o usuário seja alertado se ele não tiver obtido um pacote de recursos de idioma disponível para sua localidade se houver um disponível (ou se ele tiver acidentalmente excluído o pacote após o download e a instalação).
-   Certifique-se de que cada arquivo de recursos de ativo ou cadeia de caracteres (.resw) tenha o mesmo nome em cada diretório. Por exemplo, menu\_option1.png deve ter o mesmo nome nos diretórios \\en-us e \\jp-jp, mesmo que o conteúdo do arquivo seja para um idioma diferente. Nesse caso, você os veria como \\en-us\\menu\_option1.png e \\jp-jp\\menu\_option1.png.
    > **Observação**  , opcionalmente, você pode anexar a localidade ao nome do arquivo e armazená-los no mesmo diretório; Por exemplo, \\assets\\menu\_option1\_lang-en-us.png, \\assets\\menu\_option1\_lang-JP-jp.PNG..

     

-   Use as APIs em [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022) e [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) para especificar e carregar os recursos específicos de localidade para seu aplicativo. Além disso, use referências de ativo que não incluam a localidade específica, já que essas APIs determinam a localidade correta com base nas configurações do usuário e recuperam o recurso certo para ele.
-   No Microsoft Visual Studio2015, selecione **projeto -> loja -> Criar pacote do aplicativo...** e crie o pacote.

## <a name="defining-scaling-factor-resource-packs"></a>Definição do fator de dimensionamento dos pacotes de recursos


Windows 10 oferece três fatores de escala de interface de usuário: 1.0 x, 1.4 x e 1.8 x. Os valores de dimensionamento para cada tela são definidos durante a instalação, de acordo com diversos fatores combinados: o tamanho da tela, a resolução da tela e a distância média presumida do usuário em relação à tela. O usuário também pode ajustar fatores de escala para melhorar a legibilidade. Seu jogo deve permitir reconhecimento de DPI e de fator de dimensionamento para a melhor experiência possível. Parte desse reconhecimento significa criar versões de ativos visuais críticos para cada um dos três fatores de dimensionamento. Isso também inclui testes de clique e interação do ponteiro!

Quando estiver configurando seu aplicativo para oferecer suporte a pacotes de recursos para diferentes fatores de escala de aplicativo UWP, você deve:

-   Crie um subdiretório do aplicativo (ou versão do arquivo) para cada fator de dimensionamento ao qual você dará suporte (escala-100, escala-140 e escala-180).
-   Durante o desenvolvimento, coloque cópias apropriadas ao fator de dimensionamento de TODOS os ativos em cada diretório de recursos do fator de dimensionamento, mesmo que eles não sejam diferentes entre os fatores de dimensionamento.
-   Certifique-se de que cada ativo tenha o mesmo nome em cada diretório. Por exemplo, menu\_option1.png deve ter o mesmo nome nos diretórios \\scale-100 e \\scale-180, mesmo que o conteúdo do arquivo seja diferente. Nesse caso, você os veria como \\scale-100\\menu\_option1.png e \\scale-140\\menu\_option1.png.
    > **Observação**  novamente, você pode, opcionalmente, acrescente o sufixo do fator de escala ao nome do arquivo e armazená-los no mesmo diretório; Por exemplo, \\assets\\menu\_option1\_scale-100.png, \\assets\\menu\_option1\_scale-140.PNG..

     

-   Use as APIs em [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) para carregar os ativos. As referências a ativos devem ser generalizadas (sem sufixo), omitindo a variação de escala específica. O sistema irá recuperar o recurso de escala apropriado para a exibição e as configurações do usuário.
-   No Visual Studio2015, selecione **projeto -> loja -> Criar pacote do aplicativo...** e crie o pacote.

## <a name="defining-directx-feature-level-resource-packs"></a>Definição do nível de recursos do DirectX dos pacotes de recursos


Os níveis de recursos do DirectX correspondem a configurações de recursos de GPU para versões anteriores e atuais do DirectX (especificamente, o Direct3D). Isso inclui especificações e funcionalidades do modelo de sombreador, suporte a linguagem de sombreador, suporte a compressão de textura e recursos de pipeline de elementos gráficos gerais.

Seu pacote do aplicativo básico deve usar os formatos básicos de compactação de textura: BC1, BC2 ou BC3. Esses formatos podem ser consumidos por qualquer dispositivo UWP, desde plataformas ARM inferiores até estações de trabalho multi-GPU dedicadas e computadores de mídia.

O suporte ao formato de textura no nível de recursos 10 do DirectX ou superior deve ser adicionado em um pacote de recursos para conservar espaço em disco local e largura de banda para download. Isso possibilita o uso dos esquemas de compactação mais avançados para 11, como BC6H e BC7. (Para obter mais detalhes, consulte [Compactação do bloco de textura no Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh308955)). Esses formatos são mais eficientes para ativos de textura de alta resolução suportados por GPUs modernas, sendo que usá-los melhora a aparência, o desempenho e os requisitos de espaço de seu jogo em plataformas de tecnologia de ponta.

| Nível de recursos do DirectX | Compactação de textura suportada |
|-----------------------|-------------------------------|
| 9                     | BC1, BC2, BC3                 |
| 10                    | BC4, BC5                      |
| 11                    | BC6H, BC7                     |

 

Além disso, cada nível de recursos do DirectX dá suporte a diferentes versões de modelo de sombreador. Os recursos de sombreador compilado podem ser criados por recurso e podem ser incluídos nos pacotes de recursos do nível de recursos do DirectX. Algumas versões mais recentes de modelos de sombreador podem usar ativos, como mapas normais, que as versões anteriores de sombreador não podiam. Esses ativos específicos do modelo de sombreador também devem ser incluídos em um pacote de recursos do nível de recursos do DirectX.

O mecanismo de recursos concentra-se principalmente nos formatos de textura suportados para os ativos, por isso ele dá suporte a apenas 3 níveis de recursos gerais. Se você precisa ter sombreadores separados para subníveis (versões de ponto) como DX9\_1 vs DX9\_3, seu gerenciamento de ativos e código de renderização devem manipulá-los explicitamente.

Ao configurar seu aplicativo para dar suporte a pacotes de recursos de diferentes níveis de recursos do DirectX:

-   Crie um subdiretório do aplicativo (ou versão do arquivo) para cada nível de recursos do DirectX ao qual você dará suporte (dxfl-dx9, dxfl-dx10 e dxfl-dx11).
-   Durante o desenvolvimento, coloque ativos específicos do nível de recursos em cada diretório de recursos do nível de recursos. Ao contrário das localidades e dos fatores de dimensionamento, você pode ter diferentes ramificações de código de renderização para cada nível de recursos em seu jogo, e se você tiver texturas, sombreadores compilados ou outros ativos que são usados em um nível ou em um subconjunto de todos os níveis de recursos suportados, coloque os ativos correspondentes apenas nos diretórios dos níveis de recursos que os utilizam. No caso dos ativos que são carregados em todos os níveis de recursos, verifique se cada diretório de recursos do nível de recursos possui uma versão com o mesmo nome. Por exemplo, para uma textura independente de nível de recursos com o nome "coolsign.dds", coloque a versão comprimida BC3 no diretório \\dxfl-dx9 e a versão comprimida BC7 no diretório \\dxfl-dx11.
-   Certifique-se de que cada ativo (se estiverem disponíveis para vários níveis de recursos) tenha o mesmo nome em cada diretório. Por exemplo, coolsign.dds deve ter o mesmo nome nos diretórios \\dxfl-dx9 e \\dxfl-dx11, mesmo que o conteúdo do arquivo seja diferente. Nesse caso, você os veria como \\dxfl-dx9\\coolsign.dds e \\dxfl-dx11\\coolsign.dds.
    > **Observação**  novamente, você pode, opcionalmente, acrescente o sufixo do nível de recurso ao nome do arquivo e armazená-los no mesmo diretório; Por exemplo, \\textures\\coolsign\_dxfl-dx9.dds, \\textures\\coolsign\_dxfl-dx11.DDS..

     

-   Declare os níveis de recursos do DirectX quando estiver configurando seus recursos de elementos gráficos.
    ```cpp
    D3D_FEATURE_LEVEL featureLevels[] = 
    {
      D3D_FEATURE_LEVEL_11_1,
      D3D_FEATURE_LEVEL_11_0,
      D3D_FEATURE_LEVEL_10_1,
      D3D_FEATURE_LEVEL_10_0,
      D3D_FEATURE_LEVEL_9_3,
      D3D_FEATURE_LEVEL_9_1
    };
    ```

    ```cpp
    ComPtr<ID3D11Device> device;
    ComPtr<ID3D11DeviceContext> context;
    D3D11CreateDevice(
        nullptr,                    // Use the default adapter.
        D3D_DRIVER_TYPE_HARDWARE,
        0,                      // Use 0 unless it is a software device.
        creationFlags,          // defined above
        featureLevels,          // What the app will support.
        ARRAYSIZE(featureLevels),
        D3D11_SDK_VERSION,      // This should always be D3D11_SDK_VERSION.
        &device,                    // created device
        &m_featureLevel,            // The feature level of the device.
        &context                    // The corresponding immediate context.
    );
    ```

-   Use as APIs em [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) para carregar os recursos. As referências a ativos devem ser generalizadas (sem sufixo), omitindo o nível de recursos. Entretanto, ao contrário de idioma e escala, o sistema não determina automaticamente qual nível de recurso é ideal para uma determinada exibição; cabea você determinar com base na lógica do código. Depois de fazer essa determinação, use as APIs para informar ao sistema operacional sobre o nível de recurso preferencial. Assim, o sistema poderá recuperar o recurso correto com base nessa preferência. Aqui está um exemplo de código que mostra como informar seu aplicativo sobre o nível de recursos do DirectX atual para a plataforma:
    
    ```cpp
    // Set the current UI thread's MRT ResourceContext's DXFeatureLevel with the right DXFL. 

    Platform::String^ dxFeatureLevel;
        switch (m_featureLevel)
        {
        case D3D_FEATURE_LEVEL_9_1:
        case D3D_FEATURE_LEVEL_9_2:
        case D3D_FEATURE_LEVEL_9_3:
            dxFeatureLevel = L"DX9";
            break;
        case D3D_FEATURE_LEVEL_10_0:
        case D3D_FEATURE_LEVEL_10_1:
            dxFeatureLevel = L"DX10";
            break;
        default:
            dxFeatureLevel = L"DX11";
        }

        ResourceContext::SetGlobalQualifierValue(L"DXFeatureLevel", dxFeatureLevel);
    ```

    > **Observação**em seu código, carregue a textura diretamente pelo nome (ou o caminho abaixo do diretório de nível de recursos). Não inclua o nome do diretório de nível de recursos nem o sufixo. Por exemplo, carregue "textures\\coolsign.dds", não "dxfl-dx11\\textures\\coolsign.dds" ou "textures\\coolsign\_dxfl-dx11.dds".

     

-   Agora, use o [**ResourceManager**](https://msdn.microsoft.com/library/windows/apps/br206078) para localizar o arquivo que corresponda ao nível de recursos de DirectX atual. O **ResourceManager** retorna um [**ResourceMap**](https://msdn.microsoft.com/library/windows/apps/br206089), que você consulta com [**ResourceMap:: GetValue**](https://msdn.microsoft.com/library/windows/apps/br206098) (ou [**ResourceMap:: TryGetValue**](https://msdn.microsoft.com/library/windows/apps/jj655438)) e um [**ResourceContext**](https://msdn.microsoft.com/library/windows/apps/br206064) fornecido. Isso retorna um [**ResourceCandidate**](https://msdn.microsoft.com/library/windows/apps/br206051) que corresponde mais proximamente ao nível de recursos de DirectX que foi especificando chamando [**SetGlobalQualifierValue**](https://msdn.microsoft.com/library/windows/apps/mt622101).
    
    ```cpp
    // An explicit ResourceContext is needed to match the DirectX feature level for the display on which the current view is presented.

    auto resourceContext = ResourceContext::GetForCurrentView();
    auto mainResourceMap = ResourceManager::Current->MainResourceMap;

    // For this code example, loader is a custom ref class used to load resources.
    // You can use the BasicLoader class from any of the 8.1 DirectX samples similarly.


    auto possibleResource = mainResourceMap->GetValue(
        L"Files/BumpPixelShader.cso",
        resourceContext
    );
    Platform::String^ resourceName = possibleResource->ValueAsString;
    ```

-   No Visual Studio2015, selecione **projeto -> loja -> Criar pacote do aplicativo...** e crie o pacote.
-   Certifique-se de ativar lotes de aplicativo nas configurações de manifesto package.appxmanifest.

## <a name="related-topics"></a>Tópicos relacionados


* [Definindo recursos de aplicativo](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321)
* [Empacotando aplicativos](https://msdn.microsoft.com/library/windows/apps/mt270969)
* [Empacotador de aplicativo (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767)

 

 




