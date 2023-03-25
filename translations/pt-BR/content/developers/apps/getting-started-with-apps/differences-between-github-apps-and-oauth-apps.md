---
title: Diferenças entre os aplicativos GitHub e OAuth
intro: 'Entender as diferenças entre {% data variables.product.prodname_github_app %}s e {% data variables.product.prodname_oauth_app %}s ajudará você a decidir qual aplicativo você deseja criar. O {% data variables.product.prodname_oauth_app %} atua como usuário do GitHub, enquanto o {% data variables.product.prodname_github_app %} usa sua própria identidade quando instalado em uma organização ou em repositórios de uma organização.'
redirect_from:
  - /early-access/integrations/integrations-vs-oauth-applications/
  - /apps/building-integrations/setting-up-a-new-integration/about-choosing-an-integration-type/
  - /apps/differences-between-apps
  - /developers/apps/differences-between-github-apps-and-oauth-apps
versions:
  free-pro-team: '*'
  enterprise-server: '*'
  github-ae: '*'
topics:
  - GitHub Apps
  - OAuth Apps
---

### Quem pode instalar aplicativos GitHub e autorizar aplicativos OAuth?

Você pode instalar os aplicativos GitHub em sua conta pessoal ou em organizações das quais você é proprietário. Se você tiver permissões de administrador em um repositório, você poderá instalar os aplicativos GitHub em contas de organização. Se um aplicativo GitHub for instalado em um repositório e exigir permissões de organização, o proprietário da organização deverá aprovar o aplicativo.

{% data reusables.apps.app_manager_role %}

Por outro lado, os usuários _autorizam_ aplicativos OAuth, que dão ao aplicativo a capacidade de atuar como usuário autenticado. Por exemplo, você pode autorizar um aplicativo OAuth que encontra todas as notificações para o usuário autenticado. Você sempre pode revogar as permissões de um aplicativo OAuth.

{% data reusables.apps.deletes_ssh_keys %}

| Aplicativos do GitHub                                                                                                                                                                                                                                                                                               | Aplicativos OAuth                                                                                                                                                                                                                            |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Você deve ser proprietário de uma organização ou ter permissões de administrador em um repositório para instalar um aplicativo do GitHub em uma organização. Se um aplicativo GitHub for instalado em um repositório e exigir permissões de organização, o proprietário da organização deverá aprovar o aplicativo. | Você pode autorizar um aplicativo OAuth para ter acesso a recursos.                                                                                                                                                                          |
| Você pode instalar um aplicativo GitHub no seu repositório pessoal.                                                                                                                                                                                                                                                 | Você pode autorizar um aplicativo OAuth para ter acesso a recursos.                                                                                                                                                                          |
| Você deve ser um proprietário da organização, proprietário pessoal do repositório ou ter permissões de administrador em um repositório para desinstalar um aplicativo GitHub e remover seu acesso.                                                                                                                  | Você pode excluir um token de acesso do OAuth para remover o acesso.                                                                                                                                                                         |
| Você deve ser um proprietário de uma organização ou ter permissões de administrador em um repositório para solicitar a instalação de um aplicativo GitHub.                                                                                                                                                          | Se uma política de aplicativos da organização estiver ativa, qualquer integrante da organização poderá solicitar a instalação de um aplicativo OAuth em uma organização. Um proprietário da organização deve aprovar ou negar a solicitação. |

### O que o aplicativo GitHub e o aplicativo OAuth podem acessar?

Os proprietários de contas podem usar um {% data variables.product.prodname_github_app %} em uma conta sem conceder acesso a outra. Por exemplo, você pode instalar um serviço de criação de terceiros na organização do seu empregador, mas decidir não conceder esse acesso de serviço de criação aos repositórios na sua conta pessoal. Um aplicativo GitHub permanece instalado se a pessoa que o configura sair da organização.

Um aplicativo OAuth _authorized_ tem acesso a todos os recursos acessíveis do usuário ou do proprietário da organização.

| Aplicativos do GitHub                                                                                                                                                                                       | Aplicativos OAuth                                                                                                                                                                                                                                                                                                     |
| ----------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| A instalação de um aplicativo GitHub concede o acesso do aplicativo aos repositórios escolhidos por um usuário ou conta de organização.                                                                     | Autorizar um aplicativo OAuth concede acesso aos recursos acessíveis do usuário. Por exemplo, repositórios que podem acessar.                                                                                                                                                                                         |
| O token de instalação de um aplicativo GitHub perde acesso aos recursos se um administrador remover repositórios da instalação.                                                                             | Um token de acesso do OAuth perde acesso aos recursos quando o usuário perde acesso, como quando perdem acesso de gravação em um repositório.                                                                                                                                                                         |
| Os tokens de acesso de instalação são limitados a repositórios especificados com as permissões escolhidas pelo criador do aplicativo.                                                                       | Um token de acesso do OAuth é limitado por meio de escopos.                                                                                                                                                                                                                                                           |
| Os aplicativos GitHub podem solicitar acesso separado para problemas e pull requests sem acessar o conteúdo real do repositório.                                                                            | Os aplicativos OAuth precisam solicitar o escopo do `repositório` para obter acesso a problemas, pull requests ou qualquer coisa que seja propriedade do repositório.                                                                                                                                                 |
| Os aplicativos GitHub não estão sujeitos às políticas do aplicativo da organização. Um aplicativo GitHub só tem acesso aos repositórios que o proprietário de uma organização concedeu.                     | Se uma política de aplicativos da organização estiver ativa, somente um proprietário da organização poderá autorizar a instalação de um aplicativo OAuth. Se instalado, o aplicativo OAuth ganhará acesso a qualquer coisa visível para o token que o proprietário da organização tem dentro da organização aprovada. |
| Um aplicativo GitHub recebe um evento do webhook quando uma instalação é alterada ou removida. Isto informa ao criador do aplicativo quando receberam mais ou menos acesso aos recursos de uma organização. | Os aplicativos OAuth podem perder acesso a uma organização ou repositório a qualquer momento com na alteração de acesso do usuário concedido. O aplicativo OAuth não irá informá-lo quando perde acesso a um recurso.                                                                                                 |

### Identificação baseada em token

{% note %}

**Observação:** Os aplicativos GitHub também podem usar token baseado em usuários. Para obter mais informações, consulte "[Identificar e autorizar usuários para aplicativos GitHub](/apps/building-github-apps/identifying-and-authorizing-users-for-github-apps/)".

{% endnote %}

| Aplicativos do GitHub                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                       | Aplicativos OAuth                                                                                                                                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Um aplicativo GitHub pode solicitar um token de acesso de instalação usando uma chave privada com um formato de token do JSON fora da banda.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                | Um aplicativo OAuth pode trocar um token de solicitação por um token de acesso após um redirecionamento por meio de uma solicitação da web.                                                                                                                                                             |
| Um token de instalação identifica o aplicativo como o bot do aplicativo GitHub, como, por exemplo, @jenkins-bot.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                            | Um token de acesso identifica o aplicativo como o usuário que concedeu o token ao aplicativo, como, por exemplo, o @octocat.                                                                                                                                                                            |
| Os tokens de instalação expiram após um tempo predefinido (atualmente, 1 hora).                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                             | Os tokens do OAuth permanecem ativos até que sejam cancelados pelo cliente.                                                                                                                                                                                                                             |
| {% data reusables.apps.api-rate-limits-non-ghec %}{% if currentVersion == "free-pro-team@latest" %} Limites de taxa mais alto aplicam-se a {% data variables.product.prodname_ghe_cloud %}. Para obter mais informações, consulte "[Limites de taxas para os aplicativos GitHub](/developers/apps/rate-limits-for-github-apps)."{% endif %}                                                                                                                                                                                                                                                                                                                               | Os tokens do OAuth usam o limite de taxa de usuário de 5.000 solicitações por hora.                                                                                                                                                                                                                     |
| Os aumentos no limite de taxa pode ser concedido tanto no nível do aplicativo GitHub (afetando todas as instalações) quanto no nível de instalação individual.                                                                                                                                                                                                                                                                                                                                                                                                                                                                                                              | Os aumentos no limite de taxa são concedidos pelo aplicativo OAuth. Todo token concedido para que o aplicativo OAuth obtém um aumento do limite. |{% if currentVersion == "free-pro-team@latest" or currentVersion ver_gt "enterprise-server@2.21" or currentVersion == "github-ae@latest" %}
| {% data variables.product.prodname_github_app %}s podem efetuar a autenticação em nome do usuário, o que é denominado solicitações de usuário para servidor. O fluxo para autorizar é o mesmo que o fluxo de autorização do aplicativo OAuth. Os tokens de usuário para servidor podem expirar e ser renovados com um token de atualização. Para obter mais informações, consulte "[Atualizando tokens de acesso do usuário para servidor](/apps/building-github-apps/refreshing-user-to-server-access-tokens/)" e "[identificando e autorizando os usuários para os aplicativos GitHub](/apps/building-github-apps/identifying-and-authorizing-users-for-github-apps/)". | O fluxo do OAuth usado por {% data variables.product.prodname_oauth_app %}s autoriza um {% data variables.product.prodname_oauth_app %} em nome do usuário. Este é o mesmo fluxo de uso na autorização de usuário para servidor do {% data variables.product.prodname_github_app %}. 
{% endif %}

### Solicitar níveis de permissão para os recursos

Ao contrário dos aplicativos OAuth, os aplicativos GitHub têm permissões direcionadas que lhes permitem solicitar acesso apenas ao que precisam. Por exemplo, uma Integração Contínua (CI) do aplicativo GitHub pode solicitar acesso de leitura ao conteúdo do repositório e acesso de gravação à API de status. Outro aplicativo GitHub não pode ter acesso de leitura ou de gravação para o código, mas ainda assim pode gerenciar problemas, etiquetas e marcos. Os aplicativos OAuth não podem usar permissões granulares.

| Access                                                    | Aplicativos GitHub (permissões de `leitura` ou `gravação`)        | Aplicativos OAuth                             |
| --------------------------------------------------------- | ----------------------------------------------------------------- | --------------------------------------------- |
| **Para acesso a repositórios públicos**                   | O repositório público precisa ser escolhido durante a instalação. | Escopo do `public_repo`.                      |
| **Para acesso ao código/conteúdo do repositório**         | Conteúdo do repositório                                           | Escopo do `repositório`.                      |
| **Para acesso a problema, etiquetas e marcos**            | Problemas                                                         | Escopo do `repositório`.                      |
| **Para acesso a pull requests, etiquetas e marcos**       | Pull requests                                                     | Escopo do `repositório`.                      |
| **Para acesso ao status do commit (para criações de CI)** | Status do commit                                                  | Escopo do `repo:status`.                      |
| **Para acesso às implantações e status de implantação**   | Implantações                                                      | Escopo do `repo_deployment`.                  |
| **Para receber eventos por meio de um webhook**           | Um aplicativo de GitHub inclui um webhook por padrão.             | Escopo `write:repo_hook` ou `write:org_hook`. |

### Descoberta de repositório

| Aplicativos do GitHub                                                                                                       | Aplicativos OAuth                                                                                                                                                                                       |
| --------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Os aplicativos GitHub podem olhar para `/installation/repositórios` para ver os repositórios que a instalação pode acessar. | Aplicativos OAuth podem olhar para `/user/repos` para obter uma visualização do usuário ou para `/orgs/:org/repos` para obter uma visualização da organização dos repositórios repositórios acessíveis. |
| Os aplicativos GitHub recebem webhooks quando os repositórios são adicionados ou removidos da instalação.                   | Os aplicativos OAuth criam webhooks de organização para notificações quando um novo repositório é criado dentro de uma organização.                                                                     |

### Webhooks

| Aplicativos do GitHub                                                                                                                                      | Aplicativos OAuth                                                                                                                                                                          |
| ---------------------------------------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| Por padrão, os aplicativos GitHub têm um único webhook que recebe os eventos que estão configurados para receber para cada repositório ao qual têm acesso. | Os aplicativos OAuth solicitam o escopo do webhook para criar um webhook do repositório para cada repositório do qual precisam receber eventos.                                            |
| O aplicativo GitHub recebe certos eventos a nível da organização com a permissão do integrante da organização.                                             | Os aplicativos OAuth solicitam o escopo do webhook da organização para criar um webhook da organização para cada organização da qual precisam para receber eventos a nível da organização. |

### Acesso Git

| Aplicativos do GitHub                                                                                                                                                                                                                                                     | Aplicativos OAuth                                                                                                                                                                                                |
| ------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| Os aplicativos GitHub solicitam permissão de conteúdo de repositórios e usam seu token de instalação para efetuar a autenticação por meio do [Git baseado em HTTP](/apps/building-github-apps/authenticating-with-github-apps/#http-based-git-access-by-an-installation). | Os aplicativos OAuth solicitam escopo `write:public_key` e [Criar uma chave de implantação](/rest/reference/repos#create-a-deploy-key) por meio da API. Você pode usar essa chave para realizar comandos do Git. |
| O token é usado como senha HTTP.                                                                                                                                                                                                                                          | O token é usado como nome de usuário HTTP.                                                                                                                                                                       |

### Máquina vs. contas de bot

Contas de usuário de máquina são contas de usuário baseadas no OAuth que separam sistemas automatizados usando o sistema de usuário do GitHub.

As contas do bot são específicas para os aplicativos GitHub e são construídas em todos os aplicativos GitHub.

| Aplicativos do GitHub                                                                                   | Aplicativos OAuth                                                                                                         |
| ------------------------------------------------------------------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------- |
| Os bots do aplicativo GitHub não consomem uma estação {% data variables.product.prodname_enterprise %}. | Uma conta de usuário de máquina consome uma estação {% data variables.product.prodname_enterprise %}.                     |
| Como um bot do aplicativo GitHub nunca recebe uma senha, um cliente não pode entrar diretamente nele.   | Um nome de usuário e senha são concedidos a uma conta de usuário de máquina para ser gerenciada e protegida pelo cliente. |