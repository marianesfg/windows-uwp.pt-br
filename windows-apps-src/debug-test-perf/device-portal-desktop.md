---
author: mcleblanc
ms.assetid: 5c34c78e-9ff7-477b-87f6-a31367cd3f8b
title: Device Portal para desktop
description: Saiba como o Windows Device Portal abre diagnósticos e automação em sua área de trabalho do Windows.
---
# Device Portal para desktop

A partir do Windows 10, versão 1604, outros recursos de desenvolvedor estão disponíveis para desktop. Esses recursos estão disponíveis apenas quando o Modo de Desenvolvedor está habilitado.

Para saber mais sobre como habilitar o Modo de Desenvolvedor, consulte [Habilitar seu dispositivo para desenvolvimento](../get-started/enable-your-device-for-development.md).

O Device Portal permite exibir informações de diagnóstico e interagir com seu desktop por HTTP no seu navegador. Você pode usar o Device Portal para fazer o seguinte:
- Ver e manipular uma lista de processos em execução
- Instalar, excluir, iniciar e encerrar os aplicativos
- Alterar perfis de Wi-Fi, exibir a intensidade do sinal e ver ipconfig
- Exibir gráficos dinâmicos de uso de CPU, memória, E/S, rede e GPU
- Coletar despejos de memória do processo
- Coletar rastreamentos do ETW 
- Manipular o armazenamento isolado dos aplicativos de sideload

## Configurar o Device Portal na área de trabalho do Windows

### Ativar o portal de dispositivo

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

## Páginas do Device Portal

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

## Configurando números de porta

Se você quiser selecionar números de porta para o Device Portal (como 80 e 443), será possível definir as seguintes chaves do registro:

- Em HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\Windows\CurrentVersion\WebManagement\Service
    - UseDynamicPorts: um DWORD necessário. Defina como 0 para manter os números de porta que você escolheu.
    - HttpPort: um DWORD necessário. Contém o número da porta na qual o Device Portal escutará conexões HTTP.  
    - HttpsPort: um DWORD necessário. Contém o número da porta na qual o Device Portal escutará conexões HTTPS.

## Falha ao instalar o pacote do Modo de Desenvolvedor
Às vezes, devido a problemas de rede ou de compatibilidade, o Modo de Desenvolvedor não será instalado corretamente. O pacote do Modo de Desenvolvedor é necessário para a implantação remota – Device Portal e SSH – mas não para o desenvolvimento local.  

### Falha ao localizar o pacote

"Não foi possível localizar o pacote do Modo de Desenvolvedor no Windows Update. Código do erro 0x001234 Saiba mais"   

Esse erro pode ocorrer devido a um problema de conectividade de rede, configurações corporativas ou o pacote pode estar faltando. 

Para resolver este problema:

1. Certifique-se de que o computador esteja conectado à Internet. 
2. Se você estiver em um computador associado a um domínio, fale com o administrador de rede. 
3. Verifique se existem atualizações do Windows em Configurações > Atualizações e Segurança > [Atualizações do Windows](ms-settings:windowsupdate).
4. Verifique se o pacote do Modo de Desenvolvedor do Windows está presente em Configurações > Sistema > Aplicativos e Recursos > [Gerenciar recursos opcionais](ms-settings:optionalfeatures) > Adicionar um recurso. Se ele estiver ausente, o Windows não poderá encontrar o pacote correto para o seu computador. 

Depois de realizar qualquer uma das etapas acima, desabilite e habilite novamente o Modo de Desenvolvedor para verificar a correção. 


### Falha ao instalar o pacote

"Falha ao instalar o pacote do Modo de Desenvolvedor. Código do erro 0x001234 Saiba mais"

Esse erro pode ocorrer devido à incompatibilidade entre a compilação do Windows e o pacote do Modo de Desenvolvedor. 

Para resolver este problema:

1. Verifique se existem atualizações do Windows em Configurações > Atualizações e Segurança > [Atualizações do Windows](ms-settings:windowsupdate).
2. Reinicialize o computador para garantir que todas as atualizações sejam aplicadas.


<!--HONumber=May16_HO2-->


