---
author: WilliamsJason
title: "Referência de API Fiddler do Device Portal"
description: Saiba como habilitar/desabilitar o rastreamento Fiddler programaticamente.
translationtype: Human Translation
ms.sourcegitcommit: 3cc2a4bd1859e46a73f3e806489eac7381fa6c17
ms.openlocfilehash: bd215058c71118d8b3e5ce81e2302ce8b151c3f6

---

# Referência de API de configurações Fiddler   
Você pode habilitar e desabilitar o rastreamento de rede Fiddler no seu devkit usando essa API REST.

## Habilite o rastreamento Fiddler

**Solicitação**

Você pode habilitar o rastreamento Fiddler para o devkit usando a solicitação a seguir.  Observe que o dispositivo deve ser reiniciado para ter efeito.

Método      | URI da solicitação
:------     | :-----
POST | /ext/fiddler
<br />
**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| proxyAddress       | O endereço IP ou nome de host do dispositivo que executa o Fiddler |
| proxyPort          | A porta que o Fiddler está usando para monitorar o tráfego. O padrão é 8888 |
| updateCert (opcional)| Um valor booliano que indica se o certificado de Fiddler raiz é fornecido. Isso deve ser "true" se o Fiddler nunca foi configurado neste devkit ou foi configurado para um host diferente.  |
<br>

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum se updateCert for "false" ou não foi fornecido. Várias partes seguem as diretrizes http corpo que contém o arquivo de FiddlerRoot.cer.

**Resposta**   

- Nenhuma  

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | A solicitação para habilitar o Fiddler foi aceita. O Fiddler será habilitado na próxima vez que o dispositivo for reiniciado.
4XX | Códigos de erro
5XX | Códigos de erro

## Desativar o rastreamento de Fiddler no devkit

**Solicitação**

Você pode desabilitar o rastreamento de Fiddler no dispositivo usando a solicitação a seguir. Observe que o dispositivo deve ser reiniciado para ter efeito.

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/fiddler
<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum

**Resposta**   

- Nenhum 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | A solicitação para desabilitar o rastreamento de Fiddler foi bem-sucedida. O rastreamento será desabilitado na próxima reinicialização do dispositivo.
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox

## Consulte também
- [Configurando o Fiddler para UWP no Xbox](uwp-fiddler.md)




<!--HONumber=Aug16_HO3-->


