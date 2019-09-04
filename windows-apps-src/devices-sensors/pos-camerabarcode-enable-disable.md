---
title: Configuração do scanner de código de barras da câmera
description: Habilitação ou desabilitação do scanner de código de barras da câmera
ms.date: 04/08/2019
ms.topic: article
keywords: windows 10, uwp, ponto de serviço, pos
ms.localizationpriority: medium
ms.openlocfilehash: 8394f79e9581101d6def0f1568cb000ffdee5987
ms.sourcegitcommit: 0dec04de501a3db6b22dfd4a320fc09b5c4a21b5
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 09/04/2019
ms.locfileid: "70243310"
---
# <a name="enable-or-disable-the-software-decoder-that-ships-with-windows"></a>Habilitar ou desabilitar o decodificador de software que vem com o Windows

No Windows 10, versão 1803, o decodificador de software está instalado e habilitado por padrão.  Você pode desabilitar o decodificador de software que vem com o Windows se não quiser usar o scanner de código de barras da câmera ou se tiver adquirido um decodificador de terceiros que funciona com as APIs Windows.Devices.PointOfService.BarcodeScanner APIs e não quiser usar os dois.

## <a name="enable-or-disable-using-the-system-registry"></a>Habilitar ou desabilitar usando o registro do sistema

O decodificador de software que vem com o Windows pode ser habilitado ou desabilitado por meio do registro do sistema adicionando a chave do registro *InboxDecoder* em *HKLM\Software\Microsoft\PointOfService\BarcodeScanner* e configurando o valor *Enable* conforme descrito abaixo.

| Nome do valor  | Tipo do Valor | Valor | Status |
| ----------- | --------- | -------|--------|
| Habilitar      | DWORD     | 1 (padrão)<br/>0 |  Habilita o decodificador de software que vem com o Windows <br/> Desabilita o decodificador de software que vem com o Windows |

Veja um exemplo de arquivo de registro que você pode usar para **desabilitar** o decodificador de software que vem com o Windows:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000000
```  

Use esse exemplo de arquivo de registro para **habilitar** o decodificador de software que vem com o Windows:

```text
Windows Registry Editor Version 5.00

[HKEY_LOCAL_MACHINE\SOFTWARE\Microsoft\PointOfService\BarcodeScanner\InboxDecoder]
"Enable"=dword:0000001
```  

> [!Warning]
> Problemas sérios podem ocorrer se você modificar o registro incorretamente.  Para obter proteção adicional, faça backup do registro antes de modificá-lo.  Assim, será possível restaurá-lo se houver algum problema.  Para obter mais informações sobre como fazer backup do registro e restaurá-lo, clique no número do artigo a seguir para visualizar o artigo na Base de Dados de Conhecimento Microsoft: <br/><br/> [322756](https://support.microsoft.com/help/322756/how-to-back-up-and-restore-the-registry-in-windows) Como fazer o backup e a restauração do Registro no Windows.

> [!NOTE]
> O decodificador de software integrado ao Windows 10 é uma cortesia fornecida pela  [**Digimarc Corporation**](https://www.digimarc.com/).

## <a name="see-also"></a>Consulte também

### <a name="samples"></a>Exemplos

- [Exemplo de scanner de código de barras](https://github.com/microsoft/Windows-universal-samples/tree/master/Samples/BarcodeScanner)
