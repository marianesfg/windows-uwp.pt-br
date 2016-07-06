---
Description: "Mostra como converter manualmente um aplicativo de área de trabalho do Windows (Win32, WPF e Windows Forms) em um aplicativo UWP (Plataforma Universal do Windows)."
Search.Product: eADQiWindows 10XVcnh
title: "Converter manualmente um aplicativo da área de trabalho do Windows em um aplicativo UWP (Plataforma Universal do Windows)"
ms.sourcegitcommit: 606d5237cb67cb4439704f81b180c3c48cc1556f
ms.openlocfilehash: be7599403e78c8db7ba91fe5a7c25b1c1a1ab644

---

# Converter manualmente o seu aplicativo da área de trabalho do Windows em um aplicativo UWP (Plataforma Universal do Windows)

\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não fornece nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

O uso do Conversor é conveniente e automático, e será útil se houver dúvidas sobre o que o instalador faz. Mas se o aplicativo for instalado usando xcopy ou se você estiver familiarizado com as alterações que o instalador de seu aplicativo faz no sistema, poderá optar por criar um pacote do aplicativo e manifestá-lo manualmente.

Veja a seguir as etapas para criar manualmente um pacote do Centennial:

## Crie um manifesto manualmente.

Seu arquivo _appxmanifest_ precisa ter o seguinte conteúdo (no mínimo). Altere os espaços reservados que estão formatados como \*\*\*THIS\*\*\* para valores reais de seu aplicativo.

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

## Execute a ferramenta MakeAppX.

Use o [empacotador de aplicativo (MakeAppx.exe)](https://msdn.microsoft.com/library/windows/desktop/hh446767(v=vs.85).aspx) para gerar um AppX para o seu projeto. O MakeAppx.exe está incluído no SDK do Windows 10. 

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

## Assinar seu pacote AppX

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




<!--HONumber=Jun16_HO5-->


