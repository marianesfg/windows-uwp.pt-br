---
author: TylerMSFT
title: Iniciar o aplicativo Pessoas
description: Este tópico descreve o esquema de URI ms-people. Seu app pode usar esse esquema de URI para iniciar o app Pessoas para ações específicas.
ms.assetid: 1E604599-26EF-421C-932F-E9935CDB248E
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 0b87a49f24035215d44dbabcf9e401ddfefdff47
ms.sourcegitcommit: 144f5f127fc4fbd852f2f6780ef26054192d68fc
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/02/2018
ms.locfileid: "5989954"
---
# <a name="launch-the-people-app"></a>Iniciar o app Pessoas

Este tópico descreve o esquema de URI **ms-people:**. Seu aplicativo pode usar esse esquema de URI para iniciar o aplicativo Pessoas para ações específicas.

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
<td align="left">Permite que outros aplicativos iniciem a página principal do aplicativo Pessoas.</td>
<td align="left">ms-people:</td>
</tr>
<tr class="even">
<td align="left">Permite que outros aplicativos iniciem a página Configurações do aplicativo Pessoas.</td>
<td align="left">ms-people:settings</td>
</tr>
<tr class="odd">
<td align="left">Permite que os outros aplicativos forneçam uma cadeia de caracteres de pesquisa que iniciará o aplicativo Pessoas com a página de resultados da pesquisa.
<div class="alert">
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>Caso você não insira a sintaxe corretamente ou o valor da cadeia de caracteres de pesquisa não seja encontrado, o comportamento padrão é retornar uma lista completa de contatos sem nenhuma filtragem.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:search?SearchString=&lt;contactsearchinfo&gt;</td>
</tr>
<tr class="even">
<td align="left">Inicia um cartão de visita existente, caso o contato seja encontrado. Ou inicia um cartão de visita temporário, caso nenhum contato seja encontrado. Se nenhum parâmetro de entrada for fornecido, iniciaremos o aplicativo Pessoas usando uma lista de contatos.
<div class="alert">
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>A ordem dos parâmetros não importa.</p>
<p>Se houver mais de uma correspondência, retornaremos a primeira correspondência do contato.</p>
</div>
<div> 
</div></td>
<td align="left">ms-people:viewcontact:?ContactId=&lt;contactid&gt;&amp;AggregatedId=&lt;aggid&gt;&amp;PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;&amp;Contact=&lt;contactobj&gt;</td>
</tr>
<tr class="odd">
<td align="left">Inicia uma página para salvar contato no aplicativo Pessoas a fim de salvar o contato indicado com o número de telefone ou o endereço de email fornecido.
<div class="alert">
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>A ordem dos parâmetros não importa.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savetocontact?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
</tr>
<tr class="even">
<td align="left">Inicia uma página para adicionar uma nova página de contato no aplicativo Pessoas a fim de salvar o contato fornecido.
<div class="alert"><p>Use <a href="https://docs.microsoft.com/uwp/api/Windows.System.Launcher#Windows_System_Launcher_LaunchUriForResultsAsync_Windows_Foundation_Uri_Windows_System_LauncherOptions_Windows_Foundation_Collections_ValueSet_">LaunchUriForResultsAsync</a> para abrir o salvar nova página de contato. O uso de <strong>LaunchUriAsync</strong> somente iniciará a página principal do aplicativo Pessoas.</p>
<p>Os parâmetros diferenciam maiúsculas de minúsculas.</p>
<p>A ordem dos parâmetros não importa.</p>
<p>É possível usar qualquer combinação dos parâmetros suportados.</p>
</div>
<div>
</div></td>
<td align="left">ms-people:savecontacttask?PhoneNumber= &lt;phonenum&gt;&amp;Email=&lt;email&gt;&amp;ContactName=&lt;name&gt;</td>
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
<td align="left"><b>SearchString</b></td>
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
<td align="left"><b>ContactId</b></td>
<td align="left"><p>Opcional.</p>
<p>ID do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactId={ContactId}</p></td>
</tr>
<tr class="even">
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Email</b></td>
<td align="left"><p>Opcional.</p>
<p>Email do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="even">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nome do contato.</p></td>
<td align="left"><p>ms-people:viewcontact?ContactName=John%20%Smith</p></td>
</tr>
<tr class="odd">
<td align="left"><b>Contact</b></td>
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
<td align="left"><b>PhoneNumber</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone do contato.</p></td>
<td align="left"><p>ms-people:savetocontact?PhoneNumber=%2014257069326</p></td>
</tr>
<tr class="even">
<td align="left"><b>Email</b></td>
<td align="left"><p>Opcional.</p>
<p>Email do contato.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com</p></td>
</tr>
<tr class="odd">
<td align="left"><b>ContactName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nome do contato.</p></td>
<td align="left"><p>ms-people:savetocontact?Email=johnsmith@contsco.com&amp;ContactName= John%20%Smith</p></td>
</tr>
</tbody>
</table>

## <a name="ms-peoplesavecontacttask-parameter-reference"></a>Referência de parâmetro ms-people:savecontacttask:

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

</tr>
</thead>
<tbody>
<tr class="odd">
<td align="left"><b>Company</b></td>
<td align="left"><p>Opcional.</p>
<p>Nome da empresa do contato.</p></td>

</tr>
<tr class="even">
<td align="left"><b>FirstName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nome do contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressCity</b></td>
<td align="left"><p>Opcional.</p>
<p>Cidade do endereço residencial.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressCountry</b></td>
<td align="left"><p>Opcional.</p>
<p>País do endereço residencial.</p></td>

</tr>
<tr class="odd">
<td align="left"><b>HomeAddressState</b></td>
<td align="left"><p>Opcional.</p>
<p>Estado do endereço residencial.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomeAddressStreet</b></td>
<td align="left"><p>Opcional.</p>
<p>Rua do endereço residencial.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>HomeAddressZipCode</b></td>
<td align="left"><p>Opcional.</p>
<p>Código postal do endereço residencial.</p></td>

</tr>
<tr class="even">
<td align="left"><b>HomePhone</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone residencial do contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>JobTitle</b></td>
<td align="left"><p>Opcional.</p>
<p>Cargo do contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>LastName</b></td>
<td align="left"><p>Opcional.</p>
<p>Sobrenome do contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>MiddleName</b></td>
<td align="left"><p>Opcional.</p>
<p>Nome do meio do contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>MobilePhone</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone celular do contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Nickname</b></td>
<td align="left"><p>Opcional.</p>
<p>Apelido do contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Observações</b></td>
<td align="left"><p>Opcional.</p>
<p>Observações sobre o contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>OtherEmail</b></td>
<td align="left"><p>Opcional.</p>
<p>Outro email de contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>PersonalEmail</b></td>
<td align="left"><p>Opcional.</p>
<p>Email pessoal do contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Suffix</b></td>
<td align="left"><p>Opcional.</p>
<p>Sufixo do contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>Title</b></td>
<td align="left"><p>Opcional.</p>
<p>Título do contato.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>Website</b></td>
<td align="left"><p>Opcional.</p>
<p>Site do contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressCity</b></td>
<td align="left"><p>Opcional.</p>
<p>Cidade do endereço de trabalho.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressCountry</b></td>
<td align="left"><p>Opcional.</p>
<p>País do endereço de trabalho.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressState</b></td>
<td align="left"><p>Opcional.</p>
<p>Estado do endereço de trabalho.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkAddressStreet</b></td>
<td align="left"><p>Opcional.</p>
<p>Rua do endereço de trabalho.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkAddressZipCode</b></td>
<td align="left"><p>Opcional.</p>
<p>Código postal do endereço de trabalho.</p></td>
</tr>

<tr class="odd">
<td align="left"><b>WorkEmail</b></td>
<td align="left"><p>Opcional.</p>
<p>Email de trabalho do contato.</p></td>
</tr>

<tr class="even">
<td align="left"><b>WorkPhone</b></td>
<td align="left"><p>Opcional.</p>
<p>Número de telefone do trabalho do contato.</p></td>
</tr>
</tbody>
</table>
