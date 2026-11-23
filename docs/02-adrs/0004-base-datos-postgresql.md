# ADR-0004: PostgreSQL 16 como base de datos única

| Campo | Valor |
|---|---|
| Número | ADR-0004 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.4–18.5 (RDS PostgreSQL Multi-AZ) |

## Contexto

Cada movimiento debe actualizar de forma atómica el stock, el kardex y el log de auditoría. El costo promedio ponderado exige precisión numérica exacta. Hay ~4.000 referencias en 6 ubicaciones, 30 usuarios concurrentes y metas de < 2 s (consulta) y < 3 s (movimiento). El equipo es de 7 personas y el presupuesto de nube ronda $1.200.000 COP/mes.

## Decisión

Decidimos usar **PostgreSQL 16** como base de datos única y relacional (Amazon RDS) para stock, kardex y log de auditoría, con transacciones ACID y tipos numéricos de precisión exacta para costos.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| MySQL | Capaz, pero PostgreSQL ofrece mejor soporte de restricciones, tipos numéricos, índices parciales y control de permisos finos para el log append-only. |
| NoSQL documental | El dominio es fuertemente relacional y transaccional (stock + kardex + log); la consistencia entre documentos sería costosa de garantizar. |
| Una BD por módulo | Rompe la transacción única de movimiento y añade costo operativo, injustificable para el volumen y el equipo ([ADR-0001](0001-arquitectura-monolito-modular.md)). |

## Consecuencias

**Positivas (+)**
- Atomicidad de stock + kardex + log; integridad referencial.
- Cálculo exacto del costo promedio ponderado.
- Servicio administrado con Multi-AZ y respaldos ([ADR-0015](0015-respaldos-diarios-aws-backup.md)).

**Negativas / trade-offs (−)**
- Un único punto de escalado vertical; suficiente para 30 usuarios, pero separar módulos exigirá migración.
- El costo de RDS Multi-AZ pesa en el presupuesto mensual.

## Enlaces

- Relacionados: [ADR-0008](0008-cache-consultas-redis.md), [ADR-0012](0012-kardex-inmutable-movimiento-inverso.md), [ADR-0013](0013-log-auditoria-append-only.md), [ADR-0015](0015-respaldos-diarios-aws-backup.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
