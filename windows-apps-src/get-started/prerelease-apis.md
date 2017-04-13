---
author: drewbat
ms.assetid: 
title: "Desenvolvendo aplicativos UWP com APIs de pré-lançamento"
description: "Entenda os benefícios e limitações do uso de APIs de pré-lançamento que estão incluídos nas visualizações de SDK UWP."
ms.openlocfilehash: ede7e8d5e9cce39850edbdb70a715d76c78f0c01
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="developing-uwp-apps-with-pre-release-apis"></a>Desenvolvendo aplicativos UWP com APIs de pré-lançamento

A Microsoft publica versões de visualização da Plataforma Universal do Windows (UWP) SDK para permitir que os desenvolvedores se envolvam com novos recursos da plataforma antes que eles sejam finalizados. Isso proporciona um ponto de partida sobre como incorporar recursos em seu aplicativo e ajuda você a publicar seu aplicativo mais cedo depois que a versão RTM oficial do SDK é lançada. Usar as APIs de pré-lançamento também permite que você forneça comentários à Microsoft para ajudar a influenciar a direção da plataforma em versões futuras.

## <a name="important-limitations-on-the-use-of-pre-release-apis"></a>Limitações importantes sobre o uso de APIs de pré-lançamento
Antes de usar uma API de pré-lançamento em seu aplicativo, lembre-se de as seguintes importantes implicações de fazê-lo: 
* Aplicativos que usam APIs de pré-lançamento não podem ser enviados para a Windows Store até que as APIs sejam publicadas oficialmente em um lançamento de RTM. É altamente recomendável que você mantenha o código de desenvolvimento de pré-lançamento separado do código de seus aplicativos publicados no momento. 
* Aplicativos que você desenvolve usando uma visualização SDK não podem ser enviados para a Loja mesmo se você não usar quaisquer APIs pré-lançados. Você deve instalar as ferramentas de pré-visualização ou VM que é separado da máquina de produção que você usa para seu desenvolvedor principal. 
* APIs de pré-lançamento têm probabilidade de mudar antes da RTM. Quando APIs estão incluídas no SDK de pré-visualização, é provável que o recurso ou cenário permitam que seja incluído no SDK final. Mas os nomes, assinaturas e comportamento de APIs específicas podem ser alterados antes da versão final, e é possível que a API seja totalmente removida. 

## <a name="how-to-identify-a-prerelease-api"></a>Como identificar uma API de pré-lançamento 
Na documentação de referência da API para UWP, as APIs que são pré-lançadas são identificadas com o seguinte texto: 

Este artigo discute uma API de pré-lançamento ou recurso que pode ser modificado substancialmente antes de ser lançado comercialmente. Você pode usar esse recurso para desenvolvimento e teste no momento, mas os aplicativos que o usam não podem ser enviados para a Windows Store até que seu uso seja lançado comercialmente. A Microsoft não oferece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui. Para obter mais informações sobre o desenvolvimento com as APIs de pré-lançamento, consulte Aplicativos de desenvolvimento UWP com APIs de pré-lançamento. 

## <a name="get-the-latest-sdk-insider-preview"></a>Baixe a pré-visualização do SDK Insider mais recente 
1. [Inscreva-se no Programa Windows Insider](https://insider.windows.com/) para obter acesso a compilações de pré-visualização do SDK. 
3. Antes de instalar as ferramentas de visualização de desenvolvedor, examine as [notas de versão do emulador atual de SDK e Mobile](http://go.microsoft.com/fwlink/?LinkId=829180).
4. Instale o [SDK Insider Preview](https://www.microsoft.com/en-us/software-download/windowsinsiderpreviewSDK).
5. Confira o [Fórum da Comunidade do Windows Insider Preview](http://go.microsoft.com/fwlink/p/?LinkId=507620)
