---
Description: The Microsoft Store enforces certain rules related to version numbers, which work somewhat differently in different OS versions.
title: Numeração de versão do pacote
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 7a8ce14094733ef5598c510198f4268744cb581e
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928658"
---
# <a name="package-version-numbering"></a>Numeração de versão do pacote

Cada pacote que você fornece precisa ter um número de versão (fornecido como um valor no atributo **Version** do elemento [Package/Identity](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity) no manifesto do aplicativo). A Microsoft Store impõe certas regras relacionadas a números de versão, que funcionam de maneira um pouco diversa em diferentes versões do sistema operacional.

> [!NOTE]
> Este tópico se refere a "pacotes", mas a menos que indicado, as mesmas regras se aplicam aos números de versão para arquivos.msix/.appx e.msixbundle/.appxbundle.


## <a name="version-numbering-for-windows10-packages"></a>Numeração de versão para pacotes do Windows 10

> [!IMPORTANT]
> Para pacotes do Windows 10 (UWP), a última (quarta) seção do número da versão é reservada para uso da loja e deve ser deixada como 0 quando você compilar o pacote (embora a loja possa alterar o valor nessa seção). Outras seções devem ser definidas como um número inteiro entre 0 e 65535 (exceto a primeira seção, que não pode ser 0).

Ao escolher um pacote UWP do seu envio publicado, a Microsoft Store sempre usará o pacote maior número de versão que é aplicável para o dispositivo do cliente Windows 10. Isso proporciona maior flexibilidade e coloca você no controle de quais pacotes serão fornecidos para clientes em tipos específicos de dispositivos. Importante, você pode enviar esses pacotes em qualquer ordem. Você não está limitado a fornecer pacotes com maior número de versão em cada envio subsequente.

Você pode fornecer vários pacotes UWP com o mesmo número de versão. Contudo, os pacotes que compartilharem um número de versão não poderão ter também a mesma arquitetura, pois a identidade completa que a Loja usa para cada um de seus pacotes deve ser única. Para obter mais informações, consulte [**Identity**](https://docs.microsoft.com/uwp/schemas/appxpackage/uapmanifestschema/element-identity).

Quando você fornece vários pacotes UWP que usam o mesmo número de versão, a arquitetura (na ordem x64, x86, ARM, neutro) será usada para decidir qual é a maior classificação (quando o armazenamento determina qual pacote fornecer para o dispositivo do cliente). Ao classificar lotes de aplicativos que usem o mesmo número de versão, a classificação de arquitetura mais alta dentro do lote será considerada: um lote de aplicativos que contém um pacote x64 terá uma classificação maior do que aquele que contém somente um pacote x86.

Isso dá uma grande flexibilidade para desenvolver seu aplicativo ao longo do tempo. Você pode carregar e enviar novos pacotes que usam números de versão menores para adicionar suporte para dispositivos Windows 10 que não oferecia suporte anteriormente, você pode adicionar pacotes superiores dependências mais estritas para aproveitar o hardware ou recursos do sistema operacional ou você pode adicionar pacotes superiores que servem como atualizações para alguns ou todos os seus clientes existentes base.

O exemplo a seguir ilustra como a numeração de versão pode ser gerenciada para a entrega dos pacotes pretendidos para seus clientes em vários envios.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Exemplo: Movendo para um único pacote em vários envios

Windows 10 permite que você escreva uma única base de código que é executado em todos os lugares. Isso torna muito mais fácil iniciar um novo projeto de plataforma cruzada. No entanto, por várias razões, não convém mesclar bases de código existente para criar um único projeto imediatamente.

Você pode usar as regras de controle de versão de pacote para mover gradualmente seus clientes para um único pacote para a família de dispositivos Universal, enquanto um número de atualizações interinas para famílias de dispositivo específico (inclusive aqueles que tirar proveito das APIs do Windows 10). O exemplo a seguir ilustra como as mesmas regras são aplicadas consistentemente ao longo de uma série de envios para o mesmo aplicativo.

| Envio | Conteúdo                                                  | Experiência do cliente                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.0.0 <br> -   Família de dispositivos: Windows.Mobile, minVersion 10.0.10240.0     | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10240.0 e superior receberão 1.1.0.0 <br> -   Outras famílias de dispositivos não poderá comprar e instalar o aplicativo |
| 2          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.0.0 <br> -   Família de dispositivos: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10240.0 e superior receberão 1.1.0.0 <br> -   Outras famílias de dispositivos (que não sejam móveis, nem desktop) receberão 1.0.0.0 quando forem introduzidas <br> -   Os dispositivos móveis e desktop que já tiverem o aplicativo instalado não verão nenhuma atualização (porque já têm a melhor versão disponível: 1.1.10.0 e 1.1.0.0 são maiores do que 1.0.0.0) |
| 3          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.5.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Versão do pacote: 1.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10250.0 e superior receberão 1.1.5.0 <br> -   Dispositivos na compilação Windows 10 Mobile >= 10.0.10240.0 e < 10.010250.0 receberão 1.1.0.0 
| 4          | -   Versão do pacote: 2.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0   | -   Todos os clientes em todas as famílias de dispositivos na compilação Windows 10 v10.0.10240.0 e superior receberão pacotes 2.0.0.0 | 

> [!NOTE]
>  Em todos os casos, os dispositivos clientes receberão o pacote com o maior número possível de versão a que eles se qualificam. Por exemplo, no terceiro envio acima, todos os dispositivos da área de trabalho obterão v1.1.10.0, mesmo se eles tiverem a versão de sistema operacional 10.0.10250.0 ou superior e, portanto, também podem aceitar v1.1.5.0. Como 1.1.10.0 é o maior número de versão disponível para eles, esse é o pacote que eles receberão.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Usando a numeração de versão para reverter para um pacote enviado anteriormente para novas aquisições

Se você mantém cópias de seus pacotes, você terá a opção de reverter o pacote do aplicativo na loja para um pacote anterior do Windows 10 se você deve descobrir problemas com uma versão. Essa é uma maneira temporária de limitar a interrupção para seus clientes enquanto você dedicar tempo para corrigir o problema.

Para fazer isso, crie um novo [envio](app-submissions.md). Remova o pacote problemático e carregue o pacote antigo que você deseja fornecer na Loja. Os clientes que já receberam o pacote que você está revertendo ainda terão o pacote problemático (já que seu pacote mais antigo terá um número de versão anterior). Mas isso impedirá que qualquer um adquira o pacote problemático, enquanto permite que o aplicativo fique disponível na Loja.

Para corrigir o problema para os clientes que já receberam o pacote problemático, você pode enviar um novo pacote do Windows 10 que tem um número de versão maior do que o pacote ruim assim que possível. Depois que esse envio passar pelo processo de certificação, todos os clientes serão atualizados para o novo pacote, já que ele terá um número de versão superior.


## <a name="version-numbering-for-windows81-and-earlier-and-windows-phone-81-packages"></a>Para Windows 8.1 (e versões anteriores) de numeração de versão e pacotes do Windows Phone 8.1

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, produtos recém-criado não podem incluir pacotes que segmentem 8.x/Windows do Windows Phone 8. x ou anterior. Para obter mais informações, consulte esta [postagem de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

Para pacotes .appx destinados ao Windows Phone 8.1, o número da versão do pacote em um novo envio deve sempre ser maior do que o pacote incluído no último envio (ou qualquer envio anterior).

Para pacotes. AppX destinados a Windows8 e Windows 8.1, a mesma regra se aplica por arquitetura: o número da versão do pacote em um novo envio deve ser sempre maior do que o último pacote publicado na Store para a mesma arquitetura.

Além disso, o número de versão dos pacotes do Windows 8.1 sempre deve ser maior que os números de versão de qualquer um dos seus pacotes Windows8 para o mesmo aplicativo. Em outras palavras, o número de versão de qualquer pacote Windows8 que você envia deve ser menor do que o número de versão de qualquer pacote do Windows 8.1 que você enviou para o mesmo aplicativo.

> [!NOTE]
> Se seu aplicativo também tiver pacotes do Windows 10, o número de versão dos pacotes do Windows 10 deve ser maior do que aqueles para qualquer um dos seus pacotes Windows8, Windows 8.1 e/ou Windows Phone 8.1. Para obter mais informações, consulte [adicionando pacotes para Windows 10 para um aplicativo publicado anteriormente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app).

Aqui estão alguns exemplos do que acontece em situações atualização números de versão diferentes para pacotes Windows8 e Windows 8.1.

| Com esta versão do aplicativo na Loja  | E você carrega essa versão | Depois que a nova versão estiver na Store, ela será instalada em uma nova aquisição | Depois que a nova versão estiver na Store, ela será atualizada se um cliente já tiver o aplicativo |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nada                                     | x86, v1.0.0.0               | x86, v1.0.0.0 em computadores x86 e x64                                                | Nada. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 para a arquitetura do cliente                                                   | Nada. Os números de versão são os mesmos. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 para os clientes com x86 <br> v1.0.0.1 para os clientes com x64                 | Nada para os clientes que executam o aplicativo em um computador x86. <br> v1.0.0.0 será atualizada para v1.0.0.1 para os clientes que executarem o aplicativo em um computador x64. <br> **Observação**se a versão do aplicativo está em execução em x64 de x86 computador, o aplicativo não será atualizado para x64 versão, a menos que o cliente desinstale e reinstale. |
| Nada                                     | neutra, v1.0.0.1           | neutra, v1.0.0.1 em todos os computadores                                                         | Nada. |
| neutra, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 para a arquitetura do computador do cliente.          | Nada. Quem tiver a versão neutra v1.0.0.1 do aplicativo continuará a usá-la. |
| neutra, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 para a arquitetura do computador do cliente. | Nada para os clientes que executam a versão v1.0.0.1 neutra do aplicativo. <br> v1.0.0.0 será atualizada para v1.0.0.1 nos clientes que executam a v1.0.0.0 do aplicativo compilado para a arquitetura específica do computador. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 para a arquitetura do computador do cliente.  | v1.0.0.1 será atualizada para v1.0.0.2 para os clientes que estiverem executando a v1.0.0.1 da compilação do aplicativo para a arquitetura específica de seus computadores. |

> [!NOTE]
> Diferentemente dos pacotes .appx, os números de versão em todos os pacotes .xap não são considerados ao determinar qual pacote fornecer a um cliente específico. Para atualizar um cliente de um pacote .xap para um mais recente, certifique-se de remover o .xap mais antigo no novo envio.
