---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal para HoloLens
description: Saiba como o Windows Device Portal para HoloLens permite que você configure e gerencie seu dispositivo HoloLens remotamente.
ms.date: 01/3/2019
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 2561f18e2ac054c8b378b0c7c0a9689bebcc4140
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116085"
---
# <a name="device-portal-for-hololens"></a>Device Portal para HoloLens


## <a name="set-up-device-portal-on-hololens"></a>Configurar o Device Portal no HoloLens

### <a name="enable-device-portal"></a>Habilitar o Device Portal

1. Ligue seu HoloLens e coloque o dispositivo.
2. Faça o gesto [bloom](https://dev.windows.com/holographic/Gestures.html#Bloom) para iniciar o menu principal.
3. Fixe o olhar no bloco **Configurações** e faça o gesto [air tap](https://dev.windows.com/holographic/Gestures.html#Press_and_release). Faça um segundo air tap para colocar o aplicativo em seu ambiente. O aplicativo Configurações será iniciado depois disso.
4. Selecione item de menu **Atualização**.
5. Selecione o item de menu **Para desenvolvedores**.
6. Habilite o **Modo de Desenvolvedor**.
7. [Role para baixo](https://dev.windows.com/holographic/Gestures.html#Navigation) e habilite o Device Portal.


### <a name="pair-your-device"></a>Emparelhar seu dispositivo

#### <a name="connect-over-wi-fi"></a>Conectar-se por Wi-Fi 

1. Conecte seu HoloLens ao Wi-Fi.
2. Procure o endereço IP do seu dispositivo. Localize o endereço IP do dispositivo em **Configurações gt _ & Internet gt _ Wi-Fi gt _ Hardware propriedades da rede**. Você também pode perguntar, "Ei Cortana, qual é meu endereço IP?"

3. Usando um navegador da Web em seu computador, acesse `https://<YOUR_HOLOLENS_IP_ADDRESS>`
    - O navegador exibirá a seguinte mensagem: "Há um problema com o certificado de segurança deste site". Isso acontece porque o certificado emitido para o Device Portal é um certificado de teste. Você pode ignorar esse erro de certificado por enquanto e continuar.

#### <a name="connect-over-usb"></a>Conectar-se por USB 

1. Instale as ferramentas para certificar-se de que você tem o Visual Studio Update 1 com as ferramentas de desenvolvedor do Windows 10 instaladas no computador. Isso permite a conectividade por USB.
2. Conecte seu HoloLens ao computador com um cabo Micro USB.
3. Usando um navegador da Web em seu computador, acesse `http://127.0.0.1:10080`.

> [!IMPORTANT]
> Se seu computador não consegue encontrar o dispositivo, tente usar o endereço IP de rede real do dispositivo HoloLens, em vez de `http://127.0.0.1:10080`.

#### <a name="connect-to-an-emulator"></a>Conectar-se a um emulador 

Você também pode usar o Device Portal com seu emulador. Para se conectar ao Device Portal, use a barra de ferramentas. Clique neste ícone:
- Open Device Portal: abra o Windows Device Portal para o sistema operacional HoloLens no emulador.

#### <a name="create-a-username-and-password"></a>Criar um nome de usuário e senha 

Na primeira vez que se conectar ao Device Portal em seu HoloLens, você precisará criar um nome de usuário e senha.
1. Em um navegador da Web em seu computador, digite o endereço IP do HoloLens. A página Set up access é aberta.
2. Clique ou toque em Request pin e olhe para a tela do HoloLens para obter o PIN gerado.
3. Insira o PIN na caixa de texto PIN displayed on your device.
4. Insira o nome de usuário que você usará para se conectar ao Device Portal. Não é necessário ser um nome de MSA (conta da Microsoft) ou um nome de domínio.
5. Insira uma senha e confirme-a. A senha deve ter pelo menos sete caracteres de comprimento. Não é necessário ser uma senha de MSA ou de domínio.
6. Clique em Emparelhar para se conectar ao Windows Device Portal no HoloLens.

Se quiser alterar o nome de usuário ou a senha, você pode repetir esse processo a qualquer momento entrando na página de segurança do dispositivo, seja clicando no link Segurança na parte superior direita ou acessando: `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`.

#### <a name="security-certificate"></a>Certificado de segurança 

Se você vir um "erro de certificado" no navegador, poderá corrigi-lo criando uma relação de confiança com o dispositivo.

Cada HoloLens gera um certificado autoassinado exclusivo para sua conexão SSL. Por padrão, esse certificado não é confiável pelo navegador da Web do seu computador e você poderá receber um "erro de certificado". Ao baixar esse certificado do seu HoloLens (por USB ou uma rede Wi-Fi confiável) e torná-lo confiável em seu computador, você poderá se conectar com segurança ao seu dispositivo.
1. Certifique-se de que você esteja em uma rede segura (por USB ou uma rede Wi-Fi confiável).
2. Baixe o certificado do dispositivo da página "Segurança" no Device Portal.– Clique no link Segurança na lista superior direita dos ícones ou navegue até: `https://<YOUR_HOLOLENS_IP_ADDRESS>/devicesecurity.htm`

3. Instale o certificado no repositório "Autoridades de Certificação Confiáveis" no computador.– No menu Windows, digite: Gerenciar certificados de computador e iniciar o miniaplicativo.
    - Expanda a pasta Autoridades de Certificação Confiáveis.
    - Clique na pasta Certificados.
    - No menu Ações, selecione: Todas as tarefas > Importar...
    - Conclua o Assistente para Importação de Certificados, usando o arquivo de certificado baixado do Device Portal.

4. Reinicie o navegador.


## <a name="device-portal-pages"></a>Páginas do Device Portal 

### <a name="home"></a>Página Inicial 

A sessão do Device Portal é iniciada na home page. Acesse outras páginas a partir da barra de navegação no lado esquerdo da página inicial.

A barra de ferramentas na parte superior da página fornece acesso a recursos e status normalmente usados.
- **Online**: indica se o dispositivo está conectado ao Wi-Fi.
- **Desligar**: desativa o dispositivo.
- **Reiniciar**: repete o ciclo de energia no dispositivo.
- **Segurança**: abre a página Device Security.
- **Frio**: indica a temperatura do dispositivo.
- **A/C**: indica se o dispositivo está conectado e carregando.
- **Ajuda**: abre a página de documentação de interface REST.

A página inicial mostra as seguintes informações:
- **Status do Dispositivo**: monitora a integridade do dispositivo e relata erros críticos.
- **Informações do Windows**: mostra o nome do HoloLens e a versão do Windows instalada atualmente.
- A seção **Preferências** contém as seguintes configurações:
    - **IPD**: define a DIP (distância interpupilar), que é a distância, em milímetros, entre o centro das pupilas do usuário quando ele olha para a frente. A configuração entra em vigor imediatamente. O valor padrão é calculado automaticamente quando você configura seu dispositivo.
    - **Nome do dispositivo**: atribua um nome para o HoloLens. Você deve reinicializar o dispositivo depois de alterar esse valor para que ele entre em vigor. Depois de clicar em Salvar, uma caixa de diálogo perguntará se você deseja reiniciar o dispositivo imediatamente ou reiniciar mais tarde.
    - **Configurações de suspensão**: define o tempo de espera antes que o dispositivo entre em suspensão quando ele estiver conectado e quando estiver na bateria.

### <a name="3d-view"></a>Modo de exibição 3D 

Use a página Modo de exibição 3D para ver como o HoloLens interpreta seus arredores. Navegue pelo modo de exibição usando o mouse:
- **Girar**: clique com o botão esquerdo + mouse;
- **Panorâmica**: clique com o botão direito + mouse;
- **Zoom**: a rolagem do mouse.
- **Opções de controle**: ative o rastreamento visual contínuo ao marcar Force visual tracking. Pausar para o rastreamento visual.
- **Opções de exibição**: defina opções no modo de exibição 3D: – Rastreamento: indica se o rastreamento visual está ativo.
- **Mostrar piso**: exibe um plano de chão quadriculado.
- **Mostrar tronco**: exibe o tronco de exibição.
- **Mostrar o plano de estabilização**: exibe o plano que o HoloLens usa para estabilizar o movimento.
- **Mostrar malha**: exibe a malha de mapeamento de superfície que representa seus arredores.
- **Mostrar detalhes**: exibe as posições das mãos, os quatérnios de rotação da cabeça e o vetor de origem do dispositivo conforme eles mudam em tempo real.
- **Botão tela inteira**: mostra o Modo de exibição 3D em modo de tela inteira. Pressione ESC para sair do modo de exibição de tela inteira.

- Surface reconstruction: clique ou toque em Atualizar para exibir a malha de mapeamento espacial mais recente do dispositivo. Uma passagem completa pode levar um tempo para ser concluída, cerca de alguns segundos. A malha não atualiza automaticamente no modo de exibição 3D, e você deve clicar manualmente em Atualizar para obter a malha mais recente do dispositivo. Clique em Salvar para salvar a malha de mapeamento espacial atual como um arquivo obj em seu computador.

### <a name="mixed-reality-capture"></a>Mixed Reality Capture 

Use a página Mixed Reality Capture para salvar fluxos de mídia do HoloLens.
- Configurações: controle os fluxos de mídia que são capturados verificando as seguintes configurações:– Hologramas: captura o conteúdo holográfico no fluxo de vídeo. Hologramas são renderizados em mono, não em estéreo.
- **Câmera de PV**: captura o fluxo de vídeo da câmera de foto/vídeo.
- **Mic Audio**: captura o áudio do grupo de microfones.
- **App Audio**: captura o áudio do aplicativo em execução no momento.
- **Qualidade de visualização dinâmica**: selecione a resolução de tela, a taxa de quadros e a taxa de streaming para a visualização dinâmica.

- Clique ou toque no botão Visualização dinâmica para mostrar o fluxo de captura. Parar a visualização dinâmica interrompe o fluxo de captura.
- Clique ou toque em Gravar para iniciar a gravação do fluxo de realidade combinada, usando as configurações especificadas. Parar a gravação encerra a gravação e salva-a.
- Clique ou toque em Tirar foto para tirar uma imagem estática do fluxo de captura.
- **Vídeos e fotos**: mostra uma lista de capturas de vídeos e fotos feitas no dispositivo.

Observe que os aplicativos do HoloLens não poderão capturar uma foto ou um vídeo MRC enquanto você estiver gravando ou transmitindo uma visualização dinâmica do Device Portal.

### <a name="system-performance"></a>Desempenho do sistema 

A ferramenta Desempenho do Sistema no HoloLens tem três métricas adicionais que podem ser registradas. 

Estas são as métricas disponíveis:
- **Energia SoC**: utilização instantânea de energia do sistema-em-um-chip, com base na média de um minuto
- **Energia do sistema**: utilização instantânea de energia do sistema, com base na média de um minuto
- **Taxa de quadros**: quadros por segundo, VBlanks perdidos por segundo e VBlanks perdidos consecutivos

### <a name="app-crash-dumps-page"></a>Página de despejos de memória de aplicativos 

Essa página permite que você colete despejos de memória para seus aplicativos de sideload. Marque a caixa de seleção Crash Dumps Enabled para cada aplicativo para o qual você deseja coletar despejos de memória. Retorne a essa página para coletar despejos de memória. Arquivos de despejo podem ser abertos no Visual Studio para depuração.

### <a name="kiosk-mode"></a>Modo de quiosque 

Habilita o modo de quiosque, que limita a capacidade do usuário para iniciar novos aplicativos ou alterar o aplicativo em execução. Quando o modo de quiosque estiver habilitado, o gesto Bloom e a Cortana serão desabilitados, e os aplicativos posicionados não serão mostrados nos arredores do usuário.

Marque Enable Kiosk Mode para colocar o HoloLens no modo de quiosque. Selecione o aplicativo a ser executado no momento da inicialização na lista suspensa Startup app. Clique ou toque em Salvar para confirmar as configurações.

Observe que o aplicativo será executado no momento da inicialização, mesmo se o modo de quiosque não estiver habilitado. Selecione Nenhum para que nenhum aplicativo seja executado no momento da inicialização.

### <a name="simulation"></a>Simulação 

Permite que você grave e reproduza dados de entrada para testes.
- **Capturar sala**: usado para baixar um arquivo de sala simulada que contém a malha de mapeamento espacial para os arredores do usuário. Dê um nome à sala e clique em Capturar para salvar os dados como um arquivo .xef em seu computador. Esse arquivo de sala pode ser carregado no emulador do HoloLens.
- **Gravação**:  Verifique os fluxos para gravar, dê um nome para a gravação e clique ou toque em Gravar para iniciar a gravação. Execute ações com o HoloLens e, em seguida, clique em Parar para salvar os dados como um arquivo .xef em seu computador. Esse arquivo pode ser carregado no emulador ou dispositivo HoloLens.
- **Reprodução**: clique ou toque em Carregar gravação para selecionar um arquivo .xef do computador e enviar os dados ao HoloLens.
- **Modo de controle**: selecione Padrão ou Simulação na lista suspensa e clique ou toque no botão Definir para selecionar o modo no HoloLens. Escolher "Simulação" desabilita os sensores reais em seu HoloLens e usa dados simulados carregados em vez disso. Se você alternar para "Simulação", o HoloLens não responderá ao usuário real até que você volte para "Padrão".


### <a name="virtual-input"></a>Entrada virtual 

Envia entrada do teclado da máquina remota para o HoloLens.

Clique ou toque na região abaixo de Virtual keyboard para habilitar o envio de pressionamento de teclas ao HoloLens. Digite na caixa de texto Input text e clique ou toque em Enviar para enviar os pressionamentos de teclas ao aplicativo ativo.

## <a name="see-also"></a>Veja também

* [Visão geral do Portal de Dispositivos do Windows](device-portal.md)
* [Referência de API central do Portal de Dispositivos](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core) (APIs comuns a todos os dispositivos Windows 10)
* [Referência de APO da realidade misturada do Portal de Dispositivos](https://docs.microsoft.com/windows/mixed-reality/device-portal-api-reference) (uma lista extensa de todas as APIs REST disponíveis para HoloLens)
