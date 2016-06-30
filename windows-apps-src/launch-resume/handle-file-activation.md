---
author: TylerMSFT
title: "Manipular a ativação de arquivos"
description: "Um aplicativo pode se registrar para ser o manipulador padrão de um determinado tipo de arquivo."
ms.assetid: A0F914C5-62BC-4FF7-9236-E34C5277C363
ms.sourcegitcommit: 39a012976ee877d8834b63def04e39d847036132
ms.openlocfilehash: 9c6358bccdea55a7c3749388c35aa770f4325960

---

# Manipular a ativação de arquivos


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


**APIs importantes**

-   [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
-   [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

Um aplicativo pode se registrar para ser o manipulador padrão de um determinado tipo de arquivo. Os aplicativos da Plataforma Clássica do Windows (CWP) e os aplicativos da Plataforma Universal do Windows (UWP) podem ser registrados como um manipulador de arquivo padrão. Se o usuário escolher seu aplicativo como o manipulador padrão de um determinado tipo de arquivo, seu aplicativo será ativado quando esse tipo de arquivo for iniciado.

Recomendamos que você só se registre para um tipo de arquivo se quiser manipular todas as inicializações desse tipo de arquivo. Se o seu aplicativo precisar usar o tipo de arquivo apenas internamente, então você não precisará se registrar para ser o manipulador padrão. Se você decidir se registrar para um tipo de arquivo, você deve fornecer ao usuário a funcionalidade esperada quando o seu aplicativo é ativado para esse tipo de arquivo. Por exemplo, um aplicativo de visualização de imagens pode se registrar para exibir um arquivo .jpg. Para mais informações sobre associações de arquivos, consulte [Diretrizes de tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321).

Estas etapas mostram como registrar um tipo de arquivo personalizado, o .alsdk, e como ativar seu aplicativo quando o usuário inicia um arquivo .alsdk.

> **Observação**  Nos aplicativos UWP, determinados URIs e extensões de arquivo são reservados para uso por aplicativos nativos e pelo sistema operacional. Tentativas de registrar seu aplicativo com um URI ou extensão de arquivo reservada serão ignoradas. Para obter mais informações, consulte [Nomes de arquivos e esquemas de URI reservados](reserved-uri-scheme-names.md).

## Etapa 1: especificar o ponto de extensão no manifesto do pacote


O aplicativo recebe os eventos de ativação somente para as extensões de arquivo listadas no manifesto do pacote. Veja como indicar se seu aplicativo manipula os arquivos com a extensão `.alsdk`.

1.  No **Gerenciador de Soluções**, clique duas vezes em package.appxmanifest para abrir o designer de manifesto. Selecione a guia **Declarações** no menu suspenso **Declarações disponíveis**, selecione **Associações de tipo de arquivo** e clique em **Adicionar**. Consulte [Identificadores programáticos](https://msdn.microsoft.com/library/windows/desktop/cc144152) para obter mais detalhes sobre os identificadores usados por associações de arquivo.

    Esta é uma breve descrição de cada um dos campos que você pode preencher no designer de manifesto:

| Campo | Descrição |
|------------------|----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| **Nome de exibição** | Especifique o nome de exibição para um grupo de tipos de arquivos. O nome de exibição é usado para identificar o tipo de arquivo em [Definir Programas Padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154) no **Painel de Controle**. |
| **Logotipo** | Especifique o logotipo que é usado para identificar o tipo de arquivo na área de trabalho e em [Definir Programas Padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154) no **Painel de Controle**. Se nenhum logotipo for especificado, o logotipo pequeno do aplicativo será usado. |
| **Dica de informações** | Especifique a [dica de informações](https://msdn.microsoft.com/library/windows/desktop/cc144152) para um grupo de tipos de arquivo. O texto dessa dica de ferramenta é exibido quando o usuário passa o mouse sobre o ícone de um arquivo desse tipo. |
| **Nome** | Escolha o nome de um grupo de tipos de arquivos que compartilham o mesmo nome de exibição, logotipo, dica de informações e sinalizadores de edição. Escolha um nome de grupo que se mantenha igual entre atualizações de aplicativos. **Observação**  O nome precisa estar completamente em letras minúsculas. |
| **Tipo de conteúdo** | Especifique o tipo de conteúdo MIME, como **image/jpeg**, para um tipo de arquivo específico. **Observação importante sobre tipos de conteúdo permitidos: **está é uma lista em ordem alfabética dos tipos de conteúdo MIME que você não pode inserir no manifesto do pacote porque eles são reservados ou proibidos: **application/force-download**, **application/octet-stream**, **application/unknown**, **application/x-msdownload**. |
| **Tipo de arquivo** | Especifique o tipo de arquivo para o qual registrar, precedido por um ponto, por exemplo, ".jpeg". **Tipos de arquivos reservados e proibidos** Consulte [Nomes de esquemas de URI e tipos de arquivos reservados](reserved-uri-scheme-names.md) para obter uma lista, em ordem alfabética, dos tipos de arquivo para aplicativos internos para os quais você não pode registrar seus aplicativos UWP porque são reservados ou proibidos. |

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

## Etapa 2: adicionar os ícones apropriados


Os aplicativos que se tornam padrão para um tipo de arquivo têm seus ícones exibidos em vários lugares em todo o sistema. Por exemplo, esses ícones são mostrados nos seguintes locais:

-   No Modo de Exibição de Itens do Windows Explorer, em menus de contexto e na Faixa de Opções
-   No Painel de Controle de programas padrão
-   Seletor de arquivos
-   Nos Resultados da pesquisa na tela inicial

Combine a aparência do logotipo do bloco do aplicativo e use a cor de fundo do aplicativo em vez de deixar o ícone transparente. Faça o logotipo se estender até a borda sem preenchê-la. Teste seus ícones em telas em segundo plano brancas. Para ícones de exemplo, consulte o [Amostra de inicialização de associação](http://go.microsoft.com/fwlink/p/?LinkID=620490).
![o gerenciador de soluções com uma exibição dos arquivos na pasta imagens. Há 16, 32, 48 e 256 versões de pixels de 'icon.targetsize' e 'smalltile-sdk'](images/seviewofimages.png)

## Etapa 3: manipular o evento ativado


O manipulador de eventos [**OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331) recebe todos os eventos de ativação de arquivos.

> [!div class="tabbedCodeSnippets"]
```vb
Protected Overrides Sub OnFileActivated(ByVal args As Windows.ApplicationModel.Activation.FileActivatedEventArgs)
      ' TODO: Handle file activation
      ' The number of files received is args.Files.Size
      ' The name of the first file is args.Files(0).Name
End Sub
```
```cpp
void App::OnFileActivated(Windows::ApplicationModel::Activation::FileActivatedEventArgs^ args)
{
       // TODO: Handle file activation
       // The number of files received is args->Files->Size
       // The first file is args->Files->GetAt(0)->Name
}
```
```cs
protected override void OnFileActivated(FileActivatedEventArgs args)
{
       // TODO: Handle file activation
       // The number of files received is args.Files.Size
       // The name of the first file is args.Files[0].Name
}
```

    > **Note**  When launched via File Contract, make sure that Back button takes the user back to the screen that launched the app and not to the app's previous content.

É recomendado que os aplicativos criem um novo Quadro XAML para cada evento de ativação que abre uma nova página. Dessa forma, o backstack de navegação do novo Quadro XAML não terá nenhum conteúdo anterior que o aplicativo possa ter na janela atual quando suspenso. Aplicativos que decidem usar um único Quadro XAML para Contrato de inicialização e arquivos devem limpar as páginas do diário de navegação do Quadro antes de navegar para uma nova página.

Quando iniciado por ativação de Arquivo, os aplicativos devem considerar incluir uma interface do usuário que permita ao usuário voltar para o início da página do aplicativo.

## Comentários


Os arquivos recebidos podem vir de uma fonte não confiável. Recomendamos que você valide o conteúdo de um arquivo antes de processá-lo. Para obter mais informações sobre a validação de entrada, consulte [Escrevendo código seguro](http://go.microsoft.com/fwlink/p/?LinkID=142053)

> **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).

 

## Tópicos relacionados

**Exemplo completo**

* [Amostra de inicialização de associação](http://go.microsoft.com/fwlink/p/?LinkID=231484)

**Conceitos**

* [Programas padrão](https://msdn.microsoft.com/library/windows/desktop/cc144154)
* [Tipo de Arquivo e Modelo de Associações de Protocolo](https://msdn.microsoft.com/library/windows/desktop/hh848047)

**Tarefas**

* [Iniciar o aplicativo padrão para um arquivo](launch-the-default-app-for-a-file.md)
* [Manipular a ativação do URI](handle-uri-activation.md)

**Diretrizes**

* [Diretrizes de tipos de arquivos e URIs](https://msdn.microsoft.com/library/windows/apps/hh700321)

**Referência**
* [**Windows.ApplicationModel.Activation.FileActivatedEventArgs**](https://msdn.microsoft.com/library/windows/apps/br224716)
* [**Windows.UI.Xaml.Application.OnFileActivated**](https://msdn.microsoft.com/library/windows/apps/br242331)

 

 



<!--HONumber=Jun16_HO4-->


