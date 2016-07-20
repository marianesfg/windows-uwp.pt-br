---
author: awkoren
Description: "Implante e depure um aplicativo UWP (Plataforma Universal do Windows) convertido de um aplicativo de área de trabalho do Windows (Win32, WPF e Windows Forms), usando as extensões de conversão da área de trabalho."
Search.Product: eADQiWindows 10XVcnh
title: "Implantar e depurar um aplicativo UWP (Plataforma Universal do Windows) convertido de um aplicativo de área de trabalho do Windows"
translationtype: Human Translation
ms.sourcegitcommit: 3de603aec1dd4d4e716acbbb3daa52a306dfa403
ms.openlocfilehash: 618b129449d285054604008615c32de74c8bfd9b

---

# Implantar e depurar seu aplicativo UWP convertido (Project Centennial)

\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

Este tópico contém informações para ajudá-lo a implantar e depurar seu aplicativo com êxito após convertê-lo. Além disso, se você estiver curioso sobre alguns dos elementos internos das extensões de conversão de área de trabalho, este tópico é para você.

## Depurar seu aplicativo UWP convertido

Você tem duas opções principais para depurar seu aplicativo convertido usando o Visual Studio.

### Anexar ao processo

Quando o Microsoft Visual Studio está em execução "como administrador", os comandos __Iniciar Depuração__ e __Iniciar sem Depurar__ funcionarão em um projeto de aplicativo convertido, mas o aplicativo iniciado será executado com [nível de integridade médio](https://msdn.microsoft.com/library/bb625963). Ou seja, ele _não_ terá privilégios elevados. Para conceder privilégios de administrador para o aplicativo iniciado, primeiro você precisa iniciar o aplicativo "administrador" por meio de um atalho ou um bloco. Quando o aplicativo estiver em execução, em uma instância do Microsoft Visual Studio executada "como administrador", invoque __Anexar ao Processo__ e selecione o processo do seu aplicativo na caixa de diálogo.

### Depuração F5

O Visual Studio agora oferece suporte a um novo projeto de empacotamento que permite que você copie automaticamente as atualizações feitas quando seu aplicativo é compilado no pacote AppX criado quando você executou o conversor no instalador do seu aplicativo. Depois de configurar o projeto de empacotamento, você também pode usar F5 para depurar diretamente no pacote AppX. 

Veja como começar: 

1. Primeiro, certifique-se de que você está configurado para usar o Centennial. Para obter instruções, consulte [Visualização de conversor de aplicativo da área de trabalho (projeto Centennial)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter). 

2. Execute o conversor e o instalador do seu aplicativo Win32. O conversor captura o layout, e todas as alterações feitas no registro, e produz um Appx com manifesto e registery.dat para virtualizar o registro:

![alt](images/desktop-to-uwp/debug-1.png)

3. Instalar e iniciar [Visual Studio "15" Preview 2](https://www.visualstudio.com/downloads/visual-studio-next-downloads-vs.aspx). 

4. Instale o projeto VSIX de empacotamento de área de trabalho para UWP a partir da [Galeria do Visual Studio](http://go.microsoft.com/fwlink/?LinkId=797871). 

5. Abra a solução Win32 correspondente que foi convertida no Visual Studio.
 
6. Adicione o novo projeto de empacotamento à sua solução, clicando com o botão direito do mouse na solução e escolhendo "Adicionar Novo Projeto". Em seguida, escolha o projeto de empacotamento de área de trabalho para UWP em Implantação e Instalação:

    ![alt](images/desktop-to-uwp/debug-2.png)

    O projeto resultante será adicionado à sua solução:

    ![alt](images/desktop-to-uwp/debug-3.png)

    No projeto empacotamento, o AppXFileList fornece um mapeamento de arquivos para o layout de AppX. As referências começam vazias, mas devem ser definidas manualmente no projeto .exe na ordem de compilação. 

7. O projeto DesktopToUWPPackaging tem uma página de propriedade que permite que você configure a raiz do pacote AppX e qual bloco executar:

    ![alt](images/desktop-to-uwp/debug-4.png)

    Defina o PackageLayout como o local raiz do AppX que foi criado pelo conversor (acima). Em seguida, escolha qual bloco executar.

8.  Abra e edite o AppXFileList.xml. Esse arquivo define como copiar a saída da compilação de depuração Win32 para o layout de AppX que o conversor criou. Por padrão, temos um espaço reservado no arquivo com uma marca de exemplo e comentário:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="4.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
      <ItemGroup>
    <!— Use the following syntax to copy debug output to the AppX layout
       <AppxPackagedFile Include="$(outdir)\App.exe">
          <PackagePath>App.exe</PackagePath>
        </AppxPackagedFile> 
        See http://etc...
    -->
      </ItemGroup>
    </Project>
    ```

    A seguir está um exemplo de como criar o mapeamento. Nesse caso, copiamos o .exe e .dll do local da compilação Win32 para o local do layout do pacote. 

    ```XML
    <?xml version="1.0" encoding=utf-8"?>
    <Project ToolsVersion=14.0" xmlns="http://scehmas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\{path}</MyProjectOutputPath>
        </PropertyGroup>
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

    O arquivo é definido da seguinte maneira: 

    Primeiro, definimos *MyProjectOutputPath* para apontar para o local em que o projeto Win32 está sendo compilado:

    ```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Project ToolsVersion="14.0" xmlns="http://schemas.microsoft.com/developer/msbuild/2003">
        <PropertyGroup>
            <MyProjectOutputPath>C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP</MyProjectOutputPath>
        </PropertyGroup>
    ```

    Em seguida, cada *LayoutFile* especifica um arquivo para copiar do local de compilação Win32 para o layout de pacote Appx. Nesse caso, primeiro um .exe, depois um .dll são copiados. 

    ```XML
        <ItemGroup>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.exe">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.exe</PackagePath>
            </LayoutFile>
            <LayoutFile Include="$(MyProjectOutputPath)\ProjectTracker.Models.dll">
                <PackagePath>$(PackageLayout)\VFS\Program Files (x86)\Contoso Software\Project Tracker\ProjectTracker.Models.dll</PackagePath>
            </LayoutFile>
        </ItemGroup>
    </Project>
    ```

9. Defina o projeto de empacotamento como o projeto de inicialização. Isso copiará os arquivos Win32 para o AppX e, em seguida, iniciará o depurador quando o projeto estiver compilado e em execução.  

    ![alt](images/desktop-to-uwp/debug-5.png)

10. Por fim, você pode definir um ponto de interrupção no código Win32 e pressionar F5 para iniciar o depurador. Isso copiará as atualizações feitas em seu aplicativo Win32 para o pacote AppX e permitirá que você depure diretamente no Visual Studio.

11. Se você atualizar seu aplicativo, será preciso usar o MakeAppX para empacotar o aplicativo novamente. Para obter mais informações, consulte [Empacotador de aplicativo (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx). 

Caso tenha várias configurações de compilação (por exemplo, para depurar e liberar), você poderá adicionar o seguinte ao arquivo AppXFileList.xml para copiar a compilação Win32 de locais diferentes:

```XML
<PropertyGroup>
    <MyProjectOutputPath Condition="$(Configuration) == 'DesktopUWP'">C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\DesktopUWP>
    </MyProjectOutputPath>
    <MyProjectOutputPath Condition="$(Configuration) == 'ReleaseDesktopUWP'"> C:\Users\peterfar\Desktop\ProjectTracker\ProjectTracker\bin\ReleaseDesktopUWP</MyProjectOutputPath>
</PropertyGroup>
```

Você também pode usar a compilação condicional para habilitar caminhos de código em particular, se você atualizar seu aplicativo para UWP, mas ainda deseja criá-lo para Win32. 

1.  No exemplo a seguir, o código só será compilado para DesktopUWP e mostrará um bloco usando a API do WinRT. 

    ```C#
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

2.  Você pode usar o Gerenciador de Configurações para adicionar a nova configuração de compilação:

    ![alt](images/desktop-to-uwp/debug-6.png)

    ![alt](images/desktop-to-uwp/debug-7.png)

3.  Em seguida, nas propriedades do projeto, adicione suporte para os símbolos de compilação condicional:

    ![alt](images/desktop-to-uwp/debug-8.png)

4.  Agora você pode alternar o destino de compilação para DesktopUWP, se quiser compilar no destino a API de UWP que você adicionou.

## Implantar seu aplicativo UWP convertido

Para implantar seu aplicativo durante o desenvolvimento, execute o seguinte cmdlet do PowerShell: 

```Add-AppxPackage –Register AppxManifest.xml```

Para atualizar os arquivos .exe ou .dll do seu aplicativo, simplesmente substitua os arquivos existentes em seu pacote pelos novos, aumente o número de versão em AppxManifest.xml e, em seguida, execute o comando acima novamente.

Observe o seguinte: 

Qualquer unidade em que você instale seu aplicativo convertido deve ser formatada para o formato NTFS.

Um aplicativo convertido sempre é executado como o usuário interativo. Isso tem significado particular para uma aplicativo .NET cujo manifesto especifica um nível de execução __requireAdministrator__. Se o usuário interativo tiver privilégios de administrador, será exibido um aviso de UAC _cada vez que o aplicativo for iniciado_. Para usuários padrão, o aplicativo falhará ao iniciar.

Se você tentar executar o cmdlet Add-AppxPackage em um computador para o qual ainda não tiver importado o certificado criado, você receberá um erro.

Antes de implantar seu aplicativo, você precisará assiná-lo com um certificado. Para obter informações sobre a criação de um certificado, consulte [Assinar seu pacote .AppX](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter#deploy-your-converted-appx). 

Veja a seguir como importar um certificado que você criou anteriormente. Você pode instalá-lo diretamente, ou pode instalá-lo de um appx que você assinou, como o cliente fará.
1.  No Explorador de Arquivos, clique com botão direito do mouse em um appx que você assinou com um certificado de teste e escolha **Propriedades** no menu de contexto.
2.  Clique ou toque na guia **Assinaturas Digitais**.
3.  Clique ou toque no certificado e escolha **Detalhes**.
4.  Clique ou toque em **Exibir Certificado**.
5.  Clique ou toque em **Instalar Certificado**.
6.  No grupo **Local de Armazenamento**, selecione **Máquina Local**.
7.  Clique ou toque em **Avançar** e **OK** para confirmar a caixa de diálogo do UAC.
8.  Na tela seguinte do Assistente para importação de certificados, mude a opção selecionada para **Colocar todos os certificados no repositório a seguir**.
9.  Clique ou toque em **Procurar**. Na janela Selecionar Repositório de Certificados, role para baixo e selecione **Pessoas Confiáveis** e clique ou toque em **OK**.
10. Clique ou toque em **Avançar**. Uma nova tela aparece. Clique ou toque em **Concluir**.
11. Uma caixa de diálogo de confirmação deve aparecer. Em caso afirmativo, clique em **OK**. Caso apareça uma caixa diferente, isso significa que há um problema com o certificado. Talvez seja preciso solucionar esses problemas.

Para o Windows confiar no certificado, o certificado deve estar no nó **Certificados (Computador Local) > Autoridades de Certificação Confiáveis > Certificados** ou no nó **Certificados (Computador Local) > Pessoas Confiáveis > Certificados**. Somente certificados nessas duas localizações podem validar a relação de confiança de certificado no contexto da máquina local. Caso contrário, aparece uma mensagem de erro que se parece com a seguinte cadeia de caracteres:
```CMD
"Add-AppxPackage : Deployment failed with HRESULT: 0x800B0109, A certificate chain processed,
but terminated in a rootcertificate which is not trusted by the trust provider.
(Exception from HRESULT: 0x800B0109) error 0x800B0109: The root certificate of the signature
in the app package must be trusted."
```

### Nos bastidores

Quando você executar o aplicativo convertido, seu pacote de aplicativo UWP será iniciado em \Arquivos de Programas\WindowsApps\\&lt;_nome do pacote_&gt;\\&lt;_nome do aplicativo_&gt;.exe. Se você observar lá, verá que o aplicativo tem um manifesto de pacote de aplicativo (chamado AppxManifest.xml), que faz referência a um namespace xml especial usado para aplicativos convertidos. Dentro desse arquivo de manifesto está um elemento __&lt;EntryPoint&gt;__, que faz referência a um aplicativo de confiança total. Quando esse aplicativo é iniciado, ele não é executado dentro de um contêiner de aplicativo, mas, em vez disso, ele é executado como o usuário normalmente faria.

Mas o aplicativo é executado em um ambiente especial onde quaisquer acessos que o aplicativo faça ao sistema de arquivos e ao Registro são redirecionados. O arquivo chamado Registry.dat é usado para redirecionamento do Registro. Ele é de fato um hive do Registro, portanto, você pode visualizá-lo no Windows Registry Editor (Regedit). Observe que esse mecanismo significa que você não pode usar o Registro para comunicação entre processos. O Registro não foi projetado nem é adequado para essa prática em nenhum caso. Em se tratando do sistema de arquivos, o único item redirecionado é a pasta AppData, que é redirecionada para o mesmo local em que os dados do aplicativo são armazenados para todos os aplicativos UWP. Esse local é conhecido como o armazenamento de dados de aplicativo local, e você pode acessá-lo usando a propriedade [ApplicationData](https://msdn.microsoft.com/library/windows/apps/br241621). Dessa forma, seu código já é portado para ler e gravar dados de aplicativo no local correto sem que você tenha que fazer nada. E você também pode gravar diretamente lá. Um dos benefícios do redirecionamento do sistema de arquivos é uma experiência de desinstalação mais limpa.

Em uma pasta chamada VFS, você verá pastas que contêm as DLLs das quais o seu aplicativo depende. Essas DLLs são instaladas em pastas de sistema para a versão de área de trabalho clássica do seu aplicativo. Mas, como se trata de um aplicativo UWP, as DLLs são locais em seu aplicativo. Dessa forma, não há problemas de controle de versão quando aplicativos UWP são instalados e desinstalados.

## Consulte também
[Converter o seu aplicativo da área de trabalho em um aplicativo UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-root)

[Visualização Conversor de Aplicativos da Área de Trabalho (Projeto Centennial)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-run-desktop-app-converter)

[Converter manualmente o seu aplicativo da área de trabalho do Windows em um aplicativo UWP (Plataforma Universal do Windows)](https://msdn.microsoft.com/windows/uwp/porting/desktop-to-uwp-manual-conversion)

[Exemplos de código de ponte de aplicativos da área de trabalho para UWP no GitHub](https://github.com/Microsoft/DesktopBridgeToUWP-Samples)


<!--HONumber=Jul16_HO2-->


