---
author: mtoepke
title: Guia de desenvolvimento de jogos do Windows 10
description: "Um guia completo de recursos e informações para desenvolver jogos da Plataforma Universal do Windows (UWP)."
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
translationtype: Human Translation
ms.sourcegitcommit: a9beb420ac13eb74c0109b30508e49d5305bc67c
ms.openlocfilehash: 30f8408e6d125423e69615a3f9341e8f7d886fc8

---

# Guia de desenvolvimento de jogos do Windows 10


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo morto](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Bem-vindo ao guia de desenvolvimento de jogos do Windows 10!

Este guia fornece uma coleção completa dos recursos e informações necessários para desenvolver um jogo da Plataforma Universal do Windows (UWP).

## Introdução ao desenvolvimento de jogos para a Plataforma Universal do Windows (UWP)


Ao criar um jogo do Windows 10, você tem a oportunidade de alcançar milhões de jogadores no mundo inteiro no telefone, no computador e no Xbox One. Com o Xbox no Windows, o Xbox Live, vários jogadores em vários dispositivos, uma comunidade incrível de jogadores e novos recursos avançados, como a Plataforma Universal do Windows (UWP) e o DirectX 12, os jogos do Windows 10 vão animar jogadores de todas as idades e gêneros. A nova Plataforma Universal do Windows (UWP) oferece compatibilidade para seu jogo em todos os dispositivos Windows 10 com uma API comum para telefone, computador e Xbox One, juntamente com ferramentas e opções para adaptar seu jogo a cada experiência de dispositivo.

Este guia fornece uma coleção completa de informações e recursos que ajudarão você a desenvolver seu jogo. As seções são organizadas de acordo com os estágios do desenvolvimento de jogos, para que você saiba onde procurar as informações quando precisar delas.

Para começar, a seção [Recursos de desenvolvimento de jogos](#resources) fornece uma pesquisa de alto nível de documentação, programas e outros recursos que são úteis para a criação de um jogo.

Este guia será atualizado à medida que recursos e materiais adicionais de desenvolvimento de jogos do Windows 10 se tornarem disponíveis.

## Recursos de desenvolvimento de jogos

De documentação a programas, fóruns, blogs e exemplos para desenvolvedores, há muitos recursos disponíveis para ajudar você em sua jornada de desenvolvimento de jogos. Este é um resumo dos recursos que você deve conhecer para desenvolver seu jogo do Windows 10.

> 
              **Observação**   Os recursos de desenvolvimento do Xbox One e alguns recursos de jogos do Windows 10 (Serviços Xbox Live, por exemplo) são gerenciados por meio de programas como ID@Xbox e Microsoft Studios. Este guia abrange uma ampla variedade de recursos, então, você pode descobrir que alguns recursos não são acessíveis dependendo do programa em que você está ou de sua função de desenvolvimento específica. Os exemplos são links que se resolvem em developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou no Game Developer Network (GDN). Para obter informações sobre parcerias com a Microsoft, consulte [Programas de desenvolvedor](#programs).

### Documentação de desenvolvimento de jogos

Ao longo deste guia, você encontrará links profundos para a documentação relevante, organizados por tarefa, tecnologia e estágio do desenvolvimento de jogos. Para que você tenha uma visão ampla do que está disponível, aqui estão os principais portais de documentação sobre desenvolvimento de jogos do Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portal principal do Centro de Desenvolvimento do Windows</td>
        <td>[Centro de Desenvolvimento do Windows](https://dev.windows.com)</td>
    </tr>
    <tr>
        <td>Desenvolvendo aplicativos do Windows</td>
        <td>[Desenvolver aplicativos do Windows](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>Desenvolvimento de aplicativos da Plataforma Universal do Windows</td>
        <td>[Guias de instruções para aplicativos do Windows 10](https://msdn.microsoft.com/library/windows/apps/mt244352)</td>
    </tr>
    <tr>
        <td>Guias de instruções para jogos UWP</td>
        <td>[Jogos e DirectX](index.md) </td>
    </tr>
    <tr>
        <td>Referência e visões gerais do DirectX</td>
        <td>[Elementos gráficos e jogos do DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Documentação do Xbox Live</td>
        <td>[SDK do Xbox Live](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Documentação do desenvolvedor do Xbox One (GDN)</td>
        <td>[Documentação do XDK do Xbox One](https://developer.xboxlive.com/platform/development/documentation/Pages/home.aspx)</td>
    </tr>
    <tr>
        <td>White papers para desenvolvedores do Xbox One (GDN)</td>
        <td>[White Papers](https://developer.xboxlive.com/platform/development/education/Pages/WhitePapers.aspx)</td>
    </tr>     
</table>

### Programas de desenvolvedor

A Microsoft oferece vários programas de desenvolvedor para ajudar você a desenvolver e publicar jogos do Windows. Para publicar um jogo na Windows Store, você precisará criar uma conta de desenvolvedor no Centro de Desenvolvimento do Windows. Outros programas podem ser de seu interesse, dependendo de suas necessidades de jogo e estúdio, e podem gerar oportunidades, como desenvolvimento para o Xbox One e integração com o Xbox Live.

### Centro de Desenvolvimento do Windows

Registrar uma conta de desenvolvedor no Centro de Desenvolvimento do Windows é a primeira etapa para publicar seu jogo do Windows. Com uma conta de desenvolvedor, você pode reservar o nome de seu jogo e enviar jogos gratuitos e pagos à Windows Store para todos os dispositivos Windows. Use sua conta de desenvolvedor para gerenciar seu jogo e produtos no jogo, obter análises detalhadas e habilitar serviços que criam ótimas experiências para jogadores do mundo inteiro.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Registrar uma conta de desenvolvedor</td>
        <td>[Você está pronto para se inscrever?](https://msdn.microsoft.com/library/windows/apps/bg124287)</td>
    </tr> 
</table>  


### ID@Xbox

O programa ID@Xbox ajuda desenvolvedores de jogos qualificados a autopublicar no Windows e no Xbox One. Se você quiser desenvolver para o Xbox One ou adicionar recursos do Xbox Live, como pontuação, conquistas, placares de líderes, ao seu jogo do Windows 10, inscreva-se no ID@Xbox. Torne-se um desenvolvedor do ID@Xbox para obter as ferramentas e o suporte necessários para soltar sua imaginação e maximizar seu sucesso. Antes de se inscrever no ID@Xbox, registre uma conta de desenvolvedor no Centro de Desenvolvimento do Windows.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programa de desenvolvedor ID@Xbox</td>
        <td>[Programa de desenvolvedor independente para Xbox One](http://go.microsoft.com/fwlink/p/?LinkID=526271)</td>
    </tr>
    <tr>
        <td>Site para consumidores do ID@Xbox</td>
        <td>[ID@Xbox](http://www.idatxbox.com/)</td>
    </tr>
</table>


### Programa de acesso antecipado ao DirectX

Os desenvolvedores de jogos profissionais que quiserem prévias das alterações da API do Direct3D 12 e fornecer comentários nos fóruns podem participar do programa de acesso antecipado ao DirectX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Registre-se no programa de acesso antecipado ao DirectX 12</td>
        <td>[Programa de acesso antecipado ao DirectX](http://1drv.ms/1dgelm6)</td>
    </tr>
</table>


### Ferramentas e middleware do Xbox

O programa de ferramentas e middleware do Xbox licencia kits de desenvolvimento do Xbox para desenvolvedores profissionais de ferramentas e middleware de jogos. Os desenvolvedores aceitos no programa podem compartilhar e distribuir seus tecnologias do XDK do Xbox para outros desenvolvedores licenciados do Xbox.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Entre em contato com o programa de ferramentas e middleware</td>
        <td><xboxtlsm@microsoft.com></td>
    </tr>
</table>


### Exemplos de jogos

Há muitas amostras de jogos e aplicativos do Windows 10 disponíveis para ajudar você a entender os recursos de jogos do Windows 10 e começar rapidamente a desenvolver jogos. Mais amostras são desenvolvidas e publicadas regularmente. Por isso, não se esqueça de revisitar ocasionalmente os portais de amostras para conferir as novidades. Você também pode [acompanhar](https://help.github.com/articles/watching-repositories/) os repositórios GitHub para receber notificações sobre alterações e adições.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Exemplos de aplicativos da Plataforma Universal do Windows</td>
        <td>[Windows-universal-samples](https://github.com/Microsoft/Windows-universal-samples)</td>
    </tr>
    <tr>
        <td>Exemplos de elementos gráficos Direct3D 12</td>
        <td>[DirectX-Graphics-Samples](https://github.com/Microsoft/DirectX-Graphics-Samples)</td>
    </tr>
    <tr>
        <td>Amostra de jogo em primeira pessoa do Direct3D 11</td>
        <td>[Criar um jogo UWP simples com o DirectX](tutorial--create-your-first-metro-style-directx-game.md)</td>
    </tr>
    <tr>
        <td>Amostra de efeitos de imagem personalizados do Direct2D</td>
        <td>[D2DCustomEffects](http://go.microsoft.com/fwlink/p/?LinkId=620531)</td>
    </tr>
    <tr>
        <td>Amostra de malha de gradiente do Direct2D</td>
        <td>[D2DGradientMesh](http://go.microsoft.com/fwlink/p/?LinkId=620532)</td>
    </tr>
    <tr>
        <td>Amostra de ajuste de fotos do Direct2D</td>
        <td>[D2DPhotoAdjustment](http://go.microsoft.com/fwlink/p/?LinkId=620533)</td>
    </tr>
    <tr>
        <td>Exemplos de jogos do Xbox One (GDN)</td>
        <td>[Exemplos](https://developer.xboxlive.com/platform/development/education/Pages/Samples.aspx)</td>
    </tr>
    <tr>
        <td>Exemplos de jogos do Windows 8 (Galeria de Códigos do MSDN)</td>
        <td>[Exemplos de jogos da Windows Store](https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft)</td>
    </tr>
    <tr>
        <td>Amostra de jogo em JavaScript e HTML5</td>
        <td>[Amostra de jogo por toque em JavaScript e HTML5](https://code.msdn.microsoft.com/windowsapps/JavaScript-and-HTML5-touch-d96f6031)</td>
    </tr>      
</table>


### Fóruns para desenvolvedores

Os fóruns de desenvolvedores são um ótimo lugar para fazer e responder perguntas sobre desenvolvimento de jogos e conectar-se com a comunidade de desenvolvimento de jogos. Os fóruns também podem ser recursos fantásticos para encontrar respostas existentes para problemas difíceis que os desenvolvedores encontraram e resolveram no passado.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Fóruns de desenvolvedores de aplicativos do Windows</td>
        <td>[Fóruns de aplicativos e Windows Store](https://social.msdn.microsoft.com/Forums/home?category=windowsapps)</td>
    </tr>
    <tr>
        <td>Fórum do desenvolvedor de aplicativos UWP</td>
        <td>[Desenvolvendo aplicativos da Plataforma Universal do Windows](https://social.msdn.microsoft.com/Forums/home?forum=wpdevelop)</td>
    </tr>

    <tr>
        <td>Fóruns de desenvolvedores de aplicativos da área de trabalho</td>
        <td>[Fóruns de aplicativos da área de trabalho do Windows](https://social.msdn.microsoft.com/Forums/home?category=windowsdesktopdev)</td>
    </tr>
    <tr>
        <td>Jogos da Windows Store com DirectX (postagens arquivadas no fórum)</td>
        <td>[Criando jogos da Windows Store com DirectX (arquivado)](https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx)</td>
    </tr>
    <tr>
        <td>Fóruns de desenvolvedores parceiros gerenciados do Windows 10</td>
        <td>[Fóruns de desenvolvedores XBOX: Windows 10](http://aka.ms/win10devforums)</td>
    </tr>
    <tr>
        <td>Fóruns do programa de acesso antecipado ao DirectX</td>
        <td>[Fórum do DirectX 12](http://directx12forum.azurewebsites.net/index.php)</td>
    </tr>
</table>


### Blogs de desenvolvedores

Os blogs de desenvolvedores são outro recurso excelente para as informações mais recentes sobre o desenvolvimento de jogos. Você encontrará postagens sobre novos recursos, detalhes de implementação, práticas recomendadas, histórico de arquitetura e muito mais.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Blog Criando aplicativos para Windows</td>
        <td>[Criando aplicativos para Windows](http://blogs.windows.com/buildingapps/)</td>
    </tr>
    <tr>
        <td>Windows 10 (postagens de blog)</td>
        <td>[Postagens no Windows 10](http://blogs.windows.com/blog/tag/windows-10/)</td>
    </tr>
    <tr>
        <td>Blog da equipe de engenharia do Visual Studio</td>
        <td>[Blog do Visual Studio](http://blogs.msdn.com/b/visualstudio/)</td>
    </tr>
    <tr>
        <td>Blogs de ferramentas de desenvolvedor do Visual Studio</td>
        <td>[Blogs de ferramentas de desenvolvedor](http://blogs.msdn.com/b/developer-tools/)</td>
    </tr>
    <tr>
        <td>Blog de ferramentas de desenvolvedor do Somasegar</td>
        <td>[Blog do Somasegar](http://blogs.msdn.com/b/somasegar/)</td>
    </tr>
    <tr>
        <td>Blog de desenvolvedores do DirectX</td>
        <td>[Blog de desenvolvedores do DirectX](http://blogs.msdn.com/b/directx)</td>
    </tr>
    <tr>
        <td>Introdução ao DirectX 12 (postagem de blog)</td>
        <td>[DirectX 12](http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx)</td>
    </tr>
    <tr>
        <td>Blog da equipe de ferramentas do Visual C++</td>
        <td>[Blog da equipe do Visual C++](http://blogs.msdn.com/b/vcblog/)</td>
    </tr>
    <tr>
        <td>Blog de desenvolvedores do ID@Xbox</td>
        <td>[Blog de desenvolvedores do ID@Xbox](http://www.idatxbox.com/category/developer-blog/)</td>
    </tr>
</table>
 

## Conceito e planejamento


Na fase de conceito e planejamento, você decide como será seu jogo e as tecnologias e ferramentas que você usará para dar vida a ele.

### Visão geral das tecnologias de desenvolvimento de jogos

Quando você começa a desenvolver um jogo para a UWP, existem várias opções disponíveis de elementos gráficos, entrada, áudio, rede, utilitários e bibliotecas.

Se você já decidiu quais tecnologias você usará em seu jogo, ótimo! Caso contrário, o guia [Tecnologias de jogos para aplicativos UWP](game-development-platform-guide.md) é uma excelente visão geral de muitas das tecnologias disponíveis, e é altamente recomendável que você leia esse guia para entender melhor as opções e como elas se complementam.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Pesquisa de tecnologias de jogos UWP</td>
        <td>[Tecnologias de jogos para aplicativos UWP](game-development-platform-guide.md)</td>
    </tr>
</table>
 

Estes três vídeos da GDC 2015 fornecem uma boa visão geral do desenvolvimento e da experiência de jogos do Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral do desenvolvimento de jogos do Windows 10 (vídeo)</td>
        <td>[Desenvolvendo jogos para Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Experiência de jogos do Windows 10 (vídeo)</td>
        <td>[Experiência de consumidor de jogos no Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10)</td>
    </tr>
    <tr>
        <td>Jogos no ecossistema da Microsoft (vídeo)</td>
        <td>[O futuro dos jogos no ecossistema da Microsoft](http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem)</td>
    </tr>
</table>

### Planejamento de jogo

Estes são alguns tópicos de conceito e planejamento de nível alto a serem considerados ao planejar seu jogo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Tornando seu jogo acessível</td>
        <td>[Acessibilidade para jogos](https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games)</td>
    </tr>
    <tr>
        <td>Usando a nuvem para jogos</td>
        <td>[Nuvem para jogos](https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games)</td>
    </tr>
</table>



### Escolhendo a tecnologia de elementos gráficos e a linguagem de programação

Há várias tecnologias gráficas e linguagens de programação disponíveis para os jogos do Windows 10. Seu caminho dependerá do tipo de jogo que está desenvolvendo, da experiência e das preferências de seu estúdio de desenvolvimento e dos requisitos de recursos específicos do seu jogo. Você usará o C#, C++ ou JavaScript? DirectX, XAML ou HTML5?

#### DirectX

O Microsoft DirectX é a opção certa para criar elementos gráficos e multimídia 2D e 3D com maior desempenho. 

O Direct3D 12, uma novidade do Windows 10, oferece a eficiência de uma API tipo console e está mais rápido e mais eficiente do que nunca. Seu jogo pode fazer uso total do hardwares gráfico moderno e apresentar mais objetos, cenas mais ricas e efeitos avançados. O Direct3D 12 fornece elementos gráficos otimizados em computadores Windows 10 e Xbox One. Se desejar usar o pipeline de gráfico familiar do Direct3D 11, você ainda aproveitará os novos recursos de renderização e otimização do Direct3D 11.3. E, se você for um desenvolvedor consagrado da API do Windows para área de trabalho com origem no Win32, continuará tendo essa opção no Windows 10.

Os vários recursos e a profunda integração da plataforma do DirectX possibilitam a eficiência e o desempenho necessários para os jogos mais exigentes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guias de instruções para jogos do DirectX</td>
        <td>[Jogos e DirectX](index.md)</td>
    </tr>
    <tr>
        <td>Referência e visões gerais do DirectX</td>
        <td>[Elementos gráficos e jogos do DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Guia e referência de programação do Direct3D 12</td>
        <td>[Elementos gráficos do Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>Vídeos sobre elementos gráficos e desenvolvimento com o DirectX 12 (canal do YouTube)</td>
        <td>[Microsoft DirectX 12 and Graphics Education](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
</table>
 

#### XAML

O XAML é uma linguagem de interface do usuário declarativa fácil de usar com recursos convenientes, como animações, storyboards, vinculação de dados, elementos gráficos vetoriais escalonáveis, redimensionamento dinâmico e gráficos de cena. O XAML funciona muito bem em interfaces do usuário, menus, sprites e elementos gráficos 2D de jogos. Para facilitar o layout da interface do usuário, o XAML é compatível com ferramentas de design e desenvolvimento como Expression Blend e Microsoft Visual Studio. O XAML costuma ser usado com o C#, mas o C++ também é uma boa opção, caso seja sua linguagem preferida ou se o jogo tiver grande demanda de CPU.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral da plataforma XAML</td>
        <td>[Plataforma XAML](https://msdn.microsoft.com/library/windows/apps/mt228259)</td>
    </tr>
    <tr>
        <td>Interface do usuário e controles XAML</td>
        <td>[Controles, layouts e texto](https://msdn.microsoft.com/library/windows/apps/mt228348)</td>
    </tr>
</table>
 

#### HTML 5

O HTML (HyperText Markup Language) é uma linguagem de marcação da interface do usuário comum usada em páginas da web, aplicativos e clientes sofisticados. Os jogos do Windows podem usar HTML5 como uma camada de apresentação com todos os recursos familiares do HTML, acesso à Plataforma Universal do Windows e suporte a recursos modernos da Web, como AppCache, Web Workers, canvas, arrastar e soltar, programação assíncrona e SVG. Nos bastidores, a renderização do HTML aproveita a capacidade de aceleração de hardware do DirectX. Assim, você obtém os benefícios de desempenho do DirectX sem criar nenhum código extra. O HTML5 é uma boa opção quando você tem experiência com o desenvolvimento da Web, quando está portando um jogo da Web ou se quiser usar uma linguagem e camadas gráficas mais fáceis de abordar do que nos outras opções. O HTML5 é usado com JavaScript, mas também pode chamar componentes criados em C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informações sobre modelos de objetos HTML5 e de documento</td>
        <td>[Referência de HTML e DOM](https://msdn.microsoft.com/library/windows/apps/br212882.aspx)</td>
    </tr>
    <tr>
        <td>Recomendação de HTML5 do W3C</td>
        <td>[HTML5](http://go.microsoft.com/fwlink/p/?linkid=221374)</td>
    </tr>
</table>
 

#### Combinando tecnologias de apresentação

O Microsoft DirectX Graphics Infrastructure (DXGI) oferece interoperabilidade e compatibilidade entre várias tecnologias gráficas. Para elementos gráficos de alto desempenho, você pode combinar XAML e DirectX, usando XAML para menus e outras interfaces do usuário simples e DirectX para a renderização de cenas 2D e 3D complexas. A DXGI também fornece compatibilidade entre Direct2D, Direct3D, DirectWrite, DirectCompute e Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia de programação e referência do DirectX Graphic Infrastructure</td>
        <td>[DXGI](https://msdn.microsoft.com/library/windows/desktop/hh404534)</td>
    </tr>
    <tr>
        <td>Combinando DirectX e XAML</td>
        <td>[Interoperabilidade entre DirectX e XAML](directx-and-xaml-interop.md)</td>
    </tr>
</table>
 

#### C++

O C++/CX é uma linguagem de baixa sobrecarga e alto desempenho que oferece a poderosa combinação de velocidade, compatibilidade e acesso à plataforma. Com o C++/CX, é fácil usar todos os excelentes recursos de jogo no Windows 10, inclusive o DirectX e o Xbox Live. Você também pode reutilizar o código C++ e as bibliotecas existentes. O C++/CX cria código nativo rápido que não incorre na sobrecarga de coleta de lixo para que seu jogo possa ter um ótimo desempenho e baixo consumo de energia, fazendo com que a bateria dure mais. Use o C++/CX com DirectX ou XAML, orou crie um jogo usando uma combinação dos dois.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referência e visões gerais da linguagem C++/CX</td>
        <td>[Referência da linguagem Visual C++ (C++/CX)](https://msdn.microsoft.com/library/windows/apps/hh699871.aspx)</td>
    </tr>
    <tr>
        <td>Guia e referência de programação do Visual C++</td>
        <td>[Visual C++ no Visual Studio 2015](https://msdn.microsoft.com/library/60k1461a.aspx)</td>
    </tr>
</table>
 

#### C#

O C# (fala-se "C sharp") é uma linguagem inovadora e moderna, simples, eficiente, fortemente tipada e orientada a objeto. O C# permite um desenvolvimento rápido, pois mantém a familiaridade e expressividade das linguagens no estilo C. Embora seja fácil de usar, o C# tem vários recursos de linguagem avançados, como polimorfismo, representantes, lambdas, fechamentos, métodos de iterador, covariância e expressões de consulta integrada à linguagem (LINQ). O C# é uma excelente opção se você está visando o XAML, deseja começar a desenvolver seu jogo rapidamente ou tem experiência anterior em C#. O C# é usado principalmente com XAML. Portanto, para usar DirectX, escolha C++, ou crie parte do seu jogo como um componente C++ que interaja com DirectX. Ou considere o [Win2D](https://github.com/Microsoft/Win2D), uma biblioteca de elementos gráficos em modo imediato Direct2D para C# e C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia e referência de programação em C#</td>
        <td>[Referência da linguagem C#](https://msdn.microsoft.com/library/kx37x362.aspx)</td>
    </tr>
</table>
 

#### JavaScript

O JavaScript é uma linguagem de scripts dinâmica amplamente usada em aplicativos cliente avançados e da Web moderna.

Os aplicativos do Windows em JavaScript podem acessar os recursos avançados da Plataforma Universal do Windows de forma fácil e intuitiva, como métodos e propriedades de classes de JavaScript orientadas a objeto. O JavaScript é uma boa opção para seu jogo quando você vem de um ambiente de desenvolvimento da Web, já está familiarizado com o JavaScript ou deseja usar as bibliotecas de HTML5, CSS, WinJS ou JavaScript. Se quiser usar o DirectX ou XAML, escolha o C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referência de JavaScript e do Windows Runtime</td>
        <td>[Referência de JavaScript](https://msdn.microsoft.com/library/windows/apps/jj613794)</td>
    </tr>
</table>


#### Usar os componentes do Windows Runtime para combinar linguagens

Com a Plataforma Universal do Windows, é fácil combinar componentes criados em linguagens diferentes. Crie componentes do Windows Runtime em C++, C# ou Visual Basic, e os chame do JavaScript, C#, C++ ou Visual Basic. Essa é uma ótima maneira de programar partes do jogo na linguagem de sua escolha. O componentes também permitem o uso de bibliotecas externas disponíveis somente em uma determinada linguagem, além do código herdado que você já criou.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Como criar componentes do Windows Runtime</td>
        <td>[Criando componentes do Windows Runtime](https://msdn.microsoft.com/library/windows/apps/hh441572.aspx)</td>
    </tr>
</table>


### Que versão do DirectX seu jogo deve usar?

Se você escolher o DirectX para seu jogo, precisará decidir qual versão usar: Microsoft Direct3D 12 ou Microsoft Direct3D 11.

O Direct3D 12, uma novidade do Windows 10, oferece a eficiência de uma API tipo console e está mais rápido e mais eficiente do que nunca. Seu jogo pode fazer uso total do hardwares gráfico moderno e apresentar mais objetos, cenas mais ricas e efeitos avançados. O Direct3D 12 fornece elementos gráficos otimizados em computadores Windows 10 e Xbox One. Como Direct3D 12 funciona em um nível muito baixo, é possível dar a uma equipe de desenvolvimento de elementos gráficos especializada ou a uma equipe de desenvolvimento com o DirectX 11 experiente o controle que precisam para maximizar a otimização dos elementos gráficos.

O Direct3D 11.3 é uma API de elemento gráfico de baixo nível que usa o modelo de programação Direct3D e manipula grande parte da complexidade envolvida na renderização de GPU. Ela também é compatível com o Windows 10 e o Xbox One. Se você tem um mecanismo escrito em Direct3D 11 mas ainda não está pronto para passar ao Direct3D 12, você pode usar o Direct3D 11 sobre o 12 para obter algumas melhorias de desempenho. As Versões 11.3+ contêm os novos recursos de renderização e otimização habilitados também no Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Escolhendo o Direct3D 12 ou o Direct3D 11</td>
        <td>[O que é Direct3D 12?](https://msdn.microsoft.com/library/windows/desktop/dn899228)</td>
    </tr>
    <tr>
        <td>Visão geral do Direct3D 11</td>
        <td>[Elementos gráficos do Direct3D 11](https://msdn.microsoft.com/library/windows/desktop/ff476080)</td>
    </tr>
    <tr>
        <td>Visão geral do Direct3D 11 on 12</td>
        <td>[Direct3D 11 on 12](https://msdn.microsoft.com/library/windows/desktop/dn913195)</td>
    </tr>
</table>


### Pontes, mecanismos de jogos e middleware

Dependendo das necessidades de seu jogo, usar pontes, mecanismos de jogos ou middleware pode poupar tempo e recursos de desenvolvimento e teste. Aqui estão alguns recursos e visões gerais de pontes, mecanismos de jogos e middleware para ajudar você a decidir se algum deles é ideal para seu projeto.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Pontes e mecanismos de jogos para Windows 10 (postagem de blog)</td>
        <td>[Mais maneiras de trazer seu código para a Loja em rápida expansão do Windows 10](http://blogs.windows.com/buildingapps/2015/09/17/more-ways-to-bring-your-code-to-fast-growing-windows-10-store/)</td>
    </tr>
    <tr>
        <td>Desenvolvimento de jogos com middleware (vídeo)</td>
        <td>[Acelerando o desenvolvimento de jogos da Windows Store com o middleware](https://channel9.msdn.com/Events/Build/2013/3-187)</td>
    </tr>
    <tr>
        <td>Visual Studio e Unity, Unreal e Cocos2d (postagem de blog)</td>
        <td>[Visual Studio para desenvolvimento de jogos: novas parcerias com Unity, Unreal Engine e Cocos2d](http://blogs.msdn.com/b/somasegar/archive/2015/04/17/visual-studio-for-game-development-new-partnerships-with-unity-unreal-engine-and-cocos2d.aspx)</td>
    </tr>
    <tr>
        <td>Introdução ao middleware de jogos (postagem de blog)</td>
        <td>[Middleware de desenvolvimento de jogos – o que é isso? Preciso disso?](http://blogs.msdn.com/b/wsdevsol/archive/2014/05/02/game-development-middleware-what-is-it-do-i-need-it.aspx)</td>
    </tr>
</table>
 

#### Pontes da Plataforma Universal do Windows

Pontes de Plataforma Universal do Windows são tecnologias que levam seu aplicativo ou jogo existente para a UWP. Pontes são uma ótima maneira de iniciar rapidamente o desenvolvimento de jogos na UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Pontes UWP</td>
        <td>[Traga seu código para o Windows](https://dev.windows.com/bridges/)</td>
    </tr>
    <tr>
        <td>Ponte do Windows para iOS</td>
        <td>[Traga seu aplicativo iOS para o Windows](https://dev.windows.com/bridges/ios)</td>
    </tr>
    <tr>
        <td>Prévia do Ponte do Windows para .NET e Win32 ("Project Centennial")</td>
        <td>[Programas Windows Developer Preview](http://go.microsoft.com/fwlink/p/?LinkID=624543)</td>
    </tr>
</table>
 

#### Unity

O Unity 5 representa a próxima geração da premiada plataforma de desenvolvimento para a criação de jogos e experiências interativas em 2D e 3D. O Unity 5 reúne capacidade artística renovada, recursos gráficos aprimorados e melhor eficiência.

No [Unity roadmap](https://unity3d.com/unity/roadmap), o suporte para o DirectX 12 estará disponível em uma versão futura do Unity.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Mecanismo de jogos Unity</td>
        <td>[Unity – Mecanismo de jogos](http://unity3d.com/)</td>
    </tr>
    <tr>
        <td>Obter o Unity 5</td>
        <td>[Obtenha o Unity](http://unity3d.com/get-unity)</td>
    </tr>
    <tr>
        <td>Suporte para aplicativos da Plataforma Universal do Windows no Unity 5.2 (postagem de blog)</td>
        <td>[Aplicativos da plataforma universal do Windows 10 no Unity 5.2](http://blogs.unity3d.com/2015/09/09/windows-10-universal-apps-in-unity-5-2/)</td>
    </tr>
    <tr>
        <td>Documentação do Unity para Windows</td>
        <td>[Manual do Unity / Windows](http://docs.unity3d.com/Manual/Windows.mdl)</td>
    </tr>
    <tr>
        <td>Publicar seu jogo do Unity como um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td>[Como publicar seu jogo do Unity como um aplicativo UWP](https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app)</td>
    </tr>
    <tr>
        <td>Usar o Unity para criar aplicativos e jogos do Windows (vídeo)</td>
        <td>[Criar jogos e aplicativos do Windows com o Unity](https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity)</td>
    </tr>
    <tr>
        <td>Desenvolvimento de jogos Unity usando o Visual Studio (série de vídeos)</td>
        <td>[Usando o Unity com o Visual Studio 2015](http://go.microsoft.com/fwlink/?LinkId=722359)</td>
    </tr>
</table>
 

#### Havok

O pacote modular de ferramentas e tecnologias do Havok ajuda criadores de jogos a alcançar novos níveis de interatividade e imersão. O Havok oferece física altamente realista, simulações interativas e uma cinemática incrível.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Site do Havok</td>
        <td>[Havok](http://www.havok.com/)</td>
    </tr>
    <tr>
        <td>Pacote de ferramentas Havok</td>
        <td>[Visão geral do produto Havok](http://www.havok.com/products/)</td>
    </tr>
    <tr>
        <td>Fóruns de suporte do Havok</td>
        <td>[Havok](https://software.intel.com/forums/havok/)</td>
    </tr>
</table>
 

#### Cocos2d

Cocos2d-X é um pacote de ferramentas e mecanismos de desenvolvimento de jogos de software livre e plataforma cruzada que oferece suporte à criação de jogos UWP. A partir da versão 3, recursos 3D também estão sendo adicionados.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td>[O que é Cocos2d-X?](http://www.cocos2d-x.org/)</td>
    </tr>
    <tr>
        <td>Guia do programador do Cocos2d-x</td>
        <td>[Guia para programadores do Cocos2d-x v3.8](http://www.cocos2d-x.org/programmersguide/)</td>
    </tr>
    <tr>
        <td>Cocos2d-x no Windows 10 (postagem de blog)</td>
        <td>[Executando o Cocos2d-x no Windows 10](https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/)</td>
    </tr>
    <tr>
        <td>Jogos da Windows Store em Cocos2d-x (vídeo)</td>
        <td>[Criar um jogo com o Cocos2d-x para dispositivos Windows](http://www.microsoftvirtualacademy.com/training-courses/build-a-game-with-cocos2d-x-for-windows-devices)</td>
    </tr>
</table>


#### Unreal Engine

O Unreal Engine 4 é um conjunto completo de ferramentas de desenvolvimento de jogos para todos os tipos de jogos e desenvolvedores. Para os jogos de console e de computador mais exigentes, o Unreal Engine é usado por desenvolvedores de jogos em todo o mundo. 
              Os membros do [Programa de acesso antecipado ao DirectX 12](#dxeap) que se inscrevem no Unreal Engine 4 podem receber acesso a um projeto de desenvolvimento Unreal Engine 4.4 que oferece suporte ao DirectX 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral do Unreal Engine</td>
        <td>[O que é o Unreal Engine 4](https://www.unrealengine.com/what-is-unreal-engine-4)</td>
    </tr>
</table>
 

### Middleware e parceiros

Há muitos outros parceiros de middleware e mecanismos que podem fornecer soluções dependendo de suas necessidades de desenvolvimento de jogos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Parceiros de jogos do Centro de Desenvolvimento do Windows</td>
        <td>[Parceiros do Centro de Desenvolvimento (jogos)](https://devcenterpartners.windows.com/directory#filter=gaming)</td>
    </tr>
    <tr>
        <td>Parceiros do Centro de Desenvolvimento do Windows</td>
        <td>[Parceiros do Centro de Desenvolvimento](https://devcenterpartners.windows.com/directory)</td>
    </tr>
</table>
 

### Fazendo a portabilidade de seu jogo

Se você tiver um jogo existente, há muitos recursos e guias disponíveis para lhe ajudar a trazer seu jogo rapidamente para a UWP. Para impulsionar seus esforços de portabilidade, você também pode usar as [Ponte da Plataforma Universal do Windows](#uwp_bridges).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo do Windows 8 para um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td>[Mudar do Windows Runtime 8.x para a UWP](https://msdn.microsoft.com/library/windows/apps/mt238322)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo do Windows 8 para um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td>[Fazendo a portabilidade de aplicativos 8.1 para o Windows 10](https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo iOS para um aplicativo da Plataforma Universal do Windows</td>
        <td>[Mudar do iOS para a UWP](https://msdn.microsoft.com/library/windows/apps/mt238320)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo Silverlight para um aplicativo da Plataforma Universal do Windows</td>
        <td>[Mudar do Windows Phone Silverlight para a UWP](https://msdn.microsoft.com/library/windows/apps/mt238323)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do XAML ou Silverlight para um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td>[Fazendo a portabilidade de um aplicativo XAML ou Silverlight para Windows 10](https://channel9.msdn.com/Events/Build/2015/3-741)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um jogo do Xbox para um aplicativo da Plataforma Universal do Windows</td>
        <td>[Fazendo a portabilidade do Xbox One para a UWP do Windows 10](https://developer.xboxlive.com/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do DirectX 9 para o DirectX 11</td>
        <td>[Portar do DirectX 9 para a Plataforma Universal do Windows (UWP)](porting-your-directx-9-game-to-windows-store.md)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do Direct3D 11 para o Direct3D 12</td>
        <td>[Fazendo a portabilidade do Direct3D 11 para o Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709)</td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do OpenGL ES para o Direct3D 11</td>
        <td>[Fazer a portabilidade do OpenGL ES 2.0 para o Direct3D 11](port-from-opengl-es-2-0-to-directx-11-1.md)</td>
    </tr>
    <tr>
        <td>OpenGL ES para Direct3D 11 usando o ANGLE</td>
        <td>[ANGLE](http://go.microsoft.com/fwlink/p/?linkid=618387)</td>
    </tr>
    <tr>
        <td>Equivalentes das APIs clássicas do Windows na UWP</td>
        <td>[Alternativas a APIs do Windows em aplicativos da Plataforma Universal do Windows (UWP)](https://msdn.microsoft.com/library/windows/apps/hh464945)</td>
    </tr>
</table>


## Protótipo e design


Agora que você decidiu o tipo de jogo que deseja criar e as ferramentas e a tecnologia de elementos gráficos que usará para criá-lo, você está pronto para começar a trabalhar no design e no protótipo. Essencialmente, seu jogo é um aplicativo da Plataforma Universal do Windows. Portanto, é por aí que você vai começar.

### Introdução à Plataforma Universal do Windows (UWP)

O Windows 10 apresenta a Plataforma Universal do Windows (UWP), que fornece uma plataforma de API comuns em dispositivos Windows 10. A UWP evolui e expande o modelo do Windows Runtime e o transforma em um núcleo coeso e unificado. Os jogos destinados à UWP podem chamar APIs do WinRT que são comuns a todos os dispositivos. Como a UWP fornece uma camada de API essencial garantida, você pode optar por criar um único pacote de aplicativo que será instalado em dispositivos Windows 10. E se você quiser, seu jogo ainda pode chamar APIs (incluindo algumas APIs clássicas do Windows, do Win32 e .NET) que são específicas para os dispositivos nos quais seu jogo funciona.

O objetivo da UWP é ter:

-   Um sistema operacional principal
-   Uma plataforma de aplicativo
-   Uma rede social de jogos
-   Uma loja
-   Um caminho de ingestão

As referências a seguir são guias excelentes que explicam em detalhes os aplicativos da Plataforma Universal do Windows, e sua leitura é recomendada para ajudar você a entender a plataforma.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introdução a aplicativos da Plataforma Universal do Windows</td>
        <td>[O que é um aplicativo da Plataforma Universal do Windows?](https://msdn.microsoft.com/library/windows/apps/dn726767)</td>
    </tr>
    <tr>
        <td>Visão geral da UWP</td>
        <td>[Guia para aplicativos UWP](https://msdn.microsoft.com/library/windows/apps/dn894631)</td>
    </tr>
</table>
 

### Introdução ao desenvolvimento da UWP

É fácil e rápido se preparar para desenvolver um aplicativo da Plataforma Universal do Windows. Os guias a seguir explicarão o processo passo a passo para você.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introdução ao desenvolvimento da UWP</td>
        <td>[Introdução aos aplicativos do Windows](https://dev.windows.com/getstarted)</td>
    </tr>
    <tr>
        <td>Preparando-se para o desenvolvimento na UWP</td>
        <td>[Prepare-se](https://msdn.microsoft.com/library/windows/apps/dn726766)</td>
    </tr>
</table>

Se você é um "iniciante" na programação da UWP e está considerando usar XAML no seu jogo (consulte [Escolhendo a tecnologia de elementos gráficos e a linguagem de programação](#choosing_technology)), a série de vídeos sobre [desenvolvimento do Windows 10 para iniciantes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) é um bom ponto de partida.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia para iniciantes no desenvolvimento do Windows 10 com XAML (série de vídeos)</td>
        <td>[Desenvolvimento do Windows 10 para iniciantes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners)</td>
    </tr>
    <tr>
        <td>Anunciando a série para iniciantes do Windows 10 usando XAML (postagem de blog)</td>
        <td>[Desenvolvimento do Windows 10 para iniciantes](http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/)</td>
    </tr>
</table>

### Conceitos de desenvolvimento da UWP

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral do desenvolvimento de aplicativos da Plataforma Universal do Windows</td>
        <td>[Desenvolver aplicativos do Windows](https://dev.windows.com/develop)</td>
    </tr>
    <tr>
        <td>Visão geral da programação de rede na UWP</td>
        <td>[Serviços de rede e Web](https://msdn.microsoft.com/library/windows/apps/mt280378)</td>
    </tr>
    <tr>
        <td>Usando Windows.Web.HTTP e Windows.Networking.Sockets em jogos</td>
        <td>[Rede para jogos](work-with-networking-in-your-directx-game.md)</td>
    </tr>
    <tr>
        <td>Conceitos de programação assíncrona na UWP</td>
        <td>[Programação assíncrona](https://msdn.microsoft.com/library/windows/apps/mt187335)</td>
    </tr>
</table>
 

### Gerenciamento do tempo de vida do processo

Gerenciamento do tempo de vida do processo, ou ciclo de vida do aplicativo, descreve os vários estados de ativação pelos quais um aplicativo da Plataforma Universal do Windows pode passar. Seu jogo pode ser ativado, suspenso, retomado ou encerrado e pode passar por esses estados em uma variedade de maneiras.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Manipulando transições do ciclo de vida do aplicativo</td>
        <td>[Ciclo de vida do aplicativo](https://msdn.microsoft.com/library/windows/apps/mt243287)</td>
    </tr>
    <tr>
        <td>Usando o Microsoft Visual Studio para disparar as transições do aplicativo</td>
        <td>[Como disparar eventos de suspensão, retomada e segundo plano para aplicativos da Windows Store no Visual Studio](https://msdn.microsoft.com/library/hh974425.aspx)</td>
    </tr>
</table>
 

### Projetando a experiência do usuário para jogos

O origem de um jogo excelente é o design inspirado.

Os jogos compartilham alguns elementos comuns da interface do usuário e princípios de design com os aplicativos, mas os jogos geralmente têm visual e objetivo de design exclusivos para a experiência do usuário. Os jogos alcançam o sucesso quando o design cuidadoso considera dois aspectos: quando o jogo deve usar uma experiência do usuário testada e quando deve divergir e inovar? A tecnologia de apresentação que você escolhe para o seu jogo — DirectX, XAML, HTML5 ou alguma combinação das três — afetará os detalhes da implementação, mas os princípios de design aplicados dependem pouco dessa opção.

Separadamente do design da experiência do usuário, o design de jogo, como o design de níveis, ritmo, design do mundo e outros aspectos, é uma forma de arte propriamente dita que depende de você e da sua equipe e não faz parte do escopo deste guia de desenvolvimento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Diretrizes e noções básicas de design da UWP</td>
        <td>[Crie aplicativos UWP](https://dev.windows.com/design)</td>
    </tr>
    <tr>
        <td>Design para estados do ciclo de vida do aplicativo</td>
        <td>[Diretrizes da experiência do usuário para execução, suspensão e reinício](https://msdn.microsoft.com/library/windows/apps/dn611862)</td>
    </tr>
    <tr>
        <td>Focando em vários fatores forma de dispositivo (vídeo)</td>
        <td>[Criando jogos para o mundo Windows Core](http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World)</td>
    </tr>
</table>
 

#### Diretrizes e paleta de cores

Seguir uma diretriz de cores consistente no jogo melhora sua estética, ajuda na navegação e é uma ferramenta poderosa para mostrar as funcionalidades do menu e de HUD ao jogador. O uso de cores coerentes nos elementos do jogo, como avisos, perigo, desempenho extremo e conquistas resulta em uma interface do usuário mais limpa e reduz a necessidade de rótulos explícitos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia de cores</td>
        <td>[Práticas recomendadas: cores](https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip)</td>
    </tr>
</table>
 

#### Tipografia

O uso apropriado da tipografia melhora vários aspectos do jogo, como layout, navegação, legibilidade, atmosfera, marca e a imersão do jogador na interface do usuário.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia de tipografia</td>
        <td>[Práticas recomendadas: tipografia](http://go.microsoft.com/fwlink/?LinkId=535007)</td>
    </tr>
</table>
 

#### Mapa da interface do usuário

Um mapa da interface do usuário é um layout de navegação e dos menus do jogo mostrados em um fluxograma. O mapa da interface do usuário ajuda os participantes a entenderem a interface e os caminhos de navegação do jogo, e pode expor possíveis obstáculos e becos sem saída logo no início do ciclo de desenvolvimento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia do mapa da interface do usuário</td>
        <td>[Práticas recomendadas: mapa da interface do usuário](http://go.microsoft.com/fwlink/?LinkId=535008)</td>
    </tr>
</table>
 

### Desenvolvimento com o DirectX

Guias e referências para desenvolvimento de jogos com o DirectX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desenvolvimento de jogos com o DirectX na UWP</td>
        <td>[Jogos e DirectX](index.md)</td>
    </tr>
    <tr>
        <td>Interação do DirectX com o modelo de aplicativo UWP</td>
        <td>[Objeto de aplicativo e DirectX](about-the-metro-style-user-interface-and-directx.md)</td>
    </tr>
    <tr>
        <td>Vídeos sobre elementos gráficos e desenvolvimento com o DirectX 12 (canal do YouTube)</td>
        <td>[Microsoft DirectX 12 and Graphics Education](https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA)</td>
    </tr>
    <tr>
        <td>Referência e visões gerais do DirectX</td>
        <td>[Elementos gráficos e jogos do DirectX](https://msdn.microsoft.com/library/windows/desktop/ee663274)</td>
    </tr>
    <tr>
        <td>Guia e referência de programação do Direct3D 12</td>
        <td>[Elementos gráficos do Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>Conceitos básicos do DirectX 12 (vídeo)</td>
        <td>[Mais energia, melhor desempenho: seu jogo em DirectX 12](http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12)</td>
    </tr>
</table>

#### Conhecendo o Direct3D 12

Saiba o que mudou no Direct3D 12 e como começar a programar usando o Direct3D 12. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Configure seu ambiente de programação</td>
        <td>[Configuração do ambiente programação do Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx)</td>
    </tr>
    <tr>
        <td>Como criar um componente básico</td>
        <td>[Criando um componente básico do Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx)</td>
    </tr>
    <tr>
        <td>Alterações no Direct3D 12</td>
        <td>[Migrando alterações importantes do Direct3D 11 para o Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx)</td>
    </tr>
    <tr>
        <td>Como fazer a portabilidade do Direct3D 11 para o Direct3D 12</td>
        <td>[Fazendo a portabilidade do Direct3D 11 para o Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx)</td>
    </tr>
    <tr>
        <td>Conceitos de associação de recursos (abrangendo descritor, tabela de descritores, heap de descritor e assinatura de raiz) </td>
        <td>[Associação de recursos no Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx)</td>
    </tr>
    <tr>
        <td>Gerenciamento de memória</td>
        <td>[Gerenciamento de memória no Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx)</td>
    </tr>
</table>
 
#### Bibliotecas e kit de ferramentas do DirectX

O kit de ferramentas DirectX, a biblioteca de processamento de texturas DirectX, a biblioteca de processamento de geometria DirectXMesh, a biblioteca UVAtlas e a biblioteca DirectXMath fornecem textura, malha, sprite e outras classes auxiliares e funcionalidades de utilitários para desenvolvimento no DirectX. Essas bibliotecas podem ajudá-lo a economizar tempo e esforço de desenvolvimento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Baixar o Kit de ferramentas do DirectX para DirectX 11</td>
        <td>[DirectXTK](http://go.microsoft.com/fwlink/?LinkId=248929)</td>
    </tr>
    <tr>
        <td>Baixar o Kit de ferramentas do DirectX para DirectX 12</td>
        <td>[DirectXTK 12](http://go.microsoft.com/fwlink/?LinkID=615561)</td>
    </tr>
    <tr>
        <td>Baixar a biblioteca de processamento de texturas DirectX</td>
        <td>[DirectXTex](http://go.microsoft.com/fwlink/?LinkId=248926)</td>
    </tr>
    <tr>
        <td>Baixar a biblioteca de processamento de geometria DirectXMesh</td>
        <td>[DirectXMesh](http://go.microsoft.com/fwlink/?LinkID=324981)</td>
    </tr>
    <tr>
        <td>Baixar UVAtlas para criar e compactar atlas de textura de isográfico</td>
        <td>[UVAtlas](http://go.microsoft.com/fwlink/?LinkID=512686)</td>
    </tr>
    <tr>
        <td>Baixar a biblioteca DirectXMath</td>
        <td>[DirectXMath](http://go.microsoft.com/fwlink/?LinkID=615560)</td>
    </tr>
    <tr>
        <td>Suporte para Direct3D 12 no DirectXTK (postagem de blog)</td>
        <td>[Suporte para DirectX 12](https://github.com/Microsoft/DirectXTK/issues/2)</td>
    </tr>
</table>

#### Recursos do DirectX de parceiros

Esta é a documentação adicional do DirectX criada por parceiros externos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: O que fazer e o que não fazer no DX12 (postagem de blog) </td>
        <td>[DirectX 12 em GPUs Nvidia](https://developer.nvidia.com/dx12-dos-and-donts-updated)</td>
    </tr>
    <tr>
        <td>Intel: Renderização eficiente com o DirectX 12</td>
        <td>[Renderização do DirectX 12 em Gráficos Intel](https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf)</td>
    </tr>
    <tr>
        <td>Intel: Suporte a vários adaptadores no DirectX 12</td>
        <td>[Como implementar um aplicativo de vários adaptadores explícitos usando o DirectX 12](https://software.intel.com/articles/multi-adapter-support-in-directx-12)</td>
    </tr>
    <tr>
        <td>Intel: Tutorial do DirectX 12</td>
        <td>[White paper colaborativo da Intel, da Suzhou Snail e da Microsoft](https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1)</td>
    </tr>
</table>


## Produção


Agora seu estúdio está totalmente preparado e passando para o ciclo de produção, com trabalho distribuído por toda a sua equipe. Você está retocando, refatorando e estendendo o protótipo para transformá-lo em um jogo completo.

### Notificações e blocos dinâmicos

Um bloco é a representação de seu jogo no menu Iniciar. Os blocos e notificações podem atrair o interesse do jogador, mesmo quando ele não está jogando no momento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desenvolvendo blocos e selos</td>
        <td>[Blocos, selos e notificações](https://msdn.microsoft.com/library/windows/apps/mt185606)</td>
    </tr>
    <tr>
        <td>Amostra que ilustram blocos dinâmicos e notificações</td>
        <td>[Amostra de notificações](https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications)</td>
    </tr>
    <tr>
        <td>Modelos de blocos adaptáveis (postagem de blog)</td>
        <td>[Modelos de blocos adaptáveis: esquema e documentação](http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx)</td>
    </tr>
    <tr>
        <td>Criando blocos e selos</td>
        <td>[Diretrizes de blocos e notificações](https://msdn.microsoft.com/library/windows/apps/hh465403)</td>
    </tr>
    <tr>
        <td>Aplicativo do Windows 10 para desenvolver modelos de blocos dinâmicos interativamente</td>
        <td>[Visualizador de notificações](https://www.microsoft.com/store/apps/9nblggh5xsl1)</td>
    </tr>
    <tr>
        <td>Extensão de gerador de bloco UWP para o Visual Studio</td>
        <td>[Ferramenta de criação de todos os blocos necessários usando uma única imagem](https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2)</td>
    </tr>
    <tr>
        <td>Extensão de gerador de bloco UWP para o Visual Studio (postagem de blog)</td>
        <td>[Dicas sobre como usar a ferramenta de gerador de bloco UWP](https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/)</td>
    </tr>
</table>
 

### Habilitar compras de produtos no aplicativo (IAP)

Um IAP (produto no aplicativo) é um item suplementar que os jogadores podem comprar no jogo. Os IAPs podem ser novos complementos, níveis de jogo, itens ou qualquer outra coisa que os jogadores possam aproveitar. Usados adequadamente, os IAPs podem gerar receita e ainda melhorar a experiência do jogo. Você define e publica os IAPs de seu jogo por meio do painel do Centro de Desenvolvimento do Windows e habilita compras no aplicativo no código do jogo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Produtos duráveis no aplicativo</td>
        <td>[Habilitar compras de produtos no aplicativo](https://msdn.microsoft.com/library/windows/apps/mt219684)</td>
    </tr>
    <tr>
        <td>Produtos consumíveis no aplicativo</td>
        <td>[Habilitar compras de produtos consumíveis no aplicativo](https://msdn.microsoft.com/library/windows/apps/mt219683)</td>
    </tr>
    <tr>
        <td>Detalhes e envio de produtos no aplicativo</td>
        <td>[Envios de IAP](https://msdn.microsoft.com/library/windows/apps/mt148551)</td>
    </tr>
    <tr>
        <td>Monitorar vendas IAP e dados demográficos para seu jogo</td>
        <td>[Relatório de aquisições de IAP](https://msdn.microsoft.com/library/windows/apps/mt148538)</td>
    </tr>
</table>
 
### Ferramentas de depuração e monitoramento de desempenho

O Windows Performance Toolkit (WPT) consiste em ferramentas que produzem perfis de desempenho detalhadas de sistemas operacionais Windows e aplicativos de monitoramento de desempenho. Isso é especialmente útil para monitorar o uso da memória e melhorar o desempenho do jogo. O Windows Performance Toolkit está incluído no SDK do Windows 10 e Windows ADK. Esse kit de ferramentas consiste em duas ferramentas independentes: Windows Performance Recorder (WPR) e Windows Performance Analyzer (WPA). Outra ferramenta útil para gerar arquivos de despejo de memória para investigar falhas do jogos é o ProcDump, que faz parte do [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Obtenha o Windows Performance Toolkit (WPT) do SDK do Windows 10</td>
        <td>[SDK do Windows 10](https://developer.microsoft.com/windows/downloads/windows-10-sdk)</td>
    </tr>
    <tr>
        <td>Obtenha o Windows Performance Toolkit (WPT) do Windows ADK</td>
        <td>[Windows ADK](https://msdn.microsoft.com/windows/hardware/dn913721.aspx)</td>
    </tr>
    <tr>
        <td>Solucionar problemas de irresponsabilidade da interface do usuário usando o Windows Performance Analyzer (vídeo)</td>
        <td>[Análise de caminho crítico com WPA](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer)</td>
    </tr>
    <tr>
        <td>Diagnosticar o uso de memória e perdas usando o Windows Performance Recorder (vídeo)</td>
        <td>[Volume de memória e perda](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks)</td>
    </tr>
    <tr>
        <td>Obtenha o ProcDump</td>
        <td>[ProcDump](https://technet.microsoft.com/sysinternals/dd996900)</td>
    </tr>
    <tr>
        <td>Aprenda a usar o ProcDump (vídeo)</td>
        <td>[Configurar o ProcDump para criar arquivos de despejo](https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK)</td>
    </tr>
</table>

### Conceitos e técnicas avançadas do DirectX

Algumas partes do desenvolvimento no DirectX podem ser sutis e complexas. Quando se atinge um ponto na produção em que você precisa investigar os detalhes de seu mecanismo DirectX ou depurar problemas difíceis de desempenho, os recursos e as informações desta seção podem ajudar.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Otimizando elementos gráficos e desempenho (vídeo)</td>
        <td>[Elementos gráficos DirectX 12 e desempenho avançados](http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance)</td>
    </tr>
    <tr>
        <td>Depuração de elementos gráficos do DirectX (vídeo)</td>
        <td>[Resolver problemas difíceis de elementos gráficos em seu jogo usando as ferramentas do DirectX](http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools)</td>
    </tr>
    <tr>
        <td>Ferramentas do Visual Studio 2015 para depuração do DirectX 12 (vídeo)</td>
        <td>[Ferramentas do DirectX para Windows 10 no Visual Studio 2015](https://channel9.msdn.com/Series/ConnectOn-Demand/212)</td>
    </tr>
    <tr>
        <td>Guia de programação do Direct3D 12</td>
        <td>[Guia de programação do Direct3D 12](https://msdn.microsoft.com/library/windows/desktop/dn903821)</td>
    </tr>
    <tr>
        <td>Combinando DirectX e XAML</td>
        <td>[Interoperabilidade entre DirectX e XAML](directx-and-xaml-interop.md)</td>
    </tr>
</table>

### Globalização e localização

Desenvolva jogos prontos para o mundo para a plataforma Windows e saiba mais sobre os recursos internacionais incorporados aos principais produtos da Microsoft.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Preparando seu jogo para o mercado global</td>
        <td>[Diretrizes ao desenvolver para um público global](https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx)</td>
    </tr>
    <tr>
        <td>Transpondo idiomas, culturas e tecnologias</td>
        <td>[Recurso online para convenções de idioma e a terminologia Microsoft padrão](http://www.microsoft.com/Language/Default.aspx)</td>
    </tr>
</table>


## Enviando e publicando seu jogo


Os guias e as informações a seguir ajudam a tornar o processo de publicação e envio o mais simples possível.

### Empacotamento e upload

Você usará o novo painel unificado do Centro de Desenvolvimento do Windows para publicar e gerenciar seus pacotes de jogos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publicação de aplicativos do Centro de Desenvolvimento do Windows</td>
        <td>[Publicar aplicativos do Windows](https://dev.windows.com/publish)</td>
    </tr>
    <tr>
        <td>Classificando seu jogo (postagem de blog)</td>
        <td>[Fluxo de trabalho único para atribuir classificações etárias usando o sistema IARC](https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/)</td>
    </tr>
    <tr>
        <td>Empacotando seu jogo</td>
        <td>[Empacote seu jogo do DirectX da UWP (Plataforma Universal do Windows)](package-your-windows-store-directx-game.md)</td>
    </tr>
    <tr>
        <td>Empacotando seu jogo como um desenvolvedor terceirizado (postagem de blog)</td>
        <td>[Criar pacotes carregáveis sem acesso à conta do fornecedor da loja](https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/)</td>
    </tr>
    <tr>
        <td>Criando aplicativos e pacotes de aplicativo usando MakeAppx</td>
        <td>[Criar pacotes usando a ferramenta de empacotador de aplicativo MakeAppx.exe](https://msdn.microsoft.com/library/windows/desktop/hh446767)</td>
    </tr>
    <tr>
        <td>Assinando seus arquivos digitalmente com a SignTool</td>
        <td>[Assinar arquivos e verificar assinaturas em arquivos usando a SignTool](https://msdn.microsoft.com/library/windows/desktop/aa387764)</td>
    </tr>      
    <tr>
        <td>Carregando e controlando a versão de seu jogo</td>
        <td>[Carregar pacotes de aplicativo](https://msdn.microsoft.com/library/windows/apps/mt148542)</td>
    </tr>
</table>
 

### Políticas e certificação

Não deixe que problemas de certificação atrasem o lançamento de seu jogo. Aqui estão as políticas e os problemas comuns de certificação sobre os quais você deve estar ciente.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Contrato de Desenvolvedor de Aplicativos da Windows Store</td>
        <td>[Contrato de Desenvolvedor de Aplicativos](https://msdn.microsoft.com/library/windows/apps/hh694058)</td>
    </tr>
    <tr>
        <td>Políticas para publicar aplicativos na Windows Store</td>
        <td>[Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944)</td>
    </tr>
    <tr>
        <td>Como evitar alguns problemas comuns de certificação de aplicativo</td>
        <td>[Evitar erros comuns de certificação](https://msdn.microsoft.com/library/windows/apps/jj657968)</td>
    </tr>
</table>
 

### Manifesto da Loja (StoreManifest.xml)

O manifesto da loja (StoreManifest.xml) é um arquivo de configuração opcional que pode ser incluído no pacote do aplicativo. O manifesto da loja fornece recursos adicionais que não fazem parte do arquivo AppxManifest.xml. Por exemplo, você pode usar o manifesto da loja para bloquear a instalação de seu jogo se um dispositivo de destino não tiver o nível de recurso mínimo especificado do DirectX ou o mínimo especificado para a memória do sistema.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Esquema do manifesto da loja</td>
        <td>[Esquema StoreManifest (Windows 10)](https://msdn.microsoft.com/library/windows/apps/mt617335)</td>
    </tr>
</table>
 

## Gerenciamento do ciclo de vida do jogo


Quando você termina o desenvolvimento e envia seu jogo, não é "fim de jogo". Você pode ter terminado o desenvolvimento da primeira versão, mas a jornada de seu jogo no mercado está apenas começando. Você pode monitorar o uso e os relatórios de erros, responder aos comentários dos usuários e publicar atualizações para seu jogo.

### Análises e promoções do Centro de Desenvolvimento do Windows

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Aplicativo do Centro de Desenvolvimento</td>
        <td>[Aplicativo do Centro de Desenvolvimento do Windows 10 para exibir o desempenho de seus aplicativos publicados](https://www.microsoft.com/store/apps/dev-center/9nblggh4r5ws)</td>
    </tr>  
    <tr>
        <td>Análises do Centro de Desenvolvimento do Windows</td>
        <td>[Análises](https://msdn.microsoft.com/library/windows/apps/mt148522)</td>
    </tr>
    <tr>
        <td>Respondendo às críticas dos clientes</td>
        <td>[Responder às críticas dos clientes](https://msdn.microsoft.com/library/windows/apps/mt148546)</td>
    </tr>
    <tr>
        <td>Maneiras de promover seu jogo</td>
        <td>[Promova seus aplicativos](https://dev.windows.com/store-promotion)</td>
    </tr>
</table>
 

### Visual Studio Application Insights

O Visual Studio Application Insights fornece análises de desempenho, telemetria e uso para seu jogo publicado. O Application Insights ajuda a detectar e solucionar problemas depois que seu jogo é lançado, a monitorar e aprimorar continuamente o uso e a entender como os jogadores continuam a interagir com seu jogo. O Application Insights funciona com a adição de um SDK ao seu aplicativo, que envia telemetria para o [portal do Azure](http://portal.azure.com/).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análise de desempenho e uso do aplicativo</td>
        <td>[Visual Studio Application Insights](https://azure.microsoft.com/documentation/articles/app-insights-get-started/)</td>
    </tr>
    <tr>
        <td>Habilitar o Application Insights em aplicativos do Windows</td>
        <td>[Application Insights para aplicativos do Windows Phone e da Loja](https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/)</td>
    </tr>
</table>
 

### Criando e gerenciando atualizações de conteúdo

Para atualizar seu jogo publicado, envie um novo pacote do aplicativo com um número de versão mais alto. Depois que o pacote passar pelo processo de envio e certificação, ele será disponibilizado automaticamente para os clientes como uma atualização.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Atualizando e controlando a versão de seu jogo</td>
        <td>[Numeração de versão do pacote](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
    <tr>
        <td>Orientação para gerenciamento de pacotes de jogos</td>
        <td>[Orientação para gerenciamento do pacote de aplicativo](https://msdn.microsoft.com/library/windows/apps/mt188602)</td>
    </tr>
</table>


## Adicionando o Xbox Live ao seu jogo


> 
              **Observação**   O desenvolvimento para o Xbox Live é gerenciado por programas como ID@Xbox e Microsoft Studios. Este guia abrange uma ampla variedade de recursos, e você pode descobrir que alguns recursos não são acessíveis dependendo de sua participação no programa ou de sua função de desenvolvimento específica. Os exemplos são links que se resolvem em developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou no Game Developer Network (GDN). Para obter informações sobre parcerias com a Microsoft, consulte [Programas de desenvolvedor](#programs).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Baixar o SDK do Xbox Live mais recente</td>
        <td>[SDK do Xbox Live](http://aka.ms/xsapi2)</td>
    </tr>
    <tr>
        <td>Adicionando o Xbox Live ao seu aplicativo da Plataforma Universal do Windows</td>
        <td>[Como adicionar o SDK do Xbox Live a aplicativos da Plataforma Universal do Windows (UWP)](http://aka.ms/xsapi2uwp)</td>
    </tr>
    <tr>
        <td>Requisitos para jogos que usam o Xbox Live</td>
        <td>[Requisitos do Xbox para o Xbox Live no Windows 10](http://go.microsoft.com/fwlink/?LinkId=533217)</td>
    </tr>
    <tr>
        <td>Visão geral de desenvolvimento de jogos para o Xbox Live (vídeo)</td>
        <td>[Desenvolvendo com o Xbox Live para Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10)</td>
    </tr>
    <tr>
        <td>Associação de plataforma cruzada (vídeo)</td>
        <td>[Xbox Live Multiplayer: apresentando os serviços de associação de plataforma cruzada e jogabilidade](http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay)</td>
    </tr>
    <tr>
        <td>Jogabilidade entre dispositivos no Fable Legends (vídeo)</td>
        <td>[Fable Legends: jogabilidade entre dispositivos com o Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live)</td>
    </tr>
    <tr>
        <td>Estatísticas e conquistas no Xbox Live (vídeo)</td>
        <td>[Práticas recomendadas para utilizar as estatísticas e as conquistas do usuário baseado em nuvem no Xbox Live](http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live)</td>
    </tr>
</table>
 

## Recursos adicionais

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desenvolvimento de jogos independentes (vídeo)</td>
        <td>[Novas oportunidades para desenvolvedores independentes](http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers)</td>
    </tr>
    <tr>
        <td>Considerações para dispositivos móveis com vários núcleos (vídeo)</td>
        <td>[Desempenho de jogos sustentado em dispositivos móveis com vários núcleos](http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices)</td>
    </tr>
    <tr>
        <td>Desenvolvimento de jogos da área de trabalho do Windows 10 (vídeo)</td>
        <td>[Jogos de computador para Windows 10](http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10)</td>
    </tr>
</table>



 

 

 



<!--HONumber=Jul16_HO2-->


