---
author: jnHs
Description: "Assim que tiver criado seu app reservando um nome, você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um envio."
title: Envios de apps
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "lista de verificação"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: df66981ae8355ea62128a881f02fd6fb891ffb30
ms.lasthandoff: 02/07/2017

---

# <a name="app-submissions"></a>Envios de apps


Assim que tiver [criado seu app reservando um nome](create-your-app-by-reserving-a-name.md), você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um **envio**.

Você pode iniciar o envio quando seu app estiver completo e pronto para ser publicado, ou pode começar a inserir informações mesmo antes de ter escrito uma única linha de código. O envio será salvo em seu painel, para que você possa trabalhar nele sempre que estiver pronto.

Depois que o app for publicado, você poderá publicar uma versão atualizada criando outro envio em seu painel. Criar um novo envio permite fazer e publicar quaisquer mudanças necessárias, caso você esteja carregando novos pacotes ou apenas alterando detalhes, como o preço ou a categoria. Para criar um novo envio para um app, clique em **Atualizar** ao lado de envio mais recente mostrado na página de visão geral do app.

> **Observação**&nbsp;&nbsp;Esta seção da documentação descreve como usar um envio de app no painel do Centro de Desenvolvimento. Opcionalmente, você poderá usar a [API de envio da Windows Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de apps.

## <a name="app-submission-checklist"></a>Lista de verificação de envio de app


Aqui estão os detalhes que você pode fornecer quando for criar um envio de app, com links para mais informações.

Os itens que você será solicitado a fornecer ou especificar são indicados abaixo. Algumas áreas são opcionais ou têm valores padrão fornecidos que podem ser alterados conforme desejar.

### <a name="pricing-and-availability-page"></a>Página de preços e disponibilidade
| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Preço de base**                | Obrigatório                                    | [Preço de base](set-app-pricing-and-availability.md#base-price)              |
| **Avaliação gratuita**                | Padrão: nenhuma avaliação gratuita                      | [Adicionando avaliações e compras no app](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **Mercados e preços personalizados** | Padrão: todos os mercados possíveis, nenhum preço personalizado | [Definir preço e seleção de mercado](define-pricing-and-market-selection.md)              |
| **Preço de venda**              | Opcional                                    | [Colocar apps e complementos à venda](put-apps-and-add-ons-on-sale.md)                                       |
| **Distribuição e visibilidade** | Padrão: disponibilize este app na Loja. | [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **Licenciamento para organizações**    | Padrão: permitir aquisição de volume por organizações | [Opções de licenciamento para organizações](organizational-licensing.md)                        |
| **Data de publicação**                | Padrão: publicar assim que possível      | [Data de publicação](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### <a name="app-properties-page"></a>Página de propriedades do app

| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoria e subcategoria**  | Obrigatório                                    | [Tabela de categoria e subcategoria](category-and-subcategory-table.md)       |
| **Requisitos do sistema**      | Opcional                                    | [Requisitos do sistema](enter-app-properties.md#system-requirements)      |
| **Declarações de app**          | Padrão: os clientes podem instalar esse app em drives alternativos ou armazenamento removível; o Windows pode incluir os dados desse app em backups automáticos do OneDrive. | [Declarações de app](app-declarations.md) |

<span/>

### <a name="age-ratings-page"></a>Página de classificações etárias

| Nome do campo                    | Observações                                       | Para obter mais informações                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classificações etárias**               | Obrigatório                                    | [Classificações etárias](age-ratings.md)          |

<span/>

### <a name="packages-page"></a>Página de pacotes

| Nome do campo                    | Observações                                  | Para obter mais informações                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Controle de carregamento de pacote**    | Obrigatório (pelo menos um pacote)        | [Carregue os pacotes do app](upload-app-packages.md) |
| **Disponibilidade da família de dispositivos** | Padrão: com base em seus pacotes       | [Disponibilidade da família de dispositivos](upload-app-packages.md#device-family-availability) |
| **Distribuição gradual de pacote**   | Opcional (somente para atualizações)            | [Distribuição gradual de pacote](gradual-package-rollout.md) |
| **Atualização obrigatória**          | Opcional (somente para atualizações)            | [Atualização obrigatória](upload-app-packages.md#mandatory-update)

<span/>

### <a name="store-listings"></a>Listagens da Loja

Você precisará de todas as informações obrigatórias para pelo menos um dos idiomas para os quais seu app oferece suporte. É recomendável fornecer [Listagens da Loja](create-app-store-listings.md) em todos os idiomas compatíveis com seu app, e você também poderá [fornecer listagens da Loja em outros idiomas](create-app-store-listings.md#store-listing-languages).

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descrição**               | Obrigatório                                    | [Escreva uma ótima descrição do app](write-a-great-app-description.md) |
| **Notas de versão**             | Opcional                                    | [Notas de versão](create-app-store-listings.md#release-notes)         |
| **Capturas de tela**               | Obrigatório (pelo menos uma captura de tela)          | [Imagens e capturas de tela do app](app-screenshots-and-images.md)       |
| **Ícone do bloco do app**             | Opcional, mas altamente recomendado para o Windows Phone 8.1 e versões anteriores | [Ícone do bloco do app](create-app-store-listings.md#app-tile-icon) |
| **Arte final promocional**       | Opcional                                    | [Imagens e capturas de tela do app](app-screenshots-and-images.md)       |
| **Recursos do app**              | Opcional                                    | [Recursos](create-app-store-listings.md#app-features)               |
| **Requisitos adicionais do sistema**      | Opcional                                    | [Requisitos adicionais do sistema](create-app-store-listings.md#additional-system-requirements) |
| **Palavras-chave**                  | Opcional                                    | [Palavras-chave](create-app-store-listings.md#keywords)                   |
| **Informações sobre direitos autorais e marcas registradas** | Opcional                                 | [Informações sobre direitos autorais e marcas registradas](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termos de licença adicionais**  | Opcional                                    | [Termos adicionais da licença](create-app-store-listings.md#additional-license-terms) |
| **Site**                   | Opcional                                    | [Site](create-app-store-listings.md#website)                     |
| **Informações de contato de suporte**      | Opcional                                    | [Informações de contato de suporte](create-app-store-listings.md)                |
| **Política de privacidade**            | Obrigatório para alguns apps. Consulte o [Contrato de Desenvolvedor de Aplicativo](https://msdn.microsoft.com/library/windows/apps/hh694058) e as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [Política de privacidade](create-app-store-listings.md#privacy-policy) |
| **Listagens da Loja específicas da plataforma** | Opcional                               | [Criar listagens específicas de plataforma da Loja](create-platform-specific-store-listings.md) |

<span/>

### <a name="notes-for-certification-page"></a>Observações para página de certificação

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Observações**                     | Opcional                                    | [Notas para certificação](notes-for-certification.md)             |

<span/>

**Observação**&nbsp;&nbsp;Para obter informações sobre a publicação de apps de linha de negócios (LOB) diretamente para empresas, consulte [Distribuir apps LOB para empresas](distribute-lob-apps-to-enterprises.md).

