---
title: Tratar a ativação do arquivo
description: Um aplicativo pode se registrar para ser o manipulador padrão de um determinado tipo de arquivo.
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.date: 07/05/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
dev_langs:
- csharp
- vb
- cppwinrt
- cpp
ms.openlocfilehash: abe77526a7ac12bc905839065913dd59d70fdf62
ms.sourcegitcommit: d2517e522cacc5240f7dffd5bc1eaa278e3f7768
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/30/2018
ms.locfileid: "8324807"
---
# <a name="handle-file-activation"></a>Tratar a ativação do arquivo

**APIs importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

Seu aplicativo pode se registrar para ser o manipulador padrão para um determinado tipo de arquivo. Os aplicativos da área de trabalho do Windows e os aplicativos da Plataforma Universal do Windows (UWP) podem ser registrados como um manipulador de arquivo padrão. Se o usuário escolher seu aplicativo como o manipulador padrão de um determinado tipo de arquivo, seu aplicativo será ativado quando esse tipo de arquivo for iniciado.

Recomendamos que você só se registre para um tipo de arquivo se quiser manipular todas as inicializações desse tipo de arquivo. Se o seu aplicativo precisar usar o tipo de arquivo apenas internamente, então você não precisará se registrar para ser o manipulador padrão. Se você decidir se registrar para um tipo de arquivo, você deve fornecer ao usuário a funcionalidade esperada quando o seu aplicativo é ativado para esse tipo de arquivo. Por exemplo, um aplicativo de visualização de imagens pode se registrar para exibir um arquivo .jpg. Para mais informações sobre associações de arquivos, consulte [Diretrizes de tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

Estas etapas mostram como registrar um tipo de arquivo personalizado, o .alsdk, e como ativar seu aplicativo quando o usuário inicia um arquivo .alsdk.

> **Observação**em aplicativos UWP, determinados URIs e extensões de arquivo são reservados para uso por aplicativos nativos e o sistema operacional. Tentativas de registrar seu aplicativo com um URI ou extensão de arquivo reservada serão ignoradas. Para obter mais informações, consulte [Nomes de arquivos e esquemas de URI reservados](reserved-uri-scheme-names.md).

## <a name="step-1-specify-the-extension-point-in-the-package-manifest"></a>Etapa 1: especificar o ponto de extensão no manifesto do pacote

O aplicativo recebe os eventos de ativação somente para as extensões de arquivo listadas no manifesto do pacote. Veja como indicar se seu aplicativo manipula os arquivos com a extensão `.alsdk`.

1.  No **Gerenciador de Soluções**, clique duas vezes em package.appxmanifest para abrir o designer de manifesto. Selecione a guia **Declarações** no menu suspenso **Declarações disponíveis**, selecione **Associações de tipo de arquivo** e clique em **Adicionar**. Consulte [Identificadores programáticos](https://msdn.microsoft.com/library/windows/desktop/cc144152) para obter mais detalhes sobre os identificadores usados por associações de arquivo.

    Esta é uma breve descrição de cada um dos campos que você pode preencher no designer de manifesto:

| Campo | Descrição |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome de exibição** | Especifique o nome de exibição para um grupo de tipos de arquivos. O nome de exibição é usado para identificar o tipo de arquivo em [Definir Programas Padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154) no **Painel de Controle**. |
| **Logotipo** | Especifique o logotipo que é usado para identificar o tipo de arquivo na área de trabalho e em [Definir Programas Padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154) no **Painel de Controle**. Se nenhum logotipo for especificado, o logotipo pequeno do aplicativo será usado. |
| **Dica de informações** | Especifique a [dica de informações](https://msdn.microsoft.com/library/windows/desktop/cc144152) para um grupo de tipos de arquivo. O texto dessa dica de ferramenta é exibido quando o usuário passa o mouse sobre o ícone de um arquivo desse tipo. |
| **Nome** | Escolha o nome de um grupo de tipos de arquivos que compartilham o mesmo nome de exibição, logotipo, dica de informações e sinalizadores de edição. Escolha um nome de grupo que se mantenha igual entre atualizações de aplicativos. **Observação**  O nome precisa estar completamente em letras minúsculas. |
| **Tipo de conteúdo** | Especifique o tipo de conteúdo MIME, como **image/jpeg**, para um tipo de arquivo específico. **Observação importante sobre tipos de conteúdo permitidos:** esta é uma lista em ordem alfabética dos tipos de conteúdo MIME que você não pode inserir no manifesto do pacote porque eles são reservados ou proibidos: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **Tipo de arquivo** | Especifique o tipo de arquivo para o qual registrar, precedido por um ponto, por exemplo, ".jpeg". **Tipos de arquivos reservados e proibidos:** consulte [Nomes de esquemas de URI e tipos de arquivos reservados](reserved-uri-scheme-names.md) para obter uma lista em ordem alfabética dos tipos de arquivo para apps internos para os quais você não pode registrar seus aplicativos UWP porque são reservados ou proibidos. |

2.  Insira `alsdk` como o **Nome**.
3.  Insira `.alsdk` como o **Tipo de arquivo**.
4.  Insira “images\\Icon.png” como o logotipo.
5.  Pressione Ctrl+S para salvar a alteração no package.appxmanifest.

As etapas acima adicionam um elemento [**Extension**](https://msdn.microsoft.com/library/windows/apps/br211400) semelhante a este ao manifesto do pacote. A categoria **windows.fileTypeAssociation** indica se o aplicativo manipula arquivos com a extensão `.alsdk`.

```xml
      <Extensions>
        <uap:Extension Category="windows.fileTypeAssociation">
          <uap:FileTypeAssociation Name="alsdk">
            <uap:Logo>images\icon.png</uap:Logo>
            <uap:SupportedFileTypes>
              <uap:FileType>.alsdk</uap:FileType>
            </uap:SupportedFileTypes>
          </uap:FileTypeAssociation>
        </uap:Extension>
      </Extensions>
```

## <a name="step-2-add-the-proper-icons"></a>Etapa 2: adicionar os ícones apropriados

Os aplicativos que se tornam padrão para um tipo de arquivo têm seus ícones exibidos em vários lugares em todo o sistema. Por exemplo, esses ícones são mostrados nos seguintes locais:

-   No Modo de Exibição de Itens do Windows Explorer, em menus de contexto e na Faixa de Opções
-   No Painel de Controle de programas padrão
-   Seletor de arquivos
-   Nos Resultados da pesquisa na tela inicial

Inclua um ícone 44x44 com seu projeto para que seu logotipo apareça nesses locais. Combine a aparência do logotipo do bloco do aplicativo e use a cor da tela de fundo do aplicativo em vez de deixar o ícone transparente. Faça o logotipo se estender até a borda sem preenchê-la. Teste seus ícones em telas em segundo plano brancas. Consulte [Diretrizes para ativos de bloco e ícone](https://docs.microsoft.com/windows/uwp/design/shell/tiles-and-notifications/app-assets) para obter mais detalhes sobre ícones.

## <a name="step-3-handle-the-activated-event"></a>Etapa 3: manipular o evento ativado

O manipulador de eventos [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) recebe todos os eventos de ativação de arquivos.

```csharp
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```

```cppwinrt
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs const& args)
{
    // TODO: Handle file activation.
    auto numberOfFilesReceived{ args.Files().Size() };
    auto nameOfTheFirstFile{ args.Files().GetAt(0).Name() };
}
```

```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
    // TODO: Handle file activation
    // The number of files received is args->Files->Size
    // The name of the first file is args->Files->GetAt(0)->Name
}
```

> [!NOTE]
> Quando iniciado por contrato de arquivo, verifique se o que botão Voltar leva o usuário voltar para a tela que iniciou o aplicativo e não o conteúdo do aplicativo anterior.

Recomendamos que você crie um novo XAML **quadro** para cada evento de ativação que abre uma nova página. Dessa forma, o backstack de navegação para o novo quadro XAML não contém nenhum conteúdo anterior que o aplicativo possa ter na janela atual quando suspenso. Se você decidir usar um único **quadro** de XAML para inicialização e contratos de arquivo, você deve limpar as páginas do diário de navegação do **quadro**antes de navegar para uma nova página.

Quando seu aplicativo é iniciado por ativação de arquivo, você deve considerar incluindo da interface do usuário que permite ao usuário voltar para o início da página do aplicativo.

## <a name="remarks"></a>Comentários

Os arquivos recebidos podem vir de uma fonte não confiável. Recomendamos que você valide o conteúdo de um arquivo antes de processá-lo. Para obter mais informações sobre a validação de entrada, consulte [Escrevendo código seguro](http://go.microsoft.com/fwlink/p/?LinkID=142053)

## <a name="related-topics"></a>Tópicos relacionados

### <a name="complete-example"></a>Exemplo completo

* [Amostra de inicialização de associação](http://go.microsoft.com/fwlink/p/?LinkID=231484)

### <a name="concepts"></a>Conceitos

* [Programas padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Tipo de Arquivo e Modelo de Associações de Protocolo](https://msdn.microsoft.com/library/windows/desktop/hh848047)

### <a name="tasks"></a>Tarefas

* [Iniciar o app padrão para um arquivo](launch-the-default-app-for-a-file.md)
* [Tratar a ativação do URI](handle-uri-activation.md)

### <a name="guidelines"></a>Diretrizes

* [Diretrizes para tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

### <a name="reference"></a>Referência
* [Windows.ApplicationModel.Activation.FileActivatedEventArgs](https://msdn.microsoft.com/library/windows/apps/br224716)
* [Windows.UI.Xaml.Application.OnFileActivated](https://msdn.microsoft.com/library/windows/apps/br242331)
