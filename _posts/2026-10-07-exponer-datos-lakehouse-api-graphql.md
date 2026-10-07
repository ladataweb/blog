---
layout: post
title: '[Fabric] Exponer datos de un Lakehouse mediante API REST'
date: 2026-10-07 07:48:00 -0300
slug: exponer-datos-lakehouse-api-graphql
tags:
- graph ql
- azure functions
- fabric lakehouse
- data engineering
- fabric warehouse 
- fabric training 
- fabric tutorial 
- fabric tips 
- fabric argentina 
- fabric cordoba 
- microsoft fabric
- fabric jujuy 
- ladataweb 
- fabric
description: 'En este artículo vamos a mostrar cómo exponer datos de un Lakehouse de Microsoft Fabric mediante una API REST utilizando API for GraphQL y Azure Functions.'
legacy: true
tumblr_id: '829840629069643777'
faqs:
  - q: '¿Puedo exponer datos de un Lakehouse mediante API REST?'
    a: 'Sí, es posible exponer datos de un Lakehouse mediante una API REST utilizando API for GraphQL y Azure Functions como capa intermedia.'
  - q: '¿Qué es GraphQL?'
    a: 'GraphQL es un lenguaje de consulta para APIs que permite solicitar exactamente los datos que se necesitan, evitando la sobrecarga de información y mejorando la eficiencia en la comunicación entre cliente y servidor. En Microsoft Fabric, API for GraphQL es un ítem de la plataforma que nos permite exponer los datos de un Lakehouse de manera controlada y segura, facilitando la integración con otros sistemas mediante consultas precisas.'
  - q: '¿Puedo usar GraphQL con Microsoft Fabric?'
    a: 'Sí, Microsoft Fabric incluye API for GraphQL. La API de Microsoft Fabric para GraphQL aporta este potente estándar al ecosistema de Fabric como una capa de acceso a datos que le permite consultar varios orígenes de datos de forma rápida y eficaz. '
  - q: '¿Cómo relaciono api for graphql y azure functions?'
    a: 'API for GraphQL nos permite exponer los datos de un Lakehouse de Fabric mediante consultas GraphQL. Azure Functions actúa como capa intermedia que recibe las solicitudes REST del sistema externo, traduce esas solicitudes a consultas GraphQL y las envía a Fabric. De esta manera, el sistema externo nunca accede directamente al Lakehouse, sino que interactúa con la Function que maneja la comunicación segura con Fabric y devuelve los datos en formato JSON.'
---

Hay ítems en Fabric que no siempre utilizamos y pueden fortalecer escenarios. En este caso hablo de API for GraphQL.

En el siguiente artículo invitamos a Nazarena a contarnos de un requisito para disponibilizar datos de un Lakehouse de Fabric mediante API REST que fue satisfactorio gracias al ítem en cuestión.

<!--more-->

Para comenzar voy a poner en contexto el requerimiento que guía el artículo.

> *Implementar un mecanismo de consulta a los datos mediante una API del tipo REST, accediendo de forma segura, pero sin darle acceso directo al Lake, al SQL Endpoint ni al workspace.*

La necesidad era bastante concreta: exponer información y devolver un JSON entendible para otro sistema.

Para evaluar la viabilidad aplicamos una prueba sobre la arquitectura medallón ya desarrollada, al igual que en el caso del cliente, sumándole una API for GraphQL de Fabric y una Azure Function como capa intermedia. La Function recibe una llamada, consulta Fabric con identidad administrada y devuelve la información requerida.

El objetivo era que el sistema externo accediera solamente a un endpoint.

La arquitectura a continuación permite visualizar el flujo que vamos a implementar.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="211" data-orig-width="850"><img src="https://64.media.tumblr.com/7090d0af6eecdfa71bffb62c5629cc13/a852e6285f29d41c-57/s1280x1920/512ccbed98aec5995e001245a4c8eaacd738e3a5.pnj" data-orig-height="211" data-orig-width="850" srcset="https://64.media.tumblr.com/7090d0af6eecdfa71bffb62c5629cc13/a852e6285f29d41c-57/s1280x1920/512ccbed98aec5995e001245a4c8eaacd738e3a5.pnj 850w" sizes="(max-width: 850px) 100vw, 850px"></figure></div>

Para el caso, vamos a tomar como referencia datos disponibles de una demo, donde el objetivo será exponer únicamente las operaciones de Venta de Exportaciones. Los datos se obtienen de la tabla de hechos fact\_operaciones, ubicada dentro de la capa Gold y contiene las operaciones de ventas.

## **API for GraphQL en Fabric**

Como primer paso, creamos el ítem API for GraphQL en el workspace de Fabric y lo conectamos al Lakehouse Gold. Pero antes de seguir voy a sumar la definición como les gusta hacer aquí en LaDataWeb:

> *API for GraphQL es un ítem de Fabric que permite crear una capa de acceso sobre los datos de Fabric, exponiendo los campos y fuentes que se necesiten consultar. *

<div class="npf_row"><figure class="tmblr-full" data-orig-height="285" data-orig-width="334"><img src="https://64.media.tumblr.com/4bf769227eae9bafc4f1af04d5cadf60/a852e6285f29d41c-88/s400x600/e712fb28b425d7ecf88712c7f335f603e23c9e46.pnj" data-orig-height="285" data-orig-width="334" srcset="https://64.media.tumblr.com/4bf769227eae9bafc4f1af04d5cadf60/a852e6285f29d41c-88/s400x600/e712fb28b425d7ecf88712c7f335f603e23c9e46.pnj 334w" sizes="(max-width: 334px) 100vw, 334px"></figure></div>

Paso posterior podremos definir en que datos trabajar.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="409" data-orig-width="324"><img src="https://64.media.tumblr.com/786dd7361b7549a36594c40dbf05f31a/a852e6285f29d41c-d0/s400x600/f57740b84334283e88bd39dd9ab07c66226f24f1.pnj" data-orig-height="409" data-orig-width="324" srcset="https://64.media.tumblr.com/786dd7361b7549a36594c40dbf05f31a/a852e6285f29d41c-d0/s400x600/f57740b84334283e88bd39dd9ab07c66226f24f1.pnj 324w" sizes="(max-width: 324px) 100vw, 324px"></figure></div>

Desde el editor de API for GraphQL podemos probar consultas directamente sobre las fuentes expuestas por la API. Desde ahí se puede validar el esquema disponible, aplicar filtros, trabajar con variables y ejecutar agregaciones antes de implementarlo.

En este caso lo utilizamos para probar la consulta sobre fact\_operaciones y asegurar que los filtros y agrupaciones funcionaran antes de incorporarlos en la Azure Function.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="669" data-orig-width="849"><img src="https://64.media.tumblr.com/57422babeac62141b311979554e13045/a852e6285f29d41c-39/s1280x1920/3e7a4d13e1ccbba967954af159c3f16e17a3273f.pnj" data-orig-height="669" data-orig-width="849" srcset="https://64.media.tumblr.com/57422babeac62141b311979554e13045/a852e6285f29d41c-39/s1280x1920/3e7a4d13e1ccbba967954af159c3f16e17a3273f.pnj 849w" sizes="(max-width: 849px) 100vw, 849px"></figure></div>

Una vez validada la consulta en Fabric, con los datos y resultados correctos, copiamos el endpoint del ítem para utilizarlo luego en la Function.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="417" data-orig-width="849"><img src="https://64.media.tumblr.com/d576096aab2d849e4c93e3d7fee066f6/a852e6285f29d41c-12/s1280x1920/c2f021b4ab8ec77c475444d3e2fb2df8aee6dcff.pnj" data-orig-height="417" data-orig-width="849" srcset="https://64.media.tumblr.com/d576096aab2d849e4c93e3d7fee066f6/a852e6285f29d41c-12/s1280x1920/c2f021b4ab8ec77c475444d3e2fb2df8aee6dcff.pnj 849w" sizes="(max-width: 849px) 100vw, 849px"></figure></div>

## **Azure Function**

Con GraphQL funcionando, nos queda resolver cómo exponer esos datos de una forma simple hacia un sistema externo.

Se podría consumir GraphQL directamente, pero eso implicaría el acceso al modelo de Fabric y su entorno. Por eso utilizamos Azure Function como una capa intermedia que recibe los parámetros, consulta GraphQL y devuelve un JSON como el requerimiento lo solicitaba.

Para este caso, al igual que en el [artículo](<https://blog.ladataweb.com.ar/fabric-notebooks-obtener-datos-por-red-virtual-con-ip-unico/>) de La Data Web donde usamos Azure Functions como puente entre Fabric y una API externa, volvemos a usar la Function como una capa intermedia.

La diferencia es que ahora el flujo va en sentido contrario, el sistema externo consulta la Azure Function, esta se conecta a la API for GraphQL de Fabric y devuelve la información en un JSON simple de consumir.

El endpoint final que se expone es el siguiente:

GET /api/exportaciones/resumen

<div class="npf_row"><figure class="tmblr-full" data-orig-height="240" data-orig-width="849"><img src="https://64.media.tumblr.com/aa4e18fc7177e4dee540e5ac43f2ef60/a852e6285f29d41c-d9/s1280x1920/7b85f5724d0dd1a10adfeed5def591952ccf84a5.pnj" data-orig-height="240" data-orig-width="849" srcset="https://64.media.tumblr.com/aa4e18fc7177e4dee540e5ac43f2ef60/a852e6285f29d41c-d9/s1280x1920/7b85f5724d0dd1a10adfeed5def591952ccf84a5.pnj 849w" sizes="(max-width: 849px) 100vw, 849px"></figure></div>

> *NOTA: Para la prueba se utilizó una Function App en Flex Consumption de manera de contar con un esquema flexible y de pago por uso. *

<div class="npf_row"><figure class="tmblr-full" data-orig-height="363" data-orig-width="849"><img src="https://64.media.tumblr.com/f9c138865e1902ed2719f940b264970f/a852e6285f29d41c-c2/s1280x1920/ef04f537b096d0dded73c8403f4a51bd534a37cf.pnj" data-orig-height="363" data-orig-width="849" srcset="https://64.media.tumblr.com/f9c138865e1902ed2719f940b264970f/a852e6285f29d41c-c2/s1280x1920/ef04f537b096d0dded73c8403f4a51bd534a37cf.pnj 849w" sizes="(max-width: 849px) 100vw, 849px"></figure></div>

**Configuración del endpoint de Fabric **

En la Function App se agregó una variable de entorno con el nombre FABRIC\_GRAPHQL\_ENDPOINT, donde guardamos la URL copiada desde el ítem GraphQL.

De esta forma el endpoint no queda escrito dentro del código y puede ser cambiado sin modificar la lógica de la Function.

*FABRIC\_GRAPHQL\_ENDPOINT = <endpoint copiado desde Fabric>*

<div class="npf_row"><figure class="tmblr-full" data-orig-height="456" data-orig-width="567"><img src="https://64.media.tumblr.com/620e16128a2299dc51920230b3055b96/a852e6285f29d41c-9d/s640x960/2d4b0f12007ae5717d9594067166960e9059db46.pnj" data-orig-height="456" data-orig-width="567" srcset="https://64.media.tumblr.com/620e16128a2299dc51920230b3055b96/a852e6285f29d41c-9d/s640x960/2d4b0f12007ae5717d9594067166960e9059db46.pnj 567w" sizes="(max-width: 567px) 100vw, 567px"></figure></div>

## **Managed Identity**

Un punto que hay que resolver antes de poder implementar la solución completa es la autenticación contra Fabric.

La autenticación podría resolverse mediante una app registration, con Client ID y Client Secret, pero para el caso implementado y para evitar la creación de secretos, se habilitó una identidad administrada del sistema directamente en la Function App.

*Function App > Identity > System assigned > On*

<div class="npf_row"><figure class="tmblr-full" data-orig-height="801" data-orig-width="687"><img src="https://64.media.tumblr.com/64d65a58e3c50d4104fb8852cbc87357/a852e6285f29d41c-7c/s1280x1920/a729d578daf75a5d44cf0d5d034375c1d211eacd.pnj" data-orig-height="801" data-orig-width="687" srcset="https://64.media.tumblr.com/64d65a58e3c50d4104fb8852cbc87357/a852e6285f29d41c-7c/s1280x1920/a729d578daf75a5d44cf0d5d034375c1d211eacd.pnj 687w" sizes="(max-width: 687px) 100vw, 687px"></figure></div>

Con esto Azure le asigna una identidad propia al recurso. En el código se utiliza DefaultAzureCredential, por lo que cuando corre en Azure toma automáticamente esa identidad y solicita un token para Fabric.

**From azure.identity import DefaultAzureCredential <br>

<br>

credential = DefaultAzureCredential() <br>

token = credential.get\_token( <br>

 “https:**//api.fabric.microsoft.com/.default” <br>

).token

**Dar permiso a esa identidad dentro de Fabric**

Con habilitar la identidad en Azure no alcanza. Fabric también tiene que reconocerla y autorizarla.

En este caso agregamos la identidad de la Function App directamente al workspace de Fabric con rol Contributor. De esta forma, la identidad obtiene acceso a los ítems del workspace, incluida la API for GraphQL y la fuente de datos utilizada por la consulta.

<figure data-orig-height="195" data-orig-width="273"><img src="https://64.media.tumblr.com/94d427e737639c752b27fe6eeafc976f/a852e6285f29d41c-1d/s400x600/2e003e37268ee15451d032bd6055f41e5578655e.pnj" data-orig-height="195" data-orig-width="273" srcset="https://64.media.tumblr.com/94d427e737639c752b27fe6eeafc976f/a852e6285f29d41c-1d/s400x600/2e003e37268ee15451d032bd6055f41e5578655e.pnj 273w" sizes="(max-width: 273px) 100vw, 273px"></figure>

También se podría dar acceso de forma más granular directamente sobre el ítem GraphQL, asignando únicamente el permiso necesario para ejecutar consultas.

<figure data-orig-height="387" data-orig-width="210"><img src="https://64.media.tumblr.com/e01e07ca3ab890df0eb9f8881057cdc1/a852e6285f29d41c-8f/s250x400/5e0fb4cb61668ed5be6a0136fb6b412ec30ab763.pnj" data-orig-height="387" data-orig-width="210" srcset="https://64.media.tumblr.com/e01e07ca3ab890df0eb9f8881057cdc1/a852e6285f29d41c-8f/s250x400/5e0fb4cb61668ed5be6a0136fb6b412ec30ab763.pnj 210w" sizes="(max-width: 210px) 100vw, 210px"></figure>

Es importante tener en cuenta la configuración del tenant. Dependiendo de las políticas de la organización, el uso de service principals o identidades administradas puede requerir ciertas configuraciones y habilitación en el Admin Portal por parte del administrador.

## **Del resultado GraphQL a un JSON de negocio**

La Function no devuelve la respuesta de GraphQL tal cual viene.

Fabric devuelve los grupos y agregaciones. La Function toma esa información, calcula el precio promedio como facturación dividida por toneladas y arma una estructura más cómoda para consumir: totales, campaña, cultivo y destinos.

La consulta GraphQL procesa los datos, filtra las operaciones, aplica el rango de fechas y agrupa la información. Sobre esos grupos calcula directamente en Fabric métricas como facturación total, toneladas y cantidad de operaciones.

A partir de esa respuesta, la Azure Function termina de darle forma al resultado, calcula el precio promedio como facturación / toneladas y arma una estructura para consumir desde afuera: totales generales, campañas, cultivos y destinos.

GraphQL: filtra, agrupa y agrega los datos directamente en Fabric.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="570" data-orig-width="588"><img src="https://64.media.tumblr.com/e9bf3a464721e32f9cdd8955169ed332/a852e6285f29d41c-28/s640x960/993b5251030b5f210adb256759a069ccf88c0490.pnj" data-orig-height="570" data-orig-width="588" srcset="https://64.media.tumblr.com/e9bf3a464721e32f9cdd8955169ed332/a852e6285f29d41c-28/s640x960/993b5251030b5f210adb256759a069ccf88c0490.pnj 588w" sizes="(max-width: 588px) 100vw, 588px"></figure></div>

Azure Function: calcula la métrica derivada y transforma la respuesta en un JSON más simple para el consumidor.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="435" data-orig-width="420"><img src="https://64.media.tumblr.com/9bf511b80c2d9d2df5b889ffda24c769/a852e6285f29d41c-8b/s500x750/bc0c3b0b3460c1a873cf66a3d97aaa1f5fbd247a.pnj" data-orig-height="435" data-orig-width="420" srcset="https://64.media.tumblr.com/9bf511b80c2d9d2df5b889ffda24c769/a852e6285f29d41c-8b/s500x750/bc0c3b0b3460c1a873cf66a3d97aaa1f5fbd247a.pnj 420w" sizes="(max-width: 420px) 100vw, 420px"></figure></div>

El resultado que se obtiene al hacer la llamada es:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="1053" data-orig-width="849"><img src="https://64.media.tumblr.com/c305a0f83ac09321c1fbab646d733e2d/a852e6285f29d41c-37/s1280x1920/c764425967cf9b220e64b450acef63d065908686.pnj" data-orig-height="1053" data-orig-width="849" srcset="https://64.media.tumblr.com/c305a0f83ac09321c1fbab646d733e2d/a852e6285f29d41c-37/s1280x1920/c764425967cf9b220e64b450acef63d065908686.pnj 849w" sizes="(max-width: 849px) 100vw, 849px"></figure></div>

## **¿Y cómo se protege el endpoint externo?**

Para la prueba se configuró la Function con AuthLevel.FUNCTION. Esto obliga a enviar una Function Key para poder ejecutar el endpoint.

La clave es que esa Function Key solo controla el acceso al endpoint de la Function. La conexión entre la Function y Fabric se autentica por separado, usando Entra ID y Managed Identity.

> *https://<function-app>.azurewebsites.net/api/exportaciones/resumen?code=<function-key>*

## **Conclusión**

La prueba permitió validar la posibilidad de mantener el dato dentro de Microsoft Fabric y exponer hacia afuera solamente la información que necesita otra aplicación.

- El Lakehouse sigue siendo interno. 
- GraphQL se ocupa de consultar y agregar. 
- La Azure Function define lo que se disponibiliza. 
- Y la autenticación entre servicios queda resuelta con una identidad administrada. 

<!-- -->

*Escrito por<br>

**[Nazarena Tossolini](<https://www.linkedin.com/in/nazarenatossolini/>)*

