---
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Portal de Dispositivos para desktop Windows
description: Saiba como o Windows Device Portal abre diagnósticos e automação em sua área de trabalho do Windows.
ms.date: 02/6/2019
ms.topic: article
keywords: Windows 10, uwp, portal de dispositivos
ms.localizationpriority: medium
ms.openlocfilehash: c499c14d98f63836d80134aac70b2c80902f049b
ms.sourcegitcommit: 079801609165bc7eb69670d771a05bffe236d483
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/27/2019
ms.locfileid: "9116128"
---
# <a name="device-portal-for-windows-desktop"></a>Portal de Dispositivos para desktop Windows

O Portal de Dispositivos do Windows permite exibir informações de diagnóstico e interagir com seu desktop por HTTP em uma janela de seu navegador. Você pode usar o Portal de Dispositivos para fazer o seguinte:
- Ver e manipular uma lista de processos em execução
- Instalar, excluir, iniciar e encerrar os aplicativos
- Alterar perfis de Wi-Fi, exibir a intensidade do sinal e ver ipconfig
- Exibir gráficos dinâmicos de uso de CPU, memória, E/S, rede e GPU
- Coletar despejos de memória do processo
- Coletar rastreamentos do ETW 
- Manipular o armazenamento isolado dos aplicativos de sideload

## <a name="set-up-device-portal-on-windows-desktop"></a>Configurar o Portal de Dispositivos na área de trabalho do Windows

### <a name="turn-on-developer-mode"></a>Ativar o modo de desenvolvedor

A partir do Windows 10, versão 1607, alguns dos recursos mais recentes para desktop só estão disponíveis quando o modo de desenvolvedor está habilitado. Para saber mais sobre como habilitar o modo de desenvolvedor, consulte [Habilitar seu dispositivo para desenvolvimento](../get-started/enable-your-device-for-development.md).

> [!IMPORTANT]
> Às vezes, devido a problemas de rede ou de compatibilidade, o modo de desenvolvedor não será instalado corretamente em seu dispositivo. Consulte a [seção pertinente de Habilitar seu dispositivo para desenvolvimento](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#failure-to-install-developer-mode-package) para obter ajuda na solução desses problemas.

### <a name="turn-on-device-portal"></a>Ativar o Portal de Dispositivos

Você pode habilitar o Portal de Dispositivos na seção **Para desenvolvedores** das **Configurações**. Ao habilitá-lo, você também deverá criar um nome de usuário correspondente e uma senha. Não use sua conta da Microsoft ou outras credenciais do Windows. 

![A seção Portal de Dispositivos do aplicativo de configurações](images/device-portal/device-portal-desk-settings.png) 

Depois que o Portal de Dispositivos estiver habilitado, você verá links da web na parte inferior da seção. Anote o número da porta acrescentado ao final das URLs listadas: esse número é gerado aleatoriamente quando o Portal de Dispositivos é habilitado, mas deve permanecer consistente entre reinicializações da área de trabalho. Se você gostaria de definir os números de portas manualmente para que eles sejam permanentes, consulte [Definindo números de porta](device-portal-desktop.md#setting-port-numbers).

Esses links oferecem duas maneiras de se conectar ao Portal de Dispositivos: pela rede local (incluindo VPN) ou por meio do host local.

### <a name="connect-to-device-portal"></a>Conectar-se ao Portal de Dispositivos

Para se conectar por meio do host local, abra uma janela do navegador e insira o endereço mostrado aqui para o tipo de conexão que você está usando.

* Localhost: `http://127.0.0.1:<PORT>` ou `http://localhost:<PORT>`
* Rede local: `https://<IP address of the desktop>:<PORT>`

O HTTPS é necessário para autenticação e comunicação segura.

Se você estiver usando o Portal de Dispositivos em um ambiente protegido, como um laboratório de teste, onde confia em todos na rede local, não tem informações pessoais no dispositivo e tem requisitos exclusivos, você poderá desabilitar a opção de autenticação. Isso permite a comunicação não criptografada e que qualquer pessoa com o endereço IP do seu computador se conecte a ele e o controle.

## <a name="device-portal-content-on-windows-desktop"></a>Conteúdo do Portal de Dispositivos na área de trabalho do Windows

O Portal de Dispositivos na área de trabalho do Windows fornece o conjunto padrão de páginas. Para obter descrições detalhadas delas, consulte [Visão geral do Portal de Dispositivos do Windows](device-portal.md).

- Gerente de aplicativos
- Explorador de Arquivos
- Processos em execução
- Desempenho
- Depuração
- Rastreamento de Eventos para Windows (ETW)
- Rastreamento de desempenho
- Gerenciador de dispositivos
- Rede
- Dados de pane
- Recursos
- Realidade Misturada
- Depurador de instalação de streaming
- Local
- Rascunho

## <a name="more-device-portal-options"></a>Mais opções do Portal de Dispositivos

### <a name="registry-based-configuration-for-device-portal"></a>Configuração baseada no registro para o Portal de Dispositivos

Se você quiser selecionar números de porta para o Portal de Dispositivos (como 80 e 443), será possível definir as seguintes chaves do registro:

- Na opção `HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service`
    - `UseDynamicPorts`: A um DWORD necessário. Defina como 0 para manter os números de porta que você escolheu.
    - `HttpPort`: A um DWORD necessário. Contém o número da porta na qual o Portal de Dispositivos escutará conexões HTTP.    
    - `HttpsPort`: A um DWORD necessário. Contém o número da porta na qual o Portal de Dispositivos escutará conexões HTTPS.
    
No mesmo caminho da chave do registro, você também pode desativar o requisito de autenticação:
- `UseDefaultAuthorizer` - `0` para desabilitado, `1` para habilitado.  
    - Isso controla o requisito de autenticação básica para cada conexão e o redirecionamento de HTTP para HTTPS.  
    
### <a name="command-line-options-for-device-portal"></a>Opções de linha de comando para o Portal de Dispositivos
Em um prompt de comando administrativo, você pode ativar e configurar partes do Portal de Dispositivos. Para ver o último conjunto de comandos compatível com a compilação, você pode executar `webmanagement /?`

- `sc start webmanagement` ou `sc stop webmanagement` 
    - Ativar ou desativar o serviço. Isso ainda requer que o modo de desenvolvedor esteja habilitado. 
- `-Credentials <username> <password>` 
    - Defina um nome de usuário e senha para o Portal de Dispositivos. O nome de usuário deve estar em conformidade com padrões de autenticação básicos, não podendo conter dois pontos (:), e deve ser criado a partir de caracteres ASCII padrão, por exemplo, [a-zA-Z0-9], pois os navegadores não analisam o conjunto de caracteres completo de uma maneira padrão.  
- `-DeleteSSL` 
    - Isso redefine o cache de certificado SSL usado para conexões HTTPS. Se você encontrar erros de conexão de TLS que não podem ser ignorados (em vez do aviso de certificado esperado), essa opção pode corrigir o problema para você. 
- `-SetCert <pfxPath> <pfxPassword>`
    - Consulte [Provisionar o Portal de Dispositivos com um certificado SSL personalizado](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-ssl) para obter detalhes.  
    - Isso permite que você instale seu próprio certificado SSL para corrigir a página de aviso SSL que normalmente é vista no Portal de Dispositivos. 
- `-Debug <various options for authentication, port selection, and tracing level>`
    - Execute uma versão autônoma do Portal de Dispositivos com uma configuração específica e mensagens de depuração visível. Isso é mais útil para criar um [plug-in empacotado](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-plugin). 
    - Consulte o [artigo de revista do MSDN](https://msdn.microsoft.com/en-us/magazine/mt826332.aspx) para obter detalhes sobre como executar isso como sistema para testar completamente o plug-in empacotado.

## <a name="common-errors-and-issues"></a>Erros e problemas comuns

Abaixo estão alguns erros comuns que você pode encontrar ao configurar o Portal de dispositivos.

### <a name="windowsupdatesearch-returns-invalid-number-of-updates-0x800f0950-cbseinvalidwindowsupdatecount"></a>WindowsUpdateSearch retorna o número de atualizações inválido (0x800f0950 CBS_E_INVALID_WINDOWS_UPDATE_COUNT)

Você pode obter esse erro ao tentar instalar os pacotes de desenvolvedor em uma compilação de pré-lançamento do Windows 10. Esses pacotes de recursos sob demanda (FoD) são hospedados no Windows Update e baixá-las em compilações de pré-lançamento requer que você escolhe de pré-lançamento. Se não tiver optado pela sua instalação em pré-lançamento para a combinação de toque e compilação certa, a carga não estará disponível para download. Verificar o seguinte:

1. Navegue até **Configurações gt _ atualização & segurança gt _ programa Windows Insider** e confirme se a seção **Windows Insider conta** tem suas informações de conta correto. Se você não vir essa seção, selecione **vincular uma conta do Windows Insider**, adicione sua conta de email e confirmar se ele é exibida sob o título de **conta do Windows Insider** (talvez seja necessário selecionar **vincular uma conta do Windows Insider** uma segunda vez para Na verdade, vincule uma conta do recém-adicionado).
 
2. Sob **que tipo de conteúdo você gostaria de receber?**, certifique-se de **desenvolvimento ativo do Windows** é selecionado.
 
3. Sob **qual ritmo você deseja obter novas compilações?**, verifique se **Windows Insider-modo rápido** está selecionado.
 
4. Agora você deve ser capaz de instalar os FoDs. Se você tiver confirmado que você está no Windows Insider rápido e ainda não pode instalar os FoDs, forneça comentários e anexar arquivos de log em **c:\Windows\Logs\CBS.**.

### <a name="sc-startservice-openservice-failed-1060-the-specified-service-does-not-exist-as-an-installed-service"></a>[SC] StartService: OpenService falha 1060: O serviço especificado não existe como um serviço instalado

Você pode obter esse erro se os pacotes de desenvolvedor não são instalados. Sem os pacotes de desenvolvedor, não há nenhum serviço de gerenciamento da web. Tente instalar os pacotes de desenvolvedor novamente.

### <a name="cbs-cannot-start-download-because-the-system-is-on-metered-network-cbsemeterednetwork"></a>CBS não pode iniciar o download porque o sistema está em rede limitada (CBS_E_METERED_NETWORK)

Se você estiver em uma conexão de internet limitada, você pode obter esse erro. Você não conseguirá baixar os pacotes de desenvolvedor em uma conexão limitada.

## <a name="see-also"></a>Veja também

* [Visão geral do Portal de Dispositivos do Windows](device-portal.md)
* [Referência de API central do Portal de Dispositivos](https://docs.microsoft.com/windows/uwp/debug-test-perf/device-portal-api-core)
