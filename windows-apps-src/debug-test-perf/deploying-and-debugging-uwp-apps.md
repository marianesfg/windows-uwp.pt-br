---
author: mcleblanc
ms.assetid: 9322B3A3-8F06-4329-AFCB-BE0C260C332C
description: Este artigo apresenta as etapas para abordar vários destinos de depuração e implantação.
title: Implantar e depurar aplicativos UWP (Plataforma Universal do Windows)
---

# Implantar e depurar aplicativos UWP (Plataforma Universal do Windows)

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este artigo apresenta as etapas para abordar vários destinos de depuração e implantação.

O Microsoft Visual Studio permite que você implante e depure seus aplicativos UWP (Plataforma Universal do Windows) em uma variedade de dispositivos Windows 10. O Visual Studio manipulará o processo de criação e registro do aplicativo no dispositivo de destino.

## Selecionando um destino de implantação

Para escolher um destino, navegue até a lista suspensa de destino de depuração próxima ao botão **Iniciar Depuração** e selecione um destino onde você deseja implantar seu aplicativo. Depois que o destino for selecionado, escolha **Iniciar Depuração (F5)** para implantar e depurar no alvo, ou pressione **Ctrl+F5** para implantar apenas nesse destino.

![](images/debug-device-target-list.png)

-   **Máquina Local** implantará o aplicativo no computador de desenvolvimento atual. Essa opção estará disponível apenas se a **Versão Mínima da Plataforma de Destino** de seu aplicativo for menor ou igual ao sistema operacional no computador de desenvolvimento.
-   **Simulador** implantará o aplicativo em um ambiente simulado em seu computador de desenvolvimento atual. Essa opção estará disponível apenas se a **Versão Mínima da Plataforma de Destino** de seu aplicativo for menor ou igual ao sistema operacional no computador de desenvolvimento.
-   O **Dispositivo** implantará o aplicativo em um dispositivo conectado USB. O dispositivo deve ser desbloqueado pelo desenvolvedor e ter a tela desbloqueada.
-   Um destino de **Emulador** será inicializado e implantará o aplicativo em um emulador com a configuração especificada no nome. Os emuladores estão disponíveis apenas em computadores Hyper-V habilitados que executam o Windows 8.1 ou posterior.
-   A **Máquina Remota** permitirá que você especifique um destino remoto para implantar o aplicativo. Mais informações sobre a implantação em um computador remoto podem ser encontradas em [Especificando um dispositivo remoto](#specifying-a-remote-device)

## Especificando um dispositivo remoto

### C# e Microsoft Visual Basic

Para especificar um computador remoto para aplicativos do C# ou Microsoft Visual Basic, selecione **Computador Remoto** na lista suspensa de destino de depuração. A caixa de diálogo **Conexões Remotas** aparecerá o que permitirá que você especifique um endereço IP ou selecione um dispositivo descoberto. Por padrão, o modo de autenticação **Universal** é selecionado. Para determinar qual modo de autenticação usar, consulte [Modos de autenticação](#authentication-modes)

![](images/debug-remote-connections.png)

Para retornar a essa caixa de diálogo, você pode abrir as propriedades do projeto e navegar até a guia **Depurar**. Então, selecione **Localizar...** próximo a **Máquina remota:**

![](images/debug-remote-machine-config.png)

Para implantar um aplicativo em um computador remoto, você também precisará baixar e instalar as Ferramentas Remotas do Visual Studio para no computador de destino. Consulte [Instruções para computador remoto](#remote-pc-instructions) a fim de obter instruções completas.

### C++ e JavaScript

Para especificar um destino de computador remoto para aplicativo UWP C++ ou JavaScript, vá até as propriedades do projeto. Clique com o botão direito do mouse no projeto no **Gerenciador de Soluções** e clique em **Propriedades**. Navegue até as configurações de **Depuração** e altere **Depurador a iniciar** para **Computador Remoto**. Em seguida, preencha o **Nome do Computador** (ou clique em **Localizar...** para encontrar um) e defina a propriedade **Tipo de Autenticação** .

![](images/debug-property-pages.png)
Depois que o computador for especificado, você pode selecionar **Computador Remoto** na lista suspensa de destino de depuração para retornar ao computador especificado. Somente um computador remoto pode ser selecionado por vez.

### Instruções para computador remoto

Para implantar em um computador remoto, o computador de destino deve ter as Ferramentas Remotas do Visual Studio instaladas. O computador remoto também deve estar executando uma versão do Windows que seja maior ou igual à propriedade **Versão Mínima da Plataforma de Destino** de seus aplicativos. Após instalar as ferramentas remotas, você deve iniciar o depurador remoto no computador de destino. Para fazer isso, pesquise **Depurador Remoto** no menu **Iniciar**, inicie-o e, se solicitado, permita que o depurador defina as configurações de firewall. Por padrão, o depurador é iniciado com a autenticação do Windows. Isso requer credenciais de usuário, caso o usuário conectado não seja o mesmo em ambos os computadores. Para alterá-lo para **Nenhuma autenticação**, acesse **Ferramentas** -&gt; **Opções** no **Depurador Remoto** e defina-o como **Sem Autenticação**. Depois que o depurador remoto for configurado, você poderá implantar de seu computador de desenvolvimento.

Para obter mais informações, consulte a página de download [Ferramentas Remotas para o Visual Studio]( http://go.microsoft.com/fwlink/?LinkId=717039).

## Modos de autenticação

Há três modos de autenticação para implantação de computadores remotos:

- **Universal (protocolo não criptografado)**: use esse modo de autenticação sempre que você estiver implantando em um dispositivo remoto que não seja um computador Windows (desktop ou notebook). Atualmente, apenas dispositivos IoT. Universal (protocolo não criptografado) só deve ser usado em redes confiáveis. A conexão de depuração é vulnerável a usuários mal-intencionados que podem interceptar e alterar dados passados entre o desenvolvimento e o computador remoto.
- **Windows**: esse modo de autenticação apenas se destina a ser usado para a implantação de computador remoto (desktop ou notebook). Use esse modo de autenticação quando você tiver acesso às credenciais do usuário conectado no computador de destino. Esse é o canal mais seguro para a implantação remota.
- **Nenhum**: esse modo de autenticação se destina apenas a ser usado para a implantação do computador remoto (desktop ou notebook). Use esse modo de autenticação quando você tiver uma configuração de computador de teste em um ambiente que tenha uma conta de teste conectada e você não possa inserir as credenciais. Verifique se que as configurações do depurador remoto estão definidas para aceitar Sem Autenticação.

## Opções de depuração

No Windows 10, o desempenho de inicialização de aplicativos UWP é melhorado iniciando-os de forma proativa e, em seguida, suspendendo os aplicativos em uma técnica chamada [pré-inicialização](https://msdn.microsoft.com/library/windows/apps/Mt593297). Muitos aplicativos não precisam fazer nada especial para trabalhar nesse modo, mas alguns aplicativos talvez precisem ajustar seu comportamento. Para ajudar a depurar problemas nesses caminhos de código, você pode iniciar o aplicativo do Visual Studio no modo de pré-inicialização. Há suporte para depuração de um projeto do Visual Studio (**Depurar** -&gt; **Outros Destinos de Depuração** -&gt; **Depurar Pré-inicialização de Aplicativo Universal do Windows**) e para aplicativos já instalados no computador (**Depurar** -&gt; **Outros Destinos de Depuração** -&gt; **Depurar Pacote do Aplicativo Instalado** e marque a caixa **Ativar Aplicativos com Pré-inicialização**). Para obter mais informações, leia sobre como [Depurar pré-inicialização de UWP]( http://go.microsoft.com/fwlink/?LinkId=717245)

Você pode definir as seguintes opções de implantação na página da propriedade de **depuração** do projeto de inicialização.

**Permitir loopback de rede**

Por motivos de segurança, um aplicativo UWP que foi instalado de modo padrão não pode fazer chamadas de rede para o dispositivo em que estiver instalado. Por padrão, a implantação do Visual Studio cria uma isenção da regra para o aplicativo implantado. Esse isolamento permite que você teste procedimentos de comunicação em uma única máquina. Antes de enviar seu aplicativo para a Windows Store, você deve testar seu aplicativo sem a isenção.

Para remover a isenção de loopback de rede do aplicativo:

-   Na página da propriedade **Debug** do C# e do Visual Basic, desmarque a caixa de seleção **Permitir Loopback de Rede**.
-   Na página da propriedade **Depuração** do JavaScript e do C++, defina o valor **Permitir loopback de rede** como **Não**

**Não iniciar, mas depurar meu código quando ele for iniciado (C# e Visual Basic) / Iniciar Aplicativo (JavaScript e C++)**

Para configurar a implantação para iniciar uma sessão de depuração quando o aplicativo for iniciado automaticamente:

-   Na página da propriedade **Debug** do C# e do Visual Basic, marque a caixa de seleção **Não iniciar, mas depurar meu código quando ele for iniciado**.
-   Na página da propriedade **Depuração** do JavaScript e do C++, defina o valor de **Iniciar Aplicativo** como **Sim**




<!--HONumber=May16_HO2-->


