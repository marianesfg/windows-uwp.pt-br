---
author: normesta
Description: "Execute seu app empacotado e veja sua aparência sem ter que assiná-lo. Em seguida, defina pontos de interrupção e percorra o código. Quando você estiver pronto para testar seu aplicativo em um ambiente de produção, assine seu aplicativo e então instale-o."
Search.Product: eADQiWindows 10XVcnh
title: "Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)"
ms.author: normesta
ms.date: 06/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: f45d8b14-02d1-42e1-98df-6c03ce397fd3
ms.openlocfilehash: c160fecc530a6366de48f4f2ecc24df2463c0469
ms.sourcegitcommit: 77bbd060f9253f2b03f0b9d74954c187bceb4a30
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/11/2017
---
# <a name="run-debug-and-test-a-packaged-desktop-app-desktop-bridge"></a>Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)

Execute seu app empacotado e veja sua aparência sem ter que assiná-lo. Em seguida, defina pontos de interrupção e percorra o código. Quando você estiver pronto para testar seu aplicativo em um ambiente de produção, assine seu aplicativo e então instale-o. Este tópico mostra como fazer cada uma dessas coisas.

<span id="run-app" />
## <a name="run-your-app"></a>Executar seu aplicativo

Você pode executar seu aplicativo para testá-lo localmente sem precisar obter um certificado e assiná-lo.

Se você criou seu pacote usando um projeto UWP no Visual Studio, basta definir o projeto de empacotamento como o projeto inicial e então pressione CTRL+F5 para iniciar seu app.

Se você usou o Desktop App Converter ou se empacotou seu aplicativo manualmente, abra um prompt de comando do Windows PowerShell, e a partir da subpasta **PacakgeFiles** de sua pasta de saída, execute esse cmdlet:

```
Add-AppxPackage –Register AppxManifest.xml
```
Para iniciar seu aplicativo, encontre-o no menu Iniciar do Windows.

![App empacotado no menu Iniciar](images/desktop-to-uwp/converted-app-installed.png)

> [!NOTE]
> Um aplicativo empacotado sempre é executado como um usuário interativo, e qualquer unidade na qual você instale seu aplicativo empacotado deve estar formatada no formato NTFS.

## <a name="debug-your-app"></a>Depurar seu aplicativo

Selecione seu pacote em uma caixa de diálogo cada vez que você depurar seu app ou instalar uma extensão e depurar seu aplicativo sem precisar selecionar seu pacote sempre que iniciar a sessão.

### <a name="debug-your-app-by-selecting-the-package"></a>Depurar seu app ao selecionar o pacote

Essa opção possui o menor tempo de instalação, mas exige que você execute uma etapa extra sempre que você quiser iniciar a sessão de depuração.


1. Inicie seu app empacotado pelo menos uma vez para que ele seja instalado em seu computador local.

   Consulte a seção [Executar seu aplicativo](#run-app) acima.

2. Inicie o Visual Studio.

   Se você quiser depurar seu aplicativo com permissões elevadas, inicie o Visual Studio usando a opção **Executar como Administrador**.

3. No Visual Studio, escolha **Depurar**->**Outros Destinos de Depuração**->**Depurar Pacote de Aplicativo Instalado**.

4. Na lista **Pacotes de Aplicativo Instalados**, selecione o pacote de aplicativo e então escolha o botão **Anexar**.


### <a name="debug-your-app-without-having-to-select-the-package"></a>Depurar seu app sem precisar selecionar o pacote

Essa opção possui o maior tempo de instalação, mas você não precisará selecionar o pacote instalado sempre que você iniciar seu aplicativo. Você precisará instalar o [Visual Studio 2017](https://www.visualstudio.com/vs/whatsnew/) para usar essa abordagem.

1. Primeiro, instale o [Desktop Bridge Debugging Project](http://go.microsoft.com/fwlink/?LinkId=797871).

2. Inicie o Visual Studio e abra o projeto do aplicativo desktop.

6. Adicione um projeto do **Desktop Bridge Debugging** à sua solução.

   Você pode encontrar o modelo de projeto no grupo de modelos instalados **Outros Tipos de Projeto**.

    ![alt](images/desktop-to-uwp/debug-2.png)

    O projeto do **Desktop Bridge Debugging** aparecerá na sua solução.

    ![alt](images/desktop-to-uwp/debug-3.png)

7. Abra as páginas de propriedades do projeto do **Desktop Bridge Debugging**.

8. Defina o campo **Layout do Pacote** com a localização do arquivo manifesto do pacote (AppxManifest.xml), e escolha o arquivo executável do seu aplicativo na lista suspensa **Bloco Inicial**.

     ![alt](images/desktop-to-uwp/debug-4.png)

8. Abra o arquivo AppXPackageFileList.xml no editor de código.

9. Descomente o bloco de XML e adicione valores para estes elementos:

   **MyProjectOutputPath**: O caminho relativo à pasta de depuração do seu aplicativo desktop.

   **LayoutFile**: O executável que está na pasta de depuração do seu aplicativo desktop.

   **PackagePath**: O nome de arquivo totalmente qualificado do executável do seu aplicativo desktop que foi copiado para sua pasta de pacote de aplicativo do Windows durante o processo de conversão.

    Aqui está um exemplo:

    ```XML
  <?xml version="1.0" encoding="utf-8"?>
  <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
    <PropertyGroup>
     <MyProjectOutputPath>..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    </PropertyGroup>
    <ItemGroup>
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.exe">
        <PackagePath>$(PackageLayout)\MyDesktopApp.exe</PackagePath>
      </LayoutFile>
    </ItemGroup>
  </Project>
    ```

  Se seu aplicativo consome arquivos dll que são gerados de outros projetos em sua solução, e você deseja entrar no código que está contido nessas dlls, inclua um elemento **LayoutFile** para cada um desses arquivos dll.

  ```XML
  ...
      <LayoutFile Include="$(MyProjectOutputPath)\MyDesktopApp.Models.dll">
      <PackagePath>$(PackageLayout)\MyDesktopApp.Models.dll</PackagePath>
      </LayoutFile>
  ...
  ```

10. Defina o projeto de integração como o projeto de inicialização.  

    ![alt](images/desktop-to-uwp/debug-5.png)

11. Defina pontos de interrupção no código do seu aplicativo desktop e, em seguida, inicie o depurador.

  ![Botão depurar](images/desktop-to-uwp/debugger-button.png)

  O Visual Studio copia os arquivos executáveis e os dll especificados no arquivo XML para o pacote de aplicativo do Windows e, em seguida, inicie o depurador.

#### <a name="handle-multiple-build-configurations"></a>Manipular várias configurações de compilação

Se você definiu várias configurações de compilação (por exemplo: Lançar e Depurar), você pode modificar o arquivo AppXPackageFileList.xml para copiar somente os arquivos que correspondem a configuração de compilação que escolheu no Visual Studio ao iniciar o depurador.

Aqui está um exemplo.

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'Debug'">..\MyDesktopApp\bin\Debug</MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'Release'"> ..\MyDesktopApp\bin\Release</MyProjectOutputPath>
</PropertyGroup>
```

#### <a name="debug-uwp-enhancements-to-your-app"></a>Depurar aprimoramentos da UWP para seu aplicativo

Você pode querer melhorar seu aplicativo com experiências modernas como blocos dinâmicos. Se você fizer isso, você pode usar a compilação condicional para habilitar caminhos de código com configurações de compilação específicas.

1. Primeiro, no Visual Studio, defina uma configuração de compilação e coloque um nome, como "DesktopUWP".

2. Na guia **Compilar** das propriedades do seu projeto, adicione esse nome no campo **Símbolos de compilação condicional**.

     ![alt](images/desktop-to-uwp/debug-8.png)

3. Adicione blocos de código condicionais. Esse código compila apenas para a configuração de compilação do **DesktopUWP**.

    ```csharp
    [Conditional("DesktopUWP")]
    private void showtile()
    {
        XmlDocument tileXml = TileUpdateManager.GetTemplateContent(TileTemplateType.TileSquare150x150Text01);
        XmlNodeList textNodes = tileXml.GetElementsByTagName("text");
        textNodes[0].InnerText = string.Format("Welcome to DesktopUWP!");
        TileNotification tileNotification = new TileNotification(tileXml);
        TileUpdateManager.CreateTileUpdaterForApplication().Update(tileNotification);
    }
    ```

### <a name="debug-the-entire-app-lifecycle"></a>Depurar todo o ciclo de vida do aplicativo

Em alguns casos, você pode querer um controle mais refinado sobre o processo de compilação, incluindo a habilidade de depurar seu aplicativo antes que ele inicie.

Você pode usar o [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) para obter controle total sobre o ciclo de vida do aplicativo incluindo a suspensão, a retomada e o encerramento.

O [PLMDebug](https://msdn.microsoft.com/library/windows/hardware/jj680085(v=vs.85).aspx) está incluso no SDK do Windows.


### <a name="modify-your-app-in-between-debug-sessions"></a>Modificar seu aplicativo entre sessões de depuração

Se você fizer alterações ao seu aplicativo para corrigir bugs, reempacote-o usando a ferramenta MakeAppx. Consulte [Executar a ferramenta MakeAppx](desktop-to-uwp-manual-conversion.md#make-appx).

## <a name="test-your-app"></a>Testar seu aplicativo

Para testar seu aplicativo em uma configuração realista enquanto você se prepara para distribuição, é melhor assinar seu aplicativo e instalá-lo.

Se você tiver empacotado seu app usando o Visual Studio, poderá executar um script para assinar seu aplicativo e depois instalá-lo. Consulte [Fazer o sideload do pacote](../packaging/packaging-uwp-apps.md#sideload-your-app-package).

Se você empacotar seu aplicativo usando o Desktop App Converter, poderá usar o parâmetro ``sign`` para assinar automaticamente seu aplicativo usando um certificado gerado. Você precisará instalar esse certificado e, em seguida, instalar o aplicativo. Consulte [Executar o aplicativo empacotado](desktop-to-uwp-run-desktop-app-converter.md#run-app).   

Você também pode assinar seu aplicativo manualmente. Veja como

1. Crie um certificado. Consulte [Criar um certificado](../packaging/create-certificate-package-signing.md).

2. Instale esse certificado para no repositório de certificados **Trusted Root** ou **Trusted People** em seu sistema.

3. Assine seu aplicativo usando esse certificado, consulte [Assinar um pacote de aplicativo usando a SignTool](../packaging/sign-app-package-using-signtool.md).

  > [!IMPORTANT]
  > Certifique-se de que o nome do fornecedor no certificado corresponde ao nome do fornecedor do seu aplicativo.

### <a name="related-sample"></a>Exemplos relacionados

[SigningCerts](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/SigningCerts)


### <a name="test-your-app-for-windows-10-s"></a>Testar seu aplicativo para o Windows 10 S

Antes de publicar seu app, verifique se ele funcionará corretamente em dispositivos que executam o Windows 10 S. Na verdade, se você planeja publicar seu app na Windows Store, deverá fazer isso porque é um requisito da loja. Os aplicativos que não funcionam corretamente em dispositivos que executam o Windows 10 S não serão certificados. 

Consulte [Testar seu aplicativo do Windows para o Windows 10 S](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-test-windows-s).

### <a name="run-another-process-inside-the-full-trust-container"></a>Executar outro processo dentro do recipiente de confiança total

Você pode chamar processos personalizados dentro do contêiner de um pacote do aplicativo especificado. Isso pode ser útil para testar os cenários (por exemplo, se você tiver um utilitário de teste personalizado e deseja testar a saída do aplicativo). Para fazer isso, use o cmdlet do PowerShell ```Invoke-CommandInDesktopPackage```:

```CMD
Invoke-CommandInDesktopPackage [-PackageFamilyName] <string> [-AppId] <string> [-Command] <string> [[-Args]
    <string>]  [<CommonParameters>]
```

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para perguntas específicas**

Nossa equipe monitora as [tags de StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.
