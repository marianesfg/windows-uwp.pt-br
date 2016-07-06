---
author: jnHs
Description: "Gerenciar seus IAPs em massa permite fazer alterações em vários IAPs ao mesmo tempo, em vez de enviar individualmente cada atualização."
title: Gerenciar IAPs em massa
ms.sourcegitcommit: 475371dd55aa111f3743c03dc1600e8cfdbeb5b0
ms.openlocfilehash: ae4d4ed33b9bd10a2b01b336c942ad3212de6533


---

# Gerenciar IAPs em massa

> **Importante** Este recurso está disponível atualmente apenas para contas de desenvolvedor que participam do [Programa Insider do Centro de Desenvolvimento](dev-center-insider-program.md). A implementação deste recurso pode ser alterada antes que ele esteja disponível para todos os desenvolvedores. Esta documentação preliminar fornece algumas informações básicas sobre como funciona o recurso.

Gerenciar seus IAPs em massa permite fazer alterações em vários IAPs ao mesmo tempo, em vez de enviar individualmente cada atualização. Você pode acessar essa funcionalidade na página da visão geral do aplicativo. Clique em **Gerenciar IAPs em massa**.

## Exportar informações de IAP atuais

Para começar, primeiro você precisa baixar um arquivo de modelo. csv. Se você já tiver criado IAPs, este arquivo inclui informações sobre eles. Caso contrário, ele será um arquivo em branco que você pode usar para inserir informações de IAPs novos. 

Para gerar e baixar este arquivo de modelo, clique em **Exportar IAPs** e salve o arquivo. csv em seu computador.

O arquivo. csv contém as colunas a seguir. 

| Nome da coluna               | Descrição                            | Obrigatório?      |
|---------------------------|----------------------------------|----------------------|
| ID do Produto    |  A [ID de produto](set-your-iap-product-id.md#product-id) exclusiva do IAP.  | Sim. Não pode ser alterado depois que o IAP for publicado. |
| Ação |A ação que você deseja aplicar ao importar o modelo. Os valores compatíveis são **Enviar** (para enviar um novo IAP ou atualizar um IAP publicado anteriormente) e **CreateDraft** (para salvar as alterações sem enviá-las para a loja). |    Sim |
| Tipo de produto  | O [tipo de produto](set-your-iap-product-id.md#product-type) do IAP. Os valores compatíveis são **Consumível** ou **Durável**. | Sim. Não pode ser alterado depois que o IAP for publicado. |
| Tempo de vida do produto  | Para um IAP durável, este é **Para sempre** (de um produto que nunca expira) ou uma duração definida. Os valores de duração aceitáveis são: **1 dia, 3days, 5days, 7days, 14days, 30days, 60days, 90days, 180days, 365days**   | Sim (se o Tipo de produto é Durável) |
| Tipo de conteúdo  | O [tipo de conteúdo](enter-iap-properties.md#content-type) do IAP. Para a maioria dos IAPs, isso deve ser **ElectronicSoftwareDownload**. Outros valores compatíveis são: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService** | Sim |
| Marca   | As informações opcionais de [Marca](enter-iap-properties.md#tag) usadas na implementação do seu aplicativo. | Não |
| Preço base    | A [faixa de preço](set-iap-pricing-and-availability.md#base-price) na qual você deseja oferecer o IAP. Deve ser **Grátis** ou uma faixa de preço válida no formato **0.99USD**. |   Sim |
| Data do lançamento  | A data em que você deseja publicar o IAP. Os valores aceitáveis são **Imediatamente**, **Manual** ou uma cadeia de caracteres de data que esteja em conformidade com o [padrão ISO 8601](http://go.microsoft.com/fwlink/p/?LinkId=817237). | Sim |
| Títulos    | O nome que os clientes verão para IAP, precedido por código de idioma e um ponto e vírgula. Por exemplo, para usar o título "Título de exemplo" em inglês/Estados Unidos, você deve inserir *en-us;Título de exemplo*. Os títulos adicionais para outros idiomas podem ser separados por ponto e vírgula. Cada título deve ser 100 caracteres ou menos.     | Sim |
|Descrições   | Informações adicionais opcionais para exibição aos clientes, precedidas pelo código de idioma-localidade e um ponto e vírgula. Por exemplo, para usar a descrição "Este é um exemplo" em inglês/Estados Unidos, você deve inserir *en-us;Este é um exemplo*. Os títulos adicionais para outros idiomas podem ser separados por ponto e vírgula. Cada descrição deve ter 200 caracteres ou menos.    | Não |
| Mercados | Um ou mais [mercados](define-pricing-and-market-selection.md#windows-store-consumer-markets) no qual você deseja oferecer o IAP. Separe cada mercado por um ponto e vírgula. | Sim |
|Palavras-chave | As [palavras-chave](enter-iap-properties.md#keywords) opcionais usadas na implementação de seu aplicativo. | Não |

## Importar IAPs

Antes de importar as alterações, você precisará atualizar o arquivo. csv baixado com as alterações que gostaria de fazer.

Para fazer alterações nos IAPs que você já publicou, atualize os valores que deseja alterar em sua cópia da planilha. Você pode remover todas as linhas dos IAPs que você não deseja atualizar ou deixá-las como está. Observe que se já houver um envio em andamento do IAP, você não poderá fazer alterações usando o arquivo. csv.

> **Importante** Ao enviar atualizações de IAPs que você já publicou, não é possível alterar os campos **ID do Produto** e o **Tipo de Produto**.

Para enviar um novo IAP, adicione uma nova linha e insira as informações de seu novo IAP. Assegure-se de fornecer todas as informações necessárias. 

Quando terminar de fazer todas as alterações, salve o arquivo. csv (com o mesmo nome de arquivo) e carrego-o. Basta arrastá-lo para o campo especificado (ou clique em **Procurar seus arquivos**). Um resumo das alterações será mostrado, juntamente com erros que devem ser corrigidos antes do envio. Após verificar se as informações estão correta, você poderá clicar **Enviar para a loja**. Cada IAP passará pelo processo de envio usando as informações que você forneceu.




<!--HONumber=Jun16_HO4-->


