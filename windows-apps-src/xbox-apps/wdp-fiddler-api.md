---
author: WilliamsJason
title: Referência de API Fiddler do Device Portal
description: Saiba como habilitar/desabilitar o rastreamento Fiddler programaticamente.
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 819f039f04d1e0a7fd035b10e3cbe408645e8f4d
ms.sourcegitcommit: 897a111e8fc5d38d483800288ad01c523e924ef4
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/13/2018
ms.locfileid: "409938"
---
# <a name="fiddler-settings-api-reference"></a>Referência de API de configurações Fiddler   
Você pode habilitar e desabilitar o rastreamento de rede Fiddler no seu devkit usando essa API REST.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Determinar se o Fiddler rastreamento está habilitado

**Solicitação**

Você pode verificar se o rastreamento do Fiddler está habilitado no dispositivo usando a seguinte solicitação.

Método      | URI da solicitação
:------     | :-----
GET | /ext/fiddler
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- Propriedade bool JSON IsProxyEnabled quais especificadores se o proxy está habilitado ou não.

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | Êxito
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="enable-fiddler-tracing"></a>Habilite o rastreamento Fiddler

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

- Nenhum(a)  

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | A solicitação para habilitar o Fiddler foi aceita. O Fiddler será habilitado na próxima vez que o dispositivo for reiniciado.
4XX | Códigos de erro
5XX | Códigos de erro

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Desativar o rastreamento de Fiddler no devkit

**Solicitação**

Você pode desabilitar o rastreamento de Fiddler no dispositivo usando a solicitação a seguir. Observe que o dispositivo deve ser reiniciado para ter efeito.

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/fiddler
<br />
**Parâmetros do URI**

- Nenhum(a)

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   

- Nenhum(a)

**Resposta**   

- Nenhum(a) 

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

## <a name="see-also"></a>Consulte também
- [Configurando o Fiddler para UWP no Xbox](uwp-fiddler.md)

