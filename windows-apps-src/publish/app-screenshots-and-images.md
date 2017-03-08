---
author: jnHs
Description: "Seu app precisa incluir vários logotipos, capturas de tela e imagens."
title: Imagens e capturas de tela do app
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 2fbbaac1b6b0f3133b9d541d6887ceb6c379f520
ms.lasthandoff: 02/07/2017

---

# <a name="app-screenshots-and-images"></a>Imagens e capturas de tela do app


Seu app precisa incluir vários logotipos, capturas de tela e imagens. Algumas dessas imagens são obrigatórias, enquanto outras são opcionais. Lembre-se de que suas imagens constituem uma das principais maneiras de representar seu app. Imagens ricamente elaboradas podem ser uma grande ajuda para tornar seu app atraente aos olhos dos clientes.

Durante o [processo de envio do app](app-submissions.md), você fornece [capturas de tela](#screenshots) e [arte promocional](#promotional-artwork) na etapa [Listagens da Loja](create-app-store-listings.md). Essas imagens são usadas para ajudar a exibir seu app na Loja.

A Loja também usa o bloco do app e outras imagens que você inclui no pacote do app. Execute o [Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) para determinar se está faltando alguma imagem exigida. Para diretrizes e recomendações sobre essas imagens, consulte [Ativos de bloco e ícone](../controls-and-patterns/tiles-and-notifications-app-assets.md).

> **Observação**  O modo como as imagens são usadas na Loja, na tela inicial do cliente e dentro do próprio app pode variar, dependendo do sistema operacional do cliente e de outros fatores.


## <a name="images-provided-during-the-submission-process"></a>Imagens fornecidas durante o processo de envio

Ao inserir as informações da Listagem da Loja do seu app, você tem a opção de fornecer várias capturas de tela (uma captura de tela é obrigatória) e arte promocional. Essas imagens não são tiradas do pacote do seu app; você precisará fornecê-las na etapa **Listagem da Loja** para cada um dos seus idiomas.

A tabela a seguir lista as diversas imagens que você pode carregar e explica como elas são usadas. Mais detalhes são fornecidos nas seções a seguir.

| Imagem                                                       | Tamanho do pixel                           | Utilização                                                                                                                                                                           | Quando incluir                                                                                                                                            |
|-------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturas de tela de desktop](#screenshots)                         | 1366 x 768 ou maior                 | Exibidas nos detalhes de seu app na Loja quando visualizadas em um dispositivo de desktop.                                                                                                          | Recomendadas para todos os apps, especialmente se seu app for destinado para uso em dispositivos de desktop. Pelo menos uma captura de tela (para qualquer família de dispositivos) é necessária. |
| [Capturas de tela de dispositivo móvel](#screenshots)                          | 768 x 1280, 720 x 1280 ou 480 x 800 | Exibidas nos detalhes de seu app na Loja quando visualizadas em um dispositivo móvel.                                                                                                           | Recomendado para todos os apps, principalmente se o seu app for voltado para uso em dispositivos móveis. Pelo menos uma captura de tela (para qualquer família de dispositivos) é necessária. |
| [Capturas de tela holográficas](#screenshots)                          | 1268 x 720 ou maior                 | Exibidas nos detalhes de seu app na Loja quando visualizadas em um dispositivo holográfico.                                                                                                           | Recomendado se o seu app se destina ao uso no Microsoft HoloLens. Pelo menos uma captura de tela (para qualquer família de dispositivos) é necessária. |
| [Ícone do bloco do app](#app-tile-icon)                             | 300 x 300                            | Exibido como o ícone de seu app na Loja do Windows Phone 8.1 e de versões anteriores (e, se você tiver apenas pacotes voltados para Windows Phone 8.1 e versões anteriores, também na Loja no Windows 10) | Obrigatório para a exibição correta na Loja se seu app for destinado ao Windows Phone 8.1 ou versões anteriores.                                                                 |
| [Imagem promocional: destaque do Windows 10](#promotional-artwork) | 2400 x 1200                          | Usada para layouts promocionais na Loja do Windows 10.                                                                                                                        | Recomendado para todos os apps, principalmente os com pacotes UWP voltados para clientes do Windows 10.                                                               |
| [Imagem promocional: Windows Phone 8.1 e versões anteriores](#promotional-artwork) | 1000 x 800, 358 x 358                | Usada para layouts promocionais na Loja no Windows Phone 8.1 e versões anteriores.                                                                                                     | Recomendada para todos os apps voltados para o Windows Phone 8.1 ou versões anteriores.                                                                                           |
| [Imagem promocional: Windows 8.1 e versões anteriores](#promotional-artwork)        | 414 x 180                            | Usada para layouts promocionais na Loja do Windows 8.1.                                                                                                                       | Recomendado para todos os apps voltados para clientes do Windows 8.1 ou de versões anteriores.                                                                                                 |
 

## <a name="screenshots"></a>Capturas de tela

As capturas de tela são as imagens do seu app que são exibidas para os clientes nos detalhes de seu app.

Você verá vários campos na página **Listagem da Loja** onde você tem a opção de fornecer capturas de tela de famílias de dispositivos diferentes (que serão mostradas quando um cliente visualizar detalhes do app na Loja sobre esse tipo de dispositivo).

Apenas uma captura de tela é necessária para seu envio (mas você pode fornecer várias; até 9 capturas de tela de desktop e até 8 capturas de tela de dispositivos móveis e holográficos). Você não precisa fornecer capturas de tela separadas para cada família de dispositivos, mas é recomendável fornecer capturas de tela de cada tipo de dispositivo compatível com app, para que os clientes vejam imagens semelhantes à aparência do app nos dispositivos deles.

> **Observação**  O Microsoft Visual Studio fornece uma [ferramenta para ajudá-lo a capturar telas](http://go.microsoft.com/fwlink/p/?LinkId=221135).

Cada captura de tela deve ser um arquivo .png em orientação paisagem ou retrato e o tamanho do arquivo não pode ser maior que 2 MB.

Os requisitos de tamanho variam de acordo com a família de dispositivos:
- Dispositivo móvel: 768 x 1280, 720 x 1280 ou 480 x 800 pixels
- Desktop: 1366 x 768 pixels ou maior
- Holográfico: 1268 x 720 pixels ou maior

Você pode fornecer uma legenda curta que descreva cada captura de tela em 200 caracteres ou menos.

> **Observação**  Caso crie as listagens da Loja para [vários idiomas](supported-languages.md), você terá uma página **Listagens da Loja** para cada um. Você precisará carregar imagens para cada idioma separadamente (mesmo que esteja usando as mesmas imagens) e fornecer legendas a serem usadas para cada idioma.


## <a name="app-tile-icon"></a>Ícone do bloco do app

Isso não é obrigatório para todos os envios, mas é altamente recomendável se seu app funcionar no Windows Phone 8.1 ou versões anteriores. O ícone do bloco do app é usado na exibição da listagem da Loja do app aos clientes no Windows Phone 8.1 e versões anteriores. Caso não forneça essa imagem, os cliente do Windows Phone 8.1 ou de versões anteriores verão um ícone em branco com a listagem de seu app. (Isso também se aplica a clientes no Windows 10, caso seu app tenha pacotes voltados apenas para Windows Phone 8.1 ou versões anteriores).

Se seu envio **somente** incluir pacotes UWP, você não precisará fornecer essa imagem. Observe que, se seu envio incluir pacotes UWP e fornecer um ícone do bloco do app, ele poderá ser exibido com a listagem do seu app no Windows 10 em alguns layouts de loja. Para evitar completamente que o ícone do bloco de app seja exibido aos clientes no Windows 10, você poderá criar uma [listagem específica da plataforma](create-platform-specific-descriptions.md) para versões anteriores do sistema operacional e incluir somente o ícone do bloco de app lá.

O ícone do bloco do app deve ser um arquivo .png medindo 300 x 300 pixels.

## <a name="promotional-artwork"></a>Arte final promocional

A equipe editorial da Windows Store usa imagens diferentes para promover apps na Loja. O envio de ilustrações promocionais permite que a equipe da Windows Store leve seu app em consideração nos layouts promocionais.

> **Importante**  O envio de imagens promocionais para seu app não garante que ele ficará em destaque, mas não enviá-las significa que ele não poderá ser considerado para nenhuma oportunidade promocional. Veja [Facilite a promoção do seu app](make-your-app-easier-to-promote.md) para saber mais.

Você pode enviar artes finais promocionais em tamanhos diferentes, dependendo de quais versões de sistema operacional sejam o alvo de seu app. Para todos os tamanhos, as imagens devem estar no formato .png.

Veja aqui algumas dicas das quais se lembrar ao criar sua arte final promocional:

-   Selecione as imagens dinâmicas relacionadas ao app e gere reconhecimento e diferenciação. Evite fotografias prontas ou elementos visuais genéricos.
-   Não inclua texto (além de sua identidade visual).
-   Minimize o espaço vazio na imagem.
-   Evite mostrar a interface do usuário do app e não use as imagens específicas de um dispositivo.
-   Evite temas políticos e nacionais, bandeiras ou símbolos religiosos.
-   Não inclua imagens com gestos ofensivos, nudez, jogatina, dinheiro, drogas, tabagismo ou bebidas alcoólicas.
-   Não use armas apontando para o usuário ou violência excessiva e mutilação.

### <a name="for-windows-10-2400-x-1200"></a>Para Windows 10: 2400 x 1200

Na Loja no Windows 10, o topo das páginas de categoria Aplicativos e Jogos apresenta uma imagem em destaque rotativa para promover conteúdo. Para tornar seu app qualificado para essa posição de destaque, envie uma imagem de 2400 x 1200.

Ao criar sua imagem, tenha em mente que se pudermos usá-la para destaque, aplicaremos um gradiente sobre a terça parte inferior para que possamos exibir um texto de marketing legível sobre a imagem. Por isso, evite colocar texto e elementos visuais essenciais na terça parte inferior. Além disso, podemos cortar a imagem, então, coloque a identidade visual e os detalhes mais importantes de seu app no centro.

A imagem abaixo mostra as proporções essenciais que devem ser lembradas. A "zona segura" no centro ficará proeminente mesmo se cortarmos a imagem. A "área de texto dinâmico" é onde o texto e um gradiente podem aparecer.

![diretrizes para imagem de destaque](images/spotlight1.jpg) O exemplo abaixo mostra uma imagem em destaque bem projetada que leva essas diretrizes em consideração. (As linhas na imagem são para ilustrar como a arte se encaixa em áreas designadas e não devem ser incluídas na imagem final.)

![imagem em destaque bem projetada](images/spotlight2.jpg)
### <a name="for-windows-phone-81-and-earlier-1000-x-800-358-x-358"></a>Para Windows Phone 8.1 e versões anteriores: 1000 x 800, 358 x 358

Na Loja no Windows Phone 8.1 e versões anteriores, dois tamanhos de imagem podem ser usados em layouts promocionais: 1000 x 800 pixels e 358 x 358 pixels. Se seu app é executado no Windows Phone 8.1 ou em versões anteriores, é recomendável fornecer imagens nesses dois tamanhos para que sejam levadas em consideração para fins promocionais.

> **Dica**   Além disso, certifique-se de fornecer uma [imagem de ícone de bloco do app](#app-tile-icon) 300 x 300 para garantir que seu app não apareça na Loja com um ícone em branco.

### <a name="for-windows-81-and-earlier-414-x-180"></a>Para Windows 8.1 e versões anteriores: 414 x 180

Na Loja do Windows 8.1 e de versões anteriores, os layouts promocionais podem usar uma imagem no tamanho 414 x 180 pixel. Se seu app é executado no Windows 8.1 ou em versões anteriores, é recomendável fornecer uma imagem nesse tamanho para que seja levado em consideração para fins promocionais.

