# Botiquín Tongoy · Asistente de Orientación Farmacéutica

Chat de orientación general sobre medicamentos para personas usuarias del Botiquín CESFAM Tongoy, Posta de Salud Rural Guanaqueros, Puerto Aldea y alrededores (comuna de Coquimbo, Chile).

No reemplaza la atención de la Químico Farmacéutica ni del equipo de salud. No diagnostica, no indica cambios de dosis y no emite recetas. Ante urgencias deriva al 131 (SAMU).

## Cómo funciona

```
Persona usuaria ──> index.html (GitHub Pages) ──> Worker (Cloudflare) ──> API de Anthropic (Claude)
```

- `index.html`: la página que ve la persona usuaria. No contiene claves.
- `worker.js`: servidor intermedio. Guarda las instrucciones de seguridad del asistente y la clave de la API como secreto de Cloudflare.

## Configuración

1. **Clave de la API.** Crear una clave en https://console.anthropic.com y definir un límite de gasto mensual.
2. **Worker de Cloudflare.**
   - Crear un Worker y pegar el contenido de `worker.js`.
   - En *Settings → Variables and Secrets* agregar:
     - `ANTHROPIC_API_KEY` (Secret): la clave del paso 1.
     - `ALLOWED_ORIGIN` (Text): la dirección de la página, por ejemplo `https://usuario.github.io`.
     - `MODEL` (Text, opcional): modelo a usar. Por defecto, `claude-sonnet-5-5`.
3. **Página.**
   - En `index.html`, reemplazar `https://TU-WORKER.workers.dev` por la dirección del Worker.
   - Activar GitHub Pages en *Settings → Pages*, rama `main`, carpeta raíz.

## Seguridad y privacidad

- La clave de la API **nunca** debe subirse al repositorio.
- El Worker solo acepta solicitudes desde la dirección definida en `ALLOWED_ORIGIN`.
- El Worker limita el largo de los mensajes y de las respuestas.
- La página no guarda conversaciones. Las consultas se procesan en la API de Anthropic, por lo que se pide a las personas no escribir RUT, dirección ni datos personales.
- Las detecciones de urgencia ocurren también en la propia página, sin esperar al modelo.

## Datos institucionales

Los horarios, direcciones y contactos están en dos lugares:

- En `index.html`: el panel de horarios.
- En `worker.js`: las instrucciones del asistente, dentro de `DATOS INSTITUCIONALES`.

Si cambian, se deben actualizar en ambos archivos.

Última actualización de horarios: 29 de septiembre de 2026.
