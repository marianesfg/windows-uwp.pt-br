---
author: mcleblanc
ms.assetid: ba2ac5f5-1e0d-4f1d-a6f8-6a65b4cff501
description: "Esta seção descreve como portar seus apps existentes para a Plataforma Universal do Windows (UWP), onde você pode criar um único pacote de aplicativos do Windows 10 que os clientes podem instalar em todos os tipos de dispositivos. Seu app se beneficiará com o novo hardware instigante, ótimas oportunidades de monetização, um conjunto de APIs modernas, controles de interface do usuário adaptáveis e uma variedade de modalidades de entrada, incluindo mouse/teclado, toque e fala."
title: Portando apps para o Windows 10
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: cbb3b501fcaf1e51ca313423e812a4119ffca49c
ms.lasthandoff: 02/07/2017

---

# <a name="porting-apps-to-windows-10"></a>Portando apps para o Windows 10

[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção descreve como portar seus apps existentes para a Plataforma Universal do Windows (UWP), onde você pode criar um único pacote de aplicativos do Windows 10 que os clientes podem instalar em todos os tipos de dispositivos. Seu app se beneficiará com o novo hardware instigante, ótimas oportunidades de monetização, um conjunto de APIs modernas, controles de interface do usuário adaptáveis e uma variedade de modalidades de entrada, incluindo mouse/teclado, toque e fala.

O Windows Runtime (WinRT) é a tecnologia que permite criar apps da Plataforma Universal do Windows (UWP). Você pode consultar [O que é um app UWP (Plataforma Universal do Windows)?](https://msdn.microsoft.com/library/windows/apps/dn726767) para obter mais detalhes sobre os apps do WinRT e UWP.

Este guia de portabilidade explica as diferenças entre a tecnologia do seu app atual e a Plataforma Universal do Windows (UWP). Assim que o caminho entre as tecnologias for entendido, você poderá mergulhar no restante da Central de Desenvolvedores, que é um recurso abrangente para o desenvolvimento de apps UWP. Uma boa maneira de fazer isso, quando você estiver pronto, é começar sabendo [como desenvolver um app da Loja](https://msdn.microsoft.com/library/windows/apps/dn726537).

| Tópico | Descrição |
|-------|-------------|
| [Mudar do Windows Phone Silverlight para a UWP](wpsl-to-uwp-root.md) | Se você for um desenvolvedor com um app Windows Phone Silverlight, poderá fazer excelente uso de seu conjunto de habilidades e seu código-fonte na mudança para o Windows 10. Com o Windows 10, é possível criar um app UWP, que é um único pacote de apps que os clientes podem instalar em todos os tipos de dispositivo. |
| [Mudar do Windows Runtime 8.x para a UWP](w8x-to-uwp-root.md) | Se você tem um app Universal 8.1 voltado para o Windows 8.1, Windows Phone 8.1 ou ambos, saiba que seu código-fonte e suas habilidades serão portados perfeitamente para o Windows 10. Com o Windows 10, é possível criar um app UWP, que é um único pacote de apps que os clientes podem instalar em todos os tipos de dispositivo. |
| [Atualizar seu projeto do Microsoft Visual Studio 2015 RC para RTM na UWP](update-your-visual-studio-2015-rc-project-to-rtm.md) | Se você tiver um projeto do Windows 10 criado com o Microsoft Visual Studio 2015 RC, você tem duas opções de atualização dos arquivos de projeto para o formato adequado para Visual Studio 2015 RTM. O método recomendado é criar um novo projeto do Windows 10 no Visual Studio 2015 RTM e copiar seus arquivos para ele. Como alternativa, você pode seguir a documentação avançada para editar seus arquivos de projeto existentes e transferi-los para o novo formato. |
| [Mapeamento do conceito de aplicativos do Windows para desenvolvedores do Android e iOS](android-ios-uwp-map.md) | Se você for um desenvolvedor com habilidades em Android, iOS ou código e desejar mudar para o Windows 10 e a Plataforma Universal do Windows, esse recurso tem tudo o que você precisa para mapear recursos da plataforma — e o seu conhecimento — entre as três plataformas. |
| [Mudar do iOS para a UWP](ios-to-uwp-root.md) | Você é desenvolvedor do iOS e não sabe como fazer a transição para o Windows 10 e a UWP? Não é tão difícil quanto você pensa. Temos as ferramentas, as técnicas e as informações de que você precisa para criar excelentes apps que funcionam tão bem no Windows, quanto em seus dispositivos iOS: talvez melhor! |
| [Mudar do desktop para a UWP](desktop-to-uwp-root.md) | Converta seus apps da área de trabalho Win32 e .NET 4.6.1 em apps da Plataforma Universal do Windows (UWP). |
| [Mover um app Web para UWP](hwa-to-uwp-root.md) | Converter o seu app Web em apps UWP (Plataforma Universal do Windows). *Inclui instruções para usar o Windows ou Mac como sua plataforma de desenvolvimento, bem como instrução para conversão de um app do Chrome para trabalhar com UWP. |
 
## <a name="related-topics"></a>Tópicos relacionados

* [Mudar do WPF e do Silverlight para o WinRT](https://msdn.microsoft.com/library/windows/apps/dn263237)
* [Mudar do Android para o WinRT](https://msdn.microsoft.com/library/windows/apps/jj945421)
* [Mudar da Web para o WinRT](https://msdn.microsoft.com/library/windows/apps/hh465151)

