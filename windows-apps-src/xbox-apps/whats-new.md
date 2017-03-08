---
author: v-angraf
title: "O que há de novo para UWP no Xbox One"
description: Destaca os novos recursos para aplicativos UWP em Xbox One.
ms.author: v-angraf
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: fe63c527-8f06-43a5-868f-de909f5664b3
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 96f9c9ef355382c72423187a7f81635571762071
ms.lasthandoff: 02/08/2017

---

# <a name="whats-new-for-developers-in-the-latest-update-of-uwp-on-xbox-one"></a>Novidades para desenvolvedores na atualização mais recente da UWP em Xbox One

A versão de julho de 2016 da Plataforma Universal do Windows (UWP) no Xbox One contém os seguintes novos recursos, atualizações para os recursos existentes, e correções de bugs.

## <a name="networking-using-tcpudp-sockets-is-now-available"></a>Rede usando soquetes TCP/UDP está disponível agora  
O acesso à rede de entrada e de saída do console que usa soquetes TCP/UDP tradicionais (WinSock, Windows.Networking.Sockets) está disponível agora.

## <a name="fiddler-support"></a>Suporte de Fiddler
Agora você pode habilitar o Fiddler como um proxy para um console que tenha habilitado a UWP no Xbox One. O Fiddler permite que você faça logon e inspecione todo o tráfego HTTP/HTTPS dos e a partir dos serviços do Xbox e serviços web de terceiros. Para obter mais informações, consulte [Como usar o Fiddler com o Xbox One durante o desenvolvimento do UWP](uwp-fiddler.md).

## <a name="mouse-mode-is-now-enabled-by-default"></a>Modo de mouse agora é habilitado por padrão
Modo de mouse agora é habilitado por padrão para aplicativos Web hospedados e XAML.
É altamente recomendável desativar essa opção e otimizar para navegação direcional de controlador.
Para saber como desativar o modo de mouse, consulte [Como desabilitar o modo de mouse](how-to-disable-mouse-mode.md).
Para obter mais informações sobre como compilar ótimos aplicativos para Xbox, consulte [Projetando para Xbox e TV](../input-and-devices/designing-for-tv.md#mouse-mode).

## <a name="extended-uwp-api-surface-area-is-now-functional-on-the-console"></a>Área de superfície de API UWP estendida agora está funcional no console
APIs de UWP adicionais agora são funcionais no console do Xbox. Para obter mais informações sobre o suporte a API de UWP, consulte [Recursos UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/p/?LinkID=760755). 

## <a name="background-music-and-audio-capabilities"></a>Recursos de áudio e música em segundo plano
Agora você pode reproduzir música e áudio de um aplicativo que está em execução no plano de fundo.

## <a name="xaml-improvements"></a>Melhorias de XAML
Os seguintes aprimoramentos foram feitos para a plataforma XAML:
-    Agora o retângulo de foco é estilizado para uma experiência de 10 pés de televisão.
-    Os sons Xbox agora são incorporados na plataforma XAML.
-    Navegação de foco XY entre elementos de interface do usuário foi aprimorada. 

## <a name="you-can-now-change-the-size-of-allocated-developer-storage-on-the-console"></a>Já é possível alterar o tamanho do armazenamento de desenvolvedor alocado no console
Uma nova configuração no aplicativo Dev Home permite aumentar ou diminuir o tamanho do armazenamento do desenvolvedor alocado no console. Para obter mais informações sobre como alterar o tamanho do armazenamento do desenvolvedor alocado, consulte [Introdução a ferramentas do Xbox One](introduction-to-xbox-tools.md).

## <a name="wdp-tool-enhancements"></a>Melhorias na ferramenta WDP
As seguintes melhorias foram feitas na ferramenta Windows Device Portal (WDP) para Xbox:
 - A ferramenta inclui configurações do console adicionais. Para obter mais informações sobre configurações do console, consulte o tópico de referência [/ext/settings](wdp-xboxsettings-api.md). 
 - Os usuários podem entrar e sair do console. Para obter mais informações sobre usuários, consulte o tópico de referência [/ext/user](wdp-user-management.md).
 - Já é possível fazer uma captura de tela do console. Para obter mais informações sobre fazer uma captura de tela, consulte o tópico de referência [/ext/screenshot](wdp-media-capture-api.md).
 - A ferramenta pode implantar uma compilação de arquivo flexível do aplicativo. Para obter mais informações compilações de arquivo flexíveis, consulte o tópico de referência [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Os arquivos de desenvolvedor no console podem ser acessados pelo Explorador de Arquivos no computador de desenvolvimento. Para obter mais informações sobre como acessar arquivos por meio do Explorador de Arquivos, consulte o tópico de referência [/ext/smb/developerfolder](wdp-smb-api.md).

## <a name="see-also"></a>Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)

