# AGENTS.md — HACK//UTEC Frontend

> Archivo de referencia para agentes de código. Describe la arquitectura, convenciones y comandos de este proyecto.

## 1. Descripción general

Este repositorio contiene el **sitio web estático de HACK//UTEC**, una landing page para un hackathon organizado por UTEC. Aunque el campo `name` de `package.json` es `"attendance"`, el contenido real del sitio es la página promocional del evento HACK//UTEC 2026.

El sitio es una **aplicación de una sola página (SPA visual)** compuesta por secciones:

- **Navbar**: navegación fija con enlaces ancla.
- **Hero**: presentación principal del evento.
- **About**: descripción del hackathon y motivos para participar.
- **Schedule**: calendario interactivo por días (tabs con JavaScript nativo).
- **Prizes**: tabla de premios (puntos de examen y participación).
- **Register**: llamado a la acción con enlace a Google Forms.
- **Footer**: información de contacto y accesos directos.

El idioma del contenido visible es **español**.

## 2. Stack tecnológico

| Capa | Tecnología | Versión / Notas |
|------|-----------|-----------------|
| Framework | [Astro](https://astro.build/) | `^5.5.6` |
| Integración UI | `@astrojs/react` | `^4.2.3` (React disponible pero no utilizado actualmente) |
| Estilos | [Tailwind CSS](https://tailwindcss.com/) | `^4.0.17` con plugin `@tailwindcss/vite` |
| Animaciones CSS | `tw-animate-css` | `^1.2.5` |
| Íconos | `lucide-react` | configurado como librería de íconos de shadcn/ui |
| Temas | `next-themes` | instalado pero no utilizado actualmente |
| Notificaciones | `sonner` | instalado pero no utilizado actualmente |
| Autenticación | `@react-oauth/google`, `jwt-decode` | instalados pero no utilizados actualmente |
| Gestor de paquetes | [pnpm](https://pnpm.io/) | vía `pnpm-lock.yaml` y `pnpm-workspace.yaml` |
| TypeScript | `astro/tsconfigs/strict` | con alias `@/*` apuntando a `./src/*` |

### Notas sobre shadcn/ui

El archivo `components.json` indica que el proyecto está configurado para usar [shadcn/ui](https://ui.shadcn.com/) con el estilo "new-york", Tailwind v4 y variables CSS. Sin embargo, **no hay componentes de shadcn/ui instalados** en `src/components/ui/` ni en `src/lib/utils`. La carpeta `src/components/` solo contiene componentes Astro propios.

## 3. Estructura del proyecto

```text
front-end/
├── astro.config.mjs          # Configuración de Astro (integración React + Tailwind Vite)
├── components.json           # Configuración de shadcn/ui
├── package.json              # Scripts y dependencias
├── pnpm-lock.yaml            # Lockfile de pnpm
├── pnpm-workspace.yaml       # Configuración del workspace de pnpm
├── tsconfig.json             # Configuración TypeScript (strict, alias @/*)
├── public/                   # Archivos estáticos servidos tal cual
│   ├── _headers              # Cabeceras HTTP personalizadas (COOP/COEP vacías)
│   ├── bg.svg                # Fondo de ruido visual
│   └── logo.webp             # Favicon / logo del evento
└── src/
    ├── components/           # Componentes Astro de la landing page
    │   ├── About.astro       # Incluye el resumen del reto y un modal con el contenido completo del PDF
    │   ├── Footer.astro
    │   ├── Hero.astro
    │   ├── Navbar.astro
    │   ├── Prizes.astro
    │   ├── Register.astro
    │   ├── Rubric.astro      # Sección opcional de rúbrica de evaluación (activable vía prop `show`)
    │   └── Schedule.astro
    ├── layouts/
    │   └── Layout.astro      # Layout base (HTML, metadatos, fuentes, estilos globales)
    ├── pages/
    │   └── index.astro       # Página única que compone todas las secciones
    └── styles/
        └── global.css        # Variables CSS de shadcn/ui + tema claro/oscuro + utilidades Tailwind
```

## 4. Comandos de desarrollo, build y preview

> Requiere tener `pnpm` instalado.

```bash
# Instalar dependencias
pnpm install

# Servidor de desarrollo local (hot reload)
pnpm dev

# Build de producción (genera carpeta dist/)
pnpm build

# Servir el build de producción localmente
pnpm preview
```

### Detalles del build

- Astro genera un **sitio estático** (output por defecto `static`).
- El directorio de salida es `dist/`.
- Tailwind CSS se procesa a través del plugin de Vite `@tailwindcss/vite`.

## 5. Guía de estilo y convenciones

### 5.1 Idioma

- El contenido visible de la UI está en **español**.
- Los nombres de archivos, componentes y variables en el código usan **inglés** (convención habitual de Astro/React).

### 5.2 Componentes Astro

- Cada sección de la página es un componente `.astro` en `src/components/`.
- El frontmatter (`---`) se usa para definir datos estáticos, como los arreglos `schedule` y `prizes`.
- Los estilos globales del layout están en `src/layouts/Layout.astro` dentro de una etiqueta `<style is:global>`.
- Los estilos locales de un componente se colocan en `<style>` dentro del mismo archivo (ej. `Schedule.astro`).

### 5.3 Tailwind CSS

- Se usa **Tailwind v4** con la sintaxis de `@import "tailwindcss"` en `src/styles/global.css`.
- Las variables de color de shadcn/ui están definidas en `:root` y `.dark` con valores `oklch`.
- El layout fuerza un tema oscuro con clases directas (`bg-black text-white`) en `<body>`, por lo que las variables de tema claro/oscuro de shadcn/ui no se alternan actualmente.
- Paleta de acentos visual:
  - Azul principal: `#0066ff`
  - Cyan secundario: `#00ccff`
  - Magenta: `#ff00ff`

### 5.4 Alias de importación

- El alias `@/*` apunta a `./src/*` (configurado en `tsconfig.json` y `components.json`).
- Actualmente las importaciones entre componentes usan rutas relativas (`../components/About.astro`), pero el alias está disponible para nuevo código.

### 5.5 JavaScript nativo

- La interactividad es mínima: `Schedule.astro` usa JavaScript nativo del lado del cliente para cambiar entre tabs.
- No hay componentes React, hooks ni estado en uso.

## 6. Instrucciones de testing

**El proyecto no cuenta con tests automatizados.** No hay configuración de Vitest, Jest, Playwright ni Cypress.

Para validar cambios:

1. Ejecutar `pnpm dev` y revisar visualmente en el navegador.
2. Ejecutar `pnpm build` para verificar que el build estático se genera sin errores.
3. Ejecutar `pnpm preview` para revisar el resultado de producción.

Si se agregan tests en el futuro, se recomienda documentar aquí los comandos y la estructura de archivos de tests.

## 7. Proceso de despliegue

El proyecto genera un sitio estático. El flujo de despliegue típico sería:

1. `pnpm install`
2. `pnpm build`
3. Desplegar el contenido de `dist/` en el servicio de hosting estático elegido.

Actualmente **no hay configuración de CI/CD** en el repositorio (no hay archivos `.github/workflows/`, `.gitlab-ci.yml`, etc.).

### Archivos estáticos importantes

- `public/_headers`: define cabeceras HTTP. Actualmente contiene directivas `Cross-Origin-Opener-Policy:` y `Cross-Origin-Embedder-Policy:` sin valores asignados. Si se despliega en Cloudflare Pages, este archivo tiene efecto; de lo contrario, puede ignorarse o ajustarse según el host.

## 8. Consideraciones de seguridad

- **Variables de entorno**: el proyecto no usa variables de entorno actualmente, pero `.env` y `.env.production` están ignorados en `.gitignore`.
- **Enlaces externos**: `Register.astro` enlaza a un Google Form (`https://forms.gle/...`) con `target="_blank" rel="noopener noreferrer"`, lo cual es correcto.
- **Cabeceras COOP/COEP**: `public/_headers` declara estas cabeceras sin valores. Si se habilitan con valores estrictos (`same-origin`, `require-corp`), se debe verificar que no rompan el uso de fuentes de Google Fonts o el iframe de Google Forms.
- **Dependencias de autenticación**: `@react-oauth/google` y `jwt-decode` están instalados pero no se usan. Si se activan, se debe configurar adecuadamente el flujo OAuth y manejar tokens del lado del cliente con cuidado.

## 9. Notas para agentes de código

- Este es un proyecto pequeño y enfocado: **landing page estática de una sola página**. Evita agregar complejidad innecesaria (rutas, estado global, backend, etc.) salvo que el usuario lo solicite explícitamente.
- Antes de instalar nuevos componentes de shadcn/ui, verifica que el CLI de shadcn/ui sea compatible con Tailwind v4 y la configuración actual.
- Si se modifica la paleta de colores, actualízala tanto en `src/styles/global.css` (variables) como en `src/layouts/Layout.astro` (clases y estilos globales).
- Los datos de horarios y premios viven directamente en los componentes Astro (`Schedule.astro`, `Prizes.astro`). Para cambios de contenido del evento, esos son los archivos principales.
- `Rubric.astro` y `Register.astro` son secciones opcionales controladas por la prop booleana `show` (por defecto `false` y `true` respectivamente). En `src/pages/index.astro` se definen las constantes `showRegister` y `showRubric` en el frontmatter y se pasan a `Navbar.astro`, `Hero.astro`, `Footer.astro` y a las propias secciones, manteniendo los enlaces y CTAs sincronizados con las secciones visibles.
- No hay linter ni formatter configurado. Si se agrega Prettier, ESLint o Biome, considérelo una mejora de infraestructura y documente los comandos en esta sección.
