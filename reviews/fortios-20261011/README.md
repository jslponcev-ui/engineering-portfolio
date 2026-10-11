# FortiOS — revisión previa al envío upstream

Este directorio reúne trabajo preparado para revisión humana. **FortiOS no está aprobado ni fusionado y no forma parte del índice de contribuciones aceptadas.** La autorización de esta etapa permite subir ramas a forks y publicar este material de revisión; no acredita comprensión del código ni autoriza PRs, issues o comentarios upstream.

## Commits publicados en forks de jslponcev-ui

| Repositorio y rama | SHA completo | Comparación con el padre |
|---|---|---|
| [Core: work/fortios-firmware-recovery-20261011](https://github.com/jslponcev-ui/core/tree/work/fortios-firmware-recovery-20261011) | [1299fb759270388a4c785d5e136c2ae444bbcfd3](https://github.com/jslponcev-ui/core/commit/1299fb759270388a4c785d5e136c2ae444bbcfd3) | [957f0aa… → 1299fb7…](https://github.com/jslponcev-ui/core/compare/957f0aa283db4a245f3780869945af1a02f19df0...1299fb759270388a4c785d5e136c2ae444bbcfd3) |
| [aiofortiosapi: work/master-mac-devices-20261011](https://github.com/jslponcev-ui/aiofortiosapi/tree/work/master-mac-devices-20261011) | [707f2495f2609c7c1a8d5213142fbcefda45993c](https://github.com/jslponcev-ui/aiofortiosapi/commit/707f2495f2609c7c1a8d5213142fbcefda45993c) | [e0f5673… → 707f249…](https://github.com/jslponcev-ui/aiofortiosapi/compare/e0f5673b8c596972789bbb9eef654b60e6f21aab...707f2495f2609c7c1a8d5213142fbcefda45993c) |
| [Documentación: work/fortios-migration-docs-20261011](https://github.com/jslponcev-ui/home-assistant.io/tree/work/fortios-migration-docs-20261011) | [bbcb53e6d1b72d761cca1dbb68db039e675ba75d](https://github.com/jslponcev-ui/home-assistant.io/commit/bbcb53e6d1b72d761cca1dbb68db039e675ba75d) | [811af7b… → bbcb53e…](https://github.com/jslponcev-ui/home-assistant.io/compare/811af7b806b0913702ca0f3b30c50c3ba53fb01f...bbcb53e6d1b72d761cca1dbb68db039e675ba75d) |

Cada rama coincide con el SHA local esperado. Las comparaciones confirman un commit añadido sobre la base, sin divergencia. Los forks nuevos de biblioteca y documentación pertenecen a jslponcev-ui y sus padres son kimfrellsen/aiofortiosapi y home-assistant/home-assistant.io.

## Material para revisar

- [Informe de cambios, pruebas y pendientes](Informe.md).
- Textos preparados, sin enviar: [Core](Core-PR-description.md), [biblioteca](aiofortiosapi-PR-description.md) y [documentación](Documentacion-PR-description.md). Se conservan las plantillas y no se marca comprensión humana.
- [Resultados de validación](Validation.md) y [logs seleccionados](validation/).
- [Checklist físico pendiente](Hardware-checklist.md).
- [Página de documentación](fortios.markdown) y [evidencia de sus afirmaciones](Documentacion-evidencia.md).
- [Estado de las limitaciones del entorno](Infrastructure-status.md) y [revisión de contenido sensible](Security-review.md).
- Diffs para lectura: [Core](Core-nuevos-cambios.patch), [biblioteca](aiofortiosapi.patch) y [adaptador posterior al release](Core-adaptador-tipado-pendiente-release.patch).

El adaptador está separado del commit de Core. La dependencia sigue siendo aiofortiosapi==0.1.0: el mantenedor debe revisar y publicar primero una versión real con la API tipada propuesta. No se subieron entornos, cachés, scripts temporales, bundles ni logs de instalación.

El [PR upstream #185565](https://github.com/home-assistant/core/pull/185565), su rama work/fortios-config-flow-20261009 y su descripción permanecen sin cambios; su head sigue siendo 957f0aa283db4a245f3780869945af1a02f19df0. No se abrió ningún PR nuevo. Esta revisión vive solo en la rama review/fortios-20261011 del portafolio; main y su índice permanecen intactos.

Las pruebas previamente aprobadas no se repitieron para esta publicación porque el código no cambió. El CI histórico valida 957f0aa y no se atribuye al nuevo commit. Siguen pendientes comprensión humana, release de biblioteca, integración de ese release con requisitos reales, CI del head final y pruebas físicas.
