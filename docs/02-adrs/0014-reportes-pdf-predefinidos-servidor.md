# ADR-0014: Reportes PDF predefinidos generados en el servidor

| Campo | Valor |
|---|---|
| Número | ADR-0014 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §5 y §7 (reportes predefinidos, sin generación dinámica) y §18.5 (S3) |

## Contexto

Se requieren seis reportes en PDF para imprimir: stock por sede, kardex por producto, productos bajo el mínimo, rotación, movimientos (por día, mes y proveedor) y diferencias de toma física. La generación dinámica de reportes está explícitamente fuera de alcance. Los reportes pueden ser pesados y no deben afectar el tiempo de respuesta de las operaciones. Equipo de 7, 14 semanas.

## Decisión

Decidimos **construir** los 6 reportes predefinidos: se generan en el servidor por la **tarea worker de forma asíncrona**, se almacenan en **S3** y se descargan mediante **URL firmada**. La librería concreta de PDF queda como punto abierto que no cambia esta decisión.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Herramienta BI / generador de reportes (buy) | Licencias y operación sin necesidad real, pues el alcance excluye reportes dinámicos; más piezas que asegurar. |
| Generar el PDF en el navegador | Exige traer grandes volúmenes de datos al cliente, el resultado depende del equipo del usuario y es difícil de auditar y controlar por permisos. |

## Consecuencias

**Positivas (+)**
- Los reportes pesados no bloquean la API; resultados controlados por permisos y sede.
- Sin costo de licencias.

**Negativas / trade-offs (−)**
- Hay que desarrollar y mantener las plantillas de los 6 reportes.
- El usuario espera la generación asíncrona (no instantánea).
- Cualquier reporte nuevo requiere desarrollo.

## Enlaces

- Necesidad de reportes (§5), alcance (§7).
- Relacionados: [ADR-0009](0009-integraciones-asincronas-cola-mensajes.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md).
- Specs: [0003](../03-specs/0003-toma-fisica-conciliacion.md), [0004](../03-specs/0004-dashboard-stock-consolidado.md).
