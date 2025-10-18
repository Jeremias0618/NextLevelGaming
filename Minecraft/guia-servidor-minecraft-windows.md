# Guía: Crear un **Servidor Minecraft Java** en Windows

![Minecraft Banner](/IMG/minecraft-background-cfljc4haleghnajo.jpg)

> Esta guía en **Markdown** te explica paso a paso cómo montar un servidor Minecraft Java en Windows, cómo iniciarlo, archivos importantes y todos los comandos y opciones para **whitelist**, **IP permitida (firewall)**, **ban**, **kick**, **ops**, y medidas básicas de seguridad. Incluye ejemplos de `.bat` / PowerShell.

---

## Índice

1. Requisitos previos
2. Descargar Java y el servidor
3. Preparar la carpeta del servidor
4. Primer arranque (EULA)
5. Archivos importantes y `server.properties`
6. Scripts de inicio (Windows `.bat` y PowerShell)
7. Abrir puerto en Windows Firewall y permitir IPs específicas
8. Comandos de consola / juego (vanilla)
9. Gestión de whitelist
10. Ban / Kick / IP Ban
11. Operators (ops) y permisos
12. Plugins útiles (opcional)
13. Copias de seguridad y mantenimiento
14. Ejemplos prácticos y atajos útiles

---

## 1) Requisitos previos

* Windows 10/11 o Windows Server (64-bit recomendable).
* Java 17+ (o la versión recomendada por la versión de servidor que uses). Desde 1.18+ se recomienda Java 17; versiones recientes pueden requerir Java 17/20/21 según la build.
* Conexión a Internet y control del router (si quieres que sea accesible desde fuera de tu red local).
* Puerto por defecto: `25565` (TCP).

## 2) Descargar Java y el servidor

1. Instala Java (OpenJDK o Oracle JDK). Ejemplo: OpenJDK 17.
2. Descarga el `server.jar` oficial de minecraft.net (o usa Paper/Spigot para mejores prestaciones si quieres plugins).

Guarda el `.jar` dentro de una carpeta dedicada, por ejemplo `C:\mc-server\`.

## 3) Preparar la carpeta del servidor

Crea `C:\mc-server\` y coloca `server.jar` allí. Desde PowerShell (ejemplo):

```powershell
mkdir C:\mc-server
cd C:\mc-server
# mover/pegar el server.jar aquí
```

## 4) Primer arranque (aceptar EULA)

Ejecuta desde la carpeta:

```powershell
java -Xmx1G -Xms1G -jar server.jar nogui
```

Al primer arranque saldrá un archivo `eula.txt`. Ábrelo y cambia `eula=false` a `eula=true` para aceptar el EULA.

```text
# eula.txt
# Debes aceptar cambiando a true
eula=true
```

Vuelve a ejecutar el comando de Java para que genere los mundos y archivos.

## 5) Archivos importantes

* `server.properties` — configuración del servidor (puerto, modo de juego, dificultad, whitelist, etc.).
* `ops.json` — lista de operadores (ops).
* `whitelist.json` — lista de jugadores permitidos cuando `white-list=true`.
* `banned-players.json` — jugadores baneados por nombre/UUID.
* `banned-ips.json` — IPs baneadas.
* `logs` — logs del servidor.

Algunos ajustes útiles en `server.properties`:

```
server-port=25565
online-mode=true
white-list=false   # true = solo jugadores en whitelist pueden entrar
motd=Servidor de Ejemplo
max-players=20
view-distance=10
```

## 6) Scripts de inicio (Windows)

**Ejemplo `start-server.bat`** (doble clic para iniciar):

```bat
@echo off
cd /d %~dp0
REM Ajusta la memoria según tu equipo
java -Xms1G -Xmx4G -jar server.jar nogui
pause
```

**Ejemplo PowerShell (`start-server.ps1`)** (ejecutar con PowerShell):

```powershell
Set-Location -Path $PSScriptRoot
# Ajusta Xms/Xmx según RAM disponible
java -Xms2G -Xmx6G -jar .\server.jar nogui
```

> Nota: Si usas más memoria, asegúrate de que la máquina tenga RAM disponible.

## 7) Abrir puerto en Windows Firewall y permitir IPs específicas

### Abrir puerto 25565 (permitir a todos)

Ejecuta en PowerShell como administrador o CMD:

```powershell
netsh advfirewall firewall add rule name="Minecraft TCP" dir=in action=allow protocol=TCP localport=25565
```

### Permitir solo IP(s) específicas (ejemplo: permitir 1.2.3.4)

Si quieres permitir solo conexiones desde IPs concretas (y bloquear el resto), primero añade regla que permita sólo esas IPs y luego asegúrate de no tener otra regla general que permita 25565 a todo el mundo.

```powershell
# Permitir solo desde 1.2.3.4
netsh advfirewall firewall add rule name="Minecraft From 1.2.3.4" dir=in action=allow protocol=TCP localport=25565 remoteip=1.2.3.4

# Permitir desde varias IPs
netsh advfirewall firewall add rule name="Minecraft From Office" dir=in action=allow protocol=TCP localport=25565 remoteip=1.2.3.4,5.6.7.8
```

Si tienes una regla previa que permite todo, elimínala o desactívala:

```powershell
netsh advfirewall firewall delete rule name="Minecraft TCP"
```

> **Importante:** Este método filtra a nivel de Windows; si quieres filtrado por usuario/UUID usa whitelist en Minecraft o un plugin.

## 8) Comandos de consola / juego (vanilla)

Los comandos pueden ejecutarse desde la consola del servidor (la ventana donde corre `java`) o como operador en el juego (prefijo `/`). A continuación los comandos más útiles y su sintaxis.

### Comandos generales del servidor

* `stop` — Apaga el servidor de manera segura.
* `save-all` — Guarda el mundo.
* `save-off` / `save-on` — Desactivar/activar guardado automático.
* `say <mensaje>` — Envía un mensaje a todos los jugadores desde la consola.

### Gestión de jugadores (kick/ban/pardon)

* `kick <player> [reason]` — Expulsa a un jugador inmediatamente.

  * Ejemplo: `kick Yeremi Violación de reglas`.

* `ban <player> [reason]` — Prohíbe el jugador por nombre/UUID (añade a `banned-players.json`).

  * Ejemplo: `ban Troll123 Abuso de chat`.

* `pardon <player>` — Quita el baneo por nombre/UUID.

* `ban-ip <IP> [reason]` — Banea una dirección IP (añade a `banned-ips.json`).

  * Ejemplo: `ban-ip 203.0.113.42 DDoS`.

* `pardon-ip <IP>` — Quita baneo de IP.

* `list` — Muestra jugadores conectados.

* `whitelist on|off` — Activa/desactiva whitelist.

* `whitelist add <player>` — Agrega jugador a whitelist.

* `whitelist remove <player>` — Quita jugador de whitelist.

* `whitelist list` — Lista jugadores en whitelist.

* `whitelist reload` — Recarga la whitelist desde archivo.

### Operators y permisos

* `op <player>` — Da permisos de operador a un jugador (lo añade a `ops.json`).
* `deop <player>` — Quita permisos de operador.

### Otros comandos útiles

* `gamemode <mode> [player]` — Cambia modo de juego (survival, creative, adventure, spectator).
* `tp <player> <target>` — Teletransporta.
* `time set <value>` — Ajusta la hora del mundo.
* `difficulty <peaceful|easy|normal|hard>` — Cambia dificultad.

## 9) Gestión de **whitelist** (detallado)

La whitelist limita el acceso por **nombre/UUID** de jugador.

**Activar la whitelist (consola):**

```
whitelist on
```

**Agregar jugador por nombre:**

```
whitelist add NombreJugador
```

**Quitar jugador:**

```
whitelist remove NombreJugador
```

**Listar jugadores en whitelist:**

```
whitelist list
```

**Archivo:** `whitelist.json` contiene los UUID y nombres. Cuando `white-list=true` en `server.properties`, sólo esos entran.

> Si necesitas controlar acceso por IP (en vez de nombre) usa `netsh advfirewall` o un plugin que acepte IPs.

## 10) Ban / Kick / Ban-IP (detallado)

**Kick:**

```
kick Jugador Motivo opcional
```

**Ban por nombre/UUID:**

```
ban Jugador Motivo opcional
```

**Quitar ban:**

```
pardon Jugador
```

**Ban por IP:**

```
ban-ip 203.0.113.42 Motivo
```

**Quitar ban IP:**

```
pardon-ip 203.0.113.42
```

> Los bans por IP bloquean la IP completa. Ten cuidado con IPs dinámicas compartidas.

## 11) Operators (ops) y permisos

**Dar operador (desde consola o archivo):**

```
op NombreJugador
```

**Quitar operador:**

```
deop NombreJugador
```

`ops.json` también puede configurarse manualmente con nivel de permiso (1-4) en servidores que lo soporten.

## 12) Plugins útiles (si usas Paper/Spigot)

Si quieres control por IP o más opciones de administración, instala **Paper** o **Spigot** y plugins como:

* `EssentialsX` — Comandos administrativos extendidos (`/banip`, `/tempban`, `/kick`, `/mute`, etc.).
* `LuckPerms` — Sistema de permisos avanzado.
* `IPWhitelist` / `AdvancedBan` — Plugins para control más fino por IP y bans avanzados.

Instalar plugins: coloca los `.jar` en la carpeta `plugins` y reinicia el servidor.

## 13) Copias de seguridad y mantenimiento

* Haz backups completos de la carpeta `world` periódicamente.
* Antes de actualizar servidor/plugin, crear backup.
* Ejecuta `save-all` y luego copia la carpeta para evitar corrupción.

Script rápido para backup (PowerShell):

```powershell
# backup.ps1
$fecha = Get-Date -Format "yyyyMMdd-HHmmss"
$origen = "C:\mc-server"
$destino = "D:\backups\mc-server-$fecha.zip"
Compress-Archive -Path $origen -DestinationPath $destino -Force
```

## 14) Ejemplos prácticos y atajos

* **Expulsar y banear IP maliciosa:**

  1. `kick NombreJugador Attacking players`
  2. `ban NombreJugador Abuso`
  3. `ban-ip 203.0.113.42 DDoS` (o usar netsh para bloquear a nivel Windows)

* **Activar whitelist y dejar sólo a 2 jugadores:**

  ```
  whitelist add Jugador1
  whitelist add Jugador2
  whitelist on
  ```

* **Permitir solo IP privada de tu oficina (192.0.2.5) para conectar:**

  ```powershell
  # eliminar regla amplia si existe
  netsh advfirewall firewall delete rule name="Minecraft TCP"
  # permitir solo IP concreta
  netsh advfirewall firewall add rule name="Minecraft From Office" dir=in action=allow protocol=TCP localport=25565 remoteip=192.0.2.5
  ```
