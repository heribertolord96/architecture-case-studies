---
title: "Resolución de dominios locales en LAN con dnsmasq"
date: 2026-11-15
tags: [networking, homelab, linux]
summary: "Guía paso a paso para apuntar dominios .lan a servicios locales usando dnsmasq y un router "
---


# Guía de Arquitectura: Dominios Intralan con NginxProxyManager(NPM) y Resolución DNS

## Opción 1: El método tradicional mediante el archivo `hosts`

*Ideal para pruebas rápidas, equipos sueltos o cuando se prescinde por completo de AdGuard y del router.*

* **Cómo funciona:** El sistema operativo del cliente intercepta la petición del dominio antes de consultar a cualquier servidor DNS externo y la redirige directamente a la IP local de tu servidor NPM.
* **Procedimiento:**
1. Ubica el archivo de configuración del sistema operativo:
* **Windows:** `C:\Windows\System32\drivers\etc\hosts`
* **Linux / macOS:** `/etc/hosts`


2. Abre el archivo con privilegios de administrador y añade una línea por cada servicio apuntando a la IP fija del servidor (`192.158.60.100`):
```text
192.158.60.100 casa.home.lab
192.158.60.100 emby.home.lab

```

|Ventajas|Limitaciones|
|--|--|
|Cero infraestructura de red requerida; |**No soporta comodines (`*`)**: Tienes que registrar cada subdominio manualmente línea por línea.|
|no dependes de contenedores ni de la configuración del router.|   **Incompatible con móviles:** En Android (sin root) es inaccesible, por lo que los teléfonos se quedan fuera de este método.|



---

## Opción 2: El método definitivo (NPM + AdGuard + Router DHCP Selectivo + VPN)

*La arquitectura robusta de homelab: comodines automáticos (`*.home.lab`), compatibilidad con PC y Android, sin romper el internet de la casa si el servidor se apaga.*

### Paso 1: Configuración en Nginx Proxy Manager (NPM)

1. Entra al panel de NPM y da de alta tus servicios mediante **Proxy Hosts**.
2. Configura el dominio deseado (ej. `casa.home.lab`) y apúntalo hacia la IP interna y el puerto del servicio correspondiente (ej. `[http://192.158.60.100:82](http://192.158.60.100:82)`).



### Paso 2: Reescritura Wildcard en AdGuard Home

1. Dirígete al panel de AdGuard Home en **Filtros > Reescrituras DNS** (*DNS Rewrites*).


2. Añade una regla global de comodín para que cualquier subdominio de tu entorno local apunte a la IP de tu servidor:
* **Dominio:** `*.home.lab`
* **Dirección IP (Answer):** `192.158.60.100`





### Paso 3: Asignación Quirúrgica en el Router (Evitando el SPOF)

*Para evitar que la caída de tu servidor homelab deje sin internet a toda la casa, **no** cambies el DNS global del router. Usa el filtrado por dirección MAC:*

1. Accede al panel de tu router y navega a **LAN > DHCP Server**.
2. En la sección **Manual Assignment**, activa la opción en **Yes**.


3. Registra las direcciones MAC de tus dispositivos personales (por ejemplo, tu laptop `pc-client` o tu celular):


* Asignales su IP estática local correspondiente.


* En la columna **`DNS Server (Optional)`**, escribe explícitamente la IP de tu servidor AdGuard (`192.158.60.100`).




4. Deja el resto de la red de la casa con el DNS predeterminado (*Default*). Así, si apagas tu servidor para desarrollo, el internet general de la familia sigue fluyendo sin interrupciones.

### Paso 4: Adaptación para Clientes Externos vía VPN (WireGuard)

*Dado que los túneles VPN aislados no consultan el DHCP tradicional de la LAN para asignar DNS personalizados:*

1. Exporta el archivo de configuración del cliente (`.conf`) desde el servidor WireGuard nativo de tu router 
2. Abre el archivo `.conf` en un editor de texto y añade la línea del servidor DNS local dentro de la sección `[Interface]`:
```ini
[Interface]
PrivateKey = <tu-clave-privada>
Address = 10.6.0.X/32
DNS = 192.158.60.100

```


3. Guarda los cambios en el archivo del cliente. Al activar la VPN desde la calle, el tráfico de nombres viajará seguro por el túnel y resolverá `*.home.lab` directamente contra AdGuard.
4 Intenta abrir en el navegador cualquier nuevo dominio creado con NPM , deberia funcionar.
