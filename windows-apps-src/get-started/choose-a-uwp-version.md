---
author: QuinnRadich
title: "Escolher uma versão do UWP"
description: "Ao escrever um aplicativo UWP no Microsoft Visual Studio, você pode escolher para qual versão você o escreverá. Saiba mais sobre a diferença entre as diferentes versões de UWP e como configurar suas escolhas em projetos novos e existentes."
redirect_url: ../updates-and-versions/choose-a-uwp-version/
translationtype: Human Translation
ms.sourcegitcommit: a86002c944841536d37735bb8c4b657905582144
ms.openlocfilehash: d6d2be6c91ddf5fb85cdec759c753db1561f066f

---

# Escolher uma versão do UWP

**Esta página foi realocada para ../updates-and-versions/choose-a-uwp-version/**

Ao escrever um aplicativo UWP no Microsoft Visual Studio, você pode escolher para qual versão você o escreverá. Saiba mais sobre a diferença entre as diferentes versões de UWP e como configurar suas escolhas em projetos novos e existentes.

## O que está diferente em cada versão do UWP?

APIs novas e alteradas para UWP estão disponíveis em todas as versões sucessivas do Windows 10. Para obter informações específicas sobre quais recursos foram adicionados a qual versão, consulte [Novidades para desenvolvedores no Windows 10](../whats-new/windows-10-version-1607.md).

Para tópicos de referência que enumeram todas as famílias de dispositivos e suas versões e todos os contratos de API e suas versões, consulte as [Famílias de dispositivos](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) e os [Contratos de API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## Escolha qual versão deve ser usada para o seu aplicativo

Na caixa de diálogo **Novo Projeto Universal do Windows** no Visual Studio, você pode escolher uma versão para **Versão de destino** e para **Versão mínima**.

* **Versão de destino**. Isso define a configuração *TargetPlatformVersion* no seu arquivo de projeto. Também determina o valor do atributo *TargetDeviceFamily@MaxVersionTested* no manifesto do pacote do aplicativo. O valor que você escolher especifica a versão da plataforma UWP a qual seu projeto se destina — e, portanto, o conjunto de APIs disponíveis para seu aplicativo — portanto, recomendamos que você escolha a versão mais recente possível. Para saber mais sobre o manifesto do pacote de aplicativo e algumas diretrizes sobre como configurar a TargetDeviceFamily manualmente, consulte [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Versão mínima**. Isso define a configuração *TargetPlatformMinVersion* no seu arquivo de projeto. Também determina o valor do atributo *TargetDeviceFamily@MinVersion* no manifesto do pacote do aplicativo. O valor que você escolher especifica a versão mínima da plataforma UWP com a qual seu projeto pode trabalhar.

Lembre-se de que você está declarando que seu aplicativo funciona em qualquer versão do Windows no intervalo de **Versão mínima** até **Versão de destino**. Se as duas forem da mesma versão, então você não precisará fazer nada especial. Se eles forem diferentes, então aqui estão algumas coisas que devem ser lembradas.

* Em seu código, você pode livremente (ou seja, sem verificações de condição) chamar qualquer API que existe na versão especificada pela **Versão mínima**.
* O valor de **Versão de destino** é usado para identificar todas as referências (contrato winmds) usadas para compilar seu projeto. Mas essas referências permitirão que você compile o código com chamadas para APIs que não existem necessariamente em dispositivos para os quais você declarou que oferece suporte (via **Versão mínima**). Portanto, qualquer API introduzida após a **Versão mínima** precisará ser chamada por meio do código adaptável. Para obter mais informações sobre o código adaptável, consulte o [Guia para aplicativos UWP (Plataforma Universal do Windows)](universal-application-platform-guide.md).


<!--HONumber=Aug16_HO5-->


