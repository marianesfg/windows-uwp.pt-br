---
title: "Segurança"
description: "Esta seção contém artigos sobre como compilar apps seguros da Plataforma Universal do Windows (UWP) para o Windows 10."
ms.assetid: 41E2EEFB-E8A9-4592-814C-72B703CD952C
author: awkoren
ms.author: alkoren
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: e769a9aa7d381a527c1e1504efc52c2ead031c70
ms.lasthandoff: 02/07/2017

---

# <a name="security"></a>Segurança


[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Esta seção contém artigos sobre como compilar apps seguros da Plataforma Universal do Windows (UWP) para o Windows 10.

## <a name="introduction"></a>Introdução 

Se você for iniciante no desenvolvimento em Windows ou UWP, comece com a [Introdução ao desenvolvimento de apps seguros do Windows](intro-to-secure-windows-app-development.md). Este artigo de nível introdutório fornece uma visão geral das considerações de segurança para apps e os vários recursos disponíveis no Windows 10.

## <a name="authentication-and-user-identity"></a>Autenticação e identidade do usuário

A [seção de autenticação e identidade do usuário](authentication-and-user-identity.md) contém instruções passo a passo para cenários relacionados ao logon do usuário e à identidade. Os apps têm diversas opções para a autenticação do usuário, desde SSO (logon único) usando [agente de autenticação da Web](web-authentication-broker.md) até autenticação de dois fatores altamente segura.

<table>
<tr><th>Tópico</th><th>Descrição</th></tr>
<tr><td>[Cofre de credenciais](credential-locker.md)</td><td>Este artigo descreve como apps podem usar o Cofre de Credenciais para armazenar e recuperar credenciais do usuário com segurança, além de alternar dispositivos com a conta da Microsoft do usuário.</td></tr>

<tr><td>[Biometria por impressão digital](fingerprint-biometrics.md) </td><td>Este artigo explica como adicionar biometria por impressão digital ao app. Incluir um pedido de autenticação por impressão digital quando o usuário precisar autorizar uma determinada ação aprimora a segurança do seu app. Por exemplo, você pode exigir autenticação por impressão digital antes de autorizar uma compra realizada em app ou acessar recursos restritos. A autenticação por impressão digital é gerenciada com a classe [UserConsentVerifier](https://msdn.microsoft.com/library/windows/apps/dn279134) no namespace [Windows.Security.Credentials.UI](https://msdn.microsoft.com/library/windows/apps/hh701356).</td></tr>
<tr><td>[Microsoft Passport e Windows Hello](microsoft-passport.md)</td><td>Este artigo descreve a nova tecnologia Microsoft Passport do Windows 10 e aborda como os desenvolvedores podem implementar essa tecnologia para proteger os apps e os serviços back-end. Ele destaca os recursos específicos dessas tecnologias que ajudam a mitigar os riscos das credenciais convencionais e orienta sobre como criar e implantar essas tecnologias como parte de sua distribuição do Windows 10. </td></tr>
<tr><td>[Criar um app de logon do Microsoft Passport](microsoft-passport-login.md)</td><td>Parte 1 de um guia passo a passo completo sobre como criar um app UWP (Plataforma Universal do Windows) do Windows 10 que usa o Microsoft Passport como uma alternativa para sistemas tradicionais de autenticação de nome de usuário e senha.</td></tr>
<tr><td>[Criar um serviço de logon do Microsoft Passport](microsoft-passport-login-auth-service.md)</td><td>Parte 2 de um guia passo a passo completo sobre como usar o Microsoft Passport como uma alternativa para sistemas tradicionais de autenticação de nome de usuário e senha em apps Windows 10 UWP (Plataforma Universal do Windows).</td></tr>
<tr><td>[Cartões inteligentes](smart-cards.md)</td><td>Este tópico explica como os apps podem usar cartões inteligentes para conectar usuários a serviços de rede seguros, inclusive como acessar leitores de cartão inteligente físicos, criar cartões inteligentes virtuais, comunicar-se usando cartões inteligentes, autenticar usuários, redefinir PINs de usuário e remover ou desconectar cartões inteligentes.</td></tr>
<tr><td>[Compartilhar certificados entre apps](share-certificates.md)</td><td>Os apps UWP que requerem autenticação segura além de uma combinação de ID de usuário e senha podem usar certificados na autenticação. A autenticação de certificado oferece um elevado nível de confiança ao autenticar um usuário. Em alguns casos, um grupo de serviços desejará autenticar um usuário para vários apps. Este artigo mostra como você pode autenticar vários apps usando o mesmo certificado e fornecer código prático para que um usuário importe um certificado que foi fornecido para acessar serviços Web seguros.</td></tr>
<tr><td>[Desbloqueio do Windows com dispositivos complementares IoT](companion-device-unlock.md)</td><td>Dispositivo complementar é um dispositivo que pode atuar em conjunto com sua área de trabalho do Windows 10 para melhorar a experiência de autenticação do usuário. Usando a Estrutura de Dispositivo Complementar, um dispositivo complementar pode fornecer uma experiência avançada para o Microsoft Passport mesmo quando o Windows Hello não está disponível (por exemplo, se a área de trabalho do Windows 10 não tiver uma câmera para autenticação de face ou dispositivo de leitor de impressão digital, por exemplo).</td></tr>
<tr><td>[Gerenciador de Contas da Web](web-account-manager.md)</td><td>Este artigo descreve como mostrar o AccountsSettingsPane e conectar seu app da Plataforma Universal do Windows (UWP) a provedores de identidade externos, como a Microsoft ou o Facebook, usando as novas APIs do Gerenciador de Contas da Web do Windows 10. Você aprenderá a solicitar a permissão de um usuário para usar a conta da Microsoft dele, obter um token de acesso e usá-lo para realizar operações básicas (como obter dados de perfil ou carregar arquivos no OneDrive). </td></tr>
<tr><td>[Agente de autenticação da Web](web-authentication-broker.md)</td><td>Este artigo explica como conectar o app a um provedor de identidade online que usa protocolos de autenticação, como OpenID ou OAuth, como Facebook, Twitter, Flickr, Instagram etc. O método [AuthenticateAsync](https://msdn.microsoft.com/library/windows/apps/br212066) envia uma solicitação ao provedor de identidade online e obtém um token de acesso que descreve os recursos do provedor aos quais o app tem acesso.</td></tr>
</table>

## <a name="cryptography"></a>Criptografia 

A seção de criptografia contém informações sobre tópicos relacionados mais complexos, criptográficos. 

| Tópico                                                                         | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            |
|-------------------------------------------------------------------------------|------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [Introdução aos certificados](certificates.md)                                      | Este artigo aborda o uso de certificados em apps. Os certificados digitais são usados na criptografia de chave pública para associar uma chave pública a uma pessoa, um computador ou uma organização. As identidades associadas são usadas com mais frequência para autenticar uma entidade em outra. Por exemplo, os certificados são geralmente usados para autenticar um servidor Web em um usuário e um usuário em um servidor Web. É possível criar solicitações de certificados e instalar ou importar os certificados emitidos. Também é possível inscrever um certificado em uma hierarquia de certificados. |
| [Chaves criptográficas](cryptographic-keys.md)                                   | Este artigo mostra como usar funções de derivação de chaves padrão para derivar chaves e como criptografar conteúdo usando chaves simétricas e assimétricas.                                                                                                                                                                                                                                                                                                                                                                         |
| [Proteção de dados](data-protection.md)                                         | Este artigo explica como usar a classe [DataProtectionProvider](https://msdn.microsoft.com/library/windows/apps/br241559) no [Windows.Security.Cryptography.DataProtection](https://msdn.microsoft.com/library/windows/apps/br241585) para criptografar e descriptografar dados digitais em um app UWP.                                                                                                                                                                                                              |
| [MACs, hashes e assinaturas](macs-hashes-and-signatures.md)               | Este artigo aborda como códigos de autenticação de mensagem (MACs), hashes e assinaturas podem ser usados em apps para detectar adulteração de mensagem.                                                                                                                                                                                                                                                                                                                                                                                |
| [Restrições de exportação na criptografia](export-restrictions-on-cryptography.md) | Use essas informações para determinar se o app usa a criptografia de maneira que isso possa evitar que ele seja listado na .                                                                                                                                                                                                                                                                                                                                                                                                     |
| [Tarefas comuns de criptografia](common-cryptography-tasks.md)                     | Estes artigos fornecem código de amostra para tarefas de criptografia comuns, como criar números aleatórios, comparar buffers, converter entre cadeias de caracteres e dados binários, copiar de e para matrizes de bytes e codificar e decodificar dados.                                                                                                                                                                                                                                                                                    |

