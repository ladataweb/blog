---
layout: post
title: '[PowerBi] OrgApp por audiencias - el compartir definitivo'
date: 2026-07-21 10:00:00 -0300
slug: orgapp-por-audiencias
tags:
- powerbi
- power bi
- power bi tips
- power bi tutorial
- power bi training
- power bi service
- power bi argentina
- power bi cordoba
- ladataweb
description: "Hace tiempo que salió este nuevo ítem en Power Bi que evoluciona las Power Bi Apps convencionales. Sin embargo, había un pendiente clave. Podías crear muchas apps para una sola área de trabajo pero no podíamos compartir a distintas audiencias. ¡La actualización está aquí! en este artículo vamos a mostrar como es la nueva configuración que Org Apps trajo para permitirnos generar grupos de distribución para el contenido que seleccionamos dentro de nuestra app para compartir mejor."
legacy: true
tumblr_id: ''
---

# [PowerBi] OrgApp por audiencias - el compartir definitivo

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

<div class="npf_row"><figure class="tmblr-full" data-orig-height="488" data-orig-width="753"><img src="https://64.media.tumblr.com/b07936d77a9057234f094a20d91ff064/23e3be54faa404d3-9c/s540x810/92b0f76430469e7fb1c98a9f0bc41ea9a17249ee.png" data-orig-height="488" data-orig-width="753" srcset="https://64.media.tumblr.com/b07936d77a9057234f094a20d91ff064/23e3be54faa404d3-9c/s540x810/92b0f76430469e7fb1c98a9f0bc41ea9a17249ee.png 753w" sizes="(max-width: 753px) 100vw, 753px"></figure></div>

La nueva pantalla nos mostrará una audiencia por defecto, pero nosotros vamos a dar click en "Nueva Audiencia" que nos pedirá ponerle un nombre.

Desde éste menú de audiencias nos dirigimos a la nueva pestaña que se creo y podemos seleccionar específicamente los elementos que verán, en mi ejemplo se llama "Prod":

<div class="npf_row"><figure class="tmblr-full" data-orig-height="515" data-orig-width="650"><img src="https://64.media.tumblr.com/94de60bf1352f78ffaf8367f9353054d/23e3be54faa404d3-2b/s540x810/114e039c96695bb320c18a72a1cc524932fc29c3.png" data-orig-height="515" data-orig-width="650" srcset="https://64.media.tumblr.com/94de60bf1352f78ffaf8367f9353054d/23e3be54faa404d3-2b/s540x810/114e039c96695bb320c18a72a1cc524932fc29c3.png 650w" sizes="(max-width: 650px) 100vw, 650px"></figure></div>

Para compartirla, volvemos al menú convencional de la Org App que nos permitía agregar contenido. Recuerden que este es otro ítem en fabric, entonces lo compartimos desde el botón compartir de arriba a la derecha:

<figure data-orig-height="184" data-orig-width="260"><img src="https://64.media.tumblr.com/d1530aba623e828ec0473a5d2590bc06/23e3be54faa404d3-e8/s540x810/2674dc2a65201a18bb955df56045b9cd58625a21.png" data-orig-height="184" data-orig-width="260" srcset="https://64.media.tumblr.com/d1530aba623e828ec0473a5d2590bc06/23e3be54faa404d3-e8/s540x810/2674dc2a65201a18bb955df56045b9cd58625a21.png 260w" sizes="(max-width: 260px) 100vw, 260px"></figure>

El menú convencional de compartir cambió y nos permite elegir la audiencia:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="823" data-orig-width="734"><img src="https://64.media.tumblr.com/6b53ed54a5afb118c158d7f51b71f972/23e3be54faa404d3-e5/s540x810/840caa28bc07a2c97e5d29e40f6d581b40bc5b74.png" data-orig-height="823" data-orig-width="734" srcset="https://64.media.tumblr.com/6b53ed54a5afb118c158d7f51b71f972/23e3be54faa404d3-e5/s540x810/840caa28bc07a2c97e5d29e40f6d581b40bc5b74.png 734w" sizes="(max-width: 734px) 100vw, 734px"></figure></div>

En ese ejemplo agregamos un usuario para la audiencia Prod. De esa forma el usuario puede buscar la app en el menú de apps:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="339" data-orig-width="474"><img src="https://64.media.tumblr.com/6a00214096bc7c72eeb35e896f04fb59/23e3be54faa404d3-c3/s540x810/e5e32f517e85740e6620568f3cb88207e25997eb.png" data-orig-height="339" data-orig-width="474" srcset="https://64.media.tumblr.com/6a00214096bc7c72eeb35e896f04fb59/23e3be54faa404d3-c3/s540x810/e5e32f517e85740e6620568f3cb88207e25997eb.png 474w" sizes="(max-width: 474px) 100vw, 474px"></figure></div>

Cuando lo abra verá solo lo que le hemos permitido:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="653" data-orig-width="1138"><img src="https://64.media.tumblr.com/b9236305edab80e7c6742086c07044df/23e3be54faa404d3-9d/s540x810/cbfec622b116b6f7b34b6308bc967969412fa37e.png" data-orig-height="653" data-orig-width="1138" srcset="https://64.media.tumblr.com/b9236305edab80e7c6742086c07044df/23e3be54faa404d3-9d/s540x810/cbfec622b116b6f7b34b6308bc967969412fa37e.png 1138w" sizes="(max-width: 1138px) 100vw, 1138px"></figure></div>

Así llegamos al final de la configuración. Con esto bastará para que puedan comenzar a distribuir sus contenidos mediante esta metodología que es fantástica para integración continua. Las audiencias serán visibles como sub ítems en el área de trabajo:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="486" data-orig-width="798"><img src="https://64.media.tumblr.com/396d2443c197191c108dd10abfe71716/23e3be54faa404d3-97/s540x810/9af3c08bae0955872e94805578504420846eab2d.png" data-orig-height="486" data-orig-width="798" srcset="https://64.media.tumblr.com/396d2443c197191c108dd10abfe71716/23e3be54faa404d3-97/s540x810/9af3c08bae0955872e94805578504420846eab2d.png 798w" sizes="(max-width: 798px) 100vw, 798px"></figure></div>

Espero que esta nueva característica les sea tan útil como a mi. Me parece que era una gran deuda que termina de dar un ciclo fantástico para mejores prácticas en Power Bi.

