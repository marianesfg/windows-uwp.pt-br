---
title: Usar um banco de dados do MongoDB em um aplicativo UWP
description: Use um banco de dados do MongoDB em um aplicativo UWP.
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MongoDB, banco de dados
ms.localizationpriority: medium
ms.openlocfilehash: ea3e630a3bb0b8fc5f7f4168b0b946f115b97446
ms.sourcegitcommit: 139717a79af648a9231821bdfcaf69d8a1e6e894
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 07/10/2019
ms.locfileid: "67713993"
---
# <a name="use-a-mongodb-database"></a>Usar um banco de dados do MongoDB
Este artigo contém as etapas necessárias para habilitar o trabalho com um banco de dados do MongoDB de um aplicativo UWP. Também contém um pequeno trecho de código que mostra como é possível interagir com o banco de dados no código.

## <a name="set-up-your-solution"></a>Configurar sua solução

Para conectar seu aplicativo diretamente a um banco de dados do MongoDB, verifique se a versão mínima do seu projeto tem como alvo a atualização Fall Creators (Build 16299).  Essas informações estão na página de propriedades do seu projeto da UWP.

![Imagem do painel de propriedades Direcionamento no Visual Studio mostrando as versões de destino e mínima definidas para a atualização Fall Creators](images/min-version-fall-creators.png)

Abra o **Console do Gerenciador de Pacotes** (Exibir -> Outras Janelas -> Console do Gerenciador de Pacotes). Use o comando **Install-Package MongoDB** para instalar o driver do MongoDB. Isso permitirá acessar os bancos de dados do MongoDB via programação.

## <a name="test-your-connection-using-sample-code"></a>Testar sua conexão usando o exemplo de código
O exemplo de código a seguir obtém uma coleção de um cliente remoto do MongoDB e adiciona um novo documento a essa coleção. Em seguida, usa as APIs do MongoDB para recuperar o novo tamanho da coleção, o documento inserido e os imprime. Observe que os nomes dos bancos de dados e o endereço IP precisarão ser personalizados.

```csharp
var client = new MongoClient("mongodb://10.xxx.xx.xxx:xxx");
IMongoDatabase database = client.GetDatabase("foo");
IMongoCollection<BsonDocument> collection = database.GetCollection<BsonDocument>("bar");
BsonDocument document = new BsonDocument
{
     { "name","MongoDB"},
     { "type","Database"},
     { "count",1},
     { "info",new BsonDocument { { "x", 203 }, { "y", 102 } }}
};
collection.InsertOne(document);
long count = collection.CountDocuments(document);
Debug.WriteLine(count);
IFindFluent<BsonDocument, BsonDocument> document1 = collection.Find(document);
Debug.WriteLine(document1.ToString());
```
