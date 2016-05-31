---
author: Karl-Bridge-Microsoft
Description: Saiba como estender a Cortana com comandos de voz mais flexíveis e naturais, para que um usuário possa dizer o nome do seu aplicativo em qualquer lugar do comando.
title: Suporte a comandos de voz em linguagem natural na Cortana
ms.assetid: 281E068A-336A-4A8D-879A-D8715C817911
label: Support natural language voice commands
template: detail.hbs
---

# Suporte a comandos de voz em linguagem natural na Cortana

Estenda a **Cortana** com comandos de voz mais flexíveis e naturais, para que um usuário possa dizer o nome do seu aplicativo em qualquer lugar do comando.

**APIs importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos e atributos de Voice Command Definition (VCD) v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)


Usar comandos de voz para estender o Cortana com funcionalidade de seu aplicativo requer que o usuário especifique o aplicativo e o comando ou função a ser executada. Normalmente, isso é feito anunciando o nome do aplicativo no início ou no final do comando de voz. Por exemplo, "Adventure Works, adicione uma nova viagem para Las Vegas."

No entanto, pode parecer estranho, artificial ou não fazer sentido especificar o nome do aplicativo dessa forma. Em muitos casos, a capacidade de dizer o nome do aplicativo em outro lugar do comando é mais confortável e natural, e ajuda a tornar a interação muito mais intuitiva e atraente para o usuário. Nosso exemplo anterior, "Adventure Works, adicione uma nova viagem para Las Vegas." pode ser reescrito como "Adicionar uma nova viagem Adventure Works a Las Vegas". ou "Usando Adventure Works, adicione uma nova viagem a Las Vegas."

Você pode configurar os comandos de voz para dar suporte ao nome do aplicativo como um:

-   Prefixo - antes da frase de comando
-   Infixo - na frase de comando
-   Sufixo - após a frase de comando

**Pré-requisitos:  **

Este tópico complementa [Iniciar um aplicativo em segundo plano com comandos de voz na Cortana](launch-a-background-app-with-voice-commands-in-cortana.md). Continuaremos demonstrando recursos com um aplicativo de planejamento e gerenciamento de viagens chamado **Adventure Works**.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário:  **

Consulte as [Diretrizes de design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), para obter informações sobre como integrar seu aplicativo à **Cortana** e as [Diretrizes de design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obter dicas úteis para criar um aplicativo interessante e prático, habilitado para controle por voz.

## <span id="Specify_an_AppName_element_in_the_VCD"></span><span id="specify_an_appname_element_in_the_vcd"></span><span id="SPECIFY_AN_APPNAME_ELEMENT_IN_THE_VCD"></span>Especifique um elemento **AppName** no VCD


O elemento **AppName** é usado para especificar um nome amigável para um aplicativo em um comando de voz.
```XML
<AppName>Adventure Works</AppName>
```

## <span id="Specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="specify_where_the_app_name_can_be_spoken_in_the_voice_command"></span><span id="SPECIFY_WHERE_THE_APP_NAME_CAN_BE_SPOKEN_IN_THE_VOICE_COMMAND"></span>Especifique em que parte do comando de voz o nome do aplicativo pode ser falado


O elemento **ListenFor** tem um atributo **RequireAppName** que especifica onde o nome do aplicativo pode aparecer no comando de voz. Esse atributo dá suporte a quatro valores.

1.  **BeforePhrase**

    O padrão.

    Indica que os usuários devem dizer o nome do aplicativo antes da frase de comando.

    Assim, a Cortana ouve "Adventure Works quando é minha viagem para Las Vegas".
```xml
<ListenFor RequireAppName="BeforePhrase"> show [my] trip to {destination} </ListenFor>
```

2.  **AfterPhrase**

    Indica que os usuários devem dizer o nome do aplicativo depois da frase de comando.

    Uma lista de frases localizadas de conjunções preposicionais é fornecida pelo sistema. Ela inclui frases como "usando", "com" e "em".

    Assim, a Cortana ouve comandos como "Mostre minha próxima viagem para Las Vegas no Adventure Works" e "Mostre minha próxima viagem para Las Vegas usando o Adventure Works".
```xml
<ListenFor RequireAppName="AfterPhrase">show [my] next trip to {destination} </ListenFor>
```

3.  **BeforeOrAfterPhrase**

    Indica que os usuários devem dizer o nome do aplicativo antes ou depois da frase de comando.

    Para a versão de sufixo, uma lista de frases localizadas de conjunções preposicionais é fornecida pelo sistema. Ela inclui frases como "usando", "com" e "em".

    Assim, a Cortana ouve comandos como "Adventure Works, mostre minha próxima viagem para Las Vegas" ou "Mostre minha próxima viagem para Las Vegas no Adventure Works".
``` xml
<ListenFor RequireAppName="BeforeOrAfterPhrase">show [my] next trip to {destination}</ListenFor>
```

4.  **ExplicitlySpecified**

    Indica que os usuários devem dizer o nome do aplicativo exatamente onde você especificar na frase comando. O usuário não precisa dizer o nome do aplicativo antes ou depois da frase.

    É necessário fazer referência explicitamente ao nome do aplicativo usando a marca **{builtin:AppName}**.

    Assim, a Cortana ouve comandos como "Adventure Works, mostre minha próxima viagem para Las Vegas" ou "Mostre minha próxima viagem do Adventure Works para Las Vegas".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">show [my] next {builtin:AppName} trip to {destination} </ListenFor>
```

## <span id="Special_cases"></span><span id="special_cases"></span><span id="SPECIAL_CASES"></span>Casos especiais

Ao declarar um elemento **ListenFor**, em que **RequireAppName** é "AfterPhrase" ou "ExplicitlySpecified", determinados requisitos devem ser atendidos:

1.  **{builtin:AppName}** deve aparecer apenas uma vez quando **RequireAppName** for "ExplicitlySpecified".

    Com esse valor, o sistema não pode inferir onde o nome do aplicativo pode aparecer no comando de voz. Você deve especificar explicitamente o local.

2.  O comando de voz não pode começar com um elemento **PhraseTopic**, que normalmente é usado para o reconhecimento de fala de grande vocabulário. Deve haver pelo menos uma palavra antes dele.

    Isso ajuda a minimizar a chance de a **Cortana** iniciar seu aplicativo caso um comando contenha o nome do aplicativo ou parte dele em qualquer lugar da expressão.

    Esta é uma declaração inválida que poderia fazer a **Cortana** iniciar o aplicativo **Adventure Works** se o usuário dissesse algo como "Mostre-me críticas do Kinect Adventure works".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">{searchPhrase} {builtin:AppName}</ListenFor>
```

3.  Deve haver pelo menos duas palavras na cadeia de caracteres **ListenFor** além do nome do aplicativo e referências a elementos **PhraseTopic**.

    Semelhante ao caso 2, você precisa garantir que seus comandos tenham conteúdo fonético suficiente para minimizar as chances do aplicativo ser iniciado sem querer.

    Isso ajuda a configurar o aplicativo para o maior sucesso possível, de forma que ele não seja iniciado incorretamente quando o usuário disser, por exemplo, "Localizar Kinect Adventure works".

    Estas são declarações inválidas que poderiam fazer a **Cortana** iniciar o aplicativo **Adventure Works** quando o usuário dissesse algo como "Ei, adventure works" ou "Localizar Kinect adventure works".
```xml
<ListenFor RequireAppName="ExplicitlySpecified">Hey {builtin:AppName}</ListenFor>
<ListenFor RequireAppName="ExplicitlySpecified">Find {searchPhrase} {builtin:AppName}</ListenFor>
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Comentários

O suporte a mais variações de como um comando de voz pode ser expresso pelo usuário na **Cortana** também aumenta a usabilidade geral do aplicativo.

Evite ter "Ei \[nome do aplicativo\]" como seu **AppName** ou **CommandPrefix**. Os usuários estão muito mais propensos a dizer "Ei Cortana" para invocar a Cortana por meio de ativação por voz, e "Hey \ [nome do aplicativo]" na expressão não parece natural. Por exemplo, "Ei Cortana", mostre minha próxima viagem para Las Vegas no Ei Adventure Works".

Considere a adição de variações de infixo/sufixo para os comandos de voz existentes. Como mostramos aqui, não é muito difícil adicionar mais um atributo aos elementos **ListenFor** existentes nem dar suporte a variações de sufixo. Parece muito mais natural dizer "Ei Cortana, mostre minha próxima viagem para Las Vegas no Adventure works" do que "Ei Cortana, Adventure Works, mostre minha próxima viagem para Las Vegas".

Considere usar o nome do aplicativo como prefixo quando o comando de voz estiver em conflito com a funcionalidade da **Cortana** existente (chamada, mensagens e assim por diante). Por exemplo, "Adventure Works, mensagem \[agente de viagem\] sobre viagem para Las Vegas".

## <span id="Complete_example"></span><span id="complete_example"></span><span id="COMPLETE_EXAMPLE"></span>Exemplo completo


Este é um arquivo VCD que demonstra várias formas de fornecer comandos de voz em linguagem mais natural.

**Observação**  Isso é válido para ter vários elementos **ListenFor**, cada um com um valor de atributo **RequireAppName** diferente. 

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="commandSet_en-us">
    <AppName>Adventure Works</AppName>
    <Example> When is my trip to Las Vegas? </Example>
    <Command Name="whenIsTripToDestination">
      <Example> When is my trip to Las Vegas?</Example>
      <ListenFor RequireAppName="BeforePhrase">
        when is my] trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands like 
           “Show my next trip to Las Vegas on Adventure Works”; “Show my next 
           trip to Las Vegas using Adventure Works” -->
      <ListenFor RequireAppName="AfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands when 
           the user specifies your app name either before or after the command. 
           “Adventure Works, show my next trip to Las Vegas”; 
           “Show my next trip to Last Vegas on Adventure works” -->
      <ListenFor RequireAppName="BeforeOrAfterPhrase">
        show [my] next trip to {destination} </ListenFor>

      <!-- This ListenFor command will set up Cortana to accept commands 
           when the user specifies your app name inline. 
           “Show my next Adventure Works trip to Las Vegas” -->
      <ListenFor RequireAppName="ExplicitlySpecified">
        show [my] next {builtin:AppName} trip to {destination} </ListenFor>

      <Feedback> Looking for trip to {destination} </Feedback>
      <Navigate />
    </Command>
    <PhraseList Label="destination">
      <Item> Las Vegas </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>
  </CommandSet>
  <!-- Other CommandSets for other languages -->
</VoiceCommands>
```

## <span id="related_topics"></span>Artigos relacionados


**Desenvolvedores**
* [Interações da Cortana](cortana-interactions.md)
* [Definir restrições de reconhecimento personalizadas](define-custom-recognition-constraints.md)
* [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Diretrizes para design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemplos**
* [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


