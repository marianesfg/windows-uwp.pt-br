---
author: mtoepke
title: Correlacionar recursos do DirectX 9 com APIs do DirectX 11
description: Compreenda como os recursos utilizados por seu jogo do Direct3D 9 serão convertidos para o Direct3D 11 e a UWP (Plataforma Universal do Windows).
ms.assetid: 3aa8a114-4e47-ae0a-9447-88ba324377b8
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, directx 9, directx 11, portabilidade
ms.localizationpriority: medium
ms.openlocfilehash: 8dcf1749f1e7db4d514466d6a753d6f8cace5713
ms.sourcegitcommit: 93c0a60cf531c7d9fe7b00e7cf78df86906f9d6e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/21/2018
ms.locfileid: "7567830"
---
# <a name="map-directx-9-features-to-directx-11-apis"></a>Correlacionar recursos do DirectX 9 com APIs do DirectX 11



**Resumo**

-   [Planejar a portabilidade do DirectX](plan-your-directx-port.md)
-   [Alterações importantes do Direct3D 9 para o Direct3D 11](understand-direct3d-11-1-concepts.md)
-   Mapeamento de recursos


Compreenda como os recursos utilizados por seu jogo do Direct3D 9 serão convertidos para o Direct3D 11 e a UWP (Plataforma Universal do Windows).

## <a name="mapping-direct3d-9-to-directx-11-apis"></a>Mapeamento do Direct3D 9 para APIs do DirectX 11


O [Direct3D](https://msdn.microsoft.com/library/windows/desktop/hh309466) ainda é a base dos elementos gráficos do DirectX, mas a API mudou a partir do DirectX 9:

-   A DXGI (infraestrutura gráfica do DirectX) da Microsoft é usada para configurar adaptadores gráficos. Use [DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534) para selecionar formatos de buffer, criar cadeias de troca, apresentar quadros e criar recursos compartilhados. Consulte a [visão geral de DXGI](https://msdn.microsoft.com/library/windows/desktop/bb205075).
-   Um contexto de dispositivo Direct3D é usado para definir o estado do pipeline e gerar comandos de renderização. A maioria das amostras usa um contexto imediato para realizar a renderização diretamente no dispositivo; o Direct3D 11 também oferece suporte a renderização multithreading, onde ocorre o uso de contextos adiados. Consulte a [introdução a um dispositivo no Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476880).
-   Alguns recursos foram preteridos, com destaque para o pipeline de funções fixas. Consulte [Recursos preteridos](https://msdn.microsoft.com/library/windows/desktop/cc308047).

Veja uma lista completa de recursos do Direct3D 11 em [Recursos do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476342) e [Recursos do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh404562).

## <a name="moving-from-direct2d-9-to-direct2d-11"></a>Mudando do Direct2D 9 para p Direct2D 11


O [Direct2D (Windows)](https://msdn.microsoft.com/library/windows/desktop/dd370990) ainda é uma parte importante dos elementos gráficos do DirectX e do Windows. Você ainda pode usar o Direct2D para desenhar jogos 2D e sobreposições (HUDs) sobre o Direct3D.

O Direct2D é executado sobre o Direct3D; os jogos 2D podem ser implementados usando qualquer uma das APIs. Por exemplo, um jogo 2D implementado usando Direct3D pode usar projeção ortográfica, definir valores de Z para controlar a ordem do desenho de primitivas e usar sombreadores de pixel para adicionar efeitos especiais.

Como o Direct2D é baseado no Direct3D, ele também usa DXGI e contextos de dispositivos. Consulte a [visão geral de APIs do Direct2D](https://msdn.microsoft.com/library/windows/desktop/dd317121).

A API [DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd368038) acrescenta o suporte a texto formatado usando Direct2D. Consulte a [introdução ao DirectWrite](https://msdn.microsoft.com/library/windows/desktop/dd371554).

## <a name="replace-deprecated-helper-libraries"></a>Substituir bibliotecas de ajuda preteridas


D3DX e DXUT foram preteridos e não podem ser usados por jogos UWP. Essas bibliotecas de ajuda forneciam recursos para tarefas como carregamento de texturas e malhas.

-   O guia passo a passo [Portabilidade simples do Direct3D 9 para a UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md) demonstra como configurar uma janela, inicializar o Direct3D e realizar renderização 3D básica.
-   O guia passo a passo sobre [Jogos UWP simples com o DirectX](tutorial--create-your-first-uwp-directx-game.md) demonstra tarefas comuns de programação de jogos incluindo elementos gráficos, carregamento de arquivos, interface do usuário, controles e som.
-   O projeto da comunidade [Kit de ferramentas do DirectX)](http://go.microsoft.com/fwlink/p/?LinkID=248929) oferece classes de ajuda para usar com aplicativos UWP e Direct3D 11.

## <a name="move-shader-programs-from-fx-to-hlsl"></a>Mover programas de sombreador do FX para HLSL


A biblioteca de utilitários do D3DX (D3DX 9, D3DX 10 e D3DX 11), incluindo efeitos, foi preterida para o UWP. Todos os jogos do DirectX para UWP conduzem o pipeline de elementos gráficos usando [HLSL](https://msdn.microsoft.com/library/windows/desktop/bb509561) sem efeitos.

O Visual Studio ainda usa FXC para compilar objetos de sombreador. Os sombreadores de jogos UWP são compilados antecipadamente. O código de bytes é carregado em tempo de execução. Em seguida, cada recurso de sombreador é associado ao pipeline dos elementos gráficos durante a passagem da renderização adequada. Os sombreadores devem ser movidos para arquivos .HLSL separados, e as técnicas de renderização devem ser implementadas no código C++.

Para ter uma ideia do carregamento de recursos de sombreador, consulte o tópico sobre [portabilidade simples do Direct3D 9 para UWP](walkthrough--simple-port-from-direct3d-9-to-11-1.md).

O Direct3D 11 introduziu o modelo de sombreador 5, que requer o nível de recursos do Direct3D 11\_0 (ou superior). Consulte o tópico sobre [Recursos do modelo de sombreador 5 do HLSL para Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff471419).

## <a name="replace-xnamath-and-d3dxmath"></a>Substituir XNAMath e D3DXMath


O código que utiliza XNAMath (ou D3DXMath) deve ser migrado para [DirectXMath](https://msdn.microsoft.com/library/windows/desktop/hh437833). O DirectXMath inclui tipos cuja portabilidade pode ser feita para x86, x64 e ARM. Consulte o tópico sobre [migração de código da biblioteca  de matemática XNA](https://msdn.microsoft.com/library/windows/desktop/ee418730).

Observe que os tipos float do DirectXMath são convenientes para uso com sombreadores. Por exemplo, [**XMFLOAT4**](https://msdn.microsoft.com/library/windows/desktop/ee419608) e [**XMFLOAT4X4**](https://msdn.microsoft.com/library/windows/desktop/ee419621) alinham dados para buffers constantes de modo conveniente.

## <a name="replace-directsound-with-xaudio2-and-background-audio"></a>Substituir DirectSound por XAudio2 (e áudio de fundo)


Não há suporte para DirectSound na UWP:

-   Use [XAudio2](https://msdn.microsoft.com/library/windows/desktop/hh405049) para adicionar efeitos sonoros ao jogo.

##  <a name="replace-directinput-with-xinput-and-uwp-apis"></a>Substituir DirectInput por XInput e APIs UWP


Não há suporte para DirectInput na UWP:

-   Use retornos de chamada do evento de entrada [**CoreWindow**](https://msdn.microsoft.com/library/windows/apps/br208225) para entrada por mouse, teclado e toque.
-   Use [XInput](https://msdn.microsoft.com/library/windows/desktop/ee417001) 1.4 para suporte ao controlador do jogo (e ao fone de ouvido do controlador do jogo). Caso esteja usando uma base de código compartilhada para a área de trabalho e para a UWP, consulte [Versões de XInput](https://msdn.microsoft.com/library/windows/desktop/hh405051) para saber mais sobre compatibilidade com versões anteriores.
-   Registre-se para eventos [**EdgeGesture**](https://msdn.microsoft.com/library/windows/apps/hh701600) se seu jogo precisar usar a barra de aplicativo.

## <a name="use-microsoft-media-foundation-instead-of-directshow"></a>Usar a Microsoft Media Foundation em vez do DirectShow


O DirectShow não faz mais parte da API do DirectX (ou do Windows). O [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197) fornece conteúdo de vídeo para o Direct3D usando superfícies compartilhadas. Consulte [APIs de vídeo do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/hh447677).

## <a name="replace-directplay-with-networking-code"></a>Substituir o DirectPlay por código de rede


O Microsoft DirectPlay foi preterido. Caso o jogo use serviços de rede, será necessário fornecer código de rede em conformidade com os requisitos de certificação da UWP. Use as seguintes APIs:

-   [Win32 e COM para aplicativos UWP (rede) (Windows)](https://msdn.microsoft.com/library/windows/apps/br205759)
-   [**Namespace Windows.Networking (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207124)
-   [**Namespace Windows.Networking.Sockets (Windows)**](https://msdn.microsoft.com/library/windows/apps/br226960)
-   [**Namespace Windows.Networking.Connectivity (Windows)**](https://msdn.microsoft.com/library/windows/apps/br207308)
-   [**Namespace Windows.ApplicationModel.Background (Windows)**](https://msdn.microsoft.com/library/windows/apps/br224847)

Os artigos a seguir ajudam a adicionar recursos de rede e a declarar suporte à rede no manifesto do pacote do aplicativo.

-   [Conectando-se com soquetes (aplicativos UWP usando C#/VB/C++ e XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh452976)
-   [Conectando-se com WebSockets (aplicativos UWP usando C#/VB/C++ e XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh994396)
-   [Conectando serviços Web (aplicativos UWP usando C#/VB/C++ e XAML) (Windows)](https://msdn.microsoft.com/library/windows/apps/xaml/hh761504)
-   [Noções básicas de rede](https://msdn.microsoft.com/library/windows/apps/mt280233)

Observe que todos os aplicativos UWP (incluindo jogos) usam tipos específicos de tarefas em segundo plano para manter a conectividade enquanto o aplicativo é suspenso. Caso o jogo precise manter o estado de conexão enquanto suspenso, consulte [Noções básicas de rede](https://msdn.microsoft.com/library/windows/apps/mt280233).

## <a name="function-mapping"></a>Mapeamento de funções


Use a tabela a seguir para converter o código do Direct3D 9 em Direct3D 11. Isso pode ajudar a distinguir entre o dispositivo e seu contexto.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Direct3D9</th>
<th align="left">Equivalente no Direct3D 11</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174336">IDirect3DDevice9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280493">ID3D11Device2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280498">ID3D11DeviceContext2</a></p>
<p>Os estágios de pipeline gráfico são descritos no tópico sobre <a href="https://msdn.microsoft.com/library/windows/desktop/ff476882">pipeline gráfico</a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174300">IDirect3D9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404556">IDXGIFactory2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404537">IDXGIAdapter2</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/dn280345">IDXGIDevice3</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174423">IDirect3DDevice9::Present</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174472">IDirect3DDevice9::TestCooperativeLevel</a></p></td>
<td align="left"><p>Chame <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a> com o sinalizador definido DXGI_PRESENT_TEST.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174322">IDirect3DBaseTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205909">IDirect3DTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174329">IDirect3DCubeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205941">IDirect3DVolumeTexture9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205865">IDirect3DIndexBuffer9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205915">IDirect3DVertexBuffer9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476351">ID3D11Buffer</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476633">ID3D11Texture1D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476635">ID3D11Texture2D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476637">ID3D11Texture3D</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476628">ID3D11ShaderResourceView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476582">ID3D11RenderTargetView</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476377">ID3D11DepthStencilView</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205922">IDirect3DVertexShader9</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205869">IDirect3DPixelShader9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476641">ID3D11VertexShader</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476576">ID3D11PixelShader</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205919">IDirect3DVertexDeclaration9</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476575">ID3D11InputLayout</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205805">IDirect3DDevice9::SetRenderState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205806">IDirect3DDevice9::SetSamplerState</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/hh404571">ID3D11BlendState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476375">ID3D11DepthStencilState</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/hh446828">ID3D11RasterizerState1</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476588">ID3D11SamplerState</a></p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174369">IDirect3DDevice9::DrawIndexedPrimitive</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174371">IDirect3DDevice9::DrawPrimitive</a></p></td>
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476407">ID3D11DeviceContext::Draw</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/ff476409">ID3D11DeviceContext::DrawIndexed</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173566">ID3D11DeviceContext::DrawIndexedInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173567">ID3D11DeviceContext::DrawInstanced</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173590">ID3D11DeviceContext::IASetPrimitiveTopology</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb173564">ID3D11DeviceContext::DrawAuto</a></p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174350">IDirect3DDevice9::BeginScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174375">IDirect3DDevice9::EndScene</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174372">IDirect3DDevice9::DrawPrimitiveUP</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174370">IDirect3DDevice9::DrawIndexedPrimitiveUP</a></p></td>
<td align="left"><p>Sem equivalente direto</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174470">IDirect3DDevice9::ShowCursor</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174429">IDirect3DDevice9::SetCursorPosition</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174430">IDirect3DDevice9::SetCursorProperties</a></p></td>
<td align="left"><p>Use as APIs de cursor padrão.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174425">IDirect3DDevice9::Reset</a></p></td>
<td align="left"><p>Dispositivo LOST e POOL_MANAGED não existem mais. <a href="https://msdn.microsoft.com/library/windows/desktop/hh446797">IDXGISwapChain1::Present1</a> pode falhar com um valor de retorno <a href="https://msdn.microsoft.com/library/windows/desktop/bb509553">DXGI_ERROR_DEVICE_REMOVED</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174373">IDirect3DDevice9:DrawRectPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174374">IDirect3DDevice9:DrawTriPatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174421">IDirect3DDevice9:LightEnable</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174422">IDirect3DDevice9:MultiplyTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205798">IDirect3DDevice9:SetLight</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174437">IDirect3DDevice9:SetMaterial</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174438">IDirect3DDevice9:SetNPatchMode</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174463">IDirect3DDevice9:SetTransform</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174433">IDirect3DDevice9:SetFVF</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174462">IDirect3DDevice9:SetTextureStageState</a></p></td>
<td align="left"><p>O pipeline de funções fixas foi preterido.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174308">IDirect3DDevice9:CheckDepthStencilMatch</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174309">IDirect3DDevice9:CheckDeviceFormat</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb174320">IDirect3DDevice9:GetDeviceCaps</a></p>
<p><a href="https://msdn.microsoft.com/library/windows/desktop/bb205859">IDirect3DDevice9:ValidateDevice</a></p></td>
<td align="left"><p>Os bits de recursos são substituídos por níveis de recursos. Somente alguns casos de uso de formatos e recursos são opcionais para qualquer nível de recursos. Eles podem ser verificados com <a href="https://msdn.microsoft.com/library/windows/desktop/ff476497">ID3D11Device::CheckFeatureSupport</a> e <a href="https://msdn.microsoft.com/library/windows/desktop/bb173536">ID3D11Device::CheckFormatSupport</a>.</p></td>
</tr>
</tbody>
</table>

 

## <a name="surface-format-mapping"></a>Mapeamento do formato da superfície


Use a tabela a seguir para converter formatos do Direct3D 9 em formatos DXGI.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Formato do Direct3D 9</th>
<th align="left">Formato do Direct3D 11</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>D3DFMT_UNKNOWN</p></td>
<td align="left"><p>DXGI_FORMAT_UNKNOWN</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8B8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8A8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8A8_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8R8G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_B8G8R8X8_UNORM</p>
<p>DXGI_FORMAT_B8G8R8X8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R5G6B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G6R5_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X1R5G5B5</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A1R5G5B5</p></td>
<td align="left"><p>DXGI_FORMAT_B5G5R5A1_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A4R4G4B4</p></td>
<td align="left"><p>DXGI_FORMAT_B4G4R4A4_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R3G3B2</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8</p></td>
<td align="left"><p>DXGI_FORMAT_A8_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8R3G3B2</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4R4G4B4</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2B10G10R10</p></td>
<td align="left"><p>DXGI_FORMAT_R10G10B10A2</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8B8G8R8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p>
<p>DXGI_FORMAT_R8G8B8A8_UNORM_SRGB</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8B8G8R8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A2R10G10B10</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A8P8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_P8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8_UNORM</p>
<div class="alert">
<strong>Observação</strong>  Use swizzling r no sombreador para duplicar o vermelho em outros componentes para obter o comportamento do Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A8L8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_UNORM</p>
<div class="alert">
<strong>Observação</strong>  Use o swizzle rrrg no sombreador para duplicar o vermelho e mover o verde para os componentes alfa para obter o comportamento do Direct3D 9.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A4L4</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L6V5U5</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X8L8V8U8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_Q8W8V8U8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_W11V11U10</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A2W10V10U10</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_UYVY</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R8G8_B8G8</p></td>
<td align="left"><p>DXGI_FORMAT_G8R8_G8B8_UNORM</p>
<div class="alert">
<strong>Observação</strong>  no Direct3D 9 os dados foi dimensionado para 255.0, mas isso pode ser manipulado no sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_YUY2</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G8R8_G8B8</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8_B8G8_UNORM</p>
<div class="alert">
<strong>Observação</strong>  no Direct3D 9 os dados foi dimensionado para 255.0, mas isso pode ser manipulado no sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT1</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT2</p></td>
<td align="left"><p>DXGI_FORMAT_BC1_UNORM & DXGI_FORMAT_BC1_UNORM_SRGB</p>
<div class="alert">
<strong>Observação</strong>  DXT1 e DXT2 são iguais na perspectiva de API/hardware. A única diferença é quando alfa pré-multiplicado é usado, o que pode ser acompanhado por um aplicativo, sem a necessidade de um formato separado.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT3</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_DXT4</p></td>
<td align="left"><p>DXGI_FORMAT_BC2_UNORM & DXGI_FORMAT_BC2_UNORM_SRGB</p>
<div class="alert">
<strong>Observação</strong>  DXT3 e DXT4 são iguais na perspectiva de API/hardware. A única diferença é quando alfa pré-multiplicado é usado, o que pode ser acompanhado por um aplicativo, sem a necessidade de um formato separado.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_DXT5</p></td>
<td align="left"><p>DXGI_FORMAT_BC3_UNORM & DXGI_FORMAT_BC3_UNORM_SRGB</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16 & D3DFMT_D16_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D15S1</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24S8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24X8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D24X4S4</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D16</p></td>
<td align="left"><p>DXGI_FORMAT_D16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_D32F_LOCKABLE</p></td>
<td align="left"><p>DXGI_FORMAT_D32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_D24FS8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_S1D15</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_S8D24</p></td>
<td align="left"><p>DXGI_FORMAT_D24_UNORM_S8_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_X8D24</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_X4S4D24</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_L16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UNORM</p>
<div class="alert">
<strong>Observação</strong>  Use swizzling r no sombreador para duplicar o vermelho em outros componentes para obter o comportamento do D3D9.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_INDEX16</p></td>
<td align="left"><p>DXGI_FORMAT_R16_UINT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_INDEX32</p></td>
<td align="left"><p>DXGI_FORMAT_R32_UINT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_Q16W16V16U16</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_MULTI2_ARGB8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_A16B16G16R16F</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DFMT_A32B32G32R32F</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DFMT_CxV8U8</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT1</p></td>
<td align="left"><p>DXGI_FORMAT_R32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT2</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT3</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT4</p></td>
<td align="left"><p>DXGI_FORMAT_R32G32B32A32_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPED3DCOLOR</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UBYTE4</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UINT</p>
<div class="alert">
<strong>Observação</strong>  o sombreador obtém valores UINT, mas se estilo integrante do Direct3D 9 sejam necessários floats no (0, 0F, 1, 0F... 255. f), UINT poderá ser convertido em float32 no sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SINT</p>
<div class="alert">
<strong>Observação</strong>  o sombreador obtém valores SINT, mas caso sejam necessários floats integrais no estilo do Direct3D 9, SINT poderá ser convertido em float32 no sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SINT</p>
<div class="alert">
<strong>Observação</strong>  o sombreador obtém valores SINT, mas caso sejam necessários floats integrais no estilo do Direct3D 9, SINT poderá ser convertido em float32 no sombreador.
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_UBYTE4N</p></td>
<td align="left"><p>DXGI_FORMAT_R8G8B8A8_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_SHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_SNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_SHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_SNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_USHORT2N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_UNORM</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_USHORT4N</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_UNORM</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_UDEC3</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_DEC3N</p></td>
<td align="left"><p>Indisponível</p></td>
</tr>
<tr class="even">
<td align="left"><p>D3DDECLTYPE_FLOAT16_2</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16_FLOAT</p></td>
</tr>
<tr class="odd">
<td align="left"><p>D3DDECLTYPE_FLOAT16_4</p></td>
<td align="left"><p>DXGI_FORMAT_R16G16B16A16_FLOAT</p></td>
</tr>
<tr class="even">
<td align="left"><p>FourCC 'ATI1'</p></td>
<td align="left"><p>DXGI_FORMAT_BC4_UNORM</p>
<div class="alert">
<strong>Observação</strong>  exige nível de recursos 10.0 ou posterior
</div>
<div>
 
</div></td>
</tr>
<tr class="odd">
<td align="left"><p>FourCC 'ATI2'</p></td>
<td align="left"><p>DXGI_FORMAT_BC5_UNORM</p>
<div class="alert">
<strong>Observação</strong>  exige nível de recursos 10.0 ou posterior
</div>
<div>
 
</div></td>
</tr>
</tbody>
</table>

 

 

 




