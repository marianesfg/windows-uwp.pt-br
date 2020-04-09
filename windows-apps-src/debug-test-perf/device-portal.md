---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Visão geral do Windows Device Portal
description: Saiba como o Windows Device Portal permite que você configure e gerencie seu dispositivo remotamente por uma rede ou conexão USB.
ms.date: 04/09/2019
ms.topic: article
keywords: windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: 2292d97166d34905bb895aa3f53f864510a21f46
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74254758"
---
# <a name="windows-device-portal-overview"></a>Visão geral do Windows Device Portal

O Windows Device Portal permite que você configure e gerencie seu dispositivo remotamente por uma rede ou conexão USB. Ele também fornece ferramentas avançadas de diagnóstico para ajudar você a solucionar problemas e exibir o desempenho de seu dispositivo Windows em tempo real.

O Portal de Dispositivos do Windows é um servidor Web em seu dispositivo ao qual você pode se conectar usando um navegador da Web em um computador. Se o dispositivo tiver um navegador da Web, você também poderá se conectar localmente com o navegador nesse dispositivo.

O Portal de Dispositivos do Windows está disponível em todas as famílias de dispositivos, mas os recursos e a configuração variam com base nos requisitos de em cada dispositivo. Este artigo fornece uma descrição geral do Windows Device Portal e links para artigos com informações mais específicas para cada família de dispositivos.

A funcionalidade do Portal de Dispositivos do Windows é implementada com [APIs REST](device-portal-api-core.md), que você pode usar diretamente para acessar os dados e controlar o dispositivo de forma programática.

## <a name="setup"></a>Instalação

Cada dispositivo possui instruções específicas para se conectar ao Device Portal, mas estas etapas gerais são necessárias em todos:

1. Habilite o Modo de Desenvolvedor e o Portal de Dispositivos em seu dispositivo (definido no aplicativo de Configurações).

2. Conecte o dispositivo e o computador por meio de uma rede local ou com o USB.

3. Navegue até a página do Device Portal em seu navegador. Esta tabela mostra as portas e os protocolos usados em cada família de dispositivos.

Família de dispositivos | Ativado por padrão? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sim, no Modo de Desenvolvedor | 80 (padrão) | 443 (padrão) | http://127.0.0.1:10080
IoT | Sim, no Modo de Desenvolvedor | 8080 | Habilitar por meio da regkey | N/D
Xbox | Habilitar dentro do Modo de Desenvolvedor | Desabilitado | 11443 | N/D
Desktop| Habilitar dentro do Modo de Desenvolvedor | 50080\* | 50043\* | N/D
Telefone | Habilitar dentro do Modo de Desenvolvedor | 80| 443 | http://127.0.0.1:10080

\* Esse nem sempre é o caso, já que o Portal de Dispositivos na área de trabalho declara portas no intervalo efêmero (>50.000) para evitar colisões com declarações de portas existentes no dispositivo. Para saber mais, consulte a seção [Configurações de porta](device-portal-desktop.md#registry-based-configuration-for-device-portal) para área de trabalho.  

Para obter instruções de instalação específicas ao dispositivo, consulte:

- [Portal de Dispositivos para HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portal de Dispositivos para IoT](https://docs.microsoft.com/windows/iot-core/manage-your-device/DevicePortal)
- [Portal de Dispositivos para celulares](device-portal-mobile.md)
- [Portal de Dispositivos para Xbox](../xbox-apps/device-portal-xbox.md)
- [Portal de Dispositivos para Desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Recursos

### <a name="toolbar-and-navigation"></a>Barra de ferramentas e navegação

A barra de ferramentas na parte superior da página fornece acesso a recursos usados com frequência.

- **Energia**: acessa as opções de energia.
  - **Desligamento**: desativa o dispositivo.
  - **Reinicialização**: repete o ciclo de energia no dispositivo.
- **Ajuda**: abre a página de ajuda.

Use os links no painel de navegação ao lado esquerdo da página para navegar até as ferramentas de gerenciamento e monitoramento disponíveis para seu dispositivo.

As ferramentas que são comuns em todas as famílias de dispositivos estão descritas aqui. Talvez haja outras opções disponíveis dependendo do dispositivo. Para saber mais, confira a página específica para seu tipo de dispositivo.

### <a name="apps-manager"></a>Gerenciador de aplicativos

O Gerenciador de aplicativos fornece funcionalidade de gerenciamento e de instalação/desinstalação para pacotes de aplicativo no dispositivo do host.

![Página do Gerenciador de aplicativos do Portal de Dispositivos](images/device-portal/WDP_AppsManager2.png)

* **Implantar aplicativos**: implante aplicativos empacotados de hosts da rede, locais ou da Web e registre arquivos soltos de compartilhamentos de rede.
* **Aplicativos instalados**: use o menu suspenso para remover ou iniciar aplicativos que estão instalados no dispositivo.
* **Aplicativos em execução**: obtenha informações sobre os aplicativos que estão sendo executados atualmente e feche-os conforme necessário.

#### <a name="install-sideload-an-app"></a>Instalar (fazer o sideload de) um aplicativo

Você pode fazer o sideload de aplicativos durante o desenvolvimento usando o Portal de Dispositivos do Windows:

1. Quando tiver criado um pacote do aplicativo, você poderá instalá-lo remotamente em seu dispositivo. Após a compilação no Visual Studio, uma pasta de saída será gerada.

    ![Instalação de aplicativos](images/device-portal/iot-installapp0.png)

2. No Portal de Dispositivos do Windows, navegue até a página **Gerenciador de aplicativos**.

3. Na seção **Implantar aplicativos**, selecione **Armazenamento Local**.

4. Em **Selecionar o pacote do aplicativo**, selecione **Escolher arquivo** e navegue até o pacote do aplicativo do qual você deseja fazer o sideload.

5. Em **Selecionar o arquivo do certificado (.cer) usado para assinar um pacote do aplicativo**, selecione **Escolher Arquivo** e navegue até o certificado associado a esse pacote de aplicativo.

6. Marque as respectivas caixas se desejar instalar pacotes opcionais ou de estrutura junto com a instalação do aplicativo e selecione **Próximo** para selecioná-los.

7. Selecione **Instalar** para iniciar a instalação.

8. Se o dispositivo estiver executando o Windows 10 no modo S, e for a primeira vez que o certificado fornecido foi instalado no dispositivo, reinicie o dispositivo.

#### <a name="install-a-certificate"></a>Instalar um certificado

Como alternativa, você pode instalar o certificado por meio do Portal de Dispositivos do Windows e instalar o aplicativo por outros meios:

1. No Portal de Dispositivos do Windows, navegue até a página **Gerenciador de aplicativos**.

2. Na seção **Implantar aplicativos**, selecione **Instalar Certificado**.

3. Em **Selecionar o arquivo do certificado (.cer) usado para assinar um pacote do aplicativo**, selecione **Escolher Arquivo** e navegue até o certificado associado ao pacote do aplicativo do qual você deseja fazer o sideload.

4. Selecione **Instalar** para iniciar a instalação.

5. Se o dispositivo estiver executando o Windows 10 no modo S, e for a primeira vez que o certificado fornecido foi instalado no dispositivo, reinicie o dispositivo.

#### <a name="uninstall-an-app"></a>Desinstalar um aplicativo

1. Certifique-se de que seu aplicativo não esteja em execução.
2. Se estiver, vá para **Aplicativos em execução** e feche-o. Se você tentar desinstalar enquanto o aplicativo estiver em execução, isso causará problemas ao tentar instalar o aplicativo novamente.
3. Selecione o aplicativo na lista suspensa e clique em **Remover**.

### <a name="running-processes"></a>Processos em execução

Esta página mostra detalhes sobre os processos em execução no momento no dispositivo host. Isso inclui aplicativos e processos do sistema. Em algumas plataformas (Área de Trabalho, IoT e HoloLens), você pode encerrar processos.

![Página de processos em execução do Portal de Dispositivos](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorador de Arquivos

Esta página permite que você exiba e manipule arquivos armazenados por seus aplicativos carregados por sideload. Confira a postagem no blog [Usar o Explorador de Arquivos do aplicativo](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) para saber mais sobre o Explorador de Arquivos e como usá-lo.

![Página do Explorador de Arquivos do Portal de Dispositivos](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Desempenho

A página de Desempenho mostra gráficos em tempo real de informações de diagnóstico do sistema, como o uso de energia, a taxa de quadros e a carga da CPU.

Estas são as métricas disponíveis:

- **CPU**: percentual do total disponível de utilização de CPU
- **Memória**: total, em uso, disponível, comprometida, paginada e não paginada
- **E/S**: quantidades de dados de leitura e gravação
- **Rede**: dados recebidos e enviados
- **GPU**: percentual do total disponível de utilização do mecanismo GPU

![Página de Desempenho do Portal de Dispositivos](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Registro em log de ETW (Rastreamento de Eventos para Windows)

A página de registro em log de ETW gerencia informações de ETW (Rastreamento de Eventos para Windows) em tempo real no dispositivo.

![Página de registro em log de ETW do Portal de Dispositivos](images/device-portal/mob-device-portal-etw.png)

Marque **Ocultar provedores** para mostrar apenas a lista de eventos.

- **Provedores registrados**: selecione o provedor de eventos e o nível de rastreamento. O nível de rastreamento é um destes valores:
  1. Saída anormal ou encerramento
  2. Erros graves
  3. Avisos
  4. Avisos que não são de erro
  5. Rastreamento detalhado

  Clique ou toque em **Habilitar** para iniciar o rastreamento. O provedor é adicionado à lista suspensa **Provedores Habilitados**.
- **Provedores personalizados**: seleciona um provedor ETW personalizado e o nível de rastreamento. Identifique o provedor pelo GUID. Não inclua colchetes no GUID.
- **Provedores habilitados**: lista os provedores habilitados. Selecione um provedor da lista suspensa e clique ou toque em **Desabilitar** para parar o rastreamento. Clique ou toque em **Parar todos** para suspender todo o rastreamento.
- **Histórico de provedores**: mostra os provedores de ETW que foram habilitados durante a sessão atual. Clique ou toque em **Habilitar** para ativar um provedor que foi desabilitado. Clique ou toque em **Limpar** para limpar o histórico.
- **Filtros/Eventos**: A seção **Eventos** lista os eventos de ETW dos provedores selecionados no formato de tabela. A tabela é atualizada em tempo real. Use o menu **Filtros** para configurar filtros personalizados de quais eventos serão exibidos. Clique no botão **Limpar** para excluir todos os eventos de ETW da tabela. Isso não desabilita os provedores. Você pode clicar em **Salvar no arquivo** para exportar os atuais eventos ETW coletados para um arquivo CSV local.

Para mais informações sobre como usar o registro em log de ETW, confira a postagem no blog [Usar o Portal de Dispositivos para visualizar logs de depuração](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/).

### <a name="performance-tracing"></a>Rastreamento de desempenho

A página de Rastreamento de desempenho permite que você confira os rastreamentos do [WPR (Windows Performance Recorder)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) do dispositivo host.

![Página de Rastreamento de desempenho do Portal de Dispositivos](images/device-portal/mob-device-portal-perf-tracing.png)

- **Perfis disponíveis**: selecione o perfil WPR na lista suspensa e clique ou toque em **Iniciar** para iniciar o rastreamento.
- **Perfis personalizados**: clique ou toque em **Procurar** para escolher um perfil WPR do seu computador. Clique ou toque em **Carregar e iniciar** para iniciar o rastreamento.

Para interromper o rastreamento, clique em **Parar**. Fique nessa página até que o download do arquivo de rastreamento (.ETL) seja concluído.

Os arquivos .ETL capturados podem ser abertos para análise no [Windows Performance Analyzer](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Gerenciador de dispositivos

A página do Gerenciador de Dispositivos enumera todos os periféricos conectados ao seu dispositivo. Você pode clicar nos ícones de configurações para exibir as propriedades de cada um.

![Página do Gerenciador de Dispositivos do Portal de Dispositivos](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Rede

A página Rede gerencia conexões de rede no dispositivo. A menos que você esteja conectado ao Portal de Dispositivos por meio de USB, alterar essas configurações provavelmente desconectará você desse Portal.

- **Redes disponíveis**: mostra as redes de Wi-Fi disponíveis para o dispositivo. Clicar ou tocar em uma rede permitirá que você se conecte a ela e forneça uma chave de acesso, se necessário. O Portal de Dispositivos ainda não dá suporte à Autenticação de Empresa. Você também pode usar a lista suspensa **Perfis** para tentar se conectar a qualquer um dos perfis de WiFi conhecidos pelo dispositivo.
- **Configuração de IP**: mostra informações de endereço sobre cada uma das portas de rede do dispositivo host.

![Página Rede do Portal de Dispositivos](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Notas e recursos do serviço

### <a name="dns-sd"></a>DNS-SD

O Device Portal anuncia sua presença na rede local usando DNS-SD. Todas as instâncias do Device Portal, independentemente do tipo de dispositivo, anunciam em "WDP._wdp._tcp.local". Os registros TXT para a instância do serviço fornecem o seguinte:

Chave | Tipo | Descrição
----|------|-------------
S | int | Porta segura para o Device Portal. Se 0 (zero), o Device Portal não está escutando conexões HTTPS.
D | cadeia de caracteres | Tipo de dispositivo. Isso estará no formato "Windows.*", por exemplo, Windows.Xbox ou Windows.Desktop
A | cadeia de caracteres | Arquitetura do dispositivo. Isso será ARM, x86 ou AMD64.  
T | lista de cadeias de caracteres delineadas de caracteres nulos | Marcas aplicadas pelo usuário para o dispositivo. Consulte as Marcas API de REST para saber como usá-las. A lista é encerrada com duplo nulo.  

É recomendável estabelecer conexão na porta HTTPS, pois nem todos os dispositivos estão ouvindo na porta HTTP anunciada pelo registro DNS SD.

### <a name="csrf-protection-and-scripting"></a>Proteção contra CSRF e scripts

Para proteger-se contra [ataques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), um token exclusivo é necessário em todas as solicitações não GET. Esse token, o cabeçalho de solicitação X-CSRF-Token, é derivado de um cookie de sessão, o CSRF-Token. Na interface do usuário Web do Device Portal, o cookie CSRF-Token é copiado no cabeçalho X-CSRF-Token em cada solicitação.

> [!IMPORTANT]
> Essa proteção impede o uso das APIs REST a partir de um cliente autônomo (por exemplo, utilitários de linha de comando). Isso pode ser resolvido de três modos:
> - Use um nome de usuário "auto-". Os clientes que têm o prefixo "auto-" no nome de usuário serão ignorados pela proteção contra CSRF. É importante que esse nome de usuário não seja usado para fazer logon no Device Portal por meio do navegador, pois ele abrirá o serviço para ataques CSRF. Exemplo: se o nome de usuário do Portal de Dispositivos for "admin", ```curl -u auto-admin:password <args>``` deverá ser usado para ignorar a proteção contra CSRF.
> - Implemente o esquema cookie-para-cabeçalho no cliente. Isso exige uma solicitação GET para estabelecer o cookie de sessão e, em seguida, a inclusão do cabeçalho e o cookie em todas as solicitações subsequentes.
> - Desabilite a autenticação e use HTTP. A proteção contra CSRF só se aplica a pontos de extremidade HTTPS, portanto, as conexões em pontos de extremidade HTTP não precisam fazer nada do que é mencionado acima.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Proteção de Cross-Site WebSocket Hijacking (CSWSH)

Para se proteger contra [ataques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos os clientes que abrem uma conexão WebSocket com o Device Portal também devem fornecer um cabeçalho Origin que corresponda ao cabeçalho Host. Isso prova para o Device Portal que a solicitação vem da interface do usuário do Device Portal ou um aplicativo cliente válido. Sem o cabeçalho Origin, sua solicitação será rejeitada.
