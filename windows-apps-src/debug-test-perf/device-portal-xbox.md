---
ms.assetid: bf0a8b01-79f1-4944-9d78-9741e235dbe9
title: Device Portal para Xbox
description: Saiba como habilitar o Device Portal para Xbox One.
ms.date: 02/12/2017
ms.topic: article
keywords: Windows 10, uwp, o portal do dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: 42077756beff4269cc91624502fb9958c580bbc0
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635711"
---
# <a name="device-portal-for-xbox"></a>Device Portal para Xbox

## <a name="set-up-device-portal-on-xbox"></a>Configurar o Device Portal no Xbox

As etapas a seguir mostram como habilitar o Portal de Dispositivos do Xbox, que lhe dá acesso remoto para o seu desenvolvimento do Xbox.

1. Abra Início do Desenvolvimento. Isso deve abrir por padrão quando você inicializa o seu Xbox de desenvolvimento, mas você também pode abri-lo na tela inicial.

    ![DevHome do Device Portal](images/device-portal-xbox-1.png)

2. No Início do Desenvolvimento, na guia **Início**, em **Acesso Remoto**, selecione **Configurações de acesso remoto**.

    ![Ferramenta de gerenciamento remoto do Portal de Dispositivos](images/device-portal-xbox-15.png)

3. Marque a configuração **Habilitar Portal de Dispositivos do Xbox**.

4. Em **Autenticação**, selecione **Definir o nome de usuário e a senha**. Insira um **Nome de usuário** e uma **Senha**, a serem usados para autenticar o acesso ao seu kit de desenvolvimento a partir de um navegador, e **Salve** em seguida.

5. **Feche** a página de **Acesso Remoto** e observe a URL listada em **Acesso Remoto** na guia **Início**.

6. Insira a URL em seu navegador e entre com as credenciais que você configurou.

7. Você receberá um aviso sobre o certificado que foi fornecido, semelhante ao mostrado a seguir. No Microsoft Edge, clique em **Detalhes** e então em **Ir para a página da Web** para acessar o Portal de Dispositivos do Xbox. Na caixa de diálogo que aparecer, digite o nome de usuário e a senha que você inseriu anteriormente no seu Xbox.

    ![Erro de certificado do Device Portal](images/device-portal-xbox-3.png)

## <a name="device-portal-pages"></a>Páginas do Device Portal

O Portal de Dispositivos Xbox fornece um conjunto padrão de páginas semelhante ao que está disponível no Portal de Dispositivos do Windows, bem como várias páginas que são exclusivas. Para obter descrições detalhadas delas, consulte [Visão geral do Portal de Dispositivos do Windows](device-portal.md). As seções a seguir descrevem as páginas que são exclusivas do Portal de Dispositivos do Xbox.

### <a name="home"></a>Início

Semelhante à página **Gerenciador de aplicativos** do Portal de Dispositivos do Windows, a página **Início** do Portal de Dispositivos do Xbox exibe uma lista de jogos e aplicativos instalados em **Meus jogos & aplicativos**. Você pode clicar no nome de um jogo ou aplicativo para ver mais detalhes sobre ele, como o **nome de família de pacotes**. Na lista suspensa **Ações**, você pode executar uma ação no jogo ou aplicativo, como **Iniciar**.

Em **Contas de teste do Xbox Live**, você pode gerenciar as contas associadas ao seu Xbox. Você pode adicionar usuários e contas convidadas, criar novos usuários, conectar e desconectar usuários e remover contas.

![Início](images/device-portal-xbox-16.png)

### <a name="xbox-live-game-saves"></a>Xbox Live (Jogo salva)

Tanto o Portal de Dispositivos do Windows quanto o Portal de Dispositivos do Xbox têm uma página **Xbox Live**. No entanto, o Portal de Dispositivo Xbox tem uma seção exclusiva, **Jogos salvos do Xbox Live**, onde você pode salvar dados de jogos instalados no seu Xbox. Insira o **ID de configuração de serviço (SCID)** (veja [configuração do serviço Xbox Live](../xbox-live/xbox-live-service-configuration.md#get-your-ids) para obter mais informações), **NomeDoMembro (MSA)**, e **Nome da Família de Pacotes (PFN)** associados com o título e o salvamento de jogo, procure o **Arquivo de entrada (.json ou. xml)** e, em seguida, selecione um dos botões (**Redefinir**, **Importar**, **Exportar**, e **Excluir**) para manipular os dados salvos.

Na seção **Gerar**, você pode gerar dados fictícios e salvar o arquivo de entrada especificado. Basta inserir os **Contêineres (padrão 2)**, **Blobs (padrão 3)**, e **Tamanho de Blob (padrão 1024)** e selecione **Gerar**.

![Xbox Live](images/device-portal-xbox-17.png)

### <a name="http-monitor"></a>Monitor HTTP

O Monitor HTTP permite que você veja o tráfego HTTP e HTTPS descriptografado de seu aplicativo ou jogo quando ele é executado em seu Xbox One.

![Monitor HTTP](images/device-portal-xbox-18.png)

Para habilitá-lo, abra o Início do Desenvolvimento no seu Xbox One, vá para a guia **Configurações**, e na caixa **Configurações do Monitor HTTP**, marque **Habilitar Monitor HTTP**.

![Página inicial do desenvolvimento: Rede](images/device-portal-xbox-14.png)

Uma vez habilitado, no Portal de Dispositivos do Xbox, você pode **Parar**, **Limpar**, e **Salvar para o arquivo** tráfego HTTP e HTTPS, selecionando os respectivos botões.

### <a name="network-fiddler-tracing"></a>Rede (Rastreamento do Fiddler)

O **rede** página no Portal de Dispositivos do Xbox é quase idêntica ao **Rede** de página no Portal de Dispositivos do Windows, com exceção do **rastreamento Fiddler**, que é exclusivo do Portal de Dispositivos do Xbox. Isso permite que você execute o Fiddler em seu computador para fazer logon e inspecionar o tráfego HTTP e HTTPS entre seu Xbox One e a internet. Consulte [Como usar o Fiddler com o Xbox One durante o desenvolvimento do UWP](../xbox-apps/uwp-fiddler.md) para obter mais informações.

![Rede](images/device-portal-xbox-19.png)

### <a name="media-capture"></a>Captura de mídia

Na página **Captura de mídia**, você pode selecionar **Capturar tela** para fazer uma captura de tela do seu Xbox One. Depois que fizer isso, seu navegador avisará que você deve baixar o arquivo. Você pode marcar **Preferir HDR** se você quiser tirar a captura de tela em HDR (se o console for compatível).

![Captura de mídia](images/device-portal-xbox-12.png)

### <a name="settings"></a>Configurações

Na página **Configurações**, você pode exibir e editar várias configurações para seu Xbox One. Na parte superior, você pode selecionar **Importar** para importar configurações de um arquivo e **Exportar** para exportar as configurações atuais para um arquivo .txt. Importar configurações pode facilitar a edição em massa, especialmente ao configurar vários consoles. Para criar um arquivo de configurações para importar, altere as configurações para como deseja que eles sejam e, em seguida, exporte as configurações. Em seguida, você pode usar esse arquivo para importar configurações de forma rápida e fácil para outros consoles.

Há várias seções com diferentes configurações para exibir e/ou editar, que são explicadas abaixo.

![Configurações](images/device-portal-xbox-20.png)

![Configurações](images/device-portal-xbox-21.png)

#### <a name="device-information"></a>Informações do dispositivo

* **Nome do dispositivo**: O nome do dispositivo. Para editar, altere o nome na caixa e selecione **Salvar**.

* **Versão do sistema operacional**: Somente leitura. O número da versão do sistema operacional.

* **Edição do sistema operacional**: Somente leitura. O nome da versão principal do sistema operacional.

* **Xbox Live ID do dispositivo**: Somente leitura.

* **ID do console**: Somente leitura.

* **Número de série**: Somente leitura.

* **Tipo de console**: Somente leitura. O tipo de dispositivo Xbox One (Xbox One, Xbox One S ou Xbox One X).

* **Modo de desenvolvimento**: Somente leitura. O modo de desenvolvedor em que o dispositivo está.

#### <a name="audio-settings"></a>Configurações de áudio

* **Formato de fluxo de bits de áudio**: O formato dos dados de áudio.

* **Áudio HDMI**: O tipo de áudio através da porta HDMI.

* **Formato de fone de ouvido**: O formato de áudio que é fornecida por meio de fones de ouvido.

* **Áudio óptico**: O tipo de áudio através da porta óptica.

* **Usar HDMI ou óptica fone de ouvido**: Marque esta caixa se você estiver usando um headset conectado via HDMI ou mídia óptico.

#### <a name="display-settings"></a>Configurações de tela

* **Profundidade de cor**: O número de bits usados para cada componente de cor de um único pixel.

* **Espaço de cores**: A gama de cores disponível para a exibição.

* **Resolução de vídeo**: A resolução da tela.

* **Exibir conexão**: O tipo de conexão para a exibição.

* **Permitir alta de intervalo dinâmico (HDR)**: Habilita HDR na exibição. Disponível somente para as telas compatíveis.

* **Permitir 4K**: Permite a resolução de 4K no vídeo. Disponível somente para as telas compatíveis.

* **Permitir que a taxa de atualização de variável (VRR)**: Habilite VRR na exibição. Disponível somente para as telas compatíveis.

#### <a name="kinect-settings"></a>Configurações do Kinect

Um sensor Kinect deve estar conectado ao console para alterar essas configurações.

* **Habilitar o Kinect**: Habilite o sensor Kinect anexado.

* **Forçar Kinect Recarregar na alteração de aplicativo**: Recarregue o sensor Kinect anexado sempre que um aplicativo diferente ou um jogo for executado.

#### <a name="localization-settings"></a>Configurações de localização

* **Região geográfica**: A região geográfica que o dispositivo é definido como. Deve ser o código de país específico de 2 caracteres (por exemplo, **EUA** para Estados Unidos).

* **Idiomas de preferência**: O idioma que o dispositivo é definido como.

* **Fuso horário**: O fuso horário que o dispositivo é definido como.

#### <a name="network-settings"></a>Configurações de rede

* **Configurações de rádio sem fio**: As configurações sem fio do dispositivo (se determinados aspectos como LAN sem fio são ativado ou desativado).

#### <a name="power-settings"></a>Configurações de Ligar/Desligar

* **Quando está ocioso, dim tela após (minutos)**: A tela fica esmaecida depois que o dispositivo estiver ocioso durante esse período de tempo. Defina como **0** para nunca escurecer a tela.

* **Quando está ocioso, desativar após**: O dispositivo será desligado após ele ficar ocioso por essa quantidade de tempo.

* **Modo de energia**: O modo de energia do dispositivo. Consulte [Sobre os modos de economia de energia e instantâneo](https://support.xbox.com/xbox-one/console/learn-about-power-modes) para obter mais informações.

* **Automaticamente console de inicialização quando conectado à energia**: O dispositivo será automaticamente ligado quando ele está conectado a uma fonte de alimentação.

#### <a name="preference-settings"></a>Configurações de preferências

* **Padrão de experiência em casa**: Define qual tela inicial aparece quando o dispositivo for ativado.

* **Permitir conexões do aplicativo do Xbox**: O aplicativo do Xbox em outro dispositivo (como um computador Windows 10) pode se conectar a esse console.

* **Tratar os aplicativos UWP como jogos por padrão**: Jogos e aplicativos obtém diferentes recursos alocados para eles no Xbox. Se você marcar essa caixa, todos os pacotes UWP serão identificados como jogos e, portanto, receberão mais recursos.

#### <a name="user-settings"></a>Configurações do usuário

* **Automático de usuário que entrou**: Entrar automaticamente no usuário selecionado quando o dispositivo for ativado.

* **Auto logon no controlador de usuário**: Automaticamente associa um tipo de controlador específico a um usuário específico.

#### <a name="xbox-live-sandbox"></a>Área restrita do Xbox Live

Aqui você pode alterar a área restrita do Xbox Live no qual o dispositivo está. Insira o nome da área restrita na caixa e selecione **Alterar**.

### <a name="scratch"></a>Rascunho

Este é um espaço de trabalho em branco, que você pode personalizar de acordo com suas preferências. Você pode usar o menu (clique no botão de menu na parte superior esquerda) para adicionar ferramentas (selecione **Adicionar ferramentas ao espaço de trabalho**, depois as ferramentas que você deseja adicionar e, em seguida, **Adicionar**). Observe que você pode usar esse menu para adicionar ferramentas a qualquer espaço de trabalho, bem como gerenciar os espaços de trabalho em si.

![Adicionar ferramentas ao espaço de trabalho](images/device-portal-xbox-13.png)

### <a name="game-event-data"></a>Dados de eventos de jogos

Sobre o **dados de eventos de jogos** , você pode exibir um gráfico em tempo real que transmite o número de eventos de jogos Event Tracing for Windows (ETW) atualmente registrados em seu Xbox One. Se não houver eventos de jogos registrados no sistema, você também pode exibir os detalhes (nome do evento, ocorrência de evento e o título do jogo) que descreve cada evento em uma tabela de dados abaixo do gráfico de dados. A tabela só está disponível se não houver eventos registrados.

![Dados de eventos de jogos](images/device-portal-xbox-22.PNG)

## <a name="see-also"></a>Consulte também

* [Visão geral do Windows Device Portal](device-portal.md)
* [Núcleo do Portal de dispositivo referência de API](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)