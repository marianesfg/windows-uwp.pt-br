---
author: jnHs
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: Diretrizes para gerenciamento do pacote do aplicativo
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.author: wdg-dev-content
ms.date: 10/02/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: a43f3b4c5684d93ea6986c4d1f1e4dae46c1a959
ms.sourcegitcommit: 4b97117d3aff38db89d560502a3c372f12bb6ed5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/23/2018
ms.locfileid: "5436144"
---
# <a name="guidance-for-app-package-management"></a>Orientação para gerenciamento do pacote de aplicativo

Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.

-   [Versões do sistema operacional e distribuição de pacote](#os-versions-and-package-distribution)
-   [Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Mantendo a compatibilidade de pacote para o Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Removendo um aplicativo da Loja](#removing-an-app-from-the-store)
-   [Removendo pacotes para uma família de dispositivos com suporte anterior](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versões do sistema operacional e distribuição de pacote

Diferentes sistemas operacionais podem executar diferentes tipos de pacotes. Se mais de um dos seus pacotes puder ser executado no dispositivo do cliente, a Microsoft Store fornecerá a melhor correspondência disponível.

De modo geral, as versões posteriores do sistema operacional podem executar pacotes para versões anteriores do sistema operacional para a mesma família de dispositivos. No entanto, os clientes só obterão esses pacotes se o aplicativo não incluir um pacote direcionado a sua versão do sistema operacional atual.

Por exemplo, os dispositivos Windows 10 podem executar todas as versões anteriores suportadas do sistema operacional (por família de dispositivos). Os dispositivos de desktop Windows 10 podem executar aplicativos criados para o Windows 8.1 ou Windows 8; os dispositivos móveis Windows 10 podem executar aplicativos criados para Windows Phone 8.1, Windows Phone 8 e até mesmo Windows Phone 7.x. 

Os exemplos a seguir ilustram diversos cenários para um aplicativo que inclui pacotes para diferentes versões do sistema operacional (exceto quando restrições específicas dos pacotes não permitem a execução em cada versão do sistema operacional/tipo de dispositivo listado aqui; por exemplo, a arquitetura do pacote deve ser adequada ao dispositivo). 

### <a name="example-app-1"></a>Aplicativo de exemplo 1

| Sistema operacional específico do pacote | Sistemas operacionais que receberão esse pacote |
|-------------------------------------|----------------------------------------------|
| Windows 8.1                         | Dispositivos de desktop Windows 10, Windows 8.1      |
| Windows Phone 8.1                   | Dispositivos móveis Windows 10, Windows Phone 8.1 |
| Windows Phone 8                     | Windows Phone 8                              |
| Windows Phone 7.1                   | Windows Phone 7.x                            |

No exemplo de aplicativo 1, o aplicativo ainda não possui pacotes da UWP (Plataforma Universal do Windows) criados especificamente para dispositivos Windows 10, mas os clientes do Windows 10 ainda podem obter o aplicativo. Esses clientes obtêm os melhores pacotes disponíveis para seu tipo de dispositivo.

### <a name="example-app-2"></a>Aplicativo de exemplo 2

| Sistema operacional específico do pacote  | Sistemas operacionais que receberão esse pacote |
|--------------------------------------|----------------------------------------------|
| Windows 10 (família de dispositivos universal) | Windows 10 (todas as famílias de dispositivos)             |
| Windows 8.1                          | Windows 8.1                                  |
| Windows Phone 8.1                    | Windows Phone 8.1                            |
| Windows Phone 7.1                    | Windows Phone 7.x, Windows Phone 8           |

No exemplo de aplicativo 2, não há nenhum pacote que possa ser executado no Windows 8. Os clientes que executam qualquer outra versão do sistema operacional podem obter o aplicativo. Todos os clientes no Windows 10 obtêm o mesmo pacote.

### <a name="example-app-3"></a>Aplicativo de exemplo 3

| Sistema operacional específico do pacote | Sistemas operacionais que receberão esse pacote                  |
|-------------------------------------|---------------------------------------------------------------|
| Windows 10 (família de dispositivos de desktop)  | Dispositivos de desktop Windows 10                                    |
| Windows Phone 8                     | Dispositivos móveis Windows 10, Windows Phone 8, Windows Phone 8.1 |

No exemplo de aplicativo 3, como não há nenhum pacote UWP destinado à família de dispositivos móveis, os clientes de dispositivos móveis Windows 10 obterão o pacote do Windows Phone 8. Se, mais tarde, esse aplicativo adicionar um pacote direcionado à família de dispositivos móveis (ou à família de dispositivos universal), esse pacote estará disponível para os clientes em dispositivos móveis Windows 10 em vez do pacote do Windows Phone 8.

Observe também que este exemplo de aplicativo não inclui nenhum pacote que pode ser executado no Windows Phone 7.x.

### <a name="example-app-4"></a>Aplicativo de exemplo 4

| Sistema operacional específico do pacote  | Sistemas operacionais que receberão esse pacote |
|--------------------------------------|----------------------------------------------|
| Windows 10 (família de dispositivos universal) | Windows 10 (todas as famílias de dispositivos)             |

No exemplo de aplicativo 4, qualquer dispositivo que execute o Windows 10 pode obter o aplicativo, mas não estará disponível para clientes com uma versão anterior do sistema operacional. Como o pacote UWP é direcionado a família de dispositivos universal, ele estará disponível para qualquer dispositivo Windows 10 (de acordo com suas [seleções de disponibilidade da família de dispositivo](device-family-availability.md)).


## <a name="removing-an-app-from-the-store"></a>Como remover um app da Store

Às vezes, você pode desejar cancelar a oferta de um aplicativo aos clientes efetivamente "cancelando sua publicação". Para fazer isso, clique em **Make app unavailable** na página **Visão geral do aplicativo**. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Microsoft Store, e os novos clientes não poderão obtê-lo (exceto com um [código promocional](generate-promotional-codes.md) e se usarem o dispositivo Windows 10).

> [!IMPORTANT]
> Essa opção substituirá qualquer configuração de [visibilidade](choose-visibility-options.md#discoverability) que você selecionou nos envios. 

Essa opção tem o mesmo efeito de como se você tivesse criado um envio e escolhido **Disponibilizar este produto mas não torná-lo detectável na Store** com a opção **Parar aquisição**. No entanto, isso não exige que você crie um novo envio.

Observe que todos os clientes que já têm o aplicativo ainda poderão usá-lo e baixá-lo novamente (e ainda poderão receber atualizações se você enviar novos pacotes mais tarde).

Depois de tornar o aplicativo indisponível, você continuará a vê-lo em seu painel. Se optar por oferecer o aplicativo aos clientes novamente, você poderá clicar em **Tornar aplicativo disponível** na página Visão geral do aplicativo. Depois que você confirmar, o aplicativo estará disponível a novos clientes (a menos que esteja restrito pelas configurações no seu último envio) dentro de algumas horas.

> [!NOTE]
> Se você quer manter seu aplicativo disponível, mas não quer continuar oferecendo-o aos novos clientes em uma determinada versão de sistema operacional, crie um novo envio e remova todos os pacotes da versão do sistema operacional na qual deseja impedir novas aquisições. Por exemplo, se antes você tinha pacotes para Windows Phone 8 e Windows 10, e agora não deseja continuar oferecendo o aplicativo para novos clientes no Windows Phone 8.1, remova todos os pacotes do Windows Phone 8.1 do envio. Depois que a atualização for publicada, os novos clientes do Windows Phone 8.1 não poderão adquirir o aplicativo (embora os clientes que já o tenham possam continuar utilizando-o). Entretanto, o aplicativo ainda estará disponível para novos clientes no Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Removendo pacotes para uma família de dispositivos com suporte anterior

Se você remover todos os pacotes para uma determinada [família de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) de que seu aplicativo suportado anteriormente, você será solicitado a confirmar que isso é sua intenção para poder salvar suas alterações na página de **pacotes** .

Quando você publica um envio que remove todos os pacotes que podem ser executado em uma família de dispositivos que seu aplicativo suporte anteriormente, novos clientes não poderão adquirir o aplicativo nessa família. Você sempre pode publicar outra atualização posteriormente para fornecer pacotes para essa família de dispositivos novamente.

Lembre-se de que mesmo se você remover todos os pacotes que dão suporte a uma determinada família de dispositivos, quaisquer clientes existentes que já tiverem instalado o aplicativo nesse tipo de dispositivo ainda poderão usá-lo e receberão as atualizações que você fornecer mais tarde.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente

Se você tiver um aplicativo na loja que incluído apenas os pacotes para Windows 8. x e/ou Windows Phone 8. x e você desejar atualizar seu aplicativo para Windows 10, crie um novo envio e adicione seus pacotes de .msixupload ou. appxupload UWP durante a etapa de [pacotes](upload-app-packages.md) . Depois que seu aplicativo passa pelo processo de certificação, o pacote UWP também estará disponível para novas aquisições pelos clientes no Windows 10.

> [!NOTE]
> Depois que um cliente no Windows 10 obtiver o pacote UWP, não é possível reverter esse cliente para usar um pacote de qualquer versão anterior do sistema operacional. 

Observe que o número de pacotes do Windows 10 versão deve ser maior do que aqueles para pacotes qualquer Windows 8, Windows 8.1 e/ou Windows Phone 8.1 que você usou. Para saber mais, veja [Numeração de versão do pacote](package-version-numbering.md).

Para obter mais informações sobre como empacotar aplicativos UWP para a Microsoft Store, consulte [Empacotando aplicativos](../packaging/index.md).
