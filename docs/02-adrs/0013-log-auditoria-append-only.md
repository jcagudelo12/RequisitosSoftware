# ADR-0013: Log de auditoría transversal append-only en PostgreSQL

| Campo | Valor |
|---|---|
| Número | ADR-0013 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §5 (auditoría), §18.4 (módulo Auditoría) y HU-01.5/HU-01.6 |

## Contexto

Toda acción relevante (inicio de sesión, movimientos, anulaciones, cambios de permisos, intentos denegados) debe quedar registrada y ser consultable y filtrable por usuarios del negocio (HU-01.5, HU-01.6), no solo por el equipo técnico. El equipo es de 7 personas y el presupuesto es limitado.

## Decisión

Decidimos un **módulo de auditoría transversal** con registro **append-only en PostgreSQL**: la aplicación no tiene permisos UPDATE ni DELETE sobre la tabla. Registra inicio de sesión, movimientos, anulaciones, cambios de permisos e intentos denegados (IP, permiso, sede). Los **logs técnicos** van a CloudWatch.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Solo logs en CloudWatch | No son consultables ni filtrables por el negocio desde la aplicación y mezclan eventos técnicos con eventos de negocio. |
| SIEM externo | Costo y complejidad desproporcionados para 30 usuarios y el presupuesto de referencia. |

## Consecuencias

**Positivas (+)**
- Auditoría consultable por negocio, en la misma base y bajo las mismas copias de seguridad.
- Inmutabilidad reforzada por permisos de base de datos.

**Negativas / trade-offs (−)**
- Crecimiento de la tabla (requiere índices y política de retención).
- Un administrador de BD con privilegios elevados podría alterar el log; se mitiga con control de accesos y respaldos.

## Enlaces

- HU-01.5, HU-01.6.
- Relacionados: [ADR-0004](0004-base-datos-postgresql.md), [ADR-0006](0006-autenticacion-jwt-autorizacion-rbac-sede.md), [ADR-0007](0007-restriccion-acceso-ip-perimetro.md), [ADR-0012](0012-kardex-inmutable-movimiento-inverso.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
