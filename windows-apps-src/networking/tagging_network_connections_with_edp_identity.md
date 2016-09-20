---
author: DelfCo
Description: "Este tópico mostra como criar um contexto de thread protegido antes de criar conexões de rede em um cenário de EDP (proteção de dados empresariais)."
MS-HAID: dev\_networking.tagging\_network\_connections\_with\_edp\_identity
MSHAttr: PreferredLib:/library/windows/apps
Search.Product: eADQiWindows 10XVcnh
title: "Marcando conexões de rede com a identidade EDP"
ms.sourcegitcommit: 6530fa257ea3735453a97eb5d916524e750e62fc
ms.openlocfilehash: 2b960bbb5cf58991778e5c20bb915a202ecf6e04

---

# Marcando conexões de rede com a identidade EDP


            __Observação__ A EDP (Proteção de Dados Empresariais) não pode ser aplicada ao Windows 10, Versão 1511 (compilação 10586) ou anterior.

Este tópico mostra como criar um contexto de thread protegido antes de criar conexões de rede em um cenário de EDP (proteção de dados empresariais). Para obter o panorama completo para desenvolvedores de como a EDP está relacionada a arquivos, fluxos, buffers, área de transferência, redes, tarefas em segundo plano e proteção de dados sob bloqueio, consulte [Proteção de dados corporativos (EDP)](../enterprise/edp-hub.md).


            **Observação**  A [amostra de proteção de dados corporativos (EDP)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409) abrange muitos cenários de EDP.



## Pré-requisitos


-   **Preparar-se para a EDP**

    Veja [Configurar seu computador para EDP](../enterprise/edp-hub.md#set-up-your-computer-for-EDP).

-   **Comprometer-se a criar um aplicativo habilitado para empresas**

    Um aplicativo que garante de forma autônoma que os dados empresariais permaneçam sob o controle administrativo da empresa é conhecido como aplicativo habilitado para empresas. Um aplicativo habilitado é mais avançado, inteligente, flexível e confiável que um não habilitado. Seu aplicativo avisa ao sistema que é habilitado declarando a funcionalidade restrita **enterpriseDataPolicy**. Entretanto, ser habilitado significa mais do que definir uma funcionalidade. Para obter mais informações, consulte [Aplicativos habilitados para empresas](../enterprise/edp-hub.md#enterprise-enlightened-apps).

-   **Entender programação assíncrona para aplicativos UWP (Plataforma Universal do Windows)**

    Para saber mais sobre como escrever aplicativos assíncronos em in C\# ou Visual Basic, consulte [Chamar APIs assíncronas no Visual Basic ou C#](https://msdn.microsoft.com/library/windows/apps/mt187337). Para saber como escrever aplicativos assíncronos em C++, consulte [Programação assíncrona em C++](https://msdn.microsoft.com/library/windows/apps/mt187334).

## Acessando recursos da empresa pela rede


Neste cenário, um aplicativo de email habilitado está sincronizando um conjunto de caixas de correio que são uma mistura de caixas de correio pessoais e corporativas. O aplicativo passa a identidade do usuário para uma chamada a [**ProtectionPolicyManager.CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) para criar um contexto de thread protegido. Isso marca todas as conexões de rede que são feitas subsequentemente no mesmo thread com essa identidade, e permite o acesso a recursos de rede da empresa que são de acesso controlado pela política da empresa.

Aqui, "a empresa" refere-se à empresa à qual a identidade do usuário pertence. 
            [
              **CreateCurrentThreadNetworkContext**
            ](https://msdn.microsoft.com/library/windows/apps/dn706025) retorna um objeto [**ThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706029) independentemente da aplicação da política. Em geral, se o aplicativo espera lidar com recursos mistos, é possível optar por chamar **CreateCurrentThreadNetworkContext** para todas as identidades. Depois de recuperar recursos de rede, o aplicativo chama **Dispose** no **ThreadNetworkContext** para limpar qualquer marca de identidade do thread atual. O padrão que você usa para descartar o objeto de contexto dependerá da sua linguagem de programação.

Se a identidade for desconhecida, o aplicativo poderá consultar a identidade gerenciada pela política empresarial no endereço de rede do recurso usando a API [**ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync**](https://msdn.microsoft.com/library/windows/apps/dn706027).


            **Observação**  Como você pode ver no exemplo de código, o uso correto padrão para [**CreateCurrentThreadNetworkContext**](https://msdn.microsoft.com/library/windows/apps/dn706025) é manter no mínimo o escopo no qual ele está em vigor. Você deve definir o contexto de rede corporativa, criar conexões de rede e, em seguida, reverter o contexto, usar as conexões e fechá-las. O exemplo de código abaixo ilustra os detalhes. É importante não criar arquivos em um thread enquanto o contexto de rede corporativa é definido nesse thread. Isso fará o arquivo ser criptografado automaticamente, independentemente da intenção do arquivo ser pessoal. Esse é um dos motivos pelos quais recomendamos reverter o contexto o quanto antes.



```CSharp
using Windows.Data.Xml.Dom;
using Windows.Networking;
using Windows.Security.EnterpriseData;
using Windows.Storage.Streams;
using Windows.Web.Http;

...

public static async void SyncMailbox(string identity)
{
    HttpClient client = null;
    // Create a protected network context for "identity" on the current
    // thread. Network connections made after this call will be tagged
    // to "identity", and will have policy enforced. This is required
    // to access enterprise network resources for "identity".
    using (ThreadNetworkContext threadNetworkContext = 
        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(identity))
    {
        // Retrieve new mail.
        client = new HttpClient();
    }

    string xmlResponse = await client.GetStringAsync
        (new Uri("https://contosomail/getnewmail?user=" + identity));

    // Now, process mail data received.
    var xmlDocument = new XmlDocument();
    xmlDocument.LoadXml(xmlResponse);
    foreach (IXmlNode mailNode in xmlDocument.GetElementsByTagName("mail"))
    {
        // Code to process text data in mail (body, recipients etc.)
        // would go here ...

        // Processes resource links in mail body.
        foreach (IXmlNode childNode in mailNode.ChildNodes)
        {
            if (childNode.NodeName == "resource")
            {
                var resourceUri = new Uri(childNode.InnerText);

                // Check if the resource is on an enterprise-managed endpoint.
                string resourceIdentity =
                    await ProtectionPolicyManager.GetPrimaryManagedIdentityForNetworkEndpointAsync
                        (new HostName(resourceUri.Host));
                if (!string.IsNullOrEmpty(resourceIdentity))
                {
                    using (ThreadNetworkContext threadNetworkContext =
                        ProtectionPolicyManager.CreateCurrentThreadNetworkContext(resourceIdentity))
                    {
                        client = new HttpClient();
                    }
                    IBuffer data = await client.GetBufferAsync(resourceUri);
                    // Code to process retrieved resource data would go here...
                }
            }
        }
    }
}
```


            **Observação**  Este artigo se destina a desenvolvedores do Windows 10 que elaboram aplicativos UWP (Plataforma Universal do Windows). Se você estiver desenvolvendo para Windows 8.x ou Windows Phone 8.x, consulte a [documentação arquivada](http://go.microsoft.com/fwlink/p/?linkid=619132).



## Tópicos relacionados


[exemplo de EDP (proteção de dados empresariais)](http://go.microsoft.com/fwlink/p/?LinkId=620031&clcid=0x409)

[**Windows.Security.EnterpriseData namespace**](https://msdn.microsoft.com/library/windows/apps/dn279153)

 

 






<!--HONumber=Jun16_HO4-->


