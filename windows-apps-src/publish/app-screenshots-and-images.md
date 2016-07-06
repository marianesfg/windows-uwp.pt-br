---
author: jnHs
Description: "Seu aplicativo precisa incluir vários logotipos, capturas de tela e imagens."
title: Imagens e capturas de tela do aplicativo
ms.assetid: D216DD2B-F43D-4D26-82EE-0CD34DB929D8
ms.sourcegitcommit: ecb030b7c529f765eded46e4e3e9db99ad0c27e8
ms.openlocfilehash: 9eac5658e2ac04b2abc1bf06abf5c73b16260bc7

---

# Imagens e capturas de tela do aplicativo


Seu aplicativo precisa incluir vários logotipos, capturas de tela e imagens. Algumas dessas imagens são obrigatórias, enquanto outras são opcionais. Lembre-se de que suas imagens constituem uma das principais maneiras de representar seu aplicativo. Imagens ricamente elaboradas podem ser uma grande ajuda para tornar seu aplicativo atraente aos olhos dos clientes.

Durante o [processo de envio do aplicativo](app-submissions.md), você fornece [capturas de tela](#screenshots) e [arte promocional](#promotional-artwork) na etapa [Descrições](create-app-descriptions.md). Essas imagens são usadas para ajudar a exibir seu aplicativo na Loja.

A Loja também usa o bloco do aplicativo e outras imagens que você inclui no pacote do aplicativo. Execute o [Kit de Certificação de Aplicativos Windows](https://msdn.microsoft.com/library/windows/apps/mt186449) para determinar se está faltando alguma imagem exigida. Para diretrizes e recomendações sobre essas imagens, consulte [Ativos de bloco e ícone](../controls-and-patterns/tiles-and-notifications-app-assets.md).

> **Observação**  O modo como as imagens são usadas na Loja, na tela inicial do cliente e dentro do próprio aplicativo pode variar, dependendo do sistema operacional do cliente e de outros fatores.


## Imagens fornecidas durante o processo de envio

Ao inserir as informações de descrição do seu aplicativo, você tem a opção de fornecer várias capturas de tela (uma captura de tela é obrigatória) e arte promocional. Essas imagens não são tiradas do pacote do seu aplicativo; você precisará fornecê-las na etapa **Descrição** para cada um dos seus idiomas.

A tabela a seguir lista as diversas imagens que você pode carregar e explica como elas são usadas. Mais detalhes são fornecidos nas seções a seguir.

| Imagem                                                       | Tamanho do pixel                           | Utilização                                                                                                                                                                           | Quando incluir                                                                                                                                            |
|-------------------------------------------------------------|--------------------------------------|---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Capturas de tela de desktop](#screenshots)                         | 1366 x 768 ou maior                 | Exibidas nos detalhes de seu aplicativo na Loja quando visualizadas em um dispositivo de desktop.                                                                                                          | Recomendadas para todos os aplicativos, especialmente se seu aplicativo for destinado para uso em dispositivos de desktop. Pelo menos uma captura de tela (para qualquer família de dispositivos) é necessária. |
| [Capturas de tela de dispositivo móvel](#screenshots)                          | 768 x 1280, 720 x 1280 ou 480 x 800 | Exibidas nos detalhes de seu aplicativo na Loja quando visualizadas em um dispositivo móvel.                                                                                                           | Recomendado para todos os aplicativos, principalmente se o seu aplicativo for voltado para uso em dispositivos móveis. Pelo menos uma captura de tela (para qualquer família de dispositivos) é necessária. |
| [Capturas de tela holográficas](#screenshots)                          | 1268 x 720 ou maior                 | Exibidas nos detalhes de seu aplicativo na Loja quando visualizadas em um dispositivo holográfico.                                                                                                           | Recomendado se o seu aplicativo se destina ao uso no Microsoft HoloLens. Pelo menos uma captura de tela (para qualquer família de dispositivos) é necessária. |
| [Ícone do bloco do aplicativo](#app-tile-icon)                             | 300 x 300                            | Exibido como o ícone de seu aplicativo na Loja do Windows Phone 8.1 e de versões anteriores (e, se você tiver apenas pacotes voltados para Windows Phone 8.1 e versões anteriores, também na Loja no Windows 10) | Necessário para a exibição correta na Loja se seu aplicativo for destinado ao Windows Phone 8.1 ou versões anteriores.                                                                 |
| [Imagem promocional: destaque do Windows 10](#promotional-artwork) | 2400 x 1200                          | Usada para layouts promocionais na Loja do Windows 10.                                                                                                                        | Recomendado para todos os aplicativos, principalmente os com pacotes UWP voltados para clientes do Windows 10.                                                               |
| [Imagem promocional: Windows Phone 8.1 e versões anteriores](#promotional-artwork) | 1000 x 800, 358 x 358                | Usada para layouts promocionais na Loja no Windows Phone 8.1 e versões anteriores.                                                                                                     | Recomendada para todos os aplicativos voltados para o Windows Phone 8.1 ou versões anteriores.                                                                                           |
| [Imagem promocional: Windows 8.1 e versões anteriores](#promotional-artwork)        | 414 x 180                            | Usada para layouts promocionais na Loja do Windows 8.1.                                                                                                                       | Recomendado para todos os aplicativos voltados para clientes do Windows 8.1 ou de versões anteriores.                                                                                                 |
 

## Capturas de tela

As capturas de tela são as imagens do seu aplicativo que são exibidas para os clientes nos detalhes de seu aplicativo.

Você verá vários campos na página **Descrição** onde você tem a opção de fornecer capturas de tela de famílias de dispositivos diferentes (que serão mostradas quando um cliente visualizar detalhes do aplicativo na Loja sobre esse tipo de dispositivo).

Apenas uma captura de tela é necessária para seu envio (mas você pode fornecer várias; até 9 capturas de tela de desktop e até 8 capturas de tela de dispositivos móveis e holográficos). Você não precisa fornecer capturas de tela separadas para cada família de dispositivos, mas é recomendável fornecer capturas de tela de cada tipo de dispositivo compatível com aplicativo, para que os clientes vejam imagens semelhantes à aparência do aplicativo nos dispositivos deles.

> **Observação**  O Microsoft Visual Studio fornece uma [ferramenta para ajudá-lo a capturar telas](http://go.microsoft.com/fwlink/p/?LinkId=221135).

Cada captura de tela deve ser um arquivo .png em orientação paisagem ou retrato e o tamanho do arquivo não pode ser maior que 2 MB.

Os requisitos de tamanho variam de acordo com a família de dispositivos:
- Dispositivo móvel: 768 x 1280, 720 x 1280 ou 480 x 800 pixels
- Desktop: 1366 x 768 pixels ou maior
- Holográfico: 1268 x 720 pixels ou maior

Você pode fornecer uma legenda curta que descreva cada captura de tela em 200 caracteres ou menos.

> **Observação**  Caso crie descrições para [vários idiomas](supported-languages.md), você terá uma página **Descrição** para cada um. Você precisará carregar imagens para cada idioma separadamente (mesmo que esteja usando as mesmas imagens) e fornecer legendas a serem usadas para cada idioma.


## Ícone do bloco do aplicativo

Isso não é obrigatório para todos os envios, mas é altamente recomendável se seu aplicativo funcionar no Windows Phone 8.1 ou versões anteriores. O ícone do bloco do aplicativo é usado na exibição dos detalhes do aplicativo aos clientes no Windows Phone 8.1 e versões anteriores. Caso não forneça essa imagem, os cliente do Windows Phone 8.1 ou de versões anteriores verão um ícone em branco com a listagem de seu aplicativo. (Isso também se aplica a clientes no Windows 10, caso seu aplicativo tenha pacotes voltados apenas para Windows Phone 8.1 ou versões anteriores).

O ícone do bloco do aplicativo deve ser um arquivo .png medindo 300 x 300 pixels.

## Arte final promocional

A equipe editorial da Windows Store usa imagens diferentes para promover aplicativos na Loja. O envio de ilustrações promocionais permite que a equipe da Windows Store leve seu aplicativo em consideração nos layouts promocionais.

> **Importante**  O envio de imagens promocionais para seu aplicativo não garante que ele ficará em destaque, mas não enviá-las significa que ele não poderá ser considerado para nenhuma oportunidade promocional. Veja [Facilite a promoção do seu aplicativo](make-your-app-easier-to-promote.md) para saber mais.

Você pode enviar artes finais promocionais em tamanhos diferentes, dependendo de quais versões de sistema operacional sejam o alvo de seu aplicativo. Para todos os tamanhos, as imagens devem estar no formato .png.

Veja aqui algumas dicas das quais se lembrar ao criar sua arte final promocional:

-   Selecione as imagens dinâmicas relacionadas ao aplicativo e gere reconhecimento e diferenciação. Evite fotografias prontas ou elementos visuais genéricos.
-   Não inclua texto (além de sua identidade visual).
-   Minimize o espaço vazio na imagem.
-   Evite mostrar a interface do usuário do aplicativo e não use as imagens específicas de um dispositivo.
-   Evite temas políticos e nacionais, bandeiras ou símbolos religiosos.
-   Não inclua imagens com gestos ofensivos, nudez, jogatina, dinheiro, drogas, tabagismo ou bebidas alcoólicas.
-   Não use armas apontando para o usuário ou violência excessiva e mutilação.

### Para Windows 10: 2400 x 1200

Na Loja no Windows 10, o topo das páginas de categoria Aplicativos e Jogos apresenta uma imagem em destaque rotativa para promover conteúdo. Para tornar seu aplicativo qualificado para essa posição de destaque, envie uma imagem de 2400 x 1200.

Ao criar sua imagem, tenha em mente que se pudermos usá-la para destaque, aplicaremos um gradiente sobre a terça parte inferior para que possamos exibir um texto de marketing legível sobre a imagem. Por isso, evite colocar texto e elementos visuais essenciais na terça parte inferior. Além disso, podemos cortar a imagem, então, coloque a identidade visual e os detalhes mais importantes de seu aplicativo no centro.

A imagem abaixo mostra as proporções essenciais que devem ser lembradas. A "zona segura" no centro ficará proeminente mesmo se cortarmos a imagem. A "área de texto dinâmico" é onde o texto e um gradiente podem aparecer.

![diretrizes para imagem de destaque](images/spotlight1.jpg) O exemplo abaixo mostra uma imagem em destaque bem projetada que leva essas diretrizes em consideração. (As linhas na imagem são para ilustrar como a arte se encaixa em áreas designadas e não devem ser incluídas na imagem final.)

![imagem em destaque bem projetada](images/spotlight2.jpg)
### Para Windows Phone 8.1 e versões anteriores: 1000 x 800, 358 x 358

Na Loja no Windows Phone 8.1 e versões anteriores, dois tamanhos de imagem podem ser usados em layouts promocionais: 1000 x 800 pixels e 358 x 358 pixels. Se seu aplicativo é executado no Windows Phone 8.1 ou em versões anteriores, é recomendável fornecer imagens nesses dois tamanhos para que sejam levadas em consideração para fins promocionais.

> **Dica**   Além disso, certifique-se de fornecer uma [imagem de ícone de bloco do aplicativo](#app-tile-icon) 300 x 300 para garantir que seu aplicativo não apareça na Loja com um ícone em branco.

### Para Windows 8.1 e versões anteriores: 414 x 180

Na Loja do Windows 8.1 e de versões anteriores, os layouts promocionais podem usar uma imagem no tamanho 414 x 180 pixel. Se seu aplicativo é executado no Windows 8.1 ou em versões anteriores, é recomendável fornecer uma imagem nesse tamanho para que seja levado em consideração para fins promocionais.



<!--HONumber=Jun16_HO4-->


