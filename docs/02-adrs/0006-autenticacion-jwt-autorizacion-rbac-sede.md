# ADR-0006: Autenticación con JWT y autorización RBAC por rol, permiso y sede

| Campo | Valor |
|---|---|
| Número | ADR-0006 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §5 (necesidades de roles) y §18.4 (capa de seguridad) |

## Contexto

Hay cinco roles (administrador, bodeguero, comprador, contador, jefe de sede) y cinco permisos (visualizar, consultar, crear, modificar, anular), siempre combinados con la sede asignada al usuario. El administrador opera en cualquier sede. Todo intento denegado debe quedar en auditoría. La API corre en 2-4 tareas Fargate detrás de un balanceador ([ADR-0010](0010-despliegue-aws-ecs-fargate.md)) y el equipo es de 7 personas con 14 semanas.

## Decisión

Decidimos autenticar con **JWT** y autorizar con **RBAC por rol + permiso + sede asignada**, evaluado en la capa de seguridad del backend en cada solicitud. El administrador puede operar en cualquier sede. Todo intento denegado se registra en el log de auditoría ([ADR-0013](0013-log-auditoria-append-only.md)).

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Sesiones en servidor | Exigen estado compartido o sesiones pegajosas con 2-4 tareas; añade un componente más (almacén de sesiones). |
| Proveedor de identidad externo (Cognito/Keycloak) | Costo y complejidad de operación o integración desproporcionados para ~30 usuarios; las reglas por sede igual habría que implementarlas en la aplicación. |

## Consecuencias

**Positivas (+)**
- API sin estado: escala horizontalmente sin sesión compartida.
- Regla de acceso por sede aplicada de forma uniforme en el servidor.

**Negativas / trade-offs (−)**
- Revocar un token antes de su expiración es difícil; se mitiga con expiración corta.
- Hay que custodiar la clave de firma (Secrets Manager + KMS) y el almacenamiento del token en el cliente.
- Gestión propia de usuarios y contraseñas.

## Enlaces

- Necesidades de roles, IP y auditoría (§5); HU-01.x de la épica EP01.
- Relacionados: [ADR-0002](0002-backend-java-spring-boot.md), [ADR-0007](0007-restriccion-acceso-ip-perimetro.md), [ADR-0013](0013-log-auditoria-append-only.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
