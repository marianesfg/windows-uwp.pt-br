---
title: Compartilhar certificados entre aplicativos
description: Os aplicativos da Plataforma Universal do Windows (UWP) que exigem autenticação segura além de uma combinação de ID de usuário e senha podem usar certificados na autenticação.
ms.assetid: 159BA284-9FD4-441A-BB45-A00E36A386F9
author: PatrickFarley
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, segurança
ms.localizationpriority: medium
ms.openlocfilehash: 863658438ce53f2c74faddb845a7d17c6ec3130c
ms.sourcegitcommit: 72835733ec429a5deb6a11da4112336746e5e9cf
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/20/2018
ms.locfileid: "5168677"
---
# <a name="share-certificates-between-apps"></a>Compartilhar certificados entre aplicativos




Os aplicativos da Plataforma Universal do Windows (UWP) que exigem autenticação segura além de uma combinação de ID de usuário e senha podem usar certificados na autenticação. A autenticação de certificado oferece um elevado nível de confiança ao autenticar um usuário. Em alguns casos, um grupo de serviços desejará autenticar um usuário para vários aplicativos. Este artigo mostra como você pode autenticar vários aplicativos usando o mesmo certificado e fornecer código prático para que um usuário importe um certificado que foi fornecido para acessar serviços Web seguros.

Os aplicativos podem se autenticar com um serviço Web usando um certificado e vários aplicativos podem usar um único certificado do repositório de certificados para autenticar o mesmo usuário. Se um certificado não existir no repositório, você poderá adicionar código ao seu aplicativo para importar um certificado de um arquivo PFX.

## <a name="enable-microsoft-internet-information-services-iis-and-client-certificate-mapping"></a>Habilitar Serviços de Informações da Internet (IIS) da Microsoft e mapeamento de certificados do cliente


Este artigo usa o Microsoft Internet Information Services (IIS) para fins de exemplo. O IIS não está habilitado por padrão. Você pode habilitar o IIS usando o Painel de Controle.

1.  Abra o Painel de Controle e selecione **Programas**.
2.  Selecione **Ativar/desativar recursos do Windows**.
3.  Expanda **Serviços de Informações da Internet** e, em seguida, expanda **Serviços Web**. Expanda **Recursos de Desenvolvimento de Aplicativos** e selecione **ASP.NET 3.5** e **ASP.NET 4.5**. Fazer essas seleções habilitará automaticamente o **Serviços de Informações da Internet**.
4.  Clique em **OK** para aplicar as alterações.

## <a name="create-and-publish-a-secured-web-service"></a>Crie e publique um serviço Web seguro


1.  Execute o Microsoft Visual Studio como administrador e selecione **Novo Projeto** na página inicial. O acesso do administrador é obrigatório para publicar um serviço Web em um servidor IIS. Na caixa de diálogo Novo Projeto, altere a estrutura para **.NET Framework 3.5**. Selecione **Visual C#** -&gt; **Web** -&gt; **Visual Studio** -&gt; **Aplicativo de Serviço Web do ASP.NET**. Nomeie o aplicativo como "FirstContosoBank". Clique em **OK** para criar o projeto.
2.  No arquivo **Service1.asmx.cs**, substitua o método padrão da Web **HelloWorld** pelo seguinte método "Logon".
    ```cs
            [WebMethod]
            public string Login()
            {
                // Verify certificate with CA
                var cert = new System.Security.Cryptography.X509Certificates.X509Certificate2(
                    this.Context.Request.ClientCertificate.Certificate);
                bool test = cert.Verify();
                return test.ToString();
            }
    ```

3.  Salve o arquivo **Service1.asmx.cs**.
4.  No **Gerenciador de Soluções**, clique com o botão direito do mouse no aplicativo "FirstContosoBank" e selecione **Publicar**.
5.  Na caixa de diálogo **Publicar na Web**, crie um novo perfil e nomeie-o "ContosoProfile". Clique em **Avançar**.
6.  Na próxima página, insira o nome do seu servidor IIS e especifique um nome de site "Site padrão/FirstContosoBank". Clique em **Publicar** para publicar seu serviço Web.

## <a name="configure-your-web-service-to-use-client-certificate-authentication"></a>Configure seu serviço Web para usar autenticação de certificado cliente


1.  Execute o **Gerenciador do Serviços de Informações da Internet (IIS)**.
2.  Expanda os sites do seu servidor IIS. No **Site padrão**, selecione o novo serviço Web "FirstContosoBank". Na seção **Ações**, selecione **Configurações Avançadas...**.
3.  Defina o **Pool de aplicativos** como **.NET v2.0** e clique em **OK**.
4.  No **Gerenciador do Serviços de Informações da Internet (IIS)**, selecione o servidor IIS e, em seguida, clique duas vezes em **Certificados do servidor**. Na seção **Ações**, selecione **Criar Certificado Autoassinado...**. Insira "ContosoBank" como o nome amigável para o certificado e clique em **OK**. Isso criará um novo certificado para uso pelo servidor IIS no formato de "&lt;nome-do-servidor&gt;.&lt;nome-do-domínio&gt;".
5.  No **Gerenciador dos Serviços de Informações da Internet (IIS)**, selecione o site padrão. Na seção **Ações**, selecione **Associação** e então clique em **Adicionar...**. Selecione "https" como o tipo, defina a porta como "443" e insira o nome de host completo do seu servidor IIS ("&lt;nome-do-servidor&gt;.&lt;nome-do-domínio&gt;"). Defina o certificado SSL como "ContosoBank". Clique em **OK**. Clique em **Fechar** na janela **Associações do Site** .
6.  No **Gerenciador dos Serviços de Informações da Internet (IIS)**, selecione o serviço Web "FirstContosoBank". Clique duas vezes em **Configurações do SSL**. Marque **Requer SSL**. Em **Certificados clientes**, selecione **Requer**. Na seção **Ações**, clique em **Aplicar**.
7.  Você pode verificar se o serviço Web está configurado corretamente abrindo o seu navegador da Web e inserindo o seguinte endereço da Web: "https://&lt;nome-do-servidor&gt;.&lt;nome -do-domínio&gt;/FirstContosoBank/Service1.asmx". Por exemplo, "https://myserver.example.com/FirstContosoBank/Service1.asmx". Se o seu serviço Web estiver configurado corretamente, você será solicitado a selecionar um certificado cliente, a fim de acessar o serviço Web.

Você pode repetir as etapas anteriores para criar vários serviços Web que podem ser acessados usando o mesmo certificado cliente.

## <a name="create-a-uwp-app-that-uses-certificate-authentication"></a>Criar um aplicativo UWP que usa autenticação de certificados


Agora que você já tem um ou mais serviços Web seguros, seus aplicativos podem usar certificados para autenticar esses serviços Web. Quando você cria uma solicitação para um serviço Web autenticado usando o objeto [**HttpClient**](https://msdn.microsoft.com/library/windows/apps/dn298639), a solicitação inicial não conterá um certificado cliente. O serviço Web autenticado responderá com uma solicitação para autenticação do cliente. Quando isso ocorre, o cliente Windows consultará automaticamente o repositório de certificados para obter os certificados cliente disponíveis. O seu usuário pode selecionar dentre esses certificados para autenticar para o serviço Web. Alguns certificados são protegidos por senha, portanto você precisará fornecer ao usuário uma maneira de inserir a senha para um certificado.

Se não houver certificados cliente disponíveis, então o usuário precisará adicionar um certificado ao repositório de certificados. Você pode incluir código em seu aplicativo que permite que um usuário selecione um arquivo PFX que contenha um certificado cliente e, em seguida, importe esse certificado para o repositório de certificados cliente.

**Dica**  Você pode usar makecert.exe para criar um arquivo PFX para usar com esse guia de início rápido. Para obter informações sobre como usar makecert.exe, consulte [MakeCert.](https://msdn.microsoft.com/library/windows/desktop/aa386968)

 

1.  Abra o Visual Studio e crie um novo projeto na página inicial. Nomeie o novo projeto "FirstContosoBankApp". Clique em **OK** para criar o novo projeto.
2.  No arquivo MainPage.xaml, adicione o seguinte XAML ao elemento padrão **Grid**. Esse XAML inclui um botão para navegar para um arquivo PFX a ser importado, uma caixa de texto para inserir uma senha para um arquivo PFX protegido por senha, um botão para importar um arquivo PFX selecionado, um botão para fazer logon no serviço Web seguro e um bloco de texto para exibir o status da ação atual.
    ```xml
    <Button x:Name="Import" Content="Import Certificate (PFX file)" HorizontalAlignment="Left" Margin="352,305,0,0" VerticalAlignment="Top" Height="77" Width="260" Click="Import_Click" FontSize="16"/>
    <Button x:Name="Login" Content="Login" HorizontalAlignment="Left" Margin="611,305,0,0" VerticalAlignment="Top" Height="75" Width="240" Click="Login_Click" FontSize="16"/>
    <TextBlock x:Name="Result" HorizontalAlignment="Left" Margin="355,398,0,0" TextWrapping="Wrap" VerticalAlignment="Top" Height="153" Width="560"/>
    <PasswordBox x:Name="PfxPassword" HorizontalAlignment="Left" Margin="483,271,0,0" VerticalAlignment="Top" Width="229"/>
    <TextBlock HorizontalAlignment="Left" Margin="355,271,0,0" TextWrapping="Wrap" Text="PFX password" VerticalAlignment="Top" FontSize="18" Height="32" Width="123"/>
    <Button x:Name="Browse" Content="Browse for PFX file" HorizontalAlignment="Left" Margin="352,189,0,0" VerticalAlignment="Top" Click="Browse_Click" Width="499" Height="68" FontSize="16"/>
    <TextBlock HorizontalAlignment="Left" Margin="717,271,0,0" TextWrapping="Wrap" Text="(Optional)" VerticalAlignment="Top" Height="32" Width="83" FontSize="16"/>
    ```
    
3.  Salve o arquivo MainPage.xaml.
4.  No arquivo MainPage.xaml.cs, adicione o seguinte usando as instruções.
    ```cs
    using Windows.Web.Http;
    using System.Text;
    using Windows.Security.Cryptography.Certificates;
    using Windows.Storage.Pickers;
    using Windows.Storage;
    using Windows.Storage.Streams;
    ```

5.  No arquivo MainPage.xaml.cs, adicione as seguintes variáveis à classe **MainPage** . Elas especificam o endereço para o método "Logon" seguro do seu serviço Web "FirstContosoBank" e uma variável global que mantém um certificado PFX a ser importado para o repositório de certificados. Atualize o &lt;nome-do-servidor&gt; para o nome do servidor completo do seu servidor Microsoft Internet Information Server (IIS).
    ```cs
    private Uri requestUri = new Uri("https://<server-name>/FirstContosoBank/Service1.asmx?op=Login");
    private string pfxCert = null;
    ```

6.  No arquivo MainPage.xaml.cs, adicione o seguinte manipulador de clique ao botão de logon e o método para acessar o serviço Web seguro.
    ```cs
    private void Login_Click(object sender, RoutedEventArgs e)
    {
        MakeHttpsCall();
    }

    private async void MakeHttpsCall()
    {

        StringBuilder result = new StringBuilder("Login ");
        HttpResponseMessage response;
        try
        {
            Windows.Web.Http.HttpClient httpClient = new Windows.Web.Http.HttpClient();
            response = await httpClient.GetAsync(requestUri);
            if (response.StatusCode == HttpStatusCode.Ok)
            {
                result.Append("successful");
            }
            else
            {
                result = result.Append("failed with ");
                result = result.Append(response.StatusCode);
            }
        }
        catch (Exception ex)
        {
            result = result.Append("failed with ");
            result = result.Append(ex.Message);
        }

        Result.Text = result.ToString();
    }
    ```

7.  No arquivo MainPage.xaml.cs, adicione os seguintes manipuladores de clique ao botão para navegar para um arquivo PFX e ao botão para importar um arquivo PFX selecionado para o repositório de certificados.
    ```cs
    private async void Import_Click(object sender, RoutedEventArgs e)
    {
        try
        {
            Result.Text = "Importing selected certificate into user certificate store....";
            await CertificateEnrollmentManager.UserCertificateEnrollmentManager.ImportPfxDataAsync(
                pfxCert,
                PfxPassword.Password,
                ExportOption.Exportable,
                KeyProtectionLevel.NoConsent,
                InstallOptions.DeleteExpired,
                "Import Pfx");

            Result.Text = "Certificate import succeded";
        }
        catch (Exception ex)
        {
            Result.Text = "Certificate import failed with " + ex.Message;
        }
    }

    private async void Browse_Click(object sender, RoutedEventArgs e)
    {

        StringBuilder result = new StringBuilder("Pfx file selection ");
        FileOpenPicker pfxFilePicker = new FileOpenPicker();
        pfxFilePicker.FileTypeFilter.Add(".pfx");
        pfxFilePicker.CommitButtonText = "Open";
        try
        {
            StorageFile pfxFile = await pfxFilePicker.PickSingleFileAsync();
            if (pfxFile != null)
            {
                IBuffer buffer = await FileIO.ReadBufferAsync(pfxFile);
                using (DataReader dataReader = DataReader.FromBuffer(buffer))
                {
                    byte[] bytes = new byte[buffer.Length];
                    dataReader.ReadBytes(bytes);
                    pfxCert = System.Convert.ToBase64String(bytes);
                    PfxPassword.Password = string.Empty;
                    result.Append("succeeded");
                }
            }
            else
            {
                result.Append("failed");
            }
        }
        catch (Exception ex)
        {
            result.Append("failed with ");
            result.Append(ex.Message); ;
        }

        Result.Text = result.ToString();
    }
    ```

8.  Execute o seu aplicativo e faça logon em seu serviço Web seguro, bem como importar um arquivo PFX para o repositório de certificados local.

É possível usar essas etapas para criar vários aplicativos que usam o mesmo certificado de usuário para acessar os mesmos ou diferentes serviços Web seguros.