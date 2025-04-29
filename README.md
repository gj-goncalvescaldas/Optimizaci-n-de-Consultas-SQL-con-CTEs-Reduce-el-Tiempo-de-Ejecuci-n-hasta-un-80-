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
