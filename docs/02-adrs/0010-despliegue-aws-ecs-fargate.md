# ADR-0010: Despliegue en AWS con ECS Fargate

| Campo | Valor |
|---|---|
| Número | ADR-0010 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §18.5 (diagrama de despliegue AWS) |

## Contexto

El sistema debe estar disponible de 6:00 a. m. a 10:00 p. m., soportar 30 usuarios concurrentes, tener respaldos diarios y operar por ~$1.200.000 COP/mes de referencia. La fuente indica que la Actividad 3 no fija un proveedor de nube y que se eligió AWS por su madurez y por ofrecer todos los servicios administrados (con equivalentes en Azure y GCP). El equipo es de 7 personas, sin capacidad para operar servidores.

## Decisión

Decidimos desplegar en **AWS, región us-east-1**, con servicios administrados:
- Perímetro: Route 53, CloudFront + WAF; SPA en S3.
- Cómputo: ALB y **ECS Fargate en 2 zonas de disponibilidad**, con autoescalado de 2 a 4 tareas API y 1 tarea worker; imágenes en ECR.
- Datos: RDS PostgreSQL Multi-AZ (primaria + standby), ElastiCache Redis en subred privada, S3 (PDF y respaldos).
- Mensajería y correo: SQS, SES.
- Seguridad y operación: Secrets Manager + KMS, CloudWatch, NAT Gateway.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Azure | Equivalente funcional, sin ventaja diferencial; se prefirió AWS por madurez y conocimiento disponible. |
| Google Cloud | Igual que Azure: servicios equivalentes, sin ventaja que justifique el cambio. |
| Servidor propio en la bodega | Contradice el problema 8 (continuidad y respaldos) y la necesidad de no instalar software; depende de energía, red y personal en sitio. |
| VM EC2 autogestionadas | Obliga a parchear y operar servidores y escalado manual con un equipo de 7 y 14 semanas. |

## Consecuencias

**Positivas (+)**
- Alta disponibilidad (2 AZ, Multi-AZ) y escalado sin gestionar servidores.
- Servicios administrados reducen la carga operativa.

**Negativas / trade-offs (−)**
- Costo recurrente en USD, sensible al tipo de cambio; Multi-AZ, NAT Gateway y Redis pesan en el presupuesto.
- Dependencia de AWS (se mitiga con contenedores y [ADR-0011](0011-infraestructura-como-codigo-ci-cd.md)).
- Muchas piezas que configurar y asegurar.

## Enlaces

- Recursos y costos (§ de recursos); NFR de disponibilidad (§6); problema 8.
- Relacionados: [ADR-0002](0002-backend-java-spring-boot.md), [ADR-0003](0003-frontend-spa-react-typescript.md), [ADR-0004](0004-base-datos-postgresql.md), [ADR-0007](0007-restriccion-acceso-ip-perimetro.md), [ADR-0008](0008-cache-consultas-redis.md), [ADR-0009](0009-integraciones-asincronas-cola-mensajes.md), [ADR-0011](0011-infraestructura-como-codigo-ci-cd.md), [ADR-0015](0015-respaldos-diarios-aws-backup.md).
