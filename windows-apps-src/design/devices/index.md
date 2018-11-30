---
Description: Getting to know the devices that support Universal Windows Platform (UWP) apps will help you offer the best user experience for each form factor.
title: Cartilha de dispositivos para aplicativos UWP (Plataforma Universal do Windows)
ms.assetid: 7665044E-F007-495D-8D56-CE7C2361CDC4
label: Device primer
template: detail.hbs
keywords: dispositivo, entrada, interação
ms.date: 02/08/2017
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: de67a61b4d5982675d32e0446b67d7cf70051923
ms.sourcegitcommit: 89ff8ff88ef58f4fe6d3b1368fe94f62e59118ad
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8214082"
---
#  <a name="device-primer-for-universal-windows-platform-uwp-apps"></a>Cartilha de dispositivos para aplicativos UWP (Plataforma Universal do Windows)



![dispositivos da plataforma Windows](images/device-primer/device-primer-ramp.png)

Conhecer os dispositivos que dão suporte a aplicativos UWP (Plataforma Universal do Windows) o ajudará a oferecer a melhor experiência de usuário para cada fator forma. Ao projetar para um dispositivo específico, as principais considerações incluem como o aplicativo aparecerá no dispositivo, onde, quando e como o aplicativo será usado nesse dispositivo, e como o usuário vai interagir com esse dispositivo.

## <a name="pcs-and-laptops"></a>Computadores e notebooks


Os computadores e notebooks Windows incluem uma ampla gama de dispositivos e tamanhos de tela. Em geral, notebooks e computadores podem exibir mais informações que o telefone ou tablets.

Tamanhos de tela
-   13" e maior

![um computador](images/device-primer/device-primer-desktop.png)

Uso típico
-   Os aplicativos em desktops e notebooks permitem o uso compartilhado, mas de um usuário de cada vez e geralmente por períodos maiores.

Considerações sobre a interface do usuário
-   Os aplicativos podem ter um modo de exibição em janelas. O tamanho delas é determinado pelo usuário. Dependendo do tamanho da janela, pode haver entre um e três quadros. Em monitores maiores, o aplicativo pode ter mais de três quadros.

-   Ao usar um aplicativo em um desktop ou notebook, o usuário tem controle sobre arquivos de aplicativo. Como designer de aplicativos, forneça os mecanismos para gerenciar o conteúdo do seu aplicativo. Considere a inclusão de comandos e recursos, como "Salvar como", "Arquivos recentes" e assim por diante.

-   A volta ao sistema é opcional. Quando um desenvolvedor de aplicativos optar por exibi-lo, ele aparecerá na barra de título do aplicativo.

Entradas
-   Mouse
-   Teclado
-   Touch em laptops e em desktops tudo em um.
-   Consoles, por exemplo, o controlador do Xbox, algumas vezes são usados.

Recursos típicos do dispositivo
-   Câmera
-   Microfone

## <a name="tablets-and-2-in-1s"></a>Tablets e conversíveis


Computadores tablet ultraportáteis são equipados com telas touch, câmeras, microfones e acelerômetros. O tamanho da tela do tablet geralmente varia de 7" a 13,3". Dispositivos conversíveis podem atuar como um tablet ou um notebook com um teclado e mouse, dependendo da configuração (geralmente envolvendo virar a tela para trás ou colocá-la na posição vertical).

Tamanhos de tela
- 7" a 13,3" para tablet
- 13,3" e maior para conversível

![um tablet](images/device-primer/device-primer-tablet.png)

Uso típico
-   Cerca de 80% do uso do tablet é feito pelo proprietário, com outros 20% sendo de uso compartilhado.
-   Ele geralmente é usado em casa como um dispositivo complementar ao assistir TV.
-   Ele é usado por períodos mais longos que os telefones e phablets.
-   O texto é inserido em ciclos rápidos.

Considerações sobre a interface do usuário
-   Nas orientações retrato e paisagem, os tablets permitem dois quadros de cada vez.
-   O recurso Voltar do sistema está localizado na barra de navegação.

Entradas
-   Touch
-   Caneta digitalizadora
-   Teclado externo (ocasionalmente)
-   Mouse (ocasionalmente)
-   Voz (ocasionalmente)

Recursos típicos do dispositivo
-   Câmera
-   Microfone
-   Sensores de movimento
-   Sensores de localização

> [!NOTE]
> A maioria das considerações para computadores e notebooks se aplica também aos conversíveis.

## <a name="xbox-and-tv"></a>TV e Xbox

A experiência de sentar em seu sofá na sala, usando um gamepad ou um controle remoto para interagir com sua TV, é chamada de **experiência de 3 metros**. Ela é chamada assim porque o usuário em geral está sentado a aproximadamente 3 metros de distância da tela. Isso proporciona desafios únicos que não estão presentes na, digamos, experiência de *meio metro* ou na interação com um computador. Se você estiver desenvolvendo um aplicativo para Xbox One ou qualquer outro dispositivo que seja conectado a uma tela de TV e possa usar um gamepad ou controle remoto para entrada, você sempre deve ter isso em mente.

Projetar seu aplicativo UWP para a experiência de 3 metros é muito diferente de projetar para qualquer uma das outras categorias de dispositivo listadas aqui. Para obter mais informações, consulte [Projetando para Xbox e TV](designing-for-tv.md).

Tamanhos de tela
- A partir de 24"

![TV e Xbox](images/device-primer/device-primer-tv-and-xbox.png)

Uso típico
- Geralmente compartilhado entre várias pessoas, mas também costuma ser usado por apenas uma pessoa.
- Geralmente usado por períodos mais longos.
- Mais comumente usado em casa, ficando em um só lugar.
- Raramente solicita entrada de texto porque demora mais com um gamepad ou controle remoto.
- A orientação da tela é fixa.
- Geralmente executa somente um app por vez, mas talvez seja possível ajustar apps ao lado (como no Xbox).

Considerações sobre a interface do usuário
- Geralmente, os apps continuam do mesmo tamanho, a menos que outro app seja ajustado ao lado.
- Voltar ao sistema é uma funcionalidade útil que é oferecida na maioria dos apps do Xbox, acessada usando o botão B no gamepad.
- Como o cliente está sentado a aproximadamente 3 metros da tela, certifique-se de que a interface do usuário seja grande e clara o suficiente para ser visível.

Entradas
- Gamepad (como um controle do Xbox)
- Controle remoto
- Voz (ocasionalmente, se o cliente tiver um Kinect ou headset)

Recursos típicos do dispositivo
- Câmera (ocasionalmente, se o cliente tiver um Kinect)
- Microfone (ocasionalmente, se o cliente tiver um Kinect ou headset)
- Sensores de movimento (ocasionalmente, se o cliente tiver um Kinect)

## <a name="phones-and-phablets"></a>Telefones e phablets


O mais usado de todos os dispositivos de computação, o telefone pode fazer muito com o tamanho da tela limitado e entradas básicas. Os telefones estão disponíveis em uma variedade de tamanhos; telefones maiores são chamados de phablets. As experiências de aplicativo nos phablets são semelhantes às dos telefones, mas o espaço maior da tela dos phablets permite algumas mudanças importantes no consumo de conteúdo.

Com o Continuum para telefones, uma nova experiência para dispositivos móveis Windows 10 compatíveis, os usuários podem conectar seus telefones a um monitor e até mesmo usar um mouse e teclado para fazê-los funcionar como um notebook. (Para saber mais, veja o artigo [Continuum para telefone](http://go.microsoft.com/fwlink/p/?LinkID=699431).)

Tamanhos de tela
-   De 4 a 5 polegadas para telefone
-   De 5,5 a 7 polegadas para phablet

![windows phone](images/device-primer/device-primer-phablet.png)

Uso típico
-   Usado principalmente na orientação retrato, principalmente devido à facilidade de segurar o telefone com uma mão e poder interagir totalmente com ele dessa forma, mas há algumas experiências que funcionam bem em paisagem, como exibir fotos e vídeo, ler um livro e escrever texto.
-   Usados principalmente por apenas uma pessoa, o proprietário do dispositivo.
-   Sempre ao alcance, geralmente guardado em um bolso ou bolsa.
-   Usado por breves períodos.
-   Os usuários costumam fazer várias tarefas ao utilizarem o telefone.
-   O texto é inserido em ciclos rápidos.

Considerações sobre a interface do usuário
-   O tamanho pequeno da tela do telefone permite que somente um quadro por vez seja exibido nas orientações retrato e paisagem. Todos os padrões de navegação hierárquica em um telefone usam o modelo "detalhado", com o usuário navegando pelas camadas de interface do usuário de quadro único.

-   Semelhantes aos telefones, os phablets no modo retrato podem exibir apenas um quadro por vez. Mas, com o maior espaço da tela disponível em um phablet, os usuários têm a capacidade de girar para a orientação paisagem e permanecer nela, de modo que dois quadros de aplicativo podem estar visíveis por vez.

-   Nas orientações retrato e paisagem, verifique se há espaço na tela suficiente para a barra de aplicativos quando o teclado virtual está ativado.

Entradas
-   Touch
-   Voz

Recursos típicos do dispositivo
-   Microfone
-   Câmera
-   Sensores de movimento
-   Sensores de localização

 

## <a name="surface-hub-devices"></a>Dispositivos Surface Hub


O Microsoft Surface Hub é um dispositivo de colaboração em equipe de tela grande projetado para uso simultâneo por vários usuários.

Tamanhos de tela
-   55 e 84 polegadas

![um Surface Hub](images/device-primer/device-primer-surfacehub3.png)

Uso típico
-   Os aplicativos no Surface Hub veem o uso compartilhado por períodos curtos, como em reuniões.

-   Dispositivos Surface Hub são principalmente estáticos e raramente movidos.

Considerações sobre a interface do usuário
-   Os aplicativos no Surface Hub podem aparecer em um destes quatro estados: inteiro (modo de exibição padrão em tela inteira), segundo plano (exibição oculta, embora o aplicativo ainda esteja em execução em segundo plano, disponível no alternador de tarefas), preenchido (um modo de exibição fixo que ocupa a área de cenário disponível) e encaixado (um modo de exibição variável que ocupa o lado direito ou esquerdo do cenário).
-   Nos modos encaixado ou preenchido, o sistema exibe a barra lateral do Skype e reduz o aplicativo horizontalmente.
-   A volta ao sistema é opcional. Quando um desenvolvedor de aplicativos optar por exibi-lo, ele aparecerá na barra de título do aplicativo.

Entradas
-   Touch
-   Caneta
-   Voz
-   Teclado (na tela/remoto)
-   Touchpad (remoto)

Recursos típicos do dispositivo
-   Câmera
-   Microfone

 

## <a name="windows-iot-devices"></a>Dispositivos Windows IoT


Os dispositivos Windows IoT são uma classe emergente de dispositivos voltada para incorporação de eletrônicos pequenos, sensores e conectividade em objetos físicos. Geralmente, esses dispositivos são conectados por meio de uma rede ou da Internet para registrar dados reais detectados e, em alguns casos, agir com base neles. Os dispositivos podem ter nenhuma tela (também conhecidos como dispositivos "sem periféricos") ou são conectados a uma tela pequena (conhecidos como dispositivos "periféricos") com um tamanho de tela geralmente de 3,5" ou menos.

Tamanhos de tela
-   3,5" ou menor
-   Alguns dispositivos não têm tela

![um dispositivo IoT](images/device-primer/device-primer-iot-device.png)

Uso típico
-   Geralmente conectado por meio de uma rede ou da Internet para registrar dados reais detectados e, em alguns casos, agir com base neles.
-   Esses dispositivos podem executar somente um aplicativo de cada vez, diferente de telefones ou outros dispositivos maiores.
-   Não é algo que interage o tempo todo, mas está disponível quando você precisa, fora do caminho quando você não precisa.
-   O aplicativo não tem uma funcionalidade voltar dedicada, que é a responsabilidade de desenvolvedores.

Considerações sobre a interface do usuário
-   Os dispositivos "sem periféricos" não têm nenhuma tela.
-   A exibição em dispositivos "periféricos" é mínima, mostrando apenas o que é necessário devido a funcionalidade e estado real da tela limitados.
-   A orientação é bloqueada na maioria das vezes, de modo que seu aplicativo só precisa considerar uma direção de exibição.

Entradas
-   Variável, dependendo do dispositivo

Recursos típicos do dispositivo
-   Variável, dependendo do dispositivo