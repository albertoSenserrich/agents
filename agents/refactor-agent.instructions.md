Instrucciones operativas para RefactorAgent
==========================================

Objetivo
--------
Proveer una secuencia reproducible para modernizar proyectos Java/Maven, aplicar refactors y preparar PRs.

Pasos detallados
-----------------
1. Inicializar la tarea en la lista de trabajo usando `manage_todo_list` (añadir ítem si no existe).
2. Leer `pom.xml` a nivel de raíz y módulos (`file_search` + `read_file`).
3. Detectar:
   - Versión de Java (propiedades `maven.compiler.source` / `maven.compiler.target` o `java.version`).
   - Dependencias críticas (log4j, spring-boot, junit, mockito, h2, gson).
4. Proponer cambios mínimos en el POM padre: actualizar `maven.compiler` props, versiones centrales y plugins.
5. Aplicar cambios con `apply_patch` en parches atómicos; por cada parche, ejecutar (si disponible) `mvn -DskipTests clean package` para validar compilación.
6. Si `mvn` no está instalado, proponer la creación del Maven Wrapper y añadirlo en un parche.
7. Migrar tests a JUnit 5 en módulos donde convenga (reemplazar imports y anotaciones), ejecutar tests y corregir fallos.
8. Reemplazar `log4j:1.x` por `log4j2` o `spring-boot-starter-logging` según el stack.
9. Mejorar cobertura: añadir tests unitarios para clases con lógica crítica.
10. Documentar los cambios realizados y actualizar `AGENTS.md` si se añaden nuevas capacidades.

Reglas de parcheo
-----------------
- Un cambio principal por parche (por ejemplo: "Actualizar parent pom: maven.compiler → 17").
- Incluir mensaje claro en el comentario del parche.
- Ejecutar la compilación después de cada parche significativo.

Reporte final
-------------
Al terminar, proporcionar:
- Lista de archivos modificados.
- Resumen de tests pasados/fallados.
- Recomendaciones para la migración a Java 25 (pasos y riesgos).
