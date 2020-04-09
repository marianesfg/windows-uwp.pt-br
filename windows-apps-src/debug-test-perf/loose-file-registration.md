---
title: Implantar um aplicativo por meio de registro de arquivo flexível
description: Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem necessidade de empacotá-los.
ms.date: 06/01/2018
ms.topic: article
keywords: Windows 10, UWP, portal de dispositivos, gerenciador de aplicativos, implantação, SDK
ms.localizationpriority: medium
ms.openlocfilehash: 7bf3dab97be67a3b97aca4b3132bd9fe18691d15
ms.sourcegitcommit: 26bb75084b9d2d2b4a76d4aa131066e8da716679
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/06/2020
ms.locfileid: "75681927"
---
# <a name="deploy-an-app-through-loose-file-registration"></a>Implantar um aplicativo por meio de registro de arquivo flexível 

Este guia mostra como usar o layout de arquivo flexível para validar e compartilhar aplicativos do Windows 10 sem necessidade de empacotá-los. Registrar layouts de arquivo flexíveis permite aos desenvolvedores validar rapidamente seus aplicativos sem a necessidade de empacotar e instalá-los. 

## <a name="what-is-a-loose-file-layout"></a>O que é um layout de arquivo flexível?

O layout de arquivo flexível é simplesmente o ato de colocar o conteúdo do aplicativo em uma pasta em vez de passar pelo processo de empacotamento. O conteúdo do pacote está disponível de forma "flexível" em uma pasta, e não é empacotado. 

> [!WARNING]
> O registro de layout de arquivo flexível é para desenvolvedores e designers validarem rapidamente seus aplicativos durante o desenvolvimento ativo. Essa abordagem não deve ser usada para "dogfood" ou para a versão de pré-lançamento do aplicativo. Recomendamos que a validação final seja executada em um aplicativo empacotado assinado com um certificado confiável. 

## <a name="advantages-of-loose-file-registration"></a>Vantagens do registro de arquivo solto

- **Avaliação rápida** – Como os arquivos do aplicativo já estão desempacotadas, os usuários podem registrar rapidamente o layout de arquivo flexível e iniciar o aplicativo. Assim como acontece com um aplicativo regular, o usuário poderá usar o aplicativo conforme ele foi projetado. 
- **Distribuição fácil na rede** – Se os arquivos soltos estiverem localizados em um compartilhamento de rede em vez de em uma unidade local, os desenvolvedores poderão enviar o local de compartilhamento de rede para outros usuários que têm acesso à rede, e poderão registrar o arquivo de layout flexível, além de executar o aplicativo. Isso permite que vários usuários validem o aplicativo simultaneamente. 
- **Colaboração** – O registro de arquivo solto permite que desenvolvedores e designers continuem a trabalhar em ativos visuais enquanto o aplicativo é registrado. Os usuários verão essas alterações quando iniciarem o aplicativo. Observe que só é possível alterar ativos estáticos dessa maneira. Se for preciso modificar qualquer código ou conteúdo criado dinamicamente, você deverá recompilar o aplicativo.

## <a name="how-to-register-a-loose-file-layout"></a>Como registrar um layout de arquivo flexível

O Windows fornece várias ferramentas de desenvolvedor para registrar layouts de arquivo flexíveis em dispositivos locais e remotos. Você pode escolher entre `WinDeployAppCmd` (ferramenta de SDK do Windows), o Portal de Dispositivos do Windows, o PowerShell e o [Visual Studio](https://docs.microsoft.com/windows/uwp/debug-test-perf/deploying-and-debugging-uwp-apps#register-layout-from-network). Abaixo, vamos falar sobre como registrar arquivos soltos usando essas ferramentas. Mas primeiro, verifique se você tem a seguinte configuração:

- Seus dispositivos devem estar na Atualização do Windows 10 para Criadores (Build 14965) ou posterior.
- Será necessário habilitar o [modo desenvolvedor](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development) e a [detecção de dispositivo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) em todos os dispositivos.

> [!IMPORTANT]
> O registro de arquivo solto só está disponível em dispositivos que dão suporte ao protocolo SMB (compartilhamento de rede): Desktop e Xbox. 

### <a name="register-with-windeployappcmd"></a>Registrar com WinDeployAppCmd

Se você estiver usando as ferramentas do SDK correspondentes à atualização do Windows 10 para Criadores (Build 14965) ou posterior, poderá usar o comando `WinDeployAppCmd` em um prompt de comando.

```cmd
WinAppDeployCmd.exe registerfiles -remotedeploydir <Network Path> -ip <IP Address> -pin <target machine PIN>
```

**Caminho de Rede** – o caminho para os arquivos soltos do aplicativo.

**Endereço IP** – o Endereço IP do computador de destino.

**PIN do computador de destino** – Um PIN, se necessário, para estabelecer uma conexão com o dispositivo de destino. Você deverá tentar novamente usando a opção `-pin` caso a autenticação seja necessária. Confira [Descoberta de dispositivo](https://docs.microsoft.com/windows/uwp/get-started/enable-your-device-for-development#device-discovery) para saber como obter um PIN.

### <a name="windows-device-portal"></a>Portal de Dispositivos do Windows

O Portal de Dispositivos do Windows está disponível em todos os dispositivos com Windows 10, e é usado por desenvolvedores para testar e validar o trabalho deles. Ele atende a todos os públicos da comunidade de desenvolvedores com a experiência de usuário de navegador e pontos de extremidade REST. Para obter mais informações sobre o Portal de Dispositivos, confira a [visão geral do Portal de Dispositivos do Windows](device-portal.md).

Para registrar o layout de arquivo flexível no Portal de Dispositivos, siga estas etapas.

1. Conecte-se ao Portal de Dispositivos seguindo as etapas na seção **Instalação** da [visão geral do Portal de Dispositivos do Windows](device-portal.md).
1. Na guia Gerenciador de Aplicativos, selecione **Registrar-se de Compartilhamento de Rede**.
1. Insira o caminho de compartilhamento de rede para o layout de arquivo flexível. 
1. Se o dispositivo host não tiver acesso ao compartilhamento de rede, haverá um prompt para inserir as credenciais solicitadas.
1. Quando o registro for concluído, você poderá iniciar o aplicativo.

Na página Gerenciador de Aplicativos do Portal de Dispositivos, você também pode registrar layouts de arquivo flexíveis opcionais para seu aplicativo principal, selecionando a caixa de seleção **Desejo especificar pacotes opcionais** e, em seguida, especificando os caminhos de compartilhamento de rede dos aplicativos opcionais. 

### <a name="powershell"></a>PowerShell 

O Windows PowerShell também permite registrar layouts de arquivo flexíveis, mas somente no dispositivo local. Se você precisa registrar um layout em um dispositivo remoto, será preciso usar um dos outros métodos. 

Para registrar o layout de arquivo flexível, inicie o PowerShell e insira o seguinte.

```PowerShell
Add-AppxPackage -Register <path to manifest file>
```

## <a name="troubleshooting"></a>Solução de problemas

### <a name="mapped-network-drives"></a>Unidades de rede mapeadas
Atualmente, as unidades de rede mapeadas não têm suporte para registros de arquivos soltos. Confira a unidade mapeada com o caminho completo do compartilhamento de rede.

### <a name="registration-failure"></a>Falha no registro
O dispositivo no qual o registro está ocorrendo precisará ter acesso ao layout do arquivo. Se o layout do arquivo estiver hospedado em um compartilhamento de rede, verifique se o dispositivo tem acesso. 

### <a name="modifications-to-visual-assets-arent-being-loaded-in-the-app"></a>As modificações em ativos visuais não estão sendo carregadas no aplicativo 
O aplicativo carregará os ativos visuais no momento da inicialização. Se foram feitas modificações no ativos visuais após iniciar o aplicativo, você deverá reiniciar o aplicativo para exibir as alterações mais recentes.
