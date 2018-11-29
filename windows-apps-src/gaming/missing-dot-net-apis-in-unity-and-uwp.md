---
title: .NET APIs ausentes no Unity e na UWP
description: Saiba mais sobre as .NET APIs ausentes ao criar jogos UWP no Unity e as soluções para problemas comuns.
ms.assetid: 28A8B061-5AE8-4CDA-B4AB-2EF0151E57C1
ms.date: 2/21/2018
ms.topic: article
keywords: windows 10, uwp, jogos, .net, unity
ms.localizationpriority: medium
ms.openlocfilehash: 323d710a18ab738f89ed691cd56309ae827a7a14
ms.sourcegitcommit: b5c9c18e70625ab770946b8243f3465ee1013184
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/28/2018
ms.locfileid: "7981381"
---
# <a name="missing-net-apis-in-unity-and-uwp"></a>.NET APIs ausentes no Unity e na UWP

Ao criar um jogo UWP usando o .NET, você pode descobrir que algumas APIs que você poderia usar no editor do Unity ou em um jogo de computador autônomo não estão presentes para UWP. Isso ocorre porque o .NET para aplicativos UWP inclui um subconjunto dos tipos fornecidos no .NET Framework completo para cada namespace.

Além disso, alguns mecanismos de jogos usam diferentes versões do .NET que não são totalmente compatíveis com o .NET para UWP, como o Mono do Unity. Por isso, quando você estiver escrevendo seu jogo, tudo poderá estar funcionando bem no editor, mas, quando você começar a realizar a compilação para UWP, erros como este poderão aparecer: **O tipo ou namespace 'Formatters' não existe no namespace 'System.Runtime.Serialization' (há uma referência de assembly ausente?)**

Felizmente, o Unity fornece algumas dessas APIs ausentes como métodos de extensão e tipos de substituição, que são descritas em [Plataforma Universal do Windows: tipos .NET ausentes no back-end de script do .NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html). No entanto, se a funcionalidade que você precisa não estiver aqui, [Visão geral do .NET para apps do Windows 8.x](https://msdn.microsoft.com/library/windows/apps/br230302) aborda maneiras de você converter o código para usar WinRT ou .NET nas APIs UWP. (Ele aborda o Windows 8, mas também aplica-se aos aplicativos UWP do Windows 10.)

## <a name="net-standard"></a>.NET Standard

Para compreender por que algumas APIs podem não estar funcionando, é importante entender os diferentes tipos .NET e como a UWP implementa o .NET. O [.NET Standard](https://docs.microsoft.com/dotnet/standard/net-standard) é uma especificação formal de .NET APIs que deve funcionar como uma plataforma cruzada e unificar os diferentes tipos .NET. Cada implementação do .NET oferece suporte a uma determinada versão do .NET Standard. Você pode ver uma tabela de padrões e implementações em [Suporte à implementação do .NET](https://docs.microsoft.com/dotnet/standard/net-standard#net-implementation-support).

Cada versão do SDK da UWP está de acordo com um nível diferente do .NET Standard. Por exemplo, o SDK 16299 (o Fall Creators Update) oferece suporte ao .NET Standard 2.0.

Se você quiser saber se um determinado API .NET é compatível com a versão de UWP desejada, consulte a [Referência de API do .NET Standard](https://docs.microsoft.com/dotnet/api/index?view=netstandard-2.0) e selecione a versão do .NET Standard compatível com essa versão da UWP.

## <a name="scripting-backend-configuration"></a>Configuração do back-end de script

A primeira coisa que você deve fazer se estiver com problemas na compilação para UWP é verificar as **Configurações do Player** (**Arquivo > Configurações da Compilação**, selecione **Plataforma Universal do Windows** e, depois, **Configurações do Player**). Em **Other Settings > Configuração**, as primeiras três listas suspensas (**Scripting Runtime Version**, **Scripting Backend**, e **Api Compatibility Level**) são configurações importantes a serem consideradas.

**Scripting Runtime Version** é o que o back-end de script do Unity usa para permitir que você obtenha a versão (aproximadamente) equivalente do suporte do .NET Framework desejado. No entanto, tenha em mente que nem todas as APIs nessa versão do .NET Framework será compatível, mas apenas as da versão do .NET Standard que a UWP tem como alvo.

Geralmente nas novas versões do .NET, mais APIs são adicionadas ao .NET Standard, o que pode permitir que você use o mesmo código entre uma plataforma autônoma e a UWP. Por exemplo, o namespace [System.Runtime.Serialization.Json](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json) foi incorporado no .NET Standard 2.0. Se você definir **Scripting Runtime Version** para **.NET 3.5 Equivalent** (que direciona uma versão anterior do .NET Standard), receberá um erro ao tentar usar a API; alterne para **.NET 4.6 Equivalent** (que é compatível com o .NET Standard 2.0), e a API funcionará.

O **Scripting Backend** pode ser **.NET** ou **IL2CPP**. Neste tópico, assumimos que você escolheu **.NET**, já que foi nesse ponto que os problemas discutidos aqui surgiram. Consulte [Back-ends de script](https://docs.unity3d.com/Manual/windowsstore-scriptingbackends.html) para obter mais informações.

Por fim, você deve definir **Api Compatibility Level** para a versão do .NET na qual deseja que o jogo seja executado. Isso deve corresponder à **Scripting Runtime Version**.

Em geral, em **Scripting Runtime Version** e **Api Compatibility Level**, você deve selecionar a versão mais recente disponível para ter mais compatibilidade com o .NET Framework e, assim, permitir que você use mais .NET APIs.

![Configuração: Scripting Runtime Version; Scripting Backend; Api Compatibility Level](images/missing-dot-net-apis-in-unity-1.png)

## <a name="platform-dependent-compilation"></a>Compilação dependente de plataforma

Se você estiver compilando o jogo do Unity para várias plataformas, incluindo a UWP, precisará usar a compilação dependente de plataforma para certificar-se de que código destinado à UWP seja executado apenas quando o jogo for compilado como uma UWP. Dessa forma, você pode usar o .NET Framework completo para área de trabalho autônoma e outras plataformas e APIs do WinRT para UWP, sem receber erros de compilação.

Use as seguintes diretivas para compilar o código somente quando ele estiver sendo executado como um aplicativo UWP:

```csharp
#if NETFX_CORE
    // Your UWP code here
#else
    // Your standard code here
#endif
```

> [!NOTE]
> `NETFX_CORE` Serve apenas para verificar se você está compilando o código C# no back-end de script do .NET. Se você estiver usando outro back-end de script, como IL2CPP, use `UNITY_WSA_10_0`.

Para obter a lista completa de diretivas de compilação dependente de plataforma, consulte [Compilação dependente de plataforma](https://docs.unity3d.com/Manual/PlatformDependentCompilation.html).

## <a name="common-issues-and-workarounds"></a>Problemas comuns e soluções alternativas

Os cenários a seguir descrevem os problemas comuns que poderão surgir quando as .NET APIs estiverem ausentes no subconjunto da UWP e maneiras de contorná-los.

### <a name="data-serialization-using-binaryformatter"></a>Serialização de dados usando BinaryFormatter

É comum os jogos serializarem os dados salvos para que os jogadores não consigam manipulá-los facilmente. No entanto, [BinaryFormatter](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.formatters.binary.binaryformatter), que serializa um objeto em binário, não está disponível nas versões anteriores do .NET Standard (anteriores à 2.0). É recomendável o uso de [XmlSerializer](https://docs.microsoft.com/dotnet/api/system.xml.serialization.xmlserializer) ou [DataContractJsonSerializer](https://docs.microsoft.com/dotnet/api/system.runtime.serialization.json.datacontractjsonserializer).

```csharp
private void Save()
{
    SaveData data = new SaveData(); // User-defined object to serialize

    DataContractJsonSerializer serializer = 
      new DataContractJsonSerializer(typeof(SaveData));

    FileStream stream = 
      new FileStream(Application.persistentDataPath, FileMode.CreateNew);

    serializer.WriteObject(stream, data);
    stream.Dispose();
}
```

### <a name="io-operations"></a>Operações de E/S

Alguns tipos no namespace [System.IO](https://docs.microsoft.com/dotnet/api/system.io), como [FileStream](https://docs.microsoft.com/dotnet/api/system.io.filestream), não estão disponíveis nas versões anteriores do .NET Standard. No entanto, o Unity fornece os tipos [Directory](https://docs.microsoft.com/dotnet/api/system.io.directory), [File](https://docs.microsoft.com/dotnet/api/system.io.file) e **FileStream** tipos para que você pode usá-los em seu jogo.

Como alternativa, você pode usar as APIS [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage) , que só estão disponíveis para aplicativos UWP. No entanto, essas APIs restringem o app à gravação em seu armazenamento específico e não concede a ele acesso gratuito ao sistema de arquivos inteiro. Consulte [Arquivos, pastas e bibliotecas](https://docs.microsoft.com/windows/uwp/files/) para obter mais informações.

Uma observação importante é que o método [Close](https://docs.microsoft.com/dotnet/api/system.io.stream.close) só está disponível no .NET Standard 2.0 e posterior (embora o Unity forneça um método de extensão). Use [Dispose](https://docs.microsoft.com/dotnet/api/system.io.stream.dispose) em vez disso.

### <a name="threading"></a>Threading

Alguns tipos no namespace [System.Threading](https://docs.microsoft.com/dotnet/api/system.threading), como [ThreadPool](https://docs.microsoft.com/dotnet/api/system.threading.threadpool), não estão disponíveis nas versões anteriores do .NET Standard. Nesses casos, você pode usar o namespace [Windows.System.Threading](https://docs.microsoft.com/uwp/api/windows.system.threading) em vez disso.

Veja como tratar o threading em um jogo do Unity, usando a compilação dependente de plataforma para preparar o cenário para as plataformas UWP e não UWP:

```csharp
private void UsingThreads()
{
#if NETFX_CORE
    Windows.System.Threading.ThreadPool.RunAsync(workItem => SomeMethod());
#else
    System.Threading.ThreadPool.QueueUserWorkItem(workItem => SomeMethod());
#endif
}
```

### <a name="security"></a>Segurança

Alguns dos namespaces **System.Security.***, como [System.Security.Cryptography.X509Certificates](https://docs.microsoft.com/dotnet/api/system.security.cryptography.x509certificates?view=netstandard-2.0), não estão disponíveis na criação de um jogo Unity para UWP. Nesses casos, use as APIs de **Windows.Security.***, que abrangem praticamente a mesma funcionalidade.

O exemplo a seguir simplesmente obtém os certificados de um repositório de certificados com o nome fornecido:

```cs
private async void GetCertificatesAsync(string certStoreName)
    {
#if NETFX_CORE
        IReadOnlyList<Certificate> certs = await CertificateStores.FindAllAsync();
        IEnumerable<Certificate> myCerts = 
            certs.Where((certificate) => certificate.StoreName == certStoreName);
#else
        X509Store store = new X509Store(certStoreName, StoreLocation.CurrentUser);
        store.Open(OpenFlags.OpenExistingOnly);
        X509Certificate2Collection certs = store.Certificates;
#endif
    }
```

Consulte [Segurança](https://docs.microsoft.com/windows/uwp/security/) para obter mais informações sobre o uso das APIs de segurança do WinRT.

### <a name="networking"></a>Rede

Alguns dos namespaces **System&period;Net.***, como [System.Net.Mail](https://docs.microsoft.com/dotnet/api/system.net.mail?view=netstandard-2.0), também estão disponíveis na criação de um jogo Unity para UWP. Para a maioria dessas APIs, use as APIs do WinRT correspondentes **Windows.Networking.*** e **Windows.Web.*** para obter uma funcionalidade semelhante. Consulte [Serviços de rede e Web](https://docs.microsoft.com/windows/uwp/networking/) para obter mais informações.

No caso de **System.Net.Mail**, use o namespace [Windows.ApplicationModel.Email](https://docs.microsoft.com/uwp/api/windows.applicationmodel.email). Consulte [Enviar email](https://docs.microsoft.com/windows/uwp/contacts-and-calendar/sending-email) para obter mais informações.

## <a name="see-also"></a>Veja também

* [Plataforma Universal do Windows: tipos .NET ausentes no back-end de script do .NET](https://docs.unity3d.com/Manual/windowsstore-missingtypes.html)
* [Visão geral do .NET para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/br230302)
* [Guias de portabilidade de UWP do Unity](https://unity3d.com/partners/microsoft/porting-guides)