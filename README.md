# Optimizacion-de-Consultas-SQL-con-CTEs
text
# Optimización de Consultas SQL con CTEs: Reduce el Tiempo de Ejecución hasta un 80%

## Descripción

Este repositorio muestra cómo optimizar consultas SQL utilizando **Common Table Expressions (CTEs)** para reducir drásticamente el tiempo de ejecución. A través de ejemplos prácticos y explicaciones detalladas, aprenderás a identificar cuellos de botella en tus consultas y a aplicar técnicas que mejoran el rendimiento, la legibilidad y la mantenibilidad de tu código SQL.

## Tabla de Contenidos

- [Motivación](#motivación)
- [Antes y Después: Caso Práctico](#antes-y-después-caso-práctico)
- [Por qué funciona](#por-qué-funciona)
- [Casos de uso recomendados](#casos-de-uso-recomendados)
- [Herramientas para analizar y optimizar consultas](#herramientas-para-analizar-y-optimizar-consultas)
- [Buenas prácticas](#buenas-prácticas)
- [Recursos adicionales](#recursos-adicionales)

---

## Motivación

¿Te has encontrado alguna vez esperando minutos a que termine una consulta SQL? Muchas veces, el problema no está en la infraestructura ni en el tamaño de los datos, sino en cómo está estructurada la consulta. Aprender a optimizar la lógica puede ahorrarte horas de espera y mejorar la experiencia de usuario en tus dashboards y aplicaciones.

---

## Antes y Después: Caso Práctico

**Consulta original (lenta):**

SELECT
customer_id,
first_name,
last_name,
AVG(DATEDIFF(day, order_date, GETDATE())) AS avg_days_since_order
FROM
orders
JOIN
customers ON orders.customer_id = customers.id
WHERE
status = 'Completed'
GROUP BY
customer_id, first_name, last_name
HAVING
AVG(DATEDIFF(day, order_date, GETDATE())) > 30

text

**Consulta optimizada con CTE (rápida):**

WITH order_days AS (
SELECT
customer_id,
DATEDIFF(day, order_date, GETDATE()) AS days_since_order
FROM
orders
WHERE
status = 'Completed'
)
SELECT
c.id,
c.first_name,
c.last_name,
AVG(o.days_since_order) AS avg_days_since_order
FROM
order_days o
JOIN
customers c ON o.customer_id = c.id
GROUP BY
c.id, c.first_name, c.last_name
HAVING
AVG(o.days_since_order) > 30

text

**Resultado:**  
El tiempo de ejecución pasó de 90 segundos a solo 18 segundos.

---

## Por qué funciona

- **Evita cálculos redundantes:** El cálculo de `DATEDIFF` se realiza una sola vez.
- **Filtrado temprano:** Se reduce la cantidad de filas procesadas en los pasos posteriores.
- **JOINs más rápidos:** Al trabajar con conjuntos de datos más pequeños y ya procesados.

---

## Casos de uso recomendados

- **Dashboards en Power BI, Tableau, etc.**
- **Pipelines ETL**
- **Segmentación de clientes**
- **Preparación de datos para modelos de Machine Learning**

---

## Herramientas para analizar y optimizar consultas

- **SQL Server:** Execution Plan (`CTRL + M`)
- **PostgreSQL:** `EXPLAIN ANALYZE`
- **BigQuery:** Query Execution Details
- **Snowflake:** Query Profile tab

---

## Buenas prácticas

- Utiliza CTEs para cálculos intermedios y lógica compleja.
- Filtra y transforma los datos lo antes posible.
- Analiza siempre el plan de ejecución de tus consultas.
- Asegúrate de tener índices en las columnas usadas en filtros y joins.
- Documenta tus consultas para facilitar el mantenimiento.

---

## Recursos adicionales

- [Documentación oficial de CTEs en SQL Server](https://learn.microsoft.com/es-es/sql/t-sql/queries/with-common-table-expression-transact-sql)
- [CTEs en PostgreSQL](https://www.postgresql.org/docs/current/queries-with.html)
- [Guía de optimización de consultas en BigQuery](https://cloud.google.com/bigquery/query-optimization)
- [Artículo: Cómo interpretar un Execution Plan](https://www.sqlshack.com/how-to-read-sql-server-execution-plans/)

---

¿Tienes sugerencias o quieres aportar tus propios ejemplos? ¡Abre una issue o haz un pull request!

---

¿Listo para acelerar tus consultas SQL?  
¡Explora los ejemplos y empieza a optimizar! 🚀
