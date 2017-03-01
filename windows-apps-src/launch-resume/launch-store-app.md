---
author: TylerMSFT
title: Iniciar o aplicativo da Windows Store
description: "Este tópico descreve o esquema de URI ms-windows-store. Seu app pode usar esse esquema de URI para iniciar o aplicativo da Windows Store para páginas específicas na Loja."
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: windows 10, uwp
translationtype: Human Translation
ms.sourcegitcommit: c6b64cff1bbebc8ba69bc6e03d34b69f85e798fc
ms.openlocfilehash: d80055e8c8ca1e8586cb8f2a54612206301282a1
ms.lasthandoff: 02/07/2017

---

# <a name="launch-the-windows-store-app"></a>Iniciar o aplicativo da Windows Store


\[ Atualizado para apps UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]

Este tópico descreve o esquema de URI **ms-windows-store:**. Seu app pode usar esse esquema de URI para iniciar o aplicativo da Windows Store para páginas específicas na Loja.

## <a name="ms-windows-store-uri-scheme-reference"></a>Referência do esquema de URI ms-windows-store:

<table>
<tr><th>Descrição</th><th></th><th>Esquema de URI</th></tr>
<tr><td>Abre a página inicial da Loja.</td><td /><td>ms-windows-store://home</td></tr>
<tr><td>Inicia um vertical de nível superior na loja.<p>Observação: Nem todos os usuários têm acesso a todos os verticais.</p>
</td><td /><td>
<p>ms-windows-store://navigatetopage/?Id=Apps </p>
<p>ms-windows-store://navigatetopage/?Id=Games</p>
<p>ms-windows-store://navigatetopage/?Id=Music</p>
<p>ms-windows-store://navigatetopage/?Id=Video</p>
<p>ms-windows-store://navigatetopage/?Id=LOB</p>
</td>
</tr>
<tr>
<td rowspan="4">Inicia a página de detalhes (PDP) de um produto. <p>O ID da Loja é recomendado para clientes no Windows 10 e funcionará em todas as versões de sistema operacional, mas as maneiras anteriores de fazer isso (ex: PFN) ainda têm suporte.</p>
<p>Esses valores podem ser encontrados no painel Centro de Desenvolvimento do Windows na página <a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">Identidade do app</a> da seção Gerenciamento do app de cada app.</p>
</td>
<td>
ID da Loja <p>(Recomendado)</p>
</td>
<td>
<p>ms-windows-store://pdp/?ProductId=9WZDNCRFHVJL</p>
</td>
</tr>
<tr>
<td>Nome da Família de Pacotes (PFN)</td>
<td>ms-windows-store://pdp/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>ID do Produto (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://pdp/?PhoneAppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>ID do Produto (Windows 8.x)</td>
<td>ms-windows-store://pdp/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117
</td>
</tr>
<tr>
<td rowspan="4">Inicia a gravação de uma experiência de revisão para um produto.</td>
<td>ID da Loja <p>(Recomendado)</p></td>
<td>ms-windows-store://review/?ProductId=9WZDNCRFHVJL </td>
</tr>
<tr>
<td>Nome da Família de Pacotes (PFN)</td>
<td>ms-windows-store://review/?PFN= Microsoft.Office.OneNote_8wekyb3d8bbwe
</td>
</tr>
<tr>
<td>ID do Produto (Windows Phone 7.x/8.x)</td>
<td>ms-windows-store://reviewapp/?AppId=ca05b3ab-f157-450c-8c49-a1f127f5e71d </td>
</tr>
<tr>
<td>ID do Produto (Windows 8.x)</td>
<td>ms-windows-store://review/?AppId=f022389f-f3a6-417e-ad23-704fbdf57117 </td>
</tr>
<tr>
<td>Inicia uma pesquisa de produtos associados a uma extensão de arquivo. </td>
<td />
<td>ms-windows-store://assoc/?FileExt=pdf
</td>
</tr>
<tr>
<td>Inicia uma pesquisa de produtos associados a um protocolo.</td>
<td />
<td>ms-windows-store://assoc/?Protocol=ms-word </td>
</tr>
<tr>
<td>Inicia uma pesquisa de produtos associados a uma ou mais marcas. As marcas devem ser separadas por vírgulas.
</td>
<td />
<td>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit </p>
<p>ms-windows-store://assoc/?Tags=Photos_Rich_Media_Edit, Camera_Capture_App</p>
</td>
</tr>
<tr>
<td>
Inicia uma pesquisa para a consulta especificada. São permitidos espaços na consulta.
</td>
<td />
<td>ms-windows-store://search/?query=OneNote </td>
</tr>
<tr>
<td>Inicia uma pesquisa de produto em uma categoria.</td>
<td />
<td>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Productivity</p>
<p>ms-windows-store://browse/?type=Apps&amp;cat=Health+%26+fitness </p>
</td>
</tr>
<tr>
<td>Inicia uma pesquisa de produtos do fornecedor especificado. Espaços no nome são permitidos.
</td>
<td />
<td>ms-windows-store://publisher/?name=Microsoft Corporation
</td>
</tr>
<tr><td>Abre a página de downloads e atualizações.</td>
<td />
<td>ms-windows-store://downloadsandupdates </td>
</tr>
<tr>
<td>Inicia a página de configurações da Loja.</td>
<td />
<td>ms-windows-store://settings </td>
</tr>
</table>

 

 

