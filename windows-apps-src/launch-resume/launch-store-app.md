---
author: TylerMSFT
title: Iniciar o aplicativo da Microsoft Store
description: Este tópico descreve o esquema de URI ms-windows-store. Seu aplicativo pode usar esse esquema de URI para iniciar o aplicativo Microsoft Store para páginas específicas na loja.
ms.assetid: 9A9C6576-1637-47D1-AC3B-D1A20D49E0FF
ms.author: twhitney
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: ecb99c16d413e5e9869215f2d048ad6d9d52206f
ms.sourcegitcommit: e2fca6c79f31e521ba76f7ecf343cf8f278e6a15
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/15/2018
ms.locfileid: "6981715"
---
# <a name="launch-the-microsoft-store-app"></a>Iniciar o aplicativo da Microsoft Store



Este tópico descreve o esquema de URI **ms-windows-store:**. Seu aplicativo pode usar esse esquema de URI para iniciar o aplicativo da Microsoft Store para páginas específicas na loja usando o método [**LaunchUriAsync**](https://msdn.microsoft.com/library/windows/apps/hh701476) .

Este exemplo mostra como abrir o armazenamento para a página de jogos:

```cs
bool result = await Windows.System.Launcher.LaunchUriAsync(new Uri("ms-windows-store://navigatetopage/?Id=Games"));
```

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
<p>Esses valores podem ser encontrados no [Partner Center](https://partner.microsoft.com/dashboard) na página <a href="https://msdn.microsoft.com/library/windows/apps/mt148561.aspx">identidade do aplicativo</a> na seção de gerenciamento do aplicativo para cada aplicativo.</p>
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

 

 
