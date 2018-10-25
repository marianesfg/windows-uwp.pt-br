---
author: jnHs
Description: Once you've created your app by reserving a name, you can start working on getting it published. The first step is to create a submission.
title: Envios de aplicativos
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de verificação, windows, uwp, envio, enviar, jogo, app, envio
ms.author: wdg-dev-content
ms.date: 08/07/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
ms.localizationpriority: medium
ms.openlocfilehash: 9802577f9252b590657406bcb59b0c28adeb4781
ms.sourcegitcommit: 2c4daa36fb9fd3e8daa83c2bd0825f3989d24be8
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 10/25/2018
ms.locfileid: "5515272"
---
# <a name="app-submissions"></a>Envios de aplicativos


Assim que tiver [criado seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md), você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um **envio**.

Você pode iniciar o envio quando seu aplicativo estiver completo e pronto para ser publicado, ou pode começar a inserir informações mesmo antes de ter escrito uma única linha de código. Atualizações feitas ao seu envio são salvas, para que você possa voltar e trabalhar nele sempre que você está pronto.

> [!NOTE]
> Você deve ter uma [conta de desenvolvedor](http://go.microsoft.com/fwlink/p/?LinkId=615100) para acessar o [Centro de desenvolvimento do Windows](https://partner.microsoft.com/dashboard) e enviar aplicativos na Microsoft Store.

Depois que o aplicativo for publicado, você poderá publicar uma versão atualizada criando outro envio em seu painel. Criar um novo envio permite fazer e publicar quaisquer mudanças necessárias, caso você esteja carregando novos pacotes ou apenas alterando detalhes, como o preço ou a categoria. Para criar um novo envio para um aplicativo publicado, clique em **Atualizar** ao lado do envio mais recente mostrado na página de visão geral do aplicativo. Você também pode [Remover um aplicativo da loja](guidance-for-app-package-management.md#removing-an-app-from-the-store) se você precisar fazer isso (e, em seguida, disponibilizá-lo novamente mais tarde, se você quiser).

> [!NOTE]
> Esta seção da documentação descreve como criar um envio de aplicativo no painel do Centro de Desenvolvimento. Opcionalmente, você poderá usar a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de apps.

## <a name="app-submission-checklist"></a>Lista de verificação de envio de aplicativo

Aqui estão os detalhes que você pode fornecer quando for criar um envio de aplicativo, com links para mais informações.

Os itens que você será solicitado a fornecer ou especificar são indicados abaixo. Algumas áreas são opcionais ou têm valores padrão fornecidos que podem ser alterados conforme desejar. Você não precisa trabalhar com essas seções na ordem listada aqui.

### <a name="pricing-and-availability-page"></a>Página de preços e disponibilidade
| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercados**                   | Padrão: todos os mercados possíveis  | [Definir preço e seleção de mercado](define-pricing-and-market-selection.md)         |
| **Público**                | Padrão: Audiência pública | [Público](choose-visibility-options.md#audience) |
| **Detectabilidade**                | Padrão: Make this app available and discoverable in the Store | [Detectabilidade](choose-visibility-options.md#discoverability) |
| **Agendar**                  | Padrão: Liberar o mais rápido possível        | [Configurar o agendamento preciso do lançamento](configure-precise-release-scheduling.md) |
| **Preço base**                | Necessário                                    | [Definir e agendar preço do aplicativo](set-and-schedule-app-pricing.md)              |
| **Avaliação gratuita**                | Padrão: Nenhuma avaliação gratuita                      | [Avaliação gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Preço de venda**              | Opcional                                    | [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md)           |
| **Licenciamento para organizações**    | Padrão: permitir aquisição de volume por organizações | [Opções de licenciamento organizacional](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página de propriedades

| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoria e subcategoria**  | Necessário                                    | [Tabela de categoria e subcategoria](category-and-subcategory-table.md)       |
| **URL da política de privacidade**            | Obrigatório para muitos apps. Consulte o [Contrato de Desenvolvedor de Aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) e as [Políticas da Microsoft Store](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [URL da política de privacidade](enter-app-properties.md#privacy-policy-url)        |
| **Site**                   | Opcional                                    | [Site](enter-app-properties.md#website)                   |
| **Informações de contato de suporte**      | Obrigatório se seu produto estiver disponível no Xbox; caso contrário, opcional (mas recomendado)                                   | [Informações de contato de suporte](enter-app-properties.md#support-contact-info)              |
| **Configurações do jogo**             | Opcional (aplicável somente a jogos)         | [Configurações do jogo](enter-app-properties.md#game-settings) |
| **Modo de exibição**             | Opcional                   | [Modo de exibição](enter-app-properties.md#display-mode) |
| **Declarações do produto**          | Padrão: os clientes podem instalar esse aplicativo em drives alternativos ou armazenamento removível; o Windows pode incluir os dados desse aplicativo em backups automáticos do OneDrive. | [Declarações do produto](app-declarations.md) |
| **Requisitos do sistema**      | Opcional                                    | [Requisitos do sistema](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Página de classificações etárias

| Nome do campo                    | Observações                                       | Para obter mais informações                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classificações etárias**               | Necessário                                    | [Classificações etárias](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de pacotes

| Nome do campo                    | Observações                                  | Para obter mais informações                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Controle de carregamento de pacote**    | Obrigatório (pelo menos um pacote)        | [Carregue os pacotes do aplicativo](upload-app-packages.md) |
| **Disponibilidade da família de dispositivos** | Padrão: com base em seus pacotes       | [Disponibilidade da família de dispositivos](device-family-availability.md) |
| **Distribuição gradual de pacote**   | Opcional (somente para atualizações)            | [Distribuição gradual de pacote](gradual-package-rollout.md) |
| **Atualização obrigatória**          | Opcional (somente para atualizações)            | [Atualização obrigatória](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Listagens da Loja

Você precisará de todas as informações obrigatórias para pelo menos um dos idiomas para os quais seu aplicativo oferece suporte. É recomendável fornecer [Listagens da Store](create-app-store-listings.md) em todos os idiomas compatíveis com seu aplicativo, e você também poderá [fornecer listagens da Store em outros idiomas](create-app-store-listings.md#store-listing-languages). Para facilitar o gerenciamento de várias listagens para o mesmo produto, você poderá [importar e exportar listagens da Store](import-and-export-store-listings.md).

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descrição**               | Necessário                                    | [Escreva uma ótima descrição do app](write-a-great-app-description.md) |
| **Novidades desta versão**   | Opcional                                 | [Notas de versão](create-app-store-listings.md#whats-new-in-this-version)       |
| **Recursos do aplicativo**              | Opcional                                    | [Recursos do aplicativo](create-app-store-listings.md#app-features)         |
| **Capturas de tela**               | Obrigatório (pelo menos uma captura de tela; recomenda-se quatro ou mais)          | [Capturas de tela](app-screenshots-and-images.md#screenshots)          |
| **Logotipos da Store**               | Recomendado; necessário em algumas versões do sistema operacional | [Logotipos da Store](app-screenshots-and-images.md#store-logos)             |
| **Ativos de arte adicionais**     | Recomendado (especialmente para algumas versões do sistema operacional)         | [Ativos de arte adicionais](app-screenshots-and-images.md#additional-art-assets) |
| **Trailers**                  | Opcional                                    | [Trailers](app-screenshots-and-images.md#trailers)                | 
| **Campos complementares**  | Opcional                                    | [Informações complementares](create-app-store-listings.md#supplemental-fields) 
| **Termos de pesquisa**              | Opcional                                    | [Termos de pesquisa](create-app-store-listings.md#search-terms)         |
| **Informações sobre direitos autorais e marcas registradas** | Opcional                                 | [Informações sobre direitos autorais e marcas registradas](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termos de licença adicionais**  | Opcional                                    | [Termos de licença adicionais](create-app-store-listings.md#additional-license-terms) |
| **Desenvolvido por**              | Opcional                                    | [Desenvolvido por](create-app-store-listings.md#developed-by)                   |
| **Listagens da Store específicas de plataforma** | Opcional                               | [Criar listagens da Store específicas de plataforma](create-platform-specific-store-listings.md)  |

<span/>

### <a name="submission-options-page"></a>Página de opções de envio

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opções de suspensão de publicação**     | Padrão: Publicar este envio assim que ele for aprovado na certificação (ou de acordo com as datas selecionadas por você na seção Agendar)      | [Opções de suspensão de publicação](manage-submission-options.md#publishing-hold-options)    
| **Notas para certificação**     | Recomendações          | [Notas para certificação](notes-for-certification.md)             |
| **Funcionalidades restritas**     | Obrigatório se seu produto declara [funcionalidades restritas](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Funcionalidades restritas](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obter informações sobre como publicar aplicativos de linha de negócios (LOB) diretamente para empresas, consulte [Distribuir aplicativos LOB às empresas](distribute-lob-apps-to-enterprises.md).
