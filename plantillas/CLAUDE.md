# <Nombre del proyecto>

Resumen en 1-2 líneas de qué es. El contexto de producto vive en docs/PRD.md y el
sistema de diseño en DESIGN.md — leelos antes de cualquier trabajo no trivial.

## Tech stack
- Frontend + backend liviano: Next.js (App Router, TypeScript)
- Base de datos, auth y storage: Supabase (Postgres)
- Estilos: Tailwind CSS, según DESIGN.md
- Deploy: Vercel (frontend). Railway solo si hay backend always-on.

## Comandos
- Dev: npm run dev
- Build: npm run build
- Lint: npm run lint
- Tests: npm test
- Migraciones: supabase db push

## Convenciones
- TypeScript estricto, sin "any".
- Componentes en src/components, rutas en src/app.
- Nombres del código en inglés; comentarios al mínimo.
- Plata en centavos (enteros), nunca floats.

## Reglas (qué NO hacer)
- No instalar librerías nuevas sin justificar.
- No modificar migraciones ya aplicadas.
- No exponer secrets: usar variables de entorno.

## Cómo correr y deployar
- Local: supabase start + npm run dev
- Deploy: push a main → Vercel. Backend (si existe) → Railway.

## Links
- docs/PRD.md
- DESIGN.md
