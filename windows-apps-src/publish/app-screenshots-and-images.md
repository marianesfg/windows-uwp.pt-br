---
Description: Você pode selecionar as capturas de tela, os logotipos e outros ativos de arte (como trailers e imagens promocionais) para incluir na listagem da Loja do aplicativo.
title: Capturas de tela, imagens e trailers do aplicativo
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.date: 03/07/2019
ms.topic: article
keywords: windows 10, uwp, trailer, vídeo, captura de tela, imagem, ícone, listagem da Store, imagens de listagem da Store
ms.localizationpriority: medium
ms.openlocfilehash: 6a09395a57a83eaa57124173723b96a87d14b3b3
ms.sourcegitcommit: a937963ce63a14c254420926661b9b68be28a8ee
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/12/2020
ms.locfileid: "84746746"
---
# <a name="app-screenshots-images-and-trailers"></a>Capturas de tela, imagens e trailers do aplicativo

Imagens ricamente elaboradas são uma das principais maneiras para você representar seu aplicativo para clientes em potencial na Loja.

Você pode fornecer [capturas de tela](#screenshots), [logotipos](#store-logos), [trailers](#trailers)e outros ativos de arte para incluir na listagem de armazenamento do aplicativo. Algumas delas são necessárias, e algumas são opcionais (embora algumas das imagens opcionais sejam importantes para incluir a fim de obter a melhor exibição da Loja).

Durante o [processo de envio de aplicativo](app-submissions.md), você fornece esses ativos de arte na etapa [Listagens da Loja](create-app-store-listings.md). Observe que as imagens usadas na Loja e a forma como aparecem podem variar dependendo do sistema operacional do cliente e de outros fatores.

A loja também pode usar o ícone do aplicativo e outras imagens que você incluir no pacote do aplicativo. Execute o [Kit de Certificação de Aplicativos Windows](../debug-test-perf/windows-app-certification-kit.md) para determinar se está faltando alguma imagem necessária antes de enviar o aplicativo. Para obter orientações e recomendações sobre essas imagens, consulte [ícones e logotipos do aplicativo](../design/style/app-icons-and-logos.md).

## <a name="screenshots"></a>Capturas de tela

As capturas de tela são as imagens do app exibidas para os clientes na listagem da Store do app.

Você tem a opção de fornecer capturas de tela para as diferentes famílias de dispositivos com seu app é compatível a fim de que as capturas de tela apropriadas apareçam quando um cliente exibir a listagem da Store do app nesse tipo de dispositivo. 

Apenas uma captura de tela (para qualquer família de dispositivos) é necessária para seu envio, mas você pode fornecer várias; até nove capturas de tela de desktop e até oito capturas de tela das outras famílias de dispositivos. Sugerimos que você forneça pelo menos quatro capturas de tela de cada família de dispositivos compatíveis com seu aplicativo para que todos vejam como será a aparência do aplicativo no dispositivo. (Não inclua capturas de tela para as famílias de dispositivos que não são compatíveis com seu app). Observe que as capturas de tela de **Desktop** também serão mostradas aos clientes em dispositivos Surface Hub.

> [!NOTE]
> O Microsoft Visual Studio fornece uma [ferramenta para ajudá-lo a capturar capturas de tela](https://docs.microsoft.com/visualstudio/debugger/run-windows-store-apps-in-the-simulator#BKMK_Capture_a_screenshot_of_your_app_for_submission_to_the_Microsoft_Store).

Cada captura de tela deve ser um arquivo .png em orientação paisagem ou retrato e o tamanho do arquivo não pode ser maior que 50 MB.

Os requisitos de tamanho variam de acordo com a família de dispositivos:
- Desktop: 1366 x 768 pixels ou maior. Dá suporte a imagens de 4K (3840 x 2160). (Também serão mostradas aos clientes em dispositivos Surface Hub).
- Celular: as imagens devem ser uma destas: 1080 x 1920, 1920 x 1080, 768 x 1280, 1280 x 768, 720 x 1280, 1280 x 720, 800 x 480 ou 480 x 800 pixels.
- Xbox: 3480 x 2160 pixels ou menor. Dá suporte a imagens de 4K (3840 x 2160).
- Holographic: 1268 x 720 pixels ou maior. Dá suporte a imagens de 4K (3840 x 2160).

Para obter a melhor exibição, mantenha as seguintes diretrizes em mente ao criar capturas de tela:
- Mantenha o texto e os elementos visuais críticos nos 3/4 superiores da imagem. Podem aparecer sobreposições de texto no 1/4 inferior. 
- Não adicione outros logotipos, ícones ou mensagens de marketing às capturas de tela.
- Não use cores extremamente claras ou escuras ou faixas altamente contrastantes que possam interferir na legibilidade das sobreposições de texto.

Você também pode fornecer uma legenda curta que descreva cada captura de tela em 200 caracteres ou menos.

> [!TIP]
> As capturas de tela são exibidas na listagem na ordem. Após carregar as capturas de tela, arraste-as e solte-as para reordená-las. 

Observe que, se você criar as listagens da Loja em [vários idiomas](supported-languages.md), você terá uma página de **listagem da Loja** para cada um. Você precisará carregar imagens para cada idioma separadamente (mesmo que esteja usando as mesmas imagens) e fornecer legendas a serem usadas para cada idioma. (Se você tiver listagens de lojas em vários idiomas, poderá achar mais fácil atualizá-las [exportando os dados de listagem e trabalhando offline](import-and-export-store-listings.md).)


## <a name="store-logos"></a>Armazenar logotipos

Você pode carregar logotipos da Store para criar uma exibição mais personalizada na Store. Recomendamos que você forneça essas imagens de forma que sua listagem da Store apareça idealmente em todos os dispositivos e versões do sistema operacional compatíveis com o app. Observe que, se seu app estiver disponível para clientes no Xbox, algumas dessas imagens serão necessárias.

Você pode fornecer essas imagens como arquivos .png (com até 50 MB), que devem seguir as diretrizes abaixo.

### <a name="23-poster-art-720-x-1080-or-1440-x-2160-pixels"></a>arte de cartaz 2:3 (720 x 1080 ou 1440 x 2160 pixels)

É usado como a imagem de logotipo principal para clientes no Windows 10 e dispositivos Xbox; portanto, é **altamente recomendável** fornecer essa imagem para garantir uma exibição adequada. Sua listagem poderá não parecer boa se você não a incluir, e não será consistente com outras listagens que os clientes veem ao navegar na loja. Essa imagem também pode ser usada nos resultados da pesquisa ou nas coleções editorialmente comissionadas.

Essa imagem deve incluir o nome do app, e qualquer texto na imagem deve atender aos requisitos de legibilidade acessíveis (taxa de contraste de 4,51). Observe que as sobreposições de texto podem aparecer no quarto inferior dessa imagem, portanto, não inclua texto ou imagens chave ali.

> [!NOTE]
> Se o seu app estiver disponível para clientes no Xbox, essa imagem será **necessária** e deverá incluir o título do produto. O título deve aparecer nos três quartos superiores da imagem, já que sobreposições de texto podem aparecer no quarto inferior da imagem.

### <a name="11-box-art-1080-x-1080-or-2160-x-2160-pixels"></a>1:1 box art (1080 x 1080 ou 2160 x 2160 pixels)

Essa imagem pode aparecer em várias páginas de repositório para o Windows 10 (incluindo o Xbox) e, se você não fornecer a imagem de **arte do pôster 2:3** , ela será usada como seu logotipo principal. Essa imagem também deve incluir o nome do app. As sobreposições de texto podem aparecer no quarto inferior dessa imagem, portanto não inclua texto ou imagens chave ali. Inclua o nome do seu app nessa imagem. 

> [!NOTE]
> Se o seu app estiver disponível para clientes no Xbox, essa imagem será **necessária** e deverá incluir o título do produto. O título deve aparecer nos três quartos superiores da imagem, já que sobreposições de texto podem aparecer no quarto inferior da imagem.

### <a name="11-app-tile-icon-300-x-300-pixels"></a>1:1 Ícone do bloco do app (300 x 300 pixels)

Essa imagem é necessária para a exibição correta no Windows Phone 8.1 e versões anteriores. Se o aplicativo publicado anteriormente der suporte ao Windows Phone 8,1 ou anterior e você não fornecer essa imagem, esses clientes verão um ícone em branco com a listagem do seu aplicativo. (Isso também se aplicará a clientes no Windows 10 se seu aplicativo tiver apenas pacotes direcionados Windows Phone 8,1 ou anteriores.)

Se o envio incluir *apenas* pacotes UWP, você não precisará fornecer essa imagem (a menos que marque a caixa para **clientes no Windows 10 e no Xbox, exiba imagens de logotipo carregadas em vez das imagens de meus pacotes**, conforme descrito na próxima seção).

### <a name="display-only-uploaded-logo-images-in-the-store"></a>Exibir somente imagens de logotipo carregadas na loja

Você tem a opção de impedir que o armazenamento use as imagens de logotipo nos pacotes de seu aplicativo ao exibir sua listagem para clientes no Windows 10 (incluindo o Xbox) e, em vez disso, fazer com que o repositório Use apenas imagens que você carregar. Isso oferece mais controle sobre a aparência do aplicativo em várias telas em toda a Store para clientes no Windows 10 (incluindo o Xbox). (Se o aplicativo publicado anteriormente oferecer suporte a versões anteriores do sistema operacional, esses clientes ainda poderão ver imagens de seus pacotes.)

Para que o armazenamento Use apenas as imagens carregadas (para clientes no Windows 10, incluindo o Xbox) e não use imagens de seus pacotes, marque a caixa que diz **para clientes no Windows 10 e no Xbox, exiba imagens de logotipo carregadas em vez das imagens de meus pacotes**.

Ao marcar esta caixa, uma nova seção chamada **Imagens de exibição da Store** é exibida. Aqui, você pode carregar três imagens, incluindo o tamanho do **ícone de bloco do aplicativo 1:1 (300 x 300 pixels)** (se marcar a caixa, o campo para fornecer essa imagem será movido para esta seção). É recomendável fornecer os três tamanhos de imagem se você usar essa opção: 300 x 300, 150 x 150 e 71 x 71 pixels. No entanto, somente o tamanho 300 x 300 é necessário.


<span id="promotional-images" />

## <a name="trailers-and-additional-assets"></a>Trailers e ativos adicionais

Essa seção permite fornecer ilustrações para ajudar a exibir seu produto com mais eficiência na Store. É recomendável fornecer essas imagens para criar uma listagem da Store mais convidativa.

> [!TIP]
> A imagem de [arte Superhero 16:9](#windows-10-and-xbox-image-169-super-hero-art) é especialmente recomendada se você planeja incluir os [trailers de vídeo](#trailers) na sua listagem de armazenamento; Se você não incluí-lo, seus trailers não aparecerão na parte superior da sua listagem.


### <a name="trailers"></a>Trailers

Trailers são vídeos curtos que permitem aos clientes ver o produto em ação, para que ele possam ter uma melhor compreensão de como ele é. Eles são mostrados na parte superior da listagem da loja do seu aplicativo (desde que você inclua uma imagem de [arte Superhero 16:9](#windows-10-and-xbox-image-169-super-hero-art) ). 

Os trailers são codificados com [Smooth Streaming](https://www.iis.net/downloads/microsoft/smooth-streaming), que adapta a qualidade de um streaming de vídeo entregue aos clientes em tempo real com base na largura de banda disponível e nos recursos da CPU.

> [!NOTE]
> Os trailers só são mostrados aos clientes no Windows 10, versão 1607 ou posterior (o que inclui o Xbox).

### <a name="upload-trailers"></a>Carregar trailers

Você pode adicionar até 15 trailers à listagem da Store. Certifique-se de que eles atendem aos [requisitos](#trailer-requirements) listados a seguir.

Para cada trailer fornecido, você deve enviar um arquivo de vídeo (.mp4 ou .mov), uma imagem em miniatura e um título para cada trailer.

> [!IMPORTANT]
> Ao usar os trailers, você também deve fornecer uma seção de imagem de [arte Superhero 16:9](#windows-10-and-xbox-image-169-super-hero-art) para que os seus trailers apareçam na parte superior da sua listagem de armazenamento. Essa imagem será exibida após a reprodução dos trailers.

Siga estas recomendações para que seus trailers sejam eficazes:
- Os trailers devem ser de boa qualidade e ter a duração mínima (recomenda-se 60 segundos ou menos, e menos de 2 GB). 
- Use uma miniatura diferente para cada trailer para que os clientes saibam que eles são exclusivos.
- Como alguns layouts da Microsoft Store podem cortar ligeiramente as partes superior e inferior do trailer, verifique se as informações principais estão aparecendo no centro da tela.
- A resolução e a taxa de quadros devem coincidir com o material de origem. Por exemplo, a captura de conteúdo a 720p 60 deve ser codificada e carregada a 720p 60. 

Você também deve cumprir os requisitos listados a seguir.

**Para adicionar trailers à listagem:**
1. Carregue o **arquivo de vídeo** do trailer na caixa indicada. Uma caixa suspensa também é mostrada caso você queira reutilizar um trailer que já tenha sido carregado (talvez para uma listagem da Loja em outro idioma).
2. Após carregar o trailer, você precisará carregar uma **imagem em miniatura** para acompanhá-lo. Essa imagem deve ser um arquivo .png de 1920 x 1080 pixels e é geralmente uma imagem estática obtida a partir do trailer.
3. Clique no ícone de lápis para adicionar um **título** para o trailer (255 caracteres ou menos).
4. Se você quiser adicionar mais trailers à listagem, clique em **Adicionar trailer** e repita as etapas listadas acima.

> [!TIP]
> Se você tiver criado listagens da Microsoft Store em vários idiomas, você pode selecionar **Escolha dentre trailers existentes** para reutilizar os trailers já enviados. Você não precisa carregá-los individualmente para cada idioma.

Para remover um trailer da lista, clique em **X** ao lado do nome de arquivo. Você pode escolher se deseja removê-lo somente da listagem de repositório atual na qual está trabalhando ou removê-lo de todas as listagens de loja do seu produto (em cada idioma).


### <a name="trailer-requirements"></a>Requisitos do trailer

Ao fornecer os trailers, verifique estes requisitos estão sendo cumpridos:

- O formato do vídeo deve ser MOV ou MP4.
- O arquivo do trailer deve ter no máximo 2 GB.
- A resolução de vídeo deve ser 1920 x 1080 pixels.
- A miniatura deve ser um arquivo PNG com uma resolução de 1920 x 1080 pixels.
- O título não pode exceder 255 caracteres.
- Não inclua classificações etárias em seus trailers.

> [!WARNING]
> A exceção ao requisito de incluir classificações etárias em seus trailers aplica-se **somente** aos trailers no **Microsoft Store** que são mostrados **na página do produto**. Qualquer trailer Postado fora do Partner Center, que não se destina a exibição exclusivamente na página do produto Microsoft Store, **deve** exibir informações de classificação incorporadas, quando necessário, de acordo com as diretrizes da autoridade de classificação apropriada.  

Assim como os outros campos na página de listagem da Store, os trailers devem passar na certificação para que você possa publicá-los na Microsoft Store. Verifique se os trailers estão em conformidade com as [Políticas da Microsoft Store](store-policies.md).

Há requisitos adicionais dependendo do tipo de arquivo.

#### <a name="mov"></a>MOV

| Vídeo | Áudio | 
| --- | --- | 
| <ul><li>ProRes 1080p (HQ quando apropriado)</li><li>Taxa de quadros nativa; 29,97 FPS (preferencial)</li></ul> | <ul><li>Estéreo necessário</li><li>Nível de áudio recomendado: -16 LKFS/LUFS</li></ul> |


#### <a name="mp4"></a>MP4

| Vídeo | Áudio |
| --- | --- |
| <ul><li>Codec: [H. 264](https://docs.microsoft.com/windows/desktop/DirectShow/h-264-video-types) (AVC1)  </li><li>Varredura progressiva (sem entrelaçamento)</li><li>Alto Perfil</li><li>Dois quadros B consecutivos</li><li>GOP fechado. GOP da metade da taxa de quadros</li><li>CABAC</li><li>50 MB/s </li><li>Espaço de cores: 4.2.0</li></ul> | <ul><li>Codec: AAC-LC</li><li>Canais: estéreo ou som surround</li><li>Taxa de amostragem: 48 KHz</li><li>Taxa de bits de áudio: 384 KB/s para estéreo, 512 KB/s para sistema surround</li></ul> |

> [!WARNING]
> Os clientes não podem ouvir áudio para arquivos MP4 codificados com codecs diferentes de AVC1.

Para arquivos H.264 Mezzanine, é recomendável:
- Contêiner: MP4
- Sem listas de edição (do contrário, você pode perder a sincronização AV)
- Átomo moov na frente do arquivo (Início Rápido)

### <a name="windows-10-and-xbox-image-169-super-hero-art"></a>Imagem do Windows 10 e Xbox (16:9 Arte de super-herói)

Na seção **imagem do Windows 10 e Xbox** , a imagem do **super herói 16:9 (1920 x 1080 ou 3840 x 2160 pixels)** é usada em vários layouts no Microsoft Store em todos os tipos de dispositivo do Windows 10 (incluindo o Xbox). É recomendável fornecer essa imagem, independentemente das versões de sistema operacional ou dos tipos de dispositivo aos quais seu aplicativo se destina.

Essa imagem é *necessária* para a exibição correta se a listagem inclui [trailers vídeo](#trailers). Para clientes no Windows 10, versão 1607 ou posterior (que inclui o Xbox), ela é usada como a imagem principal na parte superior da listagem da Store (ou aparece após a reprodução dos trailers). Ela também pode ser usada para apresentar seu app em layouts promocionais na Store. Observe que essa imagem não deve incluir o título do produto ou outros textos.

Veja aqui algumas dicas a serem consideradas ao criar esta imagem:

- A imagem deve ser uma .png com 1920 x 1080 pixels ou 3840 x 2160 pixels.
- Selecione uma imagem dinâmica que esteja relacionada ao app para promover reconhecimento e diferenciação. Evite fotografias prontas ou elementos visuais genéricos.
- Não inclua texto na imagem.
- Evite colocar elementos visuais chave na terceira parte inferior da imagem (já que em alguns layouts, podemos aplicar um gradiente sobre essa parte).
- Coloque os detalhes mais importantes no centro da imagem (já que em alguns layouts, podemos cortar a imagem).
- Minimize o espaço vazio.
- Evite mostrar a interface do usuário do aplicativo e não use as imagens específicas de um dispositivo.
- Evite temas políticos e nacionais, bandeiras ou símbolos religiosos.
- Não inclua imagens com gestos ofensivos, nudez, jogatina, dinheiro, drogas, tabagismo ou bebidas alcoólicas.
- Não use armas apontando para o usuário ou violência excessiva e mutilação.

Embora o fornecimento dessa imagem nos permita considerar seu app para oportunidades promocionais em destaque; ela não garante que seu app aparecerá em destaque. Consulte [Facilitar a promoção do aplicativo](make-your-app-easier-to-promote.md) para saber mais.


### <a name="xbox-images"></a>Imagens do Xbox

Essas imagens são necessárias para exibição correta, se você publicar o aplicativo no Xbox. 

Há três tamanhos diferentes que podem ser carregados:
- **Arte de chave com marcas, 584 x 800 pixels**: deve incluir o título do produto. Uma Barra de Identidade Visual é necessária nessa imagem. Mantenha o título e todas as imagens chave nos três quartos superiores da imagem, já que uma sobreposição pode aparecer sobre o quarto inferior.
- **Arte de herói com título, 1920 x 1080 pixels**: deve incluir o título do produto. Mantenha o título e todas as imagens chave nos três quartos superiores da imagem, já que uma sobreposição pode aparecer sobre o quarto inferior.
- **Arte de Quadrado Promocional em Destaque, 1080 x 1080 pixels**: *não* deve incluir o título do produto.

> [!NOTE]
> Para obter a melhor exibição no Xbox, você também deve fornecer uma imagem de **9:16 (1080 x 720 ou 1440 x 2160 pixels)** na seção [logotipos da Loja](#store-logos).


### <a name="holographic-image"></a>Imagem do Holographic

A imagem **2:1 (2400 x 1200)** só será usada se o seu app for compatível com a família de dispositivos Holographic. Em caso afirmativo, é recomendável fornecer essa imagem.


<span id="optional-promotional-images" />

### <a name="images-only-for-windows-8x-andor-windows-phone-8x"></a>Imagens somente para Windows 8.x e/ou Windows Phone 8.x 

Se o aplicativo enviado anteriormente oferecer suporte a versões anteriores do sistema operacional (Windows 8. x e/ou Windows Phone 8. x), essas imagens deverão ser fornecidas para que possamos considerar o seu aplicativo em layouts promocionais (embora eles não garantam que seu aplicativo será apresentado). Se seu aplicativo não oferecer suporte a essas versões anteriores do sistema operacional, pule esta seção. (Esta seção anteriormente era chamada de **Imagens promocionais opcionais**).

**Para o Windows Phone 8.1 e versões anteriores**, dois tamanhos de imagem podem ser usados em layouts promocionais: **1000 x 800 pixels (5:4)** e **358 x 358 pixels (1:1)**. Se seu aplicativo for executado no Windows Phone 8,1 ou anterior, é recomendável fornecer imagens em ambos os tamanhos.  

> [!TIP]
> Forneça uma imagem Ícone do bloco de aplicativo 300 x 300 na seção [Logotipos da Loja](#store-logos) para qualquer envio compatível com o Windows Phone 8.1 ou anterior. Isso garantirá que o aplicativo não aparecerá na Loja com um ícone em branco.  

**Para Windows 8.1 e versões anteriores**, alguns layouts promocionais podem usar uma imagem no tamanho **414 x 180** pixel. Se seu aplicativo for executado em Windows 8.1 ou anterior, é recomendável fornecer uma imagem nesse tamanho.




