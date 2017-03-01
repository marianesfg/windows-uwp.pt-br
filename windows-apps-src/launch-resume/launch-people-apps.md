---
author: TylerMSFT
title: Iniciar o app Pessoas
description: "Este tópico descreve o esquema de URI ms-people. Seu app pode usar esse esquema de URI para iniciar o app Pessoas para ações específicas."
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: 25abea87eaf374a1a8b5432c522d51bd7bbc1c1e
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-people-app"></a>Iniciar o app Pessoas


\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Este tópico descreve o esquema de URI **ms-people:**. Seu app pode usar esse esquema de URI para iniciar o app Pessoas para ações específicas.

## <a name="ms-people-uri-scheme-reference"></a>Referência do esquema de URI ms-people:


<table>
<colgroup>
<col width="50%" />
<col width="50%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Resultados</th>
<th align="left">Esquema de URI</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">Permite que outros apps iniciem a página principal do app Pessoas.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Permite que outros apps iniciem a página Configurações do app Pessoas.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Permite que os outros apps forneçam uma cadeia de caracteres de pesquisa que iniciará o app Pessoas com a página de resultados da pesquisa.
<div class="alert">
**Observação**  
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>Caso você não insira a sintaxe corretamente ou o valor da cadeia de caracteres de pesquisa não seja encontrado, o comportamento padrão é retornar uma lista completa de contatos sem nenhuma filtragem.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Inicia um cartão de visita existente, caso o contato seja encontrado. Ou inicia um cartão de visita temporário, caso nenhum contato seja encontrado. Se nenhum parâmetro de entrada for fornecido, iniciaremos o app Pessoas usando uma lista de contatos.
<div class="alert">
**Observação**  
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>A ordem dos parâmetros não importa.</p>
<p>Se houver mais de uma correspondência, retornaremos a primeira correspondência do contato.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Inicia uma página para salvar contato no app Pessoas a fim de salvar o contato indicado com o número de telefone ou o endereço de email fornecido.
<div class="alert">
**Observação**  
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>A ordem dos parâmetros não importa.</p>
</div>
<div>
 
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
</tbody>
</table>

 

## <a name="ms-peoplesearch-parameter-reference"></a>Referência de parâmetro ms-people:search:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parâmetro</th>
<th align="left">Descrição</th>
<th align="left">Exemplo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**SearchString**</td>
<td align="left"><p>Opcional.</p>
<p>A cadeia de caracteres de pesquisa para as informações de pesquisa de contato.</p>
<p>O número de telefone ou o nome do contato.</p></td>
<td align="left"><p>ms-people:search?SearchString=Smith</p></td>
</tr>
</tbody>
</table>

 

## <a name="ms-peopleviewcontact-parameter-reference"></a>Referência de parâmetro ms-people:viewcontact:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parâmetro</th>
<th align="left">Descrição</th>
<th align="left">Exemplo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**ContactId**</td>
<td align="left"><p>Opcional.</p>
<p>ID do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left">**Email**</td>
<td align="left"><p>Opcional.</p>
<p>Email do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left">**ContactName**</td>
<td align="left"><p>Opcional.</p>
<p>Nome do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left">**Contact**</td>
<td align="left"><p>Opcional.</p>
<p>Objeto de contato.</p></td>
<td align="left"><p>ms-people:viewcontact?Contact={Serialized Contact}</p></td>
</tr>
</tbody>
</table>

 

## <a name="ms-peoplesavetocontact-parameter-reference"></a>Referência de parâmetro ms-people:savetocontact:


<table>
<colgroup>
<col width="33%" />
<col width="33%" />
<col width="33%" />
</colgroup>
<thead>
<tr class="header">
<th align="left">Parâmetro</th>
<th align="left">Descrição</th>
<th align="left">Exemplo</th>
</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left">**PhoneNumber**</td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone do contato.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left">**Email**</td>
<td align="left"><p>Opcional.</p>
<p>Email do contato.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left">**ContactName**</td>
<td align="left"><p>Opcional.</p>
<p>Nome do contato.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

 

 

 

