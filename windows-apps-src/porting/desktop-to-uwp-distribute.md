---
Description: Distribute a packaged desktop application (Desktop Bridge)
Search.Product: eADQiWindows 10XVcnh
title: Publicar seu aplicativo da área de trabalho empacotado para a Microsoft Store ou carregue-o em um ou mais dispositivos.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.localizationpriority: medium
ms.openlocfilehash: 9fe216b70b7c10f254c0e3b7b18275a72d46215c
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928766"
---
# <a name="distribute-a-packaged-desktop-application"></a>Distribuir um aplicativo da área de trabalho empacotado

Publicar seu aplicativo da área de trabalho empacotado para a Microsoft Store ou carregue-o em um ou mais dispositivos.  

> [!NOTE]
> Você tem um plano de como fazer a transição de usuários para seu aplicativo empacotado? Antes de distribuir seu aplicativo, consulte a seção [Transição de usuários para seu aplicativo empacotado](#transition-users) deste guia para obter algumas ideias.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribuir seu aplicativo publicando-o na Microsoft Store

A [Microsoft Store](https://www.microsoft.com/store/apps) é uma maneira conveniente para que seus clientes obtenham o aplicativo.

Publica seu aplicativo na Microsoft Store para atingir um público maior. Além disso, clientes organizacionais podem adquirir seu aplicativo para distribuição interna em suas organizações através de [Da Microsoft Store para empresas](https://www.microsoft.com/business-store).

Caso planeje publicar na Microsoft Store, você verá algumas perguntas adicionais como parte do processo de envio. Isso ocorre porque o manifesto do pacote declara uma funcionalidade restrita denominada **runFullTrust** e precisamos aprovar o uso desse recurso pelo aplicativo. Você pode ler mais sobre esse requisito aqui: [Funcionalidade restrita](https://docs.microsoft.com/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Você não precisa assinar seu aplicativo antes de enviá-lo para a loja.

>[!IMPORTANT]
> Se você planeja publicar seu aplicativo na Microsoft Store, certifique-se de que seu aplicativo funcione corretamente em dispositivos que executam o Windows 10 S. Isso é um requisito da loja. Consulte [Testar seu aplicativo do Windows para o Windows 10 S](desktop-to-uwp-test-windows-s.md).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribuir seu aplicativo sem colocá-lo na Microsoft Store

Se você preferir distribuir seu aplicativo sem utilizar a loja, você pode distribuir aplicativos para um ou mais dispositivos manualmente.

Isso pode fazer sentido se você deseja ter mais controle sobre a experiência de distribuição ou se você não quiser se envolver com o processo de certificação da Microsoft Store.

Para distribuir seu aplicativo para outros dispositivos sem colocá-lo na loja, você precisa obter um certificado, assinar seu aplicativo usando esse certificado e, em seguida, sideload seu aplicativo para esses dispositivos.

Você pode [criar um certificado](../packaging/create-certificate-package-signing.md) ou obtê-lo de um fornecedor popular, como o [Verisign](https://www.verisign.com/).

Se você pretende distribuir seu aplicativo em dispositivos que executam o Windows 10 S, seu aplicativo deve ser assinado pela Microsoft Store e você terá que passar pelo processo de envio da loja antes de distribuir seu aplicativo para esses dispositivos.

Se você criar um certificado, você precisa instalá-lo na loja de certificados **Trusted Root** ou **Trusted People** em cada dispositivo que executa seu aplicativo. Se você receber um certificado de um fornecedor popular, você não precisa instalar nada em outros sistemas além de seu aplicativo.  

> [!IMPORTANT]
> Certifique-se de que o nome do fornecedor no certificado corresponde ao nome do fornecedor do seu aplicativo.

Para assinar seu aplicativo usando um certificado, consulte o [sinal de um pacote de aplicativo usando a SignTool](../packaging/sign-app-package-using-signtool.md).

Para fazer o sideload seu aplicativo em outros dispositivos, consulte o [Sideload de aplicativos LOB no Windows 10](https://technet.microsoft.com/itpro/windows/deploy/sideload-apps-in-windows-10).

**Vídeos**

|Publicar seu aplicativo na Microsoft Store |Distribuir um aplicativo empresarial  |
|---|---|
|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Windows-Store-Publication-3cWyG5WhD_5506218965"      width="426" height="472" allowFullScreen frameBorder="0"></iframe>|<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Video-Distribution-for-Enterprise-Apps-XJ5Hd5WhD_1106218965" width="426" height="472" allowFullScreen frameBorder="0"></iframe>|

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Transição dos usuários para o aplicativo empacotado

Antes de distribuir o seu aplicativo, considere adicionar algumas extensões ao manifesto do pacote para ajudar os usuários a terem o hábito de usar seu aplicativo empacotado. Aqui estão algumas coisas que você pode fazer.

* Aponte os blocos Iniciar e os botões da barra de tarefas existentes para o seu aplicativo empacotado.
* Associe seu aplicativo empacotado com um conjunto de tipos de arquivo.
* Faça com que seu aplicativo empacotado abra certos tipos de arquivos por padrão.

Para obter a lista completa das extensões e as orientações de como usá-las, consulte [Transição de usuários para seu aplicativo](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Além disso, considere adicionar código ao seu aplicativo empacotado que realize estas tarefas:

* Migra os dados do usuário associados ao seu aplicativo da área de trabalho para as localizações de pastas apropriadas do seu aplicativo empacotado.
* Ofereça aos usuários a opção de desinstalar a versão desktop do seu aplicativo.

Vamos falar sobre cada uma dessas tarefas. Vamos começar com a migração de dados do usuário.

### <a name="migrate-user-data"></a>Migrar dados do usuário

Se você vai adicionar código que migra os dados do usuário, é melhor executar esse código somente quando o aplicativo é iniciado pela primeira vez. Antes de migrar os dados dos usuários, exiba uma caixa de diálogo para o usuário explicando o que está acontecendo, por que é recomendável e o que vai acontecer com seus dados existentes.

Aqui está um exemplo de como você pode fazer isso em um aplicativo empacotado com base em .NET.

```csharp
private void MigrateUserData()
{
    String sourceDir = Environment.GetFolderPath
        (Environment.SpecialFolder.ApplicationData) + "\\AppName";

    if (sourceDir != null)
    {
        DialogResult migrateResult = MessageBox.Show
            ("Would you like to migrate your data from the previous version of this app?",
             "Data Migration", MessageBoxButtons.YesNo);

        if (migrateResult.Equals(DialogResult.Yes))
        {
            String destinationDir =
                Windows.Storage.ApplicationData.Current.LocalFolder.Path + "\\AppName";

            Process process = new Process();
            process.StartInfo.FileName = "robocopy.exe";
            process.StartInfo.Arguments = "%LOCALAPPDATA%\\AppName " + destinationDir + " /move";
            process.StartInfo.CreateNoWindow = true;
            process.Start();
            process.WaitForExit();

            if (process.ExitCode > 1)
            {
                //Migration was unsuccessful -- you can choose to block/retry/other action
            }
        }
    }
}
```

### <a name="uninstall-the-desktop-version-of-your-app"></a>Desinstalar a versão desktop do seu aplicativo

É melhor não desinstalar o aplicativo da área de trabalho de usuários sem antes solicitar deles permissão. Exiba uma caixa de diálogo que solicita essa permissão ao usuário. Os usuários podem decidir não desinstalar a versão desktop do seu aplicativo. Se isso acontecer, você precisará decidir se deseja bloquear o uso do aplicativo da área de trabalho ou suporte ao uso de ambos os aplicativos lado a lado.

Veja um exemplo de como você pode fazer isso em um aplicativo empacotado com base em .NET.

Para exibir o contexto completo deste trecho de código, consulte o arquivo **MainWindow.cs** deste exemplo [Visualizador de imagens do WPF com transição/migração/desinstalação](https://github.com/Microsoft/DesktopBridgeToUWP-Samples/tree/master/Samples/DesktopAppTransition).

```csharp
private void RemoveDesktopApp()
{              
    //Typically, you can find your uninstall string at this location.
    String uninstallString = (String)Microsoft.Win32.Registry.GetValue
        (@"HKEY_LOCAL_MACHINE\SOFTWARE\WOW6432Node\Microsoft\Windows\CurrentVersion" +
         @"\Uninstall\{7AD02FB8-B85E-44BC-8998-F4803BA5A0E3}\", "UninstallString", null);

    //Detect if the previous version of the Desktop application is installed.
    if (uninstallString != null)
    {
        DialogResult uninstallResult = MessageBox.Show
            ("To have the best experience, consider uninstalling the "
              + " previous version of this app. Would you like to do that now?",
              "Uninstall the previous version", MessageBoxButtons.YesNo);

        if (uninstallResult.Equals(DialogResult.Yes))
        {
                    string[] uninstallArgs = uninstallString.Split(' ');

            Process process = new Process();
            process.StartInfo.FileName = uninstallArgs[0];
            process.StartInfo.Arguments = uninstallArgs[1];
            process.StartInfo.CreateNoWindow = true;

            process.Start();
            process.WaitForExit();

            if (process.ExitCode != 0)
            {
                //Uninstallation was unsuccessful - You can choose to block the application here.
            }
        }
    }

}
```

### <a name="video"></a>Vídeo

<iframe src="https://mva.microsoft.com/en-US/training-courses-embed/developers-guide-to-the-desktop-bridge-17373/Demo-Transition-Taskbar-Pins-Start-Tiles-File-Type-Associations-and-Protocol-Handlers-MD5mv5WhD_2406218965" width="636" height="480" allowFullScreen frameBorder="0"></iframe>

## <a name="next-steps"></a>Próximas etapas

**Encontrar respostas para suas dúvidas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Se você tiver problemas para publicar seu aplicativo na Store, essa [postagem de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) contém algumas dicas úteis.

**Fazer comentários ou sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
