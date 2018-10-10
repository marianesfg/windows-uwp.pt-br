---
author: QuinnRadich
Description: How to use thumbnail images to help users preview files in UWP apps.
title: Diretrizes para imagens em miniatura em aplicativos UWP
label: Thumbnail images
template: detail.hbs
ms.author: quradic
ms.date: 01/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: df1eec58d936ba4f03e1eadae534abf0620b1a39
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4501901"
---
# <a name="thumbnail-images"></a>Imagens em miniatura

Essas diretrizes descrevem como usar imagens em miniatura para ajudar os usuários a visualizar arquivos à medida que eles navegam em seu aplicativo UWP. 

> **APIs importantes**: [enumeração ThumbnailMode](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)

## <a name="should-my-app-include-thumbnails"></a>O meu aplicativo deve incluir miniaturas?

Se o seu app permitir que os usuários procurem arquivos, você poderá exibir imagens em miniatura para ajudar os usuários a visualizar rapidamente esses arquivos. 

Use miniaturas quando: 
- Exibir visualizações para muitos itens em uma coleção de galeria (como arquivos e pastas). Por exemplo, uma galeria de fotos deve usar miniaturas para fornecer aos usuários uma pequena exibição de cada imagem à medida que eles procuram seus arquivos de fotos.

    ![galeria de vídeos](images/thumbnail-gallery.png)

- Exibição de uma visualização para um item individual em uma lista (como um arquivo específico). Por exemplo, o usuário pode querer ver mais informações sobre um arquivo, incluindo uma miniatura maior para melhor visualização, antes de decidir se abrir o arquivo. 

    ![visualização de vídeo](images/thumbnail-preview.png)

## <a name="dos-and-donts"></a>O que fazer e o que não fazer
- Especifique o [modo de miniatura](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) (PicturesView, VideosView, DocumentsView, MusicView, ListView ou SingleItem) quando recuperar miniaturas. Isso garante que as imagens em miniatura sejam otimizadas para exibir o tipo de arquivos que os usuários desejam ver. 
    - Use o modo SingleItem para recuperar uma miniatura para um único item, independentemente do tipo de arquivo. Os outros modos de miniatura devem exibir visualizações de vários arquivos. 

- Exiba imagens de espaço reservado genéricas em vez de miniaturas enquanto as miniaturas são carregadas. Usar espaços reservados dessa forma faz com que seu app pareça mais responsivo, porque os usuários podem interagir com as visualizações antes que a miniatura seja carregada. 

    As imagens de espaço reservado devem ser:
    * Específica ao item de item que ela representa. Por exemplo, pastas, imagens e vídeos devem ter seus próprios espaços reservados especializados. 
    * O mesmo tamanho e taxa de proporção da imagem em miniatura que ela representa. 
    * Exibida até que a imagem da miniatura seja carregada. 

- Use imagens de espaço reservado com rótulos de texto para representar grupos de pastas e de arquivos para diferenciá-las de arquivos individuais.

- Se você não conseguir recuperar uma miniatura, exiba uma imagem de espaço reservado. 

- Exiba informações adicionais do arquivo ao fornecer visualizações para arquivos de documento e de música. Então, os usuários poderão identificar informações-chave sobre um arquivo que podem não estar prontamente disponíveis em uma imagem em miniatura sozinha. Por exemplo, para um arquivo de música, você pode exibir o nome do artista de um arquivo de música junto com a miniatura da capa do álbum. 

- Não exiba informações adicionais do arquivo para arquivos de imagem e de vídeo. Na maioria dos casos, uma imagem em miniatura é suficiente para os usuários procurarem pelas imagens e vídeos. 

## <a name="additional-usage-guidelines"></a>Diretrizes de uso adicional
[Modos de miniatura recomendados](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode) recomendados e seus recursos:

<table>
<tr>
<th> Exibir visualizações para</th>
<th> Modos de miniatura </th>
<th> Recursos das imagens em miniatura recuperadas </th>
</tr>
<tr>
<td> Imagens<br /> Vídeos </td>
<td> PicturesView <br />VideosView </td>
<td> <b>Tamanho</b>: médio, preferencialmente pelo menos 190 (se o tamanho da imagem for 190x130) <br />
<b>Taxa de proporção</b>: taxa de proporção uniforme e ampla de cerca de 0,7 (190x130 se o tamanho for 190) <br />
Recortado para visualizações. <br /> 
Ideal para alinhar imagens em uma grade, devido à taxa de proporção uniforme.  </td>
</tr>
<tr>
<td> Documentos<br />Música </td>
<td> DocumentsView <br />MusicView <br /> ListView</td>
<td> <b>Tamanho</b>: pequeno, preferencialmente pelo menos 40 x 40 pixels 
 <br />
<b>Taxa de proporção</b>: taxa de proporção uniforme e quadrada  <br />
Ideal para visualização da capa do álbum devido à taxa de proporção quadrada. <br /> 
Os documentos têm a mesma aparência apresentada em uma janela do seletor de arquivos (usam os mesmos ícones). </td>
</tr>
</tr>
<tr>
<td> Qualquer item simples (independentemente do tipo de arquivo) </td>
<td> SingleItem </td>
<td> <b>Tamanho</b>: pequeno, preferencialmente pelo menos 40 x 40 pixels 
 <br />
<b>Taxa de proporção</b>: taxa de proporção uniforme e quadrada  <br />
Ideal para visualização da capa do álbum devido à taxa de proporção quadrada. <br /> 
Os documentos têm a mesma aparência apresentada em uma janela do seletor de arquivos (usam os mesmos ícones). </td>
</tr>
</table>

Aqui estão exemplos mostrando como as imagens em miniatura recuperadas diferem dependendo do tipo de arquivo e do modo de miniatura:
<div class="mx-responsive-img">
<table>
<tr>
<th>Tipo de item</th>
<th>Quando recuperado usando: <ul><li>PicturesView <li>VideosView</ul></th>
<th>Quando recuperado usando: <ul><li>DocumentsView <li>MusicView <li>ListView</ul></th>
<th>Quando recuperado usando: <ul><li>SingleItem</ul></th>
<tr>
<tr>
<td>Imagem</td>
<td>A imagem de miniatura usa a taxa de proporção original do arquivo. <br />
<img src="images/thumbnail-pic-picvidmode.png" alt="Picture thumbnail in picture or video mode"/></td>
<td>A miniatura está recortada de acordo com uma taxa de proporção quadrada. <br />
<img src="images/thumbnail-pic-doclistmusic-modes.png" alt="Picture thumbnail in documents, music, or list modes"/></td>
<td>A imagem de miniatura usa a taxa de proporção original do arquivo.<br />
<img src="images/thumbnail-pic-single-mode.png" alt="Picture thumbnail in single mode"/> </td>
</tr>
<tr>
<td>Vídeo</td>
<td>A miniatura tem um ícone que a diferencia de imagens. <br />
<img src="images/thumbnail-vid-picvid-modes.png" alt="Video thumbnail in picture or video mode"/></td>
<td>A miniatura está recortada de acordo com uma taxa de proporção quadrada. <br />
<img src="images/thumbnail-vid-doclistmusic-modes.png" alt="Video thumbnail in documents, music, or list mode"/> </td>
<td>A imagem de miniatura usa a taxa de proporção original do arquivo. <br />
<img src="images/thumbnail-vid-single-mode.png" alt="Video thumbnail in single mode"/></td>
</tr>
<tr>
<td>Música</td>
<td>A miniatura é um ícone em tela de fundo de tamanho apropriado. A cor da tela de fundo é determinada pela cor da tela de fundo do bloco do app. <br />
<img src="images/thumbnail-music-picvid-modes.png" alt="Music thumbnail in picture or video mode"/></td>
<td>Se o arquivo tiver capa do álbum, então a miniatura será a capa do álbum.  <br />
<img src="images/thumbnail-music-doclistmusic-modes.png" alt="Music thumbnail in documents, music, or list mode"/> <br />
Caso contrário, a miniatura será um ícone em tela de fundo de tamanho apropriado.</td>
<td>Se o arquivo tiver capa do álbum, então a miniatura será a capa do álbum com a taxa de proporção original do arquivo.  <br />
<img src="images/thumbnail-music-single-mode.png" alt="Music thumbnail in single mode"/> <br />
Caso contrário, a miniatura será um ícone. </td>
</tr>
<tr>
<td>Documento</td>
<td>A miniatura é um ícone em tela de fundo de tamanho apropriado. A cor da tela de fundo é determinada pela cor da tela de fundo do bloco do app. <br />
<img src="images/thumbnail-docs-picvid-modes.png" alt="Document thumbnail in picture or video mode"/></td>
<td>A miniatura é um ícone em tela de fundo de tamanho apropriado. A cor da tela de fundo é determinada pela cor da tela de fundo do bloco do app. <br />
<img src="images/thumbnail-doc-doclistmusic-modes.png" alt="Document thumbnail in documents, music, or list mode"/></td>
<td>A miniatura do documento, se existir. <br />
<img src="images/thumbnail-doc1-single-mode.png" alt="Document thumbnail in single mode"/><br />
Caso contrário, a miniatura será um ícone. <br />
<img src="images/thumbnail-doc2-single-mode.png" alt="Document thumbnail icon in single mode"/></td>
</tr>
<tr>
<td>Pasta</td>
<td>Se houver um arquivo de imagem na pasta, então a miniatura da imagem será utilizada.  <br />
<img src="images/thumbnail-dir-picvid-modes.png" alt="Folder thumbnail in picture or video mode"/> <br />
Caso contrário, nenhuma miniatura será recuperada.</td>
<td>Nenhuma imagem em miniatura será recuperada.</td>
<td>A miniatura é o ícone da pasta.<br />
<img src="images/thumbnail-dir-single-mode.png" alt="Folder icon thumbnail in single mode"/></td>
</tr>
<tr>
<td>Grupo de arquivos</td>
<td>Se houver um arquivo de imagem na pasta, então a miniatura da imagem será utilizada.<br />
<img src="images/thumbnail-grp-picvid-modes.png" alt="File group thumbnail in picture or video mode"/> <br /> Caso contrário, nenhuma miniatura será recuperada. </td>
<td>Se houver um arquivo que tenha a capa do álbum entre os arquivos do grupo, a miniatura será a capa do álbum. <br />
<img src="images/thumbnail-grp-doclistmusic-modes.png" alt="File group thumbnail in documents, music or list mode"/> <br />Caso contrário, nenhuma miniatura será recuperada. </td>
<td>Se houver um arquivo que tenha a capa do álbum entre os arquivos do grupo, a miniatura será a capa do álbum e utilizará a taxa de proporção original do arquivo. <br />
<img src="images/thumbnail-grp1-single-mode.png" alt="File group thumbnail in picture or video mode"/> <br />Caso contrário, a miniatura será um ícone que representa um grupo de arquivos. <br />
<img src="images/thumbnail-grp2-single-mode.png" alt="File group icon in single mode"/> 
</td>
</tr>
</table>
</div>

## <a name="related-topics"></a>Tópicos relacionados
- [Enumeração ThumbnailMode](https://docs.microsoft.com/uwp/api/windows.storage.fileproperties.thumbnailmode)
- [Classe StorageItemThumbnail](https://docs.microsoft.com/uwp/api/Windows.Storage.FileProperties.StorageItemThumbnail)
- [Classe StorageFile](https://docs.microsoft.com/uwp/api/windows.storage.storagefile)
- [Exemplo de miniatura de arquivo e pasta (GitHub)](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/FileThumbnails)
- [Exibição de lista e de grade](../design/controls-and-patterns/lists.md)