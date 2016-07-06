---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: "Este artigo apresenta as etapas para abordar vários destinos de depuração e implantação."
title: Implantar e depurar aplicativos UWP (Plataforma Universal do Windows)
translationtype: Human Translation
ms.sourcegitcommit: 14f6684541716034735fbff7896348073fa55f85
ms.openlocfilehash: e2209e90080c7346bb363304b1a28f6446300332

---

# Implantar e depurar aplicativos UWP (Plataforma Universal do Windows)

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artigo apresenta as etapas para abordar vários destinos de depuração e implantação.

O Microsoft Visual Studio permite que você implante e depure seus aplicativos UWP (Plataforma Universal do Windows) em uma variedade de dispositivos Windows 10. O Visual Studio manipulará o processo de criação e registro do aplicativo no dispositivo de destino.

## Selecionando um destino de implantação

Para escolher um destino, navegue até a lista suspensa de destino de depuração próxima ao botão **Iniciar Depuração** e selecione um destino onde você deseja implantar seu aplicativo. Depois que o destino for selecionado, escolha **Iniciar Depuração (F5)** para implantar e depurar no alvo, ou pressione **Ctrl+F5** para implantar apenas nesse destino.

![](images/debug-device-target-list.png)

-   **Máquina Local** implantará o aplicativo no computador de desenvolvimento atual. Essa opção estará disponível apenas se a **Versão Mínima da Plataforma de Destino** de seu aplicativo for menor ou igual ao sistema operacional no computador de desenvolvimento.
-   **Simulador** implantará o aplicativo em um ambiente simulado em seu computador de desenvolvimento atual. Essa opção estará disponível apenas se a **Versão Mínima da Plataforma de Destino** de seu aplicativo for menor ou igual ao sistema operacional no computador de desenvolvimento.
-   O **Dispositivo** implantará o aplicativo em um dispositivo conectado USB. O dispositivo deve ser desbloqueado pelo desenvolvedor e ter a tela desbloqueada.
-   Um destino de **Emulador** será inicializado e implantará o aplicativo em um emulador com a configuração especificada no nome. Os emuladores estão disponíveis apenas em computadores Hyper-V habilitados que executam o Windows 8.1 ou posterior.
-   A **Máquina Remota** permitirá que você especifique um destino remoto para implantar o aplicativo. Mais informações sobre a implantação em um computador remoto podem ser encontradas em [Especificando um dispositivo remoto](#specifying-a-remote-device).

## Depurando aplicativos implantados
O Visual Studio também pode ser anexado a qualquer processo de aplicativo UWP em execução com a seleção de **Depurar** e, depois, **Anexar ao Processo**. Anexar a um processo em execução não exige o projeto do Visual Studio original, mas o carregamento dos [símbolos](#symbols) do processo ajudarão significativamente durante a depuração de um processo para o qual você não tem o código original.  
  
Além disso, qualquer pacote do aplicativo instalado pode ser anexado e depurado com a seleção de **Depurar**, **Outros** e, depois, **Depurar Pacotes do Aplicativo Instalados**.   
 
![Caixa de diálogo Depurar Pacotes do Aplicativo Instalados](images/gs-debug-uwp-apps-002.png)  

A seleção de **Não iniciar, mas sim depurar meu código quando iniciar** fará com que o depurador do Visual Studio seja anexado ao seu aplicativo UWP quando você o iniciar em um horário personalizado. Essa é uma forma eficiente de depurar caminhos de controle de [métodos de inicialização diferentes](../xbox-apps/automate-launching-uwp-apps.md), como a ativação de protocolo com parâmetros personalizados.  

Os aplicativos UWP podem ser desenvolvidos e compilados no Windows 8.1 ou posterior, mas exigem que o Windows 10 seja executado. Se você estiver desenvolvendo um aplicativo UWP em um computador Windows 8.1, poderá depurar remotamente um aplicativo UWP em execução em outro dispositivo Windows 10, desde que os computadores host e de destino estejam na mesma LAN. Para fazer isso, baixe e instale as [Ferramentas Remotas para Visual Studio](http://aka.ms/remotedebugger) em ambos os computadores. A versão instalada deve corresponder à versão existente do Visual Studio que você instalou, e a arquitetura que você selecionar (x86, x64) também deverá corresponder à do aplicativo de destino.   
  

## Especificando um dispositivo remoto

### C# e Microsoft Visual Basic

Para especificar um computador remoto para aplicativos do C# ou Microsoft Visual Basic, selecione **Computador Remoto** na lista suspensa de destino de depuração. A caixa de diálogo **Conexões Remotas** aparecerá o que permitirá que você especifique um endereço IP ou selecione um dispositivo descoberto. Por padrão, o modo de autenticação **Universal** é selecionado. Para determinar qual modo de autenticação usar, consulte [Modos de autenticação](#authentication-modes).

![](images/debug-remote-connections.png)

Para retornar a essa caixa de diálogo, você pode abrir as propriedades do projeto e navegar até a guia **Depurar**. Então, selecione **Localizar...** próximo a **Máquina remota:**

![](images/debug-remote-machine-config.png)

Para implantar um aplicativo em um computador remoto, você também precisará baixar e instalar as Ferramentas Remotas do Visual Studio para no computador de destino. Consulte [Instruções para computador remoto](#remote-pc-instructions) a fim de obter instruções completas.

### C++ e JavaScript

Para especificar um destino de computador remoto para aplicativo UWP C++ ou JavaScript, vá até as propriedades do projeto. Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e clique em **Propriedades**. Navegue até as configurações de **Depuração** e altere **Depurador a iniciar** para **Computador Remoto**. Em seguida, preencha o **Nome do Computador** (ou clique em **Localizar...** para encontrar um) e defina a propriedade **Tipo de Autenticação** .

![](images/debug-property-pages.png)
Depois que o computador for especificado, você pode selecionar **Computador Remoto** na lista suspensa de destino de depuração para retornar ao computador especificado. Somente um computador remoto pode ser selecionado por vez.

### Instruções para computador remoto

Para implantar em um computador remoto, o computador de destino deve ter as Ferramentas Remotas do Visual Studio instaladas. O computador remoto também deve estar executando uma versão do Windows que seja maior ou igual à propriedade **Versão Mínima da Plataforma de Destino** de seus aplicativos. Após instalar as ferramentas remotas, você deve iniciar o depurador remoto no computador de destino. Para fazer isso, pesquise **Depurador Remoto** no menu **Iniciar**, inicie-o e, se solicitado, permita que o depurador defina as configurações de firewall. Por padrão, o depurador é iniciado com a autenticação do Windows. Isso requer credenciais de usuário, caso o usuário conectado não seja o mesmo em ambos os computadores. Para alterá-lo para **Nenhuma autenticação**, acesse **Ferramentas** -&gt;**Opções** no **Depurador Remoto** e defina-o como **Sem Autenticação**. Depois que o depurador remoto for configurado, você poderá implantar de seu computador de desenvolvimento.

Para obter mais informações, consulte a página de download [Ferramentas Remotas para o Visual Studio]( http://go.microsoft.com/fwlink/?LinkId=717039).

## Modos de autenticação

Há três modos de autenticação para implantação de computadores remotos:

- **Universal (protocolo não criptografado)**: use esse modo de autenticação sempre que você estiver implantando em um dispositivo remoto que não seja um computador Windows (desktop ou notebook). Atualmente, apenas dispositivos IoT. Universal (protocolo não criptografado) só deve ser usado em redes confiáveis. A conexão de depuração é vulnerável a usuários mal-intencionados que podem interceptar e alterar dados passados entre o desenvolvimento e o computador remoto.
- **Windows**: esse modo de autenticação apenas se destina a ser usado para a implantação de computador remoto (desktop ou notebook). Use esse modo de autenticação quando você tiver acesso às credenciais do usuário conectado no computador de destino. Esse é o canal mais seguro para a implantação remota.
- **Nenhum**: esse modo de autenticação se destina apenas a ser usado para a implantação do computador remoto (desktop ou notebook). Use esse modo de autenticação quando você tiver uma configuração de computador de teste em um ambiente que tenha uma conta de teste conectada e você não possa inserir as credenciais. Verifique se que as configurações do depurador remoto estão definidas para aceitar Sem Autenticação.

## Opções de depuração

No Windows 10, o desempenho de inicialização de aplicativos UWP é melhorado iniciando-os de forma proativa e, em seguida, suspendendo os aplicativos em uma técnica chamada [pré-inicialização](https://msdn.microsoft.com/library/windows/apps/Mt593297). Muitos aplicativos não precisam fazer nada especial para trabalhar nesse modo, mas alguns aplicativos talvez precisem ajustar seu comportamento. Para ajudar a depurar problemas nesses caminhos de código, você pode iniciar o aplicativo do Visual Studio no modo de pré-inicialização. Há suporte para depuração de um projeto do Visual Studio (**Depurar** -&gt;**Outros Destinos de Depuração** -&gt;**Depurar Pré-inicialização de Aplicativo Universal do Windows**) e para aplicativos já instalados no computador (**Depurar** -&gt;**Outros Destinos de Depuração** -&gt;**Depurar Pacote do Aplicativo Instalado** e marque a caixa **Ativar Aplicativos com Pré-inicialização**). Para obter mais informações, leia sobre como [Depurar pré-inicialização de UWP]( http://go.microsoft.com/fwlink/?LinkId=717245).

Você pode definir as seguintes opções de implantação na página da propriedade de **depuração** do projeto de inicialização.

**Permitir loopback de rede**

Por motivos de segurança, um aplicativo UWP que foi instalado de modo padrão não pode fazer chamadas de rede para o dispositivo em que estiver instalado. Por padrão, a implantação do Visual Studio cria uma isenção da regra para o aplicativo implantado. Esse isolamento permite que você teste procedimentos de comunicação em uma única máquina. Antes de enviar seu aplicativo para a Windows Store, você deve testar seu aplicativo sem a isenção.

Para remover a isenção de loopback de rede do aplicativo:

-   Na página da propriedade **Debug** do C# e do Visual Basic, desmarque a caixa de seleção **Permitir Loopback de Rede**.
-   Na página da propriedade **Depuração** do JavaScript e do C++, defina o valor **Permitir loopback de rede** como **Não**.

**Não iniciar, mas depurar meu código quando ele for iniciado (C# e Visual Basic) / Iniciar Aplicativo (JavaScript e C++)**

Para configurar a implantação para iniciar uma sessão de depuração quando o aplicativo for iniciado automaticamente:

-   Na página da propriedade **Debug** do C# e do Visual Basic, marque a caixa de seleção **Não iniciar, mas depurar meu código quando ele for iniciado**.
-   Na página de propriedades **Depuração** do JavaScript e do C++, defina o valor de **Iniciar Aplicativo** como **Sim**.

## Símbolos

Os arquivos de símbolos contêm uma variedade de dados muito úteis na depuração de código, como variáveis, nomes de funções e endereços de ponto de entrada, permitindo que você compreenda melhor as exceções e a ordem de execução da pilha de chamadas. Os símbolos da maioria das variantes do Windows estão disponíveis no [servidor de símbolos da Microsoft](http://msdl.microsoft.com/download/symbols), ou podem ser baixados para que as pesquisas offline fiquem mais rápidas em [Baixar pacotes de símbolos do Windows](http://aka.ms/winsymbols).

Para definir opções de símbolos para o Visual Studio, selecione **Ferramentas > Opções** e navegue até **Depuração > Símbolos** na janela da caixa de diálogo.

**Figura 4. Caixa de diálogo Opções. **
            
![Caixa de diálogo Opções](images/gs-debug-uwp-apps-004.png)

Para carregar símbolos em uma sessão de depuração com [WinDbg](#windbg), defina a variável **sympath** para o local do pacote de símbolos. Por exemplo, executar o comando a seguir carregará símbolos do servidor de símbolos da Microsoft e, em seguida, os armazenará em cache no diretório C:\Symbols:

```
.sympath SRV*C:\Symbols*http://msdl.microsoft.com/download/symbols
.reload
```

Você pode adicionar mais caminhos usando o delimitador ';' ou usar o comando `.sympath+`. Para operações de símbolos mais avançadas que usam WinDbg, consulte [Símbolos públicos e privados](https://msdn.microsoft.com/library/windows/hardware/ff553493).

## WinDbg

WinDbg é um depurador avançado fornecido como parte do conjunto de Ferramentas de Depuração para Windows, que está incluído no [SDK do Windows](http://go.microsoft.com/fwlink/p?LinkID=271979). A instalação do SDK do Windows permite que você instale as Ferramentas de Depuração para Windows como um produto autônomo. Embora seja altamente útil para a depuração de código nativo, não recomendamos o WinDbg para aplicativos escritos em código gerenciado ou HTML5. 

Para usar o WinDbg com aplicativos UWP, você precisará primeiro desabilitar o PLM para seu pacote do aplicativos usando o PLMDebug, conforme descrito na seção anterior. 

```
plmdebug /enableDebug [PackageFullName] "\"C:\Program Files\Debugging Tools for Windows (x64)\WinDbg.exe\" -server npipe:pipe=test"
```

Em contraste com o Visual Studio, a maior parte da funcionalidade central do WinDbg depende de fornecer comandos à janela de comando. Os comandos fornecidos permitem que você veja o estado de execução, investigue despejos de memória do modo de usuário e depure em diversos modos. 

Um dos comandos mais populares no WinDbg é `!analyze -v`, que é usado para recuperar uma quantidade detalhada de informações sobre a exceção atual, incluindo:

- FAULTING_IP: ponteiro de instrução no momento da falha
- EXCEPTION_RECORD: endereço, código e sinalizadores da exceção atual
- STACK_TEXT: rastreamento de pilha antes da exceção

Para obter uma lista completa de todos os comandos WinDbg, consulte [Comandos do depurador](https://msdn.microsoft.com/library/ff540507).

## Tópicos relacionados
- [Testando e depurando ferramentas para PLM (Gerenciamento do Tempo de Vida do Processo)](testing-debugging-plm.md)
- [Depuração, teste e desempenho](index.md)



<!--HONumber=Jun16_HO4-->


