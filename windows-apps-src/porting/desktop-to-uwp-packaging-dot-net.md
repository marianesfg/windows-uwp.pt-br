---
author: normesta
Description: "Este guia explica como configurar sua solução do Visual Studio para editar, depurar e empacotar seu aplicativo da área de trabalho usando a Ponte de Desktop."
Search.Product: eADQiWindows 10XVcnh
title: empacotar um app usando o Visual Studio (Ponte de Desktop)
ms.author: normesta
ms.date: 07/20/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 807a99a7-d285-46e7-af6a-7214da908907
ms.openlocfilehash: d8919448b965f18ff7f8fdaeda325889e495ef85
ms.sourcegitcommit: f6dd9568eafa10ee5cb2b849c0d82d84a1c5fb93
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 08/02/2017
---
# <a name="package-an-app-by-using-visual-studio-desktop-bridge"></a>empacotar um app usando o Visual Studio (Ponte de Desktop)

Você pode usar o Visual Studio para gerar um pacote para seu aplicativo da área de trabalho. Em seguida, você pode publicar esse pacote na Windows Store ou fazer o sideload dele em um ou mais computadores.

Este guia mostra como configurar a sua solução e, então, gerar um pacote para seu aplicativo da área de trabalho.

## <a name="first-consider-how-youll-distribute-your-app"></a>Primeiro, considere como você vai distribuir seu app

Se você planeja publicar seu app na [Windows Store](https://www.microsoft.com/store/apps), comece preenchendo [este formulário](https://developer.microsoft.com/windows/projects/campaigns/desktop-bridge). A Microsoft entrará em contato com você para iniciar o processo de instalação. Como parte deste processo, você irá reservar um nome na loja e obter informações que você precisará para empacotar seu aplicativo.

## <a name="add-a-packaging-project-to-your-solution"></a>Adicionar um projeto de empacotamento à sua solução

1. No Visual Studio, abra a solução que contém seu projeto de aplicativo da área de trabalho.

2. Adicione um projeto **App em Branco (Universal do Windows)** JavaScript à sua solução.

   Você não terá que adicionar nenhum código a ele. Ele existe apenas para gerar um pacote para você. Vamos nos referir a esse projeto como o "projeto de empacotamento".

   ![Projeto UWP javascript](images/desktop-to-uwp/javascript-uwp-project.png)

   >[!IMPORTANT]
   >Em geral, você deve usar a versão JavaScript deste projeto.  As versões do C#, VB.NET e C++ têm alguns problemas, mas se você quiser usar uma delas, consulte o guia [Problemas conhecidos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) antes de fazer isso.

## <a name="add-the-desktop-application-binaries-to-the-packaging-project"></a>Adicionar os binários do aplicativo da área de trabalho ao projeto de empacotamento

Adicione os binários diretamente ao projeto de empacotamento.

1. No **Solution Explorer**, expanda a pasta do projeto de empacotamento, crie uma subpasta e dê a ela o nome que quiser (por exemplo: **win32**).

2. Clique com o botão direito do mouse na subpasta e então escolha **Adicionar Item Existente**.

3. Na caixa de diálogo **Adicionar Item Existente**, localize e então adicione os arquivos da pasta de saída do seu aplicativo da área de trabalho. Isso inclui não apenas os arquivos executáveis, mas quaisquer arquivos dlls ou .config localizados nessa pasta.

   ![Arquivo executável de referência](images/desktop-to-uwp/cpp-exe-reference.png)

   Sempre que você fizer uma alteração ao seu projeto de aplicativo da área de trabalho, precisará copiar uma nova versão desses arquivos para o projeto de empacotamento. Você pode automatizar isso adicionando um evento de pós-compilação ao arquivo de projeto do projeto de empacotamento. Veja um exemplo.

   ```XML
   <Target Name="PostBuildEvent">
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.exe.config"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyWindowsFormsApplication.pdb"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.dll"
       DestinationFolder="win32" />
     <Copy SourceFiles="..\MyWindowsFormsApplication\bin\Debug\MyBusinessLogicLibrary.pdb"
       DestinationFolder="win32" />
   </Target>
   ```

## <a name="modify-the-package-manifest"></a>Modificar o manifesto do pacote

O projeto de empacotamento contém um arquivo que descreve as configurações do seu pacote. Por padrão, esse arquivo descreve um aplicativo UWP e, portanto, será necessário modificá-lo para que o sistema compreenda que seu pacote inclui um aplicativo da área de trabalho que é executado em confiança total.  

1. No **Solution Explorer**, expanda o projeto de empacotamento, clique com o botão direito do mouse no arquivo **package.appxmanifest** e então escolha **Exibir Código**.

   ![Fazer referência a projeto dotnet](images/desktop-to-uwp/reference-dotnet-project.png)

2. Adicione este namespace à parte superior do arquivo e adicione o prefixo de namespace à lista ``IgnorableNamespaces``.

   ```XML
   xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
   ```
   Quando terminar, suas declarações de namespace serão algo parecido com isto:

   ```XML
   <Package
     xmlns="http://schemas.microsoft.com/appx/manifest/foundation/windows10"
     xmlns:mp="http://schemas.microsoft.com/appx/2014/phone/manifest"
     xmlns:uap="http://schemas.microsoft.com/appx/manifest/uap/windows10"
     xmlns:rescap="http://schemas.microsoft.com/appx/manifest/foundation/windows10/restrictedcapabilities"
     IgnorableNamespaces="uap mp rescap">
   ```

3. Encontre o elemento ``TargetDeviceFamily`` e defina o atributo ``Name`` como **Windows. Desktop**, o atributo ``MinVersion`` como a versão mínima do projeto de empacotamento e o ``MaxVersionTested`` como a versão de destino do projeto de empacotamento.

   ```XML
   <TargetDeviceFamily Name="Windows.Desktop" MinVersion="10.0.10586.0" MaxVersionTested="10.0.15063.0" />
   ```

   Você pode encontrar a versão mínima e a versão de destino nas páginas de propriedades do projeto de empacotamento.

   ![Configurações de versão mínima e de destino](images/desktop-to-uwp/min-target-version-settings.png)


4. Remova o atributo ``StartPage`` do elemento ``Application``. Em seguida, adicione os atributos ``Executable`` e ``EntryPoint``.

   O elemento ``Application`` terá esta aparência.

   ```XML
   <Application Id="App"  Executable=" " EntryPoint=" ">
   ```

5. Defina o atributo ``Executable``como o nome do arquivo executável do seu aplicativo da área de trabalho. Em seguida, defina o atributo ``EntryPoint`` como **Windows.FullTrustApplication **.

   O elemento ``Application`` será semelhante a este.

   ```XML
   <Application Id="App"  Executable="win32\MyWindowsFormsApplication.exe" EntryPoint="Windows.FullTrustApplication">
   ```
6. Adicione o recurso ``runFullTrust`` ao elemento ``Capabilities``.

   ```XML
     <rescap:Capability Name="runFullTrust"/>
   ```
   Marcas rabiscadas azuis podem aparecer sob esta declaração, mas você pode ignorá-las com segurança.

   >[!IMPORTANT]
   Se você estiver criando um pacote para um aplicativo da área de trabalho em C++, precisará fazer algumas alterações extras em seu arquivo de manifesto para que possa implantar os tempos de execução do Visual C++ junto com seu app. Consulte [Uso de tempos de execução do Visual C++ em um projeto de ponte de desktop](https://blogs.msdn.microsoft.com/vcblog/2016/07/07/using-visual-c-runtime-in-centennial-project/).

7. Crie o projeto de empacotamento para garantir que nenhum erro apareça.

8. Se você quiser testar seu pacote, consulte [Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-debug.md).

   Em seguida, retorne para este guia e veja a próxima seção para gerar seu pacote.

## <a name="generate-a-package"></a>Gerar um pacote

Para gerar um pacote de seu app, siga as diretrizes descritas neste tópico: [Empacotamento de aplicativos UWP](..\packaging\packaging-uwp-apps.md).

Quando você chegar à tela **Selecionar e configurar pacotes**, reserve um momento para considerar quais tipos de binários você está incluindo em seu pacote antes de marcar qualquer uma das caixas de seleção.

* Se você tiver [estendido](desktop-to-uwp-extend.md) seu aplicativo da área de trabalho ao adicionar um projeto da Plataforma Universal do Windows baseado em C#, C++ ou VB.NET à sua solução, marque as caixas de seleção **x86** e **x64**.  

* Caso contrário, escolha a caixa de seleção **Neutro**.

>[!NOTE]
O motivo pelo qual você teria de escolher explicitamente cada plataforma com suporte é que uma solução estendida contém dois tipos de binários, um para o projeto UWP e outro para o projeto da área de trabalho. Como eles são tipos diferentes de binários, o .NET Native precisa produzir explicitamente binários nativos para cada plataforma.

Se você receber erros ao tentar gerar seu pacote, consulte o guia [Problemas conhecidos](https://docs.microsoft.com/windows/uwp/porting/desktop-to-uwp-known-issues#known-issues-anchor) e, se o seu problema não estiver nessa lista, compartilhe-o conosco [aqui](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

## <a name="next-steps"></a>Próximas etapas

**Execute seu aplicativo/encontre e corrija os problemas**

Consulte [Executar, depurar e testar um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-debug.md)

**Aprimorar seu aplicativo da área de trabalho ao adicionar APIs UWP**

Consulte [Aprimorar seu aplicativo da área de trabalho para Windows 10](desktop-to-uwp-enhance.md)

**Estender seu aplicativo da área de trabalho ao adicionar componentes UWP**

Consulte [Estender seu aplicativo da área de trabalho com componentes UWP modernos](desktop-to-uwp-extend.md).

**Distribua seu aplicativo**

Consulte [Distribuir um aplicativo da área de trabalho empacotado (Ponte de Desktop)](desktop-to-uwp-distribute.md)

**Encontre respostas para dúvidas específicas**

Nossa equipe monitora estas [marcas do StackOverflow](http://stackoverflow.com/questions/tagged/project-centennial+or+desktop-bridge).

**Envie seus comentários sobre este artigo**

Use a seção de comentários abaixo.
