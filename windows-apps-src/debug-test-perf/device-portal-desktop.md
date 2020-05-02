---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Portal de Dispositivos para Área de Trabalho do Windows
description: Saiba como o Windows Device Portal abre diagnósticos e automação em sua área de trabalho do Windows.
ms.date: 02/06/2019
ms.topic: article
keywords: windows 10, uwp, device portal
ms.localizationpriority: medium
ms.openlocfilehash: 73f7e827c0ec8ca289d3523da06601de978a91d2
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "79210182"
---
# <a name="device-portal-for-windows-desktop"></a>Portal de Dispositivos para Área de Trabalho do Windows

O Portal de Dispositivos do Windows permite que você veja informações de diagnóstico e interagir com seu desktop por HTTP em uma janela de seu navegador. Você pode usar o Device Portal para fazer o seguinte:
- Ver e manipular uma lista de processos em execução
- Instalar, excluir, iniciar e encerrar os aplicativos
- Alterar perfis de Wi-Fi, exibir a intensidade do sinal e ver ipconfig
- Exibir gráficos dinâmicos de uso de CPU, memória, E/S, rede e GPU
- Coletar despejos de memória do processo
- Coletar rastreamentos do ETW 
- Manipular o armazenamento isolado dos aplicativos de sideload

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurar o Portal de Dispositivos na Área de Trabalho do Windows

### <a name="turn-on-developer-mode"></a>Ligar o modo de desenvolvedor

Começando no Windows 10, versão 1607, alguns dos recursos mais recentes para desktop só estão disponíveis quando o modo de desenvolvedor está habilitado. Para saber mais sobre como habilitar o modo de desenvolvedor, confira [Habilitar o dispositivo para desenvolvimento](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> Às vezes, devido a problemas de rede ou de compatibilidade, o modo de desenvolvedor não será instalado corretamente em seu dispositivo. Confira a [seção pertinente de Habilitar o dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package) para obter ajuda com a solução desses problemas.

### <a name="turn-on-device-portal"></a>Ativar o Portal de Dispositivos

Você pode habilitar o Portal de Dispositivos na seção **Para desenvolvedores** das **Configurações**. Ao habilitá-lo, você também deverá criar um nome de usuário correspondente e uma senha. Não use sua conta da Microsoft ou outras credenciais do Windows. 

![A seção Portal de Dispositivos do aplicativo de configurações](images/device-portal/device-portal-desk-settings.png) 

Depois que o Portal de Dispositivos estiver habilitado, você verá links da Web na parte inferior da seção. Anote o número da porta acrescentado ao final das URLs listadas: esse número é gerado aleatoriamente quando o Portal de Dispositivos é habilitado, mas deve permanecer consistente entre reinicializações da área de trabalho. 

Esses links oferecem duas maneiras de se conectar ao Portal de Dispositivos: pela rede local (incluindo VPN) ou por meio do host local.

### <a name="connect-to-device-portal"></a>Conectar-se ao Portal de Dispositivos

Para se conectar por meio do host local, abra uma janela do navegador e insira o endereço mostrado aqui para o tipo de conexão que você está usando.

* Localhost: `http://127.0.0.1:<PORT>` ou `http://localhost:<PORT>`
* Rede local: `https://<IP address of the desktop>:<PORT>`

O HTTPS é necessário para autenticação e comunicação segura.

Se você estiver usando o Portal de Dispositivos em um ambiente protegido, como um laboratório de teste, onde confia em todos na rede local, não tem informações pessoais no dispositivo e tem requisitos exclusivos, você poderá desabilitar a opção de autenticação. Isso permite a comunicação não criptografada e que qualquer pessoa com o endereço IP do seu computador se conecte a ele e o controle.

## <a name="device-portal-content-on-windows-desktop"></a>Conteúdo do Portal de Dispositivos na área de trabalho do Windows

O Portal de Dispositivos na área de trabalho do Windows fornece o conjunto de páginas padrão. Para obter descrições detalhadas delas, confira [Visão geral do Portal de Dispositivos do Windows](device-portal.md).

- Gerente de aplicativos
- Explorador de Arquivos
- Processos em Execução
- Desempenho
- Depuração
- Rastreamento de Eventos para Windows (ETW)
- Rastreamento de desempenho
- Gerenciador de dispositivos
- Rede
- Dados de falha
- Recursos
- Realidade Misturada
- Depurador de instalação de streaming
- Local
- Rascunho

## <a name="more-device-portal-options"></a>Mais opções do Portal de Dispositivos

### <a name="registry-based-configuration-for-device-portal"></a>Configuração baseada no Registro para o Portal de Dispositivos

Se você quiser selecionar números de porta para o Device Portal (como 80 e 443), será possível definir as seguintes chaves do registro:

- Em `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: : um DWORD necessário. Defina como 0 para manter os números de porta que você escolheu.
    - `HttpPort`: : um DWORD necessário. Contém o número da porta na qual o Device Portal escutará conexões HTTP.    
    - `HttpsPort`: : um DWORD necessário. Contém o número da porta na qual o Device Portal escutará conexões HTTPS.
    
No mesmo caminho da chave do registro, você também pode desativar o requisito de autenticação:
- `UseDefaultAuthorizer` - `0` para desabilitado, `1` para habilitado.  
    - Isso controla o requisito de autenticação básica para cada conexão e o redirecionamento de HTTP para HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Opções de linha de comando para o Portal de Dispositivos
Em um prompt de comando administrativo, você pode habilitar e configurar partes do Portal de Dispositivos. Para ver o último conjunto de comandos compatível com a compilação, você pode executar `webmanagement /?`

- `sc start webmanagement` ou `sc stop webmanagement` 
    - Ativar ou desativar o serviço. Isso ainda requer que o modo de desenvolvedor esteja habilitado. 
- `-Credentials <username> <password>` 
    - Defina um nome de usuário e senha para o Portal de Dispositivos. O nome de usuário deve estar em conformidade com padrões de autenticação básicos, não podendo conter dois-pontos (:), e deve ser criado com base em caracteres ASCII padrão, por exemplo, [a-zA-Z0-9], pois os navegadores não analisam o conjunto de caracteres completo de uma forma padrão.  
- `-DeleteSSL` 
    - Isso redefine o cache de certificado SSL usado para conexões HTTPS. Se você encontrar erros de conexão de TLS que não podem ser ignorados (em vez do aviso de certificado esperado), essa opção poderá corrigir o problema para você. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Confira [Provisionar o Portal de Dispositivos com um certificado SSL personalizado](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl) para obter detalhes.  
    - Isso permite que você instale seu certificado SSL para corrigir a página de aviso SSL que normalmente é vista no Portal de Dispositivos. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Execute uma versão autônoma do Portal de Dispositivos com uma configuração específica e mensagens de depuração visível. Isso é mais útil para criar um [plug-in empacotado](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Confira o [artigo de revista do MSDN](https://msdn.microsoft.com/magazine/mt826332.aspx) para obter detalhes sobre como executar isso como sistema para testar completamente o plug-in empacotado.

## <a name="common-errors-and-issues"></a>Erros e problemas comuns

Veja abaixo alguns erros comuns que você pode encontrar ao configurar o Portal de Dispositivos.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbs_e_invalid_windows_update_count"></a>WindowsUpdateSearch retorna um número inválido de atualizações (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Você pode receber esse erro ao tentar instalar os pacotes de desenvolvedor em um build pré-lançamento do Windows 10. Esses pacotes de FoD (recurso sob demanda) são hospedados no Windows Update e baixá-los em builds pré-lançamento exige que você aceite a liberação de versões de pré-lançamento. Se a instalação não tiver aceitado a liberação de versões de pré-lançamento para a combinação correta de build e anel, não será possível baixar o conteúdo. Verifique o seguinte:

1. Navegue até **Configurações > Atualização & Segurança > Programa Windows Insider** e confirme se a seção **Conta do Windows Insider** tem suas informações de conta corretas. Se não vir essa seção, selecione **Vincular uma conta do Windows Insider**, adicione sua conta de email e confirme que ela aparece sob o título **Conta do Windows Insider** (talvez seja necessário selecionar **Vincular uma conta do Windows Insider** novamente para de fato vincular uma conta recém-adicionada).
 
2. Em **Que tipo de conteúdo você gostaria de receber?** , selecione **Desenvolvimento ativo do Windows**.
 
3. Em **Em que ritmo deseja receber novos builds?** , selecione **Windows Insider Rápido**.
 
4. Agora, você poderá instalar os FoDs. Se você confirmou que está no Windows Insider Rápido e ainda assim não conseguir instalar os FoDs, forneça comentários e anexe os arquivos de log em **C:\Windows\Logs\CBS**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: OpenService COM FALHA 1060: O serviço especificado não existe como um serviço instalado

Você poderá receber esse erro se os pacotes de desenvolvedor não estiverem instalados. Sem os pacotes de desenvolvedor, não há nenhum serviço de gerenciamento da Web. Tente instalar os pacotes de desenvolvedor novamente.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbs_e_metered_network"></a>O CBS não pode iniciar o download porque o sistema está em uma rede limitada (CBS_E_METERED_NETWORK)

Você poderá receber esse erro se estiver em uma conexão de Internet limitada. Não é possível baixar os pacotes de desenvolvedor com uma conexão limitada.

## <a name="see-also"></a>Veja também

* [Visão geral do Portal de Dispositivos do Windows](device-portal.md)
* [Referência de API central do Portal de Dispositivos](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
