---
title: Configuração do scanner de código de barras da câmera
description: Habilitação ou desabilitação do scanner de código de barras da câmera
ms.date: 05/1/2018
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 6b802dfa44f36768dc2446ee1d15bf9ca6d4f9f3
ms.sourcegitcommit: bf600a1fb5f7799961914f638061986d55f6ab12
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 02/05/2019
ms.locfileid: "9051039"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Habilitar ou desabilitar o decodificador de software que vem com o Windows
No Windows 10, versão 1803, o decodificador de software está instalado e habilitado por padrão.  Você pode desabilitar o decodificador de software que vem com o Windows se não quiser usar o scanner de código de barras da câmera ou se tiver adquirido um decodificador de terceiros que funciona com as APIs Windows.Devices.PointOfService.BarcodeScanner APIs e não quiser usar os dois.

## <a name="enable-or-disable-using-the-system-registry"></a>Habilitar ou desabilitar usando o registro do sistema
O decodificador de software que vem com o Windows pode ser habilitado ou desabilitado por meio do registro do sistema adicionando a chave do registro *InboxDecoder* em *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* e configurando o valor *Enable* conforme descrito abaixo.

| Nome do valor  | Tipo de valor | Valor | Status |
| ----------- | --------- | -------|--------|
| Enable      | DWORD     | 1 (padrão)<br/>0 |  Habilita o decodificador de software que vem com o Windows <br/> Desabilita o decodificador de software que vem com o Windows |


Veja um exemplo de arquivo de registro que você pode usar para **desabilitar** o decodificador de software que vem com o Windows:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000


```  
    
Use esse exemplo de arquivo de registro para **habilitar** o decodificador de software que vem com o Windows:

```
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001


```  

> [!Warning] 
> Problemas sérios podem ocorrer se você modificar o Registro incorretamente.  Para obter proteção adicional, faça backup do registro antes de modificá-lo.  Assim, será possível restaurá-lo se houver algum problema.  Para obter mais informações sobre como fazer backup do registro e restaurá-lo, clique no número do artigo a seguir para visualizar o artigo na Base de Dados de Conhecimento Microsoft: <br/><br/> [322756](https://support.microsoft.com/kb/322756) Como fazer o backup e a restauração do Registro no Windows.

> [!NOTE]
> O decodificador de software integrado ao Windows 10 é uma cortesia fornecida pela  [**Digimarc Corporation**](https://www.digimarc.com/).
