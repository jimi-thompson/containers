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


# Risoluzione dei problemi dei cluster e dei nodi di lavoro
{: #cs_troubleshoot_clusters}

Mentre utilizzi {{site.data.keyword.containerlong}}, tieni presente queste tecniche per la risoluzione dei problemi dei tuoi cluster e nodi di lavoro.
{: shortdesc}

Se hai un problema più generale, prova a [eseguire il debug del cluster](/docs/containers?topic=containers-cs_troubleshoot).
{: tip}

Mentre risolvi i problemi, puoi utilizzare il [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) per eseguire dei test e raccogliere informazioni pertinenti dal tuo cluster.
{: tip}

## Impossibile creare un cluster o gestire i nodi di lavoro a causa di errori di autorizzazione
{: #cs_credentials}

{: tsSymptoms}
Provi a gestire i nodi di lavoro per un cluster nuovo o esistente eseguendo uno dei seguenti comandi.
* Provisioning dei nodi di lavoro: `ibmcloud ks cluster-create`, `ibmcloud ks worker-pool-rebalance` o `ibmcloud ks worker-pool-resize`
* Ricaricamento dei nodi di lavoro: `ibmcloud ks worker-reload` o `ibmcloud ks worker-update`
* Riavvio dei nodi di lavoro: `ibmcloud ks worker-reboot`
* Eliminazione dei nodi di lavoro: `ibmcloud ks cluster-rm`, `ibmcloud ks worker-rm`, `ibmcloud ks worker-pool-rebalance` o `ibmcloud ks worker-pool-resize`

Tuttavia, ricevi un messaggio di errore simile a uno dei seguenti.

```
We were unable to connect to your IBM Cloud infrastructure (SoftLayer) account.
Creating a standard cluster requires that you have either a
Pay-As-You-Go account that is linked to an IBM Cloud infrastructure (SoftLayer)
account term or that you have used the {{site.data.keyword.containerlong_notm}}
CLI to set your {{site.data.keyword.Bluemix_notm}} Infrastructure API keys.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
'Item' must be ordered with permission.
```
{: screen}

```
{{site.data.keyword.Bluemix_notm}} Infrastructure Exception:
The user does not have the necessary {{site.data.keyword.Bluemix_notm}}
Infrastructure permissions to add servers
```
{: screen}

```
IAM token exchange request failed: Cannot create IMS portal token, as no IMS account is linked to the selected BSS account
```
{: screen}

```
The cluster could not be configured with the registry. Make sure that you have the Administrator role for {{site.data.keyword.registrylong_notm}}.
```
{: screen}

{: tsCauses}
Non disponi delle autorizzazioni corrette per creare un cluster. Per creare un cluster, devi disporre delle seguenti autorizzazioni:
*  Ruolo **Super utente** per l'infrastruttura IBM Cloud (SoftLayer).
*  Ruolo di gestione della piattaforma **Amministratore** per {{site.data.keyword.containerlong_notm}} a livello di account.
*  Ruolo di gestione della piattaforma **Amministratore** per {{site.data.keyword.registrylong_notm}} a livello di account. Non limitare le politiche per {{site.data.keyword.registryshort_notm}} al livello del gruppo di risorse. Se hai iniziato a utilizzare {{site.data.keyword.registrylong_notm}} prima del 4 ottobre 2018, assicurati di [abilitare l'applicazione delle politiche {{site.data.keyword.Bluemix_notm}} IAM](/docs/services/Registry?topic=registry-user#existing_users).

Per gli errori relativi all'infrastruttura, gli account Pagamento a consumo {{site.data.keyword.Bluemix_notm}} che sono stati creati dopo l'abilitazione del collegamento automatico degli account sono già configurati con l'accesso al portfolio dell'infrastruttura IBM Cloud (SoftLayer). Puoi acquistare le risorse dell'infrastruttura per il tuo cluster senza ulteriori configurazioni. Se hai un account Pagamento a consumo valido e ricevi questo messaggio di errore, è possibile che tu non stia utilizzando le credenziali dell'account dell'infrastruttura IBM Cloud (SoftLayer) corrette per accedere alle risorse dell'infrastruttura.

Gli utenti con altri tipi di account {{site.data.keyword.Bluemix_notm}} devono configurare i loro account per creare dei cluster standard. Sono di seguito riportati degli esempi di quando potresti avere un tipo di account differente:
* Hai un account dell'infrastruttura IBM Cloud (SoftLayer) che ha una data anteriore al tuo account della piattaforma {{site.data.keyword.Bluemix_notm}} e vuoi continuare a utilizzarlo.
* Vuoi utilizzare un account dell'infrastruttura IBM Cloud (SoftLayer) differente per eseguire il provisioning di risorse dell'infrastruttura. Ad esempio, potresti configurare un account {{site.data.keyword.Bluemix_notm}} di team per utilizzare un account dell'infrastruttura differente per scopi di fatturazione.

Se utilizzi un account dell'infrastruttura IBM Cloud (SoftLayer) differente per eseguire il provisioning di risorse dell'infrastruttura, potresti avere anche dei [cluster orfani](#orphaned) nel tuo account.

{: tsResolve}
Il proprietario dell'account deve configurare correttamente le credenziali dell'account dell'infrastruttura. Le credenziali dipendono dal tipo di account dell'infrastruttura che stai utilizzando.

1.  Verifica di avere accesso a un account dell'infrastruttura. Esegui l'accesso alla [console {{site.data.keyword.Bluemix_notm}}![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/) e, dal menu ![icona Menu](../icons/icon_hamburger.svg "icona Menu"), fai clic su **Infrastruttura classica**. Se vedi un menu, hai accesso a un account dell'infrastruttura. Se non hai accesso, vedi un'opzione per eseguire l'upgrade del tuo account.
2.  Controlla se il tuo cluster utilizza un account dell'infrastruttura differente da quello fornito con il tuo account Pagamento a consumo.
    1.  Dal menu ![icona Menu](../icons/icon_hamburger.svg "icona Menu"), fai clic su **Kubernetes > Cluster**.
    2.  Dalla tabella, seleziona il tuo cluster.
    3.  Nella scheda **Panoramica**. controlla l'eventuale presenza di un campo **Utente infrastruttura**.
        * Se non vedi il campo **Utente infrastruttura**, hai un account Pagamento a consumo collegato che utilizza le stesse credenziali per i tuoi account dell'infrastruttura e della piattaforma.
        * Se vedi un campo **Utente infrastruttura**, il tuo cluster utilizza un account dell'infrastruttura differente da quello che era stato fornito con il tuo account Pagamento a consumo. Queste credenziali differenti si applicano a tutti i cluster nella regione.
3.  Decidi quale tipo di account vuoi avere per determinare come risolvere il tuo problema di autorizzazione dell'infrastruttura. Per la maggior parte degli utenti, l'account Pagamento a consumo collegato predefinito è sufficiente.
    *  Account Pagamento a consumo {{site.data.keyword.Bluemix_notm}} collegato: [verifica che la chiave API sia configurata con le autorizzazioni corrette](/docs/containers?topic=containers-users#default_account). Se il tuo cluster sta utilizzando un account dell'infrastruttura differente, devi annullare l'impostazione di queste credenziali come parte del processo.
    *  Account {{site.data.keyword.Bluemix_notm}} della piattaforma e dell'infrastruttura differenti: verifica che puoi accedere al portfolio dell'infrastruttura e che [le credenziali dell'account dell'infrastruttura siano configurate con le autorizzazioni corrette](/docs/containers?topic=containers-users#credentials).
4.  Se non riesci a vedere i nodi di lavoro del cluster nel tuo account dell'infrastruttura, puoi controllare se il [cluster è orfano](#orphaned).

<br />


## Il firewall impedisce l'esecuzione dei comandi della CLI
{: #ts_firewall_clis}

{: tsSymptoms}
Quando esegui i comandi `ibmcloud`, `kubectl` o `calicoctl` dalla CLI, hanno esito negativo.

{: tsCauses}
Puoi avere delle politiche di rete aziendali che impediscono l'accesso dal tuo sistema locale agli endpoint pubblici tramite i proxy o i firewall

{: tsResolve}
[Consenti l'accesso TCP per il funzionamento dei comandi della CLI](/docs/containers?topic=containers-firewall#firewall_bx). Questa attività richiede **Amministratore** come [ruolo della piattaforma {{site.data.keyword.Bluemix_notm}} IAM](/docs/containers?topic=containers-users#platform) per il cluster.


## Impossibile accedere alle risorse nel mio cluster
{: #cs_firewall}

{: tsSymptoms}
Quando i nodi di lavoro nel tuo cluster non possono comunicare sulla rete privata, potresti riscontrare vari sintomi differenti.

- Messaggio di errore di esempio quando esegui `kubectl exec`, `attach`, `logs`, `proxy` o `port-forward`:
  ```
  Error from server: error dialing backend: dial tcp XXX.XXX.XXX:10250: getsockopt: connection timed out
  ```
  {: screen}

- Messaggio di errore di esempio quando `kubectl proxy` ha esito positivo ma il dashboard Kubernetes non è disponibile:
  ```
  timeout on 172.xxx.xxx.xxx
  ```
  {: screen}

- Messaggio di errore di esempio quando `kubectl proxy` non riesce o la connessione al tuo servizio non riesce.
  ```
  Connection refused
  ```
  {: screen}

  ```
  Connection timed out
  ```
  {: screen}

  ```
  Impossibile collegarsi al server: net/http: timeout handshake TLS
  ```
  {: screen}


{: tsCauses}
Per accedere alle risorse nel cluster, i tuoi nodi di lavoro devono essere in grado di comunicare sulla rete privata. Potresti aver configurato un firewall Vyatta o un altro firewall o aver personalizzato le tue impostazioni firewall esistenti nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). {{site.data.keyword.containerlong_notm}}
richiede che alcuni indirizzi IP e porte siano aperti per consentire le comunicazioni dal nodo di lavoro al master Kubernetes e viceversa. Se i tuoi nodi di lavoro sono distribuiti tra più zone, devi consentire le comunicazioni di rete privata abilitando lo spanning della VLAN. Le comunicazioni tra i nodi di lavoro potrebbero anche non essere possibili se i tuoi nodi di lavoro sono bloccati in un loop di ricaricamento.

{: tsResolve}
1. Elenca i nodi di lavoro nel tuo cluster e verifica che i tuoi nodi di lavoro non siano bloccati in uno stato di `Reloading`.
   ```
   ibmcloud ks workers <cluster_name_or_id>
   ```
   {: pre}

2. Se hai un cluster multizona e il tuo account non è abilitato per la VRF, verifica di avere [abilitato lo spanning della VLAN](/docs/containers?topic=containers-subnets#subnet-routing) per il tuo account.
3. Se hai delle impostazioni di firewall Vyatta o personalizzate, assicurati di avere [aperto le porte richieste](/docs/containers?topic=containers-firewall#firewall_outbound) per consentire al cluster di accedere ai servizi e alle risorse dell'infrastruttura.

<br />



## Impossibile visualizzare o lavorare con un cluster
{: #cs_cluster_access}

{: tsSymptoms}
* Non riesci a trovare un cluster. Quando esegui `ibmcloud ks clusters`, il cluster non viene elencato nell'output.
* Non riesci a lavorare con un cluster. Quando esegui `ibmcloud ks cluster-config` o altri comandi specifici del cluster, il cluster non viene trovato.


{: tsCauses}
In {{site.data.keyword.Bluemix_notm}}, ogni risorsa deve trovarsi in un gruppo di risorse. Ad esempio, il cluster `mycluster` potrebbe essere presente nel gruppo di risorse `default`. Quando il proprietario dell'account ti concede l'accesso alle risorse assegnandoti un ruolo della piattaforma {{site.data.keyword.Bluemix_notm}} IAM, l'accesso può essere a una specifica risorsa o al gruppo di risorse. Quando ti viene fornito l'accesso a una risorsa specifica, non hai accesso al gruppo di risorse. In questo caso, non devi specificare un gruppo di risorse per lavorare con il cluster a cui hai accesso. Se specifichi un gruppo di risorse diverso da quello in cui si trova il cluster, le azioni sul cluster potrebbero non riuscire. Al contrario, quando ti viene fornito l'accesso a una risorsa come parte dell'accesso a un gruppo di risorse, devi specificare un gruppo di risorse per lavorare con un cluster in quel gruppo. Se non indirizzi la tua sessione della CLI al gruppo di risorse in cui si trova il cluster, le azioni in tale cluster potrebbero non riuscire.

Se non riesci a trovare o lavorare con un cluster, potresti riscontrare uno dei seguenti problemi:
* Hai accesso al cluster e al gruppo di risorse in cui si trova il cluster, ma la sessione della CLI non è indirizzata al gruppo di risorse in cui si trova il cluster.
* Hai accesso al cluster, ma non come parte del gruppo di risorse in cui si trova il cluster. La tua sessione della CLI è indirizzata a questo o a un altro gruppo di risorse.
* Non hai accesso al cluster.

{: tsResolve}
Per controllare le tue autorizzazioni di accesso utente:

1. Elenca tutte le tue autorizzazioni utente.
    ```
    ibmcloud iam user-policies <your_user_name>
    ```
    {: pre}

2. Controlla se disponi dell'accesso al cluster e al gruppo di risorse in cui si trova il cluster.
    1. Cerca una politica che abbia il valore **Nome gruppo di risorse** uguale al gruppo di risorse del cluster e il valore **Memo** uguale a `La politica si applica al gruppo di risorse`. Se disponi di questa politica, hai accesso al gruppo di risorse. Ad esempio, questa politica indica che un utente ha accesso al gruppo di risorse `test-rg`:
        ```
        Policy ID:   3ec2c069-fc64-4916-af9e-e6f318e2a16c
        Roles:       Viewer
        Resources:
                     Resource Group ID     50c9b81c983e438b8e42b2e8eca04065
                     Resource Group Name   test-rg
                     Memo                  Policy applies to the resource group
        ```
        {: screen}
    2. Cerca una politica che abbia il valore **Nome gruppo di risorse** uguale al gruppo di risorse del cluster, il valore **Nome servizio** di `containers-kubernetes` o nessun valore e il valore **Memo** uguale a `La politica si applica alla risorsa o alle risorse nel gruppo di risorse`. Se disponi di questa politica, hai accesso ai cluster o a tutte le risorse all'interno del gruppo di risorse. Ad esempio, questa politica indica che un utente ha accesso ai cluster nel gruppo di risorse `test-rg`:
        ```
        Policy ID:   e0ad889d-56ba-416c-89ae-a03f3cd8eeea
        Roles:       Administrator
        Resources:
                     Resource Group ID     a8a12accd63b437bbd6d58fb6a462ca7
                     Resource Group Name   test-rg
                     Service Name          containers-kubernetes
                     Service Instance
                     Region
                     Resource Type
                     Resource
                     Memo                  Policy applies to the resource(s) within the resource group
        ```
        {: screen}
    3. Se hai entrambe queste politiche, vai al passo 4, primo punto di elenco. Se non hai la politica del passo 2a, ma hai la politica del passo 2b, vai al passo 4, secondo punto di elenco. Se non hai nessuna di queste politiche, continua al passo 3.

3. Controlla se disponi dell'accesso al cluster, ma non come parte dell'accesso al gruppo di risorse in cui si trova il cluster.
    1. Cerca una politica che non abbia valori oltre ai campi **ID politica** e **Ruoli**. Se hai questa politica, hai accesso al cluster come parte dell'accesso all'intero account. Ad esempio, questa politica indica che un utente ha accesso a tutte le risorse nell'account:
        ```
        Policy ID:   8898bdfd-d520-49a7-85f8-c0d382c4934e
        Roles:       Administrator, Manager
        Resources:
                     Service Name
                     Service Instance
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    2. Cerca una politica che abbia il valore **Nome servizio** uguale a `containers-kubernetes` e il valore **Istanza del servizio** uguale all'ID del cluster. Puoi trovare un ID cluster eseguendo `ibmcloud ks cluster-get --cluster <cluster_name>`. Ad esempio, questa politica indica che un utente ha accesso a un cluster specifico:
        ```
        Policy ID:   140555ce-93ac-4fb2-b15d-6ad726795d90
        Roles:       Administrator
        Resources:
                     Service Name       containers-kubernetes
                     Service Instance   df253b6025d64944ab99ed63bb4567b6
                     Region
                     Resource Type
                     Resource
        ```
        {: screen}
    3. Se hai una di queste politiche, vai al secondo punto di elenco del passo 4. Se non hai nessuna di queste politiche, vai al terzo punto di elenco del passo 4.

4. A seconda delle tue politiche di accesso, scegli una delle seguenti opzioni.
    * Se hai accesso al cluster e al gruppo di risorse in cui si trova il cluster:
      1. Specifica il gruppo di risorse. **Nota**: non puoi lavorare con i cluster in altri gruppi di risorse fino a quando non annulli la specifica di questo gruppo di risorse.
          ```
          ibmcloud target -g <resource_group>
          ```
          {: pre}

      2. Specifica il cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

    * Se hai accesso al cluster ma non al gruppo di risorse in cui si trova il cluster:
      1. Non specificare un gruppo di risorse. Se hai già specificato un gruppo di risorse, annulla la specifica:
        ```
        ibmcloud target -g none
        ```
        {: pre}
        Questo comando non riesce perché non esiste alcun gruppo di risorse denominato `none`. Tuttavia, il gruppo di risorse corrente viene automaticamente disattivato quando il comando non riesce.

      2. Specifica il cluster.
        ```
        ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
        ```
        {: pre}

    * Se non hai accesso al cluster:
        1. Chiedi al proprietario dell'account di assegnarti un [ruolo della piattaforma {{site.data.keyword.Bluemix_notm}}IAM](/docs/containers?topic=containers-users#platform) per tale cluster.
        2. Non specificare un gruppo di risorse. Se hai già specificato un gruppo di risorse, annulla la specifica:
          ```
          ibmcloud target -g none
          ```
          {: pre}
          Questo comando non riesce perché non esiste alcun gruppo di risorse denominato `none`. Tuttavia, il gruppo di risorse corrente viene automaticamente disattivato quando il comando non riesce.
        3. Specifica il cluster.
          ```
          ibmcloud ks cluster-config --cluster <cluster_name_or_ID>
          ```
          {: pre}

<br />


## L'accesso al tuo nodo di lavoro con SSH non riesce
{: #cs_ssh_worker}

{: tsSymptoms}
Non è possibile accedere al tuo nodo di lavoro utilizzando una connessione SSH.

{: tsCauses}
SSH tramite password non è disponibile sui nodi di lavoro.

{: tsResolve}
Utilizza una [`Serie daemon` ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/concepts/workloads/controllers/daemonset/) di Kubernetes per le azioni che devi eseguire su ogni nodo o utilizza i lavori per le azioni uniche che devi eseguire.

<br />


## L'ID dell'istanza bare metal non è congruente con i record di lavoro
{: #bm_machine_id}

{: tsSymptoms}
Quando utilizzi i comandi `ibmcloud ks worker` con il nodo di lavoro bare metal, vedi un messaggio simile al seguente.

```
Instance ID inconsistent with worker records
```
{: screen}

{: tsCauses}
L'ID macchina può diventare incongruente con il record di lavoro {{site.data.keyword.containerlong_notm}} quando la macchina riscontra dei problemi hardware. Quando l'infrastruttura IBM Cloud (SoftLayer) risolve questo problema, un componente può subire delle variazioni all'interno del sistema che il servizio non identifica.

{: tsResolve}
Perché {{site.data.keyword.containerlong_notm}} identifichi nuovamente la macchina, [ricarica il nodo di lavoro bare metal](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reload). **Nota**: il ricaricamento aggiorna anche la [versione patch](/docs/containers?topic=containers-changelog) della macchina.

Puoi anche [eliminare il nodo di lavoro bare metal](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_rm). **Nota**: le istanze bare metal vengono fatturate mensilmente.

<br />


## Impossibile modificare o eliminare l'infrastruttura in un cluster orfano
{: #orphaned}

{: tsSymptoms}
Non puoi eseguire comandi relativi all'infrastruttura sul cluster, come ad esempio:
* Aggiunta o rimozione di nodi di lavoro
* Ricaricamento o riavvio dei nodi di lavoro
* Ridimensionamento dei pool di nodi di lavoro
* Aggiornamento del tuo cluster

Non puoi visualizzare i nodi di lavoro del cluster nel tuo account dell'infrastruttura IBM Cloud (SoftLayer). Tuttavia, puoi aggiornare e gestire altri cluster nell'account.

Inoltre, hai verificato di disporre delle [credenziali di infrastruttura appropriate](#cs_credentials).

{: tsCauses}
Il cluster potrebbe essere stato fornito in un account dell'infrastruttura IBM Cloud (SoftLayer) che non è più collegato al tuo account {{site.data.keyword.containerlong_notm}}. Il cluster è orfano. Poiché le risorse si trovano in un account diverso, non disponi delle credenziali dell'infrastruttura per modificare le risorse.

Considera il seguente scenario per capire come i cluster potrebbero diventare orfani.
1.  Hai un account Pagamento a consumo {{site.data.keyword.Bluemix_notm}}.
2.  Crei un cluster denominato `Cluster1`. I nodi di lavoro e altre risorse dell'infrastruttura vengono forniti nell'account dell'infrastruttura fornito con il tuo account Pagamento a consumo.
3.  Successivamente, scopri che il tuo team utilizza un account legacy o condiviso dell'infrastruttura IBM Cloud (SoftLayer). Utilizzi il comando `ibmcloud ks credential-set` per modificare le credenziali dell'infrastruttura IBM Cloud (SoftLayer) per utilizzare l'account del tuo team.
4.  Crei un altro cluster denominato `Cluster2`. I nodi di lavoro e altre risorse dell'infrastruttura vengono forniti nell'account dell'infrastruttura del team.
5.  Noti che `Cluster1` richiede un aggiornamento del nodo di lavoro, un ricaricamento del nodo di lavoro o semplicemente vuoi cancellarlo eliminandolo. Tuttavia, poiché `Cluster1` è stato fornito in un account dell'infrastruttura differente, non puoi modificare le sue risorse di infrastruttura. `Cluster1` è orfano.
6.  Segui i passi di risoluzione nella seguente sezione, ma senza reimpostare le tue credenziali dell'infrastruttura sull'account del tuo team. Puoi eliminare `Cluster1`, ma adesso `Cluster2` è orfano.
7.  Modifichi le tue credenziali dell'infrastruttura nell'account del team che ha creato `Cluster2`. Adesso, non hai più un cluster orfano.

<br>

{: tsResolve}
1.  Verifica quale account dell'infrastruttura utilizza la regione in cui si trova attualmente il tuo cluster per eseguire il provisioning dei cluster.
    1.  Accedi alla [console dei cluster {{site.data.keyword.containerlong_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/containers-kubernetes/clusters).
    2.  Dalla tabella, seleziona il tuo cluster.
    3.  Nella scheda **Panoramica**. controlla l'eventuale presenza di un campo **Utente infrastruttura**. Questo campo ti aiuta a determinare se il tuo account di {{site.data.keyword.containerlong_notm}} utilizza un account di infrastruttura diverso da quello predefinito.
        * Se non vedi il campo **Utente infrastruttura**, hai un account Pagamento a consumo collegato che utilizza le stesse credenziali per i tuoi account dell'infrastruttura e della piattaforma. Il cluster che non può essere modificato potrebbe essere stato fornito in un altro account dell'infrastruttura.
        * Se vedi un campo **Utente infrastruttura**, utilizzi un account dell'infrastruttura diverso da quello che viene fornito con il tuo account Pagamento a consumo. Queste credenziali differenti si applicano a tutti i cluster nella regione. Il cluster che non può essere modificato potrebbe essere stato fornito nel tuo account Pagamento a consumo o in un altro account dell'infrastruttura.
2.  Verifica quale account dell'infrastruttura è stato utilizzato per eseguire il provisioning del cluster.
    1.  Nella scheda **Nodi di lavoro**, seleziona un nodo di lavoro e prendi nota del suo **ID**.
    2.  Apri il menu ![Icona Menu](../icons/icon_hamburger.svg "Icona Menu") e fai clic su **Infrastruttura classica**.
    3.  Dal riquadro di navigazione dell'infrastruttura, fai clic su **Dispositivi > Elenco dispositivi**.
    4.  Cerca l'ID nodo di lavoro di cui hai precedentemente preso nota.
    5.  Se non trovi l'ID nodo di lavoro, il nodo di lavoro non è fornito in questo account dell'infrastruttura. Passa a un altro account dell'infrastruttura e riprova.
3.  Utilizza il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set) `ibmcloud ks credential-set` per modificare le credenziali dell'infrastruttura sull'account in cui è stato eseguito il provisioning dei nodi di lavoro del cluster, che hai trovato nel passo precedente.
    Se non hai più accesso e non puoi ottenere le credenziali dell'infrastruttura, devi aprire un caso di supporto {{site.data.keyword.Bluemix_notm}} per rimuovere il cluster orfano.
    {: note}
4.  [Elimina il cluster](/docs/containers?topic=containers-clusters#remove).
5.  Se lo desideri, ripristina le credenziali dell'infrastruttura sull'account precedente. Nota che se hai creato dei cluster con un account dell'infrastruttura diverso dall'account a cui passi, potresti rendere orfani tali cluster.
    * Per impostare le credenziali su un account dell'infrastruttura diverso, utilizza il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_set) `ibmcloud ks credential-set`.
    * Per utilizzare le credenziali predefinite fornite con il tuo account Pagamento a consumo di {{site.data.keyword.Bluemix_notm}}, utilizza il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_credentials_unset) `ibmcloud ks credential-unset`.

<br />


## I comandi `kubectl` vanno in timeout
{: #exec_logs_fail}

{: tsSymptoms}
Se esegui comandi quali `kubectl exec`, `kubectl attach`, `kubectl proxy`, `kubectl port-forward` o `kubectl logs`, vedi il seguente messaggio:

  ```
  <workerIP>:10250: getsockopt: connection timed out
  ```
  {: screen}

{: tsCauses}
La connessione OpenVPN tra il nodo master e i nodi di lavoro non funziona correttamente.

{: tsResolve}
1. Se hai più VLAN per un cluster, più sottoreti sulla stessa VLAN o un cluster multizona, devi abilitare una [VRF (Virtual Router Function)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#customer-vrf-overview) per il tuo account dell'infrastruttura IBM Cloud (SoftLayer) in modo che i tuoi nodi di lavoro possano comunicare tra loro sulla rete privata. Per abilitare VRF, [contatta il tuo rappresentante dell'account dell'infrastruttura IBM Cloud (SoftLayer)](/docs/infrastructure/direct-link?topic=direct-link-overview-of-virtual-routing-and-forwarding-vrf-on-ibm-cloud#how-you-can-initiate-the-conversion). Se non puoi o non vuoi abilitare VRF, abilita lo [spanning della VLAN](/docs/infrastructure/vlans?topic=vlans-vlan-spanning#vlan-spanning). Per eseguire questa azione, ti serve l'[autorizzazione dell'infrastruttura](/docs/containers?topic=containers-users#infra_access) **Rete > Gestisci il VLAN Spanning di rete** oppure puoi richiedere al proprietario dell'account di abilitarlo. Per controllare se lo spanning della VLAN è già abilitato, utilizza il [comando](/docs/containers?topic=containers-cs_cli_reference#cs_vlan_spanning_get) `ibmcloud ks vlan-spanning-get`.
2. Riavvia il pod client OpenVPN.
  ```
  kubectl delete pod -n kube-system -l app=vpn
  ```
  {: pre}
3. Se ancora visualizzi lo stesso messaggio di errore, il nodo di lavoro su cui è il pod VPN potrebbe non essere integro. Per riavviare il pod VPN e ripianificarlo su un nodo di lavoro diverso, [delimita, trascina e riavvia il nodo di lavoro](/docs/containers?topic=containers-cs_cli_reference#cs_worker_reboot) su cui è il pod VPN.

<br />


## L'associazione di un servizio a un cluster provoca un errore di stessa denominazione
{: #cs_duplicate_services}

{: tsSymptoms}
Quando esegui `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, visualizzi il seguente messaggio.

```
Multiple services with the same name were found.
Run 'ibmcloud service list' to view available Bluemix service instances...
```
{: screen}

{: tsCauses}
Più istanze del servizio potrebbero avere lo stesso nome in regioni differenti.

{: tsResolve}
Utilizza il GUID del servizio invece del nome dell'istanza del servizio nel comando `ibmcloud ks cluster-service-bind`.

1. [Accedi alla regione che include l'istanza del servizio da associare.](/docs/containers?topic=containers-regions-and-zones#bluemix_regions)

2. Ottieni il GUID per l'istanza del servizio.
  ```
  ibmcloud service show <service_instance_name> --guid
  ```
  {: pre}

  Output:
  ```
  Invoking 'cf service <service_instance_name> --guid'...
  <service_instance_GUID>
  ```
  {: screen}
3. Associa nuovamente il servizio al cluster.
  ```
  ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_GUID>
  ```
  {: pre}

<br />


## L'associazione di un servizio a un cluster provoca un errore di non trovato
{: #cs_not_found_services}

{: tsSymptoms}
Quando esegui `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, visualizzi il seguente messaggio.

```
Binding service to a namespace...
FAILED

The specified IBM Cloud service could not be found. If you just created the service, wait a little and then try to bind it again. To view available IBM Cloud service instances, run 'ibmcloud service list'. (E0023)
```
{: screen}

{: tsCauses}
Per associare i servizi a un cluster, devi avere il ruolo utente di sviluppatore Cloud Foundry per lo spazio in cui è stato eseguito il provisioning dell'istanza del servizio. Inoltre, devi disporre dell'accesso alla piattaforma {{site.data.keyword.Bluemix_notm}} IAM come Editor per {{site.data.keyword.containerlong}}. Per accedere all'istanza del servizio, devi aver eseguito il login allo spazio in cui è stato eseguito il provisioning dell'istanza del servizio.

{: tsResolve}

**Come utente:**

1. Accedi a {{site.data.keyword.Bluemix_notm}}.
   ```
   ibmcloud login
   ```
   {: pre}

2. Specifica l'organizzazione e lo spazio in cui viene eseguito il provisioning dell'istanza del servizio.
   ```
   ibmcloud target -o <org> -s <space>
   ```
   {: pre}

3. Verifica di essere nello spazio corretto elencando le tue istanze del servizio.
   ```
   ibmcloud service list
   ```
   {: pre}

4. Riprova ad associare il servizio. Se ricevi lo stesso messaggio di errore, contatta l'amministratore dell'account e verifica di avere le autorizzazioni necessarie per associare i servizi (consulta la seguente procedura di gestione dell'account).

**Come amministratore dell'account:**

1. Verifica che l'utente che ha riscontrato questo problema abbia le [autorizzazioni di editor per {{site.data.keyword.containerlong}}](/docs/iam?topic=iam-iammanidaccser#edit_existing).

2. Verifica che l'utente che ha riscontrato questo problema abbia il [ruolo di sviluppatore Cloud Foundry dello spazio](/docs/iam?topic=iam-mngcf#update_cf_access) in cui è stato eseguito il provisioning del servizio.

3. Se le autorizzazioni sono corrette, prova ad assegnare un'autorizzazione differente e riassegna l'autorizzazione necessaria.

4. Attendi alcuni minuti, quindi consenti all'utente di riprovare l'associazione al servizio.

5. Se ciò non risolve il problema, le autorizzazioni {{site.data.keyword.Bluemix_notm}} IAM non sono sincronizzate e non puoi risolvere il problema da solo. [Contatta il supporto IBM](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support) aprendo un caso di supporto. Assicurati di fornire l'ID cluster, l'ID utente e l'ID istanza del servizio.
   1. Richiama l'ID cluster.
      ```
      ibmcloud ks clusters
      ```
      {: pre}

   2. Richiama l'ID istanza del servizio.
      ```
      ibmcloud service show <service_name> --guid
      ```
      {: pre}


<br />


## L'esecuzione del bind di un servizio a un cluster provoca un errore che indica che il servizio non supporta le chiavi di servizio
{: #cs_service_keys}

{: tsSymptoms}
Quando esegui `ibmcloud ks cluster-service-bind <cluster_name> <namespace> <service_instance_name>`, visualizzi il seguente messaggio.

```
This service doesn't support creation of keys
```
{: screen}

{: tsCauses}
Alcuni servizi in {{site.data.keyword.Bluemix_notm}}, come ad esempio {{site.data.keyword.keymanagementservicelong}}, non supportano la creazione delle credenziali del servizio, indicate anche come chiavi del servizio. Senza il supporto delle chiavi del servizio, non è possibile eseguire il bind del servizio a un cluster. Per trovare un elenco di servizi che supportano la creazione di chiavi del servizio, vedi [Abilitazione di applicazioni esterne a utilizzare servizi {{site.data.keyword.Bluemix_notm}}](/docs/resources?topic=resources-externalapp#externalapp).

{: tsResolve}
Per integrare i servizi che non supportano le chiavi del servizio, controlla se il servizio fornisce un'API che puoi utilizzare per accedere al servizio direttamente dalla tua applicazione. Ad esempio, se vuoi utilizzare {{site.data.keyword.keymanagementservicelong}}, vedi la [guida di riferimento API ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/apidocs/kms?language=curl).

<br />


## Dopo aver aggiornato o ricaricato un nodo di lavoro, vengono visualizzati i pod e i nodi duplicati
{: #cs_duplicate_nodes}

{: tsSymptoms}
Quando esegui `kubectl get nodes` visualizzi i nodi di lavoro duplicati con la condizione **`NotReady`**. I nodi di lavoro con la condizione **`NotReady`** hanno indirizzi IP pubblici, mentre i nodi di lavoro con **`Ready`** hanno indirizzi IP privati.

{: tsCauses}
I cluster meno recenti hanno i nodi di lavoro elencati per indirizzo IP pubblico del cluster. Ora, i nodi di lavoro sono elencati per indirizzo IP privato del cluster. Quando ricarichi o aggiorni un nodo, l'indirizzo IP viene modificato, ma il riferimento all'indirizzo IP pubblico rimane.

{: tsResolve}
Il servizio non viene interrotto a causa di questi duplicati ma puoi rimuovere i riferimenti al nodo di lavoro precedente dal server API.

  ```
  kubectl delete node <node_name1> <node_name2>
  ```
  {: pre}

<br />


## L'accesso a un pod su un nodo di lavoro non è riuscito a causa di un timeout
{: #cs_nodes_duplicate_ip}

{: tsSymptoms}
Hai eliminato un nodo di lavoro nel tuo cluster e quindi aggiunto un nodo di lavoro. Quando distribuisci un pod o un servizio Kubernetes, la risorsa non può accedere al nodo di lavoro appena creato e la connessione va in timeout.

{: tsCauses}
Se elimini un nodo di lavoro dal tuo cluster e quindi ne aggiungi un altro, è possibile che al nuovo nodo di lavoro venga assegnato l'indirizzo IP privato nel nodo di lavoro eliminato. Calico utilizza questo indirizzo IP come una tag e continua a provare a raggiungere il nodo eliminato.

{: tsResolve}
Aggiorna manualmente i riferimenti all'indirizzo IP privato in modo che puntino al nodo corretto.

1.  Conferma di avere due nodi di lavoro con lo stesso indirizzo **IP privato**. Prendi nota di **IP privato** e **ID** del nodo di lavoro eliminato.

  ```
  ibmcloud ks workers --cluster <cluster_name_or_id>
  ```
  {: pre}

  ```
  ID                                                 Public IP       Private IP       Machine Type   State     Status   Zone   Version
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w1   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       normal    Ready    dal10      1.12.6
  kube-dal10-cr9b7371a7fcbe46d08e04f046d5e6d8b4-w2   169.xx.xxx.xxx  10.xxx.xx.xxx    b2c.4x16       deleted    -       dal10      1.12.6
  ```
  {: screen}

2.  Installa la [CLI Calico](/docs/containers?topic=containers-network_policies#adding_network_policies).
3.  Elenca i nodi di lavoro disponibili in Calico. Sostituisci <path_to_file> con il percorso locale al file di configurazione Calico.

  ```
  calicoctl get nodes --config=filepath/calicoctl.cfg
  ```
  {: pre}

  ```
  NAME
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w1
  kube-dal10-cr9b7371a7faaa46d08e04f046d5e6d8b4-w2
  ```
  {: screen}

4.  Elimina il nodo di lavoro duplicato in Calico. Sostituisci NODE_ID con l'ID del nodo di lavoro.

  ```
  calicoctl delete node NODE_ID --config=<path_to_file>/calicoctl.cfg
  ```
  {: pre}

5.  Riavvia il nodo di lavoro che non è stato eliminato.

  ```
  ibmcloud ks worker-reboot --cluster <cluster_name_or_id> --worker <worker_id>
  ```
  {: pre}


Il nodo di lavoro non è più elencato in Calico.

<br />




## La distribuzione dei pod non riesce a causa di una politica di sicurezza dei pod
{: #cs_psp}

{: tsSymptoms}
Dopo la creazione di un pod o l'esecuzione di `kubectl get events` per controllare una distribuzione del pod, vedi un messaggio di errore simile al seguente.

```
unable to validate against any pod security policy
```
{: screen}

{: tsCauses}
[Il controller di ammissione `PodSecurityPolicy` ](/docs/containers?topic=containers-psp)controlla l'autorizzazione dell'account utente o di servizio, come ad esempio una distribuzione o un tiller Helm, che ha tentato di creare il pod. Se nessuna politica di sicurezza dei pod supporta l'account utente o di servizio, il controller di ammissione ` PodSecurityPolicy` impedisce la creazione dei pod.

Se hai eliminato una delle risorse della politica di sicurezza dei pod per [la gestione del cluster {{site.data.keyword.IBM_notm}}](/docs/containers?topic=containers-psp#ibm_psp), potresti riscontrare problemi simili.

{: tsResolve}
Assicurati che l'account utente o di servizio sia autorizzato da una politica di sicurezza dei pod. Potresti dover [modificare una politica esistente](/docs/containers?topic=containers-psp#customize_psp).

Se hai eliminato una risorsa di gestione del cluster {{site.data.keyword.IBM_notm}}, aggiorna il master Kubernetes per ripristinarla.

1.  [Accedi al tuo account. Specifica la regione appropriata e, se applicabile, il gruppo di risorse. Imposta il contesto per il tuo cluster](/docs/containers?topic=containers-cs_cli_install#cs_cli_configure).
2.  Aggiorna il master Kubernetes per eseguirne il ripristino.

    ```
    ibmcloud ks apiserver-refresh
    ```
    {: pre}


<br />




## Il cluster rimane nello stato In sospeso
{: #cs_cluster_pending}

{: tsSymptoms}
Quando distribuisci il cluster, rimane in uno stato in sospeso e non si avvia.

{: tsCauses}
Se hai appena creato il cluster, i nodi di lavoro potrebbero essere ancora in fase di configurazione. Se hai già atteso per un po', potresti avere una VLAN non valida.

{: tsResolve}

Puoi provare una delle seguenti soluzioni:
  - Controlla lo stato del tuo cluster eseguendo `ibmcloud ks clusters`. Quindi verifica che i nodi di lavoro siano distribuiti eseguendo `ibmcloud ks workers --cluster <cluster_name>`.
  - Controlla se la tua VLAN è valida. Perché sia valida, una VLAN deve essere associata a un'infrastruttura in grado di ospitare un nodo di lavoro con archiviazione su disco locale. Puoi [elencare le tue VLAN](/docs/containers?topic=containers-cs_cli_reference#cs_vlans) eseguendo `ibmcloud ks vlans --zone <zone>` se la VLAN non viene visualizzata nell'elenco significa che non è valida. Scegli una VLAN diversa.


<br />


## Non è possibile eseguire il pull dell'immagine dal registro
{: #ts_image_pull}

{: tsSymptoms}
Quando distribuisci un carico di lavoro che segue il pull di un'immagine da {{site.data.keyword.registrylong_notm}}, si verifica un malfunzionamento dei tuoi pod con lo stato **`ImagePullBackOff`**.

```
kubectl get pods
```
{: pre}

```
NAME         READY     STATUS             RESTARTS   AGE
<pod_name>   0/1       ImagePullBackOff   0          2m
```
{: screen}

Quando descrivi il pod, vedi degli errori di autenticazione simili al seguente:

```
kubectl describe pod <pod_name>
```
{: pre}

```
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "<region>.icr.io/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

```
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... unauthorized: authentication required
Failed to pull image "registry.ng.bluemix.net/<namespace>/<image>:<tag>" ... 401 Unauthorized
```
{: screen}

{: tsCauses}
Il tuo cluster utilizzare un token o una chiave API archiviati in un [segreto di pull dell'immagine](/docs/containers?topic=containers-images#cluster_registry_auth) per autorizzare il cluster ad eseguire il pull di immagini da {{site.data.keyword.registrylong_notm}}. Per impostazione predefinite i nuovi cluster hanno dei segreti di pull dell'immagine che utilizzano chiavi API per consentire al cluster di eseguire il pull di immagini da qualsiasi registro regionale per i contenitori distribuiti allo spazio dei nomi Kubernetes `default`.  Se il cluster ha un segreto di pull dell'immagine che utilizza un token, l'accesso predefinito a {{site.data.keyword.registrylong_notm}} è ulteriormente limitato solo a specifici registri regionali.

{: tsResolve}

1.  Verifica di utilizzare il nome e la tag corretti dell'immagine nel tuo file YAML di distribuzione.
    ```
    ibmcloud cr images
    ```
    {: pre}
2.  Ottieni il file di configurazione di un pod in errore e cerca la sezione `imagePullSecrets`.
    ```
    kubectl get pod <pod_name> -o yaml
    ```
    {: pre}

    Output di esempio:
    ```
    ...
    imagePullSecrets:
    - name: bluemix-default-secret
    - name: bluemix-default-secret-regional
    - name: bluemix-default-secret-international
    - name: default-us-icr-io
    - name: default-uk-icr-io
    - name: default-de-icr-io
    - name: default-au-icr-io
    - name: default-jp-icr-io
    - name: default-icr-io
    ...
    ```
    {: screen}
3.  Se non è elencato alcun segreto di pull dell'immagine, eseguine la configurazione nel tuo spazio dei nomi.
    1.  [Copia i segreti di pull dell'immagine dallo spazio dei nomi Kubernetes `default` allo spazio dei nomi dove vuoi distribuire il tuo carico di lavoro](/docs/containers?topic=containers-images#copy_imagePullSecret).
    2.  [Aggiungi il segreto di pull dell'immagine all'account del servizio per questo spazio dei nomi Kubernetes](/docs/containers?topic=containers-images#store_imagePullSecret) in modo che tutti i pod nello spazio dei nomi possano utilizzare le credenziali del segreto di pull dell'immagine.
4.  Se sono elencati i segreti di pull dell'immagine, determina quale tipo di credenziali utilizzi per accedere al registro del contenitore.
    *   **Obsoleto**: se il segreto ha `bluemix` nel nome, utilizzi un token di registro per eseguire l'autenticazione presso i nomi dominio `registry.<region>.bluemix.net` obsoleti. Continua con [Risoluzione dei problemi dei segreti di pull dell'immagine che utilizzano i token](#ts_image_pull_token).
    *   Se il segreto ha `icr` nel nome, utilizzi una chiave API per eseguire l'autenticazione presso i nomi dominio `icr.io`. Continua con [Risoluzione dei problemi dei segreti di pull dell'immagine che utilizzano chiavi API](#ts_image_pull_apikey).
    *   Se hai entrambi i tipi di segreti, utilizzi entrambi i metodi di autenticazione. Andando avanti, utilizza i nomi dominio `icr.io` nei tuoi YAML di distribuzione per l'immagine del contenitore. Continua con [Risoluzione dei problemi dei segreti di pull dell'immagine che utilizzano chiavi API](#ts_image_pull_apikey).

<br>
<br>

**Risoluzione dei problemi dei segreti di pull dell'immagine che utilizzano chiavi API**</br>
{: #ts_image_pull_apikey}

Se la tua configurazione dl pod ha un segreto di pull dell'immagine che utilizza una chiave API, controlla che le credenziali di chiave API siano configurate correttamente.
{: shortdesc}

La seguente procedura presume che la chiave API memorizzi le credenziali di un ID servizio. Se configuri il tuo segreto di pull dell'immagine per utilizzare una chiave API di un singolo utente, devi verificare le credenziali e le autorizzazioni {{site.data.keyword.Bluemix_notm}} IAM di tale utente.
{: note}

1.  Trova l'ID servizio utilizzato dalla chiave API per il segreto di pull dell'immagine esaminando la descrizione (**Description**). L'ID servizio creato con il cluster indica `ID for <cluster_name>` ed è utilizzato nello spazio dei nomi Kubernetes `default`. Se hai creato un altro ID servizio come ad esempio per accedere a uno spazio dei nomi Kubernetes differente o per modificare le autorizzazioni {{site.data.keyword.Bluemix_notm}} IAM, hai personalizzato la descrizione.
    ```
    ibmcloud iam service-ids
    ```
    {: pre}

    Output di esempio:
    ```
    UUID                Name               Created At              Last Updated            Description                                                                                                                                                                                         Locked
    ServiceId-aa11...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   ID for <cluster_name>                                                                                                                                         false
    ServiceId-bb22...   <service_ID_name>  2019-02-01T19:01+0000   2019-02-01T19:01+0000   Service ID for IBM Cloud Container Registry in Kubernetes cluster <cluster_name> namespace <kube_namespace>                                                                                                                                         false
    ```
    {: screen}
2.  Verifica che all'ID servizio sia assegnato almeno una [politica di ruolo di accesso al servizio {{site.data.keyword.Bluemix_notm}} IAM di **Lettore** per {{site.data.keyword.registryshort_notm}}](/docs/services/Registry?topic=registry-user#create). Se l'ID servizio non ha il ruolo del servizio **Lettore**, [modifica le politiche IAM](/docs/iam?topic=iam-serviceidpolicy#access_edit). Se le politiche sono corrette, continua con il passo successivo per appurare se le credenziali sono valide.
    ```
    ibmcloud iam service-policies <service_ID_name>
    ```
    {: pre}

    Output di esempio:
    ```
    Policy ID:   a111a111-b22b-333c-d4dd-e555555555e5
    Roles:       Reader
    Resources:
                  Service Name       container-registry
                  Service Instance
                  Region
                  Resource Type      namespace
                  Resource           <registry_namespace>
    ```
    {: screen}
3.  Controlla se le credenziali del segreto di pull dell'immagine sono valide.
    1.  Ottieni la configurazione del segreto di pull dell'immagine. Se il pod non si trova nello spazio dei nomi `default`, includi l'indicatore `-n`.
        ```
        kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
        ```
        {: pre}
    2.  Nell'output, copia il valore con codifica base64 del campo `.dockercfg`.
        ```
        apiVersion: v1
        kind: Secret
        data:
          .dockercfg: eyJyZWdp...==
        ...
        ```
        {: screen}
    3.  Decodifica la stringa base64. Ad esempio, su OS X puoi eseguire il seguente comando.
        ```
        echo -n "<base64_string>" | base64 --decode
        ```
        {: pre}

        Output di esempio:
        ```
        {"auths":{"<region>.icr.io":{"username":"iamapikey","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
        ```
        {: screen}
    4.  Confronta il nome dominio del registro regionale del segreto di pull dell'immagine con il nome dominio che hai specificato nell'immagine del contenitore. Per impostazione predefinita, i nuovi cluster hanno dei segreti di pull dell'immagine per ogni nome dominio del registro regionale per i contenitori che vengono eseguiti nello spazio dei nomi Kubernetes `default`. Tuttavia, se hai modificato le impostazioni predefinite o se stai utilizzando uno spazio dei nomi Kubernetes differente, potresti non avere un segreto di pull dell'immagine per il registro regionale. [Copia un segreto di pull dell'immagine](/docs/containers?topic=containers-images#copy_imagePullSecret) per il nome dominio del registro regionale.
    5.  Esegui l'accesso al registro dalla tua macchina locale utilizzando `username` e `password` dal tuo segreto di pull dell'immagine. Se non riesci a eseguire l'accesso, potresti dover correggere l'ID servizio.
        ```
        docker login -u iamapikey -p <password_string> <region>.icr.io
        ```
        {: pre}
        1.  Crea nuovamente l'ID servizio del cluster, le politiche {{site.data.keyword.Bluemix_notm}} IAM, la chiave API e i segreti di pull dell'immagine per i contenitori che vengono eseguiti nello spazio dei nomi Kubernetes `default`.
            ```
            ibmcloud ks cluster-pull-secret-apply --cluster <cluster_name_or_ID>
            ```
            {: pre}
        2.  Crea nuovamente la tua distribuzione nello spazio dei nomi Kubernetes `default`. Se continui a vedere un messaggio di errore di autorizzazione, ripeti i passi da 1 a 5 con i nuovi segreti di pull dell'immagine. Se ancora non riesci ad eseguire l'accesso, [contatta il team IBM su Slack oppure apri un caso di supporto {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    6.  Se l'accesso riesce, esegui il pull di un'immagine localmente. Se il comando non riesce con un errore `access denied`, l'account del registro si trova in un account {{site.data.keyword.Bluemix_notm}} differente da quello in cui si trova il tuo cluster. [Crea un segreto di pull dell'immagine per accedere alle immagini nell'altro account](/docs/containers?topic=containers-images#other_registry_accounts). Se il tuo comando ha esito positivo, [contatta il team IBM su Slack oppure apri un caso di supporto {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
        ```
        docker pull <region>icr.io/<namespace>/<image>:<tag>
        ```
        {: pre}

<br>
<br>

**Obsoleto: Risoluzione dei problemi dei segreti di pull dell'immagine che utilizzano i token**</br>
{: #ts_image_pull_token}

Se la tua configurazione di pod ha un segreto di pull dell'immagine che utilizza un token, controlla che le credenziali del token siano valide.
{: shortdesc}

Questo metodo di utilizzo di un token per autorizzare il cluster ad accedere a {{site.data.keyword.registrylong_notm}} è supportato per i nomi di dominio `registry.bluemix.net` ma è obsoleto. Invece, [utilizza il metodo della chiave API](/docs/containers?topic=containers-images#cluster_registry_auth) per autorizzare il cluster ad accedere ai nuovi nomi di dominio del registro `icr.io`.
{: deprecated}

1.  Ottieni la configurazione del segreto di pull dell'immagine. Se il pod non si trova nello spazio dei nomi `default`, includi l'indicatore `-n`.
    ```
    kubectl get secret <image_pull_secret_name> -o yaml [-n <namespace>]
    ```
    {: pre}
2.  Nell'output, copia il valore con codifica base64 del campo `.dockercfg`.
        ```
    apiVersion: v1
    kind: Secret
    data:
      .dockercfg: eyJyZWdp...==
    ...
    ```
    {: screen}
3.  Decodifica la stringa base64. Ad esempio, su OS X puoi eseguire il seguente comando.
    ```
    echo -n "<base64_string>" | base64 --decode
    ```
    {: pre}

    Output di esempio:
    ```
    {"auths":{"registry.<region>.bluemix.net":{"username":"token","password":"<password_string>","email":"<name@abc.com>","auth":"<auth_string>"}}}
    ```
    {: screen}
4.  Confronta il nome dominio del registro con il nome dominio che hai specificato nell'immagine del contenitore. Ad esempio, se il segreto di pull dell'immagine autorizza l'accesso al dominio `registry.ng.bluemix.net` ma hai specificato un'immagine che è archiviata in `registry.eu-de.bluemix.net`, devi [creare un token da utilizzare in un segreto di pull dell'immagine ](/docs/containers?topic=containers-images#token_other_regions_accounts) per `registry.eu-de.bluemix.net`.
5.  Esegui l'accesso al registro dalla tua macchina locale utilizzando `username` e `password` dal segreto di pull dell'immagine. Se non puoi eseguire l'accesso, il token presenta un problema che non puoi risolvere. [Contatta il team di supporto su Slack oppure apri un caso di supporto {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
    ```
    docker login -u token -p <password_string> registry.<region>.bluemix.net
    ```
    {: pre}
6.  Se l'accesso riesce, esegui il pull di un'immagine localmente. Se il comando non riesce con un errore `access denied`, l'account del registro si trova in un
account {{site.data.keyword.Bluemix_notm}} differente da quello in cui si trova il tuo cluster. [Crea un segreto di pull dell'immagine per accedere alle immagini nell'altro account](/docs/containers?topic=containers-images#token_other_regions_accounts). Se il tuo comando ha esito positivo, [contatta il team IBM su Slack oppure apri un caso di supporto {{site.data.keyword.Bluemix_notm}}](#clusters_getting_help).
        ```
    docker pull registry.<region>.bluemix.net/<namespace>/<image>:<tag>
    ```
    {: pre}

<br />


## I pod rimangono nello stato in sospeso
{: #cs_pods_pending}

{: tsSymptoms}
Quando esegui `kubectl get pods`, puoi visualizzare i pod
che rimangono in uno stato **In sospeso**.

{: tsCauses}
Se hai appena creato il cluster Kubernetes, i nodi di lavoro potrebbero essere ancora in fase di configurazione.

Se questo cluster è un cluster che già esiste:
*  Potresti non disporre di sufficiente capacità nel tuo cluster per distribuire il pod.
*  Il pod potrebbe aver superato una richiesta o un limite di risorse.

{: tsResolve}
Questa attività richiede il ruolo della piattaforma {{site.data.keyword.Bluemix_notm}} IAM di [**Amministratore**](/docs/containers?topic=containers-users#platform) per il cluster e il ruolo del servizio di [**Gestore**](/docs/containers?topic=containers-users#platform) per tutti gli spazi dei nomi.

Se hai appena creato il cluster Kubernetes, immetti il seguente comando e attendi che il nodo
di lavoro venga inizializzato.

```
kubectl get nodes
```
{: pre}

Se questo cluster è un cluster che già esiste, controlla la capacità del tuo cluster.

1.  Imposta il proxy con il numero di porta predefinito.

  ```
  kubectl proxy
  ```
   {: pre}

2.  Apri il dashboard Kubernetes.

  ```
  http://localhost:8001/ui
  ```
  {: pre}

3.  Controlla di avere abbastanza capacità nel tuo cluster per distribuire il tuo pod.

4.  Se non hai abbastanza capacità nel tuo cluster, ridimensiona il tuo pool di nodi di lavoro per aggiungere ulteriori nodi.

    1.  Esamina le dimensioni e i tipi di macchina attuali dei tuoi pool di nodi di lavoro per decidere quale ridimensionare.

        ```
        ibmcloud ks worker-pools
        ```
        {: pre}

    2.  Ridimensiona i tuoi pool di nodi di lavoro per aggiungere ulteriori nodi a ciascuna zona a cui si estende il pool.

        ```
        ibmcloud ks worker-pool-resize <worker_pool> --cluster <cluster_name_or_ID> --size-per-zone <workers_per_zone>
        ```
        {: pre}

5.  Facoltativo: controlla le tue richieste di risorse del pod.

    1.  Conferma che i valori `resources.requests` non siano superiori alla capacità del nodo di lavoro. Ad esempio, se la richiesta del pod è `cpu: 4000m`, oppure 4 core, ma la dimensione del nodo di lavoro è solo di 2 core, il pod non può essere distribuito.

        ```
        kubectl get pod <pod_name> -o yaml
        ```
        {: pre}

    2.  Se la richiesta supera la capacità disponibile, [aggiungi un nuovo pool di nodi di lavoro](/docs/containers?topic=containers-clusters#add_pool) con i nodi di lavoro che possono soddisfare la richiesta.

6.  Se i tuoi pod sono ancora nello stato **in attesa** dopo che il nodo di lavoro è stato completamente distribuito,
controlla la [ Documentazione Kubernetes
![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://kubernetes.io/docs/tasks/debug-application-cluster/debug-pod-replication-controller/#my-pod-stays-pending) per ulteriore risoluzione dei problemi sullo stato in attesa del tuo pod.

<br />


## I contenitori non si avviano
{: #containers_do_not_start}

{: tsSymptoms}
I pod sono stati distribuiti correttamente ai cluster, ma i contenitori non sono avviati.

{: tsCauses}
I contenitori potrebbero non essere stati avviati quando è stata raggiunta la quota del registro.

{: tsResolve}
[Libera archiviazione in {{site.data.keyword.registryshort_notm}}.](/docs/services/Registry?topic=registry-registry_quota#registry_quota_freeup)

<br />


## Continui errori di riavvio o rimozione imprevista dei pod
{: #pods_fail}

{: tsSymptoms}
Il tuo pod era integro ma viene rimosso in modo imprevisto o rimane bloccato in un loop di riavvio.

{: tsCauses}
I tuoi contenitori potrebbero superare i limiti di risorsa o i tuoi pod potrebbero essere sostituiti da pod con priorità più alta.

{: tsResolve}
Per vedere se un contenitore viene arrestato a causa di un limite di risorsa:
<ol><li>Richiama il nome del tuo pod. Se hai utilizzato un'etichetta, puoi includerla per filtrare i risultati.<pre class="pre"><code>kubectl get pods --selector='app=wasliberty'</code></pre></li>
<li>Descrivi il pod e cerca la sezione **Restart Count**.<pre class="pre"><code>kubectl describe pod</code></pre></li>
<li>Se il pod è stato riavviato più volte in un breve periodo di tempo, recupera il suo stato. <pre class="pre"><code>kubectl get pod -o go-template={{range.status.containerStatuses}}{{"Container Name: "}}{{.name}}{{"\r\nLastState: "}}{{.lastState}}{{end}}</code></pre></li>
<li>Esamina il motivo. Ad esempio, `OOM Killed` significa "memoria insufficiente", il che indica che il contenitore si arresta in modo anomalo a causa di un limite di risorse.</li>
<li>Aggiungi capacità al tuo cluster in modo che le risorse possano essere soddisfatte.</li></ol>

<br>

Per vedere se il tuo pod viene sostituito da pod con priorità più alta:
1.  Richiama il nome del tuo pod.

  ```
  kubectl get pods
  ```
  {: pre}

2.  Descrivi lo YAML del pod: `kubectl get pod <pod_name> -o yaml`

3.  Controlla il campo `priorityClassName`.

    1.  Se non c'è alcun valore di campo `priorityClassName`, il tuo pod ha la classe di priorità `globalDefault`. Se il tuo amministratore cluster non ha impostato una classe di priorità `globalDefault`, il valore predefinito è zero (0) o la priorità più bassa. Qualsiasi pod con una classe di priorità più alta può prevenire o rimuovere il tuo pod.
    2.  Se c'è un valore di campo `priorityClassName`, ottieni la classe di priorità `kubectl get priorityclass <priority_class_name> -o yaml`
    3.  Prendi nota del campo `value` per controllare la priorità del tuo pod.

4.  Elenca le classi di priorità esistenti nel cluster.

  ```
  kubectl get priorityclasses
  ```
  {: pre}

5.  Per ogni classe di priorità, richiama il file YAML e prendi nota del campo `value`.

  ```
  kubectl get priorityclass <priority_class_name> -o yaml
  ```
  {: pre}

6.  Confronta il valore della classe di priorità del tuo pod con gli altri valori della classe di priorità per verificare se ha priorità superiore o inferiore.

7.  Ripeti i passi da 1 a 3 per gli altri pod nel cluster, per verificare quale classe di priorità stanno utilizzando. Se la classe di priorità di questi altri pod è superiore a quella del tuo pod, il pod non viene fornito a meno che non vi siano risorse sufficienti per il tuo pod e per ogni pod con priorità più alta.

8.  Contatta il tuo amministratore cluster per aggiungere ulteriore capacità al tuo cluster e confermare che vengano assegnate le classi di priorità corrette.

<br />


## Impossibile installare un grafico Helm con valori di configurazione aggiornati
{: #cs_helm_install}

{: tsSymptoms}
Quando tenti di installare un grafico Helm aggiornato eseguendo `helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>`, ricevi il messaggio di errore `Error: failed to download "ibm/<chart_name>"`.

{: tsCauses}
L'URL per il repository {{site.data.keyword.Bluemix_notm}} nella tua istanza Helm potrebbe non essere corretto.

{: tsResolve}
Per risolvere i problemi relativi al tuo grafico Helm:

1. Elenca i repository attualmente disponibili nella tua istanza Helm.

    ```
    helm repo list
    ```
    {: pre}

2. Nell'output, verifica che l'URL per il repository {{site.data.keyword.Bluemix_notm}}, `ibm`, sia `https://registry.bluemix.net/helm/ibm`.

    ```
    NAME    URL
    stable  https://kubernetes-charts.storage.googleapis.com
    local   http://127.0.0.1:8888/charts
    ibm     https://registry.bluemix.net/helm/ibm
    ```
    {: screen}

    * Se l'URL non è corretto:

        1. Rimuovi il repository {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo remove ibm
            ```
            {: pre}

        2. Aggiungi di nuovo il repository {{site.data.keyword.Bluemix_notm}}.

            ```
            helm repo add ibm  https://registry.bluemix.net/helm/ibm
            ```
            {: pre}

    * Se l'URL è corretto, ottieni gli ultimi aggiornamenti dal repository.

        ```
        helm repo update
        ```
        {: pre}

3. Installa il grafico Helm con i tuoi aggiornamenti.

    ```
    helm install -f config.yaml --namespace=kube-system --name=<release_name> ibm/<chart_name>
    ```
    {: pre}

<br />


## Non è possibile installare il tiller Helm o distribuire contenitori dalle immagini pubbliche nel mio cluster
{: #cs_tiller_install}

{: tsSymptoms}

Quando provi a installare il tiller Helm o vuoi distribuire immagini da registri pubblici, come ad esempio Dockerhub, l'installazione non riesce con un errore simile al seguente:

```
Failed to pull image "gcr.io/kubernetes-helm/tiller:v2.12.0": rpc error: code = Unknown desc = failed to resolve image "gcr.io/kubernetes-helm/tiller:v2.12.0": no available registry endpoint:
```
{: screen}

{: tsCauses}
È possibile che tu abbia configurato un firewall personalizzato, specificato delle politiche Calico personalizzate o creato un cluster solo privato utilizzando l'endpoint del servizio privato che blocca la connettività di rete pubblica al registro del contenitore in cui è memorizzata l'immagine.

{: tsResolve}
- Se hai un firewall personalizzato o hai impostato delle politiche Calico personalizzate, consenti il traffico di rete in uscita e in entrata tra i nodi di lavoro e il registro del contenitore in cui è memorizzata l'immagine. Se l'immagine è memorizzata in {{site.data.keyword.registryshort_notm}}, esamina le porte richieste in [Consentire al cluster di accedere alle risorse dell'infrastruttura e ad altri servizi](/docs/containers?topic=containers-firewall#firewall_outbound).
- Se hai creato un cluster privato abilitando solo l'endpoint del servizio privato, puoi [abilitare l'endpoint del servizio pubblico](/docs/containers?topic=containers-cs_cli_reference#cs_cluster_feature_disable) per il tuo cluster. Se vuoi installare i grafici Helm in un cluster privato senza aprire una connessione pubblica, puoi installare Helm [con Tiller](/docs/containers?topic=containers-integrations#private_local_tiller) o [senza Tiller](/docs/containers?topic=containers-integrations#private_install_without_tiller).

<br />


## Come ottenere aiuto e supporto
{: #clusters_getting_help}

Stai ancora avendo problemi con il tuo cluster?
{: shortdesc}

-  Nel terminale, ricevi una notifica quando sono disponibili degli aggiornamenti ai plugin e alla CLI `ibmcloud`. Assicurati di mantenere la tua CLI aggiornata in modo che tu possa utilizzare tutti i comandi e gli indicatori disponibili.
-   Per vedere se {{site.data.keyword.Bluemix_notm}} è disponibile, [controlla la pagina sugli stati {{site.data.keyword.Bluemix_notm}} ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://cloud.ibm.com/status?selected=status).
-   Pubblica una domanda in [{{site.data.keyword.containerlong_notm}} Slack ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://ibm-container-service.slack.com).
    Se non stai utilizzando un ID IBM per il tuo account {{site.data.keyword.Bluemix_notm}}, [richiedi un invito](https://bxcs-slack-invite.mybluemix.net/) a questo Slack.
    {: tip}
-   Rivedi i forum per controllare se altri utenti hanno riscontrato gli stessi problemi. Quando utilizzi i forum per fare una domanda, contrassegna con una tag la tua domanda in modo che sia visualizzabile dai team di sviluppo {{site.data.keyword.Bluemix_notm}}.
    -   Se hai domande tecniche sullo sviluppo o la distribuzione di cluster o applicazioni con
{{site.data.keyword.containerlong_notm}}, inserisci la tua domanda in
[Stack Overflow ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://stackoverflow.com/questions/tagged/ibm-cloud+containers) e contrassegnala con le tag `ibm-cloud`, `kubernetes` e `containers`.
    -   Per domande sul servizio e istruzioni introduttive, utilizza il forum
[IBM Developer Answers ![Icona link esterno](../icons/launch-glyph.svg "Icona link esterno")](https://developer.ibm.com/answers/topics/containers/?smartspace=bluemix). Includi le tag `ibm-cloud`
e `containers`.
    Consulta [Come ottenere supporto](/docs/get-support?topic=get-support-getting-customer-support#using-avatar) per ulteriori dettagli sull'utilizzo dei forum.
-   Contatta il supporto IBM aprendo un caso. Per informazioni su come aprire un caso di supporto IBM o sui livelli di supporto e sulla gravità dei casi, consulta [Come contattare il supporto](/docs/get-support?topic=get-support-getting-customer-support#getting-customer-support).
Quando riporti un problema, includi il tuo ID del cluster. Per ottenere il tuo ID del cluster, esegui `ibmcloud ks clusters`. Puoi anche utilizzare il [{{site.data.keyword.containerlong_notm}} Diagnostics and Debug Tool](/docs/containers?topic=containers-cs_troubleshoot#debug_utility) per raccogliere ed esportare informazioni pertinenti dal tuo cluster da condividere con il supporto IBM.
{: tip}
