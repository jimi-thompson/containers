---

copyright:
  years: 2014, 2019
lastupdated: "2019-03-21"

keywords: kubernetes, iks, ibmcloud, ic, ks, kubectl

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




# Configuración de la CLI y la API
{: #cs_cli_install}

Puede utilizar la CLI o la API de {{site.data.keyword.containerlong}} para crear y gestionar sus clústeres de Kubernetes.
{:shortdesc}

## Instalación de la CLI
{: #cs_cli_install_steps}

Instale las CLI necesarias para crear y gestionar sus clústeres de Kubernetes en {{site.data.keyword.containerlong_notm}} y para desplegar apps contenerizadas en el clúster.
{:shortdesc}

Esta tarea incluye la información para instalar estas CLI y plugins:

-   CLI de {{site.data.keyword.Bluemix_notm}} versión 0.8.0 o posterior
-   Plugin de {{site.data.keyword.containerlong_notm}}
-   Versión de CLI de Kubernetes que coincida con la versión `major.minor` de su clúster
-   Opcional: plugin {{site.data.keyword.registryshort_notm}}



<br>
Para instalar las CLI:

1.  Un requisito previo del plugin de {{site.data.keyword.containerlong_notm}} es que instale la [CLI de {{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli). El prefijo para ejecutar mandatos mediante la CLI de {{site.data.keyword.Bluemix_notm}} es `ibmcloud`.

    ¿Tiene pensado utilizar mucho la CLI? Pruebe [Habilitación del rellenado automático de shell para la CLI de {{site.data.keyword.Bluemix_notm}} (solo Linux/MacOS)](/docs/cli/reference/ibmcloud?topic=cloud-cli-shell-autocomplete#shell-autocomplete-linux).
    {: tip}

2.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite.

    ```
    ibmcloud login
    ```
    {: pre}

    Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
    {: tip}

3.  Para crear clústeres de Kubernetes y gestionar nodos trabajadores, instale el plugin {{site.data.keyword.containerlong_notm}}. El prefijo para ejecutar mandatos mediante los plugins de {{site.data.keyword.containerlong_notm}} es `ibmcloud ks`.

    ```
    ibmcloud plugin install container-service
    ```
    {: pre}

    Para comprobar que el plugin se ha instalado correctamente, ejecute el siguiente mandato:

    ```
    ibmcloud plugin list
    ```
    {: pre}

    El plugin {{site.data.keyword.containerlong_notm}} se visualiza en los resultados como **container-service**.

4.  {: #kubectl}Para ver una versión local del panel de control Kubernetes y desplegar apps en los clústeres, [instale la CLI de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/tasks/tools/install-kubectl/). El prefijo para ejecutar mandatos utilizando la CLI de Kubernetes es `kubectl`.

    1.  Descargue la versión `major.minor` de la CLI de Kubernetes que coincida con la versión `major.minor` del clúster de Kubernetes que piensa utilizar. La versión de Kubernetes predeterminada actual de {{site.data.keyword.containerlong_notm}} es la 1.12.6.

        Si utiliza una versión de CLI `kubectl` que no coincide al menos con la versión `major.minor` de los clústeres, puede experimentar resultados inesperados. Mantenga actualizadas las versiones de la CLI y el clúster de Kubernetes.
        {: note}

        - **OS X**:   [https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/darwin/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/darwin/amd64/kubectl)
        - **Linux**:   [https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/linux/amd64/kubectl ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/linux/amd64/kubectl)
        - **Windows**:    [https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/windows/amd64/kubectl.exe ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://storage.googleapis.com/kubernetes-release/release/v1.12.6/bin/windows/amd64/kubectl.exe)

    2.  **Para OS X y Linux**: Complete los pasos siguientes.
        1.  Mueva el archivo ejecutable al directorio `/usr/local/bin`.

            ```
            mv /filepath/kubectl /usr/local/bin/kubectl
            ```
            {: pre}

        2.  Asegúrese de que `/usr/local/bin` aparezca en la variable del sistema `PATH`. La variable `PATH` contiene todos los directorios en los que el sistema operativo puede encontrar archivos ejecutables. Los directorios que se muestran en la variable `PATH` sirven para distintos propósitos. `/usr/local/bin` se utiliza para guardar archivos ejecutables correspondientes a software que no forma parte del sistema operativo y que ha instalado manualmente el administrador del sistema.

            ```
            echo $PATH
            ```
            {: pre}

            Ejemplo de salida de CLI:

            ```
            /usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
            ```
            {: screen}

        3.  Convierta el archivo en ejecutable.

            ```
            chmod +x /usr/local/bin/kubectl
            ```
            {: pre}

    3.  **Para Windows**: Instale la CLI de Kubernetes en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}. Esta configuración le ahorra algunos cambios en la vía de acceso a archivo cuando ejecute mandatos posteriormente.

5.  Para gestionar un repositorio de imágenes privadas, instale el plugin {{site.data.keyword.registryshort_notm}}. Utilice este plugin para configurar su propio espacio de nombres en un registro privado de imágenes multiarrendatario, de alta disponibilidad y escalable alojado por IBM, y para almacenar y compartir imágenes de Docker con otros usuarios. Las imágenes de Docker son necesarias para desplegar contenedores en un clúster. El prefijo para ejecutar mandatos de registro es `ibmcloud cr`.

    ```
    ibmcloud plugin install container-registry 
    ```
    {: pre}

    Para comprobar que el plugin se ha instalado correctamente, ejecute el siguiente mandato:

    ```
    ibmcloud plugin list
    ```
    {: pre}

    El plugin se muestra en los resultados como registro de contenedor.

A continuación, empiece a [crear clústeres de Kubernetes desde la CLI con {{site.data.keyword.containerlong_notm}}](/docs/containers?topic=containers-clusters#clusters_cli).

Para obtener información acerca de estas CLI, consulte la documentación de dichas herramientas.

-   [Mandatos `ibmcloud`](/docs/cli/reference/ibmcloud?topic=cloud-cli-ibmcloud_cli#ibmcloud_cli)
-   [Mandatos `ibmcloud ks`](/docs/containers?topic=containers-cs_cli_reference#cs_cli_reference)
-   [Mandatos `kubectl`![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/)
-   [Mandatos `ibmcloud cr`](/docs/services/Registry?topic=registry-registry_cli_reference#registry_cli_reference)

<br />




## Ejecución de la CLI en un contenedor en su sistema
{: #cs_cli_container}

En lugar de instalar cada CLI individualmente en el sistema, puede instalar las CLI en un contenedor que se ejecute en el sistema.
{:shortdesc}

Antes de empezar, [instale Docker ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://www.docker.com/community-edition#/download) para crear y ejecutar imágenes localmente. Si está utilizando Windows 8 o anterior, puede instalar [Docker Toolbox ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://docs.docker.com/toolbox/toolbox_install_windows/) en su lugar.

1. Cree una imagen del Dockerfile proporcionado.

    ```
    docker build -t <image_name> https://raw.githubusercontent.com/IBM-Cloud/kube-samples/master/install-clis-container/Dockerfile
    ```
    {: pre}

2. Despliegue la imagen localmente como un contenedor y monte un volumen para acceder a los archivos locales.

    ```
    docker run -it -v /local/path:/container/volume <image_name>
    ```
    {: pre}

3. Empiece ejecutando los mandatos `ibmcloud ks` y `kubectl` desde el shell interactivo. Si crea datos que desee guardar, guarde los datos en el volumen que desea montar. Cuando salga del shell, el contenedor se detiene.

<br />



## Configuración de la CLI para que ejecute mandatos `kubectl`
{: #cs_cli_configure}

Puede utilizar los mandatos que se proporcionan con la CLI de Kubernetes para gestionar clústeres en {{site.data.keyword.Bluemix_notm}}.
{:shortdesc}

Todos los mandatos `kubectl` disponibles en Kubernetes 1.12.6 se pueden utilizar con clústeres en {{site.data.keyword.Bluemix_notm}}. Después de crear un clúster, establezca el contexto de la CLI local para dicho clúster con una variable de entorno. A continuación, puede ejecutar el mandato de Kubernetes `kubectl` para trabajar con el clúster en {{site.data.keyword.Bluemix_notm}}.

Para poder ejecutar mandatos `kubectl`:
* [Instale las CLI necesarias](#cs_cli_install).
* [Cree un clúster](/docs/containers?topic=containers-clusters#clusters_cli).
* Asegúrese de que tiene un [rol de servicio](/docs/containers?topic=containers-users#platform) que otorgue el rol de RBAC de Kubernetes adecuado para que pueda trabajar con los recursos de Kubernetes. Si solo tiene un rol de servicio pero no tiene un rol de plataforma, necesita que el administrador del clúster le dé el nombre y el ID del clúster o el rol de plataforma de **Visor** para ver una lista de clústeres.

Para utilizar mandatos `kubectl`:

1.  Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite. Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [incluya el punto final de API](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).

    ```
    ibmcloud login
    ```
    {: pre}

    Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
    {: tip}

2.  Seleccione una cuenta de {{site.data.keyword.Bluemix_notm}}. Si se le ha asignado a varias organizaciones de {{site.data.keyword.Bluemix_notm}}, seleccione la
organización en la que se ha creado el clúster. Los clústeres son específicos de una
organización, pero son independientes de un espacio de {{site.data.keyword.Bluemix_notm}}. Por lo tanto, no es necesario que seleccione un espacio.

3.  Para crear y trabajar con clústeres en un grupo de recursos que no sea el predeterminado, seleccione como destino dicho grupo de recursos. Para ver el grupo de recursos al que pertenece cada clúster, ejecute `ibmcloud ks clusters`. **Nota**: debe tener [acceso de **Visor**](/docs/containers?topic=containers-users#platform) sobre el grupo de recursos.
    ```
    ibmcloud target -g <resource_group_name>
    ```
    {: pre}

4.  Para crear o acceder al clúster de Kubernetes en una región distinta a la región de
{{site.data.keyword.Bluemix_notm}} que ha seleccionado anteriormente, utilice la región como objetivo.
    ```
    ibmcloud ks region-set
    ```
    {: pre}

5.  Obtenga una lista de todos los clústeres de la cuenta para obtener el nombre del clúster. Si solo tiene un rol de servicio de {{site.data.keyword.Bluemix_notm}} IAM y no puede ver clústeres, solicite al administrador del clúster el rol de **Visor** de
plataforma de IAM o bien el nombre y el ID del clúster.

    ```
    ibmcloud ks clusters
    ```
    {: pre}

6.  Defina el clúster que ha creado como contexto para esta sesión. Siga estos pasos de configuración cada vez que de trabaje con el clúster.
    1.  Obtenga el mandato para establecer la variable de entorno y descargar los archivos de configuración de Kubernetes.

        ```
        ibmcloud ks cluster-config <cluster_name_or_ID>
        ```
        {: pre}

        Después de descargar los archivos de configuración, se muestra un mandato que puede utilizar para establecer la vía de acceso al archivo de configuración de
Kubernetes local como variable de entorno.

        Ejemplo:

        ```
        export KUBECONFIG=/Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

    2.  Copie y pegue el mandato que se muestra en el terminal para definir la variable de entorno `KUBECONFIG`.

        **Usuarios de Mac o Linux**: en lugar de ejecutar el mandato `ibmcloud ks cluster-config` y copiar la variable de entorno `KUBECONFIG`, puede ejecutar `ibmcloud ks cluster-config --export <cluster-name>`. En función de su shell, puede configurar el shell ejecutando `eval $(ibmcloud ks cluster-config --export <cluster-name>)`.
        {: tip}

        **Usuarios de PowerShell de Windows**: En lugar de copiar y pegar el mandato `SET` de la salida de `ibmcloud ks cluster-config`, debe establecer la variable de entorno `KUBECONFIG` ejecutando, por ejemplo, `$env:KUBECONFIG = "C:\Users\<user_name>\.bluemix\plugins\container-service\clusters\mycluster\kube-config-prod-dal10-mycluster.yml"`.
        {:tip}

    3.  Compruebe que la variable de entorno `KUBECONFIG` se haya establecido correctamente.

        Ejemplo:

        ```
        echo $KUBECONFIG
        ```
        {: pre}

        Salida:
        ```
        /Users/<user_name>/.bluemix/plugins/container-service/clusters/mycluster/kube-config-prod-dal10-mycluster.yml
        ```
        {: screen}

7.  Compruebe que los mandatos `kubectl` se ejecutan correctamente con el clúster comprobando la versión del cliente de la CLI de Kubernetes.

    ```
    kubectl version  --short
    ```
    {: pre}

    Salida de ejemplo:

    ```
    Client Version: v1.12.6
    Server Version: v1.12.6
    ```
    {: screen}

Ahora puede ejecutar mandatos `kubectl` para gestionar sus clústeres en {{site.data.keyword.Bluemix_notm}}. Para ver una lista completa de mandatos, consulte la [documentación de Kubernetes ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://kubernetes.io/docs/reference/kubectl/overview/).

**Sugerencia:** Si utiliza Windows y la CLI de Kubernetes no está instalada en el mismo directorio que la CLI de {{site.data.keyword.Bluemix_notm}}, debe cambiar los directorios por la vía de acceso donde está instalada la CLI de Kubernetes para poder ejecutar correctamente los mandatos `kubectl`.


<br />


## Actualización de la CLI
{: #cs_cli_upgrade}

Quizás desee actualizar las CLI periódicamente para usar nuevas funciones.
{:shortdesc}

Esta tarea incluye la información para actualizar estas CLI.

-   CLI de {{site.data.keyword.Bluemix_notm}} versión 0.8.0 o posterior
-   Plugin de {{site.data.keyword.containerlong_notm}}
-   CLI de Kubernetes versión 1.12.6 o posterior
-   Plugin de {{site.data.keyword.registryshort_notm}}

<br>
Para actualizar las CLI:

1.  Actualice la CLI de {{site.data.keyword.Bluemix_notm}}. Descargue la [versión más reciente ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](/docs/cli?topic=cloud-cli-ibmcloud-cli) y ejecute el instalador.

2. Inicie la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Escriba
sus credenciales de {{site.data.keyword.Bluemix_notm}} cuando se le solicite. Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [incluya el punto final de API](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).

    ```
    ibmcloud login
    ```
    {: pre}

     Si tiene un ID federado, utilice `ibmcloud login --sso` para iniciar la sesión en la CLI de {{site.data.keyword.Bluemix_notm}}. Especifique su nombre de usuario y utilice el URL proporcionado en la salida de la CLI para recuperar el código de acceso de un solo uso. Sabe tiene un ID federado cuando el inicio de sesión falla sin el `--sso` y se lleva a cabo correctamente con la opción `--sso`.
     {: tip}

3.  Actualice el plugin de {{site.data.keyword.containerlong_notm}}.
    1.  Instale la actualización desde el repositorio del plugin de {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-service 
        ```
        {: pre}

    2.  Para verificar la instalación del plugin, ejecute el mandato siguiente
y compruebe la lista de plugins instalados.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        El plugin {{site.data.keyword.containerlong_notm}} se visualiza en los resultados como container-service.

    3.  Inicialice la CLI.

        ```
        ibmcloud ks init
        ```
        {: pre}

4.  [Actualice la CLI de Kubernetes](#kubectl).

5.  Actualice el plugin de {{site.data.keyword.registryshort_notm}}.
    1.  Instale la actualización desde el repositorio del plugin de {{site.data.keyword.Bluemix_notm}}.

        ```
        ibmcloud plugin update container-registry 
        ```
        {: pre}

    2.  Para verificar la instalación del plugin, ejecute el mandato siguiente
y compruebe la lista de plugins instalados.

        ```
        ibmcloud plugin list
        ```
        {: pre}

        El plugin de registro se muestra en los resultados como container-registry.

<br />


## Desinstalación de la CLI
{: #cs_cli_uninstall}

Si ya no necesita una CLI, puede desinstalarla.
{:shortdesc}

Esta tarea incluye la información para eliminar estas CLI:


-   Plugin de {{site.data.keyword.containerlong_notm}}
-   CLI de Kubernetes
-   Plugin de {{site.data.keyword.registryshort_notm}}

Para desinstalar las CLI:

1.  Desinstalar el plugin de {{site.data.keyword.containerlong_notm}}.

    ```
    ibmcloud plugin uninstall container-service
    ```
    {: pre}

2.  Desinstalar el plugin de {{site.data.keyword.registryshort_notm}}.

    ```
    ibmcloud plugin uninstall container-registry
    ```
    {: pre}

3.  Para verificar la desinstalación de los plugins, ejecute el mandato siguiente y compruebe la lista de plugins de instalados.

    ```
    ibmcloud plugin list
    ```
    {: pre}

    El servicio de contenedor y el plugin de servicio de contenedor no se muestran en los resultados.

<br />




## Automatización de despliegues de clústeres con la API
{: #cs_api}

Puede utilizar la API de {{site.data.keyword.containerlong_notm}} para automatizar la creación, el despliegue y la gestión de clústeres de Kubernetes.
{:shortdesc}

La API de {{site.data.keyword.containerlong_notm}} precisa de información de cabecera que debe proporcionar en su solicitud de la API y que depende del tipo de API utilizado. Para determinar la información de cabecera que se necesita para la API, consulte la [documentación de la API de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://us-south.containers.cloud.ibm.com/swagger-api).

Para autenticarse con {{site.data.keyword.containerlong_notm}}, debe especificar su señal de {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) que se genera con las credenciales de {{site.data.keyword.Bluemix_notm}} y que incluye el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en la que se ha creado el clúster. Dependiendo de la forma en que se autentique con {{site.data.keyword.Bluemix_notm}}, puede elegir entre las siguientes opciones para automatizar la creación de la señal de {{site.data.keyword.Bluemix_notm}} IAM.

También puede utilizar el [archivo JSON swagger de API ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/swagger-api-json) para generar un cliente que pueda interactuar con la API como parte del trabajo de automatización.
{: tip}

<table>
<caption>Opciones y tipos de ID</caption>
<thead>
<th>ID de {{site.data.keyword.Bluemix_notm}}</th>
<th>Mis opciones</th>
</thead>
<tbody>
<tr>
<td>ID no federado</td>
<td><ul><li>Nombre de usuario y contraseña de <strong>{{site.data.keyword.Bluemix_notm}}:</strong> Puede seguir los pasos de este tema para automatizar por completo la creación de la señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM.</li>
<li><strong>Generar una clave de API de {{site.data.keyword.Bluemix_notm}}:</strong> Como alternativa a utilizar el nombre de usuario y la contraseña de {{site.data.keyword.Bluemix_notm}}, puede <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">utilizar claves de API de {{site.data.keyword.Bluemix_notm}}</a>. Las claves de API de {{site.data.keyword.Bluemix_notm}} dependen de la cuenta de {{site.data.keyword.Bluemix_notm}} para la que se generan. No puede combinar su clave de API de {{site.data.keyword.Bluemix_notm}} con otro ID de cuenta en la misma señal de {{site.data.keyword.Bluemix_notm}} IAM. Para acceder a los clústeres que se han creado con una cuenta distinta de aquella en la que se basa la clave de API de {{site.data.keyword.Bluemix_notm}}, debe iniciar una sesión en la cuenta para generar una nueva clave de API. </li></ul></tr>
<tr>
<td>ID federado</td>
<td><ul><li><strong>Generar una clave de API de {{site.data.keyword.Bluemix_notm}}: </strong> <a href="/docs/iam?topic=iam-userapikey#create_user_key" target="_blank">Las claves de API de {{site.data.keyword.Bluemix_notm}}</a> dependen de la cuenta de {{site.data.keyword.Bluemix_notm}} para la que se generan. No puede combinar su clave de API de {{site.data.keyword.Bluemix_notm}} con otro ID de cuenta en la misma señal de {{site.data.keyword.Bluemix_notm}} IAM. Para acceder a los clústeres que se han creado con una cuenta distinta de aquella en la que se basa la clave de API de {{site.data.keyword.Bluemix_notm}}, debe iniciar una sesión en la cuenta para generar una nueva clave de API. </li><li><strong>Utilizar un código de acceso puntual: </strong> Si se autentica con {{site.data.keyword.Bluemix_notm}} mediante un código de acceso puntual, no puede automatizar completamente la creación de la señal de {{site.data.keyword.Bluemix_notm}} IAM porque la recuperación del código de acceso puntual requiere una interacción manual con el navegador web. Para automatizar por completo la creación de la señal de {{site.data.keyword.Bluemix_notm}} IAM, debe crear en su lugar una clave de API de {{site.data.keyword.Bluemix_notm}}. </ul></td>
</tr>
</tbody>
</table>

1.  Cree la señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM. La información del cuerpo que se incluye en la solicitud varía en función del método de autenticación de {{site.data.keyword.Bluemix_notm}} que utilice. Sustituya los valores siguientes:
  - `<username>`: Su nombre de usuario de {{site.data.keyword.Bluemix_notm}}.
  - `<password>`: Su contraseña de {{site.data.keyword.Bluemix_notm}}.
  - `<api_key>`: Su clave de API de {{site.data.keyword.Bluemix_notm}}.
  - `<passcode>`: Su código de acceso de uso único de {{site.data.keyword.Bluemix_notm}}. Ejecute `ibmcloud login --sso` y siga las instrucciones de la salida de la CLI para recuperar el código de acceso puntual mediante su navegador web.

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Ejemplo:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [consulte las abreviaturas de región tal como se usan en los puntos finales de la API](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).

    <table summary-"Input parameters to retrieve tokens">
    <caption>Parámetros de entrada para obtener señales</caption>
    <thead>
        <th>Parámetros de entrada</th>
        <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>Content-Type:application/x-www-form-urlencoded</li> <li>Authorization: Basic Yng6Yng=<p><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username: <em>&lt;username&gt;</em>`</li>
    <li>`password: <em>&lt;password&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente a las claves de API de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey: <em>&lt;api_key&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:``</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al código de acceso puntual de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode: <em>&lt;passcode&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
    "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la señal de {{site.data.keyword.Bluemix_notm}} IAM en el campo **access_token** de la salida de API. Anote la señal de {{site.data.keyword.Bluemix_notm}} IAM para recuperar información de cabecera adicional en los pasos siguientes.

2.  Recupere el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en la que se ha creado el clúster. Sustituya `<iam_token>` por la señal de {{site.data.keyword.Bluemix_notm}} IAM que ha recuperado en el paso anterior.

    ```
    GET https://accountmanagement.<region>.bluemix.net/v1/accounts
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para obtener el ID de cuenta de {{site.data.keyword.Bluemix_notm}}">
    <caption>Parámetros de entrada para obtener un ID de cuenta de {{site.data.keyword.Bluemix_notm}}</caption>
    <thead>
  	<th>Parámetros de entrada</th>
  	<th>Valores</th>
    </thead>
    <tbody>
  	<tr>
  		<td>Cabeceras</td>
  		<td><ul><li>`Content-Type: application/json`</li>
      <li>`Authorization: bearer &lt;iam_token&gt;`</li>
      <li>`Accept: application/json`</li></ul></td>
  	</tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "total_results": 3,
      "total_pages": 1,
      "prev_url": null,
      "next_url": null,
      "resources":
        {
          "metadata": {
            "guid": "<account_ID>",
            "url": "/v1/accounts/<account_ID>",
            "created_at": "2016-01-07T18:55:09.726Z",
            "updated_at": "2017-04-28T23:46:03.739Z",
            "origin": "BSS"
    ...
    ```
    {: screen}

    Encontrará el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} en el campo **resources/metadata/guid** de la salida de la API.

3.  Genere una nueva señal de {{site.data.keyword.Bluemix_notm}} IAM que incluya sus credenciales de {{site.data.keyword.Bluemix_notm}} y el ID de la cuenta en la que se ha creado el clúster. Sustituya `<account_ID>` por el ID de la cuenta de {{site.data.keyword.Bluemix_notm}} que ha recuperado en el paso anterior.

    Si está utilizando una clave de API de {{site.data.keyword.Bluemix_notm}}, debe utilizar el ID de cuenta de {{site.data.keyword.Bluemix_notm}} y la clave de API se ha creado para la misma. Para acceder a los clústeres en otras cuentas, inicie una sesión en esta cuenta y cree una clave de API de {{site.data.keyword.Bluemix_notm}} que se base en esta cuenta.
    {: note}

    ```
    POST https://iam.<region>.bluemix.net/oidc/token
    ```
    {: codeblock}

    Ejemplo:
    ```
    POST https://iam.ng.bluemix.net/oidc/token
    ```
    {: codeblock}

    Para especificar una región de {{site.data.keyword.Bluemix_notm}}, [consulte las abreviaturas de región tal como se usan en los puntos finales de la API](/docs/containers?topic=containers-regions-and-zones#bluemix_regions).

    <table summary-"Input parameters to retrieve tokens">
    <caption>Parámetros de entrada para obtener señales</caption>
    <thead>
        <th>Parámetros de entrada</th>
        <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>`Content-Type:application/x-www-form-urlencoded`</li> <li>`Authorization: Basic Yng6Yng=`<p><strong>Nota</strong>: <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</p></li></ul>
    </td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al nombre de usuario y contraseña de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: password`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`username: <em>&lt;username&gt;</em>`</li>
    <li>`password: <em>&lt;password&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:``</li>
    <li>`bss_account: <em>&lt;account_ID&gt;</em>`</li></ul>
    <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente a las claves de API de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey: <em>&lt;api_key&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:``</li>
    <li>`bss_account: <em>&lt;account_ID&gt;</em>`</li></ul>
      <strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
    <td>Cuerpo correspondiente al código de acceso puntual de {{site.data.keyword.Bluemix_notm}}</td>
    <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:passcode`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`passcode: <em>&lt;passcode&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account: <em>&lt;account_ID&gt;</em>`</li></ul><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la señal de {{site.data.keyword.Bluemix_notm}} IAM en el campo **access_token**, y la señal de renovación en el campo **refresh_token**.

4.  Obtenga una lista de todos los clústeres de Kubernetes de la cuenta. Utilice la información que ha recuperado en pasos anteriores para crear la información de cabecera.

     ```
     GET https://containers.cloud.ibm.com/v1/clusters
     ```
     {: codeblock}

     <table summary="Parámetros de entrada para trabajar con la API">
     <caption>Parámetros de entrada para trabajar con la API</caption>
     <thead>
     <th>Parámetros de entrada</th>
     <th>Valores</th>
     </thead>
     <tbody>
     <tr>
     <td>Cabecera</td>
     <td><ul><li>`Authorization: bearer <em>&lt;iam_token&gt;</em>`</li>
     <li>`X-Auth-Refresh-Token: <em>&lt;refresh_token&gt;</em>`</li></ul></td>
     </tr>
     </tbody>
     </table>

5.  Consulte la [documentación de la API de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/swagger-api) para ver una lista de las API admitidas.

<br />


## Renovación de señales de acceso de {{site.data.keyword.Bluemix_notm}} IAM y obtención de nuevas señales de renovación con la API
{: #cs_api_refresh}

Cada señal de acceso de {{site.data.keyword.Bluemix_notm}} Identity and Access Management (IAM) que se emite mediante la API caduca transcurrida una hora. Debe renovar la señal de acceso de forma periódica para garantizar el acceso a la API de {{site.data.keyword.Bluemix_notm}}. Puede utilizar los mismos pasos para obtener una nueva señal de renovación.
{:shortdesc}

Antes de empezar, asegúrese de que tiene una señal de renovación de {{site.data.keyword.Bluemix_notm}} IAM o una clave de API de {{site.data.keyword.Bluemix_notm}} para solicitar una nueva señal de acceso.
- **Señal de renovación:** Siga las instrucciones en [Automatización del proceso de creación y gestión de clústeres con la API de {{site.data.keyword.Bluemix_notm}}](#cs_api).
- **Clave de API:** Recupere su clave de API de [{{site.data.keyword.Bluemix_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://cloud.ibm.com/) tal como se indica a continuación.
   1. En la barra de menús, pulse **Gestionar** > **Acceso (IAM)**.
   2. Pulse la página **Usuarios** y, a continuación, selecciónese usted mismo.
   3. En el panel **Claves de API**, pulse **Crear una clave de API de IBM Cloud**.
   4. Especifique un **Nombre** y una **Descripción** para la clave de API y, a continuación, pulse **Crear**.
   4. Pulse **Mostrar** para ver la clave de API generada en su nombre.
   5. Copie la clave de API de forma que la pueda utilizar para recuperar su señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM.

Utilice los pasos siguientes si desea crear una señal de {{site.data.keyword.Bluemix_notm}} IAM o si desea obtener una nueva señal de renovación.

1.  Genere una nueva señal de acceso de {{site.data.keyword.Bluemix_notm}} IAM utilizando la señal de renovación o la clave de API de {{site.data.keyword.Bluemix_notm}}.
    ```
    POST https://iam.bluemix.net/identity/token
    ```
    {: codeblock}

    <table summary="Parámetros de entrada para una nueva señal de IAM">
    <caption>Parámetros de entrada para una nueva señal de {{site.data.keyword.Bluemix_notm}} IAM</caption>
    <thead>
    <th>Parámetros de entrada</th>
    <th>Valores</th>
    </thead>
    <tbody>
    <tr>
    <td>Cabecera</td>
    <td><ul><li>`Content-Type: application/x-www-form-urlencoded`</li>
      <li>`Authorization: Basic Yng6Yng=`</br></br><strong>Nota:</strong> <code>Yng6Yng=</code> equivale a la autorización codificada de URL para el nombre de usuario <strong>bx</strong> y la contraseña <strong>bx</strong>.</li></ul></td>
    </tr>
    <tr>
    <td>Cuerpo al utilizar la señal de renovación</td>
    <td><ul><li>`grant_type: refresh_token`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`refresh_token: <em>&lt;iam_refresh_token&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
    <li>`uaa_client_secret:`</li>
    <li>`bss_account: <em>&lt;account_ID&gt;</em>`</li></ul><strong>Nota</strong>: Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    <tr>
      <td>Cuerpo al utilizar la clave de API de {{site.data.keyword.Bluemix_notm}}</td>
      <td><ul><li>`grant_type: urn:ibm:params:oauth:grant-type:apikey`</li>
    <li>`response_type: cloud_iam uaa`</li>
    <li>`apikey: <em>&lt;api_key&gt;</em>`</li>
    <li>`uaa_client_ID: cf`</li>
        <li>`uaa_client_secret:`</li></ul><strong>Nota:</strong> Añada la clave <code>uaa_client_secret</code> sin especificar un valor.</td>
    </tr>
    </tbody>
    </table>

    Ejemplo de salida de API:

    ```
    {
      "access_token": "<iam_token>",
      "refresh_token": "<iam_refresh_token>",
      "uaa_token": "<uaa_token>",
      "uaa_refresh_token": "<uaa_refresh_token>",
      "token_type": "Bearer",
      "expires_in": 3600,
      "expiration": 1493747503
    }

    ```
    {: screen}

    Encontrará la nueva señal de {{site.data.keyword.Bluemix_notm}} IAM en el campo **access_token**, y la señal de renovación en el campo **refresh_token** de la salida de la API.

2.  Siga trabajando con la [documentación de la API de {{site.data.keyword.containerlong_notm}} ![Icono de enlace externo](../icons/launch-glyph.svg "Icono de enlace externo")](https://containers.cloud.ibm.com/swagger-api) utilizando la señal del paso anterior.

<br />


## Renovación de señales de acceso de {{site.data.keyword.Bluemix_notm}} IAM y obtención de nuevas señales de renovación con la CLI
{: #cs_cli_refresh}

Al iniciar una nueva sesión de CLI, o si ha caducado la sesión actual de CLI de 24 horas, debe establecer el contexto para el clúster ejecutando `ibmcloud ks cluster-config <cluster_name>`. Cuando establezca el contexto del clúster con este mandato, se descargará el archivo
`kubeconfig` para el clúster de Kubernetes. Además, se emiten una señal de ID de Identity and Access Management (IAM) de
{{site.data.keyword.Bluemix_notm}} y una señal de renovación para proporcionar autenticación.
{: shortdesc}

**Señal de ID**: cada señal de ID de IAM que se emite a través de la CLI caduca después de una hora. Cuando caduca la señal de ID, se envía la señal de renovación al proveedor de señales para renovar la señal de ID. Se renueva la autenticación y puede seguir ejecutando mandatos en el clúster.

**Señal de renovación**: las señales de renovación caducan cada 30 días. Si caduca la señal de renovación, no se puede renovar la señal de ID y no podrá seguir ejecutando mandatos en la CLI. Puede obtener una nueva señal de renovación ejecutando `ibmcloud ks cluster-config <cluster_name>`. Este mandato también renueva la señal de ID.
