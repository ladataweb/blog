---
layout: post
title: '[PowerBi] Totales de medidas en dos clicks'
date: 2026-07-21 15:48:00 -0300
slug: totales-de-medidas-en-dos-clicks
tags:
- power bi
- powerbi 
- power bi argentina 
- power bi cordoba 
- power bi jujuy 
- power bi tutorial 
- power bi training 
- power bi tips 
- ladataweb 
- power bi desktop 
- DAX
description: 'Si hay algo que ha perseguido a Power Bi por mucho tiempo, son los totales de las medidas. Para mi nunca estuvieron mal, pero no es sencillo explicarle a un usuario final que esa medida super compleja no esta sumando para el total, sino que hace la operación sobre el total. En este artículo hablamos de la nueva característica de Power Bi que esta sugiriendo acompañar de forma más simple el desarrollo para la experiencia de usuarios. Estoy seguro que alguna vez les pasó que ante una medida complicada el total que mostraba una tabla'
legacy: true
tumblr_id: '822221552506535936'
faqs:
  - q: '¿Qué es un total personalizado en Power BI?'
    a: 'Un total personalizado en Power BI es una característica que permite a los usuarios definir cómo se calcula el total de una medida en una tabla o matriz. En lugar de usar la agregación predeterminada, como la suma, los usuarios pueden elegir otras funciones de agregación como promedio, máximo, mínimo, recuento distinto o recuento de filas mostradas.'
  - q: '¿Cómo se puede cambiar el total de una medida en Power BI?'
    a: 'Para cambiar el total de una medida en Power BI, se puede hacer clic derecho sobre el total en la visualización o sobre el campo de las columnas donde se hace el gráfico, seleccionar "Customize Total" y elegir la agregación deseada (SUM, MAX, MIN, AVERAGE, etc.).'
  - q: '¿Cómo arreglar el total de una medida dax en Power BI?'
    a: 'Para arreglar el total de una medida DAX en Power BI, se puede hacer clic derecho sobre el total en la visualización o sobre el campo de las columnas donde se hace el gráfico, seleccionar "Customize Total" y elegir la agregación deseada (SUM, MAX, MIN, AVERAGE, etc.).'
---

# [PowerBi] Totales de medidas en dos clicks

Si hay algo que ha perseguido a Power Bi por mucho tiempo, son los totales de las medidas. Para mi nunca estuvieron mal, pero no es sencillo explicarle a un usuario final que esa medida super compleja no esta sumando para el total, sino que hace la operación sobre el total.

En este artículo hablamos de la nueva característica de Power Bi que esta sugiriendo acompañar de forma más simple el desarrollo para la experiencia de usuarios.

Estoy seguro que alguna vez les pasó que ante una medida complicada el total que mostraba una tabla no era el esperado. Algo similar a la siguiente imagen:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="427" data-orig-width="361"><img src="https://64.media.tumblr.com/80fbf15673e3954aecf17656f6eb3b3a/ba8cbde35daa4687-bb/s400x600/94d1e91e17dcb9cace0fea7be26f160e93c3440d.pnj" data-orig-height="427" data-orig-width="361" srcset="https://64.media.tumblr.com/80fbf15673e3954aecf17656f6eb3b3a/ba8cbde35daa4687-bb/s400x600/94d1e91e17dcb9cace0fea7be26f160e93c3440d.pnj 361w" sizes="(max-width: 361px) 100vw, 361px"></figure></div>

Proyección es una predicción matematica de cuanto se esperaba para esa fecha. Sin embargo, la medida es tan compleja que el 13.271 del final no es la suma de ventas del producto en si. Esto confunde al usuario.

Antes de hoy, para solucionarlo, ejecutabamos una segunda medida que hiciera un precalculo por cada fila y un SUMX. Algo que vimos en un artículo de [como corregir totales con DAX](<https://ibarrau.tumblr.com/post/636304907248467968/dax-corrigiendo-el-total-de-una-medida>). A partir de hoy tenemos, "Totales Personalizados" o "Custom Totals".

Comencemos definiendo como nos gusta hacer en LaDataWeb el concepto según Microsof

> *Con los totales personalizados en tablas y matrices de Power BI, puede determinar fácilmente lo que muestra la fila total para una columna específica si es necesario.*<br>
> 
> *De forma predeterminada, la fila total muestra el resultado de evaluar el campo en todo el contexto de filtro de la página del informe. Este comportamiento es correcto en la mayoría de los casos. Sin embargo, en algunos escenarios específicos es posible que desee cambiar lo que muestra la fila total. Puede usar DAX para influir en lo que muestra la fila de totales, pero los totales personalizados ofrecen una forma sencilla de cambiar el valor de la fila de totales a la suma, la media, el mínimo, el máximo, el recuento distinto o el recuento de las filas mostradas.*

Basicamente, podemos cambiar la agregación de un total por una de las básicas, como por ejemplo SUM, MAX, MIN, AVERAGE.

Para ello basta con hacer click derecho sobre un total en la visual o sobre el campo de las columnas donde se hace el gráfico, elegir Customize Total y elegir la agregación que deseamos.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="574" data-orig-width="850"><img src="https://64.media.tumblr.com/b8b6a4d73f09f8b2a9b22b0fca07fe70/ba8cbde35daa4687-8b/s1280x1920/be91940c511be198f4ba507ce882a38f5ddd4815.pnj" data-orig-height="574" data-orig-width="850" srcset="https://64.media.tumblr.com/b8b6a4d73f09f8b2a9b22b0fca07fe70/ba8cbde35daa4687-8b/s1280x1920/be91940c511be198f4ba507ce882a38f5ddd4815.pnj 850w" sizes="(max-width: 850px) 100vw, 850px"></figure></div>

Fácilmente obtenemos el resultado esperado que podemos compararlo con la medida que hicimos que hace SUMX del contexto.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="439" data-orig-width="904"><img src="https://64.media.tumblr.com/51824e39c052a44b2c7336059cb5c18c/ba8cbde35daa4687-b4/s1280x1920/fd793c7d6a30a99eebe618ff24696b8a41a66a6a.pnj" data-orig-height="439" data-orig-width="904" srcset="https://64.media.tumblr.com/51824e39c052a44b2c7336059cb5c18c/ba8cbde35daa4687-b4/s1280x1920/fd793c7d6a30a99eebe618ff24696b8a41a66a6a.pnj 904w" sizes="(max-width: 904px) 100vw, 904px"></figure></div>

Se crea un ícono en nuestro gráfico para avisarnos que su total fue modificado y ahora podemos ver que coincide con el total esperado.

¿Qué ocurre por detrás? el motor genera una visual calculation, es decir, una medida que solo existe en esa visualización y no dentro del modelo. Antes podíamos generarla nosotros, pero ahora ambas features conviven para simplificarnos los totales.

Así rápidamente tenemos un total modificable para el usuario final que no represente grandes cambios para un desarrollador.

