---
author: awkoren
Description: "Mostra como converter manualmente um aplicativo de área de trabalho do Windows (Win32, WPF e Windows Forms) em um aplicativo UWP (Plataforma Universal do Windows)."
Search.Product: eADQiWindows 10XVcnh
title: "Converter manualmente um aplicativo da área de trabalho do Windows em um aplicativo UWP (Plataforma Universal do Windows)"
translationtype: Human Translation
ms.sourcegitcommit: ee697323af75f13c0d36914f65ba70f544d046ff
ms.openlocfilehash: f55f3bd6479cdf076c51cf574b07bfb5ce3a805c

---

# <a name="manually-convert-your-app-to-uwp-using-the-desktop-bridge"></a>Converter manualmente o app em UWP usando a ponte da área de trabalho

O uso do [Desktop App Converter (DAC)](desktop-to-uwp-run-desktop-app-converter.md) é prático e automático, e será útil se houver dúvidas sobre o que o instalador faz. Mas se o app for instalado usando xcopy ou se você estiver familiarizado com as alterações que o instalador do seu app faz no sistema, poderá criar um pacote de aplicativo e manifestá-lo manualmente. Este artigo contém as etapas para você começar. Ele também explica como adicionar ativos sem fundo ao seu app, o que não é coberto pelo DAC. 

Veja como começar:

## <a name="create-a-manifest-by-hand"></a>Criar um manifesto manualmente

Seu arquivo _appxmanifest_ precisa ter o conteúdo a seguir (no mínimo). Altere os espaços reservados que estão formatados como \*\*\*THIS\*\*\* para os valores reais do seu app.

```XML
    <?xml version="1.0" encoding="utf-8"?>
    <Package
       xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
       xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
       xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities">
      <Identity Name="***YOUR_PACKAGE_NAME_HERE***"
        ProcessorArchitecture="x64"
        Publisher="CN=***COMPANY_NAME***, O=***ORGANIZATION_NAME***, L=***CITY***, S=***STATE***, C=***COUNTRY***"
        Version="***YOUR_PACKAGE_VERSION_HERE***" />
      <Properties>
        <DisplayName>***YOUR_PACKAGE_DISPLAY_NAME_HERE***</DisplayName>
        <PublisherDisplayName>Reserved</PublisherDisplayName>
        <Description>No description entered</Description>
        <Logo>***YOUR_PACKAGE_RELATIVE_DISPLAY_LOGO_PATH_HERE***</Logo>
      </Properties>
      <Resources>
        <Resource Language="en-us" />
      </Resources>
      <Dependencies>
        <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.14316.0" MaxVersionTested="10.0.14316.0" />
      </Dependencies>
      <Capabilities>
        <rescap:Capability Name="runFullTrust"/>
      </Capabilities>
      <Applications>
        <Application Id="***YOUR_PRAID_HERE***" Executable="***YOUR_PACKAGE_RELATIVE_EXE_PATH_HERE***" EntryPoint="Windows.FullTrustApplication">
          <uap:VisualElements
           BackgroundColor="#464646"
           DisplayName="***YOUR_APP_DISPLAY_NAME_HERE***"
           Square150x150Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Square44x44Logo="***YOUR_PACKAGE_RELATIVE_PNG_PATH_HERE***"
           Description="***YOUR_APP_DESCRIPTION_HERE***" />
        </Application>
      </Applications>
    </Package>
```

## <a name="add-unplated-assets"></a>Adicionar ativos sem fundo

Aqui está como configurar ativos de 44 x 44 para seu app que apareçam na barra de tarefas.

1. Obtenha as imagens de 44 x 44 corretas e copie-as na pasta que contém suas imagens (isto é, os ativos).

2. Para cada imagem de 44 x 44, crie uma cópia na mesma pasta e acrescente *.targetsize-44_altform-unplated* ao nome do arquivo. Você deve ter duas cópias de cada ícone, cada uma nomeada de uma maneira específica. Por exemplo, após a conclusão do processo, sua pasta de ativos poderá conter *MYAPP_44x44.png* e *MYAPP_44x44.targetsize-44_altform-unplated.png* (observação: o primeiro é o ícone referenciado em appxmanifest, no atributo VisualElements *Square44x44Logo*). 

3.  No AppXManifest, defina o valor de BackgroundColor para cada ícone que você estiver corrigindo como transparent. Esse atributo pode ser encontrado em VisualElements para cada app.

4.  Abra CMD, mude o diretório para a pasta raiz do pacote e crie um arquivo priconfig.xml executando o comando ```makepri createconfig /cf priconfig.xml /dq en-US```.

5.  Usando CMD e permanecendo na pasta raiz do pacote, crie o(s) arquivo(s) resources.pri usando o comando ```makepri new /pr <PHYSICAL_PATH_TO_FOLDER> /cf <PHYSICAL_PATH_TO_FOLDER>\priconfig.xml```. Por exemplo, o comando para o seu app poderia ser parecido com ```makepri new /pr c:\MYAPP /cf c:\MYAPP\priconfig.xml```. 

6.  Empacote seu AppX usando as instruções na próxima etapa para ver os resultados.

## <a name="run-the-makeappx-tool"></a>Executar a ferramenta MakeAppX

Use o [Empacotador de aplicativo (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) para gerar um AppX para o seu projeto. O MakeAppx.exe está incluído no SDK do Windows 10. 

Para executar o MakeAppx, primeiro verifique se você criou um arquivo de manifesto conforme descrito anteriormente. 

Em seguida, crie um arquivo de mapeamento. O arquivo deve começar com **[Files]**, em seguida, listar cada um dos seus arquivos de origem no disco seguido pelo caminho de destino no pacote. Aqui está um exemplo: 

```
[Files]
"C:\MyApp\StartPage.htm"     "default.html"
"C:\MyApp\readme.txt"        "doc\readme.txt"
"\\MyServer\path\icon.png"   "icon.png"
"MyCustomManifest.xml"       "AppxManifest.xml"
```

Por fim, execute este comando: 

```cmd
MakeAppx.exe pack /f mapping_filepath /p filepath.appx
```

## <a name="sign-your-appx-package"></a>Assinar seu pacote AppX

O cmdlet Add-AppxPackage requer que o pacote do aplicativo (.appx) que está sendo implantado seja assinado. Use o [SignTool.exe](https://msdn.microsoft.com/library/windows/desktop/aa387764(v=vs.85).aspx), que é fornecido no SDK do Microsoft Windows 10, para assinar o pacote .appx.

Exemplo de uso: 

```cmd
C:\> MakeCert.exe -r -h 0 -n "CN=<publisher_name>" -eku 1.3.6.1.5.5.7.3.3 -pe -sv <my.pvk> <my.cer>
C:\> pvk2pfx.exe -pvk <my.pvk> -spc <my.cer> -pfx <my.pfx>
C:\> signtool.exe sign -f <my.pfx> -fd SHA256 -v .\<outputAppX>.appx
```

Quando você executar o MakeCert.exe e for solicitado a inserir uma senha, selecione **none**. Para obter mais informações sobre assinatura e certificados, consulte o seguinte: 

- [Como: criar certificados temporários para uso durante o desenvolvimento](https://msdn.microsoft.com/library/ms733813.aspx)

- [SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764.aspx)

- [SignTool.exe (ferramenta de assinatura)](https://msdn.microsoft.com/library/8s9b9yaz.aspx)




<!--HONumber=Dec16_HO1-->


