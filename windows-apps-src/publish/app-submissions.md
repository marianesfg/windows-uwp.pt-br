---
Description: Assim que tiver criado seu aplicativo reservando um nome, você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um envio.
title: Envios de aplicativos
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: lista de verificação, windows, uwp, envio, enviar, jogo, app, envio
ms.date: 10/31/2018
ms.topic: article
ms.localizationpriority: medium
ms.openlocfilehash: b98ea7f1d28c4fcd63cd2d4706905578b240e126
ms.sourcegitcommit: b034650b684a767274d5d88746faeea373c8e34f
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 03/06/2019
ms.locfileid: "57643281"
---
# <a name="app-submissions"></a>Envios de aplicativos


Assim que tiver [criado seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md), você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um **envio**.

Você pode iniciar o envio quando seu aplicativo estiver completo e pronto para ser publicado, ou pode começar a inserir informações mesmo antes de ter escrito uma única linha de código. Atualizações feitas ao seu envio são salvos, portanto, você pode voltar e trabalhar nele sempre que você está pronto.

> [!NOTE]
> Você deve ter um ativo [conta de desenvolvedor](https://go.microsoft.com/fwlink/p/?LinkId=615100) na [Partner Center](https://partner.microsoft.com/dashboard) para enviar aplicativos para a Microsoft Store.

Depois que o aplicativo for publicado, você pode publicar uma versão atualizada, criando outro envio no Partner Center. Criar um novo envio permite fazer e publicar quaisquer mudanças necessárias, caso você esteja carregando novos pacotes ou apenas alterando detalhes, como o preço ou a categoria. Para criar um novo envio para um aplicativo publicado, clique em **atualização** ao lado de envio mais recente mostrado no seu **visão geral** página. Você também pode [remover um aplicativo a Store](guidance-for-app-package-management.md#removing-an-app-from-the-store) se você precisa fazê-lo (e, em seguida, disponibilizá-lo novamente mais tarde, se desejar).

> [!NOTE]
> Esta seção da documentação descreve como criar um envio de aplicativo no Partner Center. Opcionalmente, você poderá usar a [API de envio da Microsoft Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de apps.

> [!IMPORTANT]
> A partir de 31 de outubro de 2018, produtos recém-criado não podem incluir os pacotes direcionados a 8.x/Windows do Windows Phone 8.x ou anterior. Para obter mais informações, consulte este [postagem de blog](https://blogs.windows.com/buildingapps/2018/08/20/important-dates-regarding-apps-with-windows-phone-8-x-and-earlier-and-windows-8-8-1-packages-submitted-to-microsoft-store/#SzKghBbqDMlmAO4c.97).

## <a name="app-submission-checklist"></a>Lista de verificação de envio de aplicativo

Aqui estão os detalhes que você pode fornecer quando for criar um envio de aplicativo, com links para mais informações.

Os itens que você será solicitado a fornecer ou especificar são indicados abaixo. Algumas áreas são opcionais ou têm valores padrão fornecidos que podem ser alterados conforme desejar. Você não precisa trabalhar com essas seções na ordem listada aqui.

### <a name="pricing-and-availability-page"></a>Página de preços e disponibilidade
| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Mercados**                   | Default: Todos os mercados possíveis  | [Defina seleção de preços e mercado](define-pricing-and-market-selection.md)         |
| **Público-alvo**                | Default: Audiência pública | [Público-alvo](choose-visibility-options.md#audience) |
| **Capacidade de descoberta**                | Default: Tornar este aplicativo disponíveis e podem ser descobertos na Store | [Capacidade de descoberta](choose-visibility-options.md#discoverability) |
| **Agenda**                  | Default: Versão assim que possível        | [Configurar o agendamento de versão exato](configure-precise-release-scheduling.md) |
| **Preço base**                | Obrigatório                                    | [Definir e agendar o preço do aplicativo](set-and-schedule-app-pricing.md)              |
| **Avaliação gratuita**                | Default: Sem avaliação gratuita                      | [Avaliação gratuita](set-app-pricing-and-availability.md#free-trial)              |
| **Preço de venda**              | Opcional                                    | [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md)           |
| **Licenciamento organizacional**    | Default: Permitir que a aquisição de volume por organizações | [Opções de licenciamento organizacionais](organizational-licensing.md)        |
      |


### <a name="properties-page"></a>Página de propriedades

| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoria e subcategoria**  | Obrigatório                                    | [Tabela de categoria e subcategoria](category-and-subcategory-table.md)       |
| **URL da política de privacidade**            | Obrigatório para muitos apps. Consulte o [Contrato de Desenvolvedor de Aplicativo](https://docs.microsoft.com/legal/windows/agreements/app-developer-agreement) e as [Políticas da Microsoft Store](https://docs.microsoft.com/en-us/legal/windows/agreements/store-policies#105-personal-information) | [URL da política de privacidade](enter-app-properties.md#privacy-policy-url)        |
| **Site**                   | Opcional                                    | [Site](enter-app-properties.md#website)                   |
| **Informações de contato de suporte**      | Obrigatório se seu produto estiver disponível no Xbox; caso contrário, opcional (mas recomendado)                                   | [Informações de contato de suporte](enter-app-properties.md#support-contact-info)              |
| **Configurações de jogo**             | Opcional (aplicável somente a jogos)         | [Configurações de jogo](enter-app-properties.md#game-settings) |
| **Modo de exibição**             | Opcional                   | [Modo de exibição](enter-app-properties.md#display-mode) |
| **Declarações de produto**          | Default: Os clientes podem instalar este aplicativo para unidades alternativas ou armazenamento removível; Windows podem incluir dados desse aplicativo em backups automáticos para o OneDrive | [Declarações de produto](app-declarations.md) |
| **Requisitos do sistema**      | Opcional                                    | [Requisitos do sistema](enter-app-properties.md#system-requirements)      |

<span/>

### <a name="age-ratings-page"></a>Página de classificações etárias

| Nome do campo                    | Observações                                       | Para obter mais informações                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classificação etária**               | Obrigatório                                    | [Classificação etária](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de pacotes

| Nome do campo                    | Observações                                  | Para obter mais informações                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Controle de carregamento de pacote**    | Obrigatório (pelo menos um pacote)        | [Carregar pacotes de aplicativos](upload-app-packages.md) |
| **Disponibilidade de família do dispositivo** | Padrão: com base em seus pacotes       | [Disponibilidade de família do dispositivo](device-family-availability.md) |
| **Distribuição gradual do pacote**   | Opcional (somente para atualizações)            | [Distribuição gradual do pacote](gradual-package-rollout.md) |
| **Atualização obrigatória**          | Opcional (somente para atualizações)            | [Atualização obrigatória](upload-app-packages.md#mandatory-update)


### <a name="store-listings"></a>Listagens da Loja

Você precisará de todas as informações obrigatórias para pelo menos um dos idiomas para os quais seu aplicativo oferece suporte. É recomendável fornecer [Listagens da Loja](create-app-store-listings.md) em todos os idiomas compatíveis com seu aplicativo, e você também poderá [fornecer listagens da Loja em outros idiomas](create-app-store-listings.md#store-listing-languages). Para facilitar o gerenciamento de várias listagens para o mesmo produto, você poderá [importar e exportar listagens da Store](import-and-export-store-listings.md).

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descrição**               | Obrigatório                                    | [Escreva uma descrição excelente aplicativo](write-a-great-app-description.md) |
| **O que há de novo nesta versão**   | Opcional                                 | [Notas de versão](create-app-store-listings.md#whats-new-in-this-version)       |
| **Recursos de aplicativo**              | Opcional                                    | [Recursos do produto](create-app-store-listings.md#product-features)         |
| **Capturas de tela**               | Obrigatório (pelo menos uma captura de tela; recomenda-se quatro ou mais)          | [Capturas de tela](app-screenshots-and-images.md#screenshots)          |
| **Logotipos da Store**               | Recomendado; necessário em algumas versões do sistema operacional | [Logotipos da Store](app-screenshots-and-images.md#store-logos)             |
| **Trailers**                  | Opcional                                    | [Trailers](app-screenshots-and-images.md#trailers)                | 
| **Imagem do Windows 10 e Xbox (arte de Super hero 16: 9)**     | Recomendações        | [Windows 10 e Xbox imagem (arte de Super hero 16: 9)
] (app-screenshots-and-images.md#windows-10-and-xbox-image-169-super-hero-art) |
| **Imagens do Xbox**     | Necessário para a exibição correta, se você publicar em Xbox        | [Xbox imagens
] (#xbox – imagens de aplicativo capturas de tela e images.md) |
| **Campos complementares**  | Opcional                                    | [Campos complementares](create-app-store-listings.md#supplemental-fields) 
| **Termos de pesquisa**              | Opcional                                    | [Termos de pesquisa](create-app-store-listings.md#search-terms)         |
| **Informações de direitos autorais e marcas** | Opcional                                 | [Informações de direitos autorais e marcas](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termos de licença adicionais**  | Opcional                                    | [Termos de licença adicionais](create-app-store-listings.md#additional-license-terms) |
| **Desenvolvido pelo**              | Opcional                                    | [Desenvolvido pelo](create-app-store-listings.md#developed-by)                   |


<span/>

### <a name="submission-options-page"></a>Página de opções de envio

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Opções de retenção de publicação**     | Default: Publicar esse envio assim que ele for aprovado na certificação (ou por datas que você selecionou na seção de agendamento)      | [Opções de retenção de publicação](manage-submission-options.md#publishing-hold-options)    
| **Notas para certificação**     | Recomendações          | [Notas para certificação](notes-for-certification.md)             |
| **Recursos restritos**     | Necessário se o seu produto declara que qualquer [somente recursos restritos](../packaging/app-capability-declarations.md#restricted-capabilities)    | [Recursos restritos](manage-submission-options.md#publishing-hold-options)       

<span/>

> [!NOTE]
> Para obter informações sobre como publicar aplicativos de linha de negócios (LOB) diretamente para empresas, consulte [Distribuir aplicativos LOB às empresas](distribute-lob-apps-to-enterprises.md).
