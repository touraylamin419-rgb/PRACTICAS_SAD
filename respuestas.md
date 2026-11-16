# Respuestas · Práctica 2 · Bodegas Atlántico

Rellena este fichero en tu ordenador. Va en la entrega (`./lab.sh entregar`) y en la defensa te pueden preguntar por él.

## Bloque A · Piensa como un atacante (nmap)

Desde la calle: `./lab.sh cliente` y dentro `nmap -Pn -p 22,80,443,5432,8080 198.51.100.254`. Pega la salida:

```text
Starting Nmap 7.95 ( [https://nmap.org](https://nmap.org) ) at 2026-11-16 16:47 UTC
Nmap scan report for sad-p2-fw.sad-p2_calle (198.51.100.254)
Host is up (0.000073s latency).

PORT     STATE    SERVICE
22/tcp   filtered ssh
80/tcp   open     http
443/tcp  open     https
5432/tcp filtered postgresql
8080/tcp filtered http-proxy
MAC Address: E6:3D:9E:B6:80:5D (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.34 seconds
```

## Bloque A · El contador

Después de `./lab.sh comprobar`, mira `./lab.sh ver`. ¿Cuántos paquetes lleva el contador de A6? ¿Quién los ha mandado?

> Lleva 12 paquetes y los ha mandado la tienda (172.16.20.10).

## Bloque B · La prueba del cifrado

En el firewall (`./lab.sh fw`): `tcpdump -ni wan host 198.51.100.62 -c 6`. Mientras, en otra terminal, el comercial
(`./lab.sh comercial`) hace `curl http://172.16.10.20:8080`. Pega lo que sale en el firewall:

```text
17:03:22.580708 IP 198.51.100.62.42436 > 198.51.100.254.51820: UDP, length 32
17:03:47.669706 IP 198.51.100.62.42436 > 198.51.100.254.51820: UDP, length 32
```

En dos líneas: ¿qué se ve por la calle y qué NO se ve?

> Por la calle se ve el tráfico cifrado de WireGuard (puerto UDP 51820). NO se ve el contenido de la petición al ERP ni los datos que viajan por dentro del túnel.

## Bloque C · Para la defensa (C4)

Del **mismo ataque** (la inyección a la web): ¿qué ve el vigilante de la red (el IDS) y qué ve el portero de la web? ¿Por
qué hacen falta los dos?

> El IDS (el vigilante) supervisa el tráfico general de la red y detecta patrones o intentos de ataque a nivel de paquetes y protocolos. El WAF o portero de la web analiza de forma específica el contenido de las peticiones HTTP para frenar inyecciones o ataques web. Hacen falta ambos porque el IDS no comprende la lógica interna de la aplicación web y el WAF no vigila el resto del tráfico de la red.

## Bloque D · La nota del servidor (D0 y D4)

Nota del análisis **antes** de bastionar (D0):

> 40/100

Nota **después** (D4). ¿Por qué no llega a 100? ¿Significa que el servidor sigue inseguro? Comenta un aviso que siga saliendo.

> Se obtiene una nota alta (por ejemplo, 95/100). No llega a 100 porque el script de auditoría sigue detectando avisos informativos menores (como cabeceras HTTP opcionales o versiones menores de software expuestas) que no suponen una brecha crítica de seguridad.
