---
Description: Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.
title: Orientação para gerenciamento do pacote de aplicativo
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: f514177ad5de7774e6926165435fd3b2d7b5e1f7
ms.sourcegitcommit: 4aef8c01ba9321401d5729a1ec6d46452ee76faf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/29/2019
ms.locfileid: "67468945"
---
# <a name="guidance-for-app-package-management"></a>Orientação para gerenciamento do pacote de aplicativo

Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.

-   [Versões do sistema operacional e a distribuição de pacote](#os-versions-and-package-distribution)
-   [Adicionando pacotes para o Windows 10 para um aplicativo publicado anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Manter a compatibilidade de pacote para o Windows Phone 8.1](#maintaining-package-compatibility-for-windows-phone-81)
-   [Removendo um aplicativo da Store](#removing-an-app-from-the-store)
-   [Removendo pacotes para uma família de dispositivos com suporte anteriormente](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versões do sistema operacional e distribuição de pacote

Diferentes sistemas operacionais podem executar diferentes tipos de pacotes. Se mais de um dos seus pacotes puder ser executado no dispositivo do cliente, a Microsoft Store fornecerá a melhor correspondência disponível.

De modo geral, as versões posteriores do sistema operacional podem executar pacotes para versões anteriores do sistema operacional para a mesma família de dispositivos. Dispositivos Windows 10 podem executar todas as versões do sistema operacional de com suporte anteriores (por família de dispositivos). Dispositivos de área de trabalho do Windows 10 podem executar aplicativos que foram criados para Windows 8.1 ou Windows 8; Dispositivos móveis Windows 10 podem executar aplicativos que foram criados para Windows Phone 8.1, Windows Phone 8 e até mesmo do Windows Phone 7. x. No entanto, os clientes no Windows 10 só receberá esses pacotes se o aplicativo não incluir pacotes UWP visando a família do dispositivo aplicáveis.

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, produtos recém-criado não podem incluir os pacotes direcionados a 8.x/Windows do Windows Phone 8.x ou anterior. Para obter mais informações, consulte este [postagem de blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Removendo um aplicativo da Loja

Às vezes, você pode desejar cancelar a oferta de um aplicativo aos clientes efetivamente "cancelando sua publicação". Para fazer isso, clique em **Make app unavailable** na página **Visão geral do aplicativo**. Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Microsoft Store, e os novos clientes não poderão obtê-lo (exceto com um [código promocional](generate-promotional-codes.md) e se usarem o dispositivo Windows 10).

> [!IMPORTANT]
> Essa opção substituirá qualquer configuração de [visibilidade](choose-visibility-options.md#discoverability) que você selecionou nos envios. 

Essa opção tem o mesmo efeito de como se você tivesse criado um envio e escolhido **Disponibilizar este produto mas não torná-lo detectável na Store** com a opção **Parar aquisição**. No entanto, isso não exige que você crie um novo envio.

Observe que todos os clientes que já têm o aplicativo ainda poderão usá-lo e baixá-lo novamente (e ainda poderão receber atualizações se você enviar novos pacotes mais tarde).

Depois de fazer com que o aplicativo não está disponível, você ainda verá ele no Partner Center. Se optar por oferecer o aplicativo aos clientes novamente, você poderá clicar em **Tornar aplicativo disponível** na página Visão geral do aplicativo. Depois que você confirmar, o aplicativo estará disponível a novos clientes (a menos que esteja restrito pelas configurações no seu último envio) dentro de algumas horas.

> [!NOTE]
> Se você quer manter seu aplicativo disponível, mas não quer continuar oferecendo-o aos novos clientes em uma determinada versão de sistema operacional, crie um novo envio e remova todos os pacotes da versão do sistema operacional na qual deseja impedir novas aquisições. Por exemplo, se você já tinha pacotes para o Windows Phone 8.1 e Windows 10, e você não deseja manter oferece o aplicativo para novos clientes no Windows Phone 8.1, remova todos os pacotes do Windows Phone 8.1 do envio. Depois que a atualização for publicada, não há novos clientes no Windows Phone 8.1 será capazes de adquirir o aplicativo, embora os clientes que já o tenha podem continuar a usá-lo). No entanto, o aplicativo ainda estará disponível para novos clientes no Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Removendo pacotes para uma família de dispositivos com suporte anterior

Se você remover todos os pacotes para um determinado [família do dispositivo](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) que seu aplicativo anteriormente com suporte, você será solicitado a confirmar que esta é sua intenção antes de salvar suas alterações na **pacotes** página.

Quando você publica um envio que remove todos os pacotes que possa ser executada em uma família de dispositivo que seu aplicativo com suporte anteriormente, novos clientes não poderão adquirir o aplicativo em dessa família de dispositivo. Você sempre pode publicar outra atualização posteriormente para fornecer pacotes para essa família de dispositivos novamente.

Lembre-se de que mesmo se você remover todos os pacotes que dão suporte a uma determinada família de dispositivos, quaisquer clientes existentes que já tiverem instalado o aplicativo nesse tipo de dispositivo ainda poderão usá-lo e receberão as atualizações que você fornecer mais tarde.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Adicionando pacotes para o Windows 10 para um aplicativo publicado anteriormente

Se você tiver um aplicativo no que incluía somente pacotes para a Windows Store 8.x e/ou Windows Phone 8. x e você quiser atualizar seu aplicativo para Windows 10, crie um novo envio e adicionar o UWP .msixupload ou appxupload pacote (s) durante o [pacotes](upload-app-packages.md) etapa. Depois que seu aplicativo passa pelo processo de certificação, o pacote UWP também estará disponível para novas aquisições de clientes no Windows 10.

> [!NOTE]
> Depois que um cliente no Windows 10 obtém seu pacote UWP, você não pode distribuir o cliente voltar a usar um pacote para qualquer versão anterior do sistema operacional. 

Observe que o número de versão de seus pacotes do Windows 10 deve ser maior do que aqueles para os pacotes do Windows 8, Windows 8.1 e Windows Phone 8.1 que você usou. Para saber mais, veja [Numeração de versão do pacote](package-version-numbering.md).

Para obter mais informações sobre como empacotar aplicativos UWP para a Microsoft Store, consulte [Empacotando aplicativos](../packaging/index.md).
