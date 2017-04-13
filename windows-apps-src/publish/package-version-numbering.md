---
author: jnHs
Description: "A Windows Store impõe certas regras relacionadas a números de versão, que funcionam de maneira um pouco diversa em diferentes versões do sistema operacional."
title: "Numeração de versão do pacote"
ms.assetid: DD7BAE5F-C2EE-44EE-8796-055D4BCB3152
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.openlocfilehash: 00131a10892e80f3bd81384fa80fe39915b17ec8
ms.sourcegitcommit: 909d859a0f11981a8d1beac0da35f779786a6889
translationtype: HT
---
# <a name="package-version-numbering"></a>Numeração de versão do pacote


Cada pacote que você fornece precisa ter um número de versão (fornecido como um valor no atributo **Version** do elemento **Package/Identity** no manifesto do aplicativo). A Windows Store impõe certas regras relacionadas a números de versão, que funcionam de maneira um pouco diversa em diferentes versões do sistema operacional.

> **Observação**  Este tópico se refere a "pacotes", mas, a menos que seja especificado, as mesmas regras aplicam-se a números de versão para arquivos .appx e .appxbundle.

## <a name="version-numbering-for-windows-10-packages"></a>Numeração de versão para pacotes do Windows 10


O número de versão de qualquer pacote do Windows 10 deve ser sempre maior do que qualquer número de versão para pacotes do Windows 8, do Windows 8.1 e/ou do Windows Phone 8.1 que você for publicar (ou pacotes para essas versões de SO que você já publicou) para o mesmo aplicativo. (Para obter mais informações, consulte [Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app)).

> **Importante**  A última (quarta) seção do número da versão é reservada para uso da Loja e deve ser deixada como 0 quando você compilar o pacote (embora a Loja possa alterar o valor nessa seção).

Quando for escolher um pacote do Windows 10 de seu envio publicado, a Windows Store sempre usará o pacote com versão mais alta que for aplicável ao dispositivo do cliente. Isso proporciona maior flexibilidade e coloca você no controle de quais pacotes serão fornecidos para clientes em tipos específicos de dispositivos. Importante, você pode enviar esses pacotes em qualquer ordem. Você não está limitado a fornecer pacotes com maior número de versão em cada envio subsequente.

Você pode até oferecer vários pacotes do Windows 10 com o mesmo número de versão. Contudo, os pacotes que compartilharem um número de versão não poderão ter também a mesma arquitetura, pois a identidade completa que a Loja usa para cada um de seus pacotes deve ser única. Para obter mais informações, consulte [**Identity**](https://msdn.microsoft.com/library/windows/apps/br211441).

Se você fornecer vários pacotes do Windows 10 que usam o mesmo número de versão, a arquitetura (na ordem x64, x86, ARM, neutro) será usada para decidir qual tem a maior classificação na consideração de qual pacote fornecer para determinado dispositivo. Ao classificar lotes de aplicativos que usem o mesmo número de versão, a classificação de arquitetura mais alta dentro do lote será considerada: um lote de aplicativos que contém um pacote x64 terá uma classificação maior do que aquele que contém somente um pacote x86.

Isso dá uma grande flexibilidade para desenvolver seu aplicativo ao longo do tempo. Você pode carregar e enviar novos pacotes que usam números de versão menores para adicionar suporte a dispositivos acessíveis aos quais não oferecia suporte anteriormente. Você pode adicionar pacotes com maior número de versão com dependências mais estritas para aproveitar o hardware ou os recursos do sistema operacional, ou pode adicionar pacotes com maior número de versão que servem como atualizações para alguns ou toda a sua base de clientes existentes.

O exemplo a seguir ilustra como a numeração de versão pode ser gerenciada para a entrega dos pacotes pretendidos para seus clientes em vários envios.

### <a name="example-moving-to-a-single-package-over-multiple-submissions"></a>Exemplo: Movendo para um único pacote em vários envios

O Windows 10 permite que você escreva uma única base de código que é executada em todos os lugares. Isso torna muito mais fácil iniciar um novo projeto de plataforma cruzada. No entanto, por várias razões, não convém mesclar bases de código existente para criar um único projeto imediatamente.

Você pode usar as regras de versão de pacotes para mover gradualmente seus clientes a um único pacote para a família de dispositivos Universal, enquanto envia atualizações interinas para famílias de dispositivos específicas (incluindo as que se beneficiam com APIs do Windows 10) O exemplo abaixo ilustra como as mesmas regras são aplicadas consistentemente.

| Envio | Conteúdo                                                  | Experiência do cliente                                                                                                                                                                             |
|------------|-----------------------------------------------------------|-------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------|
| 1          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.0.0 <br> -   Família de dispositivos: Windows.Mobile, minVersion 10.0.10240.0     | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10240.0 e superior receberão 1.1.0.0 <br> -   Outras famílias de dispositivos não poderá comprar e instalar o aplicativo |
| 2          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.0.0 <br> -   Família de dispositivos: Windows.Mobile, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10240.0 e superior receberão 1.1.0.0 <br> -   Outras famílias de dispositivos (que não sejam móveis, nem desktop) receberão 1.0.0.0 quando forem introduzidas <br> -   Os dispositivos móveis e desktop que já tiverem o aplicativo instalado não verão nenhuma atualização (porque já têm a melhor versão disponível: 1.1.10.0 e 1.1.0.0 são maiores do que 1.0.0.0) |
| 3          | -   Versão do pacote: 1.1.10.0 <br> -   Família de dispositivos: Windows.Desktop, minVersion 10.0.10240.0 <br> <br> -   Versão do pacote: 1.1.5.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10250.0 <br> <br> -   Versão do pacote: 1.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0    | -   Dispositivos na compilação Windows 10 Desktop 10.0.10240.0 e superior receberão 1.1.10.0 <br> -   Dispositivos na compilação Windows 10 Mobile 10.0.10250.0 e superior receberão 1.1.5.0 <br> -   Dispositivos na compilação Windows 10 Mobile >= 10.0.10240.0 e < 10.010250.0 receberão 1.1.0.0 
| 4          | -   Versão do pacote: 2.0.0.0 <br> -   Família de dispositivos: Windows.Universal, minVersion 10.0.10240.0   | -   Todos os clientes em todas as famílias de dispositivos na compilação Windows 10 v10.0.10240.0 e superior receberão pacotes 2.0.0.0 | 

> **Observação**  Em todos os casos, os dispositivos dos clientes receberão o pacote que tem o maior número de versão possível para o qual se qualificam. Por exemplo, no terceiro envio acima, todos os dispositivos da área de trabalho obterão v1.1.10.0, mesmo se eles tiverem a versão de sistema operacional 10.0.10250.0 ou superior e, portanto, também podem aceitar v1.1.5.0. Como 1.1.10.0 é o maior número de versão disponível para eles, esse é o pacote que eles receberão.

### <a name="using-version-numbering-to-roll-back-to-a-previously-shipped-package-for-new-acquisitions"></a>Usando a numeração de versão para reverter para um pacote enviado anteriormente para novas aquisições

Se você mantém cópias de seus arquivos de pacotes do Windows 10 anteriores, poderá voltar o pacote de seu aplicativo na Loja para um pacote anterior do Windows 10, caso descubra problemas em uma versão. Essa é uma maneira temporária de limitar a interrupção para seus clientes enquanto você corrige o problema.

Para fazer isso, crie um novo envio. Remova o pacote problemático e carregue o pacote antigo que você deseja fornecer na Loja. Os clientes que já receberam o pacote que você está revertendo ainda terão o pacote problemático (já que seu pacote mais antigo terá um número de versão anterior). Mas isso impedirá que qualquer um adquira o pacote problemático, enquanto permite que o aplicativo fique disponível na Loja.

Para consertar os problemas para os clientes que já receberam o pacote defeituoso, você pode enviar um novo pacote do Windows 10 com um número de versão maior do que o pacote ruim assim que possível. Depois que esse envio passar pelo processo de certificação, todos os clientes serão atualizados para o novo pacote, já que ele terá um número de versão superior.

## <a name="version-numbering-for-windows-81-and-earlier-and-windows-phone-81-packages"></a>Numeração de versão para pacotes do Windows 8.1 (e anterior) e Windows Phone

Para pacotes .appx destinados ao Windows Phone 8.1, o número da versão do pacote em um novo envio deve sempre ser maior do que o pacote incluído no último envio (ou qualquer envio anterior).

Para pacotes .appx destinados a Windows 8 e Windows 8.1, a mesma regra se aplica por arquitetura: o número de versão do pacote em um novo envio deve ser sempre maior do que o do último pacote enviado para a Windows Store para a mesma arquitetura.

Além disso, o número de versão dos pacotes do Windows 8.1 devem ser sempre maiores do que os números de versão de seus pacotes do Windows 8 para o mesmo aplicativo. Em outras palavras, o número de versão de qualquer pacote do Windows 8 que você enviar deve ser menor do que o número de versão de qualquer pacote do Windows 8.1 que você enviar para o mesmo aplicativo.

> **Observação**  Se você também tiver pacotes do Windows 10, o número de versão dos pacotes do Windows 10 deverão ser maiores do que os dos pacotes do Windows 8, do Windows 8.1, e/ou do Windows Phone 8.1 que você for publicar ou tiver publicado. (Para obter mais informações, consulte [Adicionando pacotes para Windows 10 a um aplicativo publicado anteriormente](guidance-for-app-package-management.md#adding-packages-for-windows-10-to-a-previously-published-app)).

Aqui estão alguns exemplos do que acontece em diferentes situações de atualização de números de versão para Windows 8 e Windows 8.1

| Com esta versão do aplicativo na Loja  | E você carrega essa versão | Depois que a nova versão estiver na Windows Store, ela será instalada em uma nova aquisição | Depois que a nova versão estiver na Windows Store, ela será atualizada se um cliente já tiver o aplicativo |
|---------------------------------------------|-----------------------------|--------------------------------------------------------------------------------------------|----------|
| Nada                                     | x86, v1.0.0.0               | x86, v1.0.0.0 em computadores x86 e x64                                                | Nada. |
| x86, v1.0.0.0                               | x64, v1.0.0.0               | v1.0.0.0 para a arquitetura do cliente                                                   | Nada. Os números de versão são os mesmos. |
| x86, v1.0.0.0 <br> x64, v1.0.0.0            | x64, v1.0.0.1               | v1.0.0.0 para os clientes com x86 <br> v1.0.0.1 para os clientes com x64                 | Nada para os clientes que executam o aplicativo em um computador x86. <br> v1.0.0.0 será atualizada para v1.0.0.1 para os clientes que executarem o aplicativo em um computador x64. <br> **Observação**  Se a versão x86 do aplicativo estiver sendo executada em um computador x64, o aplicativo não será atualizado para a versão x64 a menos que o cliente desinstale e reinstale. |
| Nada                                     | neutra, v1.0.0.1           | neutra, v1.0.0.1 em todos os computadores                                                         | Nada. |
| neutra, v1.0.0.1                           | x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | v1.0.0.0 para a arquitetura do computador do cliente.          | Nada. Quem tiver a versão neutra v1.0.0.1 do aplicativo continuará a usá-la. |
| neutra, v1.0.0.1 <br> x86, v1.0.0.0 <br> x64, v1.0.0.0 <br> ARM, v1.0.0.0 | x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | v1.0.0.1 para a arquitetura do computador do cliente. | Nada para os clientes que executam a versão v1.0.0.1 neutra do aplicativo. <br> v1.0.0.0 será atualizada para v1.0.0.1 nos clientes que executam a v1.0.0.0 do aplicativo compilado para a arquitetura específica do computador. |
| x86, v1.0.0.1 <br> x64, v1.0.0.1 <br> ARM, v1.0.0.1 | x86, v1.0.0.2 <br> x64, v1.0.0.2 <br> ARM, v1.0.0.2 | v1.0.0.2 para a arquitetura do computador do cliente.  | v1.0.0.1 será atualizada para v1.0.0.2 para os clientes que estiverem executando a v1.0.0.1 da compilação do aplicativo para a arquitetura específica de seus computadores. |
 
> **Observação**  Diferentemente dos pacotes. appx, os números de versão em todos os pacotes .xap não são considerados ao determinar qual pacote fornecer a um determinado cliente. Para atualizar um cliente de um pacote .xap para um mais recente, certifique-se de remover o .xap mais antigo no novo envio.
