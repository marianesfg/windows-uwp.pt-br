---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 76e8b4fb3f76cc162aab80982a441bfc18507fb4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 04/29/2020
ms.locfileid: "72314900"
---
Digitar `sudo service mongodb start` ou `sudo service postgres start` e `sudo -u postgrest psql` pode ser entediante.  No entanto, você pode considerar a configuração de aliases no arquivo `.profile` no WSL para tornar esses comandos mais rápidos e fáceis de serem lembrados. 

Para configurar seu próprio alias personalizado ou atalho para execução destes comandos:

1. Abra o terminal do WSL e insira `cd ~` para ter certeza de que você está no diretório raiz.
2. Abra o arquivo `.profile`, que controla as configurações do terminal, com o editor de texto do terminal, o Nano: `sudo nano .profile`
3. Na parte inferior do arquivo (não altere as configurações de `# set PATH`), adicione o seguinte:

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Isso permitirá que você insira `start-pg` para iniciar a execução do serviço PostgreSQL e `run-pg` para abrir o shell do psql. Você pode alterar `start-pg` e `run-pg` para os nomes que desejar; apenas tenha cuidado para não substituir um comando que o Postgres já usa.

4. Depois de adicionar seus novos aliases, saia do editor de texto Nano usando **Ctrl+X** – selecione `Y` (Sim) quando precisar salvar o conteúdo e Enter (deixando o nome de arquivo como `.profile`).
5. Feche e abra novamente o terminal do WSL e, em seguida, experimente os novos comandos de alias.
