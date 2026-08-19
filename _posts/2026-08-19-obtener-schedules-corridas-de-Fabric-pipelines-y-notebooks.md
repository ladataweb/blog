---
layout: post
title: '[SimplePBI] Obtener schedules-corridas de Fabric pipelines y notebooks'
date: 2026-08-19 07:48:00 -0300
slug: obtener-schedules-corridas-de-fabric-pipelines-y-notebooks
tags:
- fabric
- fabric tutorial 
- fabric training 
- simplepbi
- fabric tips 
- fabric python
- fabric argentina 
- fabric cordoba 
- fabric jujuy 
- ladataweb 
description: 'En este articulo vamos a mostrar como obtener de forma simple los datos de las corridas de los fabric pipelines y fabric notebooks usando la librería SimplePBI.'
legacy: true
tumblr_id: '825393745482694656'
faqs:
  - q: '¿Para que sirve la librería SimplePBI?'
    a: 'La librería SimplePBI es un wrapper de la API de Power Bi y Fabric que nos permite trabajar con los requests de forma simple y rápida. En este artículo vamos a mostrar como obtener de forma simple los datos de las corridas de los fabric pipelines y fabric notebooks usando la librería SimplePBI.'
  - q: '¿Que es un schedule y una corrida en Fabric?'
    a: 'En Fabric, un schedule es una programación que define cuándo se ejecutará un pipeline o notebook. Una corrida (o instancia) es la ejecución real de ese pipeline o notebook en el momento definido por el schedule. Por ejemplo, si tenemos un pipeline que se ejecuta todos los días a las 8:00 AM, el schedule define esa hora y cada vez que se ejecute, se generará una corrida.'
  - q: '¿Como puedo obtener los schedules y corridas de mis pipelines y notebooks?'
    a: 'Para obtener los schedules y corridas de tus pipelines y notebooks en Fabric, puedes utilizar la librería SimplePBI. Esta librería te permite interactuar con la API de Fabric de manera sencilla. En el artículo se muestra un ejemplo de cómo autenticarte, listar los items (pipelines y notebooks) de un área de trabajo, y luego obtener los schedules e instancias (corridas) de cada item utilizando las funciones proporcionadas por SimplePBI.'
---


Fabric avanza a grandes pasos y si bien hoy cuenta con una sección de monitoreo de cada actividad en el tenant, puede que sea demasiado para nuestro análisis diario o para un perfil de terminado.

Si querés armar un tablero conociendo los horarios en que corren tus ítems o analizando las corridas que tuvieron, entonces seguí leyendo.

<!--more-->

Este artículo nos muestra como obtener de forma simple los datos de las corridas de los fabric pipelines y fabric notebooks.

Para trabajar en esta solución necesitamos tener conocimientos básicos de setear la todo para usar la Fabric rest api. [Podes leerlo en este artículo](<https://blog.ladataweb.com.ar/seteo-powerbi-rest-api-por-primera-vez/>).

Conociendo el funcionamiento de la API vamos a ejecutar código de python para obtener esos datos. Podemos hacerlo desde un notebook de fabric como en cualquier otro espacio que querramos para transformar estos datos.

En esta solución vamos a ejecutar le código python buscando obtener los schedules y las instancias (corridas) de los Data Pipelines y Notebooks dentro de una área de trabajo. La librería SimplePBI nos permite hacerlo simple usando su clase Schedules e ítems en el objeto Core

> *NOTA: los objetos y clases de la librería están tal como se categorizan en el índice de la **[documentación de la Fabric Rest API](<https://learn.microsoft.com/en-us/rest/api/fabric/core/job-scheduler?wt.mc_id=DP-MVP-5004778>)**. *

Para lograrlo, ejecutaremos los siguientes pasos.

1. Obtener los DataPipelines del área de trabajo
2. Obtener los Notebooks del área de trabajo
3. Juntar schedules e instancias de los Data Pipelines
4. Juntar schedules e instnacias de los Notebooks
5. Agrupar en dos dataframes

<!-- -->

Veamos los detalles de las librería.

Importamos objetos

from simplepbi import token<br>

from simplepbi.fabric import core

Variables para autenticar y buscar area

TENANT\_ID = "xxxxxx-xxxx-xxxx-xxxx-xxxxxx"<br>

power\_bi\_client\_id = "xxxxxx-xxxx-xxxx-xxxx-xxxxxx"<br>

power\_bi\_secret = ""<br>

workspace\_id = "xxxxxx-xxxx-xxxx-xxxx-xxxxxx"

Crear objetos

t = token.Token(TENANT\_ID,power\_bi\_client\_id,None,None,power\_bi\_secret,use\_service\_principal=True)<br>

it = core.Items(t.token)<br>

job = core.Scheduler(t.token)

Obtener los items del area de trabajo

notebooks\_resp = it.list\_items(workspace\_id, type="Notebook")<br>

pipelines\_resp = it.list\_items(workspace\_id, type="DataPipeline")

Ejemplo de como obtener schedules o instancias

job.list\_item\_schedules(workspace\_id, item\_id, "RunNotebook", False)<br>

job.list\_item\_job\_instances(workspace\_id, item\_id)

Con esas simples líneas podemos obtener los datos deseados. Luego cada quien puede procesarlos como guste. En mi caso, elegí procesar las instancias con un mensaje de fallo en caso que lo haya y los schedules mostrando uno por tiempo configurado. El código puede correrse desde Fabric notebook y hay una celda para usar pandas u otra para usar spark frame.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="781" data-orig-width="1223"><img src="https://64.media.tumblr.com/551aa1e813ed494aba82b894ff1546af/aa4456f486a210f9-a5/s1280x1920/7f310a00189f374d529caa48da7c9af7629bde9b.pnj" data-orig-height="781" data-orig-width="1223" srcset="https://64.media.tumblr.com/551aa1e813ed494aba82b894ff1546af/aa4456f486a210f9-a5/s1280x1920/7f310a00189f374d529caa48da7c9af7629bde9b.pnj 1223w" sizes="(max-width: 1223px) 100vw, 1223px"></figure></div>

[Pueden encontrar el notebook en mi github](<https://github.com/ibarrau/PowerBi-code/blob/master/Python/Get%20run%20instances.ipynb>).

Espero que les sirva para generar los datos para explotarlos luego como les quede más cómodo y útil, ya sea con Power Bi u otro modo. No se olviden de usar Azure Key Vaults para no exponer sus secretos [como indica este artículo](<https://blog.ladataweb.com.ar/protege-credenciales-en-notebooks-con-azure-keyvault/>).

