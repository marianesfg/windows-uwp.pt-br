---
author: ridomin
title: Criar um arquivo do Instalador de Aplicativo com o Visual Studio
description: Saiba como usar o Visual Studio para permitir atualizações automáticas usando o arquivo .appinstaller.
ms.author: rmpablos
ms.date: 5/2/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, instalador do aplicativo, AppInstaller, sideload
ms.localizationpriority: medium
ms.openlocfilehash: 6158b804e1d4ece3c76099a3f8d33d5fa562078d
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4258539"
---
# <a name="create-an-app-installer-file-with-visual-studio"></a>Criar um arquivo do Instalador de Aplicativo com o Visual Studio

A partir do Windows 10, versão 1804, e do Visual Studio 2017, atualização 15.7, aplicativos de sideload podem ser configurados para receber atualizações automáticas usando um arquivo `.appinstaller`. O Visual Studio permite ativar essas atualizações.

## <a name="app-installer-file-location"></a>Local do arquivo do Instalador de Aplicativo
O arquivo `.appinstaller` pode ser hospedado em um local compartilhado como um ponto de extremidade HTTP ou uma pasta compartilhada UNC e inclui o caminho para encontrar os pacotes de aplicativos a serem instalados. Os usuários instalam o aplicativo a partir do local compartilhado e ativam verificações periódicas de novas atualizações. 


### <a name="configure-the-project-to-target-the-correct-windows-version"></a>Configurar o projeto para direcionar para a versão correta do Windows

Você pode configurar a propriedade `TargetPlatformMinVersion` quando cria o projeto ou alterá-la mais tarde nas propriedades do projeto. 

>[!IMPORTANT]
> O arquivo do instalador de aplicativo só é gerado quando o `TargetPlatformMinVersion` é Windows 10, versão 1804 ou posterior.


### <a name="create-packages"></a>Criar pacotes

Para distribuir um aplicativo por meio de sideload, você deve criar um pacote de aplicativo (.appx/.msix) ou lote de aplicativo (.appxbundle/.msixbundle) e publicá-lo em um local compartilhado.

Para fazer isso, use o assistente **Criar pacotes de aplicativo** no Visual Studio com as etapas a seguir.

1. Clique com botão direito no projeto e escolha **Store** -> **Criar pacotes de aplicativo**.  

![Menu de contexto com navegação para Criar Pacotes de Aplicativos](images/packaging-screen2.jpg)   

O assistente **Criar pacotes de aplicativo** aparecerá.

2. Selecione **Quero criar pacotes para sideload**. e **Ativar atualizações automáticas**  

![Janela da caixa de diálogo Crie Seus Pacotes mostrada](images/select-sideloading.png)  

**Ativar atualizações automáticas** só será ativado se o `TargetPlatformMinVersion` do projeto for definido como a versão correta do Windows 10.

3. A caixa de diálogo **Selecionar e configurar pacotes** permite selecionar as configurações de arquitetura compatíveis. Se você selecionar um pacote, um instalador único será gerado. No entanto, se você não quiser um pacote e preferir um pacote por arquitetura, também receberá um arquivo do instalador por arquitetura.  Se você não tiver certeza de quais arquiteturas escolher ou deseja saber mais sobre quais arquiteturas são usadas por vários dispositivos, consulte [Arquiteturas de pacote do aplicativo](device-architecture.md).

4. Configure todos os detalhes adicionais, como a numeração de versão ou o local de saída do pacote.

![Janela Criar Pacotes de Aplicativos com a configuração do pacote mostrada](images/packaging-screen5.jpg)  

5. Se você marcou **Ativar atualizações automáticas** na etapa 2, a caixa de diálogo **Definir configurações de atualização** será exibida. Aqui, você pode especificar o **URL de instalação** e a frequência das verificações de atualização.

![Janela Definir configurações de atualização com publicar a configuração de localização exibida](images/sideloading-screen.png)  

6. Quando seu aplicativo tiver sido empacotado, uma caixa de diálogo exibirá o local da pasta de saída que contém o pacote do aplicativo. A pasta de saída inclui todos os arquivos necessários para fazer o sideload do aplicativo, incluindo uma página HTML que pode ser usada para promover seu aplicativo.

### <a name="publish-packages"></a>Publicar pacotes

Para disponibilizar o aplicativo, os arquivos gerados devem ser publicados no local especificado:

#### <a name="publish-to-shared-folders-unc"></a>Publicar em pastas compartilhadas (UNC)

Se você deseja publicar seus pacotes em pastas compartilhadas UNC, configure a pasta de saída do pacote do aplicativo e o URL de instalação (veja a etapa 6 para obter detalhes) para o mesmo caminho. O assistente irá gerar os arquivos no local correto, e os usuários obterão o aplicativo e atualizações futuras no mesmo caminho.

#### <a name="publish-to-a-web-location-http"></a>Publicar em um local da Web (HTTP)

A publicação em um local da Web exige acesso para publicar conteúdo no servidor Web, garantindo que o URL final corresponda ao URL de instalação definido no assistente (veja a etapa 6 para obter detalhes). Normalmente, o protocolo de transferência de arquivos (FTP) ou protocolo de transferência de arquivos SSH (SFTP) é usado para carregar os arquivos, mas existem outros métodos de publicação como armazenamento MSDeploy, SSH ou Blob, dependendo do seu provedor de Internet.

Para configurar o servidor Web, você deve verificar os tipos MIME usados para os tipos de arquivo em uso. Este exemplo ilustra o `web.config` para Serviços de Informações da Internet (IIS):

```xml
<configuration>
  <system.webServer>
    <staticContent>
      <mimeMap fileExtension=".appx" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appxbundle" mimeType="application/vns.ms-appx" />
      <mimeMap fileExtension=".appinstaller" mimeType="application/xml" />
    </staticContent>  
  </system.webServer>  
</configuration>
```




















