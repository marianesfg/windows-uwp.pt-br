---
ms.assetid: ''
description: Este artigo explica como usar a classe SoftwareBitmap com a Biblioteca de visão de computador do código-fonte aberto (OpenCV).
title: Processar bitmaps com OpenCV
ms.date: 03/19/2018
ms.topic: article
keywords: Windows 10, uwp, opencv, softwarebitmap
ms.localizationpriority: medium
ms.openlocfilehash: 45f76744070a7557939d1d7f2307113852737072
ms.sourcegitcommit: a3dc929858415b933943bba5aa7487ffa721899f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/07/2018
ms.locfileid: "8791758"
---
# <a name="process-bitmaps-with-opencv"></a>Processar bitmaps com OpenCV

Este artigo explica como usar a classe **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)**, que é usada por muitas APIs diferentes da UWP para representar imagens, com a Biblioteca de visão do computador de código-fonte aberto (OpenCV), uma biblioteca de código aberto nativa e livre com uma ampla variedade de algoritmos de processamento de imagem. 

Os exemplos neste artigo explicam como criar um componente do Tempo de execução do Windows para código nativo, que pode ser usado a partir de um aplicativo UWP, incluindo os aplicativos criados usando C#. Esse componente auxiliar expõe um único método, **Desfocar**, que usará a função de processamento de imagem desfocada do OpenCV. O componente implementa métodos particulares que obtêm um ponteiro para o buffer de dados de imagem subjacente, o qual pode ser usado diretamente pela biblioteca do OpenCV, facilitando a extensão do componente auxiliar para implementar outros recursos de processamento de OpenCV. 

* Para ver as noções básicas de uso do **SoftwareBitmap**, consulte [Criar, editar e salvar imagens de bitmap](imaging.md). 
* Para saber como usar a biblioteca OpenCV, acesse [http://opencv.org](http://opencv.org).
* Para ver como usar o componente auxiliar do OpenCV mostrado neste artigo com **[MediaFrameReader](https://docs.microsoft.com/uwp/api/windows.media.capture.frames.mediaframereader)** a fim de implementar o processamento de quadros de imagens em tempo real de uma câmera, consulte [Usar o OpenCV com MediaFrameReader](use-opencv-with-mediaframereader.md).
* Para obter um exemplo de código completo que implementa alguns efeitos diferentes, consulte [Quadros de câmera + Amostra do OpenCV](https://go.microsoft.com/fwlink/?linkid=854003) no repositório do GitHub de amostras universais do Windows.

> [!NOTE] 
> A técnica usada pelo componente OpenCVHelper, descrita em detalhes neste artigo, exige que os dados da imagem a ser processada estejam na memória da CPU em vez da GPU. Portanto, você deve especificar a memória da CPU para APIs que permitem a solicitação da localização das imagens na memória, como a classe **[MediaCapture](https://docs.microsoft.com/uwp/api/windows.media.capture.mediacapture)**.

## <a name="create-a-helper-windows-runtime-component-for-opencv-interop"></a>Criar um componente auxiliar do Tempo de execução do Windows para interoperabilidade de OpenCV

### <a name="1-add-a-new-native-code-windows-runtime-component-project-to-your-solution"></a>1. Adicione um projeto Componente do Tempo de Execução do Windows do novo código nativo à solução

1. Adicione um novo projeto à solução no Visual Studio ao clicar com o botão direito do mouse na sua solução no Gerenciador de Soluções e selecionar **Adicionar->Novo projeto**. 
2. Na categoria **Visual++**, selecione **Componente do Tempo de Execução do Windows (Universal do Windows)**. Neste exemplo, digite um nome para o projeto "OpenCVBridge" e clique em **OK**. 
3. Na caixa de diálogo **Novo projeto universal do Windows**, selecione a versão mínima e de destino do sistema operacional para seu aplicativo e clique em **OK**.
4. Clique com botão direito do mouse no arquivo gerado automaticamente Class1.cpp no Gerenciador de Soluções e selecione **Remover**. Quando a caixa de diálogo de confirmação aparecer, escolha **Excluir**. Em seguida, exclua o arquivo de cabeçalho Class1.h.
5. Clique com botão direito dou mouse no ícone de projeto OpenCVBridge e selecione **Adicionar->Classe...**. Na caixa de diálogo **Adicionar classe**, digite "OpenCVHelper" no campo **Nome da classe** e, em seguida, clique em **OK**. O código será adicionado aos arquivos da classe criada em uma etapa posterior.

### <a name="2-add-the-opencv-nuget-packages-to-your-component-project"></a>2. Adicione os pacotes NuGet do OpenCV ao projeto do componente

1. Clique com o botão direito do mouse no ícone do projeto OpenCVBridge no Explorador de Soluções e selecione **Gerenciar pacotes NuGet...**
2. Quando a caixa de diálogo do Gerenciador de pacotes NuGet abrir, selecione a guia **Buscar** e digite "OpenCV.Win" na caixa de pesquisa.
3. Selecione "OpenCV.Win.Core" e clique em **Instalar**. Na caixa de diálogo **Visualização**, clique em **OK**.
4. Use o mesmo procedimento para instalar o pacote "OpenCV.Win.ImgProc".

> [!NOTE]
> OpenCV.Win.Core e OpenCV.Win. ImgProc não são atualizadas regularmente, mas ainda são recomendados para a criação de um OpenCVHelper conforme descrito nesta página.

### <a name="3-implement-the-opencvhelper-class"></a>3. Implemente a classe OpenCVHelper

Cole o seguinte código no arquivo de cabeçalho OpenCVHelper.h. Esse código inclui arquivos de cabeçalho OpenCV para os pacotes *Core* e *ImgProc* instalados e declara três métodos que serão mostrados nas etapas a seguir.

[!code-cpp[OpenCVHelperHeader](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.h#SnippetOpenCVHelperHeader)]

Exclua o conteúdo do arquivo OpenCVHelper.cpp atual e adicione as seguintes diretivas de inclusão. 

[!code-cpp[OpenCVHelperInclude](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperInclude)]

Após as diretivas de inclusão, adicione as seguintes diretivas de **uso**. 

[!code-cpp[OpenCVHelperUsing](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperUsing)]

Em seguida, adicione o método **GetPointerToPixelData** a OpenCVHelper.cpp. Esse método aceita um **[SoftwareBitmap](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.SoftwareBitmap)** e, por meio de uma série de conversões, obtém uma representação da interface COM dos dados de pixel, por meio da qual podemos obter um ponteiro para o buffer de dados subjacente como uma matriz de **char**. 

Primeiro um **[BitmapBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer)** contendo os dados de pixel é obtido ao chamar **[LockBuffer](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.lockbuffer)**, solicitando um buffer de leitura/gravação para que a biblioteca OpenCV possa modificar esses dados de pixel.  **[CreateReference](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapbuffer.CreateReference)** é chamado para obter um objeto **[IMemoryBufferReference](https://docs.microsoft.com/uwp/api/windows.foundation.imemorybufferreference)**. Em seguida, a interface de **IMemoryBufferByteAccess** é convertida como um **IInspectable**, a interface base de todas as classes do Windows Runtime, e **[QueryInterface](https://msdn.microsoft.com/library/windows/desktop/ms682521(v=vs.85).aspx)** é chamado para obter uma interface COM de **[IMemoryBufferByteAccess](https://msdn.microsoft.com/library/mt297505(v=vs.85).aspx)**, que nos permite obter o buffer de dados de pixel como uma matriz de **char**. Por fim, preencha a matriz de **char** ao chamar **[IMemoryBufferByteAccess::GetBuffer](https://msdn.microsoft.com/library/mt297506(v=vs.85).aspx)**. Se ocorrer uma falha em qualquer uma das etapas de conversão neste método, ele retorna **false**, indicando que não é possível fazer mais processamentos.

[!code-cpp[OpenCVHelperGetPointerToPixelData](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperGetPointerToPixelData)]

Em seguida, adicione o método **TryConvert** mostrado abaixo. Esse método aceita um **SoftwareBitmap** e tenta convertê-lo em um objeto **Mat**, que é o objeto de matriz usado pelo OpenCV para representar os buffers de dados de imagem. Esse método chama o método **GetPointerToPixelData** definido acima para obter uma representação da matriz de **char** do buffer de dados de pixel. Em caso de sucesso, o construtor da classe **Mat** é chamado, passando a largura e a altura dos pixels obtidas do objeto **SoftwareBitmap** de origem. 

> [!NOTE] 
> O exemplo especifica a constante CV_8UC4 como o formato de pixel do objeto **Mat** criado. Ou seja, o **SoftwareBitmap** passado para esse método deve ter um valor de propriedade **[BitmapPixelFormat](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapPixelFormat)** de **[BGRA8](https://docs.microsoft.com/uwp/api/Windows.Graphics.Imaging.BitmapPixelFormat)** com alfa pré-multiplicado, o equivalente de CV_8UC4, para trabalhar com este exemplo.

Uma cópia superficial do objeto **Mat** criado é retornada do método, então o processamento adicional opera no mesmo buffer de dados de pixel dos dados consultados por **SoftwareBitmap** e não uma cópia desse buffer.

[!code-cpp[OpenCVHelperTryConvert](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperTryConvert)]

Por fim, essa classe auxiliar de exemplo implementa um método de processamento de imagem único, **Desfocar**, que usa apenas o método **TryConvert** definido acima para recuperar um objeto **Mat** que representa o bitmap de origem e o bitmap de destino da operação de desfoque e, em seguida, chama o método **desfocar** da biblioteca do OpenCV ImgProc. O outro parâmetro para **desfocar** especifica o tamanho do efeito de desfoque nas direções X e Y.

[!code-cpp[OpenCVHelperBlur](./code/ImagingWin10/cs/OpenCVBridge/OpenCVHelper.cpp#SnippetOpenCVHelperBlur)]


## <a name="a-simple-softwarebitmap-opencv-example-using-the-helper-component"></a>Um exemplo simples de SoftwareBitmap OpenCV usando o componente auxiliar
Agora que o componente OpenCVBridge foi criado, podemos criar um aplicativo C# simples que usa o método **desfocar** de OpenCV para modificar um **SoftwareBitmap**. Para acessar o componente de tempo de execução do Windows do aplicativo UWP, primeiro você deve adicionar uma referência ao componente. No Gerenciador de Soluções, clique com botão direito do mouse no nó **Referências** em seu projeto de aplicativo UWP e selecione **Adicionar referência...**. Na caixa de diálogo Gerenciador de referências, selecione **Projetos -> Solução**. Marque a caixa ao lado do seu projeto OpenCVBridge e clique em **OK**.

O código de exemplo abaixo permite que o usuário selecione um arquivo de imagem e, em seguida, usa **[BitmapDecoder](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.bitmapencoder)** para criar uma representação de **SoftwareBitmap** da imagem. Para obter mais informações sobre como trabalhar com **SoftwareBitmap**, consulte [Criar, editar e salvar imagens de bitmap](https://docs.microsoft.com/windows/uwp/audio-video-camera/imaging).

Conforme discutido anteriormente neste artigo, a classe **OpenCVHelper** exige que todas as imagens **SoftwareBitmap** fornecidas sejam codificadas usando o formato de pixel BGRA8 com valores de alfa pré-multiplicado, portanto, se a imagem ainda não estiver nesse formato, o código de exemplo chama **[Converter](https://docs.microsoft.com/uwp/api/windows.graphics.imaging.softwarebitmap.BitmapAlphaMode)** para converter a imagem no formato esperado.

Em seguida, um **SoftwareBitmap** é criado para ser usado como o destino da operação de desfoque. As propriedades da imagem de entrada são usadas como argumentos para o construtor a fim de criar um bitmap com o formato de correspondência.

Uma nova instância do **OpenCVHelper** é criada e o método **Desfocar** é chamado, passando os bitmaps de origem e destino. Por fim, um **SoftwareBitmapSource** é criado para atribuir a imagem de saída ao controle de **Imagem** XAML.


[!code-cs[OpenCVBlur](./code/ImagingWin10/cs/MainPage.OpenCV.xaml.cs#SnippetOpenCVBlur)]

## <a name="related-topics"></a>Tópicos relacionados

* [Referência de opções de BitmapEncoder](bitmapencoder-options-reference.md)
* [Metadados de imagem](image-metadata.md)
 

 




