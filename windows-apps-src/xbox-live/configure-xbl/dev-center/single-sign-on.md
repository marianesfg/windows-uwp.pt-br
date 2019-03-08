---
title: Configurar o logon único no Partner Center
description: Descreve como você pode configurar o logon único no Partner Center para permitir que um título conectar a um usuário em seus serviços usando sua ID do Xbox Live.
ms.assetid: ''
ms.date: 02/21/2018
ms.topic: article
ms.localizationpriority: medium
keywords: Xbox live, xbox, jogos, uwp, windows 10, xbox um, udc, Central de desenvolvedores universal e logon único
ms.openlocfilehash: 32f06edd407d8c1fa74795d0a230c7d56ba8e838
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632871"
---
# <a name="configure-single-sign-on-in-partner-center"></a>Configurar o logon único no Partner Center

O logon único permite que um player usando o título para entrar em seus serviços usando seu sign-in do Xbox Live. Isso permite que um player que está conectado no Xbox Live, executar um aplicativo ou jogo para o seu serviço sem precisar fazer logon em uma segunda vez usando uma credencial de conta diferente específica para seu serviço.

> [!NOTE]
> Este tópico não se aplica a títulos do programa de criadores do Xbox Live.

Por exemplo, o título pode ser um aplicativo que permite que seu serviço para transmitir o conteúdo (vídeos, música, etc.) para seu dispositivo, desde que eles têm uma conta válida com o seu serviço. Se o usuário está conectado à sua conta do Xbox Live, eles devem ser capazes de transmitir o conteúdo sem precisar entrar no seu serviço de cada vez.

Também, se seu aplicativo envia dados do Kinect para um serviço externo, você pode configurá-lo aqui.

Se o serviço exigir que o usuário crie uma conta separada do Xbox Live, você deve fornecer uma maneira para o usuário vincular sua conta do Xbox Live com sua conta no seu serviço como uma ação de tempo.

Quando você configura o logon único, você pode especificar URLs e sua terceira parte confiável. Sempre que seu aplicativo chamar qualquer uma das URLs especificadas, Xbox Live automaticamente anexa um token do Xbox Secure Token Service (XSTS). O serviço que recebe a chave é conhecido como um *terceira parte confiável*, e você deve configurar [terceiras partes confiáveis](https://developer.microsoft.com/en-US/xboxconfig/relyingparties/index) antes de configurar o logon único. Cada configuração da terceira parte confiável Especifica quais informações estão contidas no token XSTS, bem como uma chave de criptografia exclusivo que a terceira parte confiável pode usar para decodificar o token XSTS.

Adicione configuração fazendo o seguinte:

1. Depois de selecionar o título na [Partner Center](https://partner.microsoft.com/dashboard), navegue até **Services** > **Xbox Live**.

2. Clique no link para **Xbox Live sign-on único**.

3. Clique no **Adicionar URL** botão para criar uma novo logon entrada única. Isso adicionará uma nova linha na parte inferior da lista de configurações.

4. Na caixa URL, insira a URL para seu serviço usando um nome de domínio totalmente qualificado. Você pode substituir o subdomínio de nível mais baixo com um caractere curinga ('\*'). Isso corresponderá a qualquer URL que tem os mesmos domínios de nível superior. Por exemplo, "*. example.com&quot; corresponderá a"bar.example.com"ou"foo.bar.example.com".

5. Na caixa de terceiros de terceira parte confiável, selecione a configuração da terceira parte confiável que especifica como o token XSTS é codificado.

6. Clique no **salvar** botão para salvar suas alterações.

![Captura de tela da página de configuração de logon único](../../images/dev-center/single-signon.png)
