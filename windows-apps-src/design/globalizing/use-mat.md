---
author: stevewhims
Description: The Multilingual App Toolkit (MAT) 4.0 integrates with Microsoft Visual Studio 2017 to provide UWP apps with translation support, translation file management, and editor tools.
title: Use o Kit de Ferramentas de Aplicativo Multilíngue
template: detail.hbs
ms.author: stwhi
ms.date: 01/23/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 39e002247cabb6389ddf23860499ebf1f166b03a
ms.sourcegitcommit: 8e30651fd691378455ea1a57da10b2e4f50e66a0
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/10/2018
ms.locfileid: "4498450"
---
# <a name="use-the-multilingual-app-toolkit-40"></a>Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0

O Kit de Ferramentas de Aplicativo Multilíngue (MAT) 4.0 se integra ao Microsoft Visual Studio 2017 para fornecer aplicativos UWP com suporte a tradução, gerenciamento de arquivos de tradução e ferramentas de edição. Aqui estão algumas das propostas de valor do kit de ferramentas.

- Ajuda você a gerenciar as alterações de recursos e o status da tradução durante o desenvolvimento.
- Fornece uma IU para escolha de idiomas com base nos provedores de tradução configurados.
- Dá suporte ao formato de arquivo XLIFF que é padrão do mercado de localização.
- Fornece um mecanismo de pseudoidioma que ajuda a identificar problemas de tradução durante o desenvolvimento.
- Conecta o Portal de Idiomas Microsoft para acessar facilmente as cadeias de caracteres traduzidas e a terminologia.
- Conecta-se ao Microsoft Translator para obter rápidas sugestões de tradução.

## <a name="how-to-use-the-toolkit"></a>Como usar o kit de ferramentas

### <a name="step-1-design-your-app-for-globalization-and-localization"></a>Etapa 1. Projete seu app para globalização e localização

Antes de usar efetivamente o MAT, seu app precisa estar localizável. Especificamente, o seu projeto deve conter um ou mais Arquivos de Recursos (.resw) com cadeias de caracteres do seu app no idioma padrão. Para mais informações, consulte [Localizar as cadeias de caracteres em seu IU e o manifesto do pacote do aplicativo](../../app-resources/localize-strings-ui-manifest.md). Depois disso, o kit de ferramentas torna a inclusão de idiomas adicionais rápida e fácil.

Para a proposta de valor de globalização e localização&mdash;, bem como as definições dos termos **globalização**, **localizabilidade**, e **localização**&mdash;, consulte [Globalização e localização ](globalizing-portal.md).

Consulte também [Diretrizes de globalização](guidelines-and-checklist-for-globalizing-your-app.md) e [Tornar seu app localizável](prepare-your-app-for-localization.md).

### <a name="step-2-download-and-install-the-multilingual-app-toolkit-40"></a>Etapa 2. Baixe e instale o Kit de Ferramentas de Aplicativo Multilíngue 4.0

Há duas partes do Kit de Ferramentas de Aplicativo Multilíngue 4.0 (MAT 4.0), cada uma com seu próprio instalador.

- [Kit de Ferramentas de Aplicativo Multilíngue Extensão 4.0 Extension para o Visual Studio 2017](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308). Ele contém a extensão MAT 4.0 para o Visual Studio 2017, na forma de instalador .vsix.
- [Editor do Kit de Ferramentas de Aplicativo Multilíngue 4.0](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit). Ele contém a ferramenta Editor Multilíngue autônomo MAT 4.0, na forma de instalador .msi. Ele também inclui a extensão MAT 4.0 para o Visual Studio 2015 e para o Visual Studio 2013.

Se você usa o Visual Studio 2017, baixe e execute a ambos os instaladores, um após o outro. Se você usa o Visual Studio 2015 ou o Visual Studio 2013, baixe e execute o instalador .msi.

### <a name="step-3-enable-the-multilingual-app-toolkit-for-your-project"></a>Etapa 3. Habilite o Kit de Ferramentas de Aplicativo Multilíngue para o seu projeto

O MAT precisa estar habilitado para o projeto antes de se poder iniciar a localização do app. Veja como habilitar o kit de ferramentas.

- Abra o projeto de solução no Visual Studio.
- Selecione o projeto desejado no Gerenciador de Soluções.
- No menu **Ferramentas**, selecione **Kit de Ferramentas de Aplicativo Multilíngue** > **Habilitar seleção**. 

Na janela de saída (mostrando a saída do Kit de Ferramentas de Aplicativo Multilíngue), aguarde a mensagem `Project '<project-name>' was enabled. The project's source culture is '<language-tag>' <language-name>`. Se a mensagem for exibida, o MAT está pronto para uso.

### <a name="step-4-add-languages-to-your-project"></a>Etapa 4. Adicionar idiomas ao projeto

Siga estas etapas para adicionar idiomas ao seu projeto.

1. Clique com o botão direito no nó do projeto, em Gerenciador de Soluções.
2. Clique em **Kit de Ferramentas de Aplicativo Multilíngue** > **Adicionar idiomas de tradução...**.
3. No diálogo Idiomas de Tradução, selecione o(s) idioma(s) aos quais deseja oferecer suporte e clique em OK.

O kit de ferramentas faz isso em resposta.

- Para cada idioma que você adicionou, uma nova pasta é criada nomeada de acordo com a [marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302) do idioma. Dentro dessa pasta, novos Arquivos de Recursos (.resw) são criados para corresponder àqueles que contêm as cadeias de caracteres no idioma padrão.
- Se esta for a primeira vez que você adicionou um idioma, uma nova pasta chamada `MultilingualResources` será adicionada ao projeto. Dentro dessa pasta, um arquivo .xlf será adicionado a cada idioma. Os arquivos .xlf contêm uma unidade de conversão para cada cadeia de caracteres em cada Arquivo de Recursos (.resw) do seu projeto.
- A janela de Saída confirma a inclusão do(s) idioma(s) que você adicionou.

Sempre que você adicionar/remover um Arquivo de Recursos (.resw) no idioma padrão ou adicionar/remover uma cadeia de caracteres em um Arquivo de Recursos (.resw) no idioma padrão, faça a compilação do projeto novamente para ressincronizar os arquivos .xlf. Isso garante que os arquivos .xlf contenham a união das cadeias de caracteres no idioma padrão.

Os provedores de tradução instalados&mdash; como os serviços do [Portal de Idiomas Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=330295) e [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)&mdash; podem ser usados para traduzir os recursos do seu app. Quando um provedor dá suporte a um idioma específico, o ícone do provedor é exibido próximo do nome do idioma no diálogo Idiomas de Tradução.

Na caixa de diálogo Idiomas de Tradução, todo idioma baseado em .xlf existente detectável pelo kit de ferramentas terá a caixa de seleção pré-marcada para indicar que o idioma já está incluído no projeto.

Depois de adicionado ao projeto, o idioma não pode ser removido desmarcando a caixa no diálogo Idiomas de Tradução. Para remover um idioma, clique com o botão direito do mouse no arquivo .xlf do idioma específico e escolha **Excluir**. Se você confirmar, isso também apagará o Arquivo de Recursos (.resw) correspondente.

### <a name="step-5-test-your-app-using-pseudo-language"></a>Etapa 5. Teste seu app usando o pseudoidioma

Pseudoidioma é uma modificação artificial do produto de software destinada a simular a localização de idiomas reais, mas que permanece legível aos falantes nativos. O pseudoidioma substitui caracteres e expande o comprimento da cadeia do recurso para detectar potenciais problemas ou bugs de localização no início do ciclo do projeto, antes de a localização começar oficialmente.

Siga estas etapas para pseudolocalizar e testar seu projeto.

1. Use a caixa de diálogo Idiomas de Tradução para adicionar o Pseudoidioma (Pseudo) [qps-ploc] ao seu projeto.
2. Clique com botão direito no arquivo `<project-name>.qps-ploc.xlf` do Gerenciador de Soluções e clique em **Kit de Ferramentas de Aplicativo Multilíngue** > **Gerar Traduções Automáticas**.
3. Em **Configurações** > **Hora e idioma** > **Região e idioma** > **Idiomas**, clique em **Adicionar um idioma**.
5. Na caixa de pesquisa, digite `qps-ploc`.
6. Clique em `English (qps-ploc)` para adicionar.
7. Na lista de idiomas, selecione `English (qps-ploc)` e clique em **Definir como padrão**.
8. Teste seu app pseudolocalizado. Por exemplo, procure por problemas de layout de interface do usuário em que nem todas as cadeias de caracteres sejam exibidas (a cadeia de caracteres é truncada) ou cadeias de caracteres que não são traduzidas (mas codificadas).

Além da substituição e expansão de caracteres, o pseudomecanismo fornece um identificador de controle exclusivo a cada recurso. Esse rastreador é anexado ao início de cada cadeia de caracteres e entre colchetes `[xxxxx]`. Você pode usar esses rastreadores durante o teste de inspeção visual da interface do usuário. Eles podem ajudar a rastrear recursos específicos no produto, especialmente se vários recursos possuem texto similar ou duplicado.

No texto de exemplo "Olá, Mundo!" a pseudotradução é expandida para ocupar aproximadamente 30% mais espaço de tela. Em seguida, é aplicado o rastreador de recursos.

```
"Hello World" -> "Ĥèĺļõ Ŵòŗłđ" -> "[!!_Ĥèĺļõ Ŵòŗłđ_!!]" -> "[hJ8s1][!!_Ĥèĺļõ Ŵòŗłđ_!!]"
```

### <a name="step-6-translate-your-app-into-selected-languages"></a>Etapa 6. Traduza o app para os idiomas escolhidos

O Kit de Ferramentas de Aplicativo Multilíngue é integrado ao processo de compilação. Durante a compilação, as cadeias de caracteres atualizadas são adicionadas automaticamente ao arquivo .xlf de cada idioma.
Depois que você testou seu aplicativo usando o Pseudoidioma, haverá três opções para traduzi-lo para outros idiomas para ser lançado.

#### <a name="option-1-translate-the-strings-yourself"></a>Opção 1. Traduza você mesmo as cadeias de caracteres

Você pode usar o Editor Multilíngue para traduzir as cadeias de caracteres individualmente. Como já mencionado, isso está incluído em [O instalador .msi](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit).

- Clique com o botão direito do mouse no arquivo .xlf que deseja traduzir.
- Clique em **Abrir com...** e selecione Editor Multilíngue. Opcionalmente, você pode clicar em **Definir como Padrão**.
- Para cada cadeia de caracteres **Origem** mostra a cadeia de caracteres original no idioma padrão. Em **Tradução**, digite a cadeia de caracteres traduzida no idioma apropriado para o arquivo .xlf que você está editando.
- Quando terminar, salve e feche o arquivo.

Faça a compilação do seu projeto novamente para que as cadeias de caracteres traduzidas sejam copiadas para o Arquivo de Recursos (.resw) que corresponde ao arquivo .xlf que você acabou de editar.

Você também pode iniciar o Editor Multilíngue da seguinte forma. Ir até o Início, exibir todos os aplicativos, abrir a pasta do Kit de Ferramentas de Aplicativo Multilíngue e clicar em Editor Multilíngue para iniciar.

#### <a name="option-2-send-the-xlf-files-to-a-third-party-for-translation"></a>Opção 2. Envie os arquivos .xlf para um terceiro para serem traduzidos

Para terceirizar o trabalho de tradução e edição aos tradutores, selecione os arquivos .xlf desejados no Gerenciador de Soluções, clique neles com o botão direito do mouse e em **Kit de Ferramentas de Aplicativo Multilíngue** > **Exportar traduções...**.

Selecione **Saída: Destinatário do e-mail** na caixa de diálogo Exportar recursos de sequência e clique em OK. Seus arquivos serão zipados e anexados a um novo email. Selecione **Saída: Local da pasta de arquivos**, navegue pela pasta e clique em OK, opcionalmente escolha a compactação (zip) dos arquivos, clique em OK novamente e seus arquivos serão (zipados e) salvos no local que você escolheu, dentro de uma nova pasta com o nome do seu projeto.

Depois que os tradutores concluírem o trabalho de tradução e enviarem os arquivos .xlf traduzidos, você poderá importá-los para o seu projeto. Selecione os arquivos .xlf desejados no Gerenciador de Soluções, clique neles com o botão direito do mouse e em **Kit de Ferramentas de Aplicativo Multilíngue** > **Importar/reciclar traduções...**. Clique em **Adicionar**, navegue até os arquivos .xlf ou .zip e clique em **Importar**.

**Observação** O processo de importação executa uma validação básica antes da importação. Isso garante que as informações da cultura de destino nos arquivos que estão sendo importados correspondam aos arquivos .xlf existentes.

Faça a compilação do seu projeto novamente para que as cadeias de caracteres traduzidas sejam copiadas para o(s) Arquivo(s) de Recursos (.resw) que corresponde(m) ao(s) arquivo(s) .xlf que você acabou de importar.

Estes provedores de terceiros oferecem serviços de localização e podem ajudar você.

- [Elanex](https://www.elanex.com/)
- [Keywords Studios](https://www.keywordsstudios.com/)
- [Lionbridge](https://www.lionbridge.com)
- [Moravia](https://www.moravia.com/)
- [SDL](https://www.sdl.com/languagecloud/managed-translation/ilp/instantquote)
- [Welocalize](https://www.welocalize.com/)

> [!NOTE]
> A lista acima é fornecida apenas para fins informativos e não é um endosso. A Microsoft não faz nenhuma representação ou garantia em relação a esses fornecedores ou seus serviços e, em nenhuma circunstância, a Microsoft será responsabilizada pelo seu uso desses fornecedores ou serviços. Todas as perguntas, reclamações ou declarações em relação a esses fornecedores ou seus serviços devem ser direcionadas ao fornecedor apropriado.

#### <a name="option-3-use-the-integrated-translation-services"></a>Opção 3. Use os serviços de tradução integrados

Os serviços de tradução são integrados ao Visual Studio IDE bem como ao Editor Multilíngue. Isso proporciona fácil acesso aos serviços de tradução ao mesmo tempo em que desenvolve seu produto e localiza seus recursos. Para esse serviço, você precisará de uma assinatura de conta do Azure, conforme descrito em [Microsoft Translator muda para o portal do Azure](https://multilingualapptoolkit.uservoice.com/knowledgebase/articles/1167898-microsoft-translator-moves-to-the-azure-portal).

Para acessar os serviços de tradução no Visual Studio, selecione e clique com o botão direito do mouse em um ou mais arquivos .xlf no Gerenciador de Soluções e clique em **Gerar Traduções Automáticas**.

O Editor Multilíngue fornece o mesmo suporte para tradução e também a adição de sugestões de tradução interativa, o que permite escolher a tradução que melhor se ajuste às cadeias de caracteres do recurso. Depois que a sugestão de tradução é fornecida, você pode fazer um ajuste fino da cadeia de caracteres para seu estilo de tradução.

Dois provedores são enviados com o Kit de Ferramentas de Aplicativo Multilíngue.

- O provedor do [Portal de Idiomas Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=330295) permite o suporte para correspondência de terminologias e reciclagem de tradução com base nas traduções de textos da interface do usuário para os produtos e serviços da Microsoft.
- O provedor do [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220) permite serviços de tradução de máquina a pedido.

Você e o(s) tradutor(es) pode(m) gerenciar as traduções no Editor Multilíngue para examinar as traduções posteriormente. Você pode definir o status de cada cadeia de caracteres na guia **Propriedades**. Os valores de status são: **Novo**, **Precisa de revisão**, **Traduzido**, **Final**, e **Desconectado**. O indicador à esquerda da linha mostra o status. Quando todas as linhas são exibidas em verde no Editor Multilíngue, o trabalho de tradução é feito.

Faça a compilação do seu projeto novamente para que as cadeias de caracteres traduzidas sejam copiadas para o(s) Arquivo(s) de Recursos (.resw) que corresponde(m) ao(s) arquivo(s) .xlf que você acabou de editar.

### <a name="step-7-upload-your-app-to-the-microsoft-store"></a>Etapa 7. Faça o upload do seu app para a Microsoft Store

Antes de começar o processo de Certificação da Microsoft Store, exclua o arquivo `<project-name>.qps-ploc.xlf` do projeto. O pseudoidioma é usado para detectar possíveis problemas ou bugs de localização, mas não é um idioma válido da Microsoft Store. Se não for removido, haverá falha em seu app durante o processo de Certificação da Microsoft Store.

## <a name="related-topics"></a>Tópicos relacionados

* [Localizar cadeias de caracteres em seu IU e manifesto do conjunto de aplicativo](../../app-resources/localize-strings-ui-manifest.md)
* [Globalização e localização](globalizing-portal.md)
* [Diretrizes de globalização](guidelines-and-checklist-for-globalizing-your-app.md)
* [Torne seu app localizável](prepare-your-app-for-localization.md)
* [Marca de idioma BCP-47](http://go.microsoft.com/fwlink/p/?linkid=227302)

## <a name="downloads"></a>Downloads

* [Instalador .vsix do Kit de Ferramentas de Aplicativo Multilíngue 4.0](https://marketplace.visualstudio.com/items?itemName=MultilingualAppToolkit.MultilingualAppToolkit-18308)
* [Instalador .msi do Kit de Ferramentas de Aplicativo Multilíngue 4.0](https://developer.microsoft.com/en-us/windows/develop/multilingual-app-toolkit)

## <a name="translation-services"></a>Serviços de tradução

* [Portal de Idiomas Microsoft](http://go.microsoft.com/fwlink/p/?LinkId=330295)
* [Microsoft Translator ](http://go.microsoft.com/fwlink/p/?LinkId=258220)
