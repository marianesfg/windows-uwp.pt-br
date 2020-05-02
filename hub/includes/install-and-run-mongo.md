---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: f594600991f08a7dfda784ae127be2e6438dacbd
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314880"
---
Para instalar o MongoDB:

1. Abra o terminal do WSL (ou seja, Ubuntu 18.04).
2. Atualize os pacotes do Ubuntu: `sudo apt update`
3. Depois que os pacotes forem atualizados, instale o MongoDB com: `sudo apt-get install mongodb`
4. Confirme a instalação e obtenha o número de versão: `mongod --version`

Há três comandos que você precisará saber quando o MongoDB estiver instalado:

1. `sudo service mongodb status` para verificar o status do banco de dados.
2. `sudo service mongodb start` para iniciar a execução do banco de dados.
3. `sudo service mongodb stop` para interromper a execução do banco de dados.

> [!NOTE]
> Você pode ver o comando `sudo systemctl status mongodb` usado em tutoriais ou artigos. Para permanecer leve, o WSL não inclui `systemd` (um sistema de gerenciamento de serviços no Linux). Em vez disso, ele usa o SysVinit para iniciar serviços no computador. Você não deverá notar uma diferença, mas se um tutorial recomendar o uso de `sudo systemctl`, use `sudo /etc/init.d/`. Por exemplo, `sudo systemctl status mongodb` para o WSL será `sudo /etc/inid.d/mongodb status`, ou você também pode usar `sudo service mongodb status`.

### <a name="run-your-mongo-database-in-a-local-server"></a>Executar o banco de dados Mongo em um servidor local

1. Verifique o status do banco de dados: `sudo service mongodb status` Você deverá ver uma resposta [Falha], a menos que já tenha iniciado o banco de dados.

2. Inicie o banco de dados: `sudo service mongodb start` Agora você deverá ver uma resposta [OK].

3. Verifique isso conectando-se ao servidor de banco de dados e executando um comando de diagnóstico: `mongo --eval 'db.runCommand({ connectionStatus: 1 })'` Isso produzirá a versão atual do banco de dados, o endereço do servidor e a porta, bem como a saída do comando de status. Um valor igual a `1` para o campo "OK" na resposta indica que o servidor está funcionando.

4. Para interromper a execução do serviço do MongoDB, insira: `sudo service mongodb stop`

> [!NOTE]
> O MongoDB tem vários parâmetros padrão, incluindo o armazenamento de dados em /data/db e a execução na porta 27017. Além disso, `mongod` é o daemon (processo de host para o banco de dados) e `mongo` é o shell de linha de comando que se conecta a uma instância específica do `mongod`.
