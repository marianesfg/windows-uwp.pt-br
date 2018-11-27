---
Description: Learn how your app's packages are made available to your customers, and how to manage specific package scenarios.
title: Diretrizes para gerenciamento do pacote do aplicativo
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0548ae9f9b3b33808cd7420eb542bcbac6a1a431
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7828851"
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

De modo geral, as versões posteriores do sistema operacional podem executar pacotes para versões anteriores do sistema operacional para a mesma família de dispositivos. Dispositivos do Windows 10 podem executar todas as versões anteriores suportadas do sistema operacional (por família de dispositivos). Dispositivos da área de trabalho do Windows 10 podem executar aplicativos criados para Windows 8.1 ou Windows8; Dispositivos móveis do Windows 10 podem executar aplicativos criados para Windows Phone 8.1, WindowsPhone8 e até mesmo Windows Phone 7. x. No entanto, os clientes no Windows 10 só obterão esses pacotes se o aplicativo não incluir pacotes UWP destinados à família aplicável.

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, produtos recém-criado não podem incluir pacotes que segmentem 8.x/Windows do Windows Phone 8. x ou anterior. Para obter mais informações, consulte esta [postagem de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/).


## <a name="removing-an-app-from-the-store"></a>Como remover um app da Store

Às vezes, você pode desejar cancelar a oferta de um aplicativo aos clientes efetivamente "cancelando sua publicação". Para fazer isso, clique em **Make app unavailable** na página **Visão geral do aplicativo**. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Microsoft Store, e os novos clientes não poderão obtê-lo (exceto com um [código promocional](generate-promotional-codes.md) e se usarem o dispositivo Windows 10).

> [!IMPORTANT]
> Essa opção substituirá qualquer configuração de [visibilidade](choose-visibility-options.md#discoverability) que você selecionou nos envios. 

Essa opção tem o mesmo efeito de como se você tivesse criado um envio e escolhido **Disponibilizar este produto mas não torná-lo detectável na Store** com a opção **Parar aquisição**. No entanto, isso não exige que você crie um novo envio.

Observe que todos os clientes que já têm o aplicativo ainda poderão usá-lo e baixá-lo novamente (e ainda poderão receber atualizações se você enviar novos pacotes mais tarde).

Depois de tornar o aplicativo indisponível, você ainda verá no Partner Center. Se optar por oferecer o aplicativo aos clientes novamente, você poderá clicar em **Tornar aplicativo disponível** na página Visão geral do aplicativo. Depois que você confirmar, o aplicativo estará disponível a novos clientes (a menos que esteja restrito pelas configurações no seu último envio) dentro de algumas horas.

> [!NOTE]
> Se você quer manter seu aplicativo disponível, mas não quer continuar oferecendo-o aos novos clientes em uma determinada versão de sistema operacional, crie um novo envio e remova todos os pacotes da versão do sistema operacional na qual deseja impedir novas aquisições. Por exemplo, se antes você tinha pacotes para Windows Phone 8.1 e Windows 10 e você não deseja continuar oferecendo o aplicativo para novos clientes no WindowsPhone8.1, remova todos os seus pacotes WindowsPhone8.1 do envio. Depois que a atualização for publicada, não há novos clientes no WindowsPhone8.1 será capazes de adquirir o aplicativo, embora os clientes que já o tenham possam continuar a usá-lo). No entanto, o aplicativo ainda estará disponível para novos clientes no Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Removendo pacotes para uma família de dispositivos com suporte anterior

Se você remover todos os pacotes para uma determinada [família de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) de que seu aplicativo anteriormente com suporte, você será solicitado a confirmar que isso é sua intenção para poder salvar suas alterações na página de **pacotes** .

Quando você publica um envio que remove todos os pacotes que podem ser executado em uma família de dispositivos que seu aplicativo suporte anteriormente, novos clientes não poderão adquirir o aplicativo nessa família. Você sempre pode publicar outra atualização posteriormente para fornecer pacotes para essa família de dispositivos novamente.

Lembre-se de que mesmo se você remover todos os pacotes que dão suporte a uma determinada família de dispositivos, quaisquer clientes existentes que já tiverem instalado o aplicativo nesse tipo de dispositivo ainda poderão usá-lo e receberão as atualizações que você fornecer mais tarde.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Adicionando pacotes para Windows 10 para um aplicativo publicado anteriormente

Se você tiver um aplicativo na loja que incluído apenas os pacotes para Windows 8. x e/ou Windows Phone 8. x e você desejar atualizar seu aplicativo para Windows 10, crie um novo envio e adicione seus pacotes de .msixupload ou. appxupload UWP durante a etapa de [pacotes](upload-app-packages.md) . Depois que seu aplicativo passa pelo processo de certificação, o pacote UWP também estará disponível para novas aquisições pelos clientes no Windows 10.

> [!NOTE]
> Depois que um cliente no Windows 10 obtiver seu pacote UWP, não é possível reverter esse cliente para usar um pacote de qualquer versão anterior do sistema operacional. 

Observe que o número de versão de seus pacotes do Windows 10 deve ser maior do que aqueles para pacotes qualquer Windows8, Windows 8.1 e/ou Windows Phone 8.1 que você usou. Para saber mais, veja [Numeração de versão do pacote](package-version-numbering.md).

Para obter mais informações sobre como empacotar aplicativos UWP para a Microsoft Store, consulte [Empacotando aplicativos](../packaging/index.md).
