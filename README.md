\# Diseño y validación de resultados de Liga MX con XML y DTD



\## Propósito

Diseñar un formato XML para representar información estructurada de partidos de fútbol y definir mediante un DTD las reglas que deben cumplir los documentos, demostrando el uso de elementos, atributos, cardinalidades y la diferencia entre un XML bien formado y uno válido.



\---



\## Actividad 1 - Analizar la información

\*\*Preguntas de análisis:\*\*

1\. \*\*¿Cuál debería ser el elemento raíz?\*\* `<liga>`.

2\. \*\*¿Una jornada puede contener varios partidos?\*\* Sí, una jornada funciona como contenedor para agrupar todos los encuentros de esa fecha.

3\. \*\*¿Cada partido debe contener exactamente dos equipos?\*\* Sí, siempre debe existir un equipo local y un equipo visitante.

4\. \*\*¿Cómo distinguirían al equipo local del visitante?\*\* Utilizando dos elementos XML distintos con nombres descriptivos: `<equipoLocal>` y `<equipoVisitante>`.

5\. \*\*¿El marcador debe representarse como un solo dato o separar los goles?\*\* Separado. Cada equipo debe tener su propio elemento `<marcador>` anidado.

6\. \*\*¿Las estadísticas pertenecen al partido o a cada equipo?\*\* Pertenecen a cada equipo de manera individual (cada uno tiene su propia posesión, tiros, faltas, etc.).

7\. \*\*¿Qué datos son obligatorios?\*\* Los equipos involucrados, los marcadores, la fecha/número de la jornada, el ID del partido y el estado del encuentro.

8\. \*\*¿Cuáles podrían ser opcionales?\*\* Las estadísticas detalladas (posesión, tiros, faltas), ya que a veces esta información no está disponible inmediatamente.



\---



\## Actividad 2 - Diseñar el modelo conceptual



| Información | Elemento/Atributo | Justificación |

| :--- | :--- | :--- |

| Jornada | Elemento | Es un contenedor complejo que agrupará a múltiples partidos. |

| Fecha | Atributo | Es un metadato corto, simple y descriptivo propio de la jornada. |

| ID del partido | Atributo | Es un identificador único y corto que sirve como clave del elemento partido. |

| Equipo local | Elemento | Es una entidad compleja que requiere anidar su propio marcador y estadísticas. |

| Equipo visitante| Elemento | Es una entidad compleja que requiere anidar su propio marcador y estadísticas. |

| Goles | Elemento | Es un valor fundamental (marcador) que le pertenece estructuralmente a cada equipo. |

| Estadio | Elemento | Es un dato en formato de texto que describe dónde ocurre el partido. |

| Estado del partido| Atributo | Metadato descriptivo simple (Ej. Finalizado, En curso). |

| Posesión | Elemento | Información que debe ir anidada dentro de las estadísticas específicas de un equipo. |

| Tarjetas | Elemento | Información que debe ir anidada dentro de las estadísticas de un equipo. |



\---



\## Actividad 4 - Incorporar estadísticas

\*\*Decisión de diseño:\*\* Se decidió anidar el elemento `<estadisticas>` directamente dentro de `<equipoLocal>` y `<equipoVisitante>`, en lugar de dejarlas sueltas o agrupadas a nivel del partido. Esto hace que el modelo sea más comprensible, reduce la duplicación de código y evita ambigüedades sobre a qué equipo le pertenecen los números de posesión o tiros.



\---



\## Actividad 5 - Diseñar el DTD



| Regla | Expresión DTD |

| :--- | :--- |

| Una liga contiene una o más jornadas | `<!ELEMENT liga (jornada+)>` |

| Una jornada contiene uno o más partidos | `<!ELEMENT jornada (partido+)>` |

| Un partido tiene exactamente un local | `equipoLocal` (declarado sin modificadores en la secuencia de partido) |

| Un partido tiene exactamente un visitante | `equipoVisitante` (declarado sin modificadores en la secuencia de partido) |

| Una estadística opcional | `estadisticas?` |

| Puede haber cero o más tarjetas | `tarjetas\*` |



\---



\## Actividad 6 - Definir atributos

\*\*Pregunta:\*\* Si cada partido tiene un identificador `P001`, `P002`, etc., ¿qué ventaja tendría declararlo como `ID` en lugar de `CDATA`?

\*\*Respuesta:\*\* Si se declara como tipo `ID` en el DTD, el validador XML asegurará de forma estricta que no existan dos identificadores repetidos en todo el documento. Si fuera `CDATA`, el validador permitiría identificadores duplicados, lo que corrompería la integridad de los datos.



\---



\## Actividad 8 - Pruebas negativas



| Prueba | ¿Bien formado? | ¿Válido? | Error detectado |

| :--- | :---: | :---: | :--- |

| Falta visitante | Sí | No | Elemento esperado `equipoVisitante` no encontrado. |

| Dos locales | Sí | No | Elemento `equipoLocal` no esperado en esa posición estructural. |

| Orden incorrecto | Sí | No | Los elementos no coinciden con la secuencia estricta obligatoria del DTD. |

| Falta atributo obligatorio | Sí | No | Falta el atributo etiquetado como `#REQUIRED` (ej. estado). |

| ID duplicado | Sí | No | El valor del atributo ID ya fue asignado a otro elemento previamente. |

| Elemento desconocido | Sí | No | Etiqueta no declarada en el esquema DTD. |



\*\*Conclusión clave:\*\* Un XML puede estar bien formado (etiquetas cerradas y sintaxis correcta), pero no ser válido si rompe las reglas establecidas en el DTD.



\---



\## Actividad 9 - Construir la jornada solicitada

Se incorporaron los resultados de la jornada correspondiente al \*\*domingo 27 de septiembre de 2026\*\* de la Liga MX, incluyendo la estructura de estadísticas opcionales y respetando la regla del DTD.

Los partidos modelados en el archivo `resultados.xml` son:

1\. Pumas UNAM (2) vs Atlético de San Luis (3)

2\. Club León (2) vs FC Juárez (1)

3\. Club Necaxa (2) vs Club América (4)



El documento completo es 100% válido respecto a `resultados.dtd`.

