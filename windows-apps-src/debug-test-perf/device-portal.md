---
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: Visão geral do Windows Device Portal
description: Saiba como o Windows Device Portal permite que você configure e gerencie seu dispositivo remotamente por uma rede ou conexão USB.
ms.date: 04/09/2019
ms.topic: article
keywords: Windows 10, UWP, portal do dispositivo
ms.localizationpriority: medium
ms.openlocfilehash: e2d149faaa967846244553ed53ed5e8255dae276
ms.sourcegitcommit: 445320ff0ee7323d823194d4ec9cfa6e710ed85d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/11/2019
ms.locfileid: "72282279"
---
# <a name="windows-device-portal-overview"></a>Visão geral do Windows Device Portal

O Windows Device Portal permite que você configure e gerencie seu dispositivo remotamente por uma rede ou conexão USB. Ele também fornece ferramentas de diagnóstico avançadas para ajudá-lo a solucionar problemas e exibir o desempenho em tempo real do seu dispositivo Windows.

O portal do dispositivo Windows é um servidor Web em seu dispositivo ao qual você pode se conectar de um navegador da Web em um PC. Se o dispositivo tiver um navegador da Web, você também poderá se conectar localmente com o navegador nesse dispositivo.

O portal de dispositivos do Windows está disponível em cada família de dispositivos, mas os recursos e a instalação variam de acordo com os requisitos de cada dispositivo. Este artigo fornece uma descrição geral do Windows Device Portal e links para artigos com informações mais específicas para cada família de dispositivos.

A funcionalidade do portal de dispositivo do Windows é implementada com [APIs REST](device-portal-api-core.md) que você pode usar diretamente para acessar dados e controlar seu dispositivo de forma programática.

## <a name="setup"></a>Configuração

Cada dispositivo possui instruções específicas para se conectar ao Device Portal, mas estas etapas gerais são necessárias em todos:

1. Habilite o modo de desenvolvedor e o portal do dispositivo em seu dispositivo (configurado no aplicativo de configurações).

2. Conecte seu dispositivo e computador por meio de uma rede local ou com USB.

3. Navegue até a página do Device Portal em seu navegador. Esta tabela mostra as portas e os protocolos usados por cada família de dispositivos.

Família de dispositivos | Ativado por padrão? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sim, no Modo de Desenvolvedor | 80 (padrão) | 443 (padrão) | http://127.0.0.1:10080
IoT | Sim, no Modo de Desenvolvedor | 8080 | Habilitar por meio da regkey | N/D
Xbox | Habilitar dentro do Modo de Desenvolvedor | Desabilitado | 11443 | N/D
Área de Trabalho| Habilitar dentro do Modo de Desenvolvedor | 50080 @ no__t-0 | 50043 @ no__t-0 | N/D
Phone | Habilitar dentro do Modo de Desenvolvedor | 80| 443 | http://127.0.0.1:10080

\* esse não é sempre o caso, pois o portal do dispositivo no desktop alega portas no intervalo efêmero (> 50000) para evitar colisões com declarações de porta existentes no dispositivo. Para saber mais, consulte a seção [Configurações de porta](device-portal-desktop.md#registry-based-configuration-for-device-portal) para área de trabalho.  

Para obter instruções de instalação específicas ao dispositivo, consulte:

- [Portal de Dispositivos para HoloLens](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-hololens)
- [Portal de Dispositivos para IoT](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Portal de Dispositivos para celulares](device-portal-mobile.md)
- [Portal de Dispositivos para Xbox](../xbox-apps/device-portal-xbox.md)
- [Portal de Dispositivos para Desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## <a name="features"></a>Recursos

### <a name="toolbar-and-navigation"></a>Barra de ferramentas e navegação

A barra de ferramentas na parte superior da página fornece acesso a recursos comumente usados.

- **Energia**: Acessar opções de energia.
  - **Desligamento**: Desliga o dispositivo.
  - **Reiniciar**: Circula a energia no dispositivo.
- **Ajuda**: Abre a página de ajuda.

Use os links no painel de navegação ao lado esquerdo da página para navegar até as ferramentas de gerenciamento e monitoramento disponíveis para seu dispositivo.

As ferramentas que são comuns nas famílias de dispositivos são descritas aqui. Talvez haja outras opções disponíveis dependendo do dispositivo. Para obter mais informações, consulte a página específica para seu tipo de dispositivo.

### <a name="apps-manager"></a>Gerente de aplicativos

O Gerenciador de aplicativos fornece a funcionalidade de instalação/desinstalação e gerenciamento para pacotes de aplicativos e grupos no dispositivo host.

![Página do Gerenciador de aplicativos do portal do dispositivo](images/device-portal/WDP_AppsManager2.png)

* **Implantar aplicativos**: Implante aplicativos empacotados de hosts locais, de rede ou da Web e registre arquivos soltos de compartilhamentos de rede.
* **Aplicativos instalados**: Use o menu suspenso para remover ou iniciar os aplicativos que estão instalados no dispositivo.
* **Aplicativos em execução**: Obtenha informações sobre os aplicativos que estão sendo executados no momento e feche-os conforme necessário.

#### <a name="install-sideload-an-app"></a>Instalar (Sideload) um aplicativo

Você pode Sideload aplicativos durante o desenvolvimento usando o portal de dispositivo do Windows:

1. Depois de criar um pacote de aplicativo, você pode instalá-lo remotamente em seu dispositivo. Após a compilação no Visual Studio, uma pasta de saída será gerada.

    ![Instalação de aplicativos](images/device-portal/iot-installapp0.png)

2. No portal de dispositivos do Windows, navegue até a página do **Gerenciador de aplicativos** .

3. Na seção **implantar aplicativos** , selecione **armazenamento local**.

4. Em **selecionar o pacote de aplicativos**, selecione **escolher arquivo** e navegue até o pacote do aplicativo que você deseja Sideload.

5. Em **Selecionar arquivo de certificado (. cer) usado para assinar o pacote do aplicativo**, selecione **escolher arquivo** e navegue até o certificado associado a esse pacote do aplicativo.

6. Marque as caixas respectivas se desejar instalar pacotes opcionais ou de estrutura junto com a instalação do aplicativo e selecione **Avançar** para escolher.

7. Selecione **instalar** para iniciar a instalação.

8. Se o dispositivo estiver executando o Windows 10 no modo S e for a primeira vez que o certificado fornecido foi instalado no dispositivo, reinicie o dispositivo.

#### <a name="install-a-certificate"></a>Instalar um certificado

Como alternativa, você pode instalar o certificado por meio do portal do dispositivo do Windows e instalar o aplicativo por outros meios:

1. No portal de dispositivos do Windows, navegue até a página do **Gerenciador de aplicativos** .

2. Na seção **implantar aplicativos** , selecione **Instalar certificado**.

3. Em **Selecionar arquivo de certificado (. cer) usado para assinar um pacote de aplicativo**, selecione **escolher arquivo** e navegue até o certificado associado ao pacote do aplicativo que você deseja Sideload.

4. Selecione **instalar** para iniciar a instalação.

5. Se o dispositivo estiver executando o Windows 10 no modo S e for a primeira vez que o certificado fornecido foi instalado no dispositivo, reinicie o dispositivo.

#### <a name="uninstall-an-app"></a>Desinstalar um aplicativo

1. Certifique-se de que seu aplicativo não esteja em execução.
2. Se for, vá para **executando aplicativos** e feche-o. Se você tentar desinstalar o enquanto o aplicativo estiver em execução, ele causará problemas quando você tentar reinstalar o aplicativo.
3. Selecione o aplicativo na lista suspensa e clique em **remover**.

### <a name="running-processes"></a>Processos em execução

Esta página mostra detalhes sobre os processos em execução no momento no dispositivo host. Isso inclui aplicativos e processos do sistema. Em algumas plataformas (desktop, IoT e HoloLens), você pode encerrar processos.

![Página de processos em execução do portal do dispositivo](images/device-portal/mob-device-portal-processes.png)

### <a name="file-explorer"></a>Explorador de Arquivos

Esta página permite que você exiba e manipule arquivos armazenados por qualquer aplicativo do Sideload. Consulte a postagem do blog [usando o explorador de arquivos do aplicativo](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) para saber mais sobre o explorador de arquivos e como usá-lo.

![Página Gerenciador de arquivos do portal do dispositivo](images/device-portal/mob-device-portal-AppFileExplorer.png)

### <a name="performance"></a>Desempenho

A página desempenho mostra grafos em tempo real de informações de diagnóstico do sistema, como uso de energia, taxa de quadros e carga de CPU.

Estas são as métricas disponíveis:

- **CPU**: Percentual da utilização total da CPU disponível
- **Memória**: Total, em uso, disponível, confirmado, paginado e não paginado
- **E/S**: Ler e gravar quantidades de dados
- **Rede**: Dados recebidos e enviados
- **GPU**: Percentual da utilização total do mecanismo de GPU disponível

![Página de desempenho do portal do dispositivo](images/device-portal/mob-device-portal-perf.png)

### <a name="event-tracing-for-windows-etw-logging"></a>Log do ETW (rastreamento de eventos para Windows)

A página de log do ETW gerencia informações de ETW (rastreamento de eventos para Windows) em tempo real no dispositivo.

![Página de log do ETW do portal do dispositivo](images/device-portal/mob-device-portal-etw.png)

Marque **Ocultar provedores** para mostrar apenas a lista de eventos.

- **Provedores registrados**: Selecione o provedor de eventos e o nível de rastreamento. O nível de rastreamento é um destes valores:
  1. Saída anormal ou encerramento
  2. Erros graves
  3. Avisos
  4. Avisos que não são de erro
  5. Rastreamento detalhado

  Clique ou toque em **Habilitar** para iniciar o rastreamento. O provedor é adicionado à lista suspensa **Provedores Habilitados**.
- **Provedores personalizados**: Selecione um provedor de ETW personalizado e o nível de rastreamento. Identifique o provedor pelo GUID. Não inclua colchetes no GUID.
- **Provedores habilitados**: Isso lista os provedores habilitados. Selecione um provedor da lista suspensa e clique ou toque em **Desabilitar** para parar o rastreamento. Clique ou toque em **Parar todos** para suspender todo o rastreamento.
- **Histórico de provedores**: Isso mostra os provedores de ETW que foram habilitados durante a sessão atual. Clique ou toque em **Habilitar** para ativar um provedor que foi desabilitado. Clique ou toque em **Limpar** para limpar o histórico.
- **Filtros/eventos**: A seção de **eventos** lista os eventos ETW dos provedores selecionados em formato de tabela. A tabela é atualizada em tempo real. Use o menu **filtros** para configurar filtros personalizados para os quais os eventos serão exibidos. Clique no botão **limpar** para excluir todos os eventos ETW da tabela. Isso não desabilita os provedores. Você pode clicar em **salvar em arquivo** para exportar os eventos ETW coletados no momento para um arquivo CSV local.

Para obter mais detalhes sobre como usar o log ETW, consulte a postagem do blog [usar o portal do dispositivo para exibir logs de depuração](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) .

### <a name="performance-tracing"></a>Rastreamento de desempenho

A página rastreamento de desempenho permite exibir os rastreamentos do [gravador de desempenho do Windows (WPR)](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448205(v=win.10)) do dispositivo host.

![Página de rastreamento de desempenho do portal do dispositivo](images/device-portal/mob-device-portal-perf-tracing.png)

- **Perfis disponíveis**: Selecione o perfil WPR na lista suspensa e clique ou toque em **Iniciar** para iniciar o rastreamento.
- **Perfis personalizados**: Clique ou toque em **procurar** para escolher um perfil de WPR do seu PC. Clique ou toque em **Carregar e iniciar** para iniciar o rastreamento.

Para interromper o rastreamento, clique em **Parar**. Permaneça nesta página até o arquivo de rastreamento (. ETL) concluiu o download.

Realizadas. Os arquivos ETL podem ser abertos para análise no [analisador de desempenho do Windows](https://docs.microsoft.com/previous-versions/windows/it-pro/windows-8.1-and-8/hh448170(v=win.10)).

### <a name="device-manager"></a>Gerenciador de dispositivos

A página Gerenciador de dispositivos enumera todos os periféricos anexados ao seu dispositivo. Você pode clicar nos ícones de configurações para exibir as propriedades de cada um.

![Página do Gerenciador de dispositivos do portal do dispositivo](images/device-portal/mob-device-portal-devices.png)

### <a name="networking"></a>Rede

A página rede gerencia as conexões de rede no dispositivo. A menos que você esteja conectado ao portal do dispositivo por meio de USB, a alteração dessas configurações provavelmente será desconectada do portal do dispositivo.

- **Redes disponíveis**: Mostra as redes WiFi disponíveis para o dispositivo. Clicar ou tocar em uma rede permitirá que você se conecte a ela e forneça uma chave de acesso, se necessário. O portal do dispositivo ainda não dá suporte à autenticação corporativa. Você também pode usar o menu suspenso **perfis** para tentar se conectar a qualquer um dos perfis de WiFi conhecidos pelo dispositivo.
- **Configuração de IP**: Mostra informações de endereço sobre cada uma das portas de rede do dispositivo host.

![Página rede do portal do dispositivo](images/device-portal/mob-device-portal-network.png)

## <a name="service-features-and-notes"></a>Recursos e observações sobre o serviço

### <a name="dns-sd"></a>DNS-SD

O Device Portal anuncia sua presença na rede local usando DNS-SD. Todas as instâncias do Device Portal, independentemente do tipo de dispositivo, anunciam em "WDP._wdp._tcp.local". Os registros TXT para a instância do serviço fornecem o seguinte:

Chave | type | Descrição
----|------|-------------
S | int | Porta segura para o Device Portal. Se 0 (zero), o Device Portal não está escutando conexões HTTPS.
D | cadeia de caracteres | Tipo de dispositivo. Isso estará no formato "Windows. *", por exemplo, Windows. Xbox ou Windows. desktop
A | cadeia de caracteres | Arquitetura do dispositivo. Isso será ARM, x86 ou AMD64.  
T | lista de cadeias de caracteres delineadas de caracteres nulos | Marcas aplicadas pelo usuário para o dispositivo. Consulte as Marcas API de REST para saber como usá-las. A lista é encerrada com duplo nulo.  

É recomendável estabelecer conexão na porta HTTPS, pois nem todos os dispositivos estão ouvindo na porta HTTP anunciada pelo registro DNS SD.

### <a name="csrf-protection-and-scripting"></a>Proteção contra CSRF e scripts

Para proteger-se contra [ataques CSRF](https://en.wikipedia.org/wiki/Cross-site_request_forgery), um token exclusivo é necessário em todas as solicitações não GET. Esse token, o cabeçalho de solicitação X-CSRF-Token, é derivado de um cookie de sessão, o CSRF-Token. Na interface do usuário Web do Device Portal, o cookie CSRF-Token é copiado no cabeçalho X-CSRF-Token em cada solicitação.

> [!IMPORTANT]
> Essa proteção impede o uso das APIs REST de um cliente autônomo (como utilitários de linha de comando). Isso pode ser resolvido de três modos:
> - Use um nome de usuário "auto-". Os clientes que têm o prefixo "auto-" no nome de usuário serão ignorados pela proteção contra CSRF. É importante que esse nome de usuário não seja usado para fazer logon no Device Portal por meio do navegador, pois ele abrirá o serviço para ataques CSRF. Exemplo: Se o nome de usuário do portal do dispositivo for "admin", ```curl -u auto-admin:password <args>``` deverá ser usado para ignorar a proteção CSRF.
> - Implemente o esquema cookie-para-cabeçalho no cliente. Isso exige uma solicitação GET para estabelecer o cookie de sessão e, em seguida, a inclusão do cabeçalho e o cookie em todas as solicitações subsequentes.
> - Desabilite a autenticação e use HTTP. A proteção contra CSRF só se aplica a pontos de extremidade HTTPS, portanto, as conexões em pontos de extremidade HTTP não precisam fazer nada do que é mencionado acima.

#### <a name="cross-site-websocket-hijacking-cswsh-protection"></a>Proteção de Cross-Site WebSocket Hijacking (CSWSH)

Para se proteger contra [ataques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos os clientes que abrem uma conexão WebSocket com o Device Portal também devem fornecer um cabeçalho Origin que corresponda ao cabeçalho Host. Isso prova para o Device Portal que a solicitação vem da interface do usuário do Device Portal ou um aplicativo cliente válido. Sem o cabeçalho Origin, sua solicitação será rejeitada.
