---
author: jnHs
Description: "Assim que tiver criado seu aplicativo reservando um nome, você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um envio."
title: Envios de aplicativos
ms.assetid: 363BB9E4-4437-4238-A80F-ABDFC70D96E4
keywords: "lista de verificação"
translationtype: Human Translation
ms.sourcegitcommit: 5f975d0a99539292e1ce91ca09dbd5fac11c4a49
ms.openlocfilehash: ce9858da15cac0e34a2bb2c68c25ba63ec79af4c

---

# Envios de aplicativos


Assim que tiver [criado seu aplicativo reservando um nome](create-your-app-by-reserving-a-name.md), você poderá começar a trabalhar na publicação dele. A primeira etapa é criar um **envio**.

Você pode iniciar o envio quando seu aplicativo estiver completo e pronto para ser publicado, ou pode começar a inserir informações mesmo antes de ter escrito uma única linha de código. O envio será salvo em seu painel, para que você possa trabalhar nele sempre que estiver pronto.

Depois que o aplicativo for publicado, você poderá publicar uma versão atualizada criando outro envio em seu painel. Criar um novo envio permite fazer e publicar quaisquer mudanças necessárias, caso você esteja carregando novos pacotes ou apenas alterando detalhes, como o preço ou a categoria. Para criar um novo envio para um aplicativo, clique em **Atualizar** ao lado de envio mais recente mostrado na página de visão geral do aplicativo.

> **Observação**&nbsp;&nbsp;Esta seção da documentação descreve como usar um envio de aplicativo no painel do Centro de Desenvolvimento. Opcionalmente, você poderá usar a [API de envio da Windows Store](../monetize/create-and-manage-submissions-using-windows-store-services.md) para automatizar envios de aplicativos.

## Lista de verificação de envio de aplicativo


Aqui estão os detalhes que você pode fornecer quando for criar um envio de aplicativo, com links para mais informações.

Os itens que você será solicitado a fornecer ou especificar são indicados abaixo. Algumas áreas são opcionais ou têm valores padrão fornecidos que podem ser alterados conforme desejar.

### Página de preços e disponibilidade
| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Preço de base**                | Necessário                                    | [Preço de base](set-app-pricing-and-availability.md#base-price)              |
| **Avaliação gratuita**                | Padrão: nenhuma avaliação gratuita                      | [Adicionando avaliações e compras no aplicativo](https://msdn.microsoft.com/library/windows/apps/jj193599)  |
| **Mercados e preços personalizados** | Padrão: todos os mercados possíveis, nenhum preço personalizado | [Definir preço e seleção de mercado](define-pricing-and-market-selection.md)              |
| **Preço de venda**              | Opcional                                    | [Colocar aplicativos e complementos à venda](put-apps-and-add-ons-on-sale.md)                                       |
| **Distribuição e visibilidade** | Padrão: disponibilize este aplicativo na Loja. | [Distribuição e visibilidade](set-app-pricing-and-availability.md#distribution-and-visibility) |
| **Licenciamento para organizações**    | Padrão: permitir aquisição de volume por organizações | [Opções de licenciamento para organizações](organizational-licensing.md)                        |
| **Data de publicação**                | Padrão: publicar assim que possível      | [Data de publicação](set-app-pricing-and-availability.md#publish-date)          |

<span/>

### Página de propriedades do aplicativo

| Nome do campo                    | Observações                                       | Para obter mais informações                                                             |
|-------------------------------|---------------------------------------------|---------------------------------------------------------------------------|
| **Categoria e subcategoria**  | Necessário                                    | [Tabela de categoria e subcategoria](category-and-subcategory-table.md)       |
| **Requisitos do sistema**      | Opcional                                    | [Requisitos do sistema](enter-app-properties.md#system-requirements)      |
| **Declarações de aplicativo**          | Padrão: os clientes podem instalar esse aplicativo em drives alternativos ou armazenamento removível; o Windows pode incluir os dados desse aplicativo em backups automáticos do OneDrive. | [Declarações de aplicativo](app-declarations.md) |

<span/>

### Página de classificações etárias

| Nome do campo                    | Observações                                       | Para obter mais informações                          |
|-------------------------------|---------------------------------------------|----------------------------------------|
| **Classificações etárias**               | Necessário                                    | [Classificações etárias](age-ratings.md)          |

<span/>

### Página de pacotes

| Nome do campo                    | Observações                                  | Para obter mais informações                          |
|-------------------------------|----------------------------------------|----------------------------------------|
| **Controle de carregamento de pacote**    | Obrigatório (pelo menos um pacote)        | [Carregue os pacotes do aplicativo](upload-app-packages.md) |
| **Disponibilidade da família de dispositivos** | Padrão: com base em seus pacotes       | [Disponibilidade da família de dispositivos](upload-app-packages.md#device-family-availability) |
| **Distribuição gradual de pacote**   | Opcional (somente para atualizações)            | [Distribuição gradual de pacote](gradual-package-rollout.md) |
| **Atualização obrigatória**          | Opcional (somente para atualizações)            | [Atualização obrigatória](upload-app-packages.md#mandatory-update)

<span/>

### Listagens da Loja

Você precisará de todas as informações obrigatórias para pelo menos um dos idiomas para os quais seu aplicativo oferece suporte. É recomendável fornecer [Listagens da Loja](create-app-store-listings.md) em todos os idiomas compatíveis com seu aplicativo, e você também poderá [fornecer listagens da Loja em outros idiomas](create-app-store-listings.md#store-listing-languages).

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Descrição**               | Necessário                                    | [Escreva uma ótima descrição do aplicativo](write-a-great-app-description.md) |
| **Notas de versão**             | Opcional                                    | [Notas de versão](create-app-store-listings.md#release-notes)         |
| **Capturas de tela**               | Obrigatório (pelo menos uma captura de tela)          | [Imagens e capturas de tela do aplicativo](app-screenshots-and-images.md)       |
| **Ícone do bloco do aplicativo**             | Opcional, mas altamente recomendado para o Windows Phone 8.1 e versões anteriores | [Ícone do bloco do aplicativo](create-app-store-listings.md#app-tile-icon) |
| **Arte final promocional**       | Opcional                                    | [Imagens e capturas de tela do aplicativo](app-screenshots-and-images.md)       |
| **Recursos do aplicativo**              | Opcional                                    | [Recursos](create-app-store-listings.md#app-features)               |
| **Requisitos adicionais do sistema**      | Opcional                                    | [Requisitos adicionais do sistema](create-app-store-listings.md#additional-system-requirements) |
| **Palavras-chave**                  | Opcional                                    | [Palavras-chave](create-app-store-listings.md#keywords)                   |
| **Informações sobre direitos autorais e marcas registradas** | Opcional                                 | [Informações sobre direitos autorais e marcas registradas](create-app-store-listings.md#copyright-and-trademark-info) |
| **Termos de licença adicionais**  | Opcional                                    | [Termos adicionais da licença](create-app-store-listings.md#additional-license-terms) |
| **Site**                   | Opcional                                    | [Site](create-app-store-listings.md#website)                     |
| **Informações de contato de suporte**      | Opcional                                    | [Informações de contato de suporte](create-app-store-listings.md)                |
| **Política de privacidade**            | Obrigatório para alguns aplicativos. Consulte o [Contrato de Desenvolvedor de Aplicativo](https://msdn.microsoft.com/library/windows/apps/hh694058) e as [Políticas da Windows Store](https://msdn.microsoft.com/library/windows/apps/dn764944.aspx#pol_10_5_1) | [Política de privacidade](create-app-store-listings.md#privacy-policy) |
| **Listagens da Loja específicas da plataforma** | Opcional                               | [Criar listagens específicas de plataforma da Loja](create-platform-specific-store-listings.md) |

<span/>

### Observações para página de certificação

| Nome do campo                    | Observações                                       | Para obter mais informações                                                     |
|-------------------------------|---------------------------------------------|-------------------------------------------------------------------|
| **Observações**                     | Opcional                                    | [Notas para certificação](notes-for-certification.md)             |

<span/>

**Observação**&nbsp;&nbsp;Para obter informações sobre a publicação de aplicativos de linha de negócios (LOB) diretamente para empresas, consulte [Distribuir aplicativos LOB para empresas](distribute-lob-apps-to-enterprises.md).



<!--HONumber=Aug16_HO5-->


