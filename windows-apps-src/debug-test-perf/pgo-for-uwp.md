---
title: Execução da otimização guiada por perfil (PGO) em aplicativos da Plataforma Universal do Windows (UWP)
description: Um guia passo a passo para a aplicação de Otimização Guiada por perfil (PGO) para aplicativos da plataforma Universal do Windows (UWP).
ms.date: 02/08/2017
ms.localizationpriority: medium
ms.topic: article
ms.openlocfilehash: 8c19ea1701c6b5e82e66a54223620dace57de4b6
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57632911"
---
# <a name="running-profile-guided-optimization-on-universal-windows-platform-apps"></a>Execução da otimização guiada por perfil em aplicativos da Plataforma Universal do Windows 
 
Este tópico apresenta um guia passo a passo para aplicar a otimização guiada por perfil (PGO) a aplicativos da Plataforma Universal do Windows (UWP). Como nem todas as etapas disponíveis para aplicativos win32 clássicos estão disponíveis para aplicativos UWP, a meta é explicar o processo necessário para incorporar PGO a fim de deixar a otimização fácil e mais acessível para os desenvolvedores UWP.

Este é um passo a passo básico da aplicação de PGO ao modelo de aplicativo (UWP) DirectX 11 padrão usando-se o Visual Studio 2015 Update 3.
 
As capturas de tela em todo este guia baseiam-se o novo projeto a seguir: ![Caixa de diálogo Nova projeto](images/pgo-001.png)

Para aplicar PGO ao modelo de aplicativo DirectX 11:

1. Defina a configuração da solução como **Versão** ou escolha uma configuração de solução na qual você esteja gerando código otimizado destinado à versão. Embora pudesse executar, teoricamente, PGO em uma compilação de depuração, seria ineficaz usar PGO para otimizar uma compilação não otimizada. 
 
 ![Janela App1](images/pgo-002.png)
 
2. Verifique, nas propriedades do projeto (**Propriedades** > **C/C++** > **Otimização**), se você está compilando com o sinalizador /GL definido como **Otimização do Programa Inteiro** (isso talvez já esteja definido pela configuração).

 ![Otimização do Programa Inteiro](images/pgo-003.png)

3. Vá até as propriedades do vinculador (**Propriedades** > **Vinculador** > **Otimização**) e defina **Geração de Código Durante o Tempo de Vinculação** como **Otimização Guiada por Perfil - Instrumento (LTCG:PGInstrument)**.
 
 ![Geração de Código Durante o Tempo de Vinculação](images/pgo-004.png)

4. Selecione **Compilar Solução** e **Implantar Solução**. 

 ![Caixa de diálogo Novo Projeto](images/pgo-005.png)
 
 Você pode verificar se tudo funcionou corretamente observando o local de saída da compilação e verificando se um arquivo .pgd foi gerado. Neste caso de exemplo, isso significa que o seguinte arquivo foi gerado com a saída da compilação:
 
 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd`

 Por padrão, o arquivo .pgd terá o mesmo nome do executável. Você também pode alterar o nome do arquivo .pgd gerado, mudando opção do vinculador **Banco de Dados Guiado por Perfil**. 
 
5. Navegue até o diretório de binários VC do Visual Studio (por padrão, algo parecido com `C:\Program Files (x86)\Microsoft Visual Studio 14.0\VC\bin`). Para executáveis x86, copie `pgort140.dll`; para executáveis x64, copie a versão x64 de `amd64\pgort140.dll`. Cole a versão apropriada de `pgort140.dll` na raiz do pacote implantado. Para essa amostra o caminho é:

 `C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\AppX\`

 Esta etapa é necessária porque aplicativos UWP só podem carregar as bibliotecas existentes dentro do pacote.

 ![Caixa de diálogo Novo Projeto](images/pgo-006.png)
 
6. Execute o aplicativo no menu Iniciar ou no menu **Depurar** do Visual Studio com a opção **Iniciar sem Depuração**. 

 ![Caixa de diálogo Novo Projeto](images/pgo-007.png)
 
7. A compilação em execução está instrumentada e gerando dados PGO. A esta altura, você deve executar o aplicativo por meio de alguns dos cenários mais comuns que pretende otimizar. Depois que o programa for executado nos cenários desejados, encontre a ferramenta pgosweep.exe localizada na mesma pasta onde você encontrou a versão apropriada de `pgort140.dll`. Como alternativa, um Prompt de Comando de Ferramentas Nativas do Visual Studio (x86/x64) já terá a versão apropriada no caminho. Para coletar os dados PGO, execute o seguinte comando enquanto o aplicativo ainda estiver em execução para gerar um arquivo .pgc que conterá os dados de criação de perfil:
 
  `pgosweep.exe <executable name> <output file>` 
 
  Você também pode examinar a ajuda de pgosweep.exe (`pgosweep.exe /help`) para exibir outros argumentos opcionais a fim controlar a maneira como coleta dados PGO.
 
  É uma boa ideia colocar os arquivos .pgc no local da compilação onde o .pgd está localizado e nomear os arquivos como `<PGDName>!<RunIdentifier>.pgc`. Para esse exemplo, isso significava:
 
  ```
  pgosweep.exe App1.exe “C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!1.pgc”
  ```
 
  Também poderia ser ainda mais coletando `App1!CoreScenario.pgc`, `App1!UseCase5.pgc`, etc. Se os arquivos. PGC forem nomeados dessa forma e o local de saída da compilação junto com o PGD, eles serão automaticamente mesclados ao vincular na etapa 9.
 
8. OPCIONAL: Por padrão, todos os arquivos. PGC nomeados como especificado na etapa 7 e colocado ao lado de PGD serão mesclados quando vinculação e igualmente ponderados, mas você também pode ter maior controle em particular como execuções são ponderadas. Para isso, você usará a ferramenta **pgomgr.exe** também localizada na mesma pasta onde encontrou inicialmente a cópia de `pgort140.dll`. Por exemplo, para mesclar a execução `CoreScenario` com 3 vezes a prioridade de outras execuções, posso usar o seguinte comando:
 
 ```
 pgomgr.exe -merge:3 “C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1!CoreScenario.pgc” “C:\Users\<USER>\Documents\Visual Studio 2015\Projects\App1\Release\App1\App1.pgd”
 ```
 
9. Depois que você tiver gerado um ou mais arquivos .pgc e os colocado com o .pgd ou os mesclado manualmente (etapa 8), poderemos usar vinculador para criar a compilação otimizada final. Volte para as propriedades do vinculador (**Propriedades** > **Vinculador** > **Otimização**) e defina **Geração de Código Durante o Tempo de Vinculação** como **Otimização Guiada Por Perfil - Otimização (LTCG:PGOptimize)** e verifique se **Banco de Dados Guiado por Perfil** está apontando para o .pgd que você deseja usar (se você não tiver alterado isso, tudo estará em ordem).

 ![Caixa de diálogo Novo Projeto](images/pgo-009.png)
 
10. Quando o projeto for compilado, o vinculador chamará pgomgr.exe para mesclar todos os arquivos `<PGDName>!*.pgc` no .pgd com o peso padrão 1, e o aplicativo resultante será otimizado com base nos dados da criação de perfil.

## <a name="see-also"></a>Consulte também
- [Desempenho](performance-and-xaml-ui.md)

 

