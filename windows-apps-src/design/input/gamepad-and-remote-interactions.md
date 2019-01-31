---
autor de cabeça <<<<<<<: descrição Karl ponte-Microsoft: otimizar seu aplicativo para entrada do gamepad do Xbox e controle remoto.
=== Descrição: TODO
>>>>>>> título do mestre: MS. AssetID interações Gamepad e controle remoto: 784a08dc-2736-4bd3-bea0-08da16b1bd47 rótulo: Gamepad e modelo de interações remoto: isNew detail.hbs: verdadeiro <<<<<<< HEAD MS: kbridge =======
mestre MS. Date: 08/02/2017 ms.topic: palavras-chave do artigo: windows 10, uwp MS. localizationpriority: médio
---
# <a name="gamepad-and-remote-control-interactions"></a>Interações de Gamepad e de controle remoto

![imagem de teclado e gamepad](images/keyboard/keyboard-gamepad.jpg)

***Padrões de interação comuns são compartilhados entre teclado, gamepad e controle remoto***

Certificar-se de que seu aplicativo funciona bem com gamepad e controle remoto é a etapa mais importante na otimização para experiências de 3 metros. Existem várias melhorias específicas no gamepad e no controle remoto que você pode fazer para otimizar a experiência de interação do usuário em um dispositivo em que suas ações são relativamente limitadas.

Os aplicativos UWP (Plataforma Universal do Windows) agora dão suporte à entrada de gamepad e de controle remoto. 

Gamepads e controles remotos são os dispositivos de entrada principais para Xbox e experiências com TV. 

Os aplicativos UWP devem ser otimizados para esses tipos de dispositivo de entrada, da mesma forma como são otimizados para entrada de mouse e de teclado em um computador e entrada por toque em um telefone ou tablet. 

Verificar se seu aplicativo funciona bem com esses dispositivos de entrada é a etapa mais importante ao otimizar para Xbox e os programas de TV.

Agora você pode conectar e usar o gamepad com aplicativos UWP no computador o que facilita a validação do trabalho.

Para garantir uma experiência de usuário bem-sucedida e agradável para seu aplicativo UWP ao usar um gamepad ou controle remoto, você deve considerar o seguinte:

* [Botões de hardware](../devices/designing-for-tv.md#hardware-buttons) - O gamepad e o controle remoto fornecem configurações e botões muito diferentes.

* [Interação e navegação de foco do plano XY](../devices/designing-for-tv.md#xy-focus-navigation-and-interaction) - A navegação de foco do plano XY permite que o usuário navegue na interface do usuário do aplicativo.

* [Modo de mouse](../devices/designing-for-tv.md#mouse-mode) - O modo de mouse permite que seu aplicativo emule uma experiência de mouse quando a navegação de foco do plano XY não for suficiente.
