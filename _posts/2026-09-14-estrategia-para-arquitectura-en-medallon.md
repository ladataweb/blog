---
layout: post
title: '[Fabric] Estrategias para arquitectura en medallón'
date: 2026-09-14 07:48:00 -0300
slug: estrategia-para-arquitectura-en-medallon
tags:
- microsoft fabric
- fabric lakehouse
- data engineering
- fabric warehouse 
- fabric training 
- fabric tutorial 
- fabric tips 
- fabric argentina 
- fabric cordoba 
- fabric jujuy 
- ladataweb 
- fabric
description: 'En este artículo vamos a conversar sobre estrategias para implementar la arquitectura en medallón tradicional de delta lake dentro de Microsoft Fabric.'
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

Si están por iniciar un camino por Fabric o ya tenían una estructura y quieren migrar, seguramente pensaron en como organizar la famosa arquitectura. Siempre pienso que me encantaría tener en ese momento un pizarrón y dibujar.

En este artículo vamos a conversar sobre formas para trabajarlo, pueden responderse preguntas respecto a usar lakehouse, warehouse, que tiene uno que otro no, etc.

<!--more-->

Asumiendo que ya conocen conceptos básicos de Fabric, [sino pasar por aquí](<https://blog.ladataweb.com.ar/intro-a-microsoft-fabric-la-solucion-data-platform-todo-en-uno/>), me gustaría comenzar alineando definiciones, tal como solemos hacer en LaDataWeb.

La arquitectura en medallón fue propuesta hace bastante tiempo y nace con la tecnología de data lake. La idea es que los datos se organicen en tres capas o etiquetas que denoten la madurez de los datos.

- **Bronze**: datos lo más crudo posibles. Evitar transformaciones que afecten la tabla original. Solo permitido el cambio de formato.
- **Silver**: datos limpios, transformados o enriquecidos.
- **Gold**: datos curados, modelados y listos para consumo analítico.

<!-- -->

Tal como estarán pensando, son solo nombres que se usan hoy. No es distinto a muchos años atrás cuando decíamos landing, staging, dwh o alguna otra que conozcan.

> *NOTA: la teoría original hablaba de tenerlos curados en silver y que gold sea una capa más fina de agrupaciones. Pero la realidad es que gold conlleva en la realidad a dejar los hechos y dimensiones más perfectas para ser leídos por herramientas de reporting.*

## Enfoque inicial

Hay mucha concentración sobre la tecnología a usar. Hoy grandes marcas están pudiendo avisar que ese no es el enfoque puesto que la tecnología converge muy bien. La propuesta es pensar en otro foco:

- ¿Qué stack tecnológico conoce el equipo de datos? ¿Hay más afinidad por python/spark o por SQL?
- ¿Qué experiencia o interés tienen los involucrados? puede que hoy trabajen con un lenguaje pero tengan años de experiencia en otro. El interés puede ser un desafío en caso de querer algo tradicional o la búsqueda de lo nuevo.
- ¿Quiénes van a explotar los datos? los analistas, cientificos, AI Engineers, desarrolladores u otros terceros son importantes. ¿Cómo van a consumir datos? ¿Qué afinidad tienen?

<!-- -->

La respuesta a estas preguntas suele mover automáticamente la balanza a una de las soluciones. Recorda que el trasfondo no cambiará mucho. Los datos van a vivir en OneLake con formato Delta Parquet de una forma u otra. Lo que estamos eligiendo es el motor la experiencia de tus usuarios en cada capa.

## Diferencias entre lakehouse y warehouse

| Aspecto                       | Lakehouse                                                                 | Warehouse                                                                 |
| ----------------------------- | ------------------------------------------------------------------------- | ------------------------------------------------------------------------- |
| **Motor principal**           | Spark (Fabric o Databricks notebooks en Python/Scala/SQL)                 | Podrían usarse notebooks, pero se eligen por T-SQL (motor SQL relacional) |
| **Escritura**                 | Spark, Dataflows, pipelines                                               | T-SQL (`INSERT`/`UPDATE`/`DELETE`), pipelines                             |
| **Manejo de archivos**        | Apartado Files para archivos (raw/prelanding)                             | No soportado                                                              |
| **Experiencia SQL**           | Solo lectura vía SQL Analytics Endpoint                                   | Lectura y escritura completas                                             |
| **DDL / DML en T-SQL**        | No podés hacer `CREATE TABLE`, `UPDATE` ni `DELETE` desde el endpoint SQL | Soportado de forma nativa                                                 |
| **Stored procedures**         | Ejecución no soportada                                                    | Creación y ejecución soportadas                                           |
| **Transacciones multi-tabla** | Limitadas (a nivel Spark/Delta)                                           | ACID multi-tabla en T-SQL                                                 |
| **Esquema motor**             | Esquemas SQL en sección Tables                                            | Esquemas SQL                                                              |
| **Perfil de equipo ideal**    | Ingeniería de datos orientada a Spark                                     | Equipos SQL-first / BI                                                    |


## **Arquitectura 1: solo lakehouse**

Un ítem de Fabric por capa para un mejor mantenimiento y separación de permisos/seguridad. Utilizando la tecnología delta parquet cada lakehouse (bronze, silver o gold) se construye sobre Tables. En caso de manejo de archivos complejos, incrementales, etc, podemos usar Files de bronze con un raw o prelanding que maneje patrones especiales.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="2036" data-orig-width="3176"><img src="https://64.media.tumblr.com/22bbab187af4a0d5831a914186ef3c0e/23e3be54faa404d3-3d/s540x810/8aebf22b3f9eb197630f3ad537e441a5f2f8f2ba.png" data-orig-height="2036" data-orig-width="3176" srcset="https://64.media.tumblr.com/22bbab187af4a0d5831a914186ef3c0e/23e3be54faa404d3-3d/s540x810/8aebf22b3f9eb197630f3ad537e441a5f2f8f2ba.png 3176w" sizes="(max-width: 1280px) 100vw, 1280px"></figure></div>

Inclinación: Elegí este camino si tu equipo de ingeniería de datos tiene una fuerte orientación a python o spark. Si su idea es que el ecosistema de datos mantenga una filosofía de mis prácticas para manipulación del dato de punta a punta. Cuando tu equipo tiene que validar un dato, si elije usar notebook antes que una consulta SQL directa. Si la mayor cantidad de usuarios explotando datos son científicos de datos. Permite trabajar con datos no estructurados y semi estructurados.

## **Arquitectura 2: híbrida lakehouse y warehouse convivendo**

Últimamente parece que equipos pretenden trabajar en un ranchito con una tecnología y un solo entorno. La tecnología se engrandece en la diversidad. Pienso que puede ser la mejor opción bien implementada o una perdición. Tenemos dos certezas y una definición. El dato crudo aterriza en lakehouse tables bronze y el modelado de hechos y dimensiones ocurre en warehouse gold. La balanza a definir estará en silver y dependerá de las preguntas antes mencionadas. La balanza inclinaría apenas a un lugar, si notebooks limpian silver o procedimientos almacenados. Yo prefiero silver en lakehouse.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="2016" data-orig-width="3176"><img src="https://64.media.tumblr.com/5558b764760e17dd4f916add6523d9d3/23e3be54faa404d3-51/s540x810/2f63f6d688e2abc2eb0af5b4e49339c30433e49f.png" data-orig-height="2016" data-orig-width="3176" srcset="https://64.media.tumblr.com/5558b764760e17dd4f916add6523d9d3/23e3be54faa404d3-51/s540x810/2f63f6d688e2abc2eb0af5b4e49339c30433e49f.png 3176w" sizes="(max-width: 1280px) 100vw, 1280px"></figure></div>

Inclinación: este patrón acompaña a profesionales que buscan excelencia en siempre estar al día con la tecnología y no quedarse en la comodidad como así también equipos con gran diversidad, BI muy marcados e Ingenieros de datos muy python. Clave si quienes explotan los datos tienen las habilidades muy identificados, ejemplo científicos de datos expertos en python y BIs en SQL. Analistas híbridos. ¿Complejidad? dos skillsets para mantener, si cuentas con un equipo chico, podrías perder a las personas que conocen más de un mundo. Permite trabajar con datos no estructurados y semi estructurados.

## **Arquitectura 3: usar solo warehouse no es de dinosaurio**

El concepto de lake sonó tan fuerte en el mercado que parecería que las personas no ponen en duda su existencia. Construir una arquitectura a puro warehouse lleva más de 30 años funcionando de maravilla y no veo porque dejaría de hacerlo. Se crean tres data warehouses en una misma área de trabajo. La experiencia SQL de los datawarehouse permite la comunicación con cada endpoint dentro del área. Los procedimientos almacenados pueden llamar tablas de cualquier otro warehouse bajo el prefijo Bronze.[esquema].[tabla], Silver.[esquema].[tabla], Gold.[esquema].[tabla].

<div class="npf_row"><figure class="tmblr-full" data-orig-height="2016" data-orig-width="3176"><img src="https://64.media.tumblr.com/2b484a0038ba9d5264615ac7e7ed1567/23e3be54faa404d3-6d/s540x810/13e6069a0461c87878ac57105bdd9bad171233af.png" data-orig-height="2016" data-orig-width="3176" srcset="https://64.media.tumblr.com/2b484a0038ba9d5264615ac7e7ed1567/23e3be54faa404d3-6d/s540x810/13e6069a0461c87878ac57105bdd9bad171233af.png 3176w" sizes="(max-width: 1280px) 100vw, 1280px"></figure></div>

Inclinación: si tu negocio tiene años de experiencia en SQL y las persona del equipo o terceros sabe como ejecutar consultas este patrón es ideal. El skill set lo tienen hace mucho, cambian motores y velocidad pero la experiencia se mantiene. Aunque este patrón trabaje exclusivamente con datos estructurados, no lo hace malo puesto que tal vez el negocio no tenga ninguna intención de analizar no estructurados.

## Consideraciones adicionales

Un nuevo factor que viene a golpear la mesa es el anuncio que Fabric Data Warehouses ahora usaría aceleración por GPU. Mencionan un motor que acelera consultas SQL activado desde la configuración del workspace. Según benchmarks de mercado, se reportó que es 7 veces más rápido contra tres proveedores actuales. Sin duda un factor fuerte en escenarios de gran concurrencia de usuarios o de Agentes.

## ¿Ganador?

No existe una solución perfecta. Existen alternativas que se adaptan a las audiencias. Lo importante es que la tecnología de fondo acompañe en esta modernización, es decir, utilizar formatos de alta velocidad actuales como Delta Parquet.

