# Revisión previa a la publicación en forks

Se inspeccionaron los once archivos modificados por los tres commits, incluyendo código, pruebas y documentación. Se revisaron patrones de tokens de GitHub y cloud, claves privadas, credenciales en URLs, literales Bearer y asignaciones de secretos. No aparecieron candidatos a secretos reales en esos archivos.

Las pruebas usan valores sintéticos: test-token/test-token-abc123, direcciones y nombres de ejemplo, FGT123456 y MACs de fixtures. La nueva consulta usa variables de autorización; no incorpora un token real. Los textos explican cómo obtener credenciales pero no contienen credenciales de dispositivos.

La carpeta de revisión del portafolio contiene únicamente documentos, diffs y logs de validación seleccionados. Se eliminaron las rutas absolutas de entornos locales en los logs. No se incluyen configuración privada de FortiGate, capturas de tráfico real, inventarios de dispositivos reales, entornos virtuales, cachés ni logs de instalación. La revisión final de patrones se ejecuta también sobre los archivos de esta carpeta antes del commit.

Las credenciales Git se usaron solo mediante el helper existente y en memoria para crear los forks; no se imprimieron ni se escribieron en los archivos de revisión. El envío no cambia los SHAs preparados, no modifica el PR upstream ni confirma comprensión humana.
