---
author: jnHs
Description: "Saiba mais sobre os formulários fiscais emitidos pela Microsoft, inclusive quem os receberá e quando eles serão disponibilizados."
title: "Noções sobre os formulários fiscais da Receita Federal dos EUA emitidos pela Microsoft"
ms.author: wdg-dev-content
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
ms.assetid: 1e475b96-f953-457c-864f-b6f4cb4c309f
translationtype: Human Translation
ms.sourcegitcommit: 5645eee3dc2ef67b5263b08800b0f96eb8a0a7da
ms.openlocfilehash: 5f51e1c5a44767e49b5c3dc5f578c2532da11008
ms.lasthandoff: 02/08/2017

---

# <a name="understand-irs-tax-forms-issued-by-microsoft"></a>Noções sobre os formulários fiscais da Receita Federal dos EUA emitidos pela Microsoft

Dependendo de sua localização e da quantidade de vendas e/ou pagamentos que você recebe, você poderá receber um ou mais formulários fiscais da Microsoft por ano. A Microsoft é obrigada a emitir esses formulários e declará-los para a Receita Federal dos EUA.

A seguir, explicaremos mais sobre esses formulários, inclusive quem os receberá e quando eles serão disponibilizados.

## <a name="types-of-tax-forms"></a>Tipos de formulários fiscais

| Formulário fiscal da Receita Federal dos EUA | Descrição | Disponibilidade |
|--------------|-------------|--------------|
|1099-MISC, 1099-K | Relacionado a atividade de vendas e/ou pagamentos feitos a você pela participação em marketplaces da Microsoft | Os formulários impressos serão protocolados até **31 de janeiro**, e as cópias em .pdf serão disponibilizados no Centro de Desenvolvimento (**Painel > Configurações da conta > Perfil fiscal**) ao mesmo tempo |
|1042-S | Relacionado a pagamentos feitos a você que estão sujeitos à retenção de imposto dos Estados Unidos. | Os formulários impressos serão protocolados até **15 de março**, e as cópias em .pdf serão disponibilizados no Centro de Desenvolvimento (**Painel > Configurações da conta > Perfil fiscal**) ao mesmo tempo |

> **Observação** O endereço listado nos formulários fiscais da Receita Federal dos EUA vem do endereço de seu [Perfil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms). Se seu endereço mudou, atualize-o em seu **Perfil fiscal**.

## <a name="for-developers-located-in-the-united-states"></a>Para desenvolvedores localizados nos Estados Unidos

<table>
  <tr>
     <th>Se eu for um desenvolvedor dos Estados Unidos que vende aplicativos pagos e... </th>
     <th> Devo receber este formulário</th>
  </tr>
  <tr> 
     <td valign="top">Atingi a marca de **mais de 200 vendas de app** com um valor total de compra dessas vendas **superior a US$ 20.000** no ano fiscal aplicável (**sem** contar vendas feitas no Brasil e na China por meio da Loja do Windows 10.)</td>
    <td valign="top">**1099-K** :<br>
Declarante: Microsoft Corporation<br>
EIN: \*\*\*\*\*4442<br>
<br>
**Importante:** O formulário 1099-K contém os valores de **compra brutos**, e não os pagamentos feitos a você.</td>
  </tr>
  <tr> 
     <td valign="top">Recebi **pelo menos US$ 10 em pagamentos** por vendas de app feitas no Brasil e na China por meio da Loja do Windows 10.<br>
<br>
**OU**<br>
<br>
Recebi pelo menos US$ 600 em pagamentos não relacionados a vendas de app da Microsoft no ano fiscal aplicável (por exemplo, pagamentos de incentivos ou pagamentos de prêmio de um concurso ou promoção)</td>
    <td valign="top">**1099-MISC** :<br>
Pagador: Microsoft Corporation<br>
EIN: \*\*\*\*\*4442<br>
<br>
**Importante:** Certas pessoas jurídicas não receberão formulários 1099-MISC, independentemente dos valores de pagamento recebidos da Microsoft.  Consulte seu contador para obter mais informações.</td>
  </tr>
  <tr>
    <td valign="top">Nenhuma das opções acima.</td>
    <td valign="top">Nenhum</td>
  </tr>
  <tr>
    <td valign="top">&nbsp;</td>
    <td valign="top">&nbsp;</td>
  </tr>
  <tr>
     <th>Se eu for um desenvolvedor dos Estados Unidos que vende aplicativos pagos e... </th>
     <th> Devo receber este formulário</th>
  </tr>
  <tr> 
     <td valign="top">Recebi **pelo menos US$ 600 em pagamentos** de anúncios em apps (Microsoft Advertising) no ano fiscal aplicável</td>
    <td valign="top">**1099-MISC** :<br>
Pagador: Microsoft Online Inc<br>
EIN: \*\*\*\*\*0505<br>
<br>
**Importante:** Certas pessoas jurídicas não receberão formulários 1099-MISC, independentemente dos valores de pagamento recebidos da Microsoft.  Consulte seu contador para obter mais informações.  </td>
  </tr>
  <tr> 
     <td valign="top">Recebi **menos de US$ 600 em pagamentos** de anúncios em apps (Microsoft Advertising) no ano fiscal aplicável</td>
     <td valign="top">Nenhum</td>
  </tr>
</table>


## <a name="for-developers-located-outside-of-the-united-states"></a>Para desenvolvedores localizados fora dos Estados Unidos

<table>
  <tr>
    <td valign="top">**Recebi um formulário 1042-S da Microsoft. Para que ele serve?**</td>
    <td valign="top">A Microsoft enviou um ou mais formulários 1042-S porque pagamos a você uma receita que é considerada declarável para as autoridades fiscais dos Estados Unidos e que foi sujeita à retenção de imposto.  O formulário 1042-S é usado para essa exigência de declaração.</td>
  </tr>
  <tr>
    <td valign="top">**O que devo fazer com os formulários?**</td>
    <td valign="top">Em geral, nenhuma ação específica é necessária de sua parte. O formulário 1042-S pode ser útil se você deseja entrar com pedido de alguma forma de crédito de imposto junto às autoridades fiscais locais.  Você deve consultar seu contador para obter mais informações sobre esse assunto.</td>
  </tr>
  <tr>
    <td valign="top">**Por que o imposto foi retido em minhas pagamentos quando preenchi um formulário W8?**</td>
    <td valign="top">Impostos serão retidos se:<br>
     1. Você não preencher a seção de tratados fiscais do W8 corretamente, ou<br>
     2. Você residir em um país que não tenha um tratado fiscal com os Estados Unidos.

     You can visit Dev Center at any time to submit an updated W8 form.<br>
     <br>
     **Note:** Not all income is subject to tax withholding.</td>
  </tr>
  <tr>
    <td valign="top">**Enviei um formulário W8 atualizado com informações de tratado válidas. A Microsoft pode me reembolsar o imposto que foi retido?**</td>
    <td valign="top">Uma vez retido o imposto, ele não pode ser reembolsado. Você deve falar sobre isso com seu contador para saber se pode solicitar um crédito local por esses impostos ou se pode entrar com um pedido de reembolso junto à Receita Federal dos EUA.</td>
  </tr>
  <tr>
    <td valign="top">**Quais vendas são declaradas no formulário 1042-S?**</td>
    <td valign="top">Somente a vendas feitas **a compradores localizados nos Estados Unidos que foram classificados como sujeitos à retenção de imposto** devem ser declaradas.  Todas as outras vendas não são consideradas declaráveis.</td>
  </tr>
  <tr>
    <td valign="top">**Por que recebi 3 cópias do mesmo formulário 1042-S em um envelope?**</td>
    <td valign="top">Os regulamentos da Receita Federal dos EUA exigem que três cópias do formulário sejam fornecidas:
<ul>
<li>Uma para o controle do destinatário</li>
<li>Um para entrar com pedido de devolução de imposto federal junto à Receita dos EUA (se aplicável)</li>
<li>Um para entrar com pedido de devolução de imposto estadual junto à Receita dos EUA (se aplicável)</li>
</ul></td>
  </tr>
</table>


> **Observação** Se você tiver outras dúvidas ou preocupações relacionadas aos **formulários fiscais da Receita Federal dos EUA**, crie um [tíquete de suporte](http://aka.ms/storesupport). A Microsoft não pode responder a perguntas relacionadas a circunstâncias específicas de seu imposto; para essas perguntas, consulte seu contador.

