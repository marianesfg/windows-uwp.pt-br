---
Description: Saiba mais sobre os benefícios de globalizar e localizar seu aplicativo e exatamente o que esses termos significam.
Search.SourceType: Video
title: Globalização e localização
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.date: 12/07/2018
ms.topic: article
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 8c2d05c87f4f7b6164afe1fcbcb62323eef3bdf1
ms.sourcegitcommit: 5f80bfc3ba04ad0a0853f83917d6a0ef3da24fa3
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/13/2019
ms.locfileid: "72302256"
---
# <a name="globalization-and-localization"></a>Globalização e localização

O Windows é usado no mundo todo por públicos diversificados em termos de idioma, região e cultura. Seus usuários falam uma variedade de idiomas diferentes e em vários países e regiões diferentes. Alguns usuários falam mais de um idioma. Assim, seu aplicativo é executado em configurações que envolvem muitas permutações de configurações do sistema para idioma, região e cultura. Você pode aumentar o mercado potencial para seu app desenvolvendo-o para ser prontamente adaptável, usando *globalização* e *localização*.

Este vídeo fornece uma breve introdução sobre como preparar seu aplicativo para o mundo: [Introdução à globalização e à localização](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

**Globalização** é o processo de projetar e desenvolver seu app de forma que ele funcione adequadamente em diferentes mercados globais (em sistemas com diferentes configurações de idioma e cultura) sem exigir alterações específicas de cultura ou personalização.

- Leve em consideração a cultura ao manipular cadeias de caracteres. Por exemplo: não altere a ocorrência de cadeias de caracteres antes de compará-las.
- Use calendários apropriados à cultura atual.
- Use APIs de globalização para exibir dados formatados adequadamente para o país ou região, como números, datas, horas e moedas.
- Leve em consideração que culturas diferentes possuem regras diferentes para agrupar (ordenar) texto e outros dados.

Seu código precisa funcionar igualmente bem em qualquer uma das culturas que você determinou que dará suporte ao seu app. O ideal é que o seu código funcione igualmente bem no contexto de *qualquer* cultura, região ou idioma. A maneira mais eficiente para globalizar as funções do seu app é usar o conceito de culturas/localidades. Uma cultura/localidade é um conjunto de regras e um conjunto de dados específicos para um determinado idioma e área geográfica. Essas regras e dados incluem informações sobre os dados a seguir.

- Classificação de caractere
- Sistema de escrita
- Formatação de data e hora
- Convenções numéricas, de moeda, peso e medida
- Regras de classificação

>[!NOTE]
> Para obter uma lista de nomes de localidade com suporte pela versão do sistema operacional Windows, consulte a coluna de marca de idioma da tabela em [Appendix A: Comportamento do produto @ no__t-0 na [referência do identificador de código de idioma do Windows (LCID)](https://docs.microsoft.com/en-us/openspecs/windows_protocols/ms-lcid/70feba9f-294e-491e-b6eb-56532684c37f).

**Localizabilidade** é o processo de preparar um app globalizado para a localização e/ou verificar se o app está pronto para a localização. Tornar um app corretamente localizável significa que o processo de localização posterior não revelará nenhum defeito funcional no app. A propriedade mais essencial de um app localizável é que seu código executável tenha sido separado cuidadosamente dos recursos localizáveis do app.

- As cadeias de caracteres traduzíveis para idiomas diferentes podem variar muito de comprimento. Portanto, crie sua interface do usuário de modo a acomodar tamanhos de texto e tamanhos de fonte diferentes para rótulos e controles de entrada de texto.
- Tente evitar textos e/ou materiais culturalmente confidenciais em imagens.
- Não codifique cadeias de caracteres e imagens dependentes de cultura no código e marcação do app. Em vez disso, armazene-as como recursos de imagens e cadeias de caracteres para que elas se adaptem a diferentes mercados locais independentemente dos binários criados do seu app.
- Pseudolocalize seu app para divulgar quaisquer problemas de localização.

**Localização** é o processo de adaptação ou tradução dos recursos localizáveis do seu app para atender aos requisitos de idioma, cultura e política dos mercados locais específicos que o app visa dar suporte. No momento em que você alcançar o estágio de localização do seu app, se o app for localizável, não será necessário modificar nenhuma lógica durante esse processo.

- Traduza os recursos da cadeia de caracteres e outros ativos do app para o novo mercado.
- Modifique as imagens dependentes de cultura conforme necessário.
- Os arquivos também podem variar dependendo da região do usuário, independentemente do idioma. Por exemplo, um mapa pode ter fronteiras diferentes, dependendo da localização do usuário, mas os rótulos devem seguir o idioma preferido do usuário.

A maioria das equipes de localização usa ferramentas especiais para ajudar no processo. Por exemplo, reciclando a tradução de textos recorrentes.

| Artigo | Descrição |
|---------|-------------|
| [Diretrizes para globalização](guidelines-and-checklist-for-globalizing-your-app.md) | Projete e desenvolva seu app de forma que ele funciona corretamente em sistemas com diferentes configurações de idioma e cultura. |
| [Entender as linguagens de perfil de usuário e linguagens de manifesto de aplicativo](manage-language-and-region.md) | Este tópico define os termos "lista de idiomas do perfil do usuário", "lista de idiomas de manifesto do app" e "lista de idiomas de tempo de execução do aplicativo". Vamos usar esses termos neste tópico e em outros tópicos nessa área de recurso, por isso, é importante saber o que significam. |
| [Globalizar os formatos de data/hora/número](use-global-ready-formats.md) | Projete seu app de modo a ser usado globalmente. Formate adequadamente datas, horas, números, números de telefone e moedas. Você poderá, posteriormente, adaptar seu app a outras culturas, regiões e idiomas do mercado global. |
| [Usar modelos e padrões para formatar datas e horas](use-patterns-to-format-dates-and-times.md) | Usar classes no namespace [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) com modelos e padrões personalizados para exibir datas e horas no formato exato que você deseja. |
| [Ajustar layout e fontes e fornecer suporte para RTL](adjust-layout-and-fonts--and-support-rtl.md) | Projete seu app de forma a dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda). |
| [Valores de NumeralSystem](glob-numeralsystem-values.md) | Este tópico lista os valores disponíveis para a propriedade **NumeralSystem** de várias classes no namespace [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live). |
| [Tornar seu aplicativo localizável](prepare-your-app-for-localization.md) | Um app localizado é aquele que pode ser localizado em outros mercados, idiomas ou regiões sem revelar defeitos funcionais no app. A propriedade mais essencial de um aplicativo localizável é que seu código executável tenha sido separado cuidadosamente de seus recursos localizáveis. |
| [Fontes internacionais](loc-international-fonts.md) | Este tópico lista as fontes disponíveis para aplicativos UWP que são localizados em outros idiomas que não sejam os EUA Inglês. |
| [Criar seu aplicativo para texto bidirecional](design-for-bidi-text.md) | Projete seu app de modo a fornecer suporte bidirecional a texto (BiDi) para que você possa combinar o script de sistemas de escrita da esquerda para a direita e da direita para a esquerda. |
| [Use o kit de ferramentas de aplicativo multilíngue 4,0](use-mat.md) | O kit de ferramentas de aplicativo multilíngue (passe-partout) 4,0 integra-se com Microsoft Visual Studio 2017 e posterior para fornecer aos aplicativos UWP suporte a tradução, gerenciamento de arquivos de tradução e ferramentas de editor. |
| [Perguntas frequentes sobre o kit de ferramentas de aplicativo multilíngue 4,0 & solução de problemas](mat-faq-troubleshooting.md) | Este tópico fornece respostas a perguntas frequentes e problemas relacionados ao Kit de Ferramentas de Aplicativo Multilíngue (MAT) 4.0. |
| [Usar a página de código UTF-8](use-utf8-code-page.md) | UTF-8 é a página de código universal para internacionalização. |
| [Preparar seu aplicativo para a alteração de era japonesa](japanese-era-change.md) | Saiba mais sobre a mudança de era japonesa em maio de 2019 e como preparar seu aplicativo. |
