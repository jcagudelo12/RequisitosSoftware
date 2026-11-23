# ADR-0007: Restricción de acceso por IP pública en el perímetro

| Campo | Valor |
|---|---|
| Número | ADR-0007 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Entrega N.°4, §5 (acceso solo desde las sedes) y §18.5 (CloudFront + WAF) |

## Contexto

El sistema solo debe accederse desde las sedes, mediante direcciones IP públicas específicas, sin instalar software y con una forma sencilla de abrir sedes nuevas (crear sede, asignar usuarios, habilitar su IP). No es una aplicación web pública. Son 6 ubicaciones y ~30 usuarios.

## Decisión

Decidimos restringir el acceso por la **IP pública fija de cada sede**, en dos capas (defensa en profundidad):
1. **Perímetro:** CloudFront + AWS WAF con lista de IP registradas.
2. **Aplicación:** filtro de IP por sede en la capa de seguridad, que además registra los intentos denegados en auditoría.

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| VPN | Exige instalar y configurar software en cada equipo, lo que contradice la necesidad; mayor costo operativo. |
| Solo filtro en la aplicación | El tráfico no autorizado llegaría hasta el backend; sin defensa en profundidad ni protección en el perímetro. |
| Sin restricción | Incumple la necesidad de acceso solo desde sedes y amplía la superficie de ataque. |

## Consecuencias

**Positivas (+)**
- Cumple «sin instalar software» y reduce la superficie de ataque.
- Abrir una sede equivale a registrar su IP.

**Negativas / trade-offs (−)**
- Cada sede necesita IP pública fija (costo de la empresa, fuera del presupuesto del proyecto).
- No hay acceso remoto (teletrabajo, viajes).
- Un cambio de IP por parte del ISP deja bloqueada la sede hasta actualizar la lista.
- Hay que mantener la lista sincronizada entre WAF y aplicación.

## Enlaces

- Relacionados: [ADR-0006](0006-autenticacion-jwt-autorizacion-rbac-sede.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md), [ADR-0013](0013-log-auditoria-append-only.md).
- Specs: [0001](../03-specs/0001-registrar-entrada-mercancia.md), [0002](../03-specs/0002-trasladar-mercancia-entre-sedes.md), [0003](../03-specs/0003-toma-fisica-conciliacion.md).
