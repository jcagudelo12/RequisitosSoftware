# ADR-0001: Arquitectura de monolito modular

| Campo | Valor |
|---|---|
| Número | ADR-0001 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.4 (diagrama de componentes) |

## Contexto

StockCentral debe cubrir 6 ubicaciones (5 sedes + bodega central), ~4.000 referencias y 30 usuarios concurrentes. El equipo es de 7 personas y el plazo de 14 semanas (7 sprints de 2 semanas). Las épicas (catálogo, movimientos, traslados, toma física, compras, costeo, integraciones, auditoría) comparten datos y deben actualizarse de forma consistente. La fuente justifica explícitamente un único despliegue por el tamaño del equipo, el plazo y la carga.

## Decisión

Decidimos construir un **monolito modular**: una sola aplicación desplegable con un módulo por épica y dependencias explícitas entre módulos. **Movimientos** es el núcleo: Traslados, Toma física, Compras e Integraciones dependen de él; él depende de Catálogo, Kardex y costeo, y Auditoría. Los límites de módulo se respetan (sin acceso directo a las tablas de otro módulo) para poder extraer servicios más adelante si la carga lo exige.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Microservicios | Con 7 personas y 14 semanas, el costo operativo (orquestación, tracing, transacciones distribuidas entre stock, kardex y log) supera el beneficio para 30 usuarios concurrentes. |
| Monolito sin módulos | Más rápido al inicio, pero acopla las épicas y hace costoso separar servicios o repartir trabajo en paralelo entre el equipo. |

## Consecuencias

**Positivas (+)**
- Un solo artefacto y una sola transacción para stock + kardex + log.
- Menor costo de infraestructura y operación; encaja en el plazo.
- Límites claros permiten separar servicios después.

**Negativas / trade-offs (−)**
- La disciplina de límites depende del equipo (revisiones de código, pruebas de arquitectura).
- Se escala y despliega como una unidad; un fallo afecta a todos los módulos.

## Enlaces

- Relacionados: [ADR-0002](0002-backend-java-spring-boot.md), [ADR-0004](0004-base-datos-postgresql.md), [ADR-0012](0012-kardex-inmutable-movimiento-inverso.md), [ADR-0013](0013-log-auditoria-append-only.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md), [0004](../03-specs/0004-dashboard-stock-consolidado.md).
