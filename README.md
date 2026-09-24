# cs2032-web-hackathon

> Proyecto del curso **CS2032 – Cloud Computing** · UTEC

Sitio web de **HACK//UTEC**, el hackathon de Cloud Computing que se organiza cada semestre. Se reutiliza en cada edición: primero como página de convocatoria y, al terminar el evento, como página de resultados.

🌐 **En producción:** https://hackathon.cs2032.com

## Secciones

| Fase | Componentes |
|---|---|
| Convocatoria | `About`, `Schedule`, `Rubric`, `Prizes`, `Register`, `Submit` |
| Resultados | `Podium` (top 3), `TeamsList` (equipos, integrantes, certificados y proyectos) |

La página que se muestra se arma en `src/pages/index.astro`.

## Stack

- Astro 5 (sitio estático) · TypeScript
- Tailwind CSS 4
- pnpm
- Python 3 + `openpyxl` para generar los resultados
- Despliegue estático en AWS S3 + CloudFront · certificados en OCI Object Storage

## Generar resultados

`scripts/generate-results.py` cruza las respuestas de los formularios de inscripción y entrega (CSV) con la hoja de certificados (`certificados.xlsx`) y genera `src/data/results.json`:

```bash
pip install openpyxl
python scripts/generate-results.py
```

> ⚠️ Los CSV y `results.json` contienen datos personales de los participantes (nombres, correos y códigos de alumno). Mantén este repositorio **privado**.

## Desarrollo

```bash
pnpm install
pnpm dev        # http://localhost:4321
pnpm build      # genera dist/
pnpm preview
```

## Nueva edición

1. Actualiza fechas, reto y requisitos en los componentes de convocatoria (`Schedule`, `Rubric`, `Prizes`, `public/Reto Hackathon Cloud.pdf`).
2. Al cerrar el evento, reemplaza los CSV y `certificados.xlsx`, ejecuta el script y cambia `index.astro` a la vista de resultados.
3. `pnpm build` y sube `dist/` al bucket.

Más detalles de arquitectura y convenciones en [`AGENTS.md`](AGENTS.md).

## Repositorios relacionados

- [cs2032-web-attendance](https://github.com/Maykol-Morales/cs2032-web-attendance) — web de asistencia para alumnos
- [cs2032-web-admin](https://github.com/Maykol-Morales/cs2032-web-admin) — panel del profesor (sesiones y QR)
