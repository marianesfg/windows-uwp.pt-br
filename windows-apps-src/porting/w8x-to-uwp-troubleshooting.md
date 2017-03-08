---
author: mcleblanc
description: "É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado."
title: Solucionando problemas de portabilidade do Windows Runtime 8.x para UWP
ms.assetid: 1882b477-bb5d-4f29-ba99-b61096f45e50
ms.author: markl
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 6c10376854656abe276c53a9b6778665c1d47a4b
ms.lasthandoff: 02/07/2017

---

# <a name="troubleshooting-porting-windows-runtime-8x-to-uwp"></a>Solucionando problemas de portabilidade do Windows Runtime 8.x para UWP

\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

O tópico anterior era [Portando o projeto](w8x-to-uwp-porting-to-a-uwp-project.md).

É altamente recomendável ler este guia de portabilidade até o final, mas também entendemos que você esteja ansioso para avançar e chegar ao estágio em que o seu projeto é compilado e executado. Para essa finalidade, você pode fazer um progresso temporário comentando ou eliminando qualquer código não essencial e, em seguida, retornando para saldar esse débito mais tarde. A tabela de solução de problemas de sintomas e soluções deste tópico pode ser útil para você neste estágio, embora não seja uma substituição para a leitura dos próximos tópicos. Você sempre pode consultar novamente a tabela conforme avança pelos próximos tópicos.

## <a name="tracking-down-issues"></a>Rastreando problemas

Exceções de análise XAML podem ser difíceis de diagnosticar, especialmente se não houver mensagens de erro significativas na exceção. Certifique-se de que o depurador esteja configurado para capturar exceções de primeira chance (para tentar e capturar a exceção de análise logo no início). Você pode inspecionar a variável de exceção no depurador para determinar se o HRESULT ou a mensagem tem informações úteis. Além disso, verifique na janela de saída do Visual Studio se há mensagens de erro de saída do analisador XAML.

Se o seu aplicativo for encerrado e tudo o que você sabe é que uma exceção sem tratamento foi lançada durante a análise da marcação XAML, então isso poderia ser o resultado de uma referência a um recurso ausente (ou seja, um recurso cuja chave existe para aplicativos Universal 8.1 mas não para aplicativos do Windows 10, tais como algumas chaves de estilo **TextBlock** do sistema). Ou ela pode ser uma exceção lançada dentro de um **UserControl**, um controle personalizado ou um painel de layout personalizado.

Um último recurso é uma divisão binária. Remova cerca da metade da marcação de uma página e execute novamente o aplicativo. Em seguida, você saberá se o erro está em algum lugar dentro da metade removida (que agora você deverá restaurar em qualquer caso) ou na metade que você *não* removeu. Repita o processo dividindo a metade que contém o erro e assim por diante, até zerar o problema.

## <a name="targetplatformversion"></a>TargetPlatformVersion

Esta seção explica o que fazer se, ao abrir um projeto do Windows 10 no Visual Studio, aparecer a mensagem "Atualização do Visual Studio necessária. Um ou mais projetos exigem um SDK <version> da plataforma que não está instalado ou que faz parte de uma atualização futura do Visual Studio".

-   Primeiro, determine o número da versão do SDK do Windows 10 que você instalou. Navegue até **C:\\Program Files (x86)\\Windows Kits\\10\\Include\\<versionfoldername>** e anote *<versionfoldername>*, que estará em notação quádrupla, "(Principal.Secundária.Compilação.Revisão".
-   Abra o arquivo do projeto para editar e encontre os elementos `TargetPlatformVersion` e `TargetPlatformMinVersion`. Edite-os como a seguir, substituindo *<versionfoldername>* pelo número da versão em notação quádrupla encontrado no disco:

```xml
   <TargetPlatformVersion><versionfoldername></TargetPlatformVersion>
    <TargetPlatformMinVersion><versionfoldername></TargetPlatformMinVersion>
```

## <a name="troubleshooting-symptoms-and-remedies"></a>Solução de problemas de sintomas e soluções

As informações de solução da tabela destinam-se a dar instruções suficientes para você agir por conta própria. Você encontrará mais detalhes sobre cada um desses problemas durante a leitura dos tópicos posteriores.

| Sintoma | Solução |
|---------|--------|
| Ao abrir um projeto do Windows 10 no Visual Studio, você verá a mensagem "Atualização do Visual Studio necessária. Um ou mais projetos exigem um SDK &lt;versão&gt; da plataforma que não está instalado ou que faz parte de uma atualização futura do Visual Studio".
 | Consulte a seção [TargetPlatformVersion](#targetplatformversion) neste tópico. |
| Um System.InvalidCastException é gerado quando InitializeComponent é chamado em um arquivo xaml.cs.| Isso pode acontecer quando você tem mais de um arquivo xaml (sendo que pelo menos um deles contém qualificadores MRT) que compartilham o mesmo arquivo xaml.cs e os elementos têm atributos x:Name inconsistentes entre os dois arquivos xaml. Tente adicionar o mesmo nome aos mesmos elementos nos dois arquivos xaml ou omita todos os nomes. |
| Quando executado no dispositivo, o aplicativo é encerrado, ou quando iniciado no Visual Studio, você vê o erro "Não é possível ativar o aplicativo da Windows Store \[…\]. A solicitação de ativação falhou com o erro "O Windows não pôde se comunicar com o aplicativo de destino". Isso geralmente indica que o processo do aplicativo de destino foi anulado. \[…\]”. | O problema poderia ser o código imperativo sendo executado em suas próprias páginas ou em propriedades vinculadas (ou outros tipos) durante a inicialização. Ou, isso pode acontecer durante a análise do arquivo XAML prestes a ser exibido quando o aplicativo foi encerrado (se inicializado no Visual Studio, essa será a página de inicialização). Procure chaves de recurso inválidas e/ou tente algumas das diretrizes da seção "Rastreando problemas" neste tópico.|
| O analisador ou o compilador de XAML, ou uma exceção de tempo de execução, mostra o erro "*O recurso "<resourcekey>" não pôde ser resolvido*". | A chave de recurso não se aplica a aplicativos da Plataforma Universal do Windows (UWP) (isso acontece com alguns recursos do Windows Phone, por exemplo). Encontre o recurso equivalente correto e atualize sua marcação. Exemplos que você pode encontrar imediatamente são chaves de sistema, como `PhoneAccentBrush`. |
| O compilador C# mostra o erro "*O nome do tipo ou do namespace '<name>' não foi encontrado \[...\]*" ou "*O nome do tipo ou do namespace '<name>' não existe no namespace \[...\]*" ou "*O nome do tipo ou do namespace '<name>' não existe no contexto atual*". | Isso provavelmente significa que o tipo é implementado em um SDK de extensão (embora possa haver casos em que a solução não seja tão simples). Use o conteúdo de referência [APIs do Windows](https://msdn.microsoft.com/library/windows/apps/bg124285) para determinar qual SDK de extensão implementa a API e, em seguida, use o comando **Adicionar** > **Referência** do Visual Studio para adicionar uma referência a esse SDK ao seu projeto. Caso o aplicativo esteja direcionado para o conjunto de APIs conhecido como a família de dispositivos universais, é essencial que você use a classe [**ApiInformation**](https://msdn.microsoft.com/library/windows/apps/dn949001) para testar em tempo de execução a presença do SDK de extensão antes de chamá-los (isso é chamado de código adaptável). Caso haja uma API universal, ela é sempre preferível a uma API no SDK de extensão. Para obter mais informações, consulte [SDKs de extensão](w8x-to-uwp-porting-to-a-uwp-project.md). |

O próximo tópico é [Portando XAML e a interface do usuário](w8x-to-uwp-porting-xaml-and-ui.md).


