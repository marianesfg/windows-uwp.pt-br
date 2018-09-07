---
author: PatrickFarley
ms.assetid: 1526FF4B-9E68-458A-B002-0A5F3A9A81FD
title: Testes do Kit de Certificação de Aplicativos Windows
description: O Kit de certificação de aplicativo do Windows contém diversos testes que podem ajudar a garantir que seu aplicativo está pronto para ser publicado na Microsoft Store.
ms.author: pafarley
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: Windows 10, uwp, certificação de aplicativos
ms.localizationpriority: medium
ms.openlocfilehash: 49ecc472c8c1d4adebd8376fce9d2d5e6e2a955e
ms.sourcegitcommit: 00d27738325d6db5b5e481911ae7fac0711b05eb
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/07/2018
ms.locfileid: "3666127"
---
# <a name="windows-app-certification-kit-tests"></a>Testes do Kit de Certificação de Aplicativos Windows


O [Kit de certificação de aplicativo do Windows](windows-app-certification-kit.md) contém diversos testes que ajudam a garantir que seu aplicativo esteja pronto para ser publicado na Microsoft Store. Os testes estão listados abaixo com seus critérios, detalhes e ações no caso de falha sugeridas.

## <a name="deployment-and-launch-tests"></a>Implantação e testes de inicialização

Monitora o aplicativo durante o teste de certificação para registrar quando ele falha ou trava.

### <a name="background"></a>Histórico

Aplicativos que param de responder ou travam podem provocar a perda de dados do usuário ou uma experiência insatisfatória.

Esperamos que os aplicativos estejam totalmente funcionais sem o uso dos modos de compatibilidade do Windows, de mensagens AppHelp ou de correções de compatibilidade.

Os aplicativos não devem listar DLLs para carregamento na chave do Registro HKEY\-LOCAL\-MACHINE\\Software\\Microsoft\\Windows NT\\CurrentVersion\\Windows\\AppInit\-DLLs.

### <a name="test-details"></a>Detalhes do teste

Testamos a resiliência e a estabilidade do aplicativo por meio do teste de certificação.

O Kit de Certificação de Aplicativos Windows chama [**IApplicationActivationManager::ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903) para iniciar os aplicativos. Para que o **ActivateApplication** inicie um aplicativo, o UAC (Controle de Conta de Usuário) deve estar habilitado, e a resolução da tela deve ser de pelo menos 1024 x 768 ou 768 x 1024. Se nenhuma condição for atendida, o aplicativo falhará nesse teste.

### <a name="corrective-actions"></a>Ações corretivas

Verifique se o UAC está habilitado no computador de teste.

Verifique se você está executando o teste em um computador com uma tela grande o suficiente.

Se o aplicativo falhar para iniciar e sua plataforma de teste satisfizer os pré-requisitos do [**ActivateApplication**](https://msdn.microsoft.com/library/windows/desktop/Hh706903), você poderá solucionar o problema analisando o log de eventos de ativação. Para encontrar essas entradas no log de eventos:

1.  Abra eventvwr.exe e navegue até a pasta Logs de Aplicativos e Serviços\\Microsoft\\Windows\\Immersive-Shell.
2.  Filtre a exibição para mostrar as IDs de Evento: 5900-6000.
3.  Analise as entradas do log para obter informações que possam explicar por que o aplicativo não foi iniciado.

Identifique e solucione o problema com o arquivo. Compile e teste novamente o aplicativo. Você também pode verificar se um arquivo de despejo, que pode ser usado para depurar o aplicativo, foi gerado na pasta de log do Kit de Certificação de Aplicativos Windows.

## <a name="platform-version-launch-test"></a>Teste de inicialização de versão de plataforma

Verifica se o aplicativo do Windows pode ser executado em uma versão futura do sistema operacional. Esse teste foi aplicado historicamente apenas ao fluxo de trabalho do aplicativo da área de trabalho, mas agora ele está habilitado para fluxos de trabalho da Loja e da Plataforma Universal do Windows (UWP).

### <a name="background"></a>Histórico

Informações de versão do sistema operacional têm uso restrito para a Microsoft Store. Elas têm sido usadas incorretamente com frequência pelos aplicativos para verificar a versão do sistema operacional, de forma que o aplicativo possa fornecer aos usuários as funcionalidades específicas de uma versão do sistema operacional.

### <a name="test-details"></a>Detalhes do teste

O Kit de Certificação de Aplicativos Windows usa a HighVersionLie para detectar como o aplicativo verifica a versão do sistema operacional. Se o aplicativo falhar, ele não passará nesse teste.

### <a name="corrective-action"></a>Ação corretiva

Os aplicativos devem usar as funções auxiliares da API de versão para verificar isso. Consulte [Versão do sistema operacional](https://msdn.microsoft.com/library/windows/desktop/ms724832) para saber mais.

## <a name="background-tasks-cancellation-handler-validation"></a>Validação do manipulador de cancelamento de tarefas em segundo plano

Verifica se o aplicativo tem um manipulador de cancelamento para tarefas em segundo plano declaradas. Precisa haver uma função dedicada que será chamada quando a tarefa for cancelada. Esse teste é aplicado somente a aplicativos implantados.

### <a name="background"></a>Histórico

Os Aplicativos Universais do Windows podem registrar um processo que seja executado em segundo plano. Por exemplo, um aplicativo de email pode executar ping no servidor de tempos em tempos. No entanto, se o sistema operacional precisar desses recursos, ele cancelará a tarefa em segundo plano e os aplicativos deverão lidar normalmente com esse cancelamento. Aplicativos que não têm um manipulador de cancelamento podem falhar ou não fechar quando o usuário tentar fechá-los.

### <a name="test-details"></a>Detalhes do teste

O aplicativo é iniciado, suspenso e a parte que não está em segundo plano do aplicativo é encerrada. Em seguida, as tarefas em segundo plano associadas a esse aplicativo serão canceladas. O estado do aplicativo é verificado e se o aplicativo ainda estiver em execução, ele falhará neste teste.

### <a name="corrective-action"></a>Ação corretiva

Adicione o manipulador de cancelamento ao seu aplicativo. Para obter mais informações, consulte [Oferecer suporte a tarefas em segundo plano em seu aplicativo](https://msdn.microsoft.com/library/windows/apps/Mt299103).

## <a name="app-count"></a>Contagem de aplicativo

Verifica se um pacote do aplicativo (APPX, lote de aplicativo) contém um aplicativo. Isso foi alterado no kit para ser um teste autônomo.

### <a name="background"></a>Histórico

Esse teste foi implementado em conformidade com a política da Loja.

### <a name="test-details"></a>Detalhes do teste

Para aplicativos do Windows Phone 8.1, o teste verifica se o número total de pacotes appx no lote é &lt; 512, se há apenas um pacote principal no lote e se a arquitetura do pacote principal no lote está marcada como ARM ou neutra.

Para aplicativos do Windows 10, o teste verifica se o número de revisão na versão do lote está definido como 0.

### <a name="corrective-action"></a>Ação corretiva

Garantir que o pacote do aplicativo e lote atendam aos requisitos acima em Detalhes do teste.

## <a name="app-manifest-compliance-test"></a>Teste de conformidade de manifesto do aplicativo

Teste o conteúdo do manifesto do aplicativo para garantir que seu conteúdo esteja correto.

### <a name="background"></a>Histórico

Os aplicativos devem ter um manifesto corretamente formatado.

### <a name="test-details"></a>Detalhes do teste

Analisa o manifesto do aplicativo para verificar se o conteúdo está correto, conforme descrito em [Requisitos do pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/Mt148525).

-   **Protocolos e extensões de arquivo**

    Seu aplicativo pode declarar as extensões de arquivo às quais se associar. Se isso for feito de maneira indevida, o aplicativo poderá declarar um número imenso de extensões de arquivo, a maior parte das quais ele poderá nem mesmo usar, resultando em uma experiência do usuário insatisfatória. Este teste adicionará uma verificação para limitar o número de extensões de arquivo aos quais um aplicativo pode se associar.

-   **Regra de Dependência de Estrutura**

    Esse teste enfatiza o requisito de que os aplicativos obtenham as devidas dependências na UWP. Se existir uma dependência inadequada, o teste falhará.

    Se houver incompatibilidade entre a versão do sistema operacional ao qual o aplicativo se aplica e as dependências de estrutura que foram feitas, o teste falhará. O teste também falha quando o aplicativo se refere a versões "preview" das dlls de estrutura.

-   **Verificação de comunicação entre processos (IPC)**

    Esse teste impõe o requisito de que os aplicativos UWP não se comunicam fora do contêiner do aplicativo para componentes de Desktop. A comunicação entre processos é destinada apenas a aplicativos de sideload. Os aplicativos que especificam o [**ActivatableClassAttribute**](https://msdn.microsoft.com/library/windows/apps/BR211414) com nome igual a "DesktopApplicationPath" falham nesse teste.

### <a name="corrective-action"></a>Ação corretiva

Analise o manifesto do aplicativo em relação aos requisitos descritos em [Requisitos do pacote do aplicativo](https://msdn.microsoft.com/library/windows/apps/Mt148525).

## <a name="windows-security-features-test"></a>Teste dos recursos de segurança do Windows

### <a name="background"></a>Histórico

Alterar as proteções de segurança padrão do Windows pode colocar os clientes em risco elevado.

### <a name="test-details"></a>Detalhes do teste

Testa a segurança do aplicativo executando o [Analisador de Binários BinScope](#binscope-binary-analyzer-tests).

Os testes do Analisador de Binários BinScope examinam os arquivos binários do aplicativo em busca de práticas de programação e compilação que o tornam menos vulnerável a ataques ou a ser usado como um vetor de ataque.

Os testes do Analisador de Binários BinScope verificam o uso correto dos seguintes recursos relacionados à segurança:

-   Testes do Analisador de Binários BinScope
-   Assinatura de códigos privados

### <a name="binscope-binary-analyzer-tests"></a>Testes do Analisador de Binários BinScope

Os testes do [Analisador de Binários BinScope](https://www.microsoft.com/en-us/download/details.aspx?id=44995) examinam os arquivos binários do aplicativo em busca de práticas de programação e compilação que o tornam menos vulnerável a ataques ou a ser usado como um vetor de ataque.

Os testes do Analisador de Binários BinScope verificam o uso correto destes recursos relacionados à segurança:

-   [AllowPartiallyTrustedCallersAttribute](#binscope-1)
-   [Proteção de manipulação de exceções /SafeSEH](#binscope-2)
-   [Prevenção de Execução de Dados](#binscope-3)
-   [ASLR (Address Space Layout Randomization)](#binscope-4)
-   [Ler/gravar seção PE compartilhada](#binscope-5)
-   [AppContainerCheck](#appcontainercheck)
-   [ExecutableImportsCheck](#binscope-7)
-   [WXCheck](#binscope-8)

### <a name="span-idbinscope-1spanallowpartiallytrustedcallersattribute"></a><span id="binscope-1"></span>AllowPartiallyTrustedCallersAttribute

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** Falha no teste APTCACheck

O atributo AllowPartiallyTrustedCallersAttribute (APTCA) habilita o acesso a código totalmente confiável a partir de código parcialmente confiável em assemblies assinados. Quando você aplica o atributo APTCA a um assembly, chamadores parcialmente confiáveis podem acessar esse assembly durante a vida do assembly, o que pode comprometer a segurança.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Não use o atributo APTCA em assemblies com nome forte, a menos que seu projeto exija e os riscos sejam bem entendidos. Nesses casos, verifique se todas as APIs estão protegidas com as devidas demandas de segurança de código de acesso. O APTCA não tem efeito quando o assembly faz parte de um aplicativo da Plataforma Universal do Windows (UWP).

**Comentários**

Esse teste é realizado apenas em código gerenciado (C#, .NET etc.).

### <a name="span-idbinscope-2spansafeseh-exception-handling-protection"></a><span id="binscope-2"></span>Proteção de manipulação de exceções /SafeSEH

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** falha no teste SafeSEHCheck

Um manipulador de exceções é executado quando o aplicativo encontra uma condição excepcional, tal como um erro de divisão por zero. Como o endereço do manipulador da exceção é armazenado na pilha quando uma função é chamadas, ele ficaria vulnerável a um ataque de estouro de buffer se algum software malicioso substituísse a pilha.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Habilite a opção /SAFESEH no comando vinculador ao compilar o seu aplicativo. Essa opção está ativada por padrão nas configurações de Versão do Visual Studio. Verifique se essa opção está habilitada nas instruções de compilação para todos os módulos executáveis do seu aplicativo.

**Comentários**

O teste não é realizado em arquivos binários de 64 bits ou em arquivos binários para o chipset ARM porque eles não armazenam endereços do manipulador de exceções na pilha.

### <a name="span-idbinscope-3spandata-execution-prevention"></a><span id="binscope-3"></span>Prevenção de Execução de Dados

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** Falha no teste NXCheck

Este teste verifica se o seu aplicativo executa código armazenado em um segmento de dados.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Habilite a opção /NXCOMPAT no comando vinculador ao compilar o seu aplicativo. Essa opção está ativada por padrão em versões do vinculador com suporte à Prevenção de Execução de Dados (DEP).

**Comentários**

Recomendamos que você teste os seus aplicativos em uma CPU habilitada para DEP e conserte qualquer falha indicada.

### <a name="span-idbinscope-4spanaddress-space-layout-randomization"></a><span id="binscope-4"></span>ASLR (Address Space Layout Randomization)

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** Falha no teste DBCheck

O ASLR carrega imagens executáveis em locais imprevisíveis da memória, o que dificulta a ação de softwares mal-intencionados que esperam que um programa seja carregado em um determinado endereço virtual para operar de maneira previsível. Seu aplicativo e todos os componentes usados por ele devem oferecer suporte para ASLR.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Habilite a opção /DYNAMICBASE no comando vinculador ao compilar o seu aplicativo. Verifique se todos os módulos usados pelo seu aplicativo usam essa opção do vinculador.

**Comentários**

Normalmente, o ASLR não afeta o desempenho. Mas, em alguns cenários, há um ligeiro aumento do desempenho em sistemas de 32 bits. É possível que o desempenho seja prejudicado em um sistema muito congestionado que possua muitas imagens carregadas em muitos locais diferentes da memória.

Esse teste apenas é realizado em aplicativos gravados em linguagens não gerenciadas, por exemplo, com o uso do C# ou do C++.

### <a name="span-idbinscope-5spanreadwrite-shared-pe-section"></a><span id="binscope-5"></span>Ler/gravar seção PE compartilhada

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** falha no teste SharedSectionsCheck.

Arquivos binários com seções graváveis marcadas como compartilhadas são uma ameaça à segurança. Não compile aplicativos com seções graváveis compartilhadas, a não ser que isso seja realmente necessário. Use [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) ou [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) para criar um objeto de memória compartilhado devidamente protegido.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Remova todas as seções compartilhadas do aplicativo e crie objetos de memória compartilhados chamando [**CreateFileMapping**](https://msdn.microsoft.com/library/windows/desktop/Aa366537) ou [**MapViewOfFile**](https://msdn.microsoft.com/library/windows/desktop/Aa366761) com os devidos atributos de segurança e depois recompile seu aplicativo.

**Comentários**

Esse teste apenas é realizado em aplicativos gravados em linguagens não gerenciadas, por exemplo, com o uso do C# ou do C++.

### <a name="appcontainercheck"></a>AppContainerCheck

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** falha no teste AppContainerCheck.

AppContainerCheck verifica se o bit **appcontainer** no cabeçalho PE de um binário executável está definido. Os aplicativos devem ter o bit **appcontainer** em todos os arquivos .exe e em todas as DLLs não gerenciadas para serem executados corretamente.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Se um arquivo executável nativo for reprovado no teste, verifique se você usou o vinculador e o compilador mais recentes para criar o arquivo e se usou o sinalizador */appcontainer* no vinculador.

Se um executável gerenciado falhar no teste, certifique-se de que você usou o vinculador, como o Microsoft Visual Studio e o compilador mais recentes para criar o aplicativo UWP.

**Comentários**

Esse teste é realizado em todos os arquivos .exe e em todas as DLLs não gerenciadas.

### <a name="span-idbinscope-7spanexecutableimportscheck"></a><span id="binscope-7"></span>ExecutableImportsCheck

**Mensagem de erro do Kit de Certificação de Aplicativos Windows:** falha no teste ExecutableImportsCheck.

Uma imagem PE é reprovada nesse teste quando sua tabela de importação foi inserida em uma seção de código executável. Isso poderá ocorrer se você tiver habilitado a mesclagem de .rdata para a imagem PE, definindo o sinalizador */merge* do vinculador Visual C++ como */merge:.rdata=.text*.

**O que fazer se o seu aplicativo for reprovado pelo teste**

Não mescle a tabela de importação em uma seção de código executável. Verifique se o sinalizador */merge* do vinculador Visual C++ não está definido para mesclar a seção ".rdata" em uma seção de código.

**Comentários**

Esse teste é realizado em todo o código binário, com exceção de assemblies inteiramente gerenciados.

### <a name="span-idbinscope-8spanwxcheck"></a><span id="binscope-8"></span>WXCheck

**Mensagem de erro do Kit de Certificação de Aplicativos para Windows:** Falha no teste WXCheck.

A verificação ajuda a garantir que um binário não tenha nenhuma página mapeada como gravável e executável. Isso pode acontecer se o binário tem uma seção gravável e executável ou se *SectionAlignment* do binário é menor que *PAGE\-SIZE*.

**O que fazer se o seu aplicativo for reprovado pelo teste**

O binário não pode ter uma seção gravável ou executável, e o valor *SectionAlignment* do binário deve ser pelo menos igual ao seu *PAGE\-SIZE*.

**Comentários**

Esse teste é feito em todos os arquivos .exe e em todas as DLLs nativas não gerenciadas.

Um executável pode ter uma seção gravável e executável quando é criado com Editar e Continuar ativado (/ZI). Desativando Editar e Continuar, a seção inválida não está presente.

*PAGE\-SIZE* é o *SectionAlignment* padrão para executáveis.

### <a name="private-code-signing"></a>Assinatura de códigos privados

Testes para a existência de binários de assinatura de código privado no pacote de aplicativo.

### <a name="background"></a>Histórico

Os arquivos de assinatura de código privado devem ser mantidos privados, já que eles podem ser utilizados para fins maliciosos no caso de serem comprometidos.

### <a name="test-details"></a>Detalhes do teste

Os testes de arquivos no pacote de aplicativos que têm uma extensão .pfx ou .snk que indica que as chaves de assinatura privadas foram incluídas.

### <a name="corrective-actions"></a>Ações corretivas

Remova todas as chaves de assinatura de código privado (por exemplo, arquivos .snk e .pfx) do pacote.

## <a name="supported-api-test"></a>Teste de API compatível

Teste o aplicativo em relação ao uso de quaisquer APIs não compatíveis.

### <a name="background"></a>Histórico

Os aplicativos devem usar as APIs para aplicativos UWP (tempo de execução do Windows ou APIs do Win32 com suporte) para ser certificados para a Microsoft Store. Esse teste também identifica as situações em que um binário gerenciado depende de uma função fora do perfil aprovado.

### <a name="test-details"></a>Detalhes do teste

-   Verifica se cada binário no pacote do aplicativo não tem uma dependência em uma API do Win32 que não há suporte para desenvolvimento de aplicativos UWP, verificando a tabela de endereço de importação do binário.
-   Verifica se cada binário gerenciado no pacote do app não depende de uma função fora do perfil aprovado.

### <a name="corrective-actions"></a>Ações corretivas

Verifique se o aplicativo foi compilado como uma compilação de versão e não como uma compilação de depuração.

> **Observação**  A compilação de depuração de um aplicativo falhará nesse teste mesmo se o aplicativo usa [APIs para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx).

Examine as mensagens de erro para identificar a API os usos de aplicativo que não é uma [API para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/xaml/bg124285.aspx).

> **Observação**  Aplicativos C++ integrados em uma configuração de depuração falhará neste teste mesmo se a configuração usar somente APIs do SDK do Windows para aplicativos UWP. Consulte, [alternativas às APIs do Windows em aplicativos UWP](http://go.microsoft.com/fwlink/p/?LinkID=244022) para obter mais informações.

## <a name="performance-tests"></a>Testes de desempenho

O aplicativo deve responder rapidamente à interação do usuário e aos comandos do sistema para proporcionar uma experiência do usuário rápida e flexível.

As características do computador em que o teste é realizado podem afetar os resultados do teste. Os limites do teste de desempenho para a certificação do aplicativo são definidos de modo que os computadores com baixo consumo de energia cumpram as expectativas do cliente de uma experiência rápida e fluida. Para determinar o desempenho do seu aplicativo, recomendamos testá-lo em um computador com baixo consumo de energia, como um computador baseado no processador Intel Atom com resolução de tela de 1366 x 768 (ou superior) e disco rígido giratório (em vez de disco rígido de estado sólido).

### <a name="bytecode-generation"></a>Geração do código de bytes

À medida que a otimização do desempenho acelera o tempo de execução do JavaScript, os arquivos JavaScript com a extensão .js geram código de bytes quando o aplicativo é implantado. Isso melhora consideravelmente o tempo de execução da inicialização e contínuo para operações JavaScript.

### <a name="test-details"></a>Detalhes do teste

Verifica a implantação do aplicativo para verificar se todos os arquivos .js foram convertidos em código de bytes.

### <a name="corrective-action"></a>Ação corretiva

Em caso de falha do teste, considere o seguinte ao lidar com o problema:

-   Verifique se o registro em log de evento está habilitado.
-   Verifique se todos os arquivos JavaScript são sintaticamente válidos.
-   Confirme se todas as versões anteriores do aplicativos foram desinstaladas.
-   Exclua os arquivos identificados no pacote do aplicativo.

### <a name="optimized-binding-references"></a>Referências de vinculação otimizadas

Ao usar vinculações, WinJS.Binding.optimizeBindingReferences deve estar definido como verdadeiro para otimizar o uso da memória.

### <a name="test-details"></a>Detalhes do teste

Verifique o valor de WinJS.Binding.optimizeBindingReferences.

### <a name="corrective-action"></a>Ação corretiva

Defina WinJS.Binding.optimizeBindingReferences como **true** no aplicativo JavaScript.

## <a name="app-manifest-resources-test"></a>Teste de recursos do manifesto do aplicativo

### <a name="app-resources-validation"></a>Validação de recursos do aplicativo

O aplicativo pode não ser instalado se as cadeias de caracteres ou imagens declaradas no manifesto do seu aplicativo estiverem incorretas. Se o aplicativo não for instalado com esses erros, o logotipo do aplicativo ou outras imagens usadas por ele poderão não ser exibidas corretamente.

### <a name="test-details"></a>Detalhes do teste

Inspeciona os recursos definidos no manifesto do aplicativo para garantir que estão presentes e são válidos.

### <a name="corrective-action"></a>Ação corretiva

Use a tabela a seguir como guia.

<table>
<tr><th>Mensagem de erro</th><th>Comentários</th></tr>
<tr><td>
<p>A imagem {image name} define os qualificadores Scale e TargetSize; você pode definir somente um qualificador por vez.</p>
</td><td>
<p>Você pode personalizar imagens para resoluções diferentes.</p>
<p>Na mensagem real, {image name} contém o nome da imagem com o erro.</p>
<p> Verifique se cada imagem define Scale ou TargetSize como o qualificador.</p>
</td></tr>
<tr><td>
<p>Houve falha nas restrições de tamanho da imagem {image name}.</p>
</td><td>
<p>Verifique se todas as imagens do aplicativo atendem às restrições adequadas de tamanho.</p>
<p>Na mensagem real, {image name} contém o nome da imagem com o erro.</p>
</td></tr>
<tr><td>
<p>A imagem {image name} está ausente no pacote.</p>
</td><td>
<p>Uma imagem necessária está ausente.</p>
<p>Na mensagem real, {image name} contém o nome da imagem que está ausente.</p>
</td></tr>
<tr><td>
<p>A imagem {image name} não é um arquivo de imagem válido.</p>
</td><td>
<p>Verifique se todas as imagens do aplicativo atendem às restrições adequadas de tipo de formato de arquivo.</p>
<p>Na mensagem real, {image name} contém o nome da imagem que não é válida.</p>
</td></tr>
<tr><td>
<p>A imagem "BadgeLogo" tem um valor ABGR {value} na posição (x, y) que não é válido. O pixel deve ser branco (##FFFFFF) ou transparente (00######)</p>
</td><td>
<p>O logotipo de selo é uma imagem mostrada ao lado da notificação de selo para identificar o aplicativo na tela de bloqueio. Essa imagem deve ser monocromática (pode conter somente pixels brancos ou transparentes).</p>
<p>Na mensagem real, {value} contém o valor da cor na imagem que não é válido.</p>
</td></tr>
<tr><td>
<p>A imagem "BadgeLogo" tem um valor ABGR {value} na posição (x, y) que não é válido para a imagem branca de alto contraste. O pixel deve ser (##2A2A2A) ou mais escuro, ou transparente (00######).</p>
</td><td>
<p>O logotipo de selo é uma imagem mostrada ao lado da notificação de selo para identificar o aplicativo na tela de bloqueio.   Como o logotipo do selo é mostrado em uma tela de fundo branca quando está em branco de alto contraste, ele deve ser uma versão mais escura do logotipo padrão do selo. No banco de alto contraste, o logotipo do selo pode conter somente pixels que são mais escuros que (##2A2A2A) ou transparentes.</p>
<p>Na mensagem real, {value} contém o valor da cor na imagem que não é válido.</p>
</td></tr>
<tr><td>
<p>A imagem deve definir pelo menos uma variante, sem um qualificador TargetSize. Ela deve definir um qualificador Scale ou deixar Scale e TargetSize não especificados, que tem o padrão Scale-100.</p>
</td><td>
<p>Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/apps/xaml/dn958435.aspx">Design responsivo 101 para aplicativos UWP</a> e <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">Diretrizes para recursos de aplicativos</a>.</p>
</td></tr>
<tr><td>
<p>O pacote tem um arquivo "resources.pri" ausente.</p>
</td><td>
<p>Se você tiver conteúdo localizável no manifesto do aplicativo, verifique se o pacote do aplicativo inclui um arquivo resources.pri válido.</p>
</td></tr>
<tr><td>
<p>O arquivo "resources.pri" deve conter um mapa de recursos com um nome que corresponda ao nome do pacote {package full name}</p>
</td><td>
<p>Você pode obter esse erro quando o manifesto é alterado e o nome do mapa de recursos no resources.pri não corresponde mais ao nome do pacote no manifesto.</p>
<p>Na mensagem real, {package full name} contém o nome do pacote que resources.pri deve conter.</p>
<p>Para corrigir isso, você precisa recompilar o resources.pri e a maneira mais fácil de fazer isso é recompilando o pacote do aplicativo.</p>
</td></tr>
<tr><td>
<p>O arquivo "resources.pri" não deve ter o AutoMerge habilitado.</p>
</td><td>
<p>O MakePRI.exe oferece suporte a uma opção denominada <strong>AutoMerge</strong>. O valor padrão de <strong>AutoMerge</strong> é <strong>desativar</strong>. Quando está habilitado, o <strong>AutoMerge</strong> mescla os recursos de pacote de idiomas do aplicativo em um único resources.pri no tempo de execução. Não recomendamos isso para aplicativos que você pretende distribuir por meio da Microsoft Store. O Resources. PRI de um aplicativo que é distribuído pela Microsoft Store deve estar na raiz do pacote do aplicativo e conter todas as referências de idiomas compatíveis com o aplicativo.</p>
</td></tr>
<tr><td>
<p>A cadeia de caracteres {string} falhou na restrição de comprimento máximo de {number} caracteres.</p>
</td><td>
<p>Consulte os <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">Requisitos do pacote do aplicativo</a>.</p>
<p>Na mensagem real, {string} é substituído pela cadeia de caracteres com o erro e {number} contém o comprimento máximo.</p>
</td></tr>
<tr><td>
<p>A cadeia de caracteres {string} não pode ser espaço inicial/final.</p>
</td><td>
<p>O esquema dos elementos no manifesto do aplicativo não permite caracteres de espaço iniciais ou finais.</p>
<p>Na mensagem real, {string} é substituído pela cadeia de caracteres com o erro.</p>
<p>Verifique se algum dos valores localizados nos campos do manifesto no resources.pri contém caracteres de espaço iniciais ou finais.</p>
</td></tr>
<tr><td>
<p>A cadeia de caracteres não pode estar vazia (o comprimento deve ser maior que zero)</p>
</td><td>
<p>Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt148525.aspx">Requisitos do pacote do aplicativo</a>.</p>
</td></tr>
<tr><td>
<p>Não há recurso padrão especificado no arquivo "resources.pri".</p>
</td><td>
<p>Para obter mais informações, consulte <a href="https://msdn.microsoft.com/library/windows/apps/xaml/hh465241.aspx">Diretrizes de recursos de aplicativos</a>.</p>
<p>Na configuração de compilação padrão, o Visual Studio inclui apenas os recursos de imagem de escala 200 no pacote do aplicativo quando gera pacotes, colocando outros recursos no pacote de recursos. Inclua recursos de imagem de escala 200 ou configure seu projeto para incluir os recursos que você tem.</p>
</td></tr>
<tr><td>
<p>Não há valor de recurso especificado no arquivo "resources.pri".</p>
</td><td>
<p>Verifique se o manifesto do aplicativo tem recursos válidos definidos no resources.pri.</p>
</td></tr>
<tr><td>
<p>O arquivo de imagem {filename} deve ter menos de 204.800 bytes.\*\*</p>
</td><td>
<p>Reduza o tamanho das imagens indicadas.</p>
</td></tr>
<tr><td>
<p>O arquivo {filename} não deve conter uma seção de mapa reverso.\*\*</p>
</td><td>
<p>Apesar do mapa reverso ser gerado durante a "depuração F5" do Visual Studio durante a chamada no makepri.exe, ele pode ser removido. Basta executar makepri.exe sem o parâmetro /m ao gerar um arquivo pri.</p>
</td></tr>
<tr><td colspan="2">
<p>\*\* Indica que um teste foi adicionado no Kit de Certificação de Aplicativos Windows 3.3 para Windows 8.1 e só é aplicável quando se utiliza essa versão do kit ou posterior.</p>
</td></tr>
</table>



 

### <a name="branding-validation"></a>Validação de marca

Aplicativos UWP devem estar completos e totalmente funcionais. Os aplicativos que usam as imagens padrão (de modelos ou exemplos de SDK) apresentam uma experiência do usuário ruim e não podem ser identificados facilmente no catálogo da loja.

### <a name="test-details"></a>Detalhes do teste

O teste valida se as imagens usadas pelo aplicativo não são imagens padrão de exemplos do SDK ou do Visual Studio.

### <a name="corrective-actions"></a>Ações corretivas

Substitua as imagens padrão por algo mais distinto e que representa seu aplicativo.

## <a name="debug-configuration-test"></a>Teste de configuração de depuração

Teste o aplicativo para ter certeza de que ele não é uma compilação de depuração.

### <a name="background"></a>Histórico

Para serem certificados para a Microsoft Store, aplicativos não devem ser compilados para depuração e não devem referenciar versões de depuração de um arquivo executável. Além disso, você deve criar seu código como otimizado para que seu aplicativo passe nesse teste.

### <a name="test-details"></a>Detalhes do teste

Teste o aplicativo para garantir que ele não é uma compilação de depuração e que não está vinculado a estruturas de depuração.

### <a name="corrective-actions"></a>Ações corretivas

-   Crie o aplicativo como uma compilação de versão antes de enviá-lo na Microsoft Store.
-   Verifique se a versão correta do .NET framework está instalada.
-   Certifique-se de que o aplicativo não está vinculando versões de depuração de uma estrutura e se a versão é de liberação. Se o aplicativo contém componentes .NET, certifique-se de instalar a versão correta da estrutura .NET.

## <a name="file-encoding-test"></a>Teste de codificação de arquivo

### <a name="utf-8-file-encoding"></a>Codificação de arquivos UTF-8

### <a name="background"></a>Histórico

Os arquivos HTML, CSS e JavaScript devem estar codificados no formato UTF-8 com a marca de ordem de byte (BOM) correspondente para aproveitar o cache do código de bytes e evitar determinadas condições de erro de tempo de execução.

### <a name="test-details"></a>Detalhes do teste

Teste se o conteúdo dos pacotes de aplicativos para verificar se estão usando a codificação de arquivos correta.

### <a name="corrective-action"></a>Ação corretiva

Abra o arquivo afetado e selecione **Salvar como** no menu **Arquivo** no Visual Studio. Selecione o controle suspenso ao lado do botão **Salvar** e selecione **Salvar com Codificação**. Na caixa diálogo de opções de salvamento **Avançado**, escolha a opção Unicode (UTF-8 com assinatura) e clique em **OK**.

## <a name="direct3d-feature-level-test"></a>Teste do nível de recursos Direct3D

### <a name="direct3d-feature-level-support"></a>Suporte em nível de recursos Direct3D

Testa aplicativos Microsoft Direct3D para garantir que funcionam em todos os dispositivos com hardware gráfico mais antigos.

### <a name="background"></a>Histórico

Microsoft Store exige que todos os aplicativos usando o Direct3D sejam renderizados apropriadamente ou não funcionem em placas de nível 9 \-1 gráfico do recurso.

Como os usuários podem alterar o hardware gráfico de seus dispositivos depois que o aplicativo for instalado, se você escolher um nível mínimo de recursos maior que 9\-1, o aplicativo deverá detectar durante a inicialização se o hardware atual atende ou não aos requisitos mínimos. Se os requisitos mínimos não forem atendidos, o aplicativo deverá exibir uma mensagem para o usuário detalhando os requisitos do Direct3D. Além disso, se um aplicativo for baixado em um dispositivo com o qual ele não é compatível, ele deverá detectar isso na inicialização e exibir uma mensagem para o cliente detalhando os requisitos.

### <a name="test-details"></a>Detalhes do teste

O teste valida se os aplicativos são renderizados com precisão no nível de recursos 9\-1.

### <a name="corrective-action"></a>Ação corretiva

Verifique se o aplicativo renderiza corretamente no recurso nível 9\-1 do Direct3D, mesmo que você espera executar em um nível de recurso superior. Para saber mais, consulte [Desenvolvendo para diferentes níveis de recursos do Direct3D](http://go.microsoft.com/fwlink/p/?LinkID=253575).

### <a name="direct3d-trim-after-suspend"></a>Corte Direct3D após a suspensão

> **Observação**  Esse teste só se aplica a aplicativos UWP desenvolvidos para Windows 8.1 e versões posteriores.

### <a name="background"></a>Histórico

Se o aplicativo não chamar [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) no dispositivo Direct3D, ele não liberará a memória alocada para seu trabalho 3D anterior. Isso aumenta o risco de os aplicativos serem encerrados devido à demanda de memória do sistema.

### <a name="test-details"></a>Detalhes do teste

Verifica a conformidade dos aplicativos com os requisitos d3d e garante que os aplicativos chamem uma nova API [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) após o retorno de chamada no modo suspenso.

### <a name="corrective-action"></a>Ação corretiva

O aplicativo deve chamar a API [**Trim**](https://msdn.microsoft.com/library/windows/desktop/Dn280346) em sua interface [**IDXGIDevice3**](https://msdn.microsoft.com/library/windows/desktop/Dn280345) a qualquer momento que esteja prestes a ser suspenso.

## <a name="app-capabilities-test"></a>Teste de capacidade do aplicativo

### <a name="special-use-capabilities"></a>Funcionalidades de uso especial

### <a name="background"></a>Histórico

As funcionalidades de uso especial destinam-se a cenários bastante específicos. Somente contas empresariais podem usar esses recursos.

### <a name="test-details"></a>Detalhes do teste

Valide se o aplicativo está declarando qualquer uma das capacidades abaixo:

-   EnterpriseAuthentication
-   SharedUserCertificates
-   DocumentsLibrary

Se qualquer uma dessas capacidades for declarada, o teste exibirá um aviso para o usuário.

### <a name="corrective-actions"></a>Ações corretivas

Considere a remoção da funcionalidade de uso especial caso ela não seja necessária ao seu aplicativo. Além disso, o uso dessas funcionalidades está sujeito à análise da política do serviço.

## <a name="windows-runtime-metadata-validation"></a>Validação dos metadados do Windows Runtime

### <a name="background"></a>Histórico

Verifica se os componentes que vêm com o aplicativo são compatíveis com o sistema de tipo UWP.

### <a name="test-details"></a>Detalhes do teste

Verifica se os arquivos **.winmd** no pacote estão em conformidade com as regras UWP.

### <a name="corrective-actions"></a>Ações corretivas

-   **Teste do atributo ExclusiveTo:** assegure-se de que as classes UWP não implementem interfaces marcadas como outra classe ExclusiveTo.
-   **Teste de localização de tipos:** assegure-se de que os metadados de todos os tipos UWP estejam localizados no arquivo winmd que tem o nome correspondente ao namespace mais longo no pacote do aplicativo.
-   **Teste de diferenciação de maiúsculas e minúsculas de nomes de tipos:** verifique se todos os tipos UWP têm nomes exclusivos e sem diferenciação de maiúsculas e minúsculas no pacote do aplicativo. Assegure-se também de que nenhum nome de tipo UWP seja usado como nome de namespace no pacote do aplicativo.
-   **Teste de exatidão de nomes de tipos:** assegure-se de que não haja tipos UWP no namespace global nem no namespace de nível superior do Windows.
-   **Teste de exatidão de metadados gerais:** assegure-se de que o compilador que você está usando para gerar seus tipos esteja atualizado de acordo com as especificações da UWP.
-   **Teste de propriedades:** verifique se todas as propriedades em uma classe UWP têm um método get (os métodos set são opcionais). Verifique se o tipo do valor de retorno do método get corresponde ao tipo do parâmetro de entrada do método set em todas as propriedades em tipos UWP.

## <a name="package-sanity-tests"></a>Testes de integridade do pacote

### <a name="platform-appropriate-files-test"></a>Teste de arquivos apropriados para a plataforma

Os aplicativos que instalam binários mistos podem falhar ou não funcionar corretamente dependendo da arquitetura do processador do usuário.

### <a name="background"></a>Histórico

Este teste valida os binários em um pacote de aplicativo para conflitos de arquitetura. Um pacote de aplicativo não deve incluir os binários que não podem ser utilizados na arquitetura do processador especificado no manifesto. Incluir binários sem suporte pode levar o aplicativo à falhas ou um aumento desnecessário no tamanho do pacote do aplicativo.

### <a name="test-details"></a>Detalhes do teste

Valida se o "número de bit" de cada arquivo no cabeçalho PE é apropriado em caso de referência cruzada com a declaração de arquitetura do processador do pacote do aplicativo

### <a name="corrective-action"></a>Ação corretiva

Siga estas diretrizes para garantir que seu pacote de aplicativos contenha apenas arquivos suportados pela arquitetura especificada no manifesto do aplicativo:

-   Se a Arquitetura do processador alvo para o aplicativo for Tipo de processador neutro, o pacote de aplicativo não pode conter binário x86, x64 ou ARM ou arquivos do tipo imagem.

-   Se a Arquitetura do processador alvo para o aplicativo for tipo de processador x86, o pacote de aplicativo deve conter apenas binário x86 ou arquivos do tipo imagem. Se o pacote contiver binário x64 ou ARM ou tipos de imagem, ele irá falhar no teste.

-   Se a Arquitetura do processador alvo para o aplicativo for tipo de processador x64, o pacote de aplicativo deve conter binário x64 ou arquivos do tipo imagem. Observe que, neste caso, o pacote pode também incluir arquivos x86, mas a experiência aplicativo primário deve utilizar o binário x64.

    No entanto, se a embalagem contiver binário ARM ou arquivos do tipo imagem, ou se contiver apenas binários x86 ou arquivos de tipo de imagem, ele irá falhar no teste.

-   Se a Arquitetura do processador alvo para o aplicativo for tipo de processador ARM, o pacote de aplicativo deve conter apenas binário ARM ou arquivos do tipo imagem. Se o pacote contiver binário x64 ou x86 ou arquivos de tipos de imagem, ele falhará no teste.

### <a name="supported-directory-structure-test"></a>Teste de estrutura de diretório compatível

Valida que os aplicativos não estão criando subdiretórios como parte da instalação e que são maiores do que MAX\-PATH.

### <a name="background"></a>Histórico

Os componentes do sistema operacional (incluindo Trident, WWAHost etc.) são limitados internamente a MAX\-PATH para caminhos do sistema de arquivos e não funcionarão corretamente em caminhos maiores.

### <a name="test-details"></a>Detalhes do teste

Verifica se nenhum caminho no diretório de instalação do aplicativo excede MAX\-PATH.

### <a name="corrective-action"></a>Ação corretiva

Use uma estrutura de diretório ou um nome de arquivo menor.

## <a name="resource-usage-test"></a>Teste do uso de recursos

### <a name="winjs-background-task-test"></a>Teste de tarefa em segundo plano de WinJS

O teste de tarefa em segundo plano WinJS verifica se os aplicativos JavaScript têm as declarações de fechamento apropriadas para que os aplicativos não consumam bateria.

### <a name="background"></a>Histórico

Os aplicativos com testes JavaScript em segundo plano precisam chamar Close() como última declaração na tarefa em segundo plano. Os aplicativos que não executam isso podem impedir que o sistema retorne ao modo de espera conectado e resulte em drenagem da bateria.

### <a name="test-details"></a>Detalhes do teste

Se o aplicativo não tiver um arquivo de tarefa em segundo plano especificado no manifesto, o teste será aprovado. Caso contrário, o teste vai analisar o arquivo JavaScript de tarefa em segundo plano, que é especificado no pacote do aplicativo, e procurar uma instrução Close(). Se a instrução for encontrada, o teste será aprovado; caso contrário, reprovado.

### <a name="corrective-action"></a>Ação corretiva

Atualize o código JavaScript em segundo plano para chamar Close() corretamente.


## <a name="related-topics"></a>Tópicos relacionados

* [Testes de app de Ponte de Desktop do Windows](windows-desktop-bridge-app-tests.md)
* [Políticas da Microsoft Store](https://msdn.microsoft.com/library/windows/apps/Dn764944)
 
