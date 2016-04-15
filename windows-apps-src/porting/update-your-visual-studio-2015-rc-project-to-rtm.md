---
description: Se você tiver um projeto do Windows 10 criado com o Microsoft Visual Studio 2015 RC, você tem duas opções de atualização dos arquivos de projeto para o formato adequado para Visual Studio 2015 RTM.
title: Atualizar seu projeto do Microsoft Visual Studio 2015 RC para RTM na UWP
ms.assetid: 104E36CE-36DE-4E9C-A944-711C200B44EF
---

# Atualizar seu projeto do Microsoft Visual Studio 2015 RC para RTM na UWP

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos do Windows 8.x, veja o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Se você tiver um projeto do Windows 10 criado com o Microsoft Visual Studio 2015 RC, você tem duas opções de atualização dos arquivos de projeto para o formato adequado para Visual Studio 2015 RTM. O método recomendado é criar um novo projeto do Windows 10 no Visual Studio 2015 RTM e copiar seus arquivos para ele. Como alternativa, você pode seguir a documentação avançada para editar seus arquivos de projeto existentes e transferi-los para o novo formato.

## O que você vê quando abre um projeto do Windows 10Visual Studio 2015 RC no Visual Studio 2015 RTM

Quando você abrir um projeto do Windows 10Visual Studio 2015 RC no Visual Studio 2015 RTM, você verá uma mensagem de "atualização necessária" no **Gerenciador de Soluções**.

![atualização necessária](images/vsrc-to-rtm/solution-explorer.png)

Se você acessar o menu de contexto do projeto no **Gerenciador de Soluções** e escolher **Recarregar Projeto**, verá esta caixa de diálogo.

![atualização do visual studio necessária](images/vsrc-to-rtm/reload-project.png)

## Crie um novo projeto e copie os arquivos nele

1.  Inicie o Microsoft Visual Studio 2015 RTM e crie um novo projeto Aplicativo em Branco (Windows Universal). Lembre-se de que, por padrão, o novo projeto cria um pacote do aplicativo (um arquivo appx) direcionado à família de dispositivos Universal. Altere isto se você está direcionando uma ou mais famílias de dispositivos específicos.
2.  No seu projeto do Visual Studio 2015 RC, identifique todos os arquivos de código-fonte e arquivos de ativo visual que você deseja copiar. Usando o Explorador de Arquivos, copie modelos de dados, modelos de exibição, ativos visuais, Dicionários de Recursos, estrutura de pastas e qualquer outra coisa de que precise (inclusive AssemblyInfo.cs) para seu novo projeto. Copie ou crie subpastas no disco conforme necessário.
3.  Copie também os modos de exibição (por exemplo, MainPage.xaml e MainPage.xaml.cs) para o novo projeto. Novamente, crie novas subpastas conforme necessário e remova os modos de exibição existentes do projeto. Porém, antes de substituir ou remover um modo de exibição gerado pelo Visual Studio, mantenha uma cópia, pois ela pode ser útil para referência posterior.
4.  No **Gerenciador de Soluções**, verifique se **Mostrar Todos os Arquivos** está ativado. Selecione os arquivos copiados, clique neles com o botão direito do mouse e clique em **Incluir no Projeto**. Isso incluirá automaticamente suas pastas continentes. Em seguida, você pode desativar **Mostrar Todos os Arquivos**, se desejar. Um fluxo de trabalho alternativo, se preferir, é usar o comando **Adicionar Item Existente**, tendo criado qualquer subpasta necessária no **Gerenciador de Soluções** do Visual Studio. Verifique se os ativos visuais têm **Ação de Compilação** definida como **Conteúdo** e **Copiar para Diretório de Saída** definida como **Não Copiar**.
5.  Adicione referências a qualquer extensão SDKs que você mencionou em seu projeto RC, e copie quaisquer alterações de seu Package.appxmanifest anterior (por exemplo, quaisquer recursos declarados) para aquela no novo projeto RTM.

## Avançadas: Editar seus arquivos de projeto existentes

Uma diferença significativa entre o formato do projeto Windows 10 do Visual Studio 2015 RC e do Visual Studio 2015 RTM é que o formato RTM utiliza [NuGet](http://docs.nuget.org/) versão 3. Mantenha esta diferença em mente se você pretende atualizar manualmente seu projeto.

Se você deseja atualizar manualmente seu projeto, ou se você estiver interessado em conhecer as diferenças entre os formatos de projeto do Visual Studio 2015 RC e do Visual Studio 2015 RTM, consulte [Migrar aplicativos para UWP (Plataforma Universal do Windows)](http://msdn.microsoft.com/library/mt148501.aspx).



<!--HONumber=Mar16_HO1-->


