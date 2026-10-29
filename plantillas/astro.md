# CLAUDE.md — Sitio Astro

## Comandos
- Desarrollo: `npm run dev`
- Build: `npm run build` · Vista previa: `npm run preview`
- Verificar tipos y contenido: `npx astro check`
- Añadir integraciones: `npx astro add <integracion>` (react, tailwind, mdx, sitemap…)

## Convenciones
- Por defecto **cero JavaScript** en el cliente: componentes `.astro` para todo lo estático.
- Islas solo cuando haga falta interactividad, con la directiva más perezosa posible:
  `client:visible` o `client:idle` antes que `client:load`.
- Contenido (blog, docs) en colecciones de contenido con esquema zod definido en `src/content.config.ts`
  (en proyectos anteriores a Astro 5: `src/content/config.ts`). Consulta con `getCollection` / `getEntry`.
- Imágenes con `<Image />` / `<Picture />` de `astro:assets`, siempre con `alt`.
- Layouts en `src/layouts/`, con `<slot />`; metadatos SEO centralizados en un componente.
- Variables de entorno: `import.meta.env`; solo las que empiezan por `PUBLIC_` llegan al cliente.

## Flujo
1. Revisa la página (`src/pages/`), su layout y la colección implicada.
2. Implementa.
3. `npx astro check` + `npm run build` (detecta enlaces y frontmatter rotos) antes de terminar.

## No hacer
- No convertir páginas enteras en componentes de React/Vue solo por costumbre.
- No usar `client:only` salvo que el componente no pueda renderizarse en servidor.
- No cambiar el `output` (`static` / `server`) ni el adaptador sin preguntar.
- No meter imágenes grandes en `public/` si pueden optimizarse desde `src/assets/`.
