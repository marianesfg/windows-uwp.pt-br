---
author: TylerMSFT
Description: "A API JavaScript do aplicativo Fazer um Teste Microsoft permite fazer avaliações seguras. Fazer um Teste oferece um navegador seguro que impede que alunos usem outros recursos de computador ou internet durante um teste."
title: API JavaScript Fazer um Teste Microsoft.
translationtype: Human Translation
ms.sourcegitcommit: f2838d95da66eda32d9cea725a33fc4084d32359
ms.openlocfilehash: d7f185e83e81583fd6d7920e5412f76f3a97edd0

---

# API JavaScript Fazer um Teste Microsoft

**Fazer um Teste** é um aplicativo baseado em navegador que renderiza avaliações online bloqueadas para testes de alto risco. Ele dá suporte ao padrão da API do navegador SBAC para testes básicos comuns importantes e permite se concentrar no conteúdo de avaliação, em vez de como bloquear o Windows.

**Fazer um Teste**, da plataforma do navegador Microsoft Edge, oferece uma API JavaScript que os aplicativos Web podem usar para oferecer uma experiência de bloqueio para realizar testes.

A API (com base na [Common Core SBAC API](http://www.smarterapp.org/documents/SecureBrowserRequirementsSpecifications_0-3.pdf)) fornece texto para controle por voz e o recurso de consulta se o dispositivo estiver bloqueado, quais são os processos e o usuário de execução e muito mais.

Consulte A [Referência técnica do aplicativo Fazer um Teste](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396) para obter informações sobre o próprio aplicativo.

**Importante**

As APIs não funcionam em uma sessão remota.  
Fazer um Teste não manipula solicitações de novas janelas HTTP.

Para a solução de problemas, consulte [Solução de problemas com Fazer um Teste da Microsoft com o visualizador de eventos](troubleshooting.md).

**A API de Fazer um Teste consiste nos seguintes namespaces:**  

| Namespace | Descrição |
|-----------|-------------|
|[namespace de segurança](#security-namespace)| Funcionalidade de texto em fala|
|[namespace tts](#tts-namespace)|Permite bloquear o dispositivo|


 ## namespace de segurança

Permite bloquear o dispositivo, verificar a lista de processos de usuário e sistema, obter endereços MAC e IP e limpar recursos da web em cache.

| Método | Descrição   |
|--------|---------------|
|[clearCache](#clearCache) | Limpa recursos da web em cache |
|[close](#close) | Fecha o navegador e desbloqueia o dispositivo |
|[enableLockDown](#enableLockDown) | Bloqueia o dispositivo. Também usado para desbloquear o dispositivo |
|[getIPAddressList](#getIPAddressList) | Obtém a lista de endereços IP do dispositivo |
|[getMACAddress](#getMACAddress)|Obtém a lista de endereços MAC do dispositivo|
|[getProcessList](#getProcessList)|Obtém a lista de processos do sistema e de usuários em execução|
|[isEnvironmentSecure](#isEnvironmentSecure)|Determina se o contexto de bloqueio continua aplicado ao dispositivo|

<span id="clearCache" />
### void clearCache()
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
### close(boolean restart)
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
### enableLockdown(boolean lockdown)
Bloqueia o dispositivo. Também usado para desbloquear o dispositivo.

**Sintaxe**  
`browser.security.enableLockDown(true|false);`

**Parâmetros**  
`lockdown` - `true` para executar o aplicativo Fazer um Teste acima da tela de bloqueio e aplicar políticas debatidas neste [documento](https://technet.microsoft.com/en-us/edu/windows/take-a-test-app-technical?f=255&MSPPError=-2147217396). `False` para a execução de Fazer um Teste acima da tela de bloqueio e o fecha, a menos que o aplicativo não esteja bloqueado; neste caso, não há efeito.

**Valor de retorno**  
`None`

**Requisitos**  
Windows 10, versão 1607

---

<span id="getIPAddressList"/>
### string[] getIPAddressList()
Obtém a lista de endereços IP do dispositivo.

**Sintaxe**  
`browser.security.getIPAddressList();`

**Parâmetros**  
`None`

**Valor de retorno**  
`An array of IP addresses.`

<span id="getMACAddress" />
### string[] getMACAddress()
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
### string[] getProcessList()
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
### boolean isEnvironmentSecure()
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

## namespace tts
| Método | Descrição |
|--------|-------------|
|[getStatus](#getStatus) | Obtém o status de reprodução do controle por voz|
|[getVoices](#getVoices) | Obtém uma lista de pacotes de voz disponíveis|
|[pause](#pause)|Pausa a sintetização do controle por voz|
|[resume](#resume)|Retome a sintetização de voz pausada|
|[speak](#speak)|Sintetização do texto para fala no lado do cliente|
|[stop](#stop)|Para sintetização do controle por voz|

> [!Tip]
> A [Microsoft Edge Speech Synthesis API](https://blogs.windows.com/msedgedev/2016/06/01/introducing-speech-synthesis-api/) é uma implementação da [W3C Speech Api](https://dvcs.w3.org/hg/speech-api/raw-file/tip/webspeechapi.html) e recomendamos que os desenvolvedores usem essa API quando possível.

<span id="getStatus" />
### string getStatus()
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
### string[] getVoices()
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
### void pause()

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
### void resume()
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
### void speak(string text, object options, function callback)
Sintetização do texto para fala no lado do cliente.

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
### void stop()
Para sintetização do controle por voz.

**Sintaxe**  
`void browser.tts.speak(“Hello world”, options, callback);`

**Parâmetros**  
`None`

**Valor de retorno**  
`None`

**Requisitos**  
Windows 10, versão 1607



<!--HONumber=Aug16_HO3-->


