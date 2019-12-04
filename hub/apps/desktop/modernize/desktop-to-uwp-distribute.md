---
Description: Distribuir um aplicativo empacotado com a ponte de desktop
title: Publique seu aplicativo de área de trabalho empacotado no Microsoft Store ou Sideload-o em um ou mais dispositivos.
ms.date: 05/18/2018
ms.topic: article
keywords: windows 10, uwp
ms.assetid: edff3787-cecb-4054-9a2d-1fbefa79efc4
ms.author: mcleans
author: mcleanbyron
ms.localizationpriority: medium
ms.openlocfilehash: 15970afbeb5d9dee1c2079cd5933b1250ecb2f09
ms.sourcegitcommit: ae9c1646398bb5a4a888437628eca09ae06e6076
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/03/2019
ms.locfileid: "74734772"
---
# <a name="distribute-your-packaged-desktop-app"></a>Distribua seu aplicativo de desktop empacotado

Se você decidir [empacotar seu aplicativo de área de trabalho em um pacote MSIX](/windows/msix/desktop/desktop-to-uwp-root), poderá publicar o aplicativo empacotado no Microsoft Store ou Sideload-lo em um ou mais dispositivos.

> [!NOTE]
> Você tem um plano de como você pode fazer a transição de usuários para seu aplicativo empacotado? Antes de distribuir seu aplicativo, consulte a seção [Transição de usuários para seu aplicativo empacotado](#transition-users) deste guia para obter algumas ideias.

## <a name="distribute-your-application-by-publishing-it-to-the-microsoft-store"></a>Distribua seu aplicativo publicando-o no Microsoft Store

A [Microsoft Store](https://www.microsoft.com/store/apps) é uma maneira conveniente para que seus clientes obtenham o aplicativo.

Publique seu aplicativo no Microsoft Store para alcançar o público mais amplo. Além disso, os clientes organizacionais podem adquirir seu aplicativo para distribuir internamente para suas organizações por meio do [Microsoft Store for Business](https://businessstore.microsoft.com/store).

Caso planeje publicar na Microsoft Store, você verá algumas perguntas adicionais como parte do processo de envio. Isso ocorre porque o manifesto do pacote declara uma funcionalidade restrita denominada **runFullTrust** e precisamos aprovar o uso desse recurso pelo aplicativo. Você pode ler mais sobre esse requisito aqui: [Funcionalidade restrita](/windows/uwp/packaging/app-capability-declarations#restricted-capabilities).

Você não precisa assinar seu aplicativo antes de enviá-lo para a loja.

>[!IMPORTANT]
> Se você planeja publicar seu aplicativo no Microsoft Store, certifique-se de que seu aplicativo opere corretamente em dispositivos que executam o Windows 10 S. Esse é um requisito de armazenamento. Consulte [Testar seu aplicativo do Windows para o Windows 10 S](/windows/msix/desktop/desktop-to-uwp-test-windows-s).

<a id="side-load" />

## <a name="distribute-your-application-without-placing-it-onto-the-microsoft-store"></a>Distribua seu aplicativo sem colocá-lo na Microsoft Store

Se você preferir distribuir seu aplicativo sem usar a loja, poderá distribuir manualmente os aplicativos para um ou mais dispositivos.

Isso pode fazer sentido se você deseja ter mais controle sobre a experiência de distribuição ou se você não quiser se envolver com o processo de certificação da Microsoft Store.

Para distribuir seu aplicativo para outros dispositivos sem colocá-lo na loja, você precisa obter um certificado, assinar seu aplicativo usando esse certificado e, em seguida, Sideload seu aplicativo nesses dispositivos.

Você pode [criar um certificado](/windows/msix/package/create-certificate-package-signing) ou obtê-lo de um fornecedor popular, como o [Verisign](https://www.verisign.com/).

Se você planeja distribuir seu aplicativo em dispositivos que executam o Windows 10 S, seu aplicativo precisa ser assinado pelo Microsoft Store para que você precise passar pelo processo de envio da loja antes de poder distribuir seu aplicativo para esses dispositivos.

Se você criar um certificado, você precisa instalá-lo na loja de certificados **Trusted Root** ou **Trusted People** em cada dispositivo que executa seu aplicativo. Se você receber um certificado de um fornecedor popular, você não precisa instalar nada em outros sistemas além de seu aplicativo.  

> [!IMPORTANT]
> Certifique-se de que o nome do fornecedor no certificado corresponde ao nome do fornecedor do seu aplicativo.

Para assinar seu aplicativo usando um certificado, consulte [assinar um pacote de aplicativos usando Signtool](/windows/msix/package/sign-app-package-using-signtool).

Para Sideload seu aplicativo em outros dispositivos, consulte [aplicativos LOB Sideload no Windows 10](/windows/application-management/sideload-apps-in-windows-10).

<a id="transition-users" />

## <a name="transition-users-to-your-packaged-app"></a>Transição dos usuários para o aplicativo empacotado

Antes de distribuir o seu aplicativo, considere adicionar algumas extensões ao manifesto do pacote para ajudar os usuários a terem o hábito de usar seu aplicativo empacotado. Aqui estão algumas coisas que você pode fazer.

* Aponte os blocos Iniciar e os botões da barra de tarefas existentes para o seu aplicativo empacotado.
* Associe o aplicativo empacotado a um conjunto de tipos de arquivo.
* Faça com que o aplicativo empacotado abra determinados tipos de arquivos por padrão.

Para obter a lista completa das extensões e as orientações de como usá-las, consulte [Transição de usuários para seu aplicativo](desktop-to-uwp-extensions.md#transition-users-to-your-app).

Além disso, considere adicionar código ao aplicativo empacotado que realiza estas tarefas:

* Migra os dados do usuário associados ao seu aplicativo de área de trabalho para os locais de pastas apropriados do aplicativo empacotado.
* Ofereça aos usuários a opção de desinstalar a versão desktop do seu aplicativo.

Vamos falar sobre cada uma dessas tarefas. Vamos começar com a migração de dados do usuário.

### <a name="migrate-user-data"></a>Migrar dados do usuário

Se você pretende adicionar um código que migre os dados do usuário, é melhor executar esse código somente quando o aplicativo for iniciado pela primeira vez. Antes de migrar os dados dos usuários, exiba uma caixa de diálogo para o usuário explicando o que está acontecendo, por que é recomendável e o que vai acontecer com seus dados existentes.

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

É melhor não desinstalar o aplicativo de área de trabalho de usuários sem primeiro pedir permissão a eles. Exiba uma caixa de diálogo que solicita essa permissão ao usuário. Os usuários podem decidir não desinstalar a versão desktop do seu aplicativo. Se isso acontecer, você precisará decidir se deseja bloquear o uso do aplicativo de área de trabalho ou dar suporte ao uso lado a lado de ambos os aplicativos.

Aqui está um exemplo de como você pode fazer isso em um aplicativo empacotado com base em .NET.

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

## <a name="next-steps"></a>Próximas etapas

**Encontre respostas para suas perguntas**

Tem dúvidas? Pergunte-nos sobre o Stack Overflow. Nossa equipe monitora estas [marcas](https://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge). Você também pode entrar em contato conosco [aqui](https://social.msdn.microsoft.com/Forums/en-US/home?filter=alltypes&sort=relevancedesc&searchTerm=%5BDesktop%20Converter%5D).

Se você tiver problemas para publicar seu aplicativo na Store, essa [postagem de blog](https://blogs.msdn.microsoft.com/appconsult/2017/09/25/preparing-a-desktop-bridge-application-for-the-store-submission/) contém algumas dicas úteis.

**Fornecer comentários ou fazer sugestões de recursos**

Consulte [UserVoice](https://wpdev.uservoice.com/forums/110705-universal-windows-platform/category/161895-desktop-bridge-centennial).
