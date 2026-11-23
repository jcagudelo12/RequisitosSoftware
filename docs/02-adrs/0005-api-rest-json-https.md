# ADR-0005: API REST/JSON sobre HTTPS

| Campo | Valor |
|---|---|
| Número | ADR-0005 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.4 (interfaz IInventarioAPI) |

## Contexto

El cliente SPA ([ADR-0003](0003-frontend-spa-react-typescript.md)) y el sistema de ventas externo necesitan una interfaz estable con el backend. Los casos de uso son operaciones sobre recursos (productos, movimientos, traslados, reportes). El equipo es de 7 personas, con 14 semanas, y debe integrar sistemas externos cuya documentación aún es desconocida.

## Decisión

Decidimos exponer una **API REST con JSON sobre HTTPS**, interfaz `IInventarioAPI`, con prefijo `/api/v1` y un **formato uniforme de errores**. Todo el tráfico va cifrado en tránsito.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| GraphQL | Flexibilidad de consulta innecesaria: las pantallas son conocidas y predefinidas; añade complejidad de caché, autorización por campo y curva de aprendizaje. |
| gRPC | Pensado para comunicación entre servicios; no es natural desde navegador ni para webhooks de terceros, y no hay microservicios ([ADR-0001](0001-arquitectura-monolito-modular.md)). |

## Consecuencias

**Positivas (+)**
- Estilo conocido por el equipo y por los sistemas externos; fácil de probar y documentar.
- Versionado por prefijo permite evolucionar sin romper clientes.
- Compatible con CloudFront, WAF y caché HTTP.

**Negativas / trade-offs (−)**
- Posible sobre-solicitud o varias llamadas para vistas compuestas.
- Requiere disciplina para mantener el contrato y el formato de errores.

## Enlaces

- Relacionados: [ADR-0003](0003-frontend-spa-react-typescript.md), [ADR-0006](0006-autenticacion-jwt-autorizacion-rbac-sede.md), [ADR-0009](0009-integraciones-asincronas-cola-mensajes.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
