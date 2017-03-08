---
author: jnHs
Description: "Se você tiver publicado aplicativos anteriormente na Loja para Windows ou para Windows Phone, esses aplicativos também estarão disponíveis para clientes em dispositivos Windows 10."
title: Como seu aplicativo aparece na Loja para clientes do Windows 10
ms.assetid: 487BB57E-F547-4764-99B1-84FE396E6D3A
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 4a7c10d93a3145466007bfe4fac63422400a9830
ms.lasthandoff: 02/07/2017

---

# <a name="how-your-app-appears-in-the-store-for-windows-10-customers"></a>Como seu aplicativo aparece na Loja para clientes do Windows 10


Se você tiver publicado aplicativos anteriormente na Loja para Windows ou para Windows Phone, esses aplicativos também estarão disponíveis para clientes em dispositivos Windows 10. Como a forma em que a Loja apresenta e categoriza os aplicativos para clientes que executam o Windows 10 foi alterada, este tópico o ajudará a entender essas mudanças.

**Observação**  Se você quiser alterar qualquer um desses detalhes, [crie um novo envio](app-submissions.md), faça suas alterações e envie a atualização para a Loja.

 

## <a name="consideration-for-apps-that-shared-identity-in-the-windows-store-and-windows-phone-store"></a>Consideração para aplicativos que compartilhavam a identidade na Windows Store e na Loja do Windows Phone


Se você usou o mesmo nome reservado para um aplicativo publicado nas duas Lojas (frequentemente chamado de compartilhar a identidade do seu aplicativo), eles agora serão considerados como um aplicativo, não dois. No painel, você os verá como um único aplicativo com pacotes para Windows e Windows Phone.

A maioria dos desenvolvedores definiu os mesmos preços e outras propriedades para o aplicativo e quaisquer complementos em cada Loja. Porém, se esses valores forem diferentes, é importante entender quais serão mostrados para os clientes do Windows 10.

### <a name="pricing"></a>Preço
Se você tiver escolhido preços base diferentes para seu aplicativo (ou complemento) em cada Loja, o preço base da Windows Store será usado.

**Observação**  Se você tiver definido preços por mercado na Loja do Windows Phone, os preços personalizados também serão mostrados para seus clientes do Windows 10.

### <a name="free-trials"></a>Avaliações gratuitas
As opções de avaliação eram diferentes nas duas Lojas anteriores. Se você as usava, é possível que tenha escolhido opções diferentes para cada Loja. A opção de avaliação disponível para seus clientes do Windows 10 é determinada de acordo com a tabela a seguir.

| Aplicativo do Windows 8       | Aplicativo do Windows Phone   | Configuração de avaliação para o Windows 10                                                  |
|---------------------|---------------------|-------------------------------------------------------------------------------|
| Sem avaliação gratuita       | Sem avaliação gratuita       | Sem avaliação gratuita                                                                 |
| Sem avaliação gratuita       | A avaliação nunca expira | Sem avaliação gratuita                                                                 |
| A avaliação nunca expira | A avaliação nunca expira | A avaliação nunca expira                                                           |
| A avaliação nunca expira | Sem avaliação gratuita       | Sem avaliação gratuita                                                                 |
| Período de avaliação limitado  | A avaliação nunca expira | Sem avaliação gratuita no Windows Phone 8.1 e versões anteriores. Caso contrário, período de avaliação limitado |
| Período de avaliação limitado  | Sem avaliação gratuita       | Sem avaliação gratuita no Windows Phone 8.1 e versões anteriores. Caso contrário, período de avaliação limitado |

### <a name="markets"></a>Mercados
Seu aplicativo estará disponível para os clientes do Windows 10 em todos os mercados em que o aplicativo foi publicado anteriormente. Isso se aplica mesmo que tenha feito seleções de mercado diferentes para cada Loja.

### <a name="categories"></a>Categorias
Se seu aplicativo aparecia em categorias diferentes nas duas Lojas, usaremos a categoria da Windows Store para determinar a nova categoria. Observe que algumas categorias são diferentes na Loja para clientes do Windows 10. Por isso, revise a [tabela](#cat) a seguir.

### <a name="age-rating"></a>Classificação etária
Se você forneceu classificações etárias diferentes, a mais estrita (idade maior) será usada.

### <a name="privacy-policy"></a>Política de privacidade
Se seu aplicativo tiver uma política de privacidade, a que foi fornecida ao enviar o aplicativo do Windows 8 será mostrada aos clientes do Windows 10.

### <a name="screenshots"></a>Capturas de tela
Usamos todas as capturas de tela que você enviou e a versão adequada para exibição aos clientes do Windows 10, com base no tipo de dispositivo que estão usando. No caso incomum de seus idiomas com suporte serem diferentes em cada Loja, alguns clientes poderão ver uma captura de tela de outro idioma que represente melhor a experiência que terão ao comprar o aplicativo.

### <a name="store-listings"></a>Listagens da Loja
Tentamos mostrar a listagem da Loja mais apropriada para os clientes do Windows 10 de acordo com o idioma deles. Quando as listagens da Loja estão disponíveis em mais de uma origem no mesmo idioma, a listagem de seu aplicativo da Windows Store é mostrada para os clientes do Windows 10. Em casos raros em que seus idiomas com suporte são diferentes para cada Loja, alguns clientes poderão ver uma listagem da Loja de seu Windows Phone app se essa for a única listagem da Loja que você forneceu no idioma.

Se você quiser atualizar a listagem da Loja exibida aos clientes do Windows 10 para informá-los sobre as experiências que funcionam em vários dispositivos, faça a atualização na [descrição do aplicativo](create-app-store-listings.md). Os clientes no Windows 10 verão a descrição padrão do aplicativo, mas você também pode [criar listagens da Loja específicas à plataforma](create-platform-specific-store-listings.md) se quiser que sua listagem da Loja apareça de forma diferente para os clientes com diferentes versões do sistema operacional.

## <a name="category-changes"></a>Alterações de categoria


Em muitos casos, as novas [categorias e subcategorias](category-and-subcategory-table.md) de aplicativos e jogos são iguais às da Loja para versões anteriores do sistema operacional. No entanto, houve algumas alterações. Examine a tabela a seguir para entender a categorização de seu aplicativo na Loja para clientes no Windows 10 de acordo com a categoria anterior.

**Observação**  Você verá a nova categoria listada no painel ao exibir a [categoria do aplicativo](category-and-subcategory-table.md) na página [Propriedades do aplicativo](enter-app-properties.md) de um envio, e os clientes que acessarem a Loja em dispositivos Windows 10 verão seu aplicativo na nova categoria. No entanto, os clientes que acessarem a Loja de um sistema operacional anterior continuarão a ver o aplicativo listado em sua categoria original.


**Alterações de categoria para aplicativos Windows Phone:**

| Categoria anterior                       | Nova categoria                  |
|-----------------------------------------|-------------------------------|
| Governo + política &gt; comentário   | Governo + política         |
| Governo + política &gt; questões legais | Governo + política         |
| Governo + política &gt; política     | Governo + política         |
| Governo + política &gt; recursos    | Governo + política         |
| Saúde + bem-estar &gt; dieta + nutrição  | Saúde + bem-estar              |
| Saúde + bem-estar &gt; bem-estar           | Saúde + bem-estar              |
| Saúde + bem-estar &gt; saúde            | Saúde + bem-estar              |
| Estilo de vida &gt; arte e entretenimento      | Estilo de vida                     |
| Estilo de vida &gt; fora de casa              | Estilo de vida                     |
| Estilo de vida &gt; bares + restaurantes            | Alimentação + restaurantes                 |
| Estilo de vida &gt; compras                 | Compras                      |
| Notícias + clima &gt; internacional       | Notícias + clima                |
| Notícias + clima &gt; local + nacional    | Notícias + clima                |
| Utilitários + produtividade                | Utilitários + ferramentas             |
| Viagem + navegação                     | Viagem                        |
| Viagem + navegação &gt; planejamento       | Viagem                        |
| Viagem + navegação &gt; ferramentas          | Viagem                        |
| Viagem + navegação &gt; com crianças      | Crianças + família &gt; viagem     |
| Viagem + navegação &gt; idioma       | Educação &gt; Idioma       |
| Viagem + navegação &gt; mapas        | Navegação + mapas             |
| Viagem + navegação &gt; navegação     | Navegação + mapas             |
| Jogos &gt; clássicos                     | Jogos &gt; Ação + aventura |
| Jogos &gt; família                       | Jogos &gt; Família + crianças      |
| Jogos &gt; esportes + recreação          | Jogos &gt; Esportes             |
| Jogos &gt; estratégia + simulação        | Jogos &gt; Estratégia           |

 

**Alterações de categoria para aplicativos do Windows 8:**

| Categoria anterior           | Nova categoria                         |
|-----------------------------|--------------------------------------|
| Livros + Referência &gt; Crianças | Crianças + família &gt; Livros + referência |
| Música + Vídeos &gt; Vídeo   | Foto + vídeo                        |
| Música + Vídeos &gt; Música   | Música                                |
| Governo                  | Governo + política                |
| Finanças                     | Finanças pessoais                     |
| Jogos &gt; Ação           | Jogos &gt; Ação + aventura        |
| Jogos &gt; Aventura        | Jogos &gt; Ação + aventura        |
| Jogos &gt; Arcade           | Jogos &gt; Ação + aventura        |
| Jogos &gt; Cartas             | Jogos &gt; Cartas + tabuleiro              |
| Jogos &gt; Crianças             | Jogos &gt; Família + crianças             |
| Jogos &gt; família           | Jogos &gt; Família + crianças             |
| Jogos &gt; Desafios           | Jogos &gt; Desafios           |
| Jogos &gt; Corrida           | Jogos &gt; Corrida + voo           |

