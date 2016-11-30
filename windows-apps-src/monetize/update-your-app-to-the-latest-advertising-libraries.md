---
author: mcleanbyron
description: "Saiba como atualizar o aplicativo para usar as bibliotecas do Microsoft Advertising compatíveis mais recentes e verifique se o aplicativo continua recebendo anúncios em faixa."
title: Atualize o aplicativo para as bibliotecas do Microsoft Advertising mais recentes
translationtype: Human Translation
ms.sourcegitcommit: 9bd83a41ea4ec4ec7a75ef89e9c92f73d86cc731
ms.openlocfilehash: 710a5f4a3ae566550939fe783af7e97d20dd1b28


---

# Atualize o aplicativo para as bibliotecas do Microsoft Advertising mais recentes

A partir de janeiro de 2017, não veicularemos mais anúncios em faixa para aplicativos que usem versões do SDK de publicidade mais antigas da Microsoft. Se você tiver um aplicativo existente (já na Loja ou ainda em desenvolvimento) que exiba anúncios em faixa usando **AdControl** ou **AdMediatorControl**, será preciso atualizar o aplicativo para usar o SDK de publicidade mais recente para o aplicativo continuar recebendo anúncios em faixa em janeiro de 2017. Siga as instruções neste artigo para determinar se o aplicativo é afetado por essa mudança e saiba como atualizá-lo, se necessário.

Se o aplicativo for afetado por essa mudança e você não atualizar o aplicativo para usar o SDK de publicidade mais recente, você notará o seguinte comportamento a partir de janeiro de 2017:

* Os anúncios em faixa não serão mais veiculados para controles **AdControl** ou **AdMediatorControl** no aplicativo, e você deixará de receber receita de publicidade desses controles.

* Quando **AdControl** ou **AdMediatorControl** no aplicativo solicitar um novo anúncio, o evento **ErrorOccurred** do controle será acionado e a propriedade **ErrorCode** dos argumentos de evento terá o valor **NoAdAvailable**.

Para contextualizar mais essa alteração, estamos removendo o suporte para versões do SDK de publicidade anteriores não compatíveis com um conjunto mínimo de recursos, inclusive a capacidade de oferecer mídia avançada HTML5 por meio da [Especificação Mobile Rich-media Ad Interface Definitions (MRAID) 1.0](http://www.iab.com/wp-content/uploads/2015/08/IAB_MRAID_VersionOne.pdf) da Interactive Advertising Bureau (IAB). Muitos dos anunciantes procuram esses recursos, e estamos fazendo essa alteração para ajudar a tornar o ecossistema de aplicativos mais atraente para anunciantes e, por fim, gerar mais receita para você.

Se você tiver algum problema ou precisar de ajuda, [contate o suporte](http://go.microsoft.com/fwlink/?LinkId=393643).

>**Observação**&nbsp;&nbsp;Se você tiver atualizado anteriormente o aplicativo para usar o [Microsoft Store Services SDK](http://aka.ms/store-services-sdk) (para aplicativos UWP) ou o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk) (para aplicativos do Windows 8.1 e do Windows Phone 8.x), o aplicativo já usará o SDK de publicidade mais recente disponível, e você não precisará fazer outras alterações no aplicativo.

## Pré-requisitos

* O código-fonte completo e os arquivos de projeto do Visual Studio do aplicativo que usam **AdControl** ou **AdMediatorControl**.

* O pacote .appx ou .xap do aplicativo.

  >**Observação**&nbsp;&nbsp;Se não tiver mais o pacote .appx ou .xap do aplicativo, mas ainda tiver um computador de desenvolvimento com a versão do Visual Studio e o SDK de publicidade que foi usado para criar o aplicativo, você poderá regenerar o pacote .appx ou .xap no Visual Studio.

<span id="part-1" />
## Parte 1: Determine se você precisa atualizar o aplicativo

Siga as instruções nas seções a seguir para determinar se você precisa atualizar o aplicativo.

### O aplicativo usa AdControl

Se o aplicativo usar **AdControl** para exibir anúncios em faixa, siga estas instruções.

**Aplicativos UWP para Windows 10**

1. Crie uma cópia do pacote .appx do aplicativo de maneira que você não afete o original, renomeie a cópia de maneira que ela tenha uma extensão .zip e extraia o conteúdo do arquivo.

2. Verifique o conteúdo extraído do pacote do aplicativo:

  * Se vir um arquivo Microsoft.Advertising.dll, o aplicativo usará um SDK antigo e você deverá atualizar o projeto seguindo as instruções nas seções abaixo. Vá até [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

  * Se não vir um arquivo Microsoft.Advertising.dll, o aplicativo UWP já usará o SDK de publicidade disponível mais recente e você não precisará fazer alterações no projeto.

<span/>

**Aplicativos do Windows 8.1 ou Windows Phone 8.x**

1. Crie uma cópia do pacote .appx ou .xap do aplicativo de maneira que você não afete o original, renomeie a cópia de maneira que ela tenha uma extensão .zip e extraia o conteúdo do arquivo.

2. Para aplicativos XAML ou JavaScript/HTML, verifique o conteúdo extraído do pacote do aplicativo:

  * Se vir um arquivo Microsoft.Advertising.winmd, mas nenhum arquivo UniversalXamlAdControl.\*.dll (para aplicativos XAML) ou um arquivo UniversalSharedLibrary.Windows.dll (para aplicativos JavaScript/HTML), o aplicativo usará um SDK anterior e você deverá atualizar o projeto seguindo as instruções nas seções abaixo. Vá até [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

  * Do contrário, prossiga à etapa a seguir.

2. Abra o Windows PowerShell, digite o comando a seguir e atribua o argumento ```-Path``` ao caminho completo do conteúdo extraído do pacote do aplicativo. Esse comando exibe todas as bibliotecas de publicidade referenciadas pelo projeto e pela versão de cada biblioteca.

    ```
    get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
    ```
2. Localize o arquivo listado na tabela a seguir para a plataforma de destino do aplicativo e compare as versões desse arquivo com a versão listada na tabela.

  <table>
    <colgroup>
      <col width="33%" />
      <col width="33%" />
      <col width="33%" />
    </colgroup>
    <thead>
      <tr class="header">
        <th align="left">Plataforma de destino</th>
        <th align="left">Arquivos</th>
        <th align="left">Versão</th>
      </tr>
    </thead>
    <tbody>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 XAML</p></td>
        <td align="left"><p>UniversalXamlAdControl.WindowsPhone.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows 8.1 JavaScript/HTML<br/>Windows Phone 8.1 JavaScript/HTML</p></td>
        <td align="left"><p>UniversalSharedLibrary.Windows.dll</p></td>
        <td align="left"><p>8.5.1601.07018</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.1 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>8.1.50112.0</p></td>
      </tr>
      <tr class="odd">
        <td align="left"><p>Windows Phone 8.0 Silverlight</p></td>
        <td align="left"><p>Microsoft.Advertising.\*.dll</p></td>
        <td align="left"><p>6.2.40501.0</p></td>
      </tr>
    </tbody>
  </table>

3. Se o arquivo tiver uma versão igual ou posterior à versão listada na tabela anterior, você não precisará fazer alterações no projeto.

  Se o arquivo tiver um número de versão menor, você deverá atualizar o projeto seguindo as instruções nas seções a seguir. Vá até [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span/>

**Aplicativos do Windows 8.0**

* Os aplicativos que segmentam o Windows 8.0 deixarão de receber anúncios em faixa a partir de janeiro de 2017. Para evitar impressões perdidas, recomendamos converter o projeto em um aplicativo UWP que segmente o Windows 10. A maioria do tráfego de aplicativo do Windows 8.0 agora está em execução em dispositivos com o Windows 10.

<span/>

**Aplicativos do Windows Phone 7.x**

* Os aplicativos que segmentam o Windows Phone 7.x não receberão mais anúncios em faixa a partir de janeiro de 2017. Para evitar impressões perdidas, recomendamos converter o projeto para segmentar o aplicativo do Windows Phone 8.1 ou um aplicativo UWP que segmente o Windows 10. A maior parte do tráfego do aplicativo do Windows 7.x agora está em execução em dispositivos com o Windows Phone 8.1 ou o Windows 10.

<span/>

### O aplicativo usa AdMediatorControl

Se o aplicativo usar **AdMediatorControl** para exibir anúncios em faixa, siga essas instruções para determinar se você precisa atualizar o aplicativo.

**Aplicativos UWP para Windows 10**

* **AdMediatorControl** não é mais compatível com aplicativos UWP. Você deve migrar usando **AdControl** seguindo as instruções nas seções abaixo. Vá até [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span/>

**Aplicativos do Windows 8.1 ou Windows Phone 8.1**

1. Crie uma cópia do pacote .appx ou .xap do aplicativo de maneira que você não afete o original, renomeie a cópia de maneira que ela tenha uma extensão .zip e extraia o conteúdo do arquivo.

2. Abra o Windows PowerShell, digite o comando a seguir e atribua o argumento ```-Path``` ao caminho completo do conteúdo extraído do pacote do aplicativo. Esse comando exibe todas as bibliotecas de publicidade referenciadas pelo projeto e pela versão de cada biblioteca.

    ```
    get-childitem -Path "<path to your extracted package>" * -Recurse -include *advert*.dll,*admediator*.dll,*xamladcontrol*.dll,*universalsharedlibrary*.dll | where-object {$_.Name -notlike "*resources*" -and $_.Name -notlike "*design*" } | foreach-object { "{0}`t{1}" -f $_.FullName, [System.Diagnostics.FileVersionInfo]::GetVersionInfo($_).FileVersion }
    ```

2. Se a versão dos arquivos Microsoft.AdMediator.\*.dll listada na saída for 2.0.1603.18005 ou posterior, você não precisará fazer nenhuma mudança no projeto.

  Se esses arquivos tiverem um número de versão menor, você deverá atualizar o projeto seguindo as instruções nas seções a seguir. Vá até [Parte 2](update-your-app-to-the-latest-advertising-libraries.md#part-2).

<span id="part-2" />
## Parte 2: Instale o SDK mais recente

Se o aplicativo usar uma versão do SDK anterior, siga estas instruções para se certificar de que você tenha o SDK mais recente no computador de desenvolvimento.

1. Verifique se o computador de desenvolvimento tem o Visual Studio 2015 (para projetos UWP, do Windows 8.1 ou Windows Phone 8.x) ou o Visual Studio 2013 (para projetos do Windows 8.1 ou Windows Phone 8.x) instalado.

  >**Observação**&nbsp;&nbsp;Se o Visual Studio estiver aberto no computador de desenvolvimento, feche-o antes de realizar as etapas a seguir.

1.  Desinstale todas as versões anteriores do SDK do Microsoft Advertising e do SDK do Ad Mediator do computador de desenvolvimento.

2.  Abra uma janela do **Prompt de Comando** e execute estes comandos para limpar todas as versões do SDK anteriores que possam ter sido instaladas com o Visual Studio, mas que talvez não sejam exibidas na lista de programas instalados no computador:

  ```
  MsiExec.exe /x{5C87A4DB-31C7-465E-9356-71B485B69EC8}
  MsiExec.exe /x{6AB13C21-C3EC-46E1-8009-6FD5EBEE515B}
  MsiExec.exe /x{6AC81125-8485-463D-9352-3F35A2508C11}
  ```

3.  Instale o SDK mais recente para o aplicativo:
  * Para aplicativos UWP no Windows 10, instale o [Microsoft Store Services SDK](http://aka.ms/store-services-sdk).
  * Para aplicativos segmentados para uma versão anterior do sistema operacional, instale o [SDK do Microsoft Advertising para Windows e Windows Phone 8.x](http://aka.ms/store-8-sdk).

## Parte 3: Atualize o projeto

Siga essas instruções para atualizar o projeto.

### Projetos UWP para Windows 10

<span/>

Se o aplicativo usar **AdMediatorControl**, [refatore o aplicativo para usar AdControl](migrate-from-admediatorcontrol-to-adcontrol.md) em vez disso. **AdMediatorControl** não é mais compatível com aplicativos UWP.

Se o aplicativo usar **AdControl**, remova todas as referências existentes às bibliotecas do Microsoft Advertising do projeto e siga as instruções [AdControl em XAML](adcontrol-in-xaml-and--net.md) ou [AdControl em HTML](adcontrol-in-html-5-and-javascript.md) para adicionar as referências necessárias. Isso garantirá que o projeto use as bibliotecas corretas. É possível preservar a marcação e o código XAML existentes.

<span/>

### Projetos do Windows 8.1 ou Windows Phone 8.1 (XAML ou JavaScript/HTML)

<span/>

1. Remova todas as referências Microsoft.Advertising.\* e Microsoft.AdMediator.\* do projeto. Será possível ter duas referências (uma para Windows e uma para Windows Phone), se você tiver usado o modelo de projeto Universal.

2. Se o aplicativo usar **AdMediatorControl**, readicione as referências de biblioteca seguindo as instruções em [Adicionando e usando o controle de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx). Se o aplicativo usar **AdControl**, readicione as referências de biblioteca seguindo as instruções em [AdControl em XAML](adcontrol-in-xaml-and--net.md) ou [AdControl em HTML](adcontrol-in-html-5-and-javascript.md).

<span/>

Observe o seguinte:

* Se o aplicativo já tiver sido compilado para a plataforma **Any CPU**, você deverá recompilar o projeto para uma plataforma específica da arquitetura (x86, x64 ou ARM).

* Se tiver um aplicativo Windows Phone 8.x XAML que usou anteriormente uma versão do SDK na qual a classe **AdControl** tenha sido definida no namespace **Microsoft.Advertising.Mobile.UI**, você deverá atualizar o código para fazer referência à classe **AdControl** no namespace **Microsoft.Advertising.WinRT.UI** (essa classe moveu namespaces nas versões do SDK mais recentes).

* Qualquer outro problema senão o anterior, você poderá preservar a marcação e o código XAML existentes.

<span/>

### Projetos do Windows Phone 8.x Silverlight

<span/>

1. Remova todas as referências Microsoft.Advertising.\* e Microsoft.AdMediator.\* do projeto.

2. Se o aplicativo usar **AdMediatorControl**, readicione as referências de biblioteca seguindo as instruções em [Adicionando e usando o controle de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/dn864355.aspx). Se o aplicativo usar **AdControl**, readicione as referências da biblioteca seguindo as instruções em [AdControl no Windows Phone Silverlight](adcontrol-in-windows-phone-silverlight.md).

<span/>

Observe o seguinte:

* É possível preservar a marcação e o código XAML existentes.

* No **Gerenciador de Soluções**, verifique as propriedades da referência **Microsoft.Advertising.Mobile.UI** no projeto. Ela deverá ter a versão 6.2.40501.0 se o aplicativo for segmentado para o Windows Phone 8.0, ou 8.1.50112.0 se o aplicativo for segmentado para o Windows Phone 8.1.

* Para aplicativos Windows Phone 8.x Silverlight, o teste das unidades de produção em um emulador não é compatível. Recomendamos testar em um dispositivo.

## Parte 4: Teste e republique o aplicativo

Teste o aplicativo para garantir que ele exiba anúncios em faixa conforme o esperado.

Se a versão anterior do aplicativo já estiver disponível na Loja, [crie um novo envio](https://msdns.microsoft.com/windows/uwp/publish/app-submissions) para o aplicativo atualizado no painel do Centro de Desenvolvimento do Windows para republicá-lo.





 



<!--HONumber=Nov16_HO1-->


