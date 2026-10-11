# Resultados de validación

Resultados obtenidos antes de publicar. No se repitieron pruebas aprobadas en esta etapa: se comprobaron commits, contenido, ancestry y SHAs remotos. Los blobs de los archivos probados coinciden con los commits publicados, según core-validated-blobs.txt, library-validated-blobs.txt y verified-commits.txt.

| Alcance | SHA o estado probado | Resultado |
|---|---|---|
| Core FortiOS | 1299fb759270388a4c785d5e136c2ae444bbcfd3 | 59 tests; 100% de cobertura de statements/branches, 270 statements y 44 branches |
| Suites relacionadas de Core | Mismo código de 1299fb759270388a4c785d5e136c2ae444bbcfd3 | 837 tests y 4 snapshots; incluye FortiOS |
| Core Mypy/Ruff/formato/Pylint | Mismos archivos de Core e integración/tests | Aprobados |
| Hassfest, requirements y licencias | Entorno completo montado con script/setup, Python 3.14.5 / Ubuntu 26.04 | Aprobados; códigos de salida en check-exits.txt |
| Hooks oficiales sobre archivos modificados | Cuatro archivos cambiados de Core | Aprobados; los no aplicables figuran Skipped |
| Hook Mypy global | Configuración oficial sin modificar | Falló con exit 2 por duplicate module event_type.py/.pyi, fuera de FortiOS |
| prek global | Intento con --all-files | Incompleto por errores de WSL y falta de espacio; no se declara verde |
| Biblioteca | 707f2495f2609c7c1a8d5213142fbcefda45993c | 52 tests; 201 statements cubiertos, cobertura combinada 99%, una rama previa sin cubrir |
| Biblioteca Mypy/Ruff/formato/build/Twine | Mismo código propuesto | Aprobados; build local no es un release publicado |
| Adaptador tipado | Core preparado más patch separado, con fuente inédita de biblioteca 707f2495 | 59 tests; 100%, 250 statements/32 branches; Mypy/Ruff/formato aprobados |
| Documentación | bbcb53e6d1b72d761cca1dbb68db039e675ba75d | Remark, Textlint, diff-check, metadatos y options YAML aprobados; revisión final sin problemas materiales |
| FortiGate físico | No ejecutado | Checklist pendiente; mocks no equivalen a hardware |

La prueba del adaptador usa PYTHONPATH con fuente propuesta, no una distribución publicada de aiofortiosapi. Core conserva su pin 0.1.0 hasta que haya un release real. No se ejecutaron localmente la matriz Python 3.12/3.13 de biblioteca ni el build completo del sitio.

[CI histórico aprobado](https://github.com/home-assistant/core/actions/runs/38010176871): completed/success, SHA exacto 957f0aa283db4a245f3780869945af1a02f19df0. No valida las tres ramas nuevas ni el adaptador. Esta entrega no afirma que exista CI aprobado para ellas.

Los logs seleccionados se encuentran en validation/. Se quitaron rutas de entornos locales; los resultados y mensajes de los checks se conservaron. Los logs extensos de instalación y los temporales de diagnóstico no se publican.
