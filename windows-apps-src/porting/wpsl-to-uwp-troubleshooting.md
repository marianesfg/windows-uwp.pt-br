---
author: stevewhims
description: É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado.
title: Solução de problemas de portabilidade do WindowsPhone Silverlight para UWP
ms.assetid: d9a9a2a7-9401-4990-a992-4b13887f2661
ms.author: stwhi
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 3a5d3ab7b50721b969859006831b33e9b00e300f
ms.sourcegitcommit: e814a13978f33654d8e995584f4b047cb53e0aef
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/05/2018
ms.locfileid: "6039656"
---
#  <a name="troubleshooting-porting-windowsphone-silverlight-to-uwp"></a>Solução de problemas de portabilidade do WindowsPhone Silverlight para UWP


O tópico anterior era [Portando o projeto](wpsl-to-uwp-porting-to-a-uwp-project.md).

É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado. Para essa finalidade, você pode fazer um progresso temporário comentando ou eliminando qualquer código não essencial e, em seguida, retornando para saldar esse débito mais tarde. A tabela de solução de problemas de sintomas e soluções deste tópico pode ser útil para você neste estágio, embora não seja uma substituição para a leitura dos próximos tópicos. Você sempre pode consultar novamente a tabela conforme avança pelos próximos tópicos.

## <a name="tracking-down-issues"></a>Rastreando problemas

Exceções de análise XAML podem ser difíceis de diagnosticar, especialmente se não houver mensagens de erro significativas na exceção. Certifique-se de que o depurador esteja configurado para capturar exceções de primeira chance (para tentar e capturar a exceção de análise logo no início). Você pode inspecionar a variável de exceção no depurador para determinar se o HRESULT ou a mensagem tem informações úteis. Além disso, verifique na janela de saída do Visual Studio se há mensagens de erro de saída do analisador XAML.

Se seu aplicativo for encerrado e tudo o que você sabe é que uma exceção sem tratamento foi lançada durante a análise da marcação XAML, isso pode ser o resultado de uma referência a um recurso ausente (ou seja, um recurso cuja chave existe para aplicativos do WindowsPhone Silverlight, mas não para Windows 10 aplicativos, como algumas chaves de estilo de **TextBlock** do sistema). Ou ela pode ser uma exceção lançada dentro de um **UserControl**, um controle personalizado ou um painel de layout personalizado.

Um último recurso é uma divisão binária. Remova cerca da metade da marcação de uma página e execute novamente o aplicativo. Em seguida, você saberá se o erro está em algum lugar dentro da metade removida (que agora você deverá restaurar em qualquer caso) ou na metade que você *não* removeu. Repita o processo dividindo a metade que contém o erro e assim por diante, até zerar o problema.

## <a name="targetplatformversion"></a>TargetPlatformVersion

Esta seção explica o que fazer se, ao abrir um projeto do Windows 10 no Visual Studio, você verá a mensagem "atualização do Visual Studio necessária. Um ou mais projetos exigem um SDK &lt;versão&gt; da plataforma que não está instalado ou que faz parte de uma atualização futura do Visual Studio".


-   Primeiro, determine o número de versão do SDK para Windows 10 que você instalou. Navegue para **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\&lt;nomedapastadaversão&gt;** e faça uma anotação de *&lt;nomedapastadaversão&gt;*, que será em notação quad, "Major.Minor.Build.Revision".
-   Abra o arquivo do projeto para editar e encontre os elementos `TargetPlatformVersion` e `TargetPlatformMinVersion`. Edite-os como a seguir, substituindo *&lt;nomedapastadaversão&gt;* pelo número da versão em notação quádrupla encontrado no disco:

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
   <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Solução de problemas de sintomas e soluções

As informações de solução da tabela destinam-se a dar instruções suficientes para você agir por conta própria. Você encontrará mais detalhes sobre cada um desses problemas durante a leitura dos tópicos posteriores.

| Sintoma | Solução |
|---------|--------|
| O analisador ou o compilador de XAML mostra o erro "_O nome "&lt;nome do tipo&gt;" não existe no namespace […]._" | Se &lt;nome do tipo&gt; for um tipo personalizado, então, em suas declarações de prefixo de namespace na marcação XAML, altere "clr-namespace" para "using", e remova quaisquer tokens de assembly. Para tipos de plataforma, isso significa que o tipo não se aplica à UWP (Plataforma Universal do Windows), portanto, encontre o equivalente e atualize sua marcação. Exemplos que você pode encontrar imediatamente são `phone:PhoneApplicationPage` e `shell:SystemTray.IsVisible`. | 
| O analisador ou compilador de XAML mostra o erro "_O membro"&lt;nomedomembro&gt;" não é reconhecido ou não está acessível._" ou "_A propriedade"&lt;nomedapropriedade&gt;" não foi encontrada no tipo [...]._". | Esses erros começarão a aparecer depois que você tiver portado alguns nomes de tipo, como a raiz **Page**. O membro ou a propriedade não se aplica à UWP, portanto, encontre o equivalente e atualize sua marcação. Exemplos que você pode encontrar imediatamente são `SupportedOrientations` e `Orientation`. |
| O analisador ou o compilador de XAML mostra o erro "_A propriedade anexável [...] não foi encontrada [...]._" ou "_Membro anexável desconhecido [...]._". | Isso provavelmente é causado pelo tipo em vez da propriedade anexada; nesse caso, você já terá um erro para o tipo e esse erro desaparecerá depois que corrigir isso. Exemplos que você pode encontrar imediatamente são `phone:PhoneApplicationPage.Resources` e `phone:PhoneApplicationPage.DataContext`. | 
|O analisador ou o compilador de XAML, ou uma exceção de tempo de execução, mostra o erro "_O recurso "&lt;resourcekey&gt;" não pôde ser resolvido._". | A chave do recurso não se aplica a aplicativos UWP (Plataforma Universal do Windows). Encontre o recurso equivalente correto e atualize sua marcação. Exemplos que você pode encontrar imediatamente são chaves de estilo de sistema **TextBlock** como `PhoneTextNormalStyle`. |
| O compilador C# mostra o erro "_O nome do tipo ou do namespace '&lt;nome&gt;' não foi encontrado [...]_" ou "_O nome do tipo ou do namespace '&lt;nome&gt;' não existe no namespace [...]_" ou "_O nome do tipo ou do namespace '&lt;nome&gt;' não existe no contexto atual_". | Isso provavelmente significa que o compilador ainda não sabe o namespace correto da UWP para um tipo. Você pode usar o comando **Resolver** do Visual Studio para corrigir isso. <br/>Se a API não está no conjunto de APIs, conhecido como a família de dispositivos universal (em outras palavras, a API é implementada em um SDK de extensão), então, use os [SDKs de extensão](wpsl-to-uwp-porting-to-a-uwp-project.md).<br/>Pode haver outros casos em que a portabilidade será menos direta. Exemplos que você pode encontrar imediatamente são `DesignerProperties` e `BitmapImage`. | 
|Quando executado no dispositivo, o aplicativo é encerrado, ou quando iniciado no Visual Studio, você vê o erro "não é possível ativar o Windows Runtime 8. x aplicativo [...]. A solicitação de ativação falhou com o erro "O Windows não pôde se comunicar com o aplicativo de destino". Isso geralmente indica que o processo do aplicativo de destino foi anulado. […]”. | O problema poderia ser o código imperativo sendo executado em suas próprias páginas ou em propriedades vinculadas (ou outros tipos) durante a inicialização. Ou, isso pode acontecer durante a análise do arquivo XAML prestes a ser exibido quando o aplicativo foi encerrado (se inicializado no Visual Studio, essa será a página de inicialização). Procure chaves de recurso inválidas e/ou tente algumas das diretrizes da seção [Rastreando problemas](#tracking-down-issues) neste tópico.|
| _Erro de XamlCompiler WMC0055: não é possível atribuir o valor de texto '&lt;sua geometria de fluxo&gt;' na propriedade 'Clip' do tipo 'RectangleGeometry'_ | Na UWP, o tipo do aplicativo [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) e XAML C++ UWP. |
| _Erro de XamlCompiler WMC0001: tipo desconhecido 'RadialGradientBrush' no namespace XML [...]_ | A UWP não tem o tipo **RadialGradientBrush**. Remova o **RadialGradientBrush** da marcação e usar algum outro tipo de aplicativo [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) e XAML C++ UWP. |
| _Erro de XamlCompiler WMC0011: membro desconhecido 'OpacityMask' no elemento '&lt;tipo UIElement&gt;'_ | O aplicativo UWP [Microsoft DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274) e XAML C++ UWP. |
| _Uma exceção de primeira chance do tipo 'System.Runtime.InteropServices.COMException' ocorreu em SYSTEM.NI.DLL. Informações adicionais: O aplicativo chamou uma interface que foi empacotada para um thread diferente. (Exceção de HRESULT: 0x8001010E (RPC_E_WRONG_THREAD))._ | O trabalho que você está fazendo precisa ser feito no thread da interface do usuário. Chame o [**CoreWindow.GetForCurrentThread**](https://msdn.microsoft.com/library/windows/apps/hh701589)). |
| Uma animação está em execução, mas ela não está tendo efeito em sua propriedade de destino. | Torne a animação independente ou defina `EnableDependentAnimation="True"` nela. Consulte [Animação](wpsl-to-uwp-porting-xaml-and-ui.md). |
| Ao abrir um projeto do Windows 10 no Visual Studio, aparecer a mensagem "atualização do Visual Studio necessária. Um ou mais projetos exigem um SDK &lt;versão&gt; da plataforma que não está instalado ou que faz parte de uma atualização futura do Visual Studio".
 | Consulte a seção [TargetPlatformVersion](#targetplatformversion) neste tópico. |
| Um System.InvalidCastException é gerado quando InitializeComponent é chamado em um arquivo xaml.cs. | Isso pode acontecer quando você tem mais de um arquivo xaml (sendo que pelo menos um deles contém qualificadores MRT) que compartilham o mesmo arquivo xaml.cs e os elementos têm atributos x:Name inconsistentes entre os dois arquivos xaml. Tente adicionar o mesmo nome aos mesmos elementos nos dois arquivos xaml ou omita todos os nomes. | 

O próximo tópico é [Portando XAML e a interface do usuário](wpsl-to-uwp-porting-xaml-and-ui.md).

