---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
---

# <a name="azure-data-fundamentals-exercises"></a>Ejercicios de Aspectos básicos de los datos en Microsoft Azure

Estos ejercicios prácticos están diseñados para admitir contenido de formación de [Microsoft Learn](https://docs.microsoft.com/training/).

Para completar estos ejercicios, necesitará una suscripción de Microsoft Azure en la que tenga permisos administrativos. Puede registrarse para obtener una prueba gratuita en [https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Ejercicio |
| --- |
{% for activity in labs  %}| [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
