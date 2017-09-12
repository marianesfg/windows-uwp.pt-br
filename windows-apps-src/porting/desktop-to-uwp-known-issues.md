---
author: normesta
Description: "Este artigo contém problemas conhecidos com a Ponte de Desktop."
Search.Product: eADQiWindows 10XVcnh
title: Problemas conhecidos (Ponte de Desktop)
ms.author: normesta
ms.date: 07/18/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 71f8ffcb-8a99-4214-ae83-2d4b718a750e
ms.openlocfilehash: bf0e81d4a6ff7bd091f25963b1cf26cdecc93636
ms.sourcegitcommit: de6bc8acec2cd5ebc36bb21b2ce1a9980c3e78b2
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/17/2017
---
# <a name="known-issues-desktop-bridge"></a>Problemas conhecidos (Ponte de Desktop)

Este artigo contém problemas conhecidos com a Ponte de Desktop.

## <a name="blue-screen-with-error-code-0x139-kernelsecuritycheckfailure"></a>Tela azul com código de erro 0x139 (KERNEL_SECURITY_CHECK_FAILURE)

Depois da instalação ou da inicialização de determinados aplicativos da Windows Store, o computador poderá ser reiniciado inesperadamente com o erro: **0x139 (KERNEL\_SECURITY\_CHECK\_ FAILURE)**.

Entre os aplicativos afetados conhecidos estão Kodi, JT2Go, Ear Trumpet, Teslagrad e outros.

Uma [atualização do Windows (versão 14393.351 - KB3197954)](https://support.microsoft.com/kb/3197954) foi lançada em 27/10/16 incluindo correções importantes que resolvem esse problema. Se você enfrentar esse problema, atualize o computador. Se não conseguir atualizar o computador porque ele reinicia antes de você conseguir fazer logon, você deverá usar a restauração do sistema para restaurar o sistema até um ponto anterior ao momento em que instalou um dos aplicativos afetados. Para obter informações sobre como usar a restauração do sistema, consulte [Opções de recuperação no Windows 10](https://support.microsoft.com/help/12415/windows-10-recovery-options).

Se a atualização não corrigir o problema ou se você não souber como recuperar o computador, entre em contato com o [Suporte da Microsoft](https://support.microsoft.com/contactus/).

Se for um desenvolvedor, você desejará impedir a instalação dos aplicativos da ponte da área de trabalho em versões do Windows que não incluam essa atualização. Com isso o aplicativo não estará disponível para os usuários que ainda não tenham instalado a atualização. Para limitar a disponibilidade do aplicativo para usuários que tenham instalado essa atualização, modifique o arquivo AppxManifest.xml da seguinte maneira:

```<TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14393.351" MaxVersionTested="10.0.14393.351"/>```

Os detalhes a respeito do Windows Update podem ser encontrados em:
* https://support.microsoft.com/kb/3197954
* https://support.microsoft.com/help/12387/windows-10-update-history

## <a name="common-errors-that-can-appear-when-you-sign-your-app"></a>Erros comuns que podem aparecer quando você assina seu aplicativo

### <a name="publisher-and-cert-mismatch-causes-signtool-error-error-signersign-failed--21470248850x8007000b"></a>O fornecedor e algumas incompatibilidades causam um erro Signtool "Error: SignerSign() Failed" (-2147024885/0x8007000b)

A entrada de Fornecedor no manifesto do pacote de aplicativo do Windows deve corresponder ao Assunto do certificado que você está assinando.  Você pode usar qualquer um dos seguintes métodos para ver o assunto do certificado.

**Opção 1: Powershell**

Execute o seguinte comando do PowerShell: Ambos .cer ou .pfx pode ser usado como o arquivo de certificado, pois eles têm as mesmas informações de fornecedor.

```ps
(Get-PfxCertificate <cert_file>).Subject
```

**Opção 2: Explorador de Arquivos**

Clique duas vezes no certificado no Explorador de Arquivos, selecione a guia *Detalhes* e depois o campo *Assunto* na lista. Em seguida, você pode copiar o conteúdo.

**Opção 3: CertUtil**

Execute o **certutil** na linha de comando no arquivo PFX e copie o campo *Assunto* da saída.

```cmd
certutil -dump <cert_file.pfx>
```

### <a name="corrupted-or-malformed-authenticode-signatures"></a>Assinaturas Authenticode corrompidas ou malformadas

Esta seção contém detalhes sobre como identificar problemas com arquivos PE em seu pacote de aplicativo do Windows que pode conter assinaturas Authenticode corrompidas ou malformadas. As assinaturas Authenticode inválidas em seus arquivos PE, que podem estar em qualquer formato binário (por exemplo, .exe,. dll,. chm, etc.), impedirão que o pacote seja assinado adequadamente e, portanto, impedirá que ele seja implantado de um pacote de aplicativo do Windows.

O local da assinatura Authenticode de um arquivo PE é especificado pela entrada da tabela de certificados nos diretórios de dados de cabeçalho opcionais e na tabela de certificados de atributos associados. Durante a verificação da assinatura, as informações especificadas nessas estruturas são usadas para localizar a assinatura em um arquivo PE. Se esses valores forem corrompidos, será possível que um arquivo pareça estar assinado de modo inválido.

Para a assinatura Authenticode estar correta, o seguinte deverá ser verdadeiro para a assinatura Authenticode:

- O início da entrada **WIN_CERTIFICATE** no arquivo PE não pode ultrapassar o final do executável
- A entrada **WIN_CERTIFCATE** deve estar localizada no final da imagem
- O tamanho da entrada **WIN_CERTIFICATE** deve ser positiva
- A entrada **WIN_CERTIFICATE** deve ser iniciada após a estrutura **IMAGE_NT_HEADERS32** para executáveis de 32 bits e a estrutura IMAGE_NT_HEADERS64 para executáveis de 64 bits

Para obter mais detalhes, consulte a [Especificação de Authenticode Portal Executable](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) e a [Especificação de formato de arquivo PE](https://msdn.microsoft.com/windows/hardware/gg463119.aspx).

Observe que SignTool.exe pode gerar uma lista dos binários corrompidos ou malformados ao tentar assinar um pacote de aplicativo do Windows. Para fazer isso, ative o registro detalhado, definindo a variável de ambiente APPXSIP_LOG como 1 (por exemplo, ```set APPXSIP_LOG=1``` ) e execute novamente SignTool.exe.

Para corrigir esses binários malformados, certifique-se de que eles estejam em conformidade com os requisitos acima.

<span id="known-issues-anchor" />
## <a name="known-issues-with-cvbnet-and-c-uwp-projects"></a>Problemas conhecidos com projetos C# / VB.NET e C++ UWP

Se preferir usar um projeto C# para empacotar o aplicativo, você precisa estar ciente dos seguintes problemas conhecidos.

- A criação do app em modo Depuração resulta no erro: _Microsoft.Net.CoreRuntime.targets(235,5): erro: Não há suporte para aplicativos com arquivos executáveis do ponto de entrada personalizados. Verifique o atributo Executable do elemento Application no manifesto do pacote._

  Para resolver esse problema, use o modo de Versão.

- Binários Win32 armazenados na pasta raiz do projeto UWP são removidos em Versão. O compilador .NET Native irá removê-los do pacote final, resultando em um erro de validação do manifesto, uma vez que o ponto de entrada executável não pode ser encontrado.

  Para resolver esse problema, crie uma subpasta em seu projeto para armazenar os binários win32.


## <a name="you-receive-the-error----msb4018-the-generateresource-task-failed-unexpectedly"></a>Você recebe o erro MSB4018 A tarefa "GenerateResource" falhou inesperadamente

Isso poderá acontecer quando você estiver tentando converter assemblies de satélite em arquivos PRI (Índice de Recurso do Pacote).

Estamos cientes desse problema e estamos trabalhando em um solução de longo prazo. Como uma solução temporária, você pode desabilitar o gerador de recursos, adicionando esta linha de XML ao primeiro elemento PropertyGroup do arquivo de projeto de hospedagem:

``<AppxGeneratePrisForPortableLibrariesEnabled>false</AppxGeneratePrisForPortableLibrariesEnabled>``
