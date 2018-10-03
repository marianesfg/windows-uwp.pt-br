---
Description: The JavaScript API for the Microsoft Take a Test app allows you to do secure assessments. Take a Test provides a secure browser that prevents students from using other computer or internet resources during a test.
title: API JavaScript Fazer um Teste.
author: PatrickFarley
ms.author: pafarley
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
ms.date: 08/08/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, educação
ms.localizationpriority: medium
ms.openlocfilehash: 38596ad12ac309db5dc60e4a5183eee9bf8c7b7c
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4259003"
---
# <a name="take-a-test-javascript-api"></a>API JavaScript Fazer um Teste

[Fazer um teste](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) é um aplicativo UWP com base em navegador que renderiza avaliações online bloqueado para testes de alto risco, permitindo que os educadores concentre-se a avaliação de conteúdo em vez de como fornecer um ambiente de teste seguro. Para conseguir isso, ele usa uma API JavaScript que qualquer aplicativo Web pode utilizar. A API Fazer um Teste oferece suporte ao [padrão da API do navegador SBAC](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf) para testes decisivos comuns importantes.

Consulte a [Referência técnica do app Fazer um Teste](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) para obter mais informações sobre o próprio app. Para obter ajuda de solução de problemas, consulte [Solução de problemas com Fazer um Teste da Microsoft usando o visualizador de eventos](troubleshooting.md).

## <a name="reference-documentation"></a>Documentação de referência
A API Fazer um Teste existe nos seguintes namespaces. Observe que todas as APIs dependem de um objeto `SecureBrowser` global.

| Namespace | Descrição |
|-----------|-------------|
|[namespace de segurança](#security-namespace)|Contém APIs que permitem bloquear o dispositivo para testar e impor um ambiente de teste. |

### <a name="security-namespace"></a>Namespace de segurança

O namespace de segurança permite bloquear o dispositivo, verificar a lista de processos de usuário e do sistema, obter endereços MAC e IP e limpar recursos da web em cache.

| Método | Descrição   |
|--------|---------------|
|[lockDown](#lockDown) | Bloqueia o dispositivo para teste. |
|[isEnvironmentSecure](#isEnvironmentSecure) | Determina se o contexto de bloqueio continua aplicado ao dispositivo. |
|[getDeviceInfo](#getDeviceInfo) | Obtém detalhes sobre a plataforma em que o aplicativo de teste é executado. |
|[examineProcessList](#examineProcessList)|Obtém a lista de processos de usuário e sistema em execução.|
|[close](#close) | Fecha o navegador e desbloqueia o dispositivo. |
|[getPermissiveMode](#getPermissiveMode)|Verifica se o modo permissivo está ativado ou desativado.|
|[setPermissiveMode](#setPermissiveMode)|Ativa ou desativa o modo permissivo.|
|[emptyClipBoard](#emptyClipBoard)|Limpa a área de transferência do sistema.|
|[getMACAddress](#getMACAddress)|Obtém a lista de endereços MAC do dispositivo.|
|[getStartTime](#getStartTime) | Obtém a hora em que o app de teste foi iniciado. |
|[getCapability](#getCapability) | Verifica se uma funcionalidade está habilitada ou desabilitada. |
|[setCapability](#setCapability)|Habilita ou desabilita a funcionalidade especificada.| 
|[isRemoteSession](#isRemoteSession) | Verifica se a sessão atual está conectada remotamente. |
|[isVMSession](#isVMSession) | Verifica se a sessão atual está em execução em uma máquina virtual. |

---

<span id="lockDown"/>

### <a name="lockdown"></a>lockDown
Bloqueia o dispositivo. Também usado para desbloquear o dispositivo. O aplicativo Web de teste invocará esta chamada antes de permitir que os alunos comecem o teste. O implementador é necessário para executar quaisquer ações necessárias para proteger o ambiente de teste. As etapas executadas para proteger o ambiente são específicas de dispositivo e, por exemplo, incluem aspectos como desabilitar capturas de tela, desabilitar chat por voz quando estiver em modo seguro, limpar a área de transferência do sistema, entrar em um modo de quiosque, desabilitar Espaços nos dispositivos OSX 10.7 + etc. O aplicativo de teste habilitará o bloqueio antes do início de uma avaliação e desabilitará o bloqueio quando o aluno concluir a avaliação e estiver fora do teste seguro.

**Sintaxe**  
`void SecureBrowser.security.lockDown(Boolean enable, Function onSuccess, Function onError);`

**Parâmetros**  
* `enable` - **true** para executar o app Fazer um Teste acima da tela de bloqueio e aplicar políticas discutidas neste [documento](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). **false** interrompe a execução de Fazer um Teste acima da tela de bloqueio e o fecha, a menos que o app não esteja bloqueado; nesse caso, não haverá nenhum efeito.  
* `onSuccess` - [opcional] A função a ser chamada após o bloqueio foi habilitada ou desabilitada com êxito. Ela deve estar no formato `Function(Boolean currentlockdownstate)`.  
* `onError` - [opcional] A função a ser chamada em caso de falha na operação de bloqueio. Ela deve estar no formato `Function(Boolean currentlockdownstate)`.  

**Requisitos**  
Windows 10, versão 1709

---

<span id="isEnvironmentSecure" />

### <a name="isenvironmentsecure"></a>isEnvironmentSecure
Determina se o contexto de bloqueio continua aplicado ao dispositivo. O aplicativo Web de teste invocará esta chamada antes de permitir que os alunos comecem o teste e periodicamente quando eles estiverem no teste.

**Sintaxe**  
`void SecureBrowser.security.isEnvironmentSecure(Function callback);`

**Parâmetros**  
* `callback` - A função a ser chamada quando esta função for concluída. Ela deve estar no formato `Function(String state)`, em que `state` é uma cadeia de caracteres JSON que contém dois campos. O primeiro é o campo `secure`, que mostrará `true` somente se todos os bloqueios necessários tiverem sido habilitados (ou os recurso tiverem sido desabilitados) para permitir um ambiente de teste seguro e nenhum deles tiver sido comprometido desde que o app entrou no modo de bloqueio. O campo, `messageKey`, inclui outros detalhes ou informações específicas do fornecedor. O objetivo aqui é permitir que os fornecedores insiram informações adicionais que ampliam o sinalizador booliano `secure`:

```JSON
{
    'secure' : "true/false",
    'messageKey' : "some message"
}
```

**Requisitos**  
Windows 10, versão 1709

---

<span id="getDeviceInfo" />

### <a name="getdeviceinfo"></a>getDeviceInfo
Obtém detalhes sobre a plataforma em que o aplicativo de teste é executado. É usado para ampliar qualquer informação perceptível no agente do usuário.

**Sintaxe**  
`void SecureBrowser.security.getDeviceInfo(Function callback);`

**Parâmetros**  
* `callback` - A função a ser chamada quando esta função for concluída. Ela deve estar no formato `Function(String infoObj)`, em que `infoObj` é uma cadeia de caracteres JSON que contém vários campos. Há suporte para os seguintes campos:
    * `os` representa o tipo de SO (por exemplo, Windows, macOS, Linux, iOS, Android etc.)
    * `name` representa o nome da versão do SO, se houver (por exemplo, Sierra, Ubuntu).
    * `version` representa a versão do SO (por exemplo, 10,1, 10 Pro etc.)
    * `brand` representa a identidade visual do navegador seguro (por exemplo, OAKS, CA, SmarterApp etc.)
    * `model` representa o modelo de dispositivo somente para dispositivos móveis; nulo/não utilizado para navegadores de desktop.

**Requisitos**  
Windows 10, versão 1709

---

<span id="examineProcessList" />

### <a name="examineprocesslist"></a>examineProcessList
Obtém a lista de todos os processos em execução no computador cliente pertencente ao usuário. O aplicativo de teste invocará este método para examinar a lista e compará-lo com uma lista de processos que foram considerados na lista de bloqueios durante o ciclo de teste. Essa chamada deve ser invocada no início de uma avaliação e periodicamente enquanto o aluno faz a avaliação. Se um processo não autorizado for detectado, a avaliação deverá ser interrompida para preservar a integridade do teste.

**Sintaxe**  
`void SecureBrowser.security.examineProcessList(String[] blacklistedProcessList, Function callback);`

**Parâmetros**  
* `blacklistedProcessList` - A lista de processos que o aplicativo de teste não autorizou.  
`callback` - A função a ser invocada quando os processos ativos foram encontrados. Deve estar no formato: `Function(String foundBlacklistedProcesses)` em que `foundBlacklistedProcesses` está no formato: `"['process1.exe','process2.exe','processEtc.exe']"`. Ele estará vazio se nenhum processo não autorizado tiver sido encontrado. Se for nulo, isso indica que ocorreu um erro na chamada da função original.

**Comentários** A lista não inclui processos do sistema.

**Requisitos**  
Windows 10, versão 1709

---

<span id="close"/>

### <a name="close"></a>close
Fecha o navegador e desbloqueia o dispositivo. O aplicativo de teste deve invocar este parâmetro quando o usuário optar por sair do navegador.

**Sintaxe**  
`void SecureBrowser.security.close(restart);`

**Parâmetros**  
* `restart` - Este parâmetro é ignorado, mas deve ser fornecido.

**Comentários** No Windows 10, versão 1607, o dispositivo deve estar inicialmente bloqueado. Em versões posteriores, este método fecha o navegador, não importando se o dispositivo está bloqueado.

**Requisitos**  
Windows 10, versão 1709

---

<span id="getPermissiveMode" />

### <a name="getpermissivemode"></a>getPermissiveMode
O aplicativo Web de teste deve invocar este parâmetro para determinar se o modo permissivo está ativado ou desativado. No modo permissivo, espera-se que um navegador alivie alguns de seus ganchos de segurança rigorosos para permitir que a tecnologia adaptativa trabalhe com o navegador seguro. Por exemplo, os navegadores que impedem agressivamente que outras interfaces do usuário de aplicativos sejam executadas sobre eles talvez queiram aliviar isso quando estiverem no modo permissivo. 

**Sintaxe**  
`void SecureBrowser.security.getPermissiveMode(Function callback)`

**Parâmetros**  
* `callback` - A função a ser invocada quando esta chamada é concluída. Deve estar no formato: `Function(Boolean permissiveMode)`, em que `permissiveMode` indica se o navegador está atualmente no modo permissivo. Se for nulo ou não definido, ocorreu um erro na operação de obtenção.

**Requisitos**  
Windows 10, versão 1709

---

<span id="setPermissiveMode" />

### <a name="setpermissivemode"></a>setPermissiveMode
O aplicativo Web de teste deve invocar este parâmetro para ativar ou desativar o modo permissivo. No modo permissivo, espera-se que um navegador alivie alguns de seus ganchos de segurança rigorosos para permitir que a tecnologia adaptativa trabalhe com o navegador seguro. Por exemplo, os navegadores que impedem agressivamente que outras interfaces do usuário de aplicativos sejam executadas sobre eles talvez queiram aliviar isso quando estiverem no modo permissivo. 

**Sintaxe**  
`void SecureBrowser.security.setPermissiveMode(Boolean enable, Function callback)`

**Parâmetros**  
* `enable` - O valor booliano que indica o status do modo permissivo pretendido.  
* `callback` - A função a ser invocada quando esta chamada é concluída. Deve estar no formato: `Function(Boolean permissiveMode)`, em que `permissiveMode` indica se o navegador está atualmente no modo permissivo. Se for nulo ou não definido, ocorreu um erro na operação de conjuntos.

**Requisitos**  
Windows 10, versão 1709

---

<span id="emptyClipBoard"/>

### <a name="emptyclipboard"></a>emptyClipBoard
Limpa a área de transferência do sistema. O aplicativo de teste deve invocar este parâmetro para forçar a limpeza de todos os dados que possam estar armazenados na área de transferência do sistema. A função **[lockDown](#lockDown)** também executa essa operação.

**Sintaxe**  
`void SecureBrowser.security.emptyClipBoard();`

**Requisitos**  
Windows 10, versão 1709

---

<span id="getMACAddress" />

### <a name="getmacaddress"></a>getMACAddress
Obtém a lista de endereços MAC do dispositivo. O aplicativo de teste deve invocar este parâmetro para facilitar o diagnóstico. 

**Sintaxe**  
`void SecureBrowser.security.getMACAddress(Function callback);`

**Parâmetros**  
* `callback` - A função a ser invocada quando esta chamada é concluída. Deve estar no formato: `Function(String addressArray)`, em que `addressArray` está no formato: `"['00:11:22:33:44:55','etc']"`.

**Comentários**  
É difícil contar com endereços IP de origem para fazer a distinção entre os computadores de usuário final nos servidores de teste, pois os firewalls/NATs/Proxies estão geralmente em uso nas escolas. Os endereços MAC permitem que o app faça a distinção de computadores de cliente final atrás de um firewall comum para fins de diagnóstico.

**Requisitos**  
Windows 10, versão 1709

---

<span id="getStartTime" />

### <a name="getstarttime"></a>getStartTime
Obtém a hora em que o app de teste foi iniciado.

**Sintaxe**  
`DateTime SecureBrowser.settings.getStartTime();`

**Retorno**  
Um objeto DateTime que indica a hora em que o app de teste foi iniciado.

**Requisitos**  
Windows 10, versão 1709

---

<span id="getCapability"/>

### <a name="getcapability"></a>getCapability
Verifica se uma funcionalidade está habilitada ou desabilitada. 

**Sintaxe**  
`Object SecureBrowser.security.getCapability(String feature)`

**Parâmetros**  
`feature` - A cadeia de caracteres que determinará qual funcionalidade será consultada. As cadeias de caracteres de funcionalidade válidas são "screenMonitoring", "printing" e "textSuggestions" (não diferencia maiúsculas de minúsculas).

**Valor de retorno**  
Esta função retorna um objeto JavaScript ou literal com o formato: `{<feature>:true|false}`. **true** se a funcionalidade consultada estiver habilitada; **false** se a funcionalidade não estiver habilitada ou a cadeia de caracteres de funcionalidade for inválida.

**Requisitos** Windows 10, versão 1703

---

<span id="setCapability"/>

### <a name="setcapability"></a>setCapability
Habilita ou desabilita uma funcionalidade específica no navegador.

**Sintaxe**  
`void SecureBrowser.security.setCapability(String feature, String value, Function onSuccess, Function onError)`

**Parâmetros**  
* `feature` - A cadeia de caracteres que determina qual funcionalidade será definida. As cadeias de caracteres de funcionalidade válidas são `"screenMonitoring"`, `"printing"` e `"textSuggestions"` (não diferencia maiúsculas de minúsculas).  
* `value` - A configuração pretendida para o recurso. Deve ser `"true"` ou `"false"`.  
* `onSuccess` - [opcional] A função a ser chamada após a operação de conjuntos foi concluída com êxito. Deve estar no formato `Function(String jsonValue)`, em que *jsonValue* está no formato: `{<feature>:true|false|undefined}`.  
* `onError` - [opcional] A função a ser chamada em caso de falha na operação de conjuntos. Deve estar no formato `Function(String jsonValue)`, em que *jsonValue* está no formato: `{<feature>:true|false|undefined}`.

**Comentários**  
Se o recurso de destino for desconhecido para o navegador, esta função passará um valor `undefined` para a função de retorno de chamada.

**Requisitos** Windows 10, versão 1703

---

<span id="isRemoteSession"/>

### <a name="isremotesession"></a>isRemoteSession
Verifica se a sessão atual está conectada remotamente.

**Sintaxe**  
`Boolean SecureBrowser.security.isRemoteSession();`

**Valor de retorno**  
**true** se a sessão atual for remota; caso contrário, será **false**.

**Requisitos**  
Windows 10, versão 1709

---

<span id="isVMSession"/>

### <a name="isvmsession"></a>isVMSession
Verifica se a sessão atual está em execução em uma máquina virtual.

**Sintaxe**  
`Boolean SecureBrowser.security.isVMSession();`

**Valor de retorno**  
**true** se a sessão atual estiver em execução em uma máquina virtual; caso contrário, será **false**.

**Comentários**  
Essa verificação de API só pode detectar as sessões de VM que estão em execução em determinados hipervisores que implementam as APIs apropriadas

**Requisitos**  
Windows 10, versão 1709

---