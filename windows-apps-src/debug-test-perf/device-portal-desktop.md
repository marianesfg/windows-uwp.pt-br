---
author: PatrickFarley
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal para desktop
description: "Saiba como o Windows Device Portal abre diagnósticos e automação em sua área de trabalho do Windows."
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 32155bfbb676a5f79dd4b1629f0a88368da36828
ms.sourcegitcommit: 0fa9ae00117e8e6b04ed38956e605bb74c1261c6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/07/2017
---
# <a name="device-portal-for-desktop"></a>Device Portal para desktop

A partir do Windows 10, versão 1607, outros recursos de desenvolvedor estão disponíveis para desktop. Esses recursos estão disponíveis apenas quando o Modo de Desenvolvedor está habilitado.

Para saber mais sobre como habilitar o Modo de Desenvolvedor, consulte [Habilitar seu dispositivo para desenvolvimento](../get-started/enable-your-device-for-development.md).

O Device Portal permite exibir informações de diagnóstico e interagir com seu desktop por HTTP no seu navegador. Você pode usar o Device Portal para fazer o seguinte:
- Ver e manipular uma lista de processos em execução
- Instalar, excluir, iniciar e encerrar os aplicativos
- Alterar perfis de Wi-Fi, exibir a intensidade do sinal e ver ipconfig
- Exibir gráficos dinâmicos de uso de CPU, memória, E/S, rede e GPU
- Coletar despejos de memória do processo
- Coletar rastreamentos do ETW 
- Manipular o armazenamento isolado dos aplicativos de sideload

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurar o Device Portal na área de trabalho do Windows

### <a name="turn-on-device-portal"></a>Ativar o portal de dispositivo

No menu **Configurações do Desenvolvedor**, com o Modo de Desenvolvedor habilitado, você pode habilitar o Device Portal.  

Ao habilitar o Device Portal, você também deverá criar um nome de usuário e senha para o Device Portal. Não use sua conta da Microsoft ou outras credenciais do Windows.  

Depois que o Device Portal estiver habilitado, você verá links para ele na parte inferior da seção **Configurações**. Anote o número da porta aplicado ao final da URL: esse número de porta é gerado aleatoriamente quando o Device Portal é habilitado, mas deve permanecer consistente entre reinicializações da área de trabalho. Se você gostaria de definir os números de portas manualmente para que eles sejam permanentes, consulte [Definindo números de porta](device-portal-desktop.md#setting-port-numbers).

Você pode escolher entre duas maneiras de se conectar ao Device Portal: host local e pela rede local (incluindo VPN).

**Para se conectar ao Device Portal**

1. Em seu navegador, insira o endereço mostrado aqui para o tipo de conexão que você está usando.

    - Localhost: `http://127.0.0.1:PORT` ou `http://localhost:PORT`

    Use esse endereço para exibir o Device Porta localmente.
    
    - Rede local: `https://<The IP address of the desktop>:PORT`

    Use esse endereço para se conectar por uma rede local.

O HTTPS é necessário para autenticação e comunicação segura.

Se você estiver usando o Device Portal em um ambiente protegido, como um laboratório de teste, onde confia em todos na rede local, não tem informações pessoais no dispositivo e tem requisitos exclusivos, você poderá desabilitar a autenticação. Isso permite a comunicação não criptografada e que qualquer pessoa com o endereço IP do seu computador controle-o.

## <a name="device-portal-pages"></a>Páginas do Device Portal

O Device Portal no desktop fornece o conjunto padrão de páginas. Para obter descrições detalhadas, consulte [Visão geral do Windows Device Portal](device-portal.md).

- Aplicativos
- Processos
- Desempenho
- Depuração
- ETW (Rastreamento de Eventos para Windows)
- Rastreamento de desempenho
- Dispositivos
- Rede
- Aplicativo Explorador de Arquivos 

## <a name="setting-port-numbers"></a>Configurando números de porta

Se você quiser selecionar números de porta para o Device Portal (como 80 e 443), será possível definir as seguintes chaves do registro:

- Em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - UseDynamicPorts: um DWORD necessário. Defina como 0 para manter os números de porta que você escolheu.
    - HttpPort: um DWORD necessário. Contém o número da porta na qual o Device Portal escutará conexões HTTP.  
    - HttpsPort: um DWORD necessário. Contém o número da porta na qual o Device Portal escutará conexões HTTPS.

## <a name="failure-to-install-developer-mode-package"></a>Falha ao instalar o pacote do Modo de Desenvolvedor
Às vezes, devido a problemas de rede ou de compatibilidade, o Modo de Desenvolvedor não será instalado corretamente. O pacote do Modo de Desenvolvedor é necessário para implantação **remota** no computador, usando o Device Portal em um navegador ou a descoberta de dispositivos para habilitar SSH, mas não para o desenvolvimento local.  Mesmo se você encontrar esses problemas, você ainda pode implementar seu aplicativo localmente usando o Visual Studio. 

Consulte o fórum [Problemas conhecidos](https://social.msdn.microsoft.com/Forums/en-US/home?forum=Win10SDKToolsIssues&sort=relevancedesc&brandIgnore=True&searchTerm=%22device+portal%22) e a [página do Modo de Desenvolvedor](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package) para encontrar soluções alternativas para esses problemas e muito mais. 

