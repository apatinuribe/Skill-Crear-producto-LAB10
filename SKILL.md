---
name: crear-producto
description: >
  Flujo de trabajo completo para crear productos de software con Claude, de la idea
  al deploy. Sigue 5 fases en cadena (PRD → DESIGN.md → CLAUDE.md → Desarrollo →
  Desplegar) y lleva el estado/pendientes de cada producto en docs/PRODUCTO-ESTADO.md.
  Activar SIEMPRE que el usuario: empiece un producto o feature nuevo, diga "vamos a
  construir/crear [algo]", pregunte "¿en qué vamos?" o "¿qué falta?" de un producto,
  pida un PRD / DESIGN.md / CLAUDE.md / spec, retome un proyecto existente para
  adaptarlo a este flujo, o vaya a desplegar. Consultarla al inicio de cada sesión de
  producto para ver pendientes y al cerrar para actualizar el estado. No activar para
  tareas técnicas puntuales sueltas (escribir una función, debuggear) salvo que sean
  parte de una feature del flujo.
---

# Crear producto con Claude

Flujo para construir software con IA sin saltarse pasos. La idea central: **los
documentos son los entregables**. Antes de escribir código, el agente ya conoce el
qué, el cómo se ve y el cómo se construye, y necesita muchas menos correcciones.

La cadena de documentos (cada uno reduce la incertidumbre del siguiente):

```
Conversación → PRD.md → DESIGN.md → CLAUDE.md → Desarrollo (spec → código → verificar) → Desplegar
```

- **PRD.md** — qué construir, para quién y por qué. Contexto permanente.
- **DESIGN.md** — cómo se ve y se siente (tokens + reglas de diseño).
- **CLAUDE.md** — cómo se construye (stack, comandos, convenciones, reglas).
- **Desarrollo** — features de punta a punta, una spec por feature.
- **Desplegar** — a internet con una URL real.

---

## ⚠️ Lo PRIMERO en cada activación (no te lo saltes)

1. **Ubicá el estado del producto.** Buscá `docs/PRODUCTO-ESTADO.md` en el proyecto
   actual (o `PRODUCTO-ESTADO.md` en la raíz).
   - **Si existe:** leelo. Reportá al usuario en qué FASE va, qué está ✅ hecho y qué
     queda ⬜ pendiente, y proponé **la siguiente acción concreta**. No empieces de cero.
   - **Si NO existe:** preguntá si es un **producto nuevo** o uno **existente** que
     queremos adaptar a este flujo. Después creá el archivo de estado copiando
     `plantillas/PRODUCTO-ESTADO.md` y completalo (para uno existente, marcá qué
     artefactos ya hay: ¿hay PRD?, ¿DESIGN.md?, ¿CLAUDE.md?, ¿código?).
2. **Nunca avances de fase sin cerrar la anterior.** Si falta el PRD, no escribas
   DESIGN.md. Si una fase tiene pendientes abiertos, resolvelos o anotalos antes de seguir.
3. **Al terminar cualquier trabajo de producto, actualizá `docs/PRODUCTO-ESTADO.md`**
   (marcá lo hecho, anotá decisiones y el próximo paso). Es la memoria del flujo.

> El archivo de estado es la fuente de verdad de "en qué vamos". Manténlo corto y al día.

---

## Verificación de herramientas (una vez por máquina)

Este flujo asume el stack por defecto **Next.js + Supabase**, deploy en **Vercel** y
backend always-on en **Railway** solo si hace falta. Antes de codear/desplegar, confirmá
que las CLIs están instaladas y logueadas (ver `referencias/herramientas.md`):

`node`, `npm`, `claude`, `git`, `gh` (logueado), `docker` (daemon corriendo),
`supabase` (logueado), `vercel` (logueado), `railway` (logueado, solo si hay backend).

Si algo falta o no está logueado, resolvelo antes de avanzar y anotalo en el estado.

---

## FASE 1 — Definir el PRD

**Objetivo:** un `docs/PRD.md` estable que le dé contexto permanente al agente.

**Cómo:** mantené una conversación tipo PM senior (una pregunta a la vez, cuestionando
supuestos), empezando por el **problema** y el **usuario**. No escribas el PRD de una;
descubrí el producto conversando. El prompt completo y la estructura de las 18 secciones
están en `referencias/prompt-prd.md`. La plantilla está en `plantillas/PRD.md`.

**Reglas clave:**
- Foco en el problema y el usuario, siempre. Nunca describas una feature como problema.
- Preguntá explícitamente el **MVP** ("si solo pudiéramos lanzar en 2 semanas, ¿qué?")
  y el **Fuera de alcance** ("¿qué NO queremos construir?"). Son las que más ahorran.
- Si hay contradicciones, no las resuelvas solo: pará y preguntá.

**Hecho cuando:** existe `docs/PRD.md` con problema, usuario, visión, estrategia,
principios, MVP, fuera de alcance y métricas, revisado por el usuario.

---

## FASE 2 — DESIGN.md

**Objetivo:** un `DESIGN.md` en la raíz con la dirección visual y reglas de diseño
(front matter YAML con tokens + prosa que explica el porqué).

**Cómo:** si hay skill `design-md-builder`, usalo a partir de capturas/URLs de
referencia. Si no, redactalo a mano con `plantillas/DESIGN.md`. Orden de secciones
obligatorio: Overview → Colors → Typography → Layout → Elevation → Shapes →
Components → Do's and Don'ts.

**Regla clave:** la sección **"Do not do"** es la más importante — la IA tiende a
agregar variantes, sombras y decoración de más. Las restricciones protegen el producto.

**Hecho cuando:** existe `DESIGN.md` válido (idealmente pasado por el linter
`@google/design.md`) con tokens, prosa y la sección de qué evitar.

---

## FASE 3 — CLAUDE.md

**Objetivo:** el manual operativo del repo — cómo se construye.

**Cómo:** corré `/init` para que Claude lo genere leyendo el repo, y ajustalo. Usá
`plantillas/CLAUDE.md` como esqueleto. Mantenelo **corto (< 300 líneas)**: por cada
línea preguntate "¿Claude se equivocaría sin esto?"; si no, borrala.

**Incluir:** resumen + links a PRD y DESIGN.md, tech stack, comandos exactos,
convenciones propias, reglas (qué NO hacer), cómo correr y desplegar.
**Excluir:** lo que se deduce del código, docs de API, obviedades.

**Hecho cuando:** existe `CLAUDE.md` corto, con links al PRD y al DESIGN.md, stack,
comandos y reglas; committeado al repo.

---

## FASE 4 — Desarrollo

**Tu rol cambia: de escribir a dirigir.** El agente es rápido y complaciente — esa es
su trampa. Hacelo pensar, dudar y dar alternativas.

**Proceso por feature (Spec-Driven, vertical):**
1. Pedí a Claude que lea `docs/PRD.md` y liste las features propuestas. Elegí UNA.
2. Research + conversación → escribí su **spec** (`plantillas/spec.md`). El código sale
   de la spec, no al revés.
3. Implementá **vertical** (pantalla + lógica + datos que funcionen de punta a punta),
   no por capas.
4. Verificá con evidencia (tests/build/captura), commiteá, siguiente.

**Los 6 hábitos al hablar con Claude:**
1. Dale contexto y un criterio para verificar (apuntá a archivos con `@`, decí qué queda fuera).
2. Primero pensar, después codear (plan antes de tocar código; esperá tu OK).
3. Dudá de la primera respuesta (pedí edge cases y supuestos).
4. Hacelo investigar, no inventar (doc oficial, repo, git; delegá en subagente).
5. Cuestioná lo que dice (abogado del diablo; "decime si me equivoco").
6. Pedí alternativas (2-3 enfoques con pros y contras, y una recomendación).

**Manejo de contexto:** `/clear` al cambiar a una feature no relacionada; `/compact`
si seguís en lo mismo pero la conversación se llenó; `/branch` en un punto de
divergencia; ramas o git worktrees para features en paralelo.

**Hábitos:** una feature a la vez hasta que funcione; commits chicos y frecuentes;
probá cada pedazo apenas lo terminás; mantené el alcance del PRD (ante la duda, al MVP).

**Hecho cuando:** las features del MVP funcionan de punta a punta, verificadas y committeadas.

---

## FASE 5 — Desplegar

**Objetivo:** la app en internet con una URL real para compartir.

Pasos: subir el repo a GitHub → publicar el frontend en Vercel → conectar el Supabase
de producción y aplicar migraciones → levantar backend en Railway (si aplica) →
verificar en vivo y compartir el link. Detalle y comandos en `referencias/herramientas.md`.

**Hecho cuando:** la URL pública funciona y está verificada.

---

## ⚠️ Cuidados (sobre todo de seguridad)

- Secrets fuera del repo: van en `.env`, y `.env` al `.gitignore`. Subí `.env.example`
  sin valores. Si pegaste una clave real en un chat, **rotala**.
- Dev y producción separados (claves y bases distintas).
- Cuidado con comandos destructivos (`drop`/`delete`, `git reset --hard`, `rm -rf`):
  revisalos antes de aprobar; probá en local primero.
- Mirá el diff antes de pushear (`git status`). Dependencias con criterio. Ojo con los
  costos de APIs pagas. Cerrá los túneles al terminar.

---

## Archivos de esta skill

- `plantillas/PRODUCTO-ESTADO.md` — estado/pendientes por proyecto (copialo a `docs/`).
- `plantillas/PRD.md` — estructura del PRD.
- `plantillas/DESIGN.md` — esqueleto del DESIGN.md.
- `plantillas/CLAUDE.md` — esqueleto del CLAUDE.md.
- `plantillas/spec.md` — template de spec por feature.
- `referencias/prompt-prd.md` — el prompt de PM senior + las 18 secciones del PRD.
- `referencias/herramientas.md` — checklist de CLIs, logins y comandos de deploy.
