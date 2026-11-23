# ADR-0008: Caché de consultas de stock con Redis

| Campo | Valor |
|---|---|
| Número | ADR-0008 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Derivado de §6 (NFR: consulta de stock < 2 s) y §18.5 (ElastiCache Redis) |

## Contexto

La consulta de stock (por sede y consolidada, ~4.000 referencias, 6 ubicaciones) debe responder en menos de 2 segundos con al menos 30 usuarios concurrentes. Las consultas consolidadas agregan datos de varias ubicaciones y se repiten con frecuencia. La API corre en 2-4 tareas Fargate.

## Decisión

Decidimos cachear las consultas de stock en **Redis (Amazon ElastiCache)**, en subred privada. La caché se **invalida por eventos de movimiento**: al confirmarse un movimiento se descartan las entradas afectadas.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Sin caché (solo índices y vistas materializadas) | Es más simple, pero las vistas materializadas se refrescan con retraso o con costo, y el margen para cumplir < 2 s en consolidados se reduce. |
| Caché en memoria local por instancia | Con 2-4 tareas cada una tendría un estado distinto: lecturas inconsistentes entre solicitudes. |

## Consecuencias

**Positivas (+)**
- Respuesta rápida y uniforme entre tareas; menor carga sobre PostgreSQL.

**Negativas / trade-offs (−)**
- Un componente más que operar y pagar.
- Riesgo de datos obsoletos si falla la invalidación; PostgreSQL sigue siendo la fuente de verdad.
- Necesidad de diseñar claves e invalidación con cuidado.

## Enlaces

- NFR de tiempos de respuesta (§6).
- Relacionados: [ADR-0004](0004-base-datos-postgresql.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md).
- Spec: [0004 Dashboard de stock consolidado](../03-specs/0004-dashboard-stock-consolidado.md).
