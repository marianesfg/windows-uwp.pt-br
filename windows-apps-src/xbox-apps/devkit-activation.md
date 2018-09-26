---
author: Mtoepke
title: Ativação do Modo de Desenvolvedor do Xbox One
description: Como ativar o Modo de Desenvolvedor para que você possa alternar entre o Modo de Varejo e o Modo de Desenvolvedor.
ms.author: scotmi
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: ade80769-17ae-46e9-9c2f-bf08ae5a51ee
ms.localizationpriority: medium
ms.openlocfilehash: 730c345fe1746bf3284f9c0ce2c9bbeaa7ab0501
ms.sourcegitcommit: e4f3e1b2d08a02b9920e78e802234e5b674e7223
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/26/2018
ms.locfileid: "4206676"
---
# <a name="xbox-one-developer-mode-activation"></a>Ativação do Modo de Desenvolvedor do Xbox One

## <a name="how-developer-mode-works"></a>Como o Modo de Desenvolvedor funciona
O Xbox One tem dois modos: o Modo de *Varejo* (**1**) e o Modo de *Desenvolvedor* (**2**). No Modo de Varejo, o console está no estado usado por qualquer cliente ou usuário de um console Xbox One: você pode jogar jogos e executar aplicativos como usuário. No Modo de Desenvolvedor, você pode desenvolver softwares para o console, mas não pode jogar jogos de varejo ou executar aplicativos de varejo.

O Modo de Desenvolvedor pode ser habilitado em qualquer console Xbox One de varejo. Depois que ele é habilitado, você pode alternar livremente entre os Modos de Varejo e (**2a**) e de Desenvolvedor (**2b**).

![Modos do Xbox One](images/dev-mode-flow.png)

## <a name="activate-developer-mode-on-your-retail-xbox-one-console"></a>Ativar o Modo de Desenvolvedor no seu console Xbox One de varejo

1.  Inicie seu console do Xbox One.

2.  Procure e instale o app **Ativação do Modo de Desenvolvedor** na loja do Xbox One.

    ![Instalar o app Ativação do Modo de Desenvolvedor](images/devkit-activation-1.png)

3.  Inicie o aplicativo na página da Store.

    ![App Ativação do Modo de Desenvolvedor](images/devkit-activation-2.png)

4.  Observe o código exibido no aplicativo Ativação do Modo de Desenvolvedor.

    ![Etapa de Ativação 5](images/activation-step-5.png)  
    
5.  Vá para [partner.microsoft.com/xboxactivate](https://partner.microsoft.com/xboxactivate).

6.  Entre no Centro de Desenvolvimento com a sua conta do Centro de Desenvolvimento.

7.  Insira o código de ativação exibido no aplicativo Ativação do Modo de Desenvolvedor. Você tem um número limitado de ativações associadas à sua conta. Após a ativação do Modo de Desenvolvedor, o Centro de Desenvolvimento indicará que você usou uma das ativações associadas à sua conta.

    ![Etapa de Ativação 8](images/activation-step-8-rs2.png)    
    
8.  Clique em **Concordar e ativar**. Isso fará com que a página seja recarregada, e você verá seu dispositivo preenchido na tabela. Os termos para o contrato do Programa de Ativação do Modo de Desenvolvedor do Xbox One pode ser encontrado em [Programa de Ativação do Modo de Desenvolvedor do Xbox One](http://go.microsoft.com/fwlink/p/?LinkId=760399).

9.  Após a inserção do código de ativação, seu console exibirá uma tela de andamento referente ao processo de ativação.  
    
10. Concluída a ativação, abra o aplicativo Ativação do Modo de Desenvolvedor e clique em **Alternar e iniciar** para acessar o Modo de Desenvolvedor. Observe que essa etapa demorará mais do que o esperado.

    ![Etapa de Ativação 12](images/activation-step-12.png)   

## <a name="switch-between-retail-and-developer-mode"></a>Alternar entre o Modo de Varejo e o Modo de Desenvolvedor
Quando o Modo de Desenvolvedor tiver sido habilitado no seu console, use **Dev Home** para alternar entre ele e o Modo de Varejo. Para saber mais sobre como iniciar e usar a Dev Home, consulte [Introdução às ferramentas do Xbox One](introduction-to-xbox-tools.md).

* Para alternar para o Modo de varejo, abra a **Home Page de Desenvolvimento**. Em **Ações rápidas**, selecione **Sair do Modo de desenvolvimento**. Isso reiniciará o console no Modo de varejo.    

  ![Etapa de Ativação 13](images/activation-step-13-rs4.png)  
  
* Para alternar para o Modo de Desenvolvedor, use o aplicativo Ativação do Modo de Desenvolvedor. Abra o aplicativo e selecione **Alternar e reiniciar**. Isso reiniciará o seu console no Modo de Desenvolvedor.  

  ![Etapa de Ativação 14](images/activation-step-12.png)  

## <a name="see-also"></a>Consulte também
- [Desativação do Modo de Desenvolvedor do Xbox One](devkit-deactivation.md)
- [UWP no Xbox One](index.md)
