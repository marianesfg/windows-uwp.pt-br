---
author: mtoepke
title: Áudio para jogos
description: Aprenda como desenvolver e incorporar música e sons a seu jogo do DirectX e como processar sinais de áudio para criar sons dinâmicos e posicionais.
ms.assetid: ab29297a-9588-c79b-24c5-3b94b85e74a8
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp, jogos, áudio, directx
ms.localizationpriority: medium
ms.openlocfilehash: a0b0ae219ea7fd014b39eb8eb7a09049f7c632a2
ms.sourcegitcommit: ca96031debe1e76d4501621a7680079244ef1c60
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/30/2018
ms.locfileid: "5824132"
---
# <a name="audio-for-games"></a>Áudio para jogos



Aprenda como desenvolver e incorporar música e sons a seu jogo do DirectX e como processar sinais de áudio para criar sons dinâmicos e posicionais.

Para programação de áudio, recomendamos usar a biblioteca XAudio2 no DirectX, que é a usada aqui. XAudio2 é uma biblioteca de áudio de baixo nível que oferece uma base de processamento e mixagem de sinais para jogos e dá suporte a diversos formatos.

Você também pode implementar sons e simples e reprodução de músicas com a [Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197). A Microsoft Media Foundation foi desenvolvida para reprodução de arquivos e fluxos de mídia (de áudio e vídeo), mas também pode ser usada em jogos e é particularmente útil para cenas cinemáticas ou componentes sem interação de seu jogo.

## <a name="concepts-at-a-glance"></a>Conceitos básicos


Consulte alguns conceitos de programação de áudio que usamos nesta seção.

-   Os sinais são a unidade básica de programação de sons, análogos aos pixels nos elementos gráficos. Os processadores de sinais digitais (DSPs) que os processam são como os sombreadores de pixels do áudio do jogo. Eles podem transformar sinais, combiná-los ou filtrá-los. Programando os DSPs, você pode alterar os efeitos sonoros e a música de seu jogo com o nível de complexidade necessário, seja baixo ou alto.
-   As vozes são compostos submixados de dois ou mais sinais. Há três tipos de objetos de voz do XAudio2: vozes de origem, submixagem e masterização. As vozes de origem operam em dados de áudio fornecidos pelo cliente. As vozes de origem e submixagem enviam sua saída para uma ou mais vozes de submixagem ou masterização. As vozes de submixagem e masterização combinam o áudio de todas as vozes que as alimentam e operam no resultado. As vozes de masterização gravam dados de áudio em um dispositivo de áudio.
-   Mixagem é o processo de combinar várias vozes discretas, como os efeitos sonoros e o áudio de fundo que são reproduzidos em uma cena, em um fluxo único. Submixagem é o processo de combinar vários sinais discretos, como os sons que compõem o barulho de um motor, e criar uma voz.
-   Formatos de áudio As músicas e os feitos sonoros podem ser armazenados em vários formatos digitais para seu jogo. Há formatos não comprimidos, como WAV, e comprimidos, como MP3 e OGG. Quanto mais a amostra é comprimida (normalmente designada por sua taxa de bits. Quanto menor for essa taxa, mais perdas haverá na compressão), pior é sua fidelidade. A fidelidade pode variar entre esquemas de compressão e taxas de bits. Por isso, experimente-as para achar a que funciona melhor em seu jogo.
-   Taxa de amostragem e qualidade. Os sons podem ser amostrados com taxas diferentes, e sons amostrados com taxas menores têm uma fidelidade pior. A taxa de amostragem com qualidade de CD é de 44,1 Khz (44.100 Hz). Se não precisar de sons com alta fidelidade, escolha uma taxa de amostragem mais baixa. As taxas altas podem ser adequadas para aplicativos de áudio profissionais. Porém, você provavelmente não as usará, a não ser que seu jogo exija som com fidelidade profissional.
-   Emissores (ou origens) de sons. No XAudio2, os emissores de sons são locais que emitem um som, de um simples ruído em um barulho de fundo até uma agressiva faixa de rock reproduzida por uma jukebox no jogo. Especifique os emissores por coordenadas do ambiente.
-   Ouvintes de sons. Geralmente, um ouvinte de sons é o jogador, mas pode ser uma entidade com inteligência artificial em um jogo mais avançado, que processa os sons recebidos de um ouvinte. Você pode submixar o som no fluxo de áudio para que seja reproduzido ao jogador ou usá-lo para realizar uma ação específica no jogo, como acordar um guarda com inteligência artificial marcado como ouvinte.

## <a name="design-considerations"></a>Considerações de design


O áudio é uma parte extremamente importante do design e desenvolvimento de jogos. Muitos jogadores podem lembrar de um jogo medíocre que foi elevado a um status lendário simplesmente em função de uma trilha sonora memorável, trabalhos incríveis de dublagem e mixagem de sons ou produção de áudio geral fora de série. A música e o som definem a personalidade de um jogo e estabelecem o motivo principal que define o jogo, fazendo-o se destacar de outros jogos semelhantes. O esforço de design e desenvolvimento do perfil de áudio de seu jogo será muito bem recompensado.

O áudio 3D posicional pode proporcionar um nível de imersão além do fornecido por gráficos 3D. Se você estiver desenvolvendo um jogo complexo que simule um mundo ou exija estilo cinemático, pense em usar técnicas de áudio posicional 3D para fazer com que o jogador realmente entre no jogo.

## <a name="directx-audio-development-roadmap"></a>Mapa de desenvolvimento de áudio em DirectX


### <a name="xaudio2-conceptual-resources"></a>Recursos conceituais do XAudio2

XAudio2 é uma biblioteca de mixagem de áudio para DirectX e destina-se, principalmente, ao desenvolvimento de mecanismos de áudio de alto desempenho para jogos. Para os desenvolvedores de jogos que desejam adicionar efeitos sonoros e música de fundo aos jogos modernos, o XAudio2 oferece um mecanismo de gráfico e mixagem de áudio com baixa latência e suporte para buffers dinâmicos, reprodução precisa de amostra síncrona e conversão implícita de taxa de origem.

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415813">Introdução ao XAudio2</a></p></td>
<td align="left"><p>O tópico fornece uma lista dos recursos de programação de áudio compatíveis com o XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415762">Introdução ao XAudio2</a></p></td>
<td align="left"><p>Esse tópico fornece informações sobre os principais conceitos de XAudio2, versões de XAudio2 e sobre o formato de áudio RIFF.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415692">Conceitos comuns de programação de áudio</a></p></td>
<td align="left"><p>Esse tópico fornece uma visão geral de conceitos comuns de áudio com os quais um desenvolvedor de áudio deve se familiarizar.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415825">Vozes XAudio2</a></p></td>
<td align="left"><p>Esse tópico contém uma visão geral de vozes XAudio2, que são usadas para submixagem, operação e dados mestres de áudio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415745">Retorno de chamadas XAudio2</a></p></td>
<td align="left"><p>Esse tópico abrange o retorno de chamadas XAudio2, as quais são usadas para evitar quebras na reprodução de áudio.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415739">Gráficos de áudio XAudio2</a></p></td>
<td align="left"><p>Esse tópico abrange os gráficos de processamento de áudio XAudio2, o que extrai um conjunto de fluxos de áudio do cliente como entrada, processa-os e entrega o resultado final em um dispositivo de áudio.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415756">Efeitos de áudio XAudio2</a></p></td>
<td align="left"><p>Esse tópico abrange os efeitos de áudio XAudio2, o que extrai dados de áudio de entrada e executa algumas operações nos dados (por exemplo, um efeito de reverberação) antes de passá-los.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415821">Fazendo streaming de dados de áudio com XAudio2</a></p></td>
<td align="left"><p>Esse tópico abrange o streaming de áudio com XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415714">X3DAudio</a></p></td>
<td align="left"><p>Esse tópico abrange o X3DAudio, uma API usada em conjunto com o XAudio2 para criar a ilusão de um som vindo de um ponto em um espaço 3D.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415899">Referência de programação em XAudio2</a></p></td>
<td align="left"><p>Essa seção contém a referência completa para as APIs XAudio2.</p></td>
</tr>
</tbody>
</table>

 

### <a name="xaudio2-how-to-resources"></a>Recursos orientadores do XAudio2

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415779">Como: Inicializar o XAudio2</a></p></td>
<td align="left"><p>Saiba como inicializar o XAudio2 para reprodução de áudio criando uma instância do mecanismo XAudio2 e criando uma voz de masterização.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415781">Como: Carregar arquivos de dados de áudio no XAudio2</a></p></td>
<td align="left"><p>Saiba como popular as estruturas necessárias à execução de dados de áudio no XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415787">Como: Reproduzir um som com o XAudio2</a></p></td>
<td align="left"><p>Saiba como executar dados de áudio (já carregados) no XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415794">Como: Usar vozes de submixagem</a></p></td>
<td align="left"><p>Saiba como definir grupos de vozes para enviar a saída para a mesma voz de submixagem.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415769">Como: Usar retornos de chamadas de voz de origem</a></p></td>
<td align="left"><p>Saiba como usar os retornos de chamada de voz de origem do XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415774">Como: Usar retornos de chamadas de dispositivo</a></p></td>
<td align="left"><p>Saiba como usar os retornos de chamada de dispositivo do XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415767">Como: Compilar um gráfico de processamento de áudio básico</a></p></td>
<td align="left"><p>Saiba como criar um gráfico de processamento de áudio, construído com base em uma única voz de masterização e em uma única voz de origem.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415772">Como: Adicionar ou remover dinamicamente vozes de um gráfico de áudio</a></p></td>
<td align="left"><p>Saiba como adicionar ou remover vozes de submixagem criado de acordo com as etapas descritas em <a href="https://msdn.microsoft.com/library/windows/desktop/ee415767">Como: Compilar um gráfico de processamento de áudio básico</a>.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415789">Como: Criar uma cadeia de efeitos</a></p></td>
<td align="left"><p>Saiba como aplicar uma cadeia de efeitos a uma voz para permitir o processamento personalizado dos dados de áudio dessa voz.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415730">Como: Criar um XAPO</a></p></td>
<td align="left"><p>Saiba como implementar <a href="https://msdn.microsoft.com/library/windows/desktop/ee415893"><strong>IXAPO</strong></a> para criar um objeto de processamento de áudio XAudio2 (XAPO).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415728">Como: Adicionar suporte de parâmetro de tempo de execução ao XAPO</a></p></td>
<td align="left"><p>Saiba como adicionar suporte de parâmetro de tempo de execução a um XAPO por meio da implementação da interface <a href="https://msdn.microsoft.com/library/windows/desktop/ee415896"><strong>IXAPOParameters</strong></a>.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415733">Como: Usar um XAPO no XAudio2</a></p></td>
<td align="left"><p>Saiba como usar um efeito implementado como um XAPO em uma cadeia de efeitos XAudio2.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415723">Como: Usar um XAPOFX no XAudio2</a></p></td>
<td align="left"><p>Saiba como usar um dos efeitos incluídos no XAPO em uma cadeia de efeitos XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415791">Como: Fazer o streaming de um som do disco</a></p></td>
<td align="left"><p>Saiba como fazer o streaming de dados de áudio no XAudio2 criando um thread separado para ler um buffer de áudio e para usar retornos de chamadas para controlar esse thread.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415798">Como: Integrar X3DAudio ao XAudio2</a></p></td>
<td align="left"><p>Saiba como usar o X3DAudio para fornecer os valores de volume e rotação sobre o eixo X para vozes do XAudio2, bem como os parâmetros para o efeito de reverberação interno do XAudio2.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415783">Como: Agrupar métodos de áudio como um conjunto de operações</a></p></td>
<td align="left"><p>Saiba como usar os conjuntos de operações XAudio2 para criar um grupo de chamadas de método que se tornam efetivas ao mesmo tempo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee415765">Depurando falhas de áudio no XAudio2</a></p></td>
<td align="left"><p>Saiba como definir o nível de log de depuração para XAudio2.</p></td>
</tr>
</tbody>
</table>

 

### <a name="media-foundation-resources"></a>Recursos Media Foundation

MF (Media Foundation) é uma plataforma de mídia para streaming de reprodução de áudio e vídeo. Você pode usar as APIs do Media Foundation para fazer o streaming de áudio e vídeo codificado e compactado com vários algoritmos. Essa plataforma não foi projetada para cenários de jogos em tempo real: em vez disso, ela oferece ferramentas poderosas e suporte abrangente de codificação para captura e apresentação mais lineares de componentes de áudio e vídeo.

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms696274">Sobre o Media Foundation</a></p></td>
<td align="left"><p>Essa seção contém informações gerais sobre as APIs do Media Foundation e as ferramentas disponíveis para dar suporte a elas.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ee663601">Media Foundation: conceitos essenciais</a></p></td>
<td align="left"><p>Esse tópico apresenta alguns conceitos que são necessários ao entendimento que deve preceder a criação de um aplicativo Media Foundation.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms696219">Arquitetura Media Foundation</a></p></td>
<td align="left"><p>Essa seção descreve o design geral do Microsoft Media Foundation, como também os primitivos de mídia e o pipeline de processamento que o aplicativo usa.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd317910">Captura de áudio/vídeo</a></p></td>
<td align="left"><p>Esse tópico descreve como usar o Microsoft Media Foundation para executar a captura de áudio e vídeo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd317914">Reprodução de áudio/vídeo</a></p></td>
<td align="left"><p>Esse tópico descreve como implementar a reprodução de áudio/vídeo no seu aplicativo.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd757927">Formatos de mídia compatíveis com o Media Foundation</a></p></td>
<td align="left"><p>Esse tópico lista os formatos de mídia para os quais o Microsoft Media Foundation dá suporte nativo. (Terceiros podem dar suporte a formatos adicionais criando plug-ins personalizados.)</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/dd318778">Codificando e criando arquivos</a></p></td>
<td align="left"><p>Esse tópico descreve como usar o Microsoft Media Foundation para executar a codificação de vídeo e a criação de arquivos de mídia.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ff819508">Codificações de mídia do Windows</a></p></td>
<td align="left"><p>Esse tópico descreve como usar os recursos de codificação de áudio e vídeo do Windows Media para produzir e consumir fluxos de dados compactados.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/ms704847">Referência de programação do Media Foundation</a></p></td>
<td align="left"><p>Essa seção contém informações de referência para as APIs do Media Foundation.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/desktop/aa371827">Exemplos de SDK do Media Foundation</a></p></td>
<td align="left"><p>Essa seção lista exemplos de aplicativos que demonstram como usar o Media Foundation.</p></td>
</tr>
</tbody>
</table>

 

### <a name="windows-runtime-xaml-media-types"></a>Tipos de mídia XAML do Windows Runtime

Se estiver usando o [Interop DirectX-XAML](https://msdn.microsoft.com/library/windows/apps/hh825871), será possível incorporar as APIs de mídia XAML do Windows Runtime aos seus aplicativos UWP usando DirectX com C++ para cenários de jogos mais simples.

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
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/br242926"><strong>Windows.UI.Xaml.Controls.MediaElement</strong></a></p></td>
<td align="left"><p>Elemento XAML que representa um objeto contendo áudio, vídeo ou ambos.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt203788">Áudio, vídeo e câmera</a></p></td>
<td align="left"><p>Saiba como incorporar áudio e vídeo básicos em seu aplicativo da Plataforma Universal do Windows (UWP).</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt187272">MediaElement</a></p></td>
<td align="left"><p>Saiba como reproduzir um arquivo de mídia armazenado no local em seu aplicativo UWP.</p></td>
</tr>
<tr class="even">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt187272">MediaElement</a></p></td>
<td align="left"><p>Saiba como transmitir um arquivo de mídia com baixa latência no seu aplicativo UWP.</p></td>
</tr>
<tr class="odd">
<td align="left"><p><a href="https://msdn.microsoft.com/library/windows/apps/mt282143">Transmissão de mídia</a></p></td>
<td align="left"><p>Saiba como usar o contrato do botão Reproduzir em para transmitir mídia do seu aplicativo UWP para outro dispositivo.</p></td>
</tr>
</tbody>
</table>

 

## <a name="reference"></a>Referência


-   [Introdução ao XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415813)
-   [Guia de Programação em XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737)
-   [Visão geral da Microsoft Media Foundation](https://msdn.microsoft.com/library/windows/desktop/ms694197)

 

## <a name="related-topics"></a>Tópicos relacionados


-   [Guia de Programação em XAudio2](https://msdn.microsoft.com/library/windows/desktop/ee415737)

 

 




