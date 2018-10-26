---
author: drewbatgit
ms.assetid: DD8FFA8C-DFF0-41E3-8F7A-345C5A248FC2
description: Este tópico descreve como adicionar conteúdos de mídia protegida do PlayReady ao seu aplicativo da Plataforma Universal do Windows (UWP).
title: DRM do PlayReady
ms.author: drewbat
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 773216dc392f7bb234e232f3dd3e7c2190a22de1
ms.sourcegitcommit: 6cc275f2151f78db40c11ace381ee2d35f0155f9
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5543763"
---
# <a name="playready-drm"></a>DRM do PlayReady



Este tópico descreve como adicionar conteúdos de mídia protegida do PlayReady ao seu aplicativo da Plataforma Universal do Windows (UWP).

O DRM do PlayReady permite que desenvolvedores criem aplicativos UWP que possam fornecer conteúdo PlayReady ao usuário enquanto impõe regras de acesso definidas pelo provedor de conteúdo. Esta seção descreve as alterações feitas ao DRM do Microsoft PlayReady para Windows 10 e como modificar seu aplicativo UWP PlayReady para dar suporte a alterações feitas desde a versão anterior do Windows 8.1 para a versão do Windows 10.
 
| Tópico                                                                     | Descrição                                                                                                                                                                                                                                                                             |
|---------------------------------------------------------------------------|-----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| [DRM de hardware](hardware-drm.md)                                           | Este tópico fornece uma visão geral de como adicionar hardware do PlayReady com base em gerenciamento de direitos digitais (DRM) ao seu aplicativo UWP.                                                                                                                                                                 |
| [Streaming adaptável com PlayReady](adaptive-streaming-with-playready.md) | Este artigo descreve como adicionar streaming adaptável de conteúdo multimídia com proteção de conteúdo do Microsoft PlayReady a um aplicativo UWP (Plataforma Universal do Windows). Atualmente, esse recurso oferece suporte à reprodução de conteúdo HLS (Http Live Streaming) e DASH (Dynamic Streaming over HTTP). |

## <a name="whats-new-in-playready-drm"></a>Novidades no DRM do PlayReady

A lista a seguir descreve os novos recursos e as alterações feitas ao DRM do PlayReady para Windows 10.

-   Inclusão do gerenciamento de direitos digitais de hardware (HWDRM).

    Suporte de proteção de conteúdo baseado em hardware permite a reprodução segura de conteúdo em alta definição (HD) e ultra-alta definição (UHD) em vários dispositivos. O material de chave (incluindo chaves privadas, chaves de conteúdo e qualquer outro material de chave usado para derivar ou desbloquear essas chaves) e amostras de vídeo compactadas e não compactadas descriptografadas são protegidos ao aproveitar a segurança do hardware. Quando o DRM de Hardware está sendo usado, nenhum ativador desconhecido (reproduzir para desconhecido / reproduzir para desconhecido com downres) tem significado, pois o pipeline HWDRM sempre conhece a saída que está sendo usada. Para obter mais informações, consulte [DRM de hardware](hardware-drm.md).

-   O PlayReady não é mais um componente de estrutura appX, mas um componente nativo do sistema operacional. O namespace foi alterado de **Microsoft.Media.PlayReadyClient** para [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454).
-   Os seguintes cabeçalhos definindo os códigos de erro do PlayReady agora fazem parte do Software Development Kit do Windows (SDK do Windows): Windows.Media.Protection.PlayReadyErrors.h e Windows.Media.Protection.PlayReadyResults.h.
-   Fornece aquisição proativa de licenças não persistentes.

    Versões anteriores ao DRM do PlayReady não davam suporte à aquisição proativa de licenças não persistentes. Esse recurso foi adicionado nesta versão. Isso pode reduzir o tempo para o primeiro quadro. Para obter mais informações, consulte [Adquirir uma licença não persistente de forma proativa antes da reprodução](#proactively-acquire-a-non-persistent-license-before-playback).

-   Fornece a aquisição de várias licenças em uma mensagem.

    Permite que o aplicativo cliente adquira várias licenças não persistentes em uma mensagem de aquisição de licença. Isso pode reduzir o tempo para o primeiro quadro pela aquisição de licenças para várias partes de conteúdo, enquanto o usuário ainda estiver navegando em sua biblioteca de conteúdo; isso impede um atraso na aquisição de licença quando o usuário seleciona o conteúdo a ser reproduzido. Além disso, ele permite a criptografia dos fluxos de áudio e vídeo para chaves separadas, por meio da habilitação de um cabeçalho de conteúdo que inclui vários identificadores de chave (KIDs); isso permite que uma aquisição de licença simples para adquirir todas as licenças para todos os fluxos dentro de um arquivo de conteúdo em vez de usar uma lógica personalizada e várias solicitações de aquisição de licença para obter o mesmo resultado.

-   Suporte de vencimento em tempo real ou licença de duração limitada (LDL) adicionada.

    Fornece a capacidade de definir a expiração em tempo real em licenças e a transição uniforme de uma licença que esteja vencendo para outra licença (válida) no meio de reprodução. Quando combinado com aquisição de várias licenças em uma mensagem, isso permite que um aplicativo adquira várias LDLs de forma assíncrona enquanto o usuário ainda estiver navegando na biblioteca de conteúdo e adquira uma licença de duração mais longa depois que o usuário tiver selecionado conteúdo para a reprodução. A reprodução será iniciada de forma mais rápida (porque uma licença já está disponível) e, desde que o aplicativo tenha adquirido uma licença de duração mais longa no momento em que a LDL expirar, continuará a ser reproduzida uniformemente até o fim do conteúdo sem interrupção.

-   Inclusão de cadeias de licença não persistentes.
-   Inclusão de suporte a restrições baseadas em tempo (incluindo expiração, expiração depois da primeira opção e expiração em tempo real) de licenças não persistentes.
-   Inclusão de suporte à política do HDCP Tipo 1 (versão 2.2 no Windows 10).

    Consulte [O que deve ser considerado](#things-to-consider) para saber mais.

-   O método Miracast agora está implícito como uma saída.
-   Inclusão de parada segura.

    A parada segura fornece os meios para um dispositivo PlayReady afirmar com confiança para um serviço de streaming de mídia que a reprodução de mídia foi interrompida para determinada parte do conteúdo. Esse recurso garante que seus serviços de streaming de mídia forneçam relatórios e imposição precisos de limitações de uso em dispositivos diferentes para uma determinada conta.

-   Inclusão da separação de licença de áudio e vídeo.

    Faixas separadas impedem que o vídeo seja decodificado como áudio; ativar a proteção de conteúdo mais robusta. Padrões emergentes precisam de chaves separadas para faixas de áudio e vídeo.

-   MaxResDecode adicionado.

    Esse recurso foi adicionado para limitar a reprodução de conteúdo a uma resolução máxima mesmo quando houver uma chave de maior capacidade (mas não uma licença). Ele suporta casos em que vários tamanhos de fluxo são codificados com uma única chave.

As novas interfaces, classes e enumerações a seguir foram adicionadas ao DRM do PlayReady:

-   Interface [**IPlayReadyLicenseAcquisitionServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986077)
-   Interface [**IPlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986080)
-   Interface [**IPlayReadySecureStopServiceRequest**](https://msdn.microsoft.com/library/windows/apps/dn986090)
-   Classe [**PlayReadyLicenseSession**](https://msdn.microsoft.com/library/windows/apps/dn986309)
-   Classe [**PlayReadySecureStopIterable**](https://msdn.microsoft.com/library/windows/apps/dn986371)
-   Classe [**PlayReadySecureStopIterator**](https://msdn.microsoft.com/library/windows/apps/dn986375)
-   Enumerador [**PlayReadyHardwareDRMFeatures**](https://msdn.microsoft.com/library/windows/apps/dn986265)

Uma nova amostra foi criada para demonstrar como usar os novos recursos DRM do PlayReady. A amostra pode ser baixada de [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="things-to-consider"></a>O que deve ser considerado

-   O DRM do PlayReady agora dá suporte à HDCP Tipo 1 (compatível com HDCP versão 2.1 ou posterior). O PlayReady carrega uma política de restrição de tipo HDCP na licença para ser imposta pelo dispositivo. No Windows 10, essa política impõe que a HDCP 2.2 ou posterior seja ativada. Esse recurso pode ser habilitado na sua licença do servidor v3.0 SDK do PlayReady (o servidor controla essa política na licença usando o GUID da Restrição de Tipo HDCP). Para saber mais, consulte [Regras de conformidade e robustez do PlayReady](http://www.microsoft.com/playready/licensing/compliance/) (em inglês).
-   Não há suporte para o Windows Media Video (também conhecido como VC-1) no DRM de hardware (consulte [Substituir DRM de hardware](hardware-drm.md#override-hardware-drm)).
-   O DRM do PlayReady agora dá suporte ao padrão de compactação de vídeo de codificação de vídeo de alta eficiência (HEVC /H.265). Para dar suporte ao HEVC, seu aplicativo deve usar conteúdo de versão 2 do esquema de criptografia comum (CENC) que inclui deixar cabeçalhos de fração de conteúdo não criptografados. Para mais informações, consulte ISO/IEC 23001-7 Information technology -- MPEG systems technologies -- Part 7: Common encryption in ISO base media file format files (Versão de especificação ISO/IEC 23001-7:2015 ou superior necessária). A Microsoft também recomenda a utilização do CENC versão 2 para todo o conteúdo HWDRM. Além disso, alguns itens do DRM de hardware darão suporte ao HEVC, e outros não (consulte [Substituir DRM de hardware](hardware-drm.md#override-hardware-drm)).
-   Para usufruir determinados recursos novos do PlayReady 3.0 (incluindo, mas não se limitando a, SL3000 para clientes baseados em hardware, a aquisição de várias licenças não persistentes em uma mensagem de aquisição de licença e restrições baseadas em tempo em licenças não persistentes), é necessário que o servidor PlayReady seja o Microsoft PlayReady Server Software Development Kit v3.0.2769 ou posterior.
-   Dependendo da política de proteção de saída especificada na licença do conteúdo, pode ocorrer falha na reprodução de mídia para os usuários finais se sua saída conectada não oferecer suporte a esses requisitos. A tabela a seguir lista o conjunto de erros comuns que ocorrem como resultado. Para saber mais, consulte [Regras de conformidade e robustez do PlayReady](http://www.microsoft.com/playready/licensing/compliance/) (em inglês).

| Erro                                                   | Valor      | Descrição                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                 |
|---------------------------------------------------------|------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| ERROR\_GRAPHICS\_OPM\_OUTPUT\_DOES\_NOT\_SUPPORT\_HDCP  | 0xC0262513 | A Política de proteção de saída da licença exige que o monitor ative HDCP, mas HDCP não pôde ser ativada.                                                                                                                                                                                                                                                                                                                                                                                              |
| MF\_E\_POLICY\_UNSUPPORTED                              | 0xC00D7159 | A Política de proteção de saída da licença exige que o monitor ative HDCP Tipo 1, mas HDCP Tipo 1 não pôde ser ativada.                                                                                                                                                                                                                                                                                                                                                                                |
| DRM\_E\_TEE\_OUTPUT\_PROTECTION\_REQUIREMENTS\_NOT\_MET | 0x8004CD22 | Esse código de erro ocorre somente quando executado em DRM de hardware. A política de proteção de saída da licença requer que o monitor envolva a HDCP ou reduza a resolução efetiva do conteúdo, mas a HDCP não pôde ser envolvida e a resolução efetiva do conteúdo não pôde ser reduzida porque o DRM de hardware não dá suporte à redução de resolução do conteúdo. Em DRM de software, o conteúdo é reproduzido. Consulte [Considerações sobre como usar o DRM de hardware](hardware-drm.md#considerations-for-using-hardware-drm). |
| ERROR\_GRAPHICS\_OPM\_NOT\_SUPPORTED                    | 0xc0262500 | O driver de elemento gráfico não oferece suporte à proteção de saída. Por exemplo, o monitor está conectado por meio de VGA ou um driver de gráficos apropriado para saída digital não está instalado. No último caso, o driver típico instalado é o adaptador de vídeo básico da Microsoft e a instalação de um driver de elementos gráficos apropriado resolverá o problema.                                                                                                                                                  |

## <a name="output-protection"></a>Proteção de saída

A seção a seguir descreve o comportamento ao usar o DRM do PlayReady para Windows 10 com as políticas de proteção de saída em uma licença do PlayReady.

O DRM do PlayReady oferece suporte aos níveis de proteção de saída contidos na **Especificação de direitos de mídia extensível do Microsoft PlayReady**. Este documento pode ser encontrado no pacote de documentação que vem com produtos licenciados PlayReady.

> [!NOTE]
> Os valores permitidos para níveis de proteção de saída que podem ser definidos por um servidor de licenciamento são regidos pelas [Regras de conformidade do PlayReady (em inglês)](https://www.microsoft.com/playready/licensing/compliance/).

O DRM do PlayReady permite a reprodução de conteúdo com as políticas de proteção de saída somente em conectores de saída conforme especificado nas Regras de conformidade do PlayReady. Para saber mais sobre os termos do conector de saída especificado nas Regras de conformidade do PlayReady, consulte [Termos definidos para as Regras de conformidade e robustez do PlayReady](https://www.microsoft.com/playready/licensing/compliance/) (em inglês).

Esta seção se concentra em cenários de proteção de saída com o DRM do PlayReady para Windows 10 e o DRM de Hardware do PlayReady para o Windows 10, que também está disponível em alguns clientes do Windows. Com o HWDRM do PlayReady, todas as proteções de saída são impostas de dentro da implementação do TEE (Ambiente de Execução Confiável) do Windows (consulte [DRM de hardware](hardware-drm.md)). Como resultado, alguns comportamentos diferem ao usar o SWDRM do PlayReady (DRM de software):

* Suporte ao OPL (Nível de Proteção de Saída) para vídeo digital descompactado de 270: o HWDRM do PlayReady para Windows 10 não oferece suporte à resolução inferior e impõe que a HDCP (Proteção de Conteúdo Digital de Grande Largura de Banda) seja ativada. Recomenda-se que o conteúdo de alta definição para o HWDRM tenha um OPL superior a 270, embora não seja necessário. Além disso, você deve definir a restrição de tipo HDCP na licença (HDCP versão 2.2 ou posterior).
* Ao contrário do SWDRM, com o HWDRM, as proteções de saída são impostas em todos os monitores com base no monitor de menor capacidade. Por exemplo, se o usuário tiver dois monitores conectados, em que um dos monitores oferece suporte à uma HDCP e o outro não, haverá falha na reprodução se a licença exigir uma HDCP, mesmo se o conteúdo só estiver sendo renderizado no monitor que oferece suporte à HDCP. No SWDRM, o conteúdo será reproduzido contanto que esteja sendo renderizado somente no monitor que oferece suporte à HDCP.
* Não há garantia de que o HWDRM seja usado pelo cliente e que seja seguro, a menos que as seguintes condições sejam atendidas pelas licenças e chaves de conteúdo:
    * A licença usada para a chave de conteúdo de vídeo deve ter um nível mínimo de segurança de 3000.
    * O áudio deve ser criptografado em uma chave de conteúdo diferente daquela do vídeo, e a licença usada para áudio deve ter um nível mínimo de segurança de 2000. Como alternativa, o áudio pode não ser criptografado.
* Todos os cenários SWDRM exigem que o nível mínimo de segurança da licença do PlayReady usada para a chave de conteúdo de áudio e/ou vídeo seja menor ou igual a 2000.

### <a name="output-protection-levels"></a>Níveis de proteção de saída

A tabela a seguir descreve os mapeamentos entre vários OPLs na licença do PlayReady e como o DRM do PlayReady para Windows 10 os impõe.

#### <a name="video"></a>Vídeo

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Vídeo digital compactado</th>
        <th colspan="2">Vídeo digital não compactado</th>
        <th>TV analógica</th>
    </tr>
    <tr>
        <th>Qualquer</th>
        <th colspan="2">HDMI, DVI, DisplayPort, MHL</th>
        <th>Componente, composição</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="6">N/D\*</td>
        <td colspan="2">Passa o conteúdo</td>
        <td>Passa o conteúdo</td>
    </tr>
    <tr>
        <th>150</th>
        <td colspan="2" rowspan="2">N/D\*</td>
        <td>Passa o conteúdo quando o CGMS-A CopyNever está ativado ou se o CGMS-A não pode ser ativado</td>
    </tr>
    <tr>
        <th>200</th>
        <td>Passa o conteúdo quando o CGMS-A CopyNever está ativado</td>
    </tr>
    <tr>
        <th>250</th>
        <td colspan="2">Tenta acionar a HDCP, mas passa conteúdo independentemente do resultado</td>
        <td rowspan="5">N/D\*</td>
    </tr>
    <tr>
        <th>270</th>
        <td><b>SWDRM</b>: tenta acionar a HDCP. Se houver falha de ativação da HDCP, o computador limitará a resolução efetiva a 520.000 pixels por quadro e passará o conteúdo</td>
        <td><b>HWDRM</b>: passa conteúdo com a HDCP. Se houver falha de ativação da HDCP, a reprodução nas portas HDMI DVI será bloqueada</td>
    </tr>
    <tr>
        <th>300</th>
        <td colspan="2">
            <p>
                **Quando a restrição de tipo HDCP NÃO está definida:** passa conteúdo com a HDCP. Se houver falha de ativação da HDCP, a reprodução nas portas HDMI DVI será bloqueada.
            </p>
            <p>
                **Quando a restrição de tipo HDCP ESTÁ definida**: Passa o conteúdo com a HDCP 2.2 e o tipo de fluxo de conteúdo definido como 1. Se houver falha de ativação da HDCP ou o tipo de fluxo de conteúdo não puder ser definido como 1, a reprodução nas HDMI/DVI será bloqueada.
            </p>
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2">O Windows 10 nunca passa o conteúdo de vídeo digital compactado para saídas, independentemente do valor do OPL subsequente. Para saber mais sobre o conteúdo de vídeo digital compactado, consulte as <a href="https://www.microsoft.com/playready/licensing/compliance/">Regras de conformidade para os produtos PlayReady</a> (em inglês).</td>
        <td colspan="2" rowspan="2">N/D\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Nem todos os valores para níveis de proteção de saída podem ser definidos por um servidor de licenciamento. Para obter mais informações, consulte [Regras de conformidade do PlayReady](https://www.microsoft.com/playready/licensing/compliance/) (em inglês).

#### <a name="audio"></a>Áudio

<table>
    <tr>
        <th rowspan="2">OPL</th>
        <th>Áudio digital compactado</th>
        <th>Áudio digital não compactado</th>
        <th>Áudio analógico ou USB</th>
    </tr>
    <tr>
        <th>HDMI, DisplayPort, MHL</th>
        <th>HDMI, DisplayPort, MHL</th>
        <th>Qualquer</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="3">Passa o conteúdo</td>
        <td>Passa o conteúdo</td>
        <td rowspan="5">Passa o conteúdo</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="4">NÃO passa o conteúdo</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td>Passa o conteúdo quando a HDCP está ativada nas saídas HDMI, DisplayPort ou MHL, ou quando SCMS está ativada e definida como CopyNever</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Passa o conteúdo quando HDCP está ativada nas saídas HDMI, DisplayPort ou MHL</td>
    </tr>
</table>
<br/>

### <a name="miracast"></a>Miracast

O DRM do PlayReady permite reproduzir conteúdo pela saída Miracast assim que a HDCP 2.0 ou posterior for ativada. No Windows 10, no entanto, Miracast é considerada uma saída *digital*. Para saber mais sobre cenários de Miracast, consulte as [Regras de conformidade do PlayReady](https://www.microsoft.com/playready/licensing/compliance/) (em inglês). A tabela a seguir descreve os mapeamentos entre vários OPLs na licença do PlayReady e como o DRM do PlayReady os impõe nas saídas de Miracast.

<table>
    <tr>
        <th>OPL</th>
        <th>Áudio digital compactado</th>
        <th>Áudio digital não compactado</th>
        <th>Vídeo digital compactado</th>
        <th>Vídeo digital não compactado</th>
    </tr>
    <tr>
        <th>100</th>
        <td rowspan="4">Passa o conteúdo quando a HDCP 2.0 ou posterior está ativada. Se houver falha de ativação, o conteúdo NÃO será passado</td>
        <td>Passa o conteúdo quando a HDCP 2.0 ou posterior está ativada. Se houver falha de ativação, o conteúdo NÃO será passado</td>
        <td rowspan="6">N/D\*</td>
        <td>Passa o conteúdo quando a HDCP 2.0 ou posterior está ativada. Se houver falha de ativação, o conteúdo NÃO será passado</td>
    </tr>
    <tr>
        <th>150</th>
        <td rowspan="3">NÃO passa o conteúdo</td>
        <td rowspan="2">N/D\*</td>
    </tr>
    <tr>
        <th>200</th>
    </tr>
    <tr>
        <th>250</th>
        <td rowspan="2">Passa o conteúdo quando a HDCP 2.0 ou posterior está ativada. Se houver falha de ativação, o conteúdo NÃO será passado</td>
    </tr>
    <tr>
        <th>270</th>
        <td colspan="2">N/D\*</td>
    </tr>
    <tr>
        <th>300</th>
        <td>Passa o conteúdo quando a HDCP 2.0 ou posterior está ativada. Se houver falha de ativação, o conteúdo NÃO será passado</td>
        <td>NÃO passa o conteúdo</td>
        <td>
            <p>
                **Quando a restrição de tipo HDCP NÃO está definida:** passa conteúdo quando a HDCP 2.0 ou posterior está ativada. Se houver falha de ativação, o conteúdo NÃO será passado.
            </p>
            <p>
                **Quando a restrição de tipo HDCP ESTÁ definida:** passa o conteúdo com a HDCP 2.2 e o tipo de fluxo de conteúdo definido como 1. Se houver falha de ativação da HDCP ou o tipo de fluxo de conteúdo não puder ser definido como 1, o conteúdo NÃO será passado.
            </p>        
        </td>
    </tr>
    <tr>
        <th>400</th>
        <td rowspan="2" colspan="2">N/D\*</td>
        <td rowspan="2">O Windows 10 nunca passa o conteúdo de vídeo digital compactado para saídas, independentemente do valor do OPL subsequente. Para saber mais sobre o conteúdo de vídeo digital compactado, consulte as <a href="https://www.microsoft.com/playready/licensing/compliance/">Regras de conformidade para os produtos PlayReady</a> (em inglês).</td>
        <td rowspan="2">N/D\*</td>
    </tr>
    <tr>
        <th>500</th>
    </tr>
</table>
<br/>

\* Nem todos os valores para níveis de proteção de saída podem ser definidos por um servidor de licenciamento. Para obter mais informações, consulte [Regras de conformidade do PlayReady](https://www.microsoft.com/playready/licensing/compliance/) (em inglês).

### <a name="additional-explicit-output-restrictions"></a>Restrições adicionais de saída explícita

A tabela a seguir descreve a implementação das restrições de proteção de saída explícita de vídeo digital do DRM do PlayReady para Windows 10.

<table>
    <tr>
        <th>Cenário</th>
        <th>GUID</th>
        <th>Se...</th>
        <th>Então...</th>
    </tr>
    <tr>
        <th>Tamanho máximo efetivo de decodificação de resolução</th>
        <td>9645E831-E01D-4FFF-8342-0A720E3E028F</td>
        <td>A saída conectada é: saída de vídeo digital, Miracast, HDMI, DVI, etc.</td>
        <td>
            <p>
                Passa o conteúdo quando restrito a:  
            </p>
            <ul>
                <li>(a) a largura do quadro deve ser menor ou igual à largura máxima do quadro em pixels e a altura do quadro menor ou igual à altura máxima do quadro em pixels, ou</li>
                <li>(b) a altura do quadro deve ser menor ou igual à largura máxima do quadro em pixels e a largura do quadro menor ou igual à altura máxima do quadro em pixels</li>
            </ul>                   
        </td>
    </tr>
    <tr>
        <th>Restrição de tipo HDCP</th>
        <td>ABB2C6F1-E663-4625-A945-972D17B231E7</td>
        <td>A saída conectada é: saída de vídeo digital, Miracast, HDMI, DVI, etc.</td>
        <td>Passa conteúdo com a HDCP 2.2 e o tipo de fluxo de conteúdo definido como 1. Se houver falha de ativação da HDCP 2.2 ou o tipo de fluxo de conteúdo não puder ser definido como 1, o conteúdo NÃO será passado. O nível de proteção de saída de vídeo digital não compactado de um valor maior ou igual a 271 também deve ser especificado</td>
    </tr>
</table>
<br/>

A tabela a seguir descreve a implementação das restrições de proteção de saída explícita de vídeo analógico do DRM do PlayReady para Windows 10.

<table>
    <tr>
        <th>Cenário</th>
        <th>GUID</th>
        <th>Se...</th>
        <th colspan="2">Então...</th>
    </tr>
    <tr>
        <th>Monitor de computador analógico</th>
        <td>D783A191-E083-4BAF-B2DA-E69F910B3772</td>
        <td>A saída conectada é: VGA, DVI&ndash;analógico, etc.</td>
        <td><b>SWDRM:</b> o computador restringirá a resolução efetiva a 520.000 epx por quadro e passará conteúdo</td>
        <td><b>HWDRM:</b> NÃO passa conteúdo</td>
    </tr>
    <tr>
        <th>Componente analógico</th>
        <td>811C5110-46C8-4C6E-8163-C0482A15D47E</td>
        <td>A saída conectada é: componente</td>
        <td><b>SWDRM:</b> o computador restringirá a resolução efetiva a 520.000 epx por quadro e passará conteúdo</td>
        <td><b>HWDRM:</b> NÃO passa conteúdo</td>
    </tr>
    <tr>
        <th rowspan="2">Saídas de TV analógicas</th>
        <td>2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td>O OPL de TV analógica é menor que 151</td>
        <td colspan="2">CGMS-A deve estar ativado</td>
    </tr>
    <tr>
        <td>225CD36F-F132-49EF-BA8C-C91EA28E4369</td>
        <td>O OPL de TV analógica é menor que 101 e a licença não contém 2098DE8D-7DDD-4BAB-96C6-32EBB6FABEA3</td>
        <td colspan="2">Deve-se tentar a ativação do CGMS-A, mas o conteúdo pode reproduzido independentemente do resultado</td>
    </tr>
    <tr>
        <th>Controle de ganho automático e listra de cores</th>
        <td>C3FD11C6-F8B7-4D20-B008-1DB17D61F2DA</td>
        <td>Conteúdo de passagem com resolução menor ou igual a 520.000 px para saída de TV analógica</td>
        <td colspan="2">Define o AGC (Controle de Ganho Automático) somente para o componente de vídeo e o modo PAL quando a resolução for menor que 520.000 px e define o AGC e as informações de listra de cores para NTSC quando a resolução for menor que 520.000 px, de acordo com a tabela 3.5.7.3. nas Regras de conformidade</td>
    </tr>
    <tr>
        <th>Saída somente digital</th>
        <td>760AE755-682A-41E0-B1B3-DCDF836A7306</td>
        <td>A saída conectada é analógica</td>
        <td colspan="2">Não passa o conteúdo</td>
    </tr>
</table>
<br/>

> [!NOTE]
> Ao usar um adaptador dongle como o "Mini DisplayPort para VGA" para a reprodução, o Windows 10 vê a saída como saída de vídeo digital e não pode impor políticas de vídeo analógico.

A tabela a seguir descreve a implementação do DRM do PlayReady para Windows 10 que permite a reprodução em outras circunstâncias.

<table>
    <tr>
        <th>Cenário</th>
        <th>GUID</th>
        <th>Se...</th>
        <th colspan="2">Então...</th>
    </tr>
    <tr>
        <th>Saída desconhecida</th>
        <td>786627D8-C2A6-44BE-8F88-08AE255B01A7</td>
        <td>Se a saída não pode ser determinada de forma razoável ou o OPM não pode ser estabelecido com o driver de elementos gráficos</td>
        <td><b>SWDRM:</b> passa conteúdo</td>
        <td><b>HWDRM:</b> NÃO passa conteúdo</td>
    </tr>
    <tr>
        <th>Saída desconhecida com restrição</th>
        <td>B621D91F-EDCC-4035-8D4B-DC71760D43E9</td>
        <td>Se a saída não pode ser determinada de forma razoável ou o OPM não pode ser estabelecido com o driver de elementos gráficos</td>
        <td><b>SWDRM:</b> o computador restringirá a resolução efetiva a 520.000 epx por quadro e passará conteúdo</td>
        <td><b>HWDRM:</b> NÃO passa conteúdo</td>
    </tr>
</table>
<br/>

## <a name="prerequisites"></a>Pré-requisitos

Antes de começar a criar seu aplicativo UWP protegido pelo PlayReady, o seguinte software deve ser instalado no sistema:

-   Windows 10.
-   Se você estiver compilando alguma das amostras para o DRM do PlayReady para aplicativos UWP, você deve usar o Microsoft Visual Studio2015 ou posterior para compilar as amostras. Você ainda pode usar o Microsoft Visual Studio2013 para compilar qualquer uma das amostras do DRM do PlayReady para aplicativos do Windows 8.1 da loja.

<!--This is no longer available-->
<!--If you are planning to play back MPEG-2/H.262 content on your app, you must also download and install [Windows 8.1 Media Center Pack](http://go.microsoft.com/fwlink/p/?LinkId=626876).-->

## <a name="playready-uwp-app-migration-guide"></a>Guia de migração de aplicativos UWP da PlayReady

Esta seção inclui informações sobre como migrar seus aplicativos existentes do PlayReady Windows 8. x Store para Windows 10.

O namespace para aplicativos UWP do PlayReady no Windows 10 foi alterado de **Microsoft.Media.PlayReadyClient** para [**Windows.Media.Protection.PlayReady**](https://msdn.microsoft.com/library/windows/apps/dn986454). Isso significa que você precisará pesquisar e substituir o namespace antigo pelo novo em seu código. Você ainda fará referência a um arquivo winmd. Ele faz parte do winmd nos sistema operacional do Windows 10. Ele está em windows.winmd como parte do SDK do Windows do TH. Para UWP, ele está referenciado em windows.foundation.univeralappcontract.winmd.

Para reproduzir conteúdo HD (alta definição) (1080p) e UHD (ultra-alta definição) protegido por PlayReady, será necessário implementar o DRM de hardware do PlayReady. Para obter mais informações sobre como implementar o DRM de hardware do PlayReady, consulte [DRM de hardware](hardware-drm.md).

Não há suporte para parte do conteúdo no DRM de hardware. Para informações sobre como desabilitar o DRM de hardware e habilitar o DRM de software, consulte [Substituir DRM de hardware](hardware-drm.md#override-hardware-drm).

Com relação ao gerenciador de proteção de mídia, confira se seu código tem as seguintes configurações:

```cs
var mediaProtectionManager = new Windows.Media.Protection.MediaProtectionManager();

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemId"] = 
             '{F4637010-03C3-42CD-B932-B48ADF3A6A54}'
var cpsystems = new Windows.Foundation.Collections.PropertySet();
cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = 
                "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput";
mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;

mediaProtectionManager.Properties["Windows.Media.Protection.MediaProtectionContainerGuid"] = 
                "{9A04F079-9840-4286-AB92-E65BE0885F95}";
```

## <a name="proactively-acquire-a-non-persistent-license-before-playback"></a>Adquirir uma licença não persistente de forma proativa antes da reprodução

Esta seção descreve como adquirir licenças não persistentes de forma proativa antes do início da reprodução.

Em versões anteriores do PlayReady DRM, as licenças não persistentes eram adquiridas somente de forma reativa durante a reprodução. Nesta versão, você pode adquirir licenças não persistentes de forma proativa antes do início da reprodução.

1.  Crie de forma proativa uma sessão de reprodução em que a licença não persistente possa ser armazenada. Por exemplo:

    ```cs
    var cpsystems = new Windows.Foundation.Collections.PropertySet();       
    cpsystems["{F4637010-03C3-42CD-B932-B48ADF3A6A54}"] = "Windows.Media.Protection.PlayReady.PlayReadyWinRTTrustedInput"; // PlayReady

    var pmpSystemInfo = new Windows.Foundation.Collections.PropertySet();
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemId"] = "{F4637010-03C3-42CD-B932-B48ADF3A6A54}";
    pmpSystemInfo["Windows.Media.Protection.MediaProtectionSystemIdMapping"] = cpsystems;
    var pmpServer = new Windows.Media.Protection.MediaProtectionPMPServer( pmpSystemInfo );
    ```

2.  Vincule a sessão de reprodução à classe de aquisição de licença. Por exemplo:

    ```cs
    var licenseSessionProperties = new Windows.Foundation.Collections.PropertySet();
    licenseSessionProperties["Windows.Media.Protection.MediaProtectionPMPServer"] = pmpServer;
    var licenseSession = new Windows.Media.Protection.PlayReady.PlayReadyLicenseSession( licenseSessionProperties );
    ```

3.  Criar uma solicitação de serviço de licença. Por exemplo:

    ```cs
    var laSR = licenseSession.CreateLAServiceRequest();
    ```

4.  Executar a aquisição de licença usando a solicitação de serviço criada na etapa 3. A licença será armazenada na sessão de reprodução.
5.  Vincule a sessão de reprodução à origem de mídia para a reprodução. Por exemplo:

    ```cs
    licenseSession.configureMediaProtectionManager( mediaProtectionManager );
    videoPlayer.msSetMediaProtectionManager( mediaProtectionManager );
    ```
    
## <a name="query-for-protection-capabilities"></a>Consulta de recursos de proteção
A partir do Windows 10, versão 1703, é possível consultar recursos de HW DRM, como codecs de decodificação, resolução e proteções de saída (HDCP). Consultas são realizadas com o método [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported) que usa uma cadeia de caracteres representando os recursos para os quais o suporte é consultado e uma cadeia de caracteres especificando o sistema chave ao qual a consulta se aplica. Para obter uma lista dos valores de cadeia de caracteres com suporte, consulte a página de referência da API de [**IsTypeSupported**](https://docs.microsoft.com/uwp/api/windows.media.protection.protectioncapabilities.istypesupported). O exemplo de código a seguir ilustra o uso desse método.  

    ```cs
    using namespace Windows::Media::Protection;

    ProtectionCapabilities^ sr = ref new ProtectionCapabilities();

    ProtectionCapabilityResult result = sr->IsTypeSupported(
    L"video/mp4; codecs=\"avc1.640028\"; features=\"decode-bpp=10,decode-fps=29.97,decode-res-x=1920,decode-res-y=1080\"",
    L"com.microsoft.playready");

    switch (result)
    {
        case ProtectionCapabilityResult::Probably:
        // Queue up UHD HW DRM video
        break;

        case ProtectionCapabilityResult::Maybe:
        // Check again after UI or poll for more info.
        break;

        case ProtectionCapabilityResult::NotSupported:
        // Do not queue up UHD HW DRM video.
        break;
    }
    ```
## <a name="add-secure-stop"></a>Adicionar parada segura

Esta seção descreve como adicionar uma parada segura ao seu aplicativo UWP.

A parada segura fornece os meios para um dispositivo PlayReady afirmar com confiança para um serviço de streaming de mídia que a reprodução de mídia foi interrompida para determinada parte do conteúdo. Esse recurso garante que seus serviços de streaming de mídia forneçam relatórios e imposição precisos de limitações de uso em dispositivos diferentes para uma determinada conta.

Há dois cenários principais para enviar um desafio de parada segura:

-   Quando a apresentação de mídia é interrompida porque foi alcançado o fim do conteúdo ou quando o usuário interrompeu a apresentação de mídia em algum ponto intermediário.
-   Quando a sessão anterior é encerrada inesperadamente (por exemplo, devido a uma falha no sistema ou no aplicativo). O aplicativo precisará consultar, na inicialização ou no desligamento, qualquer sessão de parada segura pendente e enviar desafio(s) separadamente de qualquer outra reprodução de mídia.

Para obter uma implementação de exemplo de parada segura, consulte o arquivo securestop.cs no exemplo de PlayReady localizado em [http://go.microsoft.com/fwlink/p/?linkid=331670&clcid=0x409](http://go.microsoft.com/fwlink/p/?linkid=331670).

## <a name="use-playready-drm-on-xbox-one"></a>Usar PlayReady DRM em Xbox One

Para usar PlayReady DRM em um aplicativo UWP no Xbox One, você precisará registrar inicialmente a conta do Centro de Desenvolvimento que está usando para publicar o aplicativo para autorização a fim de usar o PlayReady. É possível fazer isso de duas maneiras:

* Fazer o contato na Microsoft solicitar permissão.
* Solicitar autorização enviando a conta do Centro de Desenvolvimento e o nome da empresa para [pronxbox@microsoft.com](mailto:pronxbox@microsoft.com).

Depois de receber a autorização, você precisará incluir um `<DeviceCapability>` adicional para o manifesto do aplicativo. Você precisará adicioná-lo manualmente porque não há configuração disponível no momento no Designer de Manifesto do Aplicativo. Siga estas etapas para configurá-lo:

1. Com o projeto aberto no Visual Studio, abra o **Gerenciador de Soluções** e clique com o botão direito do mouse em **Package. appxmanifest**.
2. Selecione **Abrir Com...**, escolha **Editor (Texto) de XML**e clique em **OK**.
3. Entre as marcas `<Capabilities>`, adicione o seguinte `<DeviceCapability>`:

    ```xml
    <DeviceCapability Name="6a7e5907-885c-4bcb-b40a-073c067bd3d5" />
    ```

4. Salve o arquivo.

Por fim, há uma última consideração durante o uso do PlayReady no Xbox One: em kits de desenvolvimento, existe um limite de SL150 (ou seja, eles não conseguem reproduzir conteúdo SL2000 ou SL3000). Os dispositivos de varejo são capazes de reproduzir conteúdo com níveis de segurança mais altos, mas para testar o aplicativo em um kit de desenvolvimento, você precisará usar conteúdo SL150. É possível testar esse conteúdo das seguintes maneiras:

* Use o conteúdo de teste coletado que exige licenças SL150.
* Implemente uma lógica de maneira que apenas determinadas contas de teste autenticadas sejam capazes de adquirir licenças SL150 para um determinado conteúdo.

Use a abordagem mais razoável para a empresa e o produto.


## <a name="see-also"></a>Consulte também
- [Reprodução de mídia](media-playback.md)




