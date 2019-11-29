---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: aa1da2a65f95d92e895533ed37426b5e454c255b
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314890"
---
Para instalar o PostgreSQL:

1. Abra o terminal do WSL (ou seja, Ubuntu 18.04).
2. Atualize os pacotes do Ubuntu: `sudo apt update`
3. Depois que os pacotes forem atualizados, instale o PostgreSQL (e o pacote -contrib que tem alguns utilitários úteis) com: `sudo apt install postgresql postgresql-contrib`
4. Confirme a instalação e obtenha o número de versão: `psql --version`

Há três comandos que você precisará saber quando o PostgreSQL estiver instalado:

1. `sudo service postgresql status` para verificar o status do banco de dados.
2. `sudo service postgresql start` para iniciar a execução do banco de dados.
3. `sudo service postgresql stop` para interromper a execução do banco de dados.

### <a name="postgresql-user-setup"></a>Configuração de usuário do PostgreSQL

O usuário administrador padrão, `postgres`, precisa de uma senha atribuída para se conectar a um banco de dados. Para definir uma senha:

1. Insira o comando: `sudo passwd postgres`
2. Você deverá inserir sua nova senha.
3. Feche e abra novamente o terminal.

### <a name="run-postgresql-with-psql-shell"></a>Executar o PostgreSQL com o shell do psql

O [psql](https://www.postgresql.org/docs/10/app-psql.html) é um front-end baseado em terminal para o PostgreSQL. Ele permite que você digite consultas de maneira interativa, emita-as para o PostgreSQL e veja os resultados da consulta. Como alternativa, a entrada pode ser proveniente de um arquivo. Além disso, ele fornece uma série de metacomandos e vários recursos semelhantes ao shell para facilitar a escrita de scripts e a automatização de uma ampla variedade de tarefas.

Para iniciar o shell do psql:

1. Inicie o serviço Postgres: `sudo service postgresql start`
2. Conecte-se ao serviço Postgres e abra o shell do psql: `sudo -u postgres psql`

Depois de entrar no shell do psql com êxito, você verá a linha de comando ser alterada para ter a seguinte aparência: `postgres=#`

> [!NOTE]
> Como alternativa, você pode abrir o shell do psql alternando para o usuário do Postgres com `su - postgres` e, em seguida, inserindo o comando `psql`.

Para sair do postgres=# insira `\q` ou use a tecla de atalho: Ctrl+D

Para ver quais contas de usuário foram criadas na instalação do PostgreSQL, use `psql -c "\du"` no terminal do WSL ou apenas `\du` se o shell do psql estiver aberto. Este comando exibirá colunas: Nome de Usuário da Conta, Lista de Atributos de Funções e Membro de grupos de funções. Para voltar à linha de comando, insira `q`.
