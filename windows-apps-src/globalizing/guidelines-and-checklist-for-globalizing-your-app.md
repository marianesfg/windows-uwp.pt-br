---
author: DelfCo
Description: "Siga estas práticas recomendadas ao globalizar seus aplicativos para um público-alvo maior e ao localizar seus aplicativos para um mercado específico."
Search.Refinement.TopicID: 180
title: "Diretrizes de globalização e localização"
ms.assetid: 0342DC3F-DDD1-4DD4-872E-A4EC340CAE79
label: Do's and don'ts
template: detail.hbs
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: bdbe6b3e319aa90a78660c664f1603bac93399ca

---

# O que fazer e o que não fazer em globalização e localização





**APIs importantes**

-   [**Globalização**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Globalization.NumberFormatting**](https://msdn.microsoft.com/library/windows/apps/br226136)
-   [**Globalization.DateTimeFormatting**](https://msdn.microsoft.com/library/windows/apps/br206859)
-   [**Recursos**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039)

Siga estas práticas recomendadas ao globalizar seus aplicativos para um público-alvo maior e ao localizar seus aplicativos para um mercado específico.



## <span id="guidelines_for_internationalization"></span><span id="GUIDELINES_FOR_INTERNATIONALIZATION"></span>Globalização

Prepare seu aplicativo para adaptar-se a diferentes mercados escolhendo termos e imagens globalmente apropriados para sua interface de usuário, usando APIs [**Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) para formatar dados de aplicativos e evitar suposições com base em localização ou idioma.

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Recomendação</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Use os formatos corretos para números, datas, horas e números de telefone.</p></td>
<td align="left"><p>A formatação usada para números, datas, horas e outras formas de dados variam entre culturas, regiões, idiomas e mercados. Se você estiver exibindo números, datas, horas ou outros dados, use APIs [<strong>Globalização</strong>](https://msdn.microsoft.com/library/windows/apps/br206813) para obter o formato adequado para um público-alvo específico.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Permita tamanhos internacionais de papel.</p></td>
<td align="left"><p>Os tamanhos mais comuns de papel diferem entre os países, portanto se você incluir recursos que dependam do tamanho do papel, como impressão, assegure-se de permitir e testar tamanhos internacionais comuns.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Ofereça suporte às unidades de medidas e moedas internacionais.</p></td>
<td align="left"><p>Diferentes unidades e escalas são usadas em diferentes países, embora as mais populares sejam o sistema métrico e o sistema imperial. Se você lidar com medições, como comprimento, temperatura ou área, obtenha a medição correta do sistema usando a propriedade [<strong>CurrenciesInUse</strong>](https://msdn.microsoft.com/library/windows/apps/br206793).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Exiba o texto e as fontes corretamente.</p></td>
<td align="left"><p>A fonte ideal, o tamanho da fonte e a direção do texto variam entre diferentes mercados.</p>
<p>Para obter mais informações, consulte [<strong>Ajustar o layout e as fontes e dar suporte a RTL</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Use Unicode para a codificação de caracteres.</p></td>
<td align="left"><p>Por padrão, as versões recentes do Microsoft Visual Studio usam a codificação de caracteres Unicode para todos os documentos. Se você estiver usando um editor diferente, assegure-se de salvar os arquivos de origem nas codificações de caracteres do Unicode adequadas. Todas as APIs do Windows Runtime retornarão cadeias de caracteres codificadas UTF-16.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Registre o idioma de entrada.</p></td>
<td align="left"><p>Quando o aplicativo solicitar aos usuários entrada de texto, registre o idioma de entrada. Isso garante que quando a entrada for exibida posteriormente ela será apresentada ao usuário com a formatação adequada. Use a propriedade [<strong>CurrentInputMethodLanguage</strong>](https://msdn.microsoft.com/library/windows/apps/hh700658) para obter o idioma de entrada atual.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Não use o idioma para pressupor a localidade de um usuário, nem a localidade para pressupor o idioma do usuário.</p></td>
<td align="left"><p>No Windows, o idioma e a localidade do idioma são conceitos separados. Um usuário pode falar uma variação regional específica de um idioma, como en-gb para inglês como falado na Grã-Bretanha, mas o usuário pode estar em um país ou região completamente diferente. Considere se os aplicativos requerem conhecimento sobre o idioma do usuário, como para texto da interface do usuário ou localidade, como para problemas de licenciamento.</p>
<p>Para obter mais informações, consulte [<strong>Gerenciar idioma e região</strong>](manage-language-and-region.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Não use coloquialismos e metáforas.</p></td>
<td align="left"><p>O idioma que é específico para um grupo demográfico, como cultura e idade, pode ser difícil de compreender ou traduzir, porque só as pessoas daquele grupo demográfico usam aquele idioma. Da mesma forma, as metáforas podem fazer sentido para uma pessoa, mas não significar nada para outra pessoa. Por exemplo, um &quot;meu&quot; é algo específico da parte de uma cultura local, mas outros que não pertençam àquela cultura podem não entender a expressão. Se você planeja localizar seu aplicativo e usar uma voz ou tom informal, explique adequadamente para os tradutores o significado e a voz a ser usada.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Não use jargão técnico, abreviações ou acrônimos.</p></td>
<td align="left"><p>É menos provável que a linguagem técnica seja compreendida pelos públicos que não são técnicos ou pessoas de outras culturas ou regiões, além disso, é difícil de traduzir. As pessoas não usam esses tipos de palavras na comunicação cotidiana. A linguagem técnica normalmente aparece em mensagens de erro para identificar problemas de hardware e software. Às vezes, isso pode ser necessário, mas você deve reescrever as cadeias de caracteres para não serem técnicas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Não use imagens que possam ser ofensivas.</p></td>
<td align="left"><p>As imagens que podem ser adequadas em sua própria cultura podem ser ofensivas e mal interpretadas em outras culturas. Evite o uso de símbolos religiosos, animais ou combinações de cores que sejam associadas a bandeiras nacionais ou movimentos políticos.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Evite ofensas políticas em mapas ou quando estiver se referindo a regiões.</p></td>
<td align="left"><p>Os mapas podem incluir regiões controversas ou limites nacionais e são uma fonte frequente de ofensas políticas. Tome cuidado para que nenhuma interface de usuário usada para selecionar uma nação se refira a ela como um &quot;país/região&quot;. Colocar um território em disputa em uma lista rotulada como &quot;Países&quot;, como em um formulário de endereço, pode trazer problemas para você.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Não use somente comparação de cadeia de caracteres para comparar marcas de idiomas.</p></td>
<td align="left"><p>As marcas de idioma BCP-47 são complexas. Há uma série de problemas para comparar marcas de idioma, incluindo problemas para corresponder informações de script, marcas herdadas e diversas variações regionais. O sistema de gerenciamento de recursos no Windows cuida da correspondência para você. Você pode especificar um conjunto de recursos em quaisquer idiomas, e o sistema escolhe o idioma adequado para o usuário e para o aplicativo.</p>
<p>Para obter mais informações sobre gerenciamento de recursos, consulte [<strong>Definindo os recursos do aplicativo</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/hh965321).</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Não pressuponha que a classificação sempre será em ordem alfabética.</p></td>
<td align="left"><p>Para idiomas que não são latinos, a classificação é baseada em fatores como pronúncia, número de traços de caneta e outros. Mesmo os idiomas que são latinos, nem sempre usam a classificação em ordem alfabética. Por exemplo, em algumas culturas, um catálogo telefônico talvez não seja classificado em ordem alfabética. O sistema pode cuidar da classificação, mas se você criar seu próprio algoritmo de classificação, assegure-se de levar em conta os métodos de classificação usados em seus mercados de destino.</p></td>
</tr>
</tbody>
</table>

 

## <span id="guidelines_for_localization"></span><span id="GUIDELINES_FOR_LOCALIZATION"></span>Localização

<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Recomendação</th>
<th align="left">Descrição</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><p>Separa recursos como cadeias de caracteres da interface do usuário e imagens do código.</p></td>
<td align="left"><p>Projete seus aplicativos de forma que os recursos, como cadeias de caracteres e imagens, fiquem separados de seu código. Isso permite que eles sejam mantidos, localizados e personalizados de maneira independente para diferentes fatores de dimensionamento, opções de acessibilidade e uma série de outros contextos de usuários e computadores.</p>
<p>Separe recursos de cadeias de caracteres de seu código de aplicativo para criar uma única base de código independente do idioma. Sempre separe cadeias de caracteres do código e da marcação do aplicativo e coloque-as em um arquivo de recursos, como um arquivo ResW ou ResJSON.</p>
<p>Use a infraestrutura de recursos do Windows para manipular a seleção dos recursos mais adequados para corresponder ao ambiente do tempo de execução do usuário.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Isole outros arquivos de recursos localizáveis.</p></td>
<td align="left"><p>Pegue outros arquivos que exijam localização, como imagens que contenham texto a ser traduzido ou que precisem ser modificados devido a uma questão cultural e os coloque em pastas marcadas com nomes de idiomas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Defina o idioma padrão e marque todos os recursos, mesmo aqueles que estejam no idioma padrão.</p></td>
<td align="left"><p>Sempre defina o idioma padrão de seus aplicativos adequadamente no manifesto do aplicativo (package.appxmanifest). O idioma padrão determina o idioma que é usado quando o usuário não controle por voz nenhum dos idiomas aceitos pelo aplicativo. Marque os recursos do idioma padrão, por exemplo en-us/Logo.png, com o idioma dos recursos, assim o sistema pode saber em qual idioma o recurso está e como ele é usado em situações específicas.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Determine os recursos de seu aplicativo que precisam ser localizados.</p></td>
<td align="left"><p>O que deverá ser modificado se o aplicativo precisar ser localizado para outros mercados? As cadeias de caracteres de texto precisam ser traduzidas para outros idiomas. Talvez seja necessário adaptar as imagens para outras culturas. Considere como a localização afeta outros recursos que seu aplicativo usa, como áudio ou vídeo.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Use os identificadores de recursos no código e na marcação para fazer referência aos recursos.</p></td>
<td align="left"><p>Em vez de ter literais de cadeia de caracteres ou nomes específicos de arquivos para imagens em sua marcação, use referências para os recursos. Use identificadores exclusivos para cada recurso. Para obter mais informações, consulte [<strong>Como nomear recursos usando qualificadores</strong>](https://msdn.microsoft.com/library/windows/apps/xaml/Hh965324).</p>
<p>Escute eventos que disparam quando o sistema muda e começa a usar um conjunto diferente de qualificadores. Reprocesse o documento para que os recursos corretos sejam carregados.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Permita que o tamanho do texto seja expandido.</p></td>
<td align="left"><p>Aloque os buffers de texto dinamicamente, visto que o tamanho do texto pode aumentar quando traduzido. Se você tiver que usar buffers estáticos, os crie bem maiores (talvez dobrando o tamanho da cadeia de caracteres em português) para acomodar a possível expansão das cadeias de caracteres depois de traduzidas. Também pode existir um espaço limitado disponível para uma interface do usuário. Para acomodar os idiomas localizados, assegure-se de que a extensão da cadeia de caracteres fique aproximadamente 40% mais longa do que seria necessário para o idioma português. Para cadeias de caracteres realmente curtas, como uma única palavra, você pode precisar até 300% de espaço a mais. Além disso, habilitar suporte multilinha e quebra de texto em um controle permitirá mais espaço para exibir cada cadeia de caracteres.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Permita espelhamento.</p></td>
<td align="left"><p>O alinhamento do texto e a ordem de leitura pode ser da esquerda para a direita, como em português, ou da direita para a esquerda (RTL), como em árabe ou hebraico. Se você estiver localizando seu produto em vários idiomas que usem uma ordem de leitura diferente do seu idioma, assegure-se de que o layout dos elementos da interface do usuário permitam espelhamento. Até itens como botões Voltar, efeitos de transição da interface do usuário e imagens podem ser espelhados.</p>
<p>Para obter mais informações, consulte [<strong>Ajustar o layout e as fontes e dar suporte a RTL</strong>](adjust-layout-and-fonts--and-support-rtl.md).</p></td>
</tr>
<tr class="even">
<td align="left"><p>Comente as cadeias de caracteres.</p></td>
<td align="left"><p>Verifique se as cadeias de caracteres estão comentadas adequadamente e que somente as cadeias de caracteres que precisam ser traduzidas sejam fornecidas para os localizadores. Localização excessiva é uma fonte de problemas comuns.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Use cadeias de caracteres curtas.</p></td>
<td align="left"><p>Cadeias de caracteres mais curtas são mais fáceis de traduzir e habilitar a reciclagem da tradução. A reciclagem da tradução economiza dinheiro, porque a mesma cadeia de caracteres não é enviada para o tradutor duas vezes.</p>
<p>As cadeias de caracteres com mais de 8192 caracteres podem não ser suportadas pelas mesmas ferramentas de localização, portanto, mantenha seu tamanho em até 4000.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Forneça cadeias de caracteres que contenham uma sentença inteira.</p></td>
<td align="left"><p>Forneça cadeias de caracteres que contenham uma sentença inteira, em vez de dividir a sentença em palavras, porque a tradução de palavras poderá depender de sua posição em uma sentença. Além disso, não pressuponha que uma frase com vários parâmetros manterá esse parâmetros na mesma ordem em todos os idiomas.</p></td>
</tr>
<tr class="odd">
<td align="left"><p>Otimize arquivos de imagem e áudio para localização.</p></td>
<td align="left"><p>Reduza os custos de localização evitando usar texto em imagens ou fala em arquivos de áudio. Se você estiver localizando para um idioma com ordem de leitura diferente do seu idioma, o uso de imagens simétricas e efeitos facilitam o suporte a espelhamento.</p></td>
</tr>
<tr class="even">
<td align="left"><p>Não reutilize as cadeias de caracteres em diferentes contextos.</p></td>
<td align="left"><p>Não reutilize cadeias de caracteres em diferentes contextos, porque até mesmos palavras simples como &quot;ativado&quot; e &quot;desativado&quot;" podem ser traduzidas de formas diferentes, dependendo do contexto.</p></td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Artigos relacionados


**Exemplos**
* [Amostra de recursos e localização de aplicativos](http://go.microsoft.com/fwlink/p/?linkid=254478)
* [Amostra de preferências de globalização](http://go.microsoft.com/fwlink/p/?linkid=231608)
 

 






<!--HONumber=Jun16_HO4-->


