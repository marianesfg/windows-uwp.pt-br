---
author: jnHs
Description: Set roles or custom permissions for account users.
title: Definir funções ou permissões personalizadas para usuários de contas
ms.assetid: 99f3aa18-98b4-4919-bd7b-d78356b0bf78
ms.author: wdg-dev-content
ms.date: 10/31/2018
ms.topic: article
keywords: windows 10, uwp, funções de usuário, permissão do usuário, funções personalizadas, acesso de usuário, personalizar permissões, funções padrão
ms.localizationpriority: medium
ms.openlocfilehash: 2af203ae78ae34a0a6bc9884cbaeaa730ee83e9b
ms.sourcegitcommit: 38f06f1714334273d865935d9afb80efffe97a17
ms.translationtype: MT
ms.contentlocale: pt-BR
ms.lasthandoff: 11/09/2018
ms.locfileid: "6183303"
---
# <a name="set-roles-or-custom-permissions-for-account-users"></a>Definir funções ou permissões personalizadas para usuários de contas

Quando você [Adicionar usuários à sua conta do Partner Center](add-users-groups-and-azure-ad-applications.md), você precisará especificar o acesso que eles têm na conta. Você pode fazer isso ao atribuir [funções padrão](#roles), que se aplicam à conta inteira, ou [personalizar as permissões deles](#custom) para fornecer o nível apropriado de acesso. Algumas das permissões personalizadas se aplicam à conta inteira, e algumas podem ser limitadas a um ou mais produtos específicos (ou concedidas a todos os produtos, se desejar).

> [!NOTE] 
> As mesmas funções e permissões podem ser aplicadas, independentemente de estar adicionando um usuário, um grupo ou um aplicativo Azure AD.

Ao determinar qual função ou permissão aplicar, lembre-se: 
-   Os usuários (incluindo grupos e aplicativos Azure AD) serão capazes de acessar toda a conta do Partner Center com as permissões associadas às suas funções atribuídas, a menos que você [Personalizar permissões](#custom) e atribuir [permissões de nível de produto](#product-level-permissions) para que ele só possa trabalhar com aplicativos específicos e/ou complementos.
-   Você pode permitir que um usuário, grupo ou aplicativo Azure AD tenha acesso à funcionalidade de mais de uma função selecionando várias funções, ou usando permissões personalizadas para conceder o acesso que você queira.
-   Um usuário com uma determinada função (ou conjunto de permissões personalizadas) também pode fazer parte de um grupo que tenha uma função diferente (ou conjunto de permissões). Nesse caso, o usuário terá acesso a toda a funcionalidade associada ao grupo e à conta individual.

> [!TIP]
> Este tópico é específico para o programa de desenvolvedor de aplicativos do Windows no [Partner Center](https://partner.microsoft.com/dashboard). Para obter informações sobre funções de usuário no programa de desenvolvedores de hardware, consulte [Gerenciamento de funções de usuário](https://docs.microsoft.com/windows-hardware/drivers/dashboard/managing-user-roles). Para obter informações sobre funções de usuário no Programa de Aplicativo da Área de Trabalho do Windows, consulte [Programa de Aplicativo da Área de Trabalho do Windows](https://docs.microsoft.com/windows/desktop/appxpkg/windows-desktop-application-program#add-and-manage-account-users).


<span id="roles" />

## <a name="assign-roles-to-account-users"></a>Atribuir funções a usuários da conta

Por padrão, um conjunto de funções padrão é apresentado para escolher quando você adiciona um usuário, grupo ou aplicativo Azure AD à sua conta do Partner Center. Cada função tem um conjunto específico de permissões para executar determinadas funções na conta. 

A menos que você defina [permissões personalizadas](#custom) ao selecionar **Personalizar permissões**, cada usuário, grupo ou aplicativo Azure AD adicionado a uma conta deve receber pelo menos uma das funções padrão a seguir. 

> [!NOTE]
> O **proprietário** da conta é a pessoa que a criou com uma conta da Microsoft (e não qualquer usuário adicionado pelo Azure AD). Esse proprietário da conta é a única pessoa com acesso completo à conta, incluindo a capacidade de excluir aplicativos, criar e editar todos os usuários da conta e alterar todas as informações financeiras e configurações da conta. 


| Função                 | Descrição              |
|----------------------|--------------------------|
| Gerente              | Tem acesso completo à conta, exceto para alterar configurações de imposto e pagamento. Isso inclui o gerenciamento de usuários no Partner Center, mas observe que a capacidade de criar e excluir usuários no locatário do Azure AD é dependente da permissão da conta no Azure AD. Ou seja, se um usuário receber a função gerente, mas não tem permissões de administrador global da organização do Azure AD, não será possível criar novos usuários ou excluir usuários do diretório (embora seja possível alterar a função do usuário do Partner Center). <p> Observe que, se a conta do Partner Center estiver associada a mais de um locatário do Azure AD, um gerente não poderá ver detalhes completos de um usuário (incluindo nome, sobrenome, email de recuperação de senha, e se ele é um administrador global do Azure AD), a menos que eles são conectado ao mesmo locatário que o usuário com uma conta que tenha permissões de administrador global para esse locatário. No entanto, eles podem adicionar e remover usuários em qualquer locatário associado à conta do Partner Center. |
| Desenvolvedor            | Pode carregar pacotes e enviar aplicativos e complementos, além de exibir o [Relatório de uso](usage-report.md) para obter detalhes de telemetria. Pode acessar a funcionalidade de [Experiências entre dispositivos](https://go.microsoft.com/fwlink/?linkid=874042) . Não pode exibir informações financeiras ou configurações da conta.   |
| Colaborador comercial | Pode exibir relatórios de [Integridade](health-report.md) e de [Uso](usage-report.md). Não é possível criar ou enviar produtos, alterar configurações de conta ou exibir informações financeiras.   |
| Colaborador financeiro  | Pode exibir [relatórios de pagamento](payout-summary.md), informações financeiras e relatórios de aquisição. Não pode fazer alterações em aplicativos, complementos ou configurações da conta.    |
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

Para algumas permissões, como aquelas relacionadas à exibição de dados analíticos, apenas acesso **Somente leitura** pode ser concedido. Observe que, na implementação atual, algumas permissões não têm nenhuma distinção entre os acessos **Somente leitura** e **Leitura/gravação**. Examine os detalhes de cada permissão para entender os recursos específicos concedidos pelos acessos **Somente leitura** e/ou **Leitura/gravação**.

Os detalhes específicos sobre cada permissão são descritos nas tabelas abaixo.

## <a name="account-level-permissions"></a>Permissões em nível de conta

As permissões nesta seção não podem ser limitadas a produtos específicos. Conceder acesso a uma dessas permissões permite ao usuário ter essa permissão para a conta inteira.

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
<tr><td align="left">    <b>Configurações da conta</b>                    </td><td align="left">  Pode exibir todas as páginas da seção <b>Configurações da conta</b>, incluindo <a href="managing-your-profile.md">informações de contato</a>.       </td><td align="left">  Pode exibir todas as páginas na seção <b>Configurações da conta</b>. Pode fazer alterações para <a href="managing-your-profile.md">informações de contato</a> e outras páginas, mas não é possível alterar a conta de pagamento ou o perfil fiscal (a menos que essa permissão seja concedida separadamente).            </td></tr>
<tr><td align="left">    <b>Usuários de contas</b>                       </td><td align="left">  Pode exibir os usuários que foram adicionados à conta na seção <b>Usuários</b>.          </td><td align="left">  Pode adicionar usuários à conta e fazer alterações em usuários existentes na seção <b>Usuários</b>.             </td></tr>
<tr><td align="left">    <b>Relatório de desempenho de anúncios no nível da conta</b> </td><td align="left">  Pode exibir o <a href="advertising-performance-report.md">Relatório de desempenho de anúncios</a> no nível da conta.      </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Campanhas publicitárias</b>                        </td><td align="left">  Pode exibir <a href="create-an-ad-campaign-for-your-app.md">campanhas publicitárias</a> criadas na conta.      </td><td align="left">  Pode criar, gerenciar e exibir <a href="create-an-ad-campaign-for-your-app.md">campanhas publicitárias</a> criadas na conta.          </td></tr>
<tr><td align="left">    <b>Controle de anúncios</b>                        </td><td align="left">  Pode exibir configurações de controle de anúncios para todos os produtos na conta.    </td><td align="left">  Pode exibir e alterar configurações de controle de anúncios para todos os produtos na conta.        </td></tr>
<tr><td align="left">    <b>Relatórios de controle de anúncios</b>                </td><td align="left">  Pode exibir o <a href="ad-mediation-report.md">Relatório de controle de anúncios</a> para todos os produtos na conta.    </td><td align="left">  N/D    </td></tr>
<tr><td align="left">    <b>Relatórios de desempenho de anúncios</b>              </td><td align="left">  Pode exibir <a href="advertising-performance-report.md">Relatórios de desempenho de anúncios</a> para todos os produtos na conta.       </td><td align="left">  N/D         </td></tr>
<tr><td align="left">    <b>Unidades de anúncio</b>                            </td><td align="left">  Pode exibir as <a href="in-app-ads.md">unidades de anúncio</a> que foram criadas para a conta.    </td><td align="left">  Pode criar, gerenciar e exibir <a href="in-app-ads.md">unidades de anúncio</a> para a conta.             </td></tr>
<tr><td align="left">    <b>Anúncios de afiliadas</b>                       </td><td align="left">  Pode exibir o uso de <a href="about-affiliate-ads.md">anúncios de afiliadas</a> em todos os produtos na conta.    </td><td align="left">  Pode gerenciar e exibir o uso de <a href="about-affiliate-ads.md">anúncios de afiliadas</a> para todos os produtos na conta.                </td></tr>
<tr><td align="left">    <b>Relatórios de desempenho de afiliadas</b>      </td><td align="left">  Pode exibir o <a href="affiliates-performance-report.md">Relatório de desempenho de afiliadas</a> para todos os produtos na conta.   </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Relatórios de anúncios de instalação de aplicativo</b>             </td><td align="left">  Pode exibir o <a href="promote-your-app-report.md">Relatório de campanha publicitária</a>.           </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    <b>Anúncios de comunidade</b>                       </td><td align="left">  Pode exibir grátis o uso de <a href="about-community-ads.md">anúncio de comunidade</a> para todos os produtos na conta.          </td><td align="left">  Pode criar, gerenciar e exibir grátis o uso de <a href="about-community-ads.md">anúncio de comunidade</a> para todos os produtos na conta.               </td></tr>
<tr><td align="left">    <b>Informações de contato</b>                        </td><td align="left">  Pode exibir <a href="managing-your-profile.md">informações de contato</a> na seção Configurações da conta.        </td><td align="left">  Pode editar e exibir <a href="managing-your-profile.md">informações de contato</a> na seção Configurações da conta.            </td></tr>
<tr><td align="left">    <b>Conformidade com o COPPA</b>                    </td><td align="left">  Pode exibir as seleções de <a href="in-app-ads.md#coppa-compliance">conformidade com o COPPA</a> (indicando se os produtos são destinados a crianças com menos de 13 anos) para todos os produtos na conta.                                            </td><td align="left">  Pode editar e exibir as seleções de <a href="in-app-ads.md#coppa-compliance">conformidade com o COPPA</a> (indicando se os produtos são destinados a crianças com menos de 13 anos) para todos os produtos na conta.         </td></tr>
<tr><td align="left">    <b>Grupos de clientes</b>                     </td><td align="left">  Pode exibir <a href="create-customer-groups.md">grupos de clientes</a> (segmentos e grupos de usuários conhecidos).      </td><td align="left">  Pode criar, editar e exibir <a href="create-customer-groups.md">grupos de clientes</a> (segmentos e grupos de usuários conhecidos).       </td></tr>
<tr><td align="left">    <b>Gerenciar grupos de produtos</b>&nbsp;*                            </td><td align="left">  Pode exibir a nova página de criação de grupo de produtos, mas não pode criar novos grupos de produtos.    </td><td align="left">  Pode criar e editar grupos de produtos.     </td></tr>
<tr><td align="left">    <b>Novos aplicativos</b>                            </td><td align="left">  Pode exibir a nova página de criação de aplicativo, mas, na verdade, não é possível criar novos aplicativos na conta.    </td><td align="left">  Pode <a href="create-your-app-by-reserving-a-name.md">criar novos aplicativos</a> na conta reservando novos nomes de aplicativo, e pode criar envios e enviar aplicativos para a Store.     </td></tr>
<tr><td align="left">    <b>Novos pacotes</b>&nbsp;*                       </td><td align="left">  Pode exibir a nova página de criação de pacotes, mas, na verdade, não é possível criar novos pacotes na conta.     </td><td align="left">  Pode criar novos pacotes de produtos.          </td></tr>
<tr><td align="left">    <b>Serviços para parceiros</b>&nbsp;*                  </td><td align="left">  Pode exibir certificados de instalação para serviços para recuperar XTokens.     </td><td align="left">  Pode gerenciar e exibir certificados de instalação para serviços para recuperar XTokens.       </td></tr>
<tr><td align="left">    <b>Conta de pagamento</b>                      </td><td align="left">  Pode exibir <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">as informações de conta de pagamento</a> em <b>Configurações da conta</b>.     </td><td align="left">  Pode editar e exibir <a href="setting-up-your-payout-account-and-tax-forms.md#payout-account">as informações de conta de pagamento</a> em <b>Configurações da conta</b>.       </td></tr>
<tr><td align="left">    <b>Resumo de pagamentos</b>                      </td><td align="left">  Pode exibir o <a href="payout-summary.md">Resumo de pagamentos</a> para acessar e baixar o relatório de informações de pagamento.       </td><td align="left">  Pode exibir o <a href="payout-summary.md">Resumo de pagamentos</a> para acessar e baixar o relatório de informações de pagamento.   </td></tr>
<tr><td align="left">    <b>Partes confiáveis</b>&nbsp;*                   </td><td align="left">  Pode exibir partes confiáveis para recuperar XTokens.    </td><td align="left">  Pode gerenciar e exibir partes confiáveis para recuperar XTokens.     </td></tr>
<tr><td align="left">    <b>Solicitação de discos</b>&nbsp;*                   </td><td align="left">  Pode exibir solicitações de disco do jogo.    </td><td align="left">  Pode criar e exibir solicitações de disco do jogo.     </td></tr>
<tr><td align="left">    <b>Áreas de Segurança</b>&nbsp;*                         </td><td align="left">  Pode acessar a página <b>Áreas de Segurança</b> e visualizar as áreas restritas na conta e quaisquer configurações aplicáveis a essas áreas de segurança. Não pode exibir os produtos e envios de cada área restrita, a menos que as permissões apropriadas em nível de produto sejam concedidas. </td><td align="left">  Pode acessar a página <b>Áreas de Segurança</b> e visualizar e gerenciar as áreas restritas na conta, incluindo a criação e a exclusão de áreas de segurança e o gerenciamento de suas configurações. Não pode exibir os produtos e envios de cada área restrita, a menos que as permissões apropriadas em nível de produto sejam concedidas.    </td></tr>
<tr><td align="left">    <b>Eventos de venda da Store</b>&nbsp;*                            </td><td align="left">  N/D    </td><td align="left">  Pode configurar a opção de incluir automaticamente produtos em eventos de venda da Store.     </td></tr>
<tr><td align="left">    <b>Perfil de imposto</b>                         </td><td align="left">  Pode exibir <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">informações do perfil fiscal e formulários fiscais</a> em <b>Configurações da conta</b>.     </td><td align="left">  Pode preencher os formulários fiscais e atualizar <a href="setting-up-your-payout-account-and-tax-forms.md#tax-forms">informações do perfil fiscal</a> em <b>Configurações da conta</b>.     </td></tr>
<tr><td align="left">    <b>Contas teste</b>&nbsp;*                     </td><td align="left">  Pode exibir contas para testar a configuração do Xbox Live.      </td><td align="left">  Pode criar, gerenciar e exibir contas para testar a configuração do Xbox Live.      </td></tr>
<tr><td align="left">    <b>Dispositivos Xbox</b>                        </td><td align="left">  Pode exibir os consoles de desenvolvimento do Xbox habilitados para a conta na seção <b>Configurações da conta</b>.       </td><td align="left">  Pode adicionar, remover e exibir os consoles de desenvolvimento do Xbox habilitados para a conta na seção <b>Configurações da conta</b>.     </td></tr>
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
    <tr><td align="left">    <b>Aquisições</b>     </td><td>    Pode exibir os relatórios <a href="acquisitions-report.md">Aquisições</a> e <a href="add-on-acquisitions-report.md">Aquisições de complemento</a> do produto.        </td><td>    N/D    </td><td>    N/d (configurações do produto pai incluem o relatório de **aquisições de complemento** )        </td><td>    N/D                         </td></tr>
    <tr><td align="left">    <b>Uso</b> </td><td>    Pode exibir o <a href="usage-report.md">Relatório de uso</a> do produto.     </td><td>    N/D       </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Health</b> </td><td>    Pode exibir o <a href="health-report.md">Relatório de integridade</a> do produto.    </td><td>    N/D     </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Comentários do cliente</b>    </td><td>    Pode exibir os relatórios <a href="reviews-report.md">análises</a> e <a href="feedback-report.md">Comentários</a> do produto.       </td><td>    N/D (para responder a comentários ou análises, a permissão <b>Contato com cliente</b> deve ser concedida)   </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    <b>Análise do Xbox</b> </td><td>    Pode exibir o [relatório de análise do Xbox](xbox-analytics-report.md) para o produto.    </td><td>    N/D   </td><td>    N/D       </td><td>    N/D          </td></tr>
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
    <tr><td align="left">    <b>Códigos promocionais</b>     </td><td>    Pode exibir pedidos de <a href="generate-promotional-codes.md">código promocional</a> e informações de uso para o produto e seus complementos, e pode exibir informações de uso.         </td><td>    Pode exibir, gerenciar e criar pedidos de <a href="generate-promotional-codes.md">código promocional</a> para o produto e seus complementos, e pode exibir informações de uso.          </td><td>    N/D (configurações do produto pai se aplicam a todos os complementos)     </td><td>    N/D (configurações do produto pai se aplicam a todos os complementos)     </td></tr>
    <tr><td align="left">    <b>Ofertas direcionadas</b>     </td><td>    Pode exibir <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">ofertas direcionadas</a> do produto.         </td><td>    Pode exibir, gerenciar e criar <a href="use-targeted-offers-to-maximize-engagement-and-conversions.md">ofertas direcionadas</a> para o produto.          </td><td>    N/D     </td><td>    N/D      </td></tr>
    <tr><td align="left">    <b>Contato com cliente</b>  </td><td>    Pode exibir <a href="respond-to-customer-feedback.md">respostas aos comentários dos clientes</a> e <a href="respond-to-customer-reviews.md">respostas às críticas dos clientes</a>, desde que a permissão de <b>Comentários dos clientes</b> também tenha sido concedida. Também pode exibir <a href="send-push-notifications-to-your-apps-customers.md">notificações direcionadas</a> que foram criadas para o produto.    </td><td>    Pode <a href="respond-to-customer-feedback.md">responder aos comentários dos clientes</a> e <a href="respond-to-customer-reviews.md">responder às críticas dos clientes</a>, desde que a permissão de <b>comentários dos clientes</b> tenha sido concedida. Também pode <a href="send-push-notifications-to-your-apps-customers.md">criar e enviar notificações direcionadas</a> para o produto.                   </td><td>    N/D         </td><td>    N/D                          </td></tr>
    <tr><td align="left">    <b>Experimentação</b></td><td>    Pode exibir <a href="../monetize/run-app-experiments-with-a-b-testing.md">experimentos (testes A/B)</a> e exibir dados de experimentação do produto.   </td><td>    Pode criar, gerenciar e exibir <a href="../monetize/run-app-experiments-with-a-b-testing.md">experimentos (testes A/B)</a> para o produto, e exibir os dados de experimentação.     </td><td>    N/D  </td><td>    N/D                 </td></tr>
    <tr><td align="left">    <b>Eventos de venda da Store</b>&nbsp;*</td><td>    Pode exibir o status de evento de venda do produto.   </td><td>    Pode adicionar o produto a eventos de venda e configurar descontos.      </td><td>    Pode exibir o status de evento de venda do produto.   </td><td>    Pode adicionar o produto a eventos de venda e configurar descontos.      </td></tr>
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
    <tr><td align="left">    <b>Preço e disponibilidade</b>  </td><td>    Pode exibir a página <a href="set-app-pricing-and-availability.md">Preço e disponibilidade</a> dos envios de produto.     </td><td>    Pode exibir e editar a página <a href="set-app-pricing-and-availability.md">Preço e disponibilidade</a> dos envios de produto. </td><td>    Pode exibir a página <a href="set-add-on-pricing-and-availability.md">Preço e disponibilidade</a> dos envios de complemento.   </td><td>    Pode exibir e editar a página <a href="set-add-on-pricing-and-availability.md">Preço e disponibilidade</a> dos envios de complemento.          </td></tr>
    <tr><td align="left">    <b>Propriedades</b>   </td><td>    Pode exibir a página <a href="enter-app-properties.md">Propriedades</a> dos envios de produto.      </td><td>    Pode exibir e editar a página <a href="enter-app-properties.md">Propriedades</a> dos envios de produto.       </td><td>    Pode exibir a página <a href="enter-add-on-properties.md">Propriedades</a> dos envios de complemento.     </td><td>    Pode exibir e editar a página <a href="enter-add-on-properties.md">Propriedades</a> dos envios de complemento.               </td></tr>
    <tr><td align="left">    <b>Classificações etárias</b>    </td><td>    Pode exibir a página <a href="age-ratings.md">Classificações etárias</a> dos envios de produto.       </td><td>    Pode exibir e editar a página <a href="age-ratings.md">Classificações etárias</a> dos envios de produto.    </td><td>    Pode exibir a página de Classificações etárias dos envios de complemento.          </td><td>     Pode exibir e editar a página de Classificações etárias dos envios de complemento.       </td></tr>
    <tr><td align="left">    <b>Pacotes</b>        </td><td>    Pode exibir a página <a href="upload-app-packages.md">Pacotes</a> para envios de produto.  </td><td>    Pode exibir e editar a página <a href="upload-app-packages.md">Pacotes</a> para envios de produto, incluindo carregar pacotes.     </td><td>   Pode exibir direcionamento e pacotes de família de dispositivos (se aplicável) para envios de complemento.   </td><td>     Pode exibir e editar direcionamento da família de dispositivos para envios de complemento, incluindo carregando pacotes, se aplicável.             </td></tr>
    <tr><td align="left">    <b>Listagens da Loja</b>  </td><td>    Pode exibir a(s) <a href="create-app-store-listings.md">página(s) Listagem da Store</a> para envios de produto.  </td><td>    Pode exibir e editar a(s) <a href="create-app-store-listings.md">página(s) Listagem da Store</a> para envios de produto, e pode adicionar novas listagens da Store para vários idiomas.     </td><td>    Pode exibir a(s) <a href="create-add-on-store-listings.md">página(s) Listagem da Store</a> para envios de complemento.            </td><td>    Pode exibir e editar a(s) <a href="create-add-on-store-listings.md">página(s) Listagem da Store</a> para envios de complemento, e pode adicionar novas listagens da Store para vários idiomas.                 </td></tr>
    <tr><td align="left">    <b>Envio à Store</b>     </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.           </td><td>    Pode enviar o produto para a Store e exibir relatórios de certificação. Inclui envios novos e atualizados. </td><td>Não é concedido acesso se essa permissão for definida como somente leitura.     </td><td>    Pode enviar o complemento para a Store e exibir relatórios de certificação. Inclui envios novos e atualizados.</td></tr>
    <tr><td align="left">    <b>Criação de envio novo</b>       </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.        </td><td>    Pode criar novos <a href="app-submissions.md">envios</a> para o produto.  </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.   </td><td>    Pode criar novos <a href="add-on-submissions.md">envios</a> para o complemento.        </td></tr>
    <tr><td align="left">    <b>Novos complementos</b>    </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura. </td><td>    Pode <a href="set-your-add-on-product-id.md">criar novos complementos</a> para o produto. </td><td>    N/D    </td><td>    N/D        </td></tr>
    <tr><td align="left">    <b>Reservas de nome</b>   </td><td>    Pode exibir a página <a href="manage-app-names.md">Gerenciar nomes de aplicativo</a> do produto.</td><td>    Pode exibir e editar a página <a href="manage-app-names.md">Gerenciar nomes de aplicativo</a> do produto, incluindo reservar nomes adicionais e excluir nomes reservados. </td><td>   Pode exibir nomes reservados para o complemento.    </td><td>   Pode exibir e editar nomes reservados para o complemento.          </td></tr>
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
    <tr><td align="left">    <b>Canais de aplicativos</b>&nbsp;*</td><td>    N/D  </td><td>    Pode publicar canais de vídeo promocionais para o console do Xbox para exibição por meio do OneGuide.  </td><td>  N/D </td><td> N/D </td></tr>
    <tr><td align="left">    <b>Configuração do serviço</b>&nbsp;*    </td><td>    Pode exibir configurações relacionadas a conquistas, vários jogadores, placares de líderes e outras configurações do Xbox Live para o produto.  </td><td>    Pode exibir e editar configurações relacionadas a conquistas, vários jogadores, placares de líderes e outras configurações do Xbox Live para o produto.  </td><td>    N/D     </td><td>    N/D                      </td></tr>
</tbody>
</table>

\ * Permissões marcadas com um asterisco (*) concedem acesso a recursos que não estão disponíveis para todas as contas. Se sua conta não tiver sido habilitada para esses recursos, suas seleções para essas permissões não terão nenhum efeito.  
