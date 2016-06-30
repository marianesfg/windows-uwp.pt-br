---
author: jnHs
Description: "Você pode fornecer informações adicionais sobre seu aplicativo na seção Declarações do aplicativo da página Propriedades do aplicativo durante o processo de envio."
title: "Declarações de aplicativo"
ms.assetid: 3AF618F3-2B47-4A57-B7E8-1DF979D4A82C
translationtype: Human Translation
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: ee2e54494a868fbec8c4a8bd7bbb9bdcd6cbf9ae

---

# Declarações de aplicativo

Você pode fornecer informações adicionais sobre seu aplicativo na seção **Declarações do aplicativo** da página **Propriedades do aplicativo** durante o [processo de envio](app-submissions.md). Essas declarações podem ajudar a garantir que seu aplicativo seja exibido adequadamente e oferecido ao conjunto correto de clientes, ou podem indicar como os clientes podem usar seu aplicativo.

As seções a seguir descrevem cada declaração e o que você precisa considerar ao determinar se cada declaração se aplica ao seu aplicativo.

## Este aplicativo permite que os usuários façam compras, mas não usa o sistema de comércio da Windows Store.

Essa caixa deve ficar desmarcada na maioria dos aplicativos, pois os aplicativos que oferecem oportunidades para fazer compras no aplicativo geralmente usam a API de compra no aplicativo da Microsoft para criar e [enviar os IAPs](iap-submissions.md). Segundo o [Contrato de Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058), os aplicativos que foram criados e enviados antes de 29 de junho de 2015 podem continuar oferecendo a funcionalidade de compra no aplicativo sem usar o mecanismo de comércio da Microsoft, desde que a funcionalidade de compra esteja em conformidade com as [políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_8). Se isso se aplica ao seu aplicativo, você deve marcar esta caixa. Caso contrário, deixe-a desmarcada.

## Este aplicativo foi testado para atender às diretrizes de acessibilidade.

Marcar essa caixa torna seu aplicativo detectável para os clientes que estão procurando especificamente por aplicativos acessíveis na Loja.

Você só deve marcar essa caixa se tiver feito todas as seguintes tarefas:

-   Definido todas as informações relevantes de acessibilidade para elementos de IU, como nomes acessíveis.
-   Implementado a navegação e as operações de teclado, levando em conta a ordem da guia, a ativação do teclado, a navegação com setas de direção, atalhos.
-   Garanta uma experiência visual acessível incluindo itens como uma relação de contraste de texto de 4,5:1 e não dependa apenas da cor para passar informações ao usuário.
-   Usado ferramentas de comprovação de acessibilidade, como Inspect ou AccChecker, para verificar o aplicativo e resolver todos os erros de alta prioridade detectados por essas ferramentas.
-   Verificado os cenários principais do aplicativo de ponta a ponta usando recursos e ferramentas como Narrador, Lupa, Em Teclado de Tela, Alto Contraste e Alto DPI.

Ao declarar seu aplicativo como acessível, você concorda que o aplicativo é acessível para todos os clientes, inclusive pessoas com necessidades especiais. Por exemplo, isso significa que você testou o aplicativo com modo de alto contraste e com um leitor de tela. Você também verificou que a interface de usuário funciona corretamente com um teclado, a Lupa e outras ferramentas de acessibilidade.

Para obter mais informações, consulte [Acessibilidade para aplicativos do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/dn263101), [Testes de acessibilidade](https://msdn.microsoft.com/library/windows/apps/mt297664) e [Acessibilidade na Loja](https://msdn.microsoft.com/library/windows/apps/mt297663).

> **Importante**  Não liste seu aplicativo como acessível a menos que tenha feito a engenharia e os testes especificamente para isso. Se seu aplicativo for declarado como acessível, mas não der suporte real à acessibilidade, você provavelmente receberá comentários negativos da comunidade.

## Os clientes podem instalar esse aplicativo em unidades alternativas ou armazenamento removível.

Essa caixa é marcada por padrão, para permitir que os clientes instalem seu aplicativo em mídia de armazenamento removível, como um cartão SD, ou em um drive de volume não pertencente ao sistema, como um drive externo.

Se você deseja impedir que seu aplicativo seja instalado em unidades alternativas ou armazenamento removível, desmarque essa caixa.

Não há opção de restringir a instalação para que o aplicativo possa ser instalado apenas em mídia de armazenamento removível.

> **Observação**  Para o Windows Phone 8.1, isso costumava ser indicado por meio do StoreManifest.xml.

## O Windows pode incluir dados do aplicativo em backups automáticos no OneDrive.

Esta caixa é marcada por padrão, para permitir que os dados do seu aplicativo sejam incluídos quando um cliente optar por fazer com que o Windows faça backups automáticos no OneDrive.

Se você deseja impedir que os dados do aplicativo sejam incluídos em backups automáticos, desmarque essa caixa.

> **Observação**  Para o Windows Phone 8.1, isso costumava ser indicado por meio do StoreManifest.xml.

 

 

 







<!--HONumber=Jun16_HO4-->


