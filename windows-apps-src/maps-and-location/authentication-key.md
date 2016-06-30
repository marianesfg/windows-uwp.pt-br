---
author: msatranjr
title: "Solicitar uma chave de autenticação de mapas"
description: "Seu aplicativo Universal do Windows deve ser autenticado para que possa usar o MapControl e os serviços de mapa no namespace Windows.Services.Maps."
ms.assetid: 13B400D7-E13F-4F07-ACC3-9C34087F0F73
ms.sourcegitcommit: 92285ce32548bd6035c105e35c2b152432f8575a
ms.openlocfilehash: b7c981e071f70ab0a76d73333a94580b3c497b0e

---

# Solicitar uma chave de autenticação de mapas


\[ Atualizado para aplicativos UWP no Windows 10. Para ler artigos sobre o Windows 8.x, consulte o [arquivo](http://go.microsoft.com/fwlink/p/?linkid=619132) \]


Seu [aplicativo Universal do Windows](https://msdn.microsoft.com/library/windows/apps/dn894631) deve ser autenticado para que possa usar o [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e os serviços de mapa no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979). Para autenticar o aplicativo, você deve especificar uma chave de autenticação de mapas. Este tópico descreve como solicitar uma chave de autenticação de mapas a partir da [Central de Desenvolvimento do Bing Mapas](https://www.bingmapsportal.com/) e adicioná-la ao aplicativo.

**Dica** Para saber mais sobre o uso de mapas em seu aplicativo, baixe a amostra a seguir do [repositório Windows-universal-samples](http://go.microsoft.com/fwlink/p/?LinkId=619979) no GitHub:

-   [Amostra de mapa da Plataforma Universal do Windows (UWP)](http://go.microsoft.com/fwlink/p/?LinkId=619977)

## Obter uma chave


Crie e gerencie chaves de autenticação de mapa para aplicativos Universal do Windows usando a [Central de Desenvolvimento do Bing Mapas](https://www.bingmapsportal.com/).

Para criar uma nova chave

1.  No navegador, vá até a Central de desenvolvedores do Bing Mapas ([https://www.bingmapsportal.com](https://www.bingmapsportal.com/)).

2.  Caso você precise fazer logon, insira a conta da Microsoft e clique em **Entrar**.

3.  Escolha a conta para associar à sua conta do Bing Mapas. Caso queira usar a conta da Microsoft, clique em **Sim**. Do contrário, clique em **Sign in with another account**.

4.  Caso você ainda não tenha uma conta do Bing Maps, crie uma nova conta do Bing Maps. Insira **Nome da Conta**, **Nome de Contato**, **Nome da Empresa**, **Endereço de Email** e **Telefone**. Depois de aceitar os termos de uso, clique em **Criar**.

5.  No menu **Minha conta**, clique em **Criar ou exibir chaves**.

6.  Clique no link **para criar uma nova chave**.

7.  Preencha o formulário **Criar Chave** e clique em **Criar**.

    -   **Nome do aplicativo:** o nome do aplicativo.
    -   **URL do aplicativo (opcional):** a URL do aplicativo.
    -   **Tipo de chave:** selecione **Básico** ou **Empresa**.
    -   **Tipo de aplicativo:** selecione **Aplicativo Universal Windows** para usar no aplicativo Universal do Windows.

    Este é um exemplo da aparência do formulário.

    ![exemplo do formulário Criar Chave.](images/createkeydialog.png)

8.  Depois de você clicar em **Criar**, a nova chave aparece abaixo do formulário **Criar Chave**. Copie-a para um local seguro ou adicione-a imediatamente ao aplicativo, conforme descrito na próxima etapa.

## Adicionar a chave ao aplicativo


A chave de autenticação de mapa é obrigada a usar o [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004) e os serviços de mapa ([**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979)) no aplicativo Universal do Windows. Adicione-o ao controle de mapa e mapeie objetos de serviço, conforme aplicável.

### Para adicionar a chave a um controle de mapa

Para autenticar o [**MapControl**](https://msdn.microsoft.com/library/windows/apps/dn637004), defina a propriedade [**MapServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn637036) como o valor da chave de autenticação. Você pode definir essa propriedade no código ou na marcação XAML, dependendo das suas preferências. Para obter mais informações sobre o uso de **MapControl**, consulte [Exibir mapas com modos de exibição 2D, 3D e Streetside](display-maps.md).

-   Este exemplo define **MapServiceToken** como o valor da chave de autenticação no código.

    ```cs
    MapControl1.MapServiceToken = "abcdef-abcdefghijklmno";
    ```

-   Este exemplo define **MapServiceToken** como o valor da chave de autenticação na marcação XAML.

    ```xml
    <Maps:MapControl x:Name="MapControl1" MapServiceToken="abcdef-abcdefghijklmno"/>
    ```

### Para adicionar a chave a serviços de mapa

Para usar serviços no namespace [**Windows.Services.Maps**](https://msdn.microsoft.com/library/windows/apps/dn636979), defina a propriedade [**ServiceToken**](https://msdn.microsoft.com/library/windows/apps/dn636977) como o valor da chave de autenticação. Para obter mais informações sobre como usar serviços de mapa, consulte [Exibir rotas e trajetos](routes-and-directions.md) e [Executar geocodificação e geocodificação reversa](geocoding.md).

-   Este exemplo define **ServiceToken** como o valor da chave de autenticação no código.

    ```cs
    MapService.ServiceToken = "abcdef-abcdefghijklmno";
    ```

## Tópicos relacionados

* [Central de desenvolvedores do Bing Mapas](https://www.bingmapsportal.com/)
* [Amostra de mapa UWP](http://go.microsoft.com/fwlink/p/?LinkId=619977)
* [Diretrizes de design para mapas](https://msdn.microsoft.com/library/windows/apps/dn596102)
* [Vídeo do build 2015: Aproveitando mapas e localização em telefones, tablets e computadores em seus aplicativos do Windows](https://channel9.msdn.com/Events/Build/2015/2-757)
* [Exemplo do aplicativo de tráfego UWP](http://go.microsoft.com/fwlink/p/?LinkId=619982)





<!--HONumber=Jun16_HO4-->


