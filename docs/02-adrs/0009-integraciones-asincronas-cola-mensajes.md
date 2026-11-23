# ADR-0009: Integraciones asíncronas con cola de mensajes

| Campo | Valor |
|---|---|
| Número | ADR-0009 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.4 (IContabilidad, IVentasWebhook) y §18.5 (SQS) |

## Contexto

El sistema debe enviar la valoración y los movimientos contables al sistema de contabilidad y descontar stock por cada venta del sistema de ventas existente. Registrar un movimiento debe tardar menos de 3 s y no puede depender de la disponibilidad de terceros. La documentación de los sistemas externos aún no se conoce. La valoración manual y tardía es el problema 6.

## Decisión

Decidimos integrar de forma **asíncrona con cola de mensajes (Amazon SQS) y una tarea worker**:
- **Salida a contabilidad** (`IContabilidad`, REST): se encola y se envía fuera de la transacción de movimiento.
- **Entrada de ventas** por webhook HTTPS (`IVentasWebhook`): se encola y se procesa de forma **idempotente**.
- **Reintentos** y cola de mensajes fallidos (DLQ) monitoreados (HU-08.3).

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Llamadas síncronas dentro de la transacción | Un tercero lento o caído bloquearía o haría fallar el movimiento y comprometería el límite de 3 s. |
| Archivos por lotes nocturnos | Valoración y descuento de stock tardíos: reproduce el problema 6. |

## Consecuencias

**Positivas (+)**
- El movimiento no depende de terceros; los fallos se reintentan sin pérdida.
- Desacopla el ritmo de ventas del de procesamiento.

**Negativas / trade-offs (−)**
- Consistencia eventual: contabilidad y stock por ventas se reflejan con demora.
- Exige idempotencia, monitoreo de la DLQ y trazabilidad de mensajes.
- Riesgo: la documentación y los contratos de los sistemas externos están por confirmar.

## Enlaces

- HU-08.3; necesidad de integración con contabilidad y ventas (§5); problema 6.
- Relacionados: [ADR-0001](0001-arquitectura-monolito-modular.md), [ADR-0005](0005-api-rest-json-https.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md).
