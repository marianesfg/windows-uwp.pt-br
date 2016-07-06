---
author: payzer
title: "Referência de API da área restrita do Xbox Live do Device Portal"
description: "Saiba como acessar a área restrita do Xbox Live de maneira programática."
ms.sourcegitcommit: a857ba338a971e651653193ff2149f08b1665a36
ms.openlocfilehash: c1671db55dcb05ab7a126fad271a5e49146fe9ac

---

# Referência de API da área restrita do Xbox Live   
Você pode obter e definir a área restrita do Xbox Live usando essa API REST.

## Obter a área restrita do Xbox Live

**Solicitação**

Você pode ler o valor atual da área restrita do Xbox Live do dispositivo usando a seguinte solicitação:

Método      | URI da solicitação
:------     | :-----
GET | /ext/xboxlive/sandbox
<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**

- Nenhum

**Resposta**   
Sandbox – (sequência) a área restrita atual na qual o dispositivo está.   

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

## Definir a área restrita do Xbox Live
Você pode alterar a área restrita do Xbox Live do dispositivo usando a solicitação a seguir. No Xbox One, o dispositivo precisa ser reiniciado antes da configuração entrar em vigor.

**Solicitação**

Você pode definir o valor atual da área restrita do Xbox Live do dispositivo usando a seguinte solicitação:

Método      | URI da solicitação
:------     | :-----
PUT | /ext/xboxlive/sandbox
<br />
**Parâmetros do URI**

- Nenhum

**Cabeçalhos de solicitação**

- Nenhum

**Corpo da solicitação**   
O corpo da solicitação é um objeto JSON contendo o seguinte campo:   
Sandbox – (sequência) o novo valor para definir a área restrita do dispositivo.

**Resposta**   
Sandbox – (sequência) a área restrita atual na qual o dispositivo está.   

**Código de status**

Esta API tem os códigos de status esperados a seguir.

Código de status HTTP      | Descrição
:------     | :-----
200 | A solicitação para acessar as credenciais do compartilhamento de arquivos foi concedida.
4XX | Códigos de erro
5XX | Códigos de erro

<br />
**Famílias de dispositivos disponíveis**

* Windows Xbox




<!--HONumber=Jun16_HO4-->


