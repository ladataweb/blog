---
layout: post
title: '[Fabric][SimplePBI] Actualizar área de trabajo desde git repo'
date: 2026-10-20 07:48:00 -0300
slug: actualizar-area-de-trabajo-desde-git-repo
tags:
- fabric CICD
- data engineering
- simplepbi
- fabric training 
- fabric tutorial 
- fabric tips 
- fabric argentina 
- fabric cordoba 
- microsoft fabric
- fabric jujuy 
- ladataweb 
- fabric
description: 'En este artículo vamos a mostrar como actualizar cambios de un repositorio a un área de trabajo en Microsoft Fabric. Dicho de otra forma, vamos a aprender a implementar automáticamente la presión manual del botón "Update" que se ve en la interfaz cuando hay cambios pendientes para el área de trabajo.'
legacy: true
tumblr_id: '831020346506444800'
faqs:
  - q: '¿Puedo actualizar cambios pendientes de un repositorio a un área de trabajo de Microsoft Fabric automáticamente?'
    a: 'Sí, es posible automatizar la actualización de cambios pendientes de un repositorio a un área de trabajo en Microsoft Fabric. Esto se puede lograr utilizando la API de Microsoft Fabric junto con una librería como SimplePBI en Python. Al configurar un script que interactúe con la API, se puede ejecutar automáticamente la acción de actualización (similar a presionar el botón "Update" en la interfaz) cada vez que haya cambios en el repositorio, permitiendo así un flujo continuo de integración y despliegue (CICD) sin intervención manual.'
  - q: '¿Qué es SimplePBI?'
    a: 'SimplePBI es una librería de Python que facilita la interacción con la API de Microsoft Fabric, permitiendo automatizar tareas como la actualización de áreas de trabajo a partir de cambios en un repositorio.'
  - q: 'Qué alternativa a deployment pipelines o la librería fabricicd tengo para hacer CICD en Microsoft Fabric?'
    a: 'Una alternativa es utilizar scripts de Python con la librería SimplePBI para interactuar con la API de Microsoft Fabric. Esto permite automatizar la actualización de áreas de trabajo desde un repositorio, integrando el proceso en pipelines de CI/CD existentes, como los de Azure DevOps o GitHub Actions, sin depender exclusivamente de las herramientas nativas de Fabric.'
---

Las áreas de trabajo sincronizadas con un repositorio ya no son una novedad. De hace tiempo podemos versionar ítems de Fabric. Sin embargo, versionar no lo es todo, también pensamos en ambientes e integraciones/deploy continuos.

En este artículo muestro como actualizar una área de trabajo a partir de cambios de un repositorio utilizando SimplePBI, la python library para power bi y fabric rest api. Indispensable para construir CICD.

<!--more-->

Antes de comenzar me gustaría alinear algunos conceptos. Si bien este artículo solo muestra como ejecutar una acción que llevaría los cambios del repositorio a una área de trabajo, no podemos pensarlo sin el foco que representa.

Al trabajar con ambientes, lo que más nos gustaría es seguir un flujo unidireccional que lleve los cambios automáticamente. Con esto me refiero a que un desarrollador que termine de hacer sus cambios y quiera llevar eso a test o producción, ejecutaría un pull request que podría enlazar una acción para que automáticamente los cambios del repo vayan a la área de trabajo simulando un deploy. Veamos el siguiente diagrama asumiendo que cada área de trabajo esta sincronizada a un repositorio.

> *NOTA: Si no sabes como integrar un área de trabajo a un repositorio, podemos leer este artículo para *[*Azure DevOps*](<https://blog.ladataweb.com.ar/integracion-con-azuredevops-git-repos/>)* o este para *[*Github*](<https://blog.ladataweb.com.ar/integrar-repo-github-con-area-de-trabajo/>)*.*

<div class="npf_row"><figure class="tmblr-full" data-orig-height="1422" data-orig-width="2048"><img src="https://64.media.tumblr.com/d1d72a3af8f07ddbdc359a6a8ff15587/3fecaafcc4c32967-db/s2048x3072/48890982805c2fe7b1c3bb5491a223838125d412.pnj" data-orig-height="1422" data-orig-width="2048" srcset="https://64.media.tumblr.com/d1d72a3af8f07ddbdc359a6a8ff15587/3fecaafcc4c32967-db/s2048x3072/48890982805c2fe7b1c3bb5491a223838125d412.pnj 2048w" sizes="(max-width: 1280px) 100vw, 1280px"></figure></div>

Como pueden apreciar en la imagen, un desarrollador trabajando en una área de trabajo podría estar escribiendo un notebook, integrando nuevos datos en un pipeline o armando vistas de un warehouse. Sea el caso que sea, al terminar su trabajo llevaría los cambios al repositorio con un "Commit". Para que estos cambios fluyan, se ejecutan pull request a la branch siguiente.

Un proceso automatizado de despliegue sería el que se encargue que al recibir cambios en la rama, puedan automáticamente reflejarse en su ambiente. Este proceso es el "Actualizar de Git" que suele aparecer para ejecutar manualmente en las áreas:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="609" data-orig-width="1201"><img src="https://64.media.tumblr.com/fc6099618e02b859720c7aabc1d5b48d/3fecaafcc4c32967-0f/s1280x1920/9a840d87f8c7d9af7679564e3ee82392d56e005c.pnj" data-orig-height="609" data-orig-width="1201" srcset="https://64.media.tumblr.com/fc6099618e02b859720c7aabc1d5b48d/3fecaafcc4c32967-0f/s1280x1920/9a840d87f8c7d9af7679564e3ee82392d56e005c.pnj 1201w" sizes="(max-width: 1201px) 100vw, 1201px"></figure></div>

Ahora vamos a automatizar esto para poder ponerlo en un DevOps Pipeline o una Github action.

## 1- Permitir sincronización Git a nuestro service principal

Puede que ya tengan la opción funcionando porque ya hicieron la integración de su área de trabajo con Git, pero necesitamos que nuestro service principal también pueda hacerlo. Para eso revisamos la siguiente opción de portal de administración "Tenant Settings":

<div class="npf_row"><figure class="tmblr-full" data-orig-height="679" data-orig-width="878"><img src="https://64.media.tumblr.com/959dad70bd94c4d0f3130360ba79e290/3fecaafcc4c32967-28/s1280x1920/3fbc7984790f5e36634bc1825f52fa85c0a6571c.pnj" data-orig-height="679" data-orig-width="878" srcset="https://64.media.tumblr.com/959dad70bd94c4d0f3130360ba79e290/3fecaafcc4c32967-28/s1280x1920/3fbc7984790f5e36634bc1825f52fa85c0a6571c.pnj 878w" sizes="(max-width: 878px) 100vw, 878px"></figure></div>

No es necesario que toda la organización lo haga, si lo tenemos por grupos de seguridad bastaría con agregar como miembro al service principal que vayamos a usar en la ejecución.

## 2- Cargar la conexión.

Puede que suene raro, pero para poder actualizar los datos del repo hacia el área de trabajo, necesitamos disponer de una conexión al repositorio tal como lo hacemos para integrar datos. Desde el administrador de conexiones (manage connections en el engranaje) vamos a agregar una nueva conexión Cloud del estilo Github Source Control o Azure DevOps Source Control

<div class="npf_row"><figure class="tmblr-full" data-orig-height="707" data-orig-width="993"><img src="https://64.media.tumblr.com/c3e20f8941adfc534b43c979cb024c0f/3fecaafcc4c32967-f2/s1280x1920/71f800feba0a92c6732c691c11ac8dd47ae7afce.pnj" data-orig-height="707" data-orig-width="993" srcset="https://64.media.tumblr.com/c3e20f8941adfc534b43c979cb024c0f/3fecaafcc4c32967-f2/s1280x1920/71f800feba0a92c6732c691c11ac8dd47ae7afce.pnj 993w" sizes="(max-width: 993px) 100vw, 993px"></figure></div>

De esta forma dispondremos del Connection ID necesario para ejecutar nuestra operación. No olvidemos darle permisos al Service Principal sobre la conexión para que pueda utilizarla.

## 3- Script de python con SimplePBI

Si nunca usaste la Fabric/PowerBi Rest API, [podes leer éste artículo](<https://blog.ladataweb.com.ar/seteo-powerbi-rest-api-por-primera-vez/>). Nuestra apliacación registrada necesitará los permisos de las acciones que hacemos. En este caso Workspace.GitUpdate.All or Workspace.GitCommit.All y Workspace.ReadWrite.All. Vamos a escribir un script de python que luego pueden llamar desde el trigger de un ambiente. Solo vamos a cubrir el código para que vean lo simple que lo hace SimplePBI.

```Python
# Importar módulos
from simplepbi import token
from simplepbi.fabric import core

# Cargar variables
TENANT_ID = "xxxxxx-xxxx-xxxx-xxxxxx"
power_bi_client_id = 'xxxxxx-xxxx-xxxx-xxxxxx'
power_bi_secret = '*************'
workspace_id ="xxxxxx-xxxx-xxxx-xxxxxx"
connection_id="xxxxxx-xxxx-xxxx-xxxxxx"

# Inicializar objetos a usar de la API
t = token.Token(TENANT_ID, power_bi_client_id, None, None, power_bi_secret, use_service_principal=True)
gg = core.Git(t.token)

# Hacemo conciente al script que vamos a usar esa conexión
gg.update_my_git_credentials(workspace_id, connection_id)

# Obtenemos el status de git que nos da información del SHA del commit que acabamos de ejecutar y el HEAD.
statu = gg.get_git_status(workspace_id)

# Definimos un accionar en caso de conflictos
conflict_resolution = {"ConflictResolutionPolicy": "PreferRemote", "ConflictResolutionType": "Workspace"}

# Ejecutamos el update
gg.update_from_git(workspace_id, remoteCommitHash=statu["remoteCommitHash"], conflictResolution=conflict_resolution, workspaceHead=statu["workspaceHead"])

```

De esa forma automatizamos el botón que vimos antes en la UI para que al llegar nuevos cambios al repo, vayan automáticamente al área de trabajo. Ese script puede llamarse de una Github Action o un Azure DevOps Pipeline.

Ejemplo de ejecución:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="758" data-orig-width="749"><img src="https://64.media.tumblr.com/5ec3adb05b6998de6f0851b0d99a0ebf/3fecaafcc4c32967-af/s1280x1920/77038509379f97cab7a6d2f06b4ab3a2e654468a.pnj" data-orig-height="758" data-orig-width="749" srcset="https://64.media.tumblr.com/5ec3adb05b6998de6f0851b0d99a0ebf/3fecaafcc4c32967-af/s1280x1920/77038509379f97cab7a6d2f06b4ab3a2e654468a.pnj 749w" sizes="(max-width: 749px) 100vw, 749px"></figure></div>

Respuesta en tiempo real:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="905" data-orig-width="1057"><img src="https://64.media.tumblr.com/725f5f8812b12154d421110991b9a826/3fecaafcc4c32967-bb/s1280x1920/89f9a0f4594df183e7f2bf463f501a928985061e.gifv" data-orig-height="905" data-orig-width="1057" srcset="https://64.media.tumblr.com/725f5f8812b12154d421110991b9a826/3fecaafcc4c32967-bb/s1280x1920/89f9a0f4594df183e7f2bf463f501a928985061e.gifv 1057w" sizes="(max-width: 1057px) 100vw, 1057px"></figure></div>

Ahora si apreciamos como desaparece el cambio tras ejecutar el update desde la ventana de la consola.

Espero que este artículo les sirva para traer nuevas alternativas de CICD a la mesa que salgan de la caja puesto que ejecutar código python en cualquier parte puede ser más versátil que la librería fabric cicd que fuerza modos de autenticación o deployment pipelines que forzarían cambiar la dirección del diagrama enviando siempre de las áreas al repositorio.

