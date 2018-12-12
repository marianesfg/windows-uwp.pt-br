---
ms.assetid: 1901c4c2-5161-435d-bc7b-f40c69cdb138
title: Arquivos, pastas e bibliotecas
description: Saiba como ler e gravar configurações do app, seletores de pastas e arquivos e sobre locais especiais de área restrita, como a biblioteca de vídeos/músicas.
ms.date: 02/08/2017
ms.topic: article
keywords: windows 10, uwp
ms.localizationpriority: medium
ms.openlocfilehash: 811a9b42efe83cf89fd3df89e5c43c72274af36f
ms.sourcegitcommit: 49d58bc66c1c9f2a4f81473bcb25af79e2b1088d
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 12/11/2018
ms.locfileid: "8928530"
---
 # <a name="files-folders-and-libraries"></a>Arquivos, pastas e bibliotecas


Use as APIs nos namespaces [Windows.Storage](https://msdn.microsoft.com/library/windows/apps/br227346), [Windows.Storage.Streams](https://msdn.microsoft.com/library/windows/apps/br241791) e [Windows.Storage.Pickers](https://msdn.microsoft.com/library/windows/apps/br207928) para ler e gravar texto e outros formatos de dados em arquivos e gerenciar arquivos e pastas. Nesta seção, você aprenderá também sobre como ler e gravar configurações de aplicativo, sobre seletores de arquivos e pastas e sobre os locais de área restrita especiais, como a biblioteca de vídeos/músicas.

| Tópico | Descrição  |
|-------|--------------|
| [Enumerar e consultar arquivos e pastas](quickstart-listing-files-and-folders.md) | Acesse arquivos e pastas que estejam em uma pasta, biblioteca, dispositivo ou local de rede. Você também pode consultar arquivos e pastas em um local por meio de consultas de arquivo e pasta. |
| [Criar, gravar e ler um arquivo](quickstart-reading-and-writing-files.md) | Leia e grave um arquivo usando o objeto [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171). |
| [Obter propriedades do arquivo](quickstart-getting-file-properties.md) | Obtenha as propriedades - nível superior, básicas e estendidas - de um arquivo representado pelo objeto [StorageFile](https://msdn.microsoft.com/library/windows/apps/br227171). |
| [Abrir arquivos e pastas com um seletor](quickstart-using-file-and-folder-pickers.md) | Acesse arquivos e pastas permitindo que o usuário interaja com um seletor. Você pode usar o [FolderPicker](https://msdn.microsoft.com/library/windows/apps/br207881) para obter acesso a uma pasta. |
| [Salvar um arquivo com um seletor](quickstart-save-a-file-with-a-picker.md) | Use o [FileSavePicker](https://msdn.microsoft.com/library/windows/apps/br207871) para permitir que os usuários especifiquem o nome e o local em que desejam que o aplicativo salve um aplicativo. |
| [Acessando o conteúdo do Grupo Doméstico](quickstart-accessing-homegroup-content.md) | Acesse o conteúdo armazenado na pasta Grupo Doméstico do usuário, incluindo imagens, músicas e vídeos. |
| [Determinando a disponibilidade de arquivos do Microsoft OneDrive](quickstart-determining-availability-of-microsoft-onedrive-files.md) | Determine se um arquivo do Microsoft OneDrive está disponível usando a propriedade [StorageFile.IsAvailable](https://msdn.microsoft.com/library/windows/apps/windows.storage.storagefile.isavailable.aspx). |
| [Arquivos e pastas nas bibliotecas Música, Imagens e Vídeos](quickstart-managing-folders-in-the-music-pictures-and-videos-libraries.md) | Adicione pastas existentes de música, fotos ou vídeos às bibliotecas correspondentes. Você também pode remover pastas de bibliotecas, obter a lista de pastas em uma biblioteca e descobrir fotos, músicas e vídeos armazenados. |
| [Acompanhar arquivos e pastas usados recentemente](how-to-track-recently-used-files-and-folders.md) | Acompanhe os arquivos que o usuário acessa com frequência adicionando-os à lista de itens usados recentemente de seu aplicativo. A plataforma gerencia os itens usados recentemente para você classificando-os com base na data em que foram acessados pela última vez e removendo o item mais antigo quando o limite de 25 itens é atingido. Todos os aplicativos têm seus próprios itens usados recentemente. |
| [Controlar alterações de sistema de arquivo em segundo plano](change-tracking-filesystem.md) | Controlar alterações ao sistema de arquivos, mesmo quando o aplicativo não estiver em execução.|
| [Acessar o cartão SD](access-the-sd-card.md) | Você pode armazenar e acessar dados não essenciais em um cartão microSD opcional, especialmente em dispositivos móveis de baixo custo que têm armazenamento interno limitado. |
| [Permissões de acesso a arquivo](file-access-permissions.md) | Os aplicativos podem acessar certos locais do sistema de arquivos por padrão. Os aplicativos também podem acessar outros locais por meio do seletor de arquivos ou da declaração de funcionalidades. |
| [Acesso rápido às propriedades de arquivo UWP](fast-file-properties.md) | Coletar com eficiência uma lista de arquivos e respectivas propriedades em uma biblioteca para usar em um aplicativo UWP. |

## <a name="related-samples"></a>Exemplos relacionados
[Exemplo de enumeração de pasta](http://go.microsoft.com/fwlink/p/?linkid=619993)

[Exemplo de acesso a arquivos](http://go.microsoft.com/fwlink/p/?linkid=619995)

[Exemplo do seletor de arquivos](http://go.microsoft.com/fwlink/p/?linkid=619994)
 

 
