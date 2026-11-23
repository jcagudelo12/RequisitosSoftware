# ADR-NNNN: <Título de la decisión>

*1. Título y número: identificador estable y consecutivo global; el título nombra la decisión, no el tema.*

| Campo | Valor |
|---|---|
| Número | ADR-NNNN |
| Estado | Propuesto / Aceptado / Superado / Obsoleto |
| Fecha | AAAA-MM-DD |
| Decisores | ... |
| Supersede / Superado por | — |
| Origen | ... |

## Estado

*2. Propuesto, Aceptado, Superado (indicar «Superado por ADR-XXXX») u Obsoleto. Se refleja en la tabla de metadatos.*

## Contexto

*3. Fuerzas, restricciones y problema que obligan a decidir. Incluir cifras (equipo, plazo, carga, NFR, presupuesto).*

## Decisión

*4. Qué se decide, dicho en claro, en un párrafo o viñetas cortas. Empezar con «Decidimos…».*

## Alternativas consideradas

*5. Opciones evaluadas y por qué no se eligieron.*

| Alternativa | Por qué no |
|---|---|
| ... | ... |

## Consecuencias

*6. Trade-offs: qué ganamos (+) y qué costo o riesgo asumimos (−).*

**Positivas (+)**
- ...

**Negativas / trade-offs (−)**
- ...

## Enlaces

*7. HU/CU/necesidades relacionadas, otros ADR (enlace relativo), specs en `../03-specs/` e issues.*

- ...

---

## Reglas

- **Una decisión por ADR.**
- **Inmutable:** no se edita. Para cambiarla se crea un ADR nuevo que lo supersede y se marca el anterior como «Superado por ADR-XXXX»; nunca se borra.
- **Conciso:** 1-2 páginas. Se documentan decisiones, no diseños detallados.
- **Estados válidos:** Propuesto, Aceptado, Superado, Obsoleto.
- **Granularidad:** decisiones costosas de revertir.
- **Va aquí:** stack tecnológico, diseño de sistema/arquitectura, motor de BD y estilo de API, autenticación, topología de despliegue, build vs buy.
- **No va aquí:** el detalle de una pantalla o feature (eso es Specs).
- **Nombre de archivo:** `NNNN-titulo-en-kebab-case.md`, consecutivo global, versionado junto al código.
