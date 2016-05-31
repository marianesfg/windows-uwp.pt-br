---
author: mcleanbyron
Description: Neste passo a passo, você criará e executará seu primeiro experimento com testes A/B.
title: Criar e executar seu primeiro experimento com testes A/B
ms.assetid: 16A2B129-14E1-4C68-86E8-52F1BE58F256
---

# Criar e executar seu primeiro experimento com testes A/B

Neste passo a passo, você fará o seguinte:
* Criará um experimento no painel do Centro de Desenvolvimento do Windows que testa se uma mudança na cor de fundo de um botão de aplicativo aumenta com êxito o número de cliques do botão.
* Criará um aplicativo que recupera configurações de variação do Centro de Desenvolvimento, usa esses dados para alterar a cor de fundo de um botão e registra em log dados de eventos de exibição e conversão no Centro de Desenvolvimento.
* Executará o aplicativo para coletar dados de experimento.
* Examinará os resultados do experimento no painel do Centro de Desenvolvimento, escolherá uma variação a ser habilitada para todos os usuários do aplicativo e concluirá o experimento.

Para obter uma visão geral de testes A/B teste com o Centro de Desenvolvimento, veja [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md).

## Pré-requisitos

Para seguir este passo a passo, você deve ter uma conta do Centro de Desenvolvimento do Windows e configurar seu computador de desenvolvimento conforme descrito em [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md).

## Criar o experimento no Centro de Desenvolvimento do Windows

1. Entre no [painel do Centro de Desenvolvimento](https://dev.windows.com/overview).
2. Se já tiver um aplicativo no Centro de Desenvolvimento que você deseja usar para criar um experimento, selecione-o em **Seus aplicativos**. Se você ainda não tiver um aplicativo no seu painel, [crie um novo aplicativo reservando um nome](../publish/create-your-app-by-reserving-a-name.md) e, em seguida, selecione esse aplicativo no painel.
3. No painel de navegação, clique em **Serviços** e depois em **Experimentação**.
4. Na seção **Chaves de API**, selecione **Nova chave de API** para gerar uma nova chave de API e insira o nome **Meu primeiro experimento** para essa chave de API. Você usará essa chave de API na próxima seção deste passo a passo.
5. Na seção **Experimentos**, clique em **Novo experimento**. No campo **Nome do experimento**, digite o nome **Otimizar Cliques de Botão**.
6. No campo **Nome do evento de exibição**, digite o nome **userViewedButton**. Mais adiante neste guia passo a passo, você adicionará um código que registra esse evento de exibição quando a página principal do seu aplicativo é inicializada e o botão fica visível para o usuário.
7. Na seção **Metas e eventos de conversão**, insira os seguintes valores:
  * No campo **Nome da meta**, digite **Aumentar Cliques de Botão**.
  * No campo **Nome do evento de conversão**, digite o nome **userClickedButton**. Mais adiante neste guia passo a passo, você adicionará um código que registra esse evento de conversão no manipulador de eventos Click do botão.
  * No campo **Objetivo**, escolha **Maximizar**.
8. Na seção **Variações e configurações**, clique em **Adicionar configuração** três vezes. Agora, você terá quatro linhas das configurações vazias.
  * Na primeira linha, digite **buttonText** para o nome de configuração, **Botão Cinza** na coluna **Variação A** e **Botão Azul** na coluna **Variação B**.
  * Na segunda linha, digite **r** para o nome da configuração, **128** na coluna **Variação A** e **1** na coluna **Variação B**.
  * Na terceira linha, digite **g** para o nome da configuração, **128** na coluna **Variação A** e **1** na coluna **Variação B**.
  * Na quarta linha, digite **b** para o nome da configuração, **128** na coluna **Variação A** e **255** na coluna **Variação B**.  
9. Confirme que a caixa de seleção **Distribuir igualmente** está marcada para que as variações sejam distribuídas igualmente no seu aplicativo.
10. Clique em **Salvar** e depois em **Ativar**.

> **Importante**  Depois de ativar um experimento, você não pode mais modificar os parâmetros dele, a menos que ele seja um experimento de teste (você clicou na caixa de seleção **Experimento de teste** quando criou o experimento). Em geral, recomendamos codificar o experimento no seu aplicativo antes de ativar esse experimento. Por questões de simplicidade, neste guia passo a passo, você pode ativar o experimento agora.

## Codificar o experimento no seu aplicativo

1. No Visual Studio 2015, crie um novo projeto da Plataforma Universal do Windows usando Visual C#. Especifique o nome **SampleExperiment** para o projeto.
2. No Gerenciador de Soluções, expanda o nó do seu projeto, clique com botão direito no nó **Referências** e clique em **Adicionar Referência**.
3. No **Gerenciador de Referências**, expanda **Universal do Windows** e clique em **Extensões**.
4. Na lista de SDKs, marque a caixa de seleção ao lado de **SDK de Microsoft Store Engagement** e clique em **OK**.
5. No **Gerenciador de Soluções**, clique duas vezes em MainPage.xaml para abrir o designer da página principal no aplicativo.
6. Arraste um elemento **Button** de **Toolbox** até a página.
7. Clique duas vezes no botão do designer para abrir o arquivo de código e adicionar um manipulador de eventos para o evento **Click**.  
8. Substitua o conteúdo inteiro do arquivo de código pelo código a seguir.

  ```CSharp
  using System;
  using Windows.UI.Xaml;
  using Windows.UI.Xaml.Controls;
  using Windows.UI.Xaml.Media;
  using System.Threading.Tasks;
  using Windows.UI;
  using Windows.UI.Core;

  // Namespace for A/B testing.
  using Microsoft.Services.Store.Engagement;

  namespace SampleExperiment
  {  
    public sealed partial class MainPage : Page
    {
        private readonly ExperimentClient experiment;
        private ExperimentVariation variation;

        // Assign this variable to your API key from Dev Center. The API key
        // shown below is for example purposes only.
        private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";    

        public MainPage()
        {
            this.InitializeComponent();

            // Initialize the ExperimentClient for A/B testing.
            experiment = new ExperimentClient(apiKey);

            // Because this call is not awaited, execution of the current method
            // continues before the call is completed.
            #pragma warning disable CS4014
            Initialize();
            #pragma warning restore CS4014
        }

        private async Task Initialize()
        {
            // Get the current cached variation assignment for the experiment.
            ExperimentVariationResult result = await experiment.GetVariationAsync();
            variation = result.Variation;

            // Check whether the cached variation assignment needs to be refreshed.
            // If so, then refresh it.
            if (result.ErrorCode != EngagementErrorCode.Success || result.Variation.NeedsRefresh)
            {
                result = await experiment.RefreshVariationAsync();

                // If the call succeeds, use the new result. Otherwise, use the cached value.
                if (result.ErrorCode == EngagementErrorCode.Success)
                {
                    variation = result.Variation;
                }
            }

            // Get settings named "buttonText", "r", "g", and "b" from the variation
            // assignment. If no variation assignment is available, the settings default
            // to "Grey button" for the button text and grey RGB value for the button color.
            var buttonText = variation.GetString("buttonText", "Grey Button");
            var r = (byte)variation.GetInteger("r", 128);
            var g = (byte)variation.GetInteger("g", 128);
            var b = (byte)variation.GetInteger("b", 128);

            // Assign button text and color.
            await button.Dispatcher.RunAsync(
                CoreDispatcherPriority.Normal,
                () =>
                {
                    button.Background = new SolidColorBrush(Color.FromArgb(255, r, g, b));
                    button.Content = buttonText;
                    button.Visibility = Visibility.Visible;
                });

            // Log the view event named "userViewedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userViewedButton", variation);
        }

        private void button_Click(object sender, RoutedEventArgs e)
        {
            // Log the conversion event named "userClickedButton" to Dev Center.
            StoreServicesCustomEvents.Log("userClickedButton", variation);
        }
     }
  }
  ```
9. Na linha de código a seguir, atribua a variável *apiKey* à chave de API que você obteve no painel do Centro de Desenvolvimento na seção anterior. A chave de API mostrada abaixo é apenas para fins de exemplo.
```CSharp
private string apiKey = "F48AC670-4472-4387-AB7D-D65B095153FB";
```
10. Salve o arquivo de código e construa o projeto.

## Execute o aplicativo para coletar dados de experimento

1. Execute o aplicativo **SampleExperiment** criado na seção anterior.
2. Confirme que você está vendo um botão cinza ou azul. Clique no botão e, em seguida, feche o aplicativo.
3. Repita as etapas acima várias vezes no mesmo computador para confirmar que o aplicativo está mostrando a mesma cor de botão.

## Examine os resultados e conclua o experimento

Aguarde várias horas depois de concluir a seção anterior e, em seguida, siga estas etapas para analisar os resultados do seu experimento e concluir o processo.

> **Observação** Assim que você ativa um experimento, o Centro de Desenvolvimento começa imediatamente a coletar dados de quaisquer aplicativos que sejam instrumentados para registrar dados para o seu experimento. No entanto, pode levar várias horas para que os dados do experimento apareçam no painel.

1. No Centro de Desenvolvimento, volte para a página **Experimentação** do seu aplicativo.
2. Na seção **Experimentos**, clique no filtro **Ativo** e, em seguida, clique em **Otimizar Cliques de Botão** para acessar a página desse experimento.
3. Confirme que os resultados mostrados nas seções **Resumo dos resultados** e **Detalhes dos resultados** correspondem ao que você espera ver. Para saber mais sobre essas seções, veja [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md#review-the-results-of-your-experiment).

  >**Observação** O Centro de Desenvolvimento relata apenas o primeiro evento de conversão de cada usuário em um período de 24 horas. Se um usuário aciona vários eventos de conversão em seu aplicativo em um período de 24 horas, apenas o primeiro evento de conversão é relatado. Isso se destina a ajudar a impedir que um usuário único com muitos eventos de conversão distorça os resultados do experimento de um grupo de amostra de usuários.

4. Agora, você está pronto para finalizar o experimento. Na seção **Resumo dos resultados**, na coluna **Variação B**, clique em **Alternar**. Isso alterna todos os usuários do seu aplicativo para o botão azul.
5. Clique em **OK** para confirmar que você deseja finalizar o experimento.
6. Execute o aplicativo **SampleExperiment** criado na seção anterior.
7. Confirme que você está vendo um botão azul. Observe que pode levar até dois minutos para que o seu aplicativo receba uma atribuição de variação atualizada.

## Tópicos relacionados

  * [Definir seu experimento no painel do Centro de Desenvolvimento](define-your-experiment-in-the-dev-center-dashboard.md)
  * [Codificar seu aplicativo para experimentação](code-your-experiment-in-your-app.md)
  * [Gerenciar seu experimento no painel do Centro de Desenvolvimento](manage-your-experiment.md)
  * [Executar experimentos de aplicativo com testes A/B](run-app-experiments-with-a-b-testing.md)


<!--HONumber=May16_HO2-->


