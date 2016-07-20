---
author: mcleblanc
Description: "Este é um tópico central que cobre toda a imagem do desenvolvedor de como a EDP (proteção de dados empresariais) se relaciona com arquivos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio."
MS-HAID: dev\_enterprise.edp\_hub
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "EDP (proteção de dados empresariais)"
translationtype: Human Translation
ms.sourcegitcommit: 235a0d96c0cf86fdb16a0a6b933fc0f2bbed99f0
ms.openlocfilehash: 2cae64ff234a4fb85fd6a3e50ade3b91480b36c8

---

# EDP (proteção de dados empresariais)

> [!NOTE]
> A EDP (proteção de dados empresariais) não pode ser aplicada ao Windows 10, Versão 1511 (compilação 10586) ou anterior.

Este é um tópico central que cobre toda a imagem do desenvolvedor de como a EDP (proteção de dados empresariais) se relaciona com arquivos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio.

Para saber mais sobre a EDP do ponto de vista dos usuários finais e administradores, consulte [Visão geral da EDP (proteção de dados empresariais)](https://technet.microsoft.com/library/dn985838(v=vs.85).aspx).

## O que é a EDP?

A EDP é um conjunto de recursos em desktops, notebooks, tablets e telefones para o MDM (Gerenciamento de Dispositivo Móvel). A EDP dá às empresas maior controle sobre como seus dados (arquivos empresariais e blobs de dados) são tratados nos dispositivos que a empresa gerencia.

-   Dados empresariais são marcados com criptografia. Eles são "dados protegidos pela empresa", ou simplesmente "dados protegidos".
-   Somente aplicativos explicitamente permitidos pelo gerenciamento empresarial por meio de uma política de EDP podem acessar dados protegidos pela empresa, por exemplo, em arquivos.
-   Somente aplicativos explicitamente permitidos por meio de uma política de EDP podem ter acesso à VPN (rede virtual privada).
-   Políticas de restrição de aplicativo também determinam como os aplicativos permitidos devem lidar com dados empresariais.
-   Restrições com base em política são aplicáveis mesmo ao conteúdo empresarial trocado por meio da área de transferência do Windows ou do contrato de Compartilhamento.
-   Dependendo da demanda, a empresa encarregada do gerenciamento pode revogar o acesso do dispositivo ao conteúdo protegido, apagando essencialmente os dados empresariais do dispositivo e deixando os dados pessoais intactos.
-   Um aplicativo de canal é um aplicativo que baixa dados protegidos. Exemplos incluem aplicativos de sincronização de arquivos e email.

A EDP aprimora o [EFS (sistema de arquivos com criptografia)](http://technet.microsoft.com/library/cc700811.aspx) e a [Limpeza Seletiva do Windows](https://technet.microsoft.com/library/dn486874.aspx) para fornecer mais opções de segurança e flexibilidade. Novas APIs de EDP permitem que você crie aplicativos que protegem e revogam o acesso ao conteúdo empresarial, trabalhe com propriedades de arquivo protegidos e acesse dados criptografados em sua forma bruta. Além de apresentar novas APIs para proteger arquivos e pastas, a EDP apresenta APIs para proteger buffers e fluxos. Ela também apresenta um conjunto de APIs que permitem que os aplicativos identifiquem e indiquem a empresa que deve impor uma política de proteção de dados.

Para que a empresa encarregada do gerenciamento possa controlar o acesso aos seus dados protegidos, a política de restrição de aplicativos define uma lista de aplicativos e de restrições nesses aplicativos. Por padrão, um aplicativo não pode acessar dados protegidos de maneira autônoma. Para obter acesso, o aplicativo deve ser adicionado a uma lista chamada de "lista de permissões", e os aplicativos nessa lista são chamados de aplicativos permitidos. Em um dispositivo gerenciado, o Windows pode restringir o acesso a dados protegidos e fazer a auditoria desses dados na área de transferência ou por meio do contrato de compartilhamento, para que o acesso por um aplicativo que não conste na lista de permissões seja auditado e/ou exija o consentimento do usuário, sem o que ele será completamente bloqueado.

A política para a EDP é fornecida para um dispositivo pela empresa encarregada do gerenciamento (o que o torna um "dispositivo gerenciado"). O provisionamento da política pode acontecer por meio de inscrição pelo usuário no MDM (gerenciamento de dispositivo móvel), por meio da configuração manual pelo departamento de IT ou por meio de outro mecanismo de gerenciamento e distribuição de políticas, como o SCCM (System Center Configuration Manager).

A proteção de arquivos via EDP otimiza chaves do RMS (Serviço de Gerenciamento de Direitos), se provisionadas, pois essas chaves podem ser usadas de maneira móvel entre dispositivos e, portanto, permitem a utilização móvel de dados protegidos. Na ausência de chaves do RMS, essas APIs farão fallback para chaves de Limpeza Seletiva locais e limitarão a funcionalidade de roaming. Os dados criptografados em roaming ficarão acessíveis no Windows em nível inferior e em dispositivos de terceiros por meio de aplicativos de RMS específicos de plataforma fornecidos pela Microsoft, bem como por meio de aplicativos de terceiros com capacitação para RMS.

Em resumo, os dados protegidos com as APIs de EDP podem ser gerenciados pela empresa, para que você possa criar seu aplicativo de uma maneira que ajude a empresa a proteger e gerenciar seus dados. Em outras palavras, você pode criar um aplicativo pronto para uso pela empresa. O restante deste guia ajuda você a fazer exatamente isso.

## Configurar seu computador para EDP


Para desenvolver corretamente o seu aplicativo e testar o seu comportamento na empresa, convém configurar seu computador ou dispositivo com as condições ideias. Isso envolve algumas tarefas que costumam fazer parte do domínio de administradores de TI.

-   Inscreva sua máquina de desenvolvimento no MDM (gerenciamento de dispositivo móvel).
-   Faça com que o seu aplicativo seja adicionado à lista de permissões por meio do [provedor de serviços de configuração AppLocker](https://msdn.microsoft.com/library/windows/hardware/dn920019).

A próxima tarefa é criar um aplicativo que reconheça e possa responder dinamicamente à identidade gerenciada da empresa na qual ele está em execução, bem como à política de proteção em vigor. Isso significa que "capacitar" o seu aplicativo. Aplicativos capacitados para seguir políticas têm muito mais chances de serem incluídos na lista de permissões para acessar dados empresariais.

## Aplicativos empresariais capacitados


Quando seu aplicativo estiver na lista de permissões, ele poderá ler dados protegidos. E, por padrão, qualquer saída de dados feita pelo aplicativo é automaticamente protegida pelo sistema. Essa proteção automática está em vigor porque a empresa encarregada do gerenciamento deve de alguma forma garantir que os seus dados empresariais permaneçam em seu próprio controle. Porém, manter seu aplicativo com tamanha restrição é apenas a maneira padrão de fazer isso. Uma maneira mais eficaz é solicitar que o sistema confie em você suficientemente para lhe proporcionar maior capacitação e flexibilidade. E, para tanto, o preço é tornar seu aplicativo mais inteligente. Isso significa dar um passo além da entrada na lista de permissões, tornando seu aplicativo capacitado para a empresa e declarando-o para essa finalidade.

Seu aplicativo está capacitado quando ele utiliza as técnicas que descreveremos para manter os dados empresariais protegidos com autonomia, independentemente de eles estarem inativos, em uso ou em trânsito. Seu aplicativo capacitado reconhece fontes de dados empresariais e os dados empresariais propriamente ditos, protegendo-os assim que ele os recebe. Ser capacitado também significa reconhecer e cumprir a política de EDP sempre que os dados empresariais deixam o aplicativo. Isso inclui proibir que o conteúdo vá para um ponto de extremidade de rede não empresarial, encapsular os dados em um formato criptografado portátil antes de permitir a sua transferência móvel e possivelmente (dependendo das configurações da política) enviar um prompt para o usuário antes de colar dados empresariais em um aplicativo que não esteja na lista de permissões. Depois que você capacita o seu aplicativo, ele anuncia essa capacitação para o sistema declarando a funcionalidade restrita **enterpriseDataPolicy**. Para saber mais sobre como trabalhar com funcionalidades restritas, consulte [Funcionalidades especiais e restritas](https://msdn.microsoft.com/library/windows/apps/mt270968#special-and-restricted-capabilities).

Em condições ideais, todos os dados empresariais são protegidos, estejam eles inativos ou em trânsito. Porém, inevitavelmente, deve haver um breve período entre a geração inicial desses dados e sua proteção. E, às vezes, dados empresariais podem existir em um ponto de extremidade de rede empresarial sem estarem criptografados. Um aplicativo capacitado é capaz proteger esses dados com autonomia. Aplicativos permitidos, mas não capacitados, precisarão ter uma proteção imposta pelo sistema.

Isso porque um aplicativo não capacitado sempre é executado no modo empresarial. O sistema garante que isso sempre seja feito. No entanto, um aplicativo capacitado tem a liberdade de se mover entre o modo empresarial e o modo pessoal e conforme apropriado para o tipo de dados com o qual ele está trabalhando em qualquer momento específico. Também é importante que um aplicativo capacitado respeite dados pessoais e não os marque como dados empresariais. Um aplicativo capacitado pode manipular simultaneamente dados corporativos e dados pessoais, desde que essas promessas sejam mantidas. A próxima seção mostra como alternar entre os modos no código.

<span id="confirming-an-identity-is-managed"/>
## Confirmando que uma identidade é gerenciada e determinando o nível de imposição da política de proteção

Em geral, seu aplicativo obtém uma identidade empresarial de um recurso externo, como um endereço de email de caixa de correio, um domínio gerenciado ou um nome de host de URI. Você pode chamar [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027) para obter a identidade gerenciada, se houver, para um nome de host de ponto de extremidade de rede.

Este exemplo de código ilustra a estrutura geral do comportamento capacitado, incluindo como determinar se uma identidade empresarial é realmente gerenciada e qual política está em vigor no momento.

```CSharp
using Windows.Security.EnterpriseData;

...

string identity = "contoso.com";

if (ProtectionPolicyManager.IsIdentityManaged(identity))
{
    EnforcementLevel enforcementLevel = ProtectionPolicyManager.GetEnforcementLevel();

    // During UI activities or network access, call ProtectionPolicyManager APIs
    // (taking the enforcement level into account) to ensure that the system
    // tags data with the identity as appropriate.

    ProtectionPolicyManager.GetForCurrentView().Identity = identity;
    // The app is now in enterprise mode.

    ProtectionPolicyManager.GetForCurrentView().Identity = string.Empty;
    // The app is back in personal mode.
}
else
{
    // No policy enforcement is done on this identity.
}
```

Conforme mostrado, primeiro você determina se o política de EDP está definida para a identidade da empresa. O termo "gerenciado" é a abreviação de "gerenciado por uma política de EDP". Quando a política de EDP está definida para uma identidade específica, [**ProtectionPolicyManager.IsIdentityManaged**](https://msdn.microsoft.com/library/windows/apps/dn705171) retorna "true" para essa identidade. Esta é a indicação para usar APIs de EDP com essa identidade. Embora as APIs de arquivo e buffer sejam um tanto quanto excepcionais por funcionarem conforme o esperado mesmo para uma identidade não gerenciada, esse cenário não faz sentido. Se um dispositivo é gerenciado, isso significa que ele é gerenciado para uma identidade empresarial. Se uma identidade não é gerenciada, então não faz sentido proteger os dados dela.

A próxima etapa é determinar e implementar o nível de imposição da política. Para determinar o nível de imposição da política, chame o método [**GetEnforcementLevel**](https://msdn.microsoft.com/library/windows/apps/mt608406). Quando uma política empresarial é aplicada à identidade, seu aplicativo capacitado deve ajudar o sistema com a imposição da política chamando APIs [**ProtectionPolicyManager**](https://msdn.microsoft.com/library/windows/apps/dn705170) durante atividades da interface do usuário ou acessos da rede para garantir que o sistema marque transferências de dados com essa identidade quando necessário. Informações adicionais sobre como interpretar o nível de imposição e colocá-lo em prática estão contidas neste guia. O exemplo de código também mostra como entrar no modo empresarial e retornar ao modo pessoal, definindo o valor de [**ProtectionPolicyManager.Identity**](https://msdn.microsoft.com/library/windows/apps/dn705785) como a identidade empresarial, ou a cadeia de caracteres vazia, respectivamente. Mais uma vez, observe que entrar no modo empresarial e sair dele apenas faz sentido com uma identidade gerenciada.

## Visão geral de recursos da EDP


**Proteção de arquivos e buffer**

-   Seu aplicativo pode proteger, inserir em contêiner e limpar dados associados a uma identidade empresarial.
-   O gerenciamento de chaves é manipulado pelo Windows. O Windows usa chaves de RMS da empresa quando elas estão disponíveis para o dispositivo. Caso contrário, ele efetua fallback para a proteção de Limpeza Seletiva.

**Gerenciamento de políticas do dispositivo**

-   Seu aplicativo pode consultar a identidade (empresa ou organização) que está gerenciando o dispositivo.
-   Seu aplicativo pode proteger os usuários contra a divulgação de dados inadvertida associando uma identidade aos dados em questão.
-   Seu aplicativo pode proteger recursos empresarias na rede fazendo verificações em busca de conexões com pontos de extremidade de rede que pertencem à empresa (servidores, intervalos de IP) e associando os dados a uma identidade gerenciada (ou seja, inscrita no MDM).
-   As APIs de EDP só funcionam com identidades gerenciadas que têm uma política de EDP definida no dispositivo. Se uma identidade não for gerenciada, as APIs indicarão isso para o aplicativo, quando necessário.

Veja a seguir uma lista de links para tópicos que detalham APIs de EDP e cenários específicos para essas áreas de recursos específicas.

## Arquivos

Veja [Usar a EDP para proteger arquivos](../files/protect-your-enterprise-data-with-edp.md).

## Buffers e fluxos

Veja [Usar a EDP para proteger fluxos e buffers](../files/use-edp-to-protect-streams-and-buffers.md).

## Área de transferência, compartilhamento e intercâmbio de dados entre aplicativos

Veja [Usar a EDP para proteger dados empresariais transferidos entre aplicativos](../app-to-app/use-edp-to-protect-enterprise-data-transferred-between-apps.md).

## Rede

Veja [Marcando conexões de rede com a identidade EDP](../networking/tagging_network_connections_with_edp_identity.md).

## DPL (proteção de dados em bloqueio) e tarefas em segundo plano

Uma organização pode optar por administrar uma política segura de DPL (proteção de dados em bloqueio), na qual as chaves de criptografia necessárias para acessar recursos protegidos são temporariamente removidas da memória do dispositivo quando este está bloqueado. Para preparar seu aplicativo para esse caso, consulte a seção sobre como [manipular eventos de bloqueio de dispositivo e evitar deixar o conteúdo desprotegido na memória](#handle-lock-events) deste tópico. Além disso, se o seu aplicativo tem uma tarefa em segundo plano necessária para a proteção de arquivos, consulte o tópico sobre como [proteger dados empresariais em um novo arquivo (para uma tarefa em segundo plano)](../files/protect-your-enterprise-data-with-edp.md#protect-data-new-file-bg).

## Imposição de políticas da interface do usuário


Nesta seção, usaremos o exemplo de um aplicativo de email capacitado que exibe atualmente uma caixa de correio empresarial entre um conjunto de caixas de correio que incluem tanto caixas de correio pessoais quanto empresariais pertencentes ao usuário. Para garantir que não haja vazamento de dados empresariais na caixa de correio empresarial, o aplicativo chama [**ProtectionPolicyManager.TryApplyProcessUIPolicy**](https://msdn.microsoft.com/library/windows/apps/dn705791) para garantir que o sistema operacional saiba sobre o contexto atual do aplicativo, seja ele empresarial ou pessoal. A API retornará false se a identidade não estiver sendo gerenciada por uma política empresarial.

```CSharp
using Windows.Security.EnterpriseData;

...

public class Mailbox
{
    public bool HasEnterpriseMail { get { /* implementation */ } }
    public string Identity { get { /* implementation */ } }
}

private void SwitchMailbox(Mailbox targetMailbox)
{
    // Code goes here to perform setup for "targetMailbox".

    if (targetMailbox.HasEnterpriseMail)
    {
        bool result = 
            ProtectionPolicyManager.TryApplyProcessUIPolicy(targetMailbox.Identity);

        // Code goes here to process "result", which indicates whether
        // or not policy enforcement is in place for the identity.
    }
    else
    {
        // For personal mailboxes, we clear policy enforcement (in case
        // it is still set from when we processed an enterprise mailbox).
        ProtectionPolicyManager.ClearProcessUIPolicy();
    }
}
```
<span id="handle-lock-events"/>
## Manipular eventos de bloqueio de dispositivo e evitar deixar o conteúdo desprotegido na memória


Nesse cenário, usaremos o exemplo de um aplicativo de email capacitado que foi projetado para manipular emails empresariais e pessoais. Quando o aplicativo é executado em uma organização que optou por administrar uma política segura de DPL (proteção de dados em bloqueio), ele deve certificar-se de remover todos os dados confidenciais da memória enquanto o dispositivo está bloqueado. Para fazer isso, ele se registra nos eventos [**ProtectionPolicyManager.ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787) e [**ProtectionPolicyManager.ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) para receber uma notificação sempre que o dispositivo for bloqueado e desbloqueado (caso a DPL esteja em vigor).


            [
              **ProtectedAccessSuspending**
            ](https://msdn.microsoft.com/library/windows/apps/dn705787) é acionado antes que as chaves de proteção de dados provisionadas no dispositivo sejam temporariamente removidas. Essas chaves são removidas quando o dispositivo é bloqueado para garantir que o acesso não autorizado aos dados criptografados não seja possível durante o bloqueio ou enquanto esse dispositivo não estiver em posse do seu proprietário. 
            [
              **ProtectedAccessResumed**
            ](https://msdn.microsoft.com/library/windows/apps/dn705786) é acionado depois que as chaves voltam a ficar disponíveis após o desbloqueio do dispositivo.

Manipulando esses dois eventos, o aplicativo pode garantir a proteção de qualquer conteúdo sensível na memória com [**DataProtectionManager**](https://msdn.microsoft.com/library/windows/apps/dn706017). Ele também deve fechar qualquer fluxo de arquivo aberto em seus arquivos protegidos para garantir que o sistema não armazene dados confidenciais no cache da memória. Você pode fazer isso de várias maneiras. Para fechar um fluxo de arquivo retornado de um método Open de um **StorageFile**, você pode chamar o método **Dispose** no fluxo. É possível definir o escopo de uso do fluxo com uma instrução "using" (C\# ou VB). Ou, você pode encapsular um objeto **DataReader** ou **DataWriter** no fluxo e usar o método **Dispose** ou a instrução "using" com esse objeto.

**Observação**  
Em um ambiente sem uma política de DPL configurada, o evento [**ProtectedAccessResumed**](https://msdn.microsoft.com/library/windows/apps/dn705786) é acionado, mas não o evento [**ProtectedAccessSuspending**](https://msdn.microsoft.com/library/windows/apps/dn705787). Esteja ciente disso no seu código e tenha cautela para não pressupor que os eventos sempre chegam em pares em cada sistema ou que você sempre pode usá-los para determinar o estado bloqueado/desbloqueado do dispositivo. Como você pode ver no exemplo de código aqui, temos a cautela de não pressupor nada sobre o estado protegido dos emails atualmente exibidos nem sobre o estado aberto do fluxo de arquivos do banco de dados aberto.

Além disso, lembre-se de que, após a retomada de um bloqueio em um dispositivo sem uma política de DPL configurada, [**ProtectedAccessResumedEventArgs.Identities**](https://msdn.microsoft.com/library/windows/apps/dn705772) é uma coleção vazia.

Para finalidades de concisão, esse exemplo de código é um pouco simplificado e se concentra em como lidar com emails empresariais. Em um aplicativo real, emails pessoais seriam gravados em um arquivo de banco de dados de email diferente, não protegido, e você não protegeria emails pessoais em bloqueio.

```CSharp
using Windows.Security.Cryptography;
using Windows.Security.EnterpriseData;
using Windows.Storage;
using Windows.Storage.Streams;

...

public class DisplayedMail
{
    public string Body { get; set; }
    public IBuffer ProtectedBody { get; set; }
    public bool IsProtected { get; set; }
}

private IOutputStream mailDatabaseStream = null;
private string currentlyDisplayedMailIdentity = "contoso.com";
private DisplayedMail currentlyDisplayedMail = new DisplayedMail()
    { Body = "Lorem ipsum dolor...", ProtectedBody = null, IsProtected = false };

// Gets the app's protected mail database file, then opens and stores a stream on it.
private async void OpenMailDatabase()
{
    // Only attempt to open the database file stream if we know it's closed.
    if (this.mailDatabaseStream == null)
    {
        StorageFolder appDataStorageFolder = ApplicationData.Current.LocalFolder;
        StorageFile storageFile = await appDataStorageFolder.GetFileAsync("maildb.dat");
        using (IRandomAccessStream randomAccessStream =
            await storageFile.OpenAsync(FileAccessMode.ReadWrite))
        {
            this.mailDatabaseStream = randomAccessStream.GetOutputStreamAt(0);
        }
    }
}

// Called once by the app when starting up.
private void AppSetup()
{
    ProtectionPolicyManager.ProtectedAccessSuspending +=
        this.ProtectionPolicyManager_ProtectedAccessSuspending;
    ProtectionPolicyManager.ProtectedAccessResumed +=
        this.ProtectionPolicyManager_ProtectedAccessResumed;
    this.OpenMailDatabase();
}

// Background work called when the app receives an email.
private async void AppMailReceived(string fauxEmail)
{
    // Only attempt to write to the database file stream if we know it's open.
    if (this.mailDatabaseStream != null)
    {
        IBuffer emailAsBuffer = CryptographicBuffer.ConvertStringToBinary
            (fauxEmail, BinaryStringEncoding.Utf8);
        await this.mailDatabaseStream.WriteAsync(emailAsBuffer);
        await this.mailDatabaseStream.FlushAsync();
    }
    else
    {
        // Code goes here to handle the case where the device is
        // locked and we can't access the protected mail database.
    }
}

// Called by ProtectionPolicyManager when the device is locked if under DPL.
private async void ProtectionPolicyManager_ProtectedAccessSuspending
    (object sender, ProtectedAccessSuspendingEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Get suspension deferral.
    Windows.Foundation.Deferral deferral = e.GetDeferral();

    // Protect the displayed mail content.
    if (!this.currentlyDisplayedMail.IsProtected)
    {
        IBuffer mailBodyBuffer = CryptographicBuffer.ConvertStringToBinary
            (this.currentlyDisplayedMail.Body, BinaryStringEncoding.Utf8);
        BufferProtectUnprotectResult result = await DataProtectionManager.ProtectAsync
            (mailBodyBuffer, this.currentlyDisplayedMailIdentity);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Protected)
        {
            // Save the protected version and clear the unprotected version.
            this.currentlyDisplayedMail.ProtectedBody = result.Buffer;
            this.currentlyDisplayedMail.Body = null;
        }
    }

    // Close the mail database file stream to make sure that we have no unprotected
    // content in memory.
    this.mailDatabaseStream.Dispose();
    this.mailDatabaseStream = null;

    // Optionally, code goes here to use e.Deadline to determine whether we have more
    // than 15 seconds left before the suspension deadline. If we do then process any
    // messages queued up for sending while we are still able to access them.

    // All done. Complete deferral.
    deferral.Complete();
}

// Called by ProtectionPolicyManager when the device is unlocked.
private async void ProtectionPolicyManager_ProtectedAccessResumed
    (object sender, ProtectedAccessResumedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.currentlyDisplayedMailIdentity))
    {
        // This event is not for our identity. Another will be sent for our identity.
        return;
    }

    // Unprotect the displayed mail content.
    if (this.currentlyDisplayedMail.IsProtected)
    {
        BufferProtectUnprotectResult result = await DataProtectionManager.UnprotectAsync
            (this.currentlyDisplayedMail.ProtectedBody);
        if (result.ProtectionInfo.Status == DataProtectionStatus.Unprotected)
        {
            // Restore the unprotected version.
            this.currentlyDisplayedMail.Body = CryptographicBuffer.ConvertBinaryToString
                (BinaryStringEncoding.Utf8, result.Buffer);
            this.currentlyDisplayedMail.ProtectedBody = null;
        }
    }

    // Reopen the closed mail database.
    this.OpenMailDatabase();
}
```

## Registrar-se para ser notificado quando um conteúdo protegido for revogado


Imagine o cenário no qual um aplicativo de email configurou uma caixa de correio empresarial no dispositivo de um usuário. Em um determinado momento — e por um dos vários motivos possíveis — a empresa deseja revogar o acesso a emails empresariais protegidos e a outros recursos no dispositivo. Há vários motivos possíveis para a revogação. É mais provável que a política empresarial em questão dispare uma revogação em uma situação de cancelamento de inscrição. Portanto, um cenário é aquele no qual o usuário cancelou a inscrição de seu dispositivos da empresa (talvez ele o tenha presentado ou vendido, queira apenas usar um dispositivo diferente, esteja mudando de emprego ou esteja se aposentando). Outro cenário é aquele no qual uma notificação de cancelamento de inscrição é enviada por um administrador de TI remotamente por meio do MDM (Gerenciamento de Dispositivo Móvel), talvez no caso em que um dispositivo é comunicado como perdido.

Sejam quais forem os detalhes específicos do caso, a empresa envia uma solicitação para limpar todos os emails do dispositivo do usuário, pois eles não são mais necessários nesse dispositivo. Um cliente de gerenciamento remoto em um dispositivo recebe a solicitação do servidor de gerenciamento remoto da empresa e chama [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) para revogar as chaves que são necessárias para acessar conteúdo protegido no dispositivo para essa identidade empresarial.

Se o seu aplicativo precisar ser notificado quando uma revogação ocorrer, você poderá fazer o registro com o evento [**ProtectionPolicyManager.ProtectedContentRevoked**](https://msdn.microsoft.com/library/windows/apps/dn705788). Quando o seu aplicativo recebe o evento, ele pode excluir todos os metadados associados à caixa de correio empresarial, que não será mais necessária.

```CSharp
using Windows.Security.EnterpriseData;

...

private string mailIdentity = "contoso.com";

void MailAppSetup()
{
    ProtectionPolicyManager.ProtectedContentRevoked += ProtectionPolicyManager_ProtectedContentRevoked;
    // Code goes here to set up mailbox for 'mailIdentity'.
}

private void ProtectionPolicyManager_ProtectedContentRevoked(object sender, ProtectedContentRevokedEventArgs e)
{
    if (!new System.Collections.Generic.List<string>(e.Identities).Contains
        (this.mailIdentity))
    {
        // This event is not for our identity.
        return;
    }

    // Code goes here to delete any metadata associated with 'mailIdentity'.
}
```

## O cliente de gerenciamento remoto inicia a limpeza dos dados protegidos pela empresa


Um usuário tem vários arquivos empresariais no seu dispositivo pessoal que estão protegidos para a identidade empresarial. O usuário perde seu dispositivo. Quando a empresa é notificada de que o usuário perdeu seu dispositivo, ela envia uma solicitação para limpar todos os dados confidenciais que ele contém e, assim, evitar o vazamento desses dados. O cliente de gerenciamento remoto no dispositivo recebe essa solicitação do servidor de gerenciamento remoto da empresa e chama [**ProtectionPolicyManager.RevokeContent**](https://msdn.microsoft.com/library/windows/apps/dn705790) para revogar as chaves necessárias para acessar o conteúdo protegido para essa identidade empresarial.

```CSharp
Windows.Security.EnterpriseData.ProtectionPolicyManager.RevokeContent("contoso.com");
```

 

 






<!--HONumber=Jul16_HO1-->


