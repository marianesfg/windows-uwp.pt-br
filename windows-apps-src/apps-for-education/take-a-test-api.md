---
author: TylerMSFT
Description: "A API JavaScript do aplicativo Fazer um Teste Microsoft permite fazer avaliações seguras. Fazer um teste oferece um navegador seguro que impede que alunos usem outros recursos de computador ou internet durante um teste."
title: API do JavaScript do Fazer um Teste.
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 9bff6318-504c-4d0e-ba80-1a5ea45743da
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: ac1a9b38a9857ae536025e682f98d01135850a19
ms.lasthandoff: 02/08/2017

---

# <a name="take-a-test-javascript-api"></a>API do JavaScript do Fazer um Teste

[Fazer um Teste](https://technet.microsoft.com/edu/windows/take-tests-in-windows-10) é um aplicativo baseado em navegador que renderiza avaliações online bloqueadas para testes de alto risco. Ele dá suporte ao padrão da API do navegador SBAC para testes básicos comuns importantes e permite se concentrar no conteúdo de avaliação, em vez de como bloquear o Windows.

Fazer um Teste, da plataforma do navegador Microsoft Edge, apresenta uma API JavaScript que os aplicativos Web podem usar para oferecer uma experiência de bloqueio para realizar testes.

A API (com base na [Common Core SBAC API](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)) fornece os recursos de conversão de texto em fala e de consulta se o dispositivo estiver bloqueado, quais processos de usuários e do sistema estão em execução e muito mais.

Consulte A [Referência técnica do aplicativo Fazer um Teste](https://technet.microsoft.com/edu/windows/take-a-test-app-technical) para obter informações sobre o próprio aplicativo.

> [!Important]
> Essas APIs não funcionam em uma sessão remota.  

Para a solução de problemas, consulte [Solução de problemas com Fazer um Teste da Microsoft com o visualizador de eventos](troubleshooting.md).

## <a name="reference-documentation"></a>Documentação de referência
A API de Fazer um Teste consiste nos seguintes namespaces. 

| Namespace | Descrição |
|-----------|-------------|
|[namespace de segurança](#security-namespace)|Permite bloquear o dispositivo|
|[namespace tts](#tts-namespace)|Funcionalidade conversão de texto em fala|


 ### <a name="security-namespace"></a>Namespace de segurança

O namespace de segurança permite bloquear o dispositivo, verificar a lista de processos de usuário e sistema, obter endereços MAC e IP e limpar recursos da web em cache.

| Método | Descrição   |
|--------|---------------|
|[clearCache](#clearCache) | Limpa recursos da web em cache |
|[close](#close) | Fecha o navegador e desbloqueia o dispositivo |
|[enableLockDown](#enableLockDown) | Bloqueia o dispositivo. Também usado para desbloquear o dispositivo |
|[getIPAddressList](#getIPAddressList) | Obtém a lista de endereços IP do dispositivo |
|[getMACAddress](#getMACAddress)|Obtém a lista de endereços MAC do dispositivo|
|[getProcessList](#getProcessList)|Obtém a lista de processos do sistema e de usuários em execução|
|[isEnvironmentSecure](#isEnvironmentSecure)|Determina se o contexto de bloqueio continua aplicado ao dispositivo|  

---
<span id="clearCache"/>
### <a name="void-clearcache"></a>void clearCache()
Limpe recursos da web em cache.

**Sintaxe**  
`browser.security.clearCache();`

**Parâmetros**  
`None`

**Valor de retorno**  
`None`

**Requisitos**  
Windows 10, versão 1607

---

<span id="close"/>
### <a name="closeboolean-restart"></a>close(boolean restart)
Fecha o navegador e desbloqueia o dispositivo.

**Sintaxe**  
`browser.security.close(false);`

**Parâmetros**  
`restart` - esse parâmetro é ignorado, mas deve ser fornecido.

**Valor de retorno**  
`None`

**Requisitos**  
Windows 10, versão 1607

---

<span id="enableLockDown"/>
### <a name="enablelockdownboolean-lockdown"></a>enableLockdown(boolean lockdown)
Bloqueia o dispositivo. Também usado para desbloquear o dispositivo.

**Sintaxe**  
`browser.security.enableLockDown(true|false);`

**Parâmetros**  
`lockdown` - `true` para executar o aplicativo Fazer um Teste acima da tela de bloqueio e aplicar políticas debatidas neste [documento](https://technet.microsoft.com/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). `False` para a execução de Fazer um Teste acima da tela de bloqueio e o fecha, a menos que o aplicativo não esteja bloqueado; neste caso, não há efeito.

**Valor de retorno**  
`None`

**Requisitos**  
Windows 10, versão 1607

---

<span id="getIPAddressList"/>
### <a name="string-getipaddresslist"></a>string[] getIPAddressList()
Obtém a lista de endereços IP do dispositivo.

**Sintaxe**  
`browser.security.getIPAddressList();`

**Parâmetros**  
`None`

**Valor de retorno**  
`An array of IP addresses.`

---

<span id="getMACAddress" />
### <a name="string-getmacaddress"></a>string[] getMACAddress()
Obtém a lista de endereços MAC do dispositivo.

**Sintaxe**  
`browser.security.getMACAddress();`

**Parâmetros**  
`None`

**Valor de retorno**  
`An array of MAC addresses.`

**Requisitos**  
Windows 10, versão 1607

---

<span id="getProcessList" />
### <a name="string-getprocesslist"></a>string[] getProcessList()
Obtém a lista dos processos em execução do usuário.

**Sintaxe**  
`browser.security.getProcessList();`

**Parâmetros**  
`None`

**Valor de retorno**  
`An array of running process names.`

**Comentários** A lista não inclui processos do sistema.

**Requisitos**  
Windows 10, versão 1607

---

<span id="isEnvironmentSecure" />
### <a name="boolean-isenvironmentsecure"></a>boolean isEnvironmentSecure()
Determina se o contexto de bloqueio continua aplicado ao dispositivo.

**Sintaxe**  
`browser.security.isEnvironmentSecure();`

**Parâmetros**  
`None`

**Valor de retorno**  
`True indicates that the lockdown context is applied to the device; otherwise false.`

**Requisitos**  
Windows 10, versão 1607

---

### <a name="tts-namespace"></a>Namespace tts

O namespace tts trata da funcionalidade do aplicativo conversão de texto em fala.

| Método | Descrição |
|--------|-------------|
|[getStatus](#getStatus) | Obtém o status de reprodução do controle por voz|
|[getVoices](#getVoices) | Obtém uma lista de pacotes de voz disponíveis|
|[pause](#pause)|Pausa a sintetização do controle por voz|
|[resume](#resume)|Retome a sintetização de voz pausada|
|[speak](#speak)|Sintetização conversão do texto em fala no lado do cliente|
|[stop](#stop)|Para sintetização do controle por voz|

> [!Tip]
> A [Microsoft Edge Speech Synthesis API](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) é uma implementação da [W3C Speech Api](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) e recomendamos que os desenvolvedores usem esta API quando possível.

---

<span id="getStatus" />
### <a name="string-getstatus"></a>string getStatus()
Obtém o status de reprodução do controle por voz.

**Sintaxe**  
`browser.tts.getStatus();`

**Parâmetros**  
`None`

**Valor de retorno**  
`The speech playback status. Possible values are: “available”, “idle”, “paused”, and “speaking”.`

**Requisitos**  
Windows 10, versão 1607

---

<span id="getVoices" />
### <a name="string-getvoices"></a>string[] getVoices()
Obtém uma lista de pacotes de voz disponíveis.

**Sintaxe**  
`browser.tts.getVoices();`

**Parâmetros**  
`None`

**Valor de retorno**  
`The available voice packs. For example: “Microsoft Zira Mobile”, “Microsoft Mark Mobile”`

**Requisitos**  
Windows 10, versão 1607

---

<span id="pause" />
### <a name="void-pause"></a>void pause()

Pausa a sintetização do controle por voz.

**Sintaxe**  
`browser.tts.pause();`

**Parâmetros**

`None`

**Valor de retorno**

`None`

**Requisitos**  
Windows 10, versão 1607

---

<span id="resume" />
### <a name="void-resume"></a>void resume()
Retome a sintetização de voz pausada.

**Sintaxe**  
`browser.tts.resume();`

**Parâmetros**
`None`

**Valor de retorno**
`None`

**Requisitos**  
Windows 10, versão 1607

---

<span id="speak" />
### <a name="void-speakstring-text-object-options-function-callback"></a>void speak(string text, object options, function callback)
Inicia a sintetização da conversão de texto em fala no lado do cliente.

**Sintaxe**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parâmetros**  
`Speech options such as gender, pitch, rate, volume. For example:`  
```
var options = {
   'gender': this.currentGender,
   'language': this.currentLanguage,
   'pitch': 1,
   'rate': 1,
   'voice': this.currentVoice,
   'volume': 1
};
```

**Valor de retorno**  
`None`

**Comentários** As variáveis de opção devem estar em minúsculas. O sexo, o idioma e os parâmetros de voz utilizam cadeias de caracteres.
Volume, timbre e taxa devem ser marcados dentro o arquivo de linguagem de marcação de sintetização de controle por voz (SSML), não dentro do objeto de opções.
O objeto de opções deve seguir a ordem, a nomenclatura e o uso de maiúsculas mostrados no exemplo acima.

**Requisitos**  
Windows 10, versão 1607

---

<span id="stop" />
### <a name="void-stop"></a>void stop()
Para sintetização do controle por voz.

**Sintaxe**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parâmetros**  
`None`

**Valor de retorno**  
`None`

**Requisitos**  
Windows 10, versão 1607

