---
title: Carregar recursos no jogo em DirectX
description: A maioria dos jogos, em algum momento, carrega recursos e ativos (por exemplo, sombreadores, texturas, malhas predefinidas ou outros dados gráficos) do armazenamento local ou de algum outro fluxo de dados.
ms.assetid: e45186fa-57a3-dc70-2b59-408bff0c0b41
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx, carregamento de recursos
ms.localizationpriority: medium
ms.openlocfilehash: 56eaebfeb6d644c4c15f14f0613b1e3b1781f637
ms.sourcegitcommit: 22ed0d4edad5e6bab352e641cf86cf455cf83825
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/22/2020
ms.locfileid: "85133989"
---
# <a name="load-resources-in-your-directx-game"></a>Carregar recursos no jogo em DirectX



A maioria dos jogos, em algum momento, carrega recursos e ativos (por exemplo, sombreadores, texturas, malhas predefinidas ou outros dados gráficos) do armazenamento local ou de algum outro fluxo de dados. Aqui, vamos examinar uma exibição de alto nível daquilo que é preciso considerar ao carregar esses arquivos para uso no jogo DirectX C/C++ da UWP (Plataforma Universal do Windows).

Por exemplo, é possível que as malhas para objetos poligonais do jogo tenham sido criadas com outra ferramenta e exportadas para um formato específico. O mesmo é verdadeiro para texturas e muito mais, por isso: embora um bitmap descompactado possa ser rotineiramente criado pela maioria das ferramentas e entendido por grande parte das APIs gráficas, esse bitmap pode ser extremamente ineficiente se usado no jogo. Nesta seção, vamos guiá-lo pelas etapas básicas do carregamento de três tipos diferentes de recursos gráficos para uso com o Direct3D: malhas (modelos), texturas (bitmaps) e objetos de sombreador compilados.

## <a name="what-you-need-to-know"></a>O que você precisa saber


### <a name="technologies"></a>Tecnologias

-   Padrão PPL (ppltasks.h)

### <a name="prerequisites"></a>Pré-requisitos

-   Entender os princípios básicos do Windows Runtime
-   Entender tarefas assíncronas
-   Entenda os conceitos básicos de programação de gráficos 3D.

Esse exemplo também inclui três arquivos de código para carregamento e gerenciamento de recursos. Você encontrará o objetos de código definidos nesses arquivos, neste tópico.

-   BasicLoader.h/.cpp 
-   BasicReaderWriter.h/.cpp
-   DDSTextureLoader.h/.cpp

O código completo desses exemplos pode ser encontrado nos seguintes links.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Tópico</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-basicloader.md">Concluir código para BasicLoader</a></p></td>
<td align="left"><p>Conclua o código para classe e métodos que convertam e carreguem objetos da malha de elementos gráficos na memória.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="complete-code-for-basicreaderwriter.md">Concluir código para BasicReaderWriter</a></p></td>
<td align="left"><p>Conclua código para classe e métodos de leitura e gravação de arquivos de dados binários em geral. Usado pela classe <a href="complete-code-for-basicloader.md">BasicLoader</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="complete-code-for-ddstextureloader.md">Concluir código para DDSTextureLoader</a></p></td>
<td align="left"><p>Conclua o código para a classe e o método que carrega uma textura DDS da memória.</p></td>
</tr>
</tbody>
</table>

 

## <a name="instructions"></a>Instruções

### <a name="asynchronous-loading"></a>Carregamento assíncrono

O carregamento assíncrono é feito com o modelo **task** do padrão PPL. A **task** contém uma chamada de método seguida por um lambda que processa os resultados da chamada assíncrona depois de concluída e, normalmente, segue o formato de:

`task<generic return type>(async code to execute).then((parameters for lambda){ lambda code contents });`.

As tarefas podem ser encadeadas juntas usando a sintaxe **.then()**, portanto, quando uma operação é concluída, outra operação assíncrona que dependa dos resultados da operação anterior pode ser executada. Dessa forma, é possível carregar, converter e gerenciar ativos complexos em threads separados, de maneira que pareçam quase invisíveis para o jogador.

Para obter mais informações, consulte [Programação assíncrona em C++](https://docs.microsoft.com/windows/uwp/threading-async/asynchronous-programming-in-cpp-universal-windows-platform-apps).

Agora, vamos examinar a estrutura básica de declaração e criação de um método de carregamento de arquivo assíncrono, **ReadDataAsync**.

```cpp
#include <ppltasks.h>

// ...
concurrency::task<Platform::Array<byte>^> ReadDataAsync(
        _In_ Platform::String^ filename);

// ...

using concurrency;

task<Platform::Array<byte>^> BasicReaderWriter::ReadDataAsync(
    _In_ Platform::String^ filename
    )
{
    return task<StorageFile^>(m_location->GetFileAsync(filename)).then([=](StorageFile^ file)
    {
        return FileIO::ReadBufferAsync(file);
    }).then([=](IBuffer^ buffer)
    {
        auto fileData = ref new Platform::Array<byte>(buffer->Length);
        DataReader::FromBuffer(buffer)->ReadBytes(fileData);
        return fileData;
    });
}
```

Nesse código, quando o seu código chama o método **ReadDataAsync** definido acima, uma tarefa é criada para ler um buffer no sistema de arquivos. Concluída essa etapa, uma tarefa encadeada obtém o buffer e distribui o fluxo de bytes desse buffer em uma matriz, usando o tipo [**DataReader**](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams.DataReader) estático.

```cpp
m_basicReaderWriter = ref new BasicReaderWriter();

// ...
return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
      // Perform some operation with the data when the async load completes.          
    });
```

Esta é a chamada que você faz para **ReadDataAsync**. Depois de concluída, o seu código recebe uma matriz dos bytes lidos no arquivo fornecido. Como o próprio **ReadDataAsync** é definido como uma tarefa, é possível usar um lambda para executar uma operação específica quando a matriz de bytes é retornada; por exemplo, passar esses dados de bytes para uma função DirectX que possa usá-los.

Se o jogo for suficientemente simples, use um método como esse para carregar os recursos quando o usuário iniciar o jogo. Faça isso antes de iniciar o loop de jogo principal a partir de algum ponto na sequência de chamada da sua implementação [**IFrameworkView::Run**](https://docs.microsoft.com/uwp/api/windows.applicationmodel.core.iframeworkview.run). Novamente, você chama os métodos de carregamento de recursos de maneira assíncrona para que o jogo possa ser iniciado com mais rapidez e para que o jogador não precise esperar a conclusão do carregamento para, então, se envolver nas primeiras interações.

Contudo, você quer que o jogo comece somente quando todo o carregamento assíncrono estiver concluído. Crie algum método de sinalização para a conclusão do carregamento (por exemplo, um campo específico) e use o lambda no(s) método(s) de carregamento para definir esse sinal, quando finalizado. Verifique a variável antes de iniciar qualquer componente que utilize os recursos carregados.

Aqui está um exemplo que usa os métodos assíncronos definidos em BasicLoader.cpp para carregar sombreadores, uma malha e uma textura já na inicialização do jogo. Observe que ele define um campo específico no objeto Game, **m \_ loadingComplete**, quando todos os métodos de carregamento são concluídos.

```cpp
void ResourceLoading::CreateDeviceResources()
{
    // DirectXBase is a common sample class that implements a basic view provider. 
    
    DirectXBase::CreateDeviceResources(); 

    // ...

    // This flag will keep track of whether or not all application
    // resources have been loaded.  Until all resources are loaded,
    // only the sample overlay will be drawn on the screen.
    m_loadingComplete = false;

    // Create a BasicLoader, and use it to asynchronously load all
    // application resources.  When an output value becomes non-null,
    // this indicates that the asynchronous operation has completed.
    BasicLoader^ loader = ref new BasicLoader(m_d3dDevice.Get());

    auto loadVertexShaderTask = loader->LoadShaderAsync(
        "SimpleVertexShader.cso",
        nullptr,
        0,
        &m_vertexShader,
        &m_inputLayout
        );

    auto loadPixelShaderTask = loader->LoadShaderAsync(
        "SimplePixelShader.cso",
        &m_pixelShader
        );

    auto loadTextureTask = loader->LoadTextureAsync(
        "reftexture.dds",
        nullptr,
        &m_textureSRV
        );

    auto loadMeshTask = loader->LoadMeshAsync(
        "refmesh.vbo",
        &m_vertexBuffer,
        &m_indexBuffer,
        nullptr,
        &m_indexCount
        );

    // The && operator can be used to create a single task that represents
    // a group of multiple tasks. The new task's completed handler will only
    // be called once all associated tasks have completed. In this case, the
    // new task represents a task to load various assets from the package.
    (loadVertexShaderTask && loadPixelShaderTask && loadTextureTask && loadMeshTask).then([=]()
    {
        m_loadingComplete = true;
    });

    // Create constant buffers and other graphics device-specific resources here.
}
```

Observe também que as tarefas foram agregadas usando o operador &&, de modo que o lambda que define o sinalizador de conclusão do carregamento é acionado apenas quando todas as tarefas são concluídas. Lembre-se: se houver vários sinalizadores, poderão ocorrer condições de corrida. Por exemplo, se o lambda definir dois sinalizadores sequencialmente para o mesmo valor, outro thread poderá ver apenas o primeiro sinalizador, caso ele o examine antes de o segundo finalizador ser definido.

Você viu como carregar arquivos de recursos de maneira assíncrona. Os carregamentos síncronos de arquivos são muito mais simples, e você pode encontrar exemplos em [Concluir código para BasicReaderWriter](complete-code-for-basicreaderwriter.md) e em [Concluir código para BasicLoader](complete-code-for-basicloader.md).

Claro!, diferentes tipos de recursos e ativos geralmente exigem processamento adicional ou conversão antes de estarem prontos para uso no seu pipeline gráfico. Vamos examinar três tipos específicos de recursos: malhas, texturas e sombreadores.

### <a name="loading-meshes"></a>Carregando malhas

Malhas são dados de vértice, gerados via procedimentos pelo código no jogo ou exportados de um arquivo de outro aplicativo (como 3DStudio MAX ou Alias WaveFront) ou ferramenta. Essas malhas representam os modelos do jogo, desde primitivos simples, como cubos e esferas, até carros e casas e caracteres. Elas geralmente contêm dados de cor e animação, dependendo do formato. Vamos nos concentrar nas malhas que contêm apenas dados de vértice.

Para carregar corretamente uma malha, é preciso conhecer o formato dos dados no arquivo da malha. Nosso tipo simples **BasicReaderWriter**, acima, lê os dados como um fluxo de bytes; ele não sabe que os dados de bytes representam uma malha, muito menos que um formato de malha específico foi exportado por outro aplicativo. Você precisa executar a conversão quando traz os dados de malha para a memória.

(Tente sempre empacotar dados de ativos em um formato o mais próximo possível da representação interna. Fazendo assim, você reduzirá a utilização de recursos e economizará tempo.)

Vamos extrair os dados de bytes do arquivo da malha. O formato do exemplo pressupõe que o arquivo está em um formato específico do exemplo, com sufixo .vbo. (Novamente, esse formato não é igual ao formato VBO do OpenGL.) Cada vértice em si é mapeado para o tipo **BasicVertex**, que é uma estrutura definida no código para a ferramenta de conversão obj2vbo. O layout dos dados de vértice no arquivo .vbo se parece com o seguinte:

-   Os primeiros 32 bits (4 bytes) do fluxo de dados contêm o número de vértices (numVertices) da malha, representado como um valor uint32.
-   Os primeiros 32 bits (4 bytes) do fluxo de dados contêm o número de índices (numIndices) da malha, representado como um valor uint32.
-   Depois disso, os bits subsequentes (numVertices \* sizeof (**BasicVertex**)) contêm os dados de vértice.
-   Os últimos (numIndices \* 16) bits de dados contêm os dados de índice, representados como uma sequência de valores UInt16.

O ponto é este: saber o layout no nível de bit dos dados de malha carregados. Além disso, verifique se há consistência com endian-ness. Todas as plataformas Windows 8 são little-endian.

No exemplo, você chama um método CreateMesh no método **LoadMeshAsync** para executar essa interpretação no nível do bit.

```cpp
task<void> BasicLoader::LoadMeshAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ meshData)
    {
        CreateMesh(
            meshData->Data,
            vertexBuffer,
            indexBuffer,
            vertexCount,
            indexCount,
            filename
            );
    });
}
```

**Createmesh** interpreta os dados de byte carregados do arquivo e cria um buffer de vértice e um buffer de índice para a malha, passando as listas de vértices e de índice, respectivamente, para [**ID3D11Device:: CreateBuffer**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createbuffer) e especificando o \_ buffer de vértice de associação D3D11 ou o buffer de índice de associação de \_ \_ D3D11 \_ \_ \_ . Aqui está o código usado em **BasicLoader**:

```cpp
void BasicLoader::CreateMesh(
    _In_ byte* meshData,
    _Out_ ID3D11Buffer** vertexBuffer,
    _Out_ ID3D11Buffer** indexBuffer,
    _Out_opt_ uint32* vertexCount,
    _Out_opt_ uint32* indexCount,
    _In_opt_ Platform::String^ debugName
    )
{
    // The first 4 bytes of the BasicMesh format define the number of vertices in the mesh.
    uint32 numVertices = *reinterpret_cast<uint32*>(meshData);

    // The following 4 bytes define the number of indices in the mesh.
    uint32 numIndices = *reinterpret_cast<uint32*>(meshData + sizeof(uint32));

    // The next segment of the BasicMesh format contains the vertices of the mesh.
    BasicVertex* vertices = reinterpret_cast<BasicVertex*>(meshData + sizeof(uint32) * 2);

    // The last segment of the BasicMesh format contains the indices of the mesh.
    uint16* indices = reinterpret_cast<uint16*>(meshData + sizeof(uint32) * 2 + sizeof(BasicVertex) * numVertices);

    // Create the vertex and index buffers with the mesh data.

    D3D11_SUBRESOURCE_DATA vertexBufferData = {0};
    vertexBufferData.pSysMem = vertices;
    vertexBufferData.SysMemPitch = 0;
    vertexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC vertexBufferDesc(numVertices * sizeof(BasicVertex), D3D11_BIND_VERTEX_BUFFER);

    m_d3dDevice->CreateBuffer(
            &vertexBufferDesc,
            &vertexBufferData,
            vertexBuffer
            );
    
    D3D11_SUBRESOURCE_DATA indexBufferData = {0};
    indexBufferData.pSysMem = indices;
    indexBufferData.SysMemPitch = 0;
    indexBufferData.SysMemSlicePitch = 0;
    CD3D11_BUFFER_DESC indexBufferDesc(numIndices * sizeof(uint16), D3D11_BIND_INDEX_BUFFER);
    
    m_d3dDevice->CreateBuffer(
            &indexBufferDesc,
            &indexBufferData,
            indexBuffer
            );
  
    if (vertexCount != nullptr)
    {
        *vertexCount = numVertices;
    }
    if (indexCount != nullptr)
    {
        *indexCount = numIndices;
    }
}
```

Em geral, você cria um par de buffers de vértices/índices para cada malha usada no jogo. Você é quem decide onde e quando carregar as malhas. Quando há muitas malhas, pode ser conveniente carregar apenas algumas delas no disco, em pontos específicos do jogo; por exemplo, durante estados de carregamento específicos e predefinidos. Para malhas grandes, como dados de terreno, você pode distribuir o fluxo de vértices de um cache, mas este é um procedimento mais complexo e não faz parte do escopo deste tópico.

Enfatizando mais uma vez: conheça o formato dos seus dados de vértice. Há inúmeras formas de representar dados de vértice nas ferramentas usadas para criar modelos. Também há muitas maneiras diferentes de representar o layout de entrada dos dados de vértice para Direct3D; por exemplo, listas de triângulos e faixas. Para saber mais sobre dados de vértice, leia [Introdução aos buffers em Direct3D 11](https://docs.microsoft.com/windows/desktop/direct3d11/overviews-direct3d-11-resources-buffers-intro) e [Primitivos](https://docs.microsoft.com/windows/desktop/direct3d9/primitives).

A seguir, vamos examinar o carregamento de texturas.

### <a name="loading-textures"></a>Carregar texturas

O ativo mais comum de um jogo, e aquele que abrange a maioria dos arquivos em disco e memória, são as texturas. Assim como as malhas, as texturas podem vir em vários formatos; então, você os converte em um formato que o Direct3D possa usar quando carregá-las. Texturas também têm uma grande variedade de tipos e são usadas para criar efeitos distintos. Os níveis MIP de texturas podem ser usados para melhorar a aparência e o desempenho de objetos de distância; mapas de entulho e luzes são usados para efeitos de camada e detalhes sobre a textura base; e mapas normais são usados em cálculos de iluminação por pixel. Em um jogo moderno, um cenário típico pode ter milhares de texturas individuais e o seu código deve efetivamente gerenciar todas elas.

Também como nas malhas, há muitos formatos específicos que são usados para tornar eficiente o uso da memória. Como as texturas podem consumir facilmente uma grande parte da memória GPU (e de sistema), elas são compactadas de alguma forma. Não há necessidade de usar compactação nas texturas do seu jogo, e você pode usar qualquer algoritmo de compactação/descompactação que quiser, desde que forneça os sombreadores Direct3D com dados em um formato que ele possa entender (como um bitmap [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d)).

O Direct3D dá suporte a algoritmos de compactação de textura DXT, embora nem todo formato DXT dê suporte ao hardware gráfico do jogador. Arquivos DDS contêm texturas DXT (e também outros formatos de compactação de textura) e usam o sufixo .dds.

Um arquivo DDS é um arquivo binário que contém as seguintes informações:

-   Um DWORD (número mágico) contendo o valor de código com quatro caracteres 'DDS' (0x20534444).

-   Uma descrição dos dados no arquivo.

    Os dados são descritos com uma descrição de cabeçalho usando o [** \_ cabeçalho DDS**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-header); o formato de pixel é definido usando o [**DDS \_ PIXELFORMAT**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-pixelformat). Observe que as estruturas do ** \_ cabeçalho DDS** e do **DDS \_ PIXELFORMAT** substituem as estruturas preteridas DDSURFACEDESC2, DDSCAPS2 e DDPIXELFORMAT DirectDraw 7. **DDS \_ HEADER** é o equivalente binário de DDSURFACEDESC2 e DDSCAPS2. **DDS \_ PIXELFORMAT** é o equivalente binário de DDPIXELFORMAT.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    ```

    Se o valor de **dwFlags** em [**DDS \_ PIXELFORMAT**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-pixelformat) for definido como DDPF \_ FOURCC e **dwFourCC** for definido como "DX10", uma estrutura de [** \_ \_ DXT10 de cabeçalho DDS**](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-header-dxt10) adicional estará presente para acomodar matrizes de textura ou formatos dxgi que não podem ser expressos como um formato de pixel RGB, como formatos de ponto flutuante, formatos sRGB, etc. Quando a **estrutura \_ \_ DXT10 do cabeçalho do DDS** estiver presente, a descrição inteira dos dados será parecida com esta.

    ```cpp
    DWORD               dwMagic;
    DDS_HEADER          header;
    DDS_HEADER_DXT10    header10;
    ```

-   Um ponteiro para uma matriz de bytes que contém os dados da superfície principal.
    ```cpp
    BYTE bdata[]
    ```

-   Um ponteiro para uma matriz de bytes que contém as demais superfícies; por exemplo, níveis de minimapa, faces de um mapa de cubos, profundidades em uma textura de volume. Siga estes links para saber mais sobre o layout de arquivo DDS para: uma [textura](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-textures), um [mapa de cubos](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-cubic-environment-maps) ou uma [textura de volume](https://docs.microsoft.com/windows/desktop/direct3ddds/dds-file-layout-for-volume-textures).

    ```cpp
    BYTE bdata2[]
    ```

Muitas ferramentas exportam o formato DDS. Se não houver uma ferramenta para exportar sua textura para esse formato, considere criá-la. Para saber mais sobre o formato DDS e como trabalhar com ele no seu código, leia [Guia de Programação para DDS](https://docs.microsoft.com/windows/desktop/direct3ddds/dx-graphics-dds-pguide). No nosso exemplo, usaremos DDS.

Assim como em outros tipos de recursos, você lê os dados em um arquivo como um fluxo de bytes. Após a conclusão da tarefa de carregamento, a chamada de lambda executa o código (o método **CreateTexture**) para processar o fluxo de bytes em um formato que o Direct3D possa usar.

```cpp
task<void> BasicLoader::LoadTextureAsync(
    _In_ Platform::String^ filename,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ textureData)
    {
        CreateTexture(
            GetExtension(filename) == "dds",
            textureData->Data,
            textureData->Length,
            texture,
            textureView,
            filename
            );
    });
}
```

No trecho anterior,  o lambda faz verificações para ver se o nome do arquivo tem uma extensão "dds". Se tiver, isso significará que é uma textura DDS. Se não, use as APIs WIC (Windows Imaging Component) para descobrir o formato e decodificar os dados como um bitmap. De qualquer forma, o resultado será um bitmap [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) (ou um erro).

```cpp
void BasicLoader::CreateTexture(
    _In_ bool decodeAsDDS,
    _In_reads_bytes_(dataSize) byte* data,
    _In_ uint32 dataSize,
    _Out_opt_ ID3D11Texture2D** texture,
    _Out_opt_ ID3D11ShaderResourceView** textureView,
    _In_opt_ Platform::String^ debugName
    )
{
    ComPtr<ID3D11ShaderResourceView> shaderResourceView;
    ComPtr<ID3D11Texture2D> texture2D;

    if (decodeAsDDS)
    {
        ComPtr<ID3D11Resource> resource;

        if (textureView == nullptr)
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                nullptr
                );
        }
        else
        {
            CreateDDSTextureFromMemory(
                m_d3dDevice.Get(),
                data,
                dataSize,
                &resource,
                &shaderResourceView
                );
        }

        resource.As(&texture2D);
    }
    else
    {
        if (m_wicFactory.Get() == nullptr)
        {
            // A WIC factory object is required in order to load texture
            // assets stored in non-DDS formats.  If BasicLoader was not
            // initialized with one, create one as needed.
            CoCreateInstance(
                    CLSID_WICImagingFactory,
                    nullptr,
                    CLSCTX_INPROC_SERVER,
                    IID_PPV_ARGS(&m_wicFactory));
        }

        ComPtr<IWICStream> stream;
        m_wicFactory->CreateStream(&stream);

        stream->InitializeFromMemory(
                data,
                dataSize);

        ComPtr<IWICBitmapDecoder> bitmapDecoder;
        m_wicFactory->CreateDecoderFromStream(
                stream.Get(),
                nullptr,
                WICDecodeMetadataCacheOnDemand,
                &bitmapDecoder);

        ComPtr<IWICBitmapFrameDecode> bitmapFrame;
        bitmapDecoder->GetFrame(0, &bitmapFrame);

        ComPtr<IWICFormatConverter> formatConverter;
        m_wicFactory->CreateFormatConverter(&formatConverter);

        formatConverter->Initialize(
                bitmapFrame.Get(),
                GUID_WICPixelFormat32bppPBGRA,
                WICBitmapDitherTypeNone,
                nullptr,
                0.0,
                WICBitmapPaletteTypeCustom);

        uint32 width;
        uint32 height;
        bitmapFrame->GetSize(&width, &height);

        std::unique_ptr<byte[]> bitmapPixels(new byte[width * height * 4]);
        formatConverter->CopyPixels(
                nullptr,
                width * 4,
                width * height * 4,
                bitmapPixels.get());

        D3D11_SUBRESOURCE_DATA initialData;
        ZeroMemory(&initialData, sizeof(initialData));
        initialData.pSysMem = bitmapPixels.get();
        initialData.SysMemPitch = width * 4;
        initialData.SysMemSlicePitch = 0;

        CD3D11_TEXTURE2D_DESC textureDesc(
            DXGI_FORMAT_B8G8R8A8_UNORM,
            width,
            height,
            1,
            1
            );

        m_d3dDevice->CreateTexture2D(
                &textureDesc,
                &initialData,
                &texture2D);

        if (textureView != nullptr)
        {
            CD3D11_SHADER_RESOURCE_VIEW_DESC shaderResourceViewDesc(
                texture2D.Get(),
                D3D11_SRV_DIMENSION_TEXTURE2D
                );

            m_d3dDevice->CreateShaderResourceView(
                    texture2D.Get(),
                    &shaderResourceViewDesc,
                    &shaderResourceView);
        }
    }


    if (texture != nullptr)
    {
        *texture = texture2D.Detach();
    }
    if (textureView != nullptr)
    {
        *textureView = shaderResourceView.Detach();
    }
}
```

Após a conclusão do código, a [**Texture2D**](https://docs.microsoft.com/windows/desktop/api/d3d11/nn-d3d11-id3d11texture2d) estará na memória, carregada de um arquivo de imagem. Assim como nas malhas, provavelmente haverá muitas delas no jogo e em determinados cenários. Considere a criação de caches para texturas acessadas com regularidade, por cenário ou por nível, em vez de carregá-las todas quando o jogo ou o nível é iniciado.

(O método **CreateDDSTextureFromMemory** chamado no exemplo acima pode ser explorado totalmente em [Concluir código para DDSTextureLoader](complete-code-for-ddstextureloader.md).)

Além disso, cada textura ou as "aparências" de textura podem ser mapeadas para polígonos de malha ou superfícies específicas. Esses dados de mapeamento geralmente são exportados pela ferramenta, artista ou designer utilizado para criar o modelo e as texturas. Lembre-se de capturar também essas informações ao carregar os dados exportados, pois você as usará para mapear as texturas corretas para as superfícies correspondentes, quando executar o sombreamento de fragmento.

### <a name="loading-shaders"></a>Carregando sombreadores

Sombreadores são arquivos HLSL (High Level Shader Language), que são carregados na memória e invocados em estágios específicos do pipeline gráfico. Os sombreadores mais comuns e essenciais são os sombreadores de vértices e pixels, os quais processam cada vértice da malha e os pixels do(s) visor(es) do cenário, respectivamente. O código HLSL é executado para transformar a geometria, aplicar efeitos de luz e texturas e para executar pós-processamento no cenário renderizado.

Um jogo em Direct3D pode ter vários sombreadores diferentes, cada um deles compilado em um arquivo CSO (Compiled Shader Object, .cso) separado. Normalmente, não há tantos sombreadores que se torne necessário carregá-los dinamicamente e, na maioria das vezes, basta carregá-los quando o jogo for iniciado ou dependendo do nível (por exemplo, um sombreador para efeitos de chuva).

O código da classe **BasicLoader** fornece algumas sobrecargas para diferentes sombreadores, incluindo sombreadores de vértice, geometria, pixel e envoltório. O código a seguir abrange sombreadores de pixel como exemplo. (Você pode revisar o código completo em [Concluir código para BasicLoader](complete-code-for-basicloader.md).)

```cpp
concurrency::task<void> LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _Out_ ID3D11PixelShader** shader
    )
{
    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
        
       m_d3dDevice->CreatePixelShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);
    });
}

```

Neste exemplo, você usa a instância **BasicReaderWriter** (**m \_ BasicReaderWriter**) para ler o arquivo de objeto do sombreador compilado fornecido (. CSO) como um fluxo de bytes. Após a conclusão da tarefa, o lambda chama [**ID3D11Device::CreatePixelShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createpixelshader) com os dados de bytes carregados do arquivo. O retorno de chamada deve definir algum sinalizador indicando que houve êxito no carregamento e o seu código deve verificar esse sinalizador antes de executar o sombreador.

Sombreadores de vértice são um pouco mais complexos. Para um sombreador de vértice, você também carrega um layout de entrada separado, que define os dados de vértice. O código a seguir pode ser usado para carregar de modo assíncrono um sombreador de vértice juntamente com um layout de entrada de vértice personalizado. Verifique se as informações de vértice carregadas das malhas podem ser representadas corretamente por esse layout de entrada.

Vamos criar o layout de entrada antes de você carregar o sombreador de vértice.

```cpp
void BasicLoader::CreateInputLayout(
    _In_reads_bytes_(bytecodeSize) byte* bytecode,
    _In_ uint32 bytecodeSize,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC* layoutDesc,
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11InputLayout** layout
    )
{
    if (layoutDesc == nullptr)
    {
        // If no input layout is specified, use the BasicVertex layout.
        const D3D11_INPUT_ELEMENT_DESC basicVertexLayoutDesc[] =
        {
            { "POSITION", 0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 0,  D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "NORMAL",   0, DXGI_FORMAT_R32G32B32_FLOAT, 0, 12, D3D11_INPUT_PER_VERTEX_DATA, 0 },
            { "TEXCOORD", 0, DXGI_FORMAT_R32G32_FLOAT,    0, 24, D3D11_INPUT_PER_VERTEX_DATA, 0 },
        };

        m_d3dDevice->CreateInputLayout(
                basicVertexLayoutDesc,
                ARRAYSIZE(basicVertexLayoutDesc),
                bytecode,
                bytecodeSize,
                layout);
    }
    else
    {
        m_d3dDevice->CreateInputLayout(
                layoutDesc,
                layoutDescNumElements,
                bytecode,
                bytecodeSize,
                layout);
    }
}
```

Nesse layout em particular, os seguintes dados de cada vértice são processados pelo sombreador de vértice:

-   Uma posição de coordenada 3D (x, y, z) no espaço de coordenada do modelo, representada como um trio de valores de ponto flutuante de 32 bits.
-   Um vetor normal do vértice, também representado como três valores de ponto flutuante de 32 bits.
-   Um valor de coordenada de textura 2D (u, v), representado como um par de valores flutuantes de 32 bits.

Esses elementos de entrada por vértice são chamados de [semântica HLSL](https://docs.microsoft.com/windows/desktop/direct3dhlsl/dx-graphics-hlsl-semantics) e compõem um conjunto de registradores definidos, usados para passar dados para e do objeto de sombreador compilado. O pipeline executa o sombreador de vértice uma vez para cada vértice da malha carregada. A semântica define a entrada no (e a saída do) sombreador de vértice à medida que é executada e fornece esses dados para os cálculos por vértice do código HLSL do seu sombreador.

Agora, carregue o objeto de sombreador de vértice.

```cpp
concurrency::task<void> LoadShaderAsync(
        _In_ Platform::String^ filename,
        _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
        _In_ uint32 layoutDescNumElements,
        _Out_ ID3D11VertexShader** shader,
        _Out_opt_ ID3D11InputLayout** layout
        );

// ...

task<void> BasicLoader::LoadShaderAsync(
    _In_ Platform::String^ filename,
    _In_reads_opt_(layoutDescNumElements) D3D11_INPUT_ELEMENT_DESC layoutDesc[],
    _In_ uint32 layoutDescNumElements,
    _Out_ ID3D11VertexShader** shader,
    _Out_opt_ ID3D11InputLayout** layout
    )
{
    // This method assumes that the lifetime of input arguments may be shorter
    // than the duration of this task.  In order to ensure accurate results, a
    // copy of all arguments passed by pointer must be made.  The method then
    // ensures that the lifetime of the copied data exceeds that of the task.

    // Create copies of the layoutDesc array as well as the SemanticName strings,
    // both of which are pointers to data whose lifetimes may be shorter than that
    // of this method's task.
    shared_ptr<vector<D3D11_INPUT_ELEMENT_DESC>> layoutDescCopy;
    shared_ptr<vector<string>> layoutDescSemanticNamesCopy;
    if (layoutDesc != nullptr)
    {
        layoutDescCopy.reset(
            new vector<D3D11_INPUT_ELEMENT_DESC>(
                layoutDesc,
                layoutDesc + layoutDescNumElements
                )
            );

        layoutDescSemanticNamesCopy.reset(
            new vector<string>(layoutDescNumElements)
            );

        for (uint32 i = 0; i < layoutDescNumElements; i++)
        {
            layoutDescSemanticNamesCopy->at(i).assign(layoutDesc[i].SemanticName);
        }
    }

    return m_basicReaderWriter->ReadDataAsync(filename).then([=](const Platform::Array<byte>^ bytecode)
    {
       m_d3dDevice->CreateVertexShader(
                bytecode->Data,
                bytecode->Length,
                nullptr,
                shader);

        if (layout != nullptr)
        {
            if (layoutDesc != nullptr)
            {
                // Reassign the SemanticName elements of the layoutDesc array copy to point
                // to the corresponding copied strings. Performing the assignment inside the
                // lambda body ensures that the lambda will take a reference to the shared_ptr
                // that holds the data.  This will guarantee that the data is still valid when
                // CreateInputLayout is called.
                for (uint32 i = 0; i < layoutDescNumElements; i++)
                {
                    layoutDescCopy->at(i).SemanticName = layoutDescSemanticNamesCopy->at(i).c_str();
                }
            }

            CreateInputLayout(
                bytecode->Data,
                bytecode->Length,
                layoutDesc == nullptr ? nullptr : layoutDescCopy->data(),
                layoutDescNumElements,
                layout);   
        }
    });
}
```

Nesse código, depois de ter lido os dados de bytes do arquivo CSO do sombreador de vértice, você cria o sombreador de vértice chamando [**ID3D11Device::CreateVertexShader**](https://docs.microsoft.com/windows/desktop/api/d3d11/nf-d3d11-id3d11device-createvertexshader). Depois disso, cria o layout de entrada do sombreador no mesmo lambda.

Outros tipos de sombreador, como sombreadores de envoltório e geometria, também podem exigir configuração específica. O código completo para vários métodos de carregamento de sombreador é fornecido em [Concluir código para BasicLoader](complete-code-for-basicloader.md) e no [exemplo de carregamento de recursos do Direct3D](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/Direct3D%20resource%20loading%20sample%20(Windows%208)/C%2B%2B).

## <a name="remarks"></a>Comentários

Nesse ponto, você deverá estar apto a entender e criar ou modificar métodos de carregamento assíncrono de recursos e ativos comuns de jogos, como malhas, texturas e sombreadores compilados.

## <a name="related-topics"></a>Tópicos relacionados

* [Exemplo de carregamento de recursos do Direct3D](https://github.com/microsoftarchive/msdn-code-gallery-microsoft/tree/master/Official%20Windows%20Platform%20Sample/Windows%208%20app%20samples/%5BC%2B%2B%5D-Windows%208%20app%20samples/C%2B%2B/Windows%208%20app%20samples/Direct3D%20resource%20loading%20sample%20(Windows%208)/C%2B%2B)
* [Concluir código para BasicLoader](complete-code-for-basicloader.md)
* [Concluir código para BasicReaderWriter](complete-code-for-basicreaderwriter.md)
* [Concluir código para DDSTextureLoader](complete-code-for-ddstextureloader.md)

 

 




