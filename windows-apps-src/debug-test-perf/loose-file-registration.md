---
title: Implantar um aplicativo por meio de registro de arquivo flexível
description: Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem necessidade de empacotá-los.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, portal do dispositivo, Gerenciador de aplicativos, implantação, SDK
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681927"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implantar um aplicativo por meio de registro de arquivo flexível 

Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem necessidade de empacotá-los. O registro de layouts de arquivo flexível permite aos desenvolvedores validar rapidamente seus aplicativos sem a necessidade de empacotar e instalar os aplicativos. 

## <a name="what-is-a-loose-file-layout"></a>O que é um layout de arquivo flexível?

O layout de arquivo flexível é simplesmente o ato de colocar o conteúdo do aplicativo em uma pasta em vez de passar pelo processo de empacotamento. O conteúdo do pacote está "livremente" disponível em uma pasta e não é empacotado. 

> [!WARNING]
> O registro de layout de arquivo flexível é para desenvolvedores e designers validar rapidamente seus aplicativos durante o desenvolvimento ativo. Essa abordagem não deve ser usada para "Dogfood" ou voo do aplicativo. Recomendamos que a validação final seja executada em um aplicativo empacotado assinado com um certificado confiável. 

## <a name="advantages-of-loose-file-registration"></a>Vantagens do registro de arquivo flexível

- **Validação rápida** -como os arquivos de aplicativo já estão desempacotados, os usuários podem registrar rapidamente o layout de arquivo flexível e iniciar o aplicativo. Assim como um aplicativo regular, o usuário poderá usar o aplicativo conforme ele foi projetado. 
- **Distribuição fácil na rede** – se os arquivos soltos estiverem localizados em um compartilhamento de rede em vez de em uma unidade local, os desenvolvedores poderão enviar o local de compartilhamento de rede para outros usuários que têm acesso à rede e podem registrar o layout de arquivo flexível e executar o aplicativo. Isso permite que vários usuários validem o aplicativo simultaneamente. 
- **Colaboração** – o registro de arquivo flexível permite que desenvolvedores e designers continuem a trabalhar em ativos visuais enquanto o aplicativo é registrado. Os usuários verão essas alterações quando iniciarem o aplicativo. Observe que você só pode alterar ativos estáticos dessa maneira. Se você precisar modificar qualquer código ou conteúdo criado dinamicamente, deverá recompilar o aplicativo.

## <a name="how-to-register-a-loose-file-layout"></a>Como registrar um layout de arquivo flexível

O Windows fornece várias ferramentas de desenvolvedor para registrar layouts de arquivo flexíveis em dispositivos locais e remotos. Você pode escolher entre `WinDeployAppCmd` (ferramenta de SDK do Windows), o portal de dispositivos do Windows, o PowerShell e o [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Abaixo, vamos falar sobre como registrar arquivos soltos usando essas ferramentas. Mas primeiro, verifique se você tem a seguinte configuração:

- Seus dispositivos devem estar na atualização do Windows 10 para criadores (Build 14965) ou posterior.
- Será necessário habilitar o [modo de desenvolvedor](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) e a [descoberta de dispositivo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) em todos os dispositivos.

> [!IMPORTANT]
> O registro de arquivo flexível só está disponível em dispositivos que dão suporte ao protocolo SMB (compartilhamento de rede): desktop e Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrar com WinDeployAppCmd

Se você estiver usando as ferramentas do SDK correspondentes à atualização do Windows 10 para criadores (Build 14965) ou posterior, poderá usar o comando `WinDeployAppCmd` em um prompt de comando.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Caminho de rede** – o caminho para os arquivos soltos do aplicativo.

**Endereço IP** – o endereço IP do computador de destino.

**PIN do computador de destino** – um PIN, se necessário, para estabelecer uma conexão com o dispositivo de destino. Você será solicitado a tentar novamente com a opção `-pin` se a autenticação for necessária. Consulte [descoberta de dispositivo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para saber como obter um PIN.

### <a name="windows-device-portal"></a>Windows Device Portal

O portal do dispositivo Windows está disponível em todos os dispositivos com Windows 10 e é usado por desenvolvedores para testar e validar seu trabalho. Ele atende a todos os públicos da comunidade de desenvolvedores com seus UX de navegador e pontos de extremidade REST. Para obter mais informações sobre o portal do dispositivo, consulte a [visão geral do portal de dispositivo do Windows](device-portal.md).

Para registrar o layout de arquivo flexível no portal do dispositivo, siga estas etapas.

1. Conecte-se ao portal do dispositivo seguindo as etapas na seção **instalação** da [visão geral do portal de dispositivo do Windows](device-portal.md).
1. Na guia Gerenciador de aplicativos, selecione **registrar do compartilhamento de rede**.
1. Insira o caminho de compartilhamento de rede para o layout de arquivo flexível. 
1. Se o dispositivo host não tiver acesso ao compartilhamento de rede, haverá um prompt para inserir as credenciais necessárias.
1. Quando o registro for concluído, você poderá iniciar o aplicativo.

Na página Gerenciador de aplicativos do portal do dispositivo, você também pode registrar layouts de arquivo flexíveis opcionais para seu aplicativo principal marcando a caixa de seleção **desejo especificar pacotes opcionais** e, em seguida, especificando os caminhos de compartilhamento de rede dos aplicativos opcionais. 

### <a name="powershell"></a>PowerShell 

O Windows PowerShell também permite que você registre layouts de arquivo flexíveis, mas apenas no dispositivo local. Se você precisar registrar um layout em um dispositivo remoto, será necessário usar um dos outros métodos. 

Para registrar o layout de arquivo flexível, inicie o PowerShell e insira o seguinte.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Painel de controle da

### <a name="mapped-network-drives"></a>Unidades de rede mapeadas
Atualmente, as unidades de rede mapeadas não têm suporte para registros de arquivos soltos. Consulte a unidade mapeada com o caminho completo do compartilhamento de rede.

### <a name="registration-failure"></a>Falha no registro
O dispositivo no qual o registro está ocorrendo precisará ter acesso ao layout do arquivo. Se o layout do arquivo estiver hospedado em um compartilhamento de rede, verifique se o dispositivo tem acesso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>As modificações nos ativos visuais não estão sendo carregadas no aplicativo 
O aplicativo carregará seus ativos visuais no momento da inicialização. Se foram feitas modificações nos ativos visuais depois de iniciar o aplicativo, você deverá reiniciar o aplicativo para exibir as alterações mais recentes.
