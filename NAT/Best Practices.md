IP pool:
- Revisa el inicio y final del pool
- Revisa que no se overlapee con ips de fortigate y hosts
- Si usuarios internos y externos tienen acceso, configurar el dns para que los internos resuelvan a la ip interna

- No configurar NAT para trafico interno, no tiene sentido. A menos que sea requerido

- Cambiar modos de nateo causa un corto