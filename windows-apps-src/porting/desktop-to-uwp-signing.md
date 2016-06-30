# Assinar o aplicativo da área de trabalho convertido

Este artigo explica como assinar um aplicativo de área de trabalho convertido para a Plataforma Universal do Windows (UWP). Você deve assinar o pacote .appx com um certificado antes de implantá-lo.

## Assinar seu .appx

Primeiro, crie um certificado usando MakeCert.exe. Se você for solicitado a digitar uma senha, selecione "None". 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
```

Em seguida, copie suas informações de chaves públicas e privadas para o certificado usando pvk2pfx.exe. 

```cmd
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
```
Por fim, use SignTool.exe para assinar seu .appx com o certificado.

```cmd
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
``` 

Para obter mais detalhes, consulte [Como assinar um pacote de aplicativo usando a SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx). 

Todas as três ferramentas acima são incluídas no SDK do Microsoft Windows 10. Para chamá-las diretamente, chame o script ```C:\Program Files (x86)\Microsoft Visual Studio 14.0\Common7\Tools\VsDevCmd.bat``` em um prompt de comando.

## Erros comuns

### Assinaturas Authenticode corrompidas ou malformadas

Esta seção contém detalhes sobre como identificar problemas com arquivos PE em seu pacote AppX que pode conter assinaturas Authenticode corrompidas ou malformadas. Assinaturas Authenticode inválidas em seus arquivos PE, que podem estar em qualquer formato binário (por exemplo, .exe,. dll,. chm, etc.), impedirão que o pacote seja assinado adequadamente e, portanto, impedirá que ele seja implantado de um pacote AppX. 

O local da assinatura Authenticode de um arquivo PE é especificado pela entrada da tabela de certificados nos diretórios de dados de cabeçalho opcionais e na tabela de certificados de atributos associados. Durante a verificação da assinatura, as informações especificadas nessas estruturas são usadas para localizar a assinatura em um arquivo PE. Se esses valores forem corrompidos, será possível que um arquivo pareça estar assinado de modo inválido. 

Para a assinatura Authenticode estar correta, o seguinte deverá ser verdadeiro para a assinatura Authenticode:

- O início da entrada **WIN_CERTIFICATE** no arquivo PE não pode ultrapassar o final do executável
- A entrada **WIN_CERTIFCATE** deve estar localizada no final da imagem
- O tamanho da entrada **WIN_CERTIFICATE** deve ser positiva
- A entrada **WIN_CERTIFICATE** deve ser iniciada após a estrutura **IMAGE_NT_HEADERS32** para executáveis de 32 bits e a estrutura IMAGE_NT_HEADERS64 para executáveis de 64 bits

Para obter mais detalhes, consulte a [Especificação de Authenticode Portal Executable](http://download.microsoft.com/download/9/c/5/9c5b2167-8017-4bae-9fde-d599bac8184a/Authenticode_PE.docx) e a [Especificação de formato de arquivo PE](https://msdn.microsoft.com/en-us/windows/hardware/gg463119.aspx). 

Observe que SignTool.exe pode gerar uma lista dos binários corrompidos ou malformados ao tentar assinar um pacote AppX. Para fazer isso, ative o registro detalhado, definindo a variável de ambiente APPXSIP_LOG como 1 (por exemplo, ```set APPXSIP_LOG=1``` ) e execute novamente SignTool.exe.

Para corrigir esses binários malformados, certifique-se de que eles estejam em conformidade com os requisitos acima.

## Consulte também

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx)
- [SignTool.exe (ferramenta de assinatura)](https://msdn.microsoft.com/library/8s9b9yaz(v=vs.110).aspx)
- [Como assinar um pacote do aplicativo usando a SignTool](https://msdn.microsoft.com/en-us/library/windows/desktop/jj835835(v=vs.85).aspx)

<!--HONumber=Jun16_HO4-->


