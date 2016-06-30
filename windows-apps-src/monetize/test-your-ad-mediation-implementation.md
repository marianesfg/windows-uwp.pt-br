---
author: mcleanbyron
ms.assetid: 54ECD653-7FC2-4A95-AC5A-972C4FB5A54B
description: "Antes de enviar seu aplicativo, recomendamos que você teste sua implementação de controle de anúncios."
title: "Testar sua implementação de controle de anúncios"
translationtype: Human Translation
ms.sourcegitcommit: ec7ce299545de8e5c167e1934fb9a0b4f4370948
ms.openlocfilehash: 0805ed5462a4b100b837ed9c11ec2d9e7caabc34

---

# Testar sua implementação de controle de anúncios


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Antes de enviar seu aplicativo, recomendamos que você teste sua implementação de controle de anúncios.

## Testar com valores de configuração de rede de publicidade de teste


Se você executar o aplicativo sem inserir a configuração de rede de publicidade iniciando **Serviços Conectados** para seu projeto no Visual Studio, o controle de anúncios usará automaticamente os valores de configuração de teste quando você executar o aplicativo no computador de desenvolvimento (para aplicativos da Plataforma Universal do Windows (UWP) e XAML do Windows 8.1) ou no emulador ou dispositivo (para aplicativos do Windows Phone). Isso permite que você teste rapidamente seu aplicativo e verifique se ele está codificado corretamente antes de inserir os parâmetros necessários da rede de publicidade.

As redes de publicidade girarão em ordem sequencial, com uma rede exibida após a outra na mesma quantidade de tempo. Espere tempo suficiente para a execução de alguns ciclos para que você possa ver todas as redes de publicidade e reduzir a chance de quaisquer problemas de conectividade temporários que possam ocorrer.

Os anúncios de teste serão exibidos em redes de publicidade que deem suporte a eles. Observe que os anúncios de teste, às vezes, podem se parecer com erros. Revise os eventos para determinar se houve erros.

> **Observação**  Ao testar um aplicativo Silverlight do Windows Phone, o Google AdMob sempre retornará um erro de **Solicitação Inválida** já que ele não usa metadados de teste. Para verificar sua implementação do Google AdMob, você deve inserir os parâmetros necessários, conforme descrito na próxima seção.

 

Se você usou o código de manipulação de evento mostrado em [Adicionar e usar o controle de mediação de anúncio](add-and-use-the-ad-mediator-control.md), quaisquer erros serão mostrados na saída do console.

## Testar com valores de configuração de rede de publicidade


Depois de testar seu aplicativo com dados de configuração de teste, convém testar o aplicativo com os valores de configuração de rede de publicidade que você pretende usar para a versão do aplicativo que publicar na Windows Store.

Primeiramente, abra a janela **Adicionar Serviço Conectado** (Visual Studio 2015) ou a janela **Gerenciador de Serviços** (Visual Studio 2013) e configure cada rede de publicidade, conforme descrito em [Adicionar e usar o controle de mediação de anúncios](add-and-use-the-ad-mediator-control.md). Insira os parâmetros necessários de cada rede de publicidade.

Agora você está pronto para testar seu aplicativo. Execute o aplicativo tempo suficiente para verificar se cada rede de publicidade pode exibir corretamente um anúncio. Verifique se há quaisquer exceções e corrija os erros de codificação antes de enviar seu aplicativo.

Quando você enviar o pacote do aplicativo para o painel do Centro de Desenvolvimento do Windows, os valores de configuração inseridos no Visual Studio serão preenchidos automaticamente na página do painel **Monetizar com anúncios**. Você pode modificar esses valores para configurar o comportamento da rede de publicidade para seu aplicativo na Windows Store. Para obter mais informações, consulte [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md).

## Tópicos relacionados

* [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md)
* [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md)
* [Enviar seu aplicativo e configurar o controle de anúncios](submit-your-app-and-configure-ad-mediation.md)
* [Solucionar problemas de controle de anúncios](troubleshoot-ad-mediation.md)
 

 



<!--HONumber=Jun16_HO4-->


