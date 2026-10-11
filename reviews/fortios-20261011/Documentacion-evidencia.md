# Evidencia de la documentación

La página entregada es `fortios.markdown`, commit bbcb53e6d1b72d761cca1dbb68db039e675ba75d sobre `next` 811af7b806b0913702ca0f3b30c50c3ba53fb01f. Los archivos y tests citados pueden leerse en el [commit de Core publicado para revisión](https://github.com/jslponcev-ui/core/commit/1299fb759270388a4c785d5e136c2ae444bbcfd3).

| Afirmación | Evidencia |
|---|---|
| Configuración UI, etiquetas Host/API token/Verify SSL certificate y SSL activado por defecto | config_flow.py y strings.json; etiquetas comprobadas al parsear configuration_basic |
| HTTPS, puerto 443, puerto personalizado e IPv6 | client.py: construcción y validación de URL; pruebas test_client.py |
| FortiOS >= 6.4.3 | const.py MINIMUM_VERSION; client.py connect; traducciones y nuevas regresiones de setup/importación |
| root y solo lecturas de status/clientes | client.py: vdom=root y dos consultas GET; no acciones de escritura |
| Token mostrado una vez y trusted hosts limitados a Home Assistant | [Fortinet REST API administrator](https://docs.fortinet.com/document/fortigate/7.2.5/administration-guide/399023/rest-api-administrator) |
| Perfil read-only para monitorización | [Fortinet Using APIs](https://docs.fortinet.com/document/fortigate/7.6.6/administration-guide/940602/using-apis); los datos requeridos se deducen de las consultas concretas de Core |
| Una consulta compartida cada 12 segundos | const.py y coordinator.py; regresión de intervalo importado en test_device_tracker.py |
| YAML conserva host/token/SSL/consider_home, ignora interval_seconds | config_flow.py async_step_import; device_tracker.py async_setup_scanner; pruebas de importación |
| Mantener YAML tras fallo y retirar solo después del éxito | strings.json yaml_import_failed; nuevas pruebas de traducción y recuperación; éxito borra issue por host |
| Limpieza selectiva de entradas tracked en known_devices.yaml | device_tracker.py async_check_legacy_devices y repair known_devices; sin borrar el archivo completo |
| Trackers aparecen al estar online; descubrimiento continúa con lista inicial vacía | device_tracker.py async_add_new_devices y pruebas de descubrimiento |
| Nuevos trackers deshabilitados salvo dispositivos previamente registrados | device_tracker.py entity_registry_enabled_default; pruebas de registro |
| Consider-home por defecto de 3 minutos y transición en la siguiente actualización | configuración y device_tracker.py is_connected; pruebas con tiempo controlado |
| Fallos de scans provocan unavailable y posterior recuperación | coordinator.py y CoordinatorEntity; test_device_tracker.py |
| Firmware incompatible requiere corregir y recargar; transporte reintenta setup | __init__.py: ConfigEntryError y ConfigEntryNotReady separados; test_init.py |
| Serial en setup y reauth; no reconfigure de host | __init__.py y config_flow.py; rechazo y recuperación cubiertos por pruebas |

La revisión exigida por `.claude/skills/document-integration-docs/SKILL.md` incluyó inventario de Core, extracción de plantilla y revisión final independiente. La revisión final no encontró problemas materiales. No hay triggers, conditions ni actions propios implementados: no se crearon páginas divididas ni ejemplos de automatización inventados. Esta excepción a la sección de ejemplos se basa en el alcance real de Core y en la guía de escritura que prohíbe inventar workflows.

Comprobaciones sobre la página final: Remark, Textlint, diff --check, parseo de front matter/configuration_basic y comparación con manifest.json aprobados. Node 24.19.0; dependencias del lockfile instaladas con npm ci --ignore-scripts. No se ejecutó el build completo del sitio ni CI remoto de documentación. Los avisos del instalador sobre dependencias de desarrollo no provocaron cambios en el lockfile ni npm audit fix.
