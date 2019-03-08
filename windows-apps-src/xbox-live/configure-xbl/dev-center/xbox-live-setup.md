---
title: Configuração da instalação do Xbox Live
description: Descreve como você pode configurar o programa de instalação do Xbox Live no Partner Center.
ms.assetid: ''
ms.date: 10/30/2017
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, Xbox, jogos, uwp, windows 10, Xbox um, o Partner Center, o programa de instalação do Xbox Live
ms.openlocfilehash: 9a846a4b7f0069216e92eb123b33d9fc0f7f67c9
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57612821"
---
# <a name="configure-xbox-live-setup-in-partner-center"></a>Configure o Xbox Live no Partner Center

Você pode usar [Partner Center](https://developer.microsoft.com/dashboard) para configurar o conjunto inicial de propriedades do Xbox Live que estão associados com seu jogo. Adicione configuração fazendo o seguinte:

1. Navegue até a **programa de instalação do Xbox Live** seção em seu título, localizado sob **serviços** > **Xbox Live** > **programa de instalação do Xbox Live** .
2. Nessa página, você pode definir os nomes de título, localidade padrão, o tipo de produto, famílias de dispositivos e a data de embargo. Depois que você terminar de definir sua configuração, clique o **salvar** botão para enviar as alterações.

## <a name="title-names"></a>Nomes de título
Clicando em **adicionar localizado título**, você pode inserir um nome para o seu produto e selecione um idioma para localizá-lo para. Observe aqui que o nome do título deve ser mapeado para os nomes de produto localizado que você definiu na página de propriedades do envio. O padrão é inglês (en-US).

![Imagem da caixa de diálogo Adicionar título localizado no Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-1.png)

## <a name="default-locale"></a>Localidade padrão
Essa opção permite que você defina o idioma padrão a ser usado para configurar suas cadeias de caracteres na configuração do serviço Xbox Live. Por exemplo, se você definir a localidade padrão para o espanhol (es-ES) e você deseja configurar uma realização, em seguida, no mínimo, descrição e o nome de medalha precisaria ser em espanhol. Em outras palavras, você não pode definir essa opção para o espanhol, mas fornecem apenas as informações de medalha em inglês. Todos da sua configuração de serviço Xbox Live devem ser fornecidos na mesma versão que a localidade padrão. Por padrão, a localidade padrão é definida como inglês (en-US).
> [!NOTE]
> Além disso, todas as cadeias de caracteres podem ser localizadas na página de cadeias de caracteres localizados.  

![Imagem da seleção suspensa para escolher sua localidade padrão no Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-2.png)

## <a name="product-type"></a>Tipo de produto
O menu suspenso permite que você alterar o tipo do produto. O padrão é o tipo **jogo**. A escolha feita terá impacto sobre os recursos disponíveis para você Xbox Live. Você tem três opções à sua escolha:
1. Aplicativo 
2. Game 
3. Demonstração de jogo 

![Imagem da seleção suspensa para escolher o tipo de produto no Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-3.png)

## <a name="device-families"></a>Famílias de dispositivo
Essa configuração permite que você escolha o tipo de dispositivos nos quais seu título pode acessar o Xbox Live. Por padrão, todas as famílias de dispositivo estão habilitadas. Você pode verificar os dispositivos para habilitá-los.

![Imagem das caixas de seleção de seleção para selecionar as famílias de dispositivo no Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-4.png)

## <a name="embargo-date"></a>Data de embargo
A data em que você selecionar determinará quando a configuração do Xbox Live for lançada para o público. É importante observar que, mesmo se você publicou suas alterações para o varejo que eles não entrará no ar, a menos que a data de embargo foram atendida. Explicar mais detalhes:
1. Se você selecionar uma data no futuro, as alterações serão disponibilizados ao público nessa data.
2. Se você selecionar uma data no passado, as alterações serão disponibilizados ao público, assim que você publica suas alterações para o varejo.

Clique no seletor de data e hora e ela será expandida para permitir que você selecione a data e hora exatas. Depois de clicar em **Okey**, a data de embargo será definida.

![Imagem de como definir a data de embargo no Partner Center](../../images/dev-center/xbox-live-setup/xbox-live-setup-5.png)

## <a name="advanced-settings"></a>Configurações avançadas

Você pode clicar em **Mostrar opções** para definir o **vários pontos de presença**. Vários pontos de presença permite que o mesmo usuário entrar no Xbox Live de vários dispositivos ao mesmo tempo. Recursos do Xbox Live, como, conquistas e com vários participantes serão têm acesso limitado. Portanto, essa opção não é recomendada para jogos. Habilite esta opção, marcando a caixa.
