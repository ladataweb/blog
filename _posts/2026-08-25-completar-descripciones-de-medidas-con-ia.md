---
layout: post
title: '[PowerBi][TMDL] Completar descripciones de Medidas con IA'
date: 2026-08-25 07:48:00 -0300
slug: completar-descripciones-de-medidas-con-ia
tags:
- power bi
- TMDL
- power bi tutorial 
- power bi training 
- power bi tips 
- power bi python
- power bi argentina 
- power bi cordoba 
- power bi jujuy 
- ladataweb 
description: 'En este articulo vamos a mostrar como completar las descripciones de las medidas de un modelo semántico usando una IA gratuita y TMDL.'
legacy: true
tumblr_id: '825956504089886720'
faqs:
  - q: '¿Qué es TMDL y para qué sirve?'
    a: 'TMDL es una herramienta que nos permite manipular la estructura de un modelo por scripting. Un ejemplo es completar las descripciones de las medidas de un modelo semántico utilizando inteligencia artificial. En este artículo vamos a mostrar cómo usar TMDL junto con una IA gratuita para mejorar la documentación de nuestro modelo.'
  - q: '¿Qué es una medida en Power BI y por qué es importante su descripción?'
    a: 'Una medida en Power BI es un cálculo que se realiza sobre los datos de un modelo semántico, generalmente utilizando el lenguaje DAX. La descripción de una medida es importante porque proporciona contexto y claridad sobre lo que la medida calcula, lo que facilita la comprensión y el uso del modelo por parte de otros usuarios o desarrolladores. Una buena descripción mejora la mantenibilidad del modelo y ayuda a generar documentación automática más efectiva.'
  - q: '¿Cómo puedo completar las descripciones de las medidas de mi modelo semántico usando IA y TMDL?'
    a: 'Para completar las descripciones de las medidas de tu modelo semántico usando IA y TMDL, primero necesitas exportar el modelo a un archivo JSON. Luego, puedes utilizar una IA gratuita para generar descripciones basadas en el contenido del JSON. Finalmente, con TMDL scripting, puedes actualizar las descripciones de las medidas en el modelo de manera automatizada.'
---

No es una novedad utilizar IA para apoyar nuestros desarrollos y mucho menos para descripciones cuando tenemos licencia de Fabric. Inclusive en este blog escribimos dos artículos sobre como autocompletar descripciones.

La diferencia de este artículo es que no necesitamos APIs pagas, ni Fabric pago, basta con una IA gratuita en la que confies y la pestaña de TMDL scripting.

<!--more-->

No esta demás aclarar porque esto es importante. Lo voy a hacer al principio y no al final. Las descripciones de las medidas son fundamentales para mejorar la semántica del modelo. El código de descriptivo fortalece la mantenibilidad y ayudará a generar mejor documentación automática de cualquier herramienta. En nuestro caso, hemos publicado esta [metodología para documentar un modelo publicado en el servicio con SimplePBI](<https://blog.ladataweb.com.ar/documentar-modelo-semantico-automaticamente/>).

Para comenzar necesitamos de dos características importantes para la IA que vamos a hacer.

1. Seguridad. Si bien la probabilidad de que nuestras medidas tengan datos sensibles es baja, siempre es importante considerar donde estamos ejecutando esto.
2. Una IA que permita muchos caracteres de entrada o lectura de un archivo json.

<!-- -->

En mi caso voy a utilizar el copilot gratuito de 365 que viene con las organizaciones.

Antes ejecutabamos un Script de C# llamando una IA. Ahora vamos a ir por un ligero cambio de mentalidad. Vamos a obtener información de las medidas en json y lo usaremos de entrada para pedirle a la IA que nos escriba un script de TMDL agregando descripciones.

Comencemos obteniendo nuestro json con la información de medidas. Abrimos la vista de consultas DAX y ejecutamos lo siguiente:

EVALUATE { <br>

 TOJSON(<br>

 SELECTCOLUMNS(<br>

 INFO.VIEW.MEASURES(), "Table", [Table], "Name", [Name], "Expression", [Expression], "DataType", [DataType]<br>

 ),-1<br>

 ) <br>

}

Veremos una devolución algo así:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="337" data-orig-width="897"><img src="https://64.media.tumblr.com/78a013d6b82515881f4b788ae5bfe5f4/536c8928684fdd7a-eb/s1280x1920/e1ae7148521e789cba3d497f2ffd2f2c363ba09e.pnj" data-orig-height="337" data-orig-width="897" srcset="https://64.media.tumblr.com/78a013d6b82515881f4b788ae5bfe5f4/536c8928684fdd7a-eb/s1280x1920/e1ae7148521e789cba3d497f2ffd2f2c363ba09e.pnj 897w" sizes="(max-width: 897px) 100vw, 897px"></figure></div>

Copiamos esa celda. La celda tiene la respuesta a una consulta al modelo que pide información de las medidas. Podemos ver en el código las columnas que usaremos para que el LLM pueda interpretar de donde viene, el nombre, código y tipo de datos.

A partir de eso podemos utilizar un prompt como el que les dejo aquí para pedirle a la IA el script de TMDL:

> Actúa como un experto en Power BI, Modelado de Datos Avanzado y scripting en TMDL (Tabular Model Definition Language).<br>
> 
> Objetivo:<br>
> 
> Necesito que generes un script TMDL para actualizar las descripciones de varias medidas en mi modelo semántico a partir de un fragmento de código JSON que te proporcionaré.<br>
> 
> Instrucciones:<br>
> 
> 1\. Analiza el JSON: Lee el nombre, la tabla a la que pertenece, el código DAX y el tipo de datos de cada medida.<br>
> 
> 2\. Interpreta la lógica: Analiza el código DAX de cada medida para entender qué calcula exactamente (por ejemplo: variaciones año a año, acumulados, promedios, filtros específicos).<br>
> 
> 3\. Redacta la descripción: Escribe una descripción clara, profesional y concisa (en español) que explique qué hace la medida.<br>
> 
> 4\. Genera el código TMDL: Utiliza la sintaxis correcta de TMDL para modificar la propiedad 'description' de la medida dentro de su respectiva tabla.<br>
> 
> Formato de salida requerido en TMDL:<br>
> 
> Debes agrupar las medidas por su respectiva tabla utilizando la estructura de bloques de TMDL. Por ejemplo:<br>
> 
> createOrReplace<br>
> 
>  ref table 'NombreDeLaTabla'<br>
> 
>  /// Aquí va la descripción generada por la IA.<br>
> 
>  measure 'NombreMedida1' = [Código DAX]<br>
> 
>  /// Aquí va la otra descripción generada.<br>
> 
>  measure 'NombreMedida2' = [Código DAX]<br>
> 
>  ref table 'NombreDeLaTabla2'<br>
> 
>  /// Aquí va la descripción generada por la IA.<br>
> 
>  measure 'NombreMedida3' = [Código DAX]<br>
> 
> Reglas estrictas:<br>
> 
> \- Respeta la indentación por tabulaciones nativa de TMDL.<br>
> 
> \- No inventes lógica; básate estrictamente en el código DAX provisto.<br>
> 
> \- Devuelve solo los bloques de código TMDL correspondientes.<br>
> 
> Aquí está mi JSON de entrada:

Si pegan el código de json abajo dejen el texto "Aquí está mi json de entrada:". En caso de que no puedan completar porque la interfaz tiene límite de caracteres. Deberán pegar el texto de la consulta DAX a un editor de texto y guardar como json. Si no los deja guardar como json. Guarden como txt, luego editan el nombre del archivo reemplazando archivo.txt por archivo.json

Adjuntan el archivo y pegan el prompt sin la aclaración del json de entrada.

La IA debería generarles un script que puedan copiar y pegar en Power Bi Desktop o Service.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="563" data-orig-width="869"><img src="https://64.media.tumblr.com/011a8dd3a27f8f230e7eebf5e0826574/536c8928684fdd7a-75/s1280x1920/db166ddab7ea03ef1fd05105f2144304ea16104a.pnj" data-orig-height="563" data-orig-width="869" srcset="https://64.media.tumblr.com/011a8dd3a27f8f230e7eebf5e0826574/536c8928684fdd7a-75/s1280x1920/db166ddab7ea03ef1fd05105f2144304ea16104a.pnj 869w" sizes="(max-width: 869px) 100vw, 869px"></figure></div>

Revisan si hay alguna tabulación o problema que detecte el motor para corregir. Dependiendo del generador pueden haber pequeñas imperfecciones.

Al terminar dan en Aplicar o Apply:

<div class="npf_row"><figure class="tmblr-full" data-orig-height="468" data-orig-width="1071"><img src="https://64.media.tumblr.com/1ff4dba44cffb76c106ee869a164f7ed/536c8928684fdd7a-23/s1280x1920/f9d76fdca6a6b781ded879130fe7e95e903af105.pnj" data-orig-height="468" data-orig-width="1071" srcset="https://64.media.tumblr.com/1ff4dba44cffb76c106ee869a164f7ed/536c8928684fdd7a-23/s1280x1920/f9d76fdca6a6b781ded879130fe7e95e903af105.pnj 1071w" sizes="(max-width: 1071px) 100vw, 1071px"></figure></div>

¡Listo! sin suscripciones pagas de IAs pueden obtener las descripciones para el modelo. Ya no hay excusas para tener los modelos con mejor semántica.

