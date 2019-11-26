---
Description: Assim que tiver criado seu aplicativo reservando um nome, você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um envio.
title: Envios de aplicativos
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de verificação, windows, uwp, envio, enviar, jogo, app, envio
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: bed7f232c8ec59771c6ae80a48b12bab1307ad68
ms.sourcegitcommit: b52ddecccb9e68dbb71695af3078005a2eb78af1
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/20/2019
ms.locfileid: "74260031"
---
# <a name="app-submissions"></a>Envios de aplicativos


Assim que tiver [criado seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md), você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um **envio**.

Você pode iniciar o envio quando seu aplicativo estiver completo e pronto para ser publicado, ou pode começar a inserir informações mesmo antes de ter escrito uma única linha de código. As atualizações feitas no envio são salvas, para que você possa voltar e trabalhar nela sempre que estiver pronto.

> [!NOTE]
> Você deve ter uma [conta de desenvolvedor](https://developer.microsoft.com/store/register) ativa no [Partner Center](https://partner.microsoft.com/dashboard) para enviar aplicativos para o Microsoft Store.

Depois que o aplicativo for publicado, você poderá publicar uma versão atualizada criando outro envio no Partner Center. Criar um novo envio permite fazer e publicar quaisquer mudanças necessárias, caso você esteja carregando novos pacotes ou apenas alterando detalhes, como o preço ou a categoria. Para criar um novo envio para um aplicativo publicado, clique em **Atualizar** ao lado do envio mais recente mostrado em sua página de **visão geral** . Você também pode [remover um aplicativo da loja](guidance-for-app-package-management.md#removing-an-app-from-the-store) se precisar fazê-lo (e, em seguida, disponibilizá-lo novamente mais tarde, se desejar).

> [!NOTE]
> Esta seção da documentação descreve como criar um envio de aplicativo no Partner Center. Opcionalmente, você poderá usar a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de apps.

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, os produtos recém-criados não podem incluir pacotes destinados ao Windows 8. x/Windows Phone 8. x ou anterior. Para obter mais informações, consulte esta [postagem no blog](https://blogs.windows.com/windowsdeveloper/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store).

## <a name="app-submission-checklist"></a>Lista de verificação de envio de aplicativo

Aqui estão os detalhes que você pode fornecer quando for criar um envio de aplicativo, com links para mais informações.

Os itens que você será solicitado a fornecer ou especificar são indicados abaixo. Algumas áreas são opcionais ou têm valores padrão fornecidos que podem ser alterados conforme desejar. Você não precisa trabalhar nessas seções na ordem listada aqui.

### <a name="pricing-and-availability-page"></a>Página de preços e disponibilidade
| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercados**                   | Padrão: todos os mercados possíveis  | [Definir a seleção de preços e mercado](define-pricing-and-market-selection.md)         |
| **Platéia**                | Padrão: Audiência pública | [Platéia](choose-visibility-options.md#audience) |
| **Descoberta**                | Padrão: Make this app available and discoverable in the Store | [Descoberta](choose-visibility-options.md#discoverability) |
| **Agendamento**                  | Padrão: Liberar o mais rápido possível        | [Configurar o agendamento de liberação preciso](configure-precise-release-scheduling.md) |
| **Preço base**                | Obrigatório                                    | [Definir e agendar os preços do aplicativo](set-and-schedule-app-pricing.md)              |
| **Avaliação gratuita**                | Padrão: Nenhuma avaliação gratuita                      | [Avaliação gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Preço de venda**              | Opcional                                    | [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md)           |
| **Licenciamento organizacional**    | Padrão: permitir aquisição de volume por organizações | [Opções de licenciamento organizacional](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página de propriedades

| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoria e subcategoria**  | Obrigatório                                    | [Categoria e tabela de subcategorias](category-and-subcategory-table.md)       |
| **URL da política de privacidade**            | Obrigatório para muitos apps. Consulte o [Contrato de Desenvolvedor de Aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) e as [Políticas da Microsoft Store](store-policies.md#105-personal-information) | [URL da política de privacidade](enter-app-properties.md#privacy-policy-url)        |
| **Site**                   | Opcional                                    | [Site](enter-app-properties.md#website)                   |
| **Informações de contato de suporte**      | Obrigatório se seu produto estiver disponível no Xbox; caso contrário, opcional (mas recomendado)                                   | [Informações de contato de suporte](enter-app-properties.md#support-contact-info)              |
| **Configurações do jogo**             | Opcional (aplicável somente a jogos)         | [Configurações do jogo](enter-app-properties.md#game-settings) |
| **Modo de exibição**             | Opcional                   | [Modo de exibição](enter-app-properties.md#display-mode) |
| **Declarações de produto**          | Padrão: os clientes podem instalar esse aplicativo em drives alternativos ou armazenamento removível; o Windows pode incluir os dados desse aplicativo em backups automáticos do OneDrive. | [Declarações de produto](app-declarations.md) |
| **Requisitos do sistema**      | Opcional                                    | [Requisitos do sistema](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Página de classificações etárias

| Nome do campo                    | Observações                                       | Para obter mais informações                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classificações etárias**               | Obrigatório                                    | [Classificações etárias](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de pacotes

| Nome do campo                    | Observações                                  | Para obter mais informações                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Controle de carregamento de pacote**    | Obrigatório (pelo menos um pacote)        | [Carregar pacotes de aplicativos](upload-app-packages.md) |
| **Disponibilidade da família de dispositivos** | Padrão: com base em seus pacotes       | [Disponibilidade da família de dispositivos](device-family-availability.md) |
| **Distribuição gradual de pacotes**   | Opcional (somente para atualizações)            | [Distribuição gradual de pacotes](gradual-package-rollout.md) |
| **Atualização obrigatória**          | Opcional (somente para atualizações)            | [Atualização obrigatória](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Listagens da Store

Você precisará de todas as informações obrigatórias para pelo menos um dos idiomas para os quais seu aplicativo oferece suporte. É recomendável fornecer [Listagens da Loja](create-app-store-listings.md) em todos os idiomas compatíveis com seu aplicativo, e você também poderá [fornecer listagens da Loja em outros idiomas](create-app-store-listings.md#store-listing-languages). Para facilitar o gerenciamento de várias listagens para o mesmo produto, você poderá [importar e exportar listagens da Store](import-and-export-store-listings.md).

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descrição**               | Obrigatório                                    | [Escreva uma excelente descrição do aplicativo](write-a-great-app-description.md) |
| **O que há de novo nesta versão**   | Opcional                                 | [Notas de versão](create-app-store-listings.md#whats-new-in-this-version)       |
| **Recursos do aplicativo**              | Opcional                                    | [Recursos do produto](create-app-store-listings.md#product-features)         |
| **Capturas**               | Obrigatório (pelo menos uma captura de tela; recomenda-se quatro ou mais)          | [Capturas](app-screenshots-and-images.md#screenshots)          |
| **Armazenar logotipos**               | Recomendado; necessário em algumas versões do sistema operacional | [Armazenar logotipos](app-screenshots-and-images.md#store-logos)             |
| **Trailers**                  | Opcional                                    | [Trailers](app-screenshots-and-images.md#trailers)                | 
| **Imagem do Windows 10 e Xbox (arte Superhero 16:9)**     | Recomendações        | [Imagem do Windows 10 e Xbox (arte Superhero 16:9)](app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Imagens do Xbox**     | Necessário para a exibição apropriada se você publicar no Xbox        | [Imagens do Xbox](app-screenshots-and-images.md#xbox-images) |
| **Campos complementares**  | Opcional                                    | [Campos complementares](create-app-store-listings.md#supplemental-fields) 
| **Termos da pesquisa**              | Opcional                                    | [Termos da pesquisa](create-app-store-listings.md#search-terms)         |
| **Informações de copyright e marca registrada** | Opcional                                 | [Informações de copyright e marca registrada](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termos de licença adicionais**  | Opcional                                    | [Termos de licença adicionais](create-app-store-listings.md#additional-license-terms) |
| **Desenvolvido por**              | Opcional                                    | [Desenvolvido por](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página de opções de envio

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opções de suspensão de publicação**     | Padrão: Publicar este envio assim que ele for aprovado na certificação (ou de acordo com as datas selecionadas por você na seção Agendar)      | [Opções de suspensão de publicação](manage-submission-options.md#publishing-hold-options)    
| **Notas de certificação**     | Recomendações          | [Notas de certificação](notes-for-certification.md)             |
| **Recursos restritos**     | Necessário se o seu produto declarar quaisquer [recursos restritos](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Recursos restritos](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obter informações sobre como publicar aplicativos de linha de negócios (LOB) diretamente para empresas, consulte [Distribuir aplicativos LOB às empresas](distribute-lob-apps-to-enterprises.md).
