\# Diseño y validación de resultados de Liga MX



\## Modelo Jerárquico

Se decidió utilizar la jerarquía `liga > jornada > partido`. Dentro del partido se separó en `equipoLocal` y `equipoVisitante` para distinguir claramente a los competidores, y las `estadisticas` se anidaron dentro de cada equipo para evitar confusión sobre a quién le pertenecen los datos.



\## Atributos vs Elementos

\* Atributos: Se usaron para datos identificadores y breves (`id` del partido, `estado`, `numero` de jornada, `fecha`). La ventaja de declarar el ID como tipo `ID` en el DTD en lugar de CDATA es que el validador forzará a que no haya partidos con el mismo identificador repetido.

\* Elementos: Se usaron para información compleja y agrupadora (marcador, posesión, faltas).



\## Pruebas Negativas

Se creó `resultados-invalido.xml` para demostrar que XML Bien Formado ≠ XML Válido.

\* \*\*Falta visitante:\*\* Está bien formado, pero no es válido (el DTD exige `equipoVisitante`).

\* \*\*Dos locales:\*\* Está bien formado, pero no válido.

\* \*\*Falta atributo obligatorio:\*\* Está bien formado, pero no válido porque falta el estado.

\* \*\*ID duplicado:\*\* Bien formado, pero el validador marcará error de unicidad.



El archivo principal `xml/resultados.xml` valida perfectamente contra `dtd/resultados.dtd` e incluye los juegos de la fecha solicitada.

