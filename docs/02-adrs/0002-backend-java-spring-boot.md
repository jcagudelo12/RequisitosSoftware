# ADR-0002: Backend en Java 21 con Spring Boot 3

| Campo | Valor |
|---|---|
| Número | ADR-0002 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.4–18.5 (componentes y despliegue: servicio inventario-api.jar en Docker) |

## Contexto

El backend concentra reglas de negocio transaccionales (stock, kardex, costo promedio ponderado, permisos por sede). Debe cumplir consulta < 2 s y registro de movimiento < 3 s con 30 usuarios concurrentes, y ser entregable por 7 personas en 14 semanas. Debe empaquetarse como contenedor para ECS Fargate ([ADR-0010](0010-despliegue-aws-ecs-fargate.md)).

## Decisión

Decidimos implementar el backend en **Java 21 con Spring Boot 3**, empaquetado como servicio `inventario-api.jar` dentro de una imagen Docker. Se usa el ecosistema Spring (Web, Data JPA, Security) para API, persistencia y seguridad.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Node.js / NestJS | Buen rendimiento de desarrollo, pero tipado y manejo de decimales/transacciones menos maduros para costeo y kardex; menos experiencia asumida en el equipo para este tipo de dominio. |
| .NET | Técnicamente adecuado y equivalente en madurez transaccional; se descarta por el supuesto de que el equipo tiene más experiencia en Java/Spring (la fuente no lo declara explícitamente; si el supuesto cambia, este ADR se supersede). |

## Consecuencias

**Positivas (+)**
- Ecosistema maduro para transacciones ACID, seguridad y pruebas; tipado fuerte.
- Spring Security cubre JWT y RBAC ([ADR-0006](0006-autenticacion-jwt-autorizacion-rbac-sede.md)).
- Rendimiento suficiente de sobra para 30 usuarios concurrentes.

**Negativas / trade-offs (−)**
- Mayor consumo de memoria y arranque más lento que Node.js (impacta el tamaño de las tareas Fargate y el autoescalado).
- Más código repetitivo (boilerplate) que en otras opciones.

## Enlaces

- Relacionados: [ADR-0001](0001-arquitectura-monolito-modular.md), [ADR-0004](0004-base-datos-postgresql.md), [ADR-0005](0005-api-rest-json-https.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md), [ADR-0011](0011-infraestructura-como-codigo-ci-cd.md).
