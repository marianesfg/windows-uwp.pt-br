---
author: Mtoepke
title: "Ativação do Modo de Desenvolvedor do Xbox One"
description: "Como ativar o Modo de Desenvolvedor para que você possa alternar entre o Modo de Varejo e o Modo de Desenvolvedor."
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.openlocfilehash: d8ce552ef05ae1f5a02ed402bd800080ef177e6a
ms.sourcegitcommit: 64cfb79fd27b09d49df99e8c9c46792c884593a7
translationtype: HT
---
# <a name="xbox-one-developer-mode-activation"></a>Ativação do Modo de Desenvolvedor do Xbox One

* [Como o Modo de Desenvolvedor funciona](#how-developer-mode-works)
* [Ativar o Modo de Desenvolvedor no seu console Xbox One de varejo](#activate-developer-mode-on-your-retail-xbox-one-console)  
* [Alternar entre o Modo de Varejo e o Modo de Desenvolvedor](#switch-between-retail-and-developer-mode)

## <a name="how-developer-mode-works"></a>Como o Modo de Desenvolvedor funciona
O Xbox One tem dois modos: o Modo de *Varejo* (1) e o Modo de *Desenvolvedor* (2). No Modo de Varejo, o console está no estado usado por qualquer cliente ou usuário de um console Xbox One: você pode jogar jogos e executar aplicativos como usuário. No Modo de Desenvolvedor, você pode desenvolver softwares para o console, mas não pode jogar jogos de varejo ou executar aplicativos de varejo.
O Modo de Desenvolvedor pode ser habilitado em qualquer console Xbox One de varejo. Depois que ele é habilitado, você pode alternar livremente entre os Modos de Varejo e (2a) e de Desenvolvedor (2b).

> [!NOTE]
> Não será possível ativar o Modo de Desenvolvedor no Xbox One se você fizer parte de um programa preview existente, como o programa Xbox One Beta. Para sair de um programa preview existente, use o aplicativo Painel do Xbox Preview. 

![Modos do Xbox One](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Ativar o Modo de Desenvolvedor no seu console Xbox One de varejo

1.    Inicie seu console do Xbox One.

2.    Procure e instale o app Ativação do Modo de Desenvolvedor na loja do Xbox One.  
    ![Instalar o app Ativação do Modo de Desenvolvedor](images/activation-store-search.png)

3.    Navegue até **Meus jogos e apps** > **Apps**.  
    ![App Ativação do Modo de Desenvolvedor](images/activation-step-3.png)

4. Abra o app Ativação do Modo de Desenvolvedor. Para jogar jogos e executar aplicativos, você deve alternar para o Modo de Varejo. Aplicativos de sideload apenas funcionarão no Modo de Desenvolvedor.

5.    Observe o código exibido no aplicativo Ativação do Modo de Desenvolvedor.  
    ![Etapa de Ativação 5](images/activation-step-5.png)  
    
6.    Acesse o endereço [developer.microsoft.com/xboxactivate](https://developer.microsoft.com/xboxactivate).
7.    Entre no Centro de Desenvolvimento com a sua conta do Centro de Desenvolvimento.  
8.    Insira o código de ativação exibido no aplicativo Ativação do Modo de Desenvolvedor. Você tem um número limitado de ativações associadas à sua conta. Após a ativação do Modo de Desenvolvedor, o Centro de Desenvolvimento indicará que você usou uma das ativações associadas à sua conta.  
    ![Etapa de Ativação 8](images/activation-step-8.png)    
    
9.    Clique em **Concordar e ativar**. Isso fará com que a página seja recarregada, e você verá seu dispositivo preenchido na tabela. Os termos para o contrato do Programa de Ativação do Modo de Desenvolvedor do Xbox One pode ser encontrado em [Programa de Ativação do Modo de Desenvolvedor do Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760399).

10.    Após a inserção do código de ativação, seu console exibirá uma tela de andamento referente ao processo de ativação.  
    
11.    Concluída a ativação, abra o aplicativo Ativação do Modo de Desenvolvedor e clique em **Alternar e iniciar** para acessar o Modo de Desenvolvedor. Observe que essa etapa demorará mais do que o esperado.  
    ![Etapa de Ativação 12](images/activation-step-12.png)   
    

    
## <a name="switch-between-retail-and-developer-mode"></a>Alternar entre o Modo de Varejo e o Modo de Desenvolvedor
Quando o Modo de Desenvolvedor tiver sido habilitado no seu console, use **Dev Home** para alternar entre ele e o Modo de Varejo. Para saber mais sobre como iniciar e usar a **Dev Home**, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

* Para alternar para o Modo de Varejo, use **Dev Home** e clique em **Sair do modo de desenvolvedor**. Isso reiniciará o seu console no Modo de Varejo.    

  ![Etapa de Ativação 13](images/activation-step-13.png)  
  
* Para alternar para o Modo de Desenvolvedor, use o aplicativo Ativação do Modo de Desenvolvedor. Abra o aplicativo e clique em **Alternar e reiniciar**. Isso reiniciará o seu console no Modo de Desenvolvedor.  

  ![Etapa de Ativação 14](images/activation-step-12.png)  

## <a name="see-also"></a>Consulte também
- [Desativação do Modo de Desenvolvedor do Xbox One](devkit-deactivation.md)
- [UWP no Xbox One](index.md)
