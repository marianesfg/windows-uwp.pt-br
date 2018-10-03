---
title: Autenticação e identidade do usuário
description: Aplicativos da Plataforma Universal do Windows (UWP) têm várias opções para autenticação do usuário, desde logon único (SSO) usando agente de autenticação da Web até autenticação de dois fatores altamente segura.
ms.assetid: 53E36DDC-200A-4850-AAF0-07ECA3662BB9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 6f446299dcf1a0bcf93d483d13c926c6e4cd230f
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4316704"
---
# <a name="authentication-and-user-identity"></a>Autenticação e identidade do usuário



Aplicativos da Plataforma Universal do Windows (UWP) têm várias opções para autenticação do usuário, desde logon único (SSO) usando [agente de autenticação da Web](web-authentication-broker.md) até autenticação de dois fatores altamente segura.

Para conexões de aplicativos regulares a serviços de provedor de identidade de terceiros, como Facebook, Twitter, Flickr etc., use o [agente de autenticação Web](web-authentication-broker.md). Para mais comodidade, use o [Cofre de Credenciais](credential-locker.md) para salvar e transferir as informações de logon do usuário.

Recomenda-se enfaticamente que as empresas que usam o Windows 10 considerem usar [Microsoft Passport e Windows Hello](microsoft-passport.md), que habilitam autenticação de dois fatores altamente segura. Se não for possível usar o Microsoft Passport, os [cartões inteligentes](smart-cards.md) e a [biometria de impressão digital](fingerprint-biometrics.md) podem acrescentar uma camada adicional de segurança.

<table>
<tr><th>Tópico</th><th>Descrição</th></tr>
<tr><td><a href="credential-locker.md">Cofre de credenciais</a></td><td>Este artigo descreve como aplicativos podem usar o Cofre de Credenciais para armazenar e recuperar credenciais do usuário com segurança, além de alternar dispositivos com a conta da Microsoft do usuário.</td></tr>

<tr><td><a href="fingerprint-biometrics.md">Biometria por impressão digital</a> </td><td>Este artigo explica como adicionar biometria por impressão digital ao aplicativo. Incluir um pedido de autenticação por impressão digital quando o usuário precisar autorizar uma determinada ação aprimora a segurança do seu aplicativo. Por exemplo, você pode exigir autenticação por impressão digital antes de autorizar uma compra realizada em aplicativo ou acessar recursos restritos. A autenticação por impressão digital é gerenciada com a classe <a href="https://msdn.microsoft.com/library/windows/apps/dn279134">UserConsentVerifier</a> no namespace <a href="https://msdn.microsoft.com/library/windows/apps/hh701356">Windows.Security.Credentials.UI</a>.</td></tr>
<tr><td><a href="microsoft-passport.md">Microsoft Passport e Windows Hello</a></td><td>Este artigo descreve a nova tecnologia Microsoft Passport do Windows 10 e aborda como os desenvolvedores podem implementar essa tecnologia para proteger os aplicativos e os serviços back-end. Ele destaca os recursos específicos dessas tecnologias que ajudam a mitigar os riscos das credenciais convencionais e orienta sobre como criar e implantar essas tecnologias como parte de sua distribuição do Windows 10. </td></tr>
<tr><td><a href="microsoft-passport-login.md">Criar um aplicativo de logon do Microsoft Passport</a></td><td>Parte 1 de um guia passo a passo completo sobre como criar um aplicativo UWP (Plataforma Universal do Windows) do Windows 10 que usa o Microsoft Passport como uma alternativa para sistemas tradicionais de autenticação de nome de usuário e senha.</td></tr>
<tr><td><a href="microsoft-passport-login-auth-service.md">Criar um serviço de logon do Microsoft Passport</a></td><td>Parte 2 de um guia passo a passo completo sobre como usar o Microsoft Passport como uma alternativa para sistemas tradicionais de autenticação de nome de usuário e senha em aplicativos Windows 10 UWP (Plataforma Universal do Windows).</td></tr>
<tr><td><a href="smart-cards.md">Cartões inteligentes</a></td><td>Este tópico explica como os aplicativos podem usar cartões inteligentes para conectar usuários a serviços de rede seguros, inclusive como acessar leitores de cartão inteligente físicos, criar cartões inteligentes virtuais, comunicar-se usando cartões inteligentes, autenticar usuários, redefinir PINs de usuário e remover ou desconectar cartões inteligentes.</td></tr>
<tr><td><a href="share-certificates.md">Compartilhar certificados entre aplicativos</a></td><td>Os aplicativos UWP que requerem autenticação segura além de uma combinação de ID de usuário e senha podem usar certificados na autenticação. A autenticação de certificado oferece um elevado nível de confiança ao autenticar um usuário. Em alguns casos, um grupo de serviços desejará autenticar um usuário para vários aplicativos. Este artigo mostra como você pode autenticar vários aplicativos usando o mesmo certificado e fornecer código prático para que um usuário importe um certificado que foi fornecido para acessar serviços Web seguros.</td></tr>
<tr><td><a href="companion-device-unlock.md">Desbloqueio do Windows com dispositivos complementares IoT</a></td><td>Dispositivo complementar é um dispositivo que pode atuar em conjunto com sua área de trabalho do Windows 10 para melhorar a experiência de autenticação do usuário. Usando a Estrutura de Dispositivo Complementar, um dispositivo complementar pode fornecer uma experiência avançada para o Microsoft Passport mesmo quando o Windows Hello não está disponível (por exemplo, se a área de trabalho do Windows 10 não tiver uma câmera para autenticação de face ou dispositivo de leitor de impressão digital, por exemplo).</td></tr>
<tr><td><a href="web-account-manager.md">Gerenciador de contas da Web</a></td><td>Este artigo descreve como mostrar o AccountsSettingsPane e conectar seu aplicativo da Plataforma Universal do Windows (UWP) a provedores de identidade externos, como a Microsoft ou o Facebook, usando as novas APIs do Gerenciador de Contas da Web do Windows 10. Você aprenderá a solicitar a permissão de um usuário para usar a conta da Microsoft dele, obter um token de acesso e usá-lo para realizar operações básicas (como obter dados de perfil ou carregar arquivos no OneDrive). </td></tr>
<tr><td><a href="web-authentication-broker.md">Agente de autenticação da Web</a></td><td>Este artigo explica como conectar o aplicativo a um provedor de identidade online que usa protocolos de autenticação, como OpenID ou OAuth, como Facebook, Twitter, Flickr, Instagram etc. O método <a href="https://msdn.microsoft.com/library/windows/apps/br212066">AuthenticateAsync</a> envia uma solicitação ao provedor de identidade online e obtém um token de acesso que descreve os recursos do provedor aos quais o aplicativo tem acesso.</td></tr>
</table>

