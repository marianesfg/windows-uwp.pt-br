---
title: Usar um banco de dados MySQL em um aplicativo UWP
description: Use um banco de dados MySQL em um aplicativo UWP.
ms.date: 03/28/2019
ms.topic: article
keywords: windows 10, uwp, MySQL, database
ms.localizationpriority: medium
ms.openlocfilehash: bfed9c0a0c4198095b9be48fe71832bdfca67718
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "67713786"
---
# <a name="use-a-mysql-database"></a>Usar um banco de dados do MySQL
Este artigo contém as etapas necessárias para habilitar o trabalho com um banco de dados MySQL de um aplicativo UWP. Também contém um pequeno snippet de código que mostra como é possível interagir com o banco de dados no código.

## <a name="set-up-your-solution"></a>Configurar sua solução

Para conectar seu aplicativo diretamente a um banco de dados MySQL, verifique se a versão mínima do seu projeto tem como alvo a atualização do Fall Creators (Build 16299).  Essas informações estão na página de propriedades do seu projeto da UWP.

![Imagem do painel de propriedades Direcionamento no Visual Studio mostrando as versões de destino e mínima definidas para a atualização Fall Creators](images/min-version-fall-creators.png)

Abra o **Console do Gerenciador de Pacotes** (Exibir -> Outras Janelas -> Console do Gerenciador de Pacotes). Use o comando **Install-Package MySql.Data** para instalar o driver do MySQL. Isso permitirá que você acesse programaticamente os bancos de dados MySQL.

## <a name="test-your-connection-using-sample-code"></a>Testar sua conexão usando o exemplo de código
A seguir veja um exemplo de como conectar-se a um banco de dados remoto MySQL e fazer leituras dele. Observe que o endereço IP, as credenciais e o nome do banco de dados precisarão ser personalizados.

```csharp
string M_str_sqlcon = "server=10.xxx.xx.xxx;user id=foo;password=bar;database=baz";
MySqlConnection mysqlcon = new MySqlConnection(M_str_sqlcon);
MySqlCommand mysqlcom = new MySqlCommand("select * from table1", mysqlcon);
mysqlcon.Open();
MySqlDataReader mysqlread = mysqlcom.ExecuteReader(CommandBehavior.CloseConnection);
while (mysqlread.Read())
{
    Debug.WriteLine(mysqlread.GetString(0)+":"+mysqlread.GetString(1));
}
mysqlcon.Close();
```
