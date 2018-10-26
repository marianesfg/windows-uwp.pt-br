---
author: stevewhims
description: Alterações em C++ e notícias c++ WinRT.
title: Quais são as novidades no C++ c++ WinRT
ms.author: stwhi
ms.date: 10/03/2018
ms.topic: article
keywords: Windows 10, uwp, padrão, c++, cpp, winrt, projeção, notícias, o que do, a nova funcionalidade
ms.localizationpriority: medium
ms.openlocfilehash: 1ada059dc2acfa96dd61b6f3460e25736d96ff68
ms.sourcegitcommit: d0e836dfc937ebf7dfa9c424620f93f3c8e0a7e8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/26/2018
ms.locfileid: "5665456"
---
# <a name="whats-new-in-cwinrt"></a>Quais são as novidades no C++ c++ WinRT

A tabela a seguir contém notícias e muda para [C++ c++ WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) da versão estará disponível mais recente do SDK do Windows, que é 10.0.17763.0 (Windows 10, versão 1809). Essas alterações também podem estar presentes em versões posteriores do SDK Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Notícias e alterações, no SDK do Windows versão 10.0.17763.0 (Windows 10, versão 1809)

| Recurso novo ou alterado | Mais informações |
| - | - |
| **Alteração significativa**. Para que ele seja compilar C++, c++ WinRT não depende de cabeçalhos do SDK do Windows. | [Isolamento de arquivos de cabeçalho do SDK do Windows](#isolation-from-windows-sdk-header-files), veja a seguir. |
| O formato de sistema de projeto do Visual Studio foi alterado. | Consulte [como redirecionar C++ c++ WinRT projeto para uma versão posterior do SDK do Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk)abaixo. |
| Há novas funções e classes base para ajudá-lo a passar um objeto de coleção para uma função de tempo de execução do Windows, ou para implementar seus próprios tipos de coleção e propriedades de coleção. | Consulte [coleções com C++ c++ WinRT](collections.md). |
| Você pode usar a extensão de marcação [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) com C++ c++ classes de tempo de execução do WinRT. | Para obter mais informações e exemplos de código, consulte [Visão geral de vinculação de dados](/windows/uwp/data-binding/data-binding-quickstart). |
| Suporte ao cancelamento de uma rotina concomitante permite que você registre um retorno de chamada de cancelamento. | Para obter mais informações e exemplos de código, consulte [o cancelamento de uma operação assíncrona e os retornos de chamada de cancelamento](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Ao criar um representante apontando para uma função de membro, você pode estabelecer uma referência forte ou fraca para o objeto atual (em vez de um ponteiro bruto *Este* ) no ponto onde o manipulador é registrado. | Para obter mais informações e exemplos de código, consulte a seção subpropriedade **se você usar uma função de membro como um delegado** na seção [com segurança acessando o ponteiro *this* com um representante do manipulador de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bugs são corrigidos que foram revelados pela conformidade aprimorada do Visual Studio para o C++ padrão. A cadeia de ferramentas LLVM e Clang também melhor é usada para validar C++ c++ conformidade de padrões do WinRT. | Você não encontrará o problema porquê descrito em [não meu novo projeto compilação? Usando o Visual Studio 2017 (versão 15.8.0 ou superior) e o SDK versão 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Outras alterações.

- **Alteração significativa**. [**winrt::get_abi(winrt::hstring const&)**](/uwp/cpp-ref-for-winrt/get-abi) agora retorna `void*` em vez de `HSTRING`. Você pode usar `static_cast<HSTRING>(get_abi(my_hstring));` para obter um HSTRING.
- **Alteração significativa**. [**WinRT::put_abi(WinRT::hstring&)**](/uwp/cpp-ref-for-winrt/put-abi) agora retorna `void**` em vez de `HSTRING*`. Você pode usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obter um HSTRING *.
- **Alteração significativa**. HRESULT agora é projetada como **WinRT:: HRESULT**. Se você precisa de um HRESULT (para a verificação de tipo, ou para dar suporte a características de tipo), então você pode `static_cast` um **WinRT:: HRESULT**. Caso contrário, o **WinRT:: HRESULT** converte em HRESULT, desde que você inclua `unknwn.h` antes de incluir qualquer C + c++ cabeçalhos do WinRT.
- **Alteração significativa**. GUID agora é projetada como **winrt::guid**. Para APIs que você implementa, você deve usar **winrt::guid** para os parâmetros GUID. Caso contrário, o **WinRT:: HRESULT** converte em GUID, desde que você inclua `unknwn.h` antes de incluir qualquer C + c++ cabeçalhos do WinRT.
- **Alteração significativa**. O [**construtor winrt::handle_type**](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) foi reforçada fazendo explícito (ele agora é mais difícil de escrever código incorreto com ele). Se você precisar atribuir um valor de identificador bruto, chame a [**função handle_type::attach**](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) .
- **Alteração significativa**. As assinaturas de **WINRT_CanUnloadNow** e **WINRT_GetActivationFactory** foram alteradas. Você não deve declarar essas funções todo o tempo. Em vez disso, inclua `winrt/base.h` (que é incluído automaticamente se você incluir qualquer C + c++ arquivos de cabeçalho de namespace WinRT Windows) para incluir as declarações dessas funções.
- Para a [**estrutura de winrt::clock**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** são substituídas por **from_file_time/to_file_time**.
- APIs que esperam **IBuffer** parâmetros são simplificados. Embora a maioria das APIs preferir coleções ou matrizes, suficiente APIs dependem de **IBuffer** que ele precisava ser mais fácil de usar essas APIs de C++. Esta atualização fornece acesso direto aos dados por trás de uma implementação de **IBuffer** , usando a mesma convenção de nomenclatura de dados usada pelos contêineres da biblioteca padrão C++. Isso também evita colidam nomes de metadados que normalmente começam com uma letra maiuscula.
- Aprimorada a geração de código: várias melhorias para reduzir o tamanho do código, melhorar inlining e otimizar o armazenamento em cache de fábrica.
- Removido recursão desnecessária. Quando a linha de comando se refere a uma pasta, em vez de em um determinado `.winmd`, o `cppwinrt.exe` ferramenta não pesquisa recursivamente para `.winmd` arquivos. O `cppwinrt.exe` ferramenta agora também manipula duplicatas mais inteligente, tornando mais resistente a erro de usuário e a mal formado `.winmd` arquivos.
- Proteção avançados ponteiros inteligentes. Anteriormente, os revokers evento Falha ao revogar quando move-atribuído um novo valor. Isso ajudou a descobrir um problema em que as classes de ponteiro inteligente não foram manipular confiavelmente Self atribuição; enraizada no [**modelo de struct WinRT:: com_ptr**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT:: com_ptr** foi corrigido e revokers o evento fixados para manipular mover semântica corretamente para que eles revogar após a atribuição.

> [!NOTE]
> Com a versão 1.0.181002.2 (ou posterior) do [C++ c++ WinRT Visual Studio Extension (VSIX)](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-and-the-vsix) instalado, criando um novo C + c++ WinRT projeto automaticamente instala o [pacote Microsoft.Windows.CppWinRT NuGet](https://www.nuget.org/packages/Microsoft.Windows.CppWinRT/) para o projeto. O pacote Microsoft.Windows.CppWinRT NuGet fornece melhor C + c++ suporte de compilação do projeto WinRT, tornando seu projeto portáveis entre uma máquina de desenvolvimento e um agente de compilação (em que somente o pacote NuGet e não o VSIX, está instalado).
>
> Para um projeto existente&mdash;depois de instalar a versão 1.0.181002.2 (ou posterior) do VSIX&mdash;, recomendamos que você abra o projeto no Visual Studio, clique **no projeto** \> **Manage NuGet Packages...**  \>  **Procurar**, digite ou cole **Microsoft.Windows.CppWinRT** na caixa de pesquisa, selecione o item nos resultados da pesquisa e, em seguida, clique em **instalar** para instalar o pacote para o projeto.


## <a name="isolation-from-windows-sdk-header-files"></a>Isolamento de arquivos de cabeçalho do SDK do Windows

Isso é potencialmente uma alteração significativa para o código.

Para que ele seja compilar C++, c++ WinRT não depende de arquivos de cabeçalho do SDK do Windows. Arquivos de cabeçalho na biblioteca de tempo de execução C (CRT) e a biblioteca de modelo padrão C++ (STL) também não incluem quaisquer cabeçalhos do SDK do Windows. E o que melhora a conformidade de padrões, evita dependências inadvertidas e reduz consideravelmente o número de macros que você precisa proteger contra.

Essa independência significa ou C + c++ WinRT agora é mais portável e compatíveis com padrões, e ele amplia a possibilidade de que ele se tornar uma biblioteca de compilador cruzada e plataformas. Isso também significa que C++ c++ WinRT cabeçalhos não são afetadas negativamente macros.

Se você anteriormente deixei C + c++ WinRT incluir quaisquer cabeçalhos do Windows em seu projeto, agora você precisará incluí-los. Ele é, em qualquer caso, sempre melhor prática para incluir explicitamente os cabeçalhos que dependem de você e não deixá-la para outra biblioteca para incluí-los para você.

Atualmente, as únicas exceções para isolamento de arquivo de cabeçalho do SDK do Windows são para intrínsecos e caracteres alfanuméricos. Não há nenhum problema conhecido com essas dependências restantes últimos.

No seu projeto, você pode reativar a interoperabilidade com os cabeçalhos do SDK do Windows se você precisar. Por exemplo, convém implementar uma interface COM (enraizada no [**IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Para esse exemplo, incluem `unknwn.h` antes de incluir qualquer C + c++ cabeçalhos do WinRT. Fazendo causas afirmativo C++ c++ WinRT biblioteca base para habilitar vários ganchos dar suporte a interfaces de COM clássico. Para obter um exemplo de código, consulte [componentes de autor COM C++ c++ WinRT](author-coclasses.md). Da mesma forma, inclua explicitamente qualquer outros cabeçalhos do SDK do Windows que declaram tipos e/ou funções que você deseja chamar.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Como redirecionar C++ c++ WinRT projeto para uma versão posterior do SDK do Windows

O método redirecionamento seu projeto que é provavelmente resultará na edição de compilador e vinculador menor também é o mais trabalhoso. Esse método envolve a criação de um novo projeto (visando a versão do SDK do Windows de sua escolha) e, em seguida, copiar arquivos para seu novo projeto da sua antiga. Haverá seções de sua antigo `.vcxproj` e `.vcxproj.filters` arquivos que você pode simplesmente copiar mais salvar você adicionar arquivos no Visual Studio.

No entanto, há duas outras maneiras de redirecionar seu projeto no Visual Studio.

- Vá para projeto propriedade **Geral** \> **Versão do SDK do Windows**e selecione **Todas as configurações** e **Todas as plataformas**. Configure a **Versão do SDK do Windows** para a versão que você deseja destinar.
- No **Gerenciador de soluções**, clique com botão direito no nó do projeto, clique em **Redirecionar projetos**, escolha o artes que você deseja direcionar e clique em **Okey**.

Se você encontrar qualquer compilador ou erros de vinculador depois usando um destes dois métodos, então você poderá tentar a solução de limpeza (**Build** > **Limpar solução** e/ou excluir manualmente todos os arquivos e pastas temporárias) antes de tentar compilar novamente.

Se o compilador C++ gera "*erro C2039: 'IUnknown': não é um membro da ' \'global namespace '*", em seguida, adicione `#include <unknwn.h>` na parte superior da sua `pch.h` arquivo (antes de incluir qualquer C + c++ cabeçalhos do WinRT).

Você também precisará adicionar `#include <hstring.h>` depois disso.

Se o vinculador C++ produz "*error LNK2019: símbolo externo _WINRT_CanUnloadNow@0 referenciado na função _VSDesignerCanUnloadNow@0 *", em seguida, você pode resolver que adicionando `#define _VSDESIGNER_DONT_LOAD_AS_DLL` para sua `pch.h` arquivo.
