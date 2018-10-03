---
author: stevewhims
description: O C++/WinRT é uma linguagem C++17 moderna e inteiramente padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em arquivo e cabeçalho.
title: C++/WinRT
ms.author: stwhi
ms.date: 05/14/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp, padrão, c++, cpp, winrt, projeção
ms.localizationpriority: medium
ms.openlocfilehash: 7168ee705114523a324194b89f8450e768cfab22
ms.sourcegitcommit: 1938851dc132c60348f9722daf994b86f2ead09e
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/02/2018
ms.locfileid: "4265057"
---
# [<a name="cwinrt"></a>C++/WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt)
C++/WinRT é uma projeção de linguagem C++17 completamente moderna e padrão para APIs do Windows Runtime (WinRT), implementada como uma biblioteca com base em cabeçalho e arquivo, projetada para fornecer acesso de primeira classe à API moderna do Windows. Com o C++/WinRT, você pode criar e consumir APIs do Windows Runtime usando qualquer compilador C++17 compatível com padrões. O SDK do Windows inclui o C++/WinRT; ele foi introduzido na versão 10.0.17134.0 (Windows 10, versão 1803).

C++/WinRT é para qualquer desenvolvedor interessado em escrever códigos lindos e rápidos para Windows. Veja por quê.

## <a name="the-case-for-cwinrt"></a>O caso de C++/WinRT
&nbsp;
> [!VIDEO https://www.youtube.com/embed/TLSul1XxppA]

A linguagem de programação C++ é usada na empresa *e* em segmentos de fornecedores independentes de software (ISV) para aplicativos onde altos níveis de correção, qualidade e desempenho são avaliados. Por exemplo: sistemas de programação; sistemas incorporados e móveis com recursos limitados; jogos e elementos gráficos; drivers de dispositivo; e os aplicativos industriais, científicos e médicos, para citar apenas alguns.

Do ponto de vista do idioma, C++ sempre foi sobre criação e consumo de abstrações dos tipos rico e leve. Mas o idioma mudou radicalmente desde os ponteiros brutos, loops brutos e alocação de memória criteriosa e liberação de C++98. C++ moderno (do C++11 em diante) é sobre expressão clara de ideias, simplicidade, leitura fácil e uma menor probabilidade de introdução de bugs.

Para criar e consumir APIs do Windows Runtime usando C++, há C++/WinRT. Essa é a substituição recomendada da Microsoft para o [C++ c++ /CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) projeção de linguagem e a [Biblioteca de modelos em C++ Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl?branch=live).

Você usa tipos de dados C++ padrão, algoritmos e palavras-chave ao usar C++/WinRT. A projeção tem seus próprios tipos de dados personalizados, mas na maioria dos casos você não precisa aprendê-los, porque eles fornecem as conversões apropriadas para e de tipos padrão. Dessa forma, você pode continuar a usar os recursos de linguagem C++ padrão que você está acostumado a usar e o código-fonte que você já tem. C++/WinRT torna extremamente fácil chamar as APIs do Windows Runtime em qualquer aplicativo de C++, do Win32 a UWP.

C++/WinRT tem melhor desempenho e produz binários menores do que qualquer outra opção de idioma para o Windows Runtime. Ele até supera o desempenho do código escrito usando as interfaces ABI diretamente. Isso ocorre porque as abstrações usam idiomas C++ modernos para qual o compilador do Visual C++ é projetado para otimizar. Isso inclui estatísticas mágicas, classes de base vazia, elisão de **strlen**, assim como muitas outras otimizações mais novas na versão mais recente do Visual C++ com o objetivo específico de melhorar a performance de C++/WinRT.

### <a name="topics-about-cwinrt"></a>Tópicos sobre C++ c++ WinRT

| Tópico | Descrição |
| - | - |
| [Introdução ao C++/WinRT](intro-to-using-cpp-with-winrt.md) | Uma introdução ao C++/WinRT, uma projeção de linguagem C++ padrão para APIs do Windows Runtime. |
| [Introdução ao C++/WinRT](get-started.md) | Este tópico fornece instruções para você se atualizar sobre o uso do C++/WinRT por um exemplo de código simples. |
| [Perguntas frequentes](faq.md) | Respostas às perguntas que você pode ter sobre a criação e consumo de APIs do Windows Runtime com C++/WinRT. |
| [Solução de problemas](troubleshooting.md) | A tabela de sintomas de solução de problemas e soluções neste tópico pode ser útil para você se você estiver recortando novo código ou fazendo a portabilidade de um aplicativo existente. |
| [Aplicativos de exemplo de C++/WinRT do Editor de Fotos](photo-editor-sample.md) | O Editor de Fotos é um aplicativo de exemplo UWP que mostra o desenvolvimento com a projeção de linguagem C++/WinRT. O aplicativo de exemplo permite que você recupere fotos a partir da biblioteca de **Imagens** e, em seguida, edite a imagem selecionada com efeitos fotográficos diferentes. | 
| [Manipulação de cadeia de caracteres](strings.md) | Com C++/WinRT, você pode chamar APIs de Windows Runtime usando tipos de cadeia ampla de C++ padrão, ou você pode usar o tipo [**winrt::hstring**](/uwp/cpp-ref-for-winrt/hstring). |
| [Tipos de dados C++ padrão e C++/WinRT](std-cpp-data-types.md) | Com C++/WinRT, você pode chamar APIs do Windows Runtime usando tipos de dados C++ padrão. |
| [Valores de conversão de boxing e unboxing valores para IInspectable](boxing.md) | Um valor escalar precisa ser encapsulado em um objeto de classe de referência antes de ser passado para uma função que espera **IInspectable**. Esse processo de encapsulamento é conhecido como *conversão* do valor. |
| [Consumir APIs com C++/WinRT](consume-apis.md) | Este tópico mostra como consumir as APIs do C++/WinRT, sejam elas implementadas pelo Windows, um fornecedor de componentes de terceiros, ou por você mesmo. |
| [Criar APIs com C++/WinRT](author-apis.md) | Este tópico mostra como criar APIs de C++/WinRT usando a estrutura de base **winrt::implements**, direta ou indiretamente. |
| [Processamento de erros com C++/WinRT](error-handling.md) | Este tópico aborda as estratégias para processar erros ao programar com C++/WinRT. |
| [Manejar eventos usando delegados](handle-events.md) | Este tópico mostra como registrar e revogar delegados lidando com eventos usando C++/WinRT. |
| [Criar eventos](author-events.md) | Este tópico demonstra como criar um componente do Tempo de Execução do Windows contendo uma classe de tempo de execução que gera eventos. Ele também demonstra um aplicativo que consome o componente e maneja os eventos. |
| [Coleções com C++ c++ WinRT](collections.md) | C++ c++ WinRT fornece funções e classes base que economizar muito tempo e esforço quando você deseja implementar e/ou passe coleções. |
| [Simultaneidade e operações assíncronas](concurrency.md) | Este tópico mostra as maneiras nas quais você pode criar e consumir objetos assíncronos do Windows Runtime com C++/WinRT. |
| [Controles XAML; vincular a uma propriedade C++/WinRT](binding-property.md) | Uma propriedade que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma propriedade *observável*. Este tópico mostra como implementar e consumir uma propriedade observável e como associar um controle XAML a ela. |
| [Controles de itens XAML; vincular a uma coleção C++/WinRT](binding-collection.md) | Uma coleção que pode ser efetivamente vinculada a um controle de itens XAML é conhecida como uma coleção *observável*. Este tópico mostra como implementar e consumir uma coleção observável e como associar um controle de itens XAML a ela. |
| [Controles personalizados (modelos) de XAML com C++ c++ WinRT](xaml-cust-ctrl.md) | Este tópico o orienta pelas etapas de criação de um controle personalizado simples usando C++ c++ WinRT. Você pode se basear as informações para criar seus próprios controles de interface do usuário rico e personalizáveis. |
| [Consumir componentes COM C++ c++ WinRT](consume-com.md) | Este tópico usa um exemplo de código completo do Direct2D para mostrar como usar C++ c++ WinRT consumir COM classes e interfaces. |
| [Criar componentes COM C++ c++ WinRT](author-coclasses.md) | C++ c++ WinRT pode ajudar você a criar componentes COM clássicos, assim como ele ajuda a criar classes de tempo de execução do Windows. |
| [Interoperabilidade entre C++/WinRT e C++/CX](interop-winrt-cx.md) | Este tópico mostra duas funções auxiliares que podem ser usadas para realizar a conversão entre os objetos [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx?branch=live) e C++/WinRT. |
| [Mudar do C++/CX para C++/WinRT](move-to-winrt-from-cx.md) | Este tópico mostra como fazer a portabilidade do código C++/CX para seu equivalente no C++/WinRT. |
| [Interoperabilidade entre C++/WinRT e ABI](interop-winrt-abi.md) | Este tópico mostra como realizar a conversão entre interface binária do aplicativo (ABI) e objetos do C++/WinRT. |
| [Mudar do WRL para C++/WinRT](move-to-winrt-from-wrl.md) | Este tópico mostra como fazer a portabilidade do código da [Biblioteca de Modelos C++ do Windows Runtime (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl) para seu equivalente no C++/WinRT. |
| [Referências fracas](weak-references.md) | Suporte de referência fraca de C++/WinRT é pagar para usar, significando que não custa nada a não ser que o objeto seja consultado por [**IWeakReferenceSource**](https://msdn.microsoft.com/library/br224609). |
| [Objetos ágeis](agile-objects.md) | Um objeto ágil é aquele que pode ser acessado de qualquer conversa. Seus tipos C++/WinRT são ágeis por padrão, mas você pode recusá-los. |

### <a name="topics-about-the-c-language"></a>Tópicos sobre a linguagem C++

| Tópico | Descrição |
| - | - |
| [Categorias de valor e referências a eles](cpp-value-categories.md) | Este tópico descreve as várias categorias de valores que existem em C++. Você já deve ter ouvido de lvalues e rvalues, mas há outros tipos, também. |

## <a name="important-apis"></a>APIs Importantes
* [Namespace winrt](/uwp/cpp-ref-for-winrt/winrt)

## <a name="related-topics"></a>Tópicos relacionados
* [C++/CX](/cpp/cppcx/visual-c-language-reference-c-cx)
* [Biblioteca de Modelos C++ do Tempo de Execução do Windows (WRL)](/cpp/windows/windows-runtime-cpp-template-library-wrl)
