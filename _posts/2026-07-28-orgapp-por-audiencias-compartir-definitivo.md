---
layout: post
title: '[PowerBi] OrgApp por audiencias - el compartir definitivo'
date: 2026-07-28 07:48:00 -0300
slug: orgapp-por-audiencias-compartir-definitivo
tags:
- power bi
- power bi service 
- power bi argentina 
- power bi cordoba 
- power bi jujuy 
- power bi tutorial 
- power bi training 
- power bi tips 
- ladataweb 
- powerbi 
- fabric
description: 'En este articulo vamos a mostrar como configurar las audiencias dentro de una OrgApp para poder compartir contenido de forma más eficiente y con mejores prácticas.'
legacy: true
tumblr_id: '823387573902770176'
faqs:
  - q: '¿Qué es una Org Apps?'
    a: 'Una Org App es un ítem de creación dentro de Power Bi que nos permite productivizar un conjunto de contenido dentro de un workspace. Esto significa que podemos delimitar algunos desarrollos como informes, paginated reports o notebooks que estén creados en un área y exponerlos a una audiencia. Su fortaleza contra las Apps convencionales pasa por su integración automática de cambios, si cambias un reporte se cambia en la app también. Esto da una gran fuerza a escenarios de CICD.'
  - q: '¿Puedo compartir la app a distintas personas modificando el contenido?'
    a: 'Sí, a partir de la actualización de julio 2026, podemos crear audiencias dentro de una Org App para que distintos grupos de personas vean distintos contenidos dentro de la misma app. Esto nos permite mantener un solo desarrollo y una sola app que luego se distribuye en distintas personas con sus permisos pertinentes. La característica se llama audiencias'
  - q: '¿Cómo funciona una audiencia de una org app?'
    a: 'Una audiencia dentro de una Org App nos permite definir un grupo de personas que verán un conjunto específico de contenido dentro de la app. Podemos crear múltiples audiencias, cada una con su propio conjunto de contenidos, y asignar usuarios a cada audiencia. Esto nos permite compartir la misma Org App con diferentes grupos de usuarios, cada uno viendo solo el contenido que le corresponde.'
---

Hace tiempo que salió este nuevo ítem en Power Bi que evoluciona las Power Bi Apps convencionales. Sin embargo, había un pendiente clave. Podías crear muchas apps para una sola área de trabajo pero no podíamos compartir a distintas audiencias.

¡La actualización está aquí! en este artículo vamos a mostrar como es la nueva configuración que Org Apps trajo para permitirnos generar grupos de distribución para el contenido que seleccionamos dentro de nuestra app para compartir mejor.

<!--more-->

## ¿Para que sirven las Org Apps?

A modo de resumen las Org Apps dentro de PowerBi, son un ítem de creación al igual que un reporte. Nos ayuden a productivizar un conjunto de contenido dentro de un workspace. Esto significa que podemos delimitar algunos desarrollos como informes, paginated reports o notebooks que estén creados en un área y exponerlos a una audiencia. Su fortaleza contra las Apps convencionales pasa por su integración automática de cambios, si cambias un reporte se cambia en la app también. Esto da una gran fuerza a escenarios de CICD.

> Para más información podes revisar este post anterior: https://blog.ladataweb.com.ar/org-apps-nueva-forma-de-agrupar-y-compartir/

Anteriormente, si queríamos que un grupo de personas vea un conjunto de contenidos y otro grupo de personas vea un set distinto de reportes, debíamos crear dos OrgApp. Si es funcional, pero al momento de tener que ejecutar modificaciones en la app, teníamos que hacerlo dos o N veces. Las audiencias nos permiten mantener un solo desarrollo, una sola App que luego se distribuye en distintas personas con sus permisos pertinentes.

## ¿Cómo configurarla?

Para comenzar abrimos una área de trabajo con X cantidad de contenido. Creamos una Org App desde el menú de nuevo y le damos un nombre.

> *NOTA: Si ya tenías una Org App creada de antes de junio 2026, no verás las opciones siguientes y tendrás que crear una nueva.*

Abrimos nuestra app y agregamos el contenido o secciones deseadas. En el menú superior podrán ver un nuevo botón para controlar las audiencias "Manage audiences"

<div class="npf_row"><figure class="tmblr-full" data-orig-height="488" data-orig-width="753"><img src="https://64.media.tumblr.com/b07936d77a9057234f094a20d91ff064/dac307d0f7ffc986-4a/s1280x1920/75a714792719146be06735caf71cff0b2a8ea530.pnj" data-orig-height="488" data-orig-width="753" srcset="https://64.media.tumblr.com/b07936d77a9057234f094a20d91ff064/dac307d0f7ffc986-4a/s1280x1920/75a714792719146be06735caf71cff0b2a8ea530.pnj 753w" sizes="(max-width: 753px) 100vw, 753px"></figure></div>

La nueva pantalla nos mostrará una audiencia por defecto, pero nosotros vamos a dar click en "Nueva Audiencia" que nos pedirá ponerle un nombre.

Desde éste menú de audiencias nos dirigimos a la nueva pestaña que se creo y podemos seleccionar específicamente los elementos que verán, en mi ejemplo se llama "Prod":

<div class="npf_row"><figure class="tmblr-full" data-orig-height="515" data-orig-width="650"><img src="https://64.media.tumblr.com/94de60bf1352f78ffaf8367f9353054d/dac307d0f7ffc986-ad/s1280x1920/3769b4c1e6de779b0ec8874a8105158731e147b9.pnj" data-orig-height="515" data-orig-width="650" srcset="https://64.media.tumblr.com/94de60bf1352f78ffaf8367f9353054d/dac307d0f7ffc986-ad/s1280x1920/3769b4c1e6de779b0ec8874a8105158731e147b9.pnj 650w" sizes="(max-width: 650px) 100vw, 650px"></figure></div>

Para compartirla, volvemos al menú convencional de la Org App que nos permitía agregar contenido. Recuerden que este es otro ítem en fabric, entonces lo compartimos desde el botón compartir de arriba a la derecha:

<figure data-orig-height="184" data-orig-width="260"><img src="https://64.media.tumblr.com/d1530aba623e828ec0473a5d2590bc06/dac307d0f7ffc986-b1/s400x600/8052ed7c46dadde5a5d46ce90f60bc29a6272dbf.pnj" data-orig-height="184" data-orig-width="260" srcset="https://64.media.tumblr.com/d1530aba623e828ec0473a5d2590bc06/dac307d0f7ffc986-b1/s400x600/8052ed7c46dadde5a5d46ce90f60bc29a6272dbf.pnj 260w" sizes="(max-width: 260px) 100vw, 260px"></figure>

El menú convencional de compartir cambió y nos permite elegir la audiencia:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="823" data-orig-width="734"><img src="https://64.media.tumblr.com/6b53ed54a5afb118c158d7f51b71f972/dac307d0f7ffc986-09/s1280x1920/260f373f3700b10c8d983dcaee65b73d08eb0693.pnj" data-orig-height="823" data-orig-width="734" srcset="https://64.media.tumblr.com/6b53ed54a5afb118c158d7f51b71f972/dac307d0f7ffc986-09/s1280x1920/260f373f3700b10c8d983dcaee65b73d08eb0693.pnj 734w" sizes="(max-width: 734px) 100vw, 734px"></figure></div>

En ese ejemplo agregamos un usuario para la audiencia Prod. De esa forma el usuario puede buscar la app en el menú de apps:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="339" data-orig-width="474"><img src="https://64.media.tumblr.com/6a00214096bc7c72eeb35e896f04fb59/dac307d0f7ffc986-27/s500x750/025bd259a85f5b6d6e1681b830fa07c11db22063.pnj" data-orig-height="339" data-orig-width="474" srcset="https://64.media.tumblr.com/6a00214096bc7c72eeb35e896f04fb59/dac307d0f7ffc986-27/s500x750/025bd259a85f5b6d6e1681b830fa07c11db22063.pnj 474w" sizes="(max-width: 474px) 100vw, 474px"></figure></div>

Cuando lo abra verá solo lo que le hemos permitido:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="653" data-orig-width="1138"><img src="https://64.media.tumblr.com/b9236305edab80e7c6742086c07044df/dac307d0f7ffc986-40/s1280x1920/72ac154fa83d5578541785cc38b592fa2094bf6a.pnj" data-orig-height="653" data-orig-width="1138" srcset="https://64.media.tumblr.com/b9236305edab80e7c6742086c07044df/dac307d0f7ffc986-40/s1280x1920/72ac154fa83d5578541785cc38b592fa2094bf6a.pnj 1138w" sizes="(max-width: 1138px) 100vw, 1138px"></figure></div>

Así llegamos al final de la configuración. Con esto bastará para que puedan comenzar a distribuir sus contenidos mediante esta metodología que es fantástica para integración continua. Las audiencias serán visibles como sub ítems en el área de trabajo:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="486" data-orig-width="798"><img src="https://64.media.tumblr.com/396d2443c197191c108dd10abfe71716/dac307d0f7ffc986-3e/s1280x1920/007c4a3a598d18dbc22827fbab6aecb41f951d20.pnj" data-orig-height="486" data-orig-width="798" srcset="https://64.media.tumblr.com/396d2443c197191c108dd10abfe71716/dac307d0f7ffc986-3e/s1280x1920/007c4a3a598d18dbc22827fbab6aecb41f951d20.pnj 798w" sizes="(max-width: 798px) 100vw, 798px"></figure></div>

Espero que esta nueva característica les sea tan útil como a mi. Me parece que era una gran deuda que termina de dar un ciclo fantástico para mejores prácticas en Power Bi.

