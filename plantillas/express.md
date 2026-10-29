# CLAUDE.md — API Express (Node.js)

## Comandos
- Instalar: `npm ci`
- Desarrollo: `npm run dev` (normalmente `node --watch src/server.js` o `tsx watch src/server.ts`)
- Tests: `npm test` (uno: `npx vitest run tests/pedidos.test.js` o `npx jest tests/pedidos.test.js`)
- Lint: `npm run lint` (corregir: `npx eslint . --fix`)
- Tipos (si es TypeScript): `npx tsc --noEmit`

## Estructura
- `src/app.js`: crea y configura la app (middlewares, rutas, manejador de errores). **Sin `listen`**.
- `src/server.js`: importa la app y llama a `listen` (así los tests usan la app sin abrir puertos).
- `src/routes/`: un `Router` por recurso. `src/services/`: lógica de negocio.

## Convenciones
- Valida toda entrada (`body`, `params`, `query`) con un esquema (zod, joi…) antes de usarla.
- Manejador de errores central con **cuatro** parámetros: `(err, req, res, next)`, registrado al final.
- En Express 5 las promesas rechazadas de handlers `async` llegan solas al manejador de errores; en Express 4 hay que capturarlas y llamar a `next(err)`.
- Respuestas de error con forma consistente: `{ "error": { "codigo": "...", "mensaje": "..." } }`.
- `helmet()` y CORS configurado con orígenes explícitos.
- Configuración desde variables de entorno, validada al arrancar.
- Tests HTTP con `supertest` contra `app`.

## Flujo
1. Revisa la ruta, el servicio y el middleware afectados.
2. Test con supertest del comportamiento esperado.
3. Implementa.
4. `npm run lint` + `npm test` antes de terminar.

## No hacer
- No dejar promesas sin `await` ni sin manejar dentro de los handlers.
- No devolver `err.stack` ni mensajes internos al cliente en producción.
- No usar `cors()` sin opciones (permite cualquier origen) en APIs con cookies o auth.
- No añadir dependencias sin preguntar; no commitear `.env`.
