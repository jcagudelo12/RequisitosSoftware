# ADR-0012: Modelo de movimientos inmutable (kardex append-only) con movimiento inverso

| Campo | Valor |
|---|---|
| Número | ADR-0012 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §5 (kardex) y casos de uso CU.0002, CU.0003 y CU.0004 |

## Contexto

Todo movimiento debe quedar en un kardex con usuario, sede, fecha y hora. La falta de trazabilidad es el problema 3. Los casos de uso exigen anular entradas (CU.0002), anular traslados en tránsito (CU.0003) y aplicar ajustes de toma física (CU.0004). El costo promedio ponderado depende del historial de movimientos. Stock y kardex deben ser coherentes en cada instante.

## Decisión

Decidimos un modelo **inmutable (append-only)**: un movimiento confirmado nunca se edita ni se borra. Las correcciones se hacen con un **movimiento inverso con motivo obligatorio**. Cada movimiento registra usuario, sede, fecha y hora. Stock y kardex se actualizan en la **misma transacción** ([ADR-0004](0004-base-datos-postgresql.md)).

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Permitir edición con tabla de historial | El estado vigente puede divergir del historial, y es posible manipularlo; la trazabilidad depende de un mecanismo accesorio. |
| Borrado lógico | Oculta movimientos y distorsiona el kardex y el costo; no deja el «por qué» de la corrección. |

## Consecuencias

**Positivas (+)**
- Trazabilidad completa y auditable (problema 3).
- Una única forma de corregir, común a anulaciones y ajustes.

**Negativas / trade-offs (−)**
- Crecimiento continuo de datos del kardex (requiere índices y, a futuro, archivado).
- Al anular hay que recalcular el costo promedio ponderado.
- Más movimientos visibles para el usuario (original + inverso).

## Enlaces

- CU.0002, CU.0003, CU.0004; problema 3.
- Relacionados: [ADR-0001](0001-arquitectura-monolito-modular.md), [ADR-0004](0004-base-datos-postgresql.md), [ADR-0013](0013-log-auditoria-append-only.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
