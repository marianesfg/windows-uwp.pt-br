---
author: mcleanbyron
ms.assetid: 69E05E56-B5F0-4D4C-A1FF-B6EAFF5D0E28
description: Durante o processo de envio, você pode configurar o comportamento de mediação de anúncio que gostaria de ver. Você poderá ajustá-lo posteriormente sem precisar fazer alterações no código ou enviar novos pacotes.
title: Enviar seu aplicativo e configurar o controle de anúncios
---

# Enviar seu aplicativo e configurar o controle de anúncios


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Depois que você tiver criado seu aplicativo para incluir todas as redes de publicidade que deseja usar e o tiver testado para garantir que tudo esteja funcionando, você estará pronto para enviar o aplicativo. Durante o processo de envio, você pode configurar o comportamento de mediação de anúncio que gostaria de ver. Você poderá ajustá-lo posteriormente sem precisar fazer alterações no código ou enviar novos pacotes.

## Criar uma configuração de linha de base para um pacote de aplicativo


Quando você carrega os pacotes, o Centro de Desenvolvimento detecta automaticamente que você está usando o controle de anúncios e identifica quais redes de publicidade estão sendo usadas. Na página **Pacotes**, você verá um link **Configurar controle de anúncios**. Clique nesse link para acessar a página [Monetizar com anúncios](https://msdn.microsoft.com/library/windows/apps/mt170658), onde fará a configuração de sua lógica de controle na seção **Controle de anúncios do Windows**. Para obter mais informações sobre as outras seções nessa página, consulte Monetizar com anúncios.

Quando você definir as configurações do controle de anúncios do seu aplicativo pela primeira vez, criará uma configuração de linha de base. Depois que tiver configurado, você poderá adicionar configurações específicas do mercado para tirar vantagem dos pontos fortes específicos das redes de publicidade em diferentes mercados. Se você optar por não definir as configurações do controle de anúncios antes de enviar o aplicativo, o Centro de Desenvolvimento aplicará automaticamente as [configurações padrão](#default-ad-mediation-settings), e você poderá [modificá-las posteriormente](#optimize-ad-mediation-after-the-app-has-been-published).

As etapas a seguir descrevem como criar uma configuração de linha de base na seção **Controle de anúncios do Windows** da página **Monetizar com anúncios**.

1.  Em **Configurar controle para**, certifique-se de que o pacote do aplicativo que você deseja configurar esteja selecionado.
2.  Em **Destino**, certifique-se de a opção **Linha de base** esteja selecionada.
3.  Em **Taxa de atualização**, escolha o comprimento do ciclo de controle (a frequência com que os novos anúncios deverão ser mostrados). A duração deve ser entre 30 e 120 segundos.
  > **Observação**  Se você já tiver configurado uma taxa de atualização em qualquer um dos seus portais de rede de publicidade, certifique-se de definir a mesma taxa de atualização aqui.

4.  A seguir, a seção **Controle de anúncios do Windows** lista todas as redes de publicidade usadas pelo seu aplicativo e fornece duas maneiras diferentes de especificar a frequência com que seu aplicativo deve usar cada rede. Escolha uma destas opções na lista suspensa **Mediation type**:

    -   **Order by weight**. Escolha esta opção para aplicar valores percentuais a cada rede de publicidade que especificam a frequência com que elas devem ser usadas pelo seu aplicativo. As porcentagens totais definidas para todas as redes de publicidade devem somar exatamente 100%. Para saber mais, consulte [Ordenar redes de publicidade por peso](#order-ad-networks-by-weight).
    -   **Order by rank**. Escolha esta opção para aplicar um número de classificação a cada rede de publicidade, de 1 a  *n*, que especifica a frequência com que seu aplicativo deve usar cada rede. Para saber mais, consulte [Ordenar redes de publicidade por classificação](#order-ad-networkds-by-rank).

    O Centro de Desenvolvimento aplica automaticamente [configurações padrão](#default-ad-mediation-settings) que especificam a frequência com que seu aplicativo deve usar cada rede de publicidade.

5.  Na lista de redes de publicidade usadas pelo seu aplicativo, clique na seta suspensa de cada provedor de rede de publicidade para ver os parâmetros necessários para cada rede e insira os parâmetros necessários. Para obter uma lista dos parâmetros, consulte [Selecionar a gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md).

    Na lista expandida de parâmetros de cada rede, opcionalmente, você pode usar o campo **Tempo limite** para especificar o número de segundos (de 2 a 120) que o controle de anúncios deverá aguardar depois de solicitar um anúncio dessa rede de publicidade antes de abandonar essa solicitação e fazer uma solicitação a outra rede. Se você já [especifica esse valor em seu código](add-and-use-the-ad-mediator-control.md#set-timeouts), o valor especificado no código substituirá o valor especificado aqui.

    Se você estiver usando a Microsoft Advertising, anote o seguinte:

    -   Os parâmetros para o **Microsoft Advertising** são preenchidos para você com base no conteúdo do pacote do aplicativo. Você também pode especificar seus próprios valores de **ID do aplicativo** e **ID da unidade de anúncio** para o **Microsoft Advertising**.
    -   Além do **Microsoft Advertising**, também há uma linha para **Microsoft Advertising house ads**. Anúncios domésticos fornecem uma maneira de criar anúncios gratuitos que promovem um de seus aplicativos em outros aplicativos. Embora os anúncios domésticos sejam gratuitos, sua alocação de anúncios domésticos vem do mesmo pool que outras redes de publicidade. Portanto, se você alocar qualquer solicitação de anúncio para **Microsoft Advertising house ads**, estará optando por usar essa porcentagem do seu total de fornecimento de anúncios para anúncios domésticos. Se você alocar solicitações de anúncios para anúncios domésticos, também deverá criar uma campanha publicitária doméstica para pelo menos um outro aplicativo registrado em sua conta de desenvolvedor. Para saber mais, consulte [Sobre anúncios domésticos](https://msdn.microsoft.com/library/windows/apps/mt148566).

6.  Clique em **Salvar** para salvar a configuração de linha de base.

### Ordenar redes de publicidade por peso

Escolha essa opção na lista suspensa **Mediation type** para especificar valores percentuais que especificam a frequência com que seu aplicativo deve usar cada rede de publicidade. As porcentagens totais definidas para todas as redes de publicidade devem somar exatamente 100%.

Para distribuir automaticamente as solicitações por igual em todas as suas redes, clique em **Distribua solicitações de anúncio igualmente entre todas as suas redes de publicidade ativas**. Para disseminar as solicitações de forma desigual, use os controles deslizantes para indicar a frequência com que seu aplicativo deverá usar cada rede de publicidade. As porcentagens totais definidas para todas as redes de publicidade devem somar exatamente 100%. Ao arrastar os controles deslizantes, você tem várias opções:

-   Se você arrastar o controle deslizante para um número, o número indicará a porcentagem de tempo em que essa rede de publicidade será chamada como a primeira opção do aplicativo em um ciclo de controle.
-   Se você arrastar o controle deslizante para **Backup**, isso indicará que a rede de publicidade deverá ser chamada somente se nenhuma das redes de publicidade com uma porcentagem designada possa fornecer um anúncio. Isso é equivalente a definir a porcentagem como 0%.
-   Se você arrastar o controle deslizante para **Inativo**, isso indicará que essa rede de publicidade nunca será chamada. Os assemblies da rede de publicidade permanecerão no pacote, mas o mediador não tentará chamá-los. Você pode definir essa opção nas configurações específicas do mercado para excluir redes de publicidade que são conhecidas por ter desempenho insatisfatório ou, não dar suporte, em um determinado mercado.
-   Quando você ajustar a porcentagem de uma rede de publicidade, qualquer outro controle deslizante em que você tenha selecionado um valor diferente de **Backup** será ajustado automaticamente para que a distribuição total some 100%. Se você marcar a caixa de seleção **Bloquear** para uma rede de publicidade, essa rede permanecerá fixa no valor selecionado no momento. Em seguida, você poderá ajustar os valores de outras redes de publicidade sem afetar o valor da rede de publicidade bloqueada

### Ordenar redes de publicidade por classificação

Escolha essa opção na lista suspensa **Mediation type** para aplicar um número de classificação a cada rede de publicidade, de 1 a *n*, que especifica a frequência com que seu aplicativo deve usar cada rede.

Se você desmarcar a caixa de seleção **Ativo** para uma rede de publicidade, isso indica que essa rede nunca será chamada. Os assemblies da rede de publicidade permanecerão no pacote, mas o controle não tentará invocá-los. Você pode definir essa opção nas configurações específicas ao mercado para excluir redes de publicidade que são conhecidas por ter desempenho insatisfatório (ou não ter suporte) em um determinado mercado.

### Configurações de controle de anúncios padrão

Por padrão, o Centro de Desenvolvimento aplica as configurações de controle de anúncios a seguir ao seu aplicativo na página **Monetizar com anúncios**. Esses mesmos valores padrão também são aplicados se você optar por não configurar o controle de anúncios antes de enviar seu aplicativo.

-   Se seu aplicativo usar a Microsoft Advertising, a **Microsoft Advertising** será definida como 100 (se você escolher **Order by weight**) ou 1 (se você escolher **Order by rank**), e todas as outras redes de publicidade serão definidas como inativas.
-   Se seu aplicativo não usar a Microsoft Advertising, todas as redes de publicidade serão definidas como inativas.

## Criar uma nova configuração de destino


Depois de criar uma configuração de linha de base para seu aplicativo, você poderá criar configurações adicionais para usar em mercados específicos. Essas novas configurações herdam as configurações iniciais da configuração de linha de base, mas você poderá ajustar detalhes para os mercados específicos com suporte para o seu aplicativo.

Para criar uma nova configuração de destino:

1.  Em **Destino**, selecione o mercado para o qual você deseja criar a nova configuração.
2.  Atualize a taxa de atualização e as informações de distribuição de solicitação de anúncio conforme desejado.
3.  Clique em **Salvar** para salvar a nova configuração.

## Otimizar o controle de anúncios depois que o aplicativo foi publicado


Se você deseja ajustar o controle de anúncios de um aplicativo específico, será possível fazer isso a qualquer momento sem precisar reenviar o aplicativo. Isso é útil se você já tiver adicionado redes de publicidade em seu aplicativo para as quais não havia configurado contas anteriormente, ou se você estiver detectando que uma rede de publicidade não é capaz de preencher anúncios confiavelmente em mercados específicos. Você pode fazer alterações na configuração de linha de base e nas configurações específicas do mercado. Você também pode adicionar ou remover configurações específicas do mercado, se desejar.

Para retornar às configurações de controle do seu aplicativo, siga um destes procedimentos:

-   Na página de visão geral de conta no painel, clique em seu aplicativo na seção **Controle de anúncios**.
-   Na página de visão geral do seu aplicativo no painel, expanda **Monetization** e clique em **Monetizar com anúncios**.

## Configurações de controle de anúncios e atualizações de aplicativos


Quando você envia uma atualização de aplicativo, as informações de configuração do controle de anúncios existentes para o seu aplicativo serão aplicadas automaticamente ao pacote atualizado se as seguintes condições forem atendidas:

-   O número de controles **AdMediatorControl** é igual ao número de controles no pacote do aplicativo anterior, e todos os controles têm as mesmas IDs que o pacote do aplicativo anterior.
-   A lista de provedores de rede de publicidade é a mesma que do pacote do aplicativo anterior.

Se alguma dessas condições não for atendida, você deverá recriar as configurações de linha de base e quaisquer configurações de destino específicas do mercado aplicáveis para o seu aplicativo.

> **Observação**  O ID de um **AdMediatorControl** é gerado quando você arrasta o controle para uma superfície de design em seu aplicativo. Essa ID não será alterada, a menos que você exclua o controle e a substitua arrastando um novo controle para a mesma superfície de design.

 

## Analisar relatórios de controle de anúncios no painel unificado do Centro de Desenvolvimento


Para analisar os relatórios de controle de anúncios, vá para a seção **Análise** no Centro de Desenvolvimento e clique em **Controle de anúncios**. Para obter mais informações sobre esse relatório, consulte [Relatório de controle de anúncios](https://msdn.microsoft.com/library/windows/apps/mt148521).

## Cenários de exemplo


Você pode configurar suas redes de publicidade de uma variedade de maneiras para dar suporte a cenários e metas diferentes. Os exemplos a seguir mostram como você pode configurar seu controle de anúncios usando **Order by weight** quando tiver incluído três redes de publicidade. Usaremos Microsoft Advertising, Inneractive e AdDuplex como nossas redes de exemplo.

### Exemplo 1

Se você quiser descobrir a taxa de preenchimento e o custo por mil visualizações (eCPM) de todas as redes, antes de começar a otimizar.

Para começar, defina cada rede para usar uma distribuição igual de tráfego de mediação. Você pode revisar os relatórios da rede de cada anúncio posteriormente para ajudar a determinar as redes de anúncios de maior desempenho em cada mercado.

Depois de alguns dias ou semanas, você vai querer verificar a taxa de preenchimento e o eCPM em cada um dos portais de rede de publicidade. Isso ajudará você a determinar as melhores redes de anúncios em cada mercado. Em seguida, você pode fazer ajustes em mercados específicos (ou gerais) sem precisar enviar novos pacotes.

### Exemplo 2

Você deseja usar a Microsoft Advertising pela primeira vez, sempre que possível. Se a Microsoft Advertising não puder fornecer um anúncio, você poderá usar qualquer uma das suas outras redes de publicidade como backup, sem nenhuma preferência por uma ou outra.

| Rede de publicidade            | Configuração |
|-----------------------|---------------|
| Microsoft             | 100%          |
| Inneractive           | Backup        |
| AdDuplex              | Backup        |

### Exemplo 3

Você geralmente deseja usar a Microsoft Advertising primeiro. Se a Microsoft Advertising não puder fornecer um anúncio, você vai querer se certificar de que a Inneractive seja chamada antes da AdDuplex.

| Rede de publicidade            | Configuração |
|-----------------------|---------------|
| Microsoft             | 90%           |
| Inneractive           | 10%           |
| AdDuplex              | Backup        |

### Exemplo 4

Você deseja usar a Microsoft Advertising e a Inneractive igualmente, para que haja uma variedade maior de anúncios em seu aplicativo do que haveria com apenas uma rede sempre sendo chamada primeiro. Se nenhuma rede de publicidade estiver disponível, você usará AdDuplex como backup.

| Rede de publicidade            | Configuração |
|-----------------------|---------------|
| Microsoft             | 50%           |
| Inneractive           | 50%           |
| AdDuplex              | Backup        |


## Tópicos relacionados

* [Selecionar e gerenciar suas redes de publicidade](select-and-manage-your-ad-networks.md)
* [Adicionar e usar o controle de anúncios](add-and-use-the-ad-mediator-control.md)
* [Testar sua implementação de controle de anúncios](test-your-ad-mediation-implementation.md)
* [Solucionar problemas de controle de anúncios](troubleshoot-ad-mediation.md)
 

 


<!--HONumber=May16_HO2-->


