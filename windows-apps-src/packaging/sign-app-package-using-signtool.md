---
title: Assinar um pacote de aplicativos usando a SignTool
description: Use SignTool para assinar um pacote de aplicativos com um certificado manualmente.
ms.date: 09/30/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: 171f332d-2a54-4c68-8aa0-52975d975fb1
ms.localizationpriority: medium
ms.openlocfilehash: 6a6d39a78ba73dcb598f209ea48c4b131e375ab6
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8922608"
---
# <a name="sign-an-app-package-using-signtool"></a>Assinar um pacote de aplicativos usando a SignTool


**SignTool** é uma ferramenta de linha de comando usada para assinar digitalmente um pacote ou lote de aplicativos com um certificado. O certificado pode ser criado pelo usuário (para fins de teste) ou emitido por uma empresa (para distribuição). Assinar um pacote do aplicativo fornece ao usuário uma verificação de que os dados do aplicativo não foram modificados depois que ele foi assinado enquanto também confirma a identidade do usuário ou empresa que assinou. **SignTool** pode assinar pacotes e lotes de aplicativos criptografados ou não criptografados.

> [!IMPORTANT] 
> Se você usou o Visual Studio para desenvolver seu aplicativo, é recomendável que você use o Assistente do Visual Studio para criar e assinar seu pacote de aplicativo. Para obter mais informações, consulte [Empacotar um aplicativo UWP com Visual Studio](https://msdn.microsoft.com/windows/uwp/packaging/packaging-uwp-apps).

Para obter mais informações sobre assinatura de código e certificados em geral, consulte [Introdução à Assinatura de Código](https://msdn.microsoft.com/library/windows/desktop/aa380259.aspx#introduction_to_code_signing).

## <a name="prerequisites"></a>Pré-requisitos
- **Um aplicativo empacotado**  
    Para saber mais sobre como criar manualmente um pacote de aplicativos, consulte [Criar um pacote do aplicativo com a ferramenta MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool). 

- **Um certificado de assinatura válido**  
    Para obter mais informações sobre como criar ou importar um certificado válido de assinatura, consulte [Criar ou importar um certificado de assinatura de pacote](https://msdn.microsoft.com/windows/uwp/packaging/create-certificate-package-signing).

- **SignTool.exe**  
    Dependendo do seu caminho de instalação do SDK, a **SignTool** é instalada no computador Windows 10 nos seguintes locais:
    - x86: C:\Arquivos de Programas (x86)\Windows Kits\10\bin\x86\SignTool.exe
    - x64: C:\Arquivos de Programas (x86)\Windows Kits\10\bin\x64\SignTool.exe

## <a name="using-signtool"></a>Usando o SignTool

**SignTool** pode ser usado para assinar arquivos, verificar assinaturas ou carimbos de data e hora, remover assinaturas e muito mais. Para a finalidade de assinar um pacote de aplicativos, destacaremos o comando **sign**. Para obter informações completas sobre o **SignTool**, consulte a página de referência do [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx). 

### <a name="determine-the-hash-algorithm"></a>Determinar o algoritmo de hash
Ao usar o **SignTool** para assinar seu pacote ou lote de aplicativos, o algoritmo de hash usado em **SignTool** deve ser o mesmo algoritmo usado para empacotar seu aplicativo. Por exemplo, se você usou o **MakeAppx.exe** para criar o pacote de aplicativos com as configurações padrão, você deve especificar SHA256 ao usar o **SignTool** já que é o algoritmo padrão usado pelo **MakeAppx.exe**.

Para descobrir qual algoritmo de hash foi usado durante o empacotamento de seu aplicativo, extraia o conteúdo do pacote de aplicativos e inspecione o arquivo AppxBlockMap.xml. Para saber como descompactar/extrair um pacote de aplicativos, consulte [Extrair arquivos de um pacote ou lote](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool#extract-files-from-a-package-or-bundle). O método de hash está no elemento BlockMap e tem este formato:
```
<BlockMap xmlns="http://schemas.microsoft.com/appx/2010/blockmap" 
HashMethod="http://www.w3.org/2001/04/xmlenc#sha256">
```

Esta tabela mostra cada valor de HashMethod e seu algoritmo de hash correspondente:


| Valor de HashMethod                              | Algoritmo de hash |
|-----------------------------------------------|----------------|
| http://www.w3.org/2001/04/xmlenc#sha256       | SHA256         |
| http://www.w3.org/2001/04/xmldsig-more#sha384 | SHA384         |
| http://www.w3.org/2001/04/xmlenc#sha512       | SHA512         |

> [!NOTE]
> Como o algoritmo padrão do **SignTool** é SHA1 (não disponível no **MakeAppx.exe**), você deve sempre especificar um algoritmo de hash ao usar o **SignTool**.

### <a name="sign-the-app-package"></a>Assinar o pacote de aplicativos

Depois que você tiver todos os pré-requisitos e determinar qual algoritmo de hash foi usado para empacotar seu aplicativo, você estará pronto para assiná-lo. 

A sintaxe de linha de comando geral para a assinatura de pacote do **SignTool** é:
```
SignTool sign [options] <filename(s)>
```

O certificado usado para assinar seu aplicativo deve ser um arquivo .pfx ou ser instalado em um repositório de certificados.

Para assinar o pacote de aplicativos com um certificado de um arquivo .pfx, use a seguinte sintaxe:
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /a /f <Path to Certificate>.pfx /p <Your Password> <File path>.msix
```
Observe que a opção `/a` permite que o **SignTool** escolha o melhor certificado automaticamente.

Se o certificado não for um arquivo .pfx, use a seguinte sintaxe:
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /n <Name of Certificate> <File Path>.msix
```

Como alternativa, você pode especificar o hash SHA1 do certificado desejado, em vez do &lt;Nome do Certificado&gt; usando esta sintaxe:
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.appx
```
```
SignTool sign /fd <Hash Algorithm> /sha1 <SHA1 hash> <File Path>.msix
```

Observe que alguns certificados não usam uma senha. Se o certificado não tiver uma senha, omita "/p &lt;Sua Senha&gt;" dos comandos de amostra.

Depois que o pacote de aplicativos é assinado com um certificado válido, você está pronto para carregar o pacote para a Loja. Para obter mais orientações sobre como carregar e enviar aplicativos para a Loja, consulte [Envios de aplicativos](https://msdn.microsoft.com/windows/uwp/publish/app-submissions).

## <a name="common-errors-and-troubleshooting"></a>Erros comuns e solução de problemas
Os tipos mais comuns de erros ao usar o **SignTool** são internos e normalmente se parecem com isso:

```
SignTool Error: An unexpected internal error has occurred.
Error information: "Error: SignerSign() failed." (-2147024885 / 0x8007000B) 
```

Se o código de erro começa com 0x8008, como 0x80080206 (APPX_E_CORRUPT_CONTENT), o pacote que está sendo assinado não é válido. Se você receber esse tipo de erro, você deve recriar o pacote e executar o **SignTool** novamente.

O **SignTool** tem uma opção de depuração disponível para mostrar erros de certificado e filtragem. Para usar o recurso de depuração, coloque a opção `/debug` diretamente após `sign`, seguida pelo comando **SignTool** completo.
```
SignTool sign /debug [options]
``` 

Um erro mais comum é o 0x8007000B. Para esse tipo de erro, você pode encontrar mais informações no log de eventos.
 
Para obter mais informações no log de eventos:
- Execute Eventvwr.msc
- Abra o log de eventos: Visualizador de Eventos (Local) -> Aplicativos e Logs de Serviços -> Microsoft -> Windows -> AppxPackagingOM -> Microsoft-Windows-AppxPackaging/Operational
- Localize o evento de erro mais recente

O erro interno 0x8007000B geralmente corresponde a um destes valores:

| **ID do evento** | **Cadeia de caracteres de evento de exemplo** | **Sugestão** |
|--------------|--------------------------|----------------|
| 150          | Erro 0x8007000B: O nome do editor de manifesto de aplicativo (CN = Contoso) deve coincidir com o nome do requerente do certificado de autenticação (CN = Contoso, C = US). | O nome do editor de manifesto de aplicativo deve corresponder exatamente ao nome do assunto após a assinatura.               |
| 151          | Erro 0x8007000B: O método de hash de assinatura especificado (SHA512) deve coincidir com o método de hash usado no mapa de blocos do pacote de aplicativos (SHA256).     | O hashAlgorithm especificado no parâmetro /fd está incorreto. Execute o **SignTool** usando o hashAlgorithm que corresponda ao mapa de blocos do pacote de aplicativos (usado para criar o pacote de aplicativos)  |
| 152          | Erro 0x8007000B: O conteúdo do pacote de aplicativos deve ser validado em relação ao mapa de blocos.                                                           | O pacote de aplicativos está corrompido e precisa ser recompilado para gerar um novo mapa de blocos. Para saber mais sobre como criar um pacote de aplicativos, consulte [Criar um pacote de aplicativos com a ferramenta MakeAppx.exe](https://msdn.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool). |