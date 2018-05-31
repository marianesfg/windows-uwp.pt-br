---
author: normesta
Description: Run your packaged app and see how it looks without having to sign it. Then, set breakpoints and step through code. When you're ready to test your app in a production environment, sign your app and then install it.
Search.Product: eADQiWindows 10XVcnh
title: Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)
ms.author: normesta
ms.date: 08/31/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.localizationpriority: medium
ms.openlocfilehash: 0f8d443bc201d0e60387673edb7f9b73e2e47490
ms.sourcegitcommit: 1773bec0f46906d7b4d71451ba03f47017a87fec
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/17/2018
ms.locfileid: "1662676"
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)

Execute seu app empacotado e veja sua aparência sem ter que assiná-lo. Em seguida, defina pontos de interrupção e percorra o código. Quando você estiver pronto para testar seu aplicativo em um ambiente de produção, assine seu aplicativo e então instale-o. Este tópico mostra como fazer cada uma dessas coisas.

<a id="run-app" />

## <a name="run-your-app"></a>Executar seu aplicativo

Você pode executar seu aplicativo para testá-lo localmente sem precisar obter um certificado e assiná-lo. Como executar o aplicativo depende de qual ferramenta você usou para criar o pacote.

### <a name="you-created-the-package-by-using-visual-studio"></a>Você criou o pacote usando o Visual Studio

Defina o projeto de empacotamento como o projeto de inicialização e, em seguida, pressione CTRL+F5 para iniciar seu aplicativo.

### <a name="you-created-the-package-manually-or-by-using-the-desktop-app-converter"></a>Você criou o pacote manualmente ou usando o Desktop App Converter

Abra um aviso de comando do Windows PowerShell e, na subpasta **PackageFiles** da sua pasta de saída, execute este cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar seu aplicativo, encontre-o no menu Iniciar do Windows.

![App empacotado no menu Iniciar](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Um aplicativo empacotado sempre é executado como um usuário interativo, e qualquer unidade na qual você instale seu aplicativo empacotado deve estar formatada no formato NTFS.

## <a name="debug-your-app"></a>Depurar seu aplicativo

Como depurar o aplicativo depende de qual ferramenta você usou para criar o pacote.

Se você criou seu pacote usando o [novo projeto de empacotamento](desktop-to-uwp-packaging-dot-net.md#new-packaging-project) disponível na versão 15.4 do Visual Studio 2017, defina o projeto de empacotamento como o projeto de inicialização e, em seguida, pressione F5 para depurar seu aplicativo.

Se você criou seu pacote usando qualquer outra ferramenta, siga estas etapas.

1. Inicie seu app empacotado pelo menos uma vez para que ele seja instalado em seu computador local.

   Consulte a seção [Executar seu aplicativo](#run-app) acima.

2. Inicie o Visual Studio.

   Se você quiser depurar seu aplicativo com permissões elevadas, inicie o Visual Studio usando a opção **Executar como Administrador**.

3. No Visual Studio, escolha **Depurar**->**Outros Destinos de Depuração**->**Depurar Pacote de Aplicativo Instalado**.

4. Na lista **Pacotes de Aplicativo Instalados**, selecione o pacote de aplicativo e então escolha o botão **Anexar**.

#### <a name="modify-your-app-in-between-debug-sessions"></a>Modificar seu aplicativo entre sessões de depuração

Se você fizer alterações ao seu aplicativo para corrigir bugs, reempacote-o usando a ferramenta MakeAppx. Consulte [Executar a ferramenta MakeAppx](desktop-to-uwp-manual-conversion.md#make-appx).

### <a name="debug-the-entire-app-lifecycle"></a>Depurar todo o ciclo de vida do aplicativo

Em alguns casos, você pode querer um controle mais refinado sobre o processo de compilação, incluindo a habilidade de depurar seu aplicativo antes que ele inicie.

Você pode usar o [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obter controle total sobre o ciclo de vida do aplicativo incluindo a suspensão, a retomada e o encerramento.

O [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) está incluso no SDK do Windows.

## <a name="test-your-app"></a>Testar o app

Para testar seu aplicativo em uma configuração realista enquanto você se prepara para distribuição, é melhor assinar seu aplicativo e instalá-lo.

### <a name="test-an-app-that-you-packaged-by-using-visual-studio"></a>Testar um aplicativo que você empacotou usando o Visual Studio

O Visual Studio assina seu aplicativo usando um certificado de teste. Você encontrará esse certificado na pasta de saída gerada pelo assistente **Criar pacotes de aplicativo**. O arquivo de certificado tem a extensão *.cer* e você precisará instalar esse certificado no armazenamento **Autoridades de certificação raiz confiáveis** no computador em que deseja testar seu aplicativo. Consulte [Fazer o sideload do pacote](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

### <a name="test-an-app-that-you-packaged-by-using-the-desktop-app-converter-dac"></a>Testar um aplicativo que você empacotou usando o Desktop App Converter (DAC)

Se você empacotar seu aplicativo usando o Desktop App Converter, poderá usar o parâmetro ``sign`` para assinar automaticamente seu aplicativo usando um certificado gerado. Você precisará instalar esse certificado e, em seguida, instalar o aplicativo. Consulte [Executar o aplicativo empacotado](desktop-to-uwp-run-desktop-app-converter.md#run-app).   


### <a name="manually-sign-apps-optional"></a>Assinar aplicativos manualmente (opcional)

Você também pode assinar seu aplicativo manualmente. Veja como

1. Crie um certificado. Consulte [Criar um certificado](../packaging/create-certificate-package-signing.md).

2. Instale esse certificado para no repositório de certificados **Trusted Root** ou **Trusted People** em seu sistema.

3. Assine seu aplicativo usando esse certificado, consulte [Assinar um pacote de aplicativo usando a SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Certifique-se de que o nome do fornecedor no certificado corresponde ao nome do fornecedor do seu aplicativo.

    **Exemplos relacionados**

    [SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>Testar seu aplicativo para o Windows 10 S

Antes de publicar seu app, verifique se ele funcionará corretamente em dispositivos que executam o Windows 10 S. Na verdade, se você planeja publicar seu app na Microsoft Store, deverá fazer isso porque é um requisito da loja. Os aplicativos que não funcionam corretamente em dispositivos que executam o Windows 10 S não serão certificados.

Consulte [Testar seu aplicativo do Windows para o Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Executar outro processo dentro do recipiente de confiança total

Você pode chamar processos personalizados dentro do contêiner de um pacote do aplicativo especificado. Isso pode ser útil para testar os cenários (por exemplo, se você tiver um utilitário de teste personalizado e deseja testar a saída do aplicativo). Para fazer isso, use o cmdlet do PowerShell ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
