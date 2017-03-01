---
author: Mtoepke
title: "Introdução aos apps multiusuário"
description: "Uma introdução de alto nível simples para o modelo multiusuário do Xbox."
ms.author: mtoepke
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 2dde6ed3-7f53-48a6-aebe-2605230decb8
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 31866c400683c61b432c3fe620487f93a9187771
ms.lasthandoff: 02/08/2017

---

# <a name="introduction-to-multi-user-applications"></a>Introdução aos apps multiusuário

Este tópico se destina a ser uma introdução de alto nível simples para o modelo multiusuário do Xbox.

> [!NOTE]
> Aplicativos multiusuário não estão habilitados no momento. 

O modelo de usuário do Xbox One está ajustado aos requisitos de um console de jogos que dá suporte a vários usuários jogando de maneira cooperativa em um único dispositivo. Ele permite que vários usuários, cada um com seu próprio controlador, entre e use o console ao mesmo tempo em uma única sessão interativa. Isso é diferente do que ocorre com outros dispositivos Windows. Por exemplo:
* Os **computadores desktop com o Windows** permitem que vários usuários usem o mesmo dispositivo, mas cada usuário tem sua própria sessão interativa e cada sessão é completamente independente das outras sessões no dispositivo.
* Os **telefones Windows** permitem que um único usuário use o dispositivo. O usuário único é determinado durante a OOBE (configuração inicial pelo usuário) e o usuário não pode sair depois que está conectado. Se um usuário diferente quiser usar o dispositivo, o dispositivo deverá ser redefinido. 
* O **Xbox One** permite que vários usuários entrem e usem o dispositivo ao mesmo tempo em uma única sessão interativa.

Cada usuário no modelo de usuário Xbox One é respaldado por uma conta de usuário local. Essa conta de usuário local é associada a uma conta do Xbox Live (e, portanto, uma conta da Microsoft). Isso significa que há um mapeamento individual estrito de uma conta de usuário do Xbox para uma conta do Xbox Live e uma conta da Microsoft.

## <a name="single-user-applications"></a>Aplicativos de usuário único
Por padrão, os apps da Plataforma Universal do Windows (UWP) são executados no contexto do usuário que iniciou o app. Esses *apps de usuário único* (SUAs) reconhecem apenas esse usuário único e executam em um modo compatível com o modelo de usuário em outros dispositivos Windows. O modelo de usuário do Xbox gerencia qual usuário está associado ao app e garante que um usuário esteja conectado quando o app for iniciado. Nesse modelo, os apps UWP e os autores de jogos não precisam fazer nada especial para executar no Xbox. 

## <a name="multi-user-applications"></a>Aplicativos multiusuários
Os jogos UWP podem aceitar o modelo multiusuário do Xbox One. Esses *apps multiusuário* (MUAs) são executados no contexto de uma conta do sistema (chamada de conta padrão) e podem tirar proveito da flexibilidade e da potência do modelo de usuário do Xbox One. Para esses jogos, o modelo de usuário do Xbox não gerencia qual usuário está associado ao jogo e sequer exige que um usuário esteja conectado para que o jogo seja executado. Isso significa que eles precisam ser escritos para reconhecerem explicitamente e gerenciarem seus requisitos de usuário: exigindo ou não que um usuário esteja conectado, implementando ou não o conceito de usuário atual, permitindo ou não a entrada simultânea de vários usuários etc.
   
Para aderir ao modelo multiusuário:   
1. Abra seu projeto no Visual Studio.   
2. Selecione o arquivo package.appxmanifest.xml.   
3. Clique com o botão direito do mouse e selecione **Exibir Código**.   
4. Adicione a seguinte linha na seção `<Properties></Properties>`:

```
<uap:SupportedUsers>multiple</uap:SupportedUsers>
```

### <a name="identifying-users-and-inputs"></a>Identificação de usuários e entradas
Os desenvolvedores podem usar KeyRoutedEventArgs.DeviceId, utilizado por eventos KeyUp e KeyDown encaminhados, para diferenciar os eventos gerados de entradas diferentes.
O uso do método Windows.System.UserDeviceAssociation.FindUserFromDeviceId ajudará a identificar o usuário associado a uma entrada específica.

Consulte o tópico [KeyRoutedEventArgs.DeviceId](https://msdn.microsoft.com/library/windows/apps/windows.ui.xaml.input.keyroutedeventargs.deviceid) para obter mais informações.


## <a name="guidance-on-which-model-to-choose"></a>Orientação sobre o modelo que deve ser escolhido
Todos os apps UWP e a maioria dos jogos de usuário único podem ser programados para serem SUAs. Recomendamos que apenas jogos cooperativos multijogadores aceitem o modelo multiusuário do Xbox One.

## <a name="see-also"></a>Consulte também
- [UWP no Xbox One](index.md)

