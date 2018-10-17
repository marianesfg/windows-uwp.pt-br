---
author: c-don
title: Implantar um aplicativo por meio do registro de arquivo flexível
description: Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem a necessidade de empacotá-las.
ms.author: cdon
ms.date: 6/1/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, portal de dispositivos, gerente de aplicativos, implantação, sdk
ms.localizationpriority: medium
ms.openlocfilehash: a6a96a78cf03ce4994ddee1c929997b12a2d028f
ms.sourcegitcommit: 1c6325aa572868b789fcdd2efc9203f67a83872a
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/17/2018
ms.locfileid: "4745431"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implantar um aplicativo por meio do registro de arquivo flexível 

Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem a necessidade de empacotá-las. Registrar layouts de arquivo flexível permite aos desenvolvedores validar rapidamente seus aplicativos sem a necessidade de empacotar e instalar os aplicativos. 

## <a name="what-is-a-loose-file-layout"></a>O que é um layout de arquivo flexível?

Layout de arquivo flexível é simplesmente o ato de colocar o conteúdo do aplicativo em uma pasta em vez de passar pelo processo de empacotamento. O conteúdo do pacote é "livremente" disponível em uma pasta e não compactados. 

> [!WARNING]
> Registro de layout de arquivo flexível é para designers e desenvolvedores validar rapidamente seus aplicativos durante o desenvolvimento ativo. Essa abordagem não deve ser usado para "dogfood" ou o aplicativo de versão de pré-lançamento. Recomendamos que a validação final ser realizado em um aplicativo empacotado que é assinado com um certificado confiável. 

## <a name="advantages-of-loose-file-registration"></a>Vantagens de registro de arquivo flexível

- **Validação rápida** - porque os arquivos do aplicativo já são desempacotados, os usuários podem registrar o layout de arquivo flexível e iniciar o aplicativo rapidamente. Assim como um aplicativo normal, o usuário poderá usar o aplicativo que foi projetada. 
- **Facilitar a distribuição em rede** - se os arquivos soltos estão localizados em um compartilhamento de rede em vez de uma unidade local, os desenvolvedores podem enviar o local de compartilhamento de rede para outros usuários que tenham acesso à rede, e eles podem registrar o layout de arquivo flexível e executar o aplicativo. Isso permite que vários usuários validar o aplicativo simultaneamente. 
- **Colaboração** - registro de arquivo flexível permite que os desenvolvedores e designers continuar trabalhando com ativos visuais enquanto o aplicativo está registrado. Os usuários verão essas alterações quando ele iniciam o aplicativo. Observe que você pode alterar apenas ativos estáticos dessa maneira. Se você precisar modificar qualquer código ou conteúdo criado dinamicamente, você deve compilar novamente o aplicativo.

## <a name="how-to-register-a-loose-file-layout"></a>Como registrar um layout de arquivo flexível

O Windows fornece várias ferramentas de desenvolvedor para registrar layouts de arquivo flexível em dispositivos locais e remotos. Você pode escolher entre `WinDeployAppCmd` (ferramenta do SDK do Windows), Windows Device Portal, o PowerShell e o [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). A seguir, vamos examinar como registrar arquivos soltos usando essas ferramentas. Mas primeiro, certifique-se de que você tenha após a instalação:

- Seus dispositivos devem estar no Windows 10 Creators Update (compilação 14965) ou posterior.
- Você precisará habilitar o [modo de desenvolvedor](https://msdn.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) e [descoberta de dispositivo](https://docs.microsoft.com/en-us/windows/uwp/get-started/enable-your-device-for-development#device-discovery) em todos os dispositivos.

> [!IMPORTANT]
> Registro de arquivo flexível só está disponível em dispositivos que suportam o protocolo de compartilhamento de rede (SMB): Desktop e Xbox. 

### <a name="register-with-windeployappcmd"></a>Registre-se com WinDeployAppCmd

Se você estiver usando as ferramentas SDK correspondente para o Windows 10 para criadores (compilação 14965) ou posterior, você pode usar o `WinDeployAppCmd` comando em um Prompt de comando.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Caminho de rede** – o caminho para arquivos soltos do aplicativo.

**Endereço IP** – o endereço IP do computador de destino.

**computador de destino PIN** – um PIN, se necessário, para estabelecer uma conexão com o dispositivo de destino. Você será solicitado a tentar novamente com o `-pin` opção caso a autenticação seja necessária. Consulte a [Descoberta de dispositivo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para saber como obter um PIN.

### <a name="windows-device-portal"></a>Windows Device Portal

Windows Device Portal está disponível em todos os dispositivos Windows 10 e é usado pelos desenvolvedores para testar e validar seu trabalho. Ele atende a todos os públicos-alvo da comunidade de desenvolvedores com sua experiência do usuário do navegador e pontos de extremidade REST. Para obter mais informações sobre o Portal de dispositivos, consulte a [Visão geral do Windows Device Portal](device-portal.md).

Para registrar o layout de arquivo flexível no Portal de dispositivos, siga estas etapas.

1. Conecte ao Device Portal, seguindo as etapas na seção de **instalação** da [Visão geral do Windows Device Portal](device-portal.md).
1. Na guia Gerenciador de aplicativos, selecione **registrar de compartilhamento de rede**.
1. Insira o caminho do compartilhamento de rede para o layout de arquivo flexível. 
1. Se o dispositivo host não tem acesso ao compartilhamento de rede, haverá um prompt para inserir as credenciais necessárias.
1. Quando o registro for concluído, você pode iniciar o aplicativo.

Na página do Gerenciador de aplicativos do Device Portal, você também pode registrar layouts de arquivo flexível opcional do aplicativo principal, selecionando a caixa de seleção **que eu quiser especificar pacotes opcionais** e, em seguida, especificando os caminhos de compartilhamento de rede dos aplicativos opcionais. 

### <a name="powershell"></a>PowerShell 

O Windows PowerShell também permite que você registre layouts de arquivo flexível, mas somente no dispositivo local. Se você precisar registrar um layout para um dispositivo remoto, você precisará usar um dos outros métodos. 

Para registrar o layout de arquivo flexível, iniciar o PowerShell e digite o seguinte.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Solução de problemas

### <a name="mapped-network-drives"></a>Unidades de rede mapeadas
Atualmente, unidades de rede mapeadas não são suportadas para registros de arquivo flexível. Consulte a unidade mapeada com total o caminho do compartilhamento de rede.

### <a name="registration-failure"></a>Falha no registro
O dispositivo no qual está ocorrendo o registro precisarão ter acesso ao layout de arquivo. Se o layout do arquivo é hospedado em um compartilhamento de rede, certifique-se de que o dispositivo tem acesso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>Modificações ativos visuais não estão sendo carregadas no aplicativo 
O aplicativo será carregado seus ativos visuais no momento da inicialização. Se as modificações foram feitas em recursos visuais depois de iniciar o aplicativo, você deve iniciar novamente o aplicativo para ver as últimas alterações.