---
author: JnHs
Description: "Se seu aplicativo usa um AdMediatorControl ou AdControl para exibir anúncios na barra de notificação, você pode aumentar sua taxa de preenchimento de anúncio e receita mostrando anúncios de afiliada da Microsoft em seu aplicativo."
title: "Distribuição gradual de pacote"
translationtype: Human Translation
ms.sourcegitcommit: ac9eed95edba99cdba914ff21b25383f35a20012
ms.openlocfilehash: 3ec642ef0a21d06b20cabb12a47d2abcfd19fe59

---

# Distribuição gradual de pacote

Quando você publica uma atualização para um envio, pode optar por distribuir gradualmente os pacotes atualizados para um percentual de clientes do seu aplicativo no Windows 10. Isso permite que você monitore comentários e dados de análise dos pacotes específicos para verificar se a atualização é necessária antes de implantá-la mais amplamente. Você pode aumentar a porcentagem (ou parar a atualização) a qualquer tempo sem precisar criar um novo envio. 

> **Importante** Suas seleções de distribuição se aplicam a todos os seus pacotes, mas se aplicarão somente aos clientes que executam versões do sistema operacional compatíveis com pacotes de pré-lançamento (Windows.Desktop versão 10586 ou posterior, Windows.Mobile versão 10586.63 ou posterior e Xbox), incluindo os clientes que obtêm o aplicativo via [Licenciamento gerenciado pela Loja (online)](organizational-licensing.md) pela [Windows Store for Business](https://www.microsoft.com/business-store). Ao usar a distribuição de pacote gradual, os clientes em versões anteriores do sistema operacional não receberão pacotes do envio mais recente até finalizar a distribuição de pacote, conforme descrito a seguir.

Observe que todos os seus clientes verão os detalhes da listagem da Loja que você inseriu com seu envio mais recente. As configurações de distribuição só se aplicam aos pacotes que os clientes recebem, tanto para novas aquisições quanto para atualizações para os clientes existentes.

> **Dica** A distribuição de pacote distribui pacotes para clientes aleatórios nas porcentagens especificadas. Para distribuir pacotes específicos aos clientes selecionados que você especificar, use pacotes de pré-lançamento.  Você também pode combinar distribuição com seus pacotes de pré-lançamento se quiser distribuir gradualmente uma atualização para um de seus grupos de versão de pré-lançamento.

## Definindo o percentual de distribuição

Você pode selecionar para implantar a atualização na página **Pacotes** de um envio atualizado. Para fazer isso, marque a caixa **Distribur a atualização gradualmente depois que esse envio for publicado (somente para clientes do Windows 10)**. Insira o percentual de clientes que devem receber a atualização quando o envio for publicado pela primeira vez. Por exemplo, você pode inserir 5 se quiser começar distribuindo a atualização só para uma pequena porcentagem de clientes do seu aplicativo.

Clique em **Atualizar** para salvar suas seleções. Depois que seu aplicativo concluir o processo de certificação, os pacotes serão distribuídos para clientes de acordo com a porcentagem especificada, tanto para novas aquisições quanto para atualizações para os clientes existentes.

## Ajustando a distribuição depois que o envio for publicado

Para ajustar a distribuição depois que o envio foi publicado, acesse a página Visão geral do seu aplicativo. Você pode arrastar o seletor para alterar o percentual de clientes que recebem os pacotes de seu envio mais recente. Clique em **Atualizar** para salvar suas seleções. Os pacotes começarão a ser distribuídos para clientes de acordo com a porcentagem especificada, tanto para novas aquisições quanto para atualizações para os clientes existentes.

## Concluindo a distribuição

Antes de criar um novo envio, você precisará concluir a distribuição de pacote. Você pode **finalizar** a distribuição e distribuir os pacotes mais recentes para todos os seus clientes, ou **interromper** a distribuição para parar de distribuir os pacotes mais recentes.

Se você tiver confiança na atualização e quiser disponibilizá-la para todos os seus clientes, clique em **Finalizar a distribuição de pacote** para distribuir os pacotes mais recentes para todos os seus clientes.

> **Dica** Alterar a porcentagem de distribuição para 100% não garante que todos os clientes receberão os pacotes dos envios mais recentes, pois alguns clientes podem usar versões do sistema operacional que não suportam distribuição. Você deve finalizar a distribuição para parar de distribuir os pacotes mais antigos e atualizar todos os clientes existentes para os mais recentes.

Se você achar que há problemas com a atualização e não quiser mais distribuí-la, clique em **Interromper distribuição de pacote** parar parar de distribuir pacotes do envio mais recente. Depois que você interromper uma distribuição do pacote, esses pacotes não serão distribuídos para os clientes; apenas os pacotes do envio anterior serão usados para todos os clientes novos ou da atualização. No entanto, os clientes que já tinham os pacotes mais recentes manterão esses pacotes; eles não voltarão para a versão anterior. Para fornecer uma atualização para esses clientes, você precisará criar um novo envio com os pacotes que deseja que eles tenham. Observe que, se você usar uma distribuição gradual em seu próximo envio, os clientes que tinham o pacote interrompido receberão a nova atualização na mesma ordem em que receberam o pacote interrompido. A nova distribuição será entre seu último envio finalizado e seu envio mais recente. Depois que você interromper uma distribuição do pacote, esses pacotes não serão mais distribuídos para os clientes.



<!--HONumber=Aug16_HO5-->


