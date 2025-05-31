# Info
## Imagen Topologia
![1000](https://slink.proxylivy.work/image/9373265c-36d3-43e8-b5dd-1ec325c87fd0.png)
## Sobre
Resolviendo Problemas de Capa 2 en una Red
## Requerimientos
### Ticket 1: Conectividad en Capa 2
- Corregir todos los inconvenientes detectados a nivel capa 2.
- Debe circular VLAN Nativa con configuración apropiada para esto, en caso de no, corregir.
- Cliente menciona que sus port -channels por requisito, al ser corregido s un extremo debe negociar y otro escuchar.
- Cliente menciona que utiliza MSTP, utilizando como nombre de región TSHOOT y revisión 1. Le ha solicitado corregir el protoco lo en cuestión, mejorando sus temporizadores, dejándolo reducidos a la mitad.
- Todas las interfaces conectadas a equipos finales deben tener seguridad por aprendizaje con un máximo de 2 direcciones MAC, con deshabilitación de interfaz en caso de exceder límite permitido.
- Todas las interfaces a equipos finales deben no recibir ni enviar mensa jes de STP y pasar a FWD de manera automática.
- Documentar todos los errores detectados.

### Ticket 2: Conectividad en Capa 3
- Corregir todos los inconvenientes de EIGRP Nombrado.
- Permitir salida hacia rutas externas.
- Implemente protocolo de alta disponibilidad propietario, donde todas las VLAN utilicen por preferencia R3. Debe monitorear el enlace ascendente, en caso de caída.
- Por proceso de migración la empresa necesita probar por 2 semanas un túnel 6 over 4 entre dos routers para verificar el funcionamiento d e la red, por lo cual se ha solicitado que implemente dicho túnel y documente el funcionamiento del mismo.
- Documentar todos los errores detectados.

### Ticket 3: Conectividad por DHCP y NAT para Equipos Finales
- Usuarios de la VLAN10 reclaman que no tienen acc eso a ninguna red. Se sabe que el POOL está configurado en R2, deberá solucionar el inconveniente que permita otorgar direccionamiento a dicha red.
- Permitir que todas las redes de datos, salgan traducidas por la única IP pública recibe la empresa. Comprobar funcionamiento y corregir en caso de ser necesario.
- Comprobar conectividad de equipos finales a ISP a nivel IPv4.
- Documentar todos los errores de tectados.

# Configuracion
## Equipo: 

- Enfoque Utilizado: 
- Problema Detectado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---

- Enfoque Utilizado: 
- Problema Encontrado (Descripcion Breve): 

| Comando para encontrar el problema | Comando Aplicado |
| ---------------------------------- | ---------------- |
|                                    |                  |

---
