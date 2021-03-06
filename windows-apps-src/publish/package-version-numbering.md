---
Description: A Microsoft Store impõe certas regras relacionadas a números de versão, que funcionam de maneira um pouco diversa em diferentes versões do sistema operacional.
title: Numeração de versão do pacote
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: d5ac50cdba542de56ffb1daee01da12d4979ac45
ms.sourcegitcommit: 96b7be654a0922eeb421b5fa51ebfc586abe74fe
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 06/17/2020
ms.locfileid: "84945967"
---
# <a name="package-version-numbering"></a>Numeração de versão do pacote

Cada pacote que você fornece precisa ter um número de versão (fornecido como um valor no atributo **Version** do elemento [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do aplicativo). A Microsoft Store impõe certas regras relacionadas a números de versão, que funcionam de maneira um pouco diversa em diferentes versões do sistema operacional.

> [!NOTE]
> Este tópico refere-se a "pacotes", mas, a menos que indicado, as mesmas regras se aplicam aos números de versão dos arquivos. msix/. Appx e. msixbundle/. appxbundle.


## <a name="version-numbering-for-windows10-packages"></a>Numeração de versão para pacotes do Windows 10

> [!IMPORTANT]
> Para pacotes do Windows 10 (UWP), a última seção (quarta) do número de versão é reservada para o uso do repositório e deve ser deixada como 0 quando você cria seu pacote (embora a loja possa alterar o valor nesta seção). As outras seções devem ser definidas como um inteiro entre 0 e 65535 (exceto para a primeira seção, que não pode ser 0).

Ao escolher um pacote UWP de seu envio publicado, o Microsoft Store sempre usará o pacote com versão mais alta aplicável ao dispositivo Windows 10 do cliente. Isso proporciona maior flexibilidade e coloca você no controle de quais pacotes serão fornecidos para clientes em tipos específicos de dispositivos. Importante, você pode enviar esses pacotes em qualquer ordem. Você não está limitado a fornecer pacotes com maior número de versão em cada envio subsequente.

Você pode fornecer vários pacotes UWP com o mesmo número de versão. Contudo, os pacotes que compartilharem um número de versão não poderão ter também a mesma arquitetura, pois a identidade completa que a Loja usa para cada um de seus pacotes deve ser única. Para obter mais informações, consulte [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Quando você fornece vários pacotes UWP que usam o mesmo número de versão, a arquitetura (na ordem x64, x86, ARM, neutro) será usada para decidir qual deles é de classificação mais alta (quando a loja determina qual pacote deve ser fornecido ao dispositivo de um cliente). Ao classificar lotes de aplicativos que usem o mesmo número de versão, a classificação de arquitetura mais alta dentro do lote será considerada: um lote de aplicativos que contém um pacote x64 terá uma classificação maior do que aquele que contém somente um pacote x86.

Isso dá uma grande flexibilidade para desenvolver seu aplicativo ao longo do tempo. Você pode carregar e enviar novos pacotes que usam números de versão menores para adicionar suporte a dispositivos Windows 10 dos quais você não tinha suporte anteriormente, você pode adicionar pacotes com versão mais alta que têm dependências mais rígidas para aproveitar os recursos de hardware ou de sistema operacional, ou você pode adicionar pacotes com versão mais alta que servem como atualizações para algumas ou todas as suas bases de clientes existentes.

O exemplo a seguir ilustra como a numeração de versão pode ser gerenciada para a entrega dos pacotes pretendidos para seus clientes em vários envios.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Exemplo: Movendo para um único pacote em vários envios

O Windows 10 permite que você escreva uma única base de código que é executada em todos os lugares. Isso torna muito mais fácil iniciar um novo projeto de plataforma cruzada. No entanto, por várias razões, não convém mesclar bases de código existente para criar um único projeto imediatamente.

Você pode usar as regras de versão de pacotes para mover gradualmente seus clientes a um único pacote para a família de dispositivos Universal, enquanto envia atualizações interinas para famílias de dispositivos específicas (incluindo as que se beneficiam com APIs do Windows 10) O exemplo a seguir ilustra como as mesmas regras são aplicadas consistentemente em uma série de envios para o mesmo aplicativo.

| Envio | Sumário                                                  | Experiência do usuário                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.0.0 <br> -   Família de dispositivos: Windows.Mobile, minVersion 10.0.10240.0     | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10240.0 e superior receberão 1.1.0.0 <br> -   Outras famílias de dispositivos não poderá comprar e instalar o aplicativo |
| 2          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.0.0 <br> -   Família de dispositivos: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10240.0 e superior receberão 1.1.0.0 <br> -   Outras famílias de dispositivos (que não sejam móveis, nem desktop) receberão 1.0.0.0 quando forem introduzidas <br> -   Os dispositivos móveis e desktop que já tiverem o aplicativo instalado não verão nenhuma atualização (porque já têm a melhor versão disponível: 1.1.10.0 e 1.1.0.0 são maiores do que 1.0.0.0) |
| 3          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.5.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Versão do pacote: 1.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10250.0 e superior receberão 1.1.5.0 <br> -   Dispositivos na compilação Windows 10 Mobile >= 10.0.10240.0 e < 10.010250.0 receberão 1.1.0.0 
| 4          | -   Versão do pacote: 2.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0   | -   Todos os clientes em todas as famílias de dispositivos na compilação Windows 10 v10.0.10240.0 e superior receberão pacotes 2.0.0.0 | 

> [!NOTE]
>  Em todos os casos, os dispositivos clientes receberão o pacote com o maior número possível de versão a que eles se qualificam. Por exemplo, no terceiro envio acima, todos os dispositivos da área de trabalho obterão v1.1.10.0, mesmo se eles tiverem a versão de sistema operacional 10.0.10250.0 ou superior e, portanto, também podem aceitar v1.1.5.0. Como 1.1.10.0 é o maior número de versão disponível para eles, esse é o pacote que eles receberão.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Usando a numeração de versão para reverter para um pacote enviado anteriormente para novas aquisições

Se você mantiver cópias de seus pacotes, terá a opção de reverter o pacote do aplicativo na loja para um pacote anterior do Windows 10 se você precisar descobrir problemas com uma versão. Essa é uma maneira temporária de limitar a interrupção para seus clientes enquanto você demora para corrigir o problema.

Para fazer isso, crie um novo [envio](app-submissions.md). Remova o pacote problemático e carregue o pacote antigo que você deseja fornecer na Loja. Os clientes que já receberam o pacote que você está revertendo ainda terão o pacote problemático (já que seu pacote mais antigo terá um número de versão anterior). Mas isso impedirá que qualquer um adquira o pacote problemático, enquanto permite que o aplicativo fique disponível na Loja.

Para corrigir o problema para os clientes que já receberam o pacote problemático, você pode enviar um novo pacote do Windows 10 que tenha um número de versão mais alto do que o pacote defeituoso assim que possível. Depois que esse envio passar pelo processo de certificação, todos os clientes serão atualizados para o novo pacote, já que ele terá um número de versão superior.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Numeração de versão para pacotes do Windows 8.1 (e anterior) e Windows Phone

> [!IMPORTANT]
> Você não pode mais carregar novos pacotes XAP criados usando os SDKs do Windows Phone 8. x. Os aplicativos que já estão em armazenamento com pacotes XAP continuarão a funcionar em dispositivos Windows 10 Mobile. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

Para pacotes .appx destinados ao Windows Phone 8.1, o número da versão do pacote em um novo envio deve sempre ser maior do que o pacote incluído no último envio (ou qualquer envio anterior).

Para pacotes .appx destinados a Windows 8 e Windows 8.1, a mesma regra se aplica por arquitetura: o número de versão do pacote em um novo envio deve ser sempre maior do que o do último pacote publicado na Store para a mesma arquitetura.

Além disso, o número de versão dos pacotes do Windows 8.1 devem ser sempre maiores do que os números de versão de seus pacotes do Windows 8 para o mesmo aplicativo. Em outras palavras, o número de versão de qualquer pacote do Windows 8 que você enviar deve ser menor do que o número de versão de qualquer pacote do Windows 8.1 que você enviar para o mesmo aplicativo.

> [!NOTE]
> Se seu aplicativo também tiver pacotes do Windows 10, o número de versão dos pacotes do Windows 10 deve ser maior do que aqueles para qualquer um dos seus pacotes do Windows 8, Windows 8.1 e/ou Windows Phone 8,1. Para obter mais informações, consulte [adicionando pacotes para o Windows 10 a um aplicativo publicado anteriormente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Aqui estão alguns exemplos do que acontece em diferentes cenários de atualização de número de versão para pacotes destinados ao Windows 8 e Windows 8.1.

| Com esta versão do aplicativo na Loja  | E você carrega essa versão | Depois que a nova versão estiver na Store, ela será instalada em uma nova aquisição | Depois que a nova versão estiver na Store, ela será atualizada se um cliente já tiver o aplicativo |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nada                                     | x86, v1.0.0.0               | x86, v1.0.0.0 em computadores x86 e x64                                                | Nada. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 para a arquitetura do cliente                                                   | Nada. Os números de versão são os mesmos. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 para os clientes com x86 <br> v1.0.0.1 para os clientes com x64                 | Nada para os clientes que executam o aplicativo em um computador x86. <br> v1.0.0.0 será atualizada para v1.0.0.1 para os clientes que executarem o aplicativo em um computador x64. <br> **Observação**    Se a versão x86 do aplicativo estiver em execução em um computador x64, o aplicativo não será atualizado para a versão x64, a menos que o cliente desinstale e reinstale o. |
| Nada                                     | neutra, v1.0.0.1           | neutra, v1.0.0.1 em todos os computadores                                                         | Nada. |
| neutra, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 para a arquitetura do computador do cliente.          | Nada. Quem tiver a versão neutra v1.0.0.1 do aplicativo continuará a usá-la. |
| neutra, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 para a arquitetura do computador do cliente. | Nada para os clientes que executam a versão v1.0.0.1 neutra do aplicativo. <br> v1.0.0.0 será atualizada para v1.0.0.1 nos clientes que executam a v1.0.0.0 do aplicativo compilado para a arquitetura específica do computador. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 para a arquitetura do computador do cliente.  | v1.0.0.1 será atualizada para v1.0.0.2 para os clientes que estiverem executando a v1.0.0.1 da compilação do aplicativo para a arquitetura específica de seus computadores. |

> [!NOTE]
> Diferentemente dos pacotes .appx, os números de versão em todos os pacotes .xap não são considerados ao determinar qual pacote fornecer a um cliente específico. Para atualizar um cliente de um pacote .xap para um mais recente, certifique-se de remover o .xap mais antigo no novo envio.
