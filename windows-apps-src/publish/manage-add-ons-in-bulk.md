---
author: jnHs
Description: "Gerenciar seus complementos em massa permite fazer alterações em vários complementos ao mesmo tempo, em vez de enviar individualmente cada atualização."
title: Gerenciar complementos em massa
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 6d1ffcc1-b3c6-4e2f-8fbe-d243b20a6272
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 3d7c8f1ab468e4797096e83fbbf256f46b494439
ms.lasthandoff: 02/08/2017

---

# <a name="manage-add-ons-in-bulk"></a>Gerenciar complementos em massa

> **Importante** Este recurso está disponível atualmente apenas para contas de desenvolvedor que participam do [Programa Insider do Centro de Desenvolvimento](dev-center-insider-program.md). A implementação deste recurso pode ser alterada antes que ele esteja disponível para todos os desenvolvedores. Esta documentação preliminar fornece algumas informações básicas sobre como funciona o recurso.

Gerenciar seus complementos em massa permite fazer alterações em vários complementos ao mesmo tempo, em vez de enviar individualmente cada atualização. Você pode acessar essa funcionalidade na página da visão geral do app. Clique em **Gerenciar complementos em massa**.

## <a name="export-current-add-on-info"></a>Exportar informações atuais do complemento

Para começar, primeiro você precisa baixar um arquivo de modelo. csv. Se você já tiver criado complementos, este arquivo inclui informações sobre eles. Caso contrário, ele será um arquivo em branco que você pode usar para inserir informações de complementos novos.

Para gerar e baixar este arquivo de modelo, clique em **Exportar complementos** e salve o arquivo. csv em seu computador.

O arquivo. csv contém as colunas a seguir. 

| Nome da coluna               | Descrição                            | Obrigatório?      |
|---------------------------|----------------------------------|----------------------|
| ID do produto    |  A [ID do produto](set-your-add-on-product-id.md#product-id) exclusiva do complemento.  | Sim. Não pode ser alterado depois que o complemento for publicado. |
| Ação |A ação que você deseja aplicar ao importar o modelo. Os valores compatíveis são **Enviar** (para enviar um novo complemento ou atualizar um complemento publicado anteriormente) e **CreateDraft** (para salvar as alterações sem enviá-las para a loja). |     Sim |
| Tipo de produto    | O [tipo de produto](set-your-add-on-product-id.md#product-type) do complemento. Os valores compatíveis são **Consumível** ou **Durável**. |    Sim. Não pode ser alterado depois que o complemento for publicado. |
| Tempo de vida do produto    | Para um complemento durável, este é **Para sempre** (de um produto que nunca expira) ou uma duração definida. Os valores de duração aceitáveis são: **1 dia, 3days, 5days, 7days, 14days, 30days, 60days, 90days, 180days, 365days**    | Sim (se o Tipo de produto é Durável) |
| Tipo de conteúdo    | O [tipo de conteúdo](enter-add-on-properties.md#content-type) do complemento. Para a maioria dos complementos, isso deve ser **ElectronicSoftwareDownload**. Outros valores compatíveis são: **ElectronicBooks, ElectronicMagazineSingleIssue, ElectronicNewspaperSingleIssue, MusicDownload, MusicStreaming, OnlineDataStorageServices, VideoDownload, VideoStreaming, SoftwareAsAService** |    Sim |
| Tag    | Informações de [Tag](enter-add-on-properties.md#custom-developer-data) opcionais (também conhecidas como **Dados de desenvolvedor personalizados**) usadas na implementação do seu app. | Não |
| Preço base    | A [faixa de preço](set-add-on-pricing-and-availability.md#base-price) na qual você deseja oferecer o complemento. Deve ser **Grátis** ou uma faixa de preço válida no formato **0.99USD**. |    Sim |
| Data do lançamento    | A data em que você deseja publicar o complemento. Os valores aceitáveis são **Imediatamente**, **Manual** ou uma cadeia de caracteres de data que esteja em conformidade com o [padrão ISO 8601](http://go.microsoft.com/fwlink/p/?LinkId=817237). | Sim |
| Títulos    | O nome que os clientes verão para complemento, precedido por código de idioma e um ponto e vírgula. Por exemplo, para usar o título "Título de exemplo" em inglês/Estados Unidos, você deve inserir *en-us;Título de exemplo*. Os títulos adicionais para outros idiomas podem ser separados por ponto e vírgula. Cada título deve ser 100 caracteres ou menos.     | Sim |
|Descrições    | Informações adicionais opcionais para exibição aos clientes, precedidas pelo código de idioma-localidade e um ponto e vírgula. Por exemplo, para usar a descrição "Este é um exemplo" em inglês/Estados Unidos, você deve inserir *en-us;Este é um exemplo*. Os títulos adicionais para outros idiomas podem ser separados por ponto e vírgula. Cada descrição deve ter 200 caracteres ou menos.    | Não |
| Mercados |    Um ou mais [mercados](define-pricing-and-market-selection.md#windows-store-consumer-markets) no qual você deseja oferecer o complemento. Separe cada mercado por um ponto e vírgula. |    Sim |
|Palavras-chave |    As [palavras-chave](enter-add-on-properties.md#keywords) opcionais usadas na implementação de seu app. | Não |

## <a name="import-add-ons"></a>Importar complementos

Antes de importar as alterações, você precisará atualizar o arquivo. csv baixado com as alterações que gostaria de fazer.

Para fazer alterações nos complementos que você já publicou, atualize os valores que deseja alterar em sua cópia da planilha. Você pode remover todas as linhas dos complementos que você não deseja atualizar ou deixá-las como está. Observe que se já houver um envio em andamento do complemento, você não poderá fazer alterações usando o arquivo. csv.

> **Importante** Ao enviar atualizações de complementos que você já publicou, não é possível alterar os campos **ID do Produto** e o **Tipo de Produto**.

Para enviar um novo complemento, adicione uma nova linha e insira as informações de seu novo complemento. Assegure-se de fornecer todas as informações necessárias. 

Quando terminar de fazer todas as alterações, salve o arquivo. csv (com o mesmo nome de arquivo) e carrego-o. Basta arrastá-lo para o campo especificado (ou clique em **Procurar seus arquivos**). Um resumo das alterações será mostrado, juntamente com erros que devem ser corrigidos antes do envio. Após verificar se as informações estão correta, você poderá clicar **Enviar para a loja**. Cada complemento passará pelo processo de envio usando as informações que você forneceu.


