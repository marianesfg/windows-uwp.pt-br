---
author: jnHs
Description: "Definir permissões personalizadas para usuários de contas."
title: "Definir permissões personalizadas para usuários de contas"
ms.assetid: 
translationtype: Human Translation
ms.sourcegitcommit: 7dab1bf03bfc0920230d8cc57f48ad4a4f83e4d2
ms.openlocfilehash: 19874f940798e76a18b3377a295505f0aef201c7

---

# Definir permissões personalizadas para usuários de contas

Quando você adicionar usuários a sua conta, você pode oferecer a eles uma [função padrão](manage-account-users.md#roles-and-permissions), ou você pode optar por personalizar as permissões deles para fornecer o nível apropriado de acesso ao usuário. Algumas dessas permissões se aplicam à conta inteira, e algumas podem ser concedidas a todos os produtos ou limitadas a produtos específicos. 

Para usar permissões personalizadas em vez de funções padrão, clique em **Personalizar permissões** na seção **Funções** ao adicionar ou editar a conta de usuário. 

> **Observação** As mesmas permissões podem ser aplicadas, independentemente de estar adicionando um usuário, um grupo ou um aplicativo Azure AD.

Para habilitar uma permissão para o usuário, mude a caixa para a configuração apropriada. 

![Guia para configurações de acesso](images/permission_key.png)

- **Sem acesso**: O usuário não terá a permissão indicada.
- **Somente leitura**: O usuário terá acesso a recursos de exibição relacionados à área indicada, mas não poderá fazer alterações.
- **Leitura/gravação**: O usuário terá acesso para fazer alterações associadas à área, bem como exibi-la.
- **Misto**: Você não pode selecionar essa opção diretamente, mas o indicador **Misto** mostrará se você permitiu uma combinação de acesso para essa permissão. Por exemplo, se você conceder acesso **Somente leitura** a **Preço e disponibilidade** para **Todos os produtos**, mas, em seguida, conceder acesso **Leitura/gravação** a **Preço e disponibilidade** para um produto específico, o indicador **Preço e disponibilidade** para **Todos os produtos** será mostrado como Misto. O mesmo se aplica se alguns produtos tiverem **Sem acesso** para uma permissão, mas outros tiverem acesso **Leitura/gravação** e/ou **Somente leitura**.

Para algumas permissões, como aquelas relacionadas à exibição de dados analíticos, apenas acesso **Somente leitura** pode ser concedido. Observe que, na implementação atual, algumas permissões não têm nenhuma distinção entre os acessos **Somente leitura** e **Leitura/gravação**. Revise os detalhes de cada permissão para entender os recursos específicos concedidos pelos acessos **Somente leitura** e **Leitura/gravação**.

Os detalhes específicos sobre cada permissão são descritos nas tabelas abaixo.

## Permissões em nível de conta

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
<tr><td align="left">    **Usuários de contas**                       </td><td align="left">  Pode exibir os usuários que foram adicionados à conta na seção **Gerenciar usuários**.          </td><td align="left">  Pode adicionar usuários à conta e fazer alterações em usuários existentes na seção **Gerenciar usuários**.             </td></tr>
<tr><td align="left">    **Relatório de desempenho de anúncios no nível da conta** </td><td align="left">  Pode exibir o [Relatório de desempenho de anúncios](advertising-performance-report.md#account-level-advertising-performance-report) no nível da conta. (Não pode exibir relatórios de desempenho de anúncios para produtos individuais, a menos que essa permissão seja concedida separadamente.)       </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Campanhas publicitárias**                        </td><td align="left">  Pode exibir [campanhas publicitárias](create-an-ad-campaign-for-your-app.md) criadas na conta.      </td><td align="left">  Pode criar, gerenciar e exibir [campanhas publicitárias](create-an-ad-campaign-for-your-app.md) criadas na conta.          </td></tr>
<tr><td align="left">    **Controle de anúncios**                        </td><td align="left">  Pode exibir [configurações de controle de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) para todos os produtos na conta.    </td><td align="left">  Pode exibir e alterar [configurações de controle de anúncios](https://msdn.microsoft.com/library/windows/apps/xaml/mt149935.aspx) para todos os produtos na conta.        </td></tr>
<tr><td align="left">    **Relatórios de controle de anúncios**                </td><td align="left">  Pode exibir o [Relatório de controle de anúncios](ad-mediation-report.md) para todos os produtos na conta.    </td><td align="left">  N/D    </td></tr>
<tr><td align="left">    **Relatórios de desempenho de anúncios**              </td><td align="left">  Pode exibir [Relatórios de desempenho de anúncios](advertising-performance-report.md) para todos os produtos na conta. (Não é possível exibir o [Relatório de desempenho de anúncios](advertising-performance-report.md#account-level-advertising-performance-report) em nível de conta, a menos que essa permissão seja concedida separadamente.)       </td><td align="left">  Pode exibir [Relatórios de desempenho de anúncios](advertising-performance-report.md) para todos os produtos na conta. (Não é possível exibir o [Relatório de desempenho de anúncios](advertising-performance-report.md#account-level-advertising-performance-report) em nível de conta, a menos que essa permissão seja concedida separadamente.)         </td></tr>
<tr><td align="left">    **Unidades de anúncio**                            </td><td align="left">  Pode exibir as [unidades de anúncio](monetize-with-ads.md) que foram criadas para a conta.    </td><td align="left">  Pode criar, gerenciar e exibir [unidades de anúncio](monetize-with-ads.md) para a conta.             </td></tr>
<tr><td align="left">    **Anúncios de afiliadas**                       </td><td align="left">  Pode exibir o uso de [anúncios de afiliadas](about-affiliate-ads.md) em todos os produtos na conta.    </td><td align="left">  Pode gerenciar e exibir o uso de [anúncios de afiliadas](about-affiliate-ads.md) para todos os produtos na conta.                </td></tr>
<tr><td align="left">    **Relatórios de desempenho de afiliadas**      </td><td align="left">  Pode exibir o [Relatório de desempenho de afiliadas](affiliates-performance-report.md) para todos os produtos na conta.   </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Relatórios de anúncios de instalação de aplicativo**             </td><td align="left">  Pode exibir o [Relatório de anúncios de instalação de aplicativo](app-install-ads-reports.md) para todos os produtos na conta.           </td><td align="left">  N/D   </td></tr>
<tr><td align="left">    **Anúncios de comunidade**                       </td><td align="left">  Pode exibir grátis o uso de [anúncio de comunidade](about-community-ads.md) para todos os produtos na conta.          </td><td align="left">  Pode criar, gerenciar e exibir grátis o uso de [anúncio de comunidade](about-community-ads.md) para todos os produtos na conta.               </td></tr>
<tr><td align="left">    **Informações de contato**                        </td><td align="left">  Pode exibir [informações de contato](managing-your-profile.md) na seção Configurações da conta.        </td><td align="left">  Pode editar e exibir [informações de contato](managing-your-profile.md) na seção Configurações da conta.            </td></tr>
<tr><td align="left">    **Conformidade com o COPPA**                    </td><td align="left">  Pode exibir as seleções de [conformidade com o COPPA](monetize-with-ads.md#coppa-compliance) (indicando se os produtos são destinados a crianças com menos de 13 anos) para todos os produtos na conta.                                            </td><td align="left">  Pode editar e exibir as seleções de [conformidade com o COPPA](monetize-with-ads.md#coppa-compliance) (indicando se os produtos são destinados a crianças com menos de 13 anos) para todos os produtos na conta.         </td></tr>
<tr><td align="left">    **Grupos de clientes**                     </td><td align="left">  Pode exibir [grupos de clientes](create-customer-groups.md) (segmentos e grupos de versão de pré-lançamento) na seção **Clientes**.      </td><td align="left">  Pode criar, editar e exibir [grupos de clientes](create-customer-groups.md) (segmentos e grupos de versão de pré-lançamento) na seção **Clientes**.       </td></tr>
<tr><td align="left">    **Novos aplicativos**                            </td><td align="left">  Pode exibir a nova página de criação de aplicativo, mas, na verdade, não é possível criar novos aplicativos na conta.    </td><td align="left">  Pode [criar novos aplicativos](create-your-app-by-reserving-a-name.md) na conta reservando novos nomes de aplicativo, e pode criar envios e enviar aplicativos para a Loja.     </td></tr>
<tr><td align="left">    **Novos pacotes**&nbsp;*                       </td><td align="left">  Pode exibir a nova página de criação de pacotes, mas, na verdade, não é possível criar novos pacotes na conta.     </td><td align="left">  Pode criar novos pacotes de produtos.          </td></tr>
<tr><td align="left">    **Serviços para parceiros**&nbsp;*                  </td><td align="left">  Pode exibir certificados de instalação para serviços para recuperar XTokens.     </td><td align="left">  Pode gerenciar e exibir certificados de instalação para serviços para recuperar XTokens.       </td></tr>
<tr><td align="left">    **Conta de pagamento**                      </td><td align="left">  Pode exibir [as informações de conta de pagamento](setting-up-your-payout-account-and-tax-forms.md#payout-account) em **Configurações da conta**.     </td><td align="left">  Pode editar e exibir [as informações de conta de pagamento](setting-up-your-payout-account-and-tax-forms.md#payout-account) em **Configurações da conta**.       </td></tr>
<tr><td align="left">    **Resumo de pagamentos**                      </td><td align="left">  Pode exibir o [Resumo de pagamentos](payout-summary.md) para acessar e baixar o relatório de informações de pagamento.       </td><td align="left">  Pode exibir o [Resumo de pagamentos](payout-summary.md) para acessar e baixar o relatório de informações de pagamento.   </td></tr>
<tr><td align="left">    **Partes confiáveis**&nbsp;*                   </td><td align="left">  Pode exibir partes confiáveis para recuperar XTokens.    </td><td align="left">  Pode gerenciar e exibir partes confiáveis para recuperar XTokens.     </td></tr>
<tr><td align="left">    **Áreas de Segurança**&nbsp;*                         </td><td align="left">  Pode acessar a página **Áreas de Segurança** e visualizar as áreas restritas na conta e quaisquer configurações aplicáveis a essas áreas de segurança. Não pode exibir os produtos e envios de cada área restrita, a menos que as permissões apropriadas em nível de produto sejam concedidas. </td><td align="left">  Pode acessar a página **Áreas de Segurança** e visualizar e gerenciar as áreas restritas na conta, incluindo a criação e a exclusão de áreas de segurança e o gerenciamento de suas configurações. Não pode exibir os produtos e envios de cada área restrita, a menos que as permissões apropriadas em nível de produto sejam concedidas.    </td></tr>
<tr><td align="left">    **Perfil de imposto**                         </td><td align="left">  Pode exibir [informações do perfil fiscal e formulários fiscais](setting-up-your-payout-account-and-tax-forms.md#tax-forms) em **Configurações da conta**.     </td><td align="left">  Pode preencher os formulários fiscais e atualizar [informações do perfil fiscal](setting-up-your-payout-account-and-tax-forms.md#tax-forms) em **Configurações da conta**.     </td></tr>
<tr><td align="left">    **Contas teste**&nbsp;*                     </td><td align="left">  Pode exibir contas para testar a configuração do Xbox Live.      </td><td align="left">  Pode criar, gerenciar e exibir contas para testar a configuração do Xbox Live.      </td></tr>
<tr><td align="left">    **Dispositivos Xbox**                        </td><td align="left">  Pode exibir os consoles de desenvolvimento do Xbox habilitados para a conta na seção **Configurações da conta**.       </td><td align="left">  Pode adicionar, remover e exibir os consoles de desenvolvimento do Xbox habilitados para a conta na seção **Configurações da conta**.     </td></tr>
    </tbody>
    </table>

\ * Permissões marcadas com um asterisco (*) concedem acesso a recursos que não estão disponíveis para todas as contas. Se sua conta não tiver sido habilitada para esses recursos, suas seleções para essas permissões não terão nenhum efeito.   

## Permissões em nível de produto

As permissões nesta seção podem ser concedidas a todos os produtos na conta, ou podem ser personalizadas para possibilitar a permissão somente para um ou mais produtos específicos. Essas permissões são agrupadas em quatro categorias: **Análises**, **Monetização**, **Publicação** e **Xbox Live**. Você pode expandir cada uma dessas categorias para exibir as permissões individuais em cada categoria. 

Para conceder uma permissão para todos os produtos na conta, faça suas seleções para essa permissão (alternando a caixa para indicar **Somente leitura**, **Leitura/gravação** ou **Sem acesso**) na linha marcada **Todos os produtos**. 
 
> **Dica** As seleções feitas para **Todos os produtos** serão aplicadas a todos os produtos atualmente na conta, bem como a quaisquer produtos futuros criados na conta.

Abaixo da linha **Todos os produtos**, você verá cada produto na conta listado em uma linha separada. Para conceder uma permissão a apenas um produto específico, faça suas seleções para essa permissão na linha para esse produto.

Cada complemento está listado em uma linha separada abaixo de seu produto pai, com uma linha **Todos os complementos**. As seleções feitas para **Todos os complementos** serão aplicadas a todos os complementos atuais para esse produto, bem como a quaisquer complementos futuros criados para esse produto.

Observe que algumas permissões não podem ser definidas para complementos. Isso ocorre porque elas não se aplicam aos complementos (por exemplo, a permissão **Comentários dos clientes**) ou porque a permissão concedida no nível do produto pai se aplica a todos os complementos para aquele produto (por exemplo, **Códigos promocionais**). Observe, porém, que nenhuma permissão disponível para complementos deve ser definida separadamente; os complementos não herdam as seleções feitas para o produto pai. Por exemplo, se você deseja permitir que um usuário faça seleções de preço e disponibilidade de um complemento, você precisa habilitar a permissão **Preço e disponibilidade** para o complemento (ou para **Todos os complementos**), se você tiver concedido ou não a permissão **Preço e disponibilidade** para o produto pai. 

### Análises

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
    <tr><td align="left">    **Uso** </td><td>    Pode exibir o [Relatório de uso](usage-report.md) do produto.     </td><td>    N/D       </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Health** </td><td>    Pode exibir o [Relatório de integridade](health-report.md) do produto.    </td><td>    N/D     </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Comentários do cliente**    </td><td>    Pode exibir os relatórios [Classificações](ratings-report.md), [Críticas](reviews-report.md) e [Comentários](feedback-report.md) do produto.       </td><td>    N/D (para responder a comentários ou críticas, a permissão **Contato com cliente** deve ser concedida)   </td><td>    N/D     </td><td>    N/D         </td></tr>
    <tr><td align="left">    **Análise do Xbox** </td><td>    Pode exibir o relatório de análise do Xbox para o produto. (Observação: este relatório ainda não está disponível.)    </td><td>    N/D   </td><td>    N/D       </td><td>    N/D          </td></tr>
    <tr><td align="left">    **Tempo real**   </td><td>    Pode exibir o relatório em tempo real para o produto.       </td><td>    N/D   </td><td>    N/D     </td><td>    N/D                 </td></tr>
    </tbody>
    </table>

### Monetização

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
    <tr><td align="left">    **Contato com cliente**  </td><td>    Pode exibir [respostas aos comentários dos clientes](respond-to-customer-feedback.md) e [respostas às críticas dos clientes](respond-to-customer-reviews.md), desde que a permissão de **Comentários dos clientes** também tenha sido concedida. Também pode exibir [notificações direcionadas](send-push-notifications-to-your-apps-customers.md) que foram criadas para o produto.    </td><td>    Pode [responder aos comentários dos clientes](respond-to-customer-feedback.md), [responder às críticas dos clientes](respond-to-customer-reviews.md), desde que a permissão de **Comentários dos clientes** também tenha sido concedida. Também pode [criar e enviar notificações direcionadas](send-push-notifications-to-your-apps-customers.md) para o produto.                   </td><td>    N/D         </td><td>    N/D                          </td></tr>
    <tr><td align="left">    **Experimentação**</td><td>    Pode exibir [experimentos (testes A/B)](../monetize/run-app-experiments-with-a-b-testing.md) e exibir dados de experimentação do produto.   </td><td>    Pode criar, gerenciar e exibir [experimentos (testes A/B)](../monetize/run-app-experiments-with-a-b-testing.md) para o produto, e exibir os dados de experimentação.     </td><td>    N/D  </td><td>    N/D                 </td></tr>
    <tr><td align="left">    **Códigos promocionais**     </td><td>    Pode exibir pedidos de [código promocional](generate-promotional-codes.md) e informações de uso para o produto e seus complementos, e pode exibir informações de uso.         </td><td>    Pode exibir, gerenciar e criar pedidos de [código promocional](generate-promotional-codes.md) para o produto e seus complementos, e pode exibir informações de uso.          </td><td>    N/D (configurações do produto pai se aplicam a todos os complementos)     </td><td>    N/D (configurações do produto pai se aplicam a todos os complementos)     </td></tr>
    </tbody>
    </table>

### Publicação 

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
    <tr><td align="left">    **Listagens da Loja**  </td><td>    Pode exibir a(s) [página(s) Listagem da Loja](create-app-store-listings.md) para envios de produto.  </td><td>    Pode exibir e editar a(s) [página(s) Listagem da Loja](create-app-store-listings.md) para envios de produto, e pode adicionar novas listagens da Loja para vários idiomas.     </td><td>    Pode exibir a(s) [página(s) Listagem da Loja](create-add-on-store-listings.md) para envios de complemento.            </td><td>    Pode exibir e editar a(s) [página(s) Listagem da Loja](create-add-on-store-listings.md) para envios de complemento, e pode adicionar novas listagens da Loja para vários idiomas.                 </td></tr>
    <tr><td align="left">    **Envio à Loja**     </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.           </td><td>    Pode enviar o produto para a Loja e exibir relatórios de certificação. Inclui envios novos e atualizados. </td><td>Não é concedido acesso se essa permissão for definida como somente leitura.     </td><td>    Pode enviar o complemento para a Loja e exibir relatórios de certificação. Inclui envios novos e atualizados.</td></tr>
    <tr><td align="left">    **Criação de envio novo**       </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.        </td><td>    Pode criar novos [envios](app-submissions.md) para o produto.  </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura.   </td><td>    Pode criar novos [envios](add-on-submissions.md) para o complemento.        </td></tr>
    <tr><td align="left">    **Novos complementos**    </td><td>    Não é concedido acesso se essa permissão for definida como somente leitura. </td><td>    Pode [criar novos complementos](set-your-add-on-product-id.md) para o produto. </td><td>    N/D    </td><td>    N/D        </td></tr>
    <tr><td align="left">    **Reservas de nome**   </td><td>    Pode exibir a página [Gerenciar nomes de aplicativo](manage-app-names.md) do produto.</td><td>    Pode exibir e editar a página [Gerenciar nomes de aplicativo](manage-app-names.md) do produto, incluindo reservar nomes adicionais e excluir nomes reservados. </td><td>   * Pode exibir nomes reservados para o complemento.    </td><td>   * Pode exibir e editar nomes reservados para o complemento.          </td></tr>
    </tbody>
    </table>

### Xbox Live \*

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
    <tr><td align="left">    **Configuração do serviço Xbox**&nbsp;\*    </td><td>    Pode exibir configurações relacionadas a conquistas, vários jogadores, placares de líderes e outras configurações do Xbox Live para o produto.  </td><td>    Pode exibir e editar configurações relacionadas a conquistas, vários jogadores, placares de líderes e outras configurações do Xbox Live para o produto.  </td><td>    N/D     </td><td>    N/D                      </td></tr>
    <tr><td align="left">    **Canais de aplicativos**&nbsp;\*</td><td>    N/D  </td><td>    Pode publicar canais de vídeo promocionais para o console do Xbox para exibição por meio do OneGuide.  </td><td>  N/D </td><td> N/D </td></tr>
</tbody>
</table>

\ * Permissões marcadas com um asterisco (*) concedem acesso a recursos que não estão disponíveis para todas as contas. Se sua conta não tiver sido habilitada para esses recursos, suas seleções para essas permissões não terão nenhum efeito.  



<!--HONumber=Nov16_HO1-->


