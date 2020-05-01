---
title: Salvar e carregar configurações em um aplicativo UWP
description: Saiba como salvar e carregar configurações do aplicativo em aplicativos da Plataforma Universal do Windows.
ms.date: 05/07/2018
ms.topic: article
keywords: introdução, uwp, windows 10, caminho de aprendizagem, configurações, salvar configurações, carregar configurações
ms.localizationpriority: medium
ms.custom: RS5
ms.openlocfilehash: 490dd8f0f3841fae089626ec9c283d54cc0d8cd9
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "66370491"
---
# <a name="save-and-load-settings-in-a-uwp-app"></a>Salvar e carregar configurações em um aplicativo UWP

Este tópico aborda o que você precisa saber para começar a carregar, salvar configurações em um aplicativo UWP (Plataforma Universal do Windows). As APIs principais são introduzidas e os links são fornecidos para ajudar você a aprender mais.

Use as configurações para lembrar os aspectos personalizáveis pelo usuário de seu aplicativo. Por exemplo, um leitor de notícias pode usar as configurações do aplicativo para salvar as fontes de notícias para exibição e qual fonte será usada para ler artigos.

Vamos examinar o código para salvar e carregar as configurações do aplicativo, incluindo as configurações locais e em roaming.

## <a name="what-do-you-need-to-know"></a>O que você precisa saber

Use as configurações do aplicativo para armazenar dados de configuração, como as preferências do usuário e o estado do aplicativo.  Configurações específicas do dispositivo são armazenadas localmente. Configurações que se aplicam a qualquer dispositivo no qual o aplicativo está instalado são armazenadas no armazenamento de dados de roaming. Configurações fazem roaming entre os dispositivos nos quais o usuário está conectado com a mesma Conta Microsoft e têm a mesma versão do aplicativo instalado.

Os seguintes tipos de dados podem ser usados com as configurações: inteiros, duplicatas, flutuações, caracteres, cadeias de caracteres, pontos, DateTimes e muito mais. Você também pode armazenar instâncias da classe [ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue), o que é útil quando há várias configurações que devem ser tratadas como uma unidade. Por exemplo, um nome de fonte e um tamanho de ponto para exibir texto no painel de leitura do aplicativo deve ser salvo/restaurado como uma única unidade. Isso impede que uma configuração fique fora de sincronia com a outra devido a atrasos de roaming de uma configuração antes da outra.

Veja as principais APIs que você precisa conhecer para salvar ou carregar configurações do aplicativo:

- [Windows.Storage.ApplicationData.Current.LocalSettings](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationData#Windows_Storage_ApplicationData_LocalSettings) obtém o contêiner de configurações do aplicativo do armazenamento de dados de aplicativo local. Armazene aqui as configurações que não podem ser compartilhadas entre dispositivos, pois representam o estado específico do dispositivo ou são muito grandes.
- [Windows.Storage.ApplicationData.Current.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings#Windows_Storage_ApplicationData_RoamingSettings) obtém o contêiner de configurações do aplicativo do armazenamento de dados do aplicativo em roaming. Esses dados passam em roaming entre dispositivos.
- [Windows.Storage.ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) é um contêiner que representa as configurações de aplicativo como pares chave/valor. Use essa classe para criar e recuperar os valores de configuração.
- [Windows.Storage.ApplicationDataCompositeValue](https://docs.microsoft.com/uwp/api/Windows.Storage.ApplicationDataCompositeValue) representa diversas configurações de aplicativo que devem ser serializadas como uma unidade. Isso é útil quando uma configuração não deve ser atualizada independentemente de outra.

## <a name="save-app-settings"></a>Salvar configurações do aplicativo

Para essa introdução, destacaremos dois cenários simples: salvar e carregar um aplicativo definindo localmente e fazer o roaming da configuração de fonte composta/tamanho da fonte entre dispositivos.

 ```csharp
// Save a setting locally on the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
localSettings.Values["test setting"] = "a device specific setting";

// Save a composite setting that will be roamed between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = new Windows.Storage.ApplicationDataCompositeValue();
composite["Font"] = "Calibri";
composite["FontSize"] = 11;
roamingSettings.Values["RoamingFontInfo"] = composite;
 ```

Salve uma configuração no dispositivo local obtendo primeiro um **ApplicationDataContainer** para o armazenamento de dados de configurações local com `Windows.Storage.ApplicationData.Current.LocalSettings`. Os pares de dicionário de chave/valor que você atribuir a essa instância serão salvos no armazenamento de dados de configuração do dispositivo local.

Salve uma configuração de roaming usando um padrão semelhante. Primeiro obtenha **ApplicationDataContainer** para o armazenamento de dados de configurações de roaming com `Windows.Storage.ApplicationData.Current.RoamingSettings`. Em seguida, atribua pares de chave/valor a essa instância.  Os pares chave/valor serão compartilhados automaticamente entre dispositivos.

No snippet de código acima, uma **ApplicationDataCompositeValue** armazena vários pares chave/valor. Valores de composição são úteis quando você tem várias configurações que não devem ficar fora de sincronia entre si. Ao salvar um **ApplicationDataCompositeValue**, os valores são salvos e carregados como uma unidade ou atomicamente. Assim, as configurações relacionadas não ficam fora de sincronia, pois fazem roaming como uma unidade em vez de individualmente.

## <a name="load-app-settings"></a>Carregar configurações de aplicativo

```csharp
// load a setting that is local to the device
ApplicationDataContainer localSettings = Windows.Storage.ApplicationData.Current.LocalSettings;
String localValue = localSettings.Values["test setting"] as string;

// load a composite setting that roams between devices
ApplicationDataContainer roamingSettings = Windows.Storage.ApplicationData.Current.RoamingSettings;
Windows.Storage.ApplicationDataCompositeValue composite = (ApplicationDataCompositeValue)roamingSettings.Values["RoamingFontInfo"];
if (composite != null)
{
    String fontName = composite["Font"] as string;
    int fontSize = (int)composite["FontSize"];
}
```

Carregue uma configuração usando o dispositivo local obtendo primeiro uma instância **ApplicationDataContainer** do armazenamento de dados de configurações locais com `Windows.Storage.ApplicationData.Current.LocalSettings`. Em seguida, use- a para recuperar pares chave/valor.

Carregue uma configuração de roaming seguindo um padrão semelhante. Primeiro obtenha uma instância **ApplicationDataContainer** para o armazenamento de dados de configurações de roaming com `Windows.Storage.ApplicationData.Current.RoamingSettings`. Acesse pares de chave/valor dessa instância. Se os dados ainda não foram movidos para o dispositivo usado para acessar as configurações, você receberá um **ApplicationDataContainer** nulo. Por isso há uma verificação `if (composite != null)` no código de exemplo acima.

## <a name="useful-apis-and-docs"></a>APIs e documentos úteis

Veja um resumo rápido de APIs e outras documentações úteis para ajudar você a começar a salvar e carregar configurações de aplicativo.

### <a name="useful-apis"></a>APIs úteis

| API | Descrição |
|------|---------------|
| [ApplicationData.LocalSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.temporaryfolder) | Obtém o contêiner de configurações do aplicativo do armazenamento de dados do aplicativo local. |
| [ApplicationData.RoamingSettings](https://docs.microsoft.com/uwp/api/windows.storage.applicationdata.roamingsettings) | Obtém o contêiner de configurações do aplicativo do armazenamento de dados do aplicativo em roaming. |
| [ApplicationDataContainer](https://docs.microsoft.com/uwp/api/windows.storage.applicationdatacontainer) | Um contêiner para configurações de aplicativo que dê suporte para criar, excluir, enumerar e percorrer a hierarquia do contêiner. |
| [Namespace Windows.UI.ApplicationSettings](https://docs.microsoft.com/uwp/api/windows.ui.applicationsettings) | Fornece classes que você usará para definir as configurações do aplicativo que aparecem no painel configurações do Shell do Windows. |

### <a name="useful-docs"></a>Documentos úteis

| Tópico | Descrição |
|-------|----------------|
| [Diretrizes para configurações de aplicativos](https://docs.microsoft.com/windows/uwp/design/app-settings/guidelines-for-app-settings) | Descreve as melhores práticas para criar e exibir configurações de aplicativo. |
| [Armazene e recupere configurações e outros dados de aplicativo](https://docs.microsoft.com/windows/uwp/design/app-settings/store-and-retrieve-app-data#create-and-read-a-local-file) | Passo a passo para salvar e recuperar configurações, incluindo configurações de roaming. |

## <a name="useful-code-samples"></a>Exemplos de código úteis

| Exemplo de código | Descrição |
|-----------------|---------------|
| [Amostra de dados de aplicativos](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/ApplicationData) | Cenários 2 a 4: foco em configurações |
