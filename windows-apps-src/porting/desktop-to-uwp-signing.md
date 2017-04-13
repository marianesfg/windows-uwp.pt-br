---
author: normesta
Description: "Este artigo explica como assinar um aplicativo de área de trabalho convertido para a Plataforma Universal do Windows (UWP)."
Search.Product: eADQiWindows 10XVcnh
title: Assinar Ponte de Desktop para UWP
ms.author: normesta
ms.date: 03/09/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 232c3012-71ff-4f76-a81e-b1758febb596
ms.openlocfilehash: bed23b75a966e3a858d1e34a686fa3ba2730354c
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="desktop-to-uwp-bridge-sign"></a>Ponte de Desktop para UWP: Assinar

Este artigo explica como assinar um aplicativo de área de trabalho convertido para a Plataforma Universal do Windows (UWP). Você deve assinar o pacote de aplicativo do Windows com um certificado antes de implantá-lo.

## <a name="automatically-sign-using-the-desktop-app-converter-dac"></a>Entrar automaticamente usando o Desktop App Converter (DAC)

Use o sinalizador ```-Sign``` ao executar o DAC para entrar automaticamente no pacote de aplicativo do Windows. Para obter mais detalhes, consulte [Visualizar Desktop App Converter](desktop-to-uwp-run-desktop-app-converter.md).

## <a name="manually-sign-using-signtoolexe"></a>Entrar manualmente usando SignTool.exe

Primeiro, crie um certificado usando MakeCert.exe. Se você for solicitado a digitar uma senha, selecione "None".

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

Em seguida, copie suas informações de chaves públicas e privadas para o certificado usando pvk2pfx.exe.

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
Por fim, use SignTool.exe para assinar seu pacote de aplicativo do Windows com o certificado.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

Para obter mais detalhes, consulte [Como assinar um pacote de aplicativo usando a SignTool](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx).

Todas as três ferramentas acima são incluídas no SDK do Microsoft Windows 10. Para chamá-las diretamente, chame o script ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` em um prompt de comando.

## <a name="common-errors"></a>Erros comuns

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

## <a name="see-also"></a>Consulte também

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)
- [SignTool.exe (ferramenta de assinatura)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)
- [Como assinar um pacote do aplicativo usando a SignTool](https://msdn.microsoft.com/library/windows/desktop/jj835835.aspx)
