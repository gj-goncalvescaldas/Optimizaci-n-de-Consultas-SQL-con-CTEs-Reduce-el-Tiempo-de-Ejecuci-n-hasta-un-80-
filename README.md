# Optimización de Consultas SQL con CTEs: Reduce el Tiempo de Ejecución hasta un 80%

## Descripción

Este repositorio muestra cómo optimizar consultas SQL utilizando **Common Table Expressions (CTEs)** para reducir drásticamente el tiempo de ejecución. A través de ejemplos prácticos y explicaciones detalladas, aprenderás a identificar cuellos de botella en tus consultas y a aplicar técnicas que mejoran el rendimiento, la legibilidad y la mantenibilidad de tu código SQL.

## Tabla de Contenidos

- [Motivación](#motivación)
- [Antes y Después: Caso Práctico](#antes-y-después-caso-práctico)
- [Por Qué Funciona](#por-qué-funciona)
- [Casos de Uso Recomendados](#casos-de-uso-recomendados)
- [Herramientas para Analizar y Optimizar Consultas](#herramientas-para-analizar-y-optimizar-consultas)
- [Buenas Prácticas](#buenas-prácticas)
- [Recursos Adicionales](#recursos-adicionales)

---

## Motivación

¿Te has encontrado alguna vez esperando minutos a que termine una consulta SQL? Muchas veces, el problema no está en la infraestructura ni en el tamaño de los datos, sino en cómo está estructurada la consulta. Aprender a optimizar la lógica puede ahorrarte horas de espera y mejorar la experiencia de usuario en tus dashboards y aplicaciones.

---

## Antes y Después: Caso Práctico

### Consulta original (lenta):

```sql
SELECT
  customer_id,
  first_name,
  last_name,
  AVG(DATEDIFF(day, order_date, GETDATE())) AS avg_days_since_order
FROM orders
JOIN customers ON orders.customer_id = customers.id
WHERE status = 'Completed'
GROUP BY customer_id, first_name, last_name
HAVING AVG(DATEDIFF(day, order_date, GETDATE())) > 30;
```

### Consulta optimizada con CTE (rápida):

```sql
WITH order_days AS (
  SELECT
    customer_id,
    DATEDIFF(day, order_date, GETDATE()) AS days_since_order
  FROM orders
  WHERE status = 'Completed'
)
SELECT
  c.id,
  c.first_name,
  c.last_name,
  AVG(o.days_since_order) AS avg_days_since_order
FROM order_days o
JOIN customers c ON o.customer_id = c.id
GROUP BY c.id, c.first_name, c.last_name
HAVING AVG(o.days_since_order) > 30;
```

**Resultado:**  
El tiempo de ejecución pasó de **90 segundos** a solo **18 segundos**.

---

## Por Qué Funciona

- ✅ **Evita cálculos redundantes:** `DATEDIFF` se calcula una sola vez.
- ✅ **Filtrado temprano:** Reduce el volumen de datos antes de los joins.
- ✅ **JOINs más rápidos:** Se trabaja con datasets intermedios optimizados.

---

## Casos de Uso Recomendados

- Dashboards (Power BI, Tableau, etc.)
- Pipelines ETL
- Segmentación de clientes
- Preparación de datos para Machine Learning

---

## Herramientas para Analizar y Optimizar Consultas

- **SQL Server:** Execution Plan (`CTRL + M`)
- **PostgreSQL:** `EXPLAIN ANALYZE`
- **BigQuery:** Query Execution Details
- **Snowflake:** Query Profile tab

---

## Buenas Prácticas

- Usa CTEs para cálculos intermedios o lógicos complejos.
- Filtra lo antes posible para reducir el volumen de datos.
- Analiza siempre el plan de ejecución.
- Usa índices en columnas clave de filtros y joins.
- Documenta tus consultas.

---

## Recursos Adicionales

- [CTEs en SQL Server](https://learn.microsoft.com/es-es/sql/t-sql/queries/with-common-table-expression-transact-sql)
- [CTEs en PostgreSQL](https://www.postgresql.org/docs/current/queries-with.html)
- [Optimización de BigQuery](https://cloud.google.com/bigquery/query-optimization)
- [Cómo leer un Execution Plan](https://www.sqlshack.com/how-to-read-sql-server-execution-plans/)

---

¿Tienes sugerencias o quieres aportar tus propios ejemplos?  
¡Abre una issue o haz un pull request!

---

¿Listo para acelerar tus consultas SQL?  
**Explora los ejemplos y empieza a optimizar** 🚀

