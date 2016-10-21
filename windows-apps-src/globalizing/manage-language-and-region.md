---
author: DelfCo
Description: "Controle como o Windows seleciona os recursos da interface do usuário e formata os elementos da interface do usuário do aplicativo, usando as várias configurações de idioma e região fornecidas pelo Windows."
title: "Gerenciar idioma e região"
ms.assetid: 22D3A937-736A-4121-8285-A55DED56E594
label: Manage language and region
template: detail.hbs
translationtype: Human Translation
ms.sourcegitcommit: 59e02840c72d8bccda7e318197e4bf45ed667fa4
ms.openlocfilehash: 5a7519d9ea7a121e3e3087debba6d6193b1d8155

---

# Gerenciar idioma e região





**APIs importantes**

-   [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813)
-   [**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022)
-   [**Namespace WinJS.Resources**](https://msdn.microsoft.com/library/windows/apps/br229779)

Controle como o Windows seleciona os recursos da interface do usuário e formata os elementos da interface do usuário do aplicativo, usando as várias configurações de idioma e região fornecidas pelo Windows.

## <span id="Introduction"></span><span id="introduction"></span><span id="INTRODUCTION"></span>Introdução


Para conferir um exemplo de aplicativo que demonstre como gerenciar as configurações de idioma e região, consulte [Exemplos de recursos e localização de aplicativo](http://go.microsoft.com/fwlink/p/?linkid=231501).

Um usuário do Windows não precisa escolher apenas um idioma em um conjunto limitado de idiomas. Em vez disso, o usuário pode informar ao Windows que fala qualquer idioma, mesmo se o Windows não estiver traduzido para esse idioma. O usuário pode até especificar que fala vários idiomas.

O usuário do Windows pode especificar sua localidade, que pode ser qualquer lugar do mundo. Além disso, o usuário pode especificar que fala qualquer idioma de qualquer localidade. A localidade e o idioma não se limitam. Só porque o usuário fala francês não significa que ele está na França ou só porque o usuário está na França não significa que prefere falar em francês.

O usuário pode executar aplicativos em um idioma completamente diferente do Windows. Por exemplo, o usuário pode executar um aplicativo em espanhol enquanto o Windows é executado em inglês.

Para aplicativos da Windows Store, um idioma é representado como uma [marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302). A maioria das APIs de Windows Runtime, HTML e XAML podem retornar ou aceitar representações em cadeia de caracteres dessas marcas de idioma BCP-47. Consulte também a [lista IANA de idiomas](http://go.microsoft.com/fwlink/p/?linkid=227303).

Consulte [Idiomas com suporte](https://msdn.microsoft.com/library/windows/apps/jj657969) para obter a lista de marcas de idioma especificamente permitidas pela Windows Store.

## <span id="Tasks"></span><span id="tasks"></span><span id="TASKS"></span>Tarefas


### <span id="Users_can_set_their_language_preferences."></span><span id="users_can_set_their_language_preferences."></span><span id="USERS_CAN_SET_THEIR_LANGUAGE_PREFERENCES."></span>Os usuários podem definir suas preferências de idioma.

A lista de preferências de idioma do usuário é uma lista ordenada de idiomas que descreve os idiomas do usuário na ordem de sua preferência.

O usuário define a lista em **Configurações** &gt; **Hora e idioma** &gt; **Região e idioma**. Como alternativa, ele pode usar **Painel de Controle** &gt; **Relógio, Idioma e Região**.

A lista de preferências de idioma do usuário pode conter vários idiomas e variantes regionais ou outras específicas. Por exemplo, o usuário pode preferir fr-CA, mas também pode compreender en-GB.

### <span id="Specify_the_supported_languages_in_the_app_s_manifest."></span><span id="specify_the_supported_languages_in_the_app_s_manifest."></span><span id="SPECIFY_THE_SUPPORTED_LANGUAGES_IN_THE_APP_S_MANIFEST."></span>Especifique os idiomas com suporte no manifesto do aplicativo.

Especifique a lista de idiomas com suporte de seu aplicativo no arquivo de manifesto do aplicativo [**Resources element**](https://msdn.microsoft.com/library/windows/apps/dn934770) (normalmente Package.appxmanifest) ou o Visual Studio gera automaticamente a lista de idiomas no arquivo de manifesto com base nos idiomas encontrados no projeto. O manifesto deve descrever com precisão os idiomas com suporte no nível adequado de granularidade. Os idiomas listados no manifesto são aqueles exibidos aos usuários na Windows Store.

### <span id="Specify_the_default_language."></span><span id="specify_the_default_language."></span><span id="SPECIFY_THE_DEFAULT_LANGUAGE."></span>Especifique o idioma padrão.

Abra o package.appxmanifest no Visual Studio, vá para a guia **Aplicativo** e defina seu idioma padrão para o idioma que você usou para criar seu aplicativo.

Um aplicativo usa o idioma padrão quando não dá suporte a nenhum dos idiomas que o usuário escolheu. O Visual Studio usa o idioma padrão para adicionar metadados aos ativos marcados nesse idioma, permitindo que os ativos adequados sejam escolhidos em tempo de execução.

A propriedade do idioma padrão também deve ser definida como o primeiro idioma no manifesto para definir adequadamente o idioma do aplicativo (descrito na seguinte etapa "Criar a lista de idiomas de aplicativo"). Os recursos no idioma padrão ainda devem ser qualificados com seu idioma (por exemplo, en-US/logo.png). O idioma padrão não especifica o idioma implícito de ativos não qualificados. Para saber mais, veja [Como nomear recursos usando qualificadores](https://msdn.microsoft.com/library/windows/apps/xaml/hh965324).

### <span id="Qualify_resources_with_their_language."></span><span id="qualify_resources_with_their_language."></span><span id="QUALIFY_RESOURCES_WITH_THEIR_LANGUAGE."></span>Qualificar recursos com o respectivo idioma.

Avalie cuidadosamente seu público e o idioma e a localização dos usuários de destino. Muitas pessoas que vivem em uma região não preferem o idioma principal dessa região. Por exemplo, há milhões de residências no Brasil em que o idioma principal é o árabe, o italiano ou o japonês.

Ao qualificar recursos com idioma:

-   Inclua o script quando não houver valor de script de supressão definido para o idioma. Veja detalhes de marca de idioma no [Registro de submarca IANA](http://go.microsoft.com/fwlink/p/?linkid=227303). Por exemplo, use zh-Hant, zh-Hant-TW ou zh-Hans, em vez de zh-CN ou zh-TW.
-   Marcar todo o conteúdo linguístico com um idioma. A propriedade do projeto de idioma padrão não é o idioma de recursos não marcados (ou seja, o idioma neutro); ela especifica qual recurso de idioma marcado deve ser escolhido se nenhum outro recurso de idioma marcado corresponde ao usuário.

Marque ativos com uma representação precisa do conteúdo.

-   O Windows faz uma correspondência complexa, incluindo variantes regionais (que vão de en-US a en-GB, por exemplo), assim os aplicativos ficam livres para marcar ativos com uma representação ativa do conteúdo e permitir que o Windows tenha a correspondência apropriada para cada usuário.
-   A Windows Store exibe o conteúdo do manifesto aos usuários que veem o aplicativo.
-   Fique ciente de que algumas ferramentas e outros componentes como os tradutores automáticos podem encontrar marcas de idioma específicas, como informações de dialetos regionais, que são úteis para entender os dados.
-   Verifique se marcou os ativos com todos os detalhes, principalmente quando diversas variantes estão disponíveis. Por exemplo, marque en-GB e en-US se ambos forem específicos dessa região.
-   Para idiomas que têm um único dialeto padrão, não é necessário adicionar a região. A marcação geral é razoável em algumas situações, como a marcação de ativos com ja, em vez de ja-JP.

Há situações em que nem todos os recursos precisam ser localizados.

-   Para recursos como cadeias de caracteres de interface do usuário que vêm em todos os idiomas, marque-os com o idioma apropriado em que estão e verifique se eles dispõem de todos os recursos no idioma padrão. Não é necessário especificar um recurso neutro (um não marcado com um idioma).
-   Para recursos que vêm em um subconjunto do conjunto de idiomas de um aplicativo inteiro (localização parcial), especifique o conjunto dos idiomas em que os ativos vêm e verifique se todos dispõem desses recursos no idioma padrão. O Windows escolhe o melhor idioma possível para o usuário analisando todos os idiomas que o usuário fala em ordem de preferência. Por exemplo, nem toda a interface do usuário de um aplicativo pode ser localizada para catalão se o aplicativo tem um conjunto completo de recursos em espanhol. Para usuários que falam catalão e também espanhol, os recursos não disponíveis em catalão aparecem em espanhol.
-   Para recursos que têm exceções específicas em alguns idiomas e que todos os outros idiomas mapeiam para um recurso comum, o recurso que deve ser usado para todos os idiomas deve ser marcado com a marca de idioma indeterminada 'und'. O Windows interpreta a marca de idioma 'und' de maneira semelhante a um '\*', no sentido de que ele faz correspondência com o principal idioma do aplicativo após qualquer outra correspondência específica. Por exemplo, se alguns recursos (como a largura de um elemento) forem diferentes para finlandês, mas o restante dos recursos for o mesmo para todos os idiomas, o recurso do finlandês deverá ser marcado com a marca do idioma finlandês, e o restante deverá ser marcado com 'und'.
-   Para recursos que se baseiam no script de um idioma, em vez do idioma em si, como a fonte ou altura de um texto, use a marca de idioma indeterminado com um script especificado: 'und-&lt;script&gt;'. Por exemplo, para fontes latinas, use und-Latn\\fonts.css e para fontes cirílicas use und-Cryl\\fonts.css.

### <span id="Create_the_application_language_list."></span><span id="create_the_application_language_list."></span><span id="CREATE_THE_APPLICATION_LANGUAGE_LIST."></span>Crie a lista de idiomas de aplicativo.

Em tempo de execução, o sistema determina as preferências de idioma do usuário para as quais o aplicativo declara suporte em seu manifesto, além de criar *uma lista de idiomas do aplicativo*. Ele usa essa lista para determinar o(s) idioma(s) em que o aplicativo deve estar. A lista determina o(s) idioma(s) usado(s) para o aplicativo e os recursos do sistema, as datas, as horas e os números, além de outros componentes. Por exemplo, o Sistema de Gerenciamento de Recursos ([**Windows.ApplicationModel.Resources**](https://msdn.microsoft.com/library/windows/apps/br206022), [**Windows.ApplicationModel.Resources.Core**](https://msdn.microsoft.com/library/windows/apps/br225039) e [**namespace WinJS.Resources**](https://msdn.microsoft.com/library/windows/apps/br229779)) carrega os recursos da interface do usuário de acordo com o idioma do aplicativo. [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) também escolhe formatos com base na lista de idiomas do aplicativo. A lista de idiomas do aplicativo está disponível usando [**Windows.Globalization.ApplicationLanguages.Languages**](https://msdn.microsoft.com/library/windows/apps/hh972396).

A correspondência dos idiomas com os recursos é difícil. É recomendável delegar ao Windows a manipulação da correspondência porque há muitos componentes opcionais de uma marca de idioma que influenciam a prioridade de correspondência, e eles podem ser encontrados na prática.

Exemplos de componentes opcionais em uma marca de idioma são:

-   Script para idiomas de script de supressão. Por exemplo, en-Latn-US corresponde a en-US.
-   Região. Por exemplo, en-US corresponde a en.
-   Variantes. Por exemplo, de-DE-1996 corresponde a de-DE.
-   -x e outras extensões. Por exemplo, en-US-x-Pirate corresponde a en-US.

Também há muitos componentes de uma marca de idioma que não têm o formato xx ou xx-yy, e nem todos fazem correspondência.

-   zh-Hant não corresponde a zh-Hans.

O Windows prioriza a correspondência de idiomas de maneira padrão fácil de entender. Por exemplo, en-US corresponde, em ordem de prioridade, a en-US, en, en-GB e assim por diante.

-   O Windows faz correspondência regional cruzada. Por exemplo, en-US corresponde a en-US, depois com en e na sequência com en-\*.
-   O Windows tem dados adicionais para correspondência por afinidade em uma região, como a região principal de um idioma. Por exemplo, fr-FR é uma correspondência melhor para fr-BE do que para fr-CA.
-   Quaisquer melhorias futuras na correspondência de idiomas no Windows serão obtidas gratuitamente quando você depender de APIs do Windows.

A correspondência para o primeiro idioma de uma lista ocorre antes da correspondência do segundo idioma de uma lista, mesmo para outras variações regionais. Por exemplo, um recurso de en-GB será escolhido em detrimento do recurso fr-CA se o idioma do aplicativo for en-US. Somente se não houver recursos para um formato de en é que será escolhido um recurso para fr-CA.

A lista de idiomas do aplicativo é definida para a variação regional do usuário, mesmo se ela é diferente da variação regional que o aplicativo forneceu. Por exemplo, se o usuário falar en-GB, mas o aplicativo der suporte a en-US, a lista de idiomas do aplicativo incluirá en-GB. Isso garante que as datas, as horas e os números sejam formatados da maneira mais próxima às expectativas do usuário (en-GB), mas os recursos da interface do usuário ainda sejam carregados (devido à correspondência do idioma) no idioma do aplicativo com suporte (en-US).

A lista de idiomas do aplicativo é composta pelos seguintes itens:

1.  **Substituição do idioma principal (opcional)** O [**PrimaryLanguageOverride**](https://msdn.microsoft.com/library/windows/apps/hh972398) é uma configuração de substituição simples para aplicativos que permitirão aos usuários fazer sua própria escolha de idioma independente ou para aplicativos que, por motivos realmente importantes, substituirão as opções de idioma padrão. Para obter mais informações, consulte o [Exemplo de recursos e localização de aplicativos](http://go.microsoft.com/fwlink/p/?linkid=231501).
2.  **Os idiomas do usuário com suporte pelo aplicativo.** Esta é uma lista de preferências de idioma do usuário, em ordem de preferência de idioma. Ela é filtrada pela lista de idiomas suportados no manifesto do aplicativo. Filtrar os idiomas do usuário por aqueles suportados pelo aplicativo mantém a consistência entre os SDKs (software development kits), bibliotecas de classes, pacotes de estrutura dependente e o aplicativo.
3.  **Se 1 e 2 estão vazios, o padrão ou o primeiro idioma suportado pelo aplicativo.** Se o usuário não falar nenhum idioma com suporte pelo aplicativo, o idioma do aplicativo escolhido será o primeiro idioma suportado pelo aplicativo.

Veja a seção Comentários abaixo para obter exemplos.

### <span id="Set_the_HTTP_Accept_Language_header."></span><span id="set_the_http_accept_language_header."></span><span id="SET_THE_HTTP_ACCEPT_LANGUAGE_HEADER."></span>Defina o cabeçalho idioma aceito pelo HTTP.

As solicitações de HTTP feitas a partir dos aplicativos da Windows Store e aplicativos de área de trabalho em solicitações de Web típicas e XMLHttpRequest (XHR), use o cabeçalho HTTP Accept-Language padrão. Por padrão, o cabeçalho HTTP é definido nas preferências de idioma do usuário, na ordem preferida pelo usuário, conforme especificada em **Configurações** &gt; **Hora e idioma** &gt; **Região e idioma**. Cada idioma da lista é expandido ainda mais para incluir neutralidades de idioma e pesos (q). Por exemplo, a lista de idiomas de um usuário consistindo em fr-FR e en-US resulta em um cabeçalho HTTP Accept-Language de fr-FR, fr, en-US, en ("fr-FR,fr;q=0.8,en-US;q=0.5,en;q=0.3").

### <span id="Use_the_APIs_in_the_Windows.Globalization_namespace."></span><span id="use_the_apis_in_the_windows.globalization_namespace."></span><span id="USE_THE_APIS_IN_THE_WINDOWS.GLOBALIZATION_NAMESPACE."></span>Use as APIs no namespace Windows.Globalization.

Normalmente, os elementos da API no namespace [**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) usam a lista de idiomas do aplicativo para determinar o idioma. Se nenhum dos idiomas tiver um formato de correspondência, a localidade do usuário será usada. Esta é a mesma localidade usada no relógio do sistema. A localidade do usuário está disponível em **Configurações** &gt; **Hora e idioma** &gt; **Região e idioma** &gt; **Configurações adicionais de data, hora e regionais** &gt; **Região: Alterar formatos de data, hora ou número**. As APIs **Windows.Globalization** também aceitam uma substituição para especificar uma lista de idiomas a ser usada em vez da lista de idiomas do aplicativo.

[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) também tem um objeto [**Language**](https://msdn.microsoft.com/library/windows/apps/br206804) que é fornecido como um objeto auxiliar. Ele permite que os aplicativos inspecionem os detalhes sobre o idioma, como o script do idioma, o nome para exibição e o nome nativo.

### <span id="Use_geographic_region_when_appropriate."></span><span id="use_geographic_region_when_appropriate."></span><span id="USE_GEOGRAPHIC_REGION_WHEN_APPROPRIATE."></span>Use a região geográfica quando adequado.

Em vez do idioma, você pode usar a configuração local de região geográfica do usuário para escolher o conteúdo a ser exibido para o usuário. Por exemplo, um aplicativo de notícias pode exibir, por padrão, o conteúdo do local de residência do usuário, que é definido quando o Windows está instalado e disponível na interface do usuário do Windows em **Região: Alterar formatos de data, hora ou número** conforme descrito na tarefa anterior. Você pode recuperar a configuração da região de residência do usuário atual usando [**Windows.System.UserProfile.GlobalizationPreferences.HomeGeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br241829).

[**Windows.Globalization**](https://msdn.microsoft.com/library/windows/apps/br206813) também tem um objeto [**GeographicRegion**](https://msdn.microsoft.com/library/windows/apps/br206795) que é fornecido como um objeto auxiliar. Ele permite que os aplicativos inspecionem os detalhes sobre uma região específica, como o nome para exibição, o nome nativo e as moedas em uso.

## <span id="Remarks"></span><span id="remarks"></span><span id="REMARKS"></span>Comentários


A seguinte tabela contém exemplos do que o usuário veria na interface do usuário do aplicativo em várias configurações de idioma e região.

<table border="1">
<colgroup>
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
<col width="20%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Idiomas permitidos pelo aplicativo (definidos no manifesto)</th>
<th align="left">Preferências de idioma do usuário (definida no painel de controle)</th>
<th align="left">Substituição do idioma principal do aplicativo (opcional)</th>
<th align="left">Idiomas do aplicativo</th>
<th align="left">O que o usuário vê no aplicativo</th>
</tr>
</thead>
<tbody>
<tr>
<td align="left">Inglês (GB) (padrão); Alemão (Alemanha)</td>
<td align="left">Inglês (GB)</td>
<td align="left">nenhum(a)</td>
<td align="left">Inglês (GB)</td>
<td align="left">UI: Inglês (GB)<br>Datas/horas/números: Inglês (GB)</td>
</tr>
<tr>
<td align="left">Alemão (Alemanha) (padrão); Francês (França); Italiano (Itália)</td>
<td align="left">Francês (Áustria)</td>
<td align="left">nenhum(a)</td>
<td align="left">Francês (Áustria)</td>
<td align="left">Interface do usuário: Francês (França) (fallback do francês (Áustria))<br>Datas/horas/números: Francês (Áustria)</td>
</tr>
<tr>
<td align="left">Inglês (EUA) (padrão); Francês (França); Inglês (Reino Unido)</td>
<td align="left">Inglês (Canadá); Francês (Canadá)</td>
<td align="left">nenhum(a)</td>
<td align="left">Inglês (Canadá); Francês (Canadá)</td>
<td align="left">Interface do usuário: Inglês (EUA) (fallback do inglês (Canadá))<br>Datas/horas/números: Inglês (Canadá)</td>
</tr>
<tr>
<td align="left">Espanhol (Espanha) (padrão); Espanhol (México); Espanhol (América Latina); Português (Brasil)</td>
<td align="left">Inglês (EUA)</td>
<td align="left">nenhum(a)</td>
<td align="left">Espanhol (Espanha)</td>
<td align="left">Interface do usuário: Espanhol (Espanha) (usa o padrão visto que nenhum fallback está disponível para inglês)<br>Datas/horas/números: Espanhol (Espanha)</td>
</tr>
<tr>
<td align="left">Catalão (padrão); Espanhol (Espanha); Francês (França)</td>
<td align="left">Catalão; Francês (França)</td>
<td align="left">nenhum(a)</td>
<td align="left">Catalão; Francês (França)</td>
<td align="left">Interface do usuário: principalmente o catalão e algum francês (França), porque nem todas as cadeias de caracteres estão em catalão<br>Datas/horas/números: catalão</td>
</tr>
<tr>
<td align="left">Inglês (Reino Unido) (padrão); Francês (França); Alemão (Alemanha)</td>
<td align="left">Alemão (Alemanha); Inglês (Reino Unido)</td>
<td align="left">Inglês (Reino Unido) (escolhido pelo usuário na interface do usuário do aplicativo)</td>
<td align="left">Inglês (Reino Unido); Alemão (Alemanha)</td>
<td align="left">Interface do usuário: Inglês (GB) (substituição de idioma)<br>Datas/horas/números: Inglês (GB)</td>
</tr>
</tbody>
</table>

 

## <span id="related_topics"></span>Tópicos relacionados


* [Marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)
* [Lista IANA de idiomas](http://go.microsoft.com/fwlink/p/?linkid=227303)
* [Exemplo de recursos e localização de aplicativos](http://go.microsoft.com/fwlink/p/?linkid=231501)
* [Idiomas com suporte](https://msdn.microsoft.com/library/windows/apps/jj657969)
 

 






<!--HONumber=Aug16_HO3-->


