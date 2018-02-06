---
author: jnHs
Description: Set custom permissions for account users.
title: "Definir permissões personalizadas para usuários de contas"
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 01/12/2018
ms.topic: article
ms.prod: windows
ms.technology: uwp
keywords: "windows 10, uwp, funções de usuário, permissão do usuário, funções personalizadas, acesso de usuário, personalizar permissões, funções padrão"
ms.localizationpriority: high
ms.openlocfilehash: 1fdde4be606abae849ff3350d27afbbced157f75
ms.sourcegitcommit: 446fe2861651f51a129baa80791f565f81b4f317
ms.translationtype: HT
ms.contentlocale: pt-BR
ms.lasthandoff: 01/12/2018
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Definir funções ou permissões personalizadas para usuários de contas

Ao [adicionar usuários à sua conta do Centro de Desenvolvimento](add-users-groups-and-azure-ad-applications.md), será necessário especificar o acesso disponível na conta. Você pode fazer isso ao atribuir [funções padrão](#roles), que se aplicam à conta inteira, ou [personalizar as permissões deles](#custom) para fornecer o nível apropriado de acesso. Algumas das permissões personalizadas se aplicam à conta inteira, e algumas podem ser limitadas a um ou mais produtos específicos (ou concedidas a todos os produtos, se desejar).

> [!NOTE] 
> As mesmas funções e permissões podem ser aplicadas, independentemente de estar adicionando um usuário, um grupo ou um aplicativo Azure AD.

Ao determinar qual função ou permissão aplicar, lembre-se: 
-   Os usuários (incluindo grupos e aplicativos do Azure AD) poderão acessar a conta inteira do Centro de Desenvolvimento com as permissões associadas à função atribuídas, exceto ao [personalizar as permissões](#custom) e atribuir [permissões de nível de produto](#product-level-permissions) para que funcionem somente com aplicativos e/ou complementos específicos.
-   Você pode permitir que um usuário, grupo ou aplicativo Azure AD tenha acesso à funcionalidade de mais de uma função selecionando várias funções, ou usando permissões personalizadas para conceder o acesso que você queira.
-   Um usuário com uma determinada função (ou conjunto de permissões personalizadas) também pode fazer parte de um grupo que tenha uma função diferente (ou conjunto de permissões). Nesse caso, o usuário terá acesso a toda a funcionalidade associada ao grupo e à conta individual.

> [!TIP]
> Este tópico é específico para o programa de desenvolvedores de aplicativos do Windows. Para informações sobre funções de usuário no programa de desenvolvedores de hardware, consulte [Gerenciando funções de usuário](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles).


<span id="roles" />
## <a name="assign-roles-to-account-users"></a>Atribuir funções a usuários da conta

Por padrão, um conjunto de funções padrão é apresentado para que você escolha quando é necessário adicionar um usuário, um grupo ou um aplicativo Azure AD à sua conta do Centro de Desenvolvimento. Cada função tem um conjunto específico de permissões para executar determinadas funções na conta. 

A menos que você defina [permissões personalizadas](#custom) ao selecionar **Personalizar permissões**, cada usuário, grupo ou aplicativo Azure AD adicionado a uma conta deve receber pelo menos uma das funções padrão a seguir. 

> [!NOTE]
> O **proprietário** da conta é a pessoa que a criou com uma conta da Microsoft (e não qualquer usuário adicionado pelo Azure AD). Esse proprietário da conta é a única pessoa com acesso completo à conta, incluindo a capacidade de excluir aplicativos, criar e editar todos os usuários da conta e alterar todas as informações financeiras e configurações da conta. 


| Função                 | Descrição              |
|----------------------|--------------------------|
| Gerente              | Tem acesso completo à conta, exceto para alterar configurações de imposto e pagamento. Isso inclui o gerenciamento de usuários no Centro de Desenvolvimento, mas observe que a capacidade de criar e excluir usuários no locatário do Azure AD é dependente da permissão da conta no Azure AD. Ou seja, se um usuário receber a função Gerente, mas não tiver permissões de administrador global no Azure AD da organização, não será possível criar novos usuários ou excluir usuários do diretório (embora seja possível alterar a função do Centro de Desenvolvimento do usuário). <p> Observe que, se a conta do Centro de Desenvolvimento estiver associada a mais de um locatário do Azure AD, o Gerente não poderá ver os detalhes completos de um usuário (incluindo nome, sobrenome, email para recuperação de senha e se ele é um administrador global do Azure AD) a menos que acesse o mesmo locatário que o usuário com uma conta que possui credenciais de administrador global para esse locatário. No entanto, ele pode adicionar e remover usuários em qualquer locatário associado à conta do Centro de Desenvolvimento. |
| Desenvolvedor            | Pode carregar pacotes e enviar aplicativos e complementos, além de exibir o [Relatório de uso](usage-report.md) para obter detalhes de telemetria. Não pode exibir informações financeiras ou configurações da conta.   |
| Colaborador comercial | Pode exibir relatórios de [Integridade](health-report.md) e de [Uso](usage-report.md). Não é possível criar ou enviar produtos, alterar configurações de conta ou exibir informações financeiras.                                         |
| Colaborador financeiro  | Pode exibir [relatórios de pagamento](payout-summary.md), informações financeiras e relatórios de aquisição. Não pode fazer alterações em aplicativos, complementos ou configurações da conta.                                                                                                                                   |
| Comerciante             | Pode [responder às avaliações dos clientes](respond-to-customer-reviews.md) e exibir [relatórios analíticos](analytics.md) não financeiros. Não pode fazer alterações em aplicativos, complementos ou configurações da conta.      |

A tabela a seguir mostra alguns dos recursos específicos disponíveis para cada uma dessas funções (e para o proprietário da conta).

|                                 |    Proprietário da conta                 |    Gerente                       |    Desenvolvedor                     |    Colaborador comercial    |    Colaborador financeiro    |    Comerciante                      |
|---------------------------------|----------------------------------|----------------------------------|----------------------------------|----------------------------|---------------------------|----------------------------------|
|    Relatório de aquisição           |    Pode exibir                      |    Pode exibir                      |     Sem acesso                    |     Sem acesso              |    Pode exibir               |    Sem acesso                     |
|    Relatório/respostas de comentários    |    Pode exibir e enviar comentários    |    Pode exibir e enviar comentários    |    Pode exibir e enviar comentários    |     Sem acesso              |     Sem acesso             |    Pode exibir e enviar comentários    |
|    Relatório de integridade                |    Pode exibir                      |    Pode exibir                      |    Pode exibir                      |    Pode exibir                |     Sem acesso             |    Sem acesso                     |
|    Relatório de uso                 |    Pode exibir                      |    Pode exibir                      |    Pode exibir                      |    Pode exibir                |     Sem acesso             |    Sem acesso                     |
|    Conta de pagamento               |    Pode atualizar                    |    Sem acesso                     |    Sem acesso                     |    Sem acesso               |    Pode exibir               |    Sem acesso                     |
|    Perfil de imposto                  |    Pode atualizar                    |    Sem acesso                     |    Sem acesso                     |    Sem acesso               |    Pode exibir               |    Sem acesso                     |
|    Resumo de pagamentos               |    Pode exibir                      |    Sem acesso                     |    Sem acesso                     |    Sem acesso               |    Pode exibir               |    Sem acesso                     |

Se nenhuma das funções padrão forem apropriadas, ou se você deseja limitar o acesso a aplicativos e/ou complementos específicos, você pode conceder permissões personalizadas ao usuário selecionando **Personalizar permissões**, como descrito abaixo.


<span id="custom" />
## <a name="assign-custom-permissions-to-account-users"></a>Atribuir permissões personalizadas para usuários de contas

Para atribuir permissões personalizadas em vez de funções padrão, clique em **Personalizar permissões** na seção **Funções** ao adicionar ou editar a conta de usuário. 

Para habilitar uma permissão para o usuário, mude a caixa para a configuração apropriada. 

![Guia para configurações de acesso](images/permission_key.png)

- **Sem acesso**: O usuário não terá a permissão indicada.
- **Somente leitura**: o usuário terá acesso a recursos de exibição relacionados à área indicada, mas não poderá fazer alterações. 
- **Leitura/gravação**: O usuário terá acesso para fazer alterações associadas à área, bem como exibi-la.
- **Misto**: Você não pode selecionar essa opção diretamente, mas o indicador **Misto** mostrará se você permitiu uma combinação de acesso para essa permissão. Por exemplo, se você conceder acesso **Somente leitura** a **Preço e disponibilidade** para **Todos os produtos**, mas, em seguida, conceder acesso **Leitura/gravação** a **Preço e disponibilidade** para um produto específico, o indicador **Preço e disponibilidade** para **Todos os produtos** será mostrado como Misto. O mesmo se aplica se alguns produtos tiverem **Sem acesso** para uma permissão, mas outros tiverem acesso **Leitura/gravação** e/ou **Somente leitura**.

Para algumas permissões, como aquelas relacionadas à exibição de dados analíticos, apenas acesso **Somente leitura** pode ser concedido. Observe que, na implementação atual, algumas permissões não têm nenhuma distinção entre os acessos **Somente leitura** e **Leitura/gravação**. Revise os detalhes de cada permissão para entender os recursos específicos concedidos pelos acessos **Somente leitura** e/ou **Leitura/gravação**.

Os detalhes específicos sobre cada permissão são descritos nas tabelas abaixo.

## <a name="account-level-permissions"></a>Permissões em nível de conta

As permissões nesta seção não podem ser limitadas a produtos específicos. Conceder acesso a essas permissões permite ao usuário ter essa permissão para a conta inteira.

<table>
    <colgroup>
    <col width="20%" />
    <col width="40%" />
    <col width="40%" />
    </colgroup>
    <thead>
    <tr class="header">
    <th align="left">Nome da permissão</th>
    <th align="left">Somente leitura</th>
    <th align="left">Leitura/gravação</th>
    </tr>
    </thead>
    <tbody>
<tr><td align="left">    **Configurações da conta**                    </td><td align="left">  Pode exibir todas as páginas da seção **Configurações da conta**, incluindo [informações de contato](managing-your-profile.md).       </td><td align="left">  Pode exibir todas as páginas na seção **Configurações da conta**. Pode fazer alterações para [informações de contato](managing-your-profile.md) e outras páginas, mas não é possível alterar a conta de pagamento ou o perfil fiscal (a menos que essa permissão seja concedida separadamente).            </td></tr>
<tr><td align="left">    **Usuários de contas**                       </td><td align="left">  Pode exibir os usuários que foram adicionados à conta na seção **Usuários**.          </td><td align="left">  Pode adicionar usuários à conta e fazer alterações em usuários existentes na seção **Usuários**.             </td></tr>
<tr><td align="left">    **Relatório de desempenho de anúncios no nível da conta** </td><td align="left">  Pode exibir o [Relatório de desempenho de anúncios](advertising-performance-report.md) no nível da conta.      </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Campanhas publicitárias**                        </td><td align="left">  Pode exibir [campanhas publicitárias](create-an-ad-campaign-for-your-app.md) criadas na conta.      </td><td align="left">  Pode criar, gerenciar e exibir [campanhas publicitárias](create-an-ad-campaign-for-your-app.md) criadas na conta.          </td></tr>
<tr><td align="left">    **Controle de anúncios**                        </td><td align="left">  Pode exibir [configurações de controle de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) para todos os produtos na conta.    </td><td align="left">  Pode exibir e alterar [configurações de controle de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) para todos os produtos na conta.        </td></tr>
<tr><td align="left">    **Relatórios de controle de anúncios**                </td><td align="left">  Pode exibir o [Relatório de controle de anúncios](ad-mediation-report.md) para todos os produtos na conta.    </td><td align="left">  N/D    </td></tr>
<tr><td align="left">    **Relatórios de desempenho de anúncios**              </td><td align="left">  Pode exibir [Relatórios de desempenho de anúncios](advertising-performance-report.md) para todos os produtos na conta.       </td><td align="left">  N/D         </td></tr>
<tr><td align="left">    **Unidades de anúncio**                            </td><td align="left">  Pode exibir as [unidades de anúncio](in-app-ads.md) que foram criadas para a conta.    </td><td align="left">  Pode criar, gerenciar e exibir [unidades de anúncio](in-app-ads.md) para a conta.             </td></tr>
<tr><td align="left">    **Anúncios de afiliadas**                       </td><td align="left">  Pode exibir o uso de [anúncios de afiliadas](about-affiliate-ads.md) em todos os produtos na conta.    </td><td align="left">  Pode gerenciar e exibir o uso de [anúncios de afiliadas](about-affiliate-ads.md) para todos os produtos na conta.                </td></tr>
<tr><td align="left">    **Relatórios de desempenho de afiliadas**      </td><td align="left">  Pode exibir o [Relatório de desempenho de afiliadas](affiliates-performance-report.md) para todos os produtos na conta.   </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Relatórios de anúncios de instalação de aplicativo**             </td><td align="left">  Pode exibir o [Relatório de campanha publicitária](promote-your-app-report.md).           </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Anúncios de comunidade**                       </td><td align="left">  Pode exibir grátis o uso de [anúncio de comunidade](about-community-ads.md) para todos os produtos na conta.          </td><td align="left">  Pode criar, gerenciar e exibir grátis o uso de [anúncio de comunidade](about-community-ads.md) para todos os produtos na conta.               </td></tr>
<tr><td align="left">    **Informações de contato**                        </td><td align="left">  Pode exibir [informações de contato](managing-your-profile.md) na seção Configurações da conta.        </td><td align="left">  Pode editar e exibir [informações de contato](managing-your-profile.md) na seção Configurações da conta.            </td></tr>
<tr><td align="left">    **Conformidade com o COPPA**                    </td><td align="left">  Pode exibir as seleções de [conformidade com o COPPA](in-app-ads.md#coppa-compliance) (indicando se os produtos são destinados a crianças com menos de 13 anos) para todos os produtos na conta.                                            </td><td align="left">  Pode editar e exibir as seleções de [conformidade com o COPPA](in-app-ads.md#coppa-compliance) (indicando se os produtos são destinados a crianças com menos de 13 anos) para todos os produtos na conta.         </td></tr>
<tr><td align="left">    **Grupos de clientes**                     </td><td align="left">  Pode exibir [grupos de clientes](create-customer-groups.md) (segmentos e grupos de versão de pré-lançamento) na seção **Clientes**.      </td><td align="left">  Pode criar, editar e exibir [grupos de clientes](create-customer-groups.md) (segmentos e grupos de versão de pré-lançamento) na seção **Clientes**.       </td></tr>
<tr><td align="left">    **Novos aplicativos**                            </td><td align="left">  Pode exibir a nova página de criação de aplicativo, mas, na verdade, não é possível criar novos aplicativos na conta.    </td><td align="left">  Pode [criar novos aplicativos](create-your-app-by-reserving-a-name.md) na conta reservando novos nomes de aplicativo, e pode criar envios e enviar aplicativos para a Store.     </td></tr>
<tr><td align="left">    **Novos pacotes**&nbsp;*                       </td><td align="left">  Pode exibir a nova página de criação de pacotes, mas, na verdade, não é possível criar novos pacotes na conta.     </td><td align="left">  Pode criar novos pacotes de produtos.          </td></tr>
<tr><td align="left">    **Serviços para parceiros**&nbsp;*                  </td><td align="left">  Pode exibir certificados de instalação para serviços para recuperar XTokens.     </td><td align="left">  Pode gerenciar e exibir certificados de instalação para serviços para recuperar XTokens.       </td></tr>
<tr><td align="left">    **Conta de pagamento**                      </td><td align="left">  Pode exibir [as informações de conta de pagamento](setting-up-your-payout-account-and-tax-forms.md#payout-account) em **Configurações da conta**.     </td><td align="left">  Pode editar e exibir [as informações de conta de pagamento](setting-up-your-payout-account-and-tax-forms.md#payout-account) em **Configurações da conta**.       </td></tr>
<tr><td align="left">    **Resumo de pagamentos**                      </td><td align="left">  Pode exibir o [Resumo de pagamentos](payout-summary.md) para acessar e baixar o relatório de informações de pagamento.       </td><td align="left">  Pode exibir o [Resumo de pagamentos](payout-summary.md) para acessar e baixar o relatório de informações de pagamento.   </td></tr>
<tr><td align="left">    **Partes confiáveis**&nbsp;*                   </td><td align="left">  Pode exibir partes confiáveis para recuperar XTokens.    </td><td align="left">  Pode gerenciar e exibir partes confiáveis para recuperar XTokens.     </td></tr>
<tr><td align="left">    **Solicitação de discos**&nbsp;*                   </td><td align="left">  Pode exibir solicitações de disco do jogo.    </td><td align="left">  Pode criar e exibir solicitações de disco do jogo.     </td></tr>
<tr><td align="left">    **Áreas de Segurança**&nbsp;*                         </td><td align="left">  Pode acessar a página **Áreas de Segurança** e visualizar as áreas restritas na conta e quaisquer configurações aplicáveis a essas áreas de segurança. Não pode exibir os produtos e envios de cada área restrita, a menos que as permissões apropriadas em nível de produto sejam concedidas. </td><td align="left">  Pode acessar a página **Áreas de Segurança** e visualizar e gerenciar as áreas restritas na conta, incluindo a criação e a exclusão de áreas de segurança e o gerenciamento de suas configurações. Não pode exibir os produtos e envios de cada área restrita, a menos que as permissões apropriadas em nível de produto sejam concedidas.    </td></tr>
<tr><td align="left">    **Perfil de imposto**                         </td><td align="left">  Pode exibir [informações do perfil fiscal e formulários fiscais](setting-up-your-payout-account-and-tax-forms.md#tax-forms) em **Configurações da conta**.     </td><td align="left">  Pode preencher os formulários fiscais e atualizar [informações do perfil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) em **Configurações da conta**.     </td></tr>
<tr><td align="left">    **Contas teste**&nbsp;*                     </td><td align="left">  Pode exibir contas para testar a configuração do Xbox Live.      </td><td align="left">  Pode criar, gerenciar e exibir contas para testar a configuração do Xbox Live.      </td></tr>
<tr><td align="left">    **Dispositivos Xbox**                        </td><td align="left">  Pode exibir os consoles de desenvolvimento do Xbox habilitados para a conta na seção **Configurações da conta**.       </td><td align="left">  Pode adicionar, remover e exibir os consoles de desenvolvimento do Xbox habilitados para a conta na seção **Configurações da conta**.     </td></tr>
    </tbody>
    </table>

\ * Permissões marcadas com um asterisco (*) concedem acesso a recursos que não estão disponíveis para todas as contas. Se sua conta não tiver sido habilitada para esses recursos, suas seleções para essas permissões não terão nenhum efeito.   


## <a name="product-level-permissions"></a>Permissões em nível de produto

As permissões nesta seção podem ser concedidas a todos os produtos na conta, ou podem ser personalizadas para possibilitar a permissão somente para um ou mais produtos específicos. 

As permissões de nível de produto são agrupadas em quatro categorias: **Análises**, **Monetização**, **Publicação** e **Xbox Live**. Você pode expandir cada uma dessas categorias para exibir as permissões individuais em cada categoria. Você também tem a opção de habilitar **Todas as permissões** para um ou mais produtos específicos.

Para conceder uma permissão a cada produto na conta, faça suas seleções para essa permissão (alternando a caixa para indicar **Somente leitura** ou **Leitura/gravação**) na linha marcada **Todos os produtos**. 
 
> [!TIP]
> As seleções feitas para **Todos os produtos** serão aplicadas a todos os produtos atualmente na conta, bem como a quaisquer produtos futuros criados na conta. Para evitar que as permissões sejam aplicadas a produtos futuros, selecione todos os produtos individualmente em vez de escolher **Todos os produtos**.

Abaixo da linha **Todos os produtos**, você verá cada produto na conta listado em uma linha separada. Para conceder uma permissão a apenas um produto específico, faça suas seleções para essa permissão na linha para esse produto.

Cada complemento está listado em uma linha separada abaixo de seu produto pai, com uma linha **Todos os complementos**. As seleções feitas para **Todos os complementos** serão aplicadas a todos os complementos atuais para esse produto, bem como a quaisquer complementos futuros criados para esse produto.

Observe que algumas permissões não podem ser definidas para complementos. Isso ocorre porque elas não se aplicam aos complementos (por exemplo, a permissão **Comentários dos clientes**) ou porque a permissão concedida no nível do produto pai se aplica a todos os complementos para aquele produto (por exemplo, **Códigos promocionais**). Observe, porém, que nenhuma permissão disponível para complementos deve ser definida separadamente; os complementos não herdam as seleções feitas para o produto pai. Por exemplo, se você deseja permitir que um usuário faça seleções de preço e disponibilidade de um complemento, você precisa habilitar a permissão **Preço e disponibilidade** para o complemento (ou para **Todos os complementos**), se você tiver concedido ou não a permissão **Preço e disponibilidade** para o produto pai. 


### <a name="analytics"></a>Análises

<table>
    <thead>
    <tr class="header">
    <th align="left">Nome da&nbsp;permissão</th>
    <th align="left">Somente&nbsp;leitura</th>
    <th align="left">Leitura/gravação</th>
    <th align="left">Somente&nbsp;leitura&nbsp;(complemento) </th>
    <th align="left">Leitura&#8209;gravação&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Aquisições**     </td><td>    Pode exibir os relatórios [Aquisições](acquisitions-report.md) e [Aquisições de complemento](add-on-acquisitions-report.md) do produto.        </td><td>    N/D    </td><td>    N/D (configurações do produto pai incluem relatórios de aquisição de complemento)        </td><td>    N/D                         </td></tr>
    <tr><td align="left">    **Uso** </td><td>    Pode exibir o [Relatório de uso](usage-report.md) do produto.     </td><td>    N/D       </td><td>    N/A     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Health** </td><td>    Pode exibir o [Relatório de integridade](health-report.md) do produto.    </td><td>    N/D     </td><td>    N/A     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Comentários do cliente**    </td><td>    Pode exibir os relatórios [análises](reviews-report.md) e [Comentários](feedback-report.md) do produto.       </td><td>    N/D (para responder a comentários ou análises, a permissão **Contato com cliente** deve ser concedida)   </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Análise do Xbox** </td><td>    Pode exibir o relatório de análise do Xbox para o produto. (Observação: este relatório ainda não está disponível.)    </td><td>    N/D   </td><td>    N/A       </td><td>    N/D          </td></tr>
    <tr><td align="left">    **Tempo real**   </td><td>    Pode exibir o relatório em tempo real para o produto. (observação: este relatório está disponível no momento pelo [Programa Insider do Centro de Desenvolvimento](dev-center-insider-program.md).)      </td><td>    N/D   </td><td>    N/A     </td><td>    N/D                 </td></tr>
    </tbody>
    </table>

### <a name="monetization"></a>Monetização

<table>
    <thead>
    <tr class="header">
    <th align="left">Nome&nbsp;da permissão</th>
    <th align="left">Somente&nbsp;leitura</th>
    <th align="left">Leitura/gravação</th>
    <th align="left">Somente&nbsp;leitura&nbsp;(complemento) </th>
    <th align="left">Leitura&#8209;gravação&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Códigos promocionais**     </td><td>    Pode exibir pedidos de [código promocional](generate-promotional-codes.md) e informações de uso para o produto e seus complementos, e pode exibir informações de uso.         </td><td>    Pode exibir, gerenciar e criar pedidos de [código promocional](generate-promotional-codes.md) para o produto e seus complementos, e pode exibir informações de uso.          </td><td>    N/D (configurações do produto pai se aplicam a todos os complementos)     </td><td>    N/D (configurações do produto pai se aplicam a todos os complementos)     </td></tr>
    <tr><td align="left">    **Ofertas direcionadas**     </td><td>    Pode exibir [ofertas direcionadas](use-targeted-offers-to-maximize-engagement-and-conversions.md) do produto.         </td><td>    Pode exibir, gerenciar e criar [ofertas direcionadas](use-targeted-offers-to-maximize-engagement-and-conversions.md) para o produto.          </td><td>    N/D     </td><td>    N/D      </td></tr>
    <tr><td align="left">    **Contato com cliente**  </td><td>    Pode exibir [respostas aos comentários dos clientes](respond-to-customer-feedback.md) e [respostas às críticas dos clientes](respond-to-customer-reviews.md), desde que a permissão de **Comentários dos clientes** também tenha sido concedida. Também pode exibir [notificações direcionadas](send-push-notifications-to-your-apps-customers.md) que foram criadas para o produto.    </td><td>    Pode [responder aos comentários dos clientes](respond-to-customer-feedback.md), [responder às críticas dos clientes](respond-to-customer-reviews.md), desde que a permissão de **Comentários dos clientes** também tenha sido concedida. Também pode [criar e enviar notificações direcionadas](send-push-notifications-to-your-apps-customers.md) para o produto.                   </td><td>    N/D         </td><td>    N/D                          </td></tr>
    <tr><td align="left">    **Experimentação**</td><td>    Pode exibir [experimentos (testes A/B)](../monetize/run-app-experiments-with-a-b-testing.md) e exibir dados de experimentação do produto.   </td><td>    Pode criar, gerenciar e exibir [experimentos (testes A/B)](../monetize/run-app-experiments-with-a-b-testing.md) para o produto, e exibir os dados de experimentação.     </td><td>    N/D  </td><td>    N/D                 </td></tr>

    </tbody>
    </table>

### <a name="publishing"></a>Publicação 

<table>
    <thead>
    <tr class="header">
    <th align="left">Nome&nbsp;da permissão</th>
    <th align="left">Somente&nbsp;leitura</th>
    <th align="left">Leitura/gravação</th>
    <th align="left">Somente&nbsp;leitura&nbsp;(complemento) </th>
    <th align="left">Leitura&#8209;gravação&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Preço e disponibilidade**  </td><td>    Pode exibir a página [Preço e disponibilidade](set-app-pricing-and-availability.md) dos envios de produto.     </td><td>    Pode exibir e editar a página [Preço e disponibilidade](set-app-pricing-and-availability.md) dos envios de produto. </td><td>    Pode exibir a página [Preço e disponibilidade](set-add-on-pricing-and-availability.md) dos envios de complemento.   </td><td>    Pode exibir e editar a página [Preço e disponibilidade](set-add-on-pricing-and-availability.md) dos envios de complemento.          </td></tr>
    <tr><td align="left">    **Propriedades**   </td><td>    Pode exibir a página [Propriedades](enter-app-properties.md) dos envios de produto.      </td><td>    Pode exibir e editar a página [Propriedades](enter-app-properties.md) dos envios de produto.       </td><td>    Pode exibir a página [Propriedades](enter-add-on-properties.md) dos envios de complemento.     </td><td>    Pode exibir e editar a página [Propriedades](enter-add-on-properties.md) dos envios de complemento.               </td></tr>
    <tr><td align="left">    **Classificações etárias**    </td><td>    Pode exibir a página [Classificações etárias](age-ratings.md) dos envios de produto.       </td><td>    Pode exibir e editar a página [Classificações etárias](age-ratings.md) dos envios de produto.    </td><td>    * Pode exibir a página de Classificações etárias dos envios de complemento.          </td><td>    * Pode exibir e editar a página de Classificações etárias dos envios de complemento.       </td></tr>
    <tr><td align="left">    **Pacotes**        </td><td>    Pode exibir a página [Pacotes](upload-app-packages.md) para envios de produto.  </td><td>    Pode exibir e editar a página [Pacotes](upload-app-packages.md) para envios de produto, incluindo carregar pacotes.     </td><td>    * Pode exibir direcionamento e pacotes de família de dispositivos (se aplicável) para envios de complemento.   </td><td>    * Pode exibir e editar direcionamento da família de dispositivos para envios de complemento, incluindo carregando pacotes, se aplicável.             </td></tr>
    <tr><td align="left">    **Listagens da Store**  </td><td>    Pode exibir a(s) [página(s) Listagem da Store](create-app-store-listings.md) para envios de produto.  </td><td>    Pode exibir e editar a(s) [página(s) Listagem da Store](create-app-store-listings.md) para envios de produto, e pode adicionar novas listagens da Store para vários idiomas.     </td><td>    Pode exibir a(s) [página(s) Listagem da Store](create-add-on-store-listings.md) para envios de complemento.            </td><td>    Pode exibir e editar a(s) [página(s) Listagem da Store](create-add-on-store-listings.md) para envios de complemento, e pode adicionar novas listagens da Store para vários idiomas.                 </td></tr>
    <tr><td align="left">    **Envio à Store**     </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.           </td><td>    Pode enviar o produto para a Store e exibir relatórios de certificação. Inclui envios novos e atualizados. </td><td>Não é concedido acesso se essa permissão for definida como somente leitura.     </td><td>    Pode enviar o complemento para a Store e exibir relatórios de certificação. Inclui envios novos e atualizados.</td></tr>
    <tr><td align="left">    **Criação de envio novo**       </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.        </td><td>    Pode criar novos [envios](app-submissions.md) para o produto.  </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.   </td><td>    Pode criar novos [envios](add-on-submissions.md) para o complemento.        </td></tr>
    <tr><td align="left">    **Novos complementos**    </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura. </td><td>    Pode [criar novos complementos](set-your-add-on-product-id.md) para o produto. </td><td>    N/D    </td><td>    N/D        </td></tr>
    <tr><td align="left">    **Reservas de nome**   </td><td>    Pode exibir a página [Gerenciar nomes de aplicativo](manage-app-names.md) do produto.</td><td>    Pode exibir e editar a página [Gerenciar nomes de aplicativo](manage-app-names.md) do produto, incluindo reservar nomes adicionais e excluir nomes reservados. </td><td>   * Pode exibir nomes reservados para o complemento.    </td><td>   * Pode exibir e editar nomes reservados para o complemento.          </td></tr>
    </tbody>
    </table>

### <a name="xbox-live-"></a>Xbox Live \*

<table>
    <thead>
    <tr class="header">
    <th align="left">Nome&nbsp;da permissão</th>
    <th align="left">Somente&nbsp;leitura</th>
    <th align="left">Leitura/gravação</th>
    <th align="left">Somente&nbsp;leitura&nbsp;(complemento) </th>
    <th align="left">Leitura&#8209;gravação&nbsp;(complemento)</th>
    </tr>
    </thead>
    <tbody>
    <tr><td align="left">    **Canais de aplicativos**&nbsp;\*</td><td>    N/D  </td><td>    Pode publicar canais de vídeo promocionais para o console do Xbox para exibição por meio do OneGuide.  </td><td>  N/D </td><td> N/D </td></tr>
    <tr><td align="left">    **Configuração do serviço**&nbsp;\*    </td><td>    Pode exibir configurações relacionadas a conquistas, vários jogadores, placares de líderes e outras configurações do Xbox Live para o produto.  </td><td>    Pode exibir e editar configurações relacionadas a conquistas, vários jogadores, placares de líderes e outras configurações do Xbox Live para o produto.  </td><td>    N/D     </td><td>    N/D                      </td></tr>
</tbody>
</table>

\ * Permissões marcadas com um asterisco (*) concedem acesso a recursos que não estão disponíveis para todas as contas. Se sua conta não tiver sido habilitada para esses recursos, suas seleções para essas permissões não terão nenhum efeito.  
