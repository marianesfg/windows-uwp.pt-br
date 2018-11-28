---
title: Como usar o Fiddler com o Xbox One ao desenvolver para UWP
description: Descreve como usar a ferramenta de Fiddler gratuita para ver o tráfego de rede em um kit de desenvolvimento UWP do Xbox One.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 9c133c77-fe9d-4b81-b4b3-462936333aa3
ms.localizationpriority: medium
ms.openlocfilehash: c27891b47bb9f7774799c912cc6f4cae3cea92bc
ms.sourcegitcommit: b11f305dbf7649c4b68550b666487c77ea30d98f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/27/2018
ms.locfileid: "7834816"
---
# <a name="how-to-use-fiddler-with-xbox-one-when-developing-for-uwp"></a>Como usar o Fiddler com o Xbox One ao desenvolver para UWP

Fiddler é uma Web proxy de depuração que registra todo o tráfego HTTP e HTTPS entre seu kit de desenvolvimento do Xbox One e a Internet. Você o usará para fazer logon e inspecionar o tráfego de/para os serviços do Xbox e confiar em serviços web de terceiros, para compreender e depurar chamadas de serviço web. 

Em operação normal, um console que se comunica por meio de um proxy está em risco de ter suas comunicações modificadas por proxy, possivelmente permitindo que os jogadores trapaceiem. Assim, os consoles são projetados para não permitir a comunicação através de um proxy. Usar o Fiddler com o kit de desenvolvimento do Xbox One requer que você execute algumas etapas de configuração especiais no kit de desenvolvimento para permitir que ele use o proxy do Fiddler. 

Fiddler é gratuito e pode ser baixado no [próprio site](http://www.fiddler2.com/fiddler2/). 

Fiddler pode impactar no status de rede relatado pelo console. Se uma conexão upstream é desativada da máquina executando o Fiddler, o console não pode detectar essa desconexão até que a autenticação do console expire. Se você estiver usando o Fiddler, certifique-se de desconectar a conexão entre o console e o computador executando o Fiddler, em vez de usar o Fiddler para simular uma desconexão.

### <a name="to-install-and-enable-fiddler-on-your-development-pc"></a>Para instalar e habilitar o Fiddler no seu computador de desenvolvimento
Siga estas etapas para instalar e habilitar o Fiddler para monitorar o tráfego do seu kit de desenvolvimento:

1. Instale o Fiddler no seu computador de desenvolvimento seguindo as instruções do [site do Fiddler](http://www.fiddler2.com/fiddler2/). 
2. Inicie o Fiddler e selecione **Opções do Fiddler** a partir do menu **Ferramentas**. 
3. Selecione a aba **Conexões** e garanta que a opção **Permitir que os computadores remotos se conectem** esteja selecionada. 
4. Clique em **OK** para aceitar a alteração das configurações. Você verá uma caixa de diálogo afirmando que o Fiddler deve ser reiniciado para que a alteração entre em vigor e que talvez você precise configurar seu firewall manualmente. Clique em **OK** nesta caixa de diálogo, mas *não reinicie o Fiddler*.
5. Configure as regras de firewall necessárias para permitir que os computadores remotos se conectem. Inicie o applet do Painel de Controle de Firewall do Windows. Clique em **Configurações avançadas** e, em seguida em **Regra de entrada**. Encontre a regra denominada "FiddlerProxy" e role a tela para a direita, verificando se cada uma das configurações na tabela a seguir é exibido para essa regra.
  
  | Configuração           | Valor preferido                |
  | ----              | ----                           |
  | Nome              | FiddlerProxy                   |
  | Grupo             | *Sem valor* |
  | Perfil           | Todos                            |
  | Habilitada           | Sim                            |
  | Ação            | Permitido                          |
  | Controle manual          | Não                             |
  | Programa           | *Caminho para fiddler.exe*          |
  | LocalAddress      | Qualquer                            |
  | RemoteAddress     | Qualquer                            |
  | Protocolo          | TCP                            |
  | LocalPort         | Qualquer                            |
  | RemotePort        | Qualquer                            |
  | AllowedUsers      | Qualquer                            |
  | AllowedComputers  | Qualquer                            |


6. Configure o Fiddler para capturar e descriptografar o tráfego HTTPS fazendo o seguinte:
  1. Para habilitar o melhor desempenho, defina o Fiddler para usar o Modo Streaming clicando no botão **Fluxo** na barra de botões.
  2. No menu **Ferramentas** do Fiddler, selecione **Opções do Fiddler** e clique em **HTTPS**.
  3. Selecione a caixa de seleção **Descriptografar tráfego HTTPS**. Se uma caixa de diálogo perguntar se deseja configurar o Windows para que confie no certificado de AC, clique em **Não**.
  4. Clique em **Exportar certificado raiz para área de trabalho**.
7. Saia e reinicie o Fiddler.

### <a name="to-configure-a-dev-kit-to-use-fiddler-as-its-proxy-to-the-internet"></a>Para configurar um kit de desenvolvimento para usar o Fiddler como seu proxy para a Internet

1. Navegue até a ferramenta **Rede** na interface do usuário do Device Portal do Xbox.
2. Procure o certificado raiz Fiddler exportado para a área de trabalho. 
3. Digite o endereço IP ou nome do host do computador de desenvolvimento executando o Fiddler.
4. Digite o número da porta onde o Fiddler estiver recebendo (por padrão, o Fiddler usa a porta 8888). 
5. Clique em **Habilitar**. Isso reiniciará o kit de desenvolvimento.

### <a name="to-stop-using-fiddler"></a>Para parar de usar o Fiddler
Para parar de usar o Fiddler como um proxy para a Internet (e parar o rastreamento de todo o tráfego do kit de desenvolvimento de rede pelo Fiddler), faça o seguinte:

1. Navegue até a ferramenta **Rede** na interface do usuário do Device Portal do Xbox.
2. Clique em **Desativar**.

> [!NOTE]
> Cada computador com o Fiddler instalado usa um certificado raiz Fiddler diferente. Se você tiver mais de um computador que pode ser usado para fornecer um proxy Fiddler para o kit de desenvolvimento, você precisará selecionar o novo certificado raiz ao alternar entre eles. Se você estiver usando apenas um computador, você precisa selecionar o certificado raiz apenas na primeira vez que habilitar o Fiddler. Você ainda deve especificar o endereço IP e a porta.

## <a name="see-also"></a>Consulte também
- [Referência de API de configurações Fiddler](wdp-fiddler-api.md)
- [Perguntas frequentes](frequently-asked-questions.md)
- [UWP no Xbox One](index.md)



