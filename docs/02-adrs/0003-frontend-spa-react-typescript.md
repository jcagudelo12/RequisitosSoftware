# ADR-0003: Cliente web SPA con React y TypeScript

| Campo | Valor |
|---|---|
| Número | ADR-0003 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.4 (diagrama de componentes) y §18.5 (S3 + CloudFront) |

## Contexto

La necesidad exige acceso desde navegador y **sin instalar software**; la aplicación móvil nativa está fuera de alcance. Los usuarios operan en 6 ubicaciones con pantallas de consulta y captura frecuentes (dashboard de stock, movimientos). El equipo es de 7 personas y el plazo de 14 semanas.

## Decisión

Decidimos un cliente **SPA en React + TypeScript**, compilado y servido como **sitio estático** (S3 + CloudFront). Tiene un componente de interfaz por módulo y consume exclusivamente la interfaz `IInventarioAPI` ([ADR-0005](0005-api-rest-json-https.md)).

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Renderizado en servidor (plantillas) | Acopla UI y backend, exige servidor de aplicaciones para servir páginas y hace menos fluida la captura repetitiva de movimientos. |
| Angular | Viable, pero con curva de aprendizaje más pesada y más estructura de la necesaria para el plazo de 14 semanas. |

## Consecuencias

**Positivas (+)**
- Frontend y backend se desarrollan y despliegan de forma independiente; hosting estático barato y escalable.
- Tipado con TypeScript reduce errores en la integración con la API.
- Sin instalación: basta el navegador.

**Negativas / trade-offs (−)**
- Dos bases de código y dos pipelines por mantener.
- Hay que gestionar el almacenamiento seguro del token en el cliente ([ADR-0006](0006-autenticacion-jwt-autorizacion-rbac-sede.md)).
- Sin soporte offline ni app nativa (fuera de alcance).

## Enlaces

- Relacionados: [ADR-0001](0001-arquitectura-monolito-modular.md), [ADR-0005](0005-api-rest-json-https.md), [ADR-0007](0007-restriccion-acceso-ip-perimetro.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md).
- Spec: [0004 Dashboard de stock consolidado](../03-specs/0004-dashboard-stock-consolidado.md).
