# Skill: crear-producto (LAB 10)

Skill de Claude Code con el **flujo de trabajo completo para crear productos de
software con IA**, de la idea al deploy. Pensada para usarse en todos los proyectos
(nuevos y existentes).

## Qué hace

Conduce 5 fases en cadena, donde **los documentos son los entregables**:

```
PRD.md → DESIGN.md → CLAUDE.md → Desarrollo (spec → código → verificar) → Desplegar
```

Lleva el estado y los pendientes de cada producto en `docs/PRODUCTO-ESTADO.md`, así
que al activarla siempre sabés en qué fase vas y qué falta.

## Instalación

Copiá la carpeta a tus skills de Claude Code:

- Global (todos los proyectos): `~/.claude/skills/crear-producto/`
- Por repo: `.claude/skills/crear-producto/`

Se activa con `/crear-producto` o sola cuando empezás/retomás un producto.

## Contenido

- `SKILL.md` — la guía con las 5 fases, hábitos para dirigir al agente y cuidados.
- `plantillas/` — `PRODUCTO-ESTADO.md`, `PRD.md`, `DESIGN.md`, `CLAUDE.md`, `spec.md`.
- `referencias/` — `prompt-prd.md` (prompt de PM senior) y `herramientas.md`
  (checklist de CLIs y comandos de deploy).

## Stack por defecto del flujo

Next.js + Supabase, deploy en Vercel, Railway solo si hay backend always-on.
