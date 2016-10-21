---
author: v-angraf
title: "O que há de novo para UWP no Xbox One"
description: Destaca os novos recursos para aplicativos UWP em Xbox One.
translationtype: Human Translation
ms.sourcegitcommit: 044aac722180015586487dcc8738facccf209f5c
ms.openlocfilehash: 4cc1e0b495a80e019296b9c3be9e75a37c60224a

---

# Novidades para desenvolvedores na atualização mais recente da UWP em Xbox One

A versão de julho de 2016 da Plataforma Universal do Windows (UWP) no Xbox One contém os seguintes novos recursos, atualizações para os recursos existentes, e correções de bugs.

## Rede usando soquetes TCP/UDP está disponível agora  
O acesso à rede de entrada e de saída do console que usa soquetes TCP/UDP tradicionais (WinSock, Windows.Networking.Sockets) está disponível agora.

## Suporte de Fiddler
Agora você pode habilitar o Fiddler como um proxy para um console que tenha habilitado a UWP no Xbox One. O Fiddler permite que você faça logon e inspecione todo o tráfego HTTP/HTTPS dos e a partir dos serviços do Xbox e serviços web de terceiros. Para obter mais informações, consulte [Como usar o Fiddler com o Xbox One durante o desenvolvimento do UWP](uwp-fiddler.md).

## Modo de mouse agora é habilitado por padrão
Modo de mouse agora é habilitado por padrão para aplicativos Web hospedados e XAML.
É altamente recomendável desativar essa opção e otimizar para navegação direcional de controlador.
Para saber como desativar o modo de mouse, consulte [Como desabilitar o modo de mouse](how-to-disable-mouse-mode.md).
Para obter mais informações sobre como compilar ótimos aplicativos para Xbox, consulte [Projetando para Xbox e TV](../input-and-devices/designing-for-tv.md#mouse-mode).

## Área de superfície de API UWP estendida agora está funcional no console
APIs de UWP adicionais agora são funcionais no console do Xbox. Para obter mais informações sobre o suporte a API de UWP, consulte [Recursos UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/p/?LinkID=760755). 

## Recursos de áudio e música em segundo plano
Agora você pode reproduzir música e áudio de um aplicativo que está em execução no plano de fundo.

## Melhorias de XAML
Os seguintes aprimoramentos foram feitos para a plataforma XAML:
-   Agora o retângulo de foco é estilizado para uma experiência de 10 pés de televisão.
-   Os sons Xbox agora são incorporados na plataforma XAML.
-   Navegação de foco XY entre elementos de interface do usuário foi aprimorada. 

## Já é possível alterar o tamanho do armazenamento de desenvolvedor alocado no console
Uma nova configuração no aplicativo Dev Home permite aumentar ou diminuir o tamanho do armazenamento do desenvolvedor alocado no console. Para obter mais informações sobre como alterar o tamanho do armazenamento do desenvolvedor alocado, consulte [Introdução a ferramentas do Xbox One](introduction-to-xbox-tools.md).

## Melhorias na ferramenta WDP
As seguintes melhorias foram feitas na ferramenta Windows Device Portal (WDP) para Xbox:
 - A ferramenta inclui configurações do console adicionais. Para obter mais informações sobre configurações do console, consulte o tópico de referência [/ext/settings](wdp-xboxsettings-api.md). 
 - Os usuários podem entrar e sair do console. Para obter mais informações sobre usuários, consulte o tópico de referência [/ext/user](wdp-user-management.md).
 - Já é possível fazer uma captura de tela do console. Para obter mais informações sobre fazer uma captura de tela, consulte o tópico de referência [/ext/screenshot](wdp-media-capture-api.md).
 - A ferramenta pode implantar uma compilação de arquivo flexível do aplicativo. Para obter mais informações compilações de arquivo flexíveis, consulte o tópico de referência [/api/app/packagemanager/register](wdp-loose-folder-register-api.md).
 - Os arquivos de desenvolvedor no console podem ser acessados pelo Explorador de Arquivos no computador de desenvolvimento. Para obter mais informações sobre como acessar arquivos por meio do Explorador de Arquivos, consulte o tópico de referência [/ext/smb/developerfolder](wdp-smb-api.md).

## Consulte também
- [Problemas conhecidos](known-issues.md)
- [UWP no Xbox One](index.md)



<!--HONumber=Aug16_HO3-->


