---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-21"

keywords: kubernetes, iks

subcollection: containers

---

{:new_window: target="_blank"}
{:shortdesc: .shortdesc}
{:screen: .screen}
{:pre: .pre}
{:table: .aria-labeledby="caption"}
{:codeblock: .codeblock}
{:tip: .tip}
{:note: .note}
{:important: .important}
{:deprecated: .deprecated}
{:download: .download}
{:tsSymptoms: .tsSymptoms}
{:tsCauses: .tsCauses}
{:tsResolve: .tsResolve}



# Depurando seu cluster
{: #cs_troubleshoot}

Ao usar o {{site.data.keyword.containerlong}}, considere estas técnicas para resolução de problemas em geral e depuração de seus clusters. Também é possível verificar o [status do sistema {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/bluemix/support/#status).
{: shortdesc}

É possível seguir estas etapas gerais para assegurar que seus clusters estejam atualizados:
- Verifique mensalmente por correções de segurança e de sistema operacional disponíveis para [atualizar seus nós do trabalhador](/docs/containers?topic=containers-cs_cli_reference#cs_worker_update).
- [Atualize seu cluster](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_update) para a [versão do Kubernetes](/docs/containers?topic=containers-cs_versions) padrão mais recente do {{site.data.keyword.containerlong_notm}}

## Executando testes com o {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool
{: #debug_utility}

Enquanto você soluciona problemas, é possível usar o {{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool para executar testes e reunir informações pertinentes de seu cluster. Para usar a ferramenta de depuração, instale o [gráfico do Helm `ibmcloud-iks-debug` ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/containers-kubernetes/solutions/helm-charts/ibm/ibmcloud-iks-debug):
{: shortdesc}


1. [Configure o Helm em seu cluster, crie uma conta do serviço para o Tiller e inclua o repositório `ibm` em sua instância do Helm](/docs/containers?topic=containers-integrations#helm).

2. Instale o gráfico Helm em seu cluster.
  ```
  helm install ibm/ibmcloud-iks-debug -- name debug-tool
  ```
  {: pre}


3. Inicie um servidor proxy para exibir a interface da ferramenta de depuração.
  ```
  kubectl proxy -- port 8080
  ```
  {: pre}

4. Em um navegador da web, abra a URL da interface de ferramenta de depuração: http://localhost:8080/api/v1/namespaces/default/services/debug-tool-ibmcloud-iks-debug:8822/proxy/page

5. Selecione testes individuais ou um grupo de testes a serem executados. Alguns testes verificam potenciais avisos, erros ou problemas e alguns testes somente reúnem informações que podem ser referenciadas durante a resolução de problemas. Para obter mais informações sobre a função de cada teste, clique no ícone de informações próximo ao nome do teste.

6. Clique em  ** Executar **.

7. Verifique os resultados de cada teste.
  * Se algum teste falhar, clique no ícone de informações ao lado do nome do teste na coluna à esquerda para obter informações sobre como resolver o problema.
  * Também é possível usar os resultados de testes para reunir informações, como os YAMLs completos, que podem ajudar a depurar seu cluster nas seções a seguir.

## Depurando clusters
{: #debug_clusters}

Revise as opções para depurar seus clusters e localizar as causas raízes das falhas.

1.  Liste o cluster e localize o `Estado` dele.

  ```
  ibmcloud ks clusters
  ```
  {: pre}

2.  Revise o `Estado` do cluster. Se o seu cluster está em um estado **Crítico**, **Exclusão com falha** ou **Aviso** ou está preso no estado **Pendente** por muito tempo, inicie a [depuração dos nós do trabalhador](#debug_worker_nodes).

    <table summary="Cada linha da tabela deve ser lida da esquerda para a direita, com o estado do cluster na coluna um e uma descrição na coluna dois.">
<caption>Estados do cluster</caption>
   <thead>
   <th>Estado do cluster</th>
   <th>Descrição</th>
   </thead>
   <tbody>
<tr>
   <td>Interrompido</td>
   <td>A exclusão do cluster é solicitada pelo usuário antes da implementação do mestre do Kubernetes. Depois que a exclusão do cluster é concluída, o cluster é removido do painel. Se o seu cluster estiver preso nesse estado por um longo tempo, abra um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).</td>
   </tr>
 <tr>
     <td>Crítico</td>
     <td>O mestre do Kubernetes não pode ser atingido ou todos os nós do trabalhador no cluster estão inativos. </td>
    </tr>
   <tr>
     <td>Exclusão com falha</td>
     <td>O mestre do Kubernetes ou pelo menos um nó do trabalhador não pode ser excluído.  </td>
   </tr>
   <tr>
     <td>Excluído</td>
     <td>O cluster foi excluído, mas ainda não foi removido de seu painel. Se o seu cluster estiver preso nesse estado por um longo tempo, abra um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). </td>
   </tr>
   <tr>
   <td>Exclusão</td>
   <td>O cluster está sendo excluído e a infraestrutura de cluster está sendo desmantelada. Não é possível acessar o cluster.  </td>
   </tr>
   <tr>
     <td>Implementação com falha</td>
     <td>A implementação do mestre do Kubernetes não pôde ser concluída. Não é possível resolver esse estado. Entre em contato com o suporte do IBM Cloud abrindo um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help).</td>
   </tr>
     <tr>
       <td>Implementando</td>
       <td>O mestre do Kubernetes não está totalmente implementado ainda. Não é possível acessar seu cluster. Aguarde até que seu cluster seja totalmente implementado para revisar seu funcionamento.</td>
      </tr>
      <tr>
       <td>Normal</td>
       <td>Todos os nós do trabalhador em um cluster estão funcionando. É possível acessar o cluster e implementar apps no cluster. Esse estado é considerado funcional e não requer uma ação sua.<p class="note">Embora os nós do trabalhador possam ser normais, outros recursos de infraestrutura, como a [rede](/docs/containers?topic=containers-cs_troubleshoot_network) e o [armazenamento](/docs/containers?topic=containers-cs_troubleshoot_storage), ainda podem precisar de atenção.</p></td>
    </tr>
      <tr>
       <td>Pendente</td>
       <td>O mestre do Kubernetes foi implementado. Os nós do trabalhador estão sendo provisionados e ainda não estão disponíveis no cluster. É possível acessar o cluster, mas não é possível implementar apps no cluster.  </td>
     </tr>
   <tr>
     <td>Solicitado</td>
     <td>Uma solicitação para criar o cluster e pedir a infraestrutura para os nós principal e do trabalhador do Kubernetes é enviada. Quando a implementação do cluster é iniciada, o estado do cluster muda para <code>Deploying</code>. Se o seu cluster estiver preso no estado <code>Requested</code> por um longo tempo, abra um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](/docs/containers?topic=containers-cs_troubleshoot#ts_getting_help). </td>
   </tr>
   <tr>
     <td>Atualizando</td>
     <td>O servidor da API do Kubernetes executado no mestre do Kubernetes está sendo atualizado para uma nova versão de API do Kubernetes. Durante a atualização, não é possível acessar nem mudar o cluster. Nós do trabalhador, apps e recursos implementados pelo usuário não são modificados e continuarão a ser executados. Aguarde a atualização ser concluída para revisar o funcionamento de seu cluster. </td>
   </tr>
    <tr>
       <td>Avisar</td>
       <td>Pelo menos um nó do trabalhador no cluster não está disponível, mas outros nós do trabalhador estão disponíveis e podem assumir o controle da carga de trabalho. </td>
    </tr>
   </tbody>
 </table>


O [Mestre do Kubernetes](/docs/containers?topic=containers-ibm-cloud-kubernetes-service-technology#architecture) é o componente principal que mantém seu cluster funcionando. O mestre armazena recursos de cluster e suas configurações no banco de dados etcd que serve como o ponto único de verdade para seu cluster. O servidor da API do Kubernetes é o ponto de entrada principal para todas as solicitações de gerenciamento de cluster dos nós do trabalhador para o principal ou quando você deseja interagir com os recursos de cluster.<br><br>Se ocorrer uma falha do mestre, suas cargas de trabalho continuarão a ser executadas nos nós do trabalhador, mas não será possível usar os comandos `kubectl` para trabalhar com seus recursos de cluster ou visualizar o funcionamento do cluster até que o servidor da API do Kubernetes no mestre esteja novamente ativo. Se um pod ficar inativo durante a indisponibilidade do mestre, o pod não poderá ser reprogramado até que o nó do trabalhador possa atingir o servidor da API do Kubernetes novamente.<br><br>Durante uma indisponibilidade do mestre, ainda é possível executar os comandos `ibmcloud ks` com relação à API do {{site.data.keyword.containerlong_notm}} para trabalhar com seus recursos de infraestrutura, como nós do trabalhador ou VLANs. Se você mudar a configuração de cluster atual incluindo ou removendo nós do trabalhador no cluster, suas mudanças não ocorrerão até que o mestre esteja ativo novamente.

Não reinicie ou reinicialize um nó do trabalhador durante uma indisponibilidade do mestre. Essa ação remove os pods de seu nó do trabalhador. Como o servidor da API do Kubernetes está indisponível, os pods não podem ser reprogramados em outros nós do trabalhador no cluster.
{: important}


<br />


## Depurando nós do trabalhador
{: #debug_worker_nodes}

Revise as opções para depurar seus nós do trabalhador e localizar as causas raízes das falhas.


1.  Se o seu cluster está em um estado **Crítico**, **Exclusão com falha** ou **Aviso** ou está preso no estado **Pendente** por muito tempo, revise o estado de seus nós do trabalhador.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

2.  Revise os campos `State` e `Status` para cada nó do trabalhador em sua saída da CLI.

  <table summary="Cada linha da tabela deve ser lida da esquerda para a direita, com o estado do cluster na coluna um e uma descrição na coluna dois.">
  <caption>Estados do nó do trabalhador</caption>
    <thead>
    <th>Estado do nó do trabalhador</th>
    <th>Descrição</th>
    </thead>
    <tbody>
  <tr>
      <td>Crítico</td>
      <td>Um nó do trabalhador pode entrar em um estado Crítico por muitas razões: <ul><li>Você iniciou uma reinicialização para seu nó do trabalhador sem bloquear e drenar seu nó do trabalhador. Reinicializar um nó do trabalhador pode causar distorção de dados em <code>containerd</code>, <code>kubelet</code>, <code>kube-proxy</code> e <code>calico</code>. </li>
      <li>Os pods que são implementados em seu nó do trabalhador não usam os limites de recurso para [memória ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-memory-resource/) e [CPU ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://kubernetes.io/docs/tasks/configure-pod-container/assign-cpu-resource/). Sem limites de recurso, os pods podem consumir todos os recursos disponíveis, sem deixar recursos para outros pods executarem nesse nó do trabalhador. Esse supercomprometimento de carga de trabalho faz com que o nó do trabalhador falhe. </li>
      <li>O <code>containerd</code>, <code>kubelet</code> ou <code>calico</code> entrou em um estado irrecuperável depois que ele executou centenas ou milhares de contêineres ao longo do tempo. </li>
      <li>Você configurou um Virtual Router Appliance para seu nó do trabalhador que ficou inativo e cortou a comunicação entre o seu nó do trabalhador e o mestre do Kubernetes. </li><li> Problemas da rede atual no {{site.data.keyword.containerlong_notm}} ou na infraestrutura do IBM Cloud (SoftLayer) que fazem com que a comunicação entre o nó do trabalhador e o mestre do Kubernetes falhe.</li>
      <li>O nó do trabalhador ficou sem capacidade. Verifique o <strong>Status</strong> do nó do trabalhador para ver se ele mostra <strong>Sem disco</strong> ou <strong>Sem memória</strong>. Se o nó do trabalhador está fora de capacidade, considere reduzir a carga de trabalho em seu nó do trabalhador ou incluir um nó do trabalhador em seu cluster para ajudar no balanceamento de carga da carga de trabalho.</li>
      <li>O dispositivo foi desligado por meio da lista de recursos do console do [{{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/resources). Abra a lista de recursos e localize o ID do nó do trabalhador na lista **Dispositivos**. No menu Ação, clique em **Ligar**.</li></ul>
      Em muitos casos, [recarregar](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) seu nó do trabalhador pode resolver o problema. Ao
recarregar o nó do trabalhador, a [versão de correção](/docs/containers?topic=containers-cs_versions#version_types) mais recente é aplicada ao nó do trabalhador. As versões principal e secundária não mudam. Antes de recarregar seu nó do trabalhador, certifique-se de bloquear e drenar o seu nó do trabalhador para assegurar que os pods existentes sejam finalizados normalmente e reprogramados em outros nós do trabalhador. </br></br> Se o recarregamento do nó do trabalhador não resolver o problema, acesse a próxima etapa para continuar a resolução de problemas de seu nó do trabalhador. </br></br><strong>Dica:</strong> é possível [configurar verificações de funcionamento para seu nó do trabalhador e ativar a Recuperação automática](/docs/containers?topic=containers-health#autorecovery). Se a Recuperação automática detecta um nó do trabalhador não funcional com base nas verificações configuradas, a Recuperação automática aciona uma ação corretiva como um recarregamento do S.O. no nó do trabalhador. Para obter mais informações sobre como a Recuperação automática funciona, veja o [blog de Recuperação automática ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://www.ibm.com/blogs/bluemix/2017/12/autorecovery-utilizes-consistent-hashing-high-availability/).
      </td>
     </tr>
     <tr>
     <td>Implementado</td>
     <td>As atualizações são implementadas com êxito em seu nó do trabalhador. Após as atualizações serem implementadas, o {{site.data.keyword.containerlong_notm}} inicia uma verificação de funcionamento no nó do trabalhador. Depois que a verificação de funcionamento é bem-sucedida, o nó do trabalhador entra em um estado <code>Normal</code>. Os nós do trabalhador em um estado <code>Deployed</code> geralmente estão prontos para receber cargas de trabalho, que é possível verificar executando <code>kubectl get nodes</code> e confirmando se o estado mostra <code>Normal</code>. </td>
     </tr>
      <tr>
        <td>Implementando</td>
        <td>Quando você atualiza a versão do Kubernetes do nó do trabalhador, seu nó do trabalhador é reimplementado para instalar as atualizações. Se você recarregar ou reinicializar o nó do trabalhador, o nó do trabalhador será reimplementado para instalar automaticamente a versão de correção mais recente. Se o nó do trabalhador está preso nesse estado por muito tempo, continue com a próxima etapa para ver se um problema ocorreu durante a implementação. </td>
     </tr>
        <tr>
        <td>Normal</td>
        <td>O nó do trabalhador está totalmente provisionado e pronto para ser usado no cluster. Esse estado é considerado funcional e não requer uma ação do usuário. **Nota**: embora os nós do trabalhador possam ser normais, outros recursos de infraestrutura, como [redes](/docs/containers?topic=containers-cs_troubleshoot_network) e [armazenamento](/docs/containers?topic=containers-cs_troubleshoot_storage), ainda podem precisar de atenção.</td>
     </tr>
   <tr>
        <td>Provisionando</td>
        <td>O nó do trabalhador está sendo provisionado e ainda não está disponível no cluster. É possível monitorar o processo de fornecimento na coluna <strong>Status</strong> da saída da CLI. Se o nó do trabalhador está preso nesse estado por muito tempo, continue com a próxima etapa para ver se um problema ocorreu durante o fornecimento.</td>
      </tr>
      <tr>
        <td>Provision_failed</td>
        <td>O nó do trabalhador não pôde ser provisionado. Continue com a próxima etapa para localizar os detalhes da falha.</td>
      </tr>
   <tr>
        <td>Recarregando</td>
        <td>O nó do trabalhador está sendo recarregado e não está disponível no cluster. É possível monitorar o processo de recarregamento na coluna <strong>Status</strong> da saída da CLI. Se o nó do trabalhador está preso nesse estado por muito tempo, continue com a próxima etapa para ver se um problema ocorreu durante o recarregamento.</td>
       </tr>
       <tr>
        <td>Reloading_failed </td>
        <td>O nó do trabalhador não pôde ser recarregado. Continue com a próxima etapa para localizar os detalhes da falha.</td>
      </tr>
      <tr>
        <td>Reload_pending </td>
        <td>Uma solicitação para recarregar ou atualizar a versão do Kubernetes do nó do trabalhador é enviada. Quando o nó do trabalhador está sendo recarregado, o estado muda para <code>Reloading</code>. </td>
      </tr>
      <tr>
       <td>Desconhecido</td>
       <td>O mestre do Kubernetes não está acessível por um dos motivos a seguir:<ul><li>Você solicitou uma atualização do mestre do Kubernetes. O estado do nó do trabalhador não pode ser recuperado durante a atualização. Se o nó do trabalhador permanecer nesse estado por um período de tempo estendido mesmo depois que o mestre do Kubernetes for atualizado com sucesso, tente [recarregar](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload) o nó do trabalhador.</li><li>Você pode ter outro firewall que está protegendo seus nós do trabalhador ou mudado as configurações de firewall recentemente. O {{site.data.keyword.containerlong_notm}} requer que determinados endereços IP e portas sejam abertos para permitir a comunicação do nó do trabalhador com o mestre do Kubernetes e vice-versa. Para obter mais informações, consulte [Firewall impede que os nós trabalhadores se conectem](/docs/containers?topic=containers-cs_troubleshoot_clusters#cs_firewall).</li><li>O mestre do Kubernetes está inativo. Entre em contato com o suporte do {{site.data.keyword.Bluemix_notm}} abrindo um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](#ts_getting_help).</li></ul></td>
  </tr>
     <tr>
        <td>Avisar</td>
        <td>O nó do trabalhador está atingindo o limite para memória ou espaço em disco. Será possível reduzir a carga de trabalho em seu nó do trabalhador ou incluir um nó do trabalhador em seu cluster para ajudar no balanceamento de carga da carga de trabalho.</td>
  </tr>
    </tbody>
  </table>

5.  Liste os detalhes para o nó do trabalhador. Se os detalhes incluem uma mensagem de erro, revise a lista de [mensagens de erro comum para nós do trabalhador](#common_worker_nodes_issues) para saber como resolver o problema.
  ```
  ibmcloud ks worker-get --cluster <cluster_name_or_id> --worker <worker_node_id>
  ```
  {: pre}

<br />


## Problemas comuns com nós do trabalhador
{: #common_worker_nodes_issues}

Revise as mensagens de erro comuns e saiba como resolvê-las.

  <table>
  <caption>Mensagens de erro comuns</caption>
    <thead>
    <th>A mensagem de erro</th>
    <th>Descrição e resolução
    </thead>
    <tbody>
      <tr>
        <td>Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}: sua conta está atualmente proibida de pedir 'Instâncias de cálculo'.</td>
        <td>Sua conta de infraestrutura do IBM Cloud (SoftLayer) pode ser restringida de pedir recursos de cálculo. Entre em contato com o suporte do {{site.data.keyword.Bluemix_notm}} abrindo um [caso de suporte do {{site.data.keyword.Bluemix_notm}}](#ts_getting_help).</td>
      </tr>
      <tr>
      <td>{{site.data.keyword.Bluemix_notm}}  Exceção de infraestrutura: não foi possível colocar a ordem.<br><br>
      Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}: não foi possível fazer a ordem. Há recursos insuficientes atrás do roteador 'router_name' para preencher a solicitação para os convidados a seguir: 'worker_id'.</td>
      <td>A zona que você selecionou pode não ter capacidade de infraestrutura suficiente para provisionar os seus nós do trabalhador. Ou, você pode ter excedido um limite em sua conta de infraestrutura do IBM Cloud (SoftLayer). Para resolver, tente uma das opções a seguir:
      <ul><li>A disponibilidade do recurso de infraestrutura em zonas pode flutuar frequentemente. Espere alguns minutos e tente novamente.</li>
      <li>Para um cluster de zona única, crie o cluster em uma zona diferente. Para um cluster de múltiplas zonas, inclua uma zona no cluster.</li>
      <li>Especifique um par diferente de VLANs públicas e privadas para os nós do trabalhador em sua conta da infraestrutura do IBM Cloud (SoftLayer). Para nós do trabalhador que estão em um conjunto de trabalhadores, é possível usar o [comando](/docs/containers?topic=containers-cs_cli_reference#cs_zone_network_set) <code>ibmcloud ks zone-network-set</code>.</li>
      <li>Entre em contato com o gerenciador de conta da infraestrutura do IBM Cloud (SoftLayer) para verificar se você não excede um limite de conta, como uma cota global.</li>
      <li>Abra um [caso de suporte do IBM Cloud infrastructure (SoftLayer)](#ts_getting_help)</li></ul></td>
      </tr>
      <tr>
        <td>Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}: não foi possível obter a VLAN de rede com o ID: <code>&lt;vlan id&gt;</code>.</td>
        <td>O nó do trabalhador não pôde ser provisionado porque o ID de VLAN selecionado não pôde ser localizado por um dos motivos a seguir:<ul><li>Talvez você tenha especificado o número da VLAN, em vez do ID da VLAN. O número da VLAN tem 3 ou 4 dígitos de comprimento, enquanto o ID da VLAN tem 7 dígitos. Execute <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> para recuperar o ID da VLAN.<li>O ID da VLAN pode não estar associado à conta de infraestrutura do IBM Cloud (SoftLayer) que você usa. Execute <code>ibmcloud ks vlans --zone &lt;zone&gt;</code> para listar IDs de VLAN disponíveis para a sua conta. Para mudar a conta de infraestrutura do IBM Cloud (SoftLayer), consulte [`ibmcloud ks credential-set`](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set). </ul></td>
      </tr>
      <tr>
        <td>SoftLayer_Exception_Order_InvalidLocation: o local fornecido para essa ordem é inválido. (HTTP 500)</td>
        <td>A infraestrutura do IBM Cloud (SoftLayer) não está configurada para pedir recursos de cálculo no data center selecionado. Entre em contato com o [suporte do {{site.data.keyword.Bluemix_notm}}](#ts_getting_help) para verificar se sua conta está configurada corretamente.</td>
       </tr>
       <tr>
        <td>Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}: o usuário não tem as permissões de infraestrutura necessárias do {{site.data.keyword.Bluemix_notm}} para incluir servidores
        </br></br>
        Exceção de infraestrutura do {{site.data.keyword.Bluemix_notm}}: 'Item' deve ser pedido com permissão.
        </br></br>
        As credenciais de infraestrutura do {{site.data.keyword.Bluemix_notm}} não puderam ser validadas.</td>
        <td>Você talvez não tenha as permissões necessárias para executar a ação em seu portfólio de infraestrutura do IBM Cloud (SoftLayer) ou esteja usando as credenciais de infraestrutura erradas. Veja [Configurando a chave API para ativar o acesso ao portfólio de infraestrutura](/docs/containers?topic=containers-users#api_key).</td>
      </tr>
      <tr>
       <td>O trabalhador não consegue conversar com servidores {{site.data.keyword.containerlong_notm}}. Verifique se sua configuração de firewall está permitindo tráfego desse trabalhador.
       <td><ul><li>Se você tiver um firewall, [defina suas configurações de firewall para permitir o tráfego de saída para as portas e endereços IP](/docs/containers?topic=containers-firewall#firewall_outbound).</li>
       <li>Verifique se o seu cluster não tem um IP público executando `ibmcloud ks workers --cluster &lt;mycluster&gt;`. Se nenhum IP público está listado, então seu cluster tem somente VLANs privadas.
       <ul><li>Se você deseja que o cluster tenha somente VLANs privadas, configure sua [conexão VLAN](/docs/containers?topic=containers-plan_clusters#private_clusters) e seu [firewall](/docs/containers?topic=containers-firewall#firewall_outbound).</li>
       <li>Se você deseja que o cluster tenha um IP público, [inclua novos nós do trabalhador](/docs/containers?topic=containers-cs_cli_reference#cs_worker_add) com as VLANs públicas e privadas.</li></ul></li></ul></td>
     </tr>
      <tr>
  <td>Não é possível criar o token do portal do IMS, pois nenhuma conta do IMS está vinculada à conta selecionada do BSS</br></br>Usuário fornecido não localizado ou ativo</br></br>SoftLayer_Exception_User_Customer_InvalidUserStatus: a conta do usuário é atualmente cancel_pending.</br></br>Aguardando a máquina ficar visível para o usuário</td>
  <td>O proprietário da chave API que é usada para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer) não tem as permissões necessárias para executar a ação ou pode estar com exclusão pendente.</br></br><strong>Como o usuário</strong>, siga estas etapas:
  <ol><li>Se você tem acesso a múltiplas contas, certifique-se de que tenha efetuado login na conta em que deseja trabalhar com o {{site.data.keyword.containerlong_notm}}. </li>
  <li>Execute <code>ibmcloud ks api-key-info</code> para visualizar o proprietário da chave API atual que é usado para acessar o portfólio de infraestrutura do IBM Cloud (SoftLayer). </li>
  <li>Execute <code>ibmcloud account list</code> para visualizar o proprietário da conta do {{site.data.keyword.Bluemix_notm}} que você usa atualmente. </li>
  <li>Entre em contato com o proprietário da conta do {{site.data.keyword.Bluemix_notm}} e relate que o proprietário da chave API tem permissões insuficientes na infraestrutura do IBM Cloud (SoftLayer) ou pode estar com exclusão pendente. </li></ol>
  </br><strong>Como o proprietário da conta</strong>, siga estas etapas:
  <ol><li>Revise as [permissões necessárias na infraestrutura do IBM Cloud (SoftLayer)](/docs/containers?topic=containers-users#infra_access) para executar a ação que falhou anteriormente. </li>
  <li>Corrija as permissões do proprietário da chave API ou crie uma nova chave API usando o comando [<code>ibmcloud ks api-key-reset</code>](/docs/containers?topic=containers-cs_cli_reference#cs_api_key_reset). </li>
  <li>Se você ou outro administrador de conta configurar manualmente as credenciais de infraestrutura do IBM Cloud (SoftLayer) manualmente em sua conta, execute [<code>ibmcloud ks credential-unset</code>](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_unset) para remover as credenciais de sua conta.</li></ol></td>
  </tr>
    </tbody>
  </table>



<br />




## Depurando implementações de app
{: #debug_apps}

Revise as opções que você tiver para depurar suas implementações de app e localize as causas raiz das falhas.

Antes de iniciar, assegure-se de que você tenha a [função de serviço **Gravador** ou **Gerenciador** do {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) para o namespace no qual o seu app é implementado.

1. Procure anomalias nos recursos de serviço ou implementação executando o comando `describe`.

 Exemplo:
 <pre class="pre"><code>kubectl describe service &lt;service_name&gt; </code></pre>

2. [Verifique se os contêineres estão presos no estado `ContainerCreating` ](/docs/containers?topic=containers-cs_troubleshoot_storage#stuck_creating_state).

3. Verifique se o cluster está no estado `Critical`. Se o cluster estiver em um estado `Crítico`, verifique as regras de firewall e verifique se o mestre pode se comunicar com os nós do trabalhador.

4. Verifique se o serviço está atendendo na porta correta.
   1. Obtenha o nome de um pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Efetue login em um contêiner.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Executar curl do app de dentro do contêiner. Se a porta não estiver acessível, o serviço poderá não estar atendendo na porta correta ou o app pode ter problemas. Atualize o arquivo de configuração para o serviço com a porta correta e reimplemente ou investigue problemas em potencial com o app.
     <pre class="pre"><code>curl localhost: &lt;port&gt;</code></pre>

5. Verifique se o serviço está vinculado corretamente aos pods.
   1. Obtenha o nome de um pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Efetue login em um contêiner.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   3. Executar curl do endereço IP do cluster e da porta do serviço. Se o endereço IP e a porta não estiverem acessíveis, observe os terminais para o serviço. Se nenhum terminal está listado, o seletor para o serviço não corresponde ao pods. Se os terminais estiverem listados, olhe para o campo da porta de destino no serviço e certifique-se de que a porta de destino seja a mesma que está sendo usada para os pods.
     <pre class="pre"><code>curl &lt;cluster_IP&gt;:&lt;port&gt;</code></pre>

6. Para serviços do Ingresso, verifique se o serviço está acessível de dentro do cluster.
   1. Obtenha o nome de um pod.
     <pre class="pre"><code>kubectl get pods</code></pre>
   2. Efetue login em um contêiner.
     <pre class="pre"><code>kubectl exec -it &lt;pod_name&gt; -- /bin/bash</code></pre>
   2. Executar curl da URL especificada para o serviço do Ingresso. Se a URL não estiver acessível, procure um problema de firewall entre o cluster e o terminal externo. 
     <pre class="pre"><code>curl &lt;host_name&gt;.&lt;domain&gt;</code></pre>

<br />



## Obtendo ajuda e suporte
{: #ts_getting_help}

Ainda está tendo problemas com o seu cluster?
{: shortdesc}

-  No terminal, você é notificado quando atualizações para a CLI `ibmcloud` e plug-ins estão disponíveis. Certifique-se de manter sua CLI atualizada para que seja possível usar todos os comandos e sinalizações disponíveis.
-   Para ver se o {{site.data.keyword.Bluemix_notm}} está disponível, [verifique a página de status do {{site.data.keyword.Bluemix_notm}} ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://cloud.ibm.com/status?selected=status).
-   Poste uma pergunta no [{{site.data.keyword.containerlong_notm}} Slack ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://ibm-container-service.slack.com).
    Se você não estiver usando um IBMid para a sua conta do {{site.data.keyword.Bluemix_notm}}, [solicite um convite](https://bxcs-slack-invite.mybluemix.net/) para essa Folga.
    {: tip}
-   Revise os fóruns para ver se outros usuários tiveram o mesmo problema. Ao usar os fóruns para fazer uma pergunta, marque sua pergunta para que ela seja vista pelas equipes de desenvolvimento do {{site.data.keyword.Bluemix_notm}}.
    -   Se você tiver questões técnicas sobre como desenvolver ou implementar clusters ou apps com o {{site.data.keyword.containerlong_notm}}, poste sua pergunta no [Stack Overflow ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo") ](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e identifique-a com `ibm-cloud`, `kubernetes` e `containers`.
    -   Para perguntas sobre o serviço e instruções de introdução, use o fórum do [IBM Developer Answers ![Ícone de link externo](../icons/launch-glyph.svg "Ícone de link externo")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Inclua as tags `ibm-cloud` e `containers`.
    Consulte
[Obtendo
ajuda](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) para obter mais detalhes sobre o uso dos fóruns.
-   Entre em contato com o Suporte IBM abrindo um caso. Para saber mais sobre como abrir um caso de suporte IBM ou sobre os níveis de suporte e as severidades do caso, consulte [Entrando em contato com o suporte](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
Ao relatar um problema, inclua o ID do cluster. Para obter o ID do seu cluster, execute `ibmcloud ks clusters`. É possível também usar o [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) para reunir e exportar informações pertinentes de seu cluster para compartilhar com o Suporte IBM.
{: tip}

