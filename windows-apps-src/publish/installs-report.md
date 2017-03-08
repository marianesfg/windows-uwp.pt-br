---
author: shawjohn
Description: "O relatório de instalações no painel do Centro de Desenvolvimento do Windows permite que você veja quantas vezes seu app foi instalado com êxito em dispositivos Windows 10."
title: "Relatório de instalações"
ms.author: johnshaw
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, app, instalações, instalação, relatório, análises"
ms.assetid: 46c08fd2-00bd-4be5-b29f-01a3b5fea4c2
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 9e4dec61e275443db42b50ef9a231b5b3c46ffe4
ms.lasthandoff: 02/08/2017

---

# <a name="installs-report"></a>Relatório de instalações

O relatório de **instalações** no painel do Centro de Desenvolvimento do Windows permite que você veja quantas vezes seu app foi instalado com êxito pelos clientes em dispositivos Windows 10. É possível exibir esses dados no painel ou [baixar o relatório](download-analytic-reports.md) a fim de exibi-lo off-line.


## <a name="apply-filters"></a>Aplicar filtros


Na parte superior da página, você pode expandir os **Aplicar filtros** para filtrar todos os dados dessa página por datas, tipo de dispositivo e/ou por versão de pacote.

-   **Data**: o filtro padrão é **Últimos 30 dias**, mas você pode expandi-lo até **Últimos 12 meses**.
-   **Tipo de dispositivo**: o filtro padrão é **Todos os dispositivos**, mas você pode selecionar um tipo de dispositivo específico (**computador**, **telefone**, **tablet**, **máquina virtual**, **IoT**, **holográfico**, **console**, **Outros**, ou **Desconhecido**).
-   **Versão do pacote**: o filtro padrão é **Todas as versões**, mas é possível selecionar uma versão do pacote específica.


## <a name="installs-daily"></a>Instala diariamente


O gráfico **Instala diariamente** mostra o número total de instalações diárias do seu app durante o período de tempo selecionado.

O total de instalação inclui:
-   **Instalações em vários dispositivos Windows 10.** Por exemplo, se um usuário instalar seu app em dois computadores com Windows 10 e um telefone Windows 10, isso será considerado como três instalações.
-   **Reinstalações.** Por exemplo, se um cliente instalar o app hoje, desinstalar seu app amanhã e, em seguida, reinstalar o app próximo mês, isso contará como duas instalações.

O total de instalação não inclui ou reflete:
-   **Instalações em dispositivos não Windows 10.** Por exemplo, se um usuário instalar seu app em um dispositivo que não esteja executando o Windows 10, nós não contaremos essa instalação.
-   **Desinstalações.** Por exemplo, se um cliente desinstalar seu app, nós não subtrairemos do número total de instalações.
-   **Atualizações.** Por exemplo, se o usuário instalar seu app hoje e instalar uma atualização do app uma semana depois, isso será considerado como apenas uma instalação (não duas).
-   **Pré-instalações.** Por exemplo, se um cliente comprar um dispositivo que tenha o seu app pré-instalado, nós não contaremos isso como uma instalação.
-   **Instalações iniciadas pelo sistema.** Por exemplo, se o Windows instalar o app automaticamente por algum motivo, nós não contaremos isso como uma instalação.

> **Observação**  No momento, você não pode recuperar de forma programática dados de **Instala diariamente** por meio de uma API.

## <a name="markets"></a>Mercados


O gráfico **Mercados** mostra o número total de instalações durante o período de tempo selecionado por mercado. Por padrão, mostraremos dados de todos os mercados. No entanto, você pode filtrar isso por um mercado específico.


## <a name="package-version"></a>Versão do pacote


O gráfico **Versão do pacote** mostra o número total de instalações durante o período de tempo selecionado por versão do pacote.



 

 

