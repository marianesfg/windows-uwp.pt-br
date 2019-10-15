---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314880"
---
Para instalar o MongoDB:

1. Abra o terminal do WSL (isto é, Ubuntu 18, 4).
2. Atualize seus pacotes do Ubuntu: `sudo apt update`
3. Depois que os pacotes forem atualizados, instale o MongoDB com: `sudo apt-get install mongodb`
4. Confirme a instalação e obtenha o número de versão: `mongod --version`

Há três comandos que você precisa saber quando o MongoDB está instalado:

1. `sudo service mongodb status` para verificar o status do seu banco de dados.
2. `sudo service mongodb start` para iniciar a execução do banco de dados.
3. `sudo service mongodb stop` para parar de executar o banco de dados.

> [!NOTE]
> Você pode ver o comando `sudo systemctl status mongodb` usado em tutoriais ou artigos. Para permanecer leve, o WSL não inclui `systemd` (um sistema de gerenciamento de serviços no Linux). Em vez disso, ele usa SysVinit para iniciar serviços em seu computador. Você não deve notar uma diferença, mas se um tutorial recomendar o uso de `sudo systemctl`, use: `sudo /etc/init.d/`. Por exemplo, `sudo systemctl status mongodb`, para WSL seria `sudo /etc/inid.d/mongodb status`... ou você também pode usar `sudo service mongodb status`.

### <a name="run-your-mongo-database-in-a-local-server"></a>Executar o banco de dados Mongo em um servidor local

1. Verifique o status do seu banco de dados: `sudo service mongodb status` você deverá ver uma resposta [falha], a menos que você já tenha iniciado seu banco de dados.

2. Inicie seu banco de dados: `sudo service mongodb start` agora você deve ver uma resposta [OK].

3. Verifique conectando-se ao servidor de banco de dados e executando um comando de diagnóstico: `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` isso produzirá a versão atual do banco de dados, o endereço do servidor e a porta e a saída do comando status. Um valor de `1` para o campo "OK" na resposta indica que o servidor está funcionando.

4. Para interromper a execução do serviço MongoDB, insira: `sudo service mongodb stop`

> [!NOTE]
> O MongoDB tem vários parâmetros padrão, incluindo o armazenamento de dados no/data/DB e em execução na porta 27017. Além disso, `mongod` é o daemon (processo de host para o banco de dados) e `mongo` é o Shell de linha de comando que se conecta a uma instância específica do `mongod`.
