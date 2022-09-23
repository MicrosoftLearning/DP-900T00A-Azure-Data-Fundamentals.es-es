---
title: Instrucciones hospedadas en línea
permalink: index.html
layout: home
ms.openlocfilehash: 928f59a9cdc6a3f70d5ad651fb1b5a45b405cee8
ms.sourcegitcommit: 1117342052bce0bbd5a703bd1f763862b9129807
ms.translationtype: HT
ms.contentlocale: es-ES
ms.lasthandoff: 03/16/2022
ms.locfileid: "140682431"
---
# <a name="azure-data-fundamentals-exercises"></a>Ejercicios de Aspectos básicos de los datos en Microsoft Azure

Use los vínculos siguientes para completar los ejercicios del laboratorio práctico del curso [DP-900 *Aspectos básicos de los datos en Microsoft Azure*](https://docs.microsoft.com/learn/certifications/courses/dp-900t00).

Para completar estos ejercicios, necesitará una suscripción a Microsoft Azure. Si su instructor no se la ha proporcionado, puede registrarse para obtener una evaluación gratuita en[https://azure.microsoft.com](https://azure.microsoft.com).

{% assign labs = site.pages | where_exp:"page", "page.url contains '/Instructions/Labs'" %}
| Módulo | Laboratorio |
| --- | --- | 
{% for activity in labs  %}| {{ activity.lab.module }} | [{{ activity.lab.title }}{% if activity.lab.type %} - {{ activity.lab.type }}{% endif %}]({{ site.github.url }}{{ activity.url }}) |
{% endfor %}
