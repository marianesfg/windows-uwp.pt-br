---
title: Referência de API Fiddler do Device Portal
description: Saiba como habilitar/desabilitar o rastreamento Fiddler programaticamente.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: e7d4225e-ac2c-41dc-aca7-9b1a95ec590b
ms.localizationpriority: medium
ms.openlocfilehash: 4cbdae1084f96901e90f8237d71bd59bf2d4c592
ms.sourcegitcommit: 681c1e3836d2a51cd3b31d824ece344281932bcd
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/08/2019
ms.locfileid: "59240014"
---
# <a name="fiddler-settings-api-reference"></a>Referência de API de configurações Fiddler   
Você pode habilitar e desabilitar o rastreamento de rede Fiddler no seu devkit usando essa API REST.

## <a name="determine-if-fiddler-tracing-is-enabled"></a>Determinar se o rastreamento do Fiddler está habilitado

**Solicitação**

Você pode verificar se o rastreamento do Fiddler está habilitado no dispositivo usando a solicitação a seguir.

Método      | URI da solicitação
:------     | :-----
OBTER | /ext/fiddler


**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   

- Nenhuma

**Resposta**   

- A propriedade booliana JSON IsProxyEnabled que especifica se o proxy está habilitado ou não.

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
POSTAR | /ext/fiddler

**Parâmetros do URI**

Você pode especificar os seguintes parâmetros adicionais no URI da solicitação:

| Parâmetro do URI      | Descrição     | 
| ------------------ |-----------------|
| proxyAddress       | O endereço IP ou nome de host do dispositivo que executa o Fiddler |
| proxyPort          | A porta que o Fiddler está usando para monitorar o tráfego. O padrão é 8888 |
| updateCert (opcional)| Um valor booliano que indica se o certificado de Fiddler raiz é fornecido. Isso deve ser "true" se o Fiddler nunca foi configurado neste devkit ou foi configurado para um host diferente.  |


**Cabeçalhos de solicitação**

- Nenhuma

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

## <a name="disable-fiddler-tracing-on-the-devkit"></a>Desativar o rastreamento de Fiddler no devkit

**Solicitação**

Você pode desabilitar o rastreamento de Fiddler no dispositivo usando a solicitação a seguir. Observe que o dispositivo deve ser reiniciado para ter efeito.

Método      | URI da solicitação
:------     | :-----
DELETE | /ext/fiddler

**Parâmetros do URI**

- Nenhuma

**Cabeçalhos de solicitação**

- Nenhuma

**Corpo da solicitação**   

- Nenhuma

**Resposta**   

- Nenhuma 

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
204 | A solicitação para desabilitar o rastreamento de Fiddler foi bem-sucedida. O rastreamento será desabilitado na próxima reinicialização do dispositivo.
4XX | Códigos de erro
5XX | Códigos de erro


**Famílias de dispositivos disponíveis**

* Windows Xbox

## <a name="see-also"></a>Consulte também
- [Configurando o Fiddler para UWP no Xbox](uwp-fiddler.md)

