---
author: QuinnRadich
title: "Escolher uma versão do UWP"
description: "Ao escrever um aplicativo UWP no Microsoft Visual Studio, você pode escolher para qual versão você o escreverá. Saiba mais sobre a diferença entre as diferentes versões de UWP e como configurar suas escolhas em projetos novos e existentes."
translationtype: Human Translation
ms.sourcegitcommit: 46852d305b2c19b46a904cabf928978c6c9f1606
ms.openlocfilehash: 249bba67b844585b590294a456e3d0e74c392c95

---

# Escolher uma versão do UWP

Ao escrever um aplicativo UWP no Microsoft Visual Studio, você pode escolher para qual versão você o escreverá. Atualmente, existem apenas três versões possíveis.

| Versão | Descrição |
| --- | --- |
| Compilação 14393 (edição de aniversário) | Esta é a versão mais recente do Windows 10, lançada em julho de 2016. Alguns recursos destacados desta versão incluem: </br> \* **Windows Ink:** novos controles InkCanvas e InkToolbar. </br> \* **APIs da Cortana:** use as novas ações da Cortana para integrar o suporte da Cortana com funções específicas do seu aplicativo. </br> \* **Windows Hello:** o Mirosoft Edge agora oferece suporte ao Windows Hello, dando aos desenvolvedores da Web acesso à autenticação biométrica. </br> Para obter informações sobre estes e muitos outros recursos adicionados nesta versão do Windows, visite [o Centro de Desenvolvimento](https://developer.microsoft.com/en-us/windows/windows-10-for-developers) ou a página mais detalhada sobre [Novidades no Windows 10 para desenvolvedores](../whats-new/windows-10-version-1607.md)  |
| Build 10586 | Esta versão do Windows 10 foi lançada em novembro de 2015. Os recursos destacados incluem a introdução das APIs de ORTC (comunicações em tempo real do objeto) para a comunicação de vídeo no Microsoft Edge e as APIs de provedores para permitir que os aplicativos usem a autenticação de rosto do Windows Hello. [Mais informações sobre os recursos introduzidos nesta compilação.](../whats-new/windows-10-version-1511.md) |
| Build 10240 | Esta é a versão de lançamento inicial do Windows 10, em julho de 2015. [Mais informações sobre os recursos introduzidos nesta compilação.](../whats-new/windows-10-version-1507.md) |

É altamente recomendável que os novos desenvolvedores e aqueles que escrevem o código para o público em geral sempre usem a compilação mais recente do Windows (14393). Os desenvolvedores que criam Aplicativos corporativos devem realmente considerar oferecer suporte a uma **Versão mínima** mais antiga.

## O que está diferente em cada versão do UWP?

APIs novas e alteradas para UWP estão disponíveis em todas as versões sucessivas do Windows 10. Para obter informações específicas sobre quais recursos foram adicionados a qual versão, consulte [Novidades para desenvolvedores no Windows 10](../whats-new/windows-10-version-1607.md).

Para tópicos de referência que enumeram todas as famílias de dispositivos e suas versões e todos os contratos de API e suas versões, consulte as [Famílias de dispositivos](https://msdn.microsoft.com/library/windows/apps/dn706137.aspx) e os [Contratos de API](https://msdn.microsoft.com/library/windows/apps/dn706135.aspx).

## Escolha qual versão deve ser usada para o seu aplicativo

Na caixa de diálogo **Novo Projeto Universal do Windows** no Visual Studio, você pode escolher uma versão para **Versão de destino** e para **Versão mínima**.

* **Versão de destino**. Isso define a configuração *TargetPlatformVersion* no seu arquivo de projeto. Também determina o valor do atributo *TargetDeviceFamily@MaxVersionTested* no manifesto do pacote do aplicativo. O valor que você escolher especifica a versão da plataforma UWP a qual seu projeto se destina — e, portanto, o conjunto de APIs disponíveis para seu aplicativo — portanto, recomendamos que você escolha a versão mais recente possível. Para saber mais sobre o manifesto do pacote de aplicativo e algumas diretrizes sobre como configurar a TargetDeviceFamily manualmente, consulte [TargetDeviceFamily](https://msdn.microsoft.com/library/windows/apps/dn986903).
* **Versão mínima**. Isso define a configuração *TargetPlatformMinVersion* no seu arquivo de projeto. Também determina o valor do atributo *TargetDeviceFamily@MinVersion* no manifesto do pacote do aplicativo. O valor que você escolher especifica a versão mínima da plataforma UWP com a qual seu projeto pode trabalhar.

Lembre-se de que você está declarando que seu aplicativo funciona em qualquer versão do Windows no intervalo de **Versão mínima** até **Versão de destino**. Se as duas forem da mesma versão, então você não precisará fazer nada especial. Se eles forem diferentes, então aqui estão algumas coisas que devem ser lembradas.

* Em seu código, você pode livremente (ou seja, sem verificações de condição) chamar qualquer API que existe na versão especificada pela **Versão mínima**.
* Certifique-se de testar seu código a **Versão mínima**, certifique-se de que ela funcione sem a necessidade de APIs presentes apenas na **Versão de destino**.
* O valor de **Versão de destino** é usado para identificar todas as referências (contrato winmds) usadas para compilar seu projeto. Mas essas referências permitirão que você compile o código com chamadas para APIs que não existem necessariamente em dispositivos para os quais você declarou que oferece suporte (via **Versão mínima**). Portanto, qualquer API introduzida após a **Versão mínima** precisará ser chamada por meio do código adaptável. Para obter mais informações sobre o código adaptável, consulte o [Guia para aplicativos UWP (Plataforma Universal do Windows)](../get-started/universal-application-platform-guide.md).



<!--HONumber=Sep16_HO2-->


