# Mockup NNNN — <Pantalla o flujo>

| Campo | Valor |
|---|---|
| **ID** | NNNN |
| **Pantalla / flujo** | _Nombre de la pantalla o del flujo que representa (p. ej. «Registrar entrada de mercancía»)._ |
| **Archivo** | `04-mockups/NNNN-feature.html` (autocontenido y navegable) |
| **Spec relacionada** | `../03-specs/NNNN-feature.md` (mismo número que el mockup) |
| **Fidelidad** | Baja / Media / Alta — _indica cuál y su base (p. ej. «Alta — basado en el prototipo Figma del equipo» + enlace)._ |
| **Versión** | 0.1 |
| **Estado de aprobación** | Pendiente de aprobación / Aprobado / Rechazado |

> **Definición.** Representación visual de una pantalla o flujo **antes** de construirlo: cómo se ve y cómo se navega. Sirve para validar diseño y experiencia con el responsable antes del primer código. Es desechable e iterativo; se aprueba antes de codificar. Se enfoca en layout, flujo y estados, **no en la lógica** (la lógica y los contratos de datos van en la Spec).

---

## 1. Objetivo y pantalla

_Qué se quiere validar con este mockup y qué pantalla(s) real(es) del producto representa. Indica usuarios/rol, trazabilidad (HU, CU, problema que resuelve) y enlace a la spec y al prototipo de origen si existe._

- **Objetivo:**
- **Usuarios / rol:**
- **Trazabilidad:** HU-… · CU.… · Problema …
- **Spec:** `../03-specs/NNNN-feature.md`
- **Pantallas clave (vistas del flujo):**

## 2. Flujo / navegación

_Cómo se pasa de una pantalla a otra. Lista de pantallas y transiciones, o un mini diagrama (A → B → C). Indica qué acciones de la UI disparan cada transición._

```
Lista → Formulario → Confirmar → Guardando → Éxito → Detalle
```

## 3. Estados

_Cubre los estados, no solo el «happy path»: normal, vacío, carga, error, éxito (y los propios de la pantalla). Para cada uno indica cuándo ocurre y cómo verlo en el mockup (enlace por hash)._

| Estado | Cuándo ocurre | Cómo verlo en el mockup |
|---|---|---|
| Normal | | `NNNN-feature.html#pantalla=<id>&estado=normal` |
| Vacío | | `#pantalla=<id>&estado=vacio` |
| Carga | | `#pantalla=<id>&estado=carga` |
| Error | | `#pantalla=<id>&estado=error` |
| Éxito | | `#pantalla=<id>&estado=exito` |

## 4. Componentes reutilizados

_Primitivas del design system StockCentral usadas en esta pantalla (ver «Design system StockCentral» más abajo). Si se necesita un componente nuevo, descríbelo aquí y propón su incorporación al design system._

- 

## 5. Responsive

_Breakpoints y comportamiento por tamaño._

- **≥ 1280 px:** layout completo (sidebar 232 px).
- **768–1279 px:** sidebar colapsada a iconos (64 px); tablas con scroll horizontal.
- **< 768 px:** sidebar como drawer con botón hamburguesa; KPIs y formularios en 1 columna; tablas con scroll horizontal.
- _La app móvil nativa está fuera de alcance; el uso principal es escritorio en las sedes._

## 6. Notas de interacción

_Comportamientos que el mockup no implementa pero que el responsable debe conocer (autocompletados, validación en línea, atajos, actualización automática, confirmaciones…). Describe la intención, no la lógica ni los contratos de datos._

- 

### Accesibilidad

_Contraste (AA), foco visible, lectura por lector de pantalla, etiquetas asociadas, mensajes `aria-live`, errores con `role="alert"`, comportamiento del foco en modales/drawers (foco inicial, Escape cierra)._

- 

### Antes / después (solo para arreglos sobre lo existente)

_Si el mockup corrige una pantalla ya construida, basta con una captura «antes» y otra «después». Si es una pantalla nueva escribe: «No aplica — pantalla nueva»._

- 

## 7. Aprobación del responsable

_La aprobación es el gate: no se codifica hasta que el responsable apruebe._

- **Responsable:** Product Owner — administrador general de la empresa cliente
- **Estado:** Pendiente de aprobación
- **Fecha:** —
- **Observaciones:** —

---

## Design system StockCentral

Sistema visual común a todos los mockups. Cada `NNNN-feature.html` lo embebe (CSS y JS en el mismo archivo, sin dependencias externas: sin CDN, sin Google Fonts, sin imágenes remotas) para que se vea como un solo producto.

### Tipografía

`font-family: Inter, -apple-system, "Segoe UI", Roboto, Helvetica, Arial, sans-serif;` — tamaño base 14 px.

### Tokens de color (`:root`)

| Token | Valor | Uso |
|---|---|---|
| `--sidebar-bg` | `#1b2533` | Fondo de la barra lateral |
| `--sidebar-text` | `#c9d1dc` | Texto de la barra lateral |
| `--sidebar-muted` | `#7d8899` | Títulos de grupo y pie de la sidebar |
| `--sidebar-active` | `#2b3a4f` | Ítem activo del menú |
| `--brand` | `#f59e0b` | Logo (cuadrado ámbar «SI») |
| `--bg` | `#f3f4f6` | Fondo de la página |
| `--surface` | `#ffffff` | Tarjetas, topbar, modales |
| `--border` | `#e5e7eb` | Bordes y separadores |
| `--text` | `#111827` | Texto principal |
| `--muted` | `#6b7280` | Texto secundario |
| `--primary` / `--primary-hover` | `#2563eb` / `#1d4ed8` | Acción primaria, foco, enlaces |
| `--success` / `--success-bg` | `#15803d` / `#dcfce7` | Éxito, «Normal», botón PDF |
| `--warning` / `--warning-bg` | `#b45309` / `#fef3c7` | Advertencias |
| `--danger` / `--danger-bg` | `#b91c1c` / `#fee2e2` | Errores, stock bajo el mínimo |
| `--info` / `--info-bg` | `#1d4ed8` / `#dbeafe` | Información, borradores |
| `--radius` / `--radius-sm` | `8px` / `6px` | Radios |
| `--shadow` | `0 1px 2px rgba(16,24,40,.06)` | Sombra de tarjetas |
| `--chrome-bg` / `--chrome-text` | `#312e81` / `#ffffff` | **Solo chrome del mockup** |

### Layout (referencia 1440 × 900)

- Sidebar fija de **232 px** (64 px en tablet; drawer en móvil).
- Topbar de **64 px**, blanca, con borde inferior: título + breadcrumb, buscador «Buscar producto por código o nombre…», indicador de sede y avatar con iniciales + nombre + rol.
- Contenido con padding de **24 px** sobre `--bg`.
- Sidebar: logo («SI» + «Stock Central»); Dashboard; grupo **INVENTARIO** (Productos, Entradas, Salidas y ajustes, Traslados, Toma física, Kardex); grupo **GESTIÓN** (Compras, Reportes PDF, Sedes y usuarios, Auditoría); pie con punto verde + «IP 190.85.42.17 · Sede autorizada». Los ítems con mockup enlazan a su HTML (Dashboard → 0004, Entradas → 0001, Traslados → 0002, Toma física → 0003); el resto lleva `href="#"`, `aria-disabled="true"` y `title="Fuera del alcance de este mockup"`.

### Primitivas (clases)

`.btn` (`.btn-primary`, `.btn-secondary`, `.btn-ghost`, `.btn-danger`, `.btn-success`) · `.card` · `.kpi` · `.table` (cabecera gris, 11 px, mayúsculas) · `.badge` (`.badge-success` «Normal», `.badge-warning`, `.badge-danger`, `.badge-info`, `.badge-neutral`) · `.field` (label + input + ayuda/error; asterisco en obligatorios) · `.input` / `.input.is-error` (borde rojo + mensaje con `role="alert"`) · `.modal` (overlay + `role="dialog" aria-modal="true"`) · `.toast` · `.empty-state` (icono + título + texto + CTA) · `.skeleton` (shimmer) · `.alert-banner` (info / warning / danger) · `.drawer`.

### Chrome del mockup (NO es parte del producto)

Barra superior fija de 44 px con `--chrome-bg` que contiene: «MOCKUP NNNN · título», enlace «Ver spec» (`../03-specs/NNNN-….md`), selector de **Pantalla** (pasos del flujo), selector de **Estado** y botón «Notas del mockup» que abre un panel lateral derecho con las secciones de esta plantilla. La navegación es por hash enlazable (`#pantalla=formulario&estado=error`) y, además, los botones de la UI navegan entre pantallas. El JS solo cambia de pantalla/estado: no hay cálculos ni validaciones reales, los valores son estáticos de ejemplo.

### Accesibilidad base

Contraste AA · `:focus-visible` con outline de 2 px `--primary` · labels asociados a sus campos · `aria-live="polite"` para mensajes · `role="alert"` en errores · modal con foco inicial, foco atrapado y cierre con Escape · `lang="es"`.

### Datos de ejemplo

Ubicaciones: Bodega central (BOD), Centro (CEN), Laureles (LAU), Poblado (POB), Envigado (ENV), Bello (BEL). Moneda COP con punto de miles (`$28.500`). Fechas `dd/mm/aaaa hh:mm`.

---

## Reglas

1. El mockup es un **HTML autocontenido y navegable**, nombrado `NNNN-feature.html`, con **el mismo número** que su spec (`../03-specs/NNNN-feature.md`).
2. **Cubrir los estados** (vacío, carga, error, éxito), no solo el «happy path».
3. **Reutilizar el design system** y sus primitivas; no inventar componentes si ya existen.
4. **Sin lógica ni contratos de datos** (eso es la Spec): el JS solo cambia de pantalla/estado y los valores son de ejemplo.
5. **La aprobación del responsable es el gate**: no se codifica hasta que el mockup esté aprobado. El mockup es desechable e iterativo.
6. En **arreglos sobre lo existente** basta una captura antes/después.
