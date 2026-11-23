# Strategy — StockCentral

| Campo | Valor |
|---|---|
| Producto | StockCentral (nombre de trabajo) |
| Versión | 1.0 |
| Estado | Vigente |
| Fecha | 2026-11-22 |
| Próxima revisión | 2027-02-22 (trimestral) |
| Fuente | Entrega N.°4 — «Sistema Centralizado de Gestión de Inventarios y Control de Stock en Tiempo Real», Grupo 1, Ingeniería de Requisitos, ITM, 2026 |

---

## 1. Resumen ejecutivo

StockCentral sirve a una pyme de retail de Medellín con 5 sedes de venta y una bodega central (6 ubicaciones, ~4.000 referencias) que hoy controla su inventario con hojas de cálculo enviadas por correo o WhatsApp.

**¿Por qué ganamos hoy?** Porque eliminamos el dolor concreto del status quo: no hay vista consolidada, ni trazabilidad, ni control de quién cambia las cantidades. Un solo catálogo, un solo kardex y un solo lugar donde ver el stock de todas las sedes.

**¿Por qué seguiremos ganando mañana?** Porque cada día de uso aumenta lo que costaría abandonarlo: el kardex histórico es irrepetible, alimenta alertas y sugerencias de reposición que mejoran con el tiempo, el sistema se vuelve el punto central entre ventas y contabilidad, y abrir una sede nueva cuesta casi nada. Defensibilidad principal: **costos de cambio + datos de operación**, reforzada por integración y escala operativa.

> Toda ventaja descrita aquí es, por ahora, una hipótesis: el sistema aún no está en producción (ver sección 3).

## 2. Mercado y problema

### Segmento
Pyme de retail multisede a nivel ciudad, con operación creciente (varias sedes y catálogo extenso) que sigue controlando inventario con hojas de cálculo.

### Cliente y usuarios
- **Cliente:** la administración de la empresa, que decide compras y se relaciona con proveedores y contabilidad.
- **Usuarios (roles):** administrador, bodeguero, comprador, contador y jefe de sede, cada uno con permisos y sede asignada.

### El problema (8 problemas del documento, agrupados por impacto de negocio)

| Impacto de negocio | Problemas (n.° en el documento) |
|---|---|
| **Capital inmovilizado y ventas perdidas** | 1. Sin vista consolidada en tiempo real: una sede se queda sin producto mientras otra lo tiene sobrestockeado. 4. Sin alertas de mínimo ni rotación: compras por intuición y acumulación de baja rotación. |
| **Pérdida de control y de confianza en el dato** | 2. Traslados sin trazabilidad de quién despachó, quién recibió y cuándo. 3. Cualquiera puede alterar cantidades sin dejar rastro. 5. Conteos físicos manuales, de varios días, sin conciliación controlada. |
| **Costo administrativo y errores financieros** | 6. Valoración a contabilidad manual, tardía y con errores de digitación. |
| **Fricción para crecer y riesgo operativo** | 7. Abrir una sede implica copiar archivos, reconstruir catálogo y capacitar desde cero. 8. Archivos en equipos locales sin respaldo formal. |

### Alternativas actuales con las que compite
| Alternativa | Por qué la usan | Dónde queda corta |
|---|---|---|
| Status quo: hojas de cálculo por sede | Costo cero, conocido | Es la causa de los 8 problemas; no escala con sedes ni referencias |
| ERP/POS genérico en la nube con módulo de inventario | Cobertura amplia, cuota mensual conocida | Procesos genéricos; el inventario multisede es un módulo más, no el centro |
| Módulo de inventario del sistema de ventas existente | Ya está integrado a la venta | Su foco es vender; depende del proveedor y de su hoja de ruta |

### Propuesta de valor
Una única fuente de verdad del stock de todas las ubicaciones, con trazabilidad completa de cada movimiento, información a tiempo para decidir compras y valoración confiable para contabilidad, y con apertura de sedes sin fricción.

### Qué NO es el producto (límite estratégico)
No es facturación ni punto de venta, no es contabilidad, no es aplicación móvil nativa, no es tienda en línea, no es logística de rutas ni proyección de demanda con inteligencia artificial. Integra con ventas y contabilidad, pero no compite con ellas.

## 3. Defensibilidad: tipos + evidencia

**Tesis central:** con catálogo, existencias por sede, kardex histórico y procesos de 6 ubicaciones montados sobre StockCentral, cambiarlo frena la operación y empeora las decisiones.

| Tipo de moat | Tesis | Evidencia esperada / cómo se prueba | Estado de la evidencia |
|---|---|---|---|
| **Costos de cambio** | Catálogo único (~4.000 referencias), stock por sede, kardex histórico, costo promedio ponderado y procesos de 6 ubicaciones viven en el sistema. Reemplazarlo implica migrar historia, reintegrar ventas y contabilidad y recapacitar a los 5 roles. | % de movimientos registrados en el sistema; antigüedad del kardex; % de ubicaciones operando; ausencia de hojas paralelas. | Hipótesis — se valida con el % de movimientos en el sistema (vs fuera de él) tras la salida a producción. |
| **Datos de operación** | El kardex y la rotación alimentan alertas de mínimo y sugerencias de reposición que mejoran con el tiempo. El histórico es irrepetible: un sistema nuevo parte de cero. | % de referencias con mínimo/máximo definido; tasa de sugerencias de reposición aceptadas; quiebres de stock con existencia en otra sede. | Hipótesis — se valida con % de referencias con mín/máx y quiebres evitados tras la salida a producción. |
| **Plataforma / integración** | Se vuelve el punto central entre el sistema de ventas (descuenta stock) y la contabilidad (recibe valoración y movimientos). | % de ventas descontadas automáticamente; días para entregar la valoración a contabilidad. | Hipótesis — se valida con ambas métricas tras la salida a producción (integraciones aún con documentación desconocida). |
| **Escala operativa** | Abrir una sede = crear la sede, asignar usuarios y habilitar su IP, sin instalar software: costo marginal casi nulo por ubicación. | Tiempo para abrir una sede nueva frente a la copia manual de archivos y capacitación desde cero. | Hipótesis — se valida con el tiempo de apertura de la primera sede nueva tras la salida a producción. |

**Evidencia de partida que sí existe:** los 8 problemas documentados prueban el dolor del segmento; la relación épica↔problema (EP01-EP09) muestra que cada problema tiene respuesta planificada; la estimación (1.505 h, 14 semanas/7 sprints) acota la inversión.

**Tipos que no aplican o son débiles:**
- **Red:** es un sistema interno de una sola empresa; no hay efectos de red entre clientes.
- **Marca:** no es un producto de mercado masivo; no hay marca que defender.
- **Regulación:** el documento no identifica barreras regulatorias; no es un moat aplicable.

## 4. Amenazas y erosión

| Amenaza | Qué erosiona | Señal temprana | Mitigación estratégica |
|---|---|---|---|
| ERP/POS en la nube con inventario a bajo costo mensual | Costos de cambio y valor percibido frente a la cuota recurrente | Comparaciones de precio por parte de la administración; solicitud de cotizaciones | Profundizar lo que lo genérico no tiene: datos históricos propios, apertura de sedes sin fricción y procesos a la medida |
| El proveedor del sistema de ventas agrega inventario multisede | Plataforma/integración | Anuncios del proveedor; pedido de «usar solo el módulo de ventas» | Ser el hub que integra ventas y contabilidad y acumular historia que el módulo del proveedor no tendría |
| Resistencia al cambio y hojas de cálculo paralelas | Datos de operación y costos de cambio (el kardex deja de ser la verdad) | Baja del % de movimientos en el sistema; archivos enviados por correo/WhatsApp | Kardex como única fuente de verdad; capacitación por rol; seguimiento por sede |
| Dependencia de integraciones externas con documentación aún desconocida | Plataforma/integración | Retrasos o fallos en la integración con ventas o contabilidad | Reconocido en el documento: se aplica 15 % de contingencia; integrar temprano |
| Costo de operación recurrente (≈ $1.200.000 COP/mes) frente al valor percibido | Justificación económica del sistema | Cuestionamiento del gasto mensual; poco uso | Medir y comunicar valor generado (ver sección 6) |
| Datos incompletos en la carga inicial | Datos de operación; confianza en el sistema | Diferencias grandes en las primeras tomas físicas | Cargar catálogo e historial completos desde el día uno; conciliar con toma física temprana |

## 5. Plan para profundizar

Movimientos estratégicos, alineados al cronograma de 7 sprints / 14 semanas y a la operación posterior.

| Momento | Movimiento | Moat que profundiza |
|---|---|---|
| Sprint 3 (semanas 5-6) | Cargar todo el catálogo e historial desde el día uno; que la carga inicial sea completa y validada | Costos de cambio, datos |
| Sprints 3-5 (semanas 5-10) | Hacer del kardex la única fuente de verdad: cero movimientos fuera del sistema (entradas, traslados, toma física) | Costos de cambio, datos |
| Sprints 6-7 (semanas 11-14) | Integrar ventas y contabilidad antes de la salida a producción, para volverse el hub entre ambos | Plataforma |
| Salida a producción y primeros trimestres | Convertir la rotación en sugerencias de reposición útiles y definir mín/máx en todo el catálogo | Datos |
| Operación continua | Estandarizar la apertura de sedes como proceso repetible | Escala operativa |
| Cada trimestre | Medir las métricas de la sección 6 y convertir hipótesis en evidencia | Todos |

> Momentos alineados con el cronograma de sprints de la Entrega N.°4 (§13.5-13.6).

### Cómo esta estrategia prioriza ADRs, Specs y roadmap
- **ADRs:** tienen prioridad las decisiones que protegen el histórico y la confianza en el dato (kardex inmutable, auditoría, respaldos), el control de acceso por rol y sede y la integración desacoplada con ventas y contabilidad. Ver [ADRs](../02-adrs/).
- **Specs:** se priorizan los flujos que alimentan el kardex y lo vuelven la fuente de verdad: entrada de mercancía (CU.0002), traslados (CU.0003) y toma física con conciliación (CU.0004), más el tablero consolidado. Ver [Specs](../03-specs/).
- **Roadmap:** EP04 (movimientos), EP03/EP07 (stock consolidado y reportes), EP05 (alertas y reposición), EP06/EP08 (valoración e integración con contabilidad) y EP02 (sedes) van antes que lo que no profundiza el foso.
- Lo que no refuerza ningún moat se pospone.

## 6. Métricas de defensibilidad

Las metas son **objetivos propuestos**, no resultados medidos; se ajustan en la primera revisión tras producción.

| Métrica | Qué moat valida | Definición / fórmula | Meta (propuesta) | Frecuencia | Fuente |
|---|---|---|---|---|---|
| Registro en sistema (retención/adopción) | Costos de cambio | Movimientos registrados en el sistema ÷ movimientos totales (incluidos los hechos fuera de él) | ≥ 98 % | Mensual | Kardex y verificación en sedes |
| Usuarios activos semanales por sede | Costos de cambio | Usuarios con al menos un movimiento o consulta en la semana ÷ usuarios asignados | ≥ 80 % por sede | Semanal | Log de auditoría |
| Valor generado vs costo total (equivalente LTV/CAC) | Justificación económica | Valor generado acumulado ÷ (inversión $204.718.080 COP con IVA + operación ≈ $1.200.000 COP/mes × meses). Incluye periodo de recuperación en meses | Relación ≥ 1 dentro de un horizonte a definir con el cliente | Trimestral | Finanzas del cliente; estimación |
| Ubicaciones operando | Costos de cambio, escala | Ubicaciones con movimientos diarios ÷ 6 | 6/6 | Mensual | Kardex |
| Referencias con mín/máx definido | Datos de operación | Referencias con mín. y máx. ÷ ~4.000 | ≥ 90 % | Mensual | Catálogo |
| Ventas descontadas automáticamente | Plataforma | Ventas con descuento automático ÷ ventas totales | ≥ 99 % | Mensual | Integración con ventas |
| NPS interno por rol | Todos | % promotores − % detractores, por cada uno de los 5 roles | ≥ 30 | Trimestral | Encuesta interna |
| Tiempo para abrir una sede nueva | Escala operativa | Días desde la decisión hasta operar (sede creada, usuarios, IP) | Menos de 1 día (propuesta) | Por evento | Registro de apertura |
| Exactitud del inventario en tomas físicas | Datos de operación | 1 − (diferencia absoluta ÷ unidades del sistema) por sede | ≥ 98 % | Por toma física | Conciliación |
| Días para entregar la valoración a contabilidad | Plataforma | Días entre el cierre del periodo y el envío de la valoración | ≤ 1 día | Mensual | Integración con contabilidad |
| Quiebres con existencia en otra sede | Datos de operación | Eventos de producto agotado en una sede con stock disponible en otra | Tendencia a la baja | Mensual | Kardex y alertas |

## Revisión

- **Cadencia:** trimestral (próxima: 2027-02-22).
- **Responsable:** Product Owner del cliente.
- **Disparadores de revisión extraordinaria:** cambio de mercado (nuevas alternativas, cambio de oferta del proveedor de ventas) o cambio de estrategia de la empresa.

## Referencias

- [Visión](vision.md)
- [Plantilla de Strategy](template.md)
- [Índice de documentación (README)](../README.md)
- Fuente: Entrega N.°4 — «Sistema Centralizado de Gestión de Inventarios y Control de Stock en Tiempo Real», Grupo 1, Ingeniería de Requisitos, ITM, 2026.
