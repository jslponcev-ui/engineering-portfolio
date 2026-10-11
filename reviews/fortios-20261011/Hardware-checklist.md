# Validación pendiente con un FortiGate

No ejecutada. Registrar modelo, versión de FortiOS, SHA de Core, versión publicada de aiofortiosapi, fecha y resultado de cada paso. Usar un administrador REST de solo lectura en root, restringido a la dirección de Home Assistant. No incluir tokens ni datos privados de clientes en la evidencia.

1. Configurar desde la interfaz un FortiGate con FortiOS >= 6.4.3. Verificar host/puerto, serial y SSL con certificado válido; si se usa certificado autofirmado, comprobar la opción explícita de desactivar la verificación.
2. Habilitar un tracker de prueba. Confirmar descubrimiento cuando aparece online, identidad serial+master MAC, actualización compartida cada 12 segundos y transición a not_home tras el consider_home configurado. Confirmar que clientes solamente offline no crean trackers.
3. Interrumpir y restaurar la conexión. Confirmar unavailable en los trackers existentes y recuperación tras un escaneo correcto. Durante un fallo de arranque de transporte, confirmar reintento automático.
4. Invalidar el token y completar la reautenticación con un token del mismo FortiGate. Confirmar rechazo de un token correspondiente a otro serial. No registrar el token.
5. Probar que un host que resuelve a un FortiGate diferente detiene el arranque antes de consultar clientes. Restaurar el destino original y recargar. No modificar el serial de la entrada para sortear el error.
6. En un entorno de pruebas autorizado, comprobar firmware incompatible: mensaje específico >= 6.4.3, sin polling; después de una actualización soportada, recargar y confirmar recuperación. No hacer downgrade en producción para esta prueba.
7. Migrar YAML de prueba conservando SSL y consider_home. Forzar primero un fallo reversible de conexión/importación: conservar YAML, corregir y reiniciar; confirmar entrada cargada y desaparición del repair de fallo. Solo entonces retirar el bloque FortiOS. Limpiar únicamente las entradas tracked identificadas en known_devices.yaml, reiniciar, habilitar entidades y revisar referencias.

Antes de adoptar el adaptador tipado, registrar evidencia de que master_mac y mac pueden diferir sin cambiar los IDs, que la consulta conserva el filtro original, y que un is_online ausente no activa un cliente mediante last_seen. La equivalencia entre ambos campos no se presupone.
