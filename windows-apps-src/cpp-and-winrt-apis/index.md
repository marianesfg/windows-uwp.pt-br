---
description: O C++/WinRT é uma linguagem C++17 moderna e inteiramente padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em arquivo e cabeçalho.
title: C++/WinRT
ms.date: 04/18/2019
ms.topic: article
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção
ms.localizationpriority: medium
ms.openlocfilehash: ba8576402165f2d36d048eb3d214cb1dad601d76
ms.sourcegitcommit: 8179902299df0f124dd770a09a5a332397970043
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/24/2019
ms.locfileid: "68428627"
---
# <a name="cwinrt"></a>C++/WinRT

[C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Com o C++/WinRT, você pode criar e consumir APIs do Windows Runtime usando qualquer compilador C++17 em conformidade com os padrões. O SDK do Windows inclui o C++/WinRT; ele foi introduzido na versão 10.0.17134.0 (Windows 10, versão 1803).

C++/WinRT é para qualquer desenvolvedor interessado em escrever códigos lindos e rápidos para Windows. Veja por quê.

## <a name="the-case-for-cwinrt"></a>O caso de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

A linguagem de programação C++ é usada na empresa *e* em segmentos de ISVs (fornecedores independentes de software) para aplicativos em que altos níveis de correção, qualidade e desempenho são avaliados. Por exemplo: sistemas de programação; sistemas incorporados e móveis com recursos limitados; jogos e elementos gráficos; drivers de dispositivo; e os aplicativos industriais, científicos e médicos, para citar apenas alguns.

Do ponto de vista do idioma, C++ sempre foi sobre criação e consumo de abstrações dos tipos rico e leve. Mas o idioma mudou radicalmente desde os ponteiros brutos, loops brutos e alocação de memória criteriosa e liberação de C++98. C++ moderno (do C++11 em diante) é sobre expressão clara de ideias, simplicidade, leitura fácil e uma menor probabilidade de introdução de bugs.

Para criar e consumir APIs do Windows Runtime usando C++, há C++/WinRT. Isso é a substituição recomendada da Microsoft para o [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) projeção de linguagem e a [WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live).

Você usa tipos de dados C++ padrão, algoritmos e palavras-chave ao usar C++/WinRT. A projeção tem seus próprios tipos de dados personalizados, mas, na maioria dos casos, você não precisa aprendê-los, porque eles fornecem as conversões apropriadas para e de tipos padrão. Dessa forma, você pode continuar a usar os recursos de linguagem C++ padrão que você está acostumado a usar e o código-fonte que você já tem. C++/WinRT torna extremamente fácil chamar as APIs do Windows Runtime em qualquer aplicativo de C++, do Win32 a UWP.

C++/WinRT tem melhor desempenho e produz binários menores do que qualquer outra opção de idioma para o Windows Runtime. Ele até supera o desempenho do código escrito usando as interfaces ABI diretamente. Isso ocorre porque as abstrações usam idiomas C++ modernos para qual o compilador do Visual C++ é projetado para otimizar. Isso inclui estatísticas mágicas, classes de base vazia, elisão de **strlen**, assim como muitas outras otimizações mais novas na versão mais recente do Visual C++ com o objetivo específico de melhorar o desempenho de C++/WinRT.

### <a name="topics-about-cwinrt"></a>Tópicos sobre C++/WinRT

| Tópico | Descrição |
| - | - |
| [Introdução ao C++/WinRT](intro-to-using-cpp-with-winrt.md) | Uma introdução ao C++/WinRT&mdash;uma projeção de linguagem C++ padrão para APIs do Windows Runtime. |
| [Introdução ao C++/WinRT](get-started.md) | Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples. |
| [O que há de novo no C++/WinRT](news.md) | Notícias e as alterações a C++/WinRT. |
| [Perguntas frequentes](faq.md) | Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT. |
| [Solução de problemas](troubleshooting.md) | A tabela de sintomas de solução de problemas e soluções neste tópico pode ser útil para você se você estiver recortando novo código ou fazendo a portabilidade de um aplicativo existente. |
| [Aplicativo de exemplo de C++/WinRT do Editor de Fotos](photo-editor-sample.md) | O Editor de Fotos é um aplicativo de exemplo UWP que mostra o desenvolvimento com a projeção de linguagem C++/WinRT. O aplicativo de exemplo permite que você recupere fotos da biblioteca de **Imagens** e, em seguida, edite a imagem selecionada com efeitos fotográficos diferentes. | 
| [Manipulação de cadeia de caracteres](strings.md) | Com C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de cadeia ampla de C++ padrão ou você pode usar o tipo [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). |
| [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md) | Com o C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão. |
| [Valores de conversão boxing e unboxing valores para IInspectable](boxing.md) | Um valor escalar precisa ser encapsulado em um objeto de classe de referência antes de ser passado para uma função que espera **IInspectable**. Esse processo de encapsulamento é conhecido como *conversão* do valor. |
| [Consumir APIs com C++/WinRT](consume-apis.md) | Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas implementadas pelo Windows, um fornecedor de componentes de terceiros ou por você mesmo. |
| [Criar APIs com C++/WinRT](author-apis.md) | Este tópico mostra como criar APIs de C++/WinRT usando a estrutura de base **winrt::implements**, direta ou indiretamente. |
| [Tratamento de erro com C++/WinRT](error-handling.md) | Este tópico aborda as estratégias para processar erros ao programar com C++/WinRT. |
| [Manejar eventos usando delegados](handle-events.md) | Este tópico mostra como registrar e revogar delegados lidando com eventos usando C++/WinRT. |
| [Criar eventos](author-events.md) | Este tópico demonstra como criar um componente do Tempo de Execução do Windows contendo uma classe de tempo de execução que gera eventos. Ele também demonstra um aplicativo que consome o componente e maneja os eventos. |
| [Coleções com C++/WinRT](collections.md) | C++/WinRT fornece funções e classes base que economizam muito tempo e esforço quando você deseja implementar e/ou passar coleções. |
| [Simultaneidade e operações assíncronas](concurrency.md) | Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT. |
| [Simultaneidade e assincronia mais avançadas](concurrency-2.md) | Cenários mais avançados com simultaneidade e assincronia no C++/WinRT. |
| [Controles XAML; associar a uma propriedade C++/WinRT](binding-property.md) | Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Este tópico mostra como implementar e consumir uma propriedade observável e como associar um controle XAML a ela. |
| [Controles de itens XAML; associar a uma coleção C++/WinRT](binding-collection.md) | Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Este tópico mostra como implementar e consumir uma coleção observável e como associar um controle de itens XAML a ela. |
| [Controles personalizados (modelos) XAML com C++/WinRT](xaml-cust-ctrl.md) | Este tópico orienta você pelas etapas de criação de um controle personalizado simples usando C++/WinRT. Você pode usar as informações aqui como base para criar seus próprios controles de interface do usuário personalizáveis e ricos em recursos. |
| [Passagem de parâmetros para o limite do ABI](pass-parms-to-abi.md) | O C++/WinRT simplifica a passagem de parâmetros para o limite do ABI, fornecendo conversões automáticas para casos comuns. |
| [Consumir componentes COM com C++/WinRT](consume-com.md) | Este tópico usa um exemplo de código completo do Direct2D para mostrar como usar C++/WinRT para consumir classes e interfaces COM. |
| [Criar componentes COM com C++/WinRT](author-coclasses.md) | C++/WinRT pode ajudá-lo a criar componentes COM clássicos, exatamente como ajuda a criar classes no Windows Runtime. |
| [Mover do C++/CX para C++/WinRT](move-to-winrt-from-cx.md) | Este tópico mostra como fazer a portabilidade do código C++/CX para seu equivalente no C++/WinRT. |
| [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md) | Este tópico mostra duas funções auxiliares que podem ser usadas para realizar a conversão entre os objetos [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) e C++/WinRT. |
| [Mudar do WRL para o C++/WinRT](move-to-winrt-from-wrl.md) | Este tópico mostra como fazer a portabilidade do código da [WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows)](/cpp/windows/windows-runtime-cpp-template-library-wrl) para seu equivalente no C++/WinRT. |
| [Mover do C# para C++/WinRT](move-to-winrt-from-csharp.md) | Este tópico mostra como fazer a portabilidade do código C# para o equivalente no C++/WinRT. |
| [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md) | Este tópico mostra como realizar a conversão entre interface binária do aplicativo (ABI) e objetos do C++/WinRT. |
| [Referências fortes e fracas em C++/WinRT](weak-references.md) | O Windows Runtime é um sistema de contagem de referência; e, em um sistema desse tipo, é importante que você conheça o significado e a distinção entre referências fortes e fracas. |
| [Objetos ágeis](agile-objects.md) | Um objeto ágil é aquele que pode ser acessado de qualquer thread. Seus tipos C++/WinRT são ágeis por padrão, mas você pode recusá-los. |
| [Diagnosticando alocações diretas](diag-direct-alloc.md) | Este tópico apresenta detalhes sobre um recurso do C++s/WinRT 2.0 que ajuda você a diagnosticar o erro de criação de um objeto do tipo de implementação na pilha, em vez de usar a família [**winrt::make**](/uwp/cpp-ref-for-winrt/make) de auxiliares, como deveria ser. |
| [Detalhes sobre destruidores](details-about-destructors.md) | O C++O/WinRT 2.0 permite adiar a destruição de seus tipos de implementação e consultar com segurança durante a destruição. Este tópico descreve esses recursos e explica quando usá-los. |
| [Um exemplo simples de biblioteca de interface do usuário do Windows em C++/WinRT](simple-winui-example.md) | Este tópico orienta você no processo de adicionar suporte simples ao WinUI em um projeto C++/WinRT. |

### <a name="topics-about-the-c-language"></a>Tópicos sobre a linguagem C++

| Tópico | Descrição |
| - | - |
| [Categorias de valor e referências a eles](cpp-value-categories.md) | Este tópico descreve as diversas categorias de valores que existem em C++. Você já deve ter ouvido falar de lvalues e rvalues, mas há outros tipos também. |

## <a name="important-apis"></a>APIs Importantes
* [namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [WRL (Biblioteca de Modelos C++ do Tempo de Execução do Windows)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
