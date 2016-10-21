---
author: mcleblanc
ms.assetid: 60fc48dd-91a9-4dd6-a116-9292a7c1f3be
title: "Visão geral do Windows Device Portal"
description: "Saiba como o Windows Device Portal permite que você configure e gerencie seu dispositivo remotamente por uma rede ou conexão USB."
translationtype: Human Translation
ms.sourcegitcommit: 7f6aba331ba27d2c0c2ca7925c452da58e155cb8
ms.openlocfilehash: b316eab1f269dadbe65b7e93b5a33a8e4c4924d7

---
# Visão geral do Windows Device Portal

O Windows Device Portal permite que você configure e gerencie seu dispositivo remotamente por uma rede ou conexão USB. Ele também fornece ferramentas avançadas de diagnóstico para ajudar você a solucionar problemas e exibir o desempenho do seu dispositivo Windows em tempo real.

O Device Portal é um servidor Web em seu dispositivo ao qual você pode se conectar usando um navegador da Web em seu computador. Se o dispositivo tiver um navegador da Web, você também poderá se conectar localmente com o navegador em seu dispositivo.

O Windows Device Portal está disponível em todas as famílias de dispositivos, mas os recursos e a instalação variam com base nos requisitos do dispositivo. Este artigo fornece uma descrição geral do Windows Device Portal e links para artigos com informações mais específicas para cada família de dispositivos.

Tudo no Windows Device Portal foi criado com base em [APIs REST](device-portal-api-core.md), que você pode usar para acessar os dados e controlar seu dispositivo de forma programática.

## Instalação

Cada dispositivo possui instruções específicas para se conectar ao Device Portal, mas estas etapas gerais são necessárias em todos:
1. Habilite o Modo de desenvolvedor e o Device Portal em seu dispositivo.
2. Conecte o dispositivo e o PC via rede local ou USB.
3. Navegue até a página do Device Portal em seu navegador. Esta tabela mostra as portas e os protocolos usados em cada família de dispositivos.

Família de dispositivos | Ativado por padrão? | HTTP | HTTPS | USB
--------------|----------------|------|-------|----
HoloLens | Sim, no Modo de desenvolvimento | 80 (padrão) | 443 (padrão) | localhost:10080
IoT | Sim, no Modo de desenvolvimento | 8080 | Habilitar por meio da regkey | N/D
Xbox | Habilitar dentro do Modo de desenvolvimento | Desabilitado | 11443 | N/D
Área de trabalho| Habilitar dentro do Modo de desenvolvimento | Aleatório > 50.000 (xx080) | Aleatório > 50.000 (xx443) | N/D
Phone | Habilitar dentro do Modo de desenvolvimento | 80| 443 | localhost:10080

Para obter instruções de instalação específicas do dispositivo, consulte:
- [Device Portal para HoloLens](https://dev.windows.com/holographic/using_the_windows_device_portal)
- [Device Portal para IoT](https://go.microsoft.com/fwlink/?LinkID=616499)
- [Device Portal para celulares](device-portal-mobile.md#set-up-device-portal-on-window-phone)
- [Device Portal para Xbox](device-portal-xbox.md)
- [Device Portal para desktop](device-portal-desktop.md#set-up-device-portal-on-windows-desktop)

## Recursos

### Barra de ferramentas e navegação

A barra de ferramentas na parte superior da página fornece acesso a recursos e status normalmente usados.
- **Desligar**: desativa o dispositivo.
- **Reiniciar**: repete o ciclo de energia no dispositivo.
- **Ajuda**: abre a página de ajuda.

Use os links no painel de navegação ao lado esquerdo da página para navegar até as ferramentas de gerenciamento e monitoramento disponíveis para seu dispositivo.

As ferramentas que são comuns em todos os dispositivos estão descritas aqui. Talvez haja outras opções disponíveis dependendo do dispositivo. Para saber mais, consulte a página específica para o seu dispositivo.

### Página inicial

A sessão do Device Portal é iniciada na home page. Normalmente, a home page traz informações sobre o dispositivo, como nome e versão do sistema operacional, e preferências que você pode definir para o dispositivo.

### Aplicativos

Fornece funcionalidade de gerenciamento e de instalação/desinstalação para pacotes AppX em seu dispositivo.

![Device Portal para celulares](images/device-portal/mob-device-portal-apps.png)

- **Aplicativos instalados**: remove e inicia aplicativos.
- **Aplicativos em execução**: lista os aplicativos que estão em execução no momento.
- **Instalar aplicativo**: seleciona pacotes de aplicativos para a instalação a partir de uma pasta em seu computador ou rede.
- **Dependência**: adiciona dependências para o aplicativo que você pretende instalar.
- **Implantar**: implanta o aplicativo selecionado e as dependências em seu dispositivo.

**Para instalar um aplicativo**

1.  Quando tiver [criado um pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/xaml/hh454036(v=vs.140).aspx), você poderá instalá-lo remotamente em seu dispositivo. Após a compilação no Visual Studio, uma pasta de saída será gerada.

    ![Instalação de aplicativos](images/device-portal/iot-installapp0.png)
2.  Clique em Procurar e localize o pacote do aplicativo (.appx).
3.  Clique em Procurar e localize o arquivo de certificado (.cer). (Não é necessário em todos os dispositivos.)
4.  Adicione dependências. Se você tiver mais de uma, adicione cada uma delas individualmente.     
5.  Em **Implantar**, clique em **Ir**. 
6.  Para instalar outro aplicativo, clique no botão **Redefinir** para limpar os campos..


**Para desinstalar um aplicativo**

1.  Certifique-se de que seu aplicativo não esteja em execução. 
2.  Se estiver, vá para 'aplicativos em execução' e feche-o. Se você tentar desinstalar enquanto o aplicativo estiver em execução, isso causará problemas ao tentar instalar o aplicativo novamente. 
3.  Assim que estiver pronto, clique em **Desinstalar**.

### Processos

Mostra detalhes sobre processos em execução no momento. Isso inclui aplicativos e processos do sistema.

Semelhante ao Gerenciador de Tarefas em seu computador, essa página permite que você veja quais processos estão sendo executados e o uso da memória.  Em algumas plataformas (área de trabalho, IoT e HoloLens), você pode encerrar processos.

![Device Portal para celulares](images/device-portal/mob-device-portal-processes.png)

### Desempenho

Mostra gráficos em tempo real de informações de diagnóstico do sistema, como o uso de energia, a taxa de quadros e a carga da CPU.

Estas são as métricas disponíveis:
- **CPU**: porcentagem do total disponível
- **Memória**: total, em uso, disponível confirmada, paginada e não paginada
- **GPU**: utilização do mecanismo GPU, porcentagem do total disponível
- **E/S**: lê e grava
- **Rede**: recepção e envio

![Device Portal para celulares](images/device-portal/mob-device-portal-perf.png)

### ETW (Rastreamento de Eventos para Windows)

Gerencia o ETW (Rastreamento de Eventos para Windows) em tempo real no dispositivo.

![Device Portal para celulares](images/device-portal/mob-device-portal-etw.png)

Marque **Ocultar provedores** para mostrar apenas a lista de eventos.
- **Provedores registrados**: seleciona o provedor ETW e o nível de rastreamento. O nível de rastreamento é um destes valores:
    1. Saída anormal ou encerramento
    2. Erros graves
    3. Avisos
    4. Avisos que não são de erro
    5. Rastreamento detalhado (*)

Clique ou toque em **Habilitar** para iniciar o rastreamento. O provedor é adicionado à lista suspensa **Provedores Habilitados**.
- **Provedores personalizados**: selecione um provedor ETW personalizado e o nível de rastreamento. Identifique o provedor pelo GUID. Não inclua colchetes no GUID.
- **Provedores habilitados**: lista os provedores habilitados. Selecione um provedor da lista suspensa e clique ou toque em **Desabilitar** para parar o rastreamento. Clique ou toque em **Parar todos** para suspender todo o rastreamento.
- **Histórico de provedores**: mostra os provedores ETW que foram habilitados durante a sessão atual. Clique ou toque em **Habilitar** para ativar um provedor que foi desabilitado. Clique ou toque em **Limpar** para limpar o histórico.
- **Eventos**: lista os eventos ETW dos provedores selecionados no formato de tabela. Essa tabela é atualizada em tempo real. Abaixo da tabela, clique no botão **Limpar** para excluir todos os eventos ETW da tabela. Isso não desabilita os provedores. Você pode clicar em **Salvar no arquivo** para exportar os atuais eventos ETW coletados para um arquivo CSV localmente.

Para obter mais detalhes sobre como usar o rastreamento ETW, consulte o [blogpost](https://blogs.windows.com/buildingapps/2016/06/10/using-device-portal-to-view-debug-logs-for-uwp/) sobre uso para coletar logs em tempo real do seu aplicativo. 

### Rastreamento de desempenho

Capture rastreamentos do [Windows Performance Recorder](https://msdn.microsoft.com/library/windows/hardware/hh448205.aspx) (WPR) no seu dispositivo.

![Device Portal para celulares](images/device-portal/mob-device-portal-perf-tracing.png)

- **Perfis disponíveis**: selecione o perfil WPR na lista suspensa e clique ou toque em **Iniciar** para iniciar o rastreamento.
- **Perfis personalizados**: clique ou toque em **Procurar** para escolher um perfil WPR do seu computador. Clique ou toque em **Carregar e iniciar** para iniciar o rastreamento.

Para interromper o rastreamento, clique em **Parar**. Fique nesta página até que o download do arquivo de rastreamento (.ETL) seja concluído.

Os arquivos ETL capturados podem ser abertos para análise no [Windows Performance Analyzer](https://msdn.microsoft.com/library/windows/hardware/hh448170.aspx).

### Dispositivos

Enumera todos os periféricos conectados ao seu dispositivo.

![Device Portal para celulares](images/device-portal/mob-device-portal-devices.png)

### Rede

Gerencia conexões de rede no dispositivo.  A menos que esteja conectado ao Device Portal via USB, alterar essas configurações provavelmente desconectarão você do Device Portal.
- **Perfis**: seleciona um perfil diferente de Wi-Fi para ser usado.  
- **Redes disponíveis**: as redes de Wi-Fi disponíveis para o dispositivo. Clicar ou tocar em uma rede permitirá que você se conecte a ela e forneça uma chave de acesso, se necessário. Observação: o Device Portal ainda não oferece suporte à autenticação empresarial. 

![Device Portal para celulares](images/device-portal/mob-device-portal-network.png)

### Aplicativo Explorador de Arquivos

Permite que você exiba e manipule arquivos armazenados por seus aplicativos de sideload.  É uma nova versão de várias plataformas do [Isolated Storage Explorer](https://msdn.microsoft.com/library/windows/apps/hh286408(v=vs.105).aspx) do Windows Phone 8.1  Veja [esta postagem do blog](https://blogs.windows.com/buildingapps/2016/06/08/using-the-app-file-explorer-to-see-your-app-data/) para saber mais sobre o Aplicativo Explorador de Arquivos e como usá-lo. 

![Device Portal para celulares](images/device-portal/mob-device-portal-AppFileExplorer.png)

## Notas e recursos de serviço

### DNS-SD

O Device Portal anuncia sua presença na rede local usando DNS-SD.  Todas as instâncias do Device Portal, independentemente do tipo de dispositivo, anunciam em "WDP._wdp._tcp.local". Os registros TXT para a instância do serviço fornecem o seguinte:

Chave | Tipo | Descrição 
----|------|-------------
S | int | Porta segura para o Device Portal.  Se 0 (zero), o Device Portal não está escutando conexões HTTPS. 
D | string | Tipo de dispositivo.  Isso estará no formato "Windows.*", por exemplo, Windows.Xbox ou Windows.Desktop
A | string | Arquitetura do dispositivo.  Isso será ARM, x86 ou AMD64.  
T | lista de cadeias de caracteres delineadas de caracteres nulos | Marcas aplicadas pelo usuário para o dispositivo. Consulte as Marcas API de REST para saber como usá-las. A lista é encerrada com duplo nulo.  

É recomendável estabelecer conexão na porta HTTPS, pois nem todos os dispositivos estão ouvindo na porta HTTP anunciada pelo registro DNS SD. 

### Proteção contra CSRF e scripts

Para proteger-se contra [ataques CSRF](https://wikipedia.org/wiki/Cross-site_request_forgery), um token exclusivo é necessário em todas as solicitações não GET. Esse token, o cabeçalho de solicitação X-CSRF-Token, é derivado de um cookie de sessão, o CSRF-Token. Na interface do usuário Web do Device Portal, o cookie CSRF-Token é copiado no cabeçalho X-CSRF-Token em cada solicitação.

**Importante** Essa proteção impede o uso das APIs REST a partir de um cliente autônomo (por exemplo, utilitários de linha de comando). Isso pode ser resolvido de três modos: 

1. Uso do nome de usuário "auto-". Os clientes que têm o prefixo "auto-" no nome de usuário serão ignorados pela proteção contra CSRF. É importante que esse nome de usuário não seja usado para fazer logon no Device Portal por meio do navegador, pois ele abrirá o serviço para ataques CSRF. Exemplo: se o nome de usuário do Device Portal for "admin", ```curl -u auto-admin:password <args>``` deverá ser usado para ignorar a proteção contra CSRF. 

2. Implemente o esquema cookie-para-cabeçalho no cliente. Isso exige uma solicitação GET para estabelecer o cookie de sessão e, em seguida, a inclusão do cabeçalho e o cookie em todas as solicitações subsequentes. 
 
3. Desabilite a autenticação e use HTTP. A proteção contra CSRF só se aplica a pontos de extremidade HTTPS, portanto, as conexões em pontos de extremidade HTTP não precisam fazer nada do que é mencionado acima. 

**Observação**: um nome de usuário que começar com "auto-" não será capaz de fazer logon no Device Portal por meio do navegador.  

#### Proteção de Cross-Site WebSocket Hijacking (CSWSH)

Para se proteger contra [ataques CSWSH](https://www.christian-schneider.net/CrossSiteWebSocketHijacking.html), todos os clientes que abrem uma conexão WebSocket com o Device Portal também devem fornecer um cabeçalho Origin que corresponda ao cabeçalho Host.  Isso prova para o Device Portal que a solicitação vem da interface do usuário do Device Portal ou um aplicativo cliente válido.  Sem o cabeçalho Origin, sua solicitação será rejeitada. 



<!--HONumber=Aug16_HO5-->


