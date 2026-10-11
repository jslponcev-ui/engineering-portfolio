# FortiOS: entrega para revisión humana — 2026-10-11

Se completaron los cambios independientes de Core, la propuesta mínima de biblioteca y la documentación. Los tres commits exactos están publicados en ramas separadas de forks de jslponcev-ui para revisión humana, conservando su historial. No se han enviado cambios nuevos upstream, abierto PRs/issues ni publicado comentarios. Core #185565 y su rama actual permanecen sin modificaciones; sigue en borrador. Este trabajo no está aprobado, fusionado ni validado físicamente.

## Cambios y commits

| Repositorio | Rama publicada en el fork | Commit | Padre/base |
|---|---|---|---|
| Core | work/fortios-firmware-recovery-20261011 | 1299fb759270388a4c785d5e136c2ae444bbcfd3 | 957f0aa283db4a245f3780869945af1a02f19df0 |
| aiofortiosapi | work/master-mac-devices-20261011 | 707f2495f2609c7c1a8d5213142fbcefda45993c | e0f5673b8c596972789bbb9eef654b60e6f21aab |
| home-assistant.io | work/fortios-migration-docs-20261011 | bbcb53e6d1b72d761cca1dbb68db039e675ba75d | next 811af7b806b0913702ca0f3b30c50c3ba53fb01f |

**Core:** firmware accesible pero incompatible produce ConfigEntryError con la traducción específica “FortiOS 6.4.3 or newer is required”. Los fallos de transporte conservan ConfigEntryNotReady y su reintento. El repair de importación fallida explica corregir firmware/configuración y conservar YAML hasta confirmar éxito; un reintento correcto elimina el repair obsoleto por host. Dos regresiones verifican traducciones, ausencia de polling/entidades tras rechazo y recuperación real de setup/importación con mocks.

Se preservaron los cambios publicados de serial+MAC, sondeo fijo de 12 segundos, serial durante arranque, repairs separados por host y anotación de unique_id. El comentario de intervalo ya no aplica al código actual; no se rehizo. Los cambios locales históricos y los repositorios anteriores se preservaron; no se incorporaron archivos temporales.

**Biblioteca:** nueva API tipada get_master_mac_devices() conserva exactamente el filtro master_mac|hostname|is_online. Valida listas/envelopes, mantiene master_mac en mayúsculas y no sustituye por mac ni usa last_seen si falta is_online. get_system_status() valida envelopes y errores HTTP embebidos 401/403. La sesión sigue siendo propiedad del caller. README y changelog explican las diferencias; no se cambió versión.

La consulta a PyPI solo encontró 0.1.0 publicada; el tag v0.1.0 apunta a c37671f76834c2f45ffde4f4802854074c1b6dba. El main inspeccionado e0f5673 no contiene la API master-MAC necesaria. get_detected_devices() usa mac y fallback temporal: no se presupone equivalencia. Core sigue fijando **aiofortiosapi==0.1.0**. El build local con ese número prueba el empaquetado del código propuesto, no constituye un release ni se entrega como distribución publicada.

**Adaptador posterior:** Core-adaptador-tipado-pendiente-release.patch elimina consulta/validación/parseo raw de Core y usa get_system_status()/get_master_mac_devices(). Está separado del commit publicable y no cambia manifest ni requisitos. Pasa pruebas contra la fuente inédita de la biblioteca. Aplicarlo únicamente después de revisar una versión realmente publicada, actualizar el pin y regenerar requisitos.

**Documentación:** fortios.markdown sobre next cubre configuración UI, administrador REST de solo lectura/trusted hosts, mínimo 6.4.3, SSL, polling compartido fijo, importación YAML, limpieza selectiva de known_devices.yaml, habilitación de entidades, serial y recuperación de errores. Se reutilizó el material preparado y se cotejó con Core y documentación oficial de Fortinet. No se inventaron modelos físicamente probados, workflows, reconfigure del host ni funciones de automatización. Documentacion-evidencia.md contiene el mapa de afirmaciones y la revisión exigida por el skill del repositorio.

## Validación nueva

Los hashes en core-validated-blobs.txt/library-validated-blobs.txt se compararon con los blobs de los commits locales: **el código entregado coincide con el código comprobado**, incluidos los ajustes de Ruff. verified-commits.txt registra la comparación y ancestry.

| Comprobación | Resultado | Evidencia |
|---|---|---|
| Setup oficial Core | Aprobado, Ubuntu 26.04 / Python 3.14.5, dependencias completas | core-setup.log |
| FortiOS, Core 1299fb7 | 59 tests; 100% statements/branches, 270 statements y 44 branches | core-coverage.log |
| Suites relacionadas, mismo código | 837 tests y 4 snapshots | core-related-tests.log |
| Mypy, Ruff, format, Pylint de integración/tests | Aprobados con comandos normales | core-mypy/ruff/format/pylint.log |
| Hassfest con requirements, gen_requirements_all validate, extracción y auditoría de licencias | Aprobados | core-hassfest/requirements/license-extract/licenses.log |
| Hooks oficiales sobre los 4 archivos cambiados | Aprobados; hooks sin archivos pertinentes muestran Skipped | core-file-hooks.log |
| prek --all-files | **Incompleto**: WSL dejó de responder y el host llegó a unos 30 MB libres. Se liberaron solo cachés temporales y se interrumpió el proceso propio. No se afirma OOM ni fallo de código | core-prek.log y Infrastructure-status.md |
| Hook global Mypy, configuración normal | **Falló**, exit 2: event_type.py y event_type.pyi se detectan como módulo duplicado; fuera de los archivos modificados. No se alteró el check | core-global-mypy.log |
| Biblioteca 707f249 | 52 tests; 201 statements cubiertos, cobertura combinada 99%, una rama previa no cubierta | aiofortiosapi-tests.log |
| Biblioteca: Mypy/Ruff/formato/build/Twine | Aprobados; solo Python 3.14.5 local, no matriz 3.12/3.13 | aiofortiosapi-style.log y aiofortiosapi-build.log |
| Adaptador tipado + fuente propuesta 707f249 | 59 tests; 100%, 250 statements/32 branches; Mypy/Ruff/formato aprobados | typed-adapter-preview-tests.log y typed-adapter-checks.txt |
| Página docs bbcb53e | Remark/Textlint/diff-check/metadatos y options YAML aprobados; revisión final sin problemas materiales | docs-checks.txt, docs-metadata.log, Documentacion-evidencia.md |
| Build completo del sitio y CI nuevo | No ejecutados / pendientes de publicación autorizada | No se atribuyen resultados |
| FortiGate físico | No ejecutado | Hardware-checklist.md |

Las primeras ejecuciones del adaptador fallaron en teardown porque el checkout experimental no tenía traducciones generales de homeassistant/device_tracker. Se generaron con el comando oficial develop --all y la suite completa pasó. Se conserva el primer diagnóstico; no se cambiaron assertions, checks ni código para ocultarlo. Los 4 warnings de biblioteca son de aresponses/asyncio.iscoroutinefunction.

## Remoto y CI

- [Core PR #185565](https://github.com/home-assistant/core/pull/185565): rama remota work/fortios-config-flow-20261009, head **957f0aa283db4a245f3780869945af1a02f19df0**, open/draft, mergeable=true y mergeable_state=blocked, reconsultado al cierre.
- [CI 38010176871](https://github.com/home-assistant/core/actions/runs/38010176871): completed/success, head_sha **exactamente 957f0aa283db4a245f3780869945af1a02f19df0**.
- Ese CI no valida 1299fb7 ni el adaptador. Las ramas nuevas de Core, biblioteca y documentación están publicadas en los forks solo para revisión; no se abrieron nuevos PRs. La publicación de estas ramas no acredita comprensión humana ni autoriza envío upstream. published-commits.json registra la comprobación de SHAs remotos.

## Pendientes concretos

| Pendiente | Responsable |
|---|---|
| Revisar y comprender patches/textos antes de cualquier envío upstream; confirmar personalmente la casilla de comprensión | Contribuidor humano |
| Revisar/fusionar/publicar API master-MAC y validación de status en aiofortiosapi | Kim Frellsen, mantenedor de biblioteca |
| Verificar artefacto publicado, fijar versión real, regenerar requisitos, aplicar adaptador, tests y CI del SHA final | Trabajo posterior de Core tras el release |
| Resolver la invocación global de Mypy y completar prek global en un entorno estable, sin alterar checks para verde | Entorno/proyecto Core; fallo fuera del alcance FortiOS |
| Ejecutar y documentar pruebas físicas | Colaborador con FortiGate autorizado |
| Revisar y fusionar Core y docs | Mantenedores de Home Assistant |

## Material de revisión y envío

Los commits y comparaciones enlazados en README.md permiten revisar el código con su historial exacto. Se incluyen los diffs de Core y biblioteca, el adaptador experimental separado, la página de documentación, los tres textos de PR preparados, el checklist físico y logs seleccionados de validación. No se incluyen bundles, entornos virtuales, archivos temporales ni logs de instalación. Los textos de PR son borradores; no se marcaron casillas de comprensión humana.

La autorización de esta etapa permite únicamente publicar ramas en forks y material de revisión. El envío upstream sigue sin autorización y requiere revisión y comprensión humana; la semántica master_mac, la recuperación de setup/YAML y la dependencia de un release real deben comprenderse antes de ese paso.