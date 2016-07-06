---
author: v-angraf
title: "O que há de novo para UWP no Xbox One"
description: Destaca os novos recursos para UWP em aplicativos do Xbox One.
area: Xbox
ms.sourcegitcommit: 59019f209729b56e02ebdbdfd53a8fbf835c69f7
ms.openlocfilehash: dfa94ad42a79d0f6b3f72fbf2efe9ce043532c56

---

# Novidades no Developer Preview para UWP no Xbox One de junho de 2016

A versão de junho de 2016 Developer Preview da Plataforma Universal do Windows (UWP) no Xbox One contém os seguintes novos recursos, atualizações para os recursos existentes, e correções de bugs.

## Modo de mouse agora é habilitado por padrão
Modo de mouse agora é habilitado por padrão para aplicativos Web hospedados e XAML.
É altamente recomendável desativar essa opção e otimizar para navegação direcional de controlador.
Para saber como desativar o modo de mouse, consulte [Como desabilitar o modo de mouse](how-to-disable-mouse-mode.md).
Para obter mais informações sobre como compilar ótimos aplicativos para Xbox, consulte [Projetando para Xbox e TV](https://msdn.microsoft.com/en-us/windows/uwp/input-and-devices/designing-for-tv?f=255&MSPPError=-2147217396#mouse-mode).

## Área de superfície de API UWP estendida agora está funcional no console
APIs de UWP adicionais agora são funcionais no console do Xbox. Para obter mais informações sobre o suporte a API de UWP, consulte [Recursos UWP que ainda não têm suporte no Xbox](http://go.microsoft.com/fwlink/?LinkID=760755). 

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
- [UWP no Xbox One](index.md)
- [Problemas conhecidos](known-issues.md)



<!--HONumber=Jun16_HO4-->


