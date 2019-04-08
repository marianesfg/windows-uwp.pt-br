---
description: Notícias e as alterações a C++/WinRT.
title: O que há de novo no C + + c++ /CLI WinRT
ms.date: 01/29/2019
ms.topic: article
keywords: Windows 10, uwp, standard, c + +, cpp, winrt, projeção, notícias, o que do, new
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: cb624a93a010dfe9784cf8c26beed12c6cf2f77d
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57639951"
---
# <a name="whats-new-in-cwinrt"></a>O que há de novo no C + + c++ /CLI WinRT

A tabela a seguir contém notícias e muda para [C + + c++ /CLI WinRT](/windows/uwp/cpp-and-winrt-apis/intro-to-using-cpp-with-winrt) na versão disponível mais recente do SDK do Windows, que é 10.0.17763.0 (Windows 10, versão 1809). Essas alterações também podem estar presentes em versões posteriores do SDK Insider Preview.

## <a name="news-and-changes-in-windows-sdk-version-100177630-windows-10-version-1809"></a>Notícias e as alterações, no Windows SDK versão 10.0.17763.0 (Windows 10, versão 1809)

| Recursos novos ou alterados | Mais informações |
| - | - |
| **Alteração significativa**. Para que ele seja compilado, C + + c++ /CLI WinRT não depende de cabeçalhos do SDK do Windows. | Ver [isolamento dos arquivos de cabeçalho do SDK do Windows](#isolation-from-windows-sdk-header-files), abaixo. |
| O formato de sistema de projeto do Visual Studio foi alterado. | Ver [como redirecionar C + c++ /CLI WinRT projeto para uma versão posterior do SDK do Windows](#how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk), abaixo. |
| Há novas funções e classes base para ajudá-lo a passar um objeto de coleção para uma função de tempo de execução do Windows, ou para implementar seus próprios tipos de coleção e de propriedades de coleção. | Ver [coleções com C + + c++ /CLI WinRT](collections.md). |
| Você pode usar o [{Binding}](/windows/uwp/xaml-platform/binding-markup-extension) extensão de marcação com C + c++ /CLI classes de tempo de execução do WinRT. | Para obter mais informações e exemplos de código, consulte [visão geral da vinculação de dados](/windows/uwp/data-binding/data-binding-quickstart). |
| Suporte ao cancelamento de uma corrotina permite que você registre um retorno de chamada de cancelamento. | Para obter mais informações e exemplos de código, consulte [Cancelando uma operação assíncrona e retornos de chamada de cancelamento](concurrency.md#canceling-an-asychronous-operation-and-cancellation-callbacks). |
| Ao criar um delegado que aponta para uma função de membro, você pode estabelecer um forte ou uma referência fraca ao objeto atual (em vez de um brutos *isso* ponteiro) no ponto em que o manipulador é registrado. | Para obter mais informações e exemplos de código, consulte o **se você usar uma função de membro como representante** subseção na seção [com segurança ao acessar o *isso* ponteiro com um representante de manipulação de eventos](weak-references.md#safely-accessing-the-this-pointer-with-an-event-handling-delegate). |
| Bugs corrigidos que foram revelados por melhoria na conformidade do Visual Studio para o padrão C++. A cadeia de ferramentas do LLVM e Clang melhor também é utilizada para validar C + + / conformidade com os padrões do WinRT. | Você não encontrará o problema descrito no [por que meu novo projeto não será compilado? Usando o Visual Studio 2017 (versão 15.8.0 ou superior) e o SDK versão 17134](faq.md#why-wont-my-new-project-compile-im-using-visual-studio-2017-version-1580-or-higher-and-sdk-version-17134) |

Outras alterações.

- **Alteração significativa**. [**WinRT::get_abi(WinRT::hstring const&)** ](/uwp/cpp-ref-for-winrt/get-abi) agora retorna `void*` em vez de `HSTRING`. Você pode usar `static_cast<HSTRING>(get_abi(my_hstring));` para obter um HSTRING.
- **Alteração significativa**. [**WinRT::put_abi(WinRT::hstring&)** ](/uwp/cpp-ref-for-winrt/put-abi) agora retorna `void**` em vez de `HSTRING*`. Você pode usar `reinterpret_cast<HSTRING*>(put_abi(my_hstring));` para obter um HSTRING *.
- **Alteração significativa**. HRESULT agora é projetada como **winrt::hresult**. Se você precisa de um HRESULT (para a verificação de tipo, ou para dar suporte a características de tipo), você pode `static_cast` uma **winrt::hresult**. Caso contrário, **winrt::hresult** converte em HRESULT, contanto que você inclua `unknwn.h` antes de incluir qualquer C + + c++ /CLI cabeçalhos do WinRT.
- **Alteração significativa**. GUID agora é projetada como **winrt::guid**. Para as APIs que você implementa, você deve usar **winrt::guid** para parâmetros GUID. Caso contrário, **winrt::hresult** converte em GUID, contanto que você inclua `unknwn.h` antes de incluir qualquer C + + c++ /CLI cabeçalhos do WinRT.
- **Alteração significativa**. O [ **winrt::handle_type construtor** ](/uwp/cpp-ref-for-winrt/handle-type#handletypehandletype-constructor) protegeu fazendo explícita (ele agora é mais difícil escrever um código incorreto com ele). Se você precisar atribuir um valor de identificador bruto, chame o [ **função handle_type::attach** ](/uwp/cpp-ref-for-winrt/handle-type#handletypeattach-function) em vez disso.
- **Alteração significativa**. As assinaturas dos **WINRT_CanUnloadNow** e **WINRT_GetActivationFactory** foram alterados. Você não deve declarar essas funções em todos os. Em vez disso, inclua `winrt/base.h` (que é incluído automaticamente se você incluir qualquer C + + c++ /CLI arquivos de cabeçalho de namespace do WinRT Windows) para incluir as declarações de uma dessas funções.
- Para o [ **winrt::clock struct**](/uwp/cpp-ref-for-winrt/clock), **from_FILETIME/to_FILETIME** foram preteridos em favor do **from_file_time/to_file_time**.
- As APIs que esperam **IBuffer** parâmetros são simplificados. Embora a maioria das APIs prefira coleções ou matrizes, suficiente APIs dependem **IBuffer** que precisava ser mais fácil de usar essas APIs do C++. Essa atualização fornece acesso direto aos dados por trás de um **IBuffer** implementação, usando a mesma convenção de nomenclatura de dados usada pelos contêineres da biblioteca padrão C++. Isso também evita o conflito com nomes de metadados que convencionalmente começam com uma letra maiuscula.
- Melhor geração de código: várias melhorias para reduzir o tamanho do código, melhorar o inlining e otimizar o cache de fábrica.
- Removida a recursão desnecessária. Quando a linha de comando refere-se para uma pasta, em vez de um determinado `.winmd`, o `cppwinrt.exe` ferramenta não pesquisa recursivamente para `.winmd` arquivos. O `cppwinrt.exe` ferramenta agora também lida com as duplicatas mais inteligente, tornando mais resiliente a erro de usuário e a mal formado `.winmd` arquivos.
- Proteção avançada de ponteiros inteligentes. Anteriormente, os revokers evento Falha ao revogar quando movem-recebe um novo valor. Isso ajudou a descobrir um problema em que classes de ponteiro inteligente não eram manipular confiavelmente autoatribuição; como raiz a [ **winrt::com_ptr struct modelo**](/uwp/cpp-ref-for-winrt/com-ptr). **WinRT::com_ptr** tiver sido corrigido, e os revokers de evento fixados para lidar com a semântica de movimentação corretamente para que eles revogam após a atribuição.

> [!IMPORTANT]
> Alterações importantes foram feitas para o [C + + c++ /CLI WinRT Visual Studio VSIX (extensão)](https://aka.ms/cppwinrt/vsix), tanto na versão 1.0.181002.2 e em seguida, na versão 1.0.190128.4. Para obter detalhes sobre essas alterações e como eles afetam seus projetos existentes, [suporte do Visual Studio para C + + c++ /CLI WinRT](intro-to-using-cpp-with-winrt.md#visual-studio-support-for-cwinrt-xaml-the-vsix-extension-and-the-nuget-package) e [versões anteriores da extensão do VSIX](intro-to-using-cpp-with-winrt.md#earlier-versions-of-the-vsix-extension).

## <a name="isolation-from-windows-sdk-header-files"></a>Isolamento de arquivos de cabeçalho do SDK do Windows

É possível que haja uma alteração significativa para seu código.

Para que ele seja compilado, C + + c++ /CLI WinRT não depende mais arquivos de cabeçalho do SDK do Windows. Arquivos de cabeçalho na biblioteca em tempo de execução C (CRT) e o C++ STL Standard Template Library () também não incluem todos os cabeçalhos do SDK do Windows. E o que melhora a conformidade com padrões, evita dependências acidentais e reduz significativamente o número de macros que você precisa se proteger contra.

Essa independência significa que c++ /CLI WinRT agora está mais portátil e compatíveis com padrões, e ele amplia a possibilidade de ele se torne uma biblioteca de plataforma cruzada e o compilador cruzado. Isso também significa que o C + + c++ /CLI WinRT cabeçalhos não são afetadas negativamente macros.

Se anteriormente deixado para C + + c++ /CLI WinRT para incluir quaisquer cabeçalhos do Windows em seu projeto, em seguida, você precisará incluí-los agora. Ele é, de qualquer forma, sempre, práticas recomendadas para incluir explicitamente os cabeçalhos que dependem de você e não deixá-lo para outra biblioteca para incluí-los para você.

Atualmente, as únicas exceções para o isolamento de arquivo de cabeçalho do SDK do Windows são intrínsecos e numéricos. Não há nenhum problema conhecido com essas últimas dependências restantes.

Em seu projeto, você pode habilitar novamente a interoperabilidade com os cabeçalhos do SDK do Windows se você precisar. Por exemplo, convém implementar uma interface COM (como raiz [ **IUnknown**](https://msdn.microsoft.com/library/windows/desktop/ms680509)). Para este exemplo, incluir `unknwn.h` antes de incluir qualquer C + + c++ /CLI cabeçalhos do WinRT. Fazendo isso faz com que o C + + / biblioteca de base do WinRT para habilitar vários ganchos dar suporte a interfaces do COM clássico. Para obter um exemplo de código, consulte [componentes COM autor com C + + c++ /CLI WinRT](author-coclasses.md). Da mesma forma, inclua explicitamente outros cabeçalhos de SDK do Windows que declarar tipos de e/ou funções que você deseja chamar.

## <a name="how-to-retarget-your-cwinrt-project-to-a-later-version-of-the-windows-sdk"></a>Como redirecionar C + + / projeto WinRT para uma versão posterior do SDK do Windows

O método para seu projeto que provavelmente resultará na edição de compilador e vinculador menor número de redirecionamento também é o mais trabalhoso. Esse método envolve criar um novo projeto (direcionamento a versão do SDK do Windows de sua escolha) e, em seguida, copiando arquivos para o seu novo projeto do seu antigo. Haverá seções do seu antigo `.vcxproj` e `.vcxproj.filters` arquivos que você pode simplesmente copiar mais de salvar a você adicionar arquivos no Visual Studio.

No entanto, há duas outras maneiras de redirecionar seu projeto no Visual Studio.

- Vá para a propriedade do projeto **gerais** \> **versão do SDK do Windows**e selecione **todas as configurações** e **todas as plataformas**. Definir **versão do SDK do Windows** para a versão que você deseja direcionar.
- Na **Gerenciador de soluções**, clique com botão direito no nó do projeto, clique em **redirecionar projetos**, escolha a versão (ões) que você deseja direcionar e, em seguida, clique em **Okey**.

Se você encontrar qualquer compilador ou erros de vinculador depois de usar qualquer um desses dois métodos, você pode testar a solução de limpeza (**construir** > **limpar solução** e/ou exclua manualmente todos os arquivos e pastas temporárias) antes de tentar compilar novamente.

Se o compilador C++ produz "*erro C2039: 'IUnknown': não é um membro de '\`namespace global '*", em seguida, adicione `#include <unknwn.h>` na parte superior do seu `pch.h` arquivo (antes de incluir qualquer C + + c++ /CLI cabeçalhos WinRT).

Você também precisará adicionar `#include <hstring.h>` depois disso.

Se o vinculador C++ gera "*erro LNK2019: símbolo externo não resolvido _WINRT_CanUnloadNow@0 referenciado na função _VSDesignerCanUnloadNow@0* ", em seguida, você pode resolver que adicionando `#define _VSDESIGNER_DONT_LOAD_AS_DLL` para seu `pch.h` arquivo.
