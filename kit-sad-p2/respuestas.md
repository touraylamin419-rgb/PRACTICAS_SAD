# Respuestas · Práctica 2 · Bodegas Atlántico

> Rellena este fichero en tu ordenador. Va en la entrega (`./lab.sh entregar`) y en la defensa te pueden preguntar por él.

## Bloque A · Piensa como un atacante (nmap)

Desde la calle: `./lab.sh cliente` y dentro `nmap -Pn -p 22,80,443,5432,8080 198.51.100.254`. Pega la salida:

```text
Starting Nmap 7.95 ( https://nmap.org ) at 2026-11-23 15:48 UTC
Nmap scan report for sad-p2-fw.sad-p2_calle (198.51.100.254)
Host is up (0.000070s latency).

PORT     STATE    SERVICE
22/tcp   filtered ssh
80/tcp   open     http
443/tcp  open     https
5432/tcp filtered postgresql
8080/tcp filtered http-proxy
MAC Address: E6:D2:BE:EF:54:3C (Unknown)

Nmap done: 1 IP address (1 host up) scanned in 1.71 seconds
```

## Bloque A · El contador

Después de `./lab.sh comprobar`, mira `./lab.sh ver`. ¿Cuántos paquetes lleva el contador de A6? ¿Quién los ha mandado?

>

## Bloque B · La prueba del cifrado

En el firewall (`./lab.sh fw`): `tcpdump -ni wan host 198.51.100.62 -c 6`. Mientras, en otra terminal, el comercial
(`./lab.sh comercial`) hace `curl http://172.16.10.20:8080`. Pega lo que sale en el firewall:

```text
listening on wan, link-type EN10MB (Ethernet), snapshot length 262144 bytes
16:21:47.962932 IP 198.51.100.62.43994 > 198.51.100.254.51820: UDP, length 148
16:21:47.964003 IP 198.51.100.254.51820 > 198.51.100.62.43994: UDP, length 92
16:21:47.964123 IP 198.51.100.62.43994 > 198.51.100.254.51820: UDP, length 128
16:21:47.964216 IP 198.51.100.254.51820 > 198.51.100.62.43994: UDP, length 128
16:21:48.303267 IP 198.51.100.62.43994 > 198.51.100.254.51820: UDP, length 96
16:21:48.303400 IP 198.51.100.254.51820 > 198.51.100.62.43994: UDP, length 96
```

En dos líneas: ¿qué se ve por la calle y qué NO se ve?

>

## Bloque C · Para la defensa (C4)

Del **mismo ataque** (la inyección a la web): ¿qué ve el vigilante de la red (el IDS) y qué ve el portero de la web? ¿Por
qué hacen falta los dos?

> Suricata (el IDS) vigila el tráfico de red a nivel de paquetes (capas inferiores), detectando firmas o patrones maliciosos en las conexiones. Por su parte, el WAF actúa a nivel de aplicación (capa 7 HTTP), leyendo la petición web en detalle y viendo los parámetros exactos del ataque (como la inyección SQL). Se necesitan ambos porque el IDS protege de forma global la infraestructura ante múltiples tipos de tráfico y escaneos de red, mientras que el WAF proporciona una defensa especializada y profunda contra ataques específicos a aplicaciones web.

## Bloque D · La nota del servidor (D0 y D4)

Nota del análisis **antes** de bastionar (D0):

> 45 / 100

Nota **después** (D4). ¿Por qué no llega a 100? ¿Significa que el servidor sigue inseguro? Comenta un aviso que siga saliendo.

> La nueva nota es de 62 / 100. No llega a 100 porque Lynis evalúa directivas de seguridad estrictas que no son obligatorias para el funcionamiento básico. Esto no significa que sea inseguro. Un aviso que sigue apareciendo es el HTTP-6710 sobre HTTPS.
