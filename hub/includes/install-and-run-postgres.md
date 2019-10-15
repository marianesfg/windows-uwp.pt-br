---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314890"
---
Para instalar o PostgreSQL:

1. Abra o terminal do WSL (isto é, Ubuntu 18, 4).
2. Atualize seus pacotes do Ubuntu: `sudo apt update`
3. Depois que os pacotes forem atualizados, instale o PostgreSQL (e o pacote-contrib que tem alguns utilitários úteis) com: `sudo apt install postgresql postgresql-contrib`
4. Confirme a instalação e obtenha o número de versão: `psql --version`

Há três comandos que você precisa saber quando o PostgreSQL está instalado:

1. `sudo service postgresql status` para verificar o status do seu banco de dados.
2. `sudo service postgresql start` para iniciar a execução do banco de dados.
3. `sudo service postgresql stop` para parar de executar o banco de dados.

### <a name="postgresql-user-setup"></a>Configuração de usuário do PostgreSQL

O usuário administrador padrão, `postgres`, precisa de uma senha atribuída para se conectar a um banco de dados. Para definir uma senha:

1. Insira o comando: `sudo passwd postgres`
2. Você receberá uma solicitação para inserir sua nova senha.
3. Feche e reabra o terminal.

### <a name="run-postgresql-with-psql-shell"></a>Executar PostgreSQL com Shell psql

[psql](https://www.postgresql.org/docs/10/app-psql.html) é um front-end baseado em terminal para PostgreSQL. Ele permite que você digite consultas interativamente, emita-as para PostgreSQL e veja os resultados da consulta. Como alternativa, a entrada pode ser de um arquivo. Além disso, ele fornece uma série de comandos meta e vários recursos semelhantes ao shell para facilitar a gravação de scripts e a automatização de uma ampla variedade de tarefas.

Para iniciar o Shell psql:

1. Inicie o serviço postgres: `sudo service postgresql start`
2. Conecte-se ao serviço postgres e abra o Shell psql: `sudo -u postgres psql`

Depois de inserir o Shell do psql com êxito, você verá a alteração da linha de comando para ter esta aparência: `postgres=#`

> [!NOTE]
> Como alternativa, você pode abrir o Shell psql alternando para o usuário postgres com: `su - postgres` e, em seguida, inserindo o comando: `psql`.

Para sair de postgres = # Enter: `\q` ou use a tecla de atalho: Ctrl+D

Para ver quais contas de usuário foram criadas na instalação do PostgreSQL, use do seu terminal WSL: `psql -c "\du"`... ou apenas `\du` se você tiver o Shell psql aberto. Este comando exibirá colunas: Nome de usuário da conta, lista de atributos de funções e membro de grupos de função. Para sair de volta para a linha de comando, digite: `q`.
