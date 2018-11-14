---
author: lex
Description: The following article describes all of the properties and elements within the toast content XML payload.
title: Esquema XML de conteúdo de notificação do sistema
ms.assetid: AF49EFAC-447E-44C3-93C3-CCBEDCF07D22
label: Toast content XML schema
template: detail.hbs
ms.author: mijacobs
ms.date: 05/19/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: bcfc56264ab3063995fd9f2b06bd93e9406cd37e
ms.sourcegitcommit: 71e8eae5c077a7740e5606298951bb78fc42b22c
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/13/2018
ms.locfileid: "6647129"
---
# <a name="toast-content-xml-schema"></a>Esquema XML de conteúdo de notificação do sistema

 

A seguir, são descritas todas as propriedades e os elementos na carga XML do conteúdo da notificação do sistema.

Nos esquemas XML a seguir, o sufixo "?" significa que o atributo é opcional.

## <a name="ltvisualgt-and-ltaudiogt"></a>&lt;visual&gt; e &lt;áudio&gt;

```
<toast launch? duration? activationType? scenario? >
  <visual lang? baseUri? addImageQuery? >
    <binding template? lang? baseUri? addImageQuery? >
      <text lang? hint-maxLines? >content</text>
      <image src placement? alt? addImageQuery? hint-crop? />
      <group>
        <subgroup hint-weight? hint-textStacking? >
          <text />
          <image />
        </subgroup>
      </group>
    </binding>
  </visual>
  <audio src? loop? silent? />
</toast>
```

**Atributos na &lt;notificação do sistema&gt;**

launch?

-   launch? = string
-   Esse é um atributo opcional.
-   Uma cadeia de caracteres que é passada para o aplicativo quando ele é ativado pela notificação do sistema.
-   Dependendo do valor de activationType, esse valor pode ser recebido pelo aplicativo em primeiro plano, dentro de tarefa em segundo plano ou por outro aplicativo que é iniciado por protocolo a partir do aplicativo original.
-   O formato e o conteúdo dessa cadeia de caracteres são definidos pelo aplicativo para seu uso próprio.
-   Quando o usuário toca ou clica na notificação do sistema para iniciar o aplicativo associado, a cadeia de caracteres de inicialização fornece o contexto ao aplicativo que o permite mostrar ao usuário uma exibição relevante para o conteúdo da notificação do sistema, em vez de inicializar em sua maneira padrão.
-   Se a ativação ocorreu porque o usuário clicou em uma ação, em vez do corpo da notificação do sistema, o desenvolvedor recupera os "argumentos" predefinidos dessa marca &lt;action&gt;, em vez do "launch" predefinido na marca &lt;toast&gt;.

duration?

-   duration? = "short|long"
-   Esse é um atributo opcional. O valor padrão é "short".
-   Isso fica aqui apenas para cenários específicos e appCompat. Você não precisa mais disso para o cenário de alarme.
-   Não recomendamos o uso dessa propriedade.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   Esse é um atributo opcional.
-   O valor padrão é "foreground".

scenario?

-   scenario? = "default | alarm | reminder | incomingCall"
-   Esse é um atributo opcional, o valor padrão é "default".
-   Você não precisa disso, a menos que seu cenário seja exibir um alarme, lembrete ou chamada de entrada.
-   Não use isso apenas para manter sua notificação persistente na tela.

**Atributos em &lt;visual&gt;**

lang?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

baseUri?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

addImageQuery?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

**Atributos em &lt;vinculação&gt;**

template?

-   \[Important\] template? = "ToastGeneric"
-   Se você estiver usando qualquer um dos novos recursos de notificação interativa e adaptável, comece a usar o modelo "ToastGeneric" em vez do modelo herdado.
-   Usar os modelos herdados com as novas ações pode funcionar agora, mas esse não é o caso de uso esperado, e não podemos garantir que isso continuará a funcionar.

lang?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

baseUri?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

addImageQuery?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

**Atributos em &lt;texto&gt;**

lang?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230847) para obter detalhes sobre esse atributo opcional.

**Atributos em &lt;imagens&gt;**

src

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230844) para obter detalhes sobre esse atributo obrigatório.

placement?

-   placement? = "inline" | "appLogoOverride"
-   Esse atributo é opcional.
-   Isso especifica onde essa imagem será exibida.
-   "inline" significa dentro do corpo da notificação do sistema, abaixo do texto; "appLogoOverride" significa substituir o ícone do aplicativo (que aparece no canto superior esquerdo da notificação do sistema).
-   Você pode ter até uma imagem para cada valor de posicionamento.

alt?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230844) para obter detalhes sobre esse atributo opcional.

addImageQuery?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230844) para obter detalhes sobre esse atributo opcional.

hint-crop?

-   hint-crop? = "none" | "circle"
-   Esse atributo é opcional.
-   "none" é o valor padrão que significa nenhum corte.
-   "circle" corta a imagem em formato circular. Use isso para imagens de perfil de um contato, imagens de uma pessoa e assim por diante.

**Atributos em &lt;áudio&gt;**

src?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230842) para obter detalhes sobre esse atributo opcional.

loop?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230842) para obter detalhes sobre esse atributo opcional.

silent?

-   Consulte [este artigo sobre o esquema de elementos](https://msdn.microsoft.com/library/windows/apps/br230842) para obter detalhes sobre esse atributo opcional.

## <a name="schemas-ltactiongt"></a>Esquemas: &lt;ação&gt;


Nos esquemas XML a seguir, o sufixo "?" significa que o atributo é opcional.

```
<toast>
  <visual>
  </visual>
  <audio />
  <actions>
    <input id type title? placeHolderContent? defaultInput? >
      <selection id content />
    </input>
    <action content arguments activationType? imageUri? hint-inputId />
  </actions>
</toast>
```

**Atributos em &lt;entrada&gt;**

id

-   id = string
-   Esse atributo é obrigatório.
-   O atributo id é obrigatório e é usado pelos desenvolvedores para recuperar entradas do usuário depois que o aplicativo é ativado (em primeiro plano ou em segundo plano).

type

-   type = "text | selection"
-   Esse atributo é obrigatório.
-   Ele é usado para especificar uma entrada de texto ou entrada de uma lista de seleções predefinidas.
-   No celular e desktop, isso serve para especificar se você deseja uma entrada de caixa de texto ou uma entrada de caixa de listagem.

title?

-   title? = string
-   O atributo title é opcional e é para os desenvolvedores especificarem um título para a entrada para a renderização de shells quando há funcionalidade.
-   Para celular e desktop, esse título será exibido acima da entrada.

placeHolderContent?

-   placeHolderContent? = string
-   O atributo placeHolderContent é opcional e é o texto de dica cinza para o tipo de entrada de texto. Esse atributo é ignorado quando o tipo de entrada não é "text".

defaultInput?

-   defaultInput? = string
-   O atributo defaultInput é opcional e é usado para fornecer um valor de entrada padrão.
-   Se o tipo de entrada for "text", isso será tratado como uma entrada de cadeia de caracteres.
-   Se o tipo de entrada for "selection", isso deve ser a identificação de uma das seleções disponíveis dentro dos elementos dessa entrada.

**Atributos em &lt;seleção&gt;**

id

-   Esse atributo é obrigatório. Ele é usado para identificar as seleções do usuário. A identificação é retornada para seu aplicativo.

content

-   Esse atributo é obrigatório. Ele fornece a cadeia de caracteres a ser exibida para esse elemento de seleção.

**Atributos em &lt;ação&gt;**

content

-   content = string
-   O atributo content é obrigatório. Ele fornece a cadeia de caracteres de texto exibida no botão.

arguments

-   arguments = string
-   O atributo arguments é obrigatório. Ele descreve os dados definidos pelo aplicativo que, mais tarde, o aplicativo pode recuperar quando ele for ativado quando o usuário executar essa ação.

activationType?

-   activationType? = "foreground | background | protocol | system"
-   O atributo activationType é opcional e seu valor padrão é "foreground".
-   Descreve o tipo de ativação que essa ação causará: em primeiro plano, em segundo plano, iniciar outro aplicativo por meio de inicialização por protocolo ou invocar uma ação do sistema.

imageUri?

-   imageUri? = string
-   imageUri é opcional e é usado para fornecer um ícone de imagem para essa ação exibir dentro do botão somente com o conteúdo de texto.

hint-inputId

-   hint-inputId = string
-   O atributo hint-inpudId é obrigatório. Ele é usado especificamente para o cenário de resposta rápida.
-   O valor deve ser que a identificação do elemento input desejado a ser associado.
-   No celular e desktop, isso colocará o botão diretamente ao lado da caixa de entrada.

## <a name="attributes-for-system-handled-actions"></a>Atributos para ações manipuladas pelo sistema


O sistema pode manipular ações para adiar e ignorar notificações se você não quiser que seu aplicativo manipular o adiamento/reagendamento de notificações como tarefa em segundo plano. As ações manipuladas pelo sistema podem ser combinadas (ou especificadas individualmente), mas não é recomendável implementar uma ação de adiamento sem uma ação de descarte.

Combinação de comandos do sistema: SnoozeAndDismiss

```
<toast>
  <visual>
  </visual>
  <actions hint-systemCommands="SnoozeAndDismiss" />
</toast>
```

Ações individuais manipuladas pelo sistema

```
<toast>
  <visual>
  </visual>
  <actions>
  <input id="snoozeTime" type="selection" defaultInput="10">
    <selection id="5" content="5 minutes" />
    <selection id="10" content="10 minutes" />
    <selection id="20" content="20 minutes" />
    <selection id="30" content="30 minutes" />
    <selection id="60" content="1 hour" />
  </input>
  <action activationType="system" arguments="snooze" hint-inputId="snoozeTime" content=""/>
  <action activationType="system" arguments="dismiss" content=""/>
  </actions>
</toast>
```

Para construir ações individuais de adiamento e descarte, faça o seguinte:

-   Especifique activationType = "system"
-   Especifique arguments = "snooze" | "dismiss"
-   Especifique o conteúdo:
    -   Se você quiser que cadeias de caracteres localizadas de "snooze" e "dismiss" sejam exibidas nas ações, especifique o conteúdo para ser uma cadeia de caracteres vazia: &lt;action content = ""/&gt;
    -   Se você quiser uma cadeia de caracteres personalizada, basta fornecer seu valor: &lt;action content="Lembrar-me mais tarde" /&gt;
-   Especifica a entrada:
    -   Se você não quiser que o usuário selecione um intervalo de adiamento e, em vez disso, apenas que sua notificação seja adiada apenas uma vez por um intervalo de tempo definido pelo sistema (consistente com o sistema operacional), não crie nenhuma &lt;input&gt;.
    -   Se você quiser fornecer seleções de intervalo de adiamento:
        -   Especifique hint-inputId na ação snooze
        -   Combine a identificação da entrada com a hint-inputId da ação snooze: &lt;input id="snoozeTime"&gt;&lt;/input&gt;&lt;action hint-inputId="snoozeTime"/&gt;
        -   Especifique a identificação da seleção para ser um nonNegativeInteger que represente o intervalo de adiamento em minutos: &lt;selection id="240" /&gt; significa adiar por 4 horas
        -   Certifique-se de que o valor de defaultInput em &lt;input&gt; corresponda a uma das identificações dos elementos &lt;selection&gt; filho
        -   Forneça até (mas não mais que) 5 valores de &lt;selection&gt;