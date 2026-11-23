# ADR-0015: Respaldos diarios con AWS Backup

| Campo | Valor |
|---|---|
| Número | ADR-0015 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §6 (NFR de respaldos) y épica EP09 (HU-09.2; problema 8) |

## Contexto

La necesidad no funcional exige respaldos automáticos diarios de la información, y la pérdida de datos es parte del problema 8. El sistema concentra stock, kardex y auditoría en PostgreSQL ([ADR-0004](0004-base-datos-postgresql.md)). El equipo no puede dedicar personal a tareas manuales de respaldo y el presupuesto es limitado.

## Decisión

Decidimos usar **AWS Backup** para respaldos automáticos diarios con **retención de 35 días**, más **recuperación a un punto en el tiempo (PITR) de RDS** y una **prueba periódica de restauración** (HU-09.2).

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Scripts manuales de volcado | Dependen de personas y de mantenimiento propio; fallan en silencio y no tienen monitoreo integrado. |
| Confiar solo en Multi-AZ | Multi-AZ da alta disponibilidad, no respaldo: replica también borrados y corrupción lógica. |

## Consecuencias

**Positivas (+)**
- Cumple el NFR de respaldos diarios sin intervención manual.
- PITR permite recuperar ante errores lógicos; la prueba periódica valida que el respaldo sirve.

**Negativas / trade-offs (−)**
- Costo de almacenamiento de 35 días de retención.
- La prueba de restauración consume tiempo del equipo.
- La restauración no es instantánea (tiempo de recuperación a medir).

## Enlaces

- HU-09.2; NFR de respaldos (§6); problema 8.
- Relacionados: [ADR-0004](0004-base-datos-postgresql.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md), [ADR-0011](0011-infraestructura-como-codigo-ci-cd.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
