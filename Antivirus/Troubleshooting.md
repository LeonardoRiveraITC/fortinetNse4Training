## Updates
- Fortigate tiene conexión a internet
- Fortigate puede resolver el dns update.fortiguard.net
- TCP 443 esta abierto
- execute update-av

- Comparar las versiones de fortiguard.com/updates/antivirus con la versión local

```
diagnose debug application update -1
diagnose debug enable 
execute update-av
```

## Incapaz de atrapar virus con licencia valida

- Asegurarse de que se esta aplicando el AV en la politica de firewall
- Habilitar SSL inspection
- Revisar que el perfil de antivirus pasa por el servicio correcto
- Asegurarse que se pasa por el mismo perfil de antivirus para las conexiones redundantes

# Comandos útiles
- get system performance status - estado del sistema
- diagnose antivirus database-info - version de base de datos de av
- diagnose autoupdate versions - version de motor de av y version de firmas
- diagnose antivirus test "get scantime" - Scan times de archivos
- execute update-av - actualizar av