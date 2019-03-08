---
title: Implantar um aplicativo por meio de registro de arquivo flexível
description: Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem a necessidade de empacotá-las.
ms.date: 6/1/2018
ms.topic: article
keywords: Windows 10, uwp, portal do dispositivo, Gerenciador de aplicativos, implantação e sdk
ms.localizationpriority: medium
ms.openlocfilehash: 928c07bd23228f0fefd78be6019a0d116b2e6e4b
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57635421"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implantar um aplicativo por meio de registro de arquivo flexível 

Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem a necessidade de empacotá-las. Registrar os layouts de arquivo flexível permite aos desenvolvedores validar rapidamente seus aplicativos sem a necessidade de empacotar e instalar os aplicativos. 

## <a name="what-is-a-loose-file-layout"></a>O que é um layout de arquivo flexível?

Layout de arquivo flexível é simplesmente o ato de colocar o conteúdo do aplicativo em uma pasta em vez de percorrer o processo de empacotamento. O conteúdo do pacote é "fracamente" disponível em uma pasta e não compactados. 

> [!WARNING]
> Registro de layout de arquivo flexível é para desenvolvedores e designers validar rapidamente seus aplicativos durante o desenvolvimento ativo. Essa abordagem não deve ser usado para "ração para cachorros" ou flight o aplicativo. É recomendável que a validação final ser executada em um aplicativo empacotado que é assinado com um certificado confiável. 

## <a name="advantages-of-loose-file-registration"></a>Vantagens do registro do arquivo flexível

- **Validação rápida** -porque os arquivos de aplicativo já estão descompactados, os usuários podem registrar o layout de arquivo flexível rapidamente e iniciar o aplicativo. Assim como um aplicativo normal, o usuário será capaz de usar o aplicativo como ele foi criado. 
- **Facilitar a distribuição em rede** -se a arquivos flexíveis estão localizados em um compartilhamento de rede em vez de uma unidade local, os desenvolvedores podem enviar o local de compartilhamento de rede a outros usuários que têm acesso à rede e eles podem registrar o layout de arquivo flexível e executar o aplicativo. Isso permite que vários usuários validar o aplicativo simultaneamente. 
- **Colaboração** -registro de arquivo flexível permite que os desenvolvedores e designers continuar trabalhando com ativos visuais, enquanto o aplicativo é registrado. Os usuários verão essas alterações ao iniciar o aplicativo. Observe que você só pode alterar ativos estáticos dessa maneira. Se você precisar modificar qualquer código ou conteúdo criado dinamicamente, você deve compilar novamente o aplicativo.

## <a name="how-to-register-a-loose-file-layout"></a>Como registrar um layout de arquivo flexível

Windows fornece várias ferramentas de desenvolvedor para registrar os layouts de arquivo flexível em dispositivos locais e remotos. Você pode escolher entre `WinDeployAppCmd` (ferramenta do SDK do Windows), Windows Device Portal, PowerShell, e [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Abaixo, falaremos sobre como registrar arquivos flexíveis usando essas ferramentas. Mas primeiro, certifique-se de que você tenha após a instalação:

- Os dispositivos devem estar no Windows 10 Creators Update (Build 14965) ou posterior.
- Você precisará habilitar [modo de desenvolvedor](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) e [descoberta de dispositivo](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) em todos os dispositivos.

> [!IMPORTANT]
> Registro de arquivo flexível só está disponível em dispositivos que suportam o protocolo de compartilhamento de rede (SMB): Área de trabalho e Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrar com WinDeployAppCmd

Se você estiver usando as ferramentas do SDK correspondente para a atualização do Windows 10 para criadores (Build 14965) ou posterior, você pode usar o `WinDeployAppCmd` comando em um Prompt de comando.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Caminho de rede** – o caminho para os arquivos do aplicativo flexível.

**Endereço IP** – o endereço IP do computador de destino.

**o computador de destino PIN** – um PIN, se necessário, para estabelecer uma conexão com o dispositivo de destino. Você será solicitado para tentar novamente com o `-pin` opção se a autenticação é necessária. Ver [descoberta de dispositivo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para saber como obter um PIN.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal está disponível em todos os dispositivos Windows 10 e é usada por desenvolvedores para testar e validar seu trabalho. Ele atende a todos os públicos-alvo da comunidade de desenvolvedores com sua experiência do usuário do navegador e pontos de extremidade REST. Para obter mais informações sobre o Portal do dispositivo, consulte a [visão geral do Windows Device Portal](device-portal.md).

Para registrar o layout de arquivo flexível no Portal do dispositivo, siga estas etapas.

1. Conectar-se ao Portal do dispositivo seguindo as etapas na **instalação** seção o [visão geral do Windows Device Portal](device-portal.md).
1. Na guia Gerenciador de aplicativos, selecione **Registre-se de compartilhamento de rede**.
1. Insira o caminho de compartilhamento de rede para o layout de arquivo flexível. 
1. Se o dispositivo host não tem acesso ao compartilhamento de rede, haverá uma solicitação para inserir as credenciais necessárias.
1. Quando o registro for concluído, você pode iniciar o aplicativo.

Na página do Gerenciador de aplicativos de dispositivo do Portal, você também pode registrar os layouts de arquivo flexível opcional para seu aplicativo principal, selecionando o **desejo especificar pacotes opcionais** caixa de seleção e, em seguida, especificando a rede compartilham os caminhos dos aplicativos opcionais . 

### <a name="powershell"></a>PowerShell 

Windows PowerShell também permite que você registre layouts de arquivo flexível, mas apenas no dispositivo local. Se você precisar registrar um layout para um dispositivo remoto, você precisará usar um dos outros métodos. 

Para registrar o layout de arquivo flexível, inicie o PowerShell e digite o seguinte.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Solução de problemas

### <a name="mapped-network-drives"></a>Unidades de rede mapeadas
Atualmente, unidades de rede mapeadas não têm suporte para registros de arquivo flexível. Consulte a unidade mapeada por completo o caminho de compartilhamento de rede.

### <a name="registration-failure"></a>Falha no registro
O dispositivo no qual está ocorrendo o registro será necessário ter acesso ao layout do arquivo. Se o layout do arquivo é hospedado em um compartilhamento de rede, certifique-se de que o dispositivo tem acesso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Modificações em ativos visuais não estão sendo carregadas no aplicativo 
O aplicativo carregará os seus ativos visuais no momento da inicialização. Se foram feitas modificações para os ativos visuais após iniciar o aplicativo, você deve iniciar novamente o aplicativo para exibir as alterações mais recentes.