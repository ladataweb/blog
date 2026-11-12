---
layout: post
title: '[Fabric] Loguear Semantic Links con service principal'
date: 2026-11-12 07:48:00 -0300
slug: loguear-semantic-links-service-principal
tags:
- fabric semantic-links
- sempy
- fabric training 
- fabric tutorial 
- fabric tips 
- fabric argentina 
- fabric cordoba 
- microsoft fabric
- fabric jujuy 
- ladataweb 
- fabric
description: 'En este artículo vamos a mostrar cómo loguear Semantic Links con service principal en Microsoft Fabric, permitiendo utilizar la librería sempy fuera de los notebooks de Fabric.'
legacy: true
tumblr_id: '833107939148562432'
faqs:
  - q: '¿Es posible loguear Semantic Links con service principal en Microsoft Fabric?'
    a: 'Sí, es posible loguear Semantic Links con service principal en Microsoft Fabric utilizando la librería sempy en Python. Esto permite ejecutar operaciones sobre el ecosistema de Fabric sin depender de un usuario específico, garantizando que los scripts y automatizaciones sigan funcionando incluso si un usuario deja la compañía.'
  - q: '¿Qué es semantic links?'
    a: 'Semantic Links es un concepto dentro del ecosistema de Microsoft Fabric que permite establecer relaciones semánticas entre diferentes ítems, facilitando la automatización y consulta de datos de manera más estructurada y eficiente.'
  - q: '¿Qué es sempy?'
    a: 'Sempy es la librería de Python que permite interactuar con Semantic Links dentro del ecosistema de Microsoft Fabric, facilitando la creación, consulta y gestión de relaciones semánticas entre diferentes ítems de manera programática.'
  - q: '¿Qué puedo hacer con semantic links?'
    a: 'Con Semantic Links podemos establecer relaciones semánticas entre diferentes ítems dentro del ecosistema de Microsoft Fabric. Esto permite automatizar tareas, consultar datos de manera más estructurada y eficiente, y gestionar las relaciones entre los ítems de forma programática utilizando la librería sempy para python.'
---

Hace tiempo que Microsoft ya ha dejado bien marcada su librería para crear, consultar, borrar ítems de su ecosistema. El uso de sempy, la librería de python en cuestión, cada vez gana más popularidad en el ecosistema.

Cierto es que cuando fue lanzada solo podíamos usarla dentro de un Fabric notebook. En este artículo vamos a mostrar que ahora podemos utilizarla en cualquier entorno conectada con service principal.

<!--more-->

Antes de comenzar, vamos a coordinar conceptos. Aquí en La Data Web nos gustan las definiciones, pero como no es un tema nuevo, te sugiero pasar por esto artículo anterior que da una [introducción al tema de semantic links](<https://blog.ladataweb.com.ar/leer-powerbi-data-con-notebooks-semantic-link/>).

## ¿Por qué es importante loguear con Service Principal?

Cualquier solución de automatización de tareas que querramos ejecutar para que usuarios puedan realizar acciones, para que automaticamente sucedan operación, etc, se recomienda hacerlas mediante cuentas que no dependan exclusivamente de un usuario nominado sino más bien de servicio. Esto es así para garantizar que el código siga funcionando aún si el usuario deja la compañia.

## Dos Métodos

Para mostrar este ejemplo vamos a instalar la librería de semantic link con pip en entorno local:

pip install -U semantic-link

Veamos las formas con Jupyter Notebooks. Importamos sempy con Fabric y delimitamos las credenciales para nuestra autenticación

<div class="npf_row"><figure class="tmblr-full" data-orig-height="115" data-orig-width="446"><img src="https://64.media.tumblr.com/69992e8bae1c2d2b8ce8f9bc8bace489/5d6cabd2bdbe7de2-3a/s500x750/6055da10aef94ca69be73e9c4e13bf6930e21f32.pnj" data-orig-height="115" data-orig-width="446" srcset="https://64.media.tumblr.com/69992e8bae1c2d2b8ce8f9bc8bace489/5d6cabd2bdbe7de2-3a/s500x750/6055da10aef94ca69be73e9c4e13bf6930e21f32.pnj 446w" sizes="(max-width: 446px) 100vw, 446px"></figure></div>

**La primera forma** es utilizando la misma librería que trae un módulo de autenticación.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="444" data-orig-width="598"><img src="https://64.media.tumblr.com/b466851e2eb70f36ca8db7ee0d29cbce/5d6cabd2bdbe7de2-53/s640x960/1fa301e9fd7ec487ca52e961a882121c161b521e.pnj" data-orig-height="444" data-orig-width="598" srcset="https://64.media.tumblr.com/b466851e2eb70f36ca8db7ee0d29cbce/5d6cabd2bdbe7de2-53/s640x960/1fa301e9fd7ec487ca52e961a882121c161b521e.pnj 598w" sizes="(max-width: 598px) 100vw, 598px"></figure></div>

**La segunda forma** es algo más tradicional. Utilizar otro módulo que se utiliza en general para autenticaciones de azure. Hablo de azure identity. Usando esta técnica no necesitamos ejecutar "with" sino más bien incluir un parámetro de credenciales en cada método de función que usemos.

<div class="npf_row"><figure class="tmblr-full" data-orig-height="519" data-orig-width="684"><img src="https://64.media.tumblr.com/4ad2f5e0bbaca451394088c8bd1ae3b6/5d6cabd2bdbe7de2-8e/s1280x1920/4ee0842da772bf152baec71206bd650902fe632a.pnj" data-orig-height="519" data-orig-width="684" srcset="https://64.media.tumblr.com/4ad2f5e0bbaca451394088c8bd1ae3b6/5d6cabd2bdbe7de2-8e/s1280x1920/4ee0842da772bf152baec71206bd650902fe632a.pnj 684w" sizes="(max-width: 684px) 100vw, 684px"></figure></div>

Bajo estas dinámicas podemos ejecutar los métodos del módulo fabric para conocer los detalles de items, crearlos, obtener información, etc. logueando con service principal en cualquier entorno y no necesariamente dentro de fabric notebooks.

Hoy en noviembre 2026 existen algunas limitación para service principal que son los métodos:

- sempy.fabric.list\_apps
- sempy.fabric.list\_dataflow\_storage\_accounts
- sempy.fabric.evaluate\_measure
- sempy.fabric.read\_table(..., mode='rest')
- sempy.fabric.execute\_tmsl

<!-- -->

