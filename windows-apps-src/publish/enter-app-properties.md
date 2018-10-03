---
author: jnHs
Description: The App properties page of the app submission process lets you define your app's category and indicate hardware preferences or other declarations.
title: Inserir as propriedades do aplicativo
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, configurações de jogo, modo de exibição, requisitos do sistema, requisitos de hardware, hardware mínimo, hardware recomendado, política de privacidade, informações de contato de suporte, site do app, informações de suporte
ms.localizationpriority: medium
ms.openlocfilehash: d23fb0cb3fb4668682df1957cbf0c88bcf8649c1
ms.sourcegitcommit: e6daa7ff878f2f0c7015aca9787e7f2730abcfbf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/03/2018
ms.locfileid: "4310908"
---
# <a name="enter-app-properties"></a>Inserir as propriedades do aplicativo

A página **Propriedades** do [processo de envio de aplicativo](app-submissions.md) é onde você define a categoria do seu app e insere outras informações e declarações. Certifique-se de fornecer detalhes completos e precisos sobre seu app nessa página.


## <a name="category-and-subcategory"></a>Categoria e subcategoria

Você deve indicar a categoria (e subcategoria/gênero, se aplicável) que a Store deve usar para categorizar seu app. Especificar uma categoria é necessário para enviar seu aplicativo.

Para obter mais informações, consulte [Tabela de categorias e subcategorias](category-and-subcategory-table.md).


## <a name="support-info"></a>Informações de suporte

Esta seção permite que você forneça informações para ajudar os clientes a entender mais sobre o seu app e como obter suporte.

### <a name="privacy-policy-url"></a>URL da política de privacidade

Você é responsável por garantir que seu aplicativo esteja em conformidade com as leis e as normas privacidade e por fornecer uma URL da política de privacidade válida, se necessário.

Nesta seção, você deve indicar se o seu app acessa, coleta ou transmite quaisquer [informações pessoais](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information). Se você responder **Sim**, será necessária uma URL da política de privacidade. Caso contrário, será opcional (embora se determinamos que seu app exige uma política de privacidade, e se você não tiver fornecido uma, seu envio poderá falhar na certificação).

> [!NOTE]
> Se detectarmos que seus pacotes declaram [recursos](../packaging/app-capability-declarations.md) que podem permitir que as informações pessoais sejam acessados, transmitidos ou coletados, marcaremos essa pergunta como **Sim** e você precisará inserir uma URL da política de privacidade.

Para ajudar a determinar se o app requer uma política de privacidade, consulte o [Contrato de Desenvolvedor de Aplicativos](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) e as [Políticas da Microsoft Store](https://docs.microsoft.com/legal/windows/agreements/store-policies#105-personal-information). 

> [!NOTE]
> A Microsoft não fornece uma política de privacidade padrão para o aplicativo. Da mesma forma, o aplicativo não é coberto por nenhuma política de privacidade da Microsoft. 


### <a name="website"></a>Site

Insira a URL da página da Web do seu aplicativo. A URL deve apontar para uma página em seu próprio site, não para os detalhes do seu aplicativo na Store. Esse campo é opcional, mas recomendado.

### <a name="support-contact-info"></a>Informações de contato de suporte

Insira a URL da página da Web em que seus clientes podem buscar suporte relacionado ao seu app ou o endereço de email que os seus clientes podem contatar para obter suporte. É recomendável incluir essas informações para todos os envios, para que seus clientes saibam como obter suporte, se necessário. Observe que a Microsoft não fornece suporte para seu app aos seus clientes.

> [!IMPORTANT]
> O campo **Informações de contato de suporte** será obrigatório se o seu app ou jogo estiver disponível no Xbox. Caso contrário, será opcional, mas recomendado.


## <a name="game-settings"></a>Configurações do jogo

Esta seção só aparecerá se você tiver selecionado **Jogos** como categoria do produto. Aqui você pode especificar quais recursos são compatíveis com seu jogo. Todas as informações que você fornecer nesta seção serão exibidas na listagem da Loja do produto.

Se o jogo oferecer suporte a qualquer uma das opções multijogador, indique o número mínimo e máximo de jogadores em uma sessão. Você não pode inserir mais que o mínimo ou o máximo de 1.000 jogadores.

**Vários jogadores entre plataformas** significa que o jogo oferece suporte a sessões multijogador entre jogadores em computadores Windows 10 e no Xbox.


## <a name="display-mode"></a>Modo de exibição

Esta seção permite que você indique se o seu produto foi projetado para ser executado em uma exibição imersiva (não em 2D) do [Windows Mixed Reality](https://developer.microsoft.com/windows/mixed-reality) em computadores e/ou dispositivos HoloLens. Se você indicar que sim, também precisará:
- Selecionar a opção **Hardware mínimo** ou **Hardware recomendado** para **Headset imersivo do Windows Mixed Reality** na seção [Requisitos do sistema](#system-requirements) que é exibida na parte inferior da página **Propriedades**.
- Especificar a **Configuração do marco de delimitação** (se o computador estiver selecionado) para que os usuários saibam se ele deve ser usado apenas em uma posição sentada ou em pé ou se ele permite (ou requer) que o usuário se mova ao usá-lo. 

Se tiver selecionado **Jogos** como categoria do produto, você verá opções adicionais na seleção **Modo de exibição** que permitem indicar se seu produto oferece suporte à saída de vídeo de resolução 4K, saída de vídeo de Alto Alcance Dinâmico (HDR) ou exibições de taxa de atualização variável.

Se seu produto não oferecer suporte a nenhuma dessas opções de modo de exibição, deixe todas as caixas desmarcadas.


## <a name="product-declarations"></a>Declarações do produto

Você pode marcar caixas nesta seção para indicar se qualquer uma das declarações se aplicam ao seu aplicativo. Isso pode afetar a forma como seu aplicativo é exibido, se ele é oferecido para determinados clientes ou como os clientes podem usá-lo.

Para obter mais informações, consulte [Declarações de produto](app-declarations.md).

## <a name="system-requirements"></a>Requisitos do sistema

Nesta seção, você tem a opção de indicar se determinados recursos de hardware são necessários ou recomendados para executar e interagir com seu aplicativo corretamente. Você pode marcar a caixa (ou indicar a opção adequada) para cada item de hardware onde deseja especificar **Hardware mínimo** e/ou **Hardware recomendado**.

Se você selecionar **Hardware recomendado**, esses itens serão exibidos na listagem da Loja do seu produto como hardware recomendado para clientes do Windows 10, versão 1607 ou posterior. Os clientes com versões anteriores do sistema operacional não verão essas informações.

Se você selecionar **Hardware mínimo**, esses itens serão exibidos na listagem da Loja do seu produto como hardware necessário para clientes do Windows 10, versão 1607 ou posterior. Os clientes com versões anteriores do sistema operacional não verão essas informações. A Loja também pode exibir um aviso aos clientes que estão visualizando os detalhes do seu aplicativo em um dispositivo que não tem o hardware necessário. Isso não impede que as pessoas baixem seu aplicativo em dispositivos que não tenham o hardware apropriado, mas elas não poderão classificar ou analisar o aplicativo nesses dispositivos. 

O comportamento dos clientes varia de acordo com os requisitos específicos e a versão do Windows do cliente:

- **Para clientes com Windows 10, versão 1607 ou posterior:**
     - Todos os requisitos mínimos e recomendados serão exibidos na listagem da Loja.
     - A Loja verificará todos os requisitos mínimos e exibirá um aviso aos clientes em um dispositivo que não satisfaz os requisitos.
- **Para clientes com versões anteriores do Windows 10:**
     - Para a maioria dos clientes, todos os requisitos de hardware mínimos e recomendados serão exibidos na listagem da Loja (embora os clientes que acessarem uma versões mais antigas do cliente da Loja só verão os requisitos mínimos de hardware).
     - A Loja tentará verificar os itens que você designar como **Hardware mínimo**, com exceção de **Memória**, **DirectX**, **Memória de vídeo**, **Elementos gráficos** e **Processador**. Nenhuma dessas opções serão verificadas e os clientes não verão nenhum aviso em dispositivos que não atendam a esses requisitos. 
- **Para clientes com Windows 8.x e versões anteriores ou Windows Phone 8.x e versões anteriores:**
     - Se você marcar a caixa **Hardware mínimo** para **Tela touch**, esse requisito será exibido na listagem da Loja do seu aplicativo e os clientes em dispositivos sem tela touch verão um aviso se tentarem baixar o aplicativo. Nenhum outro requisito será verificado ou exibido em sua listagem da Loja.

Também recomendamos adicionar verificações de tempo de execução para o hardware especificado em seu aplicativo, pois a Loja nem sempre pode detectar se os recursos selecionados estão ausentes no dispositivo do cliente e ele ainda poderá baixar seu aplicativo, mesmo se um aviso for exibido. Se quiser impedir completamente que seu aplicativo UWP seja baixado em um dispositivo que não atende aos requisitos mínimos de memória ou nível do DirectX, você pode designar os requisitos mínimos em um arquivo [XML StoreManifest](https://docs.microsoft.com/uwp/schemas/storemanifest/storemanifestschema2015/schema-root).

> [!TIP]
> Se o produto exige itens adicionais que não estão listados nesta seção para funcionar corretamente, como impressoras 3D ou dispositivos USB, você também pode inserir [mais requisitos do sistema](create-app-store-listings.md#additional-system-requirements) ao criar a listagem da Microsoft Store.





