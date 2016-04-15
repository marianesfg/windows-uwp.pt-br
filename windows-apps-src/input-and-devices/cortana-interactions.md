---
Description: Estenda a funcionalidade básica da Cortana com comandos de voz que iniciam e executam uma única ação em um aplicativo externo.
title: Interações da Cortana
ms.assetid: 4C11A7CF-DA26-4CA1-A9B9-FE52670101F5
label: Cortana
template: detail.hbs
---

# Interações da Cortana em aplicativos UWP


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Estenda a funcionalidade básica da **Cortana** com comandos de voz que iniciam e executam uma única ação em um aplicativo externo. 


**Outros componentes de controle por voz**

-   Consulte [Diretrizes de design de controle por voz](speech-interactions.md) ao integrar reconhecimento de fala e conversão de texto em fala (também conhecida como TTS ou sintetização de voz) diretamente ao seu aplicativo.

> **Observação**  
> Um comando de voz é uma expressão única com um propósito específico, definido em um arquivo VCD (Definição de Comando de Voz), direcionado para um aplicativo instalado por meio da **Cortana**.

> Um arquivo VCD define um ou mais comandos de voz, cada um com um propósito exclusivo.

> A definição de um comando de voz pode variar em complexidade. Ela pode dar suporte a tudo, desde uma única expressão restrita até uma coleção de expressões mais flexíveis e de linguagem natural, todas indicando o mesmo propósito.


O aplicativo de destino pode ser iniciado em primeiro plano (o aplicativo recebe foco e a **Cortana** é ignorada) ou ativado em segundo plano (a **Cortana** retém o foco, mas fornece resultados do aplicativo), dependendo da complexidade da interação. Por exemplo, comandos de voz que exigem contexto adicional ou a entrada do usuário são mais bem manipulados em um aplicativo de primeiro plano, enquanto os comandos básicos podem ser manipulados na **Cortana** com aplicativo de segundo plano.

 

A integração da funcionalidade básica do seu aplicativo e o fornecimento de um ponto de entrada central para o usuário realizar a maioria das tarefas sem abrir o aplicativo diretamente permitem que a **Cortana** se torne uma ligação entre seu aplicativo e o usuário. Fornecer esse atalho para a funcionalidade do aplicativo e reduzir a necessidade de alternar entre aplicativos pode economizar tempo e esforço significativos do usuário.


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Artigo</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>[Design guidelines](cortana-design-guidelines.md)</p></td>
<td align="left"><p>Essas diretrizes e recomendações descrevem como seu aplicativo pode usar a **Cortana** da melhor forma para interagir com o usuário, ajudá-lo a realizar uma tarefa e comunicar claramente como tudo está sendo feito.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Launch a foreground app with voice commands](launch-a-foreground-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Além de usar comandos de voz na <strong>Cortana</strong> para acessar recursos do sistema, você também pode usar comandos de voz por meio da <strong>Cortana</strong> para iniciar um aplicativo em primeiro plano e especificar uma ação ou um comando para serem executados dentro do aplicativo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Dynamically modify VCD phrase lists](dynamically-modify-voice-command-definition--vcd--phrase-lists.md)</p></td>
<td align="left"><p>Saiba como acessar e atualizar a lista de frases com suporte (elementos <strong>PhraseList</strong>) em um arquivo VCD usando o resultado do reconhecimento de fala em tempo de execução.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Launch a background app with voice commands](launch-a-background-app-with-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Além de usar comandos de voz dentro da <strong>Cortana</strong> para acessar os recursos do sistema, você também pode estender a <strong>Cortana</strong> com recursos e funcionalidade de um aplicativo em segundo plano usando comandos de voz que especificam uma ação ou comando para execução dentro do aplicativo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Interact with a background app](interact-with-a-background-app-in-cortana.md)</p></td>
<td align="left"><p>Saiba como um usuário pode interagir com um aplicativo em segundo plano usando a voz e a tela da <strong>Cortana</strong> durante a execução de um comando de voz.</p></td>
</tr>
<tr class="even">
<td align="left"><p>[Deep link to a background app](deep-link-into-your-app-from-cortana.md)</p></td>
<td align="left"><p>Forneça links profundos do serviço de aplicativo em segundo plano na <strong>Cortana</strong> para iniciar o aplicativo em primeiro plano em um estado ou contexto específico.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>[Support natural language voice commands](support-natural-language-voice-commands-in-cortana.md)</p></td>
<td align="left"><p>Saiba como estender a <strong>Cortana</strong> com comandos de voz mais flexíveis e naturais, para que um usuário possa dizer o nome do seu aplicativo em qualquer lugar do comando.</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Artigos relacionados


* [**VCD elements and attributes v1.2**](https://msdn.microsoft.com/library/windows/apps/dn706593)

**Designers**
* [Diretrizes para design de controle por voz](https://msdn.microsoft.com/library/windows/apps/dn596121)
* [Diretrizes para design da Cortana](https://msdn.microsoft.com/library/windows/apps/dn974233)

**Exemplos**
* [Amostra de comando de voz da Cortana](http://go.microsoft.com/fwlink/p/?LinkID=619899)
 

 






<!--HONumber=Mar16_HO4-->


