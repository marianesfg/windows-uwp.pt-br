---
ms.topic: include
author: mattwojo
ms.author: mattwoj
ms.date: 10/04/2019
ms.openlocfilehash: 2f7a57f1652ecab81a70c39faa1b70c42ed6a3de
ms.sourcegitcommit: 13faf9dab9946295986f8edd79b5fae0db4ed0f6
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/15/2019
ms.locfileid: "72314900"
---
Digitar `sudo service mongodb start` ou `sudo service postgres start` e `sudo -u postgrest psql` pode ser entediante.  No entanto, você pode considerar a configuração de aliases no arquivo `.profile` no WSL para tornar esses comandos mais rápidos e fáceis de se lembrar. 

Para configurar seu próprio alias personalizado, ou atalho, para executar estes comandos:

1. Abra o terminal do WSL e insira `cd ~` para ter certeza de que você está no diretório raiz.
2. Abra o arquivo `.profile`, que controla as configurações do seu terminal, com o editor de texto do terminal, nano: `sudo nano .profile`
3. Na parte inferior do arquivo (não altere as configurações do `# set PATH`), adicione o seguinte:

    ```bash
    # My Aliases
    alias start-pg='sudo service postgresql start'
    alias run-pg='sudo -u postgres psql'
    ```

Isso permitirá que você insira `start-pg` para iniciar a execução do serviço PostgreSQL e `run-pg` para abrir o Shell psql. Você pode alterar `start-pg` e `run-pg` para quaisquer nomes que desejar, apenas tenha cuidado para não substituir um comando que postgres já usa!

4. Depois de adicionar seus novos aliases, saia do editor de texto do nano usando **Ctrl + X** --selecione `Y` (Sim) quando for solicitado a salvar e inserir (deixando o nome do arquivo como `.profile`).
5. Feche e abra novamente o terminal do WSL e tente os novos comandos de alias.
