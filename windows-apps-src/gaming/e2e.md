---
author: joannaleecy
title: Guia de desenvolvimento de jogos do Windows 10
description: Um guia completo de recursos e informações para desenvolver jogos da Plataforma Universal do Windows (UWP).
ms.assetid: 6061F498-96A8-44EF-9711-68AE5A1218C9
ms.author: joanlee
ms.date: 04/16/2018
ms.topic: article
keywords: windows 10, uwp, jogos, desenvolvimento de jogos
ms.localizationpriority: medium
ms.openlocfilehash: 7481c1d0f64ccb25168200cdf5e6ccc068f769b9
ms.sourcegitcommit: cbe7cf620622a5e4df7414f9e38dfecec1cfca99
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2018
ms.locfileid: "7418485"
---
# <a name="windows-10-game-development-guide"></a>Guia de desenvolvimento de jogos do Windows 10


Bem-vindo ao guia de desenvolvimento de jogos do Windows 10!

Este guia fornece uma coleção completa dos recursos e informações necessários para desenvolver um jogo da Plataforma Universal do Windows (UWP). Uma versão em inglês (EUA) deste guia está disponível em formato [PDF](http://download.microsoft.com/download/9/C/9/9C9D344F-611F-412E-BB01-259E5C76B17F/Windev_Game_Dev_Guide_Oct_2017.pdf).

## <a name="introduction-to-game-development-for-the-universal-windows-platform-uwp"></a>Introdução ao desenvolvimento de jogos para a Plataforma Universal do Windows (UWP)


Ao criar um jogo do Windows 10, você tem a oportunidade de alcançar milhões de jogadores no mundo inteiro no telefone, no computador e no Xbox One. Com o Xbox no Windows, o Xbox Live, vários jogadores em vários dispositivos, uma comunidade incrível de jogadores e novos recursos avançados, como a Plataforma Universal do Windows (UWP) e o DirectX 12, os jogos do Windows 10 vão animar jogadores de todas as idades e gêneros. A nova Plataforma Universal do Windows (UWP) oferece compatibilidade para seu jogo em todos os dispositivos Windows 10 com uma API comum para telefone, computador e Xbox One, juntamente com ferramentas e opções para adaptar seu jogo a cada experiência de dispositivo.

Este guia fornece uma coleção completa de informações e recursos que ajudarão você a desenvolver seu jogo. As seções são organizadas de acordo com os estágios do desenvolvimento de jogos, para que você saiba onde procurar as informações quando precisar delas.

Se você for iniciante no desenvolvimento de jogos no Windows ou no Xbox, o guia [Introdução](getting-started.md) pode ser um bom ponto de partida. A seção [Recursos de desenvolvimento de jogos](#game-development-resources) também fornece uma pesquisa de alto nível de documentação, programas e outros recursos que são úteis para a criação de um jogo. Se, em vez disso, quiser começar a analisar alguns códigos UWP, consulte os [Exemplos de jogos](#game-samples).

Este guia será atualizado à medida que recursos e materiais adicionais de desenvolvimento de jogos do Windows 10 se tornarem disponíveis.

## <a name="game-development-resources"></a>Recursos de desenvolvimento de jogos

De documentação a programas, fóruns, blogs e exemplos para desenvolvedores, há muitos recursos disponíveis para ajudar você em sua jornada de desenvolvimento de jogos. Este é um resumo dos recursos que você deve conhecer para desenvolver seu jogo do Windows 10.

> [!Note]
> Alguns recursos são gerenciados por meio de vários programas. Este guia abrange uma ampla variedade de recursos, então, você pode descobrir que alguns recursos não são acessíveis dependendo do programa em que você está ou de sua função de desenvolvimento específica. Os exemplos são links que se resolvem em developer.xboxlive.com, forums.xboxlive.com, xdi.xboxlive.com ou no Game Developer Network (GDN). Para obter informações sobre parcerias com a Microsoft, consulte [Programas de desenvolvedor](#developer-programs).


### <a name="game-development-documentation"></a>Documentação de desenvolvimento de jogos

Ao longo deste guia, você encontrará links profundos para a documentação relevante, organizados por tarefa, tecnologia e estágio do desenvolvimento de jogos. Para que você tenha uma visão ampla do que está disponível, aqui estão os principais portais de documentação sobre desenvolvimento de jogos do Windows 10.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Portal principal do Centro de Desenvolvimento do Windows</td>
        <td><a href="https://dev.windows.com">Centro de Desenvolvimento do Windows</a></td>
    </tr>
    <tr>
        <td>Desenvolvendo aplicativos do Windows</td>
        <td><a href="https://dev.windows.com/develop">Desenvolver aplicativos do Windows</a></td>
    </tr>
    <tr>
        <td>Desenvolvimento de aplicativos da Plataforma Universal do Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt244352">Guias de instruções para aplicativos do Windows 10</a></td>
    </tr>
    <tr>
        <td>Guias de instruções para jogos UWP</td>
        <td><a href="index.md">Jogos e DirectX</a> </td>
    </tr>
    <tr>
        <td>Referência e visões gerais do DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Elementos gráficos e jogos do DirectX</a></td>
    </tr>
    <tr>
        <td>Azure para jogos</td>
        <td><a href="https://azure.microsoft.com/solutions/gaming/">Compilar e dimensionar seus jogos usando o Azure</a></td>
    </tr>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://api.playfab.com/">Solução de back-end completa para jogos ao vivo</a></td>
    </tr>
    <tr>
        <td>UWP no Xbox One</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/xbox-apps/index">Criar aplicativos UWP no Xbox One</a></td>
    </tr>
    <tr>
        <td>UWP no HoloLens</td>
        <td><a href="https://developer.microsoft.com/windows/mixed-reality/development_overview">Criar aplicativos UWP no HoloLens</a></td>
    </tr>
    <tr>
        <td>Documentação do Xbox Live</td>
        <td><a href="../xbox-live/index.md">Guia do desenvolvedor do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Documentação de desenvolvimento do Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-home">Desenvolvimento para Xbox One</a></td>
    </tr>
    <tr>
        <td>White papers para desenvolvimento do Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-whitepapers">White Papers</a></td>
    </tr>
    <tr>
        <td>Documentação do Mixer Interactive</td>
        <td><a href="https://dev.mixer.com/reference/interactive/index.html">Adicionar interatividade ao seu jogo</a></td>
    </tr>        
</table>

### <a name="partner-center"></a>Partner Center

[Registrar uma conta de desenvolvedor no Partner Center](https://developer.microsoft.com/store/register) é a primeira etapa para publicar seu jogo do Windows. Com uma conta de desenvolvedor, você pode reservar o nome de seu jogo e enviar jogos gratuitos e pagos à Microsoft Store para todos os dispositivos Windows. Use sua conta de desenvolvedor para gerenciar seu jogo e produtos no jogo, obter análises detalhadas e habilitar serviços que criam ótimas experiências para jogadores do mundo inteiro. 

A Microsoft também oferece vários programas de desenvolvedor para ajudar você a desenvolver e publicar jogos do Windows. É recomendável verificar se algum deles serve para você antes de registrar uma conta do Partner Center. Para obter mais informações, acesse [Programas de desenvolvedor](#developer-programs).


### <a name="developer-programs"></a>Programas de desenvolvedor

A Microsoft oferece vários programas de desenvolvedor para ajudar você a desenvolver e publicar jogos do Windows. Considere a possibilidade de participar de um programa de desenvolvedor se você quiser desenvolver jogos para Xbox One e integrar recursos do Xbox Live ao seu jogo. Para publicar um jogo na Microsoft Store, você também precisará criar uma conta de desenvolvedor no [Partner Center](https://partner.microsoft.com/dashboard) .

#### <a name="xbox-live-creators-program"></a>Programa de Criadores do Xbox Live

O Programa de Criadores do Xbox Live permite que qualquer pessoa integre o Xbox Live a seu título e publique no Xbox One e no Windows 10. Há um processo de certificação simplificado, e nenhuma aprovação de conceito é necessária fora das [Políticas da Microsoft Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx) padrão.

Você pode implantar, criar e publicar seu jogo no Programa de Criadores sem um kit de desenvolvimento exclusivo, usando apenas um hardware de varejo. Para começar, baixe o [Aplicativo Ativação do Modo de Desenvolvedor](https://docs.microsoft.com/windows/uwp/xbox-apps/devkit-activation) no seu Xbox One.

Se você deseja ter acesso a mais recursos do Xbox Live, marketing dedicado e suporte para desenvolvimento e a chance de ganhar destaque na loja principal do Xbox One, inscreva-se no programa [ID@Xbox](http://www.xbox.com/Developers/id).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programa de Criadores do Xbox Live</td>
        <td><a href="https://developer.microsoft.com/games/xbox/xboxlive/creator">Saiba mais sobre o Programa de Criadores do Xbox Live</a></td>
    </tr>
</table>

#### <a name="idxbox"></a>ID@Xbox

O programa ID@Xbox ajuda desenvolvedores de jogos qualificados a autopublicar no Windows e no Xbox One. Se você quiser desenvolver para o Xbox One ou adicionar recursos do Xbox Live, como pontuação, conquistas, placares de líderes, ao seu jogo do Windows 10, inscreva-se no ID@Xbox. Torne-se um desenvolvedor do ID@Xbox para obter as ferramentas e o suporte necessários para soltar sua imaginação e maximizar seu sucesso. É recomendável que você aplique a ID@Xbox primeiro antes de criar uma conta de desenvolvedor no Partner Center.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Programa de desenvolvedor ID@Xbox</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkID=526271">Programa de desenvolvedor independente para Xbox One</a></td>
    </tr>
    <tr>
        <td>Site para consumidores do ID@Xbox</td>
        <td><a href="http://www.idatxbox.com/">ID@Xbox</a></td>
    </tr>
</table>

#### <a name="xbox-tools-and-middleware"></a>Ferramentas e middleware do Xbox

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


### <a name="game-samples"></a>Exemplos de jogos

Há muitas amostras de jogos e aplicativos do Windows 10 disponíveis para ajudar você a entender os recursos de jogos do Windows 10 e começar rapidamente a desenvolver jogos. Mais amostras são desenvolvidas e publicadas regularmente. Por isso, não se esqueça de revisitar ocasionalmente os portais de amostras para conferir as novidades. Você também pode [acompanhar](https://help.github.com/articles/watching-repositories/) os repositórios GitHub para receber notificações sobre alterações e adições.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Exemplos de aplicativos da Plataforma Universal do Windows</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples">Windows-universal-samples</a></td>
    </tr>
    <tr>
        <td>Exemplos de elementos gráficos Direct3D 12</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples">DirectX-Graphics-Samples</a></td>
    </tr>
    <tr>
        <td>Exemplos de elementos gráficos Direct3D 11</td>
        <td><a href="https://github.com/walbourn/directx-sdk-samples">directx-sdk-samples</a></td>
    </tr>
    <tr>
        <td>Amostra de jogo em primeira pessoa do Direct3D 11</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Criar um jogo simples da UWP com DirectX</a></td>
    </tr>
    <tr>
        <td>Amostra de efeitos de imagem personalizados do Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620531">D2DCustomEffects</a></td>
    </tr>
    <tr>
        <td>Amostra de malha de gradiente do Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620532">D2DGradientMesh</a></td>
    </tr>
    <tr>
        <td>Amostra de ajuste de fotos do Direct2D</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?LinkId=620533">D2DPhotoAdjustment</a></td>
    </tr>
    <tr>
        <td>Exemplos públicos do grupo de tecnologias avançadas do Xbox</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples">Xbox-ATG-Samples</a></td>
    </tr>
    <tr>
        <td>Exemplos do Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples">exemplos do xbox-live</a></td>
    </tr>
    <tr>
        <td>Exemplos de jogos do Xbox One (XGD)</td>
        <td><a href="https://developer.microsoft.com/games/xbox/partner/development-education-samples">Exemplos</a></td>
    </tr>
    <tr>
        <td>Exemplos de jogos do Windows (Galeria de Códigos do MSDN)</td>
        <td><a href="https://code.msdn.microsoft.com/windowsapps/site/search?f%5B0%5D.Type=SearchText&f%5B0%5D.Value=game&f%5B1%5D.Type=Contributors&f%5B1%5D.Value=Microsoft&f%5B1%5D.Text=Microsoft">Exemplos de jogos da Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Amostra de jogo em JavaScript 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-js2d.md">Criar um jogo UWP em JavaScript</a></td>
    </tr>
    <tr>
        <td>Amostra de jogo em JavaScript 3D</td>
        <td><a href="../get-started/get-started-tutorial-game-js3d.md">Criar um jogo 3D em JavaScript usando three.js</a></td>
    </tr>
    <tr>
        <td>Jogo de exemplo UWP em MonoGame 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Criar um jogo UWP em MonoGame 2D</a></td>
    </tr>      
</table>


### <a name="developer-forums"></a>Fóruns para desenvolvedores

Os fóruns de desenvolvedores são um ótimo lugar para fazer e responder perguntas sobre desenvolvimento de jogos e conectar-se com a comunidade de desenvolvimento de jogos. Os fóruns também podem ser recursos fantásticos para encontrar respostas existentes para problemas difíceis que os desenvolvedores encontraram e resolveram no passado.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Fóruns de desenvolvedores de aplicativos e jogos publicação</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsapps">Anúncios em apps e publicação</a></td>
    </tr>
    <tr>
        <td>Fórum do desenvolvedor de aplicativos UWP</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?forum=wpdevelop">Desenvolvendo aplicativos da Plataforma Universal do Windows</a></td>
    </tr>
    <tr>
        <td>Fóruns de desenvolvedores de aplicativos da área de trabalho</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsdesktopdev">Fóruns de aplicativos da área de trabalho do Windows</a></td>
    </tr>
    <tr>
        <td>Jogos da Microsoft Store com DirectX (postagens arquivadas no fórum)</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/vstudio/home?forum=wingameswithdirectx">Criando jogos da Microsoft Store com DirectX (arquivado)</a></td>
    </tr>
    <tr>
        <td>Fóruns de desenvolvedores parceiros gerenciados do Windows 10</td>
        <td><a href="http://aka.ms/win10devforums">Fóruns de desenvolvedores XBOX: Windows 10</a></td>
    </tr>
    <tr>
        <td>Fóruns do DirectX</td>
        <td><a href="http://forums.directxtech.com/index.php">Fórum do DirectX 12</a></td>
    </tr>
    <tr>
        <td>Fóruns da plataforma Azure</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-us/home?category=windowsazureplatform">Fórum do Azure</a></td>
    </tr>
    <tr>
        <td>Fórum do Xbox Live</td>
        <td><a href="https://social.msdn.microsoft.com/Forums/en-US/home?forum=xboxlivedev">Fórum de desenvolvimento do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Fóruns da PlayFab</td>
        <td><a href="https://community.playfab.com/index.html">Fóruns da PlayFab</a></td>
    </tr>
</table>


### <a name="developer-blogs"></a>Blogs de desenvolvedores

Os blogs de desenvolvedores são outro recurso excelente para as informações mais recentes sobre o desenvolvimento de jogos. Você encontrará postagens sobre novos recursos, detalhes de implementação, práticas recomendadas, histórico de arquitetura e muito mais.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Blog Criando aplicativos para Windows</td>
        <td><a href="http://blogs.windows.com/buildingapps/">Criando aplicativos para Windows</a></td>
    </tr>
    <tr>
        <td>Windows 10 (postagens de blog)</td>
        <td><a href="http://blogs.windows.com/blog/tag/windows-10/">Postagens no Windows 10</a></td>
    </tr>
    <tr>
        <td>Blog da equipe de engenharia do Visual Studio</td>
        <td><a href="http://blogs.msdn.com/b/visualstudio/">Blog do Visual Studio</a></td>
    </tr>
    <tr>
        <td>Blogs de ferramentas de desenvolvedor do Visual Studio</td>
        <td><a href="http://blogs.msdn.com/b/developer-tools/">Blogs de ferramentas de desenvolvedor</a></td>
    </tr>
    <tr>
        <td>Blog de ferramentas de desenvolvedor do Somasegar</td>
        <td><a href="http://blogs.msdn.com/b/somasegar/">Blog do Somasegar</a></td>
    </tr>
    <tr>
        <td>Blog de desenvolvedores do DirectX</td>
        <td><a href="http://blogs.msdn.com/b/directx">Blog de desenvolvedores do DirectX</a></td>
    </tr>
    <tr>
        <td>Introdução ao DirectX 12 (postagem de blog)</td>
        <td><a href="http://blogs.msdn.com/b/directx/archive/2014/03/20/directx-12.aspx">DirectX 12</a></td>
    </tr>
    <tr>
        <td>Blog da equipe de ferramentas do Visual C++</td>
        <td><a href="http://blogs.msdn.com/b/vcblog/">Blog da equipe do Visual C++</a></td>
    </tr>
    <tr>
        <td>Blog da equipe do PIX</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/">Ajuste de desempenho e depuração para jogos do DirectX 12 no Windows e no Xbox</a></td>
    </tr>
    <tr>
        <td>Blog da equipe de implantação do aplicativo universal do Windows</td>
        <td><a href="https://blogs.msdn.microsoft.com/appinstaller/">Blog da equipe de criação e implantação de aplicativos UWP</a></td>
    </tr>
</table>
 

## <a name="concept-and-planning"></a>Conceito e planejamento


Na fase de conceito e planejamento, você decide como será seu jogo e as tecnologias e ferramentas que você usará para dar vida a ele.

### <a name="overview-of-game-development-technologies"></a>Visão geral das tecnologias de desenvolvimento de jogos

Quando você começa a desenvolver um jogo para a UWP, existem várias opções disponíveis de elementos gráficos, entrada, áudio, rede, utilitários e bibliotecas.

Se você já decidiu quais tecnologias você usará em seu jogo, ótimo! Caso contrário, o guia [Tecnologias de jogos para aplicativos UWP](game-development-platform-guide.md) é uma excelente visão geral de muitas das tecnologias disponíveis, e é altamente recomendável que você leia esse guia para entender melhor as opções e como elas se complementam.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Pesquisa de tecnologias de jogos UWP</td>
        <td><a href="game-development-platform-guide.md">Tecnologias de jogos para aplicativos UWP</a></td>
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
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-Games-for-Windows-10">Desenvolvendo jogos para Windows 10</a></td>
    </tr>
    <tr>
        <td>Experiência de jogos do Windows 10 (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Gaming-Consumer-Experience-on-Windows-10">Experiência de consumidor de jogos no Windows 10</a></td>
    </tr>
    <tr>
        <td>Jogos no ecossistema da Microsoft (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/The-Future-of-Gaming-Across-the-Microsoft-Ecosystem">O futuro dos jogos no ecossistema da Microsoft</a></td>
    </tr>
</table>

### <a name="game-planning"></a>Planejamento de jogo

Estes são alguns tópicos de conceito e planejamento de nível alto a serem considerados ao planejar seu jogo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Tornando seu jogo acessível</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/accessibility-for-games">Acessibilidade para jogos</a></td>
    </tr>
    <tr>
        <td>Criar jogos usando nuvem</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/cloud-for-games">Nuvem para jogos</a></td>
    </tr>
    <tr>
        <td>Monetizar seu jogo</td>
        <td><a href="https://msdn.microsoft.com/windows/uwp/gaming/monetization-for-games">Monetização para jogos</a></td>
    </tr>
</table>



### <a name="choosing-your-graphics-technology-and-programming-language"></a>Escolhendo a tecnologia de elementos gráficos e a linguagem de programação

Há várias tecnologias gráficas e linguagens de programação disponíveis para os jogos do Windows 10. Seu caminho dependerá do tipo de jogo que está desenvolvendo, da experiência e das preferências de seu estúdio de desenvolvimento e dos requisitos de recursos específicos do seu jogo. Você usará o C#, C++ ou JavaScript? DirectX, XAML ou HTML5?

#### <a name="directx"></a>DirectX

O Microsoft DirectX é a opção certa para criar elementos gráficos e multimídia 2D e 3D com maior desempenho.

O DirectX 12 é mais rápido e mais eficiente do que qualquer versão anterior. O Direct3D 12 proporciona cenas mais detalhadas, mais objetos, efeitos mais complexos e a utilização completa de hardware de GPU moderno nos computadores do Windows 10 e Xbox One.

Se desejar usar o pipeline de gráfico familiar do Direct3D 11, você ainda aproveitará os novos recursos de renderização e otimização do Direct3D 11.3. E, se você for um desenvolvedor consagrado da API do Windows para área de trabalho com origem no Win32, continuará tendo essa opção no Windows 10.

Os vários recursos e a profunda integração da plataforma do DirectX possibilitam a eficiência e o desempenho necessários para os jogos mais exigentes.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desenvolvimento com o DirectX para a UWP</td>
        <td><a href="directx-programming.md">Programação DirectX</a></td>
    </tr>
    <tr>
        <td>Tutorial: como criar um projeto de jogo DirectX com UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Criar um jogo simples UWP com o DirectX</a></td>
    </tr>
    <tr>
        <td>Referência e visões gerais do DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Elementos gráficos e jogos do DirectX</a></td>
    </tr>
    <tr>
        <td>Guia e referência de programação do Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Elementos gráficos do Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Vídeos sobre elementos gráficos e desenvolvimento com o DirectX 12 (canal do YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 and Graphics Education</a></td>
    </tr>
</table>
 

#### <a name="xaml"></a>XAML

O XAML é uma linguagem de interface do usuário declarativa fácil de usar com recursos convenientes, como animações, storyboards, vinculação de dados, elementos gráficos vetoriais escalonáveis, redimensionamento dinâmico e gráficos de cena. O XAML funciona muito bem em interfaces do usuário, menus, sprites e elementos gráficos 2D de jogos. Para facilitar o layout da interface do usuário, o XAML é compatível com ferramentas de design e desenvolvimento como Expression Blend e Microsoft Visual Studio. O XAML costuma ser usado com o C#, mas o C++ também é uma boa opção, caso seja sua linguagem preferida ou se o jogo tiver grande demanda de CPU.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral da plataforma XAML</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228259">Plataforma XAML</a></td>
    </tr>
    <tr>
        <td>Interface do usuário e controles XAML</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt228348">Controles, layouts e texto</a></td>
    </tr>
</table>
 

#### <a name="html-5"></a>HTML 5

O HTML (HyperText Markup Language) é uma linguagem de marcação da interface do usuário comum usada em páginas da web, aplicativos e clientes sofisticados. Os jogos do Windows podem usar HTML5 como uma camada de apresentação com todos os recursos familiares do HTML, acesso à Plataforma Universal do Windows e suporte a recursos modernos da Web, como AppCache, Web Workers, canvas, arrastar e soltar, programação assíncrona e SVG. Nos bastidores, a renderização do HTML aproveita a capacidade de aceleração de hardware do DirectX. Assim, você obtém os benefícios de desempenho do DirectX sem criar nenhum código extra. O HTML5 é uma boa opção quando você tem experiência com o desenvolvimento da Web, quando está portando um jogo da Web ou se quiser usar uma linguagem e camadas gráficas mais fáceis de abordar do que nos outras opções. O HTML5 é usado com JavaScript, mas também pode chamar componentes criados em C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Informações sobre modelos de objetos HTML5 e de documento</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/br212882.aspx">Referência de HTML e DOM</a></td>
    </tr>
    <tr>
        <td>Recomendação de HTML5 do W3C</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=221374">HTML5</a></td>
    </tr>
</table>
 

#### <a name="combining-presentation-technologies"></a>Combinando tecnologias de apresentação

O Microsoft DirectX Graphics Infrastructure (DXGI) oferece interoperabilidade e compatibilidade entre várias tecnologias gráficas. Para elementos gráficos de alto desempenho, você pode combinar XAML e DirectX, usando XAML para menus e outras interfaces do usuário simples e DirectX para a renderização de cenas 2D e 3D complexas. A DXGI também fornece compatibilidade entre Direct2D, Direct3D, DirectWrite, DirectCompute e Microsoft Media Foundation.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia de programação e referência do DirectX Graphic Infrastructure</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh404534">DXGI</a></td>
    </tr>
    <tr>
        <td>Combinando DirectX e XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilidade entre DirectX e XAML</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C++

O C++/CX é uma linguagem de baixa sobrecarga e alto desempenho que oferece a poderosa combinação de velocidade, compatibilidade e acesso à plataforma. Com o C++/CX, é fácil usar todos os excelentes recursos de jogo no Windows 10, inclusive o DirectX e o Xbox Live. Você também pode reutilizar o código C++ e as bibliotecas existentes. O C++/CX cria código nativo rápido que não incorre na sobrecarga de coleta de lixo para que seu jogo possa ter um ótimo desempenho e baixo consumo de energia, fazendo com que a bateria dure mais. Use o C++/CX com DirectX ou XAML, orou crie um jogo usando uma combinação dos dois.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referência e visões gerais da linguagem C++/CX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh699871.aspx">Referência da linguagem Visual C++ (C++/CX)</a></td>
    </tr>
    <tr>
        <td>Guia e referência de programação do Visual C++</td>
        <td><a href="https://docs.microsoft.com/cpp/visual-cpp-in-visual-studio">Visual C++ no Visual Studio 2017</a></td>
    </tr>
</table>
 

#### <a name="c"></a>C#

O C# (fala-se "C sharp") é uma linguagem inovadora e moderna, simples, eficiente, fortemente tipada e orientada a objeto. O C# permite um desenvolvimento rápido, pois mantém a familiaridade e expressividade das linguagens no estilo C. Embora seja fácil de usar, o C# tem vários recursos de linguagem avançados, como polimorfismo, representantes, lambdas, fechamentos, métodos de iterador, covariância e expressões de consulta integrada à linguagem (LINQ). O C# é uma excelente opção se você está visando o XAML, deseja começar a desenvolver seu jogo rapidamente ou tem experiência anterior em C#. O C# é usado principalmente com XAML. Portanto, para usar DirectX, escolha C++, ou crie parte do seu jogo como um componente C++ que interaja com DirectX. Ou considere o [Win2D](https://github.com/Microsoft/Win2D), uma biblioteca de elementos gráficos em modo imediato Direct2D para C# e C++.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia e referência de programação em C#</td>
        <td><a href="https://msdn.microsoft.com/library/kx37x362.aspx">Referência da linguagem C#</a></td>
    </tr>
</table>
 

#### <a name="javascript"></a>JavaScript

O JavaScript é uma linguagem de scripts dinâmica amplamente usada em aplicativos cliente avançados e da Web moderna.

Os aplicativos do Windows em JavaScript podem acessar os recursos avançados da Plataforma Universal do Windows de forma fácil e intuitiva, como métodos e propriedades de classes de JavaScript orientadas a objeto. O JavaScript é uma boa opção para seu jogo quando você vem de um ambiente de desenvolvimento da Web, já está familiarizado com o JavaScript ou deseja usar as bibliotecas de HTML5, CSS, WinJS ou JavaScript. Se quiser usar o DirectX ou XAML, escolha o C# ou C++/CX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Referência de JavaScript e do Windows Runtime</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj613794">Referência de JavaScript</a></td>
    </tr>
</table>


#### <a name="use-windows-runtime-components-to-combine-languages"></a>Usar os componentes do Windows Runtime para combinar linguagens

Com a Plataforma Universal do Windows, é fácil combinar componentes criados em linguagens diferentes. Crie componentes do Windows Runtime em C++, C# ou Visual Basic, e os chame do JavaScript, C#, C++ ou Visual Basic. Essa é uma ótima maneira de programar partes do jogo na linguagem de sua escolha. O componentes também permitem o uso de bibliotecas externas disponíveis somente em uma determinada linguagem, além do código herdado que você já criou.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Como criar componentes do Windows Runtime</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/winrt-components/creating-windows-runtime-components-in-cpp">Criando componentes do Windows Runtime</a></td>
    </tr>
</table>


### <a name="which-version-of-directx-should-your-game-use"></a>Que versão do DirectX seu jogo deve usar?

Se você escolher o DirectX para seu jogo, você precisará decidir qual versão usar: Microsoft Direct3D12 ou Microsoft Direct3D11.

O DirectX 12 é mais rápido e mais eficiente do que qualquer versão anterior. O Direct3D 12 proporciona cenas mais detalhadas, mais objetos, efeitos mais complexos e a utilização completa de hardware de GPU moderno nos computadores do Windows 10 e Xbox One. Como Direct3D 12 funciona em um nível muito baixo, é possível dar a uma equipe de desenvolvimento de elementos gráficos especializada ou a uma equipe de desenvolvimento com o DirectX 11 experiente o controle que precisam para maximizar a otimização dos elementos gráficos.

O Direct3D 11.3 é uma API de elemento gráfico de baixo nível que usa o modelo de programação Direct3D e manipula grande parte da complexidade envolvida na renderização de GPU. Ela também é compatível com o Windows 10 e o Xbox One. Se você tem um mecanismo escrito em Direct3D 11 mas ainda não está pronto para passar ao Direct3D 12, você pode usar o Direct3D 11 sobre o 12 para obter algumas melhorias de desempenho. As Versões 11.3+ contêm os novos recursos de renderização e otimização habilitados também no Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Escolhendo Direct3D12 ou Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899228">O que é Direct3D12?</a></td>
    </tr>
    <tr>
        <td>Visão geral do Direct3D11</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ff476080">Elementos gráficos do Direct3D 11</a></td>
    </tr>
    <tr>
        <td>Visão geral do Direct3D 11 on 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn913195">Direct3D 11 on 12</a></td>
    </tr>
</table>


### <a name="bridges-game-engines-and-middleware"></a>Pontes, mecanismos de jogos e middleware

Dependendo das necessidades de seu jogo, usar pontes, mecanismos de jogos ou middleware pode poupar tempo e recursos de desenvolvimento e teste. Aqui estão alguns recursos e visões gerais de pontes, mecanismos de jogos e middleware.

#### <a name="universal-windows-platform-bridges"></a>Pontes da Plataforma Universal do Windows

Pontes de Plataforma Universal do Windows são tecnologias que levam seu aplicativo ou jogo existente para a UWP. Pontes são uma ótima maneira de iniciar rapidamente o desenvolvimento de jogos na UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Pontes UWP</td>
        <td><a href="https://dev.windows.com/bridges/">Traga seu código para o Windows</a></td>
    </tr>
    <tr>
        <td>Ponte do Windows para iOS</td>
        <td><a href="https://dev.windows.com/bridges/ios">Traga seu aplicativo iOS para o Windows</a></td>
    </tr>
    <tr>
        <td>Ponte do Windows para aplicativos da área de trabalho (.NET e Win32)</td>
        <td><a href="https://developer.microsoft.com/windows/bridges/desktop">Converta seu aplicativo da área de trabalho em um aplicativo UWP</a></td>
    </tr>
</table>

#### <a name="playfab"></a>PlayFab

Agora parte da família Microsoft, a PlayFab é uma plataforma completa de back-end para jogos ao vivo e uma maneira eficiente para estúdios independentes começarem. Aumente a receita, o envolvimento e a retenção — enquanto reduz os custos — com serviços de jogos, análise em tempo real e LiveOps.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PlayFab</td>
        <td><a href="https://playfab.com/">Visão geral de ferramentas e serviços</a></td>
    </tr>
    <tr>
        <td>Introdução</td>
        <td><a href="https://api.playfab.com/docs/general-getting-started">Guia de introdução general</a></td>
    </tr>
    <tr>
        <td>Série de tutoriais em vídeo</td>
        <td><a href="https://www.youtube.com/watch?v=fGNpiqVi5xU&list=PLHCfyL7JpoPbLpA_oh_T5PKrfzPgCpPT5">Série de vídeos de demonstração sobre os principais sistemas da PlayFab</a></td>
    </tr>
    <tr>
        <td>Receitas</td>
        <td><a href="https://api.playfab.com/docs/tutorials/recipes-index">Amostras de mecânica de jogo e padrão de design populares</a></td>
    </tr>
    <tr>
        <td>Plataformas</td>
        <td><a href="https://api.playfab.com/platforms">Documentação específica para uma variedade de plataformas e mecanismos de jogos</a></td>
    </tr>
    <tr>
        <td>Repositório do GitHub</td>
        <td><a href="https://github.com/PlayFab">Obtenha scripts e SDKs para várias plataformas, incluindo Android, iOS, Windows, Unity e Unreal.</a></td>
    </tr>
    <tr>
        <td>Documentação da API</td>
        <td><a href="https://api.playfab.com/documentation/">Acesse o serviço da PlayFab diretamente por APIs Web semelhantes a REST</a></td>
    </tr>
    <tr>
        <td>Fóruns</td>
        <td><a href="https://community.playfab.com/index.html">Fóruns da PlayFab</a></td>
    </tr>
</table>
 

#### <a name="unity"></a>Unity

O Unity fornece uma plataforma para criação de ótimos aplicativos e jogos em 2D, 3D, VR e RA. Permite que você concretize sua visão criativa de forma rápida e fornece seu conteúdo a quase todos os dispositivos e mídias.

A partir do Unity 5.4, o Unity oferece suporte ao desenvolvimento do Direct3D 12.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Mecanismo de jogos Unity</td>
        <td><a href="http://unity3d.com/">Unity – Mecanismo de jogos</a></td>
    </tr>
    <tr>
        <td>Obtenha o Unity</td>
        <td><a href="http://unity3d.com/get-unity">Obtenha o Unity</a></td>
    </tr>
    <tr>
        <td>Documentação do Unity para Windows</td>
        <td><a href="http://docs.unity3d.com/Manual/Windows.html">Manual do Unity / Windows</a></td>
    </tr>
    <tr>
        <td>Adicionar LiveOps usando PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unity-getting-started">Introdução: crie sua primeira chamada à API da PlayFab no seu jogo Unity</a></td>
    </tr>
    <tr>
        <td>Como adicionar interatividade ao seu jogo usando o Mixer Interactive</td>
        <td><a href="https://github.com/mixer/interactive-unity-plugin/wiki/Getting-started">Guia de Introdução</a></td>
    </tr>
    <tr>
        <td>SDK do Mixer para Unity</td>
        <td><a href="https://www.assetstore.unity3d.com/en/#!/content/88585">Plug-in do Mixer Unity</a></td>
    </tr>
    <tr>
        <td>Documentação de referência do SDK do Mixer para Unity</td>
        <td><a href="https://dev.mixer.com/reference/interactive/csharp/index.html">Referência da API para plug-in do Mixer Unity</a></td>
    </tr>
    <tr>
        <td>Publicar seu jogo do Unity na Microsoft Store</td>
        <td><a href="https://unity3d.com/partners/microsoft/porting-guides">Guia de portabilidade</a></td>
    </tr>
    <tr>
        <td>Solução de problemas de referências de assembly ausentes relacionadas a APIs .NET</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/missing-dot-net-apis-in-unity-and-uwp">.NET APIs ausentes no Unity e na UWP</a></td>
    </tr>
    <tr>
        <td>Publicar seu jogo do Unity como um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/How-to-publish-your-Unity-game-as-a-UWP-app">Como publicar seu jogo do Unity como um aplicativo UWP</a></td>
    </tr>
    <tr>
        <td>Usar o Unity para criar aplicativos e jogos do Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Blogs/One-Dev-Minute/Making-games-and-apps-with-Unity">Criar jogos e aplicativos do Windows com o Unity</a></td>
    </tr>
    <tr>
        <td>Desenvolvimento de jogos Unity usando o Visual Studio (série de vídeos)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=722359">Usando o Unity com o Visual Studio 2015</a></td>
    </tr>
</table>
 

#### <a name="havok"></a>Havok

O pacote modular de ferramentas e tecnologias do Havok ajuda criadores de jogos a alcançar novos níveis de interatividade e imersão. O Havok oferece física altamente realista, simulações interativas e uma cinemática incrível. Versão 2015.1 e superior oficialmente dá suporte a UWP no Visual Studio 2015 em x86, 64 bits e ARM.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Site do Havok</td>
        <td><a href="http://www.havok.com/">Havok</a></td>
    </tr>
    <tr>
        <td>Pacote de ferramentas Havok</td>
        <td><a href="http://www.havok.com/products/">Visão geral do produto Havok</a></td>
    </tr>
    <tr>
        <td>Fóruns de suporte do Havok</td>
        <td><a href="http://support.havok.com">Havok</a></td>
    </tr>
</table>
 

#### <a name="monogame"></a>MonoGame

MonoGame é uma estrutura de desenvolvimento de jogos de plataforma cruzada e código aberto originalmente com base no XNA Framework 4.0 da Microsoft. O Monogame atualmente oferece suporte para Windows, Windows Phone e Xbox, bem como Linux, macOS, iOS, Android e várias outras plataformas.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>MonoGame</td>
        <td><a href="http://www.monogame.net">Site do MonoGame</a></td>
    </tr>
    <tr>
        <td>Documentação do MonoGame</td>
        <td><a href="http://www.monogame.net/documentation/">Documentação do MonoGame (mais recente)</a></td>
    </tr>
    <tr>
        <td>Downloads do MonoGame</td>
        <td><a href="http://www.monogame.net/downloads/">Baixe versões, compilações de desenvolvimento e o código-fonte</a> no site do MonoGame, ou <a href="https://www.nuget.org/profiles/MonoGame">obtenha a versão mais recente via NuGet</a>.
    </tr>
    <tr>
        <td>Jogo de exemplo UWP em MonoGame 2D</td>
        <td><a href="../get-started/get-started-tutorial-game-mg2d.md">Criar um jogo UWP em MonoGame 2D</a></td>
    </tr>    
</table>


#### <a name="cocos2d"></a>Cocos2d

Cocos2d-x é um pacote de ferramentas e mecanismos de desenvolvimento de jogos de software livre e plataforma cruzada que oferece suporte à criação de jogos UWP. A partir da versão 3, recursos 3D também estão sendo adicionados.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/">O que é Cocos2d-x?</a></td>
    </tr>
    <tr>
        <td>Guia do programador do Cocos2d-x</td>
        <td><a href="http://www.cocos2d-x.org/programmersguide/">Guia do programador do Cocos2d-x</a></td>
    </tr>
    <tr>
        <td>Cocos2d-x no Windows 10 (postagem de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/06/15/running-cocos2d-x-on-windows-10/">Executando o Cocos2d-x no Windows 10</a></td>
    </tr>
    <tr>
        <td>Adicionar LiveOps usando PlayFab</td>
        <td><a href="https://api.playfab.com/docs/getting-started/cocos2d-x-getting-started-guide">Introdução: crie sua primeira chamada à API da PlayFab no seu jogo Cocos2d-x</a></td>
    </tr>
</table>


#### <a name="unreal-engine"></a>Unreal Engine

O Unreal Engine 4 é um conjunto completo de ferramentas de desenvolvimento de jogos para todos os tipos de jogos e desenvolvedores. Para os jogos de console e de computador mais exigentes, o Unreal Engine é usado por desenvolvedores de jogos em todo o mundo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral do Unreal Engine</td>
        <td><a href="https://www.unrealengine.com/what-is-unreal-engine-4">Unreal Engine 4</a></td>
    </tr>
    <tr>
        <td>Adicionar LiveOps usando PlayFab - C++</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-cpp-getting-started">Introdução: crie sua primeira chamada à API da PlayFab no seu jogo Unreal</a></td>
    </tr>
    <tr>
        <td>Adicionar LiveOps usando PlayFab - Blueprints</td>
        <td><a href="https://api.playfab.com/docs/getting-started/unreal-blueprints-getting-started">Introdução: crie sua primeira chamada à API da PlayFab no seu jogo Unreal</a></td>
    </tr>
</table>

#### <a name="babylonjs"></a>BabylonJS

BabylonJS é uma estrutura JavaScript completa para criar jogos 3D com HTML5, WebGL, WebVR e áudio da Web.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>BabylonJS</td>
        <td><a href="http://www.babylonjs.com/">BabylonJS</a></td>
    </tr>
    <tr>
        <td>WebGL 3D com HTML5 e BabylonJS (série de vídeos)</td>
        <td><a href="https://channel9.msdn.com/Series/Introduction-to-WebGL-3D-with-HTML5-and-Babylonjs/01">Conhecendo o WebGL 3D e o BabylonJS</a></td>
    </tr>
    <tr>
        <td>Criando um jogo WebGL para várias plataformas com BabylonJS</td>
        <td><a href="https://www.smashingmagazine.com/2016/07/babylon-js-building-sponza-a-cross-platform-webgl-game/">Usar o BabylonJS para desenvolver um jogo para várias plataformas</a></td>
    </tr>    
</table>

### <a name="porting-your-game"></a>Fazendo a portabilidade de seu jogo

Se você tiver um jogo existente, há muitos recursos e guias disponíveis para lhe ajudar a trazer seu jogo rapidamente para a UWP. Para impulsionar seus esforços de portabilidade, você também pode usar as [Ponte da Plataforma Universal do Windows](#universal-windows-platform-bridges).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo do Windows 8 para um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238322">Mudar do Windows Runtime 8.x para a UWP</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo do Windows 8 para um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/A-Developers-Guide-to-Windows-10/21">Fazendo a portabilidade de aplicativos 8.1 para o Windows 10</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo iOS para um aplicativo da Plataforma Universal do Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238320">Mudar do iOS para a UWP</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um aplicativo Silverlight para um aplicativo da Plataforma Universal do Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt238323">Mudar do Windows Phone Silverlight para a UWP</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do XAML ou Silverlight para um aplicativo da Plataforma Universal do Windows (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2015/3-741">Fazendo a portabilidade de um aplicativo XAML ou Silverlight para Windows 10</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade de um jogo do Xbox para um aplicativo da Plataforma Universal do Windows</td>
        <td><a href="https://developer.xboxlive.com/en-us/platform/development/education/Documents/Porting%20from%20Xbox%20One%20to%20Windows%2010.aspx">Fazendo a portabilidade do Xbox One para a UWP do Windows 10</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do DirectX 9 para o DirectX 11</td>
        <td><a href="porting-your-directx-9-game-to-windows-store.md">Portar do DirectX 9 para a Plataforma Universal do Windows (UWP)</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do Direct3D 11 para o Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709">Fazendo a portabilidade do Direct3D 11 para o Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Fazendo a portabilidade do OpenGL ES para o Direct3D 11</td>
        <td><a href="port-from-opengl-es-2-0-to-directx-11-1.md">Fazer a portabilidade do OpenGL ES 2.0 para o Direct3D 11</a></td>
    </tr>
    <tr>
        <td>OpenGL ES para Direct3D 11 usando o ANGLE</td>
        <td><a href="http://go.microsoft.com/fwlink/p/?linkid=618387">ANGLE</a></td>
    </tr>
    <tr>
        <td>Equivalentes das APIs clássicas do Windows na UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh464945">Alternativas a APIs do Windows em aplicativos da Plataforma Universal do Windows (UWP)</a></td>
    </tr>
</table>


## <a name="prototype-and-design"></a>Protótipo e design


Agora que você decidiu o tipo de jogo que deseja criar e as ferramentas e a tecnologia de elementos gráficos que usará para criá-lo, você está pronto para começar a trabalhar no design e no protótipo. Essencialmente, seu jogo é um aplicativo da Plataforma Universal do Windows. Portanto, é por aí que você vai começar.

### <a name="introduction-to-the-universal-windows-platform-uwp"></a>Introdução à Plataforma Universal do Windows (UWP)

O Windows 10 apresenta a Plataforma Universal do Windows (UWP), que fornece uma plataforma de API comuns em dispositivos Windows 10. A UWP evolui e expande o modelo do Windows Runtime e o transforma em um núcleo coeso e unificado. Os jogos destinados à UWP podem chamar APIs do WinRT que são comuns a todos os dispositivos. Como a UWP fornece camadas de API garantidas, você pode optar por criar um único pacote de aplicativo que será instalado em dispositivos Windows 10. E se você quiser, seu jogo ainda pode chamar APIs (incluindo algumas APIs clássicas do Windows, do Win32 e .NET) que são específicas para os dispositivos nos quais seu jogo funciona.

As referências a seguir são guias excelentes que explicam em detalhes os aplicativos da Plataforma Universal do Windows, e sua leitura é recomendada para ajudar você a entender a plataforma.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introdução a aplicativos da Plataforma Universal do Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726767">O que é um aplicativo da Plataforma Universal do Windows?</a></td>
    </tr>
    <tr>
        <td>Visão geral da UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn894631">Guia para aplicativos UWP</a></td>
    </tr>
</table>
 

### <a name="getting-started-with-uwp-development"></a>Introdução ao desenvolvimento da UWP

É fácil e rápido se preparar para desenvolver um aplicativo da Plataforma Universal do Windows. Os guias a seguir explicarão o processo passo a passo para você.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introdução ao desenvolvimento da UWP</td>
        <td><a href="https://dev.windows.com/getstarted">Introdução aos aplicativos do Windows</a></td>
    </tr>
    <tr>
        <td>Preparando-se para o desenvolvimento na UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn726766">Prepare-se para começar</a></td>
    </tr>
</table>

Se você é um "iniciante" na programação da UWP e está considerando usar XAML no seu jogo (consulte [Escolhendo a tecnologia de elementos gráficos e a linguagem de programação](#choosing-your-graphics-technology-and-programming-language)), a série de vídeos sobre [desenvolvimento do Windows 10 para iniciantes](https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners) é um bom ponto de partida.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia para iniciantes no desenvolvimento do Windows 10 com XAML (série de vídeos)</td>
        <td><a href="https://channel9.msdn.com/Series/Windows-10-development-for-absolute-beginners">Desenvolvimento do Windows 10 para iniciantes</a></td>
    </tr>
    <tr>
        <td>Anunciando a série para iniciantes do Windows 10 usando XAML (postagem de blog)</td>
        <td><a href="http://blogs.windows.com/buildingapps/2015/09/30/windows-10-development-for-absolute-beginners/">Desenvolvimento do Windows 10 para iniciantes</a></td>
    </tr>
</table>

### <a name="uwp-development-concepts"></a>Conceitos de desenvolvimento da UWP

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral do desenvolvimento de aplicativos da Plataforma Universal do Windows</td>
        <td><a href="https://dev.windows.com/develop">Desenvolver aplicativos do Windows</a></td>
    </tr>
    <tr>
        <td>Visão geral da programação de rede na UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt280378">Serviços de rede e Web</a></td>
    </tr>
    <tr>
        <td>Usando Windows.Web.HTTP e Windows.Networking.Sockets em jogos</td>
        <td><a href="work-with-networking-in-your-directx-game.md">Rede para jogos</a></td>
    </tr>
    <tr>
        <td>Conceitos de programação assíncrona na UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt187335">Programação assíncrona</a></td>
    </tr>
</table>

### <a name="windows-desktop-apisto-uwp"></a>Área de trabalho do Windows APIsto UWP

Estes são alguns links para ajudá-lo a mover seu jogo da área de trabalho do Windows para a UWP.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Usar código C++ existente para desenvolvimento de jogos UWP</td>
        <td><a href="https://docs.microsoft.com/cpp/porting/how-to-use-existing-cpp-code-in-a-universal-windows-platform-app">Como: usar código C++ existente em um aplicativo UWP</a></td>
    </tr>
    <tr>
        <td>APIs UWP para APIs COM e Win32</td>
        <td><a href="https://docs.microsoft.com/uwp/win32-and-com/win32-and-com-for-uwp-apps">APIs COM e Win32 para aplicativos UWP</a></td>
    </tr>
    <tr>
        <td>Funções CRT sem suporte na UWP</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj606124.aspx">Funções da CRT não suportadas em aplicativos da Plataforma Universal do Windows</a></td>
    </tr>
    <tr>
        <td>Comandos alternativos para as APIs do Windows</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt592894.aspx">Alternativas para as APIs do Windows em aplicativos da Plataforma Universal do Windows (UWP)</a></td>
    </tr>
</table>
 

### <a name="process-lifetime-management"></a>Gerenciamento do tempo de vida do processo

Gerenciamento do tempo de vida do processo, ou ciclo de vida do aplicativo, descreve os vários estados de ativação pelos quais um aplicativo da Plataforma Universal do Windows pode passar. Seu jogo pode ser ativado, suspenso, retomado ou encerrado e pode passar por esses estados em uma variedade de maneiras.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Manipulando transições do ciclo de vida do aplicativo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt243287">Ciclo de vida do aplicativo</a></td>
    </tr>
    <tr>
        <td>Usando o Microsoft Visual Studio para disparar as transições do aplicativo</td>
        <td><a href="https://msdn.microsoft.com/library/hh974425.aspx">Como disparar eventos de suspensão, retomada e segundo plano para aplicativos UWP no Visual Studio</a></td>
    </tr>
</table>
 

### <a name="designing-game-ux"></a>Projetando a experiência do usuário para jogos

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
        <td><a href="https://dev.windows.com/design">Crie aplicativos UWP</a></td>
    </tr>
    <tr>
        <td>Design para estados do ciclo de vida do aplicativo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn611862">Diretrizes da experiência do usuário para execução, suspensão e reinício</a></td>
    </tr>
    <tr>
        <td>Projete seu aplicativo UWP para Xbox One e telas de televisão</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/design/devices/designing-for-tv">Projetar para TV e Xbox</a></td>
    </tr>
    <tr>
        <td>Focando em vários fatores forma de dispositivo (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Designing-Games-for-a-Windows-Core-World">Criando jogos para o mundo Windows Core</a></td>
    </tr>   
</table>
 

#### <a name="color-guideline-and-palette"></a>Diretrizes e paleta de cores

Seguir uma diretriz de cores consistente no jogo melhora sua estética, ajuda na navegação e é uma ferramenta poderosa para mostrar as funcionalidades do menu e de HUD ao jogador. O uso de cores coerentes nos elementos do jogo, como avisos, perigo, desempenho extremo e conquistas resulta em uma interface do usuário mais limpa e reduz a necessidade de rótulos explícitos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia de cores</td>
        <td><a href="https://assets.windowsphone.com/499cd2be-64ed-4b05-a4f5-cd0c9ad3f6a3/101_BestPractices_Color_InvariantCulture_Default.zip">Práticas recomendadas: cores</a></td>
    </tr>
</table>
 

#### <a name="typography"></a>Tipografia

O uso apropriado da tipografia melhora vários aspectos do jogo, como layout, navegação, legibilidade, atmosfera, marca e a imersão do jogador na interface do usuário.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia de tipografia</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535007">Práticas recomendadas: tipografia</a></td>
    </tr>
</table>
 

#### <a name="ui-map"></a>Mapa da interface do usuário

Um mapa da interface do usuário é um layout de navegação e dos menus do jogo mostrados em um fluxograma. O mapa da interface do usuário ajuda os participantes a entenderem a interface e os caminhos de navegação do jogo, e pode expor possíveis obstáculos e becos sem saída logo no início do ciclo de desenvolvimento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Guia do mapa da interface do usuário</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=535008">Práticas recomendadas: mapa da interface do usuário</a></td>
    </tr>
</table>

### <a name="game-audio"></a>Áudio de jogo

Guias e referências para a implementação de áudio em jogos usando XAudio2, XAPO e Windows Sonic. XAudio2 é uma API de áudio de baixo nível que oferece processamento de sinais e noções básicas de mixagem para o desenvolvimento de mecanismos de áudio de alto desempenho. A API XAPO permite a criação de objetos de processamento de áudio entre plataformas (XAPO) para uso no XAudio2 no Windows e no Xbox. O suporte de áudio do Windows Sonic permite que você adicione Dolby Atmos for Home Theater, Dolby Atmos for Headphones e suporte Windows HRTF ao seu jogo ou aplicativo de mídia de streaming.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>APIs XAudio2</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/hh405049.aspx">Guia de programação e referência de API para XAudio2</a></td>
    </tr>
    <tr>
        <td>Criar objetos de processamento de áudio entre plataformas</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee415735.aspx">Visão geral do XAPO</a></td>
    </tr>
    <tr>
        <td>Introdução aos conceitos de áudio</td>
        <td><a href="working-with-audio-in-your-directx-game.md">Áudio para jogos</a></td>
    </tr>
    <tr>
        <td>Visão geral do Windows Sonic</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt807491.aspx">Som espacial</a></td>
    </tr>
    <tr>
        <td>Amostras de som espacial do Windows Sonic</td>
        <td><a href="https://github.com/Microsoft/Xbox-ATG-Samples/tree/master/UWPSamples/Audio">Amostras de áudio do grupo de tecnologias avançadas do Xbox</a></td>
    </tr>
    <tr>
        <td>Saiba como integrar o Windows Sonic aos seus jogos (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-002">Introdução aos recursos de áudio espacial para rede Xbox</a></td>
    </tr>
</table>

### <a name="directx-development"></a>Desenvolvimento com o DirectX

Guias e referências para desenvolvimento de jogos com o DirectX.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desenvolvimento com o DirectX para a UWP</td>
        <td><a href="directx-programming.md">Programação DirectX</a></td>
    </tr>
    <tr>
        <td>Tutorial: como criar um projeto de jogo DirectX com UWP</td>
        <td><a href="tutorial--create-your-first-uwp-directx-game.md">Criar um jogo simples UWP com o DirectX</a></td>
    </tr>
    <tr>
        <td>Interação do DirectX com o modelo de aplicativo UWP</td>
        <td><a href="about-the-uwp-user-interface-and-directx.md">Objeto de aplicativo e DirectX</a></td>
    </tr>
    <tr>
        <td>Vídeos sobre elementos gráficos e desenvolvimento com o DirectX 12 (canal do YouTube)</td>
        <td><a href="https://www.youtube.com/channel/UCiaX2B8XiXR70jaN7NK-FpA">Microsoft DirectX 12 and Graphics Education</a></td>
    </tr>
    <tr>
        <td>Referência e visões gerais do DirectX</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/ee663274">Elementos gráficos e jogos do DirectX</a></td>
    </tr>
    <tr>
        <td>Guia e referência de programação do Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Elementos gráficos do Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Conceitos básicos do DirectX 12 (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Better-Power-Better-Performance-Your-Game-on-DirectX12">Mais energia, melhor desempenho: seu jogo em DirectX 12</a></td>
    </tr>
</table>

#### <a name="learning-direct3d-12"></a>Conhecendo o Direct3D 12

Saiba o que mudou no Direct3D 12 e como começar a programar usando o Direct3D 12. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Configure seu ambiente de programação</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899120.aspx">Configuração do ambiente programação do Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Como criar um componente básico</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn859356.aspx">Criando um componente básico do Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Alterações no Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899194.aspx">Migrando alterações importantes do Direct3D 11 para o Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Como fazer a portabilidade do Direct3D 11 para o Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt431709.aspx">Fazendo a portabilidade do Direct3D 11 para o Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Conceitos de associação de recursos (abrangendo descritor, tabela de descritores, heap de descritor e assinatura de raiz) </td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899206.aspx">Associação de recursos no Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Gerenciamento de memória</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn899198.aspx">Gerenciamento de memória no Direct3D 12</a></td>
    </tr>
</table>
 

#### <a name="directx-tool-kit-and-libraries"></a>Bibliotecas e kit de ferramentas do DirectX

O kit de ferramentas DirectX, a biblioteca de processamento de texturas DirectX, a biblioteca de processamento de geometria DirectXMesh, a biblioteca UVAtlas e a biblioteca DirectXMath fornecem textura, malha, sprite e outras classes auxiliares e funcionalidades de utilitários para desenvolvimento no DirectX. Essas bibliotecas podem ajudá-lo a economizar tempo e esforço de desenvolvimento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Baixar o Kit de ferramentas do DirectX para DirectX 11</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248929">DirectXTK</a></td>
    </tr>
    <tr>
        <td>Baixar o Kit de ferramentas do DirectX para DirectX 12</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615561">DirectXTK 12</a></td>
    </tr>
    <tr>
        <td>Baixar a biblioteca de processamento de texturas DirectX</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=248926">DirectXTex</a></td>
    </tr>
    <tr>
        <td>Baixar a biblioteca de processamento de geometria DirectXMesh</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=324981">DirectXMesh</a></td>
    </tr>
    <tr>
        <td>Baixar UVAtlas para criar e compactar atlas de textura de isográfico</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=512686">UVAtlas</a></td>
    </tr>
    <tr>
        <td>Baixar a biblioteca DirectXMath</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkID=615560">DirectXMath</a></td>
    </tr>
    <tr>
        <td>Suporte de Direct3D12 no DirectXTK (postagem de blog)</td>
        <td><a href="https://github.com/Microsoft/DirectXTK/issues/2">Suporte para DirectX 12</a></td>
    </tr>
</table>

#### <a name="directx-resources-from-partners"></a>Recursos do DirectX de parceiros

Esta é a documentação adicional do DirectX criada por parceiros externos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Nvidia: O que fazer e o que não fazer no DX12 (postagem de blog) </td>
        <td><a href="https://developer.nvidia.com/dx12-dos-and-donts-updated">DirectX 12 em GPUs Nvidia</a></td>
    </tr>
    <tr>
        <td>Intel: Renderização eficiente com o DirectX 12</td>
        <td><a href="https://software.intel.com/sites/default/files/managed/4a/38/Efficient-Rendering-with-DirectX-12-on-Intel-Graphics.pdf">Renderização do DirectX 12 em Gráficos Intel</a></td>
    </tr>
    <tr>
        <td>Intel: Suporte a vários adaptadores no DirectX 12</td>
        <td><a href="https://software.intel.com/articles/multi-adapter-support-in-directx-12">Como implementar um aplicativo de vários adaptadores explícitos usando o DirectX 12</a></td>
    </tr>
    <tr>
        <td>Intel: Tutorial do DirectX 12</td>
        <td><a href="https://software.intel.com/articles/tutorial-migrating-your-apps-to-directx-12-part-1">White paper colaborativo da Intel, da Suzhou Snail e da Microsoft</a></td>
    </tr>
</table>


## <a name="production"></a>Produção


Agora seu estúdio está totalmente preparado e passando para o ciclo de produção, com trabalho distribuído por toda a sua equipe. Você está retocando, refatorando e estendendo o protótipo para transformá-lo em um jogo completo.

### <a name="notifications-and-live-tiles"></a>Notificações e blocos dinâmicos

Um bloco é a representação de seu jogo no menu Iniciar. Os blocos e notificações podem atrair o interesse do jogador, mesmo quando ele não está jogando no momento.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Desenvolvendo blocos e selos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt185606">Blocos, selos e notificações</a></td>
    </tr>
    <tr>
        <td>Amostra que ilustram blocos dinâmicos e notificações</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/Notifications">Amostra de notificações</a></td>
    </tr>
    <tr>
        <td>Modelos de blocos adaptáveis (postagem de blog)</td>
        <td><a href="http://blogs.msdn.com/b/tiles_and_toasts/archive/2015/06/30/adaptive-tile-templates-schema-and-documentation.aspx">Modelos de blocos adaptáveis: esquema e documentação</a></td>
    </tr>
    <tr>
        <td>Criando blocos e selos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh465403">Diretrizes de blocos e notificações</a></td>
    </tr>
    <tr>
        <td>Aplicativo do Windows 10 para desenvolver modelos de blocos dinâmicos interativamente</td>
        <td><a href="https://www.microsoft.com/store/apps/9nblggh5xsl1">Visualizador de notificações</a></td>
    </tr>
    <tr>
        <td>Extensão de gerador de bloco UWP para o Visual Studio</td>
        <td><a href="https://visualstudiogallery.msdn.microsoft.com/09611e90-f3e8-44b7-9c83-18dba8275bb2">Ferramenta de criação de todos os blocos necessários usando uma única imagem</a></td>
    </tr>
    <tr>
        <td>Extensão de gerador de bloco UWP para o Visual Studio (postagem de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/02/15/uwp-tile-generator-extension-for-visual-studio/">Dicas sobre como usar a ferramenta de gerador de bloco UWP</a></td>
    </tr>
</table>
 

### <a name="enable-in-app-product-add-on-purchases"></a>Habilitar compras de produto no aplicativo (complemento)

Um complemento (produto no aplicativo) é um item suplementar que os jogadores podem comprar no jogo. Complementos podem ser níveis de jogo, itens ou qualquer outra coisa que os jogadores podem aproveitar. Usados adequadamente, complementos podem gerar receita e ainda melhorar a experiência do jogo. Definir e publicar os complementos do seu jogo por meio do Partner Center e habilitar compras no aplicativo no código do jogo.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Complementos duráveis</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219684">Habilitar compras de produtos no aplicativo</a></td>
    </tr>
    <tr>
        <td>Complementos consumíveis</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt219683">Habilitar compras de produtos consumíveis no aplicativo</a></td>
    </tr>
    <tr>
        <td>Envio e detalhes de complemento</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148551">Envios de complemento</a></td>
    </tr>
    <tr>
        <td>Monitorar vendas de complemento e dados demográficos para seu jogo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148538">Relatório de aquisições de complemento</a></td>
    </tr>
</table>
 

### <a name="debugging-performance-optimization-and-monitoring"></a>Depuração, otimização de desempenho e monitoramento

Para otimizar o desempenho, aproveite o Modo de Jogo no Windows 10 para fornecer aos jogadores a melhor experiência de jogo possível utilizando totalmente a capacidade de hardware atual.

O Windows Performance Toolkit (WPT) consiste em ferramentas que monitoram o desempenho e produzem perfis de desempenho detalhados de sistemas operacionais Windows e aplicativos. Isso é especialmente útil para monitorar o uso da memória e melhorar o desempenho do jogo. O Windows Performance Toolkit está incluído no SDK do Windows 10 e Windows ADK. Esse kit de ferramentas consiste em duas ferramentas independentes: Windows Performance Recorder (WPR) e Windows Performance Analyzer (WPA). O ProcDump, que faz parte do [Windows Sysinternals](https://technet.microsoft.com/sysinternals/default), é um utilitário de linha de comando que monitora picos de CPU e gera arquivos de despejo durante falhas de jogos. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Teste de desempenho do código</td>
        <td><a href="https://www.visualstudio.com/team-services/cloud-load-testing/">Teste de carga baseado na nuvem</a></td>
    </tr>
    <tr>
        <td>Obtenha um tipo de console do Xbox usando informações de dispositivo para jogos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt825235">Informações de dispositivo para jogos</a></td>
    </tr>
    <tr>
        <td>Melhore o desempenho obtendo acesso exclusivo ou prioritário a recursos de hardware usando as APIs de Modo de Jogo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808808">Modo de Jogo</a></td>
    </tr>
    <tr>
        <td>Obtenha o Windows Performance Toolkit (WPT) do SDK do Windows 10</td>
        <td><a href="https://developer.microsoft.com/windows/downloads/windows-10-sdk">SDK do Windows 10</a></td>
    </tr>
    <tr>
        <td>Obtenha o Windows Performance Toolkit (WPT) do Windows ADK</td>
        <td><a href="https://msdn.microsoft.com/windows/hardware/dn913721.aspx">Windows ADK</a></td>
    </tr>
    <tr>
        <td>Solucionar problemas de irresponsabilidade da interface do usuário usando o Windows Performance Analyzer (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-156-Critical-Path-Analysis-with-Windows-Performance-Analyzer">Análise de caminho crítico com WPA</a></td>
    </tr>
    <tr>
        <td>Diagnosticar o uso de memória e perdas usando o Windows Performance Recorder (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-154-Memory-Footprint-and-Leaks">Volume de memória e perda</a></td>
    </tr>
    <tr>
        <td>Obtenha o ProcDump</td>
        <td><a href="https://technet.microsoft.com/sysinternals/dd996900">ProcDump</a></td>
    </tr>
    <tr>
        <td>Aprenda a usar o ProcDump (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Shows/Defrag-Tools/Defrag-Tools-131-Windows-10-SDK">Configurar o ProcDump para criar arquivos de despejo</a></td>
    </tr>
</table>

### <a name="advanced-directx-techniques-and-concepts"></a>Conceitos e técnicas avançadas do DirectX

Algumas partes do desenvolvimento no DirectX podem ser sutis e complexas. Quando se atinge um ponto na produção em que você precisa investigar os detalhes de seu mecanismo DirectX ou depurar problemas difíceis de desempenho, os recursos e as informações desta seção podem ajudar.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>PIX no Windows</td>
        <td><a href="https://blogs.msdn.microsoft.com/pix/2017/01/17/introducing-pix-on-windows-beta/">Ferramenta de ajuste de desempenho e depuração para DirectX 12 no Windows</a></td>
    </tr>
    <tr>
        <td>Ferramentas de depuração e validação para o desenvolvimento de D3D12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-003">D3D12 ajuste de desempenho e depuração com validação de PIX e GPUValidation</a></td>
    </tr>
    <tr>
        <td>Otimizando elementos gráficos e desempenho (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Advanced-DirectX12-Graphics-and-Performance">Elementos gráficos DirectX 12 e desempenho avançados</a></td>
    </tr>
    <tr>
        <td>Depuração de elementos gráficos do DirectX (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Solve-the-Tough-Graphics-Problems-with-your-Game-Using-DirectX-Tools">Resolver problemas difíceis de elementos gráficos em seu jogo usando as ferramentas do DirectX</a></td>
    </tr>
    <tr>
        <td>Ferramentas do Visual Studio 2015 para depuração do DirectX 12 (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Series/ConnectOn-Demand/212">Ferramentas do DirectX para Windows 10 no Visual Studio 2015</a></td>
    </tr>
    <tr>
        <td>Guia de programação do Direct3D 12</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/dn903821">Guia de programação do Direct3D 12</a></td>
    </tr>
    <tr>
        <td>Combinando DirectX e XAML</td>
        <td><a href="directx-and-xaml-interop.md">Interoperabilidade entre DirectX e XAML</a></td>
    </tr>
</table>

### <a name="high-dynamic-range-hdr-content-development"></a>Desenvolvimento de conteúdo HDR (Alto Alcance Dinâmico)

Crie o conteúdo do jogo que usa os recursos de cor completos de HDR.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Introdução a HDR e conceitos de cores (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/P4061">Destaque de HDR e cor avançada no DirectX</a></td>
    </tr>
    <tr>
        <td>Saiba como renderizar o conteúdo HDR e detectar se a exibição atual dá suporte a ele.</td>
        <td><a href="https://github.com/Microsoft/DirectX-Graphics-Samples/tree/master/Samples/UWP/D3D12HDR">Amostra de HDR</a></td>
    </tr>
    <tr>
        <td>Criar e configurar uma cor avançada usando DirectX</td>
        <td><a href="https://github.com/Microsoft/Windows-universal-samples/tree/master/Samples/D2DAdvancedColorImages">Amostra de renderização de imagem de cor avançada do Direct2D</a></td>
    </tr>   
</table>


### <a name="globalization-and-localization"></a>Globalização e localização

Desenvolva jogos prontos para o mundo para a plataforma Windows e saiba mais sobre os recursos internacionais incorporados aos principais produtos da Microsoft.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Preparando seu jogo para o mercado global</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/xaml/mt186453.aspx">Diretrizes ao desenvolver para um público global</a></td>
    </tr>
    <tr>
        <td>Transpondo idiomas, culturas e tecnologias</td>
        <td><a href="http://www.microsoft.com/Language/Default.aspx">Recurso online para convenções de idioma e a terminologia Microsoft padrão</a></td>
    </tr>
</table>

### <a name="security"></a>Segurança

Crie um ambiente no qual os jogadores possam jogar e competir de forma justa. Um jogo registrado no TruePlay é executado em um processo protegido, que reduz uma classe de ataques comuns. O sistema de monitoramento de jogos também ajuda a identificar os cenários de trapaças comuns. 

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Ferramentas para combater trapaças em jogos de computador</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/mt808781">TruePlay</a></td>
    </tr>
</table>

## <a name="submitting-and-publishing-your-game"></a>Enviando e publicando seu jogo

Os guias e as informações a seguir ajudam a tornar o processo de publicação e envio o mais simples possível.

### <a name="publishing"></a>Publicação

Você usará o [Partner Center](https://partner.microsoft.com/dashboard) para publicar e gerenciar seus pacotes de jogos.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Publicação de aplicativos do Partner Center</td>
        <td><a href="https://dev.windows.com/publish">Publique aplicativos do Windows</a></td>
    </tr>
    <tr>
        <td>Publicação avançada do parceiro (GDN)</td>
        <td><a href="https://developer.xboxlive.com/en-us/windows/documentation/Pages/home.aspx">Guia de publicação avançada do Partner Center</a></td>
    </tr>
    <tr>
        <td>Use o Azure Active Directory (AAD) para adicionar usuários à sua conta do Partner Center</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/publish/manage-account-users">Gerenciar usuários de contas</a></td>
    </tr>   
    <tr>
        <td>Classificando seu jogo (postagem de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2016/01/06/now-available-single-age-rating-system-to-simplify-app-submissions/">Fluxo de trabalho único para atribuir classificações etárias usando o sistema IARC</a></td>
    </tr>
</table>

#### <a name="packaging-and-uploading"></a>Empacotamento e upload

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Saiba como usar instalação de streaming e pacotes opcionais (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Build/2017/B8093">Distribuição de aplicativos UWP Nextgen: Criando componentizedapps extensível, fluxo capaz</a></td>
    </tr>
    <tr>
        <td>Divida e agrupe o conteúdo para habilitar instalação de streaming</td>
        <td><a href="../packaging/streaming-install.md">Instalação de streaming de aplicativo UWP</a></td>
    </tr>
    <tr>
        <td>Criar pacotes opcionais como conteúdo de jogo DLC</td>
        <td><a href="../packaging/optional-packages.md">Criação de pacotes opcionais e conjunto relacionado</a></td>
    </tr>
    <tr>
        <td>Empacotar seu jogo UWP</td>
        <td><a href="../packaging/index.md">Empacotando aplicativos</a></td>
    </tr>
    <tr>
        <td>Empacote seu jogo do DirectX UWP</td>
        <td><a href="package-your-windows-store-directx-game.md">Empacote seu jogo do DirectX UWP</a></td>
    </tr>
    <tr>
        <td>Empacotando seu jogo como um desenvolvedor terceirizado (postagem de blog)</td>
        <td><a href="https://blogs.windows.com/buildingapps/2015/12/15/building-an-app-for-a-3rd-party-how-to-package-their-store-app/">Criar pacotes carregáveis sem acesso à conta do fornecedor da loja</a></td>
    </tr>
    <tr>
        <td>Criando aplicativos e pacotes de aplicativo usando MakeAppx</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/packaging/create-app-package-with-makeappx-tool">Criar pacotes usando a ferramenta de empacotador de aplicativo MakeAppx.exe</a></td>
    </tr>
    <tr>
        <td>Assinando seus arquivos digitalmente com a SignTool</td>
        <td><a href="https://msdn.microsoft.com/library/windows/desktop/aa387764">Assinar arquivos e verificar assinaturas em arquivos usando a SignTool</a></td>
    </tr>    
    <tr>
        <td>Carregando e controlando a versão de seu jogo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148542">Carregar pacotes de aplicativo</a></td>
    </tr>
</table>


### <a name="policies-and-certification"></a>Políticas e certificação

Não deixe que problemas de certificação atrasem o lançamento de seu jogo. Aqui estão as políticas e os problemas comuns de certificação sobre os quais você deve estar ciente.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Contrato de Desenvolvedor de Aplicativos da Microsoft Store</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/hh694058">Contrato de Desenvolvedor de Aplicativos</a></td>
    </tr>
    <tr>
        <td>Políticas para publicar aplicativos na Microsoft Store</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/dn764944">Políticas da Microsoft Store</a></td>
    </tr>
    <tr>
        <td>Como evitar alguns problemas comuns de certificação de aplicativo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/jj657968">Evitar erros comuns de certificação</a></td>
    </tr>
</table>
 

### <a name="store-manifest-storemanifestxml"></a>Manifesto da Store (StoreManifest.xml)

O manifesto da loja (StoreManifest.xml) é um arquivo de configuração opcional que pode ser incluído no pacote do aplicativo. O manifesto da loja fornece recursos adicionais que não fazem parte do arquivo AppxManifest.xml. Por exemplo, você pode usar o manifesto da loja para bloquear a instalação de seu jogo se um dispositivo de destino não tiver o nível de recurso mínimo especificado do DirectX ou o mínimo especificado para a memória do sistema.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Esquema do manifesto da loja</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt617335">Esquema StoreManifest (Windows 10)</a></td>
    </tr>
</table>
 

## <a name="game-lifecycle-management"></a>Gerenciamento do ciclo de vida do jogo


Quando você termina o desenvolvimento e envia seu jogo, não é "fim de jogo". Você pode ter terminado o desenvolvimento da primeira versão, mas a jornada de seu jogo no mercado está apenas começando. Você pode monitorar o uso e os relatórios de erros, responder aos comentários dos usuários e publicar atualizações para seu jogo.

### <a name="partner-center-analytics-and-promotion"></a>Análises e promoções do partner Center

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análise do Partner Center</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148522">Analisar o desempenho do aplicativo</a></td>
    </tr>
    <tr>
        <td>Saiba como os clientes estão participando com os recursos do Xbox em seu jogo.</td>
        <td><a href="../publish/xbox-analytics-report.md">Relatório de análise do Xbox</a></td>
    </tr>
    <tr>
        <td>Respondendo às análises dos clientes</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt148546">Responder às críticas dos clientes</a></td>
    </tr>
    <tr>
        <td>Maneiras de promover seu jogo</td>
        <td><a href="https://dev.windows.com/store-promotion">Promova seus aplicativos</a></td>
    </tr>
</table>
 

### <a name="visual-studio-application-insights"></a>Visual Studio Application Insights

O Visual Studio Application Insights fornece análises de desempenho, telemetria e uso para seu jogo publicado. O Application Insights ajuda a detectar e solucionar problemas depois que seu jogo é lançado, a monitorar e aprimorar continuamente o uso e a entender como os jogadores continuam a interagir com seu jogo. O Application Insights funciona com a adição de um SDK ao seu aplicativo, que envia telemetria para o [portal do Azure](http://portal.azure.com/).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Análise de desempenho e uso do aplicativo</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-get-started/">Visual Studio Application Insights</a></td>
    </tr>
    <tr>
        <td>Habilitar o Application Insights em aplicativos do Windows</td>
        <td><a href="https://azure.microsoft.com/documentation/articles/app-insights-windows-get-started/">Application Insights para aplicativos do Windows Phone e da Store</a></td>
    </tr>
</table>


### <a name="third-party-solutions-for-analytics-and-promotion"></a>Soluções de terceiros para análises e promoções

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Entenda o comportamento dos jogadores usando o GameAnalytics</td>
        <td><a href="http://www.gameanalytics.com/">GameAnalytics</a></td>
    </tr>
    <tr>
        <td>Conecte seu jogo UWP ao Google Analytics</td>
        <td><a href="https://github.com/dotnet/windows-sdk-for-google-analytics">Obtenha o SDK do Windows para Google Analytics</a></td>
    </tr>
    <tr>
        <td>Saiba como usar o SDK do Windows para Google Analytics (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-the-Windows-SDK-for-Google-Analytics">Introdução ao SDK do Windows para Google Analytics</a></td>
    </tr>    
    <tr>
        <td>Use os anúncios de instalação de aplicativo do Facebook para promover seu jogo para os usuários do Facebook</td>
        <td><a href="https://github.com/Microsoft/winsdkfb">Obtenha o SDK do Windows para Facebook</a></td>
    </tr>
    <tr>
        <td>Saiba como usar os anúncios de instalação de aplicativo do Facebook (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/Windows/Windows-Developer-Day-Creators-Update/Getting-started-with-Facebook-App-Install-Ads">Introdução ao SDK do Windows para Facebook</a></td>
    </tr>
    <tr>
        <td>Use o Vungle para adicionar anúncios em vídeo aos seus jogos</td>
        <td><a href="https://v.vungle.com/sdk">Obtenha o SDK do Windows para Vungle</a></td>
    </tr>
</table>
 

### <a name="creating-and-managing-content-updates"></a>Criando e gerenciando atualizações de conteúdo

Para atualizar seu jogo publicado, envie um novo pacote do aplicativo com um número de versão mais alto. Depois que o pacote passar pelo processo de envio e certificação, ele será disponibilizado automaticamente para os clientes como uma atualização.

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Atualizando e controlando a versão de seu jogo</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Numeração de versão do pacote</a></td>
    </tr>
    <tr>
        <td>Orientação para gerenciamento de pacotes de jogos</td>
        <td><a href="https://msdn.microsoft.com/library/windows/apps/mt188602">Orientação para gerenciamento do pacote de aplicativo</a></td>
    </tr>
</table>


## <a name="adding-xbox-live-to-your-game"></a>Adicionando o Xbox Live ao seu jogo

O Xbox Live é uma rede de jogos de primeira linha que conecta milhões de jogadores em todo o mundo. Os desenvolvedores obtêm acesso aos recursos do Xbox Live que podem aumentar seu público de forma orgânica, incluindo presença do Xbox Live, placares, salvamento na nuvem, hubs de jogos, bate-papo de grupo, DVR de Jogos e muito mais.

> [!Note]
> Se você quiser desenvolver títulos habilitados para Xbox Live, existem várias opções disponíveis. Para obter informações sobre os vários programas, consulte [visão geral do programa de desenvolvedores](../xbox-live/developer-program-overview.md).

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral do Xbox Live</td>
        <td><a href="../xbox-live/index.md">Guia do desenvolvedor do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Saiba quais recursos estão disponíveis dependendo do programa</td>
        <td><a href="../xbox-live/developer-program-overview.md#feature-table">Visão geral do programa de desenvolvedores: tabela de recursos</a></td>
    </tr>
    <tr>
        <td>Links de recursos úteis para desenvolvimento de jogos no Xbox Live</td>
        <td><a href="../xbox-live/xbox-live-resources.md">Recursos do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Saiba como obter informações de serviços Xbox Live</td>
        <td><a href="../xbox-live/introduction-to-xbox-live-apis.md">Introdução às APIs do Xbox Live</a></td>
    </tr>
</table>


### <a name="for-developers-in-the-xbox-live-creators-program"></a>Para desenvolvedores no Programa de Criadores do Xbox Live

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral</td>
        <td><a href="../xbox-live/get-started-with-creators/get-started-with-xbox-live-creators.md">Introdução ao Programa de Criadores do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Adicionar o Xbox Live ao seu jogo</td>
        <td><a href="../xbox-live/get-started-with-creators/creators-step-by-step-guide.md">Guia passo a passo para integrar o Programa de Criadores do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Adicionar o Xbox Live ao seu jogo UWP criado com o Unity</td>
        <td><a href="../xbox-live/get-started-with-creators/develop-creators-title-with-unity.md">Comece a desenvolver um título do Programa de Criadores do Xbox Live com o mecanismo de jogo Unity</a></td>
    </tr>
    <tr>
        <td>Configurar sua área restrita de desenvolvimento</td>
        <td><a href="../xbox-live/get-started-with-creators/xbox-live-sandboxes-creators.md">Introdução às áreas restritas do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Configurar contas para testes</td>
        <td><a href="../xbox-live/get-started-with-creators/authorize-xbox-live-accounts.md">Autorizar contas do Xbox Live no seu ambiente de teste</a></td>
    </tr>
    <tr>
        <td>Exemplos para o Programa de Criadores do Xbox Live</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/CreatorsSDK">Amostras de código para desenvolvedores no Programa de Criadores</a></td>
    </tr>
    <tr>
        <td>Saiba como integrar experiências do Xbox Live entre plataformas em jogos UWP (vídeo)</td>
        <td><a href="https://channel9.msdn.com/Events/GDC/GDC-2017/GDC2017-005">Programa de Criadores do Xbox Live</a></td>
    </tr>  
</table>

### <a name="for-managed-partners-and-developers-in-the-idxbox-program"></a>Para parceiros gerenciados e desenvolvedores no programa ID@Xbox

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Visão geral</td>
        <td><a href="../xbox-live/get-started-with-partner/get-started-with-xbox-live-partner.md">Comece a usar o Xbox Live como um parceiro gerenciado ou um desenvolvedor ID</a></td>
    </tr>
    <tr>
        <td>Adicionar o Xbox Live ao seu jogo</td>
        <td><a href="../xbox-live/get-started-with-partner/partners-step-by-step-guide.md">Guia passo a passo para integrar o Xbox Live para parceiros gerenciados e membros do ID</a></td>
    </tr>
    <tr>
        <td>Adicionar o Xbox Live ao seu jogo UWP criado com o Unity</td>
        <td><a href="../xbox-live/get-started-with-partner/partner-unity-uwp-il2cpp.md">Adicionar suporte do Xbox Live ao Unity para UWP com back-end de script IL2CPP para membros do ID e parceiros gerenciados</a></td>
    </tr>
    <tr>
        <td>Configurar sua área restrita de desenvolvimento</td>
        <td><a href="../xbox-live/get-started-with-partner/advanced-xbox-live-sandboxes.md">Áreas restritas avançadas do Xbox Live</a></td>
    </tr>
    <tr>
        <td>Requisitos para jogos que usam o Xbox Live (GDN)</td>
        <td><a href="http://go.microsoft.com/fwlink/?LinkId=533217">Requisitos do Xbox para o Xbox Live no Windows 10</a></td>
    </tr>
    <tr>
        <td>Exemplos</td>
        <td><a href="https://github.com/Microsoft/xbox-live-samples/tree/master/Samples/ID%40XboxSDK">Amostras de código para desenvolvedores do ID@Xbox</a></td>
    </tr>  
    <tr>
        <td>Visão geral de desenvolvimento de jogos para o Xbox Live (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Developing-with-Xbox-Live-for-Windows-10">Desenvolvendo com o Xbox Live para Windows 10</a></td>
    </tr>
    <tr>
        <td>Associação de plataforma cruzada (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Xbox-Live-Multiplayer-Introducing-services-for-cross-platform-matchmaking-and-gameplay">Xbox Live Multiplayer: apresentando os serviços de associação de plataforma cruzada e jogabilidade</a></td>
    </tr>
    <tr>
        <td>Jogabilidade entre dispositivos no Fable Legends (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Fable-Legends-Cross-device-Gameplay-with-Xbox-Live">Fable Legends: jogabilidade entre dispositivos com o Xbox Live</a></td>
    </tr>
    <tr>
        <td>Estatísticas e conquistas no Xbox Live (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Best-Practices-for-Leveraging-Cloud-Based-User-Stats-and-Achievements-in-Xbox-Live">Práticas recomendadas para utilizar as estatísticas e as conquistas do usuário baseado em nuvem no Xbox Live</a></td>
    </tr>
</table>


## <a name="additional-resources"></a>Recursos adicionais

<table>
    <colgroup>
    <col width="50%" />
    <col width="50%" />
    </colgroup>
    <tr>
        <td>Vídeos de desenvolvimento de jogos</td>
        <td><a href="https://docs.microsoft.com/windows/uwp/gaming/game-development-videos">Vídeos das conferências principais, como GDC e //build</a></td>
    </tr>
    <tr>
        <td>Desenvolvimento de jogos independentes (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/New-Opportunities-for-Independent-Developers">Novas oportunidades para desenvolvedores independentes</a></td>
    </tr>
    <tr>
        <td>Considerações para dispositivos móveis com vários núcleos (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/Sustained-gaming-performance-in-multi-core-mobile-devices">Desempenho de jogos sustentado em dispositivos móveis com vários núcleos</a></td>
    </tr>
    <tr>
        <td>Desenvolvimento de jogos da área de trabalho do Windows 10 (vídeo)</td>
        <td><a href="http://channel9.msdn.com/Events/GDC/GDC-2015/PC-Games-for-Windows-10">Jogos de computador para Windows 10</a></td>
    </tr>
</table>



 

 

 
