# ADR-0011: Infraestructura como código (Terraform) y CI/CD con GitHub Actions

| Campo | Valor |
|---|---|
| Número | ADR-0011 |
| Estado | Aceptado |
| Fecha | 2026-11-22 |
| Decisores | Equipo Grupo 1 (líder técnico) · Product Owner del cliente |
| Supersede / Superado por | — |
| Origen | Derivado de §18.5 (despliegue AWS) y la épica EP09 (HU-09.1) |

## Contexto

La arquitectura de [ADR-0010](0010-despliegue-aws-ecs-fargate.md) tiene muchas piezas. Se necesitan entornos de desarrollo, pruebas y producción coherentes, y despliegues repetibles por un equipo de 7 personas en 14 semanas (7 sprints). El presupuesto obliga a que desarrollo y pruebas sean más baratos que producción.

## Decisión

Decidimos definir la infraestructura con **Terraform**, con la misma plantilla para desarrollo, pruebas y producción. Desarrollo y pruebas usan 1 tarea y RDS sin Multi-AZ para cuidar el presupuesto. El ciclo de entrega usa **GitHub Actions**: build, push a ECR y despliegue en ECS (HU-09.1).

## Alternativas consideradas

| Alternativa | Por qué no |
|---|---|
| Configuración manual por consola | No repetible ni auditable; los entornos divergen y se pierde conocimiento. |
| CloudFormation / CDK | Válidos, pero atan a AWS y el equipo prefiere un lenguaje declarativo y portable. |
| CI autogestionado (p. ej. Jenkins) | Obliga a operar un servidor adicional sin beneficio para este tamaño de proyecto. |

## Consecuencias

**Positivas (+)**
- Entornos reproducibles; cambios revisables en el repositorio.
- Despliegues automáticos y consistentes en cada sprint.
- Dev/pruebas más económicos con la misma plantilla.

**Negativas / trade-offs (−)**
- Curva de aprendizaje de Terraform y gestión del estado (state) remoto.
- Pruebas sin Multi-AZ no ejercitan la conmutación por fallo de producción.

## Enlaces

- HU-09.1 (épica EP09).
- Relacionados: [ADR-0002](0002-backend-java-spring-boot.md), [ADR-0010](0010-despliegue-aws-ecs-fargate.md), [ADR-0015](0015-respaldos-diarios-aws-backup.md).
