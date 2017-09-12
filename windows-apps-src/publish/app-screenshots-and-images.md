---
author: jnHs
Description: "Você pode selecionar as capturas de tela, os logotipos e outros ativos de arte (como trailers e imagens promocionais) para incluir na listagem da Loja do aplicativo."
title: Capturas de tela de aplicativo, imagens e trailers
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 08/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 7d5da868a93f958a9432e7f8360129a8be8ca486
ms.sourcegitcommit: 6c6f3c265498d7651fcc4081c04c41fafcbaa5e7
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/09/2017
---
# <a name="app-screenshots-images-and-trailers"></a>Capturas de tela de aplicativo, imagens e trailers

Imagens ricamente elaboradas são uma das principais maneiras para você representar seu aplicativo para clientes em potencial na Loja.

Você pode fornecer [capturas de tela](#screenshots), [logotipos](#store-logos) e outras artes (como [trailers](#trailers) e [imagens promocionais](##additional-art-assets)) para incluir na listagem da Loja do aplicativo. Algumas delas são necessárias, e algumas são opcionais (embora algumas das imagens opcionais sejam importantes para incluir a fim de obter a melhor exibição da Loja). 

Durante o [processo de envio de aplicativo](app-submissions.md), você fornece esses ativos de arte na etapa [Listagens da Loja](create-app-store-listings.md). Observe que as imagens usadas na Loja e a forma como aparecem podem variar dependendo do sistema operacional do cliente e de outros fatores.

A Loja também pode usar o bloco do aplicativo e outras imagens que você inclui no pacote do aplicativo. Execute o [Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) para determinar se está faltando alguma imagem necessária antes de enviar o aplicativo. Para diretrizes e recomendações sobre essas imagens, consulte [Ativos de bloco e ícone](../controls-and-patterns/tiles-and-notifications-app-assets.md).

## <a name="screenshots"></a>Capturas de tela

As capturas de tela são as imagens do aplicativo exibidas para os clientes na listagem da Loja do aplicativo.

Você tem a opção de fornecer capturas de tela para diferentes famílias de dispositivos a fim de que as capturas de tela apropriadas apareçam quando um cliente exibir a listagem da Loja do aplicativo nesse tipo de dispositivo. 

Apenas uma captura de tela (para qualquer família de dispositivos) é necessária para seu envio, mas você pode fornecer várias; até nove capturas de tela de desktop e até oito capturas de tela das outras famílias de dispositivos. Sugerimos que você forneça pelo menos quatro capturas de tela de cada família de dispositivos compatíveis com seu aplicativo para que todos vejam como será a aparência do aplicativo no dispositivo.

> [!NOTE]
> O Microsoft Visual Studio oferece uma [ferramenta para facilitar a obtenção das capturas de tela](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Cada captura de tela deve ser um arquivo .png em orientação paisagem ou retrato e o tamanho do arquivo não pode ser maior que 5 MB.

Os requisitos de tamanho variam de acordo com a família de dispositivos:
- Dispositivo móvel: 768 x 1280, 720 x 1280 ou 480 x 800 pixels
- Desktop: 1366 x 768 pixels ou maior
- Holográfico: 1268 x 720 pixels ou maior
- Xbox: 3480 x 2160 pixels ou menor

Para obter a melhor exibição, mantenha as seguintes diretrizes em mente ao criar capturas de tela:
- Mantenha o texto e os elementos visuais críticos nos 3/4 superiores da imagem. Podem aparecer sobreposições de texto no 1/4 inferior. 
- Não adicione outros logotipos, ícones ou mensagens de marketing às capturas de tela.
- Não use cores extremamente claras ou escuras ou faixas altamente contrastantes que possam interferir na legibilidade das sobreposições de texto.

Você também pode fornecer uma legenda curta que descreva cada captura de tela em 200 caracteres ou menos.

> [!TIP]
> As capturas de tela são exibidas na listagem na ordem. Após carregar as capturas de tela, arraste-as e solte-as para reordená-las. 

Observe que, se você criar as listagens da Loja em [vários idiomas](supported-languages.md), você terá uma página de **listagem da Loja** para cada um. Você precisará carregar imagens para cada idioma separadamente (mesmo que esteja usando as mesmas imagens) e fornecer legendas a serem usadas para cada idioma.


## <a name="store-logos"></a>Logotipos da Loja

Os logotipos da Loja são imagens opcionais que você pode carregar para uma exibição mais personalizada na Loja. Recomendamos que você forneça essas imagens para obter a melhor exibição na listagem da Loja exibição em todos os dispositivos e versões do sistema operacional compatíveis com o aplicativo.

Você pode fornecer essas imagens como arquivos .png, que devem seguir as diretrizes abaixo:

- **9:16 (1080 x 720 ou 1440 x 2160 pixels)**: isso é usado como a imagem principal na listagem da Loja para clientes em dispositivos Windows 10 e Xbox, portanto, é altamente recomendável fornecer essa imagem; a listagem pode não ter uma boa aparência sem ela. A imagem deve incluir o nome do aplicativo, e qualquer texto na imagem deve atender aos requisitos de legibilidade acessíveis (taxa de contraste 4.51).  
- **1:1 (1080 x 1080 ou 2160 x 2160 pixels)**: esta imagem pode aparecer em alguns layouts. Se você fornecê-la, não deixe de incluir o nome do aplicativo.
- **1:1 (300 x 300 pixels)**: esta imagem, às vezes denominada **Ícone do bloco de aplicativo**, é usada ao exibir a listagem da Loja do aplicativo para clientes no Windows Phone 8.1 e versões anteriores. Caso não forneça essa imagem, os cliente do Windows Phone 8.1 ou de versões anteriores verão um ícone em branco com a listagem de seu aplicativo. (isso também se aplica aos clientes no Windows 10, se o aplicativo tiver apenas pacotes direcionados para o Windows Phone 8.1 ou anterior.) Se o envio incluir *apenas* pacotes UWP, não será necessário fornecer essa imagem. Observe que se o envio incluir pacotes do Windows Phone 8.x e pacotes UWP, e você fornecer essa imagem, ele poderá ser usado no Windows 10 em determinados layouts da Loja. Para evitar isso, crie uma [listagem específica de plataforma](create-platform-specific-store-listings.md) para as versões do sistema operacional do Windows Phone e inclua somente o ícone do bloco de aplicativo nesse local).

Você também pode impedir que a Loja use as imagens do logotipo nos pacotes do aplicativo ao exibir a listagem para os clientes no Windows 10 e, em vez disso, permitir que a Loja use apenas as imagens enviadas. Isso oferece mais controle sobre a aparência do aplicativo em várias telas em toda a Loja no Windows 10.

Para usar somente imagens enviadas para exibição na Loja no Windows 10, marque a caixa de seleção **Para clientes com Windows 10, exibir imagens de logotipo carregadas em vez das imagens dos meus pacotes**.

Ao marcar essa caixa, uma nova seção chamada **Logotipos da Loja enviados** é exibida. Aqui, você pode enviar três imagens, incluindo o tamanho do "ícone do bloco de aplicativo" de 300 x 300 (se você marcar a caixa, o campo para fornecer essa imagem se moverá para essa seção). É recomendável fornecer os três tamanhos de imagem se você usar essa opção: 300 x 300, 150 x 150 e 71 x 71 pixels. No entanto, somente o tamanho 300 x 300 é necessário.


## <a name="additional-art-assets"></a>Ativos de arte adicionais

Esta seção permite fornecer ilustrações para ajudar a exibir seu produto com mais eficiência na Loja: imagens promocionais, imagens Xbox, imagens promocionais opcionais e trailers. É recomendável fornecer essas imagens para criar uma listagem de Loja mais convidativa. A **"imagem principal" de 16:9 (1920 x 1080 ou 3840 x 2160 pixels)** é recomendável se você pretende incluir [trailers de vídeo](#trailers) na listagem da Loja; se você não incluí-lo, os trailers não aparecerão na parte superior da listagem.

Para adicionar essas imagens, selecione **Mostrar detalhes** na seção **Ativos de arte adicionais**.


## <a name="promotional-images"></a>Imagens promocionais

As imagens nesta seção podem ajudar na visualização da listagem do aplicativo e também nos permite consider o aplicativo para oportunidades promocionais em destaque. Observe que o fornecimento dessas imagens não garante que o aplicativo ficará em destaque. Consulte [Facilitar a promoção do aplicativo](make-your-app-easier-to-promote.md) para saber mais.

Veja aqui algumas dicas a serem consideradas ao criar sua arte final promocional:

- As imagens devem estar no formato .png.
- Selecione as imagens dinâmicas relacionadas ao aplicativo e promova reconhecimento e diferenciação. Evite fotografias prontas ou elementos visuais genéricos.
- Não inclua texto (além de sua identidade visual).
- Minimize o espaço vazio na imagem.
- Evite mostrar a interface do usuário do aplicativo e não use as imagens específicas de um dispositivo.
- Evite temas políticos e nacionais, bandeiras ou símbolos religiosos.
- Não inclua imagens com gestos ofensivos, nudez, jogatina, dinheiro, drogas, tabagismo ou bebidas alcoólicas.
- Não use armas apontando para o usuário ou violência excessiva e mutilação.

A **"imagem principal" de 16:9 (1920 x 1080 ou 3840 x 2160 pixels)** é usada em vários layouts promocionais da Loja em todos os tipos de dispositivos Windows 10s. É recomendável fornecer essa imagem, independentemente das versões de sistema operacional ou dos tipos de dispositivo aos quais seu aplicativo se destina. Essa imagem é *necessária* para a exibição correta se a listagem inclui [trailers vídeo](#trailers). Para clientes no Windows 10, versão 1607 ou posterior, ela é usada como a imagem principal na parte superior da listagem da Loja (ou aparece após a reprodução dos trailers). 

O tamanho de imagem **2:1 (2400 x 1200)** é usado somente se o aplicativo oferece suporte a família de dispositivos holográficos.

Ao criar sua imagem, tenha em mente que, em alguns layouts, aplicaremos um gradiente sobre a terça parte inferior para que possamos exibir um texto de marketing legível sobre a imagem. Por isso, evite colocar texto e elementos visuais essenciais na terça parte inferior. Além disso, podemos cortar a imagem; portanto, coloque a identidade visual e os detalhes mais importantes do aplicativo no centro.  

<!-- update? ![guidelines for spotlight image](images/spotlight1.jpg) 
![well-designed spotlight image](images/spotlight2.jpg)

The image below shows the key proportions to keep in mind. The "safe zone" in the center will be prominent even if we crop the image. The "dynamic text area" is where text and a gradient may appear.  -->

## <a name="xbox-images"></a>Imagens do Xbox

Essas imagens são recomendadas para exibição correta, se você publicar o aplicativo no Xbox. 

Há três tamanhos diferentes que podem ser carregados:
- **Arte de chave marcada, 584 x 800 pixels**: usado na Loja Xbox.com. Deve incluir o título do produto. 
- **Herói com título, 1920 x 1080 pixels**: deve incluir o título do produto.
- **Promoções em destaque, 1080 x 1080 pixels**: usadas na Loja Xbox. Deve incluir o título do produto.

> [!NOTE]
> Para obter a melhor exibição no Xbox, você também deve fornecer uma imagem de **9:16 (1080 x 720 ou 1440 x 2160 pixels)** na seção [logotipos da Loja](#store-logos).


## <a name="optional-promotional-images"></a>Imagens promocionais opcionais

Se o aplicativo oferecer suporte às versões anteriores do sistema operacional (Windows 8. x e/ou Windows Phone 8. x), essas imagens devem ser fornecidas para que possamos considerar o aplicativo nos layouts promocionais (embora isso não seja garantia de destaque do aplicativo). Se o aplicativo não oferecer suporte às versões anteriores do sistema operacional, você pode ignorar esta seção.

**Para o Windows Phone 8.1 e versões anteriores**, dois tamanhos de imagem podem ser usados em layouts promocionais: **1000 x 800 pixels (5:4)** e **358 x 358 pixels (1:1)**. Se seu aplicativo é executado no Windows Phone 8.1 ou em versões anteriores, é recomendável fornecer imagens nesses dois tamanhos para que sejam levadas em consideração para fins promocionais.  

> [!TIP]
> Forneça uma imagem Ícone do bloco de aplicativo 300 x 300 na seção [Logotipos da Loja](#store-logos) para qualquer envio compatível com o Windows Phone 8.1 ou anterior. Isso garantirá que o aplicativo não aparecerá na Loja com um ícone em branco.  

**Para Windows 8.1 e versões anteriores**, alguns layouts promocionais podem usar uma imagem no tamanho **414 x 180** pixel. Se seu aplicativo é executado no Windows 8.1 ou em versões anteriores, é recomendável fornecer uma imagem nesse tamanho para que seja levado em consideração para fins promocionais.


## <a name="trailers"></a>Trailers

Trailers são vídeos curtos que permitem aos clientes ver o produto em ação, para que ele possam ter uma melhor compreensão de como ele é. São exibidos na parte superior da listagem da Loja do aplicativo (desde que você inclua uma **imagem de 1920 x 1080 pixels (16:9)** na seção [Imagens promocionais](#promotional-images)). 

Os trailers são codificados com [Smooth Streaming](http://www.iis.net/downloads/microsoft/smooth-streaming), que adapta a qualidade de um streaming de vídeo entregue aos clientes em tempo real com base na largura de banda disponível e nos recursos da CPU.

> [!NOTE]
> Os trailers só são mostrados aos clientes no Windows 10, versão 1607 ou posterior.

### <a name="upload-trailers"></a>Carregar trailers

Você pode adicionar até 15 trailers à listagem da Loja. Certifique-se de que eles atendem aos requisitos listados a seguir.

Você deve fornecer um arquivo de vídeo (.mp4 ou .mov), uma imagem em miniatura e um título para cada trailer.

> [!IMPORTANT]
> Ao usar trailers, você também deve fornecer uma **imagem de 1920 x 1080 pixels (16:9)** na seção [Imagens promocionais](#promotional-images) na ordem que seus trailers aparecem na parte superior da listagem da Loja. Essa imagem será exibida após a reprodução dos trailers.

Siga estas recomendações para que seus trailers sejam eficazes:
- Os trailers devem ser de boa qualidade e ter comprimento mínimo (recomenda-se dois minutos ou menos). 
- A resolução e a taxa de quadros devem coincidir com o material de origem. Por exemplo, a captura de conteúdo a 720p 60 deve ser codificada e carregada a 720p 60. 
- Use uma miniatura diferente para cada trailer para que os clientes saibam que eles são exclusivos.
- Como alguns layouts podem cortar ligeiramente as partes superior e inferior do trailer, verifique se as informações principais estão aparecendo no centro da tela.

Você também deve cumprir os requisitos descritos a seguir.

**Para adicionar trailers à listagem:**
1. Carregue o **arquivo de vídeo** do trailer na caixa indicada. Uma caixa suspensa também é mostrada caso você queira reutilizar um trailer que já tenha sido carregado (talvez para uma listagem da Loja em outro idioma).
2. Após carregar o trailer, você precisará carregar uma **imagem em miniatura** para acompanhá-lo. Essa imagem deve ser um arquivo .png de 1920 x 1080 pixels e é geralmente uma imagem estática obtida a partir do trailer.
3. Clique no ícone de lápis para adicionar um **título** para o trailer (255 caracteres ou menos).
4. Se você quiser adicionar mais trailers à listagem, clique em **Adicionar trailer** e repita as etapas listadas acima.

Para remover um trailer, clique em **X** ao lado do nome de arquivo. Você pode optar se deseja removê-lo apenas das listagens da Loja atuais ou de todas as listagens da Loja referentes ao seu produto (ou seja, somente neste idioma ou em todos os idiomas).

### <a name="trailer-requirements"></a>Requisitos do trailer

Ao fornecer os trailers, verifique estes requisitos estão sendo cumpridos:

- O formato do vídeo deve ser MOV ou MP4. 
- A duração do vídeo deve ser inferior a 30 minutos. 
- O tamanho do arquivo do trailer não pode exceder 10 GB. 
- A miniatura deve ser um arquivo PNG com uma resolução de 1920 x 1080 pixels. 
- O título não pode exceder 255 caracteres. 

Assim como os outros campos na página de listagem da Loja, os trailers devem passar na certificação para que você possa publicá-los na Loja. Verifique se os trailers estão em conformidade com as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx).

Há requisitos adicionais dependendo do tipo de arquivo.

#### <a name="mov"></a>MOV

<table>
<tr>
<td>
**Vídeo:**
<ul>
<li>ProRes 1080p (HQ quando apropriado)</li>
<li>Taxa de quadros nativa; 29,97 FPS (preferencial)</li>
</ul>
</td>
<td>
**Áudio:**
<ul>
<li>Estéreo necessário</li>
<li>Nível de áudio recomendado: -16 LKFS/LUFS</li>
</ul> 
</td>
</tr>
</table>

#### <a name="mp4"></a>MP4

<table>
<tr>
<td>
**Vídeo:**
<ul>
<li>Codec: H.264</li>
<li>Varredura progressiva (sem entrelaçamento)</li>
<li>Alto Perfil</li>
<li>Dois quadros B consecutivos</li>
<li>GOP fechado. GOP da metade da taxa de quadros</li>
<li>CABAC</li>
<li>50 MB/s </li>
<li>Espaço de cores: 4.2.0</li>
</ul>
</td>
<td>
**Áudio:**
<ul>
<li>Codec: AAC-LC</li>
<li>Canais: estéreo ou som surround</li>
<li>Taxa de amostragem: 48 KHz</li>
<li>Taxa de bits de áudio: 384 KB/s para estéreo, 512 KB/s para sistema surround</li>
</ul>
</td>
</tr>
</table>

Para arquivos H.264 Mezzanine, é recomendável:
- Contêiner: MP4
- Sem listas de edição (do contrário, você pode perder a sincronização AV)
- Átomo moov na frente do arquivo (Início Rápido)

