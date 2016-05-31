---
author: Karl-Bridge-Microsoft
Description: Saiba como acessar e atualizar a lista de frases com suporte (elementos PhraseList) em um arquivo VCD (Definição de Comando de Voz) usando o resultado do reconhecimento de fala em tempo de execução.
title: Modificar dinamicamente listas de frases VCD
ms.assetid: 98024EAC-EC0E-44AA-AEC5-A611BA7C5884
label: Modify VCD phrase lists
template: detail.hbs
---

# Modificar dinamicamente listas de frases VCD





**APIs importantes**

-   [**Windows.ApplicationModel.VoiceCommands**](https://msdn.microsoft.com/library/windows/apps/dn706594)
-   [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

Acesse e atualize a lista de frases com suporte (elementos **PhraseList**) em um arquivo VCD (Definição de Comando de Voz) em tempo de execução usando o resultado do reconhecimento de fala.

Modificar dinamicamente uma lista de frases em tempo de execução é útil se o comando de voz for específico para uma tarefa que envolve algum tipo de dados de aplicativos definidos pelo usuário ou transitórios. 

Por exemplo, digamos que você tenha um aplicativo de viagem onde os usuários podem inserir destinos. Você deseja que os usuários possam iniciar o aplicativo dizendo o nome do aplicativo seguido por "Mostrar viagem para &lt;destino&gt;". No próprio elemento **ListenFor**, você especificaria algo como: `<ListenFor> Show trip to {destination}  </ListenFor>`, em que "destination" é o valor do atributo **Label** do elemento **PhraseList**.

Atualizar a lista de frases em tempo de execução elimina a necessidade de criar um elemento **ListenFor** separado para cada destino possível. Em vez disso, você pode preencher dinamicamente o elemento **PhraseList** com destinos especificados pelo usuário à medida que ele insere seus itinerários. 

Para saber mais sobre **PhraseList** e outros elementos VCD, consulte a referência de [**elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593).

**Pré-requisitos:  **

Este tópico complementa [Iniciar um aplicativo em primeiro plano com comando de voz na Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md). Continuaremos demonstrando recursos com um aplicativo de planejamento e gerenciamento de viagens chamado **Adventure Works**.

Se você for iniciante no desenvolvimento de aplicativos da Plataforma Universal do Windows (UWP), consulte estes tópicos para familiarizar-se com as tecnologias discutidas aqui.

-   [Criar seu primeiro aplicativo](https://msdn.microsoft.com/library/windows/apps/bg124288)
-   Saiba mais sobre eventos com [Visão geral de eventos e eventos roteados](https://msdn.microsoft.com/library/windows/apps/mt185584)

**Diretrizes de experiência do usuário:  **

Consulte as [Diretrizes para design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233), para obter informações sobre como integrar seu aplicativo à **Cortana** e as [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121) para obter dicas úteis para criar um aplicativo interessante e prático, habilitado para controle por voz.

## <span id="Identify_the_command"></span><span id="identify_the_command"></span><span id="IDENTIFY_THE_COMMAND"></span>Identificar o comando e atualizar a lista de frases

Veja a seguir um exemplo de arquivo VCD que define um **Comando** "showTripToDestination" e um elemento **PhraseList** que define três opções de destino no nosso aplicativo de viagens da **Adventure Works**. Conforme o usuário salva e exclui destinos no aplicativo, este atualiza as opções em **PhraseList**.

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

  </CommandSet>

<!-- Other CommandSets for other languages -->

</VoiceCommands>

```

Para atualizar um elemento **PhraseList** no arquivo VCD, obtenha o elemento **CommandSet** que contém a lista de frases. Use o atributo **Name** desse elemento **CommandSet** (**Name** deve ser exclusivo no arquivo VCD) como uma chave para acessar a propriedade [**VoiceCommandManager.InstalledCommandSets**](https://msdn.microsoft.com/library/windows/apps/dn653257) e obter a referência [**VoiceCommandSet**](https://msdn.microsoft.com/library/windows/apps/dn653258).

Depois de ter identificado o conjunto de comandos, obtenha uma referência à lista de frases que você deseja modificar e chame o método [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261). Use o atributo **Label** do elemento **PhraseList** e uma matriz de cadeias de caracteres como o novo conteúdo da lista de frases.

**Observação**  Se você modificar uma lista de frases, todo o seu conteúdo será substituído. Se quiser inserir novos itens em uma lista de frases, você deverá especificar tanto os itens existentes quanto os novos itens na chamada para [**SetPhraseListAsync**](https://msdn.microsoft.com/library/windows/apps/dn653261).

Neste exemplo, atualizamos o elemento **PhraseList** mostrado no exemplo anterior com um destino adicional para Phoenix.

```CSharp
Windows.ApplicationModel.VoiceCommands.VoiceCommnadDefinition.VoiceCommandSet commandSetEnUs;

if (Windows.ApplicationModel.VoiceCommands.VoiceCommandDefinitionManager.
      InstalledCommandSets.TryGetValue(
        "AdventureWorksCommandSet_en-us", out commandSetEnUs))
{
  await commandSetEnUs.SetPhraseListAsync(
    "destination", new string[] {“London”, “Dallas”, “New York”, “Phoenix”});
}
```

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Comentários


É apropriado usar um **PhraseList** para restringir o reconhecimento em um conjunto relativamente pequeno ou em algumas palavras. Quando o conjunto de palavras é muito grande (centenas de palavras, por exemplo) ou não deve ser restrito, use o elemento **PhraseTopic** e um elemento **Subject** para refinar a relevância dos resultados do reconhecimento de fala, melhorando a escalabilidade.

No nosso exemplo, temos um elemento **PhraseTopic** com um **Scenario** de "Search", restringido ainda mais por um **Subject** de "City\\State".

```XML
<?xml version="1.0" encoding="utf-8"?>
<VoiceCommands xmlns="http://schemas.microsoft.com/voicecommands/1.1">
  <CommandSet xml:lang="en-us" Name="AdventureWorksCommandSet_en-us">
    <CommandPrefix> Adventure Works, </CommandPrefix>
    <Example> Show trip to London </Example>

    <Command Name="showTripToDestination">
      <Example> show trip to London  </Example>
      <ListenFor> show trip to {destination} </ListenFor>
      <Feedback> Showing trip to {destination} </Feedback>
      <Navigate/>
    </Command>

    <PhraseList Label="destination">
      <Item> London </Item>
      <Item> Dallas </Item>
      <Item> New York </Item>
    </PhraseList>

    <PhraseTopic Label="destination" Scenario="Search">
      <Subject>City/State</Subject>
    </PhraseTopic>

  </CommandSet>
```

## <span id="related_topics"></span>Artigos relacionados


**Desenvolvedores**
* [Interações da Cortana](cortana-interactions.md)
* [Iniciar um aplicativo em primeiro plano com comandos de voz na Cortana](launch-a-foreground-app-with-voice-commands-in-cortana.md)
* [Iniciar um aplicativo em segundo plano com comandos de voz na Cortana](launch-a-background-app-with-voice-commands-in-cortana.md)
* [**Elementos e atributos VCD v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Diretrizes para design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)

**Exemplos**
* [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=May16_HO2-->


