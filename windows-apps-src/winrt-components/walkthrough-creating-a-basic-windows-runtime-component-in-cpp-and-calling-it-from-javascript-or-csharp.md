---
title: Criando um componente do Tempo de Execução do Windows básico em C++ e chamando-o em JavaScript ou C#
description: Esse procedimento passo a passo mostra como criar uma DLL de componente do Tempo de Execução do Windows básica que pode ser chamada em JavaScript, C# ou Visual Basic.
ms.assetid: 764CD9C6-3565-4DFF-88D7-D92185C7E452
---

# Procedimento passo a passo: criando um componente do Tempo de Execução do Windows básico em C++ e chamando-o em JavaScript ou C#


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


\[Algumas informações dizem respeito a produtos de pré-lançamento que poderão ser substancialmente modificados antes do lançamento comercial. A Microsoft não dá nenhuma garantia, expressa ou implícita, com relação às informações fornecidas aqui.\]

Esse procedimento passo a passo mostra como criar uma DLL de componente do Tempo de Execução do Windows básica que pode ser chamada em JavaScript, C# ou Visual Basic. Antes de começar este procedimento passo a passo, assegure-se de que você compreendeu conceitos como a Abstract Binary Interface (ABI), as classes ref e as extensões de componente do Visual C++ que facilitam o trabalho com classes ref. Para obter mais informações, consulte [Criação de componentes do Tempo de Execução do Windows em C++](creating-windows-runtime-components-in-cpp.md) e [Referência da linguagem Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/xaml/hh699871.aspx).

## Criação da DLL do componente C++


Neste exemplo, criamos o projeto de componente primeiro, mas você pode criar o projeto JavaScript primeiro. A ordem não importa.

A classe principal do componente contém exemplos de definições de propriedade e método, além de uma declaração de evento. Eles são fornecidos apenas para mostrar como isso é feito. Eles não são necessários e, neste exemplo, substituiremos todos os códigos gerados pelo próprio código.

## **Para criar o projeto de componente C++**

1.  Na barra de menus do Visual Studio, escolha **Arquivo, Novo, Projeto**.
2.  Na caixa de diálogo **Novo Projeto**, no painel à esquerda, expanda **Visual C++** e selecione o nó de aplicativos Universais do Windows.
3.  No painel central, selecione **Componente do Tempo de Execução do Windows** e nomeie o projeto WinRT\_CPP.
4.  Escolha o botão **OK**.

## **Para adicionar uma classe ativável ao componente**

1.  Uma classe ativável é aquela que código cliente pode criar usando uma expressão **new** (**New** em Visual Basic ou **ref new** em C++). Em seu componente, declare-a como **classe ref pública selado**. Na verdade, os arquivos Class1.h e .cpp já têm uma classe ref. É possível alterar o nome, mas neste exemplo usaremos o nome padrão – Class1. É possível definir classes ref adicionais ou classes regulares no componente caso elas sejam necessárias. Para obter mais informações sobre classes ref, consulte [Sistema de tipos (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh755822.aspx).

2.  Adicione estas diretivas \#include a Class1.h:

    ```cpp
             private void PrimesUnOrderedButton_Click_1(object sender, RoutedEventArgs e)
            {
                var nativeObject = new WinRT_CPP.Class1();

                StringBuilder sb = new StringBuilder();
                sb.Append("Primes found (unordered): ");
                PrimesUnOrderedResult.Text = sb.ToString();

                // primeFoundEvent is a user-defined event in nativeObject
                // It passes the results back to this thread as they are produced
                // and the event handler that we define here immediately displays them.
                nativeObject.primeFoundEvent += (n) =>
                {
                    sb.Append(n.ToString()).Append(" ");
                    PrimesUnOrderedResult.Text = sb.ToString();
                };

                // Call the async method.
                var asyncResult = nativeObject.GetPrimesUnordered(2, 100000);

                // Provide a handler for the Progress event that the asyncResult
                // object fires at regular intervals. This handler updates the progress bar.
                asyncResult.Progress += (asyncInfo, progress) =>
                    {
                        PrimesUnOrderedProgress.Value = progress;
                    };
            }

            private void Clear_Button_Click(object sender, RoutedEventArgs e)
            {
                PrimesOrderedProgress.Value = 0;
                PrimesUnOrderedProgress.Value = 0;
                PrimesUnOrderedResult.Text = "";
                PrimesOrderedResult.Text = "";
                Result1.Text = "";
            }
    ```

## Execução do aplicativo


Selecione o projeto C# ou JavaScript como projeto de inicialização abrindo o menu de atalho do nó do projeto no Gerenciador de Soluções e escolhendo **Definir como Projeto de Inicialização**. Em seguira, pressione F5 para executar com depuração ou Ctrl+F5 para executar sem depuração.

## Inspeção do componente no Pesquisador de Objetos (opcional)


No Pesquisador de Objetos, é possível inspecionar todos os tipos de Tempo de Execução do Windows definidos em arquivos .winmd. Isso inclui os tipos nos namespaces Platform e padrão. No entanto, como os tipos no namespace Platform::Collections são definidos no arquivo de cabeçalho collections.h, e não em um arquivo winmd, eles não são exibidos no Pesquisador de Objetos.

## **Para inspecionar um componente**

1.  Na barra de menus, escolha **Exibir, Pesquisador de Objetos** (Ctrl+Alt+J).
2.  No painel à esquerda do Pesquisador de Objetos, expanda o nó WinRT\_CPP para mostrar os tipos e os métodos definidos no componente.

## Dicas de depuração


Para obter uma experiência de depuração melhor, baixe os símbolos de depuração dos servidores de símbolos Microsoft públicos:

## **Para baixar símbolos de depuração**

1.  Na barra de menus, escolha **Ferramentas, Opções**.
2.  Na caixa de diálogo **Opções**, expanda **Depuração** e selecione **Símbolos**.
3.  Selecione **Servidores de Símbolos Microsoft** e escolha o botão **OK**.

Pode levar algum tempo para baixar os símbolos pela primeira vez. Para aumentar o desempenho na próxima vez em que você pressionar F5, especifique um diretório local no qual armazenar em cache os símbolos.

Ao depurar uma solução JavaScript com uma DLL de componente, você pode definir o depurador para habilitar a passagem pelo script ou a passagem pelo código nativo no componente, mas não ambos ao mesmo tempo. Para alterar a configuração, abra o menu de atalho do nó do projeto JavaScript no Gerenciador de Soluções e escolha **Propriedades, Depuração, Tipo de Depurador**.

Não se esqueça de selecionar os recursos apropriados no designer do pacote. Você pode abrir o designer de pacote abrindo o arquivo Package.appxmanifest. Por exemplo, caso você esteja tentando acessar arquivos programaticamente na pasta Imagens, não se esqueça de marcar a caixa de seleção **Biblioteca de Imagens** no painel **Recursos** do designer de pacotes.

Caso o código JavaScript não reconheça as propriedades públicas ou os métodos no componente, assegure-se de que, no JavaScript, você esteja seguindo o uso de maiúsculas camel. Por exemplo, o método `ComputeResult` C++ deve ser referenciado como `computeResult` em JavaScript.

Caso remova um projeto de componente do Tempo de Execução do Windows C++ de uma solução, você também deve remover manualmente a referência do projeto JavaScript. Deixar de fazer isso impede operações de depuração ou compilação subsequentes. Caso necessário, é possível adicionar uma referência de assembly à DLL.

## Tópicos relacionados

* [Criando componentes do Tempo de Execução do Windows em C++](creating-windows-runtime-components-in-cpp.md)



<!--HONumber=Mar16_HO1-->


