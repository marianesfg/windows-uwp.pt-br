---
author: stevewhims
Description: This topic provides answers to frequently-asked questions and issues related to the Multilingual App Toolkit (MAT) 4.0.
title: Perguntas frequentes e solução de problemas do Kit de Ferramentas de Aplicativo Multilíngue
template: detail.hbs
ms.author: stwhi
ms.date: 11/13/2017
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 320445022fa836e05d04e93b85d333ef1ad40a53
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6978547"
---
# <a name="multilingual-app-toolkit-40-faq--troubleshooting"></a>Perguntas frequentes e solução de problemas do Kit de Ferramentas de Aplicativo Multilíngue 4.0

Este tópico fornece respostas a perguntas frequentes e problemas relacionados ao Kit de Ferramentas de Aplicativo Multilíngue (MAT) 4.0.

Consulte também [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

**Observação** O kit de ferramentas oferece suporte a arquivos .resw (XAML) e .resjson (JavaScript). No entanto, neste tópico vamos nos referir somente a arquivos. resw. Um arquivo .resw é conhecido como Arquivo de Recursos. Ele contém cadeias de caracteres, no idioma padrão ou traduzidas para outro idioma. A pasta que contém um arquivo .resw geralmente é nomeada de acordo com o valor da marca de idioma.

## <a name="do-i-need-resw-files-in-multiple-languages"></a>Preciso de arquivos .resw em vários idiomas?

Não. Um dos principais benefícios do Kit de Ferramentas é que não são necessários arquivos .resw em vários idiomas. O Kit de Ferramentas gerencia e sincroniza os recursos do seu aplicativo usando os arquivos .xlf. Isso remove os desafios associados à manutenção do conteúdo sincronizado em vários arquivos .resw.

Os projetos que contêm arquivos .resw e .xlf correspondentes fazem com que as traduções do arquivo .xlf sejam ignoradas. Quando isso acontece, um aviso é exibido durante a compilação para que você saiba que as traduções do .xlf não estão incluídas no aplicativo final. Um arquivo .resw e um arquivo .xlf são correspondentes quando possuem um idioma de destino com o mesmo código de idioma. Um exemplo de par correspondente seria `Strings\de-DE\Resources.resw` e um arquivo `<project-name>.de-DE.xlf` (contendo `target-language="de-DE"`).

## <a name="can-i-have-resw-files-in-multiple-languages"></a>Posso ter arquivos .resw em vários idiomas?

Você pode, mas não recomendamos. Se você deseja incluir arquivos .resw em vários idiomas no seu projeto e usar o Kit de Ferramentas, verifique se você não tem arquivos .resw e .xlf correspondentes.

## <a name="i-dont-see-an-option-in-the-tools-menu-to-enable-the-multilingual-app-toolkit"></a>Não vejo uma opção no menu Ferramentas para habilitar o Kit de Ferramentas de Aplicativo Multilíngue

Experimente estas etapas.

- Certifique-se de que você selecionou o nó do projeto, não o nó da solução, antes de abrir o menu **Ferramentas**.
- Confirme se a extensão do kit de ferramentas está instalada usando o Gerenciador de Extensões do Visual Studio.
- Confirme se o seu projeto é um projeto UWP.

## <a name="when-i-build-my-project-i-dont-see-a-message-saying-that-a-multilingual-app-toolkit-build-has-started"></a>Ao compilar o meu projeto, não recebo uma mensagem dizendo que a compilação do Kit de Ferramentas de Aplicativo Multilíngue foi iniciada

Confirme se você habilitou o MAT para o seu projeto. No menu **Ferramentas**, selecione **Kit de Ferramentas de Aplicativo Multilíngue** > **Habilitar seleção**. Se seu projeto foi habilitado usando uma versão anterior, desabilite e habilite novamente o MAT usando o menu **Ferramentas**. Isso atualiza o projeto para trabalhar com a nova versão do Kit de Ferramentas.

Verifique se o componente "Tarefa de Compilação para todas as edições do Visual Studio" foi instalado. O componente de compilação é instalado com a extensão, mas ele pode ser desmarcado manualmente durante a instalação. Esse componente é necessário para atualizar os arquivos .xlf e adicionar a tradução ao arquivo PRI. Quando esse componente é instalado e funciona corretamente, você recebe as mensagens de compilação.

```dosbatch
1> Multilingual App Toolkit build started.
1> Multilingual App Toolkit build completed successfully.
```

## <a name="the-toolkit-is-reporting-that-it-didnt-locate-any-xliff-language-files-during-the-build"></a>O kit de ferramentas está relatando que não localizou nenhum arquivo de linguagem XLIFF durante a compilação.

```dosbatch
No XLIFF language files were found. The app will not contain any localized resources.
```

Essa mensagem é exibida quando o kit de ferramentas não encontra nenhum arquivo no projeto com uma extensão .xlf. O kit de ferramentas gera e mantém esses arquivos na pasta `MultilingualResources` por padrão. Eles podem ser movidos; mas é melhor deixá-los nessa pasta, pois isso permite que o Editor Multilíngue encontre os arquivos de metadados relacionados.

## <a name="my-xlf-file-is-not-included-in-the-list-of-files-processed-by-the-toolkit-during-build"></a>Meu arquivo .xlf não está incluído na lista de arquivos processados pelo kit de ferramentas durante a compilação.

Se um arquivo .xlf for excluído reincluído em um projeto, o elemento de tipo de arquivo poderá ser definido incorretamente No Visual Studio, selecione o arquivo e verifique a janela Propriedades. A Ação de Compilação do arquivo deve ser definida como XliffResource, enquanto Copiar para Diretório de Saída definida como Não copiar. É assim que a referência deve aparecer no seu arquivo de projeto.

```xml
<XliffResource Include="MultilingualResources\<project-name>.fr-FR.xlf" />
```

## <a name="ive-added-xlf-based-languages-where-are-my-strings"></a>Adicionei idiomas baseados em .xlf. Onde estão minhas cadeias de caracteres?

O idioma padrão Arquivo de Recursos (.resw) é o "esquema" canônico das cadeias de caracteres que seu app usa. Os Arquivos de Recursos traduzidos podem conter todas ou um subconjunto dessas cadeias de caracteres.

Ao compilar o projeto, seus Arquivos de Recursos e seus arquivos .xlf são sincronizados.

- Os arquivos .xlf são atualizados para refletir cadeias de caracteres adicionadas ou removidas, ou adicionar ou remover Arquivos de Recursos.
- Os Arquivos de Recursos são atualizados de modo a refletir cadeias de caracteres traduzidas nos arquivos .xlf.

Isso é explicado detalhadamente em [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

## <a name="when-i-build-my-project-the-xlf-files-remain-empty"></a>Quando compilo meu projeto, os arquivos .xlf permanecem vazios

Antes de usar efetivamente o MAT, seu app precisa estar localizável. Isso é explicado detalhadamente em [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

## <a name="what-is-microsoft-translator"></a>O que é Microsoft Translator?

O Microsoft Translator é um serviço baseado em nuvem que fornece tradução baseada em máquina. A tradução por máquina é ideal para ter acesso à tradução quando a tradução humana não é razoável. Você pode saber mais em [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220).

O Kit de Ferramentas usa o serviço Microsoft Translator para fornecer sugestões de tradução a você. Você pode ver quais idiomas têm suporte pelo Microsoft Translator quando o ícone do Microsoft Translator está presente no diálogo Idiomas de Tradução.

Você pode traduzir rapidamente seu app usando o Microsoft Translator no Editor Multilíngue selecionando uma cadeia de caracteres e clicando em **Traduzir**.

## <a name="what-is-pseudo-language-and-what-are-pseudo-resource-trackers"></a>O que é pseudoidioma, e o que são pseudorastreadores de recursos?

Pseudoidioma é uma modificação artificial do produto de software destinada a simular a localização de idiomas reais. Você pode encontrar mais detalhes sobre o pseudoidioma e pseudorastreadores de recursos em [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

## <a name="how-do-i-set-my-language-preference-to-pseudo-language-so-that-i-can-test-my-pseudo-locd-strings"></a>Como configuro minha preferência de idioma para pseudoidioma de modo a poder testar minhas cadeias de caracteres pseudo-loc'd?

Isso é explicado em [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

## <a name="what-kind-of-localizability-issues-can-i-find-using-pseudo-language"></a>Que tipo de problemas de tradução posso encontrar usando o pseudoidioma?

Isso é explicado em [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

## <a name="im-not-seeing-any-translations-when-i-launch-my-app-or-my-app-is-only-partially-translated"></a>Não estou vendo nenhuma tradução quando inicializo meu aplicativo ou meu aplicativo está traduzido apenas parcialmente

Abra o arquivo .xlf no Editor Multilíngue para ver se há traduções presentes. Quando as cadeias de caracteres do arquivo .resw no idioma padrão são alteradas de forma explícita, quaisquer traduções correspondentes são removidas dos arquivos .xlf. Isso ocorre para garantir que a tradução corresponda à sua cadeia de caracteres de origem. Traduza a(s) cadeia(s) de caracteres no(s) arquivo(s) .xlf e recompile para atualizar o(s) arquivo(s) .resw no idioma não padrão.

Se as suas cadeias de caracteres estiverem traduzidas nos arquivos .xlf, mas não estiverem aparecendo no seu app, faça a compilação do seu projeto novamente para atualizar o(s) arquivo(s) .resw no idioma não padrão. O Visual Studio otimiza o comando Compilar para compilar somente os arquivos que foram alterados desde a última Compilação.

Verifique a ordem de preferência do seu idioma. Verifique se o idioma que você deseja testar está listado na parte superior da sua lista de preferências de idioma em **Configurações**.

## <a name="the-toolkit-is-reporting-error--0x80004004-in-the-build-output"></a>O kit de ferramentas está relatando um erro 0x80004004 na saída da compilação

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004004"
```

Essa mensagem pode ser exibida quando o formato da região entra em conflito com a operação de compilação do kit de ferramentas. A solução alternativa é alterar o seu idioma em **Configurações** para en-US durante a compilação.


## <a name="the-toolkit-is-reporting-error--0x80004005-in-the-build-output"></a>O kit de ferramentas está relatando um erro 0x80004005 na saída da compilação

```dosbatch
Merge of Loc PRI file failed calling makepri.exe: "0x80004005"
```

Essa mensagem pode ser exibida quando o arquivo .xlf contém um idioma de destino sem suporte. Por exemplo, "zh-cht" está incorreto (alterar para "zh-hant"), e "zh-chs" está incorreto (alterar para "zh-hans").

## <a name="is-there-a-way-to-find-out-more-information-about-the-errors-im-seeing"></a>Existe uma maneira de descobrir mais informações sobre os erros que eu estou vendo?

Sim, você pode ativar o log detalhado no Visual Studio. Clique em **Ferramentas** > **Opções** > **Projetos e soluções** > **Compilar e executar**. Altere **Detalhamento da saída de compilação do projeto no MSBuild** de Mínimo para Normal ou superior.

Executar MSBuild na linha de comando também pode produzir mensagens adicionais.

```dosbatch
msbuild /t:rebuild <project-name>
```

## <a name="import-translation-failed"></a>Falha na importação da tradução

O processo de importação executa uma validação básica antes da importação. Isso garante que as informações da cultura de destino nos arquivos que estão sendo importados correspondam aos arquivos .xlf existentes. Abra os arquivos .xlf no Editor Multilíngue e certifique-se de que as informações de cultura correspondem.

## <a name="what-if-my-translator-doesnt-have-windows-10-andor-visual-studio-andor-the-multilingual-app-toolkit-installed"></a>E se meu tradutor não tiver o Windows 10, e/ou Visual Studio e/ou o Kit de Ferramentas de Aplicativo Multilíngue instalado?

Quando você seleciona **Saída: destinatário do e-mail** na caixa de diálogo Exportar recursos de sequência, o e-mail inclui um link para baixar e instalar o Kit de Ferramentas de Aplicativo Multilíngue (MAT) 4.0. Seu tradutor ainda poderá instalar a ferramenta Editor Multilíngue autônomo MAT 4.0 mesmo sem o Windows 10 ou o Visual Studio.

Para mais informações, consulte [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md).

## <a name="what-happened-to-the-markuprulesxml-and-resourceslocksxml-files"></a>O que aconteceu com os arquivos `MarkupRules.xml` e `ResourcesLocks.xml`?

O Kit de Ferramentas de Aplicativo Multilíngue 4.0 não usa arquivos de bloqueio de recursos exclusivos. Em vez disso, a marca XLIFF 1.2 `<mrk>` é adicionada diretamente ao arquivo .xlf para identificar cadeias de caracteres que não são modificadas durante a tradução automática. Isso permite que o arquivo XLIFF seja independente, enquanto permite o bloqueio de recursos por arquivo.

Esses arquivos de suporte extra não são mais necessários e você pode excluí-los com segurança se tiver algum deles.

## <a name="what-happened-to-the-tpx-file"></a>O que aconteceu com o arquivo .tpx?

O arquivo .tpx proporcionava uma maneira fácil de incluir os arquivos `MarkupRules.xml` e `ResourcesLocks.xml` quando o arquivo .xlf era enviado para tradução. Esse recurso não é mais necessário.

Se você tiver traduções em um arquivo .tpx que precisa recuperar, simplesmente renomeie a extensão de arquivo .tpx para .zip. Isso permitirá que você abra e extraia o conteúdo com o Explorador de Arquivos ou com qualquer ferramenta compatível com .zip.

## <a name="i-think-ive-done-everything-right-but-it-still-isnt-working"></a>Acho que fiz tudo certo, mas ainda não está funcionando

Experimente estas etapas.

1. Adicione traduções usando um dos métodos já descritos.
2. Despeje o arquivo .pri (consulte [Opções de linha de comando MakePri.exe](../../app-resources/makepri-exe-command-options.md)) para ver se suas traduções estão no arquivo .pri. As traduções aparecerão com o código de idioma e o valor traduzido da seguinte forma:
   ```xml
   <Candidate qualifiers="Language-QPS-PLOC" type="String">
       <Value>[!!_Ŝéãřćĥ_!!]</Value>
   </Candidate>
   ```
3. Compilado a partir de um Prompt de comando; o erro resultante pode ter mais detalhes do que o que é relatado na saída da compilação.

## <a name="my-app-failed-certification-to-the-microsoft-store"></a>Meu app falhou na certificação da Microsoft Store

Antes de começar o processo de Certificação da Microsoft Store, exclua o arquivo `<project-name>.qps-ploc.xlf` do projeto. O pseudoidioma é usado para detectar possíveis problemas ou bugs de localização, mas não é um idioma válido da Microsoft Store. Se não for removido, haverá falha em seu app durante o processo de Certificação da Microsoft Store.

## <a name="related-topics"></a>Tópicos relacionados

* [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md)
* [Microsoft Translator](http://go.microsoft.com/fwlink/p/?LinkId=258220)
* [Opções de linha de comando do MakePri.exe](../../app-resources/makepri-exe-command-options.md)
