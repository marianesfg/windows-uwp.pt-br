---
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Arquivos, pastas e bibliotecas
description: Aprenda como ler e gravar configurações do aplicativo, seletores de pastas e arquivos e sobre locais especiais de área restrita, como a biblioteca de vídeo/música.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: c7296923b3e991a56fed115527b28a6b62b3ffd6
ms.sourcegitcommit: ac7f3422f8d83618f9b6b5615a37f8e5c115b3c4
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 05/29/2019
ms.locfileid: "66369467"
---
 # <a name="files-folders-and-libraries"></a>Arquivos, pastas e bibliotecas


Use as APIs nos namespaces [Windows.Storage](https://docs.microsoft.com/uwp/api/Windows.Storage), [Windows.Storage.Streams](https://docs.microsoft.com/uwp/api/Windows.Storage.Streams) e [Windows.Storage.Pickers](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers) para ler e gravar texto e outros formatos de dados em arquivos e gerenciar arquivos e pastas. Nesta seção, você aprenderá também sobre como ler e gravar configurações de aplicativo, sobre seletores de arquivos e pastas e sobre os locais de área restrita especiais, como a biblioteca de vídeos/músicas.

| Tópico | Descrição  |
|-------|--------------|
| [Enumerar e consultar arquivos e pastas](quickstart-listing-files-and-folders.md) | Acesse arquivos e pastas que estejam em uma pasta, biblioteca, dispositivo ou local de rede. Você também pode consultar arquivos e pastas em um local por meio de consultas de arquivo e pasta. |
| [Criar, gravar e ler um arquivo](quickstart-reading-and-writing-files.md) | Leia e grave um arquivo usando o objeto [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile). |
| [Práticas recomendadas para gravar em arquivos](best-practices-for-writing-to-files.md) | Conheça as práticas recomendadas para usar os vários métodos de gravação de arquivos das classes [FileIO](https://docs.microsoft.com/uwp/api/windows.storage.fileio) e [PathIO](https://docs.microsoft.com/uwp/api/windows.storage.pathio). |
| [Obter propriedades do arquivo](quickstart-getting-file-properties.md) | Obtenha as propriedades - nível superior, básicas e estendidas - de um arquivo representado pelo objeto [StorageFile](https://docs.microsoft.com/uwp/api/Windows.Storage.StorageFile). |
| [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md) | Acesse arquivos e pastas permitindo que o usuário interaja com um seletor. Você pode usar o [FolderPicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FolderPicker) para obter acesso a uma pasta. |
| [Salvar um arquivo com um seletor](quickstart-save-a-file-with-a-picker.md) | Use o [FileSavePicker](https://docs.microsoft.com/uwp/api/Windows.Storage.Pickers.FileSavePicker) para permitir que os usuários especifiquem o nome e o local em que desejam que o aplicativo salve um arquivo. |
| [Acessando o conteúdo do Grupo Doméstico](quickstart-accessing-homegroup-content.md) | Acesse o conteúdo armazenado na pasta Grupo Doméstico do usuário, incluindo imagens, músicas e vídeos. |
| [Determinando a disponibilidade de arquivos do Microsoft OneDrive](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Determine se um arquivo do Microsoft OneDrive está disponível usando a propriedade [StorageFile.IsAvailable](https://docs.microsoft.com/uwp/api/windows.storage.storagefile.isavailable). |
| [Arquivos e pastas nas bibliotecas Música, Fotos e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Adicione pastas existentes de música, fotos ou vídeos às bibliotecas correspondentes. Você também pode remover pastas de bibliotecas, obter a lista de pastas em uma biblioteca e descobrir fotos, músicas e vídeos armazenados. |
| [Rastrear arquivos e pastas usados recentemente](how-to-track-recently-used-files-and-folders.md) | Acompanhe os arquivos que o usuário acessa com frequência adicionando-os à lista de itens usados recentemente de seu aplicativo. A plataforma gerencia a MRU para você classificando os itens com base na data em que foram acessados pela última vez e removendo o item mais antigo quando o limite de 25 itens é atingido. Todos os aplicativos têm seus próprios itens usados recentemente. |
| [Controlar alterações no sistema de arquivos em segundo plano](change-tracking-filesystem.md) | Acompanhar as alterações ao sistema de arquivos, mesmo quando o aplicativo não está em execução.|
| [Acessar o cartão SD](access-the-sd-card.md) | Você pode armazenar e acessar dados não essenciais em um cartão microSD, principalmente em dispositivos de baixo custo que possuem espaço de armazenamento interno limitado. |
| [Permissões de acesso a arquivo](file-access-permissions.md) | Os aplicativos podem acessar certos locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades. |
| [Acesso rápido às propriedades de arquivo na UWP](fast-file-properties.md) | Coletar com eficiência uma lista de arquivos e respectivas propriedades em uma biblioteca para usar em um aplicativo UWP. |

## <a name="related-samples"></a>Exemplos relacionados
[Exemplo de enumeração de pasta](https://go.microsoft.com/fwlink/p/?linkid=619993)

[Exemplo de acesso a arquivos](https://go.microsoft.com/fwlink/p/?linkid=619995)

[Exemplo do seletor de arquivos](https://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
