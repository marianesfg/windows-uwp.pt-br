---
author: stevewhims
Description: Learn about the benefits of globalizing and localizing your app, and exactly what these terms mean.
Search.SourceType: Video
title: Globalização e localização
ms.assetid: c0791eec-5bb8-4a13-8977-61d7d98e35ce
label: Intro
template: detail.hbs
ms.author: stwhi
ms.date: 10/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, globalização, localizabilidade, localização
ms.localizationpriority: medium
ms.openlocfilehash: 06931c53ba2670fd27fbf94a204ffd62a97bb184
ms.sourcegitcommit: f9a4854b6aecfda472fb3f8b4a2d3b271b327800
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/12/2017
ms.locfileid: "1394775"
---
# <a name="globalization-and-localization"></a>Globalização e localização

O Windows é usado no mundo todo por públicos diversificados em termos de idioma, região e cultura. Seus usuários falam uma variedade de idiomas diferentes e em vários países e regiões diferentes. Alguns usuários falam mais de um idioma. Assim, seu aplicativo é executado em configurações que envolvem muitas permutações de configurações do sistema para idioma, região e cultura. Você pode aumentar o mercado potencial para seu app desenvolvendo-o para ser prontamente adaptável, usando *globalização* e *localização*.

Este vídeo apresenta uma breve introdução sobre como preparar seu app para o mundo: [Introdução à globalização e à localização](https://channel9.msdn.com/Blogs/One-Dev-Minute/Introduction-to-globalization-and-localization).

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
| [Diretrizes de globalização](guidelines-and-checklist-for-globalizing-your-app.md) | Projete e desenvolva seu app de forma que ele funciona corretamente em sistemas com diferentes configurações de idioma e cultura. |
| [Entenda os idiomas de perfil do usuário e idiomas de manifesto do app](manage-language-and-region.md) | Este tópico define os termos "lista de idiomas do perfil do usuário", "lista de idiomas de manifesto do app" e "lista de idiomas de tempo de execução do aplicativo". Vamos usar esses termos neste tópico e em outros tópicos nessa área de recurso, por isso, é importante saber o que significam. |
| [Globalize seus formatos data/hora/número.](use-global-ready-formats.md) | Projete seu app de modo a ser usado globalmente. Formate adequadamente datas, horas, números, números de telefone e moedas. Você poderá, posteriormente, adaptar seu app a outras culturas, regiões e idiomas do mercado global. |
| [Usar modelos e padrões para formatar datas e horas](use-patterns-to-format-dates-and-times.md) | Usar classes no namespace [**Windows.Globalization.DateTimeFormatting**](/uwp/api/windows.globalization.datetimeformatting?branch=live) com modelos e padrões personalizados para exibir datas e horas no formato exato que você deseja. |
| [Ajustar layout e fontes e fornecer suporte a RTL](adjust-layout-and-fonts--and-support-rtl.md) | Projete seu app de forma a dar suporte a layouts e fontes de vários idiomas, incluindo direção de fluxo RTL (da direita para a esquerda). |
| [Valores de NumeralSystem](glob-numeralsystem-values.md) | Este tópico lista os valores disponíveis para a propriedade **NumeralSystem** de várias classes no namespace [**Windows.Globalization**](/uwp/api/windows.globalization?branch=live). |
| [Tornar seu app localizável](prepare-your-app-for-localization.md) | Um app localizado é aquele que pode ser localizado em outros mercados, idiomas ou regiões sem revelar defeitos funcionais no app. A propriedade mais essencial de um aplicativo localizável é que seu código executável tenha sido separado cuidadosamente de seus recursos localizáveis. |
| [Fontes internacionais](loc-international-fonts.md) | Este tópico lista as fontes disponíveis para os aplicativos UWP que são traduzidos para outros idiomas. |
| [Projetar seu app para texto bidirecional](design-for-bidi-text.md) | Projete seu app de modo a fornecer suporte bidirecional a texto (BiDi) para que você possa combinar o script de sistemas de escrita da esquerda para a direita e da direita para a esquerda. |
| [Use o Kit de Ferramentas de Aplicativo Multilíngue 4.0](use-mat.md) | O Kit de Ferramentas de Aplicativo Multilíngue (MAT) 4.0 se integra ao Microsoft Visual Studio 2017 para fornecer aplicativos UWP com suporte a tradução, gerenciamento de arquivos de tradução e ferramentas de edição. |
| [Perguntas frequentes e solução de problemas do Kit de Ferramentas de Aplicativo Multilíngue 4.0](mat-faq-troubleshooting.md) | Este tópico fornece respostas a perguntas frequentes e problemas relacionados ao Kit de Ferramentas de Aplicativo Multilíngue (MAT) 4.0. |
