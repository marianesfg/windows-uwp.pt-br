---
author: jnHs
Description: "A página Propriedades do aplicativo do processo de envio de aplicativo permite definir a categoria do seu aplicativo e indicar as preferências de hardware ou outras declarações."
title: Insira as propriedades do aplicativo
ms.assetid: CDE4AF96-95A0-4635-9D07-A27B810CAE26
ms.author: wdg-dev-content
ms.date: 08/03/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: e4d391d551cf4e41853a1aac0e4b5be8bf0b0c3f
ms.sourcegitcommit: 968187e803a866b60cda0528718a3d31f07dc54c
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/03/2017
---
# <a name="enter-app-properties"></a>Insira as propriedades do aplicativo

A página **Propriedades** do [processo de envio de aplicativo](app-submissions.md) permite definir a categoria do aplicativo e indicar as preferências de hardware ou outras declarações. Aqui, nós o guiaremos pelas opções desta página e o que você deve considerar ao inserir essas informações.

## <a name="category-and-subcategory"></a>Categoria e subcategoria

Nesta seção, você indica a categoria (e subcategoria, se aplicável) que a Loja deve usar para categorizar seu aplicativo. Especificar uma categoria é necessário para enviar seu aplicativo.

Para obter mais informações, consulte [Tabela de categorias e subcategorias](category-and-subcategory-table.md).

## <a name="game-settings"></a>Configurações do jogo

Esta seção só aparecerá se você tiver selecionado **Jogos** como categoria do produto. Aqui você pode especificar quais recursos são compatíveis com seu jogo. Todas as informações que você fornecer nesta seção serão exibidas na listagem da Loja do produto.

Se o jogo oferecer suporte a qualquer uma das opções multijogador, indique o número mínimo e máximo de jogadores em uma sessão. Você não pode inserir mais que o mínimo ou o máximo de 1.000 jogadores.

**Vários jogadores entre plataformas** significa que o jogo oferece suporte a sessões multijogador entre jogadores no Xbox e em computadores Windows 10.


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

Também recomendamos adicionar verificações de tempo de execução para o hardware especificado em seu aplicativo, pois a Loja nem sempre pode detectar se os recursos selecionados estão ausentes no dispositivo do cliente e ele ainda poderá baixar seu aplicativo, mesmo se um aviso for exibido.

> [!TIP]
> Se quiser impedir completamente que seu aplicativo UWP seja baixado em um dispositivo que não atende aos requisitos mínimos de memória ou nível do DirectX, você pode designar os requisitos mínimos em um arquivo XML StoreManifest. Para saber mais, consulte [Esquema StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335).


