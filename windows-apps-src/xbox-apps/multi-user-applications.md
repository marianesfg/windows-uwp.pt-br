---
author: Mtoepke
title: "Introdução aos aplicativos multiusuários"
description: 
area: Xbox
ms.sourcegitcommit: f225811bd18be22807160e8670a1b7b8d51e4b10
ms.openlocfilehash: 20f84783131122343fd01e6cb1f5a60cf50158cd

---

# Introdução aos aplicativos multiusuários

Este tópico se destina a ser uma introdução de alto nível simples para o modelo multiusuário do Xbox.

> 
            **Observação**
            &nbsp;&nbsp;Nesta visualização inicial de desenvolvedor, os aplicativos multiusuários não estão habilitados. Em uma futura visualização de desenvolvedor, os aplicativos multiusuários serão habilitados e, na ocasião, publicaremos documentação, diretrizes e exemplos mais detalhados. 

O modelo de usuário do Xbox One está ajustado aos requisitos de um console de jogos que dá suporte a vários usuários jogando de maneira cooperativa em um único dispositivo. Ele permite que vários usuários, cada um com seu próprio controlador, entre e use o console ao mesmo tempo em uma única sessão interativa. Isso é diferente do que ocorre com outros dispositivos Windows. Por exemplo:
* 
            Os **computadores desktop com o Windows** permitem que vários usuários usem o mesmo dispositivo, mas cada usuário tem sua própria sessão interativa e cada sessão é completamente independente das outras sessões no dispositivo.
* 
            Os **telefones Windows** permitem que um único usuário use o dispositivo. O usuário único é determinado durante a OOBE (configuração inicial pelo usuário) e o usuário não pode sair depois que está conectado. Se um usuário diferente quiser usar o dispositivo, o dispositivo deverá ser redefinido. 
* 
            O **Xbox One** permite que vários usuários entrem e usem o dispositivo ao mesmo tempo em uma única sessão interativa.

Cada usuário no modelo de usuário Xbox One é respaldado por uma conta de usuário local. Essa conta de usuário local é associada a uma conta do Xbox Live (e, portanto, uma conta da Microsoft). Isso significa que há um mapeamento individual estrito de uma conta de usuário do Xbox para uma conta do Xbox Live e uma conta da Microsoft.

## Aplicativos de usuário único
Por padrão, os aplicativos UWP são executados no contexto do usuário que iniciou o aplicativo. Esses "aplicativos de usuário único" (SUAs) reconhecem apenas esse usuário único e executam em um modo compatível com o modelo de usuário em outros dispositivos Windows. O modelo de usuário do Xbox gerencia qual usuário está associado ao aplicativo e garante que um usuário esteja conectado quando o aplicativo for iniciado. Nesse modelo, os aplicativos UWP e os autores de jogos não precisam fazer nada especial para executar no Xbox. 

## Aplicativos multiusuários
Os jogos UWP podem aceitar o modelo multiusuário do Xbox One. Esses aplicativos"multiusuários" (MUAs) são executados no contexto de uma conta do sistema (chamada de conta padrão) e podem tirar proveito da flexibilidade e da potência do modelo de usuário do Xbox One. Para esses jogos, o modelo de usuário do Xbox não gerencia qual usuário está associado ao jogo e sequer exige que um usuário esteja conectado para que o jogo seja executado. Isso significa que eles precisam ser escritos para reconhecerem explicitamente e gerenciarem seus requisitos de usuário: exigindo ou não que um usuário esteja conectado, implementando ou não o conceito de usuário atual, permitindo ou não a entrada simultânea de vários usuários etc.
   
Para aderir ao modelo multiusuário:   
1. Abra seu projeto no Visual Studio.   
2. Selecione o arquivo package.appxmanifest.xml.   
3. Clique com o botão direito do mouse e selecione Exibir Código.   
4. Adicione a seguinte linha na seção `<Properties></Properties>`:

`<uap:SupportedUsers>multiple</uap:SupportedUsers>`

##Orientação sobre o modelo que deve ser escolhido
Todos os aplicativos UWP e a maioria dos jogos de usuário único podem ser programados para ser SUA. Recomendamos que apenas jogos cooperativos multijogadores aceitem o modelo multiusuário do Xbox One. Nós forneceremos documentação, orientações e exemplos mais detalhados em uma futura visualização de desenvolvedor.

## Veja também
- [UWP no Xbox One](index.md)



<!--HONumber=Jun16_HO5-->


