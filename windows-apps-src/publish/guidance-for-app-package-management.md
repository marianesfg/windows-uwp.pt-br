---
Description: Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.
title: Diretrizes para gerenciamento do pacote do aplicativo
ms.assetid: 55405D0B-5C1E-43C8-91A1-4BFDD336E6AB
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 4615503c41df6cef891ab8e77024d9951c489b38
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945939"
---
# <a name="guidance-for-app-package-management"></a>Diretrizes para gerenciamento do pacote do aplicativo

Saiba como os pacotes do aplicativo são disponibilizados para seus clientes e como gerenciar cenários de pacotes específicos.

-   [Versões do sistema operacional e distribuição de pacote](#os-versions-and-package-distribution)
-   [Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente](#adding-packages-for-windows-10-to-a-previously-published-app)
-   [Como remover um app da Store](#removing-an-app-from-the-store)
-   [Removendo pacotes para uma família de dispositivos com suporte anterior](#removing-packages-for-a-previously-supported-device-family)


## <a name="os-versions-and-package-distribution"></a>Versões do sistema operacional e distribuição de pacote

Diferentes sistemas operacionais podem executar diferentes tipos de pacotes. Se mais de um dos seus pacotes puder ser executado no dispositivo do cliente, a Microsoft Store fornecerá a melhor correspondência disponível.

De modo geral, as versões posteriores do sistema operacional podem executar pacotes para versões anteriores do sistema operacional para a mesma família de dispositivos. Os dispositivos Windows 10 podem executar todas as versões anteriores do sistema operacional com suporte (por família de dispositivos). Os dispositivos de desktop Windows 10 podem executar aplicativos criados para o Windows 8.1 ou Windows 8; os dispositivos móveis Windows 10 podem executar aplicativos criados para Windows Phone 8.1, Windows Phone 8 e até mesmo Windows Phone 7.x. No entanto, os clientes no Windows 10 só obterão esses pacotes se o aplicativo não incluir pacotes UWP direcionados à família de dispositivos aplicáveis.

> [!IMPORTANT]
> Você não pode mais carregar novos pacotes XAP criados usando os SDKs do Windows Phone 8. x. Os aplicativos que já estão em armazenamento com pacotes XAP continuarão a funcionar em dispositivos Windows 10 Mobile. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).


## <a name="removing-an-app-from-the-store"></a>Como remover um app da Store

Às vezes, você pode desejar cancelar a oferta de um aplicativo aos clientes efetivamente "cancelando sua publicação". Para fazer isso, clique em **tornar o aplicativo indisponível** na página **visão geral do aplicativo** . Depois que você confirmar que deseja tornar o aplicativo indisponível, em algumas horas ele não estará mais visível na Microsoft Store, e os novos clientes não poderão obtê-lo (exceto com um [código promocional](generate-promotional-codes.md) e se usarem o dispositivo Windows 10).

> [!IMPORTANT]
> Essa opção substituirá qualquer configuração de [visibilidade](choose-visibility-options.md#discoverability) que você selecionou nos envios. 

Essa opção tem o mesmo efeito de como se você tivesse criado um envio e escolhido **Disponibilizar este produto mas não torná-lo detectável na Store** com a opção **Parar aquisição**. No entanto, isso não exige que você crie um novo envio.

Observe que todos os clientes que já têm o aplicativo ainda poderão usá-lo e baixá-lo novamente (e ainda poderão receber atualizações se você enviar novos pacotes mais tarde).

Depois de tornar o aplicativo indisponível, você ainda o verá no Partner Center. Se optar por oferecer o aplicativo aos clientes novamente, você poderá clicar em **Tornar aplicativo disponível** na página Visão geral do aplicativo. Depois que você confirmar, o aplicativo estará disponível a novos clientes (a menos que esteja restrito pelas configurações no seu último envio) dentro de algumas horas.

> [!NOTE]
> Se você quer manter seu aplicativo disponível, mas não quer continuar oferecendo-o aos novos clientes em uma determinada versão de sistema operacional, crie um novo envio e remova todos os pacotes da versão do sistema operacional na qual deseja impedir novas aquisições. Por exemplo, se antes você tinha pacotes para Windows Phone 8 e Windows 10, e agora não deseja continuar oferecendo o aplicativo para novos clientes no Windows Phone 8.1, remova todos os pacotes do Windows Phone 8.1 do envio. Depois que a atualização for publicada, nenhum cliente novo no Windows Phone 8,1 será capaz de adquirir o aplicativo, embora os clientes que já tenham ele possa continuar a usá-lo). Entretanto, o aplicativo ainda estará disponível para novos clientes no Windows 10.


## <a name="removing-packages-for-a-previously-supported-device-family"></a>Removendo pacotes para uma família de dispositivos com suporte anterior

Se você remover todos os pacotes de uma determinada [família de dispositivos](https://docs.microsoft.com/uwp/extension-sdks/device-families-overview) com suporte do aplicativo anteriormente, será solicitado que você confirme se essa é sua intenção antes de salvar suas alterações na página **pacotes** .

Quando você publica um envio que remove todos os pacotes que podiam ser executados em uma família de dispositivos com suporte de seu aplicativo, novos clientes não poderão adquirir o aplicativo nessa família de dispositivos. Você sempre pode publicar outra atualização posteriormente para fornecer pacotes para essa família de dispositivos novamente.

Lembre-se de que mesmo se você remover todos os pacotes que dão suporte a uma determinada família de dispositivos, quaisquer clientes existentes que já tiverem instalado o aplicativo nesse tipo de dispositivo ainda poderão usá-lo e receberão as atualizações que você fornecer mais tarde.


<a name="adding-packages-for-windows-10-to-a-previously-published-app"></a>

## <a name="adding-packages-for-windows10-to-a-previously-published-app"></a>Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente

Se você tiver um aplicativo na loja que incluiu apenas pacotes do Windows 8. x e/ou Windows Phone 8. x e quiser atualizar seu aplicativo para o Windows 10, crie um novo envio e adicione seus pacotes UWP. msixupload ou. appxupload durante a etapa [pacotes](upload-app-packages.md) . Depois que o aplicativo passa pelo processo de certificação, o pacote UWP também estará disponível para novas aquisições por clientes no Windows 10.

> [!NOTE]
> Depois que um cliente no Windows 10 obtiver o pacote UWP, não é possível reverter esse cliente para usar um pacote de qualquer versão anterior do sistema operacional. 

Observe que o número de versão de seus pacotes do Windows 10 deve ser maior do que aqueles para qualquer pacote do Windows 8, Windows 8.1 e/ou Windows Phone 8,1 que você usou. Para saber mais, veja [Numeração de versão do pacote](package-version-numbering.md).

Para obter mais informações sobre como empacotar aplicativos UWP para a Microsoft Store, consulte [Empacotando aplicativos](../packaging/index.md).
