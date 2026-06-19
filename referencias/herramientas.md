# Herramientas (CLI), logins y deploy

El stack por defecto del flujo es Next.js + Supabase, deploy en Vercel, Railway solo si
hay backend always-on. Esta referencia cubre cómo verificar las CLIs y desplegar.

> Los comandos de verificación están en dos sabores: PowerShell (Windows) y bash
> (macOS/Linux). Usá el que corresponda a tu sistema.

## Checklist de verificación

Corré esto al empezar un proyecto para confirmar instalación + login.

**PowerShell (Windows):**
```powershell
foreach ($t in "node","npm","claude","git","gh","docker","supabase","vercel","railway") {
  if (Get-Command $t -ErrorAction SilentlyContinue) { "OK    $t" } else { "FALTA $t" }
}
```

**bash (macOS/Linux):**
```bash
for t in node npm claude git gh docker supabase vercel railway; do
  command -v "$t" >/dev/null && echo "OK    $t" || echo "FALTA $t"
done
```

Y para confirmar los logins:
```
gh auth status         # debe decir que estás logueado
vercel whoami          # cuenta de Vercel
railway whoami         # cuenta de Railway (solo si hay backend)
supabase projects list # confirma login de Supabase
docker info            # el daemon debe responder (abrir Docker Desktop)
```

> Nota (Windows): si `supabase` se instaló con scoop y no aparece, agregá el shim al
> PATH de la sesión: `$env:Path += ";$env:USERPROFILE\scoop\shims"`, o reabrí la terminal.

## Logins (si falta alguno)
```
gh auth login
vercel login
railway login        # solo si hay backend
supabase login
```

## Deploy (Fase 5)
1. Subir el repo a GitHub:
   ```
   git init && git add . && git commit -m "init"
   gh repo create <nombre> --private --source=. --push
   ```
2. Frontend en Vercel: `vercel` (preview) / `vercel --prod` (producción).
3. Supabase producción: `supabase link` y luego `supabase db push` (migraciones).
4. Backend always-on en Railway (si aplica): `railway up`.
5. Verificar la URL en vivo y compartir.

## Dónde vive cada parte (default del flujo)
| Parte | Dónde |
|---|---|
| Frontend + APIs livianas | Vercel |
| Base de datos, auth, storage | Supabase |
| Backend always-on (workers, colas, websockets) | Railway / Render / Fly.io |
